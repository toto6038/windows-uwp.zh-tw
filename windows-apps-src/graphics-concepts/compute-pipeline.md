---
title: 計算管線
description: Direct3D 計算管線主要設計用來處理大部分可與圖形管線平行進行的計算。
ms.assetid: 355B66C6-C0DF-47BA-A9C9-7AFA50B5B614
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 36d1bd524d6e71b0a1aa9477d7a2b7a5f27544aa
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750044"
---
# <a name="compute-pipeline"></a>計算管線


\[某些資訊與預先發行的產品有關，在正式發行之前可能會經過大幅修改。 針對此處提供的資訊，Microsoft 不做任何明示或默許的擔保。\]


Direct3D 計算管線主要設計用來處理大部分可與圖形管線平行進行的計算。 計算管線中只有幾個步驟：資料透過可程式化著色器階段，從輸入端流向輸出端。

## <a name="purpose"></a>目的

就像其他可程式化的著色器，[計算著色器 (CS) 階段](compute-shader-stage--cs-.md)利用 HLSL 進行設計與實作。 計算著色器提供高速一般用途運算，並且利用了圖形處理器 (GPU) 大量平行處理器的優勢。 計算著色器提供記憶體共用和執行緒同步處理功能，以允許更有效的平行程式設計方法。 |

## <a name="input"></a>輸入

與其他可程式化著色器不同，輸入的定義其實相當抽象。 輸入在本質上可以是一維、二維，或是三維的，並且決定了呼叫計算著色器執行的引動過程數量。 您可以為一組要讀取的調用定義共用資料。 |

## <a name="output"></a>輸出

來自計算著色器的輸出資料可能各不相同，並且可以在需要計算資料時與圖形轉譯管線同步。

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">Like other programmable shaders, <a href="#compute-shader-stage--cs-.md">Compute Shader (CS) stage</a> is designed and implemented with HLSL. A compute shader provides high-speed general purpose computing and takes advantage of the large numbers of parallel processors on the graphics processing unit (GPU). The compute shader provides memory sharing and thread synchronization features to allow more effective parallel programming methods.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike other programmable shaders, the definition of input is abstract. The input can be one, two or three-dimensional in nature, determining the number of invocations of the compute shader to execute. It is possible to define shared data for one set of invocations to read.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Output data from the compute shader, which can be highly varied, can be synchronized with the graphics rendering pipeline when the computed data is required.</td>
</tr>
</tbody>
</table>
-->

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[Direct3D 圖形學習指南](index.md)

 

 
