---
title: 向量、頂點和四元數
description: 在整個 Direct3D 中，頂點描述位置及方向。 基本類型中的每個頂點都由給予它位置、色彩、紋理座標的向量所述，以及給予它方向的標準向量所述。
ms.assetid: 94EC3D59-43FC-4509-A233-916E9FA8381E
keywords:
- 向量、頂點和四元數
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8942e53b7372e2e8b3cf4ed05f89b4187bdfc4be
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "7989205"
---
# <a name="vectors-vertices-and-quaternions"></a>向量、頂點和四元數


在整個 Direct3D 中，頂點描述位置及方向。 基本類型中的每個頂點都由給予它位置、色彩、紋理座標的向量所述，以及給予它方向的標準向量所述。

四元數在定義三元件向量的 \[x, y, z\] 值加入第四個元素。 四元數是通常用於 3D 旋轉之矩陣方法的替代方法。 四元數代表 3D 空間中的軸以及圍繞該軸的旋轉。 例如，四元數可能表示 (1,1,2) 軸及 1 弧度旋轉。 四元數載有重要資訊，但其真正的力量來自於您可以在其上執行兩項作業︰組合和內插補點。

對四元數執行組合類似於結合它們。 兩個四元數的組合如下圖表示。

![四元數標記法的圖例](images/quateq.png)

兩個套用到幾何之四元數的組合表示「圍繞軸₂ 和旋轉角度₂ 旋轉幾何，然後圍繞軸₁ 和旋轉角度₁ 旋轉幾何。」 在此情況下，Q 代表圍繞單一軸的旋轉，這是先套用 q₂ 再套用 q₁ 到幾何的結果。

使用四元數內插補點，應用程式可以計算從某個軸和方向到另一個軸和方向順暢且合理的路徑。 因此，q₁ 和 q₂ 之間內插補點提供從某個方向動畫到另一個方向的簡單方法。

當您一起使用組合和內插補點時，它讓您輕鬆進行看似複雜的幾何操作。 例如，想像您有想要旋轉到特定方向的幾何。 您知道您想要將它圍繞軸₂ 旋轉 r₂ 度，然後圍繞軸₁ 旋轉 r₁ 度，但您不知道最終四元數。 使用組合，您可能會在幾何上組合兩個旋轉，取得單一四元數結果。 然後，您可以從原始四元數到組合四元數內插值，達到平滑轉換。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[座標系統與幾何](coordinate-systems-and-geometry.md)

 

 




