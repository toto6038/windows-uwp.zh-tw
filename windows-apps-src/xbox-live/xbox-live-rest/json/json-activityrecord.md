---
title: ActivityRecord (JSON)
assetID: e3a7465b-3451-0266-f8ba-b7602b59f7af
permalink: en-us/docs/xboxlive/rest/json-activityrecord.html
description: " ActivityRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a8679c96c86754a8b929b44b5bd4eb402d851e90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608363"
---
# <a name="activityrecord-json"></a>ActivityRecord (JSON)
一或多個使用者的完整的目前狀態的相關格式化和當地語系化字串。 
<a id="ID4EN"></a>

 
## <a name="activityrecord"></a>ActivityRecord
 
ActivityRecord 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| richPresence| 字串| 豐富的目前狀態字串，格式化和當地語系化。| 
| 媒體| MediaRecord| 哪些使用者觀賞或接聽。| 
  
<a id="ID4ETB"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
        richPresence:"Team Deathmatch on Nirvana"
      }
    
```

  
<a id="ID4E3B"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E5B"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   