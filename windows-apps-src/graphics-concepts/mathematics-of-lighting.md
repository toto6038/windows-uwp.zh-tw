---
title: 光源的數學計算
description: Direct3D 燈光模型涵蓋環境、擴散、反射及發光。 這個範圍足以因應大範圍的照明情況。 稱為全球照明的場景中的光線總量。
ms.assetid: D0521F56-050D-4EDF-9BD1-34748E94B873
keywords:
- 光源的數學計算
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 38a65a3532fe401f31fbf0da656aff1a141fa71a
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2018
ms.locfileid: "7709473"
---
# <a name="mathematics-of-lighting"></a>光源的數學計算


Direct3D 燈光模型涵蓋環境、擴散、反射及發光。 這個範圍足以因應大範圍的照明情況。 稱為*全球照明*的場景中的光線總量。

全球照明的計算，如下所示︰

```
Global Illumination = Ambient Light + Diffuse Light + Specular Light + Emissive Light 
```

[環境光源](ambient-lighting.md)常亮。 環境光線於所有方向皆為定值，且為物件的所有像素以相同方式上色。 加以計算很快速，但是會讓物件看起來很平板且不真實。

[擴散光源](diffuse-lighting.md)取決於光方向和物件表面法向。 由於變更光方向和變更表面數字向量，擴散光源在物件表面有不同呈現。 計算擴散光源需要較長時間，因為它改變每個物件頂點，不過使用它的優點是它會遮蔽物件，並提供物件三維 (3D) 深度。

[反射光源](specular-lighting.md)辨識當光碰到物件表面並反射回到相機時發生的明亮反射強光。 反射光源比擴散光源強烈，更快速地在物件表面上衰退。 反射光源比擴散光源需要較長的時間計算，但是使用它的優點是它在物件表面上新增重要細節。

[放射光源](emissive-lighting.md)是物件發出的光線，例如光芒。 放射呈現物件看起來像自體發光。 放射會影響物件的色彩，例如讓深色材質更亮，並呈現部分的發出色彩。

可藉由運用每一種類型的光源至 3D 場景來成就實際光源。 計算環境、放射，擴散元件的值會輸出為擴散頂點色彩，而反射光源元件的值則輸出為反射頂點色彩。 環境、擴散及反射光線值可能受到指定光線衰減和聚光燈係數的影響。 參閱[衰減和聚光燈係數](attenuation-and-spotlight-factor.md)。

若要完成更多實際照明效果，您可以新增更多光線，不過場景會花較長的時間來呈現。 若要取得所有效果設計師的想法，某些遊戲使用比一般常用者更多的 CPU 功率。 若是如此，通常會將照明計算數目減到最小值，方法是在使用材質貼圖的同時，使用光照貼圖和環境貼圖將光源引入場景。

在相機空間計算光源。 請參閱[相機空間轉換](camera-space-transformations.md)。 在特殊條件下，最佳化的光源可在模型空間計算︰ 已經正規化法向向量，不需要頂點混合，而且轉換矩陣正交。

使用反世界矩陣模，藉由轉換光源位置和方向以及相機位置至模型空間，在模型空間中計算所有光源。 因此，如果世界或檢視矩陣引入不均勻的縮放比例，產生的光線可能不正確。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本節內容


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="ambient-lighting.md">環境光源</a></p></td>
<td align="left"><p>環境光線為場景提供定值光線。 它同等地照亮所有物件端點，因為其不取決於任何其他照明因素，例如頂點標準、光源方向、光源位置、範圍或衰減。 環境光線於所有方向皆為定值，且為物件的所有像素以相同方式上色。 加以計算很快速，但是會讓物件看起來很平板且不真實。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="diffuse-lighting.md">擴散光源</a></p></td>
<td align="left"><p><em>擴散光源</em>取決於光方向和物件表面法向。 由於變更光方向和變更表面數字向量，擴散光源在物件表面有不同呈現。 計算擴散光源需要較長時間，因為它改變每個物件頂點，不過使用它的優點是它會遮蔽物件，並提供物件三維 (3D) 深度。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="specular-lighting.md">反射光源</a></p></td>
<td align="left"><p><em>反射光源</em>辨識當光碰到物件表面並反射回到相機時發生的明亮反射強光。 反射光源比擴散光源強烈，更快速地在物件表面上衰退。 反射光源比擴散光源需要較長的時間計算，但是使用它的優點是它在物件表面上新增重要細節。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="emissive-lighting.md">放射光源</a></p></td>
<td align="left"><p><em>放射光源</em>是物件發出的光線，例如光芒。 放射呈現物件看起來像自體發光。 放射會影響物件的色彩，例如讓深色材質更亮，並呈現部分的發出色彩。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="camera-space-transformations.md">相機空間轉換</a></p></td>
<td align="left"><p>相機空間端點的計算方式為使用全球檢視矩陣轉換物件端點。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="attenuation-and-spotlight-factor.md">衰減和聚光燈係數</a></p></td>
<td align="left"><p>全域照明方程式的擴散和聚光燈型光源元件內含描述光衰減及聚光燈圓錐的字詞。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[光線和材料](lights-and-materials.md)

 

 




