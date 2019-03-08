---
title: Progression (JSON)
assetID: cdff6415-f12b-0a45-61f2-26dbf47b1b56
permalink: en-us/docs/xboxlive/rest/json-progression.html
description: " Progression (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dda124e5be9a4d21a1ee5b9d6130290207e31921
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593823"
---
# <a name="progression-json"></a>Progression (JSON)
使用者的進展，朝解除鎖定成就。 
<a id="ID4EN"></a>

 
## <a name="progression"></a>進展
 
進展物件有以下規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| 需求| 陣列的需求| 贏得成就的需求，而且使用者過程的進展到解除鎖定的狀況。| 
| timeUnlocked| DateTime| 成就已先解除鎖定的時間。| 
  
<a id="ID4ETB"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
  "requirements":
  [{
    "id":"12345678-1234-1234-1234-123456789012",
    "current":null,
    "target":"100"
  }],
  "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
}
    
```

  
<a id="ID4E3B"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E5B"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   