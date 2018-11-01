---
title: 點陣化規則簡介
description: 通常，為頂點指定的點不完全符合螢幕上的像素。 當發生此狀況，Direct3D 會套用點陣化規則來判斷哪些像素適用於指定三角形。
ms.assetid: 4232CDBA-F669-4417-9378-F9013E83462C
keywords:
- 點陣化規則簡介
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 65522195b9729ddd4f2ebeb193f43c905359eda2
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5942122"
---
# <a name="introduction-to-rasterization-rules"></a>點陣化規則簡介


通常，為頂點指定的點不完全符合螢幕上的像素。 當發生此狀況，Direct3D 會套用點陣化規則來判斷哪些像素適用於指定三角形。

這是點陣化規則的簡介。 如需詳細資訊，請參閱[點陣化規則](rasterization-rules.md)。 另請參閱[轉譯器 (RS) 階段](rasterizer-stage--rs-.md)。

## <a name="span-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspantriangle-rasterization-rules"></a><span id="Triangle_Rasterization_Rules"></span><span id="triangle_rasterization_rules"></span><span id="TRIANGLE_RASTERIZATION_RULES"></span>三角形點陣化規則


Direct3D 針對填充幾何使用左上角填充慣例。 這是針對 GDI 和 OpenGL 矩形中使用的相同慣例。 在 Direct3D 中，像素的中心是決定點。 如果中心在三角形內部，像素是三角形的一部分。 像素中心位於整數座標。

這個 Direct3D 使用的三角形點陣化規則描述，不一定適用於所有可用的硬體。 您的測試可能會發現這些規則實作的次要變化。

下圖顯示左上角位於 (0, 0) ，右下角位於 (5, 5) 的矩形。 如您所預期，這個矩形填滿 25 個像素。 矩形的寬度定義為右減左。 高度被定義為上減下。

![已編號的方塊分成六個列與欄](images/pixmap.png)

在左上填充慣例，*上*是指水平跨距的垂直位置，*左*是指跨距內像素的水平位置。 邊不可為頂邊，除是是水平方向。 一般而言，大部分三角形只有左右邊。 下圖顯示頂邊和右邊。

![已編號的方形包含兩個三角形](images/triedge.png)

左上角填充慣例判斷當三角形通過像素的中心時 Direct3D 會採取的動作。 下圖顯示兩個三角形，一個在 (0, 0)、(5, 0) 和 (5, 5)，另一個在 (0, 5)、(0, 0) 和 (5, 5)。 在本案例中第一個三角形取得 15 像素 (顯示黑色)，第二個僅取得 10 像素 (顯示灰色)，因為的共用邊是第一個三角形的左邊。

![已編號的方形顯示兩個三角形](images/twotris.png)

如果您定義在 (0.5, 0.5) 左上角的矩形，而其右下角在 (2.5, 4.5)，此矩形中心點則在 (1.5, 2.5)。 當 Direct3D 點陣化將這個矩形切成方格時，每個像素中心均明確地位於四個三角形的每個三角形內，不需要左上角填充慣例。 下圖顯示此項。 矩形中的像素依據 Direct3D 包含像素的三角形中標示。

![已編號的方塊包含分成四個三角形的矩形](images/noambig.png)

如果您移動前述圖中的矩形，其左上角便會在 (1.0, 1.0)，其右下角在 (3.0, 5.0)，而其中心點在 (2.0, 3.0)，Direct3D 會套用左上角填充慣例。 此矩形中的大多數像素跨兩個或更多三角形間的邊界，如下圖所示。

![已編號的方塊包含分成四個三角形的矩形](images/fillrule.png)

針對這兩個矩形，同一個像素會受到影響，如下圖所示。

![像素受到前述兩個編號正方形的影響](images/samepix.png)

## <a name="span-idpointandlinerulesspanspan-idpointandlinerulesspanspan-idpointandlinerulesspanpoint-and-line-rules"></a><span id="Point_and_Line_Rules"></span><span id="point_and_line_rules"></span><span id="POINT_AND_LINE_RULES"></span>點和行規則


點的呈現與點精靈相同，兩者均呈現為對齊螢幕的四邊形，因此遵守和多邊形著色演算相同的規則。

非平滑化行轉譯規則確實和是[GDI 行](https://msdn.microsoft.com/library/windows/desktop/dd145027)一樣。

## <a name="span-idpointspriterulesspanspan-idpointspriterulesspanspan-idpointspriterulesspanpoint-sprite-rules"></a><span id="Point_Sprite_Rules"></span><span id="point_sprite_rules"></span><span id="POINT_SPRITE_RULES"></span>點精靈規則


點精靈和修補程式的基本類型，就好像基本類型第一次鑲嵌至三角形，而結果三角形點陣被點陣化。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[裝置](devices.md)

[轉譯器 (RS) 階段](rasterizer-stage--rs-.md)

[點陣化規則](rasterization-rules.md)

 

 




