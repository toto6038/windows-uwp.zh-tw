---
title: GET (/users/{ownerId}/people)
assetID: c948d031-ec19-7571-31ef-23cb9c5ebfaf
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopleget.html
description: " GET (/users/{ownerId}/people)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6c8672188a93b2e8d27a081ae068387e7ee7aa42
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623763"
---
# <a name="get-usersowneridpeople"></a>GET (/users/{ownerId}/people)
取得呼叫者的使用者集合。
這些 Uri 的網域是`social.xboxlive.com`。

  * [註解](#ID4EV)
  * [URI 參數](#ID4E5)
  * [查詢字串參數](#ID4EJB)
  * [Authorization](#ID4ERD)
  * [必要的要求標頭](#ID4EZE)
  * [選擇性的要求標頭](#ID4EYF)
  * [要求本文](#ID4E5G)
  * [HTTP 狀態碼](#ID4EJH)
  * [必要的回應標頭](#ID4EBBAC)
  * [回應主體](#ID4ENCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>備註

GET 作業不會修改任何資源，因此如果執行一次或多次，這會產生相同的結果。

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| ownerId| 字串| 使用者正在存取的資源識別碼。 必須符合已驗證的使用者。 可能的值為"me"、 xuid({xuid}) 或 gt({gamertag})。|

<a id="ID4EJB"></a>


## <a name="query-string-parameters"></a>查詢字串參數

| 參數| 類型| 描述|
| --- | --- | --- | --- | --- | --- |
| 檢視| 字串| 傳回與檢視相關聯的人員。 預設值為"all"。 可能的值為： <ul><li><b>所有</b>-傳回使用者的人員清單上的所有人員。 這是預設值。</li><li><b>最愛</b>-傳回使用者的人員清單具有設為我的最愛屬性上的所有人員。</li><li><b>LegacyXboxLiveFriends</b> -傳回同時為舊版的 Xbox LIVE 好友使用者的人員清單上的所有人。</li></br>**注意：** 只有**所有**如果呼叫的使用者是不同的擁有使用者支援的值。|
| startIndex| 32 位元不帶正負號的整數| 傳回指定索引處開始的項目。  
| maxItems| 32 位元不帶正負號的整數| 若要從起始索引從集合中傳回的人員的最大數目。 服務可能會提供預設值，如果<b>maxItems</b>不存在，且可能會傳回少於<b>maxItems</b> （即使尚未傳回結果的最後一頁）。|

<a id="ID4ERD"></a>


## <a name="authorization"></a>Authorization

| 類型| 必要| 描述| 如果遺失的回應|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| 是| 呼叫端具有使用者的 Xbox 使用者識別碼 (XUID)。| 401 未經授權|

<a id="ID4EZE"></a>


## <a name="required-request-headers"></a>必要的要求標頭

| 標頭| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| 字串。 Xbox LIVE 的授權資料。 這通常是加密的 XSTS 語彙基元。 範例值：<b>XBL3.0 x=&lt;userhash>;&lt;token></b>.|

<a id="ID4EYF"></a>


## <a name="optional-request-headers"></a>選擇性的要求標頭

| 標頭| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。預設值：1.|
| Accept| 字串。 可接受的回應中的呼叫端的內容類型。 所有回應都皆<b>application/json</b>。|

<a id="ID4E5G"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4EJH"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。

| 程式碼| 原因片語| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 確定| 成功。|
| 400| 錯誤的要求| 查詢參數] 或 [使用者識別碼的格式不正確。|
| 403| 已禁止| 無法剖析 XUID 宣告，從授權標頭。|

<a id="ID4EBBAC"></a>


## <a name="required-response-headers"></a>必要的回應標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| 32 位元不帶正負號的整數| 長度，以位元組為單位，回應主體。 範例值：22.|
| Content-Type| 字串| 回應主體的 MIME 類型。 這一定<b>application/json</b>。|

<a id="ID4ENCAC"></a>


## <a name="response-body"></a>回應主體

如果呼叫成功，服務就會傳回呼叫端的使用者集合和陣列，其中包含呼叫者的人員集合在總人數。 請參閱[PeopleList (JSON)](../../json/json-peoplelist.md)。

<a id="ID4EZCAC"></a>


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
            "isFollowingCaller": false,
            "isFavorite": false
        },
    ],
    "totalCount": 3
}

```


<a id="ID4EDDAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EFDAC"></a>


##### <a name="parent"></a>父系

[/users/{ownerId}/people](uri-usersowneridpeople.md)
