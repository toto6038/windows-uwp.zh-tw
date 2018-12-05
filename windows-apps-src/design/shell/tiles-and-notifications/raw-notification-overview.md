---
Description: Raw notifications are short, general purpose push notifications.
title: 原始通知概觀
ms.assetid: A867C75D-D16E-4AB5-8B44-614EEB9179C7
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ad00090fdfc3ce7be34ef6271d16e76541b584bb
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8706891"
---
# <a name="raw-notification-overview"></a>原始通知概觀


原始通知是簡短、一般用途的推播通知。 它們只是指示，不會包含 UI 元件。 正如其他推播通知一樣，Windows 推播通知服務 (WNS) 功能會將原始通知從雲端服務傳送到應用程式。

您可以將原始通知用於各種用途，包括觸發您的應用程式執行背景工作 (如果使用者授與應用程式執行背景工作的權限)。 使用 WNS 與應用程式通訊可以避免建立持續通訊端連線、傳送 HTTP GET 訊息以及其他服務對應用程式連線的額外負荷。

> [!IMPORTANT]
> 若要了解原始通知，最好能夠熟悉 [Windows 推播通知服務 (WNS) 概觀](windows-push-notification-services--wns--overview.md)中討論的概念。

 

至於使用快顯通知、磚及徽章推播通知時，原始通知是從您應用程式的雲端服務透過指派的通道統一資源識別元 (URI) 推播至 WNS。 然後，WNS 再將通知傳遞至與該通道關聯的裝置和使用者帳戶。 和其他推播通知不同的是，原始通知沒有特定的格式。 裝載的內容完全由應用程式定義。

為了說明應用程式如何從原始通知獲益，讓我們看看虛構的文件協作應用程式。 假設有兩個使用者在同一個時間編輯同一份文件。 裝載共用文件的雲端服務，可以在其中一位使用者變更文件時使用原始通知來通知另一位使用者。 原始通知不一定會包含對文件所做的變更，但是會通知每個使用者應用程式連絡中央位置並同步可用的變更。 使用原始通知，應用程式及其雲端服務可以免去文件開啟期間持續保持連線的額外負荷。

## <a name="how-raw-notifications-work"></a>原始通知的運作方式


所有原始通知都是推播通知。 因此，傳送和接收推播通知所需的設定也適用於原始通知：

