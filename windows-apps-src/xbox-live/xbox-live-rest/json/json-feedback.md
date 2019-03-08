---
title: Feedback (JSON)
assetID: 726117c1-f01b-18c0-3b75-a7a7d27d84a2
permalink: en-us/docs/xboxlive/rest/json-feedback.html
description: " Feedback (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9cc1cb4ecc12219d54af1c4ab420671f2bbfa81f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644763"
---
# <a name="feedback-json"></a>Feedback (JSON)
包含有關播放程式的意見反應資訊。
<a id="ID4EN"></a>


## <a name="feedback"></a>意見反應

意見反應物件具有下列規格。

| 成員| 類型| 描述|
| --- | --- | --- |
| sessionRef| 物件 | 物件，描述 MPSD 工作階段，則為 null 與此意見反應。 |
| feedbackType| 字串 | 意見反應的型別。 可能的值會定義於<b>Microsoft.Xbox.Services.Social.ReputationFeedbackType</b>。 |
| textReason| 字串| 已提交的意見反應的原因說明加入寄件者的使用者提供文字。 |
| voiceReasonId| 字串| 加入意見反應的原因說明的傳送者的 Kinect 從使用者提供語音檔案的識別碼送出 (Base-64)。 |
| evidenceId| 字串| 可用來當做所提交的意見反應的辨識項資源的識別碼，例如，視訊檔案記錄在進行遊戲。 |

<a id="ID4EVC"></a>


### <a name="feedback-types"></a>意見反應類型

「 傳送 」 資料行指出哪些人可以提交的意見反應。

   * 「 使用者 」 表示它可以提交的驗證使用 XToken 主控台，因此 API 可接受**SubmitFeedback**。
   * 「 夥伴 」 表示它可以提交由合作夥伴使用宣告的憑證，因此 API 可接受**SubmitBatchFeedback**。
   * [隱私權] 表示只有 SLS 隱私權服務可以傳送意見反應。
   * "None"表示的意見反應進行稽核 SLS 信譽服務會在內部產生，而且無法傳送的任何呼叫端。

| 類型| 由傳送| 附註|
| --- | --- | --- | --- | --- | --- |
| CommsAbusiveVoice| 使用者| 使用者傳送意見反應，以報告不當的語音通訊從標題內和從 Xbox 儀表板。 |
| CommsInappropriateVideo| 合作夥伴的使用者，| 使用者和合作夥伴的意見來報告不當視訊從標題內和從 Xbox 儀表板。 |
| CommsMuted| 隱私權| 當使用者靜音其他播放器時，隱私權會將此意見反應傳送到 reputation service。 |
| CommsPhishing| 使用者| 使用者傳送這個回應，回報網路釣魚的訊息。 |
| CommsPictureMessage| 使用者| 收件匣服務呼叫的信譽服務，也會更新一張圖片的通訊為基礎的寄件者的信譽和報告給強制執行小組的意見反應。 |
| CommsSpam| 使用者| 使用者傳送此報告的垃圾郵件訊息的意見反應。 |
| CommsTextMessage| 使用者| 收件匣服務呼叫的信譽服務，也會更新 寄件者信譽並報告給強制執行小組的意見反應。 **注意：** 收件匣 UI 應該有按鈕可讓訊息加上旗標的使用者。 |
  | CommsVoiceMessage | 使用者 | 收件匣服務呼叫的信譽服務，也會更新語音訊息的通訊為基礎的寄件者的信譽和報告給強制執行小組的意見反應。  |
  | FairPlayBlock | 隱私權 | 隱私權時使用者會封鎖其他播放器，將此意見反應傳送到 reputation service。  |
  | FairPlayCheater | 合作夥伴的使用者， | 判斷使用者跟作弊沒什麼兩樣的標題可以傳送此意見反應，而不需使用者介入。  |
  | FairPlayConsoleBanRequest | 合作夥伴 | 夥伴會傳送此意見反應，以禁止從 Xbox Live 的主控台建議的形式。  |
  | FairPlayIdler | 合作夥伴的使用者， | 決定是否使用者代表閒置刻意在遊戲中，通常是圓形往返之後, 的項目，可以傳送此意見反應，而不需使用者介入。  |
  | FairPlayKicked | 合作夥伴的使用者， | 偵測使用者被投票離開遊戲 （開始） 的項目，可以傳送此意見反應，而不需使用者介入。  |
  | FairPlayKillsTeammates | 合作夥伴的使用者， | 當播放程式 killls 可以自動判斷的標題他的小組可以傳送此意見反應，而不需使用者介入。  |
  | FairPlayQuitter | 合作夥伴的使用者， | 判斷使用者提早結束遊戲的標題可以傳送此意見反應，而不需使用者介入。  |
  | FairPlayTampering | 合作夥伴的使用者， | 判斷使用者竄改上的磁碟內容的項目，可以傳送此意見反應，而不需使用者介入。  |
  | FairPlayUnblock | 隱私權 | 隱私權會將此意見反應傳送至信譽服務中，當使用者解除封鎖其他播放器。  |
  | FairPlayUserBanRequest | 合作夥伴 | 夥伴會傳送此意見反應，以禁止從 Xbox Live 使用者建議的形式。  |
  | InternalAmbassadorScoreUpdated | 無 | 這是內部的意見反應類型，不供呼叫端使用。  |
  | InternalReputationReset | 無 | 這是內部的意見反應類型，不供呼叫端使用。  |
  | InternalReputationUpdated | 無 | 這是內部的意見反應類型，不供呼叫端使用。  |
  | PositiveHelpfulPlayer | 合作夥伴的使用者， | 使用者和合作夥伴傳送此意見反應提交正面資訊很有幫助其他播放器從內遊戲、 論壇和等等。  |
  | PositiveHighQualityUGC | 合作夥伴的使用者， | 使用者和合作夥伴傳送這個回應，指出標題應該允許使用者提交上從共用的 UGC 在遊戲中，正面回饋意見，例如微調 Forza 中的設定。  |
  | PositiveSkilledPlayer | 合作夥伴的使用者， | 使用者和合作夥伴會傳送這個回應，指出標題可以允許使用者以票選 MVP MPSD 工作階段的結尾。  |
  | UserContentGamerpic | 使用者 | 使用者傳送這個回應，回報為不適當的玩家圖片，直接從玩家的卡片。  |
  | UserContentGamertag | 使用者 | 使用者傳送此報告不當的玩家代號，直接從玩家卡片的意見反應。  |
  | UserContentInappropriateUGC | 合作夥伴的使用者， | 使用者和合作夥伴會傳送這個回應，指出標題應該允許使用者不當共用的 UGC 從在遊戲中，加上旗標，例如 [小畫家] 中 Forza 的作業。  |
  | UserContentPersonalInfo | 使用者 | 使用者傳送此報告自傳和其他個人資訊，直接從玩家卡片的意見反應。  |

<a id="ID4EFEAC"></a>


## <a name="sample-json-syntax"></a>範例 JSON 語法


```json
{
"sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Halo556932",
  },
  "feedbackType": "CommsAbusiveVoice",
  "textReason": "He called me a doodoo-head!",
  "voiceReasonId": null,
  "evidenceId": null
}

```


<a id="ID4EOEAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EQEAC"></a>


##### <a name="parent"></a>父系

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)
