---
ms.assetid: 67a46812-881c-404b-9f3b-c6786f39e72b
title: 自訂列印工作流程
description: 建立自訂列印工作流程體驗，以符合貴組織的需求。
ms.date: 07/03/2020
ms.topic: article
keywords: windows 10、uwp、列印
ms.localizationpriority: medium
ms.openlocfilehash: 779965ca46efe7fb63adac46ef2568c2ecff66f1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172192"
---
# <a name="customize-the-print-workflow"></a>自訂列印工作流程

## <a name="overview"></a>概觀

開發人員可以使用列印工作流程應用程式，自訂列印工作流程體驗。 列印工作流程應用程式是一種 UWP app，可擴充 [Microsoft Store 裝置應用程式 (WSDA)](/windows-hardware/drivers/devapps/)的功能，所以在繼續進行之前，對 WSDA 有一定的了解會有幫助。

就像 WSDA 一樣，當來源應用程式的使用者選擇列印某些項目，並透過 [列印] 對話方塊瀏覽時，系統會檢查是否有關聯至該印表機的工作流程應用程式。 如果有，列印工作流程應用程式便會啟動 (主要做為背景工作，以下會有更詳細說明)。 工作流程應用程式可以變更列印票證 (用於設定目前列印工作之印表機裝置設定的 XML 文件) 以及要列印的實際 XPS 內容。 也可以選擇在程序中途啟動 UI，公開此功能給使用者。 完成工作之後，它會傳遞列印內容和列印票證給驅動程式。

