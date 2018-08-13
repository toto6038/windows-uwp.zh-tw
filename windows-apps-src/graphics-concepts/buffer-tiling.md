---
title: 緩衝區拼貼
description: 緩衝資源分為 64KB 的磚，若大小不是 64 KB 的倍數，則最後一個磚會有某些空白空間。
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- 緩衝區拼貼
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 03769964bfe3eff13314e62b8594edd5509b26fb
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044243"
---
# <a name="buffer-tiling"></a>緩衝區拼貼


[緩衝](introduction-to-buffers.md)資源分為 64KB 的磚，若大小不是 64 KB 的倍數，則最後一個磚會有某些空白空間。

結構化的緩衝區對於要並排顯示的分散必須無限制。 但若一開始就將其並排顯示，便可能失去利用 [**StructuredBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff471514) 所達到的可能硬體效能最佳化。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流資源區域的拼接方式](how-a-streaming-resource-s-area-is-tiled.md)

 

 




