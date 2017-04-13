---
title: "紋理簡介"
description: "紋理資源是儲存紋素的資料結構，這是可讀取或寫入的最小紋理單位。 著色器讀取紋理時，可以由紋理樣本篩選。"
ms.assetid: 6F3C76A8-F762-4296-AE02-BFBD6476A5A8
keywords: "紋理簡介"
author: mtoepke
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: d642668a9af1e62f232e13e411e51e6d850de7f5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="introduction-to-textures"></a>紋理簡介


紋理資源是儲存紋素的資料結構，這是可讀取或寫入的最小紋理單位。 著色器讀取紋理時，可以由紋理樣本篩選。

紋理資源是設計來儲存紋素的結構化資料集合。 紋素代表管線可讀取或寫入的最小紋理單位。 和緩衝區不同的是，紋理可依紋理樣本篩選，由著色器單位讀取。 紋理類型影響篩選紋理的方式。 每個紋素包含 1 到 4 個元件，以 DXGI\_FORMAT 列舉所定義的其中一種 DXGI 格式排列。

建立具已知大小的紋理為結構化資源。 不過，每個紋理在建立資源之後可能有型別或無型別，只要當紋理與管線繫結時使用檢視完全指定型別。

## <a name="span-idtexturetypesspanspan-idtexturetypesspanspan-idtexturetypesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span>紋理類型


Direct3D 支援數個浮點表示。 所有浮點數計算運作都在 IEEE 754 32 位元單精確度浮點數規則的定義子集下運作。

有數種紋理類型︰ 1D、2D、3D，每一個都可使用或不使用 Mipmap 建立。 Direct3D 也支援紋理陣列和多重取樣的紋理。

