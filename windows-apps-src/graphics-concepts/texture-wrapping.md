---
title: 紋理包裝
description: 紋理包裝可藉由對每個頂點明確指定其紋理座標，以變更 Direct3D 點陣化已貼上紋理多邊形的基本方式。
ms.assetid: C28FB369-9A91-4D57-A96D-4A5D36484B35
keywords:
- 紋理包裝
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: bd24fc0ad14657e503feeab2a89ec7e6d4eb7ee0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044437"
---
# <a name="texture-wrapping"></a>紋理包裝


紋理包裝可藉由對每個頂點明確指定其紋理座標，以變更 Direct3D 點陣化已貼上紋理多邊形的基本方式。 點陣化多邊形時，系統會藉由在多邊形每個頂點的紋理座標間進行插補，以決定多邊形的每個像素會使用哪些材質。 一般來說，系統會將紋理視為 2D 平面，並藉著在紋理中選擇 A 點到 B 點之間最短的路徑以插入新的材質。若 A 點代表座標為 (0.8, 0.1) 的 u, v 位置， B 點則是 (0.1, 0.1)，則插補線會類似下列簡圖。

![兩個點之間插補線的簡圖](images/interp1.png)

注意：在此圖例中，A 點與 B 點之間最短的距離會約略通過紋理中間。 啟用 u 紋理或 v 紋理座標包裝，可變更 Direct3D 計算 u 方向與 v 方向上紋理座標之間最短距離的方式。 根據定義，假設 0.0 與 1.0 同時存在，則紋理包裝會使轉譯器在紋理座標組之間選擇最短的路徑。 最後一段是較為棘手的部分︰您可以想像在單一方向啟用紋理包裝時，會導致系統彷彿將紋理包裝在圓柱體中。 例如，設想下列簡圖：

![一個紋理及兩個點包裝在一個圓柱體中的簡圖](images/interp2.png)

上述圖例呈現了在 u 方向上進行包裝時，影響系統於紋理座標間插補的情況。 在一般或是未包裝的紋理中使用與範例中相同的點，您可以看到 A 點與 B 點之間最短的距離不再通過紋理的中間了，而是通過 0.0 與 1.0 同時存在的邊框。 在 v 方向上包裝時情況與先前的範例類似，只是紋理會包裝在躺臥在其側邊的圓柱體中。 在 u 方向 v 方向上同時進行包裝會更複雜。 此時，您可將紋理構想成環面體或是甜甜圈。

紋理包裝最常見的實際應用就是用來執行環境貼圖。 通常，物件的紋理若為環境貼圖，其本身會帶有強烈的反射性，即完全呈現物件周遭環境的鏡映影像。 為了更有效地進行討論，您可以想像一間有著四面牆的房間，每一面牆上都各自寫著 R、G、B，及 Y，並且繪有相對的顏色：紅色、綠色、藍色，及黃色。 該房間的環境貼圖可能如下列圖例所示。

![紅色、綠色、藍色，及黃色條文的圖例](images/envmap.png)

想像一根有著四個完全反射面的柱子支撐著房間的天花板。 將環境貼圖紋理貼至柱子本身相當簡單，然而若要試圖使柱子看起來就像是反射了牆上的字和顏色，相較之下就比較困難。 以下簡圖呈現了在接近頂部頂點處列出適用紋理座標柱子的框線圖。 虛線表示包裝會超過紋理邊緣的接縫處。

![帶有二等分線矩形的簡圖](images/seam.png)

當包裝於 u 方向啟用時，已貼上紋理的柱子適當地呈現了環境貼圖中的色彩與符號，而在紋理前端的接縫處，轉譯器也適當地在紋理座標間選擇了最短的路徑。(假設 u 座標 0.0 與 1.0 共用相同的位置。) 已貼上紋理的柱子外觀如下。

![由紅色、綠色、藍色，及黃色四等分色塊組成之柱子的圖例](images/tex-seam.png)

若不啟用紋理包裝，轉譯器就不會在必要的方向上進行插補，呈現出寫實的反射影像。 而是在柱子的前方呈現 u 座標 0.175 及 0.875 間紋理的水平壓縮版本，因為其通過了紋理的中心。 包裝效果因此失去作用。

請勿將「紋理包裝」與有著相似名稱的「紋理定址模式」混淆。 紋理包裝會在紋理定址之前執行。 請務必確認紋理包裝資料不包含任何位於 \[0.0, 1.0\] 範圍之外的紋理座標，因為會產生未定義的結果。 如需有關紋理定址的更多資訊，請參閱[紋理定址模式](texture-addressing-modes.md)。

## <a name="span-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspandisplacement-map-wrapping"></a><span id="Displacement_Map_Wrapping"></span><span id="displacement_map_wrapping"></span><span id="DISPLACEMENT_MAP_WRAPPING"></span>置換貼圖包裝


置換貼圖由花格引擎進行插補。 由於花格引擎無法指定包裝模式，紋理包裝無法在置換貼圖上使用。 應用程式可選擇使用一組強制插補於任一方向進行的頂點。 應用程式也可指定插補使用簡單線性插補完成。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[紋理](textures.md)

 

 




