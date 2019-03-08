---
title: GET (/users/{requestorId}/permission/validate)
assetID: 8d22c668-af9a-1d24-8d65-830c2ce913d7
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidateget.html
description: " GET (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 07ccac63b3e6681ea35405314b0cd8b93aa60f9a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594753"
---
# <a name="get-usersrequestoridpermissionvalidate"></a>GET (/users/{requestorId}/permission/validate)
取得關於是否允許使用者執行指定的動作，與目標使用者是或否回應。

  * [URI 參數](#ID4EQ)
  * [查詢字串參數](#ID4E2)
  * [Authorization](#ID4EDC)
  * [必要的要求標頭](#ID4EID)
  * [要求本文](#ID4ETE)
  * [HTTP 狀態碼](#ID4E5E)
  * [必要的回應標頭](#ID4ETG)
  * [回應主體](#ID4EKAAC)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| requestorId| 字串| 必要。 執行此動作的使用者識別碼。 可能的值為<code>xuid({xuid})</code>和<code>me</code>。 這必須是登入的使用者。 範例值： <code>xuid(0987654321)</code>。|

<a id="ID4E2"></a>


## <a name="query-string-parameters"></a>查詢字串參數

| 參數| 類型| 描述|
| --- | --- | --- | --- | --- | --- |
| 設定| 字串列舉型別| 要檢查的 PermissionId 值。 範例值：「 CommunicateUsingText"。|
| 目標| 字串| 在其上的動作是要執行之使用者的識別項。 可能的值為<code>xuid({xuid})</code>。 範例值： <code>xuid(0987654321)</code>|

<a id="ID4EDC"></a>


## <a name="authorization"></a>Authorization

使用授權宣告 | 宣告| 類型| 必要？| 範例值|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| xuid| 64 位元帶正負號的整數| 是| 1234567890|

<a id="ID4EID"></a>


## <a name="required-request-headers"></a>必要的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值： <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例值：1.|

<a id="ID4ETE"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。

| 程式碼| 原因片語| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 確定| 已成功擷取工作階段。|
| 400| 要求無效。| 範例： 不正確的設定識別碼不正確的 Uri 等。|
| 404| 在 URI 中指定的使用者不存在。| 找不到指定的資源。|

<a id="ID4ETG"></a>


## <a name="required-response-headers"></a>必要的回應標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 字串| 在要求主體的 MIME 類型。 範例值： <code>application/json</code>|
| Content-Length| 字串| 在回應中傳送的位元組數目。 範例值：34|
| Cache-Control| 字串| 正常伺服器要求來指定快取行為。 範例： <code>no-cache, no-store</code>|

<a id="ID4EKAAC"></a>


## <a name="response-body"></a>回應主體

請參閱[PermissionCheckResponse (JSON)](../../json/json-permissioncheckresponse.md)。

<a id="ID4EWAAC"></a>


### <a name="sample-response"></a>範例回應


```cpp
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}

```


<a id="ID4EABAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4ECBAC"></a>


##### <a name="parent"></a>父系

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [PermissionId 列舉](../../enums/privacy-enum-permissionid.md)
