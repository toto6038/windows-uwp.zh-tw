---
title: 在計時器上執行背景工作
description: 了解如何排程一次性的背景工作，或執行定期的背景工作。
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10 uwp，背景工作
ms.localizationpriority: medium
ms.openlocfilehash: 46d2b5704fa8a9bf53534ded98647ce57da0f520
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661893"
---
# <a name="run-a-background-task-on-a-timer"></a>在計時器上執行背景工作

了解如何使用 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) 排程一次性的背景工作，或執行定期的背景工作。

請參閱[背景啟用範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundActivation) 中的 **Scenario4**，以查看如何實作本主題所述時間觸發背景工作的範例。

本主題假設您有背景工作需要定期執行或在特定時間執行。 如果您目前沒有背景工作，可以在 [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs) 找到範例背景工作。 或者，請依照[建立和註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)或[建立和註冊跨處理序背景工作](create-and-register-a-background-task.md)中的步驟建立背景工作。

## <a name="create-a-time-trigger"></a>建立時間觸發程序

建立一個新的 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)。 第二個參數 *OneShot* 會指定背景工作將只執行一次，還是要繼續定期執行。 如果 *OneShot* 設定成 True，第一個參數 (*FreshnessTime*) 會指定排定背景工作之前要等待的時間 (以分鐘為單位)。 如果 *OneShot* 設定成 False，*FreshnessTime* 會指定背景工作的執行頻率。

以傳統型或行動裝置系列為目標之「通用 Windows 平台」(UWP) app 的內建計時器會每隔 15 分鐘執行一次背景工作。 (計時器按照 15 分鐘的間隔執行，讓系統只需要每隔 15 分鐘喚醒一次來喚醒已要求 TimerTriggers 的 App，這樣也省節電力)。

- 如果 *FreshnessTime* 設定為 15 分鐘且 *OneShot* 為 True，就會將工作排定在從工作登錄之後的 15 到 30 分鐘之間開始，執行一次。 如果是設定為 25 分鐘且 *OneShot* 為 True，就會將工作排定在從工作登錄之後的 25 到 40 分鐘之間開始，執行一次。

- 如果 *FreshnessTime* 設定為 15 分鐘且 *OneShot* 為 False，就會將工作排定在從工作登錄之後的 15 到 30 分鐘之間開始，每隔 15 分鐘執行一次。 如果是設定為 n 分鐘且 *OneShot* 為 False，就會將工作排定在從工作登錄之後的 n 到 n + 15 分鐘之間開始，每隔 n 分鐘執行一次。

> [!NOTE]
> 如果*FreshnessTime*設定為小於 15 分鐘內，嘗試註冊背景工作時，會擲回例外狀況。
 
例如，此觸發程序會導致背景工作，以執行一小時一次。

```cs
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
```

```cppwinrt
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };
```

```cpp
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
```

## <a name="optional-add-a-condition"></a>(選用) 新增條件

您可以建立背景工作條件來控制何時執行工作。 條件會在條件符合之前，一直讓背景工作無法執行。 如需詳細資訊，請參閱[設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)。

這個範例中，條件是設成 **UserPresent**，因此觸發後工作只有在使用者作用中時才會執行一次。 如需可用條件的清單，請參閱 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

```cs
SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
```

```cpp
SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent);
```

如需有關背景觸發程序條件及類型的深入資訊，請參閱[使用背景工作支援應用程式](support-your-app-with-background-tasks.md)。

##  <a name="call-requestaccessasync"></a>呼叫 RequestAccessAsync()

註冊 **ApplicationTrigger** 背景工作之前，由於使用者可能已經停用 App 的背景活動，請呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494) 來判斷使用者允許的背景活動層級。 如需有關使用者可如何控制背景活動設定的詳細資訊，請參閱[最佳化背景活動](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)。

```cs
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
    // Depending on the value of requestStatus, provide an appropriate response
    // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>登錄背景工作

呼叫背景工作登錄函式以登錄背景工作。 如需註冊背景工作的詳細資訊，並查看以下範例程式碼中 **RegisterBackgroundTask()** 方法的定義，請參閱[註冊背景工作](register-a-background-task.md)。

> [!IMPORTANT]
> 背景工作會在您的應用程式相同的程序中執行，如未設定`entryPoint`。 背景工作會從您的應用程式執行不同的處理序中，設定`entryPoint`命名空間中，'。 '，和包含您的背景工作實作的類別名稱。

```cs
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example hourly background task";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example hourly background task" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example hourly background task";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

背景工作登錄參數都是在登錄時驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 請確認您的應用程式能夠妥善處理背景工作註冊失敗的狀況；反之，如果應用程式需依賴有效的驗證物件，則在嘗試註冊工作之後，可能會當機。

## <a name="manage-resources-for-your-background-task"></a>管理背景工作的資源

使用 [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) 可判斷使用者是否已決定限制您的應用程式的背景活動。 請留意您的電池使用量，並且只在需要完成使用者想要的動作時才在背景執行。 如需有關使用者可如何控制背景活動設定的詳細資訊，請參閱[最佳化背景活動](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)。

- 記憶體：微調您的應用程式的記憶體和能源使用是確保作業系統可讓您執行的背景工作的關鍵。 使用[記憶體管理 API](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) 來了解您的背景工作正在使用多少記憶體。 您的背景工作使用的記憶體越多，作業系統就越不容易在有其他 App 執行於前景時允許您的 App 繼續執行。 最終是由使用者控制您的應用程式可執行的所有背景活動，而且他也能看到您的應用程式對電池使用量的影響。  
- CPU 時間：背景工作會受到時鐘取得根據觸發程序類型的使用時間數量。

請參閱[使用背景工作支援應用程式](support-your-app-with-background-tasks.md)，以了解套用至背景工作的資源限制。

## <a name="remarks"></a>備註

從 Windows 10 開始，它不再需要將您的應用程式新增至在鎖定畫面中，才能利用背景工作的使用者。

如果您已經先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)，背景工作將只會使用 **TimeTrigger** 來執行。

## <a name="related-topics"></a>相關主題

* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [背景工作的程式碼範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [建立及註冊同處理序的背景工作](create-and-register-an-inproc-background-task.md)
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
