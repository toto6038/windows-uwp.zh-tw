---
title: 資料類型轉換
description: 下列章節說明 Direct3D 如何處理資料類型之間的轉換。
ms.assetid: B50AB8DE-CAED-465B-B18C-81F3A984B8AC
keywords:
- 資料類型轉換
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 08c6dda8759a6e1452daf7cf0a3cd3e5db9ea1e6
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8701179"
---
# <a name="data-type-conversion"></a>資料類型轉換


下列章節說明 Direct3D 如何處理資料類型之間的轉換。

## <a name="span-iddatatypeterminologyspanspan-iddatatypeterminologyspanspan-iddatatypeterminologyspandata-type-terminology"></a><span id="Data_Type_Terminology"></span><span id="data_type_terminology"></span><span id="DATA_TYPE_TERMINOLOGY"></span>資料類型詞彙


下列詞彙組後續將用來描述各種不同的格式轉換。

| 詞彙  | 定義                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SNORM | 帶正負號的標準化整數，表示 n-位元 2 的補數，最大值是指 1.0f (例如 5-位元值 01111 對應至 1.0f)，最小值是指 -1.0f (例如 5-位元值 10000 對應至 -1.0f)。 此外，第二小的數字對應至 -1.0f (例如 5-位元值 10001 對應至 -1.0f)。 因此，-1.0f 有兩種整數表示法。 0.0f 只有一種表示法，1.0f 也只有一種表示法。 這會對範圍 (-1.0f...0.0f) 中間隔平均的浮點值產生一組整數表示法，另外，範圍 (0.0f...1.0f) 中數字的補數也會有一組表示法 |
| UNORM | 不帶正負號的標準化整數，表示對於 n 位元數字，所有 0 都表示 0.0f，所有 1 都表示 1.0f。 表示從 0.0f 到 1.0f 間隔平均的浮點值序列。 例如 2-位元 UNORM 代表 0.0f、1/3、2/3 及 1.0f。                                                                                                                                                                                                                                                                                                                                                                                                                                |
| SINT  | 帶正負號的整數。 2 的整數補數。 例如 3-位元 SINT 代表整數值 -4、-3、-2、-1、0、1、2、3。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| UINT  | 不帶正負號的整數。 例如 3-位元 UINT 代表整數值 0、1、2、3、4、5、6、7。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| FLOAT | Direct3D 定義的任何表示法中的浮點值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| SRGB  | 類似 UNORM，對於 n-位元數字，所有 0 都表示 0.0f，所有 1 都表示 1.0f。 不過，與 UNORM 不同的是，若是 SRGB，介於所有 0 到所有 1 之間不帶正負號的整數編碼序列代表數字 (介於 0.0f 到 1.0f) 的浮點轉譯中的非線性數列。 大致來說，如果這個非線性數列 SRGB 顯示為色彩序列，則會顯示為相對於「平均」觀察值的線性亮度等級坡形，在「平均」檢視條件下「平均」顯示。 如需完整的詳細資訊，請參考 IEC (國際電子電機委員會) 的 SRGB 色彩標準 IEC 61996-2-1。                |

 

上述詞彙常做為「格式名稱修飾詞」，用來描述資料在記憶體中配置的方式，以及於傳輸路徑中要在記憶體與管線單位 (如著色器) 之間來回進行哪些轉換 (可能包括篩選)。

## <a name="span-idfloatingpointconversionspanspan-idfloatingpointconversionspanspan-idfloatingpointconversionspanfloating-point-conversion"></a><span id="Floating_Point_Conversion"></span><span id="floating_point_conversion"></span><span id="FLOATING_POINT_CONVERSION"></span>浮點轉換


只要不同的表示法之間發生浮點轉換，包括非浮點表示法之間來回轉換，就適用下列規則。

### <a name="span-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanconververting-from-a-higher-range-representation-to-a-lower-range-representation"></a><span id="Conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="CONVERVERTING_FROM_A_HIGHER_RANGE_REPRESENTATION_TO_A_LOWER_RANGE_REPRESENTATION"></span>從較高範圍表示法轉換成較低範圍表示法

