---
title: 區塊壓縮
description: 區塊壓縮是一種失真紋理壓縮技術，主要用於減少紋理的大小和磁碟使用量並提升效能。 區塊壓縮紋理可以比每個色彩 32 位元的紋理更小。
ms.assetid: 2FAD6BE8-C6E4-4112-AF97-419CD27F7C73
keywords:
- 區塊壓縮
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3f6a1277dbb2d756f0d3a4ffc1fd545f892a2096
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2019
ms.locfileid: "9047207"
---
# <a name="block-compression"></a>區塊壓縮

區塊壓縮是一種失真紋理壓縮技術，主要用於減少紋理的大小和磁碟使用量並提升效能。 區塊壓縮的紋理可以比每個色彩 32 位元的紋理更小。

區塊壓縮是一種用於降低紋理大小的紋理壓縮技術。 與每個色彩 32 位元的材質相比，區塊壓縮的紋理可省下高達 75% 的空間。 當使用區塊壓縮時，應用程式的效能通常會增加，這是因為磁碟使用量較少。

雖然區塊壓縮會造成失真，但在經過管線轉換並篩選的所有紋理上運作良好，而且建議在這些紋理上使用。 直接對應至螢幕的紋理 (例如圖示和文字等 UI 元素) 不是進行壓縮的好選擇，因為成品會更為醒目。

建立經區塊壓縮的紋理時，所有維度都必須是 4 的倍數，且無法用作管線的輸出。

## <a name="span-idbasicsspanspan-idbasicsspanspan-idbasicsspanhow-block-compression-works"></a><span id="Basics"></span><span id="basics"></span><span id="BASICS"></span>區塊壓縮的運作方式

區塊壓縮是一種用以減少儲存色彩資料所需記憶體的技術。 藉由將某些色彩儲存為其原始大小，並在其他色彩上使用編碼配置，您可以大幅減少儲存圖片所需的記憶體。 因為硬體會自動解碼壓縮過的資料，所以就不會有使用壓縮紋理而造成效能降低的情形。

若要查看壓縮的運作方式，請查看下列兩個範例。 第一個範例描述儲存未壓縮的資料時使用的記憶體量；第二個範例描述儲存壓縮資料時使用的記憶體量。

