---
title: 紋理
description: 紋理是在電腦產生 3D 影像中創造真實感的利器。 Direct3D 支援廣泛的紋理功能設定，提供開發人員輕鬆存取進階紋理技術的方式。
ms.assetid: B9E85C9E-B779-4852-9166-6FA2240B7046
keywords:
- 紋理
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 516a15c17546d9f9b5e7cb7f8c0651f1372275ae
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6980046"
---
# <a name="textures"></a>紋理


紋理是在電腦產生 3D 影像中創造真實感的利器。 Direct3D 支援廣泛的紋理功能設定，提供開發人員輕鬆存取進階紋理技術的方式。

若要改善效能，請考慮使用動態紋理。 動態紋理可以在每個影格中鎖定、寫入，或是解鎖。

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
<td align="left"><p><a href="introduction-to-textures.md">紋理簡介</a></p></td>
<td align="left"><p>紋理資源是儲存材質的資料結構，並且是可讀取或寫入的最小紋理單位。 當著色器讀取紋理時，可由紋理取樣器篩選。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="basic-texturing-concepts.md">紋理的基本概念</a></p></td>
<td align="left"><p>早期電腦產生的 3D 影像，雖然普遍對當代而言算先進，但通常會有具光澤的塑膠感。 其缺乏可讓 3D 物件看起來逼真且複雜的標記類型，例如磨損、裂縫，指紋及指尖。 紋理在加強電腦產生之 3D 影像的真實性方面，變得愈來愈受歡迎。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-addressing-modes.md">紋理定址模式</a></p></td>
<td align="left"><p>Direct3D 應用程式可為任何原始物件的任何頂點指派紋理座標。 通常您指派給頂點的 u 及 v 紋理座標，都會位於 0.0 到 1.0 (含) 之間。 不過，藉由指派在該範圍之外的紋理座標，您可以建立特殊的紋理效果。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering.md">紋理篩選</a></p></td>
<td align="left"><p>原始物件透過將 3D 原始物件對應到 2D 畫面上進行轉譯時，紋理篩選會為原始物件的 2D 轉譯影像中的每個像素產生色彩。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-resources.md">紋理資源</a></p></td>
<td align="left"><p>紋理是可作為轉譯用途的一種資源。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-wrapping.md">紋理包裝</a></p></td>
<td align="left"><p>紋理包裝可藉由對每個頂點明確指定其紋理座標，以變更 Direct3D 點陣化已貼上紋理多邊形的基本方式。 點陣化多邊形時，系統會藉由在多邊形每個頂點的紋理座標間進行插補，以決定多邊形的每個像素會使用哪些材質。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-blending.md">紋理混色</a></p></td>
<td align="left"><p>Direct3D 在一個階段中最多可以將八個紋理混合到原始物件上。 使用多重紋理混合可大幅增加 Direct3D 應用程式的畫面播放速度。 應用程式藉由使用多重紋理混合，在單一階段中套用紋理、陰影、反射光源、擴散光源等其他特殊效果。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="light-mapping-with-textures.md">光線與紋理對應</a></p></td>
<td align="left"><p>光線對應為紋理或紋理群組，其包含 3D 場景中的照明相關資訊。 光線對應將光線和陰影對應到基本類型。 多階段及多重紋理混合可讓您的應用程式以比著色技術更逼真的外觀，對場景進行轉譯。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compressed-texture-resources.md">壓縮紋理資源</a></p></td>
<td align="left"><p>紋理貼圖為繪製在三維圖形上的數位化影像，用來增加更多視覺上的細節。 他們在點陣化的過程中對應至這些圖形之上，並且整個過程可能會取用大量的系統匯流排及記憶體。 若要減少紋理所使用的記憶體，Direct3D 支援壓縮紋理表面。 某些 Direct3D 裝置原生支援壓縮紋理表面。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[Direct3D 圖形學習指南](index.md)

 

 




