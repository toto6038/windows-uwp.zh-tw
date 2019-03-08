---
title: GET (/users/{ownerId}/people/mute)
assetID: 49b6c830-95f7-3200-0e46-0a1af573971c
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemuteget.html
description: " GET (/users/{ownerId}/people/mute)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 94e2bf4d04619ffa3348ae08fc37964cdc58e7b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661583"
---
# <a name="get-usersowneridpeoplemute"></a>GET (/users/{ownerId}/people/mute)
取得使用者的 [靜音] 清單。

  * [註解](#ID4EQ)
  * [URI 參數](#ID4EZ)
  * [在資源上的隱私權設定的效果](#ID4EEB)
  * [Authorization](#ID4ENB)
  * [必要的要求標頭](#ID4ESC)
  * [要求本文](#ID4EPE)
  * [HTTP 狀態碼](#ID4E1E)
  * [必要的回應標頭](#ID4E3G)
  * [回應主體](#ID4ETAAC)

<a id="ID4EQ"></a>


## <a name="remarks"></a>備註

如果指定的目標，此 URI 會傳回只有該使用者，如果使用者是在 [靜音] 清單中，或空如果使用者不是。

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| ownerId| 字串| 必要。 使用者正在存取的資源識別碼。 可能的值為"me" <code>xuid({xuid})</code>，或 gt({gamertag})。 必須是已驗證的使用者。 範例值： <code>xuid(2603643534573581)</code>， <code>gt(SomeGamertag)</code>。 大小上限： 無。 |

<a id="ID4EEB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>在資源上的隱私權設定的效果

無。

<a id="ID4ENB"></a>


## <a name="authorization"></a>Authorization

使用授權宣告 | 宣告| 類型| 必要？| 範例值|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| 64 位元帶正負號的整數| 是| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>必要的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization | 字串| HTTP 驗證的驗證認證。 範例值： <code>Xauth=&lt;authtoken></code>。 大小上限： 無。|
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，在授權權杖中，宣告的有效性，並依此類推。 範例值： <code>1</code>， <code>vnext</code>。 大小上限： 無。|
| Accept| 字串| 可接受的內容類型。 範例值： <code>application/json</code>。 大小上限： 無。|

<a id="ID4EPE"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4E1E"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。

| 程式碼| 原因片語| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 確定| [靜音] 清單的成功要求。|
| 400| 錯誤的要求| URI 中指定的目標識別碼無效。|
| 403| 已禁止| 在 URI 中指定的擁有者不是已驗證的使用者。|
| 404| 找不到| 在 URI 中指定的擁有者不存在。|

<a id="ID4E3G"></a>


## <a name="required-response-headers"></a>必要的回應標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 字串| 在要求主體的 MIME 類型。 範例值： <code>application/json</code>|
| Content-Length| 字串| 在回應中傳送的位元組數目。 範例值：34|
| Cache-Control| 字串| 正常伺服器要求來指定快取行為。 範例： <code>no-cache, no-store</code>|

<a id="ID4ETAAC"></a>


## <a name="response-body"></a>回應主體

<a id="ID4EZAAC"></a>


### <a name="sample-response"></a>範例回應

請參閱[UserList](../../json/json-userlist.md)。


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EJBAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4ELBAC"></a>


##### <a name="parent"></a>父系

[/users/{ownerId}/people/mute](uri-privacyusersowneridpeoplemute.md)
