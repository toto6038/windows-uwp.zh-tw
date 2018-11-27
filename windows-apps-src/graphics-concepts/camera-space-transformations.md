---
title: 相機空間轉換
description: 相機空間端點的計算方式為使用全球檢視矩陣轉換物件端點。
ms.assetid: 86EDEB95-8348-4FAA-897F-25251B32B076
keywords:
- 相機空間轉換
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1b35fb71e51044ee6be6ed90001e3b5614c8cb45
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2018
ms.locfileid: "7710382"
---
# <a name="camera-space-transformations"></a>相機空間轉換


相機空間端點的計算方式為使用全球檢視矩陣轉換物件端點。

V = V \* wvMatrix

相機空間頂點標準的計算方式為，使用全球檢視矩陣的反向調換來轉換物件標準。 全球檢視矩陣不一定是正交。

N = N \* (wvMatrix⁻¹)<sup>T</sup>

矩陣反向和矩陣調換於4 x 4 矩陣運作。 乘積結合標準與產出之 4 x 4 矩陣的 3 x 3 部分。

如果轉譯狀態設為將標準正規化，則頂點標準向量將於相機空間轉換後正規化，如下所示︰

N = norm(N)

相機空間光線位置的計算方式為，使用檢視矩陣轉換光線來源位置。

Lₚ = Lₚ \* vMatrix

相機空間光線方向的計算方式為，將光線來源方向乘以檢視矩陣、正規化以及取補數結果。

L<sub>dir</sub> = -norm(L<sub>dir</sub> \* wvMatrix)

點光線及焦點光線位置的計算方式則如下所示︰

L<sub>dir</sub> = norm(V \* Lₚ)，其中參數在下表中定義。

| 參數       | 預設值 | 類型                                          | 描述                                               |
|-----------------|---------------|-----------------------------------------------|-----------------------------------------------------------|
| L<sub>dir</sub> | 無           | 3D 向量 ( x、y 和 z 浮點值) | 從物件頂點到光線的方向向量          |
| V               | N/A           | 3D 向量 ( x、y 和 z 浮點值) | 相機空間的頂點位置                           |
| wvMatrix        | 身份識別      | 浮點數值的 4 x 4 矩陣           | 複合矩陣包含全球及檢視轉換 |
| N               | 無           | 3D 向量 ( x、y 和 z 浮點值) | 頂點標準                                             |
| Lₚ              | N/A           | 3D 向量 ( x、y 和 z 浮點值) | 相機空間的光線位置                            |
| vMatrix         | 身份識別      | 浮點數值的 4 x 4 矩陣           | 矩陣包含檢視轉換                      |

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[光源的數學計算](mathematics-of-lighting.md)

 

 




