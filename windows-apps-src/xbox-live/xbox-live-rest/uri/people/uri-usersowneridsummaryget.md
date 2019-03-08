---
title: GET (/users/{ownerId}/summary)
assetID: 754190c9-b15d-f34b-1dca-5c92f6f67d12
permalink: en-us/docs/xboxlive/rest/uri-usersowneridsummaryget.html
description: " GET (/users/{ownerId}/summary)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3b228adab7b035ec8f4e65fc8b7458228a677987
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613993"
---
# <a name="get-usersowneridsummary"></a>GET (/users/{ownerId}/summary)
取得擁有者相關的摘要資料，從呼叫端的觀點來看。

  * [URI 參數](#ID4EQ)
  * [Authorization](#ID4E2)
  * [必要的要求標頭](#ID4EBC)
  * [選擇性的要求標頭](#ID4EHD)
  * [要求本文](#ID4EXE)
  * [HTTP 狀態碼](#ID4ECF)
  * [必要的回應標頭](#ID4EZG)
  * [回應主體](#ID4EGAAC)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| ownerId| 字串| 使用者正在存取的資源識別碼。 可能的值為"me"、 xuid({xuid}) 或 gt({gamertag})。 範例值： <code>me</code>， <code>xuid(2603643534573581)</code>， <code>gt(SomeGamertag)</code>|

<a id="ID4E2"></a>


## <a name="authorization"></a>Authorization

| <b>名稱</b>| <b>類型</b>| <b>描述</b>|
| --- | --- | --- | --- | --- | --- |
| xuid| 64 位元不帶正負號的整數| 必要。 呼叫端的使用者識別碼。 範例值：2533274790395904|

<a id="ID4EBC"></a>


## <a name="required-request-headers"></a>必要的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| 字串| 授權資料。 這通常是加密的 XSTS 語彙基元。 範例值：<b>XBL3.0 x=[hash];[token]</b>.|

<a id="ID4EHD"></a>


## <a name="optional-request-headers"></a>選擇性的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-xbl-contract-version| 字串| 名稱/的組建編號應該導向此要求的服務。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。範例值：1|
| Accept| 字串| 可接受的內容類型。 所有的回應會是<code>application/json</code>。|

<a id="ID4EXE"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4ECF"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。

| 程式碼| 原因片語| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 確定| 已成功擷取工作階段。|
| 400| 錯誤的要求| 使用者識別碼的格式不正確。|
| 403| 已禁止| 無法剖析 XUID 宣告，從授權標頭。|

<a id="ID4EZG"></a>


## <a name="required-response-headers"></a>必要的回應標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| 字串| 在回應中傳送的位元組數目。 範例值：232.|
| Content-Type| 字串| 回應主體的 MIME 類型。 這必須是<b>application/json</b>。|

<a id="ID4EGAAC"></a>


## <a name="response-body"></a>回應主體

請參閱[PersonSummary (JSON)](../../json/json-personsummary.md)。

<a id="ID4ESAAC"></a>


### <a name="sample-response"></a>範例回應


```cpp
{
    "targetFollowingCount": 87,
    "targetFollowerCount": 19,
    "isCallerFollowingTarget": true,
    "isTargetFollowingCaller": false,
    "hasCallerMarkedTargetAsFavorite": true,
    "hasCallerMarkedTargetAsKnown": true,
    "legacyFriendStatus": "None",
    "recentChangeCount": 5,
    "watermark": "5246775845000319351"
}

```


<a id="ID4E3AAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4E5AAC"></a>


##### <a name="parent"></a>父系

[/users/{ownerId}/summary](uri-usersowneridsummary.md)
