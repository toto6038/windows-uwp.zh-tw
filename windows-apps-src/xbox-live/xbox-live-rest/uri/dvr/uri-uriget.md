---
title: GET (/{uri})
assetID: a67a3288-88f9-c504-5fa8-8fd06055d079
permalink: en-us/docs/xboxlive/rest/uri-uriget.html
description: " GET (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 757d84c9ad5a005e042b42d699ada08504dc57ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650613"
---
# <a name="get-uri"></a>GET (/{uri})
下載遊戲的剪輯。 這些 uri 的網域`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取決於有問題之 URI 的函式。
 
  * [註解](#ID4EX)
  * [URI 參數](#ID4EDB)
  * [必要的要求標頭](#ID4EEC)
  * [選擇性的要求標頭](#ID4EQE)
  * [要求本文](#ID4EZF)
  * [必要的回應標頭](#ID4EEG)
  * [HTTP 狀態碼](#ID4EYAAC)
  * [選擇性的回應標頭](#ID4EOFAC)
  * [回應主體](#ID4EOGAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>備註
 
用戶端可以下載任何剪輯或已達到已發行的狀態，且可下載的型別，如中所指定的縮圖**GameClipUri**物件。 擷取一份使用者或公用的剪輯時，會要求檔案的 URI 包含在回應主體中。
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| <b>uri</b>| 字串| <b>Uri</b>欄位內<b>GameClipUri</b>物件。| 
  
<a id="ID4EEC"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：<b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例：1，vnext。| 
| Content-Type| 字串| 回應主體的 MIME 類型。 範例： <b>application/json</b>。| 
| Accept| 字串| 可接受的內容類型的值。 範例： <b>application/json</b>。| 
| Cache-Control| 字串| 正常的要求，來指定快取行為。| 
  
<a id="ID4EQE"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字串| 可接受的壓縮編碼。 範例值： gzip、 deflate，身分識別。| 
| ETag| 字串| 用於快取最佳化。 範例值：「 686897696a7c876b7e"。| 
  
<a id="ID4EZF"></a>

 
## <a name="request-body"></a>要求本文
 
此要求主體中不傳送任何物件。
  
<a id="ID4EEG"></a>

 
## <a name="required-response-headers"></a>必要的回應標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例：1，vnext。| 
| Content-Type| 字串| 回應主體的 MIME 類型。 範例： <b>application/json</b>。| 
| Cache-Control| 字串| 正常的要求，來指定快取行為。| 
| Accept| 字串| 可接受的內容類型的值。 範例： <b>application/json</b>。| 
| 稍後再試| 字串| 指示用戶端在無法使用伺服器的情況下請稍後再試。| 
| 而有所不同| 字串| 指示下游 proxy 如何快取的回應。| 
  
<a id="ID4EYAAC"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EOFAC"></a>

 
## <a name="optional-response-headers"></a>選擇性的回應標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| 字串| 用於快取最佳化。 範例：「 686897696a7c876b7e"。| 
  
<a id="ID4EOGAC"></a>

 
## <a name="response-body"></a>回應主體
 
<a id="ID4EUGAC"></a>

  
 
如果成功，伺服器將傳回的視訊剪輯，可能以截斷方式根據範圍要求標頭。 截斷的剪輯，回應會部分內容 (206)。 如果伺服器傳回整個檔案，它會回應 OK (200)。 發生錯誤時， **GameClipsServiceErrorResponse**可能傳回物件，以及適當的 HTTP 狀態碼 (例如 416，要求範圍 」)。
   
<a id="ID4E4GAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E6GAC"></a>

 
##### <a name="parent"></a>父系 

[/{uri}](uri-uri.md)

   