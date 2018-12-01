---
title: 磚集區可用的作業
description: 磚集區的作業包括調整磚集區的大小、提供資源 (為整個磚集區暫時將記憶體讓給系統使用) 以及回收資源。
ms.assetid: 90347F7F-C991-4287-BD70-494533ECDC8A
keywords:
- 磚集區可用的作業
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6b8b0c6f4fa578e4ec483492b320dc9bc346ab66
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8343375"
---
# <a name="operations-available-on-tile-pools"></a>磚集區可用的作業


磚集區的作業包括調整磚集區的大小、提供資源 (為整個磚集區暫時將記憶體讓給系統使用) 以及回收資源。

-   如同任何其他 Direct3D 資源，磚集區存留期的運作受參考計數的支援，包括此案例中從串流資源對應的追蹤。 當應用程式不再參考磚集區以及對於記憶體的任何磚對應消失，並且圖形處理單元 (GPU) 的存取完成時，作業系統便會解除配置磚集區。
-   與介面共用和同步相關的 API 適用於磚集區 (但不是直接在串流資源上)。 與所提供的磚集區的行為類似，若磚集區已經共用且目前有其他裝置和處理序需要磚集區，則存取指向磚集區的串流資源的 Direct3D 命令會被捨棄。
-   調整磚集區的大小。
-   提供資源和回收資源 - 這些將記憶體暫時讓給系統的作業是在整個磚集區上運作 (不適用於個別串流資源)。 如果資源串流指向所提供的磚集區中的某個磚，串流資源的行為會像是提供給集區一般 (例如，執行階段捨棄其所參考的命令)。

資料無法直接在磚集區記憶體中來回複製。 記憶體的存取始終要透過串流資源。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[建立串流資源](creating-streaming-resources.md)

 

 




