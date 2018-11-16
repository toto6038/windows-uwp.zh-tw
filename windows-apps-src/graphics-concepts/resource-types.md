---
title: 資源類型
description: 不同類型的資源會有不同的配置 (或記憶體使用量)。
ms.assetid: BCDDF227-1837-44DA-ABD4-E39BCFF2B8EF
keywords:
- 資源類型
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3c23cc07c84e9a77b36c812c6f273f518e98838e
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6979391"
---
# <a name="resource-types"></a>資源類型


不同類型的資源會有不同的配置 (或記憶體使用量)。 Direct3D 管線使用的所有資源都衍生自兩個基本資源類型︰[緩衝區](#buffer-resources)和[紋理](#texture-resources)。 緩衝區是原始資料 (元素) 的集合，紋理是紋素 (紋理元素) 的集合。

有兩種方式可以完整指定資源的配置 (或記憶體使用量)︰

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">項目</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>具類型</p></td>
<td align="left"><p>建立資源時完整指定類型。</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Typeless"></span><span id="typeless"></span><span id="TYPELESS"></span>無類型</p></td>
<td align="left"><p>資源繫結到管線時完整指定類型。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idbufferresourcesspanspan-idbufferresourcesspanspan-idbufferresourcesspanspan-idbuffer-resourcesspanbuffer-resources"></a><span id="Buffer_Resources"></span><span id="buffer_resources"></span><span id="BUFFER_RESOURCES"></span><span id="buffer-resources"></span>緩衝資源


緩衝資源是完整具類型的資料的集合，緩衝區則包含元素。 一個元素由 1 到 4 個元件組成。 元素資料類型的範例包括︰封裝資料值 (例如 R8G8B8A8)、單一的 8 位元整數、四個 32 位元浮點值。 這些資料類型是用來儲存資料，例如位置向量、一般向量、頂點緩衝區中的紋理座標、索引緩衝區中的索引，或者裝置狀態。

建立緩衝區做為未結構化的資源。 因為未建構，所以緩衝區無法包含任何 Mipmap 層次，在讀取時不會篩選，而且無法多重取樣。

### <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>緩衝區類型

-   [頂點緩衝區](#vertex-buffer)
-   [索引緩衝區](#index-buffer)
-   [常數緩衝區](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>頂點緩衝區

緩衝區是元素的集合；頂點緩衝區包含每個頂點資料。 最簡單的範例是包含一種類型資料的頂點緩衝區，例如位置資料。 它可以像下圖一樣視覺化。

![包含位置資料的頂點緩衝區圖例](images/d3d10-resources-single-element-vb2.png)

頂點緩衝區經常包含可完整指定 3D 頂點所需的所有資料。 例如，可能會包含每個頂點位置、一般及紋理座標的頂點緩衝區。 此資料通常組織成每個頂點元素集，如下圖所示。

![頂點緩區的圖例，其包含位置、一般和紋理資料](images/d3d10-vertex-buffer-element.png)

這個頂點緩衝區包含八個頂點的每個頂點資料；每個頂點儲存三個元素 (位置、一般和紋理座標)。 位置與一般座標通常會使用三個 32 位元浮點數來指定，紋理座標則會使用兩個 32 位元浮點數來指定。

若要存取頂點緩衝區的資料，您必須知道要存取哪個頂點以及這些其他緩衝區參數︰

-   *位移* - 從緩衝區的起始到第一個頂點資料的位元組數目。
-   *BaseVertexLocation* -從位移到適當繪製呼叫所使用的第一個頂點的位元組數目。

建立頂點緩衝區之前，您需要透過建立輸入配置物件來定義其配置。 建立輸入配置物件之後，將它繫結到輸入組合語言 (IA) 階段。

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>索引緩衝區

索引緩衝區包含連續的 16 位元或 32 位元索引集，而每個索引用來識別頂點緩衝區中的頂點。 使用具有一個或多個頂點緩衝區的索引緩衝區，來提供資料給 IA 階段即稱為編製索引。 索引緩衝區可以視覺化，如下圖所示。

![索引緩衝區的圖例](images/d3d10-index-buffer.png)

儲存在索引緩衝區中的循序索引，使用以下參數尋找︰

-   *位移* - 從緩衝區的起始到第一個索引的位元組數目。
-   *StartIndexLocation* - 從位移到適當繪製呼叫所使用的第一個頂點的位元組數目。
-   *IndexCount* - 要呈現的索引數目。

索引緩衝區可以透過使用區域切割索引分隔每一個，來將多行或多個三角形連環 ([基本拓撲](primitive-topologies.md)) 拼接在一起。 區域切割索引允許使用單一繪製呼叫來繪製多行或三角形連環。 區域切割索引是索引的最大可能值 (16 位元索引為 0xffff，32 位元索引為 0xffffffff)。 區域切割索引會重設已編制索引的基本類型的捲繞順序，並且可以用來消除對於變質三角形的需求，否則可能需要先維護適當的三角形連環的捲繞順序。 下列圖例顯示區域切割索引的範例。

![區域切割索引的圖例](images/d3d10-ia-strips-cut-value.png)

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>常數緩衝區

Direct3D 具有緩衝區，以提供稱為著色器常數衝緩區或只是常數緩衝區的著色器常數。 在概念上來說，它看起來像是單一元素頂點緩衝區，如下圖所示。

![著色器常數緩衝區的圖例](images/d3d10-shader-resource-buffer.png)

每個元素會儲存 1 到 4 個元件常數，取決於儲存的資料格式。

常數緩衝區透過允許著色器常數群組在一起並且同時認可，而不是進行個別呼叫以分別認可每一個常數，來減少更新著色器常數所需的頻寬。

著色器會繼續以不是常數緩衝區一部分的變數的相同方式，按照變數名稱直接讀取常數緩衝區中的變數。

每個著色器階段允許最多 15 個著色器常數緩衝區，而每個緩衝區可保留最多 4096 個常數。

使用常數緩衝區來儲存資料流輸出階段的結果。

如需在著色器中宣告常數緩衝區的範例，請參閱[著色器常數 (DirectX HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581)。

## <a name="span-idtextureresourcesspanspan-idtextureresourcesspanspan-idtextureresourcesspanspan-idtexture-resourcesspantexture-resources"></a><span id="Texture_Resources"></span><span id="texture_resources"></span><span id="TEXTURE_RESOURCES"></span><span id="texture-resources"></span>紋理資源


紋理資源是設計來儲存紋素的結構化資料集合。 和緩衝區不同的是，紋理可依紋理樣本篩選，由著色器單位讀取。 紋理類型會影響篩選紋理的方式。 紋素代表管線可讀取或寫入的最小紋理單位。 每個紋素包含 1 到 4 個元件，以其中一種 DXGI 格式排列 (請參閱 [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059))。

紋裡會建立結構化資源，以便得知其大小。 不過，每個紋理在建立資源時可能具有類型或是無類型，只要在紋理與管線繫結時使用檢視完整指定類型。

-   [紋理類型](#texture-types)
-   [子資源](#subresources)
-   [強輸入與弱輸入](#typed)

### <a name="span-idtexturetypesspanspan-idtexturetypesspanspan-idtexturetypesspanspan-idtexture-typesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span><span id="texture-types"></span>紋理類型

有數種紋理類型︰1D、2D、3D，每一個都可使用或不使用 Mipmap 建立。 Direct3D 也支援紋理陣列和多重取樣的紋理。

-   [1D 紋理](#texture1d-resource)
-   [1D 紋理陣列](#texture1d-array-resource)
-   [2D 紋理和 2D 紋理陣列](#texture2d-resource)
-   [3D 紋理](#texture3d-resource)

### <a name="span-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1d-resourcespan1d-texture"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>1D 紋理

最簡單形式的 1D 紋理所包含的紋理資料，可以使用單一紋理座標處理，它可以視覺化為紋素陣列，如下圖所示。

![1D 紋理的圖例](images/d3d10-1d-texture.png)

每個紋素根據所儲存的資料格式包含許多色彩元件。 增加更多複雜度，您可以使用 Mipmap 層次建立 1D 紋理，如下圖所示。

![具有 Mipmap 層次之 1D 紋理的圖例](images/d3d10-resource-texture1d.png)

Mipmap 層次是比上層小二的 n 次方的紋理。 最上層包含大部分的詳細資料，每個後續層級較小；若是 1D 的 Mipmap，最小層級包含一個紋素。 不同層級是以稱為 LOD (詳細層級) 的索引來識別；您可以在呈現不是那麼靠近相機的幾何圖形時，使用 LOD 存取較小的紋理。

### <a name="span-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1d-array-resourcespan1d-texture-array"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>1D 紋理陣列

Direct3D 10 對於紋理陣列也有新的資料結構。 1D 紋理陣列概念上看起來像下圖。

![1d 紋理陣列的圖例](images/d3d10-resource-texture1darray.png)

這個紋理陣列包含三種紋理。 三個紋理的每一個都具有紋理寬度 5 (也就是第一層的元素數目)。 每個紋理也包含 3 層 Mipmap。

在 Direct3D 中的所有紋理陣列都是同質紋理陣列，這表示紋理陣列中的每種紋理必須有相同資料格式和大小 (包括紋理寬度和 Mipmap 層次數目)。 只要每一個紋理陣列中的所有紋理大小都相符，您可以建立不同大小的紋理陣列。

### <a name="span-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2d-resourcespan2d-texture-and-2d-texture-array"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>2D 紋理和 2D 紋理陣列

Texture2D 資源包含 2D 紋格。 每個紋素是由 u、v 向量定位。 因為是紋理資源，它可能包含 Mipmap 層次和子資源。 完全填充的 2D 紋理資源看起來如下圖。

![2D 紋理資源的圖例](images/d3d10-resource-texture2d.png)

這個紋理資源包含具三個 Mipmap 層次的單一 3x5 紋理。

Texture2DArray 資源是同質 2D 紋理陣列；也就是每個紋理都有相同的資料格式和維度 (包括 Mipmap 層次)。 它與 1D 紋理陣列具有類似的配置，除了紋理現在包含 2D 資料以外，因此其外觀如下圖所示。

![2D 紋理資源陣列的圖例](images/d3d10-resource-texture2darray.png)

這個紋理包含三個紋理，每個紋理為 3x5 含兩個 Mipmap 層次。

### <a name="span-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanusing-a-texture2darray-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>使用 Texture2DArray 做為紋理立方體

紋理立方體是包含 6 個紋理的 2D 紋理陣列，每個立方體表面一個紋理。 完全填充的紋理立方體如下圖所示。

![代表紋理立方體之 2D 紋理資源的圖例](images/d3d10-resource-texturecube.png)

包含 6 個紋理的 2D 紋理陣列，在使用立方體紋理視圖結合管線後，可使用立方體內建功能從著色器內讀取。 使用從紋理立方體中心指出的 3D 向量，從著色器處理紋理。

### <a name="span-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3d-resourcespan3d-texture"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>3D 紋理

Texture3D 資源 (也稱為容積紋理) 包含 3D 容積的紋素。 因為是紋理資源，所以可能包含 Mipmap 層次。 完全填充的 3D 紋理外觀如下圖所示。

![3D 紋理資源的圖例](images/d3d10-resource-texture3d.png)

3D 紋理 Mipmap 切片繫結為描繪目標輸出 (描繪目標視圖) 時，3D 紋理行為等同於具 n 切片的 2D 紋理陣列。 從幾何著色器階段選擇特定著色切片。

沒有 3D 紋理陣列概念，因此 3D 紋理子資源是單一 Mipmap 層次。

### <a name="span-idsubresourcesspanspan-idsubresourcesspanspan-idsubresourcesspansubresources"></a><span id="Subresources"></span><span id="subresources"></span><span id="SUBRESOURCES"></span>子資源

Direct3D API 參考整個資源或資源子集。 若要指定部分資源，Direct3D 已創造*子資源*一詞，表示資源的子集。

緩衝區定義為單一的子資源。 紋理則稍微複雜，因為有數種不同紋理類型 (1D、2D 等)，其中部分支援 Mipmap 層次和/或紋理陣列。 從最簡單的案例開始，1D 紋理定義為單一子資源，如下圖所示。

![1D 紋理的圖例](images/d3d10-1d-texture.png)

這表示組成 1D 紋理的紋理陣列包含在單一子資源中。

如果展開含有三個 Mipmap 層次的 1D 紋理，它可以視覺化如下。

![具有 Mipmap 層次之 1D 紋理的圖例](images/d3d10-resource-texture1d.png)

將此視為三個子紋理組成的單一紋理。 每個子紋理計算為一個子資源，所以這個 1D 紋理包含 3 個子資源。 子紋理 (或子資源) 可以對單一紋理使用詳細層級 (LOD) 編制索引。 使用紋理陣列時，存取特定子紋理需要 LOD 和特定紋理二者。 或者，API 結合這兩個資訊為單一以零起始的子資源索引，如此處所示。

![以零起始的子資源索引的圖例](images/d3d10-resource-texture1darray-sub-indexing.png)

### <a name="span-idselectingsubresourcesspanspan-idselectingsubresourcesspanspan-idselectingsubresourcesspanselecting-subresources"></a><span id="Selecting_Subresources"></span><span id="selecting_subresources"></span><span id="SELECTING_SUBRESOURCES"></span>選取子資源

有些 API 會存取整個資源，其他則存取部分資源。 通常存取部分資源的 API 使用檢視描述來指定要存取的子資源。

這些圖闡述檢視描述在存取紋理陣列時使用的詞彙。

### <a name="span-idarrayslicespanspan-idarrayslicespanspan-idarrayslicespanarray-slice"></a><span id="Array_Slice"></span><span id="array_slice"></span><span id="ARRAY_SLICE"></span>陣列切片

提供紋理陣列，每個紋理含有 Mipmap，陣列切片 (以白色矩形代表) 則包含一個紋理及其所有子紋理，如下圖所示。

![陣列切片的圖例](images/d3d10-resource-array-slice.png)

### <a name="span-idmipslicespanspan-idmipslicespanspan-idmipslicespanmip-slice"></a><span id="Mip_Slice"></span><span id="mip_slice"></span><span id="MIP_SLICE"></span>Mip 切片

Mip 切片 (以白色矩形代表) 對於陣列中的每個紋理包含一個 Mipmap 層次，如下圖所示。

![Mip 切片的圖例](images/d3d10-resource-mip-slice.png)

### <a name="span-idselectingasinglesubresourcespanspan-idselectingasinglesubresourcespanspan-idselectingasinglesubresourcespanselecting-a-single-subresource"></a><span id="Selecting_a_Single_Subresource"></span><span id="selecting_a_single_subresource"></span><span id="SELECTING_A_SINGLE_SUBRESOURCE"></span>選取單一子資源

您可以使用這兩種切片來選擇單一子資源，如下圖所示。

![使用陣列切片和 Mip 切片選擇子資源的圖例](images/d3d10-resource-subresources-1.png)

### <a name="span-idselectingmultiplesubresourcesspanspan-idselectingmultiplesubresourcesspanspan-idselectingmultiplesubresourcesspanselecting-multiple-subresources"></a><span id="Selecting_Multiple_Subresources"></span><span id="selecting_multiple_subresources"></span><span id="SELECTING_MULTIPLE_SUBRESOURCES"></span>選取多個子資源

或者，您也可以使用含有數個 Mipmap 層次和/或數個紋理的這兩種切片，來選取多個子資源。

![選取多個子資源的圖例](images/d3d10-resource-subresources-2.png)

無論您使用哪種紋理類型，含有或未含 Mipmap，具有或不具紋理陣列，通常都會提供 Helper 函式來運算特定子資源的索引。

### <a name="span-idtypedspanspan-idtypedspanspan-idtypedspanstrong-vs-weak-typing"></a><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>強輸入與弱輸入

建立完整具類型的資源會限制資源為其建立時使用的格式。 這可讓執行階段將存取最佳化，尤其是若資源的建立是使用指出應用程式無法對應的旗標。 使用特定類型建立的資源無法使用檢視機制重新解譯。

在無類型的資源中，當資源是第一次建立時資料類型是未知的。 應用程式必須從可用的無類型格式中選擇。 您必須指定要配置的記憶體大小，以及執行階段是否需要在 Mipmap 中產生子紋理。

不過，確實的資料格式 (記憶體是否解釋為整數、浮點值、未簽署的整數等) 要到資源使用檢視繫結到管線時才能確定。 因為直到紋理繫結到管線，紋理格式均維持彈性，所以資源稱為弱輸入的儲存空間。 弱輸入儲存空間的優點，是可以重複使用或重新解譯 (使用另一種格式)，只要新格式的元件位元符合舊格式的位元計數。

單一資源可以繫結至多個管線階段，只要每一個都有唯一檢視，而這完全符合每個位置的格式。 例如，以無類型格式建立的資源可用做為 FLOAT 格式，以及同時管線中不同位置的 UINT 格式。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[資源](resources.md)
