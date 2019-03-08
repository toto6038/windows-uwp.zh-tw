---
title: 從您的標題傳送播放程式意見反應
description: 了解您的標題可以協助將播放程式意見反應傳送到 Xbox Live reputation service 升級正玩家遊戲體驗。
ms.assetid: 49f8eb44-1e31-4248-b645-9123df6f8689
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 信譽、 播放程式意見反應
ms.localizationpriority: medium
ms.openlocfilehash: 3e8d82fc9b195f174bf7a46a8d21fb20b2df9551
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592233"
---
# <a name="sending-player-feedback-from-your-title"></a>從您的標題傳送播放程式意見反應
大部分的 Xbox Live 的成員是太棒了，但有一小部分的"錯誤 Apples"會損及其他人的遊戲體驗。 我們識別的使用者透過使用者這些小百分比，和標題提交的意見反應。 我們幫助保護母體擴展的其餘部分確保這些 「 不正確的蘋果 」 有限制的多人遊戲體驗它們不會影響與良好玩家的遊戲。 Xbox 依賴報告讓系統維持在正確的其他使用者的使用者，但在 Xbox One 的標題可以直接參與，並大幅改善使用者的信譽母體擴展的精確度。

## <a name="steps-to-submit-feedback-from-title-or-title-service"></a>若要提交意見反應標題或標題服務的步驟
1. 加入標題或標題服務的意見反應時間
2. 判斷正確的意見反應類型
3. 呼叫信譽與意見反應的意見反應 Api

### <a name="adding-feedback-moments-to-title-or-title-service"></a>加入標題或標題服務的意見反應時間
所有的玩家有不正確的體驗與組員破壞他們自己的側邊，只是而非主動播放，就能解決的球員或 cheaters 使用者讓他們的遊戲。 Xbox Live 可讓使用者用來直接回報這些問題的參與者，但使用者意見反應並不完美。 簡單的事情讓玩家在遊戲中讓閒置和提早結束，有時甚至可以決定當有人作弊，可以輕易地判斷標題。 您的標題可以提交意見反應，在各種不同的意見反應時間，這有助於改善所有很好的播放程式的體驗。

### <a name="determining-the-correct-feedback-type"></a>判斷正確的意見反應類型
評價系統有許多要擷取的各種方式，使用者可能會保證意見反應的意見反應類型。 他們完全詳列於下面 表 1。 其中大部分都是負數，但可改善使用者的信譽正數的意見反應。

Xbox 系統 UI 可以讓玩家提交意見反應，對其他使用者在遊戲中。 該使用者對使用者意見反應並不包含太多的權重，因為使用者是容易 griefing 另一個時，他們會失去。 標題可補充該系統提供的使用者可直接提交的意見反應的 UI 的 UI，但相反地，我們比較喜歡用標題提交意見反應代表標題本身使用合作夥伴的意見。 合作夥伴的意見是受高度信任。

## <a name="example-partner-feedback-usage-scenarios"></a>合作夥伴的意見反應使用案例範例

### <a name="user-quitting-a-game-in-the-middle-of-a-match"></a>使用者結束遊戲中間相符項目
播放程式正失去遊戲，並使用來結束遊戲，遊戲的功能表，放棄她的小組成員。 當標題偵測到此行為可以回報行為使用**FairPlayQuitter。**

### <a name="idling-after-match-found-in-game"></a>在遊戲中找到的相符項目之後讓閒置
播放程式取得使用其他播放器來播放，比對，但代表閒置而不是幫助小組遊戲中的一致的方式。 標題可以報告播放程式的行為使用**FairplayIdler。**

### <a name="killing-teammates-in-game"></a>在遊戲中的終止小組成員
遊戲中的播放程式會不斷地刪除為了好玩的小組成員。 當使用者以一致的方式是偵測到的標題小組終止它可以報告使用播放程式**FairPlayKillsTeammates。**

### <a name="title-has-community-kickvote-feature"></a>標題有社群執行什麼功能
播放器有尚未表決回合中的播放器移除不正確的行為的工作階段。 如果標題會從工作階段中移除該播放程式也可以報告的使用者**FairPlayKicked。**

### <a name="helpful-player-community-vote"></a>很有幫助的播放程式社群投票
後幾個回合的遊戲中，標題會提供挑選 contourglobal 的大部分人的選項。 當標題看到這個動作可以報告的行為使用**PositiveHelpfulPlayer。**

