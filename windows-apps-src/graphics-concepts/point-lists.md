---
title: "點清單"
description: "點清單是轉譯為隔離點的頂點集合。 應用程式可以在 3D 場景中使用點清單，在多邊形的表面上有星星欄位或點線。"
ms.assetid: 332954AE-019F-4A91-B773-E3A7C92F3297
keywords:
- "點清單"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: da7fd4dfd4835e39a76daddfa8855415f0e3234b
ms.lasthandoff: 02/07/2017

---

# <a name="point-lists"></a>點清單


點清單是轉譯為隔離點的頂點集合。 應用程式可以在 3D 場景中使用點清單，在多邊形的表面上有星星欄位或點線。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>範例


下圖描述轉譯的點清單。

![點清單的圖例](images/pointlst.png)

您的應用程式可以套用資料和紋理到點清單。 資料或紋理中的色彩只出現在繪製的點，而不是點之間的任何位置。

下列程式碼顯示如何建立這個點清單的頂點。

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

下列程式碼範例顯示如何在 Direct3D 中轉譯這個點清單。

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_POINTLIST, 0, 6 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[基本型別](primitives.md)

 

 





