---
title: 緩衝區拼貼
description: 緩衝資源分為 64KB 的磚，若大小不是 64 KB 的倍數，則最後一個磚會有某些空白空間。
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- 緩衝區拼貼
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fdb78dc2cff6ccedec58acbc946068e14511fdc2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156352"
---
# <a name="buffer-tiling"></a>緩衝區拼貼


如果大小不是64KB 的倍數，則 [緩衝區](introduction-to-buffers.md) 資源會分割成64kb 個磚，並在最後一個磚中顯示一些空白空間。

結構化的緩衝區對於要並排顯示的分散必須無限制。 但若一開始就將其並排顯示，便可能失去利用 [**StructuredBuffers**](/windows/desktop/direct3dhlsl/sm5-object-structuredbuffer) 所達到的可能硬體效能最佳化。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[如何拼貼串流資源區域](how-a-streaming-resource-s-area-is-tiled.md)

 

 