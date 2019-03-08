---
title: 即時活動的服務
description: 深入了解 Xbox Live 的即時活動的服務。
ms.assetid: 50de262f-fc55-4301-83b5-0a8a30bc7852
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 即時活動的服務。
ms.localizationpriority: medium
ms.openlocfilehash: 36389fac3bd6dea2d2e24c0935087781118d8046
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592083"
---
# <a name="real-time-activity-service"></a>即時活動的服務

即時活動 (RTA) 服務允許在訂閱狀態資料、 使用者統計資料和目前狀態的任何裝置上的應用程式。 系統會允許訂用帳戶，其資料和其他人的資料，取決於其隱私權設定任何標題中。 這樣資訊的流程而不必持續輪詢以取得最新的資料。


## <a name="developer-scenarios"></a>開發人員案例

有許多 RTA 支援的案例。 此處，列出幾個，但 RTA 的真正威力是您將會提供我們尚未還假設的許多案例。 您可以協助定義下一代的遊戲，使用者通常會有手邊其 Microsoft Surface 或 Apple iPad 時，主控台標題與它們互動。 RTA 使用 WebSocket 技術，讓各種不同的子主題逐步解說包含使用 Windows 所提供的 Websocket API 實作的概觀。

以下是一些簡單的案例，您可以使用 RTA 建立您的標題：

-   成就進行應用程式
-   遊戲說明應用程式
-   警察局的刑事組檢視器應用程式
-   統計資料檢視器
-   目前狀態檢視器


## <a name="achievements-progress-app"></a>成就進行應用程式

使用者幾乎一定會想知道特定的成就，尤其是需要執行動作的次數有一定數量的成績追蹤其進度的。 即時存取使用者的統計資料 （這會彙總的 Xbox Live Player 統計資料服務），您可以呈現即時進度，播放程式和其好友朝向成就與里程碑，Xbox One 上或隨附在裝置上，同時使用者播放您的標題。


## <a name="game-help-app"></a>遊戲說明應用程式

當使用者瀏覽您的標題，即時資料的存取權可讓您呈現遊戲的說明體驗旁的其中一個 Xbox One 或任何隨附裝置上。 使用者可能會出現新的對應、 新的追蹤，或具有挑戰性的老闆辯論，和您的遊戲協助小幫手可以顯示開發人員產生或使用者產生的影片和文字來協助使用者透過在標題中的體驗。


## <a name="squad-viewer-app"></a>警察局的刑事組檢視器應用程式

在聯合多玩家遊戲，玩家和他或她的小組成員一起運作共用的目標。 這麼多的播放程式，它可能難以追蹤的較大的圖片。 存取即時資料，您可以建立顯示高層級的對應和熱度圖對應的動作可能是其中小幫手應用程式。


## <a name="statistics-viewer"></a>統計資料檢視器

雖然一般認為的附屬應用程式，考慮 RTA 時，您也可以使用 RTA core 標題中。 例如，您可以使用 RTA，為提供的多人遊戲播放程式每個人的目前統計資料顯示在遊戲中，可能是由播放程式只需按下多人遊戲的比對中的控制站上的 [檢視] 按鈕。


## <a name="presence-viewer"></a>目前狀態檢視器

在 大廳，最好有哪些朋友在線上，而且如果它們播放相同的標題的最新檢視。 您可以訂閱的使用者朋友的目前狀態，並顯示哪些朋友網上都會推出，如果它們開始播放您的標題，全都在即時。


## <a name="subscription-privacy-and-authorization"></a>訂用帳戶的隱私權與授權

RTA 的最新版本包括檢查隱私權與授權/內容隔離。 只要符合隱私權與授權檢查，您的應用程式可以訂閱會標示為 RTA 啟用任何統計資料。 (如需有關如何讓統計資料啟用 RTA，請參閱[註冊狀態的通知](register-for-stat-notifications.md)。 沒有任何限制的統計資料，可啟用 RTA 的種類，它是由開發人員。 不過，沒有限制*數字*的使用者可以訂閱每個應用程式工作階段的統計資料。 如果使用者達到該限制時，他或她會收到下一個訂用帳戶的錯誤。


## <a name="in-this-section"></a>本節內容

[播放程式狀態變更通知註冊](register-for-stat-notifications.md)  
描述如何啟用統計資料或狀態資訊的即時活動 (RTA)。

[程式設計使用 winrt api 的即時活動 (rta) 服務](programming-the-real-time-activity-service.md)  
描述如何使用 WinRT Api 的即時活動 (RTA) 服務進行程式設計。

[程式設計使用 restful 介面的即時活動 (rta) 服務](programming-the-real-time-activity-service.md)  
描述如何使用 RESTful 介面的即時活動 (RTA) 服務進行程式設計。

[即時活動 (rta) 的最佳作法](rta-best-practices.md)  
若要訂閱的 Xbox 資料平台的統計資料和狀態資料使用 Xbox 即時活動 (RTA) 服務的最佳做法。
