---
title: GameClipThumbnail (JSON)
assetID: 3ed87fc1-734c-d8b5-d908-0ae3359769ed
permalink: en-us/docs/xboxlive/rest/json-gameclipthumbnail.html
description: " GameClipThumbnail (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ad2d35431cb4c40690978f4f3920f2e47f2b9bc0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606563"
---
# <a name="gameclipthumbnail-json"></a>GameClipThumbnail (JSON)
包含個別的縮圖的相關資訊。 可以有多個大小，每個剪輯，並由用戶端選取適當的顯示。 
<a id="ID4EN"></a>

 
## <a name="gameclipthumbnail"></a>GameClipThumbnail
 
GameClipThumbnail 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| <b>uri</b>| 字串| 縮圖影像的 URI。| 
| <b>fileSize</b>| 32 位元不帶正負號的整數| 檔案大小總計的縮圖影像。| 
| <b>thumbnailType</b>| ThumbnailType| 縮圖影像的類型。| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
         "uri": "https://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
    
```

  
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   