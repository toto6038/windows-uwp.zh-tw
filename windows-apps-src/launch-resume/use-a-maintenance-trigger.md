---
author: TylerMSFT
title: "使用維護觸發程序"
description: "了解如何在裝置使用 AC 電源時，使用 MaintenanceTrigger 類別於背景中執行輕量型程式碼。"
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 174fcd8c7413b502fe2bc2476f5688a73984d7aa
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="use-a-maintenance-trigger"></a>使用維護觸發程序

\[ 針對 Windows10 上的 UWP 應用程式更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要 API**

-   [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)

了解如何在裝置使用 AC 電源時，使用 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 類別於背景中執行輕量型程式碼。

## <a name="create-a-maintenance-trigger-object"></a>建立維護觸發程序物件

這個範例假設您已經有能夠在裝置使用 AC 電源時，於背景中執行以強化應用程式的輕量型程式碼。 本主題將焦點放在 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) (與 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 類似)。

如需有關撰寫背景工作類別的詳細資訊，請參閱[建立及註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)或[建立及註冊跨處理序背景工作](create-and-register-a-background-task.md)。

建立新的 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) 物件。 第二個參數 *OneShot* 會指定維護工作將只執行一次，還是要繼續定期執行。 如果 *OneShot* 設定成 True，第一個參數 (*FreshnessTime*) 會指定排定背景工作之前要等待的時間 (以分鐘為單位)。 如果 *OneShot* 設定成 False，則 *FreshnessTime* 會指定背景工作的執行頻率。

> **備註**  如果 *FreshnessTime* 設定為少於 15 分鐘，則嘗試登錄背景工作時會擲回例外狀況。

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

## <a name="optional-add-a-condition"></a>(選用) 新增條件

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

## <a name="register-the-background-task"></a>登錄背景工作

-   呼叫背景工作登錄函式以登錄背景工作。 如需有關登錄背景工作的詳細資訊，請參閱[登錄背景工作](register-a-background-task.md)。

    下列程式碼會登錄維護工作。 請注意，它會假設您的背景工作與您的應用程式在個別的處理程序中執行，因為它指定了 `entryPoint`。 如果您的背景工作與您的應用程式在相同處理程序中執行，您就不需指定 `entryPoint`。

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

    > **備註**  就傳統型裝置以外的所有裝置系列而言，當裝置的記憶體變成不足時，背景工作就可能終止。 如果沒有顯示記憶體不足的例外狀況，或是應用程式沒有處理該狀況，背景工作將會在沒有警告也沒有引發 OnCanceled 事件的情況下終止。 這有助於確保前景應用程式的使用者體驗。 您的背景工作應該要設計成能夠處理這種情況。

    > **備註**  通用 Windows 應用程式在登錄任何背景觸發程序類型之前，必須先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

    為了確保您的通用 Windows 應用程式在您對應用程式發行更新之後繼續正常執行，您必須呼叫 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然後在應用程式於更新後啟動時呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 如需詳細資訊，請參閱[背景工作的指導方針](guidelines-for-background-tasks.md)。

    > **備註**  背景工作登錄參數會在登錄時受到驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 請確認您的應用程式能夠妥善處理背景工作註冊失敗的狀況；反之，如果應用程式需依賴有效的驗證物件，則在嘗試註冊工作之後，可能會當機。


> **備註**  本文章適用於撰寫通用 Windows 平台 (UWP) 應用程式的 Windows 10 開發人員。 如果您是為 Windows8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

## <a name="related-topics"></a>相關主題

****

* [建立及註冊同處理序序背景工作](create-and-register-an-inproc-background-task.md)。
* [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [登錄背景工作](register-a-background-task.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [偵錯背景工作](debug-a-background-task.md)
* [如何在 Windows 市集應用程式觸發暫停、繼續以及背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)
