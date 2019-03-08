---
title: 使用 SmartMatch 配對
description: 了解如何使用 Xbox Live SmartMatch 比對的多人連線遊戲。
ms.assetid: 10b6413e-51d9-4fec-9110-5e258d291040
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、windows xbox one，多人遊戲，配對，smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 89f33768efcd649987866fd0798c222aa97f7ff8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592093"
---
# <a name="using-smartmatch-matchmaking"></a>使用 SmartMatch 配對

下列流程圖說明 SmartMatch 配對程序。

![](../../images/multiplayer/Multiplayer_2015_SmartMatch_Matchmaking.png)

## <a name="creating-a-match-ticket-session-and-a-match-ticket"></a>建立比對票證工作階段，而且符合票證

開始之前配對，配對"scout 」 設定的相符項目票證工作階段來表示一群人想要輸入配對在一起。 此群組中的所有使用者都加入工作階段使用**MultiplayerSession.Join 方法 （字串、 布林值、 布林值）**。

一旦建立並填入玩家票證工作階段，標題送出到配對的服務使用的工作階段**MatchmakingService.CreateMatchTicketAsync 方法**。 這個方法會建立比對票證，其代表的票證工作階段中，並更新 /servers/matchmaking/properties/system/status 欄位票證工作階段，「 搜尋 」 中。 如需詳細資訊，請參閱[How to:建立比對票證](multiplayer-how-tos.md)。

比對的票證建立方法的回應**CreateMatchTicketResponse 類別**物件。 回應會包含比對的票證識別碼，可以使用的 GUID 可用來取消藉由刪除票證的配對。 回應也包含 hopper，可用來設定使用者期望的平均等候時間。


## <a name="setting-matchmaking-attributes-on-the-session-and-players"></a>在工作階段和播放器上設定配對屬性

當提交配對的工作階段，標題就可以設定配對服務用來群組與其他工作階段的工作階段的屬性。 在票證層級或成員層級，可以指定屬性。


### <a name="setting-matchmaking-attributes-at-the-match-ticket-level"></a>在比對的票證層級設定配對屬性

標題送出的層級符合票證中的屬性*ticketAttributesJson*的參數**MatchmakingService.CreateMatchTicketAsync**方法。 在票證層級指定的屬性會覆寫在每個成員層級指定的相同屬性。


### <a name="setting-matchmaking-attributes-at-the-per-member-level"></a>在每個成員層級設定配對屬性

標題會指定符合票證工作階段中每個成員上的每個成員的屬性。 這些設定藉由呼叫**MultiplayerSession.SetCurrentUserMemberCustomPropertyJson 方法**，使用 「 matchAttrs"的屬性名稱。 此呼叫置於屬性 /members/ {index} / 屬性/自訂/matchAttrs 票證工作階段中欄位每個播放程式上。

