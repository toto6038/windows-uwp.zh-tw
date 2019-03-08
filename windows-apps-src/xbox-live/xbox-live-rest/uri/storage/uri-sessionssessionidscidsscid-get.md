---
title: GET (/sessions/{sessionId}/scids/{scid})
assetID: 1feaceed-ba0d-0b0c-e809-44ba41f2e4ea
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidsscid-get.html
description: " GET (/sessions/{sessionId}/scids/{scid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8a0dcd24c57cbc7dce596961afcfd7e0eba476c3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650493"
---
# <a name="get-sessionssessionidscidsscid"></a>GET (/sessions/{sessionId}/scids/{scid})
擷取此儲存體類型的配額資訊。 這些 Uri 的網域是`titlestorage.xboxlive.com`。
 
  * [URI 參數](#ID4EX)
  * [Authorization](#ID4ECB)
  * [必要的要求標頭](#ID4ENB)
  * [要求本文](#ID4EWC)
  * [HTTP 狀態碼](#ID4EBD)
  * [回應主體](#ID4E2H)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| sessionId| 字串| 工作階段的識別碼來查閱。| 
| scid| guid| 要查閱的服務組態的識別碼。| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>Authorization
 
要求必須包含有效的 Xbox LIVE 授權標頭。 如果呼叫端不允許存取此資源中，服務會傳回 403 禁止 回應。 如果標頭無效或遺失，則服務會傳回 401 未授權的回應。 
  
<a id="ID4ENB"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 值| 描述| 
| --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API 合約版本。| 
| Authorization| XBL3.0 x=[hash];[token]| STS 驗證語彙基元。 驗證要求所傳回的語彙基元取代 STSTokenString。 如需關於擷取之 STS 權杖，以及建立授權標頭的詳細資訊，請參閱驗證和授權的 Xbox LIVE 服務要求。| 
  
<a id="ID4EWC"></a>

 
## <a name="request-body"></a>要求本文
 
此要求主體中不傳送任何物件。
  
<a id="ID4EBD"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 確定| 要求成功。| 
| 201| 建立時間| 建立實體。| 
| 400| 錯誤的要求| 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
| 401| 未經授權| 要求需要使用者驗證。| 
| 403| 已禁止| 要求不允許使用者或服務。| 
| 404| 找不到| 找不到指定的資源。| 
| 406| 無法接受| 不支援資源版本。| 
| 408| 要求逾時| 該要求花了太多時間完成。| 
| 500| 內部伺服器錯誤| 伺服器發生未預期的狀況，導致無法完成要求。| 
| 503| 服務無法使用| 要求已節流處理，以秒為單位 （例如 5 秒之後） 的用戶端重試值之後，再次嘗試要求。| 
  
<a id="ID4E2H"></a>

 
## <a name="response-body"></a>回應主體
 
如果呼叫會成功，則服務會傳回[quotaInfo (JSON)](../../json/json-quota.md)物件。 
 
<a id="ID4EKAAC"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
  "quotaInfo":
  {
    "usedBytes":619,
    "quotaBytes":16777216
  }
}
         
```

   
<a id="ID4EWAAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EYAAC"></a>

 
##### <a name="parent"></a>父系 

[/sessions/{sessionId}/scids/{scid}](uri-sessionssessionidscidsscid.md)

  
<a id="ID4ECBAC"></a>

 
##### <a name="reference"></a>參考 

[quotaInfo (JSON)](../../json/json-quota.md)

   