-   [1D 紋理](#texture1d-resource)
-   [1D 紋理陣列](#texture1d-array-resource)
-   [2D 紋理和 2D 紋理陣列](#texture2d-resource)
-   [3D 紋理](#texture3d-resource)

### <a name="span-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1d-resourcespan1d-textures"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>1D 紋理

最簡單形式的 1D 紋理所包含的紋理資料，可以使用單一紋理座標處理，它可以視覺化為紋素陣列，如下圖所示。

下圖顯示 1D 紋理︰

![1d 紋理](images/d3d10-1d-texture.png)

每個紋素根據所儲存的資料格式包含許多色彩元件。 增加更多複雜度，您可以使用 Mipmap 層次建立 1D 紋理，如下圖所示。

![具 Mipmap 層次的 1d 紋理](images/d3d10-resource-texture1d.png)

Mipmap 層次是比上層小二的 n 次方的紋理。 最上層包含大部分的細節，每個後續的層次較小。 對於 1D Mipmap，最小層級包含一個紋素。 此外，MIP 層級一律降至 1:1。

當專為特別尺寸紋理產生 Mipmap， 下一個較低層次一律為均勻大小 (除了最低層到達 1 以外)。 例如，圖表顯示 5x1 紋理，其下一個最低層為 2x1 紋理，其下一個 (和上一個) MIP 層是 1x1 大小的紋理。 由稱為 LOD (細節層次) 的索引識別的層次，會在呈現不是那麼靠近相機的幾何圖形時，用於存取較小的紋理。

### <a name="span-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1d-array-resourcespan1d-texture-arrays"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>1D 紋理陣列

Direct3D 也支援紋理陣列。 1D 紋理陣列概念上看起來像下圖。

![1d 紋理陣列](images/d3d10-resource-texture1darray.png)

這個紋理陣列包含三種紋理。 三個紋理的每一個都具有紋理寬度 5 (也就是第一層圖元數)。 每個紋理也包含 3 層 Mipmap。

在 Direct3D 中的所有紋理陣列都是同質紋理陣列，這表示紋理陣列中的每種紋理必須有相同資料格式和大小 (包括紋理寬度和 Mipmap 層次數目)。 只要每一個紋理陣列中的所有紋理大小都相符，您可以建立不同大小的紋理陣列。

### <a name="span-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2d-resourcespan2d-textures-and-2d-texture-arrays"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>2D 紋理和 2D 紋理陣列

Texture2D 資源包含 2D 紋格。 每個紋素是由 u v 向量定位。 因為是紋理資源，它可能包含 mipmap 層次和子資源。 完全填充的 2D 紋理資源看起來如下圖。

![2D 紋理資源](images/d3d10-resource-texture2d.png)

這個紋理資源包含具三個 Mipmap 層次的單一 3x5 紋理。

2D 紋理陣列資源是同質 2D 紋理陣列，也就是每個紋理都有相同的資料格式和尺寸 (包括 Mipmap 層次)。 它有類似 1D 紋理的版面配置，現在包含 2D 資料的紋理除外，如下圖所示。

![2d 紋理陣列](images/d3d10-resource-texture2darray.png)

這個紋理包含三個紋理，每個紋理為 3x5 含兩個 mipmap 層次。

### <a name="span-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanusing-a-2d-texture-array-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>使用 2D 紋理陣列做為立體紋理

立方紋理是是包含 6 個紋理的 2D 紋理陣列，每個立體表面一個紋理。 完全填充的立體紋理如下圖所示。

![代表立體紋理的 2d 紋理陣列](images/d3d10-resource-texturecube.png)

包含 6 個紋理的 2D 紋理陣列，在使用立體紋理視圖結合管線後，可使用立體內建功能從著色器內讀取。 使用從立體紋理中心指出的 3D 向量，從著色器處理紋理。

### <a name="span-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3d-resourcespan3d-textures"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>3D 紋理

3D 紋理資源 (也稱為容體紋理) 包含 3D 紋理元素量。 因為是紋理資源，所以可會包含 mipmap 層次。 完全填充的 3D 紋理如下圖所示。

![3d 紋理資源](images/d3d10-resource-texture3d.png)

3D 紋理 mipmap 切片繫結為描繪目標輸出 (描繪目標視圖) 時，3D 紋理行為等同於具 n 切片的 2D 紋理。 從幾何著色器階段選擇特定著色切片。

沒有 3D 紋理陣列概念，因此 3D 紋理子資源是單一 mipmap 層次。

針對像素和紋素定義 Direct3D 座標系統。

## <a name="span-idpixelspanspan-idpixelspanspan-idpixelspanpixel-coordinate-system"></a><span id="Pixel"></span><span id="pixel"></span><span id="PIXEL"></span>像素座標系統


下圖顯示 Direct3D 像素座標系統定義左上角的著色目標原點。 像素中心離整數位置偏移 (0.5f, 0.5f)。

![direct3d 10 中的像素座標系統圖表](images/d3d10-coordspix10.png)

## <a name="span-idtexelspanspan-idtexelspanspan-idtexelspantexel-coordinate-system"></a><span id="Texel"></span><span id="texel"></span><span id="TEXEL"></span>紋素座標系統


紋素座標系統在紋理左上角的原點，如下圖所示。 這樣可呈現螢幕對齊的一般紋理，如像素座標系統對齊紋素座標系統。

![紋理座標系統的圖表](images/d3d10-coordstex10.png)

紋理座標以標準化或按比例調整之數字表示。每個紋理座標對應特定紋素，如下所示︰

對於標準座標︰

-   點取樣︰紋素 \# = floor(U \* 寬度)
-   線性取樣︰左紋素 \ # = floor(U \ * 寬度)，右紋素 \ # = 左紋素 \ # + 1

對於按比例調整座標︰

-   點取樣︰紋素 \ # = floor(U)
-   線性取樣︰ 左紋素 \ # = floor(U - 0.5)，右紋素 \ # = 左紋素 \ # + 1

寬度是紋理的寬度 (以紋素為單位)。

計算紋素位置之後，就會紋理尋址環繞。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[紋理](textures.md)
