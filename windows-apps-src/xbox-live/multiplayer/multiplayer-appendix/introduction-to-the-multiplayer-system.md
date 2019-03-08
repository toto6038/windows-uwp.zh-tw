---
title: 多人遊戲系統簡介
description: 提供的 Xbox Live 的多人遊戲 2015年系統的高階簡介。
ms.assetid: d025bd2b-2ca4-4ba9-9394-4950d96ad264
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，多人遊戲 2015
ms.localizationpriority: medium
ms.openlocfilehash: 4a739aaa650a7086dfe58b1b8ca170e15b3b2ef0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629413"
---
# <a name="introduction-to-the-multiplayer-system"></a>多人遊戲系統簡介

| 注意                                                                                                                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 這篇文章是針對進階 API 使用方式。  做為起點，請看看[多人遊戲管理員 API](../multiplayer-manager.md)可大幅簡化開發。  請讓您知道是否您發現不支援的案例在多人遊戲管理員 中的補西牆。 |

本文件包含下列各節
* 關於多人遊戲系統
* 多人遊戲的元件、 介面和架構
* 合作對象 2015年多人遊戲支援
* 多人的術語
* 什麼是 2015年多人遊戲的新功能
* Xbox 360 和 Xbox 一個 MPSD 工作階段的函式之間的差異

## <a name="about-the-multiplayer-system"></a>關於多人遊戲系統


2015 多人遊戲最佳化直接使用 MPSD 和遊戲的工作階段。 它提供更好的支援單一標題或使用者的多個並行工作階段，並更新的 「 Xbox 服務 API (XSAPI) 」，若要啟用項目：

-   通知使用者的目前活動與聯結的可用性。
-   傳送邀請給工作階段，以及使用者可看見 （標題指定） 的內容字串。
-   探索並加入透過標題的程式碼的工作階段。
-   維持 web 通訊端連線 MPSD，讓他們可以收到簡短的通知 （shoulder 點選） 工作階段變更，例如，反映變更事件和連線狀態變更的訂用帳戶的更新。 MPSD 也會使用 web 通訊端連線能夠快速偵測並處理用戶端中斷連線。
-   使用 SmartMatch 配對。

## <a name="multiplayer-components-interfaces-and-architectures"></a>多人遊戲的元件、 介面和架構

### <a name="components-of-2015-multiplayer"></a>2015 多人遊戲的元件

多人遊戲是包含多個元件的系統。 它是有足夠的彈性，以允許其他元件，例如專用的伺服器和外部的配對系統。
#### <a name="multiplayer-session-directory-mpsd"></a>多人遊戲工作階段目錄 (MPSD)


多人連線的工作階段目錄 (MPSD) 是保存的工作階段集合的服務。 工作階段被指位於雲端和表示一群人玩遊戲的安全文件。 MPSD 詳細資訊，請參閱[多人遊戲工作階段目錄 (MPSD)](multiplayer-session-directory.md)。


#### <a name="multiplayer-apis"></a>多人遊戲 Api

2015 多人遊戲提供多人遊戲的 WinRT API，透過實作**Microsoft.Xbox.Services.Multiplayer 命名空間**。 其元件包含**MultiplayerService 類別**，定義包裝 MPSD web 服務。

也包含是多人遊戲的 REST API。 它會定義 Uri 和 JSON 的 WinRT API 方法所呼叫的物件。 REST 功能可以用於直接呼叫，從您的項目，但建議您使用存取 MPSD 間接透過 WinRT API。 如需詳細資訊，請參閱 <<c0>  **呼叫 MPSD**。

#### <a name="xbox-party-system"></a>Xbox 合作對象系統

在 2015年多人遊戲，Xbox 合作對象系統會支援僅對談合作對象做為外部實體。 標題可以與探索的對談的合作對象的成員資格的廠商系統互動。 如需詳細資訊，請參閱 < **2015年多人遊戲支援合作對象**。

合作對象系統現在支援直接透過 MPSD 工作階段中，而不是使用遊戲的合作對象的遊戲。 這是由使用工作階段，讓成員互動，以及將提取的播放程式到遊戲的空間可供使用，因為在等候期間的使用者參與這類作業的標題。


### <a name="2015-multiplayer-interfaces"></a>2015 多人遊戲介面

