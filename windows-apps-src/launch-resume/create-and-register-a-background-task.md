---
author: TylerMSFT
title: "建立並登錄背景工作"
description: "建立背景工作類別並加以登錄，即使您的 App 不在前景也能執行。"
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
translationtype: Human Translation
ms.sourcegitcommit: 579547b7bd2ee76390b8cac66855be4a9dce008e
ms.openlocfilehash: e8da193f96709bdd87bd6a008eb5885cc5c819fd

---

# 建立並登錄背景工作


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

建立背景工作類別並加以登錄，即使您的 App 不在前景也能執行。

## 建立背景工作類別


您可以撰寫實作 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 介面的類別，在背景執行程式碼。 這個程式碼會在觸發特定事件後執行，例如使用 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 或 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)。

下列步驟示範如何撰寫實作 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 介面的新類別。 開始之前，請先在您的方案中為背景工作建立一個新專案。 請為您的背景工作新增一個空的類別，然後匯入 [Windows.ApplicationModel.Background](https://msdn.microsoft.com/library/windows/apps/br224847) 命名空間。

1.  為背景工作建立一個新專案，並將其新增到您的方案中。 若要這麼做，請以滑鼠右鍵按一下您在 [方案總管]**** 中的方案節點，然後選取 [新增] -&gt; [新增專案]。 接著，選取 [Windows 執行階段元件 (通用 Windows)]**** 專案類型、為專案命名，然後按一下 [確定]。
2.  從您的通用 Windows 平台 (UWP) app 專案參考背景工作專案。

    如果是 C++ 應用程式，請在您的應用程式專案上按一下滑鼠右鍵，然後選取 [屬性]****。 接著，移至 [通用屬性]**** 並按一下 [加入新參考]****，核取您背景工作專案旁邊的方塊，然後在兩個對話方塊中都按一下 [確定]****。

    如果是 C# 應用程式，請在您的應用程式專案中，於 [參考]**** 上按一下滑鼠右鍵，然後選取 [加入新參考]****。 在 [方案]**** 下選取 [專案]****，然後選取您背景工作專案的名稱並按一下 [確定]****。

3.  建立一個實作 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 介面的新類別。 [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 方法是一個在觸發指定事件時將會呼叫的必要進入點；這是每個背景工作都需要的方法。

    > [!NOTE]
    > 背景工作類別本身及背景工作專案中的所有其他類別都必須是 **sealed** 的 **public** 類別。

    下列範例程式碼示範一個非常基本的背景工作起點：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     //
    >     // ExampleBackgroundTask.cs
    >     //
    >
    >     using Windows.ApplicationModel.Background;
    >
    >     namespace Tasks
    >     {
    >         public sealed class ExampleBackgroundTask : IBackgroundTask
    >         {
    >             public void Run(IBackgroundTaskInstance taskInstance)
    >             {
    >                 
    >             }        
    >         }
    >     }
    > ```
    > ```cpp
    >     //
    >     // ExampleBackgroundTask.h
    >     //
    >
    >     #pragma once
    >
    >     using namespace Windows::ApplicationModel::Background;
    >
    >     namespace RuntimeComponent1
    >     {
    >         public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    >         {
    >
    >         public:
    >             ExampleBackgroundTask();
    >
    >             virtual void Run(IBackgroundTaskInstance^ taskInstance);
    >             void OnCompleted(
    >                     BackgroundTaskRegistration^ task,
    >                     BackgroundTaskCompletedEventArgs^ args
    >                     );
    >         };
    >     }
    >
    >     //
    >     // ExampleBackgroundTask.cpp
    >     //
    >
    >     #include "ExampleBackgroundTask.h"
    >
    >     using namespace Tasks;
    >
    >     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
    >     {
    >
    >     }
    >  ```

4.  如果您在背景工作中執行任何非同步程式碼，您的背景工作就需要使用延遲。 如果沒有使用延遲，當 Run 方法在您的非同步方法呼叫完成之前先完成時，背景工作處理程序就可能意外終止。

    在呼叫非同步方法之前，請先在 Run 方法中要求延遲。 請將延遲儲存為全域變數，以便能夠從非同步方法存取它。 在非同步程式碼執行完成之後，請宣告延遲完成。

    下列範例程式碼會取得延遲、儲存它，然後在非同步程式碼執行完成時釋出它：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     BackgroundTaskDeferral _deferral = taskInstance.GetDeferral(); // Note: define at class scope
    >     public async void Run(IBackgroundTaskInstance taskInstance)
    >     {
    >         //
    >         // TODO: Insert code to start one or more asynchronous methods using the
    >         //       await keyword, for example:
    >         //
    >         // await ExampleMethodAsync();
    >         //
    >         
    >         _deferral.Complete();
    >     }
    > ```
    > ```cpp
    >     BackgroundTaskDeferral^ deferral = taskInstance->GetDeferral(); // Note: define at class scope
    >     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
    >     {
    >         //
    >         // TODO: Modify the following line of code to call a real async function.
    >         //       Note that the task<void> return type applies only to async
    >         //       actions. If you need to call an async operation instead, replace
    >         //       task<void> with the correct return type.
    >         //
    >         task<void> myTask(ExampleFunctionAsync());
    >         
    >         myTask.then([=] () {
    >             deferral->Complete();
    >         });
    >     }
    > ```

> [!NOTE]
> 在 C# 中，可以使用 **async/await** 關鍵字呼叫背景工作的非同步方法。 在 C++ 中，可以使用工作鏈結達成類似的結果。

如需有關非同步模式的詳細資訊，請參閱[非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187335)。 如需有關如何使用延遲防止背景工作提前停止的其他範例，請參閱[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)。

下列步驟是在您 app 的其中一個類別 (例如 MainPage.xaml.cs) 中完成。

> [!NOTE]
> 您也可以建立專門用來登錄背景工作的函式，請參閱[登錄背景工作](register-a-background-task.md)。 在這種情況下，您不需要使用以下三個步驟；只需建構觸發程序，並將它和工作名稱、工作進入點及 (選用) 條件提供給登錄函式即可。


## 註冊要執行的背景工作

1.  逐一查看 [**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) 屬性，以了解是否已經註冊背景工作。 這個步驟很重要；如果您的 app 不會檢查現有背景工作的註冊情況，就很容易多次註冊同一工作，因而造成效能問題，且將耗費大量 CPU 時間才能完成工作。

    下列範例會逐一查看 AllTasks 屬性，並在工作已經登錄時，將旗標變數設為 true：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     var taskRegistered = false;
    >     var exampleTaskName = "ExampleBackgroundTask";
    >
    >     foreach (var task in BackgroundTaskRegistration.AllTasks)
    >     {
    >         if (task.Value.Name == exampleTaskName)
    >         {
    >             taskRegistered = true;
    >             break;
    >         }
    >     }
    > ```
    > ```cpp
    >     boolean taskRegistered = false;
    >     Platform::String^ exampleTaskName = "ExampleBackgroundTask";
    >
    >     auto iter = BackgroundTaskRegistration::AllTasks->First();
    >     auto hascur = iter->HasCurrent;
    >
    >     while (hascur)
    >     {
    >         auto cur = iter->Current->Value;
    >
    >         if(cur->Name == exampleTaskName)
    >         {
    >             taskRegistered = true;
    >             break;
    >         }
    >
    >         hascur = iter->MoveNext();
    >     }
    > ```

2.  如果應用程式工作尚未登錄，則使用 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 建立您背景工作的執行個體。 工作進入點應該是您的背景工作類別名稱，並以命名空間為前置詞。

    背景工作觸發程序會控制背景工作將在何時執行。 如需可用觸發程序的清單，請參閱 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)。

    例如，這段程式碼會建立新的背景工作，並設定在引發 **TimeZoneChanged** 觸發程序時執行：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     var builder = new BackgroundTaskBuilder();
    >
    >     builder.Name = exampleTaskName;
    >     builder.TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
    >     builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
    > ```
    > ```cpp
    >     auto builder = ref new BackgroundTaskBuilder();
    >
    >     builder->Name = exampleTaskName;
    >     builder->TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
    >     builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
    > ```

3.  您可以新增條件，以控制觸發程序事件發生後何時執行工作 (選用)。 例如，如果您希望在使用者上線時執行工作，則可使用 **UserPresent** 條件。 如需可用條件的清單，請參閱 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

    下列範例程式碼會指派條件，要求必須要有使用者：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
    > ```
    > ```cpp
    >     builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
    > ```

4.  透過呼叫 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 物件的 Register 方法來註冊背景工作。 儲存 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) 結果以便在下一個步驟使用。

    下列程式碼會註冊背景工作並儲存結果：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     BackgroundTaskRegistration task = builder.Register();
    >     ```
    > ```cpp
    >     BackgroundTaskRegistration^ task = builder->Register();
    > ```

> [!NOTE]
> 通用 Windows app 在登錄任何背景觸發程序類型之前，必須先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

為了確保您的通用 Windows app 會在您發行更新之後繼續正常執行，您必須呼叫 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然後在 app 於更新後啟動時呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 如需詳細資訊，請參閱[背景工作的指導方針](guidelines-for-background-tasks.md)。

## 使用事件處理常式來處理背景工作完成


您應該向 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) 註冊方法，以便讓 app 可以從背景工作取得結果。 啟動或繼續執行 app 時，如果背景工作自 app 上次在前景時即已完成，將會呼叫 mark 方法 (如果背景工作是在您 app 目前處於前景時完成，將會立即呼叫 OnCompleted 方法)。

1.  在 OnCompleted 方法中處理背景工作的完成。 例如，背景工作結果可能會導致 UI 更新。 OnCompleted 事件處理常式方法需要在此處顯示的方法配置，即使這個範例未使用 *args* 參數亦同。

    下列範例程式碼會辨識背景工作完成並呼叫範例 UI 更新方法，以取得訊息字串。

     > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >     {
    >         var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    >         var key = task.TaskId.ToString();
    >         var message = settings.Values[key].ToString();
    >         UpdateUI(message);
    >     }
    > ```
    > ```cpp
    >     void ExampleBackgroundTask::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >     {
    >         auto settings = ApplicationData::Current->LocalSettings->Values;
    >         auto key = task->TaskId.ToString();
    >         auto message = dynamic_cast<String^>(settings->Lookup(key));
    >         UpdateUI(message);
    >     }
    > ```

    > [!NOTE]
    > UI 更新應該以非同步方式執行，以避免 UI 執行緒阻塞。 例如，請參閱[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)中的 UpdateUI 方法。



2.  返回登錄背景工作的位置。 在該行的程式碼後面，新增 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) 物件。 提供您的 OnCompleted 方法做為 **BackgroundTaskCompletedEventHandler** 建構函式的參數。

    下列範例程式碼會將 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) 新增到 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)：

     > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    > ```
    > ```cpp
    >     task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &ExampleBackgroundTask::OnCompleted);
    > ```

## 宣告您的應用程式有使用應用程式資訊清單的背景工作

在 app 能執行背景工作之前，您必須在 app 資訊清單中宣告每一個背景工作。 如果您的 app 嘗試使用未列在資訊清單中的觸發程序來註冊背景工作，註冊將會失敗。

1.  透過開啟名為 Package.appxmanifest 的檔案來開啟封裝資訊清單設計工具。
2.  開啟 [宣告]**** 索引標籤。
3.  從 [可用宣告]**** 下拉式清單中選擇 [背景工作]****，然後按一下 [新增]****。
4.  選取 [系統事件]**** 核取方塊。
5.  在 [進入點:]**** 文字方塊中，輸入您背景類別的命名空間與名稱，在這個範例中會是 RuntimeComponent1.ExampleBackgroundTask。
6.  關閉資訊清單設計工具。

    下列 Extensions 元素會新增至您的 Package.appxmanifest 檔案中以註冊背景工作：

    ```xml
    <Extensions>
      <Extension Category="windows.backgroundTasks" EntryPoint="RuntimeComponent1.ExampleBackgroundTask">
        <BackgroundTasks>
          <Task Type="systemEvent" />
        </BackgroundTasks>
      </Extension>
    </Extensions>
    ```

## 摘要與後續步驟


您現在應該了解如何撰寫背景工作類別的基本概念，如何在 app 內註冊背景工作，以及如何讓 app 辨識背景工作已經完成。 您也應該了解如何更新 app 資訊清單，才能讓您的 app 成功登錄背景工作。

> [!NOTE]
> 下載[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)，以查看使用背景工作、完整且健全的 UWP app 內容中的類似程式碼範例。

請參閱下列 API 參考的相關主題、背景工作概念指引，以及撰寫使用背景工作之 app 的更詳細說明。

> [!NOTE]
> 本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

## 相關主題

**詳細的背景工作說明主題**

* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [登錄背景工作](register-a-background-task.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)

**背景工作指引**

* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [偵錯背景工作](debug-a-background-task.md)
* [如何在 Windows 市集 app 觸發暫停、繼續以及背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)

**背景工作 API 參考**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)



<!--HONumber=Jul16_HO1-->