因為列印工作流程應用程式包含背景和前景元件，而且其功能可以搭配其他應用程式，因此實作上會比其他類型的 UWP 應用程式更為複雜。 建議您在閱讀本指南時一併查看[工作流程應用程式範例](https://github.com/Microsoft/print-oem-samples)，以便更加了解如何實作不同的功能。 為了簡化起見，本指南未包含某些功能，例如各種錯誤檢查和 UI 管理。

## <a name="getting-started"></a>開始使用

工作流程應用程式必須向列印系統指出其進入點，以便可在適當時間將其啟動。 做法是在 UWP 專案 *package.appxmanifest* 檔案的 `Application/Extensions` 中的插入下列宣告。

```xml
<uap:Extension Category="windows.printWorkflowBackgroundTask"  
    EntryPoint="WFBackgroundTasks.WfBackgroundTask" />
```

> [!IMPORTANT]
> 在很多情況中，列印自訂不需要使用者輸入。 基於這個原因，列印工作流程應用程式預設做為背景工作執行。

如果工作流程應用程式關聯至啟動列印工作的來源應用程式工作 (有關這方面的指示，請參閱稍後章節)，列印系統會檢查其資訊清單檔案中是否有背景工作進入點。

## <a name="do-background-work-on-the-print-ticket"></a>在列印票證上執行背景工作

列印系統與工作流程應用程式合作的第一件事就是啟動其背景工作 (在本案例中是 `WFBackgroundTasks` 命名空間中的  `WfBackgroundTask`)。 在背景工作的 `Run` 方法中，您應將工作的觸發程序詳細資料傳送為 **[PrintWorkflowTriggerDetails](/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails)** 執行個體。 這可提供列印工作流程背景工作的特殊功能。 它會公開 **[PrintWorkflowSession](/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails.PrintWorkflowSession)** 屬性，這是 **[PrintWorkFlowBackgroundSession](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession)** 的執行個體。 列印工作流程工作階段類別 - 包括背景和前景種類 - 將會控制列印工作流程應用程式的連續步驟。

然後註冊此工作階段類別會引發之兩個事件的處理常式方法。 您將在稍後定義這些方法。

```csharp
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take out a deferral here and complete once all the callbacks are done
    runDeferral = taskInstance.GetDeferral();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // cast the task's trigger details as PrintWorkflowTriggerDetails
    PrintWorkflowTriggerDetails workflowTriggerDetails = taskInstance.TriggerDetails as PrintWorkflowTriggerDetails;

    // Get the session manager, which is unique to this print job
    PrintWorkflowBackgroundSession sessionManager = workflowTriggerDetails.PrintWorkflowSession;

    // add the event handler callback routines
    sessionManager.SetupRequested += OnSetupRequested;
    sessionManager.Submitted += OnXpsOMPrintSubmitted;

    // Allow the event source to start
    // This call blocks until all of the workflow callbacks complete
    sessionManager.Start();
}
```

呼叫 `Start` 方法時，工作階段管理員將第一次引發 **[SetupRequested](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.SetupRequested)** 事件。 這個事件會公開列印工作以及列印票證的一般資訊。 在這個階段，可在背景中編輯列印票證。

```csharp
private void OnSetupRequested(PrintWorkflowBackgroundSession sessionManager, PrintWorkflowBackgroundSetupRequestedEventArgs printTaskSetupArgs) {
    // Take out a deferral here and complete once all the callbacks are done
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get general information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;

    // edit the print ticket
    WorkflowPrintTicket printTicket = printTaskSetupArgs.GetUserPrintTicketAsync();

    // ...
```

重要的是，它位於 **SetupRequested** 的處理中，應用程式用來判斷是否啟動前景元件。 這可能取決於先前儲存到本機存放裝置的設定，或在編輯列印票證期間所發生的事件，或者可能是特定應用程式的靜態設定。

```csharp
// ...

if (UIrequested) {
    printTaskSetupArgs.SetRequiresUI();

    // Any data that is to be passed to the foreground task must be stored the app's local storage.
    // It should be prefixed with the sourceApplicationName string and the SessionId string, so that
    // it can be identified as pertaining to this workflow app session.
}

// Complete the deferral taken out at the start of OnSetupRequested
setupRequestedDeferral.Complete();
```

## <a name="do-foreground-work-on-the-print-job-optional"></a>在列印工作上執行前景工作 (選用)

如果呼叫 **[SetRequiresUI](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsetuprequestedeventargs.SetRequiresUI)** 方法，則列印系統會檢查資訊清單檔案是否有前景應用程式的進入點。 *package.appxmanifest* 檔案的 `Application/Extensions` 項目必須具有下列行。 以前景應用程式的名稱取代 `EntryPoint` 的值。

```xml
<uap:Extension Category="windows.printWorkflowForegroundTask"  
    EntryPoint="MyWorkFlowForegroundApp.App" />
```

接著，列印系統呼叫特定應用程式進入點的 **OnActivated** 方法。 在 _App.xaml.cs_ 檔案的 **OnActivated** 方法中，工作流程應用程式應該會檢查啟用種類來確認是否為工作流程啟用。 如果是，工作流程應用程式可以傳送啟用引數至 **[PrintWorkflowUIActivatedEventArgs](/uwp/api/windows.graphics.printing.workflow.printworkflowuiactivatedeventargs)** 物件，此物件會將 **[PrintWorkflowForegroundSession](/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession)** 物件公開為屬性。 正如同上一節中的背景對等項目，此物件包含由列印系統引發的事件，您可以指定處理常式給這些事件。 在此情況下，事件處理功能將在稱為 `WorkflowPage` 的不同類別中實作。

首先，在 _App.xaml.cs_ 檔案中：

```csharp
protected override void OnActivated(IActivatedEventArgs args){

    if (args.Kind == ActivationKind.PrintWorkflowForegroundTask) {

        // the app should instantiate a new UI view so that it can properly handle the case when
        // several print jobs are active at the same time.
        Frame rootFrame = new Frame();
        if (null == Window.Current.Content)
        {
            rootFrame.Navigate(typeof(WorkflowPage));
            Window.Current.Content = rootFrame;
        }

        // Get the main page
        WorkflowPage workflowPage = (WorkflowPage)rootFrame.Content;

        // Make sure the page knows it's handling a foreground task activation
        workflowPage.LaunchType = WorkflowPage.WorkflowPageLaunchType.ForegroundTask;

        // Get the activation arguments
        PrintWorkflowUIActivatedEventArgs printTaskUIEventArgs = args as PrintWorkflowUIActivatedEventArgs;

        // Get the session manager
        PrintWorkflowForegroundSession taskSessionManager = printTaskUIEventArgs.PrintWorkflowSession;

        // Add the callback handlers - these methods are in the workflowPage class
        taskSessionManager.SetupRequested += workflowPage.OnSetupRequested;
        taskSessionManager.XpsDataAvailable += workflowPage.OnXpsDataAvailable;

        // start raising the print workflow events
        taskSessionManager.Start();
    }
}
```

UI 已連接事件處理常式且 **OnActivated** 方法已結束後，列印系統會引發 **[SetupRequested](/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.SetupRequested)** 事件讓 UI 來處理。 這個事件提供背景工作設定事件所提供的相同資料，包括列印工作資訊和列印票證文件，但不具有要求啟動額外 UI 的能力。 在 _WorkflowPage.xaml.cs_ 檔案中：

```csharp
internal void OnSetupRequested(PrintWorkflowForegroundSession sessionManager, PrintWorkflowForegroundSetupRequestedEventArgs printTaskSetupArgs) {
    // If anything asynchronous is going to be done, you need to take out a deferral here,
    // since otherwise the next callback happens once this one exits, which may be premature
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;
    // the following string should be used when storing data that pertains to this workflow session
    // (such as user input data that is meant to change the print content later on)
    string localStorageVariablePrefix = string.Format("{0}::{1}::", sourceApplicationName, sessionID);

    try
    {
        // receive and store user input
        // ...
    }
    catch (Exception ex)
    {
        string errorMessage = ex.Message;
        Debug.WriteLine(errorMessage);
    }
    finally
    {
        // Complete the deferral taken out at the start of OnSetupRequested
        setupRequestedDeferral.Complete();
    }
}
```

接著，列印系統會引發 UI 的 **[XpsDataAvailable](/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.XpsDataAvailable)** 事件。 在此事件的處理常式中，工作流程應用程式可以存取設定事件可用的所有資料，並可以原始位元組串流或物件模型的形式另外直接讀取 XPS 資料。 存取 XPS 資料可讓 UI 提供預覽列印服務，以及提供工作流程應用程式將在資料上執行之作業的其他相關資訊給使用者。

在這個事件處理常式中，如果工作流程應用程式將會繼續與使用者互動，則必須取得延遲物件。 如果沒有延遲，當 **XpsDataAvailable** 事件處理常式結束或呼叫非同步方法時，列印系統會將 UI 工作視為已完成。 當應用程式已從使用者與 UI 的互動收集到所需的所有資訊時，它應該完成延遲，讓列印系統可以前進。

```csharp
internal async void OnXpsDataAvailable(PrintWorkflowForegroundSession sessionManager, PrintWorkflowXpsDataAvailableEventArgs printTaskXpsAvailableEventArgs)
{
    // Take out a deferral
    Deferral xpsDataAvailableDeferral = printTaskXpsAvailableEventArgs.GetDeferral();

    SpoolStreamContent xpsStream = printTaskXpsAvailableEventArgs.Operation.XpsContent.GetSourceSpoolDataAsStreamContent();

    IInputStream inputStream = xpsStream.GetInputSpoolStream();

    using (var inputReader = new Windows.Storage.Streams.DataReader(inputStream))
    {
        // Read the XPS data from input stream
        byte[] xpsData = new byte[inputReader.UnconsumedBufferLength];
        while (inputReader.UnconsumedBufferLength > 0)
        {
            inputReader.ReadBytes(xpsData);
            // Do something with the XPS data, e.g. preview
            // ...
        }
    }

    // Complete the deferral taken out at the start of this method
    xpsDataAvailableDeferral.Complete();
}
```

此外，事件引數公開的 **[PrintWorkflowSubmittedOperation](/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation)** 執行個體提供選項來取消列印工作或指出工作成功但不需要輸出列印工作。 做法是呼叫 **[Complete](/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation.Complete)** 方法搭配 **[PrintWorkflowSubmittedStatus](/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedstatus)** 值。

> [!NOTE]
> 如果工作流程應用程式取消列印工作，強烈建議它提供快顯通知，指出為何取消工作。

## <a name="do-final-background-work-on-the-print-content"></a>在列印內容上執行最終背景工作

UI 完成 **PrintTaskXpsDataAvailable** 事件中的延遲延之後 (或如果略過 UI 步驟)，列印系統會引發背景工作的 **[Submitted](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.Submitted)** 事件。 在這個事件的處理常式中，工作流程應用程式可以存取 **XpsDataAvailable** 事件提供的所有相同資料。 然而，與先前事件不同的是，**Submitted** 還透過 **[PrintWorkflowTarget](/uwp/api/windows.graphics.printing.workflow.printworkflowtarget)** 執行個體提供最終列印工作內容的*寫入*存取權。

用來多工緩衝最終列印之資料的物件，取決於來源資料是以原始位元組串流還是 XPS 物件模型的形式來存取。 當工作流程應用程式透過位元組串流存取來源資料，則會提供輸出位元組串流來寫入最終工作資料。 當工作流程應用程式透過物件模型存取來源資料，則會提供文件寫作程式來寫入物件至輸出工作。 在任一種情況下，工作流程應用程式都應該會讀取所有資料來源、修改任何必要的資料，並將修改過的資料寫入輸出目標。

當背景工作完成寫入資料，它應該會在對應的 **PrintWorkflowSubmittedOperation** 物件上呼叫 **Complete**。 工作流程應用程式完成此步驟且 **Submitted** 事件處理常式結束後，工作流程工作階段會關閉，且使用者可以透過標準列印對話方塊監視最終列印工作的狀態。

## <a name="final-steps"></a>最後步驟

### <a name="register-the-print-workflow-app-to-the-printer"></a>將列印工作流程應用程式註冊至印表機

使用和 WSDA 相同的中繼資料檔案提交類型，將您的工作流程應用程式關聯至印表機。 事實上，單一 UWP 應用程式可同時做為工作流程應用程式和提供工作列印設定功能的 WSDA。 請依照對應的[建立中繼資料關聯的 WSDA 步驟](/windows-hardware/drivers/devapps/step-2--create-device-metadata)來進行。

不同之處在於，WSDA 會為使用者自動啟動 (當使用者在相關聯的裝置上列印時，應用程式一律會啟動)，而工作流程應用程式不會。 必須為它們另外設定原則。

### <a name="set-the-workflow-apps-policy"></a>設定工作流程應用程式的原則

工作流程應用程式原則由 Powershell 命令在執行工作流程應用程式的裝置上設定。 需修改 Set-Printer、Add-Printer (現有的連接埠) 和 Add-Printer (新的 WSD 連接埠) 命令以允許設定工作流程原則。

* `Disabled`：不啟動工作流程應用程式。
* `Uninitialized`：如果系統中安裝工作流程 DCA，則會啟動工作流程應用程式。 如果未安裝應用程式，列印仍會繼續。
* `Enabled`：如果系統中安裝工作流程 DCA，則會啟動工作流程合約。 如果未安裝應用程式，列印會失敗。

下列命令在指定的印表機上設定必要的工作流程應用程式。

```Powershell
Set-Printer –Name "Microsoft XPS Document Writer" -WorkflowPolicy Enabled
```

本機使用者可以在本機印表機上執行這項原則，而對於企業實作，印表機系統管理員可以在列印伺服器上執行這項原則。 接著原則會再同步至所有用戶端連線。 每當加入新的印表機，印表機系統管理員都可以使用這項原則。

## <a name="see-also"></a>另請參閱

[工作流程應用程式範例](https://github.com/Microsoft/print-oem-samples)

[Windows.Graphics.Printing.Workflow 命名空間](/uwp/api/windows.graphics.printing.workflow)