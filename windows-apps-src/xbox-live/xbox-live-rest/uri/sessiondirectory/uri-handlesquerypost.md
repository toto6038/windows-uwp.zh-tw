---
title: POST (/handles/query)
assetID: a1a47d49-5c3f-8021-a213-13eb8bddf16a
permalink: en-us/docs/xboxlive/rest/uri-handlesquerypost.html
description: " POST (/handles/query)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7540c117931c01c24c79cef6c8ab6540cb65bbcb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651503"
---
# <a name="post-handlesquery"></a>POST (/handles/query)
建立工作階段控制代碼的查詢。

> [!IMPORTANT]
> 這個方法由 2015年多人遊戲，並只適用於該多人遊戲的版本和更新版本。 它適合與範本合約 104/105 或更新版本，搭配使用，而且需要 X Xbl-合約版本標頭項目：104/105 或更新版本上的每個要求。

  * [註解](#ID4ET)
  * [URI 參數](#ID4EDB)
  * [查詢字串參數](#ID4EQB)
  * [HTTP 狀態碼](#ID4EBF)
  * [要求本文](#ID4EIF)
  * [回應主體](#ID4ETF)

<a id="ID4ET"></a>


## <a name="remarks"></a>備註

此 HTTP/REST 方法會建立只處理資料的查詢，但不含任何工作階段資訊。 它可以藉由包裝**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetActivitiesForSocialGroupAsync**。

在這個方法的要求主體中的 [類型] 欄位必須設定為 「 活動 」。 回應主體中的項目會直接對應到的屬性**MultiplayerActivityDetails**。

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI 參數

<a id="ID4EQB"></a>


## <a name="query-string-parameters"></a>查詢字串參數

查詢可以使用下表中的查詢字串參數來加以修改。

| <b>參數</b>| <b>類型</b>| <b>描述</b>|
| --- | --- | --- | --- |
| 關鍵字| 字串| 關鍵字，例如，"foo"，a，必須出現在工作階段或範本中所要擷取。 |
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼，例如，"123"，包含在查詢中的工作階段。 根據預設，使用者必須是作用中要包含的工作階段。 |
| 保留項目| 布林值| True 表示要包含工作階段的使用者會設定為保留的播放程式，但尚未加入成為作用中的播放程式。 當查詢自己的工作階段或查詢特定使用者的工作階段伺服器對伺服器時，才使用這個參數。 |
| 非使用中| 布林值| 包括使用者已接受，但不正在播放的工作階段，則為 true。 使用者是 「 準備 」，但不是 「 作用中 」 的工作階段計數為非作用中。 |
| 私用| 布林值| 如果為 true，則包括私用的工作階段。 當查詢自己的工作階段或查詢特定使用者的工作階段伺服器對伺服器時，才使用這個參數。 |
| 可見度| 字串| 工作階段的可見性狀態。 可能的值由定義<b>MultiplayerSessionVisibility</b>。 如果此參數設定為 「 開啟 」 時，查詢應該包含只開啟的工作階段。 如果設定為 「 私人 」，<i>私人</i>參數必須設定為 true。 |
| version| 32 位元帶正負號的整數| 應該會包含最大工作階段版本。 例如，值為 2 指定 2 的主要工作階段版本的唯一工作階段，或小於應該包含。 版本號碼必須是小於或等於要求的合約版本 mod 100。 |
| Take| 32 位元帶正負號的整數| 若要擷取的工作階段數目上限。 此數目必須介於 0 到 100 之間。 |


設定*私人*或是*保留*來為 true，則需要伺服器層級存取工作階段。 或者，這些設定會要求呼叫端的 XUID 宣告以符合 XUID 篩選器，在 URI 中。 否則，傳回的 HTTP/403 狀態碼，實際存在的任何這類工作階段。

<a id="ID4EBF"></a>


## <a name="http-status-codes"></a>HTTP 狀態碼
適用於 MPSD，服務會傳回 HTTP 狀態碼。  
<a id="ID4EIF"></a>


## <a name="request-body"></a>要求本文

使用者的 [我的最愛] 社交圖形的所有活動，要求本文看起來像這樣：


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


<a id="ID4ETF"></a>


## <a name="response-body"></a>回應主體

控制代碼會擷取成控制代碼結構的陣列，與每個結構中內嵌的唯一識別碼。


```cpp
{
  "results": [
    {
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
    }
  }]
}

```


<a id="ID4E4F"></a>


## <a name="see-also"></a>請參閱

<a id="ID4E6F"></a>


##### <a name="parent"></a>父系

[/handles/query](uri-handlesquery.md)
