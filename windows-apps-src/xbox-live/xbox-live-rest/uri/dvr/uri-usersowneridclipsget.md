---
title: GET (/users/{ownerId}/clips)
assetID: da972b4e-bc38-66f5-2222-5e79d7c8a183
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclipsget.html
description: " GET (/users/{ownerId}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7c52daf4a07914c34f1aadc84a7238771669d65f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623203"
---
# <a name="get-usersowneridclips"></a>GET (/users/{ownerId}/clips)
擷取使用者的剪輯清單。
這些 uri 的網域`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取決於有問題之 URI 的函式。

  * [註解](#ID4EX)
  * [URI 參數](#ID4EEB)
  * [查詢字串參數](#ID4EPB)
  * [要求本文](#ID4EPE)
  * [必要的回應標頭](#ID4E1E)
  * [選擇性的回應標頭](#ID4ENH)
  * [回應主體](#ID4EOAAC)
  * [相關的 Uri](#ID4EABAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>備註

此 API 可讓使用者自己的剪輯，以及其他使用者的多個剪輯會儲存在服務清單的各種方法。 數個進入點傳回不同層級的資料，並允許透過查詢參數進行篩選。 如果宣告中的 XUID 符合指定之 URI 中的擁有者，則會傳回使用者自己的剪輯之後會檢查內容隔離。 如果 URI 中的擁有者不符合宣告 XUID，然後指定的使用者的項目會根據傳回隱私權檢查和針對要求 XUID 的內容隔離檢查。

查詢會最佳化每位使用者每個服務組態識別碼 (scid)。 指定進一步篩選或預設值以外的排序順序為下列指定可能在某些情況下需要較長的時間傳回。 這是對於較大集的每位使用者的影片更明顯。

沒有任何批次 API 來取得相同的 API 呼叫中的多個使用者的清單。 接著查詢每個使用者為建議的模式 （目前） 從 SLS 架構設計人員。

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| ownerId| 字串| 正在存取的資源之使用者的使用者身分識別。 支援的格式: 「 我 」 或 「 xuid(123456789)"。 最大長度：16.|

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>查詢字串參數

| 參數| 類型| 描述|
| --- | --- | --- | --- | --- | --- |
| skipItems| 32 位元帶正負號的整數| 選用。 傳回集合 （亦即，略過 n 個項目） 中為 N + 1 開始的項目。|
| continuationToken| 字串| 選用。 傳回項目，始於指定的接續 token。 如果同時提供兩者，continuationToken 參數的優先順序高於 skipItems。 換句話說，如果 continuationToken 參數存在，會忽略 skipItems 參數。 大小上限：36.|
| maxItems| 32 位元帶正負號的整數| 選用。 若要從集合 （可結合 skipItems 與要傳回的項目範圍的 continuationToken） 中傳回的項目數目上限。 如果 maxItems 不存在，而且可能會傳回少於 maxItems （即使尚未傳回結果的最後一頁），服務可能會提供預設值。|
| 順序| Unicode 字元| 選用。 指定是否清單就會傳回 (D) escending （第一次最高值） 或 (A) scending （第一次最低值） 順序。 Default：D.|
| type| GameClipTypes| 選用。 以逗號分隔的剪輯，傳回的型別集。 Default：所有。|
| eventId| 字串| 選用。 以逗號分隔介於來篩選結果的集合。 Default：則為 null。|
| 辨識符號| 字串| 選用。 指定要用來取得剪輯 order 限定詞。 <ul><li>建立-指定剪輯的目的日期的順序傳回至系統</li><li>評等-[評分]-指定其分級值所傳回之剪輯</li><li>檢視表-[最常檢視]-可讓您指定多個檢視所傳回之剪輯</li></ul><br/> 大小上限：12. 預設值: 「 建立 」。| 

<a id="ID4EPE"></a>


## <a name="request-body"></a>要求本文

沒有此要求所需的成員。

<a id="ID4E1E"></a>


## <a name="required-response-headers"></a>必要的回應標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例：1，vnext。|
| Content-Type| 字串| 回應主體的 MIME 類型。 範例： <b>application/json</b>。|
| Cache-Control| 字串| 正常的要求，來指定快取行為。|
| Accept| 字串| 可接受的內容類型的值。 範例： <b>application/json</b>。|
| 稍後再試| 字串| 指示用戶端在無法使用伺服器的情況下請稍後再試。|
| 而有所不同| 字串| 指示下游 proxy 如何快取的回應。|

<a id="ID4ENH"></a>


## <a name="optional-response-headers"></a>選擇性的回應標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Etag| 字串| 用於快取最佳化。 範例：「 686897696a7c876b7e"。|

<a id="ID4EOAAC"></a>


## <a name="response-body"></a>回應主體

<a id="ID4EUAAC"></a>


### <a name="sample-response"></a>範例回應


```cpp
{
       "gameClips":
       [
           {
               "xuid": "2716903703773872",
               "clipName": "[en-US] Localized Greatest Moment Name",
               "titleName": "[en-US] Localized Title Name",
               "gameClipLocale": "en-US",
               "gameClipId": "6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
               "state": "Published",
               "dateRecorded": "2013-06-14T01:02:55.4918465Z",
               "lastModified": "2013-06-14T01:05:41.3652693Z",
               "userCaption": "Set by user!",
               "type": "DeveloperInitiated",
               "source": "Console",
               "visibility": "Public",
               "durationInSeconds": 30,
               "scid": "00000000-0000-0012-0023-000000000070",
               "titleId": 354975,
               "rating": 3.75,
               "ratingCount": 245,
               "views": 7453,
               "titleData": "",
               "systemProperties": "",
               "savedByUser": false,
               "achievementId": "AchievementId",
               "greatestMomentId": "GreatestMomentId",
               "thumbnails": [
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/large",
                       "fileSize": 637293,
                       "thumbnailType": "Large"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/small",
                       "fileSize": 163998,
                       "thumbnailType": "Small"
                   }
               ],
               "gameClipUris": [
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest",
                       "uriType": "SmoothStreaming",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest(format=m3u8-aapl)",
                       "uriType": "Ahls",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
                       "fileSize": 88820,
                       "uriType": "Download",
                       "expiration": "2999-12-31T11:59:40Z"
                   }
               ]
           }
       ],
   "pagingInfo":
       {
           "continuationToken": null,
           "totalItems": 1
       }
   }

```


<a id="ID4EABAC"></a>


## <a name="related-uris"></a>相關的 Uri

下列 URI 會與本文件中，但具有額外的 path 參數指定 SCID 主要密碼完全相同。 會傳回只針對該 SCID 該使用者的剪輯。 提出要求的使用者必須能夠存取要求 SCID，否則為 HTTP 403 將會傳回錯誤。

   * **/users/{ownerId}/scids/{scid}/clips**

<a id="ID4ENBAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EPBAC"></a>


##### <a name="parent"></a>父系

[/users/{ownerId}/clips](uri-usersowneridclips.md)