-   您必須具備有效的 WNS 通道才能傳送原始通知。 如需取得推播通知通道的詳細資訊，請參閱[如何要求、建立以及儲存通知通道](https://msdn.microsoft.com/library/windows/apps/hh465412)。
-   您必須在應用程式資訊清單中包含 **Internet** 功能。 在 Microsoft Visual Studio 資訊清單編輯器中，您會在 **\[功能\]** 索引標籤下找到此 **\[網際網路 (用戶端)\]** 選項。 如需詳細資訊，請參閱 [**Capabilities**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-capabilities)。

通知的本文是採用應用程式定義的格式。 用戶端會以 null 結尾字串 (**HSTRING**) 的形式接收資料，該字串只需要讓應用程式了解。

如果用戶端處於離線狀態，只有通知中包含 [X-WNS-Cache-Policy](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_cache) 標頭時，WNS 才會快取原始通知。 不過，只會快取一個原始通知，並在裝置恢復連線狀態後進行傳送。

在用戶端上原始通知只能使用三種可能的途徑進行傳送：透過通知傳送事件傳送到正在執行的應用程式、傳送到背景工作或捨棄。 所以，如果用戶端處於離線狀態且 WNS 嘗試傳送原始通知，則會捨棄通知。

## <a name="creating-a-raw-notification"></a>建立原始通知


傳送原始通知的方式與傳送磚、快顯通知或徽章推播通知類似，但是有一些差異：

-   HTTP Content-Type 標頭必須設定為 "application/octet-stream"。
-   HTTP [X-WNS-Type](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_type) 標頭必須設定為 "wns/raw"。
-   通知本文可以包含小於 5 KB 的任何字串裝載。

原始通知旨在做為可觸發應用程式採取動作的簡短訊息，例如直接連線服務以同步較大量的資料或根據通知內容修改本機狀態。 請注意，WNS 推播通知不保證一定能傳送，所以您的應用程式和雲端服務必須考慮原始通知可能不會送達用戶端的可能性，例如用戶端處於離線狀態時。

如需傳送推播通知的詳細資訊，請參閱[快速入門：傳送推播通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)。

## <a name="receiving-a-raw-notification"></a>接收原始通知


應用程式有兩個管道可以接收原始通知：

-   在應用程式執行時透過[通知傳送事件](#notification-delivery-events)接收。
-   如果應用程式能夠執行背景工作，透過[原始通知觸發的背景工作](#background-tasks-triggered-by-raw-notifications)接收。

應用程式可以使用這兩種機制來接收原始通知。 如果應用程式同時實作通知傳送事件處理常式以及由原始通知觸發的背景工作，則應用程式執行時會優先使用通知傳送事件。

-   如果應用程式正在執行，通知傳送事件會優先於背景工作，讓應用程式能夠儘速處理通知。
-   通知傳送事件處理常式可以將事件的 [**PushNotificationReceivedEventArgs.Cancel**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationReceivedEventArgs.Cancel) 屬性設為 **true**，指定在處理常式結束後，原始通知不應該傳送到應用程式的背景工作。 如果 **Cancel** 屬性設為 **false** 或未設定 (預設值為 **false**)，原始通知會在通知傳送事件處理常式完成工作後觸發背景工作。

### <a name="notification-delivery-events"></a>通知傳送事件

您的應用程式可以使用通知傳送事件 ([**PushNotificationReceived**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel.PushNotificationReceived)) 在應用程式使用中時接收原始通知。 當雲端服務傳送原始通知的時候，正在執行的應用程式可以在通道 URI 上處理通知傳送事件來接收通知。

如果應用程式沒有執行，也沒有使用[背景工作](#background-tasks-triggered-by-raw-notifications)，那麼傳送到該應用程式的任何原始通知，都會在收到後遭 WNS 捨棄。 為了避免浪費雲端服務的資源，您應該考慮在服務實作邏輯，以追蹤應用程式是否正在使用中。 可以從兩個來源獲得這個資訊：應用程式可以明確告訴服務它已準備好開始接收通知，以及 WNS 可以告訴服務何時停止。

-   **應用程式通知雲端服務**：應用程式可以連絡服務，讓它知道應用程式正在前景執行。 這種方式的缺點是應用程式可能會非常頻繁地連絡服務。 不過，它的優點是服務永遠可以知道應用程式何時準備好接收傳入的原始通知。 另一個優點是當應用程式連絡服務時，服務會知道要將原始通知傳送到該應用程式的特定執行個體而不是使用廣播。
-   **雲端服務回應 WNS 回應訊息**：應用程式服務可以使用 WNS 傳回的 [X-WNS-NotificationStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_notification) 與 [X-WNS-DeviceConnectionStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_dcs) 資訊，判斷何時停止傳送原始通知給應用程式。 服務以 HTTP POST 的形式將通知傳送到通道時，它可以在回應中接收下列其中一種訊息：

    -   **X-WNS-NotificationStatus: dropped**：這表示用戶端沒有接收到通知。 因此可以大膽假設收到 **dropped** 回應的原因是由於您的應用程式已不在使用者裝置的前景執行。
    -   **X-WNS-DeviceConnectionStatus: disconnected** 或 **X-WNS-DeviceConnectionStatus: tempconnected**：這表示 Windows 用戶端與 WNS 中斷連線。 請注意，若要從 WNS 接收這個訊息，您必須在通知的 HTTP POST 中設定 [X-WNS-RequestForStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_request) 標頭來要求該訊息。

    應用程式的雲端服務可以使用這些狀態訊息中的資訊，停止使用原始通知進行通訊。 當應用程式回到前景並連線服務時，服務就可以繼續傳送原始通知。

    請注意，您不應該依賴 [X-WNS-NotificationStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_notification) 來判斷通知是否已經成功傳送到用戶端。

    如需詳細資訊，請參閱[推播通知服務要求和回應標頭](https://msdn.microsoft.com/library/windows/apps/hh465435)

### <a name="background-tasks-triggered-by-raw-notifications"></a>原始通知觸發的背景工作

> [!IMPORTANT]
> 應用程式必須先透過 [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_) 獲得授予背景存取權，才能使用原始通知背景工作。

 

您的背景工作必須使用 [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) 進行登錄。 如果沒有登錄，收到原始通知時將不會執行工作。

原始通知觸發的背景工作，能夠讓應用程式的雲端服務連絡應用程式，即使沒有執行應用程式也一樣 (儘管可能會觸發應用程式執行)。 應用程式不需要持續保持連線。 原始通知是唯一可以觸發背景工作的通知類型。 不過，雖然快顯通知、磚及徽章推播通知無法觸發背景工作，但是原始通知觸發的背景工作可以透過本機 API 呼叫來更新磚及叫用快顯通知。

為了說明原始通知如何觸發背景工作，讓我們看看用來閱讀電子書的應用程式。 首先，使用者可能使用其他裝置在線上購買一本書。 應用程式雲端服務的回應是傳送原始通知給使用者的每個裝置，其中包含說明已購買書籍，且應用程式應下載該書籍的裝載。 接著，應用程式會直接連絡應用程式的雲端服務，開始在背景下載這本新書，如此一來，當使用者啟動應用程式時，這本書就會已經在裝置中，而且可以隨時閱讀。

若要使用原始通知觸發背景工作，您的應用程式必須：

1.  使用 [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_) 要求在背景執行工作的權限 (使用者可隨時撤銷)。
2.  實作背景工作。 如需詳細資訊，請參閱[使用背景工作支援 app](../../../launch-resume/support-your-app-with-background-tasks.md)

之後，每次收到應用程式的原始通知時，就會叫用背景工作來回應 [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)。 背景工作會轉譯原始通知的應用程式特定裝載，並據此採取動作。

每個應用程式一次只能執行一個背景工作。 如果對已經執行背景工作的應用程式觸發背景工作，必須先完成第一個背景工作，才能執行新的工作。

## <a name="other-resources"></a>其他資源


您可以進一步了解透過 Windows8.1，以及[推播和定期通知範例](http://go.microsoft.com/fwlink/p/?LinkId=231476)下載[原始通知的範例](http://go.microsoft.com/fwlink/p/?linkid=241553)，如 Windows8.1，並重複使用其原始程式碼，在 windows 10 應用程式中。

## <a name="related-topics"></a>相關主題

* [原始通知的指導方針](https://msdn.microsoft.com/library/windows/apps/hh761463)
* [快速入門：建立和登錄原始通知背景工作](https://msdn.microsoft.com/library/windows/apps/jj676800)
* [快速入門：攔截執行中應用程式的推播通知](https://msdn.microsoft.com/library/windows/apps/jj709908)
* [**RawNotification**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.RawNotification)
* [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_)
 

 




