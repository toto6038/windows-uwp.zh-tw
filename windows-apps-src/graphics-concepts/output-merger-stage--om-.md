---
title: 輸出合併 (OM) 階段
description: 輸出合併 (OM) 階段將不同類型的輸出資料 (像素著色器值、深度與樣板資訊) 與轉譯目標和深度/樣板緩衝區的內容結合，以產生最終的管線結果。
ms.assetid: ED2DC4A0-2B92-47AF-884A-BFC2183C78B8
keywords:
- 輸出合併 (OM) 階段
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 63a77048bed3ad27f2040a672d93380d0250f9aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641093"
---
# <a name="output-merger-om-stage"></a>輸出合併 (OM) 階段


輸出合併 (OM) 階段將不同類型的輸出資料 (像素著色器值、深度與樣板資訊) 與轉譯目標和深度/樣板緩衝區的內容結合，以產生最終的管線結果。

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>用途和用法


輸出合併 (OM) 階段是判斷哪一個像素可見 (含深度樣板測試) 以及混合最終像素色彩的最後步驟。

OM 階段會使用下列組合產生最終呈現的像素色彩︰

-   管線狀態
-   由像素著色器產生的像素資料
-   轉譯目標的內容
-   深度/樣板緩衝區的內容。

### <a name="span-idblending-overviewspanspan-idblending-overviewspanspan-idblending-overviewspanblending-overview"></a><span id="Blending-overview"></span><span id="blending-overview"></span><span id="BLENDING-OVERVIEW"></span>混合的概觀

混合結合了一個或多個像素值，以建立最終像素色彩。 下圖顯示混合像素資料的程序。

![混合資料的運作方式的圖表](images/d3d10-blend-state.png)

