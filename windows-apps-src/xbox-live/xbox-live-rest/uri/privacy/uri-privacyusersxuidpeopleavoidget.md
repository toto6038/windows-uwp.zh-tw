---
title: GET (/users/{ownerId}/people/avoid)
assetID: e3420658-4738-8e80-44da-8281726fce01
permalink: en-us/docs/xboxlive/rest/uri-privacyusersxuidpeopleavoidget.html
description: " GET (/users/{ownerId}/people/avoid)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 745893b4b975b5fbf64fe76591ec15d18af59d73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641613"
---
# <a name="get-usersowneridpeopleavoid"></a>GET (/users/{ownerId}/people/avoid)
取得使用者避免清單。

  * [註解](#ID4EQ)
  * [URI 參數](#ID4EZ)
  * [Authorization](#ID4EEB)
  * [必要的要求標頭](#ID4EJC)
  * [HTTP 狀態碼](#ID4EYD)
  * [必要的回應標頭](#ID4E1F)
  * [回應主體](#ID4ESH)

<a id="ID4EQ"></a>


## <a name="remarks"></a>備註

如果指定的目標，只傳回該使用者，如果它們在區塊清單中，或空如果不是。

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| ownerId| 字串| 必要。 使用者正在存取的資源識別碼。 可能的值為<code>xuid({xuid})</code>。 必須是已驗證的使用者。 範例值： <code>xuid(2603643534573581)</code>。 大小上限： 無。 |

<a id="ID4EEB"></a>


## <a name="authorization"></a>Authorization

使用授權宣告 | 宣告| 類型| 必要？| 範例值|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| 64 位元帶正負號的整數| 是| 1234567890|

<a id="ID4EJC"></a>


## <a name="required-request-headers"></a>必要的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization | 字串| HTTP 驗證的驗證認證。 範例值： <code>Xauth=&lt;authtoken></code>。 大小上限： 無。|
| Accept| 字串| 可接受的內容類型。 範例值： <code>application/json</code>。 大小上限： 無。|

<a id="ID4EYD"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。

| 程式碼| 原因片語| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 確定| 已成功擷取工作階段。|
| 400| 錯誤的要求| URI 中指定的目標識別碼無效。|
| 403| 已禁止| 在 URI 中指定的擁有者不是已驗證的使用者。|
| 404| 找不到| 在 URI 中指定的擁有者不存在。|

<a id="ID4E1F"></a>


## <a name="required-response-headers"></a>必要的回應標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 字串| 在要求主體的 MIME 類型。 範例值： <code>application/json</code>。 大小上限： 無。|
| Content-Length| 字串| 在回應中傳送的位元組數目。 範例值：34. 大小上限： 無。|
| Cache-Control| 字串| 正常伺服器要求來指定快取行為。 範例值： <code>application/json</code>。 大小上限： 無。|

<a id="ID4ESH"></a>


## <a name="response-body"></a>回應主體

<a id="ID4EYH"></a>


### <a name="sample-response"></a>範例回應


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EDAAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EFAAC"></a>


##### <a name="parent"></a>父系

[/users/{ownerId}/people/avoid](uri-privacyusersxuidpeopleavoid.md)