2015 多人會使用數個其他 Xbox 元件的介面。
#### <a name="xbox-secure-sockets"></a>Xbox 安全通訊端

2015 多人遊戲支援使用 Xbox 安全通訊端和 Winsock 裝置之間的低階網路通訊。 網路功能會使用網際網路通訊協定安全性 (IPSec)，允許以提供安全的裝置關聯的項目。 請參閱**網路功能概觀**Xbox 的詳細資料的安全通訊端功能。


#### <a name="xbox-live-real-time-activity-service"></a>Xbox Live 的即時活動的服務

2015 多人會使用**即時活動的服務**允許項目，若要訂閱 MPSD 工作階段變更，並啟用自動偵測用戶端中斷連線。 中提供詳細資訊[即時活動 (RTA) 服務](../../real-time-activity-service/real-time-activity-service.md)。


#### <a name="xbox-live-matchmaking-service"></a>Xbox Live 的配對服務

**配對服務**form 群組從播放程式，根據其喜好設定和資料，以及在 SmartMatch 配對期間所提供的資訊。 對於多人遊戲的詳細資訊，請使用這項服務，請參閱[SmartMatch 配對](smartmatch-matchmaking.md)。


#### <a name="xbox-live-reputation-service"></a>Xbox Live 的信譽服務

*信譽服務*管理有關信譽和評價篩選戰時的使用者統計資料。 它會使用透過 SmartMatch 配對 2015年多人遊戲。 如需詳細資訊，請參閱 <<c0> [ 信譽](../../social-platform/people-system/reputation.md)。


#### <a name="xbox-live-compute"></a>Xbox Live 計算

Xbox Live Compute 可提供雲端處理項目的使用 2015年多人遊戲的能力。 如需有關使用 Xbox Live 計算的詳細資訊，請參閱[使用 Xbox Live 計算多人遊戲](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer)。


### <a name="typical-2015-multiplayer-architectures"></a>一般 2015年多人遊戲架構

此章節提供的最常見的 2015年多人遊戲的架構。
#### <a name="peer-to-peer-architecture"></a>端對端架構

在端對端架構中，標題會使用 MPSD 和 SmartMatch 配對，來探索對等位址。 位址則用來連接使用 Xbox 安全通訊端對等。 如需詳細資訊，請參閱 < *Xbox One 上的 Winsock 簡介*。

![對等的架構圖](../../images/multiplayer/Mult2015ArchPeer.png)


#### <a name="client-server-architecture"></a>用戶端-伺服器架構

在用戶端-伺服器多人遊戲架構中，標題會使用 MPSD 和 SmartMatch 配對探索專用的伺服器位址。 這些則用來連接到使用 Xbox 安全通訊端的專用伺服器。 如需詳細資訊，請參閱 < *Xbox One 上的 Winsock 簡介*。

| 注意                                                                         |
|-------------------------------------------------------------------------------------------|
| Xbox Live 計算執行個體可用來當做用戶端-伺服器架構中的伺服器。 |

![用戶端伺服器架構圖](../../images/multiplayer/Mult2015ArchClientServer.png)


## <a name="parties-supported-by-2015-multiplayer"></a>合作對象 2015年多人遊戲支援
Xbox one 2015 多人不會公開 「 遊戲合作對象 」 為系統層級建構。 不過，它在系統層級，如同 2014年多人遊戲支援 「 聊天合作對象 」。

| 注意                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------|
| 您項目仍可實作類似於實作使用遊戲的合作對象，但改為使用 MPSD 工作階段的使用者體驗。 |


### <a name="chat-party"></a>對談合作對象

對談的合作對象是一群人在聊天彼此，管理使用者已透過 Xbox 一個合作對象應用程式。 玩遊戲的工作階段中時，或在執行主控台的另一個活動時，使用者可以在對談的合作對象。 不過，在對談的合作對象的使用者與這些使用者的其他活動之間沒有繫結。

使用公開的聊天合作對象*PartyChat 類別*，可讓以檢查狀態的交談合作對象的標題。 例如，標題可以列舉的成員使用的對談合作對象*PartyChat.GetPartyChatViewAsync 方法*。


### <a name="implementing-features-similar-to-those-related-to-the-game-party"></a>實作類似遊戲的合作對象的相關功能

