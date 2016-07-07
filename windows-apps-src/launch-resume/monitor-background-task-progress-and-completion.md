---
author: TylerMSFT
title: "監視背景工作進度和完成"
description: "了解 app 如何辨識背景工作所報告的進度與完成。"
ms.assetid: 17544FD7-A336-4254-97DC-2BF8994FF9B2
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 07d69b63b272153bc784ed19166e649f80a98297

---

# 監視背景工作進度和完成


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)
-   [**BackgroundTaskProgressEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224785)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

了解 app 如何辨識背景工作所報告的進度與完成。 背景工作會與應用程式分離，而且它們會分開執行，但是應用程式程式碼可以監視背景工作的進度與完成。 為了執行這項作業，app 會訂閱本身已在系統中登錄的背景工作事件。

-   這個主題假設您有一個會登錄背景工作的 app。 若要快速開始建立背景工作，請參閱[建立並登錄背景工作](create-and-register-a-background-task.md)。 如需條件與觸發程序的深入資訊，請參閱[使用背景工作支援 app](support-your-app-with-background-tasks.md)。

## 建立事件處理常式，以處理完成的背景工作。


1.  建立事件處理常式函式，以處理完成的背景工作。 這個程式碼需要遵循特定的配置，其中包括 [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) 物件與 [**BackgroundTaskCompletedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224778) 物件。

    針對 OnCompleted 背景工作事件處理常式方法使用下列配置：

>  [!div class="tabbedCodeSnippets"]
>  ```cs
>  private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
>  {
>      // TODO: Add code that deals with background task completion.
>  }
>  ```
>  ```cpp
>  auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
>  {
>      // TODO: Add code that deals with background task completion.
>  };
>  ```

2.  [!div class="tabbedCodeSnippets"]

    將程式碼新增至會處理背景工作完成的事件處理常式。

    > [!div class="tabbedCodeSnippets"]
    >     ```cs
    >     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >     {
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >     {    
    >         UpdateUI();
    >     };
    >     ```

## 例如，[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)會更新 UI。


1.  [!div class="tabbedCodeSnippets"]     ```cs
    private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    {
        UpdateUI();
    }
    ```
    ```cpp
    auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    {    
        UpdateUI();
    };
    ``` 建立事件處理常式函式，以處理背景工作進度

    建立事件處理常式函式，以處理完成的背景工作。

    > [!div class="tabbedCodeSnippets"]
    >     ```cs
    >     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     }
    >     ```
    >     ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     };
    >     ```

2.  這個程式碼需要遵循特定的配置，其中包括 [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) 物件與 [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782) 物件：

    針對 OnProgress 背景工作事件處理常式方法使用下列配置：

    > [!div class="tabbedCodeSnippets"]
    >     [!div class="tabbedCodeSnippets"]     ```cs
    >     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     }
    ```
    ```cpp
    auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    {
        // TODO: Add code that deals with background task progress.
    };
    ```
    >
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         auto progress = "Progress: " + args->Progress + "%";
    >         BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    >
    >         UpdateUI();
    >     };
    >     ```

## 將程式碼新增至會處理背景工作完成的事件處理常式。


1.  例如，[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)會使用透過 *args* 參數所傳遞的進度狀態來更新 UI：

    [!div class="tabbedCodeSnippets"] ```cs private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args) { var progress = "進度: " + args.Progress + "%"; BackgroundTaskSample.SampleBackgroundTaskProgress = progress;

    > [!div class="tabbedCodeSnippets"]
    >     ```cs
    >     private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
    >     {
    >         task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
    >         task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    >     }
    >     ```
    >     使用新的和現有的背景工作來登錄事件處理函式。
    >
    >         task->Progress += ref new BackgroundTaskProgressEventHandler(progress);
    >         
    >
    >         auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >         {
    >             UpdateUI();
    >         };
    >
    >         task->Completed += ref new BackgroundTaskCompletedEventHandler(completed);
    >     }
    >     ```

2.  當應用程式第一次登錄背景工作時，它應該登錄以接收其進度和完成更新，以防應用程式在前景仍然為使用中時執行工作。 例如，[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)會在它登錄的每個背景工作上呼叫下列函式：

    [!div class="tabbedCodeSnippets"]     ```cs
    private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
    {
        task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
        task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    }
    ```
 ```cpp void SampleBackgroundTask::AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration^ task) { auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
           { auto progress = "進度: " + args-&gt;Progress + "%"; BackgroundTaskSample::SampleBackgroundTaskProgress = progress; UpdateUI(); };

    > [!div class="tabbedCodeSnippets"]
    >     當 app 啟動或是瀏覽至背景工作狀態是相關的新頁面時，它應該取得目前登錄的背景工作清單，並將它們與進度和完成事件處理函式關聯。
    >
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     void SampleBackgroundTask::OnNavigatedTo(NavigationEventArgs^ e)
    >     {
    >         // A pointer back to the main page.  This is needed if you want to call methods in MainPage such
    >         // as NotifyUser()
    >         rootPage = MainPage::Current;
    >
    >         //
    >         // Attach progress and completed handlers to any existing tasks.
    >         //
    >         auto iter = BackgroundTaskRegistration::AllTasks->First();
    >         auto hascur = iter->HasCurrent;
    >         while (hascur)
    >         {
    >             auto cur = iter->Current->Value;
    >
    >             if (cur->Name == SampleBackgroundTaskName)
    >             {
    >                 AttachProgressAndCompletedHandlers(cur);
    >                 break;
    >             }
    >
    >             hascur = iter->MoveNext();
    >         }
    >
    >         UpdateUI();
    >     }
    >     ```

## 應用程式目前登錄的背景工作清單保留在 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786).[**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) 屬性中。


****

* [例如，[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)會使用下列程式碼來附加瀏覽 SampleBackgroundTask 頁面時要執行的事件處理常式：](create-and-register-a-background-task.md)
* [[!div class="tabbedCodeSnippets"] ```cs protected override void OnNavigatedTo(NavigationEventArgs e) { foreach (var task in BackgroundTaskRegistration.AllTasks) { if (task.Value.Name == BackgroundTaskSample.SampleBackgroundTaskName) { AttachProgressAndCompletedHandlers(task.Value); BackgroundTaskSample.UpdateBackgroundTaskStatus(BackgroundTaskSample.SampleBackgroundTaskName, true); } }](declare-background-tasks-in-the-application-manifest.md)
* [相關主題](handle-a-cancelled-background-task.md)
* [建立並登錄背景工作](register-a-background-task.md)
* [在應用程式資訊清單中宣告背景工作](respond-to-system-events-with-background-tasks.md)
* [處理已取消的背景工作](set-conditions-for-running-a-background-task.md)
* [登錄背景工作](update-a-live-tile-from-a-background-task.md)
* [使用背景工作回應系統事件](use-a-maintenance-trigger.md)
* [設定執行背景工作的條件](run-a-background-task-on-a-timer-.md)
* [從背景工作更新動態磚](guidelines-for-background-tasks.md)

****

* [使用維護觸發程序](debug-a-background-task.md)
* [在計時器上執行背景工作](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO5-->


