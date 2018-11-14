---
title: BC7 格式
description: BC7 格式是用於高品質 RGB 和 RGBA 資料壓縮的紋理壓縮格式。
ms.assetid: 788B6E8C-9A1F-45F9-BE49-742285E8D8A6
keywords:
- BC7 格式
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 70380dd0bd07cfe0c81e8339f8606029663b47d4
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6469010"
---
# <a name="bc7-format"></a>BC7 格式


BC7 格式是用於高品質 RGB 和 RGBA 資料壓縮的紋理壓縮格式。

如需 BC7 格式的區塊模式資訊，請參閱 [BC7 格式模式參考](https://msdn.microsoft.com/library/windows/desktop/hh308954) (英文)。

## <a name="span-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanabout-bc7dxgiformatbc7"></a><span id="About-BC7-DXGI-FORMAT-BC7"></span><span id="about-bc7-dxgi-format-bc7"></span><span id="ABOUT-BC7-DXGI-FORMAT-BC7"></span>關於 BC7/DXGI\_FORMAT\_BC7


BC7 是由下列 DXGI\_FORMAT 列舉值來指定：

-   **DXGI\_FORMAT\_BC7\_TYPELESS**。
-   **DXGI\_FORMAT\_BC7\_UNORM**。
-   **DXGI\_FORMAT\_BC7\_UNORM\_SRGB**。

BC7 格式可用於 [Texture2D](https://msdn.microsoft.com/library/windows/desktop/bb205277) (包括陣列)、Texture3D，或 TextureCube (包括陣列) 紋理資源。 同樣地，此格式適用於任何與這些資源建立關聯的 Mipmap 表面。

BC7 使用固定的 16 位元組 (128 位元) 區塊大小，以及固定之 4x4 材質的磚大小。 與之前的 BC 格式一樣，大於支援之磚大小 (4x4) 的紋理圖片，會使用多個區塊進行壓縮。 此位址身分識別也適用於 3D 圖片和 Mipmap、立方體貼圖，以及紋理陣列。 所有影像磚都必須格式相同。

BC7 壓縮三通道 (RGB) 和四通道 (RGBA) 定點資料影像。 一般而言，來源資料的每個色彩元件 (通道) 為 8 位元，但此格式可將來源資料的每個色彩元件編碼為較高位元。 所有影像磚都必須格式相同。

BC7 解碼器在套用紋理篩選之前，會先執行解壓縮。

BC7 解壓縮硬體必須為位元精確。也就是硬體傳回的結果，必須和本文中所述解碼器傳回的結果相同。

## <a name="span-idbc7-implementationspanspan-idbc7-implementationspanspan-idbc7-implementationspanbc7-implementation"></a><span id="BC7-Implementation"></span><span id="bc7-implementation"></span><span id="BC7-IMPLEMENTATION"></span>BC7 實作


BC7 實作可以指定 8 種模式中的一種，並具有在 16 位元組 (128 位元) 區塊之最低有效位元中指定的模式。 該模式的編碼，是由 0 或更多具有 0 值的位元，最後再加上一個 1 而成。

BC7 區塊可能包含多個端點配對。 對應至端點配對的索引集可稱為一個「子集」。 此外，在某些區塊模式中，端點表示法被編碼為一種稱為 "RBGP" 的格式，其中 "P" 位元代表端點色彩元件的共用最低有效位元。 例如，如果格式的端點表示法為 "RGB 5.5.5.1"，則端點會解譯為 RGB 6.6.6 值，其中 P 位元的狀態定義了每個元件的最低有效位元。 同樣地，對於具備 Alpha 色板的來源資料，如果表示法的格式為 "RGBAP 5.5.5.5.1"，則端點會被解譯為 RGBA 6.6.6.6。 依區塊模式而定，您能為子集的兩個端點 (每一子集 2 個 P 位元) 個別指定共用最低有效位元，或讓子集的兩個端點 (每一子集 1 個 P 位元) 共用。

對於沒有明確編碼 Alpha 元件的 BC7 區塊，BC7 區塊包含模式位元、分割位元、壓縮端點、壓縮索引，和一個選擇性的 P 位元。 在這些區塊中，端點具有僅限 RGB 的表示法，且 Alpha 元件為來源資料中的所有材質都解碼為 1.0。

對於具有合併色彩和 Alpha 元件的 BC7 區塊，一個區塊包含模式位元、壓縮端點、壓縮索引，以及選用的分割位元和一個 P 位元。 在這些區塊中，端點色彩以 RGBA 格式表示，而且會插入 Alpha 元件值和色彩元件值。

對於具有不同色彩與 Alpha 元件的 BC7 區塊，一個區塊包含模式位元、旋轉位元、壓縮端點、壓縮索引，和一個選擇性的索引選取器位元。 這些區塊具備有效 RGB 向量 \[R、G、B\] 和一個分別編碼的純量 Alpha 色板 \[A\]。

下表列出每個區塊類型的元件。

| BC7 區塊包含...     | 模式位元 | 旋轉位元 | 索引選取器位元 | 分割位元 | 壓縮端點 | P 位元    | 壓縮索引 |
|---------------------------|-----------|---------------|--------------------|----------------|----------------------|----------|--------------------|
| 僅限色彩元件     | 必要  | N/A           | N/A                | 必要       | 必要             | 選用 | 必要           |
| 色彩 + Alpha 合併    | 必要  | N/A           | N/A                | 選用       | 必要             | 選用 | 必要           |
| 色彩和 Alpha 分隔 | 必要  | 必要      | 選用           | N/A            | 必要             | N/A      | 必要           |

 

BC7 在兩個端點之間的大約行定義調色盤。 模式值判斷每個區塊的端點配對插入數量。 BC7 在每個材質上儲存一個調色盤索引。

對於每個對應至一組端點的索引子集，編碼器修正了該子集壓縮索引資料中一個位元的狀態。 它藉由選擇一個端點順序來完成此項工作，該端點順序可讓指定的「修正」索引之索引將其最高有效位元設為 0，之後便能捨棄該位元，為每個子集節省一個位元。 對於僅具有一個單一子集的區塊模式，修正索引一律為索引 0。

## <a name="span-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspandecoding-the-bc7-format"></a><span id="Decoding-the-BC7-Format"></span><span id="decoding-the-bc7-format"></span><span id="DECODING-THE-BC7-FORMAT"></span>解碼 BC7 格式


下列虛擬程式碼概述了於 16 位元組 BC7 區塊中，解壓縮位於 (x,y) 之像素的步驟。

``` syntax
decompress_bc7(x, y, block)
{
    mode = extract_mode(block);
    
    //decode partition data from explicit partition bits
    subset_index = 0;
    num_subsets = 1;
    
    if (mode.type == 0 OR == 1 OR == 2 OR == 3 OR == 7)
    {
        num_subsets = get_num_subsets(mode.type);
        partition_set_id = extract_partition_set_id(mode, block);
        subset_index = get_partition_index(num_subsets, partition_set_id, x, y);
    }
    
    //extract raw, compressed endpoint bits
    UINT8 endpoint_array[num_subsets][4] = extract_endpoints(mode, block);
    
    //decode endpoint color and alpha for each subset
    fully_decode_endpoints(endpoint_array, mode, block);
    
    //endpoints are now complete.
    UINT8 endpoint_start[4] = endpoint_array[2 * subset_index];
    UINT8 endpoint_end[4]   = endpoint_array[2 * subset_index + 1];
        
    //Determine the palette index for this pixel
    alpha_index     = get_alpha_index(block, mode, x, y);
    alpha_bitcount  = get_alpha_bitcount(block, mode);
    color_index     = get_color_index(block, mode, x, y);
    color_bitcount  = get_color_bitcount(block, mode);

    //determine output
    UINT8 output[4];
    output.rgb = interpolate(endpoint_start.rgb, endpoint_end.rgb, color_index, color_bitcount);
    output.a   = interpolate(endpoint_start.a,   endpoint_end.a,   alpha_index, alpha_bitcount);
    
    if (mode.type == 4 OR == 5)
    {
        //Decode the 2 color rotation bits as follows:
        // 00 – Block format is Scalar(A) Vector(RGB) - no swapping
        // 01 – Block format is Scalar(R) Vector(AGB) - swap A and R
        // 10 – Block format is Scalar(G) Vector(RAB) - swap A and G
        // 11 - Block format is Scalar(B) Vector(RGA) - swap A and B
        rotation = extract_rot_bits(mode, block);
        output = swap_channels(output, rotation);
    }
    
}
```

下列虛擬程式碼概述了於 16 位元組 BC7 區塊中，為每個子集完整解碼端點色彩和 Alpha 元件的步驟。

``` syntax
fully_decode_endpoints(endpoint_array, mode, block)
{
    //first handle modes that have P-bits
    if (mode.type == 0 OR == 1 OR == 3 OR == 6 OR == 7)
    {
        for each endpoint i
        {
            //component-wise left-shift
            endpoint_array[i].rgba = endpoint_array[i].rgba << 1;
        }
        
        //if P-bit is shared
        if (mode.type == 1) 
        {
            pbit_zero = extract_pbit_zero(mode, block);
            pbit_one = extract_pbit_one(mode, block);
            
            //rgb component-wise insert pbits
            endpoint_array[0].rgb |= pbit_zero;
            endpoint_array[1].rgb |= pbit_zero;
            endpoint_array[2].rgb |= pbit_one;
            endpoint_array[3].rgb |= pbit_one;  
        }
        else //unique P-bit per endpoint
        {  
            pbit_array = extract_pbit_array(mode, block);
            for each endpoint i
            {
                endpoint_array[i].rgba |= pbit_array[i];
            }
        }
    }

    for each endpoint i
    {
        // Color_component_precision & alpha_component_precision includes pbit
        // left shift endpoint components so that their MSB lies in bit 7
        endpoint_array[i].rgb = endpoint_array[i].rgb << (8 - color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a << (8 - alpha_component_precision(mode));

        // Replicate each component's MSB into the LSBs revealed by the left-shift operation above
        endpoint_array[i].rgb = endpoint_array[i].rgb | (endpoint_array[i].rgb >> color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a | (endpoint_array[i].a >> alpha_component_precision(mode));
    }
        
    //If this mode does not explicitly define the alpha component
    //set alpha equal to 1.0
    if (mode.type == 0 OR == 1 OR == 2 OR == 3)
    {
        for each endpoint i
        {
            endpoint_array[i].a = 255; //i.e. alpha = 1.0f
        }
    }
}
```

若要為每個子集產生每個插入的元件，請使用下列演算法︰讓 "c" 成為產生的元件、讓 "e0" 成為該子集端點 0 的元件，並讓 "e1" 成為該子集端點 1 的元件。

``` syntax
UINT16 aWeight2[] = {0, 21, 43, 64};
UINT16 aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
UINT16 aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

UINT8 interpolate(UINT8 e0, UINT8 e1, UINT8 index, UINT8 indexprecision)
{
    if(indexprecision == 2)
        return (UINT8) (((64 - aWeights2[index])*UINT16(e0) + aWeights2[index]*UINT16(e1) + 32) >> 6);
    else if(indexprecision == 3)
        return (UINT8) (((64 - aWeights3[index])*UINT16(e0) + aWeights3[index]*UINT16(e1) + 32) >> 6);
    else // indexprecision == 4
        return (UINT8) (((64 - aWeights4[index])*UINT16(e0) + aWeights4[index]*UINT16(e1) + 32) >> 6);
}
```

下列虛擬程式碼示範如何為色彩和 Alpha 元件擷取索引及位元計數的方式。 使用不同色彩和 Alpha 的區塊也有兩組索引資料︰一個用於向量通道，一個用於純量通道。 對於模式 4，這些索引有不同寬度 (2 或 3 位元)，而且有一個 1 位元選取器來指定向量或純量資料是否使用 3 位元索引。 (解壓縮 Alpha 位元計數類似於擷取色彩位元計數，但是具有以 **idxMode** 位元為基礎的反向行為。)

``` syntax
bitcount get_color_bitcount(block, mode)
{
    if (mode.type == 0 OR == 1)
        return 3;
    
    if (mode.type == 2 OR == 3 OR == 5 OR == 7)
        return 2;
    
    if (mode.type == 6)
        return 4;
        
    //The only remaining case is Mode 4 with 1-bit index selector
    idxMode = extract_idxMode(block);
    if (idxMode == 0)
        return 2;
    else
        return 3;
}
```

## <a name="span-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanbc7-format-mode-reference"></a><span id="BC7-format-mode-reference"></span><span id="bc7-format-mode-reference"></span><span id="BC7-FORMAT-MODE-REFERENCE"></span>BC7 格式模式參考


本節中包含了一份 8 個區塊模式和 BC7 紋理壓縮格式區塊之位元配置的清單。

區塊中每個子集的色彩會以兩個明確端點色彩以及其之間插入的色彩組呈現。 依區塊索引精確度而定，每個子集能具備 4 個、8 或 16 個可用的色彩。

### <a name="span-idmode-0spanspan-idmode-0spanspan-idmode-0spanmode-0"></a><span id="Mode-0"></span><span id="mode-0"></span><span id="MODE-0"></span>模式 0

BC7 模式 0 的特性如下︰

-   僅限色彩元件 (無 Alpha)
-   每個區塊 3 個子集
-   每個端點都有一個唯一 P 位元的 RGBP 4.4.4.1 端點
-   3 位元索引
-   16 個分割

![模式 0 位元配置](images/bc7-mode0.png)

### <a name="span-idmode-1spanspan-idmode-1spanspan-idmode-1spanmode-1"></a><span id="Mode-1"></span><span id="mode-1"></span><span id="MODE-1"></span>模式 1

BC7 模式 1 的特性如下︰

-   僅限色彩元件 (無 Alpha)
-   每個區塊 2 個子集
-   每個子集具有共用 P 位元的 RGBP 6.6.6.1 端點)
-   3 位元索引
-   64 個分割

![模式 1 位元配置](images/bc7-mode1.png)

### <a name="span-idmode-2spanspan-idmode-2spanspan-idmode-2spanmode-2"></a><span id="Mode-2"></span><span id="mode-2"></span><span id="MODE-2"></span>模式 2

BC7 模式 2 的特性如下︰

-   僅限色彩元件 (無 Alpha)
-   每個區塊 3 個子集
-   RGB 5.5.5 端點
-   2 位元索引
-   64 個分割

![模式 2 位元配置](images/bc7-mode2.png)

### <a name="span-idmode-3spanspan-idmode-3spanspan-idmode-3spanmode-3"></a><span id="Mode-3"></span><span id="mode-3"></span><span id="MODE-3"></span>模式 3

BC7 模式 3 的特性如下︰

-   僅限色彩元件 (無 Alpha)
-   每個區塊 2 個子集
-   每個子集具有唯一 P 位元的 RGBP 7.7.7.1 端點)
-   2 位元索引
-   64 個分割

