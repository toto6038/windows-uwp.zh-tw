---
title: 串流資源可用的作業
description: 這個區段列出您可以在串流資源上執行的作業。
ms.assetid: 700D8C54-0E20-4B2B-BEA3-20F6F72B8E24
keywords:
- 串流資源可用的作業
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1289b01e7ffb780c7e3faa52585eb5f002cf519c
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5931512"
---
# <a name="operations-available-on-streaming-resources"></a>串流資源可用的作業


這個區段列出您可以在串流資源上執行的作業。

-   傳回 void 的更新磚對應作業及傳回 void 的複製磚對應作業 - 這些作業將串流資源中的磚位置指向磚集區中的位置或是 NULL，或者二者。 這些作業可以更新不相鄰子集的磚指標。
-   複製和更新作業 - 可以在預設集區介面來回複製資料的所有 API 適用於串流資源。 從未對應的磚讀取會產生 0，而對未對應的磚寫入則會捨棄。
-   複製磚和更新磚作業 - 在任何串流資源和標準記憶體配置的緩衝區以 64 KB 細微度來回複製磚則有這些作業。 顯示器驅動程式和硬體執行串流資源所需的任何記憶體調配。
-   可在非串流資源上運作的 Direct3D 管線繫結和檢視建立 / 繫結也可在串流資源上運作。

磚控制項適用於即時或延遲內容 (就像一般資源的更新)，並且在執行時會影響後續對於磚的存取 (非先前提交的作業)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[建立串流資源](creating-streaming-resources.md)

 

 




