---
title: 紋理篩選
description: 當一個原始物件透過將 3D 原始物件對應到 2D 畫面上進行轉譯時，紋理篩選會為原始物件的 2D 轉譯影像中的每個像素產生色彩。
ms.assetid: 1CCF4138-5D48-4B07-9490-996844F994D8
keywords:
- 紋理篩選
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ce80bc0f64e1aba8328880203f22ea3909fdb3e3
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5863467"
---
# <a name="texture-filtering"></a>紋理篩選


當一個原始物件透過將 3D 原始物件對應到 2D 畫面上進行轉譯時，紋理篩選會為原始物件的 2D 轉譯影像中的每個像素產生色彩。

當 Direct3D 轉譯原始物件時，會將 3D 原始物件對應到 2D 畫面上。 若原始物件帶有紋理，Direct3D 必須使用該紋理為原始物件的 2D 轉譯影像中的每個像素產生色彩。 針對畫面上原始物件影像的每個像素，程式必須從紋理中取得色彩值。 這個過程稱為 *「紋理篩選」*。

執行紋理篩選作業時，通常也會放大或縮小使用的紋理。 換句話說，其會對應到比自身大或小的原始影像。 放大紋理可能會導致許多像素對應到一個材質。 其結果可能為外觀粗短。 縮小紋理通常表示一個像素會對應到許多材質。 其結果影像可能會顯得模糊或帶有鋸齒。 若要解決這些問題，您必須對材質色彩稍作混合，以達到像素的色彩。

Direct3D 將複雜的紋理篩選過程予以簡化。 Direct3D 提供您三種紋理篩選的類型：線性篩選、非等向性篩選，及 Mipmap 篩選。 如果您並未選擇任何紋理篩選方式，Direct3D 會使用一種稱作「最近點取樣」的技術。

每一種紋理篩選都有優點和缺點。 例如：線性紋理篩選可能會使最終影像產生鋸齒狀的邊緣，或是矮胖的外觀。 然而，其為運算上低負荷的一種紋理篩選方法。 使用 Mipmap 進行紋理篩選通常會產生最好的結果，特別是與非等向性篩選搭配使用時。 然而，其在 Direct3D 支援的技術中是最消耗記憶體的一種。

## <a name="span-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspantypes-of-texture-filtering"></a><span id="Types-of-texture-filtering"></span><span id="types-of-texture-filtering"></span><span id="TYPES-OF-TEXTURE-FILTERING"></span>紋理篩選的類型


Direct3D 支援以下紋理篩選方法。

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
<td align="left"><p><a href="nearest-point-sampling.md">最近點取樣</a></p></td>
<td align="left"><p>應用程式不一定需要使用紋理篩選。 Direct3D 可設定成讓其計算材質位址，其通常不評估整數，並會以最接近的整數位址複製材質色彩。 這個過程稱作<em>最近點取樣</em>。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="bilinear-texture-filtering.md">雙線性紋理篩選</a></p></td>
<td align="left"><p>「線性篩選」<em></em>會計算最接近取樣點的 4 項材質之加權平均值。 此篩選的方式比最近點篩選更準確和常見。 由於這種方式已在現代的圖形硬體內實作，因此很有效率。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="anisotropic-texture-filtering.md">非等向性紋理篩選</a></p></td>
<td align="left"><p><em>非等向性</em>是材質中可見的失真，屬於角度以螢幕平面為方向的 3D 物件表面。 當非等向性原始物件中的像素對應到材質時，其形狀即會扭曲。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering-with-mipmaps.md">使用 Mipmap 進行紋理篩選</a></p></td>
<td align="left"><p><em>Mipmap</em> 是一組紋理序列，序列中的每個影像都是相同影像漸進式降低解析度的結果。 每個影像的高度、寬度，或是層級，都是前一層級以 2 的乘冪縮小的結果。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[紋理](textures.md)

 

 




