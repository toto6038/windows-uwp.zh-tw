---
title: GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)
assetID: ee69a9e3-ea91-3cf5-a10a-811762cba892
permalink: en-us/docs/xboxlive/rest/uri-scidsscidleaderboardsleaderboardnamegetvaluemetadata.html
description: " GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fad57c5e989d933777c913030faaa594c6bbd059
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623433"
---
# <a name="get-scidsscidleaderboardsleaderboardnameincludevaluemetadata"></a>GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)
 
取得的預先定義的全球排行榜的排行榜值相關聯的任何中繼資料。
 
這些 Uri 的網域是`leaderboards.xboxlive.com`。
 
  * [註解](#ID4EY)
  * [URI 參數](#ID4EHB)
  * [查詢字串參數](#ID4ESB)
  * [Authorization](#ID4EVD)
  * [在資源上的隱私權設定的效果](#ID4EQE)
  * [必要的要求標頭](#ID4EZE)
  * [選擇性的要求標頭](#ID4EOG)
  * [HTTP 狀態碼](#ID4EOH)
  * [回應標頭](#ID4EFDAC)
  * [回應主體](#ID4ECFAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>備註
 
？ 包括 = valuemetadata 查詢參數可包含任何與排行榜值相關聯的中繼資料的回應。 值的中繼資料包含用戶端指定的值，例如用來達成最佳的時間在競爭播放軌上一輛車的色彩與模型相關聯的資料。
 
值的中繼資料被定義使用者狀態，排行榜為基礎，不能在排行榜本身。
 
排行榜 Api 都是唯讀且因此僅支援 GET 動詞命令。 它們會反映排名及排序 「 頁面 」 索引的播放程式統計資料衍生自透過資料平台撰寫個別的使用者統計資料。 完整的排行榜索引可以是相當大，並呼叫者絕對不會要求以查看完整的其中一個，因此這個 URI 都支援數個查詢字串引數可讓呼叫者必須是特定它想要查看針對該排行榜 檢視的類型。
 
GET 作業不會修改任何資源，因此如果執行一次或多次，這會產生相同的結果。
  
<a id="ID4EHB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| scid| GUID| 服務組態，其中包含所存取之資源的識別項。| 
| leaderboardname| 字串| 所存取之預先定義的排行榜資源的唯一識別碼。| 
  
<a id="ID4ESB"></a>

 
## <a name="query-string-parameters"></a>查詢字串參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | 
| include=valuemetadata| 字串| 指出回應包含任何值的中繼資料與排行榜值相關聯。| 
| maxItems| 32 位元不帶正負號的整數| 要傳回的結果頁中的排行榜記錄的最大數目。 如果未指定，預設會傳回數目 (10)。 MaxItems 的最大值是仍未定義，但我們想要避免大型資料集，因此這個值應可能目標的最大集 UI 可以處理每次呼叫的調諧器。| 
| skipToRank| 32 位元不帶正負號的整數| 傳回從指定的排行榜陣序的結果頁面。 其餘的結果會依照順位排序順序。 這個查詢字串可以傳回的接續權杖可以回後續的查詢，以取得 「 下一頁 」 的結果。| 
| skipToUser| 字串| 傳回指定的玩家 xuid，無論該使用者的陣序規範或分數周圍的排行榜結果頁面。 頁面會依據百分位數陣序規範，與指定之使用者的頁面的預先定義的檢視，最後一個位置或 stat 排行榜檢視中間進行排序。 提供這種類型的查詢沒有 continuationToken。| 
| continuationToken| 字串| 如果先前的呼叫傳回 continuationToken，然後呼叫端可以傳遞回 「 現狀 」 該權杖來取得下一頁結果的查詢字串中。| 
  
<a id="ID4EVD"></a>

 
## <a name="authorization"></a>Authorization
 
沒有內容隔離和存取控制案例實作授權邏輯。
 
   * 排行榜和使用者的統計資料可以讀取來自任何平台上的用戶端，前提是呼叫者送出要求的有效 XSTS 語彙基元。 寫入受限於顯然資料平台所支援的用戶端。
   * 標題的開發人員可以標示為開啟或限制 XDP 或合作夥伴中心的統計資料。 排行榜是開啟的統計資料。 開啟的統計資料可以存取 Smartclass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 應用程式，只要在沙箱授權的使用者。 對沙箱的使用者授權是透過 XDP 或合作夥伴中心管理。
  
檢查虛擬程式碼看起來像這樣：
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4EQE"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>在資源上的隱私權設定的效果
 
讀取排行榜資料時，會不執行任何隱私權檢查。
  
<a id="ID4EZE"></a>

 
## <a name="required-request-headers"></a>必要的要求標頭
 
| 標頭| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| 字串。 HTTP 驗證的驗證認證。 範例值：<b>XBL3.0 x=&lt;userhash>;&lt;token></b>| 
| X-Xbl-Contract-Version| 字串。 表示要使用的 API 版本。 此值必須設定"3"，才能在回應中包含值的中繼資料。| 
| X-RequestedServiceVersion| 字串。 建置此要求應導向的 Xbox LIVE 服務的名稱/號碼。 要求只會路由至該服務中，但會確認有效性的標頭、 驗證權杖等等的宣告之後。預設值：1.| 
| Accept| 字串。 可接受的內容類型。 範例值： <b>application/json</b>| 
  
<a id="ID4EOG"></a>

 
## <a name="optional-request-headers"></a>選擇性的要求標頭
 
| 標頭| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-None-Match| 字串。 如果用戶端支援快取使用的實體標記。 範例值：<b>"686897696a7c876b7e"</b>|  | 
  
<a id="ID4EOH"></a>

 
## <a name="http-status-codes"></a>HTTP 狀態碼
 
服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。
 
| 程式碼| 原因片語| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 確定| 已成功擷取工作階段。| 
| 304| 未修改| 資源尚未經過修改自上次要求。| 
| 400| 錯誤的要求| 服務無法辨識格式不正確的要求。 通常是無效的參數。| 
| 401| 未經授權| 要求需要使用者驗證。| 
| 403| 已禁止| 要求不允許使用者或服務。| 
| 404| 找不到| 找不到指定的資源。| 
| 406| 無法接受| 不支援資源版本。| 
| 408| 要求逾時| 不支援資源版本;應該會遭到拒絕，由 MVC 層。| 
  
<a id="ID4EFDAC"></a>

 
## <a name="response-headers"></a>回應標頭
 
| 標頭| 類型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| 字串| 必要。 回應主體的 MIME 類型。 範例： <b>application/json</b>。| 
| Content-Length| 字串| 必要。 在回應中傳送的位元組數目。 範例：<b>232</b>.| 
| ETag| 字串| 選用。 用於快取最佳化。 範例：<b>686897696a7c876b7e</b>。| 
  
<a id="ID4ECFAC"></a>

 
## <a name="response-body"></a>回應主體
 
<a id="ID4EIFAC"></a>

 
### <a name="response-members"></a>回應成員
 
pagingInfo | pagingInfo| 區段| 選用。 MaxItems 要求中指定時，只呈現。| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| continuationToken| 64 位元不帶正負號的整數| 必要。 指定項目值設回送入<b>skipItems</b>查詢結果的下一個頁面取得該 URI，如有需要的參數。 如果沒有<b>pagingInfo</b>傳回，則要取得的資料沒有下一個頁面。| 
| totalItems| 64 位元不帶正負號的整數| 必要。 在排行榜中的項目總數。 範例值：1234567890| 
 
leaderboardInfo | leaderboardInfo| 區段| 必要。 一律傳回。 包含要求排行榜的相關中繼資料。| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| displayName| 字串| 不使用。| 
| totalCount| 字串 （64 位元不帶正負號的整數）| 必要。 在排行榜中的項目總數。 範例值：1234567890| 
| columnDefinition| JSON 物件| 必要。 描述在排行榜中以資料行。| 
| columnDefinition.displayName| 字串| 必要。 在排行榜中以資料行的描述性名稱。| 
| columnDefinition.statName| 字串| 必要。 排行榜根據使用者狀態的名稱。| 
| columnDefinition.type| 字串| 必要。 使用者狀態資料行中的資料型別。| 
 
userList | userList| 區段| 必要。 一律傳回。 包含要求排行榜的實際使用者分數。| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 玩家代號| 字串| 必要。 使用者排行榜項目中的玩家代號。| 
| xuid| 64 位元不帶正負號的整數| 必要。 Xbox 使用者排行榜項目中的使用者識別碼。| 
| 百分位數| 字串| 必要。 在排行榜中以使用者的百分位數陣序。| 
| 陣序規範| 字串| 必要。 在排行榜中以使用者的數字順位。| 
| value| 字串| 必要。 使用者排行榜所依據的統計資料值。| 
| valueMetadata| 字串| 必要。 一串逗號分隔格式的字串組 」\"名稱\":\"值\"，..."

如果沒有任何中繼資料，這個值會是空字串。 | 
  
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>範例回應
 
下列要求 URI 會描述正在略過以陣序規範上的全球排行榜。
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/TotalFlagsCaptured?include=valuemetadata&maxItems=3&skipToRank=15000
         
```

 
為了傳回值的中繼資料，也必須指定下列標頭。
 

```cpp
X-Xbl-Contract-Version : 3
          
```

 
上述 URI 會傳回下列 JSON 回應。
 

```cpp
{
    "pagingInfo": {
        "continuationToken": "15003",
        "totalItems": 23437
    },
    "leaderboardInfo": {
        "displayName": "Total flags captured",
        "totalCount": 23437,
        "columnDefinition" : 
            {
                "displayName": "Flags Captured",
                "statName": "flagscaptured",
                "type": "Integer"
            }
    },
    "userList": [
        {
            "gamertag": "WarriorSaint",
            "xuid": "1234567890123444",
            "percentile": 0.64,
            "rank": 15000,
            "value": "1002",
            "valuemetadata" : "{\"color\" : \"silver\",\"weight\" : 2000, \"israining\" : false}"
        },
        {
            "gamertag": "xxxSniper39",
            "xuid": "1234567890123555",
            "percentile": 0.64,
            "rank": 15001,
            "value": "993",
            "valuemetadata" : "{\"color\" : \"silver\",\"weight\" : 2020, \"israining\" : true}"
 
        },
        {
            "gamertag": "WhockaWhocka",
            "xuid": "1234567890123666",
            "percentile": 0.64,
            "rank": 15002,
            "value": "700",
            "valuemetadata" : "{\"color\" : \"red\",\"weight\" : 300, \"israining\" : false}"
        }
    ]
}
         
```

   
<a id="ID4E6LAC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EBMAC"></a>

 
##### <a name="parent"></a>父系 

[/scids/{scid}/leaderboards/{leaderboardname}](uri-scidsscidleaderboardsleaderboardname.md)

   