---
title: 串流資源可使用的位址空間
description: 本章節指定虛擬位址空間，可供串流資源使用。
ms.assetid: 145EB4A3-3910-4126-BC7E-A4CF53E2A098
keywords:
- 串流資源可使用的位址空間
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0b6e3f8080d33f9aadf22d5d5b1ebdd9a4739e16
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6838286"
---
# <a name="address-space-available-for-streaming-resources"></a>串流資源可使用的位址空間


本章節指定虛擬位址空間，可供串流資源使用。

在 64 位元作業系統上，有至少 40 位元的虛擬位址空間 (1 Tb) 可用。

若為 32 位元的作業系統，位址空間為 32 位元 (4 GB)。 若為 32 位元 ARM 系統，如果配置使用超過 27 位元的位址空間 (128 MB)，則建立個別串流資源可能會失敗。 這包括位址中任何隱藏的邊框間距，硬體空間可能會將其用於 Mipmap、壓縮磚邊框間距，以及 2 之乘冪的邊框間距表面維度。

在使用不同分頁表的圖形處理器 (GPU) 圖形系統上，應用程式所做的 GPU 資源將可使用多數此種位址空間，即便顯示器驅動程式所做的 GPU 配置可納於相同的空間。

在 CPU 和 GPU 所共用之分頁表的未來系統上，處理程序的所有 CPU 和 GPU 配置會共用可用位址空間。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流資源建立參數](streaming-resource-creation-parameters.md)

 

 




