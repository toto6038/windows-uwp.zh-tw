---
title: "圖形管線"
description: "Direct3D 圖形管線專為即時遊戲應用程式產生圖形而設計。 透過每個可設定或可編程階段，從輸入到輸出的資料流。"
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- "圖形管線"
- "管線階段"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: af583deb51b93bffc05c4371466fa8412bb0476d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="graphics-pipeline"></a>圖形管線


Direct3D 圖形管線專為即時遊戲應用程式產生圖形而設計。 透過每個可設定或可編程階段，從輸入到輸出的資料流。

所有階段都可以使用 Direct3D API 設定。 搭配常見著色器核心的階段 (圓角矩形區塊) 可使用 [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561) 程式語言進行程式設計。 如此可讓管線極具彈性和適應能力。

最常使用的是頂點著色器 (VS) 階段和像素著色器 (PS) 階段。 如果您連這些著色器階段都未提供，則會使用預設的沒有選項、傳遞頂點和像素著色器。

![Direct3D 11 可程式化管線中的資料流程圖](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>輸入組合語言階段

 

| | |
|-|-|
|用途|[輸入組合語言 (IA) 階段](input-assembler-stage--ia-.md)提供基本類型和管線相鄰資料，例如三角形、行及點，包括語意識別碼，藉由減少處理尚未處理的基本類型來讓著色更有效率。|
|輸入|基本類型資料 (三角形、線條和/或點)，來自記憶體中使用者填入的緩衝區。 以及可能相鄰的資料。 三角形的每個三角形可能有 3 個頂點，而每個三角形的相鄰資料可能有 3 個頂點。|
|輸出|基本類型含有附加系統產生值 (例如基本類型識別碼、執行個體識別碼或頂點識別碼)。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Input Assembler (IA) stage](input-assembler-stage--ia-.md) supplies primitive and adjacency data to the pipeline, such as triangles, lines and points, including semantics IDs to help make shaders more efficient by reducing processing to primitives that haven't already been processed.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"> Primitive data (triangles, lines, and/or points), from user-filled buffers in memory. And possibly adjacency data. A triangle would be 3 vertices for each triangle and possibly 3 vertices for adjacency data per triangle.  </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Primitives with attached system-generated values (such as a primitive ID, an instance ID, or a vertex ID). </td>
</tr>
</tbody>
</table>
 -->



## <a name="vertex-shader-stage"></a>頂點著色器階段
 

| | |
|-|-|
|用途|[頂點著色器 (VS) 階段](vertex-shader-stage--vs-.md)處理頂點，通常執行轉換、貼圖與光線等作業。 頂點著色器採用單一輸入頂點，並產生單一輸出頂點。 個別的每個頂點作業，例如轉換、貼圖、變形及每個頂點光線。|
|輸入|單一頂點，有 VertexID 和 InstanceID 系統產生值。 每個頂點著色器輸入頂點可包含最多 16 個 32 位元向量（每個向量最多 4 個元件）。|
|輸出|單一頂點。 每個輸出頂點可包含同樣多的 16 個 32 位元 4 元件向量。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Vertex Shader (VS) stage](vertex-shader-stage--vs-.md) processes vertices, typically performing operations such as transformations, skinning, and lighting. A vertex shader takes a single input vertex and produces a single output vertex. Individual per-vertex operations, such as:
<ul>
<li>Transformations</li>
<li>Skinning</li>
<li>Morphing</li>
<li>Per-vertex lighting</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">A single vertex, with VertexID and InstanceID system-generated values. Each vertex shader input vertex can be comprised of up to 16 32-bit vectors (up to 4 components each).</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">A single vertex. Each output vertex can be comprised of as many as 16 32-bit 4-component vectors.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="hull-shader-stage"></a>輪廓著色器階段
 

