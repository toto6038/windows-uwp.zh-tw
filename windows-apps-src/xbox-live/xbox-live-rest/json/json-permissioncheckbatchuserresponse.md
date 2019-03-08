---
title: PermissionCheckBatchUserResponse (JSON)
assetID: c587dbc1-9436-4d55-afcb-deb47e3c2664
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchuserresponse.html
description: " PermissionCheckBatchUserResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c9e20cc195ad737a7e847a8ad41b76247220adfe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628353"
---
# <a name="permissioncheckbatchuserresponse-json"></a>PermissionCheckBatchUserResponse (JSON)
原因的批次的權限檢查的單一目標使用者的權限值清單。 
<a id="ID4EN"></a>

 
## <a name="permissioncheckbatchuserresponse"></a>PermissionCheckBatchUserResponse
 
PermissionCheckBatchUserResponse 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| 使用者| 字串| 必要。 這個成員是<b>，則為 true</b>提出要求的使用者是否可執行要求的動作，與目標使用者。| 
| 權限| 陣列[PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)| 必要。 A [PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)所要求的原始要求，相同的順序，與要求中的每項權限。| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
    "User": {"Xuid": "12345"},
    "Permissions":
    [
        {
            "isAllowed": true
        },
        {
            "isAllowed": false
        },
        {
            "isAllowed": false,
            "reasons":
            [
                {"reason": "BlockedByRequestor"},
                {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
            ]
        }
    ]
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   