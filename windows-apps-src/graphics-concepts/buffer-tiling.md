---
title: 緩衝區拼貼
description: 緩衝資源分為 64KB 的磚，若大小不是 64 KB 的倍數，則最後一個磚會有某些空白空間。
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- 緩衝區拼貼
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f3e5e117e05cef478ede508240a6b1d1022dea70
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655723"
---
# <a name="buffer-tiling"></a>緩衝區拼貼


[緩衝區](introduction-to-buffers.md)資源分為 64KB 的磚，若大小不是 64 KB 的倍數，則最後一個磚會有某些空白空間。

結構化的緩衝區對於要並排顯示的分散必須無限制。 但若一開始就將其並排顯示，便可能失去利用 [**StructuredBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff471514) 所達到的可能硬體效能最佳化。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[資料流資源的區域會並排顯示方式](how-a-streaming-resource-s-area-is-tiled.md)

 

 