在 2014年多人遊戲，遊戲的合作對象會提供多種用途。 它允許的標題：

-   使用者的目前活動和聯結可用性通告
-   傳送邀請 （邀請） 工作階段
-   探索並加入工作階段
-   接收合作對象為您註冊的工作階段特定變更的通知
-   使用 SmartMatch 配對
-   一群人保持跨多個遊戲的工作階段

2015 多人遊戲支援直接透過 MPSD 工作階段的所有上述功能。

## <a name="multiplayer-terminology"></a>多人的術語


| 詞彙                                 | 描述|
| --- | --- |
| 作用中的播放程式                        | 玩家可以在工作階段中已設定為作用中狀態。 玩家參與遊戲時，標題會設定播放程式為此狀態。 如需詳細資訊，請參閱 <<c0> [ 使用者的工作階段狀態](mpsd-session-details.md)。                                                                                                                                                                                                                                                                                          |
| 仲裁程式                              | 廣告配對，以尋找更多玩家遊戲工作階段，比方說，管理多人連線的工作階段目錄 (MPSD) 工作階段狀態的遊戲，遊戲工作階段中的單一主控台。 項目所設定的仲裁程式。 仲裁程式不一定遊戲的主機。 請參閱[工作階段仲裁程式](mpsd-session-details.md)。                                                                                                                                                                            |
| 排列的遊戲                        | 一種建立只能透過邀請加入，而不需要介入配對的其他播放程式的一名玩家的遊戲。                                                                                                                                                                                                                                                                                                                                                                                                    |
| 對談合作對象                           | 一組會一起聊天的人員。 使用者可能會參與相同的活動，或可能會參與不同的活動，例如遊戲、 音樂或應用程式。 請參閱**2015年多人遊戲支援的合作對象**。                                                                                                                                                                                                                                                                                 |
| 遊戲的邀請                          | 邀請，以加入遊戲工作階段。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 遊戲的工作階段                         | 工作階段所在使用者實際玩在一起。 所有的多人遊戲案例，例如配對或聯結大廳，最後都會進入遊戲的工作階段。 工作階段通常會做為使用者的目前活動，以便聯結通告。 它也會用來建立新播放清單。 請參閱[MPSD 工作階段詳細資料](mpsd-session-details.md)。                                                                                                                                                           |
| 遊戲的工作階段主機                    | 在執行遊戲的主控台播放模擬以主機為基礎的端對端網路架構為基礎的項目。 此主控台通常是仲裁程式，相同，但它不一定要相同。                                                                                                                                                                                                                                                                                                                            |
| 控制代碼 （或工作階段控制代碼）           | 有其他的狀態和行為與其相關聯的 MPSD 工作階段的參考。 請參閱[MPSD 控制代碼對應到工作階段](multiplayer-session-directory.md)。                                                                                                                                                                                                                                                                                                                                                                    |
| 非使用中的播放程式                      | 玩家可以在工作階段中已設定為非使用中狀態。 標題的播放程式這種狀態時設定遊戲已暫止，或因非使用中，項目所定義。 在某些情況下，MPSD 為非作用中，也可以設定播放程式，但它是主要的標題，若要這樣做的責任。 如需詳細資訊，請參閱 <<c0> [ 使用者的工作階段狀態](mpsd-session-details.md)。                                                                                                                           |
| Hopper                               | Hopper 是邏輯驅動集合的相符項目票證。 標題可以有多個 hoppers，但只在相同的 hopper 內的票證可以比對。 例如，標題可能會建立一個 hopper 技能的播放程式是最重要的項目進行比對。 它可能會使用另一個 hopper 中播放器只表示相符購買相同可下載的內容。 如需有關 hoppers 會納入 SmartMatch 工作流程的詳細資訊，請參閱[SmartMatch 執行階段作業](smartmatch-matchmaking.md) |
| 加入進行中                     | 遊戲開始之後，加入其他播放器的概念的遊戲。 播放程式可以透過朋友的玩家卡加入朋友的遊戲。 標題可以接著將這些播放程式移到遊戲的工作階段，在適當的時間。                                                                                                                                                                                                                                                                                                      |
| 大廳工作階段                        | 受邀加入遊戲工作階段正在等候的球員協助程式工作階段。 請參閱[MPSD 工作階段詳細資料](mpsd-session-details.md)。                                                                                                                                                                                                                                                                                                                                                                                       |
| 比對目標工作階段                 | 比對工作階段期間設定 SmartMatch 配對來表示比對。 請參閱[SmartMatch 配對](smartmatch-matchmaking.md)。                                                                                                                                                                                                                                                                                                                                                                                              |
| 比對票證工作階段                 | 初步的比對工作階段期間 SmartMatch 配對設定。 請參閱[SmartMatch 配對](smartmatch-matchmaking.md)。                                                                                                                                                                                                                                                                                                                                                                                                         |
| MPSD 工作階段                         | 安全的文件位於 Xbox Live 雲端內的多人連線的工作階段目錄 (MPSD)。 它包含一群可能連接 Xbox One 上執行的標題，以及使用者和他們的遊戲相關的中繼資料時的使用者。 請參閱[MPSD 工作階段詳細資料](mpsd-session-details.md)。                                                                                                                                                                                                                  |
| 多人遊戲工作階段目錄 (MPSD) | 操作多人遊戲系統用來儲存和擷取工作階段的雲端服務。 請參閱[多人連線的工作階段目錄 (MPSD)](multiplayer-session-directory.md)。                                                                                                                                                                                                                                                                                                                                                                |
| 合作對象的應用程式                            | Xbox One 系統嵌入式管理單元可讓使用者檢視及管理其合作對象的應用程式。                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 伺服器工作階段                       | 遊戲的工作階段，建立 Xbox Live Compute 的處理。 請參閱[MPSD 工作階段詳細資料](mpsd-session-details.md)。                                                                                                                                                                                                                                                                                                                                                                                                            |
| Shoulder 點選                         | 從 MPSD 通知的服務上發生了可能有興趣的變更。 Shoulder 點選是快速提醒您，通常是較少資訊比一般的通知。 請參閱[MPSD 變更通知處理，並中斷連線偵測](multiplayer-session-directory.md)。                                                                                                                                                                                                                     |
| SmartMatch 配對               | Xbox Live 配對項提供給功能配對服務所實作的 Xbox One 標題。 使用 MPSD 和配對，標題會要求要比對，並通知更新版本已找到相符的群組。 請參閱[SmartMatch 配對](smartmatch-matchmaking.md)。                                                                                                                                                                                                                                  |

