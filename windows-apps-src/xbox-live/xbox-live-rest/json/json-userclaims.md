---
title: UserClaims (JSON)
assetID: f88d5ee0-2875-fcfb-3098-3cd6afce8748
permalink: en-us/docs/xboxlive/rest/json-userclaims.html
description: " UserClaims (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 21b4322d002747145c3b09e0f3cd7eb03874380b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623343"
---
# <a name="userclaims-json"></a>UserClaims (JSON)
傳回目前已驗證使用者的相關資訊。 
<a id="ID4EN"></a>

 
## <a name="userclaims"></a>UserClaims
 
UserClaims 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| 玩家代號| 字串| 使用者的玩家代號。| 
| xuid| 64 位元不帶正負號的整數| Xbox 使用者識別碼 (XUID) 的使用者。| 
  
<a id="ID4EZB"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
   "xuid" : 2533274790412952,
   "gamertag" : "gamer"

}
    
```

  
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   