---
title: 使用 Alpha 色板的紋理
description: 有兩種方式可以對呈現較複雜透明度的紋理貼圖進行編碼。
ms.assetid: 768A774A-4F21-4DDE-B863-14211DA92926
keywords:
- 使用 Alpha 色板的紋理
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eef41642d371f3a8be451c2687eee007608c3b2e
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5542292"
---
# <a name="textures-with-alpha-channels"></a>使用 Alpha 色板的紋理


有兩種方式可以對呈現較複雜透明度的材質貼圖進行編碼。 在每個例子中，描述透明度的區塊都會優先於之前已描述完成的 64 位元區塊。 透明度通常不是透過每像素 4 個位元 (明確編碼) 的 4 x 4 點陣圖呈現，就是透過較少位元並且類比於色彩編碼的線性插補呈現。

透明度區塊及色彩區塊會如下表所示進行排列。

| 文字位址 | 64 位元區塊                      |
|--------------|-----------------------------------|
| 3:0          | 透明度區塊                |
| 7:4          | 之前已描述完成的 64 位元區塊 |

 

## <a name="span-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanexplicit-texture-encoding"></a><span id="Explicit-Texture-Encoding"></span><span id="explicit-texture-encoding"></span><span id="EXPLICIT-TEXTURE-ENCODING"></span>明確紋理編碼


針對明確紋理編碼 (BC2 格式)，描述透明度的材質 Alpha 元件會編碼於一個每個材質 4 個位元的 4 x 4 點陣圖中。 這四個位元可透過不同的方式達成，例如：遞色，或是使用 Alpha 資料中最高有效的四個位元。 無論其產生方式為何，其會直接使用於程式之中，而不會有任何型式的插補。

下列簡圖呈現了一個 64 位元的透明度區塊。

![64 位元透明度區塊的簡圖](images/colors4.png)

**注意：**  ： 壓縮 Direct3D 的方式使用四個最高有效位元。

 

下表描述了如何在記憶體中，針對每個 16 位元文字配置 Alpha 資訊。

文字 0 的配置：

| 位元數          | Alpha      |
|---------------|------------|
| 3:0 (LSB\*)   | \[0\]\[0\] |
| 7:4           | \[0\]\[1\] |
| 11:8          | \[0\]\[2\] |
| 15:12 (MSB\*) | \[0\]\[3\] |

 

\*最低有效位元、最高有效位元 (MSB)

文字 1 的配置：

| 位元數        | Alpha      |
|-------------|------------|
| 3:0 (LSB)   | \[1\]\[0\] |
| 7:4         | \[1\]\[1\] |
| 11:8        | \[1\]\[2\] |
| 15:12 (MSB) | \[1\]\[3\] |

 

文字 2 的配置：

| 位元數        | Alpha      |
|-------------|------------|
| 3:0 (LSB)   | \[2\]\[0\] |
| 7:4         | \[2\]\[1\] |
| 11:8        | \[2\]\[2\] |
| 15:12 (MSB) | \[2\]\[3\] |

 

文字 3 的配置：

| 位元數        | Alpha      |
|-------------|------------|
| 3:0 (LSB)   | \[3\]\[0\] |
| 7:4         | \[3\]\[1\] |
| 11:8        | \[3\]\[2\] |
| 15:12 (MSB) | \[3\]\[3\] |

 

在 BC1 中用作判斷材質是否為透明的色彩比較，並未使用於此種格式之中。 若不使用色彩比較，色彩資料預設會以 4 色模式進行處理。

## <a name="span-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanthree-bit-linear-alpha-interpolation"></a><span id="Three-Bit-Linear-Alpha-Interpolation"></span><span id="three-bit-linear-alpha-interpolation"></span><span id="THREE-BIT-LINEAR-ALPHA-INTERPOLATION"></span>三位元線性 Alpha 插補


BC3 格式的透明度編碼以類似於色彩線性編碼的概念作為基礎。 兩個 8 位元 Alpha 值和一個每個像素中有三個位元的 4 x 4 點陣圖，會儲存於區塊中前八個位元組。 代表 Alpha 值會作為插補中繼 Alpha 值之用途使用。 其他資訊會以兩個 Alpha 值儲存的方式供作使用。 若 alpha\_0 大於 alpha\_1，插補會建立六個中繼 Alpha 值。 若為小於，則四個中繼 Alpha 值會插補於指定的 Alpha 極端值之間。 另外兩個額外隱含 Alpha 值則為 0 (完全透明) 及 255 (完全不透明)。

下列程式碼示範了這個演算法。

```
// 8-alpha or 6-alpha block?    
if (alpha_0 > alpha_1) {    
    // 8-alpha block:  derive the other six alphas.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (6 * alpha_0 + 1 * alpha_1 + 3) / 7;    // bit code 010
    alpha_3 = (5 * alpha_0 + 2 * alpha_1 + 3) / 7;    // bit code 011
    alpha_4 = (4 * alpha_0 + 3 * alpha_1 + 3) / 7;    // bit code 100
    alpha_5 = (3 * alpha_0 + 4 * alpha_1 + 3) / 7;    // bit code 101
    alpha_6 = (2 * alpha_0 + 5 * alpha_1 + 3) / 7;    // bit code 110
    alpha_7 = (1 * alpha_0 + 6 * alpha_1 + 3) / 7;    // bit code 111  
}    
else {  
    // 6-alpha block.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (4 * alpha_0 + 1 * alpha_1 + 2) / 5;    // Bit code 010
    alpha_3 = (3 * alpha_0 + 2 * alpha_1 + 2) / 5;    // Bit code 011
    alpha_4 = (2 * alpha_0 + 3 * alpha_1 + 2) / 5;    // Bit code 100
    alpha_5 = (1 * alpha_0 + 4 * alpha_1 + 2) / 5;    // Bit code 101
    alpha_6 = 0;                                      // Bit code 110
    alpha_7 = 255;                                    // Bit code 111
}
```

Alpha 區塊的記憶體配置如下︰

| 位元組 | Alpha                                                          |
|------|----------------------------------------------------------------|
| 0    | Alpha\_0                                                       |
| 1    | Alpha\_1                                                       |
| 2    | \[0\]\[2\] (2 MSBs)，\[0\]\[1\]，\[0\]\[0\]                    |
| 3    | \[1\]\[1\] (1 MSB)，\[1\]\[0\]，\[0\]\[3\]，\[0\]\[2\] (1 LSB) |
| 4    | \[1\]\[3\]，\[1\]\[2\]，\[1\]\[1\] (2 LSBs)                    |
| 5    | \[2\]\[2\] (2 MSBs)，\[2\]\[1\]，\[2\]\[0\]                    |
| 6    | \[3\]\[1\] (1 MSB)，\[3\]\[0\]，\[2\]\[3\]，\[2\]\[2\] (1 LSB) |
| 7    | \[3\]\[3\]，\[3\]\[2\]，\[3\]\[1\] (2 LSBs)                    |

 

在 BC1 中用作判斷材質是否為透明的色彩比較，並未使用於此種格式之中。 若不使用色彩比較，色彩資料預設會以 4 色模式處理。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[壓縮的紋理資源](compressed-texture-resources.md)

 

 