## <a name="whats-new-in-2015-multiplayer"></a>什麼是 2015年多人遊戲的新功能

| 注意                                                                                                                                                                                                                                               |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 當使用 2015年多人遊戲時，務必請注意，2014年多人遊戲中合作對象相關的類別不應再。 混合 2015年多人遊戲的功能與合作對象相關的類別會造成不一致的行為，而且應該永遠不會嘗試。 |


### <a name="new-concepts-in-2015-multiplayer"></a>在 2015年多人遊戲的新概念


#### <a name="web-socket-connections-to-mpsd"></a>Web 通訊端連線到 MPSD

MPSD 現在可讓維護與它的 web 通訊端連線的標題。 這些連線可讓用戶端工作階段變更時接收通知。 如需詳細資訊，請參閱 < [MPSD 變更通知處理和中斷連線偵測](multiplayer-session-directory.md)。


#### <a name="mpsd-session-handles"></a>MPSD 工作階段控制代碼

2015 多人遊戲加入 MPSD 工作階段控制代碼，就是參考可以包含具類型的資料的工作階段的支援。 如需詳細資訊，請參閱 < [MPSD 控制代碼對應到工作階段](multiplayer-session-directory.md)。


### <a name="summary-of-new-2015-multiplayer-winrt-api-functionality"></a>新的 2015年多人遊戲的 WinRT API 功能的摘要

新多人遊戲的 WinRT API 功能根據現有 XSAPI，以協助的輕鬆轉換的項目已經在使用 2014年多人遊戲的合作對象 API 搭配使用。

加入 2015年多人遊戲*MultiplayerActivityDetails 類別*，代表在使用者可聯結工作階段的使用者的目前活動，例如，詳細資料。

已加入新功能*MultiplayerService 類別*。 範例包括方法和屬性處理使用者與社交群組、 使用不同的篩選器和控制代碼的工作階段擷取的活動，傳送遊戲的邀請，與工作階段讀取和寫入使用控制代碼。

