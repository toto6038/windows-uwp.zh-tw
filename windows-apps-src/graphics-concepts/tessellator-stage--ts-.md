---
title: 曲面細分器 (TS) 階段
description: 曲面細分器 (TS) 階段建立代表幾何塊面的網域取樣模式，並產生一組連接這些樣本的小物件 (三角形、點或線)。
ms.assetid: 2F006F3D-5A04-4B3F-8BC7-55567EFCFA6C
keywords:
- 曲面細分器 (TS) 階段
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1d57c60e8cba9be75e936c55800bac93f8df3e30
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5698989"
---
# <a name="tessellator-ts-stage"></a>曲面細分器 (TS) 階段


曲面細分器 (TS) 階段建立代表幾何塊面的網域取樣模式，並產生一組連接這些樣本的小物件 (三角形、點或線)。

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途和使用


下圖會反白顯示 Direct3D 圖形管線階段。

![direct3d 11 管線圖，反白顯示輪廓著色器、曲面細分器和網域著色器階段](images/d3d11-pipeline-stages-tessellation.png)

下圖顯示鑲嵌階段的進展。

![鑲嵌進展圖](images/tess-prog.png)

從低細節的細分表面開始進展。 進展接下來會使用對應幾何塊面、網域樣本，和連接這些樣本的三角形，反白顯示輸入塊面。 進展最後會反白顯示對應於這些樣本的頂點。

Direct3D 執行階段支援實作鑲嵌 (在 GPU 上將低細節的細分表面轉換為更高細節的基本類型) 的三個階段。 鑲嵌並排顯示高位表面 (或將其分裂) 成為適合轉譯的結構。

這些鑲嵌階段合作，將更高位的表面 (保持模型簡單且有效率) 轉換為許多三角形，在 Direct3D 圖形管線中詳細呈現。

鑲嵌使用 GPU，從四邊形塊面 (quad patch)、三角形塊面 (triangle patch) 或等高線 (isoline) 建構而來的表面計算更詳細的表面。 為了近似高位表面，每個塊面細分成三角形、點或使用鑲嵌因數的線。 Direct3D 圖形管線使用三個管線階段實作鑲嵌：

-   [輪廓著色器 (HS) 階段](hull-shader-stage--hs-.md) - 可程式化著色器階段，產生與每個輸入塊面 (四邊形、三角形或線) 相對應的幾何塊面 (和塊面常數)。
-   曲面細分器 (TS) 階段 - 固定函式管線階段，建立代表幾何塊面的網域取樣模式，並產生一組連接這些樣本的小物件 (三角形、點或線)。
-   [網域著色器 (DS) 階段](domain-shader-stage--ds-.md) - 可程式化著色器階段，計算對應於每個網域樣本的頂點位置。

藉由在硬體中實作鑲嵌，圖形管線可以評估較低細節的 (較少的多邊形數) 模型並以更高細節呈現。 雖然軟體鑲嵌是可做到的，但硬體實作的鑲嵌會產生不可思議的視覺細節數量 (包括位移對應的支援)，而不需要新增模型大小的視覺細節和造成運作癱瘓的更新頻率。

鑲嵌的優點：

-   鑲嵌節省大量記憶體和頻寬，讓應用程式從低解析度模型轉譯更高細節的表面。 在 Direct3D 圖形管線中實作的鑲嵌技巧也支援位移對應，這會產生讓人驚艷的表面細節數量。
-   鑲嵌支援可縮放轉譯技巧，例如持續或檢視相依的細節層級，可快速計算。
-   鑲嵌以較低頻率執行昂貴計算 (對較低細節的模型執行計算) 來改善效能。 這可能包括將混合形狀或變形目標 (morph target) 用於寫實動畫的混合計算，或用於撞擊偵測或軟體動力 (soft body dynamics) 的物理計算。

