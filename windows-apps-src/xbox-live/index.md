---
title: Xbox Live 開發人員指南
description: 了解如何使用 Xbox Live 服務，將您的遊戲連線到 Xbox Live 遊戲網路。
ms.date: 08/22/2017
ms.topic: article
keywords: 'windows 10, uwp, 遊戲, directx'
ms.localizationpriority: medium
---
# <a name="what-is-xbox-live"></a>什麼是 Xbox Live？

Xbox Live 是連接世界各地數以百萬計的玩家的首要遊戲網路。 您可以將 Xbox Live 新增到您的 Windows 10 或 Xbox One 遊戲，以便利用 Xbox Live 功能和服務。

透過 Xbox Live 創作者計畫，任何具有[合作夥伴中心](https://partner.microsoft.com/dashboard)帳戶的人員都可以建置已啟用 Xbox Live 的通用 Windows 平台 (UWP) 遊戲，該遊戲可以在 Windows 10 電腦和 Xbox One 主控台上執行。

對於想要善用完整 Xbox Live 體驗 (包括多人遊戲、成就和原生 Xbox 主控台開發) 的遊戲開發人員，[開發人員計畫概觀](developer-program-overview.md)中會詳述其他開發人員計畫。

以下是將 Xbox Live 新增到您的遊戲的一些理由：

- Xbox Live 聯合了 Xbox One 和 Windows 10 的遊戲玩家，讓遊戲玩家可和朋友一起玩遊戲，並與一大群玩家交流。
- Xbox Live 讓玩家藉由解鎖成就、分享史詩級遊戲剪輯、累積遊戲分數，以及使其化身更加完美，打造遊戲傳奇。
- Xbox Live 可讓玩家在另一部 Xbox One 或電腦上接續玩他們前次停止的遊戲，並從另一個裝置帶入其所有的存檔。
- Xbox Live 是以效能、速度和可靠性為訴求，每個月進行了超過 10 億場多人遊戲配對。
- 透過跨裝置多人遊戲，不論是使用 Xbox One 或 Windows 10 電腦，玩家都可以和朋友一起玩遊戲。

> [!note]
> 這些主題的適用對象為想要將 Xbox Live 支援新增到其遊戲的遊戲開發人員。 如果您要尋找消費性 Xbox Live 資訊，請參閱 [Xbox Live](https://www.xbox.com/live/)。

## <a name="how-xbox-live-works"></a>Xbox Live 運作方式

在技術層次上，Xbox Live 是可公開 Xbox Live 功能的微服務集合，例如個人資料、好友和顯示狀態、統計資料、排行榜、成就、多人遊戲及配對。 Xbox Live 資料會儲存在雲端並可使用 REST 端點和安全的 Websocket 來存取，而 Websocket 可從一組專為遊戲開發人員所設計的用戶端 API 存取。

除了 REST API，還有一些包裝 REST 功能的用戶端 API。 如需詳細資訊，請參閱 [Xbox Live API 簡介](introduction-to-xbox-live-apis.md)。

### <a name="get-started-with-xbox-live"></a>開始使用 Xbox Live

不論您是 UWP 或 Xbox 主控台開發人員，下列指南都可協助您開始進行 Xbox Live 開發。  另外還有設定遊戲引擎的指南。

| 主題                                                                                                                                             | 描述                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [開發人員計畫概觀](developer-program-overview.md) | 討論各種可促成 Xbox Live 開發的開發人員計畫。 |
| [開始使用 Xbox Live 創作者計畫](get-started-with-creators/get-started-with-xbox-live-creators.md) | 如何在 Xbox Live 創作者計畫中開始使用 Xbox Live。 |
| [以 ID@Xbox 或受控開發人員身分開始使用 Xbox Live](get-started-with-partner/get-started-with-xbox-live-partner.md) | 如何在 ID@Xbox 計畫中以開發人員身分開始使用 Xbox Live。 |

### <a name="using-xbox-live"></a>使用 Xbox Live

在您建立遊戲且基本概念可行後，本節會在您全心投入並開始撰寫程式碼之前提供所需的背景。

| 主題                                                                                                                                             | 描述                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [使用 Xbox Live](using-xbox-live/using-xbox-live.md) | 一旦設定好您的遊戲並整合 Xbox Live SDK，您就可以實作登入並深入了解 Xbox Live 程式設計。
| [呼叫 Xbox Live 的最佳做法](using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md) | 熟悉 Xbox Live 呼叫模式的基本概念和最佳做法，以確保您的遊戲表現良好，而且不會達到流量限制。
| [對 Xbox Live 服務 API 進行疑難排解](using-xbox-live/troubleshooting/troubleshooting-the-xbox-live-services-api.md) | 您可能遇到的常見問題，以及如何修正這些問題的建議。

### <a name="xbox-live-social-platform"></a>Xbox Live 社交平台

Xbox Live 社交功能可以自然地逐漸擴大您的目標客群，向超過 5 千 5 百萬戶的使用中使用者展開宣傳。  本節說明如何開始使用 Xbox Live 社交功能。

| 主題                                                                                                                                             | 描述                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Xbox Live 社交平台](social-platform/social-platform.md) | 如果您可讓使用者登入，即可開始使用 Xbox Live 社交功能，例如利用使用者的社交關係圖、豐富顯示狀態和其他功能。 |

### <a name="xbox-live-data-platform"></a>Xbox Live 資料平台

Xbox Live 資料平台可推動玩家統計資料、成就和排行榜的使用。  請閱讀這一系列的主題，深入了解如何在您的遊戲中使用這些功能。

| 主題                                                                                                                                             | 描述                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Xbox Live 資料平台](data-platform/data-platform.md) | 資料平台的簡短概觀，以及如何最適切地將統計資料、排行榜和成就併入您遊戲的指引。
| [玩家統計資料](leaderboards-and-stats-2017/player-stats.md) | 統計資料是排行榜的基礎。  了解如何在此定義和使用它們。
| [排行榜](leaderboards-and-stats-2017/leaderboards.md) | 以智慧方式合併排行榜，帶出使用者的競爭對手。
| [成就](achievements-2017/achievements.md) | 成就是 Xbox Live 最知名的其中一項功能，而且是玩家參與的絕佳動力。 了解如何在您的遊戲中使用它們。

