---
title: 多人連線的工作階段目錄
description: 深入了解 Xbox Live 的多人遊戲工作階段目錄 (MPSD)。
ms.assetid: a9b029ea-4cc1-485a-8253-e1c74184f35e
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 mpsd、 多人連線的工作階段目錄。
ms.localizationpriority: medium
ms.openlocfilehash: 1681fe59533ebaf0db197efb95ca46b828846f5d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662543"
---
# <a name="multiplayer-session-directory-mpsd"></a>多人遊戲工作階段目錄 (MPSD)

本文包括下列章節：

* MPSD 概觀
* MPSD 變更通知處理，並中斷連線偵測
* 工作階段的 MPSD 控制代碼
* 工作階段更新的同步處理
* 呼叫 MPSD
* MPSD 工作階段範本
* 多人連線的工作階段檔案總管

## <a name="session-overview"></a>工作階段概觀

### <a name="what-is-mpsd"></a>什麼是 MPSD？

多人連線的工作階段目錄 (MPSD) 是集中跨多個用戶端的遊戲的多人連線的系統中繼資料的 Xbox Live 的雲端作業系統服務。 它由包裝**MultiplayerService 類別**。

MPSD 允許共用連線的使用者群組所需的基本資訊的標題。 它可確保工作階段功能，是已同步處理且一致。 它會協調與 shell 和主控台作業系統和玩家卡透過聯結中傳送/接受邀請。


### <a name="mpsd-sessions"></a>MPSD 工作階段

MPSD 工作階段由**MultiplayerSession 類別**讓一或多人使用遊戲案例。 工作階段會儲存 MPSD 放在 Xbox Live 雲端以安全的 JSON 文件。 具體來說，MPSD 工作階段具有下列特性： 建立和管理的項目。

-   有一個唯一的 URI。 如需詳細資訊，請參閱 <<c0>  **工作階段目錄 Uri**。
-   允許在呼叫工作階段成員的使用者之間的連線。
-   適用於啟用遊戲的存放區資料，例如播放每個成員屬性，啟動程序資訊及遊戲伺服器資訊的遊戲設定。

MPSD 用於設定 多人遊戲支援數個工作階段變化。 每個工作階段包含玩家的 Xbox 使用者識別碼 (XUIDs) 及保護裝置關聯的地址資料。

-   遊戲工作階段中，作為遊戲模式。 對等、 對等電腦來裝載、 對等的伺服器或這些類型的混合式，可以是遊戲工作階段。
-   票證工作階段，用來追蹤期間配對的相符項目狀態的協助程式工作階段。 它通常也是大廳工作階段，有時可能遊戲工作階段。 請參閱[SmartMatch 配對](smartmatch-matchmaking.md)。
-   在配對，以表示相符的遊戲進行期間建立的協助程式工作階段的目標工作階段。 它幾乎都是也遊戲工作階段。 請參閱[SmartMatch 配對](smartmatch-matchmaking.md)。
-   大廳工作階段，用來容納受邀加入遊戲工作階段正在等候的球員的協助程式工作階段。 許多項目建立大廳工作階段和遊戲的工作階段。 如需詳細資訊，請參閱 <<c0>  **標題中的管理玩家**。

## <a name="mpsd-change-notification-handling-and-disconnect-detection"></a>MPSD 變更通知處理，並中斷連線偵測

MPSD 可讓用戶端連線到使用即時活動服務 web 通訊端。 連接是用來：

-   關閉簡短的通知 （shoulder 點選） 傳送工作階段變更時，根據項目起始的事件訂用帳戶。
-   偵測使用者的連線中斷。
-   設定為非作用中的使用者，並接著將它們移除的工作階段中，然後再根據中斷連線時偵測。


### <a name="making-user-connections"></a>讓使用者連接

XSAPI 程式庫管理用戶端與 MPSD 之間的連線。 標題的第一次呼叫**MultiplayerService.EnableMultiplayerSubscriptions 方法**。 這個方法會告訴 XSAPI 用戶端想要用於即時活動連線多人連線遊戲。 接著，當標題會以其第一次呼叫時，才**MultiplayerService.WriteSessionAsync 方法**或**MultiplayerService.WriteSessionByHandleAsync 方法**，以目前設定為 作用中的使用者建立並連結到 MPSD 連接的狀態。

| 注意                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------------------------|
| 啟用工作階段通知，並偵測中斷連線工作階段範本對 connectionRequiredForActiveMembers 設為 true。 |

