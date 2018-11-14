---
title: 非對應磚的 UAV 行為
description: 未排序存取檢視 (UAV) 讀取和寫入的行為取決於硬體支援程度。
ms.assetid: CDB224E2-CC07-4568-9AAC-C8DC74536561
keywords:
- 非對應磚的 UAV 行為
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a5418de3646f70a815f5c482f9063ea3e48ddfa1
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6651902"
---
# <a name="span-iddirect3dconceptsuavbehaviorwithnon-mappedtilesspanuav-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.uav_behavior_with_non-mapped_tiles"></span>非對應磚的 UAV 行為


未排序存取檢視 (UAV) 讀取和寫入的行為取決於硬體支援程度。 如需需求明細，請參閱[串流資源功能層級](streaming-resources-features-tiers.md)的整體讀取和寫入行為。 本節摘要理想的行為。

讀取 UAV 非對應磚的著色器作業，對於格式的所有非遺失元件會傳回 0，對遺失元件則傳回預設值。

嘗試寫入到非對應磚的著色器作業，會導致無法寫入到非對應的區域（而繼續寫入到對應的區域）。 [第 2 層](tier-2.md)不需要此寫入處理的理想定義；對非對應磚的寫入最後可能會在快取，後續讀取可能會收取此資料。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流資源的存取管線](pipeline-access-to-streaming-resources.md)

 

 




