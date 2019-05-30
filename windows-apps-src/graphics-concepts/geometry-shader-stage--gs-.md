---
title: 幾何著色器 (GS) 階段
description: 幾何著色器 (GS) 階段會處理整個基本類型三角形、線條和點，以及它們相鄰的頂點。
ms.assetid: 8A1350DD-B006-488F-9DAF-14CD2483BA4E
keywords:
- 幾何著色器 (GS) 階段
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0ea3e7ec73b042eeef560af3d88754afdfa5b441
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370465"
---
# <a name="geometry-shader-gs-stage"></a>幾何著色器 (GS) 階段


幾何著色器 (GS) 階段會處理整個基本類型：三角形、線條和點，以及它們相鄰的頂點。 它對於演算法來說很實用，包括 Point Sprite Expansion、Dynamic Particle Systems 及 Shadow Volume Generation。 它支援幾何放大和取消放大。

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途和用法


幾何著色器階段會處理整個基本類型：三角形 (3 個頂點，最多有 3 個相鄰的頂點)、線條 (2 個頂點，最多有 2 個相鄰的頂點) 及點 (1 個頂點)。

![有相鄰頂點的三角形和線條的圖](images/d3d10-gs.png)

幾何著色器也支援有限的幾何放大以及取消放大。 即使有輸入基本類型，幾何著色器可捨棄基本類型，或發出一種或多種新的基本類型。

幾何著色器 (GS) 階段是可程式化的著色器階段。它在[圖形管線](graphics-pipeline.md)圖表中顯示為圓角區塊。 這個著色器階段會公開自己的獨特功能，以著色器模型建置 (請參閱[通用著色器核心](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core))。

幾何著色器階段非常適合演算法，包括：

-   Point Sprite Expansion
-   Dynamic Particle Systems
-   Fur/Fin Generation
-   Shadow Volume Generation
-   Single Pass Render-to-Cubemap
-   Per-Primitive Material Swapping
-   Per-Primitive Material Setup - 此功能包括產生質心座標做為基本類型資料，如此像素著色器就能執行自訂屬性插補。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>輸入


幾何著色器階段會執行應用程式指定的著色器程式碼，以整個基本類型做為輸入，並且能夠在輸出上產生頂點。 與在單一頂點上操作的頂點著色器不同的是，幾何著色器的輸入是完整基本類型 (三角形的三個頂點、線條的兩個頂點，或點的單一頂點) 的頂點。 幾何著色器還可以帶入邊緣相鄰的基本類型的頂點資料做為輸入 (三角形有額外三個頂點，線條有額外兩個頂點)。

幾何著色器階段可能會耗用**SV\_PrimitiveID**系統產生的值是由自動產生[輸入組譯工具 (IA) 階段](input-assembler-stage--ia-.md)。 這樣就可以在需要時提取或運算每個基本類型資料。

當幾何著色器為使用中時，它會針對向下傳遞或先前在管線中產生的每個基本類型叫用一次。 每次叫用幾何著色器，都會視為輸入叫用基本類型的資料，無論是單一點、單一線條或單一三角形。 管線中較早的三角形連環會導致針對連環中每個個別三角形叫用幾何著色器 (就像是連環向外擴充至三角形清單)。 個別基本類型中每個頂點的所有輸入資料都會提供 (也就是三角形的 3 個頂點)，加上相鄰頂點資料 (如適用且可提供的話)。

常見的頂點縮寫：

|     |                 |
|-----|-----------------|
| TV  | 三角形頂點 |
| LV  | 線條頂點     |
| AV  | 相鄰頂點 |

 

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Output


幾何著色器 (GS) 階段能夠輸出多個頂點，以形成選取的單一拓撲。 可用的幾何著色器輸出拓撲包括 **tristrip**、**linestrip** 和 **pointlist**。 發出的基本類型數目可在幾何著色器的任何叫用內自由改變，雖然可發出的頂點最大數目必須以靜態方式宣告。 從幾何著色器叫用發出的連環長度可以是任意的，而新的連環可以透過 [RestartStrip](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) HLSL 功能建立。

從其他叫用執行幾何著色器執行個體是不可部分完成的，除非新增至資料流的資料為序列。 特定幾合著色器叫用的輸出與其他叫用無關 (雖然會遵循順序)。 產生三角形連環的幾何著色易會在每次叫用時開始新的連環。

幾何著色器輸出可能會在記憶體中透過資料流輸出階段，對點陣化階段和/或頂點緩衝區供給。 對記憶體供給的輸出會擴充至個別點/線條/三角形清單 (與它們傳遞進入點陣化一模一樣)。

幾何著色器一次輸出一個頂點的資料，藉由將頂點附加至輸出資料流物件。 資料流拓撲是由固定宣告所決定，選擇 **TriangleStream**、**LineStream** 和 **PointStream** 做為 GS 階段的輸出。

有三種類型的資料流物件：**TriangleStream**， **LineStream**並**PointStream**，這些是所有樣板化的物件。 輸出的拓撲是由各自的物件類型決定，而附加至資料流的頂點的格式是由範本類型決定。

幾何著色器輸出做為系統解譯值的識別時 (例如**SV\_RenderTargetArrayIndex**或**SV\_位置**)，硬體會查看此資料和會執行一些行為取決於值，除了能夠傳遞本身來在下一步 的著色器階段輸入資料。 幾何著色器這類資料輸出當有意義的硬體上的每個基本的基礎 (例如**SV\_RenderTargetArrayIndex**或是**SV\_ViewportArrayIndex**)，而不是以每個頂點為基礎 (例如**SV\_ClipDistance\[n\]** 或**SV\_位置**)，每個基本資料取自前置頂點發出基本項目。

部分完成的基本類型可能是由幾合著色器產生，如果幾何著色器結束且基本類型未完成。 未完成的基本類型會捨棄且無訊息。 這類似 IA 處理部分未完成基本類型的方式。

幾何著色器可以執行載入和紋理取樣作業，在不需要螢幕空間衍生項目時 (**samplelevel**、**samplecmplevelzero**、**samplegrad**)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[圖形管線](graphics-pipeline.md)

 

 




