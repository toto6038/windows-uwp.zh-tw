---
title: ResetReputation (JSON)
assetID: 15edb5e7-a00b-4188-9b49-9db5774c4a10
permalink: en-us/docs/xboxlive/rest/json-resetreputation.html
description: " ResetReputation (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d09c8bbc1130f91dfea3d4c35e391dcf9adcf127
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649413"
---
# <a name="resetreputation-json"></a>ResetReputation (JSON)
包含新基底應該要變更使用者的現有分數的信譽分數。 
<a id="ID4EN"></a>

 
## <a name="resetreputation"></a>ResetReputation
 
ResetReputation 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| fairplayReputation| 數字| 想要新增的基底 Fairplay 信譽打分數，針對使用者 （有效範圍 0 到 75）。| 
| commsReputation| 數字| 想要新增的基底使用者 （有效範圍 0 到 75） 的通訊的簡訊回覆信譽打分數。| 
| userContentReputation| 數字| 想要新增的基底 UserContent 信譽打分數，針對使用者 （有效範圍 0 到 75）。| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   