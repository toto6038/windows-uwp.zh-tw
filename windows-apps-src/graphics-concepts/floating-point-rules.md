---
title: 浮點規則
description: Direct3D 支援數個浮點表示法。 所有浮點數計算運作都在 IEEE 754 32 位元單精確度浮點數規則的定義子集下運作。
ms.assetid: 3B0C95E2-1025-4F70-BF14-EBFF2BB53AFF
keywords:
- 浮點規則
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a29fbe49e45b819ddf4ffc3172445996d3622360
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370621"
---
# <a name="span-iddirect3dconceptsfloating-pointrulesspanfloating-point-rules"></a><span id="direct3dconcepts.floating-point_rules"></span>浮點數的規則


Direct3D 支援數個浮點表示法。 所有浮點數計算運作都在 IEEE 754 32 位元單精確度浮點數規則的定義子集下運作。

## <a name="span-idalpha32bitspanspan-idalpha32bitspan32-bit-floating-point-rules"></a><span id="alpha_32_bit"></span><span id="ALPHA_32_BIT"></span>32 位元浮點數的規則


有兩組規則：符合 IEEE-754 的規則，以及偏離標準的規則。

### <a name="span-idalpha754rulesspanspan-idalpha754rulesspanspan-idalpha754rulesspanhonored-ieee-754-rules"></a><span id="alpha_754_Rules"></span><span id="alpha_754_rules"></span><span id="ALPHA_754_RULES"></span>接受的 IEEE-754 規則

有些規則是單一選項，由 IEEE-754 提供選擇。

-   除以 0 會產生 +/- INF，除了 0/0 得到 NaN 的結果。
-   (+/-) 0 的對數會產生 -INF。  

    負值 (-0 除外) 的對數會產生 NaN。
-   負數的反平方根 (rsq) 或平方根 (sqrt) 會產生 NaN。  

    例外是 -0；sqrt(-0) 會產生 -0，而 rsq(-0) 會產生 -INF。
-   INF - INF = NaN
-   (+/-)INF / (+/-)INF = NaN
-   (+/-)INF \* 0 = NaN
-   NaN (任意 OP) 任意值 = NaN
-   比較 EQ、GT、GE、LT 和 LE，若任一個或兩個運算元皆為 NaN，則傳回 **FALSE**。
-   比較會忽略 0 的正負號 (因此 +0 等於 -0)。
-   比較 NE，若任一個或兩個運算元皆為 NaN，則傳回 **TRUE**。
-   任何非 NaN 值與 +/- INF 比較會傳回正確的結果。

### <a name="span-idalpha754deviationsspanspan-idalpha754deviationsspanspan-idalpha754deviationsspandeviations-or-additional-requirements-from-ieee-754-rules"></a><span id="alpha_754_Deviations"></span><span id="alpha_754_deviations"></span><span id="ALPHA_754_DEVIATIONS"></span>標準差或從 IEEE-754 規則的其他需求

-   IEEE-754 需要浮點運算產生結果，此結果為最接近無限精確結果的可表示值，也稱為 round-to-nearest-even。

    Direct3D 11 和最多為 IEEE-754 定義相同的需求：32 位元的浮點數運算會產生結果內 0.5 單元-last-取代 (ULP) 極為精確的結果。 這表示，例如硬體允許截斷結果至 32 位元，而不是執行 round-to-nearest-even，因為這樣會讓最多 0.5 ULP 產生錯誤。 此規則僅適用於加法、減法和乘法。

    較早版本的 Direct3D 定義於 IEEE-754 鬆散需求：32 位元的浮點數運算會產生結果內單元-最後一層的一個位置 (1 ULP) 極為精確的結果。 這表示，例如硬體允許截斷結果至 32 位元，而不是執行 round-to-nearest-even，因為這樣會讓最多一個 ULP 產生錯誤。