### <a name="xbox-live-multiplayer-platform"></a>Xbox Live 多人遊戲平台

多人遊戲很適合用來延長您的遊戲存留期，並且讓遊戲體驗保持新鮮。  Xbox Live 提供了大量多人遊戲和配對功能。  您還有數個 API 選項，可提供各種層級的簡易性和彈性。

| 主題                                                                                                                                             | 描述                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Xbox Live 多人遊戲平台](multiplayer/multiplayer-intro.md) | 如果您是 Xbox Live 多人遊戲開發的新手，或不熟悉新的 API，例如多人遊戲管理員和 Xbox 整合式多人遊戲 (XIM)，則從這裡開始。 |
| [多人遊戲案例](multiplayer/multiplayer-scenarios.md) | 如何將多人遊戲併入您遊戲的建議和指引。 |
| [Xbox 整合式多人遊戲](multiplayer/xbox-integrated-multiplayer.md) | Xbox 整合式多人遊戲 (XIM) 是簡易的獨立式介面，可將多人遊戲、即時網路功能和聊天新增至您的遊戲。 |
| [多人遊戲管理員](multiplayer/multiplayer-manager.md) | 多人遊戲管理員提供著重於通用多人遊戲案例的 API。 |

### <a name="xbox-live-storage-platform"></a>Xbox Live 儲存平台

Xbox Live 儲存平台可提供遊戲儲存空間和連接的儲存空間。  這是兩個不同但互補的服務。  連接的儲存空間可讓您在雲端實作遊戲存檔，不論使用者在何處登入，這些存檔將在各裝置間漫遊。  遊戲儲存空間可讓您按照每個使用者或每個遊戲儲存資料的 Blob，而該資料 Blob 可由不同的使用者共用。

| 主題                                                                                                                                             | 描述                                                                                                   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| [Xbox Live 儲存平台](storage-platform/storage-platform.md) | 使用 Xbox Live 儲存服務，在雲端中儲存遊戲存檔、立即重播、使用者喜好設定和其他資料。 |
| [連接的儲存空間](storage-platform/connected-storage/connected-storage-technical-overview.md) | 連接的儲存空間概觀和程式設計指南。 |
| [遊戲儲存空間](storage-platform/xbox-live-title-storage/xbox-live-title-storage.md) | 遊戲儲存空間概觀和程式設計指南。 |
