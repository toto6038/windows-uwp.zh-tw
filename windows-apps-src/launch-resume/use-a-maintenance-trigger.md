---
author: TylerMSFT
title: "使用維護觸發程序"
description: "了解如何在裝置使用 AC 電源時，使用 MaintenanceTrigger 類別於背景中執行輕量型程式碼。"
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 0da08ba5431f4d5c56d06657d3d6123a67ba5079

---

# 使用維護觸發程序


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)

了解如何在裝置使用 AC 電源時，使用 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 類別於背景中執行輕量型程式碼。

## 建立維護觸發程序物件


這個範例假設您已經有能夠在裝置使用 AC 電源時，於背景中執行以強化 app 的輕量型程式碼。 這個主題的重點是 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 類別 (與 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 類似)。 如需撰寫背景工作類別的詳細資訊，請參閱[建立並登錄背景工作](create-and-register-a-background-task.md)。

建立新的 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) 物件。 第二個參數 *OneShot* 指定維護工作是要執行一次，或是繼續定期執行。 如果 *OneShot* 設定成 True，第一個參數 (*FreshnessTime*) 會指定排定背景工作之前要等待的時間 (單位：分鐘)。 如果 *OneShot* 設定成 False，*FreshnessTime* 會指定背景工作的執行頻率。

> **注意** 如果 *FreshnessTime* 設定為少於 15 分鐘，則嘗試登錄背景工作時會擲回例外狀況。

 

這個範例程式碼會建立每小時執行一次的觸發程序：

> [!div class="tabbedCodeSnippets"]
> ```cs
> uint waitIntervalMinutes = 60;
>
> MaintenanceTrigger taskTrigger = new MaintenanceTrigger(waitIntervalMinutes, false);
> ```
> ```cpp
> unsigned int waitIntervalMinutes = 60;
>
> MaintenanceTrigger ^ taskTrigger = ref new MaintenanceTrigger(waitIntervalMinutes, false);
> ```

## (選擇性) 新增條件

-   如有需要，可建立背景工作條件以控制何時執行工作。 在符合條件之前，條件會防止背景工作執行，如需詳細資訊，請參閱[設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)。

    這個範例中，條件是設成 **InternetAvailable**，讓維護只有在網際網路可用 (或是網際網路變成可用時) 時才會執行。 如需可能的背景工作條件的清單，請參閱 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

    下列程式碼會將條件新增到維護工作建立器：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemCondition exampleCondition = new SystemCondition(SystemConditionType.InternetAvailable);
    > ```
    > ```cpp
    > SystemCondition ^ exampleCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
    > ```

## 登錄背景工作


-   呼叫背景工作登錄函式以登錄背景工作。 如需登錄背景工作的詳細資訊，請參閱[登錄背景工作](register-a-background-task.md)。

    下列程式碼會登錄維護工作：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Maintenance background task example";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Maintenance background task example";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
    > ```

    > **注意** 針對桌上型電腦以外的所有裝置系列，如果裝置的記憶體變成不足，背景工作可能就會終止。 如果沒有顯示記憶體不足的例外狀況，或是 app 沒有處理該狀況，背景工作將會在沒有警告也沒有引發 OnCanceled 事件的情況下終止。 這有助於確保前景 app 的使用者體驗。 您的背景工作應該要設計成能夠處理這種情況。

    > **注意** 通用 Windows app 在登錄任何背景觸發程序類型之前，必須先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

    為了確保您的通用 Windows app 會在您發行更新之後繼續正常執行，您必須呼叫 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然後在 app 於更新後啟動時呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 如需詳細資訊，請參閱[背景工作的指導方針](guidelines-for-background-tasks.md)。

    > **注意** 背景工作登錄參數會在登錄時受到驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 確認您的 app 能夠妥善處理背景工作登錄失敗的狀況；反之，如果 app 依賴有效的驗證物件，嘗試登錄工作之後，可能會當機。


> **注意**：本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

## 相關主題


****

* [建立並登錄背景工作](create-and-register-a-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [登錄背景工作](register-a-background-task.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)

****

* [偵錯背景工作](debug-a-background-task.md)
* [如何在 Windows 市集 app 觸發暫停、繼續以及背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO4-->