![模式 3 位元配置](images/bc7-mode3.png)

### <a name="span-idmode-4spanspan-idmode-4spanspan-idmode-4spanmode-4"></a><span id="Mode-4"></span><span id="mode-4"></span><span id="MODE-4"></span>模式 4

BC7 模式 4 的特性如下︰

-   具有獨立 Alpha 元件的色彩元件
-   每個區塊 1 個子集
-   RGB 5.5.5 色彩端點
-   6 位元 Alpha 端點
-   16 x 2 位元索引
-   16 x 3 位元索引
-   2 位元元件旋轉
-   1 位元索引選取器 (不論是否使用 2 或 3 位元索引)

![模式 4 位元配置](images/bc7-mode4.png)

### <a name="span-idmode-5spanspan-idmode-5spanspan-idmode-5spanmode-5"></a><span id="Mode-5"></span><span id="mode-5"></span><span id="MODE-5"></span>模式 5

BC7 模式 5 的特性如下︰

-   具有獨立 Alpha 元件的色彩元件
-   每個區塊 1 個子集
-   RGB 7.7.7 色彩端點
-   6 位元 Alpha 端點
-   16 x 2 位元色彩索引
-   16 x 2 位元 Alpha 索引
-   2 位元元件旋轉

![模式 5 位元配置](images/bc7-mode5.png)

