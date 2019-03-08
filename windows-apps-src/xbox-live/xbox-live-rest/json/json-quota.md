---
title: quotaInfo (JSON)
assetID: 3147ab78-e671-e142-0a3a-688dc4079451
permalink: en-us/docs/xboxlive/rest/json-quota.html
description: " quotaInfo (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f3499fdba972d6e953813fc490d080910921698e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654113"
---
# <a name="quotainfo-json"></a>quotaInfo (JSON)
包含標題群組的配額資訊。 
<a id="ID4EN"></a>

 
## <a name="quotainfo"></a>quotaInfo
 
QuotaInfo 物件具有下列規格。
 
通用的儲存體
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| quotaBytes| 32 位元帶正負號的整數 | 最大項目所使用的位元組數。| 
| usedBytes| 32 位元帶正負號的整數 | 項目所使用的位元組數目。| 
  
<a id="ID4EXB"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 
下列範例會示範全域儲存體的回應：
 

```json
{
    "quotaInfo":
    {
        "usedBytes":4194304,
        "quotaBytes":536870912
    }
}
      
```

  
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   