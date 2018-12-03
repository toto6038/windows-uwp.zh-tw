---
title: 反射光源
description: 反射光源辨識當光碰到物件表面並反射回到相機時發生的明亮反射強光。
ms.assetid: 71F87137-B00F-48CE-8E6A-F98A139A742A
keywords:
- 反射光源
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7f28f1f46cfd34ee1aab614c57dc99019dbd6111
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458054"
---
# <a name="specular-lighting"></a>反射光源


*反射光源*辨識當光碰到物件表面並反射回到相機時發生的明亮反射強光。 反射光源比擴散光源強烈，更快速地在物件表面上衰退。 反射光源比擴散光源需要較長的時間計算，但是使用它的優點是它在物件表面上新增重要細節。

模型化鏡面反射需要系統知道光行進方向，以及檢視者眼睛的方向。 系統使用簡化 Phong 鏡面反射模型的版本，運用半程向量來近似計算鏡面反射的強度。

預設光源狀態不計算反射強光。

## <a name="span-idspecularlightingequationspanspan-idspecularlightingequationspanspan-idspecularlightingequationspanspecular-lighting-equation"></a><span id="Specular_Lighting_Equation"></span><span id="specular_lighting_equation"></span><span id="SPECULAR_LIGHTING_EQUATION"></span>反射光源方程式


反射光源以下列方程式描述。

|                                                                             |
|-----------------------------------------------------------------------------|
| 反射光源 = Cₛ \* sum\[Lₛ \* (N · H)<sup>P</sup> \* Atten \* Spot\] |

 

變數、其類型，以及其範圍如下所示：

| 參數    | 預設值 | 類型                                                             | 描述                                                                                            |
|--------------|---------------|------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| Cₛ           | (0,0,0,0)     | 紅色、綠色、藍色以及 alpha 透明度（浮點值） | 反射色彩。                                                                                        |
| sum          | N/A           | 無                                                              | 每道光的反射元件的總和。                                                          |
| N            | N/A           | 3D 向量 (x、y 和 z 浮點值)                    | 頂點標準。                                                                                         |
| H            | 無           | 3D 向量 (x、y 和 z 浮點值)                    | 半程向量。 請參閱有關半程向量的一節。                                                |
| <sup>P</sup> | 0.0           | 浮點數                                                   | 鏡面反射力。 範圍是 0 到 + 無限大                                                     |
| Lₛ           | (0,0,0,0)     | 紅色、綠色、藍色以及 alpha 透明度（浮點值） | 淺色反射。                                                                                  |
| Atten        | 無           | 浮點數                                                   | 光衰減值。 參閱[衰減和聚光燈係數](attenuation-and-spotlight-factor.md)。 |
| Spot         | 無           | 浮點數                                                   | 聚光燈係數。 參閱[衰減和聚光燈係數](attenuation-and-spotlight-factor.md)。        |

 

Cₛ 的值為：

-   頂點色彩 1，如果反射材料來源是擴散頂點色彩，而且第一個頂點色彩是在頂點宣告中提供。
-   頂點色彩 2，如果反射材料來源是反射頂點色彩，而且第二個頂點色彩是在頂點宣告中提供。
-   材料反射色彩

**注意：** 如果任一個反射材料來源選項，並且未提供頂點色彩，則會使用材料反射色彩。

 

所有的光分開處理和插補之後，反射元件會鉗制為從 0 到 255。

## <a name="span-idthehalfwayvectorspanspan-idthehalfwayvectorspanspan-idthehalfwayvectorspanthe-halfway-vector"></a><span id="The_Halfway_Vector"></span><span id="the_halfway_vector"></span><span id="THE_HALFWAY_VECTOR"></span>半程向量


半程向量 (H) 存在於兩個向量之間路程的一半：從物件頂點到光源的向量，以及從物件頂點到相機位置的向量。 Direct3D 提供兩種方式來計算半程向量。 當相機相對的反射強光啟用（而不是正交反射強光）時，系統會使用相機位置、頂點位置，加上光的方向向量，計算半程向量。 下列公式說明這點。

|                                           |
|-------------------------------------------|
| H = norm(norm(Cₚ - Vₚ) + L<sub>dir</sub>) |

 

| 參數       | 預設值 | 類型                                          | 描述                                                  |
|-----------------|---------------|-----------------------------------------------|--------------------------------------------------------------|
| Cₚ              | 無           | 3D 向量 (x、y 和 z 浮點值) | 相機位置。                                             |
| Vₚ              | 無           | 3D 向量 (x、y 和 z 浮點值) | 頂點位置。                                             |
| L<sub>dir</sub> | N/A           | 3D 向量 (x、y 和 z 浮點值) | 從頂點位置至光線位置的方向向量。 |

 

以這種方式判斷半程向量可能會需要大量運算資源。 或者，使用正交反射強光（而不是相機相對的反射強光）指示系統運作就像是檢視點在 z 軸上無限遠方一樣。 其計算公式如下。

|                                     |
|-------------------------------------|
| H = norm((0,0,1) + L<sub>dir</sub>) |

 

此設定比較不需要大量運算資源，但比較不精確，因此最適合供使用正交投影的應用程式使用。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>範例


在這個範例，物件使用場景反射淺色和材料反射色彩來著色。

根據方程式，為物件端點產生的色彩是材料色彩和淺色的組合。

下列兩圖顯示反射材料色彩 (灰色)，以及反射淺色 (白色)。

![灰球體圖例](images/amb1.jpg)![白色球體的圖例](images/lightwhite.jpg)

下圖顯示結果反射強光。

![反射強光的圖例](images/lights.jpg)

結合反射強光與周遭環境和擴散光源會產生下圖。 在套用所有三種光源類型時，這更加清楚地類似寫實物件。

![結合反射強光、周遭環境光源和擴散光源的圖例](images/lightads.jpg)

反射光源比擴散光源需要較多運算資源。 這通常用來提供表面材料的視覺線索。 根據表面材料，反射強光在大小和色彩上不同。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[光源的數學計算](mathematics-of-lighting.md)

 

 




