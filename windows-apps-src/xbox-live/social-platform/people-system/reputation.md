---
title: 評價
description: 了解如何 Xbox Live 使用信譽服務來促進正數的遊戲。
ms.assetid: f8966184-5db7-4cab-93ca-9a0250a6077d
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 信譽、 社交平台
ms.localizationpriority: medium
ms.openlocfilehash: 4e7763294458f19509d5b797a6fa10e03678abee
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641813"
---
# <a name="reputation"></a>評價

評價使用者統計資料，就像任何其他的且包含在呼叫擷取所有使用者的統計資料和報告和追蹤中使用這些。 由本身的評價**信譽類別**。 **ReputationService 類別**代表信譽服務。 對應 Uri 中所述**信譽 Uri**。

> [!IMPORTANT]
> 評價統計資料是全域的跨服務時，不受限於特定的標題。 服務的服務組態識別碼 (SCID) 是用來存取信譽統計資料的全域 SCID。


## <a name="features-of-the-reputation-service"></a>Reputation Service 的功能

「 信譽服務：

-   相同的方式處理意見反應和抱怨。 實體提交意見的相關使用者，與此意見反應會影響使用者的信譽。 意見反應可以再轉送給強制執行小組，以取得進一步的動作。
-   可讓使用者能夠對其他使用者提供意見反應。 標題可以自動提交意見反應。
-   允許項目直接存取 API。 使用者可以提交意見反應直接從內遊戲，並從 [遊戲] 區域，其中使用者是目前的內容。
-   控制代碼低影響哪些使用者可執行在 Xbox Live 和項目內的評價。 因此使用者必須留意其信譽和 act 更適當地在線上玩遊戲。
-   允許的正面回饋意見，以及負面意見反應。 協助 Xbox 社群或項目的社群的使用者可以獲得的貢獻，並它們甚至提供特殊權限。
-   衍生單一所代表的整體評價**Reputation.OverallReputation 屬性**。 它被衍生自下列的信譽類型：

    -   公平播放。 由**Reputation.FairplayReputation 屬性**。
    -   通訊。 由**Reputation.CommunicationsReputation 屬性**。
    -   使用者產生的內容 (UGC)。 由**Reputation.UserGeneratedContentReputation 屬性**。

請參閱**ResetReputation (JSON)** 如需詳細資訊。


## <a name="usage-flow-for-the-reputation-service"></a>Reputation Service 的使用流程

「 信譽服務的整體流程有兩個階段： 報告信譽和評價篩選配對。


## <a name="reporting-reputation"></a>報告的評價

當足夠的負面意見反應已報告的特定使用者時，會將設定標題**Reputation.OverallReputation 屬性**以指出該使用者 （JSON 屬性 OverallReputationIsBad） 不正確的評價。 有沒有抱怨的時間，使用者的信譽會緩慢提升，您也可以達到良好的信譽一次一次不正確的信譽的使用者。


## <a name="reputation-filtered-matchmaking"></a>評價篩選配對

標題的信譽篩選的配對，指定由 Xbox 需求 (XR)，擷取玩家的**Reputation.OverallReputation 屬性**。 這個值會是分數指出的播放程式的整體評價狀態。

> [!NOTE]
> 如果您的標題 OverallReputationIsBad 屬性在 JSON 檔案中，尋找，但找不到它，它是安全地假設使用者有良好的信譽。 這可能是使用新的帳戶，直到已處理的使用者的意見反應。 任何其他使用者的意見反應的播放器永遠會信譽統計資料和值**Reputation.OverallReputation**屬性。


## <a name="reputation-as-an-indicator-of-behavior"></a>評價為指標的行為

評價提供使用者系統上的運作方式的指標。 比方說，該人員易記的播放程式是否為？ 從其他工作階段成員的意見反應判斷玩家的評價。 每位使用者已傳輸的信譽分數與該員 everywhere Xbox One 上。 它會以突顯的方式公開在社交朋友所見，以便它們可以套用對等體壓力到行為更好的播放程式的位置。 例如：

-   它是在每個使用者的設定檔卡片。 在系統中的任何人都可以查看使用者的設定檔，只要按一下。
-   它會顯示多人遊戲。 當使用者播放一起線上群組的信譽等於播放程式的評價，群組中最低的信譽。 類似的信譽與群組只比對與其他人。 其他播放程式會使用聲譽決定什麼種類的播放程式會在該遊戲中，以及決定是否要加入的遊戲。


## <a name="key-elements-of-reputation"></a>評價的索引鍵的項目

標題可以判斷是否使用者已達到避免我 fair play、 通訊或使用者產生的內容 (UGC) 類別層級。 使用者已達避免我相關聯的分類如果任何一個的下列屬性設定為 1:

-   **Reputation.OverallReputation 屬性**。 OverallReputationIsBad 相關聯的 JSON 屬性。
-   **Reputation.FairplayReputation 屬性**。 FairplayReputationIsBad 相關聯的 JSON 屬性。
-   **Reputation.CommunicationsReputation 屬性**。 CommsReputationIsBad 相關聯的 JSON 屬性。
-   **Reputation.UserGeneratedContentReputation 屬性**。 UserContentReputationIsBad 相關聯的 JSON 屬性。


## <a name="good-game-reports"></a>良好的遊戲報表

