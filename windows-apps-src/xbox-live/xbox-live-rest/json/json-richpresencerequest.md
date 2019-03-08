---
title: RichPresenceRequest (JSON)
assetID: 599266be-f747-0be1-fadf-f8e0262dc27f
permalink: en-us/docs/xboxlive/rest/json-richpresencerequest.html
description: " RichPresenceRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4c49da63ecd091a886a68f508af09e33fb9c58ac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654123"
---
# <a name="richpresencerequest-json"></a>RichPresenceRequest (JSON)
如需應該使用豐富的目前狀態資訊的詳細資訊的要求。 
<a id="ID4EN"></a>

 
## <a name="richpresencerequest"></a>RichPresenceRequest
 
RichPresenceRequest 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| id| 字串| <b>FriendlyName</b>来使用完整的目前狀態字串。| 
| scid| 字串| Scid 告訴我們，並定義豐富的目前狀態的字串。| 
| params| 字串陣列| 陣列<b>friendlyName</b>字串用來完成完整的目前狀態的字串。 應該指定僅列舉易記名稱，不是統計資料。將保留此空白，將會移除任何先前的值。| 
  
<a id="ID4EDC"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f",
    }
    
```

  
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   