| | |
|-|-|
|用途|[輪廓著色器 (HS) 階段](hull-shader-stage--hs-.md)是其中一個鑲嵌階段，有效率地將模型的單一表面分成多個三角形。 輪廓著色器會針對每個塊面叫用一次，並且將定義低位表面的輸入控制點轉換成構成塊面的控制點。 它也會進行一些每個塊面計算，以提供資料給曲面細分器 (TS) 階段和網域著色器 (DS) 階段。|
|輸入|介於 1 到 32 輸入控制點之間，共同定義低位表面。|
|輸出|介於 1 到 32 輸出控制點之間，共同構成塊面。 輪廓著色器宣告曲面細分器 (TS) 階段的狀態，包括控制點的數目、塊面的類型，以及鑲嵌時要使用的分割類型。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Hull Shader (HS) stage](hull-shader-stage--hs-.md) is one of the tessellation stages, which efficiently break up a single surface of a model into many triangles. A hull shader is invoked once per patch, and it transforms input control points that define a low-order surface into control points that make up a patch. It also does some per-patch calculations to provide data for the Tessellator (TS) stage and the Domain Shader (DS) stage.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Between 1 and 32 input control points, which together define a low-order surface. </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Between 1 and 32 output control points, which together make up a patch. The hull shader declares the state for the Tessellator (TS) stage, including the number of control points, the type of patch face, and the type of partitioning to use when tessellating. </td>
</tr>
</tbody>
</table>
-->

## <a name="tessellator-stage"></a>曲面細分器階段
 

| | |
|-|-|
|用途|[曲面細分器 (TS) 階段](tessellator-stage--ts-.md)建立代表幾何塊面的網域取樣模式，並產生一組連接這些樣本的小物件 (三角形、點或線)。|
|輸入|使用從輪廓著色器階段傳入的鑲嵌因數 (這指定網域被鑲嵌的細微程度) 和分割類型 (指定用於切割塊面的演算法)，曲面細分器每個塊面運作一次。 |
|輸出|曲面細分器輸出 uv (和選擇性 w) 座標及表面拓撲至網域著色器階段。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Tessellator (TS) stage](tessellator-stage--ts-.md) creates a sampling pattern of the domain that represents the geometry patch and generates a set of smaller objects (triangles, points, or lines) that connect these samples.   </td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"> The tessellator operates once per patch using the tessellation factors (which specify how finely the domain will be tessellated) and the type of partitioning (which specifies the algorithm used to slice up a patch) that are passed in from the hull-shader stage. </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The tessellator outputs uv (and optionally w) coordinates and the surface topology to the domain-shader stage.     </td>
</tr>
</tbody>
</table>
 -->

## <a name="domain-shader-stage"></a>網域著色器階段
 

| | |
|-|-|
|用途|[網域著色器 (DS) 階段](domain-shader-stage--ds-.md)會計算輸出塊面中細分點的頂點位置；它會計算對應每一個網域取樣的頂點位置。 網域著色器會在每個曲面細分器階段輸出點執行一次，並且對輪廓著色器輸出塊面和輸出塊面常數，以及曲面細分器階段輸出 UV 座標具有唯讀存取權。|
|輸入|網域著色器會耗用來自[輪廓著色器 (HS) 階段](hull-shader-stage--hs-.md)的輸出控制點。 輪廓著色器輸出包括：控制點、塊面常數資料及鑲嵌係數 (鑲嵌係數可能包括固定函式曲面細分器使用的值，以及整數鑲嵌進位之前的原始值，可簡化地理變形)。 網域著色器會對來自[曲面細分器 (TS) 階段](tessellator-stage--ts-.md)的每個輸出座標叫用一次。|
|輸出|網域著色器 (DS) 階段會將輸出塊面中細分點的頂點位置輸出。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Domain Shader (DS) stage](domain-shader-stage--ds-.md) calculates the vertex position of a subdivided point in the output patch; it calculates the vertex position that corresponds to each domain sample. A domain shader is run once per tessellator stage output point and has read-only access to the hull shader output patch and output patch constants, and the tessellator stage output UV coordinates.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><ul>
<li>A domain shader consumes output control points from the [Hull Shader (HS) stage](hull-shader-stage--hs-.md). The hull shader outputs include:
<ul>
<li>Control points.</li>
<li>Patch constant data.</li>
<li>Tessellation factors. The tessellation factors can include the values used by the fixed-function tessellator as well as the raw values (before rounding by integer tessellation, for example), which facilitates geomorphing, for example.</li>
</ul></li>
<li>A domain shader is invoked once per output coordinate from the [Tessellator (TS) stage](tessellator-stage--ts-.md).</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Domain Shader (DS) stage outputs the vertex position of a subdivided point in the output patch.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="geometry-shader-stage"></a>幾何著色器階段
 

