---
title: 狀態物件
description: 裝置狀態群組成狀態物件，會大幅降低狀態變更的成本。 有幾個狀態物件，每個設計成針對特定管線階段初始化一組狀態。 狀態物件會依 Direct3D 的版本而不同。
ms.assetid: D998745C-2B75-4E59-9923-AD1A17A92E05
keywords:
- 狀態物件
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3437119979073a5cec27948fc90f954e06c2fc93
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2018
ms.locfileid: "7707119"
---
# <a name="state-objects"></a>狀態物件


裝置狀態群組成狀態物件，會大幅降低狀態變更的成本。 有幾個狀態物件，每個設計成針對特定管線階段初始化一組狀態。 狀態物件會依 Direct3D 的版本而不同。

## <a name="span-idinputlayoutspanspan-idinputlayoutspanspan-idinputlayoutspaninput-layout-state"></a><span id="Input_Layout"></span><span id="input_layout"></span><span id="INPUT_LAYOUT"></span>輸入配置狀態


此群組的狀態表示[輸入組合語言 (IA) 階段](input-assembler-stage--ia-.md)如何讀取輸入緩衝區的資料，並組合資料供頂點著色器使用。 這包括狀態，例如輸入緩衝區中的元素數目和輸入資料簽章。 輸入組合語言 (IA) 階段會將基本類型從記憶體串流處理到管線。

## <a name="span-idrasterizerspanspan-idrasterizerspanspan-idrasterizerspanrasterizer-state"></a><span id="Rasterizer"></span><span id="rasterizer"></span><span id="RASTERIZER"></span>轉譯器狀態


此群組的狀態初始化[轉譯器 (RS) 階段](rasterizer-stage--rs-.md)。 此物件包含填滿或揀選 (Cull) 模式、啟用剪式矩形裁剪和設定多重樣本參數等狀態。 這個階段會將基本類型點陣化成像素，執行像是裁剪及對應基本類型到檢視區等作業。

## <a name="span-iddepthstencilspanspan-iddepthstencilspanspan-iddepthstencilspandepth-stencil-state"></a><span id="DepthStencil"></span><span id="depthstencil"></span><span id="DEPTHSTENCIL"></span>深度樣板狀態


此群組的狀態初始化[輸出合併 (OM) 階段](output-merger-stage--om-.md)的深度樣板部分。 更具體而言，這個物件初始化深度和樣板測試。

## <a name="span-idblendspanspan-idblendspanspan-idblendspanblend-state"></a><span id="Blend"></span><span id="blend"></span><span id="BLEND"></span>混色狀態


此群組的狀態初始化[輸出合併 (OM) 階段](output-merger-stage--om-.md)的混色狀態。

## <a name="span-idsamplerspanspan-idsamplerspanspan-idsamplerspansampler-state"></a><span id="Sampler"></span><span id="sampler"></span><span id="SAMPLER"></span>取樣器狀態


此群組的狀態初始化取樣器物件。 著色器階段使用取樣器物件，以篩選記憶體中的紋理。

在 Direct3D，取樣器物件不繫結至特定紋理，它描述如何根據任何附加的資源進行篩選。

## <a name="span-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanperformance-considerations"></a><span id="Performance_Considerations"></span><span id="performance_considerations"></span><span id="PERFORMANCE_CONSIDERATIONS"></span>效能考量


設計 API 以使用狀態物件，會建立許多效能優點。 其中包括在物件建立時驗證狀態，在硬體啟用狀態物件快取，並大幅降低狀態設定 API 呼叫期間傳遞的狀態數量（傳遞狀態物件的控制代碼，而不是狀態）。

若要達成這些效能改進，您應該在應用程式啟動時，在轉譯迴圈之前，建立狀態物件。 狀態物件是不變，也就是狀態物件建立之後便無法變更。 相反地，您必須終結並重新建立它們。

您可以建立幾個有各種取樣器狀態組合的取樣器物件。 藉由呼叫適當「設定」API，傳遞物件的控制代碼（而不是取樣器狀態），完成變更取樣器狀態。 這大幅降低變更狀態的每個轉譯畫面期間的額外負荷數量，因為呼叫數目和資料量已大幅降低。

或者，您可以選擇使用效果系統，為您的應用程式自動管理狀態物件的有效建立和解構。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[圖形管線](graphics-pipeline.md)

[檢視](views.md)

 

 




