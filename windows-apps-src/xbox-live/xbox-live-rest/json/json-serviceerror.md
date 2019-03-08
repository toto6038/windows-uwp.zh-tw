---
title: ServiceError (JSON)
assetID: 81c43f6e-bfff-c4b5-d25c-eace22649f01
permalink: en-us/docs/xboxlive/rest/json-serviceerror.html
description: " ServiceError (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: da3d682a1b66d25a12f21a93e9596d13afae7f90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631703"
---
# <a name="serviceerror-json"></a>ServiceError (JSON)
包含當服務呼叫失敗時傳回錯誤的相關資訊。 
<a id="ID4EN"></a>

 
## <a name="serviceerror"></a>ServiceError
 
ServiceError 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| code| 32 位元帶正負號的整數 | 錯誤類型。 請參閱下表針對可能的值。 | 
| 來源| 字串 | 引發錯誤的服務名稱。 例如，值<code>ReputationFD</code>指出錯誤為信譽服務中。 | 
| description| 字串| 錯誤的描述。 | 
 
<a id="ID4EBC"></a>

 
### <a name="error-codes"></a>錯誤碼
 
| 值| 描述| 
| --- | --- | --- | --- | --- | 
| 0| 成功沒有錯誤| 
| 4000| 無效提交 POST 要求失敗驗證的要求主體的 JSON 文件。 請參閱 [說明] 欄位，如需詳細資訊。 | 
| 4100| 使用者不會不存在 XUID 要求 URI 中所含不代表有效的使用者，在 XBOX Live。| 
| 4500| 授權錯誤的呼叫端未獲授權執行要求的作業。| 
| 5000| 服務錯誤發生在服務中的發生內部錯誤| 
| 5300| 服務無法使用該服務無法使用。| 
   
<a id="ID4EQE"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
    
```

  
<a id="ID4EZE"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E2E"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   