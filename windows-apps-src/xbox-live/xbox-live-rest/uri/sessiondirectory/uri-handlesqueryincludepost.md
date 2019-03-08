---
title: POST (/handles/query?include=relatedInfo)
assetID: 66ecd1fe-24d4-4cd5-256d-8950ac658529
permalink: en-us/docs/xboxlive/rest/uri-handlesqueryincludepost.html
description: " POST (/handles/query?include=relatedInfo)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eb30561c91a085902dd9d79b6c4a2045dc709bfb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632553"
---
# <a name="post-handlesqueryincluderelatedinfo"></a>POST (/handles/query?include=relatedInfo)
建立包含相關的工作階段資訊的工作階段控制代碼的查詢。

> [!IMPORTANT]
> 這個方法由 2015年多人遊戲，並只適用於該多人遊戲的版本和更新版本。 它適合與範本合約 104/105 或更新版本，搭配使用，而且需要 X Xbl-合約版本標頭項目：104/105 或更新版本上的每個要求。

  * [註解](#ID4ET)
  * [URI 參數](#ID4ECB)
  * [查詢字串參數](#ID4EPB)
  * [HTTP 狀態碼](#ID4EAF)
  * [要求本文](#ID4EHF)
  * [回應主體](#ID4EZF)

<a id="ID4ET"></a>


## <a name="remarks"></a>備註

這個 HTTP/REST 方法建立控制代碼資料查詢的"include"查詢字串中指定的工作階段資訊。 查詢字串值設計為可延伸以支援未來的查詢選項，支援以逗號分隔清單，例如，"？ 包括 = relatedInfo，工作階段 」。 POST 方法可以前後加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetActivitiesForUsersAsync**。

在這個方法的要求主體中的 [類型] 欄位必須設定為 「 活動 」。 回應主體中的項目會直接對應到的屬性**Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**。

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>URI 參數

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>查詢字串參數

查詢可以使用下表中的查詢字串參數來加以修改。

| <b>參數</b>| <b>類型</b>| <b>描述</b>|
| --- | --- | --- | --- |
| 關鍵字| 字串| 關鍵字，例如，"foo"，a，必須出現在工作階段或範本中所要擷取。 |
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼，例如，"123"，包含在查詢中的工作階段。 根據預設，使用者必須是作用中要包含的工作階段。 |
| 保留項目| 布林值| True 表示要包含工作階段的使用者會設定為保留的播放程式，但尚未加入成為作用中的播放程式。 當查詢自己的工作階段或查詢特定使用者的工作階段伺服器對伺服器時，才使用這個參數。 |
| 非使用中| 布林值| 包括使用者已接受，但不正在播放的工作階段，則為 true。 使用者是 「 準備 」，但不是 「 作用中 」 的工作階段計數為非作用中。 |
| 私用| 布林值| 如果為 true，則包括私用的工作階段。 當查詢自己的工作階段或查詢特定使用者的工作階段伺服器對伺服器時，才使用這個參數。 |
| 可見度| 字串| 工作階段的可見性狀態。 可能的值由定義<b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b>。 如果此參數設定為 「 開啟 」 時，查詢應該包含只開啟的工作階段。 如果設定為 「 私人 」，<i>私人</i>參數必須設定為 true。 |
| version| 32 位元帶正負號的整數| 應該會包含最大工作階段版本。 例如，值為 2 指定 2 的主要工作階段版本的唯一工作階段，或小於應該包含。 版本號碼必須是小於或等於要求的合約版本 mod 100。 |
| Take| 32 位元帶正負號的整數| 若要擷取的工作階段數目上限。 此數目必須介於 0 到 100 之間。 |


設定*私人*或是*保留*來為 true，則需要伺服器層級存取工作階段。 或者，這些設定會要求呼叫端的 XUID 宣告以符合 XUID 篩選器，在 URI 中。 否則，傳回的 HTTP/403 狀態碼，實際存在的任何這類工作階段。

<a id="ID4EAF"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼
適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4EHF"></a>


## <a name="request-body"></a>要求本文

<a id="ID4ENF"></a>


### <a name="sample-request"></a>範例要求

若要取得使用者的 [我的最愛] 社交圖形的所有活動，POST 本文看起來像這樣：


```cpp
{
  "type": "activity",
  "scid": "B5B1F71F-A328-4371-89E0-C3AD222D0E92"  // optional filter on scid
  "owners": {
     "people": {
       "moniker": "favorites",
       "monikerXuid": "3210"
     }
  }
}

```


<a id="ID4EZF"></a>


## <a name="response-body"></a>回應主體

使用內嵌在每個控制代碼的唯一識別碼做為陣列的控制代碼的結構，傳回的結果。 特定的工作階段資訊都會傳入**relatedInfo**物件。 請注意，這項資訊不會傳回一般的 POST 方法上此 URI。

<a id="ID4EDG"></a>


### <a name="sample-response"></a>範例回應


```cpp
{
    "results": [{
        "id": "11111111-ebe0-42da-885f-033860a818f6",
        "type": "activity",
        "version": 1,
        "sessionRef": {
            "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
            "templateName": "party",
            "name": "e3a836aeac6f4cbe9bcab985494d3175"
        },

    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
        "relatedInfo": {
            "visibility": "open",
            "joinRestriction": "followed",
            "closed": true,
            "maxMembersCount": 8,
            "membersCount": 4,
        }
    },
    {
        "id": "11111111-ebe0-42da-885f-033860a818f7",
        "type": "activity",
        "version": 1,
        "sessionRef": {
            "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
            "templateName": "TitleStorageTestDefault",
            "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
        },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
        "relatedInfo": {
            "visibility": "open",
            "joinRestriction": "followed",
            "closed": false,
            "maxMembersCount": 8,
            "membersCount": 4,
        }
    }]
}

```


<a id="ID4ENG"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EPG"></a>


##### <a name="parent"></a>父系

[/handles/query](uri-handlesquery.md)