### <a name="high-quality-ugc-user-generated-content"></a>高品質 UGC （使用者產生的內容）
標題有的場景，播放程式可以在其中挑選一種工具最適合的設計。 當標題看到這個動作可以報告的行為使用**PositiveHighQualityUGC。**

### <a name="skilled-player"></a>技術熟練的播放程式
後幾個回合的遊戲中，標題會提供挑選的人員是 MVP，是最佳的播放程式的選項。 當標題看到 player consistenly 獲得 MVP 榮耀，它可以報告的行為使用**PositiveSkilledPlayer。**

### <a name="user-reports-questionable-ugc-in-title"></a>報表標題中的可疑 UGC 使用者。
標題有的場景，播放程式可以在這裡檢視車輛設計。 玩家通知具冒犯意味的設計，而且想要回報。 當標題看到此動作時，它就會報告使用敲打**UserContentInappropriateUGC。**

### <a name="title-wants-to-request-an-xbl-ban-review-of-a-player-in-their-title"></a>想要要求 XBL 禁止檢閱其標題中的播放程式的標題
項目的社群經理已偵測到低信譽播放程式以一致的方式在他們的遊戲中造成問題。 標題可以要求 XBL 原則並強制執行小組檢閱使用**FairPlayUserBanRequest。**

## <a name="complete-behavior-feedback-options"></a>完成行為的意見反應選項
下表列出可用來代表您的標題提交使用者意見反應的意見反應類型。 「 信譽服務具有彈性，輕鬆地加入新的意見反應類型，如果您認為這些不符合您的標題的需求。 請告訴您的帳戶管理員是否您想要查看新加入的意見反應類型。

表 1:各種合作夥伴的意見類型信譽服務支援。

**Fairplay 的意見反應類型**               | **描述**
----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------
FairplayKillsTeammates                    | 報告的玩家，刻意刪除他們自己的小組成員
FairplayCheater                           | 報表的玩家一定跟作弊沒什麼兩樣，
FairplayTampering                         | 報告的玩家，當然有 meddled 與遊戲的磁碟或否則竄改遊戲軟體或硬體
FairplayUserBanRequest                    | 報告的玩家，您將獲得暫止
FairplayConsoleBanRequest                 | 報表主控台您認為應該禁止連接到 Xbox Live
FairplayUnsporting                        | 報告的玩家，是當然算是 unsportsmanlike 管理辦法
FairplayIdler                             | 報表便會進入多人遊戲玩家比對，但無法主動播放
FairplayLeaderboardCheater                | 報告的玩家，當然有作弊才會出現在排行榜
**通訊的意見反應類型**         |
CommsInappropriateVideo                   | 報告的玩家，正在適合用在視訊交談
**使用者已產生內容的意見反應類型** |
UserContentInappropriateUGC               | 報告不當的一段 media player 會建立在您的標題中的內容
UserContentReviewRequest                  | 主動報告內容的部分，以便 XBLPET 小組會檢閱它
UserContentReviewRequestBroadcast         | 主動報告廣播，以便 XBLPET 小組會檢閱它
UserContentReviewRequestGameDVR           | 主動報告 GameDVR 剪輯，以便 XBLPET 小組會檢閱它
UserContentReviewRequestScreenshot        | 主動報告螢幕擷取畫面，以便 XBLPET 小組會檢閱它
**正面回饋意見**                     |
PositiveSkilledPlayer                     | 如果使用者可以投票，以判斷是 MVP，報告技術熟練的播放程式時某些播放程式值得正面回饋意見
PositiveHelpfulPlayer                     | 如果遊戲提供報告另一個很有幫助播放程式的 UI，報告有幫助的播放程式
PositiveHighQualityUGC                    | 如果遊戲提供來補充其他使用者的內容播放程式的 UI，報告內容正面

## <a name="feedback-api-calls"></a>意見反應 API 呼叫
標題可以使用兩種策略來呼叫 Reputation service。 慣用的方法是在彙總的報表使用者從協力廠商服務所使用的服務權杖進行驗證。 標題也可以報告直接從用戶端的使用者。 用戶端 API 有需要的使用者會視為有效的多個報表內建的防詐騙技術。 這兩個 Api 批次，而且同時報告多個使用者。

標題可以使用下列的 Xbox Live Api 提交玩家信譽意見：

語言 | API
-------- | --------------------------------------------------------------------------------------------
WinRT    | Microsoft::Xbox::Services::Social::ReputationService.SubmitBatchReputationFeedbackAsync(...)
C++      | xbox::services::social::reputation_service.submit_batch_reputation_feedback(...)

