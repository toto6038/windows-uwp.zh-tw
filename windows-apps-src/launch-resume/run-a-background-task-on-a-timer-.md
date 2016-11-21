---
author: TylerMSFT
title: "在計時器上執行背景工作"
description: "了解如何排程一次性的背景工作，或執行定期的背景工作。"
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
translationtype: Human Translation
ms.sourcegitcommit: 7d1c160f8b725cd848bf8357325c6ca284b632ae
ms.openlocfilehash: 1ad44208b3442e80212656db943ff088514cc954

---

# 在計時器上執行背景工作

\[ 針對 Windows10 上的 UWP app 更新。 如需 Windows8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要 API**

-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)
-   [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)

了解如何排程一次性的背景工作，或執行定期的背景工作。

-   這個範例假設您有需要定期或在特定時間執行以支援 app 的背景工作。 如果您已呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)，背景工作將只會使用 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) 來執行。
-   本主題假設您已經建立背景工作類別。 若要快速開始建立背景工作，請參閱[建立及註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)或[建立及註冊跨處理序背景工作](create-and-register-an-outofproc-background-task.md)。 如需條件與觸發程序的深入資訊，請參閱[使用背景工作支援 app](support-your-app-with-background-tasks.md)。

## 建立時間觸發程序

-   建立一個新的 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)。 第二個參數 *OneShot* 會指定背景工作將只執行一次，還是要繼續定期執行。 如果 *OneShot* 設定成 True，第一個參數 (*FreshnessTime*) 會指定排定背景工作之前要等待的時間 (以分鐘為單位)。 如果 *OneShot* 設定成 False，*FreshnessTime* 會指定背景工作的執行頻率。

    以傳統型或行動裝置系列為目標之「通用 Windows 平台」(UWP) app 的內建計時器會每隔 15 分鐘執行一次背景工作。

    -   如果 *FreshnessTime* 設定為 15 分鐘且 *OneShot* 為 True，就會將工作排定在從工作登錄之後的 15 到 30 分鐘之間開始，執行一次。 如果是設定為 25 分鐘且 *OneShot* 為 True，就會將工作排定在從工作登錄之後的 25 到 40 分鐘之間開始，執行一次。

    -   如果 *FreshnessTime* 設定為 15 分鐘且 *OneShot* 為 False，就會將工作排定在從工作登錄之後的 15 到 30 分鐘之間開始，每隔 15 分鐘執行一次。 如果是設定為 n 分鐘且 *OneShot* 為 False，就會將工作排定在從工作登錄之後的 n 到 n + 15 分鐘之間開始，每隔 n 分鐘執行一次。

    **注意**：如果 *FreshnessTime* 設定為少於 15 分鐘，則嘗試登錄背景工作時會擲回例外狀況。
 

    例如，這個觸發程序將導致背景工作一個小時執行一次：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
    > ```
    > ```cpp
    > TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
    > ```

## (選用) 新增條件

-   如有需要，可建立背景工作條件以控制何時執行工作。 在符合條件之前，條件會防止背景工作執行，如需詳細資訊，請參閱[設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)。

    這個範例中，條件是設成 **UserPresent**，因此觸發後工作只有在使用者作用中時才會執行一次。 如需可用條件的清單，請參閱 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
    > ```
    > ```cpp
    > SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent)
    > ```

##  呼叫 RequestAccessAsync()

-   嘗試登錄 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) 背景工作之前，請先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)。

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > BackgroundExecutionManager.RequestAccessAsync();
    > ```
    > ```cpp
    > BackgroundExecutionManager::RequestAccessAsync();
    > ```

## 登錄背景工作

-   呼叫背景工作登錄函式以登錄背景工作。 如需有關登錄背景工作的詳細資訊，請參閱[登錄背景工作](register-a-background-task.md)。

> [!Important]
> 針對與您 App 在相同處理程序中執行的背景工作，請勿設定 `entryPoint`。針對與您 App 在個別處理程序中執行的背景工作，請將 `entryPoint` 設定為命名空間、'.' 及包含您背景工作實作的類別名稱。

    The following code registers a background task that runs out-of-process:

    > > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Example hourly background task";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Example hourly background task";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```

    > **Note**  Background task registration parameters are validated at the time of registration. An error is returned if any of the registration parameters are invalid. Ensure that your app gracefully handles scenarios where background task registration fails - if instead your app depends on having a valid registration object after attempting to register a task, it may crash.


## 備註

> 
  **注意：**從 Windows10 開始，使用者不再需要將您的 app 新增到鎖定畫面，就可以使用背景工作。 如需有關背景工作觸發程序類型的指引，請參閱[使用背景工作支援 app](support-your-app-with-background-tasks.md)。

> 
  **注意**：本文章適用於撰寫通用 Windows 平台 (UWP) App 的 Windows10 開發人員。 如果您是為 Windows8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

## 相關主題

* [建立及註冊同處理序序背景工作](create-and-register-an-inproc-background-task.md)。
* [建立及註冊跨處理序的背景工作](create-and-register-an-outofproc-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [登錄背景工作](register-a-background-task.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [偵錯背景工作](debug-a-background-task.md)
* [如何在 Windows 市集 app 觸發暫停、繼續以及背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)



<!--HONumber=Nov16_HO1-->


