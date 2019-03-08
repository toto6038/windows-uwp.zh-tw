---
title: GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
assetID: 942cf0d7-f988-0495-cf28-cdac608b8109
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeopleget.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 674e52d15d115560d4813edcd9687c2fe9694c9b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650763"
---
# <a name="get-usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
傳回的統計資料值 （分數） 是已知的所有連絡人，目前的使用者或指定為最愛的人的該使用者的連絡人的排名社交排行榜。
這些 Uri 的網域是`leaderboards.xboxlive.com`。

  * [註解](#ID4EV)
  * [URI 參數](#ID4EAB)
  * [查詢字串參數](#ID4ELB)
  * [Authorization](#ID4EQD)
  * [必要的要求標頭](#ID4EGE)
  * [選擇性的要求標頭](#ID4EXF)
  * [要求本文](#ID4ETG)
  * [HTTP 狀態碼](#ID4ECEAC)
  * [必要的回應標頭](#ID4E1HAC)
  * [選擇性的回應標頭](#ID4EDJAC)
  * [回應主體](#ID4EDKAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>備註

排行榜 Api 都是唯讀且因此 （透過 HTTPS) 僅支援 GET 動詞命令。 它們會反映排名及排序 「 頁面 」 索引的播放程式統計資料衍生自透過資料平台撰寫個別的使用者統計資料。 完整的排行榜索引可以是相當大，並呼叫者絕對不會要求以查看完整的其中一個，因此這個 URI 都支援數個查詢字串引數可讓呼叫者必須是特定它想要查看針對該排行榜 檢視的類型。

GET 作業不會修改任何資源，因此如果執行一次或多次，這會產生相同的結果。

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| xuid| 字串| 使用者的識別碼。|
| scid| 字串| 服務組態，其中包含所存取之資源的識別碼。|
| statname| 字串| 正在存取的使用者統計資料資源的唯一識別碼。|
| 全部|我的最愛| 列舉| 是否要排名的統計資料的值 （分數），目前使用者的所有已知的連絡人，或指定為最愛的人的該使用者的連絡人。|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>查詢字串參數

| 參數| 類型| 描述|
| --- | --- | --- | --- | --- | --- |
| maxItems| 32 位元不帶正負號的整數| 要傳回的結果頁中的排行榜記錄的最大數目。 如果未指定，預設會傳回數目 (10)。 MaxItems 的最大值是仍未定義，但我們想要避免大型資料集，因此這個值應可能目標的最大集 UI 可以處理每次呼叫的調諧器。 |
| skipToRank| 32 位元不帶正負號的整數| 傳回從指定的排行榜陣序的結果頁面。 其餘的結果會依照順位排序順序。 這個查詢字串可以傳回的接續權杖可以回後續的查詢，以取得 「 下一頁 」 的結果。 |
| skipToUser| 字串| 傳回指定的玩家 xuid，無論該使用者的陣序規範或分數周圍的排行榜結果頁面。 頁面會依據百分位數陣序規範，與指定之使用者的頁面的預先定義的檢視，最後一個位置或 stat 排行榜檢視中間進行排序。 沒有任何<b>continuationToken</b>提供這種類型的查詢。 |
| continuationToken| 字串| 如果先前的呼叫傳回<b>continuationToken</b>，則呼叫端可以傳遞回 「 現狀 」 該權杖來取得下一頁結果的查詢字串中。 |
| sort| 字串| 指定是否要排名的玩家從低到高值順序 （「 遞增 」） 清單，或高-低值 （「 遞減 」） 的順序。 這是選擇性的參數;預設值遞減順序。|

<a id="ID4EQD"></a>


## <a name="authorization"></a>Authorization

需要 Xuid 授權。

內容隔離和存取控制案例的目的，被實作授權邏輯。

排行榜和使用者的統計資料可以讀取來自任何平台上的用戶端，前提是呼叫者提交有效的 XSTS 語彙基元，其要求。 寫入受限於 （顯然） 支援的資料平台的用戶端。

標題的開發人員可以標示為開啟或限制 XDP 或合作夥伴中心的統計資料。 排行榜是開啟的統計資料。 開啟的統計資料可以存取 Smartclass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 應用程式，只要在沙箱授權的使用者。 對沙箱的使用者授權是透過 XDP 或合作夥伴中心管理。

<a id="ID4EGE"></a>


## <a name="required-request-headers"></a>必要的要求標頭

| 標頭| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| 字串。 HTTP 驗證的驗證認證。 範例值：「 XBL3.0 x =&lt;userhash >;&lt;權杖 > 」。|
| Content-Type| 字串。 要求主體的 MIME 類型。 範例值:"application/json"。|
| X-RequestedServiceVersion| 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 預設值：1.|
| Accept| 字串。 可接受的內容類型值。 範例值:"application/json"。|

<a id="ID4EXF"></a>


## <a name="optional-request-headers"></a>選擇性的要求標頭

| 標頭| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| If-None-Match| 字串。 實體標記-如果用戶端支援快取使用。 範例值：「 686897696a7c876b7e"。|

<a id="ID4ETG"></a>


## <a name="request-body"></a>要求本文

為了充分發揮任何呼叫端能夠了解正確地顯示會重新取得它的資料，每個使用者每個狀態的值將會以傳回的格式中應該顯示，並以符合指定的 accept 語言的地區設定格式化的字串在要求中的標頭。 當地語系化 「 顯示名稱 」 也會針對該排行榜 statname 傳回。

<a id="ID4E2G"></a>


### <a name="required-members"></a>必要的成員

| 成員| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <b>pagingInfo</b>| 區段| 選用。 低於 totalItems 陣序規範的頁面中的最後一個項目時傳回。 本章節也不會傳回在要求中指定 skipToUser 時。|
| continuationToken| 字串| 必要。 指定項目值設回"continuationToken"查詢參數，如有需要，針對該 URI 取得下一頁結果摘要。 如果傳回沒有 pagingInfo 就是要取得的資料沒有 「 下一步 頁 」。|
| totalItems| 64 位元不帶正負號的整數| 必要。 在排行榜中的項目總數。|
| <b>leaderboardInfo</b>| 區段| 必要。 一律傳回。 包含要求排行榜的相關中繼資料。|
| displayName| 字串| 必要。 預先定義的排行榜的當地語系化的顯示名稱。 範例值：所有擷取的旗標 」。|
| totalCount| 字串| 必要。 在排行榜中的項目總數。|
| 資料行| 陣列| 必要。|
| displayName| 字串| 必要。 對應至在排行榜中的資料行。|
| statName| 字串| 必要。 對應至在排行榜中的資料行。|
| type| 字串| 必要。 對應至在排行榜中的資料行。|
| <b>userList</b>| 區段| 必要。 一律傳回。 包含要求排行榜的實際使用者分數。|
| 玩家代號| 字串| 必要。 對應的排行榜項目中的使用者。|
| xuid| 32 位元帶正負號的整數| 必要。 對應的排行榜項目中的使用者。|
| 百分位數| 字串| 必要。 對應的排行榜項目中的使用者。|
| 陣序規範| 字串| 必要。 對應的排行榜項目中的使用者。|
| 值| 陣列| 必要。 每個以逗號分隔的值會對應至在排行榜中的資料行。|

<a id="ID4ECEAC"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。

| 程式碼| 原因片語| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 確定| 已成功擷取工作階段。|
| 304| 未修改|  |
| 400| 錯誤的要求| | 服務無法辨識格式不正確的要求。 通常是無效的參數。|
| 401| 未經授權| | 要求需要使用者驗證。|
| 403| 已禁止| 要求不允許使用者或服務。|
| 404| 找不到| 找不到指定的資源。|
| 406| 無法接受| 不支援資源版本;應該會遭到拒絕，由 MVC 層。|
| 408| 要求逾時| 服務無法辨識格式不正確的要求。 通常是無效的參數。|

<a id="ID4E1HAC"></a>


## <a name="required-response-headers"></a>必要的回應標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 字串| 回應主體的 mime 類型。 範例值:"application/json"。|
| Content-Length| 字串| 在回應中傳送的位元組數目。 範例值："232".|

<a id="ID4EDJAC"></a>


## <a name="optional-response-headers"></a>選擇性的回應標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| 字串| 用於快取最佳化。 範例值：「 686897696a7c876b7e"。|

<a id="ID4EDKAC"></a>


## <a name="response-body"></a>回應主體

社交排行榜，任何分頁的要求：

https://leaderboards.xboxlive.com/users/xuid(2533274916402282)/scids/c1ba92a9-0000-0000-0000-000000000000/stats/EnemyDefeats/people/all?sort=descending

<a id="ID4ENKAC"></a>


### <a name="sample-response"></a>範例回應


```cpp
{
    "pagingInfo": null,
    "leaderboardInfo": {
        "displayName": "Kills",
        "totalCount": 3,
        "columns": [
            {
                "displayName": "Kills",
                "statName": "enemydefeats",
                "type": "integer"
            }
        ]
    },
    "userList": [
        {
            "gamertag":"xxxSniper39",
            "xuid":"1234567890123555",
            "percentile":1.0,
            "rank":1,
            "values": [
                "47"
            ]
        },
        {
            "gamertag":"WarriorSaint",
            "xuid":"2533274916402282",
            "percentile":0.66,
            "rank":2,
            "values": [
                "42"
            ]
        },
        {
            "gamertag":"WhockaWhocka",
            "xuid":"1234567890123666",
            "percentile":0.33,
            "rank":3,
            "values": [
                "12"
            ]
        }
    ]
}

```


<a id="ID4EXKAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EZKAC"></a>


##### <a name="parent"></a>父系

[/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite}](uri-usersxuidscidstatnamepeople.md)
