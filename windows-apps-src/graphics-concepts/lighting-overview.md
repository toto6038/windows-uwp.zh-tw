---
title: 光源概觀
description: 當您使用 Direct3D 照明時，您可以允許 Direct3D 為您處理照明細節。 如有需要，進階使用者可以自行執行照明。
ms.assetid: FCBF6A92-4EAC-4CCC-A76C-79985AF348AE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e90e460cf5f5bda7d90447440d76cf6898a83747
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8346564"
---
# <a name="lighting-overview"></a>光源概觀

當您使用 Direct3D 照明時，您可以允許 Direct3D 為您處理照明細節。 如有需要，進階使用者可以自行執行照明。

當啟用照明時，Direct3D 會根據下列組合計算每個物件頂點的色彩︰

-   紋素與紋理圖相關聯。
-   若有指定，為頂點的擴散和反射色彩。
-   色彩和光線強度由場景光源或場景環境光線層級製造。

您如何使用照明和材質，會使可呈現的場景外觀大不相同。 材質決定光線對表面的反映。 直接光線和環境光線層級定義反映的光線。 如果已啟用照明，您必須使用材料來呈現場景。

不需要用光線呈現場景，但沒有光線呈現的場景細節是無法顯現的。 呈現無光的場景，充其量會使場景中的物件產生剪影。 對大部分的用途而言，這仍不夠詳細。

## <a name="span-iddirectlightvsambientlightspanspan-iddirectlightvsambientlightspandirect-light-vs-ambient-light"></a><span id="direct_light_vs._ambient_light"></span><span id="DIRECT_LIGHT_VS._AMBIENT_LIGHT"></span>直接光線與環境光線


雖然直接和環境光線會使場景中的物件亮起來，它們彼此之間各不相干，而且使用的是非常不同的效果，所以您最好以完全不同的方式來使用。

*直接光線*直接照射物件。 直接光線一定有方向和顏色，它是陰影演算法的係數，例如 Gouraud Shading。 不同類型的光線以不同方式發出直光，以建立特殊衰減效果。

*環境光線*對場景各處都能有有效作用。 環境光線為充滿整個場景的一般光線等級，無論物件與他們在場景中的位置為何。 環境光線沒有位置或方向，只有色彩和強度。 每個光線加入場景中的整體環境光線。

環境光線的形式為 RGBA 值，其中每個元件是 0 至 255 的整數值。 這與 Direct3D 中大多數色彩值不相同。

紅色、綠色及藍色元件組合成最終環境光線的色彩。 Alpha 元件控制色彩的透明度。 使用硬體加速或 RGB 模擬時，會忽略 alpha 元件。

## <a name="span-iddirect3dlightmodelvsnaturespanspan-iddirect3dlightmodelvsnaturespandirect3d-light-model-vs-nature"></a><span id="direct3d_light_model_vs._nature"></span><span id="DIRECT3D_LIGHT_MODEL_VS._NATURE"></span>Direct3D 光線模型與大自然


在自然界，當從來源發光時，就算沒有數千或數百萬計的物件，在到達使用者的眼睛前，也會有數百種反映。 每次反映時，有些光會被表面吸收，有些隨機分散，而其餘部分不是移至另一個表面，就是移至使用者的眼睛。 這個程序會繼續，直到光線縮小變沒有，或使用者感受到光線。

很明顯的，需要經過計算才能完美模擬光線的行為，用於即時 Direct3D 圖形太耗時。 因此，記住速度的同時，Direct3D 光線模型近似光線在自然界運作的方式。 Direct3D 描述光線的紅色、綠色及藍色元件結合並建立最後色彩。

在 Direct3D 中，當光線反映表面，淺色與表面以數學方式互動，終而建立最後顯示在螢幕上的色彩。 關於 Direct3D 使用的特定演算法詳細資訊，請參閱[光源的數學計算](mathematics-of-lighting.md)。

Direct3D 光線模型可以將光線概括成兩種類型︰ 環境光線和直接光線。 每一個都有不同屬性，而每一個會用不同的方式與表面材質互動。 環境光線是已經分散到方向和來源都變得不確定的光線︰ 它在各處會維持低強度。 攝影師間接使用的照明是使用環境光線的範例。

Direct3D 的環境光線如自然界光線一樣沒有真實方向或來源，只有色彩和強度。 事實上，環境光線等級與產生光線之場景中任何物件完全無關。 環境光線不會有反射反映。

直接光線是由場景內的來源產生的，永遠具有色彩和強度，並以指定方向行進。 直接光線與表面材質互動來建立反射高光，而其方向在陰影運算中被當成係數來使用，包括 Gouraud shading。 當反映直接光線時，並不會作用成場景中的環境光線等級。 場景中產生直接光線的來源具備不同特性，影響他們照亮場景的方式。

除此之外，多邊形的材質具有會影響多邊形如何反映接收到的光線的屬性。 您設定單一反射率特徵，其描述材質如何反映環境光線，而您設定個別特徵來判斷材質的反射和擴散反射率。

## <a name="span-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspancolor-values-for-lights-and-materials"></a><span id="Color_Values_for_Lights_and_Materials"></span><span id="color_values_for_lights_and_materials"></span><span id="COLOR_VALUES_FOR_LIGHTS_AND_MATERIALS"></span>光線和材質的色彩值


Direct3D 描述結合成最後色彩的四個元件 (紅色、綠色、藍色及 alpha) 的色彩。 每個元件範圍從 0.0 到 1.0。 雖然光線和材質使用相同的結構來描述色彩，光線和材質使用值的方式有點不同。

光源的色彩值表示其發出的特定光線元件量。 由於光線不使用 alpha 元件，只有紅色、綠色和藍色的色彩元件相關。 您可以在投影電視上視覺化三個元件，分別為紅色、綠色和藍色濾鏡。 每個濾鏡可能會關閉 (適當成員的 0.0 值)，可能會非常亮 (1.0 值)，或介於之間。

透過濾鏡將色彩結合成光線的最終色彩。 R(1.0)、G(1.0)、 B(1.0) 組合成白光，R(0.0)、G(0.0)、B(0.0) 完全無法發光。 您可以發出只有一個元件的光線，例如產生純紅色、綠色或藍色光；光線無法使用組合來發出像黃色或紫色的色彩。 您甚至可以設定負片色彩元件值，建立「深色光」，實際上是將場景的光線移除。 或者，您可能設定元件的值大於 1.0 以建立非常亮的光。

另一方面，在使用材質時，色彩值代表使用該材質的表面反映出多少光線元件。 色彩元件為 R(1.0)、G(1.0)、B(1.0) 和 A(1.0) 的材質，反映了所有的光線。 同樣地，R(0.0)、G(1.0)、B(0.0)、A(1.0) 反映出所有照射而來的綠光。 材質有多個反射值，可建立各種效果。

請參閱[光線類型](light-types.md)和[光線屬性](light-properties.md)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[光線和材料](lights-and-materials.md)

 

 




