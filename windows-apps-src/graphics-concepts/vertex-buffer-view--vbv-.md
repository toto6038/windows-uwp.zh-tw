---
title: "頂點緩衝區檢視 (VBV) 和索引緩衝區檢視 (IBV)"
description: "頂點緩衝區保留頂點清單的資料。"
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords:
- "頂點緩衝區檢視 (VBV)"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: f32684b167b7582a646a7cfd47c606a9f382a4c4
ms.lasthandoff: 02/07/2017

---

# <a name="vertex-buffer-view-vbv-and-index-buffer-view-ibv"></a>頂點緩衝區檢視 (VBV) 和索引緩衝區檢視 (IBV)


頂點緩衝區保留頂點清單的資料。 每個頂點的資料可以包含位置、色彩、標準向量、紋理座標等等。 索引緩衝區會保留頂點緩衝區中的整數索引（位移），用來定義和轉譯由頂點完整清單子集組成的物件。

單一頂點的定義通常是由應用程式定義的，例如：

``` syntax
struct CUSTOMVERTEX { 
        FLOAT x, y, z;      // The position
        FLOAT nx, ny, nz;   // The normal
        DWORD color;        // RGBA color
        FLOAT tu, tv;       // The texture coordinates. 
}; 
```

CUSTOMVERTEX 的定義接著會在建立頂點緩衝區時傳遞至圖形驅動程式。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[檢視](views.md)

 

 