-   轉換成另一種浮點數格式時會使用 Round-to-zero (四捨五入至零)。 如果目標是整數或固定點格式，則會使用 round-to-nearest-even (四捨五入至最接近的偶數)，除非轉換明確記載為使用另一種四捨五入行為，例如 FLOAT 轉換成 SNORM、FLOAT 轉換成 UNORM 或 FLOAT 轉換成 SRGB 使用的 round-to-nearest (四捨五入至最接近數字)。 其他例外狀況為 ftoi 和 ftou shader 指令，會使用 round-to-zero (四捨五入至零)。 最後，紋理取樣工具和點陣化使用的 float-to-fixed 轉換有指定的容錯，以來自無限精確概念的 Unit-Last-Place 計算。
-   若來源值大於較低範圍目標格式的動態範圍 (例如 大型 32 位元浮點值寫入 16 位元浮點值 RenderTarget)，則會得出最大可表示 (帶適當正負號) 值，「不」包括帶正負號的無限大 (因為上述的四捨五入至零)。
-   採用較高範圍格式的 NaN 將轉換成採用較低範圍格式的 NaN 表示法，如果 NaN 表示法存在較低範圍格式中。 如果較低格式沒有 NaN 表示法，則結果會是 0。
-   採用較高範圍格式的 INF 將會轉換成採用較低範圍格式的 INF (如有的話)。 如果較低格式沒有 INF 表示法，它將會轉換成可表示的最大值。 正負號將保留，如果目標格式中提供。
-   採用較高範圍格式的 Denorm 將會轉換成採用較低範圍格式的 Denorm 表示法 (如較低範圍格式中有提供且可轉換)，否則結果會是 0。 正負號位元將保留，如果目標格式中提供。

### <a name="span-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanconverting-from-a-lower-range-representation-to-a-higher-range-representation"></a><span id="Converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="CONVERTING_FROM_A_LOWER_RANGE_REPRESENTATION_TO_A_HIGHER_RANGE_REPRESENTATION"></span>從較低範圍表示法轉換成較高範圍表示法

-   採用較低範圍格式的 NaN 將轉換成採用較高範圍格式的 NaN 表示法 (如果較高範圍格式中有提供)。 如果較高範圍格式沒有 NaN 表示法，它將會轉換成 0。
-   採用較低範圍格式的 INF 將轉換成採用較高範圍格式的 INF 表示法 (如果較高範圍格式中有提供)。 如果較高格式沒有 INF 表示法，它將會轉換成可表示的最大值 (採用該格式的 MAX\_FLOAT)。 正負號將保留，如果目標格式中提供。
-   採用較低範圍格式的 Denorm 將轉換成採用較高範圍格式的標準化表示法 (如有可能)，或是轉換成採用較高範圍格式的 Denorm 表示法 (如果 Denorm 表示法存在)。 若較高範圍格式沒有 Denorm 表示法，則這些都會失敗，且它將會轉換成 0。 正負號將保留，如果目標格式中提供。 請注意，32 位元浮點數計算格式時不會採用 Denorm 表示法 (因為 32 位元浮點數運算中，Denorm 會清除為保留正負號的 0)。

## <a name="span-idintegerconversionspanspan-idintegerconversionspanspan-idintegerconversionspaninteger-conversion"></a><span id="Integer_Conversion"></span><span id="integer_conversion"></span><span id="INTEGER_CONVERSION"></span>整數轉換


下表描述從上述各種不同的表示法轉換成其他表示法。 只會顯示實際在 Direct3D 中發生的轉換。

