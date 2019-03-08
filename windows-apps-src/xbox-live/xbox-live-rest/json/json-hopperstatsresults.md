---
title: HopperStatsResults (JSON)
assetID: 91927da1-2e97-f7bc-ae62-7e0e9966b98e
permalink: en-us/docs/xboxlive/rest/json-hopperstatsresults.html
description: " HopperStatsResults (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 38e345fc20e92cdf6446c6ae1100e347fe634eff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646203"
---
# <a name="hopperstatsresults-json"></a>HopperStatsResults (JSON)
JSON 物件，代表 hopper 的統計資料。 
<a id="ID4EN"></a>

  
 
HopperStatsResults JSON 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| hopperName| 字串| 選取 hopper 名稱。| 
| waitTime| 32 位元帶正負號的整數| 比對 hopper （整數秒數） 的時間的平均值。 | 
| 母體擴展| 32 位元帶正負號的整數| 人員等候 hopper 中相符項目數目。| 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法 
 

```json
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }
  
    
```

  
<a id="ID4EGB"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EIB"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUB"></a>

 
##### <a name="reference"></a>參考 

[GET (/serviceconfigs/{scid}/hoppers/{name}/stats)](../uri/matchtickets/uri-serviceconfigsscidhoppershoppernamestatsget.md)

   