| 注意                                                                                                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 在 XSAPI 的先前版本中，稱為標題**RealTimeActivityService.ConnectAsync 方法**建立即時活動的服務的使用者連接。 2015年多人遊戲，這個方法不會執行任何動作，以及依需求建立連線。 |

### <a name="subscribing-for-session-changes"></a>訂閱的工作階段變更

MPSD 使用"shoulder 點選 「 感興趣的項目已變更的輕量級通知形式。 標題應擷取已修改的資源，以判斷變更的確切性質。 啟用的訂用帳戶，如 shoulder 點選藉由呼叫的工作階段變更，可以訂閱標題**MultiplayerSession.SetSessionChangeSubscription 方法**。 請參閱[How to:MPSD 工作階段變更通知訂閱](multiplayer-how-tos.md)。


### <a name="handling-shoulder-taps"></a>處理 Shoulder 點選

當工作階段的變更符合項目的訂用帳戶，該工作階段時，MPSD 通知變更使用的標題**RealTimeActivityService.MultiplayerSessionChanged 事件**。 標題應再擷取工作階段擷取的版本，在工作階段與先前的快取檢視中，並比較適當地採取動作。


### <a name="handling-notifications-of-connection-state-changes"></a>處理連線狀態變更的通知

標題也會收到 MPSD 連線的健康狀態的變更。 兩個事件發出訊號的這些變更: * * RealTimeActivityService.MultiplayerSubscriptionsLost 事件 * *-MPSD 使用即時活動的服務項目的連線已遺失時引發。 這個事件發生時，標題應該關閉多人遊戲。
-   * * RealTimeActivityService.ConnectionStateChanged 事件 * *-在暫存的變更項目的即時活動的服務連線的健康狀態時引發。 標題不需要採取任何動作，收到此事件，但可能有助於使用事件供診斷之用。


### <a name="disconnecting-clients"></a>中斷用戶端連接

標題的用戶端時，中斷連接從 MPSD 標題會停用通知，藉由呼叫**RealTimeActivityService.DisableMultiplayerSubscriptions 方法**。 這個呼叫後不久, **MultiplayerSubscriptionsLost**事件引發時，表示與 MPSD 已中斷連接的用戶端。

| 注意                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 多人在舊版中，呼叫標題 * * RealTimeActivityService.Disconnect 方法 * * 若要從即時活動的服務中斷連線。 如 2015年多人遊戲，這個方法沒有任何作用。 中斷連接，就會發生自動之後**DisableMultiplayerSubscriptions**呼叫時，如果沒有任何使用者的 web 通訊端連線，比方說，目前狀態的即時活動服務訂用帳戶。 |


### <a name="disconnect-detection"></a>中斷連線偵測

MPSD 會使用其中斷連線偵測功能，來強制使用者中斷連線時，快速找出。 當玩家的網路失敗時，或標題損毀時，可能會發生失誤性錯誤的中斷連線。 MPSD 從作用中的中斷連線的播放程式狀態變更為 「 非作用中，並通知其他工作階段的成員視需要變更工作階段的訂用帳戶的成員為基礎。

## <a name="mpsd-handles-to-sessions"></a>工作階段的 MPSD 控制代碼

MPSD 工作階段控制代碼是抽象和不可變的參考，也可以包含其他的型別的資料的工作階段。 這就像是檔案控制代碼。 所有控制代碼會有的控制代碼識別碼 (GUID) 和完整的工作階段參考識別碼 (SCID) 的服務組態、 工作階段範本和工作階段名稱組成。 無法更新的控制代碼，但它可以是建立、 讀取和刪除。

| 注意                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| 控制代碼可以指向不存在的工作階段。 建立控制代碼使用不存在的工作階段名稱，不會造成要建立新的工作階段。 |


### <a name="handle-types"></a>控制代碼類型

2015 多人遊戲支援這一節所述的控制代碼類型。

#### <a name="invite-handle"></a>邀請控制代碼

邀請控制代碼表示的特定使用者的邀請 （邀請）。 特定類型的資料包含來源的人員、 目標的人，以及描述邀請，比方說，特定的遊戲模式的內容字串。

邀請控制代碼授與讀寫存取開啟的工作階段。 如果工作階段已關閉，控制代碼會授與唯讀工作階段存取。

