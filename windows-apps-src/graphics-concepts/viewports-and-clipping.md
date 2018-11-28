---
title: 檢視區和裁剪
description: 檢視區是 3D 場景投影到此的二維 (2D) 矩形。
ms.assetid: D0DD646E-13AE-452A-AD22-8C35000D0BA9
keywords:
- 檢視區和裁剪
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7d2a8953d202cc22729f99a096b5fb62cf1131d9
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7833069"
---
# <a name="viewports-and-clipping"></a>檢視區和裁剪


「檢視區」** 是 3D 場景的投影目標二維 (2D) 矩形。 在 Direct3D 中，矩形在 Direct3D 表面中以座標的形式存在，系統會使用矩形作為轉譯目標。 投影轉換將頂點轉換至用於檢視區的座標系統。 檢視區也用於指定轉譯目標表面 (場景轉譯目標) 上深度值的範圍 (通常是 0.0 到 1.0)。

## <a name="span-idtheviewingfrustumspanspan-idtheviewingfrustumspanspan-idtheviewingfrustumspanthe-viewing-frustum"></a><span id="The_Viewing_Frustum"></span><span id="the_viewing_frustum"></span><span id="THE_VIEWING_FRUSTUM"></span>檢視範圍


檢視範圍是場景中相對於檢視區相機放置的 3D 體積。 體積形狀影響模型如何從相機空間投影到螢幕。 最常見的投射類型，透視投影，負責讓靠近相機的物件看起來比遠方物件更大。 針對透視檢視，檢視範圍可視為金字塔，相機位於塔頂，如下圖所示。 此金字塔與前端和背景裁剪平面相交。 金字塔內前端和背景裁剪平面之間的體積是檢視範圍。 唯有在這個體積中，物件才會顯示。

![前端和背景裁剪平面之間檢視範圍的圖例](images/frustum.png)

如果您想像站在暗房裡，並看出方窗外，就會視覺化檢視範圍。 在這個類比，近裁剪平面是窗戶，背景裁剪平面是最後中斷檢視的任何東西 - 對街的摩天大樓、遠方山脈，或空無一物。 您可以看到被截斷金字塔內的所有東西，從窗戶開始到中斷檢視的任何東西結束，而且您看不到其他任何東西。

檢視範圍是由 fov（視野），以及前端和背景裁剪平面之間的距離 (在 z 座標中指定) 所定義，如下圖顯示。

![檢視範圍圖](images/fovdiag.png)

在這個圖，變數 D 是從相機到空間原點 (此空間定義在幾何管線最後一部分 - 檢視轉換) 之間的距離。 圍繞這個空間，您排列檢視範圍的限制。 有關如何使用這個 D 變數建立投影矩陣，請參閱[投影轉換](projection-transform.md)

## <a name="span-idviewportrectanglespanspan-idviewportrectanglespanspan-idviewportrectanglespanviewport-rectangle"></a><span id="Viewport_Rectangle"></span><span id="viewport_rectangle"></span><span id="VIEWPORT_RECTANGLE"></span>檢視區矩形


檢視區結構包含 4 個成員 (X、Y、寬度、高度)，定義轉譯目標表面 (場景轉譯目標) 的區域。 這些值對應目的地矩形或檢視區矩形，如下圖顯示。

![檢視區矩形圖](images/destrect.png)

指定的 X、Y、寬度、高度成員值是相對於轉譯目標表面左上角的螢幕座標。 結構定義兩個額外成員（MinZ 和 MaxZ），表示場景轉譯到的深度範圍。

Direct3D 假設檢視區裁剪體積的範圍是從 -1.0 到 1.0 (在 X 中) 以及從 1.0 到 -1.0 (在 Y 中)。這些是應用程式過去最常使用的設定。 裁剪之前，您可以使用[投影轉換](projection-transform.md)調整檢視區的長寬比例。

**注意：**  MinZ 和 MaxZ 表示場景轉譯到此的深度範圍並不會用於裁剪。 大部分的應用程式將這些值設為 0.0 和 1.0，以讓系統轉譯到深度緩衝區中深度值的整個範圍。 有時候，您可以使用其他深度範圍達成特殊效果。 例如，若要在遊戲中轉譯平視顯示器，您可以將這兩個值設定為 0.0，強迫在場景的前景中轉譯物件，或者您可以將這兩個值設定為 1.0，轉譯永遠應該會在背景中的物件。

 

檢視區結構的 X、Y、寬度、高度成員所使用的維度，會定義轉譯目標表面上檢視區的位置和維度。 這些值是在螢幕座標中，相對於表面左上角。

Direct3D 使用檢視區位置和維度來縮放頂點，讓轉譯的場景放在目標表面上的適當位置。 內部，Direct3D 會將這些值插入下列套用到每個頂點的矩陣中。

![套用到每個頂點之矩陣的方程式](images/vpscale.png)

這個矩陣依據檢視區維度和您想要的深度範圍縮放頂點，並將它們轉移到轉譯目標表面上的適當位置。 矩陣也翻轉 y 座標，隨著 y 向下漸增，以反映左上角的螢幕原點。 套用此矩陣之後，頂點仍然是同質 - 也就是它們仍是 \[x,y,z,w\] 頂點 - 而且在傳送到轉譯器之前，必須轉換成非同質座標。

**注意：** 應用程式通常將 MinZ 和 MaxZ 為 0.0 和 1.0，分別，以便讓系統轉譯到整個深度範圍。 不過，您可以使用其他值達到特定效果。 例如，您可以將這兩個值設為 0.0，強制所有物件到前景，或都設為 1.0 將所有物件轉譯到背景。

 

## <a name="span-idclearingaviewportspanspan-idclearingaviewportspanspan-idclearingaviewportspanclearing-a-viewport"></a><span id="Clearing_a_Viewport"></span><span id="clearing_a_viewport"></span><span id="CLEARING_A_VIEWPORT"></span>清除檢視區


清除檢視區會重設轉譯目標表面上檢視區矩形的內容。 它也會清除深度和樣板緩衝區表面中的矩形。

## <a name="span-idsetuptheviewportforclippingspanspan-idsetuptheviewportforclippingspanspan-idsetuptheviewportforclippingspanset-up-the-viewport-for-clipping"></a><span id="Set_Up_the_Viewport_for_Clipping"></span><span id="set_up_the_viewport_for_clipping"></span><span id="SET_UP_THE_VIEWPORT_FOR_CLIPPING"></span>設定裁剪的檢視區


投影矩陣的結果決定投影空間中的裁剪體積為：

-w<sub>c</sub>&lt;= x<sub>c</sub>&lt;= w<sub>c</sub>

-w<sub>c</sub>&lt;= y<sub>c</sub>&lt;= w<sub>c</sub>

0 &lt;= z<sub>c</sub>&lt;= w<sub>c</sub>

其中：x、y 和 w 代表套用投影轉換後的頂點座標。 x、y 或 z 元件在這些範圍外的任何頂點都會被裁剪，如果裁剪已啟用（預設行為）。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[座標系統與幾何](coordinate-systems-and-geometry.md)

 

 