配對程序 「 壓平合併 」 每個成員每個單一票證層級屬性，根據該屬性 hopper 的 Xbox Live 組態中指定的壓平合併方法。 這可以在 設定[XDP](https://xdp.xboxlive.com)或是[合作夥伴中心](https://partner.microsoft.com/dashboard)。


## <a name="making-the-match"></a>使比對

票證工作階段和票證設定配對服務相符的項目代表其他群組中，其他票證工作階段的票證表示工作階段並建立或識別比對目標工作階段。 此服務也會保留項目相符的玩家建立目標工作階段中，並視為相符，然後將標記票證工作階段。 MPSD 通知票證工作階段的這項變更的標題。

現在標題必須採取步驟來初始化目標工作階段，以確認足夠玩家已顯示，然後執行品質的服務 (QoS) 檢查，以確保它們可以連接至另一部已成功。 如果初始化和/或 QoS 失敗，標題就會標示重新提交至配對的票證工作階段，讓您可以找到另一個群組。 如需有關處理程序的詳細資訊，請參閱[目標工作階段初始化與 QoS](smartmatch-matchmaking.md)。

比對活動期間會以 JSON 物件的工作階段進行下列變更：

-   /servers/matchmaking/properties/system/status 欄位設定為 「 找到 」
-   /servers/matchmaking/properties/system/targetSessionRef 欄位設為目標的工作階段
-   /members/ {index} / 屬性/自訂每個票證工作階段的 matchAttrs 欄位複製到 /members/ {index} / 常數/自訂/matchmakingResult/playerAttrs 欄位
-   對於每個播放器，票證屬性從 ticketAttributes 票證中欄位相符項目複製到 /members/ {index} / 常數/自訂/matchmakingResult/ticketAttrs 欄位


## <a name="maintaining-the-match-ticket"></a>維護的相符項目票證

配對服務會在相符項目票證建立工作階段的時間使用票證工作階段的快照集。 因此，如果任何玩家加入或離開票證工作階段，標題必須刪除並重新建立比對票證使用配對的服務。


## <a name="reusing-the-game-session-as-a-match-ticket-session"></a>重複使用的相符項目票證工作階段的遊戲工作階段

| 重要事項                                                                                                                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 請務必了解與該兩個工作階段*preserveSession*設為永遠無法比對，因為無法將它們合併。 項目所使用的配對流程應將這一點納入考量。 |

標題可以重複使用現有的遊戲工作階段，若要尋找更多玩家加入已在進行中的遊戲的相符項目票證工作階段。 若要啟用此功能，標題必須藉由呼叫建立符合票證**CreateMatchTicketAsync**具有*preserveSession*參數設定為**PreserveSessionMode.Always**. 票證所使用的現有工作階段會保留在配對程序，並會成為產生的目標工作階段，然後可確保 「 配對 」 服務。


## <a name="deleting-the-match-ticket"></a>刪除符合票證

若要刪除的相符項目票證，此標題會呼叫**MatchmakingService.DeleteMatchTicketAsync 方法**。 刪除的票證：

1.  票證工作階段中的使用者的停駐點配對。
2.  更新票證工作階段中的 /servers/matchmaking/properties/system/status 欄位 「 取消 」。


## <a name="performing-matchmaking-for-games-using-xbox-live-compute"></a>使用 Xbox Live 計算的遊戲執行配對

以下是動作，以取得使用者 matchmade 至 Xbox Live 計算為基礎的遊戲的概要步驟。 類似的流程應該適用於協力廠商所裝載的遊戲。
1.  Scout 建立票證工作階段來代表該群組。 此工作階段包含一份潛在的資料中心，位於 /constants/system/measurementServerAddresses 中的工作階段設定。 它會從任一個工作階段範本如果資料中心清單是靜態的或寫信之後予以 Xbox Live 計算從第一個工作階段建立用戶端。 此工作階段也包含 gsiSetId、 gameVariantId 和 maxAllowedPlayers targetSessionConstants/自訂/gameServerPlatform 物件中的值。
2.  在群組中的所有用戶端會加入此票證工作階段。
3.  群組的所有成員，可從 /constants/system 物件下載 measurementServerAddresses 值，票證工作階段，ping 它們使用的平台 API，並在 /members/ {index} 中所定義，將慣用的資料中心的已排序的清單上傳至工作階段中，/properties/system/serverMeasurements。

| 注意                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 標題可以設定和擷取工作階段使用 measurementServerAddresses 值**MultiplayerSession.SetMeasurementServerAddresses**方法和**MultiplayerSessionConstants.MeasurementServerAddressesJson 屬性**。 |

4.  Scout 呼叫**CreateMatchTicketAsync**，傳遞票證工作階段的參考。

| 注意                                                                                                                                                                                                         |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 如果票證工作階段物件有不相符的常數，建立票證方法可能不會成功。 這可以避免加入不可 hopper，若要避免使用具有不相符的常數的球員比對的規則。 |

如果**MatchTicketDetailsResponse.PreserveSession 屬性**設為 [永不]，配對服務伺服器測量結果從複製的每個成員到票證的內部表示法。 它會簡維成單一伺服器的度量集合票證，並儲存為 「 特殊 」 票證屬性之票證的內部表示法中之成員的票證伺服器度量。

如果**MatchTicketDetailsResponse.PreserveSession**已設定為永遠，度量不會使用伺服器。 相反地，配對服務 /properties/system/matchmaking/serverConnectionString 會將值複製工作階段的票證，為 serverMeasurements 集合大小為 1 具有零延遲的內部表示法。

5.  配對服務符合票證工作階段與其他人代表其他群組，讓伺服器至帳戶的度量集合。 它會嘗試比對具有高慣用的相同資料中心的其他群組的群組。
6.  一旦已找到相符的群組，配對服務就會建立或識別目標工作階段並將所有玩家一起比對的票證工作階段。 服務會將相符的群組的最終的扁平化的伺服器測量值寫入至 /properties/system/serverConnectionStringCandidates。 它會將原本送出的伺服器度量單位，每個新加入成員的目標工作階段中寫入 /members/ {index} / 常數/系統/matchmakingResult/serverMeasurements。
7.  所有用戶端會執行初始化，如上面所討論的目標工作階段上。 不過，用戶端會連線到 Xbox Live Compute，因為它們不會執行 QoS 互相確認連線能力。
8.  部分或所有用戶端呼叫**GameServerPlatformService.AllocateClusterAsync 方法**。 第一個會觸發配置，而其他人收到確認通知。 方法會取得從 MPSD 的目標工作階段，並選擇根據資料中心喜好設定中的目標工作階段中，以及負載和其他 Xbox Live 計算專屬知識的資料中心。
9.  所有用戶端播放。


## <a name="see-also"></a>請參閱

[SmartMatch 執行階段作業](smartmatch-matchmaking.md)

[SmartMatch 配對](smartmatch-matchmaking.md)

**Microsoft.Xbox.Services.Matchmaking 命名空間**

**Microsoft.Xbox.Services.Multiplayer 命名空間**
