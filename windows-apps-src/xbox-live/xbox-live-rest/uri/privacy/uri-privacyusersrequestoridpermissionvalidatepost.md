---
title: POST (/users/{requestorId}/permission/validate)
assetID: 7a5ea583-ffca-5da7-a02a-535c52535928
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidatepost.html
description: " POST (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: edd91560ffb5d81b30da4b1453612cc5853a456f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623423"
---
# <a name="post-usersrequestoridpermissionvalidate"></a>POST (/users/{requestorId}/permission/validate)
取得一份是或否解答是否允許使用者執行的一組目標使用者的指定的動作。

  * [註解](#ID4EQ)
  * [URI 參數](#ID4ECB)
  * [Authorization](#ID4ENB)
  * [必要的要求標頭](#ID4ESC)
  * [要求本文](#ID4E4D)
  * [HTTP 狀態碼](#ID4ETE)
  * [必要的回應標頭](#ID4EIG)
  * [回應主體](#ID4E5H)

<a id="ID4EQ"></a>


## <a name="remarks"></a>備註

要求本文會使用的使用者清單和一份設定，且結果是允許/封鎖結果的每個使用者/設定組。

在跨網路多人遊戲案例中 （隱私權通訊檢查必須在其中執行具有 Xbox 使用者識別碼 (XUID) 的使用者與不關閉網路使用者之間），請參閱[PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md)使用者類型。

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| requestorId| 字串| 必要。 執行此動作的使用者識別碼。 可能的值為<code>xuid({xuid})</code>和<code>me</code>。 這必須是登入的使用者。 範例值： <code>xuid(0987654321)</code>。|

<a id="ID4ENB"></a>


## <a name="authorization"></a>Authorization

使用授權宣告 | 宣告| 類型| 必要？| 範例值|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| 64 位元帶正負號的整數| 是| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>必要的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值： <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| 字串| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例值：1.|

<a id="ID4E4D"></a>


## <a name="request-body"></a>要求本文

<a id="ID4EDE"></a>


### <a name="required-members"></a>必要的成員

請參閱[PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md)。


```cpp
{
    "users":
    [
        {"xuid":"12345"},
        {"xuid":"54321"}
    ],
    "permissions":
    [
        "ViewTargetGameHistory",
        "ViewTargetProfile"
    ]
}

```


<a id="ID4ETE"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。

| 程式碼| 原因片語| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 確定| 已成功擷取工作階段。|
| 400| 要求無效。| 範例： 不正確的設定識別碼不正確的 Uri 等。|
| 404| 在 URI 中指定的使用者不存在。| 找不到指定的資源。|

<a id="ID4EIG"></a>


## <a name="required-response-headers"></a>必要的回應標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 字串| 在要求主體的 MIME 類型。 範例值： <code>application/json</code>|
| Content-Length| 字串| 在回應中傳送的位元組數目。 範例值：34|
| Cache-Control| 字串| 正常伺服器要求來指定快取行為。 範例： <code>no-cache, no-store</code>|

<a id="ID4E5H"></a>


## <a name="response-body"></a>回應主體

請參閱[PermissionCheckBatchResponse (JSON)](../../json/json-permissioncheckbatchresponse.md)。

<a id="ID4ELAAC"></a>


### <a name="sample-response"></a>範例回應


```cpp
{
    "responses":
    [
        {
            "user": {"xuid":"12345"},
            "permissions":
            [
                {
                    "isAllowed":true
                },
                {
                    "isAllowed":true
                }
            ]
        },
        {
            "user": {"xuid":"54321"},
            "permissions":
            [
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"NotAllowed"}
                    ]
                },
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"PrivilegeRest", "restrictedSetting":"AllowProfileViewing"}
                    ]
                }
            ]
        }
    ]
}

```


<a id="ID4EVAAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EXAAC"></a>


##### <a name="parent"></a>父系

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [PermissionId 列舉](../../enums/privacy-enum-permissionid.md)
