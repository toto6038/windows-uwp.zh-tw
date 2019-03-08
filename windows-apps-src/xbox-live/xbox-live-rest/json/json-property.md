---
title: Property (JSON)
assetID: 93de547e-d936-6fcc-92cb-e4dd284dd609
permalink: en-us/docs/xboxlive/rest/json-property.html
description: " Property (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7e2a721886509c49c60d663d491f8d49bc3c95e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659703"
---
# <a name="property-json"></a>Property (JSON)
包含用戶端所提供的配對要求準則的屬性資料。
<a id="ID4EN"></a>


## <a name="property"></a>屬性

屬性物件具有下列規格。

| 成員| 類型| 描述|
| --- | --- | --- |
| id| 字串| 此屬性的識別碼。|
| type| 32 位元帶正負號的整數 | 屬性型別。 支援的值為： <ul><li>0 = 整數</li><li>1 = 字串</li></ul>| 
| value| 字串| 這個屬性的值。|

<a id="ID4EGC"></a>


## <a name="sample-json-syntax"></a>範例 JSON 語法


```json
{
    "id":"1",
    "value":"20",
    "type":0
}

```


<a id="ID4EPC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4ERC"></a>


##### <a name="parent"></a>父系

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)
