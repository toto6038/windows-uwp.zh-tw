---
author: TylerMSFT
title: 處理已取消的背景工作
description: 了解如何讓可辨識取消要求並停止工作的背景工作，使用永續性儲存體向應用程式回報取消。
ms.assetid: B7E23072-F7B0-4567-985B-737DD2A8728E
ms.author: twhitney
ms.date: 07/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，背景工作
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 2c78f5f43d93002b90902a7f9e5a943c7239946c
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2018
ms.locfileid: "4148955"
---
# <a name="handle-a-cancelled-background-task"></a>處理已取消的背景工作

**重要 API**

-   [**BackgroundTaskCanceledEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224775)
-   [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)
-   [**ApplicationData.Current**](https://msdn.microsoft.com/library/windows/apps/br241619)

了解如何建立一個可辨識取消要求、停止工作並使用永續性儲存體向應用程式回報取消的背景工作。

本主題假設您已經建立背景工作類別，包括做為背景工作進入點的**Run**方法。 若要快速開始建立背景工作，請參閱[建立及註冊跨處理序背景工作](create-and-register-a-background-task.md)或[建立及註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)。 如需條件與觸發程序的深入資訊，請參閱[使用背景工作支援應用程式](support-your-app-with-background-tasks.md)。

本主題也適用於同處理序背景工作。 但而**Run**方法中，不是以**OnBackgroundActivated**。 同處理序背景工作並不需要您使用永續性儲存體來發出取消訊號，因為背景工作是在與您前景應用程式相同的處理序中執行，所以您可以使用應用程式狀態來傳達取消。

## <a name="use-the-oncanceled-method-to-recognize-cancellation-requests"></a>使用 OnCanceled 方法辨識取消要求

撰寫方法以處理取消事件。

> [!NOTE]
> 針對桌上型電腦以外的所有裝置系列，如果裝置的記憶體變成不足，背景工作可能就會終止。 如果沒有顯示記憶體不足的例外，或應用程式不會處理，則沒有警告也沒有引發 OnCanceled 事件情況下，將會終止背景工作。 這有助於確保前景應用程式的使用者體驗。 您的背景工作應該要設計成能夠處理這種情況。

依下列方式建立名為 **OnCanceled** 的方法。 這個方法是當針對您的背景工作提出取消要求時，由「Windows 執行階段」呼叫的進入點。

```csharp
private void OnCanceled(
    IBackgroundTaskInstance sender,
    BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

```cppwinrt
void ExampleBackgroundTask::OnCanceled(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance,
    Windows::ApplicationModel::Background::BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

```cpp
void ExampleBackgroundTask::OnCanceled(
    IBackgroundTaskInstance^ taskInstance,
    BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

將稱為 **\_CancelRequested** 的旗標變數新增至背景工作類別。 此變數將用來指示何時提出取消要求。

```csharp
volatile bool _CancelRequested = false;
```

```cppwinrt
private:
    volatile bool m_cancelRequested;
```

```cpp
private:
    volatile bool CancelRequested;
```

您在步驟 1 中建立的**OnCanceled**方法中設定旗標變數**\_CancelRequested**為**true**。

完整[背景工作範例]( http://go.microsoft.com/fwlink/p/?linkid=227509) **OnCanceled**方法將**\_CancelRequested**設**為 true** ，並撰寫可能會用偵錯輸出。

```csharp
private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    _cancelRequested = true;

    Debug.WriteLine("Background " + sender.Task.Name + " Cancel Requested...");
}
```

```cppwinrt
void ExampleBackgroundTask::OnCanceled(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance,
    Windows::ApplicationModel::Background::BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    m_cancelRequested = true;
}
```

```cpp
void ExampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    CancelRequested = true;
}
```

在背景工作的**Run**方法，請開始工作之前先註冊**OnCanceled**事件處理常式方法。 在同處理序背景工作中，您可以在應用程式初始化的過程中進行這項註冊。 例如，使用下列這行程式碼。

```csharp
taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
```

```cppwinrt
taskInstance.Canceled({ this, &ExampleBackgroundTask::OnCanceled });
```

```cpp
taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &ExampleBackgroundTask::OnCanceled);
```

## <a name="handle-cancellation-by-exiting-your-background-task"></a>藉由結束背景工作來處理取消

收到取消要求時，執行背景工作的方法需要透過辨識 **\_cancelRequested** 何時設定為 **true**，來停止工作並結束。 針對同處理序背景工作，這表示從**OnBackgroundActivated**方法傳回。 對跨處理序背景工作，這表示從**Run**方法傳回。

修改背景工作類別的程式碼，以便在旗標變數運作時檢查旗標變數。 如果**\_cancelRequested**變得設定為 true，便會阻止工作繼續。

[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)包括，取消背景工作時停止定期計時器回呼的檢查。

```csharp
if ((_cancelRequested == false) && (_progress < 100))
{
    _progress += 10;
    _taskInstance.Progress = _progress;
}
else
{
    _periodicTimer.Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

```cppwinrt
if (!m_cancelRequested && m_progress < 100)
{
    m_progress += 10;
    m_taskInstance.Progress(m_progress);
}
else
{
    m_periodicTimer.Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

```cpp
if ((CancelRequested == false) && (Progress < 100))
{
    Progress += 10;
    TaskInstance->Progress = Progress;
}
else
{
    PeriodicTimer->Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

> [!NOTE]
> 上述的程式碼範例會使用[**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)。用來記錄背景工作進度的[**進度**](https://msdn.microsoft.com/library/windows/apps/br224800)屬性。 進度會透過 [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782) 類別回報給應用程式。

修改**Run**方法，以便讓工作停止後，記錄工作已完成還是被取消。 此步驟適用於跨處理序背景工作，因為您需要一個當背景工作被取消時，可在處理序之間通訊的方法。 針對同處理序背景工作，您只能與應用程式分享狀態以指出工作已被取消。

[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)會在 LocalSettings 中記錄狀態。

```csharp
if ((_cancelRequested == false) && (_progress < 100))
{
    _progress += 10;
    _taskInstance.Progress = _progress;
}
else
{
    _periodicTimer.Cancel();

    var settings = ApplicationData.Current.LocalSettings;
    var key = _taskInstance.Task.TaskId.ToString();

    // Write to LocalSettings to indicate that this background task ran.
    if (_cancelRequested)
    {
        settings.Values[key] = "Canceled";
    }
    else
    {
        settings.Values[key] = "Completed";
    }
        
    Debug.WriteLine("Background " + _taskInstance.Task.Name + (_cancelRequested ? " Canceled" : " Completed"));
        
    // Indicate that the background task has completed.
    _deferral.Complete();
}
```

```cppwinrt
if (!m_cancelRequested && m_progress < 100)
{
    m_progress += 10;
    m_taskInstance.Progress(m_progress);
}
else
{
    m_periodicTimer.Cancel();

    // Write to LocalSettings to indicate that this background task ran.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
    auto key{ m_taskInstance.Task().Name() };
    settings.Values().Insert(key, (m_progress < 100) ? winrt::box_value(L"Canceled") : winrt::box_value(L"Completed"));

    // Indicate that the background task has completed.
    m_deferral.Complete();
}
```

```cpp
if ((CancelRequested == false) && (Progress < 100))
{
    Progress += 10;
    TaskInstance->Progress = Progress;
}
else
{
    PeriodicTimer->Cancel();
        
    // Write to LocalSettings to indicate that this background task ran.
    auto settings = ApplicationData::Current->LocalSettings;
    auto key = TaskInstance->Task->Name;
    settings->Values->Insert(key, (Progress < 100) ? "Canceled" : "Completed");
        
    // Indicate that the background task has completed.
    Deferral->Complete();
}
```

## <a name="remarks"></a>備註

您可以下載[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)，查看方法內容中的這些程式碼範例。

為了便於說明，範例程式碼僅顯示的**Run**方法 （以及回呼計時器） 從[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)。

## <a name="run-method-example"></a>Run 方法範例

完成**Run**方法，以及回呼計時器程式碼，請從[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)如下所示的內容。

```csharp
// The Run method is the entry point of a background task.
public void Run(IBackgroundTaskInstance taskInstance)
{
    Debug.WriteLine("Background " + taskInstance.Task.Name + " Starting...");

    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    var cost = BackgroundWorkCost.CurrentBackgroundWorkCost;
    var settings = ApplicationData.Current.LocalSettings;
    settings.Values["BackgroundWorkCost"] = cost.ToString();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // Get the deferral object from the task instance, and take a reference to the taskInstance;
    _deferral = taskInstance.GetDeferral();
    _taskInstance = taskInstance;

    _periodicTimer = ThreadPoolTimer.CreatePeriodicTimer(new TimerElapsedHandler(PeriodicTimerCallback), TimeSpan.FromSeconds(1));
}

// Simulate the background task activity.
private void PeriodicTimerCallback(ThreadPoolTimer timer)
{
    if ((_cancelRequested == false) && (_progress < 100))
    {
        _progress += 10;
        _taskInstance.Progress = _progress;
    }
    else
    {
        _periodicTimer.Cancel();

        var settings = ApplicationData.Current.LocalSettings;
        var key = _taskInstance.Task.Name;

        // Write to LocalSettings to indicate that this background task ran.
        settings.Values[key] = (_progress < 100) ? "Canceled with reason: " + _cancelReason.ToString() : "Completed";
        Debug.WriteLine("Background " + _taskInstance.Task.Name + settings.Values[key]);

        // Indicate that the background task has completed.
        _deferral.Complete();
    }
}
```

```cppwinrt
void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    auto cost{ Windows::ApplicationModel::Background::BackgroundWorkCost::CurrentBackgroundWorkCost() };
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
    std::wstring costAsString{ L"Low" };
    if (cost == Windows::ApplicationModel::Background::BackgroundWorkCostValue::Medium) costAsString = L"Medium";
    else if (cost == Windows::ApplicationModel::Background::BackgroundWorkCostValue::High) costAsString = L"High";
    settings.Values().Insert(L"BackgroundWorkCost", winrt::box_value(costAsString));

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled({ this, &ExampleBackgroundTask::OnCanceled });

    // Get the deferral object from the task instance, and take a reference to the taskInstance.
    m_deferral = taskInstance.GetDeferral();
    m_taskInstance = taskInstance;

    Windows::Foundation::TimeSpan period{ std::chrono::seconds{1} };
    m_periodicTimer = Windows::System::Threading::ThreadPoolTimer::CreatePeriodicTimer([this](Windows::System::Threading::ThreadPoolTimer timer)
    {
        if (!m_cancelRequested && m_progress < 100)
        {
            m_progress += 10;
            m_taskInstance.Progress(m_progress);
        }
        else
        {
            m_periodicTimer.Cancel();

            // Write to LocalSettings to indicate that this background task ran.
            auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
            auto key{ m_taskInstance.Task().Name() };
            settings.Values().Insert(key, (m_progress < 100) ? winrt::box_value(L"Canceled") : winrt::box_value(L"Completed"));

            // Indicate that the background task has completed.
            m_deferral.Complete();
        }
    }, period);
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    auto cost = BackgroundWorkCost::CurrentBackgroundWorkCost;
    auto settings = ApplicationData::Current->LocalSettings;
    settings->Values->Insert("BackgroundWorkCost", cost.ToString());

    // Associate a cancellation handler with the background task.
    taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &ExampleBackgroundTask::OnCanceled);

    // Get the deferral object from the task instance, and take a reference to the taskInstance.
    TaskDeferral = taskInstance->GetDeferral();
    TaskInstance = taskInstance;

    auto timerDelegate = [this](ThreadPoolTimer^ timer)
    {
        if ((CancelRequested == false) &&
            (Progress < 100))
        {
            Progress += 10;
            TaskInstance->Progress = Progress;
        }
        else
        {
            PeriodicTimer->Cancel();

            // Write to LocalSettings to indicate that this background task ran.
            auto settings = ApplicationData::Current->LocalSettings;
            auto key = TaskInstance->Task->Name;
            settings->Values->Insert(key, (Progress < 100) ? "Canceled with reason: " + CancelReason.ToString() : "Completed");

            // Indicate that the background task has completed.
            TaskDeferral->Complete();
        }
    };

    TimeSpan period;
    period.Duration = 1000 * 10000; // 1 second
    PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(ref new TimerElapsedHandler(timerDelegate), period);
}
```

## <a name="related-topics"></a>相關主題

- [建立及註冊同處理序序背景工作](create-and-register-an-inproc-background-task.md)。
- [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)
- [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
- [背景工作的指導方針](guidelines-for-background-tasks.md)
- [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
- [登錄背景工作](register-a-background-task.md)
- [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
- [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
- [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
- [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
- [使用維護觸發程序](use-a-maintenance-trigger.md)
- [偵錯背景工作](debug-a-background-task.md)
- [如何在 UWP 應用程式觸發暫停、繼續和背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)
