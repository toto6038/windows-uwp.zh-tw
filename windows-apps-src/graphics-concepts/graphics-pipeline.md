---
title: 圖形管線
description: Direct3D 圖形管線專為即時遊戲應用程式產生圖形而設計。 透過每個可設定或可編程階段，從輸入到輸出的資料流。
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- 圖形管線
- 管線階段
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6cb50af5facdbf4271c9911f0e1f536dead0b8b8
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044907"
---
# <a name="graphics-pipeline"></a>圖形管線


Direct3D 圖形管線專為即時遊戲應用程式產生圖形而設計。 透過每個可設定或可編程階段，從輸入到輸出的資料流。

所有階段都可以使用 Direct3D API 設定。 搭配常見著色器核心的階段 (圓角矩形區塊) 可使用 [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561) 程式語言進行程式設計。 如此可讓管線極具彈性和適應能力。

最常使用的是頂點著色器 (VS) 階段和像素著色器 (PS) 階段。 如果您連這些著色器階段都未提供，則會使用預設的沒有選項、傳遞頂點和像素著色器。

![Direct3D 11 可程式化管線中的資料流程圖](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>輸入組合語言階段

|-|-| |目的 |[輸入 Assembler (IA-64) 階段](input-assembler-stage--ia-.md)供應基本和管線，例如三角形、 線條和點的相鄰關係資料包括語意識別碼來提升著色更有效率降低尚未已經被的基本項目處理處理。 ||輸入 |（三角形、 lines、 和/或點為單位），從使用者填入緩衝區在記憶體中的基本資料。 以及可能相鄰的資料。 三角形就是對每個三角形的 3 個頂點與每個三角形的相鄰關係資料可能是 3 個頂點。 ||輸出 |基本項目 （例如基本識別碼、 執行個體識別碼或頂點識別碼） 附加系統產生的值。 |

## <a name="vertex-shader-stage"></a>頂點著色器階段

|-|-| |目的 |[頂點著色器 (VS) 階段](vertex-shader-stage--vs-.md)處理頂點，通常執行作業，例如轉換、，面板，及光源。 頂點著色器採用單一輸入頂點，並產生單一輸出頂點。 每個頂點的個別作業，例如轉換 （英文）、 Skinning、 Morphing 及每個頂點與光源。 ||輸入 |單一頂點、 VertexID 和 InstanceID 系統產生的值。 每個頂點著色器輸入的頂點可以包含下列的最多 16 個 32 位元向量 （每個最多 4 個元件）。 ||輸出 |單一頂點。 每個輸出頂點可以組成多達 16 32 位元 4-component 向量。 |
 
## <a name="hull-shader-stage"></a>輪廓著色器階段
 
|-|-| |目的 |[船殼著色器 (1) 階段](hull-shader-stage--hs-.md)是其中一個鑲嵌階段，其有效率地自動換行設定成許多三角形模型的單一表面。 輪廓著色器會針對每個塊面叫用一次，並且將定義低位表面的輸入控制點轉換成構成塊面的控制點。 它也會提供資料 Tessellator (TS) 階段與網域具備 Shader (DS) 階段某些個別修補程式計算。 ||輸入 |介於 1 到 32 的輸入的控制項點，其中一起定義低順序表面。 ||輸出 |介於 1 到 32 輸出控制點，其組成修補程式。 船殼具備 shader 宣告狀態 Tessellator (TS) 階段，包括數目控制點、 修補程式圖像的類型及分割 tessellating 時所使用的類型。 |

## <a name="tessellator-stage"></a>曲面細分器階段

|-|-| |目的 |[Tessellator (TS) 階段](tessellator-stage--ts-.md)是建立代表幾何修補程式和會產生一組連線在這些範例的較小物件 （三角形、 點或行） 的網域取樣圖樣。 ||輸入 |Tessellator 會使用鑲嵌因素 （這會指定如何自助式將 tessellated 網域） 和類型分割 （這會指定用來安裝的修補程式切割的演算法） 從船殼具備 shader 階段傳入的修補程式每一次作業。 | |輸出 |Tessellator 輸出 uv （並選擇性地 w） 座標與網域具備 shader 階段呈現的拓樸。 |

## <a name="domain-shader-stage"></a>網域著色器階段

|-|-| |目的 |[網域具備 Shader (DS) 階段](domain-shader-stage--ds-.md)計算中的輸出修補程式; 細分點頂點位置它會計算頂點位置對應到每個網域範例。 每個 tessellator 階段輸出點與具有唯讀存取權船殼具備 shader 輸出修補程式和輸出的修補程式常數和 tessellator 階段輸出 UV 座標之後執行網域具備 shader。 ||輸入 |網域具備 shader 消耗從[船殼著色器 (1) 階段](hull-shader-stage--hs-.md)的輸出控點。 輪廓著色器輸出包括：控制點、塊面常數資料及鑲嵌係數 (鑲嵌係數可能包括固定函式曲面細分器使用的值，以及整數鑲嵌進位之前的原始值，可簡化地理變形)。 從[Tessellator (TS) 階段](tessellator-stage--ts-.md)的輸出座標個別的 「 網域具備 shader 會叫用一次。 ||輸出 |網域具備 Shader (DS) 階段輸出中輸出修補程式細分點頂點位置。 |

## <a name="geometry-shader-stage"></a>幾何著色器階段

|-|-| |目的 |[[幾何著色器 (GS) 階段](geometry-shader-stage--gs-.md)處理整個基本項目： 三角形、 線條和點，以及其相鄰頂角。 它支援幾何放大和取消放大。 它對於演算法來說很實用，包括 Point Sprite Expansion、Dynamic Particle Systems、Fur/Fin Generation、Shadow Volume Generation、Single Pass Render-to-Cubemap、Per-Primitive Material Swapping 及 Per-Primitive Material Setup (包括產生質心座標做為基本類型資料，如此像素著色器就能執行自訂屬性插補)。 | |輸入 |不同於頂點著色、 操作單一頂點幾何著色器的輸入就是完整的基本 （三個三角形的頂點、 lines、 兩個頂點或單一頂點點） 的頂點。 ||輸出 |[幾何著色器 (GS) 階段都能夠輸出構成之單一所選的拓撲的多個頂點。 可用的幾何著色器輸出拓撲包括 <strong>tristrip</strong>、<strong>linestrip</strong> 和 <strong>pointlist</strong>。 發出的基本類型數目可在幾何著色器的任何叫用內自由改變，雖然可發出的頂點最大數目必須以靜態方式宣告。 從幾何著色器引動過程所發出的區域長度可以是任意，並透過[RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL 函數可建立新的階梯狀往。 |

## <a name="stream-output-stage"></a>資料流輸出階段

|-|-| |目的 |[資料流輸出 （等） 階段](stream-output-stage--so-.md)持續輸出 （或將資料流處理） 頂點資料從舊的作用中階段記憶體中的一或多個緩衝區。 資料串流出記憶體可以做為輸入的資料或從 CPU 的讀取回 recirculated 回管線。 ||輸入 |從舊的管線階段頂點資料。 ||輸出 |持續的資料流輸出 （等） 階段輸出 （或將串流傳送） 頂點資料從舊 active 階段，例如幾何著色器 (GS) 階段中，於記憶體中的一或多個緩衝區。 如果幾何著色器 (GS) 階段為非使用中且資料流輸出 （等） 階段為作用中，它會持續輸出頂點的資料從網域具備 Shader (DS) 階段記憶體 （或如果 DS 也是不在作用中，從頂點著色器 (VS) 階段） 的緩衝區。 |

## <a name="rasterizer-stage"></a>點陣化階段

|-|-| |目的 |[轉譯器 (RS) 階段](rasterizer-stage--rs-.md)多媒體項目基本項目不檢視中基本項目準備的像素著色器 (PS) 階段，並決定如何叫用像素著色。 轉換成 （組成的像素為單位） 的點陣圖像基於顯示即時 3D 圖形向量 （所組成的圖形或基本項目） 的資訊。 ||輸入 |頂點 （x、 y、 z、 w） 傳入到轉譯器階段假設兩者都是同質的多媒體藝廊空間中。 在此座標空間 X 軸右指指 Y 及 Z 會指向從相機。 ||輸出 |需要轉譯實際像素。 包含用於部分頂點屬性中所具備 Pixel Shader 內插。 |

## <a name="pixel-shader-stage"></a>像素著色器階段
 
|-|-| |目的 |[像素著色器 (PS) 階段](pixel-shader-stage--ps-.md)接收插補的資料的基本項目和所產生的每個像素色彩等的資料。 ||輸入 |當沒有幾何著色器設定管線時，具備 pixel shader 僅限於 16、 32 位元、 4-component 輸入。 否則，像素著色器最多可以採用 32、32 位元、4 個元件的輸入。 像素著色器輸入資料包括頂點屬性 (可插補，無論是否有透視修正)，也可以視為每個基本類型常數。 像素著色器輸入會從點陣化的基本類型的頂點屬性插補，根據宣告的插補模式。 如果在點陣化之前基本類型被裁剪，則插補模式也會在裁剪過程中採用。 | |輸出 |具備 pixel shader 可以輸出到 8，32 位元、 4-component 色彩或無色彩捨棄像素。 像素著色器輸出註冊元件必須宣告才能使用這些;每個登錄允許以不同的輸出寫入遮罩。 |

## <a name="output-merger-stage"></a>輸出合併階段
 
|-|-| |目的 |[輸出購 (OM) 階段](output-merger-stage--om-.md)結合各種類型的輸出資料 （像素著色器值、 深度和樣板資訊） 以產生最終管線結果轉譯目標和深度/樣板緩衝區的內容。 ||輸入 |輸出購輸入是管線狀態，像素著色器、 轉譯目標的內容及深度/樣板緩衝區的內容所產生的像素資料。 ||輸出 |最後一呈現像素色彩。 |

## <a name="related-topics"></a>相關主題

- [Direct3D 圖形學習指南](index.md)

- [計算管線](compute-pipeline.md)
