---
title: HLSL 串流資源曝光
description: 要支援著色器模型 5中的串流資源，需要特定 Microsoft 高階著色器語言 (HLSL) 語法。
ms.assetid: 00A40D82-0565-43DC-82AB-0675B7E772E3
keywords:
- HLSL 串流資源曝光
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a8523f4895c541ffb3b92ee00d5b62c57343ae00
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2018
ms.locfileid: "7158485"
---
# <a name="hlsl-streaming-resources-exposure"></a>HLSL 串流資源曝光


要支援[著色器模型 5](https://msdn.microsoft.com/library/windows/desktop/ff471356)中的串流資源，需要特定 Microsoft 高階著色器語言 (HLSL) 語法。

著色器模型 5 的 HLSL 語意只允許在支援串流資源的裝置上使用。 下表中串流資源的每一種相關的 HLSL 方法都可接受一個 (feedback) 或兩個 (clamp 和 feedback，依此順序) 額外的選用參數。 例如，**Sample** 方法為：

**Sample(sampler, location \[, offset \[, clamp \[, feedback\] \] \])**

**Sample** 方法的範例為 [**Texture2D.Sample(S,float,int,float,uint)**](https://msdn.microsoft.com/library/windows/desktop/dn393787)。

offset、clamp 和 feedback 參數為選用。 您必須指定所有選用參數，直到您需要的參數，這與 C++ 對於預設函數引數的規則相同。 例如，如果需要 feedback 狀態，offset 和 clamp 兩個參數就需要明確提供給 **Sample**，即使邏輯上可能不需要它們。

clamp 參數是純量浮點值。 clamp=0.0f 的常值表示 clamp 作業未執行。

feedback 參數是 **uint** 變數，您可以將它提供給記憶體存取查詢內建 [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) 函數。 您不得修改或解譯 feedback 參數值；不過編譯器不會提供任何進階分析和診斷，來偵測您是否修改此值。

以下是 [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) 的語法：

**bool CheckAccessFullyMapped(in uint FeedbackVar);**

[**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) 解譯 *FeedbackVar* 的值，並傳回 true，如果所有存取的資料都在資源中對應；否則 **CheckAccessFullyMapped** 傳回 false。

如果 clamp 或 feedback 參數存在，編譯器會發出基本指令的變化。 例如，串流資源的範例會產生 `sample_cl_s` 指令。

如果 clamp 和 feedback 都未指定，編譯器就會發出基本指令，目前行為也就沒有變更。

clamp 值為 0.0f，表示不會執行 clamp；因此，驅動程式編譯器可進一步依照目標硬體建立指令。 如果 feedback 為指令中的 NULL 暫存器，則 feedback 未使用；因此，驅動程式編譯器可以進一步依照目標架構建立指令。

如果 HLSL 編譯器推斷 clamp 為 0.0f 且 feedback 未使用，則編譯器會發出對應的基本指令 (例如 `sample` 而非 `sample_cl_s`)。

如果串流資源存取是由數個要素位元組程式碼指令所組成，例如結構化資源，則編譯器會透過 OR 作業彙總個別 feedback 值，產生最終的 feedback 值。 因此，您會看到這類複雜存取的單一 feedback 值。

這是 HLSL 方法的摘要表，已變更為支援 feedback 和/或 clamp。 這些全都可在所有維度的拼接和非串流資源上運作。 非串流資源一律顯示為完整對應。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471359">HLSL 物件</a> </th>
<th align="left">包含 feedback 選項 (*) 的內建方法 - 也有 clamp 選項</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Gather</p>
<p>GatherRed</p>
<p>GatherGreen</p>
<p>GatherBlue</p>
<p>GatherAlpha</p>
<p>GatherCmp</p>
<p>GatherCmpRed</p>
<p>GatherCmpGreen</p>
<p>GatherCmpBlue</p>
<p>GatherCmpAlpha</p></td>
</tr>
<tr class="even">
<td align="left"><p>[RW]Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>[RW]Texture3D</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Sample*</p>
<p>SampleBias*</p>
<p>SampleCmp*</p>
<p>SampleCmpLevelZero</p>
<p>SampleGrad*</p>
<p>SampleLevel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[RW]Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>Texture2DMS</p>
<p>[RW]Texture2DArray</p>
<p>Texture2DArrayMS</p>
<p>[RW]Texture3D</p>
<p>[RW]Buffer</p>
<p>[RW]ByteAddressBuffer</p>
<p>[RW]StructuredBuffer</p></td>
<td align="left">Load</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流資源的存取管線](pipeline-access-to-streaming-resources.md)

 

 




