---
title: 三角形連環
description: 三角形連環是一系列的連接三角形。 因為是連接三角形，應用程式不需要重複指定每個三角形的所有三個頂點。
ms.assetid: BACC74C5-14E5-4ECC-9139-C2FD1808DB82
keywords:
- 三角形連環
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9a420ed5ed8f498eb9c900cbacb1b766c4a01214
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8881026"
---
# <a name="triangle-strips"></a>三角形連環


三角形連環是一系列的連接三角形。 因為是連接三角形，應用程式不需要重複指定每個三角形的所有三個頂點。 例如，您只需要七個頂點即可定義下列三角形連環。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>範例


![使用七個頂點的三角形連環的圖例](images/tristrip.png)

系統會使用頂點 v1、v2 及 v3 繪製第一個三角形；v2、v4 及 v3 繪製第二個三角形；v3、v4 及 v5 繪製第三個；v4、v6 及 v5 繪製第四個，以此類推。 請注意，第二個和第四個三角形的頂點失序，這是確保所有三角形都是以順時針方向繪製。

3D 場景中的大多數物件都是由三角形連環組成。 這是因為三角形連環可用於以有效率使用記憶體和處理時間的方式指定複雜物件。

下圖描述轉譯的三角形連環。

![轉譯三角形連環的圖例](images/tstrip2.png)

下列程式碼顯示如何建立這個三角形連環的頂點。

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

下列程式碼範例顯示如何在 Direct3D 中轉譯這個三角形連環。

```
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLESTRIP, 0, 4);
```

## <a name="span-idpolygonsspanspan-idpolygonsspanspan-idpolygonsspanpolygons"></a><span id="Polygons"></span><span id="polygons"></span><span id="POLYGONS"></span>多邊形


通常三角形連環用於建立多邊形。 多邊形是由至少三個頂點來描繪的 3D 封閉圖形。 最簡單的多邊形為三角形。 Microsoft Direct3D 使用三角形來組合大部分的多邊形，因為這保證三角形中的所有三個頂點為共面。 轉譯非平面頂端沒有效率。 您可以結合三角形來形成大型且複雜的多邊形和網格。

下圖顯示立方體。 兩個三角形形成立方體的每個面。 整組三角形形成一個立方體基本類型。 您可以將紋理套用至基本類型的表面，使其看起來是單一實心形式。 如需詳細資訊，請參閱[紋理](textures.md)。

![每個面有兩個三角形的立方體的圖例](images/cube3d.png)

您也可以使用三角形。建立表面看起來像是平滑曲線的基本類型。 下圖顯示使用三角形模擬球體的方式。 套用材料之後，可以讓球體轉譯時看起來有曲線。

![使用三角形模擬球體的圖例](images/sphere3d.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[基本型別](primitives.md)

 

 




