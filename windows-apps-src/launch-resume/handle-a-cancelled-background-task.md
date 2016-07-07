---
author: TylerMSFT
title: "處理已取消的背景工作"
description: "了解如何讓可辨識取消要求並停止工作的背景工作，使用永續性儲存體向 app 回報取消。"
ms.assetid: B7E23072-F7B0-4567-985B-737DD2A8728E
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: ab575415e5e6a091fb45dab49af21d0552834406

---

# 處理已取消的背景工作

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要 API**

-   [**BackgroundTaskCanceledEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224775)
-   [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)
-   [**ApplicationData.Current**](https://msdn.microsoft.com/library/windows/apps/br241619)

了解如何讓可辨識取消要求並停止工作的背景工作，使用永續性儲存體向 app 回報取消。

> **注意** 針對桌上型電腦以外的所有裝置系列，如果裝置的記憶體變成不足，背景工作可能就會終止。 如果沒有顯示記憶體不足的例外狀況，或是 app 沒有處理該狀況，背景工作將會在沒有警告也沒有引發 OnCanceled 事件的情況下終止。 這有助於確保前景 app 的使用者體驗。 您的背景工作應該要設計成能夠處理這種情況。

本主題假設您已建立背景工作類別，包括做為背景工作進入點的 Run 方法。 若要快速開始建立背景工作，請參閱[建立並登錄背景工作](create-and-register-a-background-task.md)。 如需條件與觸發程序的深入資訊，請參閱[使用背景工作支援 app](support-your-app-with-background-tasks.md)。

## 使用 OnCanceled 方法辨識取消要求

撰寫方法以處理取消事件。

建立具有下列配置且名為 OnCanceled 的方法。 這個方法是每當針對您的背景工作提出取消要求時，由 Windows 執行階段呼叫的進入點。

OnCanceled 方法必須具有下列配置：

> [!div class="tabbedCodeSnippets"]
> ```cs
>    private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
>    {
>        // TODO: Add code to notify the background task that it is cancelled.
>    }
> ```
> ```cpp
>    void ExampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
>    {
>        // TODO: Add code to notify the background task that it is cancelled.
>    }
> ```

[!div class="tabbedCodeSnippets"] 將稱為 **\_CancelRequested** 的旗標變數新增至背景工作類別。

> [!div class="tabbedCodeSnippets"]
> ```cs
>   volatile bool _CancelRequested = false;
> ```
> ```cpp
>   private:
>     volatile bool CancelRequested;
> ```

此變數將用來指示何時提出取消要求。

[!div class="tabbedCodeSnippets"]

> [!div class="tabbedCodeSnippets"]
> ```cs
>     private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
>     {
>         //
>         // Indicate that the background task is canceled.
>         //
>
>         _cancelRequested = true;
>
>         Debug.WriteLine("Background " + sender.Task.Name + " Cancel Requested...");
>     }
> ```
> ```cpp
>     void SampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
>     {
>         //
>         // Indicate that the background task is canceled.
>         //
>
>         CancelRequested = true;
>     }
> ```

在步驟 1 所建立的 OnCanceled 方法中，將旗標變數 **\_CancelRequested** 設定為 **true**。 完整的[背景工作範例]( http://go.microsoft.com/fwlink/p/?linkid=227509) OnCanceled 方法會將 **\_CancelRequested** 設定為 **true** 並撰寫可能會用到的偵錯輸出：

> [!div class="tabbedCodeSnippets"]
> ```cs
>     taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
> ```
> ```cpp
>     taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &SampleBackgroundTask::OnCanceled);
> ```

## [!div class="tabbedCodeSnippets"]


在背景工作的 Run 方法中，在開始工作之前先登錄 OnCanceled 事件處理常式方法。

例如，使用下面一行的程式碼： [!div class="tabbedCodeSnippets"]

結束 Run 方法以處理取消

> [!div class="tabbedCodeSnippets"]
> ```cs
>     if ((_cancelRequested == false) && (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
>
>         // TODO: Record whether the task completed or was cancelled.
>     }
> ```
> ```cpp
>     if ((CancelRequested == false) && (Progress < 100))
>     {
>         Progress += 10;
>         TaskInstance->Progress = Progress;
>     }
>     else
>     {
>         PeriodicTimer->Cancel();
>
>         // TODO: Record whether the task completed or was cancelled.
>     }
> ```

> 接收取消要求時，Run 方法需要透過辨識 **\_cancelRequested** 被設定為 **true** 的時機，來停止工作並結束。 修改背景工作類別的程式碼以便在旗標變數運作時檢查旗標變數。

如果將 **\_cancelRequested** 設定為 true，便會阻止工作繼續。

[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)包括會在取消背景工作時，停止定期計時器回呼的檢查：

> [!div class="tabbedCodeSnippets"]
> ```cs
>     if ((_cancelRequested == false) && (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
>
>         var settings = ApplicationData.Current.LocalSettings;
>         var key = _taskInstance.Task.TaskId.ToString();
>
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>
>         if (_cancelRequested)
>         {
>             settings.Values[key] = "Canceled";
>         }
>         else
>         {
>             settings.Values[key] = "Completed";
>         }
>         
>         Debug.WriteLine("Background " + _taskInstance.Task.Name + (_cancelRequested ? " Canceled" : " Completed"));
>         
>         //
>         // Indicate that the background task has completed.
>         //
>
>         _deferral.Complete();
>     }
> ```
> ```cpp
>     if ((CancelRequested == false) && (Progress < 100))
>     {
>         Progress += 10;
>         TaskInstance->Progress = Progress;
>     }
>     else
>     {
>         PeriodicTimer->Cancel();
>         
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>         
>         auto settings = ApplicationData::Current->LocalSettings;
>         auto key = TaskInstance->Task->Name;
>         settings->Values->Insert(key, (Progress < 100) ? "Canceled" : "Completed");
>         
>         //
>         // Indicate that the background task has completed.
>         //
>         
>         Deferral->Complete();
>     }
> ```

## [!div class="tabbedCodeSnippets"]

**注意** 上方所顯示的程式碼範例使用用來記錄背景工作進度的 [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797).[**Progress**](https://msdn.microsoft.com/library/windows/apps/br224800) 屬性。

進度會透過 [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782) 類別回報給 app。

## 修改 Run 方法，以便在停止工作後，使它記錄工作是否已完成或被取消。

[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)會在 LocalSettings 中記錄狀態：

> [!div class="tabbedCodeSnippets"]
> ```cs
> //
> // The Run method is the entry point of a background task.
> //
> public void Run(IBackgroundTaskInstance taskInstance)
> {
>     Debug.WriteLine("Background " + taskInstance.Task.Name + " Starting...");
>
>     //
>     // Query BackgroundWorkCost
>     // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
>     // of work in the background task and return immediately.
>     //
>     var cost = BackgroundWorkCost.CurrentBackgroundWorkCost;
>     var settings = ApplicationData.Current.LocalSettings;
>     settings.Values["BackgroundWorkCost"] = cost.ToString();
>
>     //
>     // Associate a cancellation handler with the background task.
>     //
>     taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
>
>     //
>     // Get the deferral object from the task instance, and take a reference to the taskInstance;
>     //
>     _deferral = taskInstance.GetDeferral();
>     _taskInstance = taskInstance;
>
>     _periodicTimer = ThreadPoolTimer.CreatePeriodicTimer(new TimerElapsedHandler(PeriodicTimerCallback), TimeSpan.FromSeconds(1));
> }
>
> //
> // Simulate the background task activity.
> //
> private void PeriodicTimerCallback(ThreadPoolTimer timer)
> {
>     if ((_cancelRequested == false) && (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
>
>         var settings = ApplicationData.Current.LocalSettings;
>         var key = _taskInstance.Task.Name;
>
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>         settings.Values[key] = (_progress < 100) ? "Canceled with reason: " + _cancelReason.ToString() : "Completed";
>         Debug.WriteLine("Background " + _taskInstance.Task.Name + settings.Values[key]);
>
>         //
>         // Indicate that the background task has completed.
>         //
>         _deferral.Complete();
>     }
> }
> ```
> ```cpp
> void SampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
> {
>     //
>     // Query BackgroundWorkCost
>     // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
>     // of work in the background task and return immediately.
>     //
>     auto cost = BackgroundWorkCost::CurrentBackgroundWorkCost;
>     auto settings = ApplicationData::Current->LocalSettings;
>     settings->Values->Insert("BackgroundWorkCost", cost.ToString());
>
>     //
>     // Associate a cancellation handler with the background task.
>     //
>     taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &SampleBackgroundTask::OnCanceled);
>
>     //
>     // Get the deferral object from the task instance, and take a reference to the taskInstance.
>     //
>     TaskDeferral = taskInstance->GetDeferral();
>     TaskInstance = taskInstance;
>
>     auto timerDelegate = [this](ThreadPoolTimer^ timer)
>     {
>         if ((CancelRequested == false) &&
>             (Progress < 100))
>         {
>             Progress += 10;
>             TaskInstance->Progress = Progress;
>         }
>         else
>         {
>             PeriodicTimer->Cancel();
>
>             //
>             // Write to LocalSettings to indicate that this background task ran.
>             //
>             auto settings = ApplicationData::Current->LocalSettings;
>             auto key = TaskInstance->Task->Name;
>             settings->Values->Insert(key, (Progress < 100) ? "Canceled with reason: " + CancelReason.ToString() : "Completed");
>
>             //
>             // Indicate that the background task has completed.
>             //
>             TaskDeferral->Complete();
>         }
>     };
>
>     TimeSpan period;
>     period.Duration = 1000 * 10000; // 1 second
>     PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(ref new TimerElapsedHandler(timerDelegate), period);
> }
> ```

> [!div class="tabbedCodeSnippets"] 備註

## 您可以下載[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)，查看方法內容中的這些程式碼範例。

* [為了便於說明，範例程式碼僅顯示[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)中 Run 方法 (以及回呼計時器) 的一部分。](create-and-register-a-background-task.md)
* [Run 方法範例](declare-background-tasks-in-the-application-manifest.md)
* [以下顯示[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)的完整的 Run方法以及回呼計時器程式碼的內容：](guidelines-for-background-tasks.md)
* [[!div class="tabbedCodeSnippets"]](monitor-background-task-progress-and-completion.md)
* [**注意**：本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。](register-a-background-task.md)
* [如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。](respond-to-system-events-with-background-tasks.md)
* [相關主題](run-a-background-task-on-a-timer-.md)
* [建立並登錄背景工作](set-conditions-for-running-a-background-task.md)
* [在應用程式資訊清單中宣告背景工作](update-a-live-tile-from-a-background-task.md)
* [背景工作的指導方針](use-a-maintenance-trigger.md)

* [監視背景工作進度和完成](debug-a-background-task.md)
* [登錄背景工作](http://go.microsoft.com/fwlink/p/?linkid=254345)



<!--HONumber=Jun16_HO5-->


