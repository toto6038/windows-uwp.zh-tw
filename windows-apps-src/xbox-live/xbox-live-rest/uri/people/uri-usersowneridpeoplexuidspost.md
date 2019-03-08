---
title: POST (/users/{ownerId}/people/xuids)
assetID: e20bfb58-9c3b-14ed-6462-85d42fa6fe1a
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuidspost.html
description: " POST (/users/{ownerId}/people/xuids)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1cb160f3276d215e3aba5dfd671c67fa17d883b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589863"
---
# <a name="post-usersowneridpeoplexuids"></a>POST (/users/{ownerId}/people/xuids)
取得由 XUID 人員呼叫者的使用者集合。 這些 Uri 的網域是`social.xboxlive.com`。
 
  * [註解](#ID4EV)
  * [URI 參數](#ID4E5)
  * [Authorization](#ID4EJB)
  * [必要的要求標頭](#ID4ERC)
  * [選擇性的要求標頭](#ID4EBE)
  * [要求本文](#ID4EHF)
  * [HTTP 狀態碼](#ID4EKH)
  * [必要的回應標頭](#ID4ENBAC)
  * [回應主體](#ID4EZCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>備註
 
POST 作業不會修改任何資源，因此如果執行一次或多次，這會產生相同的結果。
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| ownerId| 字串| 使用者正在存取的資源識別碼。 必須符合已驗證的使用者。 可能的值為"me"、 xuid({xuid}) 或 gt({gamertag})。| 
  
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
| Content-Length| 32 位元不帶正負號的整數。 長度，以位元組為單位的要求主體。 範例值：22.| 
| Content-Type| 字串。 要求主體的 MIME 類型。 這必須是<b>application/json</b>。| 
  
<a id="ID4EBE"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。預設值：1.| 
| Accept| 字串。 可接受的回應中的呼叫端的內容類型。 所有回應都皆<b>application/json</b>。| 
  
<a id="ID4EHF"></a>

 
## <a name="request-body"></a>要求本文
 
<a id="ID4ENF"></a>

 
### <a name="required-members"></a>必要的成員
 
| 成員| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| XuidList| 識別要從呼叫端的使用者集合中傳回的人員的 XUIDs 的陣列。 請參閱[XuidList (JSON)](../../json/json-xuidlist.md)。| 
  
<a id="ID4EKG"></a>

 
### <a name="optional-members"></a>選擇性的成員
 
沒有此要求的選擇性成員。
  
<a id="ID4EVG"></a>

 
### <a name="prohibited-members"></a>禁止的成員
 
所有其他成員均不得在要求中。
  
<a id="ID4EAH"></a>

 
### <a name="sample-request"></a>範例要求
 

```cpp
{
    "xuids": [
        "2533274790395904", 
        "2533274792986770", 
        "2533274794866999"
    ]
}
      
```

   
<a id="ID4EKH"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 確定| 成功時的方法是 「 get 」。| 
| 204| 沒有內容| 成功時的方法是 [新增] 或者 [移除]。| 
| 400| 錯誤的要求| 方法參數遺失或格式不正確，或使用者識別碼是格式不正確。| 
| 403| 已禁止| 無法剖析 XUID 宣告，從授權標頭。| 
  
<a id="ID4ENBAC"></a>

 
## <a name="required-response-headers"></a>必要的回應標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| 32 位元不帶正負號的整數| 長度，以位元組為單位，回應主體。 範例值：22.| 
| Content-Type| 字串| 回應主體的 MIME 類型。 這一定<b>application/json</b>。| 
  
<a id="ID4EZCAC"></a>

 
## <a name="response-body"></a>回應主體
 
要求方法為"get"時，才會傳送回應主體。 沒有任何回應本文的 [新增] 或 [移除]。
 
如果 「 get 」 方法呼叫成功，服務會在每次集合和陣列，其中包含呼叫端的使用者集合，呼叫端的人傳回總人數。 [新增] 和 [移除] 方法會不傳回任何回應。 請參閱[PeopleList (JSON)](../../json/json-peoplelist.md)。
 
<a id="ID4EHDAC"></a>

 
### <a name="sample-response"></a>範例回應
 

```cpp
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFavorite": false,
            "isFollowingCaller": false
        },
    ],
    "totalCount": 3
}
         
```

   
<a id="ID4ERDAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ETDAC"></a>

 
##### <a name="parent"></a>父系 

[/users/{ownerId}/people/xuids](uri-usersowneridpeoplexuids.md)

   