| | |
|-|-|
|用途|[幾何著色器 (GS) 階段](geometry-shader-stage--gs-.md)會處理整個基本類型：三角形、線條和點，以及它們相鄰的頂點。 它支援幾何放大和取消放大。 它對於演算法來說很實用，包括 Point Sprite Expansion、Dynamic Particle Systems、Fur/Fin Generation、Shadow Volume Generation、Single Pass Render-to-Cubemap、Per-Primitive Material Swapping 及 Per-Primitive Material Setup (包括產生質心座標做為基本類型資料，如此像素著色器就能執行自訂屬性插補)。 |
|輸入|與在單一頂點上操作的頂點著色器不同的是，幾何著色器的輸入是完整基本類型 (三角形的三個頂點、線條的兩個頂點，或點的單一頂點) 的頂點。|
|輸出|幾何著色器 (GS) 階段能夠輸出多個頂點，以形成選取的單一拓撲。 可用的幾何著色器輸出拓撲包括 <strong>tristrip</strong>、<strong>linestrip</strong> 和 <strong>pointlist</strong>。 發出的基本類型數目可在幾何著色器的任何叫用內自由改變，雖然可發出的頂點最大數目必須以靜態方式宣告。 從幾何著色器叫用發出的連環長度可以是任意的，而新的連環可以透過 [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL 功能建立。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Geometry Shader (GS) stage](geometry-shader-stage--gs-.md) processes entire primitives: triangles, lines, and points, along with their adjacent vertices. It is useful for algorithms including Point Sprite Expansion, Dynamic Particle Systems, and Shadow Volume Generation. It supports geometry amplification and de-amplification.
<ul>
<li>Point Sprite Expansion</li>
<li>Dynamic Particle Systems</li>
<li>Fur/Fin Generation</li>
<li>Shadow Volume Generation</li>
<li>Single Pass Render-to-Cubemap</li>
<li>Per-Primitive Material Swapping</li>
<li>Per-Primitive Material Setup, including generation of barycentric coordinates as primitive data so that a pixel shader can perform custom attribute interpolation.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike vertex shaders, which operate on a single vertex, the geometry shader's inputs are the vertices for a full primitive (three vertices for triangles, two vertices for lines, or single vertex for point).</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Geometry Shader (GS) stage is capable of outputting multiple vertices forming a single selected topology. Available geometry shader output topologies are <strong>tristrip</strong>, <strong>linestrip</strong>, and <strong>pointlist</strong>. The number of primitives emitted can vary freely within any invocation of the geometry shader, though the maximum number of vertices that could be emitted must be declared statically. Strip lengths emitted from a geometry shader invocation can be arbitrary, and new strips can be created via the [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL function.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="stream-output-stage"></a>資料流輸出階段
 

| | |
|-|-|
|用途|[資料流輸出 (SO) 階段](stream-output-stage--so-.md)會持續將頂點資料從上一個作用中階段輸出 (或串流) 到記憶體中的一或多個緩衝區。 串流輸出到記憶體的資料可重新循環回管線做為輸入資料，或從 CPU 讀回。|
|輸入|來自上一個管線階段的頂點資料。|
|輸出|資料流輸出 (SO) 階段會持續將頂點資料從上一個作用中階段 (例如幾何著色器 (GS) 階段) 輸出 (或串流) 到記憶體中的一或多個緩衝區。 如果幾何著色器 (GS) 階段非使用中，而資料流輸出 (SO) 階段為使用中，它會持續將頂點資料從網域著色器 (DS) 階段輸出到記憶體中的緩衝區 (如果 DS 也是非使用中，則從頂點著色器 (VS) 階段輸出)。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Stream Output (SO) stage](stream-output-stage--so-.md) continuously outputs (or streams) vertex data from the previous active stage to one or more buffers in memory. Data streamed out to memory can be recirculated back into the pipeline as input data, or read-back from the CPU.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Vertex data from a previous pipeline stage.   </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Stream Output (SO) stage continuously outputs (or streams) vertex data from the previous active stage, such as the Geometry Shader (GS) stage, to one or more buffers in memory. If the Geometry Shader (GS) stage is inactive, and the Stream Output (SO) stage is active, it continuously outputs vertex data from the Domain Shader (DS) stage to buffers in memory (or if the DS is also inactive, from the Vertex Shader (VS) stage).</td>
</tr>
</tbody>
</table>
-->

## <a name="rasterizer-stage"></a>點陣化階段
 

