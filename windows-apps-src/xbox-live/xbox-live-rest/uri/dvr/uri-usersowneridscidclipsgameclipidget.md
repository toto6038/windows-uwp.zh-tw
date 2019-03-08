---
title: GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
assetID: dbd60c93-9d8e-609b-0ae3-b3f7ee26ba2d
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipidget.html
description: " GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5d2858b11bf8fb902ea07a016c8f41db375013f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640953"
---
# <a name="get-usersowneridscidsscidclipsgameclipid"></a>GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
如果已知找出它的所有識別碼，請從系統取得單一的遊戲多媒體項目。 這些 uri 的網域`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取決於有問題之 URI 的函式。
 
  * [註解](#ID4EX)
  * [URI 參數](#ID4EVB)
  * [Authorization](#ID4EAC)
  * [必要的要求標頭](#ID4EUH)
  * [選擇性的要求標頭](#ID4EBCAC)
  * [要求本文](#ID4ETDAC)
  * [HTTP 狀態碼](#ID4E5DAC)
  * [必要的回應標頭](#ID4EQIAC)
  * [選擇性的回應標頭](#ID4EJLAC)
  * [回應主體](#ID4EJMAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>備註
 
若要裁剪可用於查詢的所有資料**遊戲剪輯**物件傳回的任何中繼資料的查詢。 **XUID**， **ServiceConfigId**， **GameClipId**並**SandboxId**要求的宣告中需要能夠透過此 API 的單一遊戲多媒體項目。 如果遊戲的剪輯已標示為強制，或內容隔離或隱私權，會檢查 「 判斷使用者沒有權限，以取得特定的遊戲剪輯，API 會傳回 HTTP 狀態碼 404 （找不到）。
 
**SandboxId**現在從 XToken 中的宣告擷取並強制執行。 如果**SandboxId**不存在，則娛樂探索服務 (EDS) 將會擲回 400 不正確的要求時發生錯誤。
 
此 API 必須也可用來重新整理過期的 Uri。 查詢完成時，任何已過期的 Uri，遊戲的剪輯都會重新整理據此。 

> [!NOTE] 
> URI 的重新整理可以最多需要 30 到 40 秒完成這項要求。 如果 URI 已過期，則嘗試立即使用的資料流作業會取得從 IIS Smooth Streaming 伺服器的 HTTP 500 狀態碼。 我們正努力來縮短，方式和這個附註將會據以作為該工作進度更新。 


  
<a id="ID4EVB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | 
| ownerId| 字串| 正在存取的資源之使用者的使用者身分識別。 支援的格式: 「 我 」 或 「 xuid(123456789)"。 最大長度：16.| 
| scid| 字串| 服務組態正在存取的資源識別碼。 必須符合 SCID 的已驗證的使用者。| 
| gameClipId| 字串| 遊戲剪輯所存取之資源的識別碼。| 
  
<a id="ID4EAC"></a>

 
## <a name="authorization"></a>Authorization
 
使用授權宣告 | 宣告| 類型| 必要？| 範例值| 備註| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| xuid| 64 位元帶正負號的整數| 是| 1234567890|  | 
| TitleId| 64 位元帶正負號的整數| 是| 1234567890| 用於<b>內容隔離</b>檢查。| 
| SandboxId| 十六進位的二進位檔| 是|  | 會指示系統進行查閱，正確的區域，用於<b>內容隔離</b>檢查。| 
  
在資源上的隱私權設定的效果 | 提出要求的使用者| 目標使用者的隱私權設定| 行為| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 我| -| 如所述。| 
| friend| 每個人| 禁止。| 
| friend| 只有朋友| 禁止。| 
| friend| 封鎖| 禁止。| 
| 非 friend 使用者| 每個人| 禁止。| 
| 非 friend 使用者| 只有朋友| 禁止。| 
| 非 friend 使用者| 封鎖| 禁止。| 
| 協力廠商網站| 每個人| 禁止。| 
| 協力廠商網站| 只有朋友| 禁止。| 
| 協力廠商網站| 封鎖| 禁止。| 
 
<a id="ID4EUH"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：<b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例：1，vnext。| 
| Content-Type| 字串| 回應主體的 MIME 類型。 範例： <b>application/json</b>。| 
| Accept| 字串| 可接受的內容類型的值。 範例： <b>application/json</b>。| 
| Cache-Control| 字串| 正常的要求，來指定快取行為。| 
  
<a id="ID4EBCAC"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字串| 可接受的壓縮編碼。 範例值： gzip、 deflate，身分識別。| 
| ETag| 字串| 用於快取最佳化。 範例值：「 686897696a7c876b7e"。| 
| 範圍| 字串|  | 
  
<a id="ID4ETDAC"></a>

 
## <a name="request-body"></a>要求本文
 
此要求主體中不傳送任何物件。
  
<a id="ID4E5DAC"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 確定| 已成功擷取工作階段。| 
| 301| 已永久移動|  | 
| 307| 暫時重新導向|  | 
| 400| 錯誤的要求| 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
| 401| 未經授權| 要求需要使用者驗證。| 
| 403| 已禁止| 要求不允許使用者或服務。| 
| 404| 找不到| 找不到指定的資源。| 
| 406| 無法接受| 不支援資源版本。| 
| 408| 要求逾時| 該要求花了太多時間完成。| 
| 410| 不存在| 無法再使用所要求的資源。| 
  
<a id="ID4EQIAC"></a>

 
## <a name="required-response-headers"></a>必要的回應標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例：1，vnext。| 
| Content-Type| 字串| 回應主體的 MIME 類型。 範例： <b>application/json</b>。| 
| Accept| 字串| 可接受的內容類型的值。 範例： <b>application/json</b>。| 
| Cache-Control| 字串| 正常的要求，來指定快取行為。| 
| 稍後再試| 字串| 指示用戶端在無法使用伺服器的情況下請稍後再試。 範例： <b>application/json</b>。| 
| 而有所不同| 字串| 指示下游 proxy 如何快取的回應。 範例： <b>application/json</b>。| 
  
<a id="ID4EJLAC"></a>

 
## <a name="optional-response-headers"></a>選擇性的回應標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| 字串| 用於快取最佳化。 範例值：「 686897696a7c876b7e"。| 
  
<a id="ID4EJMAC"></a>

 
## <a name="response-body"></a>回應主體
 
<a id="ID4EPMAC"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
 "gameClip": {
   "xuid": "2716903703773872",
   "clipName": "1234567890",
   "titleName": "",
   "gameClipId": "cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
   "state": "Published",
   "dateRecorded": "2013-05-08T21:32:17.4201279Z",
   "lastModified": "2013-05-08T21:34:48.8117829Z",
   "userCaption": "",
   "type": "DeveloperInitiated",
   "source": "Console",
   "visibility": "Public",
   "durationInSeconds": 30,
   "scid": "00000000-0000-0012-0023-000000000070",
   "titleId": 0,
   "rating": 0,
   "ratingCount": 0,
   "views": 0,
   "titleData": "",
   "systemProperties": "",
   "savedByUser": false,
   "thumbnails": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/large",
       "fileSize": 0,
       "thumbnailType": "Large"
     },
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/small",
       "fileSize": 0,
       "thumbnailType": "Small"
     }
   ],
   "gameClipUris": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
       "fileSize": 9332015,
       "uriType": "Download",
       "expiration": "9999-12-31T23:59:59.9999999"
     }
   ]
 }
}
         
```

   
<a id="ID4EZMAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E2MAC"></a>

 
##### <a name="parent"></a>父系 

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

  
<a id="ID4EFNAC"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[Marketplace Uri](../marketplace/atoc-reference-marketplace.md)

 [其他參考](../../additional/atoc-xboxlivews-reference-additional.md)

   