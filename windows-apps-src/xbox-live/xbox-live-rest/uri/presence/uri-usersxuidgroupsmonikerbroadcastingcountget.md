---
title: GET (/users/xuid({xuid})/groups/{moniker}/broadcasting/count )
assetID: 2b2df42e-9b39-3906-36e4-fd9ff22add04
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcastingcountget.html
description: " GET (/users/xuid({xuid})/groups/{moniker}/broadcasting/count )"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 219942838902381d7c9be91287056c422ea7957e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616813"
---
# <a name="get-usersxuidxuidgroupsmonikerbroadcastingcount-"></a>GET (/users/xuid({xuid})/groups/{moniker}/broadcasting/count )
擷取與 URI 中會出現 XUID 群組 moniker 所指定的廣播使用者計數。 這些 Uri 的網域是`userpresence.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [URI 參數](#ID4E5)
  * [查詢字串參數](#ID4EJB)
  * [Authorization](#ID4EKC)
  * [在資源上的隱私權設定的效果](#ID4EQD)
  * [必要的要求標頭](#ID4EEH)
  * [選擇性的要求標頭](#ID4EMBAC)
  * [要求本文](#ID4EMCAC)
  * [HTTP 狀態碼](#ID4EXCAC)
  * [必要的回應標頭](#ID4E3GAC)
  * [選擇性的回應標頭](#ID4EMJAC)
  * [回應主體](#ID4E5KAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
擷取與 URI 中會出現 XUID 群組 moniker 所指定的廣播使用者計數。
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| xuid| 字串| Xbox 使用者識別碼 (XUID) 與群組中 XUIDs 相關的使用者。| 
| moniker| 字串| 定義使用者群組的字串。 目前唯一接受的 moniker 是 「 人 」，以大寫 'P'。| 
  
<a id="ID4EJB"></a>

 
## <a name="query-string-parameters"></a>查詢字串參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| level| 字串| 傳回所指定的這個查詢字串的詳細程度。 有效的選項包括"user"、"device"、"title"，和 「 全部 」。「 使用者 」 的層級是 PresenceRecord 物件沒有 DeviceRecord 巢狀物件。 「 裝置 」 的層級是 PresenceRecord 和 DeviceRecord 物件沒有 TitleRecord 巢狀物件。 層級"title"是 PresenceRecord、 DeviceRecord 和 TitleRecord 物件沒有 ActivityRecord 巢狀物件。 「 全部 」 層級是整筆記錄，包括所有巢狀的物件。如果未提供這個參數，服務就會預設為標題層級 （也就是它會傳回項目的詳細資料向這位使用者的目前狀態）。| 
  
<a id="ID4EKC"></a>

 
## <a name="authorization"></a>Authorization
 
使用授權宣告 | 宣告| 類型| 必要？| 範例值| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>Xuid</b>| 64 位元帶正負號的整數| 是| 1234567890| 
  
<a id="ID4EQD"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>在資源上的隱私權設定的效果
 
服務一律會傳回 200 OK 如果要求本身的格式正確。 不過，它會篩選出來自回應的資訊時的隱私權檢查未通過。
 
在資源上的隱私權設定的效果 | 提出要求的使用者| 目標使用者的隱私權設定| 行為| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 我| -| 如所述。| 
| friend| 每個人| 如所述。| 
| friend| 只有朋友| 如所述。| 
| friend| 封鎖| 如所述-服務將會篩選出資料。| 
| 非 friend 使用者| 每個人| 如所述。| 
| 非 friend 使用者| 只有朋友| 如所述-服務將會篩選出資料。| 
| 非 friend 使用者| 封鎖| 如所述-服務將會篩選出資料。| 
| 協力廠商網站| 每個人| 如所述-服務將會篩選出資料。| 
| 協力廠商網站| 只有朋友| 如所述-服務將會篩選出資料。| 
| 協力廠商網站| 封鎖| 如所述-服務將會篩選出資料。| 
  
<a id="ID4EEH"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：「 XBL3.0 x =&lt;userhash >;&lt;權杖 > 」。| 
| x-xbl-contract-version| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 範例值：3，vnext。| 
| Accept| 字串| 可接受的內容類型。 只有一個組態選項支援<b>application/json</b>，但它仍必須指定標頭中。| 
| Accept-Language| 字串| 可接受的回應中的字串的地區設定。 範例值： EN-US。| 
| 主機| 字串| 伺服器的網域名稱。 範例值： userpresence.xboxlive.com。| 
  
<a id="ID4EMBAC"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 預設值：1.| 
  
<a id="ID4EMCAC"></a>

 
## <a name="request-body"></a>要求本文
 
此要求主體中不傳送任何物件。
  
<a id="ID4EXCAC"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 確定| 已成功擷取工作階段。| 
| 400| 錯誤的要求| 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
| 401| 未經授權| 要求需要使用者驗證。| 
| 403| 已禁止| 要求不允許使用者或服務。| 
| 404| 找不到| 找不到指定的資源。| 
| 405| 不允許的方法| 不支援的內容類型上使用 HTTP 方法。| 
| 406| 無法接受| 不支援資源版本。| 
| 500| 要求逾時| 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
| 503| 要求逾時| 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
  
<a id="ID4E3GAC"></a>

 
## <a name="required-response-headers"></a>必要的回應標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 範例值：1，vnext。| 
| Content-Type| 字串| 在要求主體的 mime 類型。 範例值： <b>application/json</b>。| 
| Cache-Control| 字串| 正常的要求，來指定快取行為。 範例值: [無快取]。| 
| X-XblCorrelationId| 字串| 與伺服器會傳回和收到用戶端相關聯的服務產生值。 範例值：「 4106d0bc-1cb3-47bd-83fd-57d041c6febe"。| 
| X-Content-Type-Option| 字串| 傳回符合 SDL 的值。 範例值:"nosniff"。| 
| 日期| 字串| 訊息已傳送的日期/時間。 範例值："2012 年 11 月 17 日日星期二格林威治時間 10:33:31"。| 
  
<a id="ID4EMJAC"></a>

 
## <a name="optional-response-headers"></a>選擇性的回應標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 稍後再試| 字串| 傳回 503 HTTP 錯誤。 可讓用戶端重試的呼叫之前要等待多久知道。 範例值："120".| 
| Content-Length| 字串| 回應主體的長度。 範例值："527".| 
| Content-Encoding| 字串| 回應的編碼類型。 範例值:"gzip"。| 
  
<a id="ID4E5KAC"></a>

 
## <a name="response-body"></a>回應主體
 
此 API 會傳回目前的廣播參數的數目的計數。
 
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
    "count":42
 }

         
```

   
<a id="ID4EQLAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ESLAC"></a>

 
##### <a name="parent"></a>父系 

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

   