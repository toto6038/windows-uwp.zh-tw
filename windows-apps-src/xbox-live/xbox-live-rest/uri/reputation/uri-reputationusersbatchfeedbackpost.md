---
title: POST (/users/batchfeedback)
assetID: f94dcf19-a4e3-5bd0-5276-23e43bdcae52
permalink: en-us/docs/xboxlive/rest/uri-reputationusersbatchfeedbackpost.html
description: " POST (/users/batchfeedback)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0906d32a0e15b2eaaf9c33e7f658e9e9f0cd5124
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622723"
---
# <a name="post-usersbatchfeedback"></a>POST (/users/batchfeedback)
項目的服務使用您的標題介面外的批次形式傳送意見反應。 這些 Uri 的網域是`reputation.xboxlive.com`。
 
  * [要求本文](#ID4EX)
  * [必要的標頭](#ID4E3E)
  * [HTTP 狀態碼](#ID4EWG)
  * [回應主體](#ID4EDAAC)
 
<a id="ID4EX"></a>

 
## <a name="request-body"></a>要求本文 
 
呼叫端必須包含其宣告的憑證，其 web 要求物件的 ClientCertificates 區段中。
 
<a id="ID4EBB"></a>

 
### <a name="required-members"></a>必要的成員 
 
要求應該包含的陣列**BatchFeedback**物件。 
  
<a id="ID4EPB"></a>

 
### <a name="prohibited-members"></a>禁止的成員 
 
所有其他成員均不得在要求中。
  
<a id="ID4E3B"></a>

 
### <a name="sample-request"></a>範例要求 
 

```cpp
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Killed 19 team members in a single session",
            "evidenceId": null
        },
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayQuitter",
            "textReason": "Quit 6 times from 9 sessions",
            "evidenceId": null
        }
    ]
}

      
```

 
| <b>欄位</b>| <b>JSON 類型</b>| <b>描述</b>| 
| --- | --- | --- | 
| 項目| 陣列| 意見反應的 JSON 文件的集合。| 
| targetXuid| 字串| 目標使用者的 XUID| 
| titleId| 字串| 標題，其中此意見反應已傳送或 NULL。| 
| sessionRef| 物件| 物件，描述 MPSD 工作階段，則為 NULL 與此意見反應。| 
| feedbackType| 字串| FeedbackType 列舉中值的字串版本。| 
| textReason| 字串| 提供有關已提交的意見反應詳細呇蜪嚦寄件者的廠商 / 合作夥伴提供文字。| 
| evidenceId| 字串| 可用來當做所提交的意見反應的辨識項資源的識別碼。 例如視訊檔案的識別碼。| 
   
<a id="ID4E3E"></a>

 
## <a name="required-headers"></a>必要的標頭
 
Xbox Live 服務要求時，不需要使用下列的標頭。 

> [!NOTE] 
> 交易夥伴的宣告憑證必須傳送要求以便提交批次意見反應。 


 
| 標頭| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 101| API 合約版本。| 
| Content-Type| 應用程式/json| 正在提交的資料型別。| 
| Authorization| "XBL3.0 x=&lt;userhash>;&lt;token>"| HTTP 驗證的驗證認證。| 
| X-RequestedServiceVersion| 101| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。| 
  
<a id="ID4EWG"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 400| 錯誤的要求| 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
| 401| 未經授權| 要求需要使用者驗證。| 
| 404| 找不到| 找不到指定的資源。| 
| 500| 內部伺服器錯誤| 伺服器發生未預期的狀況，導致無法完成要求。| 
| 503| 服務無法使用| 要求已節流處理，以秒為單位 （例如 5 秒之後） 的用戶端重試值之後，再次嘗試要求。| 
  
<a id="ID4EDAAC"></a>

 
## <a name="response-body"></a>回應主體 
 
如果呼叫成功，會不傳回任何物件，此回應中。 否則，服務會傳回[ServiceError](../../json/json-serviceerror.md)物件。
  
<a id="ID4EXAAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EZAAC"></a>

 
##### <a name="parent"></a>父系 

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

  
<a id="ID4EFBAC"></a>

 
##### <a name="reference"></a>參考 

[Feedback (JSON)](../../json/json-feedback.md)

 [ServiceError (JSON)](../../json/json-serviceerror.md)

   