---
title: PUT (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
assetID: b40a1a88-02c2-624f-de00-49664825bde3
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-put.html
description: " PUT (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ad6f60c53b1654ec85c4f78d39c1c2605c55bc88
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655393"
---
# <a name="put-untrustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>PUT (/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
將檔案上傳。 資料可以上傳所在的資料和中繼資料會傳送單一訊息，或為多區塊上傳的資料和中繼資料一系列較小的區塊中會傳送完整上傳。 小於 4 mb 的檔案可以當作單一訊息傳送。 多個區塊上傳不支援的型別 json 資料。 這些 Uri 的網域是`titlestorage.xboxlive.com`。
 
  * [URI 參數](#ID4EX)
  * [Authorization](#ID4EEB)
  * [選擇性的查詢字串參數](#ID4ERB)
  * [必要的要求標頭](#ID4EOE)
  * [選擇性的要求標頭](#ID4EXF)
  * [要求本文](#ID4E1G)
  * [HTTP 狀態碼](#ID4EFH)
  * [回應主體](#ID4EYEAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 參數 
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 不帶正負號的 64 位元整數| Xbox 使用者識別碼 (XUID) 播放程式的人員提出要求。| 
| scid| guid| 要查閱的服務組態的識別碼。| 
| pathAndFileName| 字串| 要存取之項目的路徑和檔案名稱。 有效的字元 （最多且包含在最後斜線） 的路徑部分包含大寫字母 (A-Z)、 小寫字母 (a-z)、 數字 (0-9)、 底線 (_)，並正斜線 （/）。路徑部分可能是空的。有效的字元 （在最後斜線之後的所有內容） 中的檔案名稱部分包含大寫字母 (A-Z)、 小寫字母 (a-z)、 數字 (0-9)、 底線 (_)、 句號 （.），以及連字號 （-）。 檔案名稱不能是空的、 以句號結束或包含兩個連續的句號。| 
| type| 字串| 資料的格式。 可能的值為二進位或 json。| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>Authorization 
 
要求必須包含有效的 Xbox LIVE 授權標頭。 如果呼叫端不允許存取此資源中，服務會傳回 403 禁止 回應。 如果標頭無效或遺失，則服務會傳回 401 未授權的回應。 
  
<a id="ID4ERB"></a>

 
## <a name="optional-query-string-parameters"></a>選擇性的查詢字串參數 
單一訊息上傳的查詢字串參數為： 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| clientFileTime| DateTime| 日期/時間的任何用戶端上的檔案上次上傳檔案。| 
| displayName| 字串| 應該向使用者顯示的檔案名稱。| 
 
多重區塊上傳的查詢字串參數為：
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| clientFileTime| DateTime| 日期/時間的任何用戶端上的檔案上次上傳檔案。| 
| displayName| 字串| 應該向使用者顯示的檔案名稱。| 
| continuationToken| 字串| 接續語彙基元，從先前上傳要求的回應。 如果這是第一個區塊時，這不應指定。 | 
| finalBlock| bool| 設定為檔案的最後一個區塊的 true。 設定為 false 的所有其他區塊。| 
  
<a id="ID4EOE"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 值| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| API 合約版本。| 
| Authorization| XBL3.0 x=[hash];[token]| STS 驗證語彙基元。 驗證要求所傳回的語彙基元取代 STSTokenString。 如需關於擷取之 STS 權杖，以及建立授權標頭的詳細資訊，請參閱驗證和授權的 Xbox LIVE 服務要求。| 
  
<a id="ID4EXF"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| 指定必須符合現有的項目來完成此作業的 ETag。| 
| If-None-Match| 指定的 ETag 必須不符合任何現有的項目，以完成作業。| 
  
<a id="ID4E1G"></a>

 
## <a name="request-body"></a>要求本文 
 
要求主體包含要上傳的檔案內容。 單一訊息上傳，本文會是檔案的完整內容。 進行多區塊上傳，本文會是檔案的一部分的查詢字串參數中指定。 
  
<a id="ID4EFH"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼 
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EYEAC"></a>

 
## <a name="response-body"></a>回應主體 
 
如果呼叫的是多區塊要求成功，服務會傳回傳遞一個區塊取代 continution 語彙基元。
 
<a id="ID4EEFAC"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
    "continuationToken":"abcd1234-1111-2222-3333-abcd12345678-1"
}
         
```

   
<a id="ID4EQFAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ESFAC"></a>

 
##### <a name="parent"></a>父系  

[/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype.md)

   