- [儲存未壓縮的資料](#storing-uncompressed-data)
- [儲存經壓縮的資料](#storing-compressed-data)

### <a name="span-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoring-uncompressed-dataspanstoring-uncompressed-data"></a><span id="Storing_Uncompressed_Data"></span><span id="storing_uncompressed_data"></span><span id="STORING_UNCOMPRESSED_DATA"></span><span id="storing-uncompressed-data"></span>儲存未壓縮的資料

下圖為一個未壓縮的 4 × 4 紋理。 假設每個色彩都包含單一色彩元件 (例如紅色)，並儲存在記憶體的一個位元組內。

![一個未壓縮的 4x4 紋理](images/d3d10-block-compress-1.png)

未壓縮的資料會在記憶體中循序配置，且需要 16 位元組的記憶體，如下所示。

![在循序記憶體中的未壓縮資料](images/d3d10-block-compress-2.png)

### <a name="span-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoring-compressed-dataspanstoring-compressed-data"></a><span id="Storing_Compressed_Data"></span><span id="storing_compressed_data"></span><span id="STORING_COMPRESSED_DATA"></span><span id="storing-compressed-data"></span>儲存經壓縮的資料

既然您已經看過未壓縮圖片會使用多少記憶體，現在就來看看壓縮圖片會節省多少記憶體。 [BC1](#bc1) 壓縮格式儲存 2 種色彩 (每種 1 位元組) 和 16 個 3 位元索引 (48 位元或 6 位元組)，用來插入材質內的原始色彩，如下列圖例所示。

![bc1 壓縮格式](images/d3d10-block-compress-3.png)

儲存壓縮資料所需的總空間為 8 位元組，和未壓縮的範例相比省下 50% 的記憶體空間。 使用多個色彩元件時，會省下更大的空間。

區塊壓縮省下的大量記憶體，可促使效能增加。 此效能增長是降低圖片品質而得 (基於色彩內插補點)。不過，品質的下降通常不明顯。

下一章節會說明 Direct3D 如何讓應用程式使用封鎖壓縮。

## <a name="span-idusingblockcompressionspanspan-idusingblockcompressionspanspan-idusingblockcompressionspanusing-block-compression"></a><span id="Using_Block_Compression"></span><span id="using_block_compression"></span><span id="USING_BLOCK_COMPRESSION"></span>使用區塊壓縮

建立區塊壓縮紋理就像建立未壓縮紋理一樣，不過您必須指定區塊壓縮的格式。

接下來，請建立一個檢視來將紋理繫結至管線，這是因為區塊壓縮紋理只能作為著色器階段的輸入，所以您會想要建立一個著色器資源檢視。

依您使用未壓縮紋理的相同方式來使用區塊壓縮紋理。 如果您的應用程式會取得記憶體指標來顯示區塊壓縮資料，你需要處理造成宣告大小和實際大小不同的 Mipmap 內記憶體填補。

- [虛擬大小與實體大小](#virtual-size-versus-physical-size)

### <a name="span-idvirtualsizespanspan-idvirtualsizespanspan-idvirtualsizespanspan-idvirtual-size-versus-physical-sizespanvirtual-size-versus-physical-size"></a><span id="Virtual_Size"></span><span id="virtual_size"></span><span id="VIRTUAL_SIZE"></span><span id="virtual-size-versus-physical-size"></span>虛擬大小與實體大小

如果您應用程式中的程式碼，使用記憶體指標來引導區塊壓縮紋理記憶體的話，有一個重要考量要注意，這可能會需要修改您應用程式中的程式碼。 區塊壓縮紋理必須在所有維度中都是 4 的倍數，因為區塊壓縮演算法是在 4x4 的材質區塊上執行。 這對原始維度能被 4 整除，但分出的子層級卻不能的 Mipmap 會出現問題。 下列圖表顯示每個 Mipmap 層級的虛擬 (宣告) 大小和實體 (實際) 大小差異。

![未壓縮和壓縮 Mipmap 層級](images/d3d10-block-compress-pad.png)

圖表左側顯示專為未壓縮 60 × 40 紋理而產生的 Mipmap 層級大小。 最上層大小是從產生該紋理的 API 呼叫而來；每個後續層級的大小為先前層級的一半。 對未壓縮紋理而言，虛擬 (宣告) 大小和實體 (實際) 大小之間並無不同。

圖表右側顯示專為使用壓縮之相同 60 × 40 紋理而產生的 Mipmap 層級大小。 請注意第二個和第三個層級都有記憶體填補，讓每個層級的大小都為 4 的因數。 這是必要的，好讓演算法可以在 4×4 材質區塊上執行。 如果您考慮讓 Mipmap 層級小於 4×4，這會特別明顯。在配置紋理記憶體時，這些非常小的 Mipmap 層級大小將會進位成最接近之 4 的因數。

取樣硬體使用虛擬大小。在取樣紋理時，會忽略記憶體填補。 對小於 4x4 的 Mipmap 層級，僅前四個材質會用於 2×2 貼圖，且只有第一個材質會由一個 1×1 區塊使用。 不過，沒有 API 結構會公開實體大小 (包括記憶體填補)。

總結來說，複製包含區塊壓縮資料的區域時，請注意使用對齊的記憶體區塊。 若要在可取得記憶體指標的應用程式中執行此動作，請確定指標使用表面字幅來處理實體記憶體大小。

## <a name="span-idcompressionalgorithmsspanspan-idcompressionalgorithmsspanspan-idcompressionalgorithmsspancompression-algorithms"></a><span id="Compression_Algorithms"></span><span id="compression_algorithms"></span><span id="COMPRESSION_ALGORITHMS"></span>壓縮演算法

在 Direct3D 內的區塊壓縮技術，會將未壓縮紋理資料切割為 4×4 的區塊、壓縮每個區塊，並儲存該資料。 基於這個原因，需要壓縮之材質的紋理維度必須要為 4 的倍數。

![區塊壓縮](images/d3d10-compression-1.png)

上述圖表顯示出一個分割成材質區塊的紋理。 第一個區塊會顯示標記為 a 至 p 的 16 種材質配置，但每個區塊都具有相同的資料組織。

Direct3D 實作多種壓縮配置，其個別實作以下三種不同折衷之處的其中一項：元件的儲存數、每個元件的位元，以及使用的記憶體數量。 使用此表格，以協助您選擇最適合所用資料類型的格式，以及最符合您應用程式的資料解析度。

| 來源資料                     | 資料壓縮解析度 (依位元計算) | 選擇此壓縮格式 |
|---------------------------------|---------------------------------------|--------------------------------|
| 三元件色彩和 Alpha | 色彩 (5:6:5)、Alpha (1) 或沒有 Alpha  | [BC1](#bc1)                    |
| 三元件色彩和 Alpha | 色彩 (5:6:5)、Alpha (4)              | [BC2](#bc2)                    |
| 三元件色彩和 Alpha | 色彩 (5:6:5)、Alpha (8)              | [BC3](#bc3)                    |
| 單元件色彩             | 單元件 (8)                     | [BC4](#bc4)                    |
| 雙元件色彩             | 雙元件 (8:8)                  | [BC5](#bc5)                    |

- [BC1](#bc1)
- [BC2](#bc2)
- [BC3](#bc3)
- [BC4](#bc4)
- [BC5](#bc5)

### <a name="span-idbc1spanspan-idbc1spanbc1"></a><span id="BC1"></span><span id="bc1"></span>BC1

使用第一個區塊壓縮格式 (BC1) (可以是 DXGI\_FORMAT\_BC1\_TYPELESS、DXGI\_FORMAT\_BC1\_UNORM，或是 DXGI\_BC1\_UNORM\_SRGB) 來依 5:6:5 色彩 (5 位元紅色、6 位元綠色、5 位元藍色) 儲存三元件色彩資料。 即使您的資料也包含 1 位元 Alpha 也是如此。 假設 4×4 紋理使用所能使用的最大資料格式，BC1 格式會將所需的記憶體從 48 位元組 (16 色 × 3 元件/色彩 × 1 位元組/元件) 降低至 8 位元組的記憶體。

此演算法適用於 4×4 材質區塊上。 此演算法並不儲存 16 種顏色，而是儲存 2 個參考色彩 (color\_0 和 color\_1)，以及 16 種 2 位元色彩索引 (a 至 p 區塊)，如下圖顯示。

![BC1 壓縮的配置](images/d3d10-compression-bc1.png)

色彩索引 (a 至 p) 是用來從色彩表查看原始的色彩。 色彩表包含 4 個色彩。 前兩個色彩，color\_0 和 color\_1 為色彩的下限和上限。 其他兩個色彩，color\_2 和 color\_3，是以線性插補計算出的中繼色彩。

```cpp
color_2 = 2/3*color_0 + 1/3*color_1
color_3 = 1/3*color_0 + 2/3*color_1
```

四個色彩都被指派了 2 位元索引值，這些值將會儲存在 a 至 p 區塊。

```cpp
color_0 = 00
color_1 = 01
color_2 = 10
color_3 = 11
```

最後，每個 a 至 p 區塊中的色彩會與四種在色彩表內的色彩對比，而最接近色彩的索引會儲存在 2 位元區塊中。

此演算法也適用於包含 1 位元 Alpha 的資料。 唯一不同的是，color\_3 設為 0 (表示透明色彩)，而且 color\_2 是 color\_0 和 color\_1 的線性混合。

```cpp
color_2 = 1/2*color_0 + 1/2*color_1;
color_3 = 0;
```

### <a name="span-idbc2spanspan-idbc2spanbc2"></a><span id="BC2"></span><span id="bc2"></span>BC2

使用 BC2 格式 (可以是 DXGI\_FORMAT\_BC2\_TYPELESS、DXGI\_FORMAT\_BC2\_UNORM 或 DXGI\_BC2\_UNORM\_SRGB) 來儲存資料，其中包含色彩和低一致性 Alpha 資料 (為高一致性 Alpha 資料使用 [BC3](#bc3))。 BC2 格式將 RGB 資料儲存為 5:6:5 色彩 (5 位元紅色、6 位元綠色、5 位元藍色)，並將 Alpha 儲存為不同的 4 位元值。 假設 4×4 紋理使用所能使用的最大資料格式，此壓縮技術會將所需的記憶體從 64 位元組 (16 色 × 4 元件/色彩 × 1 位元組/元件) 降低至 16 位元組的記憶體。

BC2 格式儲存的色彩與 [BC1](#bc1) 格式的位元數量和資料配置相同。不過，BC2 需要額外 64 位元的記憶體來儲存 Alpha 資料，如下圖所示。

![BC2 壓縮的配置](images/d3d10-compression-bc2.png)

### <a name="span-idbc3spanspan-idbc3spanbc3"></a><span id="BC3"></span><span id="bc3"></span>BC3

使用 BC3 格式 (可以是 DXGI\_FORMAT\_BC3\_TYPELESS、DXGI\_FORMAT\_BC3\_UNORM 或 DXGI\_BC3\_UNORM\_SRGB) 來儲存高一致性的色彩資料 (使用 [BC2](#bc2) 儲存低一致性 Alpha 資料)。 BC3 格式使用 5:6:5 色彩 (5 位元紅色、6 位元綠色、5 位元藍色) 儲存色彩資料，並使用 1 個位元組儲存 Alpha 資料。 假設 4×4 紋理使用所能使用的最大資料格式，此壓縮技術會將所需的記憶體從 64 位元組 (16 色 × 4 元件/色彩 × 1 位元組/元件) 降低至 16 位元組的記憶體。

BC3 格式儲存的色彩與 [BC1](#bc1) 格式的位元數量和資料配置相同。不過，BC3 需要額外 64 位元的記憶體來儲存 Alpha 資料。 BC3 格式藉由儲存兩個參考值並在它們之間內插補點來處理 Alpha (和 BC1 儲存 RGB 色彩的方式相同)。

此演算法適用於 4×4 材質區塊上。 此演算法並不儲存 16 種 Alpha 值，而是儲存 2 個參考色彩 (alpha\_0 和 alpha\_1)，以及 16 種 3 位元色彩索引 (alpha a 至 alpha p)，如下圖顯示。

![BC3 壓縮的配置](images/d3d10-compression-bc3.png)

BC3 格式使用 Alpha 索引 (a 至 p)，從包含 8 個值的查閱資料表查詢原始的色彩。 前兩個值 (alpha\_0 和 alpha\_1) 為最小和最大值。其他六個中繼值則是使用線性插補計算出來。

此演算法藉由檢查兩個 Alpha 參考值來判斷插入的 Alpha 值數目。 如果 alpha\_0 大於 alpha\_1，BC3 會插入 6 個 Alpha 值；否則會插入 4 個 Alpha 值。 當 BC3 僅插入 4 個 Alpha 值時，它就會設定兩個額外 Alpha 值 (0 為完全透明，而 255 為完全不透明)。 BC3 藉由儲存對應插補 Alpha 值的位元程式碼，以壓縮 4×4 材質區域中的 Alpha 值，插補進的 Alpha 值最符合特定材質的原始 Alpha。

```cpp
if( alpha_0 > alpha_1 )
{
  // 6 interpolated alpha values.
  alpha_2 = 6/7*alpha_0 + 1/7*alpha_1; // bit code 010
  alpha_3 = 5/7*alpha_0 + 2/7*alpha_1; // bit code 011
  alpha_4 = 4/7*alpha_0 + 3/7*alpha_1; // bit code 100
  alpha_5 = 3/7*alpha_0 + 4/7*alpha_1; // bit code 101
  alpha_6 = 2/7*alpha_0 + 5/7*alpha_1; // bit code 110
  alpha_7 = 1/7*alpha_0 + 6/7*alpha_1; // bit code 111
}
else
{
  // 4 interpolated alpha values.
  alpha_2 = 4/5*alpha_0 + 1/5*alpha_1; // bit code 010
  alpha_3 = 3/5*alpha_0 + 2/5*alpha_1; // bit code 011
  alpha_4 = 2/5*alpha_0 + 3/5*alpha_1; // bit code 100
  alpha_5 = 1/5*alpha_0 + 4/5*alpha_1; // bit code 101
  alpha_6 = 0;                         // bit code 110
  alpha_7 = 255;                       // bit code 111
}
```

### <a name="span-idbc4spanspan-idbc4spanbc4"></a><span id="BC4"></span><span id="bc4"></span>BC4

使用 BC4 格式儲存每個色彩都使用 8 位元的單一元件色彩資料。 由於正確性增加 (相較於 [BC1](#bc1))，BC4 適用於使用 DXGI\_FORMAT\_BC4\_UNORM 格式將浮點資料儲存到 \[0 至 1\] 的範圍內，以及使用 DXGI\_FORMAT\_BC4\_SNORM 將浮點資料儲存到 \[-1 至 +1\] 的範圍內。 假設 4×4 紋理使用所能使用的最大資料格式，此壓縮技術會將所需的記憶體從 16 位元組 (16 色 × 1 元件/色彩 × 1 位元組/元件) 降低至 8 位元組。

此演算法適用於 4×4 材質區塊上。 此演算法並不儲存 16 種顏色，而是儲存 2 個參考色彩 (red\_0 和 red\_1)，以及 16 種 3 位元色彩索引 (紅色 a 至紅色 p 區塊)，如下圖顯示。

![BC4 壓縮的配置](images/d3d10-compression-bc4.png)

此演算法使用 3 位元索引，從一個包含 8 種顏色的顏色表內查詢顏色。 前兩個色彩，red\_0 和 red\_1 為色彩的下限和上限。 此演算法使用線性插補來計算出剩餘的色彩。

此演算法藉由檢查兩個參考值來判斷插入的色彩值數目。 如果 red\_0 大於 red\_1，則 BC4 會插入 6 個色彩值；否則會插入 4 個色彩值。 當 BC4 僅插入 4 個色彩值時，它會設定兩個額外的色彩值 (0.0f 為完全透明，而 1.0f 為完全不透明)。 BC4 藉由儲存對應插補 Alpha 值的位元程式碼，以壓縮 4×4 材質區域中的 Alpha 值，插補進的 Alpha 值最符合特定材質的原始 Alpha。

- [BC4\_UNORM](#bc4-unorm)
- [BC4\_SNORM](#bc4-snorm)

### <a name="span-idbc4unormspanspan-idbc4unormspanspan-idbc4-unormspanbc4unorm"></a><span id="BC4_UNORM"></span><span id="bc4_unorm"></span><span id="bc4-unorm"></span>BC4\_UNORM

單一元件資料的內插補點方式如下列程式碼範例所示。

```cpp
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

參考色彩會被指派 3 位元索引 (因為有 8 個值，所以是 000-111)，此索引會於壓縮期間儲存在紅色 a 到紅色 p 區塊內。

### <a name="span-idbc4snormspanspan-idbc4snormspanspan-idbc4-snormspanbc4snorm"></a><span id="BC4_SNORM"></span><span id="bc4_snorm"></span><span id="bc4-snorm"></span>BC4\_SNORM

DXGI\_FORMAT\_BC4\_SNORM 也完全一樣，差別只在於資料會在 SNORM 範圍內及插入 4 個資料值時進行編碼。 單一元件資料的內插補點方式如下列程式碼範例所示。

```cpp
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                    // bit code 110
  red_7 =  1.0f;                    // bit code 111
}
```

參考色彩會被指派 3 位元索引 (因為有 8 個值，所以是 000-111)，此索引會於壓縮期間儲存在紅色 a 到紅色 p 區塊內。

### <a name="span-idbc5spanspan-idbc5spanbc5"></a><span id="BC5"></span><span id="bc5"></span>BC5

以 BC5 格式儲存每個色彩都使用 8 位元的雙元件色彩資料。 由於正確性增加 (相較於 [BC1](#bc1))，BC5 適用於使用 DXGI\_FORMAT\_BC5\_UNORM 格式將浮點資料儲存到 \[0 至 1\] 的範圍內，以及使用 DXGI\_FORMAT\_BC5\_SNORM 將浮點資料儲存到 \[-1 至 +1\] 的範圍內。 假設 4×4 紋理使用所能使用的最大資料格式，此壓縮技術會將所需的記憶體從 32 位元組 (16 色 × 2 元件/色彩 × 1 位元組/元件) 降低至 16 位元組。

- [BC5\_UNORM](#bc5-unorm)
- [BC5\_SNORM](#bc5-snorm)

此演算法適用於 4×4 材質區塊上。 演算法不是為兩個元件儲存 16 種色彩，而是為每個元件儲存 2 種參考色彩 (red\_0、red\_1、green\_0，以及 green\_1)，並為每個元件儲存 16 種 3 位元色彩索引 (從紅色 a 到紅色 p，及綠色 a 到綠色 p)，如下圖所示。

![BC5 壓縮的配置](images/d3d10-compression-bc5.png)

此演算法使用 3 位元索引，從一個包含 8 種顏色的顏色表內查詢顏色。 前兩個色彩，red\_0 red\_1 (或 green\_0 和 green\_1)，為色彩的下限和上限。 此演算法使用線性插補來計算出剩餘的色彩。

此演算法藉由檢查兩個參考值來判斷插入的色彩值數目。 如果 red\_0 大於 red\_1，則 BC5 會插入 6 個色彩值；否則會插入 4 個色彩值。 當 BC5 僅插入 4 個色彩值時，它會將剩下的兩個色彩值設定為 0.0f 和 1.0f。

### <a name="span-idbc5unormspanspan-idbc5unormspanspan-idbc5-unormspanbc5unorm"></a><span id="BC5_UNORM"></span><span id="bc5_unorm"></span><span id="bc5-unorm"></span>BC5\_UNORM

單一元件資料的內插補點方式如下列程式碼範例所示。 綠色元件的計算方法都很相似。

```cpp
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

參考色彩會被指派 3 位元索引 (因為有 8 個值，所以是 000-111)，此索引會於壓縮期間儲存在紅色 a 到紅色 p 區塊內。

### <a name="span-idbc5snormspanspan-idbc5snormspanspan-idbc5-snormspanbc5snorm"></a><span id="BC5_SNORM"></span><span id="bc5_snorm"></span><span id="bc5-snorm"></span>BC5\_SNORM

DXGI\_FORMAT\_BC5\_SNORM 也完全一樣，差別只在於資料會編碼至 SNORM 範圍內，而且在插入 4 個資料值時，兩個額外的值為 -1.0f 和 1.0f。 單一元件資料的內插補點方式如下列程式碼範例所示。 綠色元件的計算方法都很相似。

```cpp
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                    // bit code 110
  red_7 =  1.0f;                    // bit code 111
}
```

參考色彩會被指派 3 位元索引 (因為有 8 個值，所以是 000-111)，此索引會於壓縮期間儲存在紅色 a 到紅色 p 區塊內。

## <a name="span-iddifferencesspanspan-iddifferencesspanspan-iddifferencesspanformat-conversion"></a><span id="Differences"></span><span id="differences"></span><span id="DIFFERENCES"></span>格式轉換

Direct3D 可讓您在預先結構類型紋理與位元寬度相同的區塊壓縮紋理之間進行複製。

您可以在一些格式類型之間複製資源。 此類型的複製作業會執行一種格式轉換，將資源資料重新解譯為不同的格式類型。 請將此範例納入考慮，它用更典型的轉換行為方式來顯示重新解譯資料之間的不同︰

```cpp
FLOAT32 f = 1.0f;
UINT32 u;
```

若要將 'f' 重新解譯為 'u' 類型，請使用 [memcpy](https://msdn.microsoft.com/library/dswaw1wk.aspx)：

```cpp
memcpy( &u, &f, sizeof( f ) ); // 'u' becomes equal to 0x3F800000.
```

在上述重新解譯中，資料的基礎值不會變更。[memcpy](https://msdn.microsoft.com/library/dswaw1wk.aspx) 會將浮點數重新解譯為不帶正負號的整數。

若要執行更常見的轉換類型，請使用下列設定︰

```cpp
u = f; // 'u' becomes 1.
```

在上述轉換中，資料的基礎值有所變更。

下表列出允許的來源和目的地格式，您可以在此格式轉換的重新解譯類型內使用。 您必須正確地為值進行編碼，才能讓重新解譯正常運作。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">位元寬度</th>
<th align="left">未壓縮資源</th>
<th align="left">區塊壓縮資源</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">32</td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p>
<p>DXGI_FORMAT_R32_SINT</p></td>
<td align="left">DXGI_FORMAT_R9G9B9E5_SHAREDEXP</td>
</tr>
<tr class="even">
<td align="left">64</td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UINT</p>
<p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<p>DXGI_FORMAT_R32G32_UINT</p>
<p>DXGI_FORMAT_R32G32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC4_UNORM</p>
<p>DXGI_FORMAT_BC4_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left">128</td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_UINT</p>
<p>DXGI_FORMAT_R32G32B32A32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC3_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC5_UNORM</p>
<p>DXGI_FORMAT_BC5_SNORM</p></td>
</tr>
</tbody>
</table>

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題

[壓縮的紋理資源](compressed-texture-resources.md)
