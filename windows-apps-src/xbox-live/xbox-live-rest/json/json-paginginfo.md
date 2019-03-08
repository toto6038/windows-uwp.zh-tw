---
title: PagingInfo (JSON)
assetID: 645e575d-3e8e-d954-90e6-e51dd83da93b
permalink: en-us/docs/xboxlive/rest/json-paginginfo.html
description: " PagingInfo (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0e773d73499e79fe23f736a536027932ca1a07b4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651213"
---
# <a name="paginginfo-json"></a>PagingInfo (JSON)
包含資料頁面中傳回結果的分頁的資訊。 
<a id="ID4EN"></a>

 
## <a name="paginginfo"></a>PagingInfo
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| continuationToken| 字串| 不透明接續 token，用來存取結果的下一頁。 最多 32 個字元。呼叫端可以提供此值<b>continuationToken</b>查詢以擷取下一個集合中的項目集的參數。 如果這個屬性是<b>null</b>，然後在集合中沒有任何其他項目。 這個屬性是必要，且即使與呼叫集合，提供<b>skipItems</b>。| 
| totalItems| 32 位元帶正負號的整數| 集合中的項目總數。 未提供此服務是否無法提供集合的大小的即時檢視。| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
   "continuationToken":"16354135464161213-0708c1ba-147f-48cc-9ad9-546gaadg648"
   "totalItems":5,
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   