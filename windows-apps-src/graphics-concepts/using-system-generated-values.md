---
title: 使用系統產生的值
description: 系統產生的值由輸入組合語言 (IA) 階段產生 (根據使用者提供輸入語意) 允許特定著色器作業效率。
ms.assetid: C7CBA81D-68CA-4E9A-95E3-8185C280C843
keywords:
- 使用系統產生的值
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9f187495568892f5b489f6e109669811f4c45ab1
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5871547"
---
# <a name="span-iddirect3dconceptsusingsystem-generatedvaluesspanusing-system-generated-values"></a><span id="direct3dconcepts.using_system-generated_values"></span>使用系統產生的值


系統產生的值由[輸入組合語言 (IA) 階段](input-assembler-stage--ia-.md)產生 (根據使用者提供輸入[語意](https://msdn.microsoft.com/library/windows/desktop/bb509647)) 允許特定著色器作業效率。 藉由附加像執行個體識別碼 (顯現於[頂點著色器 (VS) 階段](vertex-shader-stage--vs-.md))、頂點識別碼 (顯現於 VS) 或基本識別碼 (顯現於[幾何著色器 (GS) 階段](geometry-shader-stage--gs-.md)/[像素著色器 (PS) 階段](pixel-shader-stage--ps-.md))，後續著色器階段可能會尋找這些系統值，以在該階段進行最佳化處理。

例如，VS 階段可能會尋找執行個體識別碼，為著色器抓取每個頂點其他資料或執行其他作業；GS 和 PS 階段可能使用基本類型識別碼，以相同方式抓取每個基本類型資料。

## <a name="span-idvertexidspanspan-idvertexidspanspan-idvertexidspanvertexid"></a><span id="VertexID"></span><span id="vertexid"></span><span id="VERTEXID"></span>VertexID


每個著色器階段使用頂點識別碼來找出每個頂點。 它是預設值為 0 的 32 位元不帶正負號整數。 當[輸入組合語言 (IA) 階段](input-assembler-stage--ia-.md)處理基本類型時，將它指派給頂點。 附加頂點識別碼語意至著色器輸入宣告，通知 IA 階段產生每個頂點識別碼。

IA 會將頂點識別碼新增至每個頂點，以供著色器階段使用。 針對每個繪製呼叫，頂點識別碼就會增加 1。 跨索引的繪製呼叫，計數會重設回到起點。 如果頂點識別碼溢位（超過 2³²– 1），會繞回到 0。

針對所有基本類型，頂點都有相關的頂點識別碼（無論相鄰關係）。

## <a name="span-idprimitiveidspanspan-idprimitiveidspanspan-idprimitiveidspanprimitiveid"></a><span id="PrimitiveID"></span><span id="primitiveid"></span><span id="PRIMITIVEID"></span>PrimitiveID


每個著色器階段使用基本類型識別碼來找出每個基本類型。 它是預設值為 0 的 32 位元不帶正負號整數。 當[輸入組合語言 (IA) 階段](input-assembler-stage--ia-.md)處理基本類型時，將它指派給基本類型。 若要通知 IA 階段產生基本類型識別碼，請將基本類型識別碼語意附加至著色器輸入宣告。

IA 階段會將基本類型識別碼新增到每個基本類型以供[幾何著色器 (GS) 階段](geometry-shader-stage--gs-.md)或[頂點著色器 (VS) 階段](vertex-shader-stage--vs-.md)使用（以 IA 階段之後的第一個作用中階段為準）。 針對每個索引的繪製呼叫，基本類型識別碼會增加 1。但是當開始新的執行個體時，基本類型識別碼會重設為 0。 所有其他繪製呼叫不會變更執行個體識別碼的值。 如果執行個體識別碼溢位（超過 2³²– 1），會繞回到 0。

[像素著色器 (PS) 階段](pixel-shader-stage--ps-.md)沒有基本類型識別碼的個別輸入；不過，任何指定基本類型識別碼的像素著色器輸入都會使用常數內插模式。

不支援自動產生相鄰基本類型的基本類型識別碼。 對於有相鄰關係的基本類型（例如有相鄰關係的三角形連環），僅保留內部基本類型（非相鄰基本類型）的基本類型識別碼，就像是沒有相鄰關係的三角形連環中的基本類型集合。

## <a name="span-idinstanceidspanspan-idinstanceidspanspan-idinstanceidspaninstanceid"></a><span id="InstanceID"></span><span id="instanceid"></span><span id="INSTANCEID"></span>InstanceID


每個著色器階段使用執行個體識別碼，來辨識目前處理中幾何的執行個體。 它是預設值為 0 的 32 位元不帶正負號整數。

如果頂點著色器輸入宣告包含執行個體識別碼語意，[輸入組合語言 (IA) 階段](input-assembler-stage--ia-.md)會新增執行個體識別碼至每個頂點。 針對每個索引的繪製呼叫，執行個體識別碼會增加 1。 所有其他繪製呼叫不會變更執行個體識別碼的值。 如果執行個體識別碼溢位（超過 2³²– 1），會繞回到 0。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>範例


下圖顯示在[輸入組合語言 (IA) 階段](input-assembler-stage--ia-.md)中系統值如何附加到執行個體化的三角形連環。

![執行個體化三角形連環系統值的圖例](images/d3d10-ia-example.png)

這些表格顯示兩個有相同三角形連環的執行個體的系統產生值。 第一個執行個體（執行個體 U）以藍色顯示，第二個執行個體（執行個體 V）以綠色顯示。 實線連接基本類型中的頂點，虛線連接相鄰頂點。

下表顯示執行個體 U 的系統產生值。

| 頂點資料    | C,U | D,U | E,U | F,U | G,U | H,U | I,U | J,U | K,U | L,U |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

 

三角形連環執行個體 U 有 3 個三角形基本類型，具有下列系統產生值：

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 0   | 0   | 0   |

 

下表顯示執行個體 V 的系統產生值。

| 頂點資料    | C,V | D,V | E,V | F,V | G,V | H,V | I,V | J,V | K,V | L,V |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   |

 

三角形連環執行個體 V 有 3 個三角形基本類型，具有下列系統產生值：

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 1   | 1   | 1   |

 

[輸入組合語言 (IA) 階段](input-assembler-stage--ia-.md)產生識別碼 (頂點、基本類型，以及執行個體)。另請注意，每個執行個體都有唯一執行個體識別碼。 資料以區域剪切結束，分隔每個三角形連環執行個體。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[輸入組合語言 (IA) 階段](input-assembler-stage--ia-.md)

 

 




