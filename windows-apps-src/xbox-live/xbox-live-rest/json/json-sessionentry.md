---
title: SessionEntry (JSON)
assetID: b5cf5c3d-83b8-635f-d1a5-0be5d9434ea5
permalink: en-us/docs/xboxlive/rest/json-sessionentry.html
description: " SessionEntry (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 73133f898ff219477cb60f54798cbd81acb87ebe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589733"
---
# <a name="sessionentry-json"></a>SessionEntry (JSON)
包含符合工作階段資料。 
<a id="ID4EN"></a>

 
## <a name="sessionentry"></a>SessionEntry
 
SessionEntry 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| durationInSeconds| 32 位元帶正負號的整數 | 持續時間，以秒為單位，工作階段。 | 
| joules| 32 位元帶正負號的整數 | 能源 — joules 中，工作階段中燒錄。 | 
| 符合| 單精確度浮點數| 在工作階段持續期間內的平均符合的值。 MET 值是相對於待用的個人的新陳代謝速率活動期間的個別新陳代謝率的比例。 因為上來新陳代謝速率是個人的權數，不論 1.0，而且 MET 值是相對於個人的上來新陳代謝速率，它們可用來比較由不同的加權的個人所執行的活動的濃度。| 
| serverTimestamp| DateTime| 時間-UTC 為基礎，在伺服器上輸入的項目。 | 
| 來源| 8 位元不帶正負號的整數| 工作階段的來源。| 
| timestamp| DateTime| 時間 — 基礎上 Coordinated Universal Time (UTC)，在用戶端上建立項目。 | 
| titleId| 64 位元不帶正負號的整數| 標題： 十進位，所建立的項目。| 
  
<a id="ID4EFE"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
   "titleId" : "1234567",
   "timestamp" : "2011-11-18T08:08:46Z",
   "serverTimestamp" : "2011-11-20T04:04:23Z",
   "durationInSeconds" : 240,
   "joules" :  1600,
   "met" :  "124"
   "source" :  "1"
}
    
```

  
<a id="ID4EOE"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EQE"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   