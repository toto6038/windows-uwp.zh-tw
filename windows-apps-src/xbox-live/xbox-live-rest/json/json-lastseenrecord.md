---
title: LastSeenRecord (JSON)
assetID: 6a93202c-801c-03c6-8386-6acd0f366780
permalink: en-us/docs/xboxlive/rest/json-lastseenrecord.html
description: " LastSeenRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e06de31cabaedb68ed57d3d4f2ff30614ceb6317
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613373"
---
# <a name="lastseenrecord-json"></a>LastSeenRecord (JSON)
當系統最後看到的使用者，使用者有無有效 DeviceRecord 時可供使用的相關資訊。 
<a id="ID4EN"></a>

 
## <a name="lastseenrecord"></a>LastSeenRecord
 
LastSeenRecord 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| deviceType| 字串| 使用者的最後一個出現的裝置類型。| 
| titleId| 32 位元不帶正負號的整數| 使用者的最後一個出現項目的識別項。| 
| titleName| 字串| 使用者的最後一個出現項目的名稱。| 
| timestamp| DateTime| 指出當使用者的最後一個出現的 UTC 時間戳記。| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
  deviceType:W8,    
  titleId:"23452345",
  titleName:"My Awesome Game",
  timestamp:"2012-09-17T07:15:23.4930000"
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>參考   