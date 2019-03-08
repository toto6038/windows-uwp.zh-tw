---
title: GET (/users/xuid({xuid})/inbox/{messageId})
assetID: d76563d0-2c74-0308-054b-762c80392a02
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageidget.html
description: " GET (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 29b4c57468148a431a10e0d74f85d360ff0992b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618053"
---
# <a name="get-usersxuidxuidinboxmessageid"></a>GET (/users/xuid({xuid})/inbox/{messageId})
擷取特定的使用者訊息，將它標示為已讀取服務的詳細的訊息文字。
這些 Uri 的網域是`msg.xboxlive.com`。

  * [註解](#ID4EV)
  * [URI 參數](#ID4EEB)
  * [Authorization](#ID4ERB)
  * [要求本文](#ID4E3B)
  * [在資源上的隱私權設定的效果](#ID4EJC)
  * [HTTP 狀態碼](#ID4EUC)
  * [JavaScript 物件標記法 (JSON) 的回應](#ID4EUE)

<a id="ID4EV"></a>


## <a name="remarks"></a>備註

取得作業只對使用者、 系統和 FriendRequest 的訊息類型。

此 URI 會要求在 Xbox.com 重新整理。 目前，在 Xbox 360 不會更新讀取/未讀取的狀態，直到使用者登出，並回到。

此 API 支援只包含內容類型為"application/json"，都需要在每個呼叫的 HTTP 標頭。

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| xuid | 不帶正負號的 64 位元整數 | Xbox 使用者識別碼 (XUID) 正提出要求者的播放程式。 |
| messageId | string[50] | 正在擷取或刪除訊息的識別碼。 |

<a id="ID4ERB"></a>


## <a name="authorization"></a>Authorization

您必須擁有您自己的使用者宣告，以擷取使用者的訊息。

<a id="ID4E3B"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4EJC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>在資源上的隱私權設定的效果

您可以在只擷取您自己的使用者訊息。

<a id="ID4EUC"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。

| 程式碼| 描述|
| --- | --- | --- | --- | --- |
| 200| 成功。|
| 400| XUID 無法正確轉換。|
| 403| 無法轉換 XUID 或找不到有效的 XUID 宣告。|
| 404| 有效的 XUID 遺漏，或將訊息識別碼找不到或無法正確剖析。|
| 500| 一般的伺服器端錯誤或訊息類型不會是適用於取得項目。|

<a id="ID4EUE"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript 物件標記法 (JSON) 的回應

如果呼叫成功，服務會以 JSON 格式傳回結果資料。 根物件是 UserMessageHeader 物件。

#### <a name="usermessageheader"></a>UserMessageHeader

| 屬性| 類型| 最大長度| 備註|
| --- | --- | --- | --- |
| header| 標頭|  | JSON 物件|
| messageText| 字串| 256| UTF-8|

#### <a name="header"></a>標頭

| 屬性| 類型| 最大長度| 備註|
| --- | --- | --- | --- |
| 傳送| DateTime|  | 訊息傳送的日期和時間。 （由服務提供。）|
| 到期日| DateTime|  | 到期日期和時間的訊息。 （所有訊息都有最長存留期，在未來決定）。|
| messageType| 字串| 13| 訊息類型：使用者、 系統、 FriendRequest。|
| senderXuid| ulong|  | XUID 寄件者。|
| 傳送者| 字串| 15| 寄件者的玩家代號。|
| hasAudio| bool|  | 訊息是否音訊 （語音） 附件。|
| hasPhoto| bool|  | 訊息是否之相片附件。|
| hasText| bool|  | 是否此訊息包含文字。|

#### <a name="sample-response"></a>範例回應

```cpp
{
          "header":
          {
            "expiration":"2011-10-11T23:59:59.9999999",
            "messageType":"User",
            "senderXuid":"123456789",
            "sender":"Striker",
            "sent":"2011-05-08T17:30:00Z",
            "hasAudio":false,
            "hasPhoto":false,
            "hasText":true
          },
        "messageText":"random user text up to 256 characters"
        }

```

#### <a name="error-response"></a>錯誤回應

如果發生錯誤，服務可能會傳回 errorResponse 物件，其中可能包含服務的環境的值。

| 屬性| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| 字串| 表示產生錯誤。|
| errorCode| 整數| （可以是 null） 的錯誤相關聯的數字代碼。|
| errorMessage| 字串| 如果設定為顯示詳細資料的錯誤詳細資料。|

<a id="ID4E3DAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4E5DAC"></a>


##### <a name="parent"></a>父系  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EMEAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>參考[標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)
