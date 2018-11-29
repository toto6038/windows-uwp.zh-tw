---
title: 擴散光源
description: 擴散光源取決於光方向和物件表面法向。
ms.assetid: 8AF78742-76B1-4BBB-86E3-94AE6F48B847
keywords:
- 擴散光源
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1785b06aa2217e8ec15aeaa560bd98a65522df2e
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "7987726"
---
# <a name="diffuse-lighting"></a>擴散光源


*擴散光源*取決於光方向和物件表面法向。 由於變更光方向和變更表面數字向量，擴散光源在物件表面有不同呈現。 計算擴散光源需要較長時間，因為它改變每個物件頂點，不過使用它的優點是它會遮蔽物件，並提供物件三維 (3D) 深度。

在調整任何衰減效果的光線強度之後，光源引擎會計算從頂點反射的剩餘光線有多少，假設頂點角度為垂直，事件的方向為光源。 光源引擎會針對方向光源跳至此步驟，因為它們不會因為距離而衰減。 系統會考量兩種反射類型：擴散和反射，並使用不同的公式判斷分別反射的光線量。

在計算反射光線量之後，Direct3D 會將這些新值套用至目前材質的擴散和反射的反射率屬性。 產生的色彩值會是擴散和反射元件，點陣化用來產生 Gouraud Shading 和反射強光。

擴散光源以下列方程式描述。

擴散光源 = sum\[C<sub>d</sub>\*L<sub>d</sub>\*(N<sup>.</sup>L<sub>dir</sub>)\*Atten\*Spot\]

| 參數       | 預設值 | 類型          | 描述                                                                                      |
|-----------------|---------------|---------------|--------------------------------------------------------------------------------------------------|
| 總和             | 無           | 無           | 每道光的擴散元件的總和。                                                     |
| C<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | 擴散色彩。                                                                                   |
| L<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | 光線擴散色彩。                                                                             |
| N               | 無           | D3DVECTOR     | 頂點垂直                                                                                    |
| L<sub>dir</sub> | 無           | D3DVECTOR     | 從物件頂點到光線的方向向量。                                                |
| Atten           | 無           | FLOAT         | 光線衰減。 參閱[衰減和聚光燈係數](attenuation-and-spotlight-factor.md)。 |
| Spot            | 無           | FLOAT         | 聚光燈係數。 參閱[衰減和聚光燈係數](attenuation-and-spotlight-factor.md)。  |

 

若要計算衰減 (Atten) 或聚光燈特性 (Spot)，請參閱[衰減和聚光燈係數](attenuation-and-spotlight-factor.md)。

所有的光分開處理和插補之後，擴散元件會鉗制為從 0 到 255。 產生的擴散光源值是環境、擴散及發光值的組合。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>範例


在這個範例中，物件是使用淺擴散色彩和材質擴散色彩著色。

根據方程式，為物件端點產生的色彩是材質色彩和淺色的組合。

下方兩個圖例顯示材質色彩 (灰色)，以及淺色 (鮮紅)。

![灰球體圖例](images/amb1.jpg)![紅色球體圖例](images/lightred.jpg)

下圖顯示產生的場景。 場景中唯一的物件為球體。 擴散光線計算會採用材質和淺擴散色彩，並使用內積修改光線方向和頂點直角之間的角度。 如此一來，球體的背面會隨著球體表面曲線遠離光源而變暗。

![使用擴散光源的球體圖例](images/lightd.jpg)

結合前一個範例的擴散光源與環境光源形成物體表面的整體色調。 環境光源形成整個表面的色調，擴散光源則幫助顯現物體的 3D 形狀，如下圖所示。

![擴散光源與環境光源的球體圖例](images/lightad.jpg)

擴散光源比環境光源需要較多運算資源。 因為它取決於頂點直角和光線方向，因此您可以看到 3D 空間的物體幾何，它會產生比環境光源更真實的光源。 您可以使用反射強光達到更真實的外觀。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[光源的數學計算](mathematics-of-lighting.md)

 

 




