---
title: 光線與紋理對應
description: 光線對應為紋理或紋理群組，其包含 3D 場景中的照明相關資訊。
ms.assetid: 5C7518D2-AC92-4A97-B7AF-4469D213D7BD
keywords:
- 光線與紋理對應
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6b5d245247d33f3c04839620615f2778ef7dfb59
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7832188"
---
# <a name="light-mapping-with-textures"></a>光線與紋理對應


光線對應為紋理或紋理群組，其包含 3D 場景中的照明相關資訊。 光線對應將光線和陰影對應到基本類型。 物件多重紋理混合，讓您的應用程式以比陰影技術更逼真外觀呈現場景。

應用程式要逼真呈現 3D 場景，必須考慮光源效果對場景外觀的影響。 雖然平面和 Gouraud shading 這方面的技術都是難能可貴的工具，但可能無法滿足您的需求。 Direct3D 支援物件多重紋理混合。 這些功能可讓應用程式的轉譯具有比單獨使用陰影技術呈現的場景更逼真的外觀場景。 藉由套用一或多個光線對應，您的應用程式可將光線和陰影區域對應到其基本類型。

光線對應為紋理或紋理群組，其包含 3D 場景中的照明相關資訊。 您可以在光線對應 alpha 值或色彩值或兩者中，儲存光源資訊。

如果您使用物件多重紋理混合執行光線對應，您的應用程式應將光線對應轉譯到第一輪基本類型。 它應該使用第二輪來呈現基底紋理。 例外是反射光線對應。 若是如此，第一次呈現基本紋理，接著新增光線對應。

多個紋理混合可讓應用程式一次呈現光線對應與基本紋理。 如果使用者的硬體提供多個紋理混合，您的應用程式應該利用它執行光線對應。 這會大幅改善應用程式效能。

使用光線對應，當在轉譯基本類型時，Direct3D 應用程式可以獲得各種照明效果。 它不僅可以在場景中對應單色及彩色光線，也可以新增細節，例如反射強光，並擴散光源。

有關使用 Direct3D 紋理混合執行光線對應的資訊，顯示於下列主題中。

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
<td align="left"><p><a href="monochrome-light-maps.md">單色光源對應</a></p></td>
<td align="left"><p>當較舊的 3D 加速板不支援使用目的地像素 alpha 值的紋理混合時，單色光線對應可讓較舊的介面卡執行物件多重紋理混合。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="color-light-maps.md">色彩光源對應</a></p></td>
<td align="left"><p>彩色光線對應使用光線對應中的 RGB 資料，作為光線資訊。 應用程式若使用彩色光線對應，通常會以更逼真的方式轉譯 3D 場景。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="specular-light-maps.md">反射光線對應</a></p></td>
<td align="left"><p>透過光源照明時，使用高度反射材質的光亮物件收到反射強光。 有時您可以透過套用反射光線對應至基本類型來取得更準確的強光，而不是使用由照明模組製造的反射強光。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="diffuse-light-maps.md">擴散光源對應</a></p></td>
<td align="left"><p>霧面表面有擴散光線反映。 擴散光線的亮度，取決於與光源的距離，以及表面法向和光源方向向量之間的角度。 紋理光線對應可模擬複雜擴散照明。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[紋理](textures.md)

 

 




