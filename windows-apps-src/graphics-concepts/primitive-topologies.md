---
title: 基本拓撲
description: Direct3D 支援數個基本拓撲，其定義管線如何轉譯及呈現頂點，例如點清單、線清單和三角形連環。
ms.assetid: 7AA5A4A2-0B7C-431D-B597-684D58C02BA5
keywords:
- 基本拓撲
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d7456cd773196520e066062c664f5e3073941dfe
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7574514"
---
# <a name="primitive-topologies"></a>基本拓撲


Direct3D 支援數個基本拓撲，其定義管線如何轉譯及呈現頂點，例如點清單、線清單和三角形連環。

## <a name="span-idprimitivetypesspanspan-idprimitivetypesspanspan-idprimitivetypesspanbasic-primitive-topologies"></a><span id="Primitive_Types"></span><span id="primitive_types"></span><span id="PRIMITIVE_TYPES"></span>基本類型拓撲


支援下列基本類型拓撲 (或基本類型)︰

-   [點清單](point-lists.md)
-   [線清單](line-lists.md)
-   [帶狀線](line-strips.md)
-   [三角形清單](triangle-lists.md)
-   [三角形連環](triangle-strips.md)

針對每個基本類型的視覺效果，查看本主題稍後的圖表[線圈方向和前置頂點位置](#winding-direction-and-leading-vertex-positions)。

[輸入組合語言 (IA) 階段](input-assembler-stage--ia-.md)讀取頂點和索引緩衝區的資料，將資料組合到下列基本類型，然後將資料傳送至剩餘管線階段。

## <a name="span-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanprimitive-adjacency"></a><span id="Primitive_Adjacency"></span><span id="primitive_adjacency"></span><span id="PRIMITIVE_ADJACENCY"></span>基本相鄰關係


所有 Direct3D 基本類型 (點清單除外) 均提供兩個版本︰一個具有相鄰關係的基本類型以及一個不具相鄰關係的基本類型。 具有相鄰關係的基本類型包含一些周圍頂點，而不具相鄰關係的基本類型僅包含目標基本類型的頂點。 例如，線清單基本類型有對應的包含相鄰關係的線清單基本類型。

相鄰基本類型旨在提供您更多幾何的相關資訊，並僅透過幾何著色器顯示。 相鄰關係適用於使用剪影偵測、陰影磁碟區立體化等的幾何著色器。

例如，假設您想要繪圖具有相鄰關係的三角形清單。 包含 36 個頂點 (具有相鄰關係) 的三角形清單將產生 6 個已完成的基本類型。 具相鄰關係的基本類型 (帶狀線除外) 包含的頂點數是不具相鄰關係的同等基本類型正好兩倍，其中每個額外的頂點是相鄰的頂點。

## <a name="span-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwinding-direction-and-leading-vertex-positionsspanwinding-direction-and-leading-vertex-positions"></a><span id="Winding_Direction_and_Leading_Vertex_Positions"></span><span id="winding_direction_and_leading_vertex_positions"></span><span id="WINDING_DIRECTION_AND_LEADING_VERTEX_POSITIONS"></span><span id="winding-direction-and-leading-vertex-positions"></span>線圈方向和前置頂點位置


如下圖所示，前置頂點是基本類型中的第一個非相鄰頂點。 基本類型可讓多個前置頂點經過定義，只要每個前置頂點是用於不同基本類型。

-   具相鄰關係三角形連環，前置頂點是 0、2、4、6 等。
-   具相鄰關係的帶狀線，前置頂點是 1、2、3 等。
-   換句話說，相鄰基本類型沒有前置頂點。

下圖顯示可產生輸入組合語言的所有基本類型的頂點排序。

![基本類型頂點排序圖表](images/d3d10-primitive-topologies.png)

前述圖中的符號在下表中說明。

| 符號                                                                                   | 名稱              | 描述                                                                         |
|------------------------------------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------------|
| ![頂點符號](images/d3d10-primitive-topologies-vertex.png)                     | 頂點            | 3D 空間中的點。                                                                |
| ![線圈方向的符號](images/d3d10-primitive-topologies-winding-direction.png) | 線圈方向 | 組合基本類型時的頂點排序。 可順時針方向或逆時鐘方向。 |
| ![前置頂點的符號](images/d3d10-primitive-topologies-leading-vertex.png)       | 前置頂點    | 包含每個常數資料的基本類型中的第一非相鄰頂點。       |

 

## <a name="span-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspangenerating-multiple-strips"></a><span id="Generating_Multiple_Strips"></span><span id="generating_multiple_strips"></span><span id="GENERATING_MULTIPLE_STRIPS"></span>產生多條寬帶


您可以透過寬帶切割產生多條寬帶。 您可以明確呼叫 [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL 函式或將特殊索引值插入索引緩衝區，來執行寬帶切割。 這個值是 –1，32 位元索引是 0xffffffff 或 16 位元索引是 0xffff。

–1 的索引表示明確「剪下」或」重新啟動」目前的寬帶。 上一個索引完成上一個基本類型或寬帶，而下一個索引開始新的基本類型或寬帶。

如需產生多條寬帶的詳細資訊，請參閱[幾何著色器 (GS) 階段](geometry-shader-stage--gs-.md)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[輸入組譯工具 (IA) 階段](input-assembler-stage--ia-.md)

[圖形管線](graphics-pipeline.md)

 

 




