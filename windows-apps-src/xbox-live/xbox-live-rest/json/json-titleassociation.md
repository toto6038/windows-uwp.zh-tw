---
title: TitleAssociation (JSON)
assetID: 05f4edbb-cc3d-c17d-0979-aac9e44a4988
permalink: en-us/docs/xboxlive/rest/json-titleassociation.html
description: " TitleAssociation (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 21a583e7556f98b827a63de3948f43d76f25c907
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592893"
---
# <a name="titleassociation-json"></a>TitleAssociation (JSON)
成就與相關聯的標題。 
<a id="ID4EN"></a>

 
## <a name="titleassociation"></a>TitleAssociation
 
TitleAssociation 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| name| 字串| 內容的當地語系化的名稱。| 
| id| 字串| TitleId （32 位元不帶正負號的整數，以十進位格式傳回） 中。| 
| version| 字串| 特定版本的相關聯的標題 （如果適用）。| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
  "name":"Microsoft Achievements Sample",
  "id":3051199919,
  "version":"abc"
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   