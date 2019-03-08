---
title: GET (/users/xuid({xuid})/inbox)
assetID: c603910d-b430-f157-2634-ceddea89f2bd
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxget.html
description: " GET (/users/xuid({xuid})/inbox)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 05f75510f15f6e6c5f1b1673673428c00f7a6c16
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632243"
---
# <a name="get-usersxuidxuidinbox"></a>GET (/users/xuid({xuid})/inbox)
從服務中擷取指定的數目的使用者訊息摘要。
這些 Uri 的網域是`msg.xboxlive.com`。

  * [註解](#ID4EV)
  * [URI 參數](#ID4EEB)
  * [查詢字串參數](#ID4EIC)
  * [Authorization](#ID4EGE)
  * [在資源上的隱私權設定的效果](#ID4ETE)
  * [HTTP 狀態碼](#ID4E5E)
  * [JavaScript 物件標記法 (JSON) 的回應](#ID4EMH)

<a id="ID4EV"></a>


## <a name="remarks"></a>備註

使用者訊息摘要包含訊息主體。 針對使用者產生的訊息，這是目前的訊息文字的前 20 個字元。 系統訊息可能會提供替代的主旨，例如 「 即時系統 」。

相反順序傳送; 會傳回訊息也就是較新的訊息會傳回第一次。

此 API 支援只包含內容類型為"application/json"，都需要在每個呼叫的 HTTP 標頭。

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| xuid| 不帶正負號的 64 位元整數| Xbox 使用者識別碼 (XUID) 正提出要求者的播放程式。|

<a id="ID4EIC"></a>


## <a name="query-string-parameters"></a>查詢字串參數

| 屬性| 類型| 最大長度| 備註|
| --- | --- | --- | --- | --- | --- | --- |
| maxItems| 整數| 100| 傳回的訊息數目上限。|
| continuationToken| 字串|  | 在先前的列舉型別呼叫; 中傳回的字串用來繼續列舉型別。|
| skipItems| 整數| 100| 略過; 訊息數目如果 continuationToken 是存在，則會忽略。|

<a id="ID4EGE"></a>


## <a name="authorization"></a>Authorization

您必須擁有您自己的使用者宣告，以擷取使用者的訊息摘要。

<a id="ID4ETE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>在資源上的隱私權設定的效果

只有您可以列舉您自己的使用者訊息。

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。

| 程式碼| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 要求成功。|
| 400| 服務無法辨識格式不正確的要求。 通常是無效的參數。|
| 403| 要求不允許使用者或服務。|
| 404| 在 URI 中遺漏有效的 XUID。|
| 409| 基礎集合變更根據傳入接續權杖。|
| 416| 略過的項目數目大於可用的項目數目。|
| 500| 一般的伺服器端錯誤。|

<a id="ID4EMH"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript 物件標記法 (JSON) 的回應

如果呼叫成功，服務會以 JSON 格式傳回結果資料。

| 屬性| 類型| 最大長度| 備註|
| --- | --- | --- | --- |
| 結果| Message[]| 100| 使用者訊息的陣列|
| pagingInfo| PagingInfo|  | 目前的結果集的分頁資訊|

#### <a name="message"></a>訊息

| 屬性| 類型| 最大長度| 備註|
| --- | --- | --- | --- |
| header| 標頭|  | 使用者訊息標頭|
| messageSummary| 字串| 20| UTF-8;通常訊息的前 20 個字元|

#### <a name="header"></a>標頭

| 屬性| 類型| 最大長度| 備註|
| --- | --- | --- | --- |
| id| 字串| 50| 用於擷取訊息詳細資料，或刪除訊息的訊息識別碼。|
| isRead| bool|  | 旗標，表示使用者已讀取的訊息詳細資料。|
| 傳送| DateTime|  | UTC 日期和時間已傳送訊息。 （由服務提供。）|
| 到期日| DateTime|  | 訊息到期 UTC 日期和時間。 （所有訊息都有最長存留期，在未來決定）。|
| messageType| 字串| 50| 訊息類型：使用者、 系統、 FriendRequest、 視訊、 QuickChat、 VideoChat、 PartyChat、 標題、 GameInvite。|
| senderXuid| ulong|  | XUID 寄件者。|
| 傳送者| 字串| 15| 寄件者的玩家代號。|
| hasAudio| bool|  | 訊息是否音訊 （語音） 附件。|
| hasPhoto| bool|  | 訊息是否之相片附件。|
| hasText| bool|  | 是否此訊息包含文字。|

#### <a name="paging-info"></a>分頁資訊

| 屬性| 類型| 最大長度| 備註|
| --- | --- | --- | --- |
| continuationToken| 字串| 100| （選擇性） 由伺服器傳回。 可讓後續的呼叫，以繼續列舉型別。|
| totalItems| 整數|  | 收件匣中的訊息總數。|

#### <a name="sample-response"></a>範例回應

```cpp
{
          "results":
          [
            {
              "header":
              {
                "expiration":"2011-10-11T23:59:59.9999999",
                "id":"opaqueBlobOfText",
                "messageType":"User",
                "isRead":false,
                "senderXuid":"123456789",
                "sender":"Striker",
                "sent":"2011-05-08T17:30:00Z",
                "hasAudio":false,
                "hasPhoto":false,
                "hasText":true
              },
            "messageSummary":"first 20 chars"
          },
          ...
        ],
        "pagingInfo":
          {
          "continuationToken":"opaqueBlobOfText"
          "totalItems":5,
          }
        }

```

#### <a name="error-response"></a>錯誤回應

如果發生錯誤，服務可能會傳回 errorResponse 物件，其中可能包含服務的環境的值。

| 屬性| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| 字串| 表示產生錯誤。|
| errorCode| 整數| （可以是 null） 的錯誤相關聯的數字代碼。|
| errorMessage| 字串| 如果設定為顯示詳細資料的錯誤詳細資料。|

<a id="ID4EIKAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EKKAC"></a>


##### <a name="parent"></a>父系  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EWKAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>參考[標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)