或者，標題可以使用下列的直接 REST 方法：

API          | URL                                                      | 驗證需求
------------ | -------------------------------------------------------- | ---------------------------------------
服務文章 | https://reputation.xboxlive.com/users/batchfeedback      | 利用合作夥伴與沙箱宣告 S 語彙基元
用戶端文章  | https://reputation.xboxlive.com/users/batchtitlefeedback | Xtoken 具有標題和沙箱的宣告

## <a name="feedback-object"></a>意見反應物件
意見反應物件具有下列規格，如需最新版本，也就是 101。 這兩個 Api 可預期下列物件的批次。

成員       | 類型   | 描述
------------ | ------ | -----------------------------------------------------------------------------------------------------------------------------------------------------------------
sessionRef   | 物件 | 物件，描述 MPSD 工作階段，則為 null 與此意見反應。
feedbackType | 字串 | 意見反應的型別。 可能的值會定義在 ReputationFeedbackType 列舉
textReason   | 字串 | 已提交的意見反應的原因說明加入寄件者的使用者提供文字。 這是我們的原則強制執行小組非常重要。
evidenceId   | 字串 | 可用來當做所提交的意見反應的辨識項資源的識別碼，比方說，視訊檔案記錄遊戲或活動摘要項目。
titleID      | 字串 | 標題的識別碼播放的標題;如果不存在於權杖，才需要。
targetXuid   | 字串 | 您所回報的播放程式 XUID。

範例：

```json
POST https://reputation.xboxlive.com/users/batchtitlefeedback
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId" : null,
            "sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Title56932",
           },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Title detected this player killing team members 19 times",
            "evidenceId": null
        }
    ]
}
```

## <a name="feedback-qa"></a>問與答的意見反應

### <a name="q-can-i-send-a-hint-to-the-system-to-help-with-humans-that-might-be-looking-at-the-player-report"></a>Q:傳送至系統，以協助進行可能會尋找在播放程式報告的人類提示可以嗎？
答：是的而且很有幫助 ！ 您可以使用 「 textReason"參數來協助強制者者最終將探討提交的意見反應。 比方說，針對 idler 中，您可以新增說 「 我們收到任何使用者輸入從這名玩家遊戲的第一個五秒後 」 的文字原因。 這個文字理由可以非常珍貴的 XBLPET 強制代理程式，，因此請確定文字原因，是很有幫助，而且描述性。

### <a name="q-should-i-worry-about-how-often-i-send-in-feedback-on-a-user"></a>Q:我應該擔心我頻率傳送意見給我們的使用者嗎？
答：確定使用者已獲得意見反應時，標題應該呼叫 Reputation service。 系統會有數個以防止標題和使用者能夠使用過度影響使用者的安全攔截。

### <a name="q-can-i-adjust-the-weight-of-the-feedback-being-sent"></a>Q:我是否可以調整傳送意見反應的加權？
答：否，信譽服務決定的意見反應的權數。 我們一律會調整，讓系統更佳的加權。

### <a name="q-can-i-undo-feedback-ive-sent-on-a-user"></a>Q:我可以復原我已傳送的使用者的意見反應嗎？
答：否，意見反應為最終狀態。 如果您認為您的標題有 bug，並正在傳送錯誤意見反應告訴我們，直到修正 bug，我們會列入黑名單的標題。

### <a name="q-can-i-see-the-feedback-sent-for-my-title-from-users"></a>Q:我是否可以看到傳送我的標題，從使用者的意見反應？
答：目前尚無法使用該資訊自助。 您可以要求您的帳戶管理員，我們沒有每個標題，我們可以共用資料。 我們很快就想要將這直接提供給您，並也與評價資料，請使用您的標題中顯示的使用者。

### <a name="q-when-i-send-in-console-or-user-ban-review-request-how-do-i-know-what-happened"></a>Q:當我在主控台中傳送，或使用者禁止檢閱要求如何知道發生了什麼事？
答：目前檢閱的資訊會傳送至 XBL 原則和強制執行小組，但目前您不會更新禁止檢閱。

### <a name="q-is-there-a-reputation-score-per-title"></a>Q:是否有每個標題的信譽分數？
答：不。 目前有信譽 fairplay、 通訊和使用者產生的內容，但不是依照標題的子分數。 我們可能會新增這項功能在未來如果沒有足夠的需求，讓您知道您想要要求該功能的客戶經理。