-   不支援浮點例外、狀態位元或設陷。
-   Denorm 會在任何浮點數學運算的輸入和輸出上，排清至保留正負號的零。 例外是針對不操作資料的任何 I/O 或資料移動作業。
-   包含浮點值的狀態，像是 Viewport MinDepth/MaxDepth 或 BorderColor 值，可能會提供為 Denorm 值，也不一定會在硬體使用它們之前排清。
-   最小值或最大值作業會針對比較排清 denorm，但結果不一定會是排清 denorm。
-   運算的 NaN 輸入一律會在輸出產生 NaN。 但 NaN 的精確位元模式不必保持不變 (除非運算操作是原始移動指示 - 不會修改資料)。
-   只有一個運算元是 NaN 的最小值或最大值運算，會傳回其他運算元做為結果 (與我們前面所說的比較規則相反)。 這是 IEEE 754R 規則。

    浮點最小值和最大值運算的 IEEE-754R 規格指出，如果其中一個最小值或最大值的輸入是 quiet QNaN 值，運算結果就是另一個參數。 例如: 

    ```ManagedCPlusPlus
    min(x,QNaN) == min(QNaN,x) == x (same for max)
    ```

    IEEE-754R 規格的修訂對最小值和最大值採用了不同的行為，當一個輸入為 "signaling" SNaN 值與 QNaN 值比較：

    ```ManagedCPlusPlus
    min(x,SNaN) == min(SNaN,x) == QNaN (same for max)
     
    ```

    一般而言，Direct3D 遵循算術運算的標準：IEEE-754 和 IEEE 754R。 但在此案例中有誤差。

    Direct3D 10 及更新版本中的算術規則不會在 quiet 和 signaling NaN 值 (QNaN 與 SNaN 比較) 之間做出區別。 所有 NaN 值都以相同方式處理。 若是最小值和最大值，任何 NaN 的 Direct3D 行為就像是 QNaN 在 IEEE-754R 定義中的處理方式。 (為了完整性，如果兩個輸入都是 NaN，則會傳回任一個 NaN 值)。

-   另一項 IEEE 754R 規則是 min(-0,+0) == min(+0,-0) == -0 和 max(-0,+0) == max(+0,-0) == +0，其採用正負號，與帶正負號的零的比較規則相反 (如前面所見)。 Direct3D 在此建議 IEEE 754R 行為，但不會強制執行；它允許比較零的結果取決於參數順序，使用忽略正負號的比較。
-   x\*1.0f 永遠會導致 x （除了 denorm 排清)。
-   x/1.0f 一律產生 x (除了會排清 denorm)。
-   x +/- 0.0f 一律產生 x (除了會排清 denorm)。 但是 -0 + 0 = +0。
-   合併運算 (例如 mad、dp3) 產生的結果肯定比非合併擴展運算可能最差的序列評估順序精確。 可能最差順序的定義 (基於誤差目的) 對於特定合併運算來說並非固定的定義；它取決於特殊的輸入值。 對於非合併擴展的個別步驟，每一個允許 1 ULP 誤差 (或對於 Direct3D 呼叫的任何指令，若 lax 誤差多於 1 ULP，則允許越大 lax 誤差)。
-   合併運算遵循與非合併運算相同的 NaN 規則。
-   sqrt 和 rcp 有 1 ULP 誤差。 著色器倒數和倒數平方根指令 [**rcp**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/hh447205(v=vs.85)) 和 [**rsq**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/rsq--sm4---asm-) 各有自己較寬鬆的精確需求。
-   乘和除各在 32 位元浮點精準度運算 (乘積的精準度為 0.5 ULP，倒數的精準度為 1.0 ULP)。 如果直接實作 x/y，結果必須大於或等於比兩步驟方法的精準度。

## <a name="span-iddoubleprec64bitspanspan-iddoubleprec64bitspan64-bit-double-precision-floating-point-rules"></a><span id="double_prec_64_bit"></span><span id="DOUBLE_PREC_64_BIT"></span>64 位元 （雙精確度） 浮動點規則