*MultiplayerSession 類別*將功能加入至工作與工作階段變更型別]、 [訂用帳戶通知、 工作階段相較之下，以及設定工作階段為已關閉。

*MultiplayerSessionReference 類別*變更為繼承自**IMultiplayerSessionReference**，以支援跨命名空間呼叫。 類別也有新的 URI 路徑剖析方法。

| 注意                                                                                                                                                                                                                                                                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 若要支援事件和通知的訂閱，2015年多人會將功能加入*Microsoft.Xbox.Services.RealTimeActivity 命名空間*。 也包含在新功能*SystemUI.ShowSendGameInvitesAsync 方法*，用來顯示 2015年多人遊戲邀請 UI。 |

## <a name="differences-between-xbox-360-and-xbox-one-mpsd-session-functions"></a>Xbox 360 和 Xbox 一個 MPSD 工作階段的函式之間的差異

| 函式 | Xbox 360 | Xbox One |
|---|---|---|
| **取得遊戲的工作階段資訊** | XSessionGetDetails、 XSessionSearchByID 或標題會追蹤。 | 標題 MPSD 要求工作階段資訊。 |
|**移轉的主機** | 當有需要標題就會呼叫 XSessionMigrateHost。 | 根據移轉的原因，或許可以指派新的主控件之工作階段的標題，或可能會建立新的 MPSD 工作階段。 |
| **多個播放程式工作階段** | 難處理一次，例如與 XNetUnregisterKey XNetReplaceKey 的多個工作階段。 | 服務為基礎的工作階段讓某個工作階段的管理更簡潔，並可簡化多個工作階段的處理。 |
| **Signouts 並中斷連線** | 標題必須處理中斷連線並以不同的方式與 XCloseHandle 或 XSessionDelete 登出。 | MPSD 簡化 signouts，並中斷連線的處理和遊戲的組態中設定的逾時。 |
| **配對** | 用戶端為基礎的配對查詢 | 服務為基礎的配對品質和標題內更輕鬆的背景配對可更佳的相符項目。 |


### <a name="sessions"></a>工作階段

在 Xbox 360，工作階段來表示玩遊戲的執行個體。 使用者搜尋 「 配對 」 服務，在工作階段，並在工作階段結束時報告統計資料。

Xbox One 上工作階段則是更泛型的並表示播放程式的群組。 工作階段需要主控台之間的任何網路連線，並保存應在工作階段中的 所有使用者之間共用的資訊。 這項資訊的一些範例包括允許工作階段中，每個主控台中的工作階段和自訂的遊戲資料的安全地址的播放程式的數目。


### <a name="xbox-matchmaking"></a>Xbox 配對

標題 Xbox 360，根據設定的屬性結構描述和一組要搜尋這些屬性的查詢進行配對。 在執行階段，標題選擇裝載的工作階段或其中一個搜尋。

Xbox One 上配對是以伺服器為基礎，並播放器和項目不會再決定是否要裝載或搜尋。 相反地，每個預先正確的群組的播放程式會建立 「 票證 」 工作階段，並送出配對服務該工作階段。 然後，服務會尋找其他工作階段，並結合以形成新的 「 目標 」 工作階段的群組。 用戶端會收到通知的相符項目，並執行的服務品質 (QoS) 來啟動遊戲之前驗證連線能力與其他工作階段的成員。


### <a name="xbox-live-compute-service"></a>Xbox Live 計算服務

Xbox Live 計算服務是新的供應項目，在 Xbox One 上。 它可讓開發人員充分利用雲端的彈性的計算能力，並可讓比對等網路中可能的大型多人遊戲情節。 如需詳細的 Xbox Live 計算服務的詳細資訊，請參閱 XDK 文件中的 Xbox Live 計算文件。


## <a name="see-also"></a>請參閱

[多人連線的工作階段目錄 (MPSD)](multiplayer-session-directory.md)

[SmartMatch 配對](smartmatch-matchmaking.md)

[即時活動 (RTA) 的服務](../../real-time-activity-service/real-time-activity-service.md)

[評價](../../social-platform/people-system/reputation.md)

[使用 Xbox Live Compute 中多人遊戲 （需要受管理的夥伴存取權）](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer)
