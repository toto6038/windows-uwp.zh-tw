---
title: GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
assetID: 890e3f93-4fdc-955f-d849-ba9579d5c1eb
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsgetvaluemetadata.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 55ad44d4c29a2d7a43c76c4df2a78e08462fa65f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612713"
---
# <a name="get-usersxuidxuidscidsscidstatsincludevaluemetadata"></a>GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
取得一份指定的統計資料，包括統計資料值，指定的服務組態中的使用者相關聯的中繼資料。
這些 Uri 的網域是`userstats.xboxlive.com`。

  * [註解](#ID4EV)
  * [URI 參數](#ID4EAB)
  * [查詢字串參數](#ID4ELB)
  * [Authorization](#ID4EWC)
  * [必要的要求標頭](#ID4ERD)
  * [選擇性的要求標頭](#ID4EDF)
  * [要求本文](#ID4EHG)
  * [HTTP 狀態碼](#ID4ESG)
  * [回應主體](#ID4EJCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>備註

？ 包括 = valuemetadata 查詢參數可包含任何使用者統計資料值，例如用來達成的比賽追蹤上一次一輛車的色彩與模型相關聯的中繼資料的回應。

若要在回應中包含值的中繼資料，要求呼叫必須也設定 X Xbl-合約版本標頭值為 3。

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- |
| xuid| GUID| Xbox 使用者識別碼 (XUID) 代表其存取的服務組態上的使用者。|
| scid| GUID| 服務組態，其中包含所存取之資源的識別碼。|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>查詢字串參數

| 參數| 類型| 描述|
| --- | --- | --- | --- | --- | --- |
| statNames| 字串| 以逗號分隔的使用者統計資料名稱的清單。例如，下列 URI 會通知服務要求四個統計資料時，所代表之 URI 中指定的使用者識別碼。{:: nomakrdown}<br/><br/>`https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots?include=valuemetadata`| 
| include=valuemetadata| 字串| 指出回應包含任何值中繼資料，請使用 stat 值相關聯。|

<a id="ID4EWC"></a>


## <a name="authorization"></a>Authorization

沒有內容隔離和存取控制案例實作授權邏輯。

   * 排行榜和使用者的統計資料可以讀取來自任何平台上的用戶端，前提是呼叫者送出要求的有效 XSTS 語彙基元。 寫入受限於資料平台所支援的用戶端。
   * 標題的開發人員可以標示為開啟或限制 XDP 或合作夥伴中心的統計資料。 排行榜是開啟的統計資料。 開啟的統計資料可以存取 Smartclass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 應用程式，只要在沙箱授權的使用者。 對沙箱的使用者授權是透過 XDP 或合作夥伴中心管理。

檢查虛擬程式碼看起來像這樣：


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4ERD"></a>


## <a name="required-request-headers"></a>必要的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| 字串| HTTP 驗證的驗證認證。 範例值：「 XBL3.0 x =&lt;userhash >;&lt;權杖 > 」。|
| X-Xbl-Contract-Version| 字串| 表示要使用的 API 版本。 此值必須設定"3"，才能在回應中包含值的中繼資料。|

<a id="ID4EDF"></a>


## <a name="optional-request-headers"></a>選擇性的要求標頭

| 標頭| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | 名稱/的組建編號應該導向此要求的服務。 要求將只會路由至該服務之後驗證標頭，而在驗證權杖中，宣告的有效性，並依此類推。 預設值：1.|

<a id="ID4EHG"></a>


## <a name="request-body"></a>要求本文

此要求主體中不傳送任何物件。

<a id="ID4ESG"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼

服務會傳回狀態碼的其中一個以回應要求，使用此方法，此資源上進行這一節。 Xbox Live 服務搭配使用的標準 HTTP 狀態碼的完整清單，請參閱 <<c0> [ 標準 HTTP 狀態碼](../../additional/httpstatuscodes.md)。

| 程式碼| 原因片語| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 確定| 已成功擷取工作階段。|
| 304| 未修改| 資源尚未經過修改自上次要求。|
| 400| 錯誤的要求| 服務無法辨識格式不正確的要求。 通常是無效的參數。|
| 401| 未經授權| 要求需要使用者驗證。|
| 403| 已禁止| 要求不允許使用者或服務。|
| 404| 找不到| 找不到指定的資源。|
| 406| 無法接受| 不支援資源版本。|
| 408| 要求逾時| 不支援資源版本;應該會遭到拒絕，由 MVC 層。|

<a id="ID4EJCAC"></a>


## <a name="response-body"></a>回應主體

<a id="ID4EPCAC"></a>


### <a name="sample-response"></a>範例回應


```cpp
{
  "user": {
    "xuid": "123456789",
    "gamertag": "WarriorSaint",
    "stats": [
      {
        "statname": "Wins",
        "type": "Integer",
        "value": 40,
        "valuemetadata" : "{\"region\" : \"EU\", \"isRanked\" : true}"
      },
      {
        "statname": "Kills",
        "type": "Integer",
        "value": 700,
        "valuemetadata" : "{\"longestKillStreak" : 15, \"favoriteTarget\" : \"CrazyPigeon\"}"
      },
      {
        "statname": "KDRatio",
        "type": "Double",
        "value": 2.23,
        "valuemetadata" : "{\"totalKills\" : 700, \"totalDeaths\" : 314}"
      },
      {
        "statname": "Headshots",
        "type": "Integer",
        "value": 173,
        "valuemetadata" : ""
      }
    ],
  }
}

```


<a id="ID4EZCAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4E2CAC"></a>


##### <a name="parent"></a>父系

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
