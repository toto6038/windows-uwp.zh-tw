---
title: User (JSON)
assetID: dbc733e4-0348-0e3d-1f55-17b465e599d6
permalink: en-us/docs/xboxlive/rest/json-user.html
description: " User (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5c1b3a34ef329696d51e615dd79d57783a132d05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641353"
---
# <a name="user-json"></a>User (JSON)
包含使用者排行榜資料。 
<a id="ID4EN"></a>

 
## <a name="user"></a>使用者
 
使用者物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| 玩家代號| 字串| 播放程式 （最多 15 個字元） 的玩家代號。 用戶端在識別播放程式時，應該在 UI 中使用此值。| 
| 陣序規範| 32 位元帶正負號的整數| 相對於要求排行榜資料的使用者之使用者的順位。| 
| rating| 字串| 使用者的評比。| 
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼 (XUID) 的使用者。| 
  
<a id="ID4EMC"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{ 
   "gamertag":"TrueBlue402",
   "rank":2,
   "rating":"2:19:21.17",
   "xuid":1234567890123456 
}
    
```

  
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   