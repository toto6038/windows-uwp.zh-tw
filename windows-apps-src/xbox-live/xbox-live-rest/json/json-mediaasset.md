---
title: MediaAsset (JSON)
assetID: 858c720a-1648-738b-bb43-f050e7cac19e
permalink: en-us/docs/xboxlive/rest/json-mediaasset.html
description: " MediaAsset (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 308a65b7c049a6aba0405865bab63fb9d28b8506
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623473"
---
# <a name="mediaasset-json"></a>MediaAsset (JSON)
成就或照樣相關聯之媒體資產。
<a id="ID4EN"></a>


## <a name="mediaasset"></a>MediaAsset

MediaAsset 物件具有下列規格。

| 成員| 類型| 描述|
| --- | --- | --- |
| name| 字串| MediaAsset，例如"tile01 」 的名稱。|
| type| MediaAssetType 列舉| 媒體資產類型： <ul><li>圖示 (0):成就 圖示。</li><li>圖案 (1):數位作品資產。</li></ul> | 
| URL| 字串| MediaAsset URL。|

<a id="ID4EFC"></a>


## <a name="sample-json-syntax"></a>範例 JSON 語法


```json
{
  "name":"Icon Name",
  "type":"Icon",
  "url":"https://www.xbox.com"
}

```


<a id="ID4EOC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4EQC"></a>


##### <a name="parent"></a>父系

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)
