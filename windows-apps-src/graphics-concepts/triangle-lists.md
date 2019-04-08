---
title: 三角形清單
description: 三角形清單是隔離三角形的清單。 隔離的三角形不一定彼此相近。 三角形清單必須至少有 3 個頂點，而且頂點總數必須可被 3 整除。
ms.assetid: BC50D532-9E9C-4AAE-B466-9E8C4AD1862A
keywords:
- 三角形清單
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: aae797db890c6bee141c3b4a79a6a85a55a6b512
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653743"
---
# <a name="triangle-lists"></a>三角形清單


三角形清單是隔離三角形的清單。 隔離的三角形不一定彼此相近。 三角形清單必須至少有 3 個頂點，而且頂點總數必須可被 3 整除。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>範例


使用三角形清單，建立由不相交項目所組成的物件。 例如，建立 3D 遊戲中力場牆的一個方式是指定小、未連接三角形的大型清單。 然後套用看起來會發出光到三角形清單的材質和紋理。 牆中每個三角形看起來會發光。 牆背後的場景透過三角形之間的縫隙變成部分可見，如玩家看向力場時可能的預期。

三角形清單也適用於建立有清晰邊緣並以 Gouraud Shading 呈現陰影的基本類型。 請參閱[面和頂點一般向量](face-and-vertex-normal-vectors.md)。

下圖描述轉譯的三角形清單。

![轉譯三角形清單的圖例](images/trilist.png)

下列程式碼顯示如何建立這個三角形清單的頂點。

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

下列程式碼範例顯示如何在 Direct3D 中轉譯這個三角形清單。

```
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLELIST, 0, 2 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[基本項目](primitives.md)

 

 