| 注意                                                |
|------------------------------------------------------------------|
| MPSD 可以建立邀請，即使工作階段為完整或已關閉。 |


#### <a name="activity-handle"></a>活動的控制代碼

活動控制代碼表示使用者在做什麼時間。 使用者的活動由**MultiplayerActivityDetails 類別**。

| 注意                                                                                                                                                                                                                      |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 活動控制代碼也可以明確地刪除，但只能由特定使用者擁有的標題。 這項刪除作業是使用**MultiplayerService.ClearActivityAsync 方法**。 |


### <a name="creating-an-invite-handle"></a>建立邀請控制代碼

若要建立邀請控點，此標題會呼叫**MultiplayerService.SendInvitesAsync 方法**。 方法會傳送邀請給指定的使用者在 UI 的表單中的收件者接受邀請時，可針對快顯通知。


### <a name="creating-an-activity-handle"></a>建立活動的控制代碼

若要建立活動控點，此標題會呼叫**MultiplayerService.SetActivityAsync 方法**。 MPSD 設定新的控制代碼識別碼做為工作階段成員的繫結的活動。 如有任何先前的繫結的活動，MPSD 會刪除對應的控制代碼。 當作用中的成員會變成非作用中，或離開工作階段時，MPSD 刪除繫結的活動的控制代碼。


### <a name="using-handles"></a>使用控制代碼

當使用者接受邀請 （邀請控點） 和使用者加入朋友的目前活動 （活動控點） 時，標題就會使用控制代碼。 在這兩種情況下，標題必須：

1.  取得控制代碼識別碼的標題中的啟動參數。
2.  建立本機 MPSD 工作階段物件，並加入為作用中。
3.  寫入的工作階段中，然後再傳遞適當的控制代碼。

## <a name="synchronization-of-session-updates"></a>工作階段更新的同步處理

由於工作階段是共用的資源，可建立或更新由它的任何成員，可能會缺少寫入衝突。 這可能會導致非預期的結果，例如，一個標題可以不知情的情況下覆寫另一個標題所做的變更。 若要解決這些衝突的 MPSD 的方法是支援開放式並行存取和讀取-修改-寫入模式。

由 MPSD 的工作階段更新同步處理的使用相關的高層級實作的兩種模式：

