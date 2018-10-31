---
title: 紋理定址模式
description: Direct3D 應用程式可為任何原始物件的任何頂點指派紋理座標。
ms.assetid: 925E8F2E-43EC-404E-8870-03E39155F697
keywords:
- 紋理定址模式
- 覆蓋紋理位址模式
- 鏡像紋理位址模式
- 鉗位紋理位址模式
- 邊框色彩紋理位址模式
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0e817dcc92741ca2e738784f387cfe49399a108c
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5821059"
---
# <a name="texture-addressing-modes"></a>紋理定址模式


Direct3D 應用程式可為任何原始物件的任何頂點指派紋理座標。 通常您指派給頂點的 u 及 v 紋理座標，都會位於 0.0 到 1.0 (含) 之間。 不過，藉由指派在該範圍之外的紋理座標，您可以建立特殊的紋理效果。 .

您可以透過設定紋理定址模式，控制 Direct3D 處理位於 \[0.0, 1.0\] 範圍之外紋理座標的方式。 例如：您可以為您的應用程式設定紋理定址模式，讓紋理並排在原始物件之上。

Direct3D 允許應用程式執行紋理包裹。 請參閱[紋理包裹](texture-wrapping.md)。

啟用紋理包裹會使位於 \[0.0, 1.0\] 範圍之外的紋理座標無效化，並且該無效座標的點陣化行為將會呈現未定義的狀態。 當啟用紋理包裹時，將不會使用紋理定址模式。 請注意：當您啟用紋理包裹時，應用程式將不會指定低於 0.0 或高於 1.0 的紋理座標。

## <a name="span-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspansummary-of-the-texture-addressing-modes"></a><span id="Summary_of_the_texture_addressing_modes"></span><span id="summary_of_the_texture_addressing_modes"></span><span id="SUMMARY_OF_THE_TEXTURE_ADDRESSING_MODES"></span>紋理定址模式摘要


| 紋理定址模式 | 描述                                                                                                                           |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| 覆蓋                    | 在每個整數的連接點重複紋理。                                                                                        |
| 鏡像                  | 在每個整數的邊界處鏡像翻轉紋理。                                                                                        |
| 鉗位                   | 將紋理限制在 \[0.0, 1.0\] 的範圍內。鉗位模式會套用紋理一次，並且將邊緣像素的色彩延伸。 |
| 邊框色彩            | 為任何位於 0.0 到 1.0 (含) 範圍之外的紋理座標強制套用*邊框色彩*。                         |

 

## <a name="span-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanwrap-texture-address-mode"></a><span id="Wrap_texture_address_mode"></span><span id="wrap_texture_address_mode"></span><span id="WRAP_TEXTURE_ADDRESS_MODE"></span>覆蓋紋理位址模式


覆蓋紋理位址模式可使 Direct3D 在每個整數的連接點重複紋理。

例如：假設您的應用程式建立了一個正方形的原始物件，並且指定其四個頂點座標分別為 (0.0, 0.0)、(0.0, 3.0)、(3.0, 3.0)，及 (3.0, 0.0)。 若您將紋理定址模式設定為「覆蓋」，則會使程式在 u 及 v 方向各套用紋理三次，如下圖例所示。

![在 u 及 v 方向覆蓋臉部紋理之圖例](images/wrap.png)

與**鏡像紋理位址模式**比較。

## <a name="span-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanmirror-texture-address-mode"></a><span id="Mirror_texture_address_mode"></span><span id="mirror_texture_address_mode"></span><span id="MIRROR_TEXTURE_ADDRESS_MODE"></span>鏡像紋理位址模式


鏡像紋理位址模式可使 Direct3D 在每個整數的邊界處鏡像翻轉紋理。

例如：假設您的應用程式建立了一個正方形的原始物件，並且指定其四個頂點座標分別為 (0.0, 0.0)、(0.0, 3.0)、(3.0, 3.0)，及 (3.0, 0.0)。 若您將紋理定址模式設定為「鏡像」，則會使程式在 u 及 v 方向各套用「鏡像翻轉」的紋理三次。 每一欄與每一列套用的紋理，都是前一欄或前一列紋理的鏡映影像，如下圖例所示。

![3x3 格線中鏡映影像的圖例](images/mirror.png)

與先前的**覆蓋紋理位址模式**比較。

## <a name="span-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanclamp-texture-address-mode"></a><span id="Clamp_texture_address_mode"></span><span id="clamp_texture_address_mode"></span><span id="CLAMP_TEXTURE_ADDRESS_MODE"></span>鉗位紋理位址模式


鉗位紋理位址模式可使 Direct3D 將紋理限制在 \[0.0, 1.0\] 的範圍內。鉗位模式會套用紋理一次，並且將邊緣像素的色彩延伸。

例如：假設您的應用程式建立了一個正方形的原始物件，並且指定其四個頂點座標分別為 (0.0, 0.0)、(0.0, 3.0)、(3.0, 3.0)，及 (3.0, 0.0)。 若您將紋理定址模式設為「鉗位」，則會使程式將紋理套用一次。 在欄頂端及列末端的像素色彩會分別延伸至原始物件的頂端及右端。

以下圖例為一個鉗位紋理。

![紋理及鉗位紋理的圖例](images/clamp.png)

## <a name="span-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanborder-color-texture-address-mode"></a><span id="Border_Color_texture_address_mode"></span><span id="border_color_texture_address_mode"></span><span id="BORDER_COLOR_TEXTURE_ADDRESS_MODE"></span>邊框色彩紋理位址模式


邊框色彩紋理位址模式可使 Direct3D 為任何位於 0.0 到 1.0 (含) 範圍之外的紋理座標套用一種稱作 *「邊框色彩」* 的強制色彩。

在以下圖例中，應用程式指定紋理使用紅色的邊框並套用至原始物件。

![紋理及紅色邊框紋理的圖例](images/border.png)

## <a name="span-iddevicelimitationsspanspan-iddevicelimitationsspanspan-iddevicelimitationsspandevice-limitations"></a><span id="Device_Limitations"></span><span id="device_limitations"></span><span id="DEVICE_LIMITATIONS"></span>裝置限制


雖然系統一般會允許位在 0.0 和 1.0 (含) 範圍之外的紋理座標，硬體限制通常會影響到在該範圍之外紋理座標的最遠距離。 當您試圖擷取裝置的能力時，轉譯裝置會與裝置允許的紋理座標範圍限制通訊。

例如：若該值為 128，輸入的紋理座標就必須維持在 -128.0 到 128.0 的範圍之中。 傳送此範圍之外的頂點紋理座標無效。 該項限制同樣適用於自動紋理座標生成產生出的紋理座標，以及紋理座標轉換。

紋理重複限制則取決於紋理座標索引的紋理大小。 在這種情況下，假設紋理維度為 32，並且裝置允許的紋理座標範圍是 512，則實際有效的紋理座標範圍將會是 512/32 = 16，故此裝置的紋理座標必須位在 -16.0 到 +16.0 的範圍中。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[紋理](textures.md)

 

 




