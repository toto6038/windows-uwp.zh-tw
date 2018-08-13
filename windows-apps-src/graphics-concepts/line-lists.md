---
title: 線清單
description: 線清單是隔離、直線段的清單。 在 3D 場景中加入冰雹或暴雨的這類工作適合使用線清單。 應用程式藉由填入頂點陣列來建立線清單。
ms.assetid: 42BF32A1-3535-42A3-82C5-3945CB309F2C
keywords:
- 線清單
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 66e2c6a455e64ce7ac782c268045f4c14dadcf7b
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "1042987"
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

 

 




