---
title: 轉換
description: 在 Direct3D 中，轉換引擎負責將幾何推入固定函式幾何管線。
ms.assetid: 0DF2A99A-335C-4D14-9720-6D7996DD635A
keywords:
- 轉換
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a29d42a9254ca47402a38ea71c8c1ef69de5c6c7
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8686038"
---
# <a name="transforms"></a>轉換


在 Direct3D 中，轉換引擎負責將幾何推入固定函式幾何管線。 它在世界中尋找模型和檢視器，將頂點投影在螢幕上顯示，並將頂點裁剪至檢視區。 轉換引擎也會執行光線計算，以判斷每個頂點上的擴散和反射元件。

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
<td align="left"><p><a href="transform-overview.md">轉換概觀</a></p></td>
<td align="left"><p>矩陣轉換處理很多 3D 圖形低階數學運算。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="world-transform.md">世界矩陣轉換</a></p></td>
<td align="left"><p>世界矩陣轉換將座標從模型空間（其中頂點是相對於模型的區域原點定義的）變更為世界空間。 在世界空間，頂點是相對於場景中所有物件之通用原點定義的。 世界矩陣轉換將模型放置到世界。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="view-transform.md">檢視轉換</a></p></td>
<td align="left"><p><em>檢視轉換</em>將檢視器放置在世界空間中，並將頂點轉換成相機空間。 在相機空間，相機或檢視器位於原點，朝著正 z 方向看。 檢視矩陣圍繞相機位置 (相機空間的起點) 將物件重新放置在世界空間中。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="projection-transform.md">投影轉換</a></p></td>
<td align="left"><p><em>投影轉換</em>控制相機內部，例如選擇相機的鏡頭。 這是三種轉換類型中最複雜的轉換。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[座標系統與幾何](coordinate-systems-and-geometry.md)

 

 




