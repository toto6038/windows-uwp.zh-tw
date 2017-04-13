---
title: "像素著色器 (PS) 階段"
description: "像素著色器 (PS) 階段會接收基本類型的插補資料，並產生每一像素資料，例如色彩。"
ms.assetid: 0AEBFDFB-0AD8-4633-AE4E-A44004B57745
keywords: "像素著色器 (PS) 階段"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 94a7d35605200b010210d16a8e0ddcc605db01e8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="pixel-shader-ps-stage"></a>像素著色器 (PS) 階段


像素著色器 (PS) 階段會接收基本類型的插補資料，並產生每一像素資料，例如色彩。

這是可程式著色器階段；它會在[圖形管線](graphics-pipeline.md)圖表中顯示為圓角區塊。。 這個著色器階段會公開自己的獨特功能，以著色器模型 4.0 [通用著色器核心](https://msdn.microsoft.com/library/windows/desktop/bb509580)建置。

像素著色器 (PS) 階段可啟用豐富的陰影技術，例如個別像素光線和後處理。 像素著色器是結合常數變數、紋理資料、插補每個頂點值和其他資料的程式，以產生每個像素的輸出。 [轉譯器 (RS) 階段](rasterizer-stage--rs-.md)會對每個基本類型所涵蓋的每個像素叫用一次像素著色器，但是可以指定 **NULL** 著色器以避免執行著色器。

多重取樣紋理時，像素著色器會在每個涵蓋的多重取樣發生深度/樣板測試期間，叫用一次每個涵蓋像素。 通過深度/樣板測試的樣本會以像素著色器輸出色彩進行更新。

像素著色器的內建功能會產生或使用數量的導數，與螢幕空間 x 和 y 相關。 導數的常見用途是計算紋理取樣的詳細層級，而在非等向性篩選的案例中，選取樣本連同非等向性軸。 通常的硬體實作會同時對多個像素執行像素著色器 (例如 2x2 格線)，如此在像素著色器中運算的數量導數可以合理地在相鄰的相同執行點大略估算為差異值。

## <a name="span-idinputsspanspan-idinputsspanspan-idinputsspaninputs"></a><span id="Inputs"></span><span id="inputs"></span><span id="INPUTS"></span>輸入


若管線設定後沒有幾何著色器，像素著色器限於 16、32 位元、4 個元件的輸入。 否則，像素著色器最多可以採用 32、32 位元、4 個元件的輸入。

像素著色器輸入資料包括頂點屬性 (可插補，無論是否有透視修正)，也可以視為每個基本類型常數。 像素著色器輸入會從點陣化的基本類型的頂點屬性插補，根據宣告的插補模式。 如果在點陣化之前基本類型被裁剪，則插補模式也會在裁剪過程中採用。

頂點屬性是在像素著色器中心位置插補 (或評估)。 像素著色器屬性插補模式會在輸入登錄宣告中宣告，以每個元素為基礎，如[引數](https://msdn.microsoft.com/library/windows/desktop/bb509606)或[輸入結構](https://msdn.microsoft.com/library/windows/desktop/bb509668)。 可線性插補的屬性，或使用距心取樣。 查看[點陣化規則](rasterization-rules.md)中的「多重取樣鋸齒化時屬性的距心取樣」一節。 距心評估只在多重取樣到基本類型所涵蓋的像素時相關，但像素中心可能不是；距心評估盡可能發生在靠近 (非涵蓋) 像素中心。

輸入也可以使用[系統值語意](https://msdn.microsoft.com/library/windows/desktop/bb509647)宣告，這會由其他管線階段使用的參數標記。 例如，像素位置應該使用 SV\_Position 語意標示。 [輸入組合語言 (IA) 階段](input-assembler-stage--ia-.md)可以為像素著色器產生一個純量 (使用 SV\_PrimitiveID)；[轉譯器 (RS) 階段](rasterizer-stage--rs-.md)也可以針對像素著色器產生一個純量 (使用 SV\_IsFrontFace)。

## <a name="span-idoutputsspanspan-idoutputsspanspan-idoutputsspanoutputs"></a><span id="Outputs"></span><span id="outputs"></span><span id="OUTPUTS"></span>輸出


像素著色器可以輸出最多 8、32 位元、4 個元件的色彩，或無色彩 (如果捨棄像素)。 像素著色器輸出暫存器元件必須先宣告，才能使用；每個暫存器可以有不同的輸出寫入遮罩。

使用可深度寫入狀態 (在[輸出合併 (OM) 階段](output-merger-stage--om-.md)中) 來控制深度資料是否寫入深度緩衝區 (或使用捨棄指令捨棄該像素的資料）。 像素著色器也可以輸出選擇性的 32 位元、1-元件、浮點、深度測試的深度值 (使用語意 SV\_Depth)。 深度值是在 oDepth 登錄中輸出，並會取代深度測試的插補深度值 (假設已啟用深度測試的話)。 無法在使用固定函式深度或著色器 oDepth 之間動態變更。

像素著色器無法輸出樣板值。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[圖形管線](graphics-pipeline.md)

 

 




