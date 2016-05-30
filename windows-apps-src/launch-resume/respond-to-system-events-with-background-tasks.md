---
author: mcleblanc
title: 使用背景工作回應系統事件
description: 了解如何建立回應 SystemTrigger 事件的背景工作。
ms.assetid: 43C21FEA-28B9-401D-80BE-A61B71F01A89
---

# 使用背景工作回應系統事件


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)

了解如何建立回應 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 事件的背景工作。

這個主題假設您有為 app 撰寫的背景工作類別，且這個工作需要執行以回應系統所觸發的事件，例如網際網路變成可用或是使用者登入。 這個主題的重點是 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 類別。 如需撰寫背景工作類別的詳細資訊，請參閱[建立並登錄背景工作](create-and-register-a-background-task.md)。

## 建立 SystemTrigger 物件


-   在應用程式程式碼中，建立新的 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 物件。 第一個參數 *triggerType* 指定將啟用這個背景工作的系統事件觸發程序類型。 如需事件類型清單，請參閱 [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839)。

    第二個參數 *OneShot* 指定執行背景工作的時間為下一次系統事件發生且觸發背景工作時，或是在工作解除登錄前每次系統事件發生時。

    下列程式碼指定每當網際網路變成可用時就執行背景工作：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemTrigger internetTrigger = new SystemTrigger(SystemTriggerType.InternetAvailable, false);
    > ```
    > ```cpp
    > SystemTrigger ^ internetTrigger = ref new SystemTrigger(SystemTriggerType::InternetAvailable, false);
    > ```

## 登錄背景工作


-   呼叫背景工作登錄函式以登錄背景工作。 如需登錄背景工作的詳細資訊，請參閱[登錄背景工作](register-a-background-task.md)。

    下列程式碼會登錄背景工作：

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Internet-based background task";
    > 
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Internet-based background task";
    > 
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```

    > **注意** 通用 Windows app 在登錄任何背景觸發程序類型之前，必須先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

    為了確保您的通用 Windows app 會在您發行更新之後繼續正常執行，您必須呼叫 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然後在 app 於更新後啟動時呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 如需詳細資訊，請參閱[背景工作的指導方針](guidelines-for-background-tasks.md)。

    > **注意** 背景工作登錄參數會在登錄時受到驗證。 如果有任一個登錄參數無效，就會傳回錯誤。 確認您的 app 能夠妥善處理背景工作登錄失敗的狀況；反之，如果 app 依賴有效的驗證物件，嘗試登錄工作之後，可能會當機。

     

## 備註


若要查看背景工作登錄的執行方式，請下載[背景工作範例](http://go.microsoft.com/fwlink/p/?LinkId=618666)。

背景工作可以執行來回應 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 和 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 事件，但是您還是必須 [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)。 您也必須在登錄任何背景工作類型之前先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

App 可以登錄會回應 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)、[**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) 和 [**NetworkOperatorNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/br224831) 事件的背景工作，以便在即使 app 不在前景的情況下，也能夠與使用者進行即時通訊。 如需詳細資訊，請參閱[使用背景工作支援 app](support-your-app-with-background-tasks.md)。

> **注意**：本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 
## 相關主題


****

* [建立並登錄背景工作](create-and-register-a-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [登錄背景工作](register-a-background-task.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [背景工作的指導方針](guidelines-for-background-tasks.md)

****

* [偵錯背景工作](debug-a-background-task.md)
* [如何在 Windows 市集 app 觸發暫停、繼續以及背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 





<!--HONumber=May16_HO2-->