若是整數，除非另行指定，否則下述整數表示法與浮點數表示法之間的所有來回轉換都將確實完成。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">來源資料類型</th>
<th align="left">目的地資料類型</th>
<th align="left">轉換規則</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">SNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>假設有一個 n-位元整數值，代表帶正負號的範圍 [-1.0f 至 1.0f]，轉換成浮點數的方式如下所述。</p>
<ul>
<li>最大負值對應至 -1.0f。 例如 5-位元值 10000 對應至 -1.0f。</li>
<li>其他每一個值都會轉換成浮點數 (稱為 c)，然後結果 = c * (1.0f / (2⁽ⁿ⁻¹⁾-1))。 例如，5 位元值 10001 轉換成 -15.0f，然後除以 15.0f，得出 -1.0f。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">SNORM</td>
<td align="left"><p>假設有一個浮點數，轉換成代表帶正負號的範圍 [-1.0f 至 1.0f] 的 n-位元整數值的方式如下所述。</p>
<ul>
<li>讓 c 代表開始值。</li>
<li>如果 c 是 NaN，則結果是 0。</li>
<li>如果 c &gt;1.0f，包括 INF，它會限制為 1.0f。</li>
<li>如果 c &lt; -1.0f，包括 -INF，它會限制為 -1.0f。</li>
<li>從浮點數進位法轉換成整數進位法：c = c * (2ⁿ⁻¹-1)。</li>
<li>如下所述轉換成整數。
<ul>
<li>如果 c &gt;= 0，則 c = c + 0.5f，否則 c = c - 0.5f。</li>
<li>去除小數，剩餘的浮點 (整數) 值就會直接轉換成整數。</li>
</ul></li>
</ul>
<p>此轉換容許的容錯為 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_Unit-Last-Place Unit-Last-Place (整數端)。 這表示，從浮點數轉換成整數進位法之後，可表示的目標格式值的 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place 內的任何值都可對應至該值。 額外的資料可逆性需求可確保轉換在整個範圍內不減少，而且所有輸出值都可實現。 (在此處顯示的常數中，<em>xx</em> 應取代為 Direct3D 版本，例如 10、11 或 12)</p></td>
</tr>
<tr class="odd">
<td align="left">UNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>開始的 n 位元值會轉換成浮點數 (0.0f、1.0f、2.0f 等)，然後除以 (2ⁿ-1)。</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">UNORM</td>
<td align="left"><p>讓 c 代表開始值。</p>
<ul>
<li>如果 c 是 NaN，則結果是 0。</li>
<li>如果 c &gt;1.0f，包括 INF，它會限制為 1.0f。</li>
<li>如果 c &lt; 0.0f，包括 -INF，它會限制為 0.0f。</li>
<li>從浮點數進位法轉換成整數進位法：c = c * (2ⁿ-1)。</li>
<li>轉換成整數。
<ul>
<li>c = c + 0.5f。</li>
<li>小數會去除，剩餘的浮點 (整數) 值就會直接轉換成整數。</li>
</ul></li>
</ul>
<p>此轉換容許的容錯為 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place (整數端)。 這表示，從浮點數轉換成整數進位法之後，可表示的目標格式值的 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place 內的任何值都可對應至該值。 額外的資料可逆性需求可確保轉換在整個範圍內不減少，而且所有輸出值都可實現。</p></td>
</tr>
<tr class="odd">
<td align="left">SRGB</td>
<td align="left">FLOAT</td>
<td align="left"><p>以下是理想的 SRGB 至 FLOAT 轉換。</p>
<ul>
<li>取一個開始的 n 位元值，將它轉換成浮點數 (0.0f、1.0f、2.0f 等)；將此稱為 c。</li>
<li>c = c * (1.0f / (2ⁿ-1))</li>
<li>如果 (c &lt; = D3D<em>xx</em>_SRGB_TO_FLOAT_THRESHOLD) then: result = c / D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_1，否則：結果 = ((c + D3D<em>xx</em>_SRGB_TO_FLOAT_OFFSET)/D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_2)D3D<em>xx</em>_SRGB_TO_FLOAT_EXPONENT</li>
</ul>
<p>此轉換容許的容錯為 D3D<em>xx</em>_SRGB_TO_FLOAT_TOLERANCE_IN_ULP Unit-Last-Place (SRGB 端)。</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">SRGB</td>
<td align="left"><p>以下是理想的 FLOAT -&gt; SRGB 轉換。</p>
<p>假設目標 SRGB 色彩元件有 n 個位元：</p>
<ul>
<li>假設開始值是 c。</li>
<li>如果 c 是 NaN，則結果是 0。</li>
<li>如果 c &gt; 1.0f，包括 INF，它會限制為 1.0f。</li>
<li>如果 c &lt; 0.0f，包括 -INF，它會限制為 0.0f。</li>
<li>如果 (c &lt;= D3D<em>xx</em>_FLOAT_TO_SRGB_THRESHOLD) then: c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_1 * c，否則：c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_2 * c(D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_NUMERATOR/D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_DENOMINATOR) - D3D<em>xx</em>_FLOAT_TO_SRGB_OFFSET</li>
<li>從浮點數進位法轉換成整數進位法：c = c * (2ⁿ-1)。</li>
<li>轉換成整數：
<ul>
<li>c = c + 0.5f。</li>
<li>小數會去除，剩餘的浮點 (整數) 值就會直接轉換成整數。</li>
</ul></li>
</ul>
<p>此轉換容許的容錯為 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place (整數端)。 這表示，從浮點數轉換成整數進位法之後，可表示的目標格式值的 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place 內的任何值都可對應至該值。 額外的資料可逆性需求可確保轉換在整個範圍內不減少，而且所有輸出值都可實現。</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">SINT 與更多位元</td>
<td align="left"><p>若要從 SINT 轉換成 SINT 與更多位元，開始數字的最高有效位元 (MSB) 會是 &quot;正負號延伸&quot; 至目標格式中提供的其他位元。</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">SINT 與更多位元</td>
<td align="left"><p>若要從 UINT 轉換成 SINT 與更多位元，數字會複製到目標格式的最低有效位元 (LSB)，且其他 MSB 會以 0 填入。</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">UINT 與更多位元</td>
<td align="left"><p>若要從 SINT 轉換成 UINT 與更多位元：如果是負數，值會限制為 0。 否則數字會複製到目標格式的 LSB，且其他 MSB 會以 0 填入。</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">UINT 與更多位元</td>
<td align="left"><p>若要從 UINT 轉換成 UINT 與更多位元，數字會複製到目標格式的 LSB，且其他 MSB 會以 0 填入。</p></td>
</tr>
<tr class="odd">
<td align="left">SINT 或 UINT</td>
<td align="left">SINT 或 UINT 與更少或等於位元</td>
<td align="left"><p>若要從 SINT 或 UINT 轉換成 SINT 或 UINT 與更少或等於位元 (和/或正負號變更)，只會將開始值限制為目標格式的範圍。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanfixed-point-integer-conversion"></a><span id="Fixed_Point_Integer_Conversion"></span><span id="fixed_point_integer_conversion"></span><span id="FIXED_POINT_INTEGER_CONVERSION"></span>固定點整數轉換


