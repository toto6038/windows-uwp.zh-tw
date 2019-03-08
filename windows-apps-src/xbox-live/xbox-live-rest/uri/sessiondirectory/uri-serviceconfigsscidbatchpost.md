---
title: POST (/serviceconfigs/{scid}/batch)
assetID: b821a6eb-1add-ef91-bdf5-10e107082197
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidbatchpost.html
description: " POST (/serviceconfigs/{scid}/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e718fda26264667a7bf08c9254a462eb268dcd74
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603443"
---
# <a name="post-serviceconfigsscidbatch"></a>POST (/serviceconfigs/{scid}/batch)
建立批次查詢中，於多個 Xbox 使用者識別碼的服務組態。

> [!IMPORTANT]
> 這個方法由 2015年多人遊戲，並只適用於該多人遊戲的版本和更新版本。 它適合與範本合約 104/105 或更新版本，搭配使用，而且需要 X Xbl-合約版本標頭項目：104/105 或更新版本上的每個要求。

  * [註解](#ID4ET)
  * [URI 參數](#ID4ELB)
  * [查詢字串參數](#ID4EVB)
  * [HTTP 狀態碼](#ID4EGF)
  * [要求本文](#ID4ENF)
  * [回應主體](#ID4EWF)

<a id="ID4ET"></a>


## <a name="remarks"></a>備註

此 HTTP/REST 方法會建立批次查詢，於多個 Xbox 使用者識別碼在服務組態識別碼 (SCID) 層級。 這個方法可以前後加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync**。

如 2015年多人遊戲，您可以結合許多查詢的單一呼叫所有查詢都都相同，但是如果處理不同的 Xbox 使用者識別碼 (XUIDs)，所示*xuid*查詢字串參數。 請注意，查詢字串參數，且回應，一般查詢和批次查詢相同。

批次查詢中，屬於每個 XUID 的工作階段會寫入至回應的順序相同*xuid*參數會在要求中。 就可能出現一次在每個在回應中，多次相同的工作階段*xuid* ，它會比對。

<a id="ID4ELB"></a>


## <a name="uri-parameters"></a>URI 參數

| 參數| 類型| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服務設定識別項 (SCID)。 工作階段識別項的第 1 部分。|

<a id="ID4EVB"></a>


## <a name="query-string-parameters"></a>查詢字串參數

查詢可以使用下表中的查詢字串參數來加以修改。

| <b>參數</b>| <b>類型</b>| <b>描述</b>|
| --- | --- | --- | --- | --- | --- | --- |
| 關鍵字| 字串| 關鍵字，例如，"foo"，a，必須出現在工作階段或範本中所要擷取。 |
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼，例如，"123"，包含在查詢中的工作階段。 根據預設，使用者必須是作用中要包含的工作階段。 |
| 保留項目| 布林值| True 表示要包含工作階段的使用者會設定為保留的播放程式，但尚未加入成為作用中的播放程式。 當查詢自己的工作階段或查詢特定使用者的工作階段伺服器對伺服器時，才使用這個參數。 |
| 非使用中| 布林值| 包括使用者已接受，但不正在播放的工作階段，則為 true。 使用者是 「 準備 」，但不是 「 作用中 」 的工作階段計數為非作用中。 |
| 私用| 布林值| 如果為 true，則包括私用的工作階段。 當查詢自己的工作階段或查詢特定使用者的工作階段伺服器對伺服器時，才使用這個參數。 |
| 可見度| 字串| 工作階段的可見性狀態。 可能的值由定義<b>MultiplayerSessionVisibility</b>。 如果此參數設定為 「 開啟 」 時，查詢應該包含只開啟的工作階段。 如果設定為 「 私人 」，<i>私人</i>參數必須設定為 true。 |
| version| 32 位元帶正負號的整數| 應該會包含最大工作階段版本。 例如，值為 2 指定 2 的主要工作階段版本的唯一工作階段，或小於應該包含。 版本號碼必須是小於或等於要求的合約版本 mod 100。 |
| Take| 32 位元帶正負號的整數| 若要擷取的工作階段數目上限。 此數目必須介於 0 到 100 之間。 |


設定*私人*或是*保留*來為 true，則需要伺服器層級存取工作階段。 或者，這些設定會要求呼叫端的 XUID 宣告以符合 XUID 篩選器，在 URI 中。 否則，傳回的 HTTP/403 狀態碼，實際存在的任何這類工作階段。

<a id="ID4EGF"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼
適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4ENF"></a>


## <a name="request-body"></a>要求本文


```cpp
{ "xuids": [ "1234", "5678" ] }

```


<a id="ID4EWF"></a>


## <a name="response-body"></a>回應主體

從這個方法傳回為工作階段的參考，包含某些工作階段資料包含內嵌的 JSON 陣列。


```cpp
{
    "results": [ {
      "xuid": "9876",    // If the session was found from a xuid, that xuid.
      "startTime": "2009-06-15T13:45:30.0900000Z",
      "sessionRef": {
        "scid": "foo",
        "templateName": "bar",
        "name": "session-seven"
      },
      "accepted": 4,    // Approximate number of non-reserved members.
      "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
      "visibility": "open",    // or "private", "visible", or "full"
      "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
      "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
      "keywords": [ "one", "two" ]
    }
  ]
}

```


<a id="ID4EDG"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EFG"></a>


##### <a name="parent"></a>父系

[/serviceconfigs/{scid}/batch](uri-serviceconfigsscidbatch.md)
