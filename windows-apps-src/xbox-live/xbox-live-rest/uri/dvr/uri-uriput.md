---
title: PUT (/{uri})
assetID: 24a24c93-f43b-017e-91e0-29e190fec8a8
permalink: en-us/docs/xboxlive/rest/uri-uriput.html
description: " PUT (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 862b956e29222bb9d28f98510f13d42fd1a51b6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633113"
---
# <a name="put-uri"></a>PUT (/{uri})
遊戲的剪輯資料上傳。
這些 uri 的網域`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取決於有問題之 URI 的函式。

  * [註解](#ID4EX)
  * [URI 參數](#ID4EQB)
  * [查詢字串參數](#ID4ERC)
  * [必要的要求標頭](#ID4EBE)
  * [選擇性的要求標頭](#ID4ENG)
  * [要求本文](#ID4EWH)
  * [HTTP 狀態碼](#ID4ECAAC)
  * [必要的回應標頭](#ID4EYEAC)
  * [選擇性的回應標頭](#ID4ELHAC)
  * [回應主體](#ID4ELIAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>備註

在後**InitialUploadResponse**傳回上, 傳會透過執行**uploadUri**傳回該物件中。 用戶端應該分割成檔案**expectedBlocks**序列區塊，不能大於 2 MB。 可以使用平行上傳。

如果您要上傳區塊中的檔案，伺服器會傳回 HTTP 狀態碼已接受 (202 的) 每個要求，直到它收到所有預期的區塊，在此情況下，它會為一個檔案，並傳回已建立 (201) 認可所有區塊。 在這些情況下，回應不包含物件，而且伺服器可能會排程其他處理。 發生錯誤時， **ServiceErrorResponse**可能傳回物件，以及適當的 HTTP 狀態碼。

可復原的錯誤程式碼中，用戶端應該重試使用標準的退避法重試機制。

> [!NOTE] 
> 即使已成功完成上傳，做進一步處理，可以的拒絕原因的剪輯無關的上傳或中繼資料補充程序會發生。


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| <b>uri</b>| 字串| <b>UploadUri</b>欄位內<b>InitialUploadResponse</b>物件。|

<a id="ID4ERC"></a>


## <a name="query-string-parameters"></a>查詢字串參數

| 參數| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- |
| <b>blockNum</b>| 32 位元不帶正負號的整數| 需要<b>expectedBlocks</b>設定。 判斷順序中的檔案區塊的零為基礎的區塊數目。 例如，如果<b>expectedBlocks</b>為 7，然後<b>blockNum</b>可以從 0 到 6。 |
| <b>uploadId</b>| 字串| 必要。 不透明的識別碼，在<b>GameClipsServiceUploadResponse</b>物件。|

<a id="ID4EBE"></a>


## <a name="required-request-headers"></a>必要的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：<b>Xauth=&lt;authtoken></b>|
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例：1，vnext。|
| Content-Type| 字串| 回應主體的 MIME 類型。 範例： <b>application/json</b>。|
| Accept| 字串| 可接受的內容類型的值。 範例： <b>application/json</b>。|
| Cache-Control| 字串| 正常的要求，來指定快取行為。|

<a id="ID4ENG"></a>


## <a name="optional-request-headers"></a>選擇性的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Accept-Encoding| 字串| 可接受的壓縮編碼。 範例值： gzip、 deflate，身分識別。|
| ETag| 字串| 用於快取最佳化。 範例值：「 686897696a7c876b7e"。|

<a id="ID4EWH"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4ECAAC"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。

| 程式碼| 原因片語| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 確定| 已成功擷取工作階段。|
| 301| 已永久移動| 服務已移動至不同的 URI。|
| 307| 暫時重新導向| 服務已移動至不同的 URI。|
| 400| 錯誤的要求| 服務無法辨識格式不正確的要求。 通常是無效的參數。|
| 401| 未經授權| 要求需要使用者驗證。|
| 403| 已禁止| 要求不允許使用者或服務。|
| 404| 找不到| 找不到指定的資源。|
| 406| 無法接受| 不支援資源版本。|
| 408| 要求逾時| 該要求花了太多時間完成。|
| 410| 不存在| 無法再使用所要求的資源。|

<a id="ID4EYEAC"></a>


## <a name="required-response-headers"></a>必要的回應標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例：1，vnext。|
| Content-Type| 字串| 回應主體的 MIME 類型。 範例： <b>application/json</b>。|
| Cache-Control| 字串| 正常的要求，來指定快取行為。|
| Accept| 字串| 可接受的內容類型的值。 範例： <b>application/json</b>。|
| 稍後再試| 字串| 指示用戶端在無法使用伺服器的情況下請稍後再試。|
| 而有所不同| 字串| 指示下游 proxy 如何快取的回應。|

<a id="ID4ELHAC"></a>


## <a name="optional-response-headers"></a>選擇性的回應標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Etag| 字串| 用於快取最佳化。 範例：「 686897696a7c876b7e"。|

<a id="ID4ELIAC"></a>


## <a name="response-body"></a>回應主體

回應主體會不傳送任何物件。

<a id="ID4EWIAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EYIAC"></a>


##### <a name="parent"></a>父系

[/{uri}](uri-uri.md)
