---
title: 紋理區塊壓縮
description: 紋理的區塊壓縮 (BC) 支援在 Direct3D 11 中已進行擴充，內含 BC6H 和 BC7 演算法。
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- 紋理區塊壓縮
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dec33768eff90b9bd35a3ea60f3158fce663345e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640183"
---
# <a name="texture-block-compression"></a>紋理區塊壓縮


紋理的區塊壓縮 (BC) 支援在 Direct3D 11 中已進行擴充，內含 BC6H 和 BC7 演算法。 BC6H 支援高動態範圍色彩來源資料，而 BC7 使用較少的標準 RGB 來源資料成品，提供高於平均品質的壓縮。

如需關於 Direct3D 11 之前區塊壓縮演算法支援 (包括 BC1 到 BC5 格式的支援) 的相關詳細資訊，請參閱[區塊壓縮 (Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb694531)。

**檔案格式的注意事項：  **BC6H 和 BC7 材質壓縮格式來儲存壓縮的紋理資料使用 DDS 檔案格式。 如需詳細資訊，請參閱[DDS 程式設計指南](https://msdn.microsoft.com/library/windows/desktop/bb943991)。

## <a name="span-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>區塊壓縮格式支援 Direct3D 11


| 來源資料                                  | 資料壓縮解析度的最低需求                              | 建議格式 | 最低支援的功能層級 |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| 帶有 Alpha 色板的三通道色彩       | 帶有 0 或 1 個位元 Alpha 的三色彩通道 (5 個位元：6 個位元：5 個位元)  | BC1                | Direct3D 9.1                    |
| 帶有 Alpha 色板的三通道色彩       | 帶有 4 個位元 Alpha 的三色彩通道 (5 個位元：6 個位元：5 個位元)         | BC2                | Direct3D 9.1                    |
| 帶有 Alpha 色板的三通道色彩       | 帶有 8 個位元 Alpha 的三色彩通道 (5 個位元：6 個位元：5 個位元)          | BC3                | Direct3D 9.1                    |
| 單一通道色彩                            | 單一色彩通道 (8 個位元)                                                | BC4                | Direct3D 10                     |
| 雙通道色彩                            | 兩個色彩通道 (8 個位元：8 個位元)                                        | BC5                | Direct3D 10                     |
| 三通道高動態範圍 (HDR) 色彩 | 三個色彩"一半 「 浮點數中的通道 （16 位元： 16 位元： 16 位元）\* | BC6H               | Direct3D 11                     |
| 三通道色彩，及選擇性的 Alpha 色板  | 帶有 0 到 8 個位元 Alpha 的三色彩通道 (每個通道 4 到 7 個位元)  | BC7                | Direct3D 11                     |

 

\*「 半 「 浮點數是 16 位元值，包含選用正負號位元，5 位元偏差指數，並為 10 或 11 位元的尾數。
## <a name="span-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>BC1、 BC2 和 B3 格式


BC1、BC2，以及 BC3 格式對等於 Direct3D 9 DXTn 紋理壓縮格式，並且各自對應到 Direct3D 10 BC1、BC2，及 BC3 格式。 支援下列三種格式所需的所有功能層級 (D3D\_功能\_層級\_9\_1，D3D\_功能\_層級\_9\_2，D3D\_功能\_層級\_9\_3，D3D\_功能\_層級\_10\_0，D3D\_功能\_層級\_10\_1 和 D3D\_功能\_層級\_11\_0)。

| 區塊壓縮格式 | DXGI 格式                                                                           | Direct3D 9 對等格式                               | 每個 4 x 4 像素區塊的位元組 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BC1                      | DXGI\_格式\_BC1\_UNORM、 DXGI\_格式\_BC1\_UNORM\_SRGB、 DXGI\_格式\_BC1\_TYPELESS | D3DFMT\_DXT1, FourCC="DXT1"                                | 8                         |
| BC2                      | DXGI\_FORMAT\_BC2\_UNORM, DXGI\_FORMAT\_BC2\_UNORM\_SRGB, DXGI\_FORMAT\_BC2\_TYPELESS | D3DFMT\_DXT2\*, FourCC="DXT2", D3DFMT\_DXT3, FourCC="DXT3" | 16                        |
| BC3                      | DXGI\_格式\_BC3\_UNORM、 DXGI\_格式\_BC3\_UNORM\_SRGB、 DXGI\_格式\_BC3\_TYPELESS | D3DFMT\_DXT4\*, FourCC="DXT4", D3DFMT\_DXT5, FourCC="DXT5" | 16                        |

 

\*這些壓縮配置 （DXT2 和 DXT4） 讓 Direct3D 9 預乘 alpha 格式與標準的 alpha 格式之間沒有差別。 此區別必須透過可程式化著色器於轉譯時間中進行處理。

## <a name="span-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>BC4 和 BC5 格式


| 區塊壓縮格式 | DXGI 格式                                                                     | Direct3D 9 對等格式 | 每個 4 x 4 像素區塊的位元組 |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC4                      | DXGI\_FORMAT\_BC4\_UNORM, DXGI\_FORMAT\_BC4\_SNORM, DXGI\_FORMAT\_BC4\_TYPELESS | FourCC="ATI1"                | 8                         |
| BC5                      | DXGI\_FORMAT\_BC5\_UNORM, DXGI\_FORMAT\_BC5\_SNORM, DXGI\_FORMAT\_BC5\_TYPELESS | FourCC="ATI2"                | 16                        |

 

## <a name="span-idbc6hformatspanspan-idbc6hformatspanspan-idbc6hformatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>BC6H 格式


如需關於此格式的詳細資訊，請參閱[BC6H 格式](https://msdn.microsoft.com/library/windows/desktop/hh308952)文件。

| 區塊壓縮格式 | DXGI 格式                                                                      | Direct3D 9 對等格式 | 每個 4 x 4 像素區塊的位元組 |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | DXGI\_FORMAT\_BC6H\_UF16, DXGI\_FORMAT\_BC6H\_SF16, DXGI\_FORMAT\_BC6H\_TYPELESS | 無                          | 16                        |

 

BC6H 格式針對每個 4 x 4 的像素區塊可以選擇不同的編碼模式。 共計有 14 個不同的編碼模式可以使用，每一種模式對於顯示紋理的最終視覺品質都有些許不同的取捨。 根據來源內容選擇或配合的品質等級，模式的選擇可允許硬體進行更快速的解碼，然而這也會大幅增加搜尋空間的複雜度。

## <a name="span-idbc7formatspanspan-idbc7formatspanspan-idbc7formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>BC7 格式


如需關於此格式的詳細資訊，請參閱[BC7 格式](https://msdn.microsoft.com/library/windows/desktop/hh308953)文件。

| 區塊壓縮格式 | DXGI 格式                                                                           | Direct3D 9 對等格式 | 每個 4 x 4 像素區塊的位元組 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC7                      | DXGI\_FORMAT\_BC7\_UNORM, DXGI\_FORMAT\_BC7\_UNORM\_SRGB, DXGI\_FORMAT\_BC7\_TYPELESS | 無                          | 16                        |

 

BC7 格式針對每個 4 x 4 的像素區塊可以選擇不同的編碼模式。 共計有 8 個不同的編碼模式可以使用，每一種模式對於顯示紋理的最終視覺品質都有些許不同的取捨。 根據來源內容選擇或配合的品質等級，模式的選擇可允許硬體進行更快速的解碼，然而這也會大幅增加搜尋空間的複雜度。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[Appendices](appendix.md)

[紋理](https://msdn.microsoft.com/library/windows/desktop/ff476902)

 

 




