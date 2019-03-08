---
title: GET (/sessions/{sessionId}/scids/{scid}/data/{path})
assetID: 007821b8-16f0-2fe1-5196-890743d77775
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidssciddatapath-get.html
description: " GET (/sessions/{sessionId}/scids/{scid}/data/{path})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 28a279347fa5a463c0d482a624af831c0cdb0fba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623713"
---
# <a name="get-sessionssessionidscidssciddatapath"></a>GET (/sessions/{sessionId}/scids/{scid}/data/{path})
列出在指定路徑的檔案資訊。 這些 Uri 的網域是`titlestorage.xboxlive.com`。
 
  * [URI 參數](#ID4EX)
  * [選擇性的查詢字串參數](#ID4ECB)
  * [Authorization](#ID4EWC)
  * [必要的要求標頭](#ID4EDD)
  * [要求本文](#ID4EME)
  * [HTTP 狀態碼](#ID4EZE)
  * [回應主體](#ID4EUBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| sessionId| 字串| 工作階段的識別碼來查閱。| 
| scid| guid| 要查閱的服務組態的識別碼。| 
| path| 字串| 要傳回資料項目的路徑。 傳回所有相符的目錄和子目錄。 有效字元包括大寫字母 (A-Z)、 小寫字母 (a-z)、 數字 (0-9)、 底線 (_) 和正斜線 （/）。 可能是空的。 最大長度 256。| 
  
<a id="ID4ECB"></a>

 
## <a name="optional-query-string-parameters"></a>選擇性的查詢字串參數 
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| 整數| 傳回在集合中，例如，N + 1 開始的項目，請略過 N 個項目。| 
| continuationToken| 字串| 傳回項目，始於指定的接續 token。 如果同時提供兩者，continuationToken 參數的優先順序高於 skipItems。 換句話說，如果 continuationToken 參數存在，會忽略 skipItems 參數。| 
| maxItems| 整數| 若要從集合中，可以結合 skipItems 和 continuationToken 要傳回的項目範圍中傳回的項目數目上限。 服務可能會提供預設值如果 maxItems 不存在，而且可能會傳回少於 maxItems，即使尚未傳回結果的最後一頁。 | 
  
<a id="ID4EWC"></a>

 
## <a name="authorization"></a>Authorization 
 
要求必須包含有效的 Xbox LIVE 授權標頭。 如果呼叫端不允許存取此資源中，服務會傳回 403 禁止 回應。 如果標頭無效或遺失，則服務會傳回 401 未授權的回應。 
  
<a id="ID4EDD"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API 合約版本。| 
| Authorization| XBL3.0 x=[hash];[token]| STS 驗證語彙基元。 驗證要求所傳回的語彙基元取代 STSTokenString。 如需關於擷取之 STS 權杖，以及建立授權標頭的詳細資訊，請參閱驗證和授權的 Xbox LIVE 服務要求。| 
  
<a id="ID4EME"></a>

 
## <a name="request-body"></a>要求本文 
 
此要求主體中不傳送任何物件。
  
<a id="ID4EZE"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EUBAC"></a>

 
## <a name="response-body"></a>回應主體
 
如果呼叫成功，則服務會傳回的陣列[TitleBlob](../../json/json-titleblob.md)物件。 
 
<a id="ID4ECCAC"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
"blobs":
[
    {
        "fileName":"foo\bar\blob.txt,binary",
        "clientFileTime":"2012-01-01T01:02:03.1234567Z",
        "displayName":"Friendly Name",
        "size":12,
        "etag":"0x8CEB3E4F8F3A5BF"
    },
    {
        "fileName":"foo\bar\blob2.txt,binary",
        "displayName":"Blob 2",
        "size":4,
        "etag":"0x8CEB3FE57F1A142"
    },
    {
        "fileName":"foo\jsonblob.txt,json",
        "size":15,
        "etag":"0x8CEB40152B4A6F8"
    }
],
"pagingInfo":
    {
        "continuationToken":"54",
    }
}
         
```

   
<a id="ID4EOCAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EQCAC"></a>

 
##### <a name="parent"></a>父系  

[/sessions/{sessionId}/scids/{scid}/data/{path}](uri-sessionssessionidscidssciddatapath.md)

  
<a id="ID4E3CAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>參考[TitleBlob (JSON)](../../json/json-titleblob.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

   