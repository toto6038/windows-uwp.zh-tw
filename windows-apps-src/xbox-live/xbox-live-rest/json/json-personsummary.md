---
title: PersonSummary (JSON)
assetID: 22fedb5f-5602-98d8-04a6-786fe3905921
permalink: en-us/docs/xboxlive/rest/json-personsummary.html
description: " PersonSummary (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a787992507405a70185140e879be731d72806eff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612623"
---
# <a name="personsummary-json"></a>PersonSummary (JSON)
集合[人員 (JSON)](json-person.md)物件。 
<a id="ID4ER"></a>

 
## <a name="personsummary"></a>PersonSummary
 
PersonSummary 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| hasCallerMarkedTargetAsFavorite| 布林值| 是否呼叫者已標示為我的最愛的目標。 範例值： true| 
| hasCallerMarkedTargetAsKnown| 布林值| 是否將呼叫者已標示為目標的已知。 範例值： true| 
| isCallerFollowingTarget| 布林值| 呼叫端是否遵循目標。 範例值： true| 
| isTargetFollowingCaller| 布林值| 目標是否遵循呼叫端。 範例值： true| 
| legacyFriendStatus| 字串| 呼叫端所見之目標的舊版 friend 狀態。 可以是"None"、"MutuallyAccepted 」、 「 OutgoingRequest"或 「 IncomingRequest"。 範例值："MutuallyAccepted"| 
| recentChangeCount| 32 位元不帶正負號的整數| 選用。 目標的社交圖形中的最近變更的數目。 當使用者在檢視他們自己的摘要時，這個值只會存在。 範例值：5| 
| targetFollowerCount| > 32 位元不帶正負號的整數| 下列目標的使用者數目。 範例值：1308| 
| targetFollowingCount| 32 位元不帶正負號的整數| 目標是關注的使用者數目。 範例值：112| 
| 浮水印| 字串| 選用。 目標的最新變更浮水印。 當使用者在檢視他們自己的摘要時，這個值只會存在。 範例值：5| 
  
<a id="ID4E4D"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
    "targetFollowingCount": 87,
    "targetFollowerCount": 19,
    "isCallerFollowingTarget": true,
    "isTargetFollowingCaller": false,
    "hasCallerMarkedTargetAsFavorite": true,
    "hasCallerMarkedTargetAsKnown": true,
    "legacyFriendStatus": "None",
    "recentChangeCount": 5,
    "watermark": "5246775845000319351"
}
    
```

  
<a id="ID4EGE"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EIE"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   