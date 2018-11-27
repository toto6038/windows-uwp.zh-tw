---
title: 危險追蹤與磚集區資源
description: 對於非串流資源，轉譯期間 Direct3D 可防止特定危險條件，但因為危險追蹤是在串流資源的磚層級，串流資源轉譯期間的追蹤危險條件可能會太昂貴。
ms.assetid: 8B0C73D3-3F77-41E8-B17D-C595DEE39E49
keywords:
- 危險追蹤與磚集區資源
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4dec176206aacb946bfd65341c483d8ba61558ad
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7714310"
---
# <a name="hazard-tracking-versus-tile-pool-resources"></a>危險追蹤與磚集區資源


對於非串流資源，轉譯期間 Direct3D 可防止特定危險條件，但因為危險追蹤是在串流資源的磚層級，串流資源轉譯期間的追蹤危險條件可能會太昂貴。

例如，對於非串流資源，執行階段不允許繫結任何特定 SubResource 同時做為輸入 (例如著色器資源檢視) 與做為輸出 (例如轉譯目標檢視)。如果發生這類情況，執行階段會解除繫結輸入。 執行階段中的這項追蹤額外負荷並不昂貴，會在子資源階段完成。 此追蹤額外負荷的其中一個優點是意外降低應用的機率，根據硬體著色器執行順序而定。 硬體著色器執行順序可能不同，如果不在特定圖形處理單元 (GPU) 上，那麼當然會跨不同的 GPU。

對於串流資源來說，追蹤資源繫結的方式可能太昂貴，因為是在磚層級追蹤。 新的問題發生，例如可能驗證嘗試轉譯為轉譯目標檢視，其中一個磚同步對應至表面的多個區域。 如果這類每個磚危險追蹤對於執行階段來說太昂貴，理想上至少會是偵錯層的選項。

當應用程式發出寫入或讀取作業至串流資源，而串流資源參考的磚集區記憶體也會被後續讀取或寫入作業的不同串流資源參考，但預期是第一個作業完成，後續作業才能開始時，它就必須通知顯示器驅動程式。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[對應在磚集區中](mappings-are-into-a-tile-pool.md)

 

 