-   仲裁程式更新工作階段的共用的的部分。 如果您的實作涉及單一仲裁程式，您可以避免使用適用於大部分的寫入作業的同步處理的更新。 標題可以避免仲裁程式對共用部分的工作階段中，除非它們與相關通訊仲裁程式的身分識別，（1） 所有更新] 和 [標題對工作階段內的成員區域 （2） 任何更新的同步處理。

| 注意                                                                                                                                                                                                                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 雖然上述的更新類型，而不需要同步處理，仍然十分重要來同步處理的任何更新 * * MultiplayerSessionProperties.HostDeviceToken 屬性 * *。 該屬性用來做為仲裁程式移轉一部分通訊的仲裁程式，比方說，身分識別。 |

-   所有用戶端更新工作階段的共用的的部分。 在此情況下，必須同步處理的工作階段的共用部分的所有更新。 不過，標題仍然可以撰寫自己的成員區域，而不需要同步處理。


### <a name="session-update-synchronization-using-the-multiplayer-winrt-api"></a>使用多人遊戲的 WinRT API 的工作階段更新同步處理

下列多人遊戲的 WinRT API 方法會實作開放式並行存取：

-   **MultiplayerService.WriteSessionAsync 方法**
-   **MultiplayerService.WriteSessionByHandleAsync 方法**
-   **MultiplayerService.TryWriteSessionAsync 方法**
-   **MultiplayerService.TryWriteSessionByHandleAsync 方法**

每個寫入方法會接受**MultiplayerSessionWriteMode 列舉**值。 傳遞值 SynchronizedUpdate 利用開放式並行存取的更新。

列舉中的其他值協助您解決可能發生的衝突，在初始建立工作階段時。 任何寫入可能寫入另一個項目所 MPSD 工作階段的部分必須使用同步的更新。 不過，並非所有 writes 必須受到都保護。

如果您的標題嘗試寫入 MPSD 使用寫入工作階段方法的其中一個本機工作階段物件，並收到的 HTTP/412 狀態碼，它應該重新整理本機複本發出**MultiplayerService.GetCurrentSessionAsync 方法**呼叫以取得最新的伺服器版本，然後再次嘗試寫入工作階段。 否則本機工作階段的文件會繼續包含不正確的資料，而且持續失敗的呼叫來撰寫工作階段。

| 注意                                                                                                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 如果標題使用其中一種**TryWrite\*** 方法，在 HTTP/412 狀態碼的情況下，會傳回更新的工作階段。 此行為可避免需要呼叫**GetCurrentSessionAsync**。 |

當標題呼叫寫入工作階段方法的其中一個時，可能會傳回工作階段的更新的版本。 如果是，標題應該切換其本機快取的複本到這個新版本的執行緒安全的方式。


### <a name="session-update-synchronization-using-the-multiplayer-rest-api"></a>使用多人遊戲的 REST API 建立的工作階段更新同步處理

MPSD 使用 HTTP"if-match"標頭使用 ETag 設定和讀取-修改-寫入模式，透過 REST 功能的工作階段更新中支援開放式並行存取。 寫入要求中傳遞 ETag 應該是 MPSD 傳回與先前的讀取要求。

## <a name="calling-mpsd"></a>呼叫 MPSD

有兩種方式，若要使用的多人遊戲系統和配對存取 MPSD 標題：
-   建議使用。 使用多人遊戲的 WinRT API，其中包含做為 RESTful 功能的包裝函式的類別。 請參閱**Microsoft.Xbox.Services.Multiplayer 命名空間**。 用於 SmartMatch 配對所代表的 WinRT API 配對**Microsoft.Xbox.Services.Matchmaking 命名空間**。
-   使用直接標準 HTTP 呼叫的多人遊戲和配對 REST Api 包含在**Xbox Live 服務 RESTful 參考**。 適用的 Uri 中所述**工作階段目錄 Uri** （適用於多人遊戲） 並**配對 Uri** （用於配對）。 相關聯的 JSON 物件中所述**JavaScript Object Notation (JSON) 物件參考**。


### <a name="using-the-multiplayer-winrt-api-to-call-mpsd"></a>使用多人遊戲的 WinRT API 呼叫 MPSD

呼叫 MPSD 的建議的方式是使用多人遊戲的 WinRT API 和配對 WinRT API。

| 注意                                                                                             |
|---------------------------------------------------------------------------------------------------------------|
| XDK 範例是撰寫使用多人遊戲和配對 WinRT Api，以及 XSAPI 的其他項目。 |

使用包裝函式程式碼為基礎的 REST 功能可讓更傳統的方法，來使用用戶端 API 方法，而不需要處理的每個呼叫的 HTTP 流量。 二進位檔和來源 XSAPI 隨附的 XDK。 您可以直接使用二進位檔或修改來源，並將其整合到您的標題，依需要。


### <a name="using-the-multiplayer-rest-api-to-interact-with-mpsd"></a>使用 REST API 的多人與 MPSD 互動

標題或其服務，可以使用標準的 HTTP 呼叫，多人遊戲的 REST API 和配對 REST API。 當直接使用 REST 功能，呼叫端問題刪除、 PUT、 POST 和取得大部分的動作呼叫對工作階段目錄 Uri。 在 PUT 要求中，要求主體會合併到現有的工作階段。 如果沒有任何現有的工作階段，要求本文用來建立新的工作階段，以及儲存在工作階段範本[Xbox 開發人員入口網站 (XDP)](https://xdp.xboxlive.com)或是[合作夥伴中心](https://partner.microsoft.com/dashboard)。 所有欄位都是選擇性的而且必須指定唯一的差異。 因此，{}有效的 PUT 要求具有零的差異。

若要執行假設的 PUT 要求會傳回合併的結果，而不會影響伺服器的官方的複製工作階段，您可以將查詢字串附加 「？ nocommit = true"PUT 要求。

要求和多人遊戲和配對 REST API 方法的回應是 JSON 文件。 多人連線的工作階段要求結構，請參閱**MultiplayerSessionRequest (JSON)**。 相關聯的回應結構所示**MultiplayerSession (JSON)**。 回應結構框架工作階段的成員為連結的清單，並填入的工作階段和其成員的其他唯讀屬性。


### <a name="querying-for-sessions-and-session-templates-rest"></a>查詢工作階段和工作階段範本 (REST)

在服務組態和工作階段範本層級的工作階段資訊可以查詢您的標題。 本主題討論使用多人遊戲的 REST API 的查詢。


#### <a name="query-for-basic-session-information"></a>基本的工作階段資訊的查詢

您可以設定為使用工作階段目錄和配對 Uri 的基本工作階段資訊的查詢。 查詢的結果是包含內嵌的參考，以特定的工作階段資料的工作階段的 JSON 陣列。 根據預設，查詢會擷取最多 100 個的非私用工作階段。

| 注意                                                          |
|----------------------------------------------------------------------------|
| 每個查詢必須包含關鍵字篩選器、 XUID 篩選器，或兩者。 |


#### <a name="query-for-session-templates"></a>查詢工作階段範本

若要擷取工作階段範本清單 SCID，以及特定的工作階段範本的詳細資訊，請使用 GET 方法的其中一個下列 Uri:
-   **/serviceconfigs/{scid}/sessiontemplates**
-   **/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}**


#### <a name="query-for-session-state"></a>工作階段狀態的查詢

若要查詢的工作階段狀態，請使用 GET 方法的其中一個這些 Uri:
-   **/serviceconfigs/{scid}/sessions**
-   **/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions**

## <a name="multiplayer-session-explorer"></a>多人連線的工作階段檔案總管

多人遊戲工作階段總管是瀏覽工作階段、 工作階段範本，以及當地語系化字串內建 MPSD 工具。 此工具只被要用於開發沙箱。


### <a name="accessing-multiplayer-session-explorer"></a>存取多人連線的工作階段檔案總管

| 注意                                                                                                      |
|------------------------------------------------------------------------------------------------------------------------|
| 若要使用此工具，您必須登入。 瀏覽限於工作階段中的成員身分登入的使用者。 |

若要存取多人遊戲工作階段總管，請開啟 Internet Explorer 您在 Xbox One 上，按下**檢視**按鈕，然後輸入<https://sessiondirectory.xboxlive.com/debug>中**位址**欄位。

| 注意                                                                                                                                                                                |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 如果您嘗試存取零售沙箱中的工具，您會收到的 HTTP/404 狀態碼。 如需有關這個程式碼的詳細資訊，請參閱[多人遊戲工作階段狀態碼](multiplayer-session-status-codes.md)。 |


### <a name="using-multiplayer-session-explorer"></a>使用多人連線的工作階段總管


#### <a name="open-the-main-page"></a>開啟主頁面

1.  存取此工具的主頁面。 在沙箱中顯示的安全性內容 （「 登入的使用者 」 與 「 沙箱 」） 和服務組態 (SCIDs) 識別碼的清單。
2.  按下** 功能表**按鈕釘選到首頁的這個頁面，讓您不必每次輸入的 URI。


#### <a name="display-available-sessions-and-templates"></a>顯示可用的工作階段和範本

1.  按一下以顯示工作階段的清單中的登入使用者為成員的該 SCID SCID 工具中。
2.  在此相同的頁面上，您可以按一下 SCID，並顯示 SCID 的服務組態中的工作階段範本和當地語系化字串。 這些項目透過內嵌[XDP](https://xdp.xboxlive.com)或是[合作夥伴中心](https://partner.microsoft.com/dashboard)。


#### <a name="display-the-full-contents-of-a-session"></a>顯示工作階段的完整內容

多人遊戲工作階段在總管中，按一下 工作階段名稱，以顯示對應的工作階段的完整內容。

有幾個原因 MPSD 所示的工作階段可能會從標準的 GET 方法回應不同工作階段的 uri:

-   GET 呼叫可能使用較舊的合約版本 X Xbl-合約版本標頭中。 工作階段總管一律會顯示使用的最新的合約版本的工作階段。
-   當要求工作階段通常透過 GET、 轉換和副作用可以觸發，例如，到期逾時。 工作階段總管會顯示工作階段的快照集，因為它會儲存，但不執行任何邏輯、 轉換或副作用。
-   因為 nextTimer JSON 物件欄位計算與副作用的同時，它不存在 MPSD 工作階段。

## <a name="see-also"></a>請參閱

[工作階段概觀](mpsd-session-details.md)

[多人連線的工作階段狀態碼](multiplayer-session-status-codes.md)

[如何：更新多人連線的工作階段](multiplayer-how-tos.md)

[如何：加入從標題啟用 MPSD 工作階段](multiplayer-how-tos.md)

[如何：MPSD 工作階段變更通知訂閱](multiplayer-how-tos.md)

[SmartMatch 配對](smartmatch-matchmaking.md)