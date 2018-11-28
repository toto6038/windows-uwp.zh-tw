---
title: 非對應磚的轉譯器行為
description: 本節說明非對應磚的轉譯器行為。
ms.assetid: AC7B818D-E52B-4727-AEA2-39CFDC279CE7
keywords:
- 非對應磚的轉譯器行為
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e3089444820f990644526eaafb7f2ef9007fa70a
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7831753"
---
# <a name="span-iddirect3dconceptsrasterizerbehaviorwithnon-mappedtilesspanrasterizer-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.rasterizer_behavior_with_non-mapped_tiles"></span>非對應磚的轉譯器行為


本節說明非對應磚的轉譯器行為。

## <a name="span-iddepthstencilviewspanspan-iddepthstencilviewspanspan-iddepthstencilviewspandepthstencilview"></a><span id="DepthStencilView"></span><span id="depthstencilview"></span><span id="DEPTHSTENCILVIEW"></span>DepthStencilView


深度樣板檢視 (DSV) 的行為會根據硬體支援程度讀取和寫入。 如需需求明細，請參閱[串流資源功能層級](streaming-resources-features-tiers.md)的整體讀取和寫入行為。

以下是理想的行為︰

如果磚在 DepthStencilView 中未對應，讀取深度的傳回值為 0，然後饋入為深度讀取值設定的任何作業。 對遺失的深度磚的寫入會捨棄。 [第 2 層](tier-2.md)不需要此寫入處理的理想定義；對非對應磚的寫入最後可能會在快取，後續讀取可能會收取此資料。

## <a name="span-idrendertargetviewspanspan-idrendertargetviewspanspan-idrendertargetviewspanrendertargetview"></a><span id="RenderTargetView"></span><span id="rendertargetview"></span><span id="RENDERTARGETVIEW"></span>RenderTargetView


轉譯目標檢視 (RTV) 的行為會根據硬體支援的層級讀取和寫入。 如需需求明細，請參閱[串流資源功能層級](streaming-resources-features-tiers.md)的整體讀取和寫入行為。

在所有實作上，同時繫結的不同 RTV (和 DSV) 可能會有已對應和未對應的不同區域，也會有不同大小的表面格式 (亦即不同磚圖形)。

以下是理想的行為︰

在遺失磚中從 RTV 的讀取傳回 0，寫入會捨棄。 [第 2 層](tier-2.md)不需要此寫入處理的理想定義；對非對應磚的寫入最後可能會在快取，後續讀取可能會收取此資料。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流資源的存取管線](pipeline-access-to-streaming-resources.md)

 

 




