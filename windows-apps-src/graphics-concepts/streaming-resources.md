---
title: 串流資源
description: 串流處理資源是使用少量實體記憶體的大型邏輯資源。 會視需要串流少部分資源，而不傳遞整個大型資源。 串流資源先前稱為並排資源。
ms.assetid: 04F0486E-4B71-4073-88DA-2AF505F4F0C1
keywords:
- 串流資源
- 資源, 串流
- 資源, 區塊式
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c15c8a82219109a96d0a9ca192c4dfff5d86c9aa
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8343464"
---
# <a name="streaming-resources"></a>串流資源


*串流處理資源*是使用少量實體記憶體的大型邏輯資源。 會視需要串流少部分資源，而不傳遞整個大型資源。 串流資源先前稱為*並排資源*。

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
<td align="left"><p><a href="the-need-for-streaming-resources.md">串流資源的需求</a></p></td>
<td align="left"><p>需要串流資源，讓 GPU 記憶體不浪費在儲存未存取的表面區域，並告訴硬體如何跨相鄰磚篩選。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="creating-streaming-resources.md">建立串流資源</a></p></td>
<td align="left"><p>藉由在建立資源指定旗標，表示資源是串流資源時，建立串流資源。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pipeline-access-to-streaming-resources.md">串流資源的存取管線</a></p></td>
<td align="left"><p>串流資源可用於著色器資源檢視(SRV)、轉譯目標檢視 (RTV)、深度樣板檢視 (DSV) 和未排序存取檢視 (UAV)，以及一些未使用檢視的繫結點，例如頂點緩衝區繫結。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resources-features-tiers.md">串流資源功能層級</a></p></td>
<td align="left"><p>Direct3D 在三個功能層級中支援串流資源。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[Direct3D 圖形學習指南](index.md)

[資源](resources.md)

 

 




