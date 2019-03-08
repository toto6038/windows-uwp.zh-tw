---
title: POST (/users/xuid({xuid})/feedback)
assetID: 48a7a510-a658-f2a3-c6bc-28a3610006e7
permalink: en-us/docs/xboxlive/rest/uri-reputationusersxuidfeedbackpost.html
description: " POST (/users/xuid({xuid})/feedback)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d8a6e118811203fd34c310840e8688c2255c6beb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660043"
---
# <a name="post-usersxuidxuidfeedback"></a>POST (/users/xuid({xuid})/feedback)
如果您想要在您的遊戲，而不被使用命令介面中新增意見反應 選項，請使用從您的標題。 這些 Uri 的網域是`reputation.xboxlive.com`。
 
  * [URI 參數](#ID4EZ)
  * [必要的要求標頭](#ID4EEB)
  * [要求本文](#ID4ENC)
  * [必要的標頭](#ID4EDE)
  * [Authorization](#ID4EXF)
  * [HTTP 狀態碼](#ID4EEG)
  * [回應主體](#ID4EZH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| ulong| Xbox 使用者識別碼 (XUID) 報告的使用者。| 
  
<a id="ID4EEB"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：「 XBL3.0 x =&lt;userhash >;&lt;權杖 > 」。| 
| X-RequestedServiceVersion|  | 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 預設值：101.| 
  
<a id="ID4ENC"></a>

 
## <a name="request-body"></a>要求本文 
 
<a id="ID4EVC"></a>

 
### <a name="required-members"></a>必要的成員 
 
要求應該包含[意見反應](../../json/json-feedback.md)物件。 
  
<a id="ID4EED"></a>

 
### <a name="prohibited-members"></a>禁止的成員 
 
所有其他成員均不得在要求中。
  
<a id="ID4ETD"></a>

 
### <a name="sample-request"></a>範例要求 
 

```cpp
{
    "sessionRef":
    {
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

   
<a id="ID4EDE"></a>

 
## <a name="required-headers"></a>必要的標頭
 
Xbox Live 服務要求時，不需要使用下列的標頭。
 
| <b>標頭</b>| <b>值</b>| <b>Deacription</b>| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 101| API 合約版本。| 
| Authorization| XBL3.0 x=[hash];[token]| STS 驗證語彙基元。 驗證要求所傳回的語彙基元取代 STSTokenString。| 
Content-Type| 
應用程式/json| 
正在提交的資料型別。| 
  
<a id="ID4EXF"></a>

 
## <a name="authorization"></a>Authorization
 
要求必須包含有效的 Xbox Live 授權標頭。 如果呼叫端不允許存取此資源中，服務就會傳回 403 禁止程式碼。 如果標頭無效或遺失，則服務會傳回 401 未經授權的程式碼。
  
<a id="ID4EEG"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| 沒有內容| 要求已完成，但並沒有可傳回的內容。| 
| 401| 未經授權| 要求需要使用者驗證。| 
| 404| 找不到| 找不到指定的資源。| 
| 406| 無法接受| 不支援資源版本。| 
| 408| 要求逾時| 該要求花了太多時間完成。| 
| 409| 衝突| 接續權杖不再有效。| 
  
<a id="ID4EZH"></a>

 
## <a name="response-body"></a>回應主體 
 
如果呼叫成功，會不傳回任何物件，此回應中。 否則，服務會傳回[ServiceError](../../json/json-serviceerror.md)物件。
  
<a id="ID4EOAAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EQAAC"></a>

 
##### <a name="parent"></a>父系 

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

  
<a id="ID4E3AAC"></a>

 
##### <a name="reference"></a>參考 

[Feedback (JSON)](../../json/json-feedback.md)

 [ServiceError (JSON)](../../json/json-serviceerror.md)

   