---
author: mtoepke
title: GLSL-to-HLSL 參考
description: 當您將圖形架構從 OpenGL ES 2.0 移植到 Direct3D 11 以建立通用 Windows 平台 (UWP) 遊戲時，也必須將 OpenGL 著色器語言 (GLSL) 程式碼移植到 Microsoft 高階著色器語言 (HLSL) 程式碼。
ms.assetid: 979d19f6-ef0c-64e4-89c2-a31e1c7b7692
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, GLSL, HLSL, OpenGL, DirectX, 著色器
ms.localizationpriority: medium
ms.openlocfilehash: 30c925f9ebb07d578147dfba373fdeb3baa364fe
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2018
ms.locfileid: "7160678"
---
# <a name="glsl-to-hlsl-reference"></a>GLSL-to-HLSL 參考



當您[將圖形架構從 OpenGL ES 2.0 移植到 Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md) 以建立通用 Windows 平台 (UWP) 遊戲時，也必須將 OpenGL 著色器語言 (GLSL) 程式碼移植到 Microsoft 高階著色器語言 (HLSL) 程式碼。 這裡所指的 GLSL 是與 OpenGL ES 2.0 相容；HLSL 則與 Direct3D 11 相容。 如需 Direct3D 11 與舊版 Direct3D 之間差異的相關資訊，請參閱[功能對應](feature-mapping.md)。

-   [比較 OpenGL ES 2.0 與 Direct3D 11](#comparing-opengl-es-20-with-direct3d-11)
-   [將 GLSL 變數移植到 HLSL](#porting-glsl-variables-to-hlsl)
-   [將 GLSL 類型移植到 HLSL](#porting-glsl-types-to-hlsl)
-   [將 GLSL 預先定義的全域變數移植到 HLSL](#porting-glsl-pre-defined-global-variables-to-hlsl)
-   [將 GLSL 變數移植到 HLSL 的範例](#examples-of-porting-glsl-variables-to-hlsl)
    -   [GLSL 中的 uniform、attribute 與 varying](#uniform-attribute-and-varying-in-glsl)
    -   [HLSL 中的常數緩衝區與資料傳輸](#constant-buffers-and-data-transfers-in-hlsl)
-   [將 OpenGL 轉譯程式碼移植到 Direct3D 的範例](#examples-of-porting-opengl-rendering-code-to-direct3d)
-   [相關主題](#related-topics)

## <a name="comparing-opengl-es-20-with-direct3d-11"></a>比較 OpenGL ES 2.0 與 Direct3D 11


OpenGL ES 2.0 與 Direct3D 11 有許多相似處。 它們都有類似的轉譯管線與圖形功能。 但 Direct3D 11 是轉譯實作與 API，並非規格；OpenGL ES 2.0 則是轉譯規格與 API，而非實作方式。 一般來說，Direct3D 11 與 OpenGL ES 2.0 的差異如下：

| OpenGL ES 2.0                                                                                         | Direct3D 11                                                                                                            |
|-------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| 硬體與作業系統無從驗證的規格與廠商提供的實作             | Microsoft 的硬體抽象實作與 Windows 平台上的憑證                                |
| 抽象的硬體多樣性；執行階段管理多數資源                                     | 直接存取硬體配置；應用程式可管理資源並進行處理                                              |
| 透過協力廠商程式庫 (例如，簡易直接媒體層 (SDL)) 提供較高階的模組 | 在較低階的模組上建置 Direct2D 這類較高階的模組，簡化 Windows 應用程式的開發             |
| 透過擴充功能展現硬體廠商差異性                                                         | Microsoft 以一般方式將選用功能新增至 API，因此不會專屬於特定硬體廠商 |

 

一般來說，GLSL 與 HLSL 的差異如下：

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL</th>
<th align="left">HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">程序性的，注重步驟 (類似 C)</td>
<td align="left">以物件為導向，注重資料 (類似 C++)</td>
</tr>
<tr class="even">
<td align="left">著色器編譯整合至圖形 API</td>
<td align="left">在 Direct3D 將著色器傳遞到驅動程式之前，HLSL 編譯器會先將<a href="https://msdn.microsoft.com/library/windows/desktop/bb509633">著色器編譯</a>至中繼的二進位表示法。
<div class="alert">
<strong>注意：</strong>此二進位表示法與硬體無關。 通常會在 app 建置時進行編譯，而不是在 app 執行時進行編譯。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><a href="#porting-glsl-variables-to-hlsl">Variable</a> 儲存區修飾詞</td>
<td align="left">透過輸入配置宣告傳輸常數緩衝區與資料</td>
</tr>
<tr class="even">
<td align="left"><p><a href="#porting-glsl-types-to-hlsl">類型</a></p>
<p>典型的向量類型：vec2/3/4</p>
<p>lowp、mediump、highp</p></td>
<td align="left"><p>典型的向量類型：float2/3/4</p>
<p>min10float、min16float</p></td>
</tr>
<tr class="odd">
<td align="left">texture2D [Function]</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/bb509695">texture.Sample</a> [datatype.Function]</td>
</tr>
<tr class="even">
<td align="left">sampler2D [datatype]</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471525">Texture2D</a> [datatype]</td>
</tr>
<tr class="odd">
<td align="left">以列為主的矩陣 (預設值)</td>
<td align="left">以欄為主的矩陣 (預設值)
<div class="alert">
<strong>注意：</strong> 使用<strong>row_major</strong>型別修飾詞來變更單一變數的配置。 如需詳細資訊，請參閱<a href="https://msdn.microsoft.com/library/windows/desktop/bb509706">變數語法</a>。 您也可指定編譯器旗標或 pragma 來變更全域預設值。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">片段著色器</td>
<td align="left">像素著色器</td>
</tr>
</tbody>
</table>

 

> **注意：** HLSL 的紋理與取樣器為兩個不同的物件。 在 GLSL (如 Direct3D 9) 中，紋理繫結為取樣器狀態的一部分。

 

在 GLSL 中，您將大部分的 OpenGL 狀態呈現為預先定義的全域變數。 例如，使用 GLSL 時，您使用 **gl\_Position** 變數指定頂點位置、使用 **gl\_FragColor** 變數指定片段色彩。 在 HLSL 中，您明確將 Direct3D 狀態從 app 程式碼傳遞至著色器。 例如，使用 Direct3D 與 HLSL 時，頂點著色器的輸入必須符合頂點緩衝區中的資料格式，而應用程式程式碼中的常數緩衝區結構必須符合著色器程式碼中的常數緩衝區 ([cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581)) 結構。

## <a name="porting-glsl-variables-to-hlsl"></a>將 GLSL 變數移植到 HLSL


在 GLSL 中，您將修飾詞 (限定詞) 套用到全域著色器變數宣告，讓變數在您的著色器中擁有特定的行為。 在 HLSL 則不需要這些修飾詞，因為您使用傳遞至著色器並從著色器所傳回的引數來定義著色器流程。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL 變數行為</th>
<th align="left">HLSL 對等項目</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>uniform</strong></p>
<p>您將 uniform 變數從 app 程式碼傳遞至頂點與片段著色器內，或是其中之一。 使用這些著色器繪製任何三角形之前，您必須設定所有 uniform 的值，這樣在繪製三角形網格的整個過程中，它們的值才能維持相同。 這些值是 uniform。 有些 uniform 是針對整個框架所設定，而其他則是特別針對某一組特殊頂點像素著色器而設定。</p>
<p>uniform 變數是基於多邊形的變數。</p></td>
<td align="left"><p>使用常數緩衝區。</p>
<p>請參閱<a href="https://msdn.microsoft.com/library/windows/desktop/ff476896">使用方法：建立常數緩衝區</a>與<a href="https://msdn.microsoft.com/library/windows/desktop/bb509581">著色器常數</a>。</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>varying</strong></p>
<p>您在頂點著色器內初始化 varying 變數，並將它傳遞至片段著色器中擁有相同名稱的 varying 變數。 因為頂點著色器只設定每個頂點的 varying 變數值，因此點陣化會插補這些值 (以透視修正方式) 來產生每個片段值，以傳遞至片段著色器。 這些變數在每個三角形都不同。</p></td>
<td align="left">使用從頂點著色器傳回的結構做為像素著色器的輸入。 請確定語意值相符。</td>
</tr>
<tr class="odd">
<td align="left"><p><strong>attribute</strong></p>
<p>attribute 是您單獨從 app 程式碼傳遞至頂點著色器之頂點描述的一部分。 與 uniform 不同，您要為每個頂點設定各個 attribute 的值，然後每個頂點便有不同的值。 Attribute 變數是基於頂點的變數。</p></td>
<td align="left"><p>在 Direct3D 應用程式程式碼中定義頂點緩衝區，並使它與頂點著色器中定義的頂點輸入相符。 您可選擇是否定義索引緩衝區。 請參閱<a href="https://msdn.microsoft.com/library/windows/desktop/ff476899">使用方法：建立頂點緩衝區</a>與<a href="https://msdn.microsoft.com/library/windows/desktop/ff476897">使用方法：建立索引緩衝區</a>。</p>
<p>在 Direct3D 應用程式程式碼中建立輸入配置，並使語意值與頂點輸入中的語意值相符。 請參閱<a href="https://msdn.microsoft.com/library/windows/desktop/bb205117#Create_the_Input_Layout">建立輸入配置</a>。</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>const</strong></p>
<p>編譯至著色器且永不會變更的常數。</p></td>
<td align="left">請使用 <strong>static const</strong>。 <strong>static</strong> 表示此值未針對常數緩衝區公開，<strong>const</strong> 表示著色器無法變更此值。 因此，根據值的初始設定式，會於編譯時識別它。</td>
</tr>
</tbody>
</table>

 

在 GLSL，沒有修飾詞的變數只是每個著色器私有的一般全域變數。

當您將資料傳遞至紋理 (HLSL 中的 [Texture2D](https://msdn.microsoft.com/library/windows/desktop/ff471525)) 以及其關聯的取樣器 (HLSL 中的 [SamplerState](https://msdn.microsoft.com/library/windows/desktop/bb509644)) 時，通常會將它們宣告為像素著色器中的全域變數。

## <a name="porting-glsl-types-to-hlsl"></a>將 GLSL 類型移植到 HLSL


使用此表格將 GLSL 類型移植到 HLSL。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL 類型</th>
<th align="left">HLSL 類型</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">純量類型：float、int、bool</td>
<td align="left"><p>純量類型：float、int、bool</p>
<p>還有 uint、double</p>
<p>如需詳細資訊，請參閱<a href="https://msdn.microsoft.com/library/windows/desktop/bb509646">純量類型</a>。</p></td>
</tr>
<tr class="even">
<td align="left"><p>向量類型</p>
<ul>
<li>浮點向量：vec2、vec3、vec4</li>
<li>布林值向量：bvec2、bvec3、bvec4</li>
<li>帶正負號的整數向量：ivec2、ivec3、ivec4</li>
</ul></td>
<td align="left"><p>向量類型</p>
<ul>
<li>float2、float3、float4 與 float1</li>
<li>bool2、bool3、bool4 與 bool1</li>
<li>int2、int3、int4 與 int1</li>
<li><p>這些類型也有類似於 float、bool 與 int 的向量擴充：</p>
<ul>
<li>uint</li>
<li>min10float、min16float</li>
<li>min12int、min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>如需詳細資訊，請參閱<a href="https://msdn.microsoft.com/library/windows/desktop/bb509707">向量類型</a>與<a href="https://msdn.microsoft.com/library/windows/desktop/bb509568">關鍵字</a>。</p>
<p>向量也是定義為 float4 (typedef vector &lt;float, 4&gt; vector;) 的類型。 如需詳細資訊，請參閱<a href="https://msdn.microsoft.com/library/windows/desktop/bb509702">使用者定義的類型</a>。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>矩陣類型</p>
<ul>
<li>mat2：2x2 浮點數矩陣</li>
<li>mat3：3x3 浮點數矩陣</li>
<li>mat4：4x4 浮點數矩陣</li>
</ul></td>
<td align="left"><p>矩陣類型</p>
<ul>
<li>float2x2</li>
<li>float3x3</li>
<li>float4x4</li>
<li>還有 float1x1、float1x2、float1x3、float1x4、float2x1、float2x3、float2x4、float3x1、float3x2、float3x4、float4x1、float4x2、float4x3</li>
<li><p>這些類型也有類似於 float 的矩陣擴充：</p>
<ul>
<li>int、uint、bool</li>
<li>min10float、min16float</li>
<li>min12int、min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>您也可使用<a href="https://msdn.microsoft.com/library/windows/desktop/bb509623">矩陣類型</a>來定義矩陣。</p>
<p>例如，matrix &lt;float, 2, 2&gt; fMatrix = {0.0f, 0.1, 2.1f, 2.2f};</p>
<p>矩陣也是定義為 float4x4 (typedef matrix &lt;float, 4, 4&gt; matrix;) 的類型。 如需詳細資訊，請參閱<a href="https://msdn.microsoft.com/library/windows/desktop/bb509702">使用者定義的類型</a>。</p></td>
</tr>
<tr class="even">
<td align="left"><p>適用於 float、int、取樣器的精確度限定詞</p>
<ul>
<li><p>highp</p>
<p>此限定詞提供的最小精確度需求大於 min16float 所提供的需求，且小於完整 32 位元浮點數。 HLSL 中的對等項目為：</p>
<p>highp float -&gt; float</p>
<p>highp int -&gt; int</p></li>
<li><p>mediump</p>
<p>套用到 float 與 int 的這個限定詞相當於 HLSL 中的 min16float 與 min12int。 最小 10 位元的 mantissa，不像 min10float。</p></li>
<li><p>lowp</p>
<p>套用至 float 的這個限定詞提供範圍從 -2 到 2 的浮點。 相當於 HLSL 中的 min10float。</p></li>
</ul></td>
<td align="left"><p>精確度類型</p>
<ul>
<li>min16float：最小 16 位元浮點值</li>
<li><p>min10float</p>
<p>最小固定點帶正負號 2.8 位元值 (2 位元的整數與 8 位元的分數元件)。 8 位元的分數元件可包含 1 而不排除，讓範圍完整包含從 -2 到 2。</p></li>
<li>min16int：最小 16 位元帶正負號的整數</li>
<li><p>min12int：最小 12 位元帶正負號的整數</p>
<p>此類型適用於 10Level9 (<a href="https://msdn.microsoft.com/library/windows/desktop/ff476876">9_x 功能層級</a>)，其中整數由浮點數表示。 這會是您在使用 16 位元浮點數列舉整數時所得到的精確度。</p></li>
<li>min16uint：最小 16 位元不帶正負號的整數</li>
</ul>
<p>如需詳細資訊，請參閱<a href="https://msdn.microsoft.com/library/windows/desktop/bb509646">純量類型</a>與<a href="https://msdn.microsoft.com/library/windows/desktop/hh968108">使用 HLSL 最小精確度</a>。</p></td>
</tr>
<tr class="odd">
<td align="left">sampler2D</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471525">Texture2D</a></td>
</tr>
<tr class="even">
<td align="left">samplerCube</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/bb509700">TextureCube</a></td>
</tr>
</tbody>
</table>

 

## <a name="porting-glsl-pre-defined-global-variables-to-hlsl"></a>將 GLSL 預先定義的全域變數移植到 HLSL


使用此表格將 GLSL 預先定義的全域變數移植到 HLSL。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL 預先定義的全域變數</th>
<th align="left">HLSL 語意</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>gl_Position</strong></p>
<p>此變數為 <strong>vec4</strong> 類型。</p>
<p>頂點位置</p>
<p>例如 - gl_Position = position;</p></td>
<td align="left"><p>SV_Position</p>
<p>Direct3D 9 中的 POSITION</p>
<p>此語意為 <strong>float4</strong> 類型。</p>
<p>頂點著色器輸出</p>
<p>頂點位置</p>
<p>例如 - float4 vPosition : SV_Position;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_PointSize</strong></p>
<p>此變數為 <strong>float</strong> 類型。</p>
<p>點大小</p></td>
<td align="left"><p>PSIZE</p>
<p>除非目標設為 Direct3D 9，否則無意義</p>
<p>此語意為 <strong>float</strong> 類型。</p>
<p>頂點著色器輸出</p>
<p>點大小</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragColor</strong></p>
<p>此變數為 <strong>vec4</strong> 類型。</p>
<p>片段色彩</p>
<p>例如 - gl_FragColor = vec4(colorVarying, 1.0);</p></td>
<td align="left"><p>SV_Target</p>
<p>Direct3D 9 中的 COLOR</p>
<p>此語意為 <strong>float4</strong> 類型。</p>
<p>像素著色器輸出</p>
<p>像素色彩</p>
<p>例如 - float4 Color[4] : SV_Target;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragData[n]</strong></p>
<p>此變數為 <strong>vec4</strong> 類型。</p>
<p>色彩附件 n 的片段色彩</p></td>
<td align="left"><p>SV_Target[n]</p>
<p>此語意為 <strong>float4</strong> 類型。</p>
<p>儲存在 n 轉譯目標的像素著色器輸出值，其中 0 &lt;= n &lt;= 7。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragCoord</strong></p>
<p>此變數為 <strong>vec4</strong> 類型。</p>
<p>框架緩衝區內的片段位置</p></td>
<td align="left"><p>SV_Position</p>
<p>Direct3D 9 中無法使用</p>
<p>此語意為 <strong>float4</strong> 類型。</p>
<p>像素著色器輸入</p>
<p>螢幕空間座標</p>
<p>例如 - float4 screenSpace : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FrontFacing</strong></p>
<p>此變數為 <strong>bool</strong> 類型。</p>
<p>決定片段是否屬於面向前方的基本型別。</p></td>
<td align="left"><p>SV_IsFrontFace</p>
<p>Direct3D 9 中的 VFACE</p>
<p>SV_IsFrontFace 為 <strong>bool</strong> 類型。</p>
<p>VFACE 為 <strong>float</strong> 類型。</p>
<p>像素著色器輸入</p>
<p>基本型別面向</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_PointCoord</strong></p>
<p>此變數為 <strong>vec2</strong> 類型。</p>
<p>點內的片段位置 (僅限點陣化)</p></td>
<td align="left"><p>SV_Position</p>
<p>Direct3D 9 中的 VPOS</p>
<p>SV_Position 為 <strong>float4</strong> 類型。</p>
<p>VPOS 為 <strong>float2</strong> 類型。</p>
<p>像素著色器輸入</p>
<p>螢幕空間中的像素或範本位置</p>
<p>例如 - float4 pos : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragDepth</strong></p>
<p>此變數為 <strong>float</strong> 類型。</p>
<p>深度緩衝區資料</p></td>
<td align="left"><p>SV_Depth</p>
<p>Direct3D 9 中的 DEPTH</p>
<p>SV_Depth 為 <strong>float</strong> 類型。</p>
<p>像素著色器輸出</p>
<p>深度緩衝區資料</p></td>
</tr>
</tbody>
</table>

 

您可使用語意來指定頂點著色器輸入與像素著色器輸入的位置、色彩等等。 您必須使輸入配置中的語意值與頂點著色器輸入相符。 如需範例，請參閱[將 GLSL 變數移植到 HLSL 的範例](#examples-of-porting-glsl-variables-to-hlsl)。 如需 HLSL 語意的詳細資訊，請參閱[語意](https://msdn.microsoft.com/library/windows/desktop/bb509647)。

## <a name="examples-of-porting-glsl-variables-to-hlsl"></a>將 GLSL 變數移植到 HLSL 的範例


這裡我們示範在 OpenGL/GLSL 程式碼中使用 GLSL 變數的範例，以及在 Direct3D/HLSL 程式碼中的相等範例。

### <a name="uniform-attribute-and-varying-in-glsl"></a>GLSL 中的 uniform、attribute 與 varying

OpenGL 應用程式程式碼

``` syntax
// Uniform values can be set in app code and then processed in the shader code.
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

// Incoming position of vertex
attribute vec4 position;
 
// Incoming color for the vertex
attribute vec3 color;
 
// The varying variable tells the shader pipeline to pass it  
// on to the fragment shader.
varying vec3 colorVarying;
```

GLSL 頂點著色器程式碼

``` syntax
//The shader entry point is the main method.
void main()
{
colorVarying = color; //Use the varying variable to pass the color to the fragment shader
gl_Position = position; //Copy the position to the gl_Position pre-defined global variable
}
```

GLSL 片段著色器程式碼

``` syntax
void main()
{
//Pad the colorVarying vec3 with a 1.0 for alpha to create a vec4 color
//and assign that color to the gl_FragColor pre-defined global variable
//This color then becomes the fragment's color.
gl_FragColor = vec4(colorVarying, 1.0);
}
```

### <a name="constant-buffers-and-data-transfers-in-hlsl"></a>HLSL 中的常數緩衝區與資料傳輸

以下範例說明如何將資料傳遞至 HLSL 頂點著色器，然後流向像素著色器。 在您的應用程式程式碼中，定義頂點與常數緩衝區。 然後，在頂點著色器程式碼中，將常數緩衝區定義為 [cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581)，並儲存每一頂點資料與像素著色器輸入資料。 這裡我們使用名為 **VertexShaderInput** 與 **PixelShaderInput** 的結構。

Direct3D app 程式碼

```cpp
struct ConstantBuffer
{
    XMFLOAT4X4 model;
    XMFLOAT4X4 view;
    XMFLOAT4X4 projection;
};
struct SimpleCubeVertex
{
    XMFLOAT3 pos;   // position
    XMFLOAT3 color; // color
};

 // Create an input layout that matches the layout defined in the vertex shader code.
 const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
 {
     { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
     { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
 };

// Create vertex and index buffers that define a geometry.
```

HLSL 頂點著色器程式碼

``` syntax
cbuffer ModelViewProjectionCB : register( b0 )
{
    matrix model; 
    matrix view;
    matrix projection;
};
// The POSITION and COLOR semantics must match the semantics in the input layout Direct3D app code.
struct VertexShaderInput
{
    float3 pos : POSITION; // Incoming position of vertex 
    float3 color : COLOR; // Incoming color for the vertex
};

struct PixelShaderInput
{
    float4 pos : SV_Position; // Copy the vertex position.
    float4 color : COLOR; // Pass the color to the pixel shader.
};

PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // shader source code

    return vertexShaderOutput;
}
```

HLSL 像素著色器程式碼

``` syntax
// Collect input from the vertex shader. 
// The COLOR semantic must match the semantic in the vertex shader code.
struct PixelShaderInput
{
    float4 pos : SV_Position;
    float4 color : COLOR; // Color for the pixel
};

// Set the pixel color value for the renter target. 
float4 main(PixelShaderInput input) : SV_Target
{
    return input.color;
}
```

## <a name="examples-of-porting-opengl-rendering-code-to-direct3d"></a>將 OpenGL 轉譯程式碼移植到 Direct3D 的範例


這裡我們示範在 OpenGL ES 2.0 程式碼中轉譯的範例，以及在 Direct3D 11 程式碼中的相等範例。

OpenGL 轉譯程式碼

``` syntax
// Bind shaders to the pipeline. 
// Both vertex shader and fragment shader are in a program.
glUseProgram(m_shader->getProgram());
 
// Input asssembly 
// Get the position and color attributes of the vertex.

m_positionLocation = glGetAttribLocation(m_shader->getProgram(), "position");
glEnableVertexAttribArray(m_positionLocation);

m_colorLocation = glGetAttribColor(m_shader->getProgram(), "color");
glEnableVertexAttribArray(m_colorLocation);
 
// Bind the vertex buffer object to the input assembler.
glBindBuffer(GL_ARRAY_BUFFER, m_geometryBuffer);
glVertexAttribPointer(m_positionLocation, 4, GL_FLOAT, GL_FALSE, 0, NULL);
glBindBuffer(GL_ARRAY_BUFFER, m_colorBuffer);
glVertexAttribPointer(m_colorLocation, 3, GL_FLOAT, GL_FALSE, 0, NULL);
 
// Draw a triangle with 3 vertices.
glDrawArray(GL_TRIANGLES, 0, 3);
```

Direct3D 轉譯程式碼

```cpp
// Bind the vertex shader and pixel shader to the pipeline.
m_d3dDeviceContext->VSSetShader(vertexShader.Get(),nullptr,0);
m_d3dDeviceContext->PSSetShader(pixelShader.Get(),nullptr,0);
 
// Declare the inputs that the shaders expect.
m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());
m_d3dDeviceContext->IASetVertexBuffers(0, 1, vertexBuffer.GetAddressOf(), &stride, &offset);

// Set the primitive's topology.
m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

// Draw a triangle with 3 vertices. triangleVertices is an array of 3 vertices.
m_d3dDeviceContext->Draw(ARRAYSIZE(triangleVertices),0);
```

## <a name="related-topics"></a>相關主題


* [從 OpenGL ES 2.0 移植到 Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md)

 

 




