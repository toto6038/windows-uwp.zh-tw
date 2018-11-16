---
title: 紋理混色
description: Direct3D 在一個階段中最多可以將八個紋理混合到原始物件上。
ms.assetid: 9AD388FA-B2B9-44A9-B73E-EDBD7357ACFB
keywords:
- 紋理混色
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d4121bd402b048ee6102ed3be30b94a66e274273
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6841187"
---
# <a name="texture-blending"></a>紋理混色


Direct3D 在一個階段中最多可以將八個紋理混合到原始物件上。 使用多重紋理混合可大幅增加 Direct3D 應用程式的畫面播放速度。 應用程式藉由使用多重紋理混合，在一個階段中套用紋理、陰影、反射光源、擴散光源等其他特殊效果。

若要使用紋理混色，您的應用程式首先應該確認使用者的硬體是否支援該功能。

## <a name="span-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespantexture-stages-and-the-texture-blending-cascade"></a><span id="Texture-Stages-and-the-Texture-Blending-Cascade"></span><span id="texture-stages-and-the-texture-blending-cascade"></span><span id="TEXTURE-STAGES-AND-THE-TEXTURE-BLENDING-CASCADE"></span>紋理階段和紋理混色串聯


Direct3D 透過使用紋理階段來支援一階段的多重紋理混合。 紋理階段取得兩個引數之後，對其進行混合運算，並且將結果傳回以供進一步處理或點陣化使用。 您可將紋理階段視覺化為以下圖例。

![紋理階段的簡圖](images/texstg.png)

如上方簡圖所示，紋理階段會利用一個指定的運算子將兩個引數混合。 常見的運算包括了簡單的調節，或是增加引數的色彩或 Alpha 元件，但應用程式支援超過 24 種運算。 階段的引數可以是關聯紋理、重複色彩及 Alpha (於 Gouraud Shading 中重複)、任意色彩及 Alpha，或是前一紋理階段的結果。

**注意：**  Direct3D 會區分色彩混合 alpha 混合。 應用程式會為色彩及 Alpha 個別設定混合運算及引數，並且其結果均各自獨立。

 

多重混合階段使用的引數及運算組合，定義了一個以流程為基礎的簡單混合語言。 一個階段的結果會流到另一個階段，並且再從該階段流至下一個階段，以此類推。 運算結果從一個階段流至另一個階段，並且最終在一個多邊形上進行點陣化的這項概念，通常稱作紋理混色串聯。 下列簡圖呈現了個別紋理階段組成紋理混色串聯的過程。

![紋理混色串聯中紋理階段的簡圖](images/tcascade.png)

裝置中的每個階段都有一個以 0 為基礎的索引。 Direct3D 雖然允許最多八個混合階段，我們仍建議您事先檢查裝置功能，以決定現有硬體支援的最大階段數量。 第一個混合階段位於索引 0 上，第二個則位於 1，以此類推直到索引 7 為止。 系統以遞增索引順序混合各階段。

建議您只使用您需要的階段數量。任何未使用的混合階段預設都會停用。 因此，若您的應用程式只會使用到前兩個階段，您只需要為階段 0 及階段 1 設定運算及引數。 系統會混合兩個階段，並且忽略所有停用的階段。

若您的應用程式根據不同的狀況，可能會使用不同數量的階段 (例如：針對一些物件設定四個階段，其他的則為兩個階段)，您不需要明確停用所有之前使用過的階段。 其中一個選項是停用第一個未使用階段的色彩運算，則所有具有更高索引值的階段皆不會啟用。 另一個選項則是藉由停用第一個紋理階段 (階段 0) 的色彩運算，以完全停用紋理對應。

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
<td align="left"><p><a href="blending-stages.md">混合階段</a></p></td>
<td align="left"><p>混合階段是一組定義了紋理混合方式的紋理運算及其引數。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multipass-texture-blending.md">多階段紋理混色</a></p></td>
<td align="left"><p>Direct3D 應用程式可透過在多個轉譯階段為原始物件套用各種紋理，以達到多項特殊效果。 針對此一行動的常用字詞為<em>多階段紋理混色</em>。 多階段紋理混色的典型用途為藉由套用數種不同紋理的多重色彩，模擬複雜光源和著色模型的效果。 這類應用方式稱為<em>光線對應</em>。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[紋理](textures.md)

 

 




