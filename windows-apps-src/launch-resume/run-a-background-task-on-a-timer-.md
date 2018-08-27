---
author: TylerMSFT
title: 在計時器上執行背景工作
description: 了解如何排程一次性的背景工作，或執行定期的背景工作。
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 背景工作
ms.localizationpriority: medium
ms.openlocfilehash: 25e3c76ae09ed6835f89f0d98c308f11c7a99624
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2018
ms.locfileid: "2862798"
---
# <a name="run-a-background-task-on-a-timer"></a>在計時器上執行背景工作

了解如何使用[**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)排程一次性背景工作，或執行定期背景工作。

請參閱**Scenario4** [背景啟用範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundActivation)查看如何實作觸發的背景工作所述的時間本主題中的範例中。

本主題假設您有背景工作需要定期，或在特定時間執行。 如果您不已經背景工作，會在[BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs)是範例背景工作。 或者，請遵循[建立與註冊程序在背景工作](create-and-register-an-inproc-background-task.md)或[建立與註冊程序 （英文） 背景工作](create-and-register-a-background-task.md)來建立 web 應用程式中的步驟。

## <a name="create-a-time-trigger"></a>建立時間觸發程序

建立一個新的 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)。 第二個參數 *OneShot* 會指定背景工作將只執行一次，還是要繼續定期執行。 如果 *OneShot* 設定成 True，第一個參數 (*FreshnessTime*) 會指定排定背景工作之前要等待的時間 (以分鐘為單位)。 如果 *OneShot* 設定成 False，*FreshnessTime* 會指定背景工作的執行頻率。

以傳統型或行動裝置系列為目標之「通用 Windows 平台」(UWP) app 的內建計時器會每隔 15 分鐘執行一次背景工作。 （計時器執行內的每隔 15 分鐘，讓系統只需要喚醒至喚醒要求進一步 TimerTriggers-儲存 power 應用程式一次每隔 15 分鐘。）

- 如果 *FreshnessTime* 設定為 15 分鐘且 *OneShot* 為 True，就會將工作排定在從工作登錄之後的 15 到 30 分鐘之間開始，執行一次。 如果是設定為 25 分鐘且 *OneShot* 為 True，就會將工作排定在從工作登錄之後的 25 到 40 分鐘之間開始，執行一次。

- 如果 *FreshnessTime* 設定為 15 分鐘且 *OneShot* 為 False，就會將工作排定在從工作登錄之後的 15 到 30 分鐘之間開始，每隔 15 分鐘執行一次。 如果是設定為 n 分鐘且 *OneShot* 為 False，就會將工作排定在從工作登錄之後的 n 到 n + 15 分鐘之間開始，每隔 n 分鐘執行一次。

> [!NOTE]
> 如果*FreshnessTime*設為小於 15 分鐘，嘗試註冊背景工作時為擲回例外。
 
例如，此觸發程序會導致執行一次一小時背景工作。

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

您可以建立控制項的背景工作條件時執行工作。 條件會防止背景工作執行之前，以符合條件。 如需詳細資訊，請參閱[設定執行背景工作中的條件](set-conditions-for-running-a-background-task.md)。

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

如需深入資訊條件和背景觸發程序類型，請參閱 ＜[支援您的應用程式與背景工作](support-your-app-with-background-tasks.md)。

##  <a name="call-requestaccessasync"></a>呼叫 RequestAccessAsync()

再登錄**ApplicationTrigger**背景工作，呼叫[**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)來判斷使用者可因為使用者可能已停用您的應用程式的背景活動的背景活動層級。 請參閱[最佳化背景活動](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)的詳細資訊的方式使用者可以控制背景活動的設定。

```cs
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
    // Depending on the value of requestStatus, provide an appropriate response
    // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>登錄背景工作

呼叫背景工作登錄函式以登錄背景工作。 如需登錄背景工作，以及查看**RegisterBackgroundTask()** 方法在下列範例程式碼中的定義的詳細資訊，請參閱[註冊背景工作](register-a-background-task.md)。

> [!IMPORTANT]
> 未設定的背景工作相同的程序為您的應用程式中執行， `entryPoint`。 背景工作從您的應用程式執行的不同程序，設定`entryPoint`是命名空間，'。 '，以及含有背景工作實作的類別的名稱。

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

背景工作登錄參數都是在登錄時驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 請確認您的 App 能夠妥善處理背景工作註冊失敗的狀況；反之，如果 App 需依賴有效的驗證物件，則在嘗試註冊工作之後，可能會當機。

## <a name="manage-resources-for-your-background-task"></a>管理資源背景工作

使用 [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) 可判斷使用者是否已決定限制您的應用程式的背景活動。 請留意您的電池使用量，並且只在需要完成使用者想要的動作時才在背景執行。 請參閱[最佳化背景活動](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)的詳細資訊的方式使用者可以控制背景活動的設定。

- 記憶體： 調整您的應用程式記憶體及能源使用為機碼，以確保作業系統會允許執行將背景工作。 若要查看背景工作正在使用的記憶體使用[記憶體管理 api （英文)](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) 。 更多的記憶體背景工作使用、 OS 保留其時另一個應用程式是在前景中執行的是更困難。 最終是由使用者控制您的應用程式可執行的所有背景活動，而且他也能看到您的應用程式對電池使用量的影響。  
- CPU 時間： 背景工作會受限於取得根據觸發類型牆時鐘流量時間量。

請參閱[使用背景工作支援應用程式](support-your-app-with-background-tasks.md)，以了解套用至背景工作的資源限制。

## <a name="remarks"></a>備註

開頭 Windows 10，它不再需要將您的應用程式新增至鎖定] 畫面上，以便使用背景工作的使用者。

背景工作才會執行使用**TimeTrigger**如果您已先呼叫[**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) 。

## <a name="related-topics"></a>相關主題

* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [背景工作的程式碼範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [建立和註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)
* [建立和註冊跨處理序背景工作](create-and-register-a-background-task.md)
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
