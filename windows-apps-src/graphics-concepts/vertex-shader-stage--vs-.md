---
title: 頂點著色器 (VS)階段
description: 頂點著色器 (VS) 階段處理頂點，通常執行轉換、皮膚形變與光照等作業。 頂點著色器採用單一輸入頂點，並產生單一輸出頂點。
ms.assetid: 5133C4BB-B4E6-4697-9276-F718AD44869C
keywords:
- 頂點著色器 (VS)階段
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ca3b5e230270b46b7cb2709d4bfa06c4c51d0224
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7977094"
---
# <a name="vertex-shader-vs-stage"></a>頂點著色器 (VS)階段


頂點著色器 (VS) 階段處理頂點，通常執行轉換、皮膚形變與光照等作業。 頂點著色器採用單一輸入頂點，並產生單一輸出頂點。

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途和使用


頂點著色器 (VS) 階段用於個別的每個頂點處理如：

-   轉換
-   皮膚形變
-   變形
-   每個頂點光源

頂點著色器階段是可程式化的著色器階段。它在[圖形管線](graphics-pipeline.md)圖表中顯示為圓角區塊。 這個著色器階段使用著色器模型 4.0 [通用著色器核心](https://msdn.microsoft.com/library/windows/desktop/bb509580)。

頂點著色器 (VS) 階段處理來自輸入組合語言的頂點。 頂點著色器一律在單一輸入頂點上操作，並產生單一輸出頂點。 頂點著色器階段必須永遠作用中，才能讓管線執行。 不需要頂點修改或轉換時，必須建立穿通頂點著色器並將其設定至管線。

每個頂點著色器輸入頂點可包含最多 16 個 32 位元向量（每個向量最多 4 個元件）。 每個輸出頂點可包含同樣多的 16 個 32 位元 4 元件向量。 所有頂點著色器都必須最少有一個輸入與一個輸出，這最少可能是一個純量值。

頂點著色器階段可從輸入組合語言取用兩個系統產生值：VertexID 和 InstanceID（請參閱＜系統值和語意＞）。 因為 VertexID 和 InstanceID 在頂點層級有意義，而且由硬體產生的識別碼只可以饋送到了解它們的第一階段，所以這些識別碼只可以饋送到頂點著色器階段。

頂點著色器永遠會在所有頂點上執行，包括在輸入基本類型拓撲中有相鄰關係的相鄰頂點。 使用 VSInvocations 管線統計資料，可以從 CPU 查詢頂點著色器已經執行的次數。

頂點著色器可以執行載入和紋理取樣作業，而不需要螢幕空間衍生物件 (使用 HLSL 內建函式：[Sample（DirectX HLSL 紋理物件）](https://msdn.microsoft.com/library/windows/desktop/bb509695)、[SampleCmpLevelZero（DirectX HLSL 紋理物件）](https://msdn.microsoft.com/library/windows/desktop/bb509697)，以及[SampleGrad（DirectX HLSL 紋理物件）](https://msdn.microsoft.com/library/windows/desktop/bb509698))。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>輸入


單一頂點，有 VertexID 和 InstanceID 系統產生值。 每個頂點著色器輸入頂點可包含最多 16 個 32 位元向量（每個向量最多 4 個元件）。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>輸出


單一頂點。 每個輸出頂點可包含同樣多的 16 個 32 位元 4 元件向量。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[圖形管線](graphics-pipeline.md)

 

 




