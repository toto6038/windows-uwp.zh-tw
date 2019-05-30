---
title: 從您的應用程式觸發背景工作
description: 說明如何從應用程式中觸發背景工作
ms.date: 07/06/2018
ms.topic: article
keywords: 背景工作觸發程序，背景工作
ms.localizationpriority: medium
ms.openlocfilehash: d5d163d36b51e414a403986d1fdd73db7925cc0b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371889"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>從您的應用程式觸發背景工作

了解如何使用 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) 從您的應用程式啟動背景工作。

如需如何建立應用程式觸發程序的範例，請參閱此[範例](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs)。

本主題假設您有想要從應用程式啟動的背景工作。 如果您目前沒有背景工作，可以在 [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs) 找到範例背景工作。 或者，依照[建立和註冊跨處理序的背景工作](create-and-register-a-background-task.md)中的步驟建立背景工作。

## <a name="why-use-an-application-trigger"></a>為什麼要使用應用程式觸發程序

使用 **ApplicationTrigger**，在不同於前景應用程式的處理序中執行程式碼。 如果 App 有工作需要在背景完成 (即使使用者關閉前景 App)，很適合使用 **ApplicationTrigger**。 如果背景工作應於關閉 App 時停止，或應繫結於前景處理序的狀態，則必須改用[延伸執行](run-minimized-with-extended-execution.md)。

## <a name="create-an-application-trigger"></a>建立應用程式觸發程序

建立新的 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)。 您可以依照下面片段的做法將其儲存在欄位中。 這是為了方便起見，不必在稍後要發出訊號通知觸發程序時建立新的執行個體， 不過，您可以使用任何 **ApplicationTrigger** 執行個體來通知觸發程序。

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

您可以建立背景工作條件來控制何時執行工作。 條件會在條件符合之前，一直讓背景工作無法執行。 如需詳細資訊，請參閱[設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)。

在此範例中，條件設定為**InternetAvailable**以便一次觸發，此工作只執行一次網際網路存取。 如需可用條件的清單，請參閱 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)。

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

如需有關背景觸發程序條件及類型的深入資訊，請參閱[使用背景工作支援應用程式](support-your-app-with-background-tasks.md)。

##  <a name="call-requestaccessasync"></a>呼叫 RequestAccessAsync()

註冊 **ApplicationTrigger** 背景工作之前，由於使用者可能已經停用 App 的背景活動，請呼叫 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) 來判斷使用者允許的背景活動層級。 如需有關使用者可如何控制背景活動設定的詳細資訊，請參閱[最佳化背景活動](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)。

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>登錄背景工作

呼叫背景工作登錄函式以登錄背景工作。 如需註冊背景工作的詳細資訊，並查看以下範例程式碼中 **RegisterBackgroundTask()** 方法的定義，請參閱[註冊背景工作](register-a-background-task.md)。

如果您想要使用應用程式觸發程序來延長前景處理序的存留期，請考慮改用[延伸執行](run-minimized-with-extended-execution.md)。 應用程式觸發程序是專為建立要用於執行工作的個別裝載處理序所設計。 下列代碼片段會註冊跨處理序背景觸發程序。

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

背景工作登錄參數都是在登錄時驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 請確認您的應用程式能夠妥善處理背景工作註冊失敗的狀況；反之，如果應用程式需依賴有效的驗證物件，則在嘗試註冊工作之後，可能會當機。

## <a name="trigger-the-background-task"></a>觸發背景工作

觸發背景工作之前，使用 [BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) 來驗證背景工作是否已註冊。 驗證所有背景工作是否皆已註冊的好時機是在 App 啟動時。

藉由呼叫 [ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger) 來觸發背景工作。 任何 **ApplicationTrigger** 執行個體都適用。

請注意，無法從背景工作本身呼叫 **ApplicationTrigger.RequestAsync**，也無法在 App 處於背景執行狀態時這麼做 (如需應用程式狀態的詳細資訊，請參閱[應用程式週期](app-lifecycle.md))。
如果使用者已設定阻止 App 執行背景活動的能源或隱私權原則，這可能會傳回 [DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult)。
此外，一次也只有一個 AppTrigger 可以執行。 如果您嘗試在另一個應用程式觸發程序已經在執行時執行 AppTrigger，函式將會傳回 [CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult)。

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>管理背景工作的資源

使用 [BackgroundExecutionManager.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) 可判斷使用者是否已決定限制您的應用程式的背景活動。 請留意您的電池使用量，並且只在需要完成使用者想要的動作時才在背景執行。 如需有關使用者可如何控制背景活動設定的詳細資訊，請參閱[最佳化背景活動](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)。  

- 記憶體：微調您的應用程式的記憶體和能源使用是確保作業系統可讓您執行的背景工作的關鍵。 使用[記憶體管理 API](https://docs.microsoft.com/uwp/api/windows.system.memorymanager) 來了解您的背景工作正在使用多少記憶體。 您的背景工作使用的記憶體越多，作業系統就越不容易在有其他 App 執行於前景時允許您的 App 繼續執行。 最終是由使用者控制您的應用程式可執行的所有背景活動，而且他也能看到您的應用程式對電池使用量的影響。  
- CPU 時間：背景工作會受到時鐘取得根據觸發程序類型的使用時間數量。 應用程式觸發程序所觸發之背景工作的時間限制約為 10 分鐘。

請參閱[使用背景工作支援應用程式](support-your-app-with-background-tasks.md)，以了解套用至背景工作的資源限制。

## <a name="remarks"></a>備註

從 Windows 10 開始，它不再需要將您的應用程式新增至在鎖定畫面中，才能利用背景工作的使用者。

如果您已經先呼叫 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)，背景工作將只會使用 **ApplicationTrigger** 來執行。

## <a name="related-topics"></a>相關主題

* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [背景工作的程式碼範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [建立及註冊同處理序序背景工作](create-and-register-an-inproc-background-task.md)。
* [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)
* [偵錯背景工作](debug-a-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [當應用程式移至背景時釋出記憶體](reduce-memory-usage.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [如何觸發暫止、 繼續及背景事件 UWP 應用程式中的 （當偵錯）](https://go.microsoft.com/fwlink/p/?linkid=254345)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [透過延長執行延後應用程式暫停](run-minimized-with-extended-execution.md)
* [註冊背景工作](register-a-background-task.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