在概念上，您可以在輸出合併階段中視覺化此實作的流程圖兩次︰第一個以平行方式混合 RGB 資料，第二個混合差異資料。 若要瞭解如何使用 API 建立並設定混合狀態，請參閱[設定混合功能](https://msdn.microsoft.com/library/windows/desktop/bb205072)。

固定函式混合可以針對每個轉譯目標單獨啟用。 不過，只有一組混合控制項，如此相同混合才會套用到所有啟用混合的 RenderTarget。 混合值 (包括 BlendFactor) 始終會限制為混合之前的轉譯目標格式的範圍之內。 關於轉譯目標類型，按照每個轉譯目標完成鉗制。 唯一例外是，位鉗制的 float16、float11 或 float10 格式，如此這些格式的混合作業都可以至少等於精確度/範圍，完成為輸出格式。 NaN 的及簽署的零會針對所有案例傳播 (包括 0.0 混合重量)。

當您使用 sRGB 轉譯目標時，執行階段會在執行混合之前，將轉譯目標色彩轉換為線性空間。 執行階段會在其將值儲存回轉譯目標之前，將最終混合值轉換回 sRGB 空間。

### <a name="span-iddual-source-color-blendingspanspan-iddual-source-color-blendingspanspan-iddual-source-color-blendingspandual-source-color-blending"></a><span id="Dual-source-color-blending"></span><span id="dual-source-color-blending"></span><span id="DUAL-SOURCE-COLOR-BLENDING"></span>雙重來源色彩透明混色

這項功能可以讓輸出合併階段，同時使用兩個像素著色器輸出 (o0 和 o1)，在插槽 0 對單一轉譯目標進行混合作業。 有效的混合作業包括︰新增、減除和 revsubtract。 混合方程式和輸出寫入遮罩會指定像素著色器將輸出哪些元件。 忽略額外的元件。

寫入到其他像素著色器輸出 (o2、o3 等) 未定義；若不是繫結到插槽 0，您可能不寫入轉譯目標。 寫入 oDepth 在雙來源色彩混合期間是有效的。

### <a name="span-iddepth-stencil-testspanspan-iddepth-stencil-testspanspan-iddepth-stencil-testspandepth-stencil-testing-overview"></a><span id="Depth-Stencil-Test"></span><span id="depth-stencil-test"></span><span id="DEPTH-STENCIL-TEST"></span>深度樣板測試概觀

深度樣板緩衝區，這會建立為紋理資源，可以包含深度資料和樣板資料這兩個資料。 深度資料用於判斷哪些像素最靠近相機，以及樣板資料用於遮罩哪一個可以更新的像素。 最後，深度與樣板數值資料二者都由輸出合併階段，用於判斷是否已繪製像素。 下圖顯示如何完成深度樣板測試的概念方式。

![深度樣板測試的運作方式的圖表](images/d3d10-depth-stencil-test.png)

若要設定深度樣板測試，請參閱[設定深度樣板功能](configuring-depth-stencil-functionality.md)。 深度樣板物件封裝深度樣板狀態。 應用程式可以指定深度樣板的狀態，否則 OM 階段將會使用預設值。 如果已停用多重取樣，就以像素為基礎執行混合作業。 如果啟用多重取樣，會以每個多重取樣為基礎進行混合。

使用深度緩衝區的程序來判斷哪一個像素應繪製，這稱為「深度緩衝」，有時也稱為「z 緩衝」。

一旦深度值到達輸出合併階段 (不論從插補或從像素著色器)，它們永遠都箝制︰z = min(Viewport.MaxDepth,max(Viewport.MinDepth,z))，根據深度緩衝區的格式/精準度，使用浮點規則。 鉗制之後，比較深度值 (使用 DepthFunc) 與現有深度緩衝區值。 如果未繫結任何深度緩衝區，一律通過深度測試。

如果沒有任何深度緩衝區格式的樣板元件，或者未繫結任何深度緩衝區，則一律通過樣板測試。

一次只有一個深度/樣板緩衝區可以使用；任何繫結的資源檢視必須符合 (相同的大小和維度) 深度/樣板檢視。 這不表示資源大小必須符合，只是檢視大小必須符合。

### <a name="span-idsample-maskspanspan-idsample-maskspanspan-idsample-maskspansample-mask-overview"></a><span id="Sample-Mask"></span><span id="sample-mask"></span><span id="SAMPLE-MASK"></span>範例遮罩概觀

樣本遮罩是 32 位元多重取樣涵蓋範圍的遮罩，判斷哪些樣本在作用中的轉譯目標中已更新。 只允許一個樣本遮罩。 樣本遮罩中位元到資源中樣本的對應，是由使用者定義。 若是 n 樣本轉譯，會使用樣本遮罩的第一個 n 位元 (從 LSB)(32 位元它的上限位元)。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>輸入


輸出合併 (OM) 階段會使用下列組合產生最終轉譯的像素色彩︰

-   管線狀態
-   由像素著色器產生的像素資料
-   轉譯目標的內容
-   深度/樣板緩衝區的內容。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Output


### <a name="span-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanoutput-write-mask-overview"></a><span id="Output-write-mask-overview"></span><span id="output-write-mask-overview"></span><span id="OUTPUT-WRITE-MASK-OVERVIEW"></span>輸出寫入遮罩概觀

使用輸出寫入遮罩來控制 (按每個元件) 可寫入轉譯目標的資料。

### <a name="span-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanmultiple-render-targets-overview"></a><span id="Multiple-render-targets-overview"></span><span id="multiple-render-targets-overview"></span><span id="MULTIPLE-RENDER-TARGETS-OVERVIEW"></span>多個轉譯目標概觀

像素著色器可用來轉譯為至少 8 個不同轉譯目標，所有都必須是相同類型 (緩衝、Texture1D、Texture1DArray 等等）。 此外，所有轉譯目標在所有維度 (寬地、高度、深度、陣列大小、樣本計數) 中必須有相同的大小。 各個轉譯目標可能會有不同的資料格式。

您可以使用任何組合的轉譯目標插槽 (最多達 8 個)。 不過，資源檢視無法同時繫結到多個轉譯目標插槽。 檢視可重複使用，只要不同時使用資源。

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
<td align="left"><p><a href="configuring-depth-stencil-functionality.md">設定 深度樣板功能</a></p></td>
<td align="left"><p>本章節涵蓋的步驟，內容為設定深度樣板緩衝區及輸出合併階段的深度樣板狀態。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[圖形管線](graphics-pipeline.md)

 

 