### <a name="span-idmode-6spanspan-idmode-6spanspan-idmode-6spanmode-6"></a><span id="Mode-6"></span><span id="mode-6"></span><span id="MODE-6"></span>模式 6

BC7 模式 6 的特性如下︰

-   合併的色彩和 Alpha 元件
-   每個區塊一個子集
-   RGBAP 7.7.7.7.1 色彩 (和 Alpha) 端點 (每個端點都有唯一 P 位元)
-   16 x 4 位元索引

![模式 6 位元配置](images/bc7-mode6.png)

### <a name="span-idmode-7spanspan-idmode-7spanspan-idmode-7spanmode-7"></a><span id="Mode-7"></span><span id="mode-7"></span><span id="MODE-7"></span>模式 7

BC7 模式 7 的特性如下︰

-   合併的色彩和 Alpha 元件
-   每個區塊 2 個子集
-   RGBAP 5.5.5.5.1 色彩 (和 Alpha) 端點 (每個端點都有唯一 P 位元)
-   2 位元索引
-   64 個分割

![模式 7 位元配置](images/bc7-mode7.png)

### <a name="span-idremarksspanspan-idremarksspanspan-idremarksspanremarks"></a><span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>備註

模式 8 (最小顯著性位元組設定為 0x00) 已保留。 請勿在您的編碼器中使用它。 如果您將此模式傳遞至硬體，就會傳回初始化至所有零的區塊。