硬體和顯示器驅動程式選擇性支援雙精確度浮點。 若要表示支援，當您呼叫[ **ID3D11Device::CheckFeatureSupport** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport)具有[ **D3D11\_功能\_雙精度浮點數**](https://docs.microsoft.com/windows/desktop/api/d3d11/ne-d3d11-d3d11_feature)，驅動程式集**DoublePrecisionFloatShaderOps**的[ **D3D11\_功能\_資料\_雙精度浮點數**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_feature_data_doubles)設為 TRUE。 驅動程式和硬體必須支援所有雙精確度浮點指令。

雙精確度指令遵循 IEEE 754R 行為需求。

產生去標準化值的支援為雙精確度資料所必要 (不允許 flush-to-zero 行為)。 同樣地，指令不會將去標準化資料讀做帶正負號的零，它們會採用 denorm 值。

## <a name="span-idalpha16bitspanspan-idalpha16bitspan16-bit-floating-point-rules"></a><span id="alpha_16_bit"></span><span id="ALPHA_16_BIT"></span>16 位元浮點數的規則


Direct3D 也支援 16 位元浮點數表示法。

格式：

-   在 MSB 位元位置 1 帶正負號位元 (s)
-   5 位元偏置指数 (e)
-   10 位元分數 (f)，含一個額外的隱藏位元

float16 值 (v) 遵循下列規則：

-   若 e == 31 且 f != 0，則 v 為 NaN，不考慮 s
-   如果 e = = 31 和 f = = 0，則 v = (-1) s\*無限大 （帶正負號的無限值）
-   如果 e 是介於 0 與 31，則 v = (-1) s\*2(e-15)\*1.f （則）
-   如果 e = = 0 且 f ！ = 0，則 v = (-1) s\*2(e-14)\*(0.f) （反正規化的號碼）
-   如果 e = = 0 且 f = = 0，則 v = (-1) s\*0 （帶正負號的零）

32 位元浮點規則也會針對 16 位元浮點數保留，並針對前面所述的位元配置調整。 例外狀況包括：

-   有效位數：16 位元浮點數的 unfused 的作業產生的結果，其會接近的極為精確的結果 （最接近甚至每 IEEE-754，套用至 16 位元值的圓形） 表示的值。 32 位元浮點規則遵循 1 ULP 誤差，16 位元浮點規則針對非合併運算遵循 0.5 ULP，合併運算則為 0.6 ULP。
-   16 位元浮點數保留 denorm。

## <a name="span-idalpha11bitspanspan-idalpha11bitspan11-bit-and-10-bit-floating-point-rules"></a><span id="alpha_11_bit"></span><span id="ALPHA_11_BIT"></span>11 位元與 10 位元浮點數的規則


Direct3D 也支援 11 位元與 10 位元浮點格式。

格式：

-   不帶正負號的位元
-   5 位元偏置指数 (e)
-   11 位元格式為 6 位元分數 (f)，10 位元格式為 5 位元分數 (f)，兩者皆有一個額外的隱藏位元。

float11/float10 值 (v) 遵循下列規則：

-   若 e == 31 且 f != 0，則 v 為 NaN
-   若 e == 31 且 f == 0，則 v = +infinity
-   如果 e 是介於 0 到 31，然後 v = 2(e-15)\*1.f （則）
-   如果 e = = 0 且 f ！ = 0，則 v = \*2(e-14)\*(0.f) （反正規化的號碼）
-   若 e == 0 且 f == 0，則 v = 0 (零)

32 位元浮點規則也會針對 11 位元和 10 位元浮點數保留，並針對前面所述的位元配置調整。 例外包括︰

-   有效位數：32 位元浮點規則需遵守 0.5 ULP。
-   10/11 位元浮點數保留 denorm。
-   任何結果為小於零的運算都會限制為零。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[附錄](appendix.md)

[資源](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources)

[紋理](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 




