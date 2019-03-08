---
title: PermissionCheckResult (JSON)
assetID: 1cf147fa-4ff1-3299-0822-0fc1726d1600
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresult.html
description: " PermissionCheckResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dc13826be1b6f81201d069f5ade7ea5ba6668cd0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598443"
---
# <a name="permissioncheckresult-json"></a>PermissionCheckResult (JSON)
從單一的權限設定，針對單一的目標使用者的單一使用者檢查的結果。 
<a id="ID4EP"></a>

 
## <a name="permissioncheckresult"></a>PermissionCheckResult
 
PermissionCheckResult 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| reason| 字串| 選用。 A <b>PermissionResultCode</b>值，指出權限被拒原因如果<b>IsAllowed</b>時發生錯誤。| 
| restrictedSetting| 字串| 選用。 如果<b>PermissionResultCode</b>中的值<b>原因</b>成員表示要求者的權限檢查失敗，這表示哪些權限失敗。| 
  
<a id="ID4E6B"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
    "reason": "MissingPrivilege",
    "restrictedSetting": "VideoCommunications"
}
    
```

  
<a id="ID4EIC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EKC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   