---
title: GET (/users/{ownerId}/people/{targetid})
assetID: 2fd37b8e-b886-14f2-3399-59f530d85e4e
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetidget.html
description: " GET (/users/{ownerId}/people/{targetid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 408b4df30f53e27b04e2a1e654e9686d2b359637
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632673"
---
# <a name="get-usersowneridpeopletargetid"></a>GET (/users/{ownerId}/people/{targetid})
取得依目標識別碼個人從呼叫者的使用者集合。 這些 Uri 的網域是`social.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [URI 參數](#ID4E5)
  * [Authorization](#ID4EJB)
  * [必要的要求標頭](#ID4ERC)
  * [選擇性的要求標頭](#ID4EQD)
  * [要求本文](#ID4EWE)
  * [HTTP 狀態碼](#ID4EBF)
  * [必要的回應標頭](#ID4EDH)
  * [回應主體](#ID4EQAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
GET 作業不會修改任何資源，因此如果執行一次或多次，這會產生相同的結果。
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| ownerId| 字串| 使用者正在存取的資源識別碼。 必須符合已驗證的使用者。 可能的值為"me"、 xuid({xuid}) 或 gt({gamertag})。| 
| targetid| 字串| 正在擷取其資料擁有者的人員清單，Xbox 使用者識別碼 (XUID) 或玩家代號從使用者的識別碼。 範例值： xuid(2603643534573581)、 gt(SomeGamertag)。| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>Authorization
 
| 類型| 必要| 描述| 如果遺失的回應| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| 是| 呼叫端具有使用者的 Xbox 使用者識別碼 (XUID)。| 401 未經授權| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| 字串。 Xbox LIVE 的授權資料。 這通常是加密的 XSTS 語彙基元。 範例值：<b>XBL3.0 x=&lt;userhash>;&lt;token></b>.| 
  
<a id="ID4EQD"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。預設值：1.| 
| Accept| 字串。 可接受的回應中的呼叫端的內容類型。 所有回應都皆<b>application/json</b>。| 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>要求本文
 
此要求主體中不傳送任何物件。
  
<a id="ID4EBF"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 確定| 成功。| 
| 400| 錯誤的要求| 使用者識別碼的格式不正確。| 
| 403| 已禁止| 無法剖析 XUID 宣告，從授權標頭。| 
| 404| 找不到| 擁有者的人員清單中找不到目標使用者。| 
  
<a id="ID4EDH"></a>

 
## <a name="required-response-headers"></a>必要的回應標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| 32 位元不帶正負號的整數| 長度，以位元組為單位，回應主體。 範例值：22.| 
| Content-Type| 字串| 回應主體的 MIME 類型。 這一定<b>application/json</b>。| 
  
<a id="ID4EQAAC"></a>

 
## <a name="response-body"></a>回應主體
 
如果呼叫成功，服務就會傳回目標人員。 請參閱[人員 (JSON)](../../json/json-person.md)。
 
<a id="ID4E3AAC"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
         
```

   
<a id="ID4EGBAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EIBAC"></a>

 
##### <a name="parent"></a>父系 

[/users/{ownerId}/people/{targetid}](uri-usersowneridpeopletargetid.md)

   