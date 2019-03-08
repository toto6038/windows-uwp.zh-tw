---
title: PermissionCheckBatchRequest (JSON)
assetID: 3100b17c-1b60-fdf2-3af9-7e424f1903ee
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchrequest.html
description: " PermissionCheckBatchRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 538547c85648970ab3e9fe3ae413e8a03df814ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623503"
---
# <a name="permissioncheckbatchrequest-json"></a>PermissionCheckBatchRequest (JSON)
PermissionCheckBatchRequest 物件的集合。 
<a id="ID4EP"></a>

 
## <a name="permissioncheckbatchrequest"></a>PermissionCheckBatchRequest
 
PermissionCheckBatchRequest 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| 使用者| 使用者的陣列| 必要。 要檢查權限的目標陣列。 此陣列中的每個項目是 Xbox 使用者識別碼 (XUID) 或匿名關閉網路使用者用於跨網路案例中 (「 anonymousUser":"allUsers")。 | 
| 權限| 陣列[PermissionId 列舉](../enums/privacy-enum-permissionid.md)| 必要。 若要檢查針對每個使用者的權限。| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
    "users":
    [
        {"xuid":"12345"},
        {"xuid":"54321"}
    ],
    "permissions":
    [
        "ShareFriendList",
        "ShareGameHistory"
    ]
}
    
```

  
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   