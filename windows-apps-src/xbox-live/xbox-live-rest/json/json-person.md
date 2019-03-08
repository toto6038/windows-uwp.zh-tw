---
title: Person (JSON)
assetID: b49234b1-03cd-f16e-c293-c74174382167
permalink: en-us/docs/xboxlive/rest/json-person.html
description: " Person (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 175d66ffc7744ca8203fe7681fcb0167e150f012
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640863"
---
# <a name="person-json"></a>Person (JSON)
人員系統中的單一個人相關的中繼資料。 
<a id="ID4EN"></a>

 
## <a name="person"></a>Person
 
Person 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| xuid| 字串| 必要。 Xbox 使用者識別碼 (XUID) 以十進位格式。 範例值：2603643534573573.| 
| isFavorite| 布林值| 必要。 這位人員是否使用者在意更多的其中一個。 由於使用者在他們的人員清單中有非常大量的人，應該優先體驗中最愛的人員，和之前不是 [我的最愛] 的其他人所示。| 
| isFollowingCaller| 布林值| 選用。 這位使用者正在關注的使用者是否代表其 API 進行呼叫。| 
| socialNetworks| 字串陣列| 選用。 中的外部網路使用者，這個人具有關聯性。| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>參考 

[/users/{ownerId}/people/{targetid}](../uri/people/uri-usersowneridpeopletargetid.md)

 [/users/{ownerId}/people/xuids](../uri/people/uri-usersowneridpeoplexuids.md)

   