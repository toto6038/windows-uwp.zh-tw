---
title: DELETE (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 486fac60-6884-2e3f-9ef8-8de5da0ad8af
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipiddelete.html
description: " DELETE (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 391e4d79a389c358dea83509b52782d086201ffc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622833"
---
# <a name="delete-usersmescidsscidclipsgameclipid"></a>DELETE (/users/me/scids/{scid}/clips/{gameClipId})
刪除的網域，這些 uri 的遊戲剪輯`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取決於有問題之 URI 的函式。
 
  * [註解](#ID4EX)
  * [URI 參數](#ID4ECB)
  * [Authorization](#ID4ENB)
  * [必要的要求標頭](#ID4EYB)
  * [選擇性的要求標頭](#ID4EEE)
  * [要求本文](#ID4ENF)
  * [HTTP 狀態碼](#ID4EYF)
  * [必要的回應標頭](#ID4EIAAC)
  * [選擇性的回應標頭](#ID4E2CAC)
  * [回應主體](#ID4E2DAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>備註
 
提供用來刪除使用者自己的視訊從 GameClips 服務的機制。 在刪除時所有的中繼資料和實際的視訊資產 （產生與原始） 會從系統移除。 這是永久性的作業。 

> [!NOTE] 
> 指定的擁有者識別碼必須符合才能成功刪除要求的授權權杖中的呼叫端。 


  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | 
| scid| 字串| 服務組態正在存取的資源識別碼。 必須符合 SCID 的已驗證的使用者。| 
| gameClipId| 字串| 遊戲剪輯所存取之資源的識別碼。| 
  
<a id="ID4ENB"></a>

 
## <a name="authorization"></a>Authorization
 
這個方法需要 Xuid 宣告。
  
<a id="ID4EYB"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：<b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例：1，vnext。| 
| Content-Type| 字串| 回應主體的 MIME 類型。 範例： <b>application/json</b>。| 
| Accept| 字串| 可接受的內容類型的值。 範例： <b>application/json</b>。| 
| Cache-Control| 字串| 正常的要求，來指定快取行為。| 
  
<a id="ID4EEE"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字串| 可接受的壓縮編碼。 範例值： gzip、 deflate，身分識別。| 
| ETag| 字串| 用於快取最佳化。 範例值：「 686897696a7c876b7e"。| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>要求本文
 
此要求主體中不傳送任何物件。
  
<a id="ID4EYF"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| 確定| 成功刪除剪輯。| 
| 401| 未經授權| 還有一個問題與要求中的驗證語彙基元格式。| 
| 403| 已禁止| 遺失某些必要的宣告。| 
| 404| 找不到| URL 中指定的剪輯沒有 （或已刪除第二次）。| 
| 503| 無法接受| 此服務，或是某些下游的相依性都已關閉。 使用標準的輪詢行為重試。| 
  
<a id="ID4EIAAC"></a>

 
## <a name="required-response-headers"></a>必要的回應標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例：1，vnext。| 
| Content-Type| 字串| 回應主體的 MIME 類型。 範例： <b>application/json</b>。| 
| Cache-Control| 字串| 正常的要求，來指定快取行為。| 
| Accept| 字串| 可接受的內容類型的值。 範例： <b>application/json</b>。| 
| 稍後再試| 字串| 指示用戶端在無法使用伺服器的情況下請稍後再試。| 
| 而有所不同| 字串| 指示下游 proxy 如何快取的回應。| 
  
<a id="ID4E2CAC"></a>

 
## <a name="optional-response-headers"></a>選擇性的回應標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| 字串| 用於快取最佳化。 範例：「 686897696a7c876b7e"。| 
  
<a id="ID4E2DAC"></a>

 
## <a name="response-body"></a>回應主體
 
服務會與 HTTP 狀態碼 204 （沒有內容），在成功的回應。 嘗試刪除相同的物件，或不存在的物件會傳回 404。
 
發生錯誤， **ServiceErrorResponse**將傳回的物件。
  
<a id="ID4EJEAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ELEAC"></a>

 
##### <a name="parent"></a>父系 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   