---
title: 串流資源的存取管線
description: 串流資源可用於著色器資源檢視(SRV)、轉譯目標檢視 (RTV)、深度樣板檢視 (DSV) 和未排序存取檢視 (UAV)，以及一些未使用檢視的繫結點，例如頂點緩衝區繫結。
ms.assetid: 18DA5D61-930D-4466-8EFE-0CED566EA4A6
keywords:
- 串流資源的存取管線
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: b650188f6e4306a8dbdaae58e333128408d0f076
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: Auto
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652697"
---
# <a name="pipeline-access-to-streaming-resources"></a>串流資源的存取管線


串流資源可用於著色器資源檢視(SRV)、轉譯目標檢視 (RTV)、深度樣板檢視 (DSV) 和未排序存取檢視 (UAV)，以及一些未使用檢視的繫結點，例如頂點緩衝區繫結。 如需支援的繫結清單，請參閱[串流資源建立參數](streaming-resource-creation-parameters.md)。 各種 D3D 複製作業也負責串流資源。

如果有一個或多個檢視中的多個磚座標繫結相同的記憶體位置、從不同路徑讀取和寫入方式到相同記憶體，會發生不確定和非重複順序的記憶體存取。

如果從著色器的記憶體存取使用量後所有磚對應到獨特的磚，所有實作到以非磚方式具有相同的記憶體內容的表面的行為是相同的。

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
<td align="left"><p><a href="srv-behavior-with-non-mapped-tiles.md">非對應磚的 SRV 行為</a></p></td>
<td align="left"><p>著色器資源檢視 (SRV) 涉及非對應磚的讀取行為取決於硬體支援程度。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="uav-behavior-with-non-mapped-tiles.md">非對應磚的 UAV 行為</a></p></td>
<td align="left"><p>未排序存取檢視 (UAV) 讀取和寫入的行為取決於硬體支援程度。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterizer-behavior-with-non-mapped-tiles.md">非對應磚的轉譯器行為</a></p></td>
<td align="left"><p>本節說明非對應磚的轉譯器行為。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-access-limitations-with-duplicate-mappings.md">重複對應的磚存取限制</a></p></td>
<td align="left"><p>重複對應的磚有存取限制，例如複製來源和目的地重疊的資料流資源時，或轉譯至在轉譯區域中共用的磚時。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources-texture-sampling-features.md">串流資源紋理取樣功能</a></p></td>
<td align="left"><p>串流資源紋理取樣功能包含取得有關對應區域的著色器狀態回饋、檢查存取的所有資料是否都在資源中對應、鉗制以協助著色器避免 mipmap 串流資源中已知為非對應的區域，以及探索整個紋理篩選足跡的完全對應最低 LOD。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="hlsl-streaming-resources-exposure.md">HLSL 串流資源曝光</a></p></td>
<td align="left"><p>要支援<a href="https://msdn.microsoft.com/library/windows/desktop/ff471356">著色器模型 5</a>中的串流資源，需要特定 Microsoft 高階著色器語言 (HLSL) 語意。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流資源](streaming-resources.md)

 

 




