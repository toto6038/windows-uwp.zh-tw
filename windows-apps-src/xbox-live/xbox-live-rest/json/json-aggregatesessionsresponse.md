---
title: AggregateSessionsResponse (JSON)
assetID: 020ee9b2-c96c-2e65-4e6d-f9f4bd25a374
permalink: en-us/docs/xboxlive/rest/json-aggregatesessionsresponse.html
description: " AggregateSessionsResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ef026fd5096d047b2014faaf95a667c69827e043
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608303"
---
# <a name="aggregatesessionsresponse-json"></a>AggregateSessionsResponse (JSON)
包含使用者的適用性工作階段的彙總的資料。 
<a id="ID4EN"></a>

 
## <a name="aggregatesessionsresponse"></a>AggregateSessionsResponse
 
AggregateSessionsResponse 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| totalDurationInSeconds| 64 位元帶正負號的整數| 中彙總期間的秒數的工作階段的總持續時間。| 
| totalJoules| 64 位元帶正負號的整數| 總能源燒錄 — 在 joules — 彙總期間。 | 
| totalSessions| 64 位元帶正負號的整數| 彙總期間的工作階段的總數。| 
| weightedAverageMets| 單精確度浮點數 | 彙總期間的加權平均新陳代謝值相等的工作 (MET)。 MET 值是相對於待用的個人的新陳代謝速率活動期間的個別新陳代謝率的比例。 因為上來新陳代謝速率是個人的權數，不論 1.0，而且 MET 值是相對於個人的上來新陳代謝速率，它們可用來比較由不同的加權的個人所執行的活動的濃度。| 
  
<a id="ID4ESC"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
   "totalSessions" : 300,
   "totalDurationInSeconds" : 1240,
   "totalJoules" :  21600,
   "weightedAvgMet" : 21,
}

    
```

  
<a id="ID4E2C"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E4C"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   