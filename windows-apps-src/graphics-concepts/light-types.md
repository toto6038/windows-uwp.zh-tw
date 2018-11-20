---
title: 光源類型
description: 光源類型屬性定義您正在使用的光源類型。 Direct3D 中有三種光線類型 - 點狀光源、聚光燈和方向燈。
ms.assetid: 57748CAF-6F08-4D1D-9ED6-8FAA8C5FE314
keywords:
- 光源類型
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9898a050131813b7b2431f8fc11397eee7c7942c
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2018
ms.locfileid: "7446067"
---
# <a name="light-types"></a>光源類型


光源類型屬性定義您正在使用的光源類型。 Direct3D 中有三種光線類型 - 點狀光源、聚光燈和方向燈。 每個類型運用不同等級的計算負荷，以不同方式照亮場景中的物件。

## <a name="span-idpointlightspanspan-idpointlightspanspan-idpointlightspanpoint-light"></a><span id="Point_Light"></span><span id="point_light"></span><span id="POINT_LIGHT"></span>點狀光源


點狀光源在場景中有色彩和位置，但無單一方向。 他們在所有方向發出同樣的光，如下圖所示。

![點狀光源的圖例](images/ptlight.png)

燈泡是使用點狀光源的好範例。 點狀光源會受到衰減和範圍的影片，並逐頂點照亮網格。 在照明期間，Direct3D 在世界空間使用點狀光源位置及頂點座標，點亮以衍生光線的方向向量，以及光線行進的距離。 並隨著頂點法線使用兩者，以計算光線對表面照明的貢獻。

## <a name="span-iddirectionallightspanspan-iddirectionallightspanspan-iddirectionallightspandirectional-light"></a><span id="Directional_Light"></span><span id="directional_light"></span><span id="DIRECTIONAL_LIGHT"></span>方向光源


方向光源有只色彩和方向，不含位置。 他們發出平行光線。 這表示方向光源產生的所有光線，均透過場景同方向行進。 請想像方向光線為附近無限距離的光源，例如陽光。 方向光源不受衰減或範圍的影響，因此當 Direct3D 計算頂點色彩時，您指定的方向與色彩為認定的唯一係數。 由於照明係數少，這些都是可使用的最少計算密集型光線。

## <a name="span-idspotlightspanspan-idspotlightspanspan-idspotlightspanspotlight"></a><span id="SpotLight"></span><span id="spotlight"></span><span id="SPOTLIGHT"></span>聚光燈


聚光燈有色彩、位置及發光方向。 從聚光燈發出的光線由明亮內錐和較大的外錐體組成，兩者之間的光照強度減弱，如下圖所示。

![具內錐和外錐體的聚光燈圖例](images/spotlt.png)

聚光燈受衰退、衰減及範圍的影片。 這些係數及行進至每個頂點的距離，會在計算場景中的物件照明效果時計入。 為每個頂點運算這些效果，使聚光燈成為 Direct3D 所有光線中運算時間最長者。

衰退、Theta 和 Phi 值僅供聚光燈使用。 這些值控制聚光燈物件的內錐或外錐體的有多大或多小，以及兩者之間的光線如何減弱。

Theta 是聚光燈的內錐體的弧度角度，Phi 值是光線外錐體的角度。 衰退控制內錐的外緣和外錐的內緣之間的光線強度如何減弱。 大部分應用程式將衰退設定為 1.0 以建立兩個錐體之間平均發生的衰退，但您可以視需要設定其他值。

下圖顯示這些值之間的關係，以及其如何影響聚光燈之光線內錐和外錐。

![了解 phi 值和 theta 值如何與聚光燈錐體相關聯](images/spotlt2.png)

聚光燈發出的光線錐體分兩部分︰明亮的內錐和外錐體。 光線是內錐體中最明亮者，且未出現在外錐體外面，而兩個區域之間的亮度衰減中。 這種類型的衰減通常稱為衰退。

頂點接收的光線量是以內錐和外錐體中頂點位置為基礎。 Direct3D 計算聚光燈之方向向量 (L) 的內積，和光線到頂點 (D) 之向量。 這個值等於兩個向量之間的餘弦角，充當頂點位置的指標，可與光線圓錐體角度做比較，以判斷頂點可能會位於內錐或外錐中的何處。 下圖提供圖形表示這兩個向量之間的關聯。

![聚光燈方向向量和頂點到聚光燈向量的圖例](images/spotalg1.png)

系統會比較此值和聚光燈之內錐和外錐角度的餘弦。 光線的 Theta 和 Phi 值代表內錐和外錐的總圓錐體角度。 當頂點離照明中心更遠 (而不是跨總圓錐角度) 便會發生衰減，因為執行階段將這些圓錐角度分成一半。

如果向量 L 和 D 的內積小於或等於外錐體角度，頂點落外錐以外，且收不到光線。 如果 L 和 D 的內積大於內錐角的餘弦，而頂點位於內錐之內，且收到最大光線量，仍要思考因距離而衰減情況。 如果頂點落在兩個地區之間某處，則可使用下列方程式計算衰退。

![頂點亮度公式，衰退之後](images/falloff.png)

其中：

-   如<sub>果</sub>是衰退之後亮度
-   Alpha 是向量 L 和 D 之間的角度
-   Theta 是內錐角度
-   Phi 是外錐角度
-   P 是衰退

這個公式產生 0.0 和 1.0 之間的值，其調整頂點的光線強度，以將衰退考慮進去。 也會套用頂點離光線之距離的衰減係數。 下圖顯示不同衰退可能會影響衰退曲線。

![光線強度的圖表與頂點離光線的距離](images/fallgraf.png)

實際照明影響各種衰退值的效果很細微，可藉由塑造 1.0 以外的衰退值衰退曲線引發小效能罰則。 基於這些原因，此值通常設為 1.0。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[光線和材料](lights-and-materials.md)

 

 




