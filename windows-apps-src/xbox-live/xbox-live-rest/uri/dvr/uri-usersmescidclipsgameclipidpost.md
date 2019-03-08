---
title: POST (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 410aecad-57f9-c3dc-f35f-19c4d8dfb704
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipidpost.html
description: " POST (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 89f3b53631f5570ab6d0d0619f6678fc3e3c2dd2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645823"
---
# <a name="post-usersmescidsscidclipsgameclipid"></a>POST (/users/me/scids/{scid}/clips/{gameClipId})
更新遊戲剪輯使用者專屬資料的中繼資料。 這些 uri 的網域`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取決於有問題之 URI 的函式。
 
  * [註解](#ID4EX)
  * [URI 參數](#ID4EAB)
  * [必要的要求標頭](#ID4ELB)
  * [選擇性的要求標頭](#ID4EXD)
  * [要求本文](#ID4EAF)
  * [必要的回應標頭](#ID4EVF)
  * [選擇性的回應標頭](#ID4EJAAC)
  * [回應主體](#ID4EJBAC)
  * [相關的 Uri](#ID4EWBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>備註
 
用於更新遊戲剪輯中繼資料 API 分為兩個類別，更新其遊戲剪輯的協助工具和標題，例如中繼資料和更新的公用屬性 （例如套用評等或遞增檢視計數） 的任何其他遊戲的剪輯。 如果 URI 中的 XUID 不符合宣告中的 XUID，只將公用資料時，則可以進行編輯，若要編輯的任何其他資料的任何要求將會遭到拒絕。 在案例中多個欄位正嘗試要編輯和其中一個無效的要求，整個要求將會失敗。
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| scid| 字串| 服務組態正在存取的資源識別碼。 必須符合 SCID 的已驗證的使用者。| 
| gameClipId| 字串| 遊戲剪輯所存取之資源的識別碼。| 
  
<a id="ID4ELB"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：<b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例：1，vnext。| 
| Content-Type| 字串| 回應主體的 MIME 類型。 範例： <b>application/json</b>。| 
| Accept| 字串| 可接受的內容類型的值。 範例： <b>application/json</b>。| 
| Cache-Control| 字串| 正常的要求，來指定快取行為。| 
  
<a id="ID4EXD"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字串| 可接受的壓縮編碼。 範例值： gzip、 deflate，身分識別。| 
| ETag| 字串| 用於快取最佳化。 範例值：「 686897696a7c876b7e"。| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>要求本文
 
要求的主體應該[UpdateMetadataRequest](../../json/json-updatemetadatarequest.md)以 JSON 格式的物件。 範例：
 
變更使用者剪輯名稱和可見性：
 

```cpp
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
變更只會標題 （這只是範例，由於此欄位的結構描述是由呼叫者） 的屬性：
 

```cpp
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EVF"></a>

 
## <a name="required-response-headers"></a>必要的回應標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例：1，vnext。| 
| Content-Type| 字串| 回應主體的 MIME 類型。 範例： <b>application/json</b>。| 
| Cache-Control| 字串| 正常的要求，來指定快取行為。| 
| Accept| 字串| 可接受的內容類型的值。 範例： <b>application/json</b>。| 
| 稍後再試| 字串| 指示用戶端在無法使用伺服器的情況下請稍後再試。| 
| 而有所不同| 字串| 指示下游 proxy 如何快取的回應。| 
  
<a id="ID4EJAAC"></a>

 
## <a name="optional-response-headers"></a>選擇性的回應標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| 字串| 用於快取最佳化。 範例：「 686897696a7c876b7e"。| 
  
<a id="ID4EJBAC"></a>

 
## <a name="response-body"></a>回應主體
 
成功的 HTTP 狀態碼 200 的中繼資料的更新時就會傳回。
 
否則會傳回 JSON 格式的 ServiceErrorResponse 物件具有適當的 HTTP 狀態碼。
  
<a id="ID4EWBAC"></a>

 
## <a name="related-uris"></a>相關的 Uri
 
下列 Uri 來更新中繼資料中的公用欄位。 沒有任何所需的這些要求的本文。 成功的 HTTP 狀態碼 200 的中繼資料的更新時就會傳回。 否則會傳回 JSON 格式的 ServiceErrorResponse 物件具有適當的 HTTP 狀態碼。
 
   * **POST /users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} /ratings/ {評等值}** -適用於指定的裁剪指定評等。 分級值，應該是介於 1 到 5 之間的整數。
   * **張貼 /users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} / 旗標**-加上旗標包含潛在可疑的內容，以強制執行檢查的剪輯。
   * **POST /users/ {ownerId} /scids/ {scid} /clips/ {gameClipId} / 檢視**-遞增指定遊戲的剪輯檢視計數。 建議您，這會呼叫不正確時播放會啟動，但 75%-80%的播放完成時。
   
<a id="ID4EMCAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EOCAC"></a>

 
##### <a name="parent"></a>父系 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   