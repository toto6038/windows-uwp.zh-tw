---
title: "頂點和索引緩衝區"
description: "頂點緩衝區是包含頂點資料的記憶體緩衝區。在頂點緩衝中處理頂點，以執行轉換、照明及裁剪。"
ms.assetid: 8A39CD23-85FB-4424-9AC3-37919704CD68
keywords: "頂點和索引緩衝區"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: f06ee013f5c09522df387d69afa0096f7a3f7044
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="vertex-and-index-buffers"></a>頂點和索引緩衝區


*頂點緩衝區*是包含頂點資料的記憶體緩衝區。在頂點緩衝中處理頂點，以執行轉換、照明及裁剪。 *索引緩衝區*是包含索引資料的記憶體緩衝區，是置入頂點緩衝區的整數位移，用於呈現基本類型。

頂點緩衝區可以包含任何可轉譯的頂點類型 - 轉換或未轉換、點亮或無光。 您可以在頂點緩衝區中處理頂點，執行例如轉換、光源，或產生裁剪旗標等作業。 轉換永遠會執行。

頂點緩衝區的彈性使其成為重複使用轉換幾何的理想預備環境點。 您可以建立單一頂點緩衝區，然後將其中的頂點轉換、照亮及裁剪，並視需要多次轉譯場景中的模型，而不重新轉換，即使有交錯的轉譯狀態變更。 轉譯使用多個紋理的模式時，這非常有用：幾何只轉換一次，然後視需要轉譯其中的一部分，以所需的紋理變更交錯。 處理頂點之後所做的轉譯狀態變更，在下一次處理頂點時才會生效。

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
<td align="left"><p>[緩衝區簡介](introduction-to-buffers.md)</p></td>
<td align="left"><p>緩衝資源是一個完整輸入的資料集合，由元素所組成。 緩衝區儲存資料，例如<em>頂點緩衝區</em>中的紋理座標、<em>索引緩衝區</em>中的索引、<em>常數緩衝區</em>中的著色器常數資料、位置向量、法向向量或裝置狀態。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[索引緩衝區](index-buffers.md)</p></td>
<td align="left"><p><em>索引緩衝區</em>是包含索引資料的記憶體緩衝區，是置入頂點緩衝區的整數位移，用於呈現基本類型。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[Direct3D 圖形學習指南](index.md)

 

 




