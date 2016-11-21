---
author: TylerMSFT
title: "背景工作的指導方針"
description: "確保您的 app 符合執行背景工作的需求。"
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
translationtype: Human Translation
ms.sourcegitcommit: 04cb13ce355b3983a55b7f7f253e6b22a7cebece
ms.openlocfilehash: 04e8776852a68d2e15c0a08732da48756f403d15

---

# 背景工作的指導方針

\[ 針對 Windows10 上的 UWP app 更新。 如需 Windows8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

確保您的 app 符合執行背景工作的需求。

## 背景工作指引

在開發背景工作時，以及在發佈 app 之前，請考慮下列指引。

如果您使用背景工作在背景播放媒體，請參閱[在背景播放媒體](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio)，以了解有關 Windows10 版本 1607 中讓此操作更容易進行的改進功能詳細資訊。


  **同處理序與跨處理序背景工作︰**Windows10 (版本 1607) 引進的[同處理序背景工作](create-and-register-an-inproc-background-task.md)可讓您在與前景 app 相同的處理序中執行背景程式碼。 決定要使用同處理序或跨處理序的背景工作時，請考慮下列因素︰

|考量 | 影響 |
|--------------|--------|
|復原能力   | 如果您的背景處理序在另一個處理序中執行，背景處理序毀損並不會關閉您的前景應用程式。 此外，如果背景活動執行超過執行時間限制，即使在您的 app 內，也可以終止。 當前景和背景處理序不需要彼此通訊 (因為同處理序背景工作的主要優點之一，就是處理程間不需要通訊) 時，最好選擇將背景工作和前景 App 的工作分開。 |
|簡潔    | 同處理序背景工作不需要跨處理序通訊，撰寫上也比較不複雜。  |
|可用的觸發程序 | 同處理序背景工作不支援下列觸發程序︰[DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396)、[DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) 和 **IoTStartupTask**。 |
|VoIP | 同處理序背景工作不支援在應用程式內啟用 VoIP 背景工作。 |  

**CPU 配額：**背景工作會受到它們根據觸發程序類型所取得的實際執行使用時間所限制。 大部分的觸發程序有 30 秒的實際執行使用限制，而部分觸發程序擁有最多 10 分鐘的執行時間，以完成需要大量運算資源的工作。 背景工作應該要輕量，才能延長電池使用時間並為前景應用程式提供較佳的使用者體驗。 請參閱[使用背景工作支援 app](support-your-app-with-background-tasks.md)，以了解套用至背景工作的資源限制。

**管理背景工作：**您的 app 應該取得已登錄的背景工作清單、登錄進度與完成處理常式，並適當地處理這些事件。 您的背景工作類別應該報告進度、取消和完成。 如需詳細資訊，請參閱[處理已取消的背景工作](handle-a-cancelled-background-task.md)和[監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)。

**使用 [BackgroundTaskDeferral](https://msdn.microsoft.com/library/windows/apps/hh700499)：**如果背景工作類別執行非同步程式碼，請務必使用延遲。 否則，執行 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 方法 (若為同處理序背景工作的情況則為 [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 方法) 時，您可能會提早終止背景工作。 如需詳細資訊，請參閱[建立及註冊跨處理序背景工作](create-and-register-an-outofproc-background-task.md)。

或者，要求一個延遲並使用 **async/await** 來完成非同步方法呼叫。 請在 **await** 方法呼叫後關閉延遲。

**更新應用程式資訊清單：**對於在跨處理序中執行的背景工作，請在應用程式資訊清單中宣告每個背景工作，以及它所使用的觸發程序類型。 否則您的 App 將無法在執行階段註冊背景工作。

與前景 App 在相同處理序中執行的背景工作，不需要在應用程式資訊清單中宣告自己。 如需在資訊清單中宣告在跨處理序中執行的背景工作的詳細資訊，請參閱[在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)。

**準備 App 更新︰**如果要更新您的 App，請建立並註冊 **ServicingComplete** 背景工作 (請參閱 [SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839)) 來取消註冊舊版 App 的背景工作，並註冊新版的背景工作。 除了在前景內執行的內容以外，此時也很適合執行可能需要的 app 更新。

**執行背景工作的要求：**

> 
  **重要** 從 Windows10 開始，app 不再是於鎖定畫面上執行背景工作的先決條件之一。

通用 Windows 平台 (UWP) app 可以在不釘選到鎖定畫面上的情況下，執行所有支援的工作類型。 不過，app 必須先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)，才能登錄任何類型的背景工作。 如果使用者已明確地在裝置設定中拒絕您應用程式的背景工作權限，則這個方法會傳回 [**BackgroundAccessStatus.Denied**](https://msdn.microsoft.com/library/windows/apps/hh700439)。
## 背景工作檢查清單

*同時適用於同處理序和跨處理序的背景工作*

-   將背景工作與正確的觸發程序關聯。
-   新增條件以協助確認背景工作順利執行。
-   處理背景工作進度、完成和取消。
-   於應用程式啟動期間，重新登錄背景工作。 這可確保它們在第一次啟動應用程式時進行註冊。 它也提供了一種方式，偵測使用者是否 (在事件註冊失敗時) 已停用 app 的背景執行功能。
-   檢查背景工作登錄是否有錯誤。 在適當狀況下，嘗試以不同的參數值再次登錄背景工作。
-   針對桌上型電腦以外的所有裝置系列，如果裝置的記憶體變成不足，背景工作可能就會終止。 如果沒有顯示記憶體不足的例外狀況，或是 app 沒有處理該狀況，背景工作將會在沒有警告也沒有引發 OnCanceled 事件的情況下終止。 這有助於確保前景 app 的使用者體驗。 您的背景工作應該要設計成能夠處理這種情況。

*僅適用於跨處理序背景工作*

-   在 Windows 執行階段元件中建立您的背景工作。
-   請勿顯示背景工作快顯通知、磚以及徽章更新以外的 UI。
-   在 [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) 方法中，針對每個非同步方法呼叫要求延遲，並在方法完成時關閉它們。 或者，搭配 **async/await** 使用單一延遲。
-   使用永久性存放裝置在背景工作和 app 之間分享資料。
-   在應用程式資訊清單中宣告每個背景工作，以及它所使用的觸發程序類型。 確認進入點與觸發程序類型是否正確。
-   除非您要使用應該與 app 在相同內容中執行的觸發程序 (例如 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032))，否則請勿在資訊清單中指定 Executable 元素。

*僅適用於同處理序背景工作*

- 取消工作時，請確認 `BackgroundActivated` 事件處理常式在取消發生或終止整個處理序之前結束。
-   撰寫短期背景工作。 背景工作的使用時間是限制為 30 秒時鐘時間。
-   請勿仰賴背景工作的使用者互動。

## Windows：具有鎖定畫面功能的 app 背景工作檢查清單

針對能夠位於鎖定畫面上的應用程式開發背景工作時，請遵循這個指導方針。 遵循[鎖定畫面磚的指導方針和檢查清單](https://msdn.microsoft.com/library/windows/apps/hh465403)中的指導方針。

-   將您的應用程式開發成具有鎖定畫面功能的應用程式之前，請先確定應用程式是否一定要位於鎖定畫面上。 如需詳細資訊，請參閱[鎖定畫面概觀](https://msdn.microsoft.com/library/windows/apps/hh779720)。

-   確認 app 不在鎖定畫面上仍然可運作。

-   納入已向 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543)、[**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 或 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) 登錄的背景工作，並在應用程式資訊清單中宣告它。 確認進入點與觸發程序類型是否正確。 這是認證的必要步驟，而且可以讓使用者在鎖定畫面上放置 app。

**注意**  
本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows10 開發人員。 如果您是為 Windows8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

## 相關主題

* [建立及註冊同處理序序背景工作](create-and-register-an-inproc-background-task.md)。
* [建立及註冊跨處理序的背景工作](create-and-register-an-outofproc-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [在背景播放媒體](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [登錄背景工作](register-a-background-task.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [偵錯背景工作](debug-a-background-task.md)
* [如何在 Windows 市集 app 觸發暫停、繼續以及背景事件 (偵錯時)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Nov16_HO1-->


