---
title: 光線屬性
description: 光線屬性描述光線類型 (點、方向、聚光燈)、衰減、色彩、方向、位置及範圍。
ms.assetid: E832C3FD-9921-41C4-87B8-056E16B61B77
keywords:
- 光線屬性
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 07a465d8fdcd1d425ed62e8d83cadd261f316da2
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5862343"
---
# <a name="light-properties"></a>光線屬性


光線屬性描述光線類型 (點、方向、聚光燈)、衰減、色彩、方向、位置及範圍。 根據使用的光線類型，光線可有衰減和範圍或聚光燈特效屬性。 並非所有光線類型均使用所有的屬性。

位置、範圍及衰減屬性定義現世空間裡的照明位置，並且定義如何經由距離發出光線的行為。

## <a name="span-idlightattenuationspanspan-idlightattenuationspanspan-idlightattenuationspanlight-attenuation"></a><span id="Light_Attenuation"></span><span id="light_attenuation"></span><span id="LIGHT_ATTENUATION"></span>光線衰減


衰減控制光線強度如何隨著範圍屬性指定的最大距離減少。 三個浮點值有時會用於代表光線衰減︰Attenuation0、Attenuation1 及 Attenuation2。 這些浮點值範圍從 0.0 到無限大來控制光線衰減。 某些應用程式設定 Attenuation1 成員至 1.0，其他成員至 0.0，導致光線強度變更為 1 / D，其中 D 是光源到頂點的距離。 最大光線強度在於來源，在光線的距離減至 1 (光線範圍)。

雖然應用程式通常將 Attenuation0 設為 0.0，將 Attenuation1 設為常數值，將 Attenuation2 設為 0.0，透過改變這些值可獲得不同的光線效果。 您可以合併衰減值，以獲得更複雜的衰減效果。 或者可將這些值設為一般範圍以外的值，以建立更怪異的衰減效果。 但不接受負衰減值。 參閱[衰減和聚光燈係數](attenuation-and-spotlight-factor.md)。

## <a name="span-idlightcolorspanspan-idlightcolorspanspan-idlightcolorspanlight-color"></a><span id="Light_Color"></span><span id="light_color"></span><span id="LIGHT_COLOR"></span>淺色


在 Direct3D 光線發出三種顏色，其在系統照明運算中各自獨立運用︰ 擴散色彩、環境色彩及反射色彩。 每個由 Direct3D 結合在一起的照明模組，與目前材質對應互動，以製作轉譯中使用的最後色彩。 擴散色彩與目前材質的擴散反射率屬性互動，反射色彩與材質反射的反射率屬性互動，依此類推。 如需有關 Direct3D 如何套用這些色彩的詳細資訊，請參閱[光源的數學計算](mathematics-of-lighting.md)。

在 Direct3D 應用程式中，通常有三種色彩值 - 擴散、環境及反射，其會定義發出的色彩。

最常運用系統運算的色彩類型是擴散色彩。 最常見的擴散色彩為白色 (R:1.0 G:1.0 B:1.0)，但是您可以依需求建立色彩來達到所需的效果。 例如，壁爐可以使用紅光，或者也可以將交通號誌的綠燈設定為「執行」(Go)。

一般而言，您將淺色元件值設定為 0.0 到 1.0 (含) 之間，但這不是必需。 例如，您可能會將所有元件設為 2.0，建立比白色亮的光線。 這類設定在您使用衰減常數以外的設定時尤其管用。

雖然 Direct3D 使用光線的 RGBA 值，但不會使用 alpha 色彩元件。

通常會使用照明材質色彩。 不過，您可以指定材質發色、環境、擴散及反射是由擴散或反射頂點色彩覆寫。

Alpha/透明度值永遠只來自擴散色彩 alpha 色板。

霧值永遠只來自反射色彩 alpha 色板。

## <a name="span-idlightdirectionspanspan-idlightdirectionspanspan-idlightdirectionspanlight-direction"></a><span id="Light_Direction"></span><span id="light_direction"></span><span id="LIGHT_DIRECTION"></span>光方向


光方向屬性決定了移動物件在自然空間發出的光方向。 方向只由方向燈和聚光燈使用，而且只用向量描述。

設定為向量的光方向。 無論場景中的光線位置為何，從邏輯原點描述方向向量為距離。 因此，只要直接將聚光燈隨正 z 軸指向場景，不論其位置定義為何，均具有 &lt;0,0,1&gt; 方向向量。 同樣地，您可以透過使用的方向燈 (方向為 &lt;0,-1,0&gt;) 模擬陽光直接照射場景。 您不需要建立沿著座標軸發亮的光線，您可以混合並配對值，以建立更多有趣角度的光線。

雖然您不需要將光線方向向量正常化，您隨時都確定它的級數。 換言之，請不要使用 &lt;0,0,0&gt; 方向向量。

## <a name="span-idlightpositionspanspan-idlightpositionspanspan-idlightpositionspanlight-position"></a><span id="Light_Position"></span><span id="light_position"></span><span id="LIGHT_POSITION"></span>光線位置


使用向量結構描述光線位置。 x、y 和 z 座標均假定為在自然空間中。 方向燈是唯一不使用位置屬性的光線類型。

## <a name="span-idlightrangespanspan-idlightrangespanspan-idlightrangespanlight-range"></a><span id="Light_Range"></span><span id="light_range"></span><span id="LIGHT_RANGE"></span>光線範圍


光線範圍屬性決定自然空間中的距離，而場景中網狀結構在此距離不會再接收該物件發出的光線。 方向燈不使用範圍屬性。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[光線和材料](lights-and-materials.md)

 

 




