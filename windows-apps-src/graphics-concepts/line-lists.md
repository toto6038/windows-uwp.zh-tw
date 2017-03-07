---
title: "線清單"
description: "線清單是隔離、直線段的清單。 在 3D 場景中加入冰雹或暴雨的這類工作適合使用線清單。 應用程式藉由填入頂點陣列來建立線清單。"
ms.assetid: 42BF32A1-3535-42A3-82C5-3945CB309F2C
keywords:
- "線清單"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 64ab9106986cd3fe5109b47710a7057ad6869c8d
ms.lasthandoff: 02/07/2017

---

# <a name="line-lists"></a>線清單


線清單是隔離、直線段的清單。 在 3D 場景中加入冰雹或暴雨的這類工作適合使用線清單。 應用程式藉由填入頂點陣列來建立線清單。 請注意，線清單中的頂點數必須是大於或等於二的偶數。

-   [範例](#example)
-   [相關主題](#related-topics)

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>範例


下圖顯示呈現的線清單。

![線清單的圖例](images/linelst.png)

您可以將資料和紋理套用到線清單。 資料或紋理中的的色彩只會隨繪製線條出現，不會出現在線之間的任何點。

下列程式碼顯示如何建立這個線段的頂點。

```
struct CUSTOMVERTEX
{
    float x,y,z;
};

CUSTOMVERTEX Vertices[] = 
{
    {-5.0, -5.0, 0.0},
    { 0.0,  5.0, 0.0},
    { 5.0, -5.0, 0.0},
    {10.0,  5.0, 0.0},
    {15.0, -5.0, 0.0},
    {20.0,  5.0, 0.0}
};
```

下列程式碼範例顯示如何在 Direct3D 中呈現線段。

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINELIST, 0, 3 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[基本型別](primitives.md)

 

 