在 BC7 中，您能以下列其中一種方式編碼 Alpha 元件：

-   不具備明確 Alpha 元件編碼的區塊類型。 在這些區塊中，色彩端點有一個僅限 RGB 的編碼，還有對所有材質解碼至 1.0 的 Alpha 元件。
-   具有合併色彩與 Alpha 元件的區塊類型。 在這些區塊中，端點色彩值會以 RGBA 格式指定，而且會插入 Alpha 元件值和色彩值。
-   具有獨立色彩與 Alpha 元件的區塊類型。 在這些區塊中，色彩和 Alpha 值是個別指定的，兩方都有獨立的索引集。 因此，它們的有效向量和純量通道是獨立編碼的。在這之中，向量通常指定色彩通道 \[R、G、B\]，而純量指定 Alpha 色板 \[A\]。 為了支援這種方式，編碼中提供了一個獨立的 2 位元欄位，允許將不同的通道編碼指定為純量值。 如此一來，區塊就可以具有此 Alpha 編碼 (同 2 元欄位所指示) 下列四個不同表示法中的其中一項︰
    -   RGB|A：獨立 Alpha 色板
    -   AGB|R：獨立「紅色」色彩通道
    -   RAB|G：獨立「綠色」色彩通道
    -   RGA|B：獨立「藍色」色彩通道

    解碼器在解碼之後會將通道順序重新排列回 RGBA，因此開發人員看不到內部區塊格式。 具有獨立色彩和 Alpha 元件的黑色也有兩組索引資料︰一個用於通道的向量集，一個用於純量通道。 (在模式 4 的例子中，這些索引具有不同寬度 \[2 或 3 位元\]。 模式 4 也包含 1 位元選取器，可指定向量或純量通道是否使用 3 位元索引。)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[紋理區塊壓縮](texture-block-compression.md)

 

 




