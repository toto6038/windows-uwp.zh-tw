---
title: POST (/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type})
assetID: 0c89b845-c40f-b28e-f102-d2a96f58dcf9
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersbatchscidssciddatapathandfilenametype-post.html
description: " POST (/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 504a73534b9ee536970caec1b5fd1ea6ce731b31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650543"
---
# <a name="post-trustedplatformusersbatchscidssciddatapathandfilenametype"></a>POST (/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type})
下載多個檔案從多個使用者具有相同的檔案名稱。 這些 Uri 的網域是`titlestorage.xboxlive.com`。
 
  * [URI 參數](#ID4EX)
  * [Authorization](#ID4ECB)
  * [要求本文](#ID4EPB)
  * [HTTP 狀態碼](#ID4E3C)
  * [必要的回應標頭](#ID4EPAAC)
  * [選擇性的回應標頭](#ID4ESBAC)
  * [回應主體](#ID4E3CAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| scid| guid| 要查閱的服務組態的識別碼。| 
| pathAndFileName| 字串| 要存取之項目的路徑和檔案名稱。 有效的字元 （最多且包含在最後斜線） 的路徑部分包含大寫字母 (A-Z)、 小寫字母 (a-z)、 數字 (0-9)、 底線 (_)，並正斜線 （/）。路徑部分可能是空的。有效的字元 （在最後斜線之後的所有內容） 中的檔案名稱部分包含大寫字母 (A-Z)、 小寫字母 (a-z)、 數字 (0-9)、 底線 (_)、 句號 （.），以及連字號 （-）。 檔案名稱不能是空的、 以句號結束或包含兩個連續的句號。| 
| type| 字串| 資料的格式。 可能的值為二進位或 json。| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>Authorization 
 
要求必須包含有效的 Xbox LIVE 授權標頭。 如果呼叫端不允許存取此資源中，服務會傳回 403 禁止 回應。 如果標頭無效或遺失，則服務會傳回 401 未授權的回應。 
  
<a id="ID4EPB"></a>

 
## <a name="request-body"></a>要求本文
 
| 屬性| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| xuids| 不帶正負號的 64 位元整數的陣列| XUIDs 要下載檔案的清單。| 
 
<a id="ID4EQC"></a>

 
### <a name="sample-request"></a>範例要求
 

```cpp
{
    "xuids" : 
    [
      12345,
      45678,
      78901
    ]
}
      
```

   
<a id="ID4E3C"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼 
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 確定 | 要求成功。| 
| 201| 建立時間 | 建立實體。| 
| 400| 錯誤的要求 | 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
| 401| 未經授權 | 要求需要使用者驗證。| 
| 403| 已禁止 | 要求不允許使用者或服務。| 
| 404| 找不到 | 找不到指定的資源。| 
| 406| 無法接受 | 不支援資源版本。| 
| 408| 要求逾時 | 該要求花了太多時間完成。| 
| 500| 內部伺服器錯誤 | 伺服器發生未預期的狀況，導致無法完成要求。| 
| 503| 服務無法使用 | 要求已節流處理，以秒為單位 （例如 5 秒之後） 的用戶端重試值之後，再次嘗試要求。| 
  
<a id="ID4EPAAC"></a>

 
## <a name="required-response-headers"></a>必要的回應標頭
 
| 標頭| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 內容配置| 描述組件的內容。 標頭的"name"和"filename"組件是 XUID 這個檔案所屬的使用者。| 
| HttpStatusCode| 擷取這個特定的檔案與相關之 HTTP 狀態碼。| 
  
<a id="ID4ESBAC"></a>

 
## <a name="optional-response-headers"></a>選擇性的回應標頭
 
| 標頭| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag 是不透明的識別碼指派至資源，在 URL 中找到的特定版本為 web 伺服器。 如果在該 URL 的資源內容變更，會指派新的和不同的 ETag。| 
| Content-Type| 如果已成功擷取檔案，這會是檔案的內容類型。| 
| Content-Range| 如果檔案已成功擷取，且部分的下載，這是檔案的在回應中所包含的位元組範圍。 | 
  
<a id="ID4E3CAC"></a>

 
## <a name="response-body"></a>回應主體
 
如果呼叫成功，服務會傳回要求的檔案的內容中的多部分回應。
 
<a id="ID4EGDAC"></a>

 
### <a name="sample-response"></a>範例回應 
 

```cpp
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=c0a9fd75-d036-4294-8b7b-85fea15a31bb

228
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="123"; filename="123"
HttpStatusCode: 200
ETag: 0x8CF327717411C31
Content-Type: application/octet-stream

asdf123
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="456"; filename="456"
HttpStatusCode: 200
ETag: 0x8CF32771E954BB8
Content-Type: application/octet-stream

asdf456
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="789"; filename="789"
HttpStatusCode: 404


--c0a9fd75-d036-4294-8b7b-85fea15a31bb--

0

```

   
<a id="ID4EUDAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EWDAC"></a>

 
##### <a name="parent"></a>父系 

[/trustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersbatchscidssciddatapathandfilenametype.md)

   