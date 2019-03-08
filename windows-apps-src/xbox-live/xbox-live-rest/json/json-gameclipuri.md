---
title: GameClipUri (JSON)
assetID: 03c097e8-7f29-1026-7a77-5c785b8511e9
permalink: en-us/docs/xboxlive/rest/json-gameclipuri.html
description: " GameClipUri (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1c74d0831c3b841ad2a1366bd2e03fb8a9b0448d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623493"
---
# <a name="gameclipuri-json"></a>GameClipUri (JSON)
 
<a id="ID4EO"></a>

 
## <a name="gameclipuri"></a>GameClipUri
 
GameClipUri 物件具有下列規格。
 
| 成員| 類型| 描述| 
| --- | --- | --- | 
| <b>uri</b>| 字串| 要的視訊資產位置的 URI。| 
| <b>fileSize</b>| 32 位元不帶正負號的整數| 檔案大小總計的縮圖影像。| 
| <b>uriType</b>| GameClipUriType| URI 的類型。| 
| <b>expiration</b>| DateTime| 包含在此回應的 URI 到期時間。 如果 URL 是空的或被視為過期，才能播放，呼叫端應該呼叫 RefreshUrl API。| 
  
<a id="ID4EMC"></a>

 
## <a name="sample-json-syntax"></a>範例 JSON 語法
 

```json
{
         "uri": "https://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
    
```

  
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>請參閱
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>父系 

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)

   