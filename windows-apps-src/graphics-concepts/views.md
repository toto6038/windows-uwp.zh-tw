---
title: 檢視
description: 詞彙「檢視」用來表示「所需格式的資料」。 例如，常數緩衝區檢視 (CBV) 是格式正確的常數緩衝區資料。 本節描述最常見且實用的檢視。
ms.assetid: 0C7FB99F-7391-472F-BA53-576888DFC171
keywords:
- 檢視
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 77ac16c55bb4292ea7c5362d007ff9569812954f
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5872047"
---
# <a name="views"></a>檢視


詞彙「檢視」用來表示「所需格式的資料」。 例如，常數緩衝區檢視 (CBV) 是格式正確的常數緩衝區資料。 本節描述最常見且實用的檢視。

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
<td align="left"><p><a href="constant-buffer-view--cbv-.md">常數緩衝區檢視 (CBV)</a></p></td>
<td align="left"><p>常數緩衝區包含著色器常數資料。 其價值在於資料會持續且可由任何 GPU 著色器存取，直到必須變更資料為止。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-buffer-view--vbv-.md">頂點緩衝區檢視 (VBV) 和索引緩衝區檢視 (IBV)</a></p></td>
<td align="left"><p>頂點緩衝區保留頂點清單的資料。 每個頂點的資料可以包含位置、色彩、標準向量、紋理座標等等。 索引緩衝區會保留頂點緩衝區中的整數索引（位移），用來定義和轉譯由頂點完整清單子集組成的物件。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="shader-resource-view--srv-.md">著色器資源檢視 (SRV) 和未排序存取檢視 (UAV)</a></p></td>
<td align="left"><p>著色器資源檢視通常會以著色器可存取的格式包裝紋理。 未排序存取檢視提供類似的功能，但啟用任何順序的紋理（或其他資源）讀取和寫入。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="sampler.md">取樣器</a></p></td>
<td align="left"><p>取樣是從紋理或其他資源讀取輸入值的程序。 「取樣器」是可讀取資源的任何物件。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-target-view--rtv-.md">轉譯目標檢視 (RTV)</a></p></td>
<td align="left"><p>轉譯目標讓場景轉譯到暫時中繼緩衝區，而不是轉譯到背景緩衝區，以轉譯到場景。 這個功能可讓您使用複雜的場景，可能轉譯為反映紋理或做為圖形管線內的其他用途使用，或可能是轉譯之前新增其他像素著色器效果至場景。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="depth-stencil-view--dsv-.md">深度樣板檢視 (DSV)</a></p></td>
<td align="left"><p>深度樣板檢視提供保留深度和樣板資訊的格式及緩衝區。 深度緩衝區用於揀選 (cull) 檢視器看不到的像素繪圖，因為它們被更近的物件阻擋在檢視之外。 樣板緩衝區可用於揀選所定義形狀之外的所有繪圖。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="stream-output-view--sov-.md">資料流輸出檢視 (SOV)</a></p></td>
<td align="left"><p>資料流輸出檢視可讓頂點、鑲嵌和幾何著色器已產生的頂點資訊串流輸出回到應用程式以供進一步使用。 例如已被這些著色器扭曲變形的物件可以寫回到應用程式，以提供更精確輸入給物理或其他引擎。 不過實際上，資料流輸出檢視是圖形管線不常使用的功能。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="rasterizer-ordered-view--rov-.md">轉譯器排序檢視 (ROV)</a></p></td>
<td align="left"><p>轉譯器排序檢視可解決讓某些深度緩衝區限制，尤其是有多個紋理的透明度全套用至同一個像素時。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[Direct3D 圖形學習指南](index.md)

 

 