固定點整數單純是某個位元大小的整數，其中某個固定位置有隱含的小數點。

普遍的「整數」資料類型是固定點整數的特殊案例，小數點位於數字結尾。

固定點數字表示法描述如下：i.f，其中 i 是整數位元數，f 是分數位元數。 例如 16.8 表示 16 位元整數，後面接著 8 位元分數。 整數部分是以 2 的補數儲存，至少如此處鎖定一 (雖然它同樣可以對不帶正負號的整數定義)。 分數部分是以不帶正負號的形式儲存。 分數部分一律代表正分數，介於最接近的兩個整數值之間，從最大負數開始。

固定點數字的加減運算是單純使用標準整數算術執行，不考慮隱含的小數點位置。 將 16.8 固定點數字加 1，僅表示加上 256，因為小數點在數字最低有效結尾起算的第 8 位。 其他運算像是乘法，同樣可以單純使用整數算術執行，假設考慮對固定小數點的效果。 例如，使用整數乘法將兩個 16.8 整數相乘，會得到 32.16 的結果。

固定點整數表示法在 Direct3D 中使用的方式有兩種。

-   點陣化中後續剪裁的頂點位置會貼齊固定點，以便跨 RenderTarget 區域統一分配精確度。 許多點陣化運算 (例如面消除) 是在固定點貼齊位置上發生，其他運算 (像是屬性 Interpolator 設定) 則使用已從固定點貼齊位置轉換回浮點數的位置。
-   取樣作業的紋理座標會貼齊固定點 (經過紋理大小縮放後)，以跨紋理空間統一分配精確度，以便選擇篩選點選位置/權重。 權重值會在進行實際篩選算術之前轉換回浮點數。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">來源資料類型</th>
<th align="left">目的地資料類型</th>
<th align="left">轉換規則</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">FLOAT</td>
<td align="left">固定點整數</td>
<td align="left"><p>以下是將浮點數 n 轉換成固定點整數 i.f 的一般程序，其中 i 是 (帶正負號的) 整數位元數，f 是分數位元數。</p>
<ul>
<li>計算 FixedMin = -2⁽ⁱ⁻¹⁾</li>
<li>計算 FixedMax = 2⁽ⁱ⁻¹⁾ - 2<sup>(-f)</sup></li>
<li>如果 n 是 NaN，結果 = 0；如果 n 是 +Inf，結果 = FixedMax*2<sup>f</sup>；如果 n 是 -Inf，結果 = FixedMin*2<sup>f</sup></li>
<li>如果 n &gt;= FixedMax，結果 = Fixedmax*2<sup>f</sup>；如果 n &lt;= FixedMin，結果 = FixedMin*2<sup>f</sup></li>
<li>否則計算 n*2<sup>f</sup> 並轉換成整數。</li>
</ul>
<p>實作在整數結果中容許 D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place 容錯，而非上方最後一個步驟之後的無限精確值 n*2<sup>f</sup>。</p></td>
</tr>
<tr class="even">
<td align="left">固定點整數</td>
<td align="left">FLOAT</td>
<td align="left"><p>假設要轉換成浮點數的特定固定點表示法未包含總計超過 24 個位元的資訊，則分數元件中不會超過其中的 23 個位元。 假設某個固定點數字 fxp 採用 i.f 形式 (i 位元整數，f 位元分數)。 轉換成浮點數類似下列虛擬程式碼。</p>
<p>浮點數結果 = (float) (fxp &gt; &gt; f) + / / 取整數</p>
((float) (fxp &amp; (2 個<sup>f</sup> - 1)) / (2 個<sup>f</sup>));取分數</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[附錄](appendix.md)

 

 