除了報告不正確的動作的使用者，使用者也可以傳送彼此良好遊戲的報表，可以建立在標題為投票給最有價值的播放程式。


## <a name="feedback-history"></a>意見反應歷程記錄

意見反應歷程記錄報告的資訊，例如當使用者上次回報和為何該人員的時間所報告，比方說，「 最近收到的意見反應通訊樣式。 」


## <a name="xbox-requirement-for-reputation"></a>評價的 Xbox 需求

Xbox 需求 (XR) 068，配對篩選評價，需要從具有高評價的低信譽的玩家區隔。 這是藉由查看**Reputation.OverallReputation**玩家、 玩家的 OverallReputationIsBad 屬性，在使用者統計資料中的值。

標題可以來傳遞 「 OverallReputation"中擷取使用者的信譽*statisticName*參數**UserStatisticsService.GetSingleUserStatisticsAsync 方法 （字串、 字串、 字串）**. WinRT 方法呼叫**取得 (/users/xuid({xuid})/scids/{scid}/stats)** 下列 JSON 主體中所示。

```json
    {
      "requestedusers": [
        "2533274792693551"
      ],
      "requestedscids": [
        {
          "scid": "7492baca-c1b4-440d-a391-b7ef364a8d40",
          "requestedstats": [
            "OverallReputationIsBad",
            "FairplayReputationIsBad",
            "CommsReputationIsBad",
            "UserContentReputationIsBad"
          ]
        }
      ]
    }
```

當使用者的意見反應，來自其他玩家達到低層級時，OverallReputationIsBad 設為 1 的使用者。 對象的人**Reputation.OverallReputation**為 1 只應該符合需要的其他人**OverallReputation**設為 1。 根據預設，當使用者輸入相符項目，它們通常不需要處理低信譽的人員。 標題可以選擇性地允許信譽良好的播放程式，以符合與低評價玩家選擇加入。

XRs 信譽適用於最新版本，請參閱[Xbox 需求](https://developer.xboxlive.com/en-us/platform/certification/requirements/Pages/home.aspx)遊戲開發人員網路 (GDN)。


## <a name="creating-users-with-bad-overall-reputation-for-testing"></a>建立不正確的整體評價進行測試使用者

進行測試，可以將很差的信譽的使用者設定來測試篩選項目的實作的相符項目。 若要設定特定值如使用者、 測試標題或工具會呼叫**POST (/users/xuid({xuid})/resetreputation)** 設定的使用者特定的信譽值的 JSON 檔案。 請參閱**ResetReputation (JSON)** 如需詳細資訊。

比方說，下列 JSON 主體會將使用者的公平 play 信譽設為很差的值：

```json
    {
      "fairplayReputation": 5,
      "commsReputation": 75,
      "userContentReputation": 75
    }
```

合作夥伴可以進行此呼叫，零售除外的所有沙箱。 此要求設定使用者的基底的信譽分數，並正面回饋意見的玩家的加權會全部會歸零。在這個呼叫之後的使用者的實際信譽包含這些基底的分數加上玩家的大使獎金，再加上玩家的粉絲加分。 這項機制會建立具有較低的分數的使用者並**Reputation.OverallReputation**来測試是否符合篩選條件 XR 設為 1。 標題可以使用者統計資料服務中，從使用者取得使用者的信譽打分數，本主題的 「 Xbox 信譽需求 」 一節中所述。


## <a name="resetting-a-users-reputation-to-the-defaults"></a>正在將使用者的信譽的預設值

標題會設定 OverallReputationIsBad 屬性，以指出使用者有良好的信譽。 它會呼叫**POST （/使用者/我/resetreputation）** 並將所有的值設為 75。 呼叫一次 **/users/xuid({xuid})/deleteuserdata**可用來重設的多個 Xbox 使用者識別碼。 要求主體是：

```json
    {
      "xuids": [
        "2814659110958830"
      ]
    }
```

## <a name="sending-feedback-from-titles"></a>從標題傳送意見反應

標題可以比對傳送 fair 播放播放程式意見。 這是直接從標題或標題中的服務批次。 可以使用標題**ReputationService.SubmitReputationFeedbackAsync 方法**或下列直接 REST 方法：

|                      |                                                             |
|----------------------|-------------------------------------------------------------|
| 用戶端文章          | https://reputation.xboxlive.com/users/xuid{XUID}/feedback |
| 服務 （批次） 文章 | https://reputation.xboxlive.com:10443/users/batchfeedback   |

請參閱**意見反應 (JSON)** 提交和允許的欄位定義的範例 JSON 主體。


## <a name="types-of-feedback-allowed"></a>類型的允許的意見反應

中所定義的類別可以張貼的意見反應**意見反應 (JSON)**。


## <a name="when-titles-should-send-feedback"></a>當項目應該傳送意見反應

事件造成負面影響遊戲的玩家體驗時，標題應該傳送負面意見反應。 一般而言，標題應該傳送只有一個意見反應，每個類型，每個往返播放。 例如，標題應該：

1.  只能傳送一個 FairPlayKillsTeammates 意見項目，刪除 3 個或多個小組成員，而不是傳送一個事件，每當該人員會終止組員循每個人的。
2.  當有人特意不結束比對早期，請傳送 FairplayQuitter 意見項目。
3.  競爭情況，每一次 FairplayUnsporting 意見項目時傳送使用者致力回溯賽車遊戲中。
