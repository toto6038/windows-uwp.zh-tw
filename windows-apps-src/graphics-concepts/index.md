---
title: Direct3D 圖形詞彙
description: 定義使用 Microsoft Direct3D 圖形詞彙。
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords:
- Direct3D 圖形詞彙
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0e325494cefeab4cf3ed40939f5b24de5e921b68
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7438146"
---
# <a name="direct3d-graphics-glossary"></a>Direct3D 圖形詞彙


定義 Microsoft Direct3D 圖形條款。 在高等級、 一般 3D 電腦圖形條款，使用 Direct3D 遊戲和應用程式開發中，這個詞彙的定義。

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
<td align="left"><p><a href="coordinate-systems-and-geometry.md">座標系統與幾何</a></p></td>
<td align="left"><p>要進行 Direct3D 應用程式的程式設計，就需要在工作上熟悉 3D 幾何原則。 本節引進建立 3D 場景所需的最重要幾何概念。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-and-index-buffers.md">頂點和索引緩衝區</a></p></td>
<td align="left"><p><em>頂點緩衝區</em>是包含頂點資料的記憶體緩衝區。在頂點緩衝中處理頂點，以執行轉換、照明及裁剪。 <em>索引緩衝區</em>是包含索引資料的記憶體緩衝區，是置入頂點緩衝區的整數位移，用於呈現基本類型。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="devices.md">裝置</a></p></td>
<td align="left"><p>Direct3D 裝置是 Direct3D 的轉譯元件。 裝置封裝並儲存呈現狀態、執行轉換照明作業，並將影像點陣化到表面。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="lights-and-materials.md">光源</a></p></td>
<td align="left"><p>光線可用於照亮場景中的物件。 每個物件頂點的色彩是以目前紋理圖、頂點色彩及光線來源為基礎。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="depth-and-stencil-buffers.md">深度和樣板緩衝區</a></p></td>
<td align="left"><p><em>深度緩衝區</em>儲存深度資訊來控制所呈現而不是隱藏的多邊形區域。 <em>樣板緩衝區</em>用於遮罩影像中的像素，以製造特效，包括合成、印花、溶解、淡化及撥動、外框及剪影，以及雙面樣板。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures.md">紋理</a></p></td>
<td align="left"><p>紋理是在電腦產生 3D 影像中創造真實感的利器。 Direct3D 支援廣泛的紋理功能設定，提供開發人員輕鬆存取進階紋理技術。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="graphics-pipeline.md">圖形管線</a></p></td>
<td align="left"><p>Direct3D 圖形管線專為即時遊戲應用程式產生圖形而設計。 透過每個可設定或可編程階段，從輸入到輸出的資料流。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="views.md">檢視</a></p></td>
<td align="left"><p>詞彙「檢視」&quot;&quot;用來表示「所需格式的資料」&quot;&quot;。 例如，常數緩衝區檢視 (CBV) 是格式正確的常數緩衝區資料。 本節描述最常見且實用的檢視。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compute-pipeline.md">計算管線</a></p></td>
<td align="left"><p>Direct3D 計算管線主要被設計用來處理大部分可與圖形管線平行進行的計算。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="resources.md">資源</a></p></td>
<td align="left"><p>資源是 Direct3D 管線可以存取的記憶體中的區域。 為了讓管線有效地存取記憶體，提供給管線的資料 (例如，輸入幾何、著色器資源及紋理) 必須儲存在資源中。 所有 Direct3D 資源衍生兩種類型的資源︰緩衝區或紋理。 每個管線階段可使用多達 128 種資源。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources.md">串流資源</a></p></td>
<td align="left"><p><em>串流處理資源</em>是使用少量實體記憶體的大型邏輯資源。 會視需要串流少部分資源，而不傳遞整個大型資源。 串流資源先前稱為<em>並排資源</em>。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="appendix.md">附錄</a></p></td>
<td align="left"><p>這些章節提供深入的技術詳細資料。</p></td>
</tr>
</tbody>
</table>

 

 

 
