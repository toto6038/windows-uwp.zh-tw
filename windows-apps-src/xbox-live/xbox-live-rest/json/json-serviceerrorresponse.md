---
title: ServiceErrorResponse (JSON)
assetID: a2077df8-f76c-0233-8e41-68267b681862
permalink: en-us/docs/xboxlive/rest/json-serviceerrorresponse.html
description: " ServiceErrorResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 86f9389f6f76c1c51955a6c784393e9b05909298
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597503"
---
# <a name="serviceerrorresponse-json"></a>ServiceErrorResponse (JSON)
發生服務錯誤時，就會傳回適當的 HTTP 錯誤碼。 （選擇性） 此服務也可能包括 ServiceErrorResponse 物件，定義如下。 在生產環境中可能包含較少的資料。 
<a id="ID4EN"></a>

 
## <a name="serviceerrorresponse"></a>ServiceErrorResponse
 
ServiceErrorResponse 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| <b>errorCode</b>| 32 位元帶正負號的整數| （可以是 null） 的錯誤相關聯的程式碼。| 
| <b>errorMessage</b>| 字串| 有關錯誤的其他詳細資料。| 
  
<a id="ID4EVB"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
   "errorCode": 8377,
   "errorMessage": "XUID specified in the claim does not match URI XUID."
 }
    
```

  
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   