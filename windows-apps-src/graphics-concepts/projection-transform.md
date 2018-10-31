---
title: 投影轉換
description: 投影轉換控制相機內部，例如選擇相機的鏡頭。 這是三種轉換類型中最複雜的轉換。
ms.assetid: 378F205D-3800-4477-9820-5EBE6528B14A
keywords:
- 投影轉換
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 01a410e0e2759dcdfd6adff9c25238447fe4138b
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5811726"
---
# <a name="projection-transform"></a>投影轉換


*投影轉換*控制相機內部，例如選擇相機的鏡頭。 這是三種轉換類型中最複雜的轉換。

投影矩陣通常是縮放和透視投影。 投影轉換會將檢視範圍轉換成立方體形狀。 由於檢視範圍的近端小於遠端，這會影響靠近相機之物體的展開；這也是透視套用到場景的方式。

在[檢視範圍](viewports-and-clipping.md)中，相機和檢視範圍空間的原點之間的距離隨意定義為 D，以便投影矩陣看起來如下圖。

![投影矩陣的圖例](images/projmat1.png)

檢視矩陣會透過在 z 方向平移 - D 將相機轉移至原點，轉移矩陣就如下圖。

![轉移矩陣的圖例](images/projmat2.png)

將轉移矩陣乘上投影矩陣 (T\*P) 可提供複合投影矩陣，如下圖所示。

![複合投影矩陣的圖例](images/projmat3.png)

透視轉換會將檢視範圍轉換成新的座標空間。 請注意，範圍會變成立方體，而原點也會從場景的右上角移到中心，如下圖所示。

![透視轉換如何將檢視範圍變更為新的座標空間的圖表](images/cuboid.png)

在透視轉換中，x 方向與 y 方向的限制是 -1 及 1。 對於正面平面 z 方向的限制是 0 而對於後面平面是 1。

這個矩陣會根據從相機到近裁剪平面的指定距離平移和縮放物件，但是不會考慮視野 (fov)，而且在距離中對於物件產生的 z 值可能幾乎相同，使得難以比較深度。 下列矩陣處理這些問題，並且調整頂點以考慮檢視區的長寬比，使其成為透視投影的不錯選擇。

![透視投影的矩陣圖例](images/prjmatx1.png)

在這個矩陣中，Zₙ 是近裁剪平面的 z 值。 變數 w、h 和 Q 的意義如下。 請注意，fov<sub>w</sub>和 fovₖ 以弧度代表區檢視區的水平和垂直視野。

![變數的方程式意義](images/prjmatx2.png)

對於您的應用程式，使用視野角度來定義 x 和 y 縮放係數，可能不如使用檢視區的水平和垂直維度 (在相機空間中) 來得便利。 既然數學解出來，下列 w 和 h 的兩個方程式使用檢視區的維度，並且相當於前述方程式。

![W 和 h 變數的方程式意義](images/prjmatx3.png)

在這些公式中，Zₙ 代表近裁剪平面的位置，而 V<sub>w</sub>和 Vₕ 變數代表檢視區的寬度與高度 (照相機空間中)。

不論您決定使用哪個公式，務必將 Zₙ 設定為越大值越好，因為 z 值非常接近相機，不會差太多。 這會使得使用 16 位元 z 緩衝區的深度比較變得有些複雜。

## <a name="span-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspana-w-friendly-projection-matrix"></a><span id="A_W_Friendly_Projection_Matrix"></span><span id="a_w_friendly_projection_matrix"></span><span id="A_W_FRIENDLY_PROJECTION_MATRIX"></span>W 方便的投影矩陣


Direct3D 可使用已被世界、檢視和投影矩陣轉換的 w 元件頂點，來執行深度緩衝區的深度計算或霧的效果。 這類運算需要投影矩陣將 w 標準化為等於世界空間 z。 簡言之，如果您的投影矩陣包含不是 1 的 (3,4) 係數，您必須透過反轉 (3,4) 係數來縮放所有係數以製作適當的矩陣。 如果您不提供相符的矩陣，霧效果和深度緩衝區就不會正確套用。

下圖顯示不相符的投影矩陣和縮放的矩陣，所以將會啟用眼睛相關的霧化。

![不相符投影矩陣及具有眼睛相關霧化的矩陣圖例](images/eyerlmx.png)

在前述矩陣中，所有變數都假設為非零。 如需 w 型深度緩衝區的詳細資訊，請參閱[深度緩衝區](depth-buffers.md)。

Direct3D 使用目前以 w 型深度計算的投影矩陣。 如此一來，應用程式必須設定相符的投影矩陣，才能接收所要的 w 型功能，即使它們不使用 Direct3D 進行轉換。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[轉換](transforms.md)

 

 