Direct3D 圖形管線在硬體實作鑲嵌，將工作從 CPU 卸載到 GPU。 如果應用程式實作大量變形目標和/或更加複雜的皮膚形變 (skinning)/變形模型，這可能會導致非常大的效能改進。

曲面細分器是固定函式階段，透過將[輪廓著色器](hull-shader-stage--hs-.md)繫結至管線而初始化。 (請參閱[如何：初始化曲面細分器階段](https://msdn.microsoft.com/library/windows/desktop/ff476341))。 曲面細分器階段的目的是將網域 (四邊形、三角形或線) 細分成很多較小的物件 (三角形、點或線)。 曲面細分器在標準化 (零到一) 座標系統中並排顯示標準網域。 例如，四邊形網域被鑲嵌為單位正方形。

### <a name="span-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanphases-in-the-tessellator-ts-stage"></a><span id="Phases_in_the_Tessellator__TS__stage"></span><span id="phases_in_the_tessellator__ts__stage"></span><span id="PHASES_IN_THE_TESSELLATOR__TS__STAGE"></span>曲面細分器 (TS) 階段中的相位

曲面細分器 (TS) 階段在兩個相位中運作：

-   第一相位處理鑲嵌因數、修正進位問題、處理非常小因數、降低並結合因數，使用 32 位元浮點算術。
-   第二個相位根據所選分割的類型產生點或拓撲清單。 這是曲面細分器階段的核心工作，並使用 16 位元分數搭配固定點算術。 固定點算術允許硬體加速，同時維持可接受的精確度。 例如，假設有 64 公尺寬塊面，這個精確度可以 2 公釐解析度放置點。

    | 分割類型 | 範圍                       |
    |----------------------|-----------------------------|
    | Fractional\_odd      | \[1...63\]                  |
    | Fractional\_even     | TessFactor 範圍：\[2..64\] |
    | 整數              | TessFactor 範圍：\[1..64\] |
    | Pow2                 | TessFactor 範圍：\[1..64\] |

     

由兩個可程式化著色器階段實作鑲嵌：[輪廓著色器](hull-shader-stage--hs-.md)和[網域著色器](domain-shader-stage--ds-.md)。 這些著色器階段是用 HLSL 程式碼 (定義在著色器模型 5 中) 程式化。 著色器目標是：hs\_5\_0 和 ds\_5\_0。 標題建立著色器，然後從已編譯著色器 (當著色器繫結至管線時傳遞至執行階段) 擷取程式碼給硬體。

### <a name="span-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanenablingdisabling-tessellation"></a><span id="Enabling_disabling_tessellation"></span><span id="enabling_disabling_tessellation"></span><span id="ENABLING_DISABLING_TESSELLATION"></span>啟用/停用鑲嵌

藉由建立輪廓著色器並將它繫結至輪廓著色器階段 (這會自動設定曲面細分器階段)，來啟用鑲嵌。 若要從鑲嵌塊面產生最終頂點位置，您也需要建立[網域著色器](domain-shader-stage--ds-.md)並將其繫結至網域著色器階段。 一旦鑲嵌啟用後，輸入到輸入組合語言 (IA) 階段的資料必須是塊面資料。 輸入組合拓撲必須是塊面常數拓撲。

若要停用鑲嵌，將輪廓著色器和網域著色器設定為 **NULL**。 [幾何著色器 (GS) 階段](geometry-shader-stage--gs-.md)和[資料流輸出 (SO) 階段](stream-output-stage--so-.md)都無法讀取輪廓著色器輸出控制點或塊面資料。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>輸入


使用從輪廓著色器階段傳入的鑲嵌因數 (這指定網域被鑲嵌的細微程度) 和分割類型 (指定用於切割塊面的演算法)，曲面細分器每個塊面運作一次。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>輸出


曲面細分器輸出 uv (和選擇性 w) 座標及表面拓撲至網域著色器階段。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[圖形管線](graphics-pipeline.md)

 

 




