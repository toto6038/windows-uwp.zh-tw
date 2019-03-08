---
title: POST (/users/xuid({xuid})/outbox)
assetID: de991d88-efe0-04f2-f6b2-0bc3e68bfd46
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutboxpost.html
description: " POST (/users/xuid({xuid})/outbox)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2b225e8441fee3d499f172e2e5701096cdbc161a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594283"
---
# <a name="post-usersxuidxuidoutbox"></a>POST (/users/xuid({xuid})/outbox)
將指定的訊息傳送至收件者清單中。
這些 Uri 的網域是`msg.xboxlive.com`。

  * [註解](#ID4EV)
  * [URI 參數](#ID4EAB)
  * [Authorization](#ID4ENB)
  * [在資源上的隱私權設定的效果](#ID4EYB)
  * [要求本文](#ID4E3F)
  * [HTTP 狀態碼](#ID4ETCAC)
  * [回應主體](#ID4E1EAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>備註

此 API 支援只包含內容類型為"application/json"，都需要在每個呼叫的 HTTP 標頭。

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| xuid | 不帶正負號的 64 位元整數 | Xbox 使用者識別碼 (XUID) 正提出要求者的播放程式。 |

<a id="ID4ENB"></a>


## <a name="authorization"></a>Authorization

您必須擁有您自己的使用者宣告和有效的金級訂用帳戶，以將使用者訊息傳送。

<a id="ID4EYB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>在資源上的隱私權設定的效果

已成功將使用者訊息傳送至播放程式，該播放程式朋友不是是，是否會導致結果碼 200。 不過，如果您將訊息傳送至已封鎖您的任何人時，收件者將不會收到訊息，而且您不會收到任何資訊指出您的訊息未成功。

另外還有限制上多少訊息可以傳送每日和幾個朋友和非朋友，如下所示。

   * 每個訊息的 20 陌生人
   * 每 24 小時 200 陌生人
   * 每 24 小時的 250 總訊息
   * 介於 2500年總計每 24 小時內的收件者

| 提出要求的使用者| 目標使用者的隱私權設定| 行為|
| --- | --- | --- | --- | --- | --- |
| 我| -| 如所述。|
| friend| 每個人| 200 OK|
| friend| 只有朋友| 200 OK|
| friend| 封鎖| 200 OK|
| 非 friend 使用者| 每個人| 200 OK|
| 非 friend 使用者| 只有朋友| 200 OK|
| 非 friend 使用者| 封鎖| 200 OK|
| 協力廠商網站| 每個人| 200 OK|
| 協力廠商網站| 只有朋友| 200 OK|
| 協力廠商網站| 封鎖| 200 OK|

<a id="ID4E3F"></a>


## <a name="request-body"></a>要求本文

| 屬性| 類型| 最大長度| 取用者| 備註|
| --- | --- | --- | --- | --- |
| header| 標頭|  | 全部| 使用者訊息標頭|
| messageText| 字串| 250| Windows 8 除外的所有平台| 使用者的訊息文字 (utf-8)|

#### <a name="header"></a>標頭

| 屬性| 類型| 最大長度| 取用者| 備註|
| --- | --- | --- | --- | --- |
| 收件者| User[]| 20| 全部| 訊息收件者清單|

#### <a name="user"></a>使用者

| 屬性| 類型| 最大長度| 取用者| 備註|
| --- | --- | --- | --- | --- |
| xuid| ulong|  | 全部| 收件者 XUID。 如果傳送的玩家代號，不使用。|
| 玩家代號| 字串| 15| 全部| 收件者的玩家代號。 如果在傳送 XUID，不使用。|

#### <a name="sample-request-body"></a>範例要求主體 

```cpp
{
          "header":
          {
            "recipients":
            [{"gamertag":"GoTeamEmily"},
            {"gamertag":"Longstreet360"}]
          },
          "messageText":"random user text"
        }

```


<a id="ID4ETCAC"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。

| 程式碼| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 成功。|
| 400| 收件者清單是空的或超過最大長度。指定的玩家代號和 XUID; 或或 messageText 太長。|
| 403| 無法轉換 XUID。|
| 404| 玩家代號無效或找不到使用者。|
| 409| 使用者已達每日系統所加諸的限制。|
| 500| 一般的伺服器端錯誤。|

<a id="ID4E1EAC"></a>


## <a name="response-body"></a>回應主體

回應主體會不傳送任何物件。

<a id="ID4EJFAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4ELFAC"></a>


##### <a name="parent"></a>父系  

[/users/xuid({xuid})/outbox](uri-usersxuidoutbox.md)


<a id="ID4EZFAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>參考[標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)
