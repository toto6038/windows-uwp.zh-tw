---
title: 建立串流資源
description: 串流資源的建立方式是在建立資源時指定旗標，表示資源是串流資源。
ms.assetid: B3F3E43C-54D4-458C-9E16-E13CB382C83F
keywords:
- 建立串流資源
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ec96f6245969d32357563c44107f539fb9043aac
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7827087"
---
# <a name="creating-streaming-resources"></a>建立串流資源


串流資源的建立方式是在建立資源時指定旗標，表示資源是串流資源。

[串流資源建立參數](streaming-resource-creation-parameters.md)內詳述您可將資源建立為串流資源之時機的限制。

非串流資源的儲存空間在建立資源時於圖形系統中配置，例如為 2D 紋理陣列進行的配置。

建立串流資源時，圖形系統不會為資源內容配置儲存空間， 而是在應用程式建立串流資源時，圖形系統僅會為並排表面的區域保留位址空間，並允許應用程式控制磚的對應。 磚的「對應」僅為資源中的邏輯磚在記憶體中所指向的實體位置 (若是對應磚則為 **NULL**)。

請勿將此概念與為 CPU 存取對應 Direct3D 資源的概念混淆，其名稱雖相同但卻是完全獨立的概念。 您將可視需要個別為每個磚定義並變更對應，並知悉無須一次為一個表面對應所有磚，因此可有效率地使用可用的記憶體量。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本節內容


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="mappings-are-into-a-tile-pool.md">對應在磚集區中</a></p></td>
<td align="left"><p>將資源建立為串流資源時，組成資源的磚來自指向磚集區中的位置。 磚集區是記憶體的集區 (由一或多個配置於幕後備份 - 應用程式中看不見)。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resource-creation-parameters.md">串流資源建立參數</a></p></td>
<td align="left"><p>可以建立為串流資源的 Direct3D 資源類型上有一些限制。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tile-pool-creation-parameters.md">磚集區建立參數</a></p></td>
<td align="left"><p>使用本節的參數，在建立緩衝區時定義磚集區。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resource-cross-process-and-device-sharing.md">串流資源跨處理序和裝置共用</a></p></td>
<td align="left"><p>磚集區可以與其他處理序共用，就像傳統資源一樣。 參考磚集區的串流資源無法跨裝置和處理序共用。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="operations-available-on-streaming-resources.md">串流資源可用的作業</a></p></td>
<td align="left"><p>這個區段列出您可以在串流資源上執行的作業。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="operations-available-on-tile-pools.md">磚集區可用的作業</a></p></td>
<td align="left"><p>磚集區的作業包括調整磚集區的大小、提供資源 (為整個磚集區暫時將記憶體讓給系統使用) 以及回收資源。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="how-a-streaming-resource-s-area-is-tiled.md">串流資源區域的拼貼方式</a></p></td>
<td align="left"><p>建立串流資源時，維度、格式項目大小及 Mipmap 數目和/或陣列配量 (如適用) 決定了備份整個表面區域所需的磚數目。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流資源](streaming-resources.md)

 

 




