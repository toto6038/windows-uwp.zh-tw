---
title: MultiplayerSessionReference (JSON)
assetID: 6e03e060-8c9b-b394-415f-af7e85be569f
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionreference.html
description: " MultiplayerSessionReference (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5986079e1cae3338d8cc24a9e85f6941cf4fbec4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651243"
---
# <a name="multiplayersessionreference-json"></a>MultiplayerSessionReference (JSON)
JSON 物件，表示**MultiplayerSessionReference**。 
<a id="ID4EQ"></a>

  
 
MultiplayerSessionReference JSON 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| scid| GUID| 服務設定識別項 (SCID)。 工作階段識別項的第 1 部分。| 
| templateName | 字串 | 工作階段範本的目前執行個體的名稱。 第 2 部分的工作階段識別碼。 | 
| name | 字串 | 工作階段的名稱。 第 3 部分的工作階段識別碼。 | 
  
<a id="ID4EZ"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法 
 

```json
{
  "scid": "8d050174-412b-4d51-a29b-d55a34edfdb7",
  "templateName": "integration",
  "name": "19de0095d8bb41048f19edbbb6bc6b04"
}
  
    
```

  
<a id="ID4EJB"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ELB"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EVB"></a>

 
##### <a name="reference"></a>參考 

[MultiplayerSession (JSON)](json-multiplayersession.md)

   