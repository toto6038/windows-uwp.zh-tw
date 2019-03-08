---
title: PermissionCheckResponse (JSON)
assetID: 7a749001-b569-b0e0-2a35-f299474c8710
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresponse.html
description: " PermissionCheckResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7623a45fa21e30a015bd5c6a7c1f5add19cc189b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623363"
---
# <a name="permissioncheckresponse-json"></a>PermissionCheckResponse (JSON)
從單一的權限設定，針對單一的目標使用者的單一使用者檢查的結果。 
<a id="ID4EN"></a>

 
## <a name="permissioncheckresponse"></a>PermissionCheckResponse
 
PermissionCheckResponse 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| IsAllowed| 布林值| 必要。 這個成員是<b>，則為 true</b>提出要求的使用者是否可執行要求的動作，與目標使用者。| 
| 結果| 陣列[PermissionCheckResult (JSON)](json-permissioncheckresult.md)| 選用。 如果<b>IsAllowed</b>時發生錯誤，並核取遭拒依相關給要求者的值，表示 的權限被拒原因。| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}
    
```

  
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   