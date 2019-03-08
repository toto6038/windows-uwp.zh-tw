---
title: InitialUploadResponse (JSON)
assetID: 6abb7d37-2c35-2cc3-d9e5-eff695235262
permalink: en-us/docs/xboxlive/rest/json-initialuploadresponse.html
description: " InitialUploadResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dab59fefb389cf550a1bc4fc6429f6b0970f50ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589833"
---
# <a name="initialuploadresponse-json"></a>InitialUploadResponse (JSON)
 
<a id="ID4EO"></a>

 
## <a name="initialuploadresponse"></a>InitialUploadResponse
 
InitialUploadResponse 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| <b>gameClipId</b>| 字串| 針對上傳資料的要求指派的識別碼。| 
| <b>uploadUri</b>| URI| 遊戲的剪輯應上傳的位置。| 
| <b>largeThumbnailUri</b>| URI| 選用。 大型縮圖應上傳的位置。 此欄位是否存在取決[ThumbnailSource 列舉](../enums/gvr-enum-thumbnailsource.md)中的值<b>InitialUploadRequest</b> （會指定上傳時的存在）。| 
| <b>smallThumbnailUri</b>| URI| 選用。 小型縮圖應上傳的位置。 此欄位是否存在取決[ThumbnailSource 列舉](../enums/gvr-enum-thumbnailsource.md)中的值<b>InitialUploadRequest</b> （會指定上傳時的存在）。| 
  
<a id="ID4EYC"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752"  ,  
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
    
```

  
<a id="ID4EBD"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EDD"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   