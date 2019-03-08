---
title: POST (/users/me/scids/{scid}/clips)
assetID: 44535926-9fb8-5498-b1c8-514c0763e6c9
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipspost.html
description: " POST (/users/me/scids/{scid}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7a8973390ccbf5dd9980410f60f03a7edd78c134
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608783"
---
# <a name="post-usersmescidsscidclips"></a>POST (/users/me/scids/{scid}/clips)
進行初始上傳要求。 這些 uri 的網域`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取決於有問題之 URI 的函式。
 
  * [註解](#ID4EX)
  * [URI 參數](#ID4EFB)
  * [Authorization](#ID4EQB)
  * [必要的要求標頭](#ID4EKC)
  * [選擇性的要求標頭](#ID4ENE)
  * [要求本文](#ID4ENF)
  * [範例要求](#ID4E1F)
  * [HTTP 狀態碼](#ID4EDG)
  * [回應主體](#ID4EVAAC)
  * [範例回應](#ID4EFBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>備註
 
這是遊戲剪輯上傳程序的第一個部分。 在擷取的影片，建議您呼叫 GameClips 服務立即取得的識別碼和 URI 的位元為單位的上傳，即使沒有排定立即開始上傳。 此呼叫將會執行使用者配額檢查和其他透過內容隔離，隱私權，等等，請參閱是否影片應即使排程上傳用戶端的檢查。 從這個呼叫的正面回應會指出服務是願意接受上傳的視訊剪輯。 上傳的所有項目必須與特定標題 （透過 SCID) 相關聯接受系統中。
 
這個呼叫不是等冪;後續呼叫會導致不同的識別碼和 Uri 發出。 失敗的重試次數應該遵循標準用戶端輪詢行為。
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| scid| 字串| 服務組態正在存取的資源識別碼。 必須符合 SCID 的已驗證的使用者。| 
  
<a id="ID4EQB"></a>

 
## <a name="authorization"></a>Authorization
 
這個方法需要下列宣告︰
 
   * xuid
   * 裝置類型-必須是上傳的裝置
   * DeviceId
   * TitleId
   * TitleSandboxId
   
<a id="ID4EKC"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：<b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例：1，vnext。| 
| Content-Type| 字串| 回應主體的 MIME 類型。 範例： <b>application/json</b>。| 
| Accept| 字串| 可接受的內容類型的值。 範例： <b>application/json</b>。| 
  
<a id="ID4ENE"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字串| 可接受的壓縮編碼。 範例值： gzip、 deflate，身分識別。| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>要求本文
 
要求的主體應該[InitialUploadRequest](../../json/json-initialuploadrequest.md)以 JSON 格式的物件。
  
<a id="ID4E1F"></a>

 
## <a name="sample-request"></a>範例要求
 

```cpp
{
   "clipNameStringId": "1193045",
   "userCaption": "OMG Look at this!",
   "sessionRef": "4587552a-a5ad-4c4c-a787-5bc5af70e4c9",
   "dateRecorded": "2012-12-23T11:08:08Z",
   "durationInSeconds": 27,
   "expectedBlocks": 7,
   "fileSize": 1234567,
   "type": "MagicMoment, Achievement",
   "source": "Console",
   "visibility": "Default",
   "titleData": "{ 'Boss': 'The Invincible' }",
   "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
   "thumbnailSource": "Offset",
   "thumbnailOffsetMillseconds": 20000,
   "savedByUser": false
 }
      
```

  
<a id="ID4EDG"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 確定| 已成功擷取工作階段。| 
| 400| 錯誤的要求| 在要求主體中，發生錯誤，或使用者已超過其配額。| 
| 401| 未經授權| 還有一個問題與要求中的驗證語彙基元格式。| 
| 403| 已禁止| 某些必要的宣告會遺失，或裝置類型不是。| 
| 503| 無法接受| 此服務，或是某些下游的相依性都已關閉。 使用標準的輪詢行為重試。| 
  
<a id="ID4EVAAC"></a>

 
## <a name="response-body"></a>回應主體
 
回應可進行[InitialUploadResponse](../../json/json-initialuploadresponse.md)物件或[ServiceErrorResponse](../../json/json-serviceerrorresponse.md)以 JSON 格式的物件。
  
<a id="ID4EFBAC"></a>

 
## <a name="sample-response"></a>範例回應
 

```cpp
{
   "clipName": "ClipName",
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752",  
   "titleName": "TitleName",
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
         
```

  
<a id="ID4EOBAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EQBAC"></a>

 
##### <a name="parent"></a>父系 

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

   