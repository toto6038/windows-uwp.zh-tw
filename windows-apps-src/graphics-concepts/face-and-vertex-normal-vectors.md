---
title: 面和頂點法向量
description: 每個網格中的平面都有一個垂直單位一般向量。 向量的方向是由定義頂點的順序，和座標系統是否為右手系或左手系而決定的。
ms.assetid: 02333579-9749-4612-B121-23F97898A3E0
keywords:
- 面和頂點法向量
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2081e8c09a6f6fd75f460af3f339902bcb80bac6
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2018
ms.locfileid: "7171437"
---
# <a name="face-and-vertex-normal-vectors"></a>面和頂點法向量


每個網格中的平面都有一個垂直單位一般向量。 向量的方向是由定義頂點的順序，和座標系統是否為右手系或左手系而決定的。

## <a name="span-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanperpendicular-unit-normal-vector-for-a-front-face"></a><span id="Perpendicular_unit_normal_vector_for_a_front_face"></span><span id="perpendicular_unit_normal_vector_for_a_front_face"></span><span id="PERPENDICULAR_UNIT_NORMAL_VECTOR_FOR_A_FRONT_FACE"></span>正面的垂直單位法向量


每個網格中的平面都有一個垂直單位法向量。 向量的方向是由定義頂點的順序，和座標系統是否為右手系或左手系而決定的。 面標準點偏離面的正面。 在 Direct3D 中，只會看見正面。 正面上的頂點是依順時針方向定義。

下圖顯示正面的法向量：

![正面的法向量](images/nrmlvect.png)

## <a name="span-idcullingbackfacesspanspan-idcullingbackfacesspanspan-idcullingbackfacesspanculling-back-faces"></a><span id="Culling_back_faces"></span><span id="culling_back_faces"></span><span id="CULLING_BACK_FACES"></span>消除背面


只要不是正面，都稱為背面。 Direct3D 不一定會呈現背面；一般會說背面被消除。 背面消除是指，消除背面不轉譯。 如果您想要的話，可以變更消除模式以轉譯背面。 如需詳細資訊，請參閱[消除狀態](https://msdn.microsoft.com/library/windows/desktop/bb204882)。

## <a name="span-idvertexunitnormalsspanspan-idvertexunitnormalsspanspan-idvertexunitnormalsspanvertex-unit-normals"></a><span id="Vertex_unit_normals"></span><span id="vertex_unit_normals"></span><span id="VERTEX_UNIT_NORMALS"></span>頂點單位法向量


Direct3D 使用頂點單位法向量呈現 Gouraud Shading、光源及紋理效果。

下圖顯示頂點法向量：

![頂點法向量](images/vertnrml.png)

對多邊形套用 Gouraud Shading 時，Direct3D 會使用頂點法向量計算光源與表面之間的角度。 它會計算頂點的色彩和濃度值，並針對所有基本類型表面上的每一個點插入它們。 Direct3D 會使用角度計算光線強度值。 角度越大，照射在表面上的光線就越弱。

## <a name="span-idflatsurfacesspanspan-idflatsurfacesspanspan-idflatsurfacesspanflat-surfaces"></a><span id="Flat_surfaces"></span><span id="flat_surfaces"></span><span id="FLAT_SURFACES"></span>平面


如果您建立了平面物件，設定頂點法向量以垂直指向表面。

下圖顯示透過頂點法向量由兩個三角形組成的平面：

![透過頂點法向量由兩個三角形組成的平面](images/flatvert.png)

## <a name="span-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspansmooth-shading-on-a-non-flat-object"></a><span id="Smooth_shading_on_a_non-flat_object"></span><span id="smooth_shading_on_a_non-flat_object"></span><span id="SMOOTH_SHADING_ON_A_NON-FLAT_OBJECT"></span>非平面物件上的平滑著色


您的物件比較可能是由三角形連環所構成，而非平面物件，而且三角形並非共面。 有一種簡單的方式可在連環中所有三角形上達到平滑著色，那就是先計算與頂點相關聯的每個多邊形面的表面法向量。 表面法向量可設定為與每個表面法向量等角。 不過，這個方法對於複雜的基本類型可能不夠有效率。

下圖顯示此方法，當中顯示的兩個表面 S1 和 S2 從上方看起來邊緣朝上。 S1 及 S2 的法向量以藍色顯示。 頂點法向量是以紅色顯示。 頂點法向量與 S1 表面法向量所呈現的角度，與 S2 的頂點法向量和表面法向量相同。 當這兩個表面亮起並採用 Gouraud Shading 時，結果會是兩者之間有平滑著色、平滑圓邊。

下圖顯示兩個表面 (S1 和 S2) 和其法向量與頂點法向量：

![兩個表面 (s1 和 s2) 與其法向量和頂點法向量](images/gvert.png)

如果頂點法向量朝向相關聯的其中一個面傾斜，會造成該表面上的點的亮度增加或減少，取決於它與光源形成的角度。 如下圖範例所示。 這些表面看起來邊緣朝上。 頂點法向量朝向 S1，使其與光源之間形成的角度較小，如果頂點法向量與表面法向量呈等角的話。

下圖顯示兩個表面 (S1 和 S2)，其中一個頂點法向量朝向一個面：

![兩個表面 (s1 和 s2)，其中有一個頂點法向量朝向一個面](images/gvert2.png)

## <a name="span-idsharpedgesspanspan-idsharpedgesspanspan-idsharpedgesspansharp-edges"></a><span id="Sharp_edges"></span><span id="sharp_edges"></span><span id="SHARP_EDGES"></span>銳利邊緣


您可以使用 Gouraud Shading 在 3D 場景中顯示一些物件的銳利邊緣。 若要這樣做，在需要銳利邊緣的面的任何交集處重複頂點法向量。

下圖顯示在銳利邊緣重複的頂點法向量：

![在銳利邊緣重複的頂點法向量](images/shade1.png)

如果您使用 DrawPrimitive 方法轉譯場景，請將有銳利邊緣的物件定義為三角形清單，而非三角形連環。 當您將物件定義為三角形連環時，Direct3D 會將它視為單一多邊形，由多個三角形面所組成。 Gouraud Shading 同時套用在多邊形的每個面以及相鄰的面之間。

結果物件會在面與面之間平滑著色。 由於三角形清單是由一系列不相鄰的三角面所組成的多邊形，因此 Direct3D 會在多邊形的每個面上套用 Gouraud Shading。 不過，它不會從一個面套用至另一個面。 如果三角形清單中有兩個以上相鄰的三角形，這些三角形之間會顯示銳利邊緣。

另一個替代方法是在轉譯有銳利邊緣的物件時改變為平面著色。 運算上來說，這是最有效率的方法，但可能會導致場景中的物件不會進行點陣化轉譯，就像套用 Gouraud 著色的物件一樣。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[座標系統與幾何](coordinate-systems-and-geometry.md)

 

 




