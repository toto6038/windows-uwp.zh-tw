---
author: TylerMSFT
title: 從您的應用程式觸發背景工作
description: 說明如何觸發背景工作從應用程式
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: 背景工作觸發程序，背景工作
ms.localizationpriority: medium
ms.openlocfilehash: 5ccd171f53795ef71830ffb022d0468facb3ac4f
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5519107"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>從您的應用程式觸發背景工作

了解如何使用 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) 從您的應用程式啟動背景工作。

如需如何建立應用程式觸發程序的範例，請參閱此[範例](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs)。

本主題假設您有您想要啟用從您的應用程式的背景工作。 如果您還沒有背景工作，在[BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs)就範例背景工作。 或者，請依照下列步驟中[建立及註冊跨處理序背景工作](create-and-register-a-background-task.md)建立一個。

## <a name="why-use-an-application-trigger"></a>為什麼要使用的應用程式觸發程序

使用**ApplicationTrigger**在個別處理程序中執行程式碼從前景應用程式。 **ApplicationTrigger**是適當，如果您的應用程式有需要執行的動作，在背景中： 即使在使用者關閉前景應用程式的工作。 如果背景工作應該停止當應用程式關閉時，或[延伸執行](run-minimized-with-extended-execution.md)應該改用，則應該與前景處理程序的狀態。

## <a name="create-an-application-trigger"></a>建立應用程式觸發程序

建立新的[ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)。 您無法儲存它的欄位中，會在下面的程式碼片段中完成。 這是為了方便起見，因此，我們不需要稍後建立新的執行個體，當我們想要發出訊號觸發程序。 但您可以使用任何**ApplicationTrigger**執行個體來發出訊號觸發程序。

```csharp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
_AppTrigger = new ApplicationTrigger();
```

```cppwinrt
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
Windows::ApplicationModel::Background::ApplicationTrigger _AppTrigger;
```

```cpp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
ApplicationTrigger ^ _AppTrigger = ref new ApplicationTrigger();
```

## <a name="optional-add-a-condition"></a>(選用) 新增條件

您可以建立背景工作條件以控制何時執行工作。 條件會執行符合條件之前，條件會防止背景工作。 如需詳細資訊，請參閱[設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)。

在此範例中，條件設成**InternetAvailable** ，如此一次觸發，工作僅能執行一旦網際網路存取權是以可用。 如需可用條件的清單，請參閱 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable)
```

如需條件與背景觸發程序類型上的深入資訊，請參閱[支援您的應用程式使用背景工作](support-your-app-with-background-tasks.md)。

##  <a name="call-requestaccessasync"></a>呼叫 RequestAccessAsync()

登錄**ApplicationTrigger**背景工作之前，呼叫[**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)來判斷背景活動，因為使用者可能已停用您的應用程式的背景活動，可讓使用者的層級。 如需有關方式使用者[最佳化背景活動](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)可以控制的背景活動的設定，請參閱。

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>登錄背景工作

呼叫背景工作登錄函式以登錄背景工作。 如需有關註冊背景工作，並查看**RegisterBackgroundTask()** 方法，在下列範例程式碼中定義的詳細資訊，請參閱[註冊背景工作](register-a-background-task.md)。

如果您正考慮使用應用程式觸發程序來擴充您的前景處理程序的存留期，請考慮改為使用[延伸執行](run-minimized-with-extended-execution.md)。 應用程式觸發程序的設計是供建立分別裝載的處理程序來執行工作。 下列程式碼片段會註冊一個處理程序背景觸發程序。

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example application trigger";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example application trigger" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example application trigger";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

背景工作登錄參數都是在登錄時驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 請確認您的 App 能夠妥善處理背景工作註冊失敗的狀況；反之，如果 App 需依賴有效的驗證物件，則在嘗試註冊工作之後，可能會當機。

## <a name="trigger-the-background-task"></a>觸發程序背景工作

觸發背景工作之前，使用[BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)來驗證已登錄背景工作。 確認您的背景工作的所有已登錄的好時機是在應用程式啟動期間。

藉由呼叫[ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger)觸發背景工作。 任何**ApplicationTrigger**執行個體將會執行動作。

請注意從背景工作本身，或應用程式在背景執行狀態時，無法呼叫**ApplicationTrigger.RequestAsync** （如需應用程式狀態的詳細資訊，請參閱[應用程式生命週期](app-lifecycle.md)）。
如果使用者已設定執行背景活動時，防止應用程式的能源或隱私權原則，它可能會傳回[DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult) 。
此外，只有一個 AppTrigger 可以執行一次。 如果您嘗試執行 AppTrigger，另一個正在執行時，該函式會傳回[CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult)。

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>管理您的背景工作的資源

使用 [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) 可判斷使用者是否已決定限制您的應用程式的背景活動。 請留意您的電池使用量，並且只在需要完成使用者想要的動作時才在背景執行。 如需有關方式使用者[最佳化背景活動](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)可以控制的背景活動的設定，請參閱。  

- Memory： 微調您的應用程式的記憶體與能源使用量很重要，作業系統將會允許您執行的背景工作。 若要查看您的背景工作正在使用多少記憶體使用[的記憶體管理 Api](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) 。 您的背景工作會使用更多的記憶體，讓它執行另一個應用程式在前景時，作業系統會越難。 最終是由使用者控制您的應用程式可執行的所有背景活動，而且他也能看到您的應用程式對電池使用量的影響。  
- CPU 時間： 背景工作會受限於取得根據觸發程序類型的實際執行使用時間量。 觸發應用程式觸發程序的背景工作限於約 10 分鐘的時間。

請參閱[使用背景工作支援應用程式](support-your-app-with-background-tasks.md)，以了解套用至背景工作的資源限制。

## <a name="remarks"></a>備註

從 windows 10 開始，它不再需要，讓使用者能夠將您的應用程式新增到鎖定畫面，就可以使用背景工作。

背景工作只會使用**ApplicationTrigger** ，如果您已先呼叫[**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)來執行。

## <a name="related-topics"></a>相關主題

* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [背景工作程式碼範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [建立及註冊同處理序序背景工作](create-and-register-an-inproc-background-task.md)。
* [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)
* [偵錯背景工作](debug-a-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [當應用程式移至背景時釋出記憶體](reduce-memory-usage.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [如何在 UWP 應用程式觸發暫停、繼續和背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [透過延長執行延後應用程式暫停](run-minimized-with-extended-execution.md)
* [登錄背景工作](register-a-background-task.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
