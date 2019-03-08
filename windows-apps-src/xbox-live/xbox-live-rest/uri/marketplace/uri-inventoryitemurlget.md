---
title: GET (/inventory/{itemID})
assetID: d3ca14a5-0214-ef42-091e-3f05f2a3482d
permalink: en-us/docs/xboxlive/rest/uri-inventoryitemurlget.html
description: " GET (/inventory/{itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 446197eb20820304088ddac4a6379fa3b2510873
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606693"
---
# <a name="get-inventoryitemid"></a>GET (/inventory/{itemID})
提供特定的清查項目的完整詳細資料。 這些 Uri 的網域是`inventory.xboxlive.com`。
 
  * [註解](#ID4EX)
  * [URI 參數](#ID4EAB)
  * [回應主體](#ID4ELB)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>備註
 
沒有原則檢查，強制執行，或篩選將會發生這個呼叫的一部分。
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>URI 參數
 
| 參數| 類型| 描述| 
| --- | --- | --- | 
| itemID| 字串| 單數的清查項目的每一位使用者的唯一識別碼| 
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>回應主體
 
<a id="ID4ERB"></a>

 
### <a name="sample-response"></a>範例回應
 
GET 要求，假設它通過驗證，並指派適當的授權內容中，回應會是完整的項目屬性集的單一的清查項目。
 

```cpp
{inventoryItem}
         
```

   
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>父系 

[GET (/inventory/{itemID})](uri-inventoryget.md)

  
<a id="ID4EJC"></a>

 
##### <a name="further-information"></a>進一步的資訊 

[EDS 常見的標頭](../../additional/edscommonheaders.md)

 [EDS 參數](../../additional/edsparameters.md)

 [EDS 查詢 Refiners](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [其他參考](../../additional/atoc-xboxlivews-reference-additional.md)

   