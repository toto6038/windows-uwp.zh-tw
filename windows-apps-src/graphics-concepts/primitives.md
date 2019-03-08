---
title: 基本型別
description: 3D 基本類型是形成單一 3D 實體的頂點集合。
ms.assetid: A1FE6F49-B0D4-4665-90E1-40AD98E668B1
keywords:
- 基本型別
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6c2bf3aa2f421efa7061f3003e6cab8c9f7b8c58
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648663"
---
# <a name="primitives"></a>基本型別


3D「原始物件」是形成單一 3D 實體的許多頂點。

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
<td align="left"><p><a href="point-lists.md">點清單</a></p></td>
<td align="left"><p>點清單是轉譯為隔離點的頂點集合。 應用程式可以在 3D 場景中使用點清單，在多邊形的表面上有星星欄位或點線。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="line-lists.md">列清單</a></p></td>
<td align="left"><p>線清單是隔離、直線段的清單。 在 3D 場景中加入冰雹或暴雨的這類工作適合使用線清單。 應用程式藉由填入頂點陣列來建立線清單。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="line-strips.md">行帶狀線</a></p></td>
<td align="left"><p>帶狀線是包含連接的帶狀線的基本類型。 您的應用程式可使用帶狀線建立未閉合之多邊形。 封閉的多邊形是用線段將其最後一個頂點連接到第一個頂點的多邊形。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="triangle-lists.md">三角形清單</a></p></td>
<td align="left"><p>三角形清單是隔離三角形的清單。 隔離的三角形不一定彼此相近。 三角形清單必須至少有 3 個頂點，而且頂點總數必須可被 3 整除。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="triangle-strips.md">三角形帶狀線</a></p></td>
<td align="left"><p>三角形連環是一系列的連接三角形。 因為是連接三角形，應用程式不需要重複指定每個三角形的所有三個頂點。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[座標系統和幾何](coordinate-systems-and-geometry.md)

 

 