| | |
|-|-|
|用途|[點陣化 (RS) 階段](rasterizer-stage--rs-.md)會裁剪不在檢視中的基本類型、為像素著色器 (PS) 階段準備基本類型，以及決定如何叫用像素著色器。 轉換向量資訊 (由圖形或基本類型組成) 為點陣影像 (由像素組成)，以顯示即時 3D 圖形。|
|輸入|即將進入點陣化階段的頂點 (x,y,z,w) 假設為在同質的剪輯空間中。 在這個座標空間中，X 軸指向右，Y 指向上，Z 偏離相機。|
|輸出|需要轉譯的實際像素。 包括一些頂點屬性，用於像素著色器插補。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Rasterizer (RS) stage](rasterizer-stage--rs-.md) clips primitives that aren't in view, prepares primitives for the Pixel Shader (PS) stage, and determines how to invoke pixel shaders. Converts vector information (composed of shapes or primitives) into a raster image (composed of pixels) for the purpose of displaying real-time 3D graphics.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Vertices (x,y,z,w) coming into the Rasterizer stage are assumed to be in homogeneous clip-space. In this coordinate space the X axis points right, Y points up and Z points away from camera.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The actual pixels that need to be rendered. Includes some vertex attributes for use in interpolation by the pixel Shader.</td>
</tr>
</tbody>
</table>
 -->

## <a name="pixel-shader-stage"></a>像素著色器階段
 

| | |
|-|-|
|用途|[像素著色器 (PS) 階段](pixel-shader-stage--ps-.md)會接收基本類型的插補資料，並產生每一像素資料，如色彩。|
|輸入|若管線設定後沒有幾何著色器，像素著色器限於 16、32 位元、4 個元件的輸入。 否則，像素著色器最多可以採用 32、32 位元、4 個元件的輸入。 像素著色器輸入資料包括頂點屬性 (可插補，無論是否有透視修正)，也可以視為每個基本類型常數。 像素著色器輸入會從點陣化的基本類型的頂點屬性插補，根據宣告的插補模式。 如果在點陣化之前基本類型被裁剪，則插補模式也會在裁剪過程中採用。 |
|輸出|像素著色器可以輸出最多 8、32 位元、4 個元件的色彩，或無色彩 (如果捨棄像素)。 像素著色器輸出暫存器元件必須先宣告，才能使用；每個暫存器可以有不同的輸出寫入遮罩。|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Pixel Shader (PS) stage](pixel-shader-stage--ps-.md) receives interpolated data for a primitive and generates per-pixel data such as color.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><p>When the pipeline is configured without a geometry shader, a pixel shader is limited to 16, 32-bit, 4-component inputs. Otherwise, a pixel shader can take up to 32, 32-bit, 4-component inputs.</p>
<p>Pixel shader input data includes vertex attributes (that can be interpolated with or without perspective correction) or can be treated as per-primitive constants. Pixel shader inputs are interpolated from the vertex attributes of the primitive being rasterized, based on the interpolation mode declared. If a primitive gets clipped before rasterization, the interpolation mode is honored during the clipping process as well.</p></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left"><p>A pixel shader can output up to 8, 32-bit, 4-component colors, or no color if the pixel is discarded. Pixel shader output register components must be declared before they can be used; each register is allowed a distinct output-write mask.</p></td>
</tr>
</tbody>
</table>
-->
 

## <a name="output-merger-stage"></a>輸出合併階段
 

| | |
|-|-|
|用途|[輸出合併 (OM) 階段](output-merger-stage--om-.md)將不同類型的輸出資料 (像素著色器值、深度與樣板資訊) 與轉譯目標和深度/樣板緩衝區的內容結合，以產生最終的管線結果。|
|輸入|輸出合併輸入是管線狀態、像素著色器產生的像素資料、轉譯目標的內容，以及深度/樣板緩衝區的內容。|
|輸出|最後轉譯的像素色彩。|
| | |


<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Output Merger (OM) stage](output-merger-stage--om-.md) combines various types of output data (pixel shader values, depth and stencil information) with the contents of the render target and depth/stencil buffers to generate the final pipeline result.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><ul>
<li>Pipeline state</li>
<li>The pixel data generated by the pixel shaders</li>
<li>The contents of the render targets</li>
<li>The contents of the depth/stencil buffers.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The final rendered pixel color.</td>
</tr>
</tbody>
</table>


| | |
|-|-|
|Purpose|xxxx|
|Input|yyyy|
|Output|zzzz|
| | |
-->

## <a name="related-topics"></a>相關主題


[Direct3D 圖形學習指南](index.md)

[計算管線](compute-pipeline.md)

 

 
