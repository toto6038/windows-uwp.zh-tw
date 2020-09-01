---
title: 移植 GLSL
description: 一旦將建立和設定緩衝區與著色器物件的程式碼移過去之後，就可以將這些著色器內部的程式碼從 OpenGL ES 2.0 的 GL 著色器語言 (GLSL) 移植到 Direct3D 11 的高階著色器語言 (HLSL)。
ms.assetid: 0de06c51-8a34-dc68-6768-ea9f75dc57ee
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, games, glsl, port, 遊戲, 連接埠
ms.localizationpriority: medium
ms.openlocfilehash: b9f3d3a8bc6bfe9205ccba9d743409519bf8a572
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173122"
---
# <a name="port-the-glsl"></a>移植 GLSL




**重要 API**

-   [HLSL 語義](/windows/desktop/direct3dhlsl/dcl-usage---ps)
-   [著色器常數 (HLSL)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants)

一旦將建立和設定緩衝區與著色器物件的程式碼移過去之後，就可以將這些著色器內部的程式碼從 OpenGL ES 2.0 的 GL 著色器語言 (GLSL) 移植到 Direct3D 11 的高階著色器語言 (HLSL)。

在 OpenGL ES 2.0 中，使用內建函式（例如**gl \_ 位置**、 **gl \_ FragColor**或**gl \_ FragData \[ n \] ** ）在執行之後，著色器會傳回資料 (其中 n 是特定轉譯目標) 的索引。 在 Direct3D 中，沒有特定的內建函式，而且著色器會傳回資料做為其各自 main() 函式的傳回類型。

您想要在著色器階段之間插補的資料 (例如，頂點位置或法向量) 是藉由使用 **varying** 宣告來處理。 但是，Direct3D 沒有這個宣告；而是必須使用 [HLSL 語意](/windows/desktop/direct3dhlsl/dcl-usage---ps)來標示您要在著色器階段之間傳送的任何資料。 所選擇的特定語意表示資料的用途及意義。 例如，您要將在片段著色器之間插補的頂點資料宣告為：

`float4 vertPos : POSITION;`

或

`float4 vertColor : COLOR;`

其中 POSITION 是用來表示頂點位置資料的語意。 POSITION 也是一個特殊案例，因為在插補之後，像素著色器便無法存取它。 因此，您必須使用 SV 位置指定圖元著色器的輸入， \_ 並將插入的頂點資料放在該變數中。

`float4 position : SV_POSITION;`

語意可以在著色器的主體 (main) 方法中進行宣告。 若為圖元著色器，則在 \_ \[ \] 主體方法上需要 SV target n （表示轉譯目標）。  (SV \_ 不含數值尾碼的目標，預設會呈現目標索引0。 ) 

另請注意，必須要有頂點著色器，才能輸出 SV \_ POSITION 系統值語義。 這個語意可以將頂點位置資料解析成座標值，其中 x 介於 -1 與 1 之間、y 介於 -1 與 1 之間、z 會除以原始同質座標 w 值 (z/w)，而 w 是 1 除以原始 w 值 (1/w)。 圖元著色器會使用 SV \_ POSITION 系統值語義來取得螢幕上的圖元位置，其中 x 介於0和轉譯目標寬度之間，y 介於0和轉譯目標高度 (每個位移 0.5) 。 功能層級 9 \_ x 圖元著色器無法從 SV \_ 位置值讀取。

常數緩衝區必須使用 **cbuffer** 來宣告，而且必須與特定的起始暫存器產生關聯，以供查詢。

Direct3D 11：HLSL 常數緩衝區宣告

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

此處的常數緩衝區使用暫存器 b0 來保留封裝的緩衝區。 所有的暫存器都是以 b 形式來參考 \# 。 如需關於常數緩衝區、暫存器及資料封裝的 HLSL 實作的詳細資訊，請參閱[著色器常數 (HLSL)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants)。

<a name="instructions"></a>Instructions
------------
### <a name="step-1-port-the-vertex-shader"></a>步驟 1：移植頂點著色器

在我們的簡單 OpenGL ES 2.0 範例中，頂點著色器會有三個輸入：常數模型-檢視-投影 4x4 矩陣，以及兩個 4 座標的向量。 這兩個向量包含頂點位置及其色彩。 著色器會將位置向量轉換為透視圖座標，並將其指派給點陣的 gl \_ 位置內建函式。 同時也會將頂點色彩複製到一個變化變數，以便在點陣化期間進行插補。

OpenGL ES 2.0：立方體物件的頂點著色器 (GLSL)

``` syntax
uniform mat4 u_mvpMatrix; 
attribute vec4 a_position;
attribute vec4 a_color;
varying vec4 destColor;

void main()
{           
  gl_Position = u_mvpMatrix * a_position;
  destColor = a_color;
}
```

現在，在 Direct3D 中，常數模型-檢視-投影矩陣會包含在暫存器 b0 中封裝的常數緩衝區，而頂點位置與色彩會特別使用個別適用的 HLSL 語意來標記：POSITION 與 COLOR。 因為我們的輸入配置會指出這兩個頂點值的特定排列方式，所以您可以建立結構來保有它們，並在著色器主體函式 (main) 上將結構宣告為輸入參數的類型 (您也可以將它們指定成兩個個別的參數，但可能很麻煩)。您也可以指定這個階段的輸出類型，其中包含插補的位置與色彩，並將它宣告為頂點著色器的主體函式的傳回值。

Direct3D 11：立方體物件的頂點著色器 (HLSL)

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float3 pos : SV_POSITION;
  float3 color : COLOR;
};

PixelShaderInput main(VertexShaderInput input)
{
  PixelShaderInput output;
  float4 pos = float4(input.pos, 1.0f); // add the w-coordinate

  pos = mul(mvp, projection);
  output.pos = pos;

  output.color = input.color;

  return output;
}
```

輸出資料類型 PixelShaderInput 是在點陣化期間所填入，並提供給片段 (像素) 著色器。

### <a name="step-2-port-the-fragment-shader"></a>步驟 2：移植片段著色器

GLSL 中的範例片段著色器非常簡單：提供 gl FragColor 內建的內 \_ 插補的色彩值。 OpenGL ES 2.0 會將它寫入預設的轉譯目標。

OpenGL ES 2.0：立方體物件的片段著色器 (GLSL)

``` syntax
varying vec4 destColor;

void main()
{
  gl_FragColor = destColor;
} 
```

Direct3D 幾乎一樣簡單。 唯一的顯著差異在於像素著色器的主體函式必須傳回值。 由於色彩是4座標 (RGBA) float 值，因此您會將 float4 表示為傳回類型，然後將預設轉譯目標指定為 SV \_ 目標系統值語義。

Direct3D 11：立方體物件的像素著色器 (HLSL)

``` syntax
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR;
};


float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color, 1.0f);
}
```

系統會將位置的像素色彩寫入轉譯目標。 現在，讓我們來看看在[繪製到螢幕](draw-to-the-screen.md)中如何顯示該轉譯目標的內容！

## <a name="previous-step"></a>上一個步驟


[移植頂點緩衝區與資料](port-the-vertex-buffers-and-data-config.md) 下一步
---------
[繪製到螢幕](draw-to-the-screen.md) 備註
-------
了解 HLSL 語意與常數緩衝區的封裝，除了可為您省掉一些令人頭痛的偵錯工作，還能提供最佳化的時機。 如果有機會，請完整閱讀[變數語法 (HLSL)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-variable-syntax)、[Direct3D 11 中的緩衝區簡介](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro)以及[使用方法：建立常數緩衝區](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-constant-how-to)。 如果沒有機會，那麼此處提供一些有關語意與常數緩衝區的入門提示，請謹記在心。

-   總是重複檢查轉譯器的 Direct3D 設定程式碼，以確定常數緩衝區的結構符合 HLSL 中的 cbuffer struct 宣告，且這兩個宣告中的元件純量類型是相符的。
-   在轉譯器的 C++ 程式碼中，於常數緩衝區宣告中使用 [DirectXMath](/windows/desktop/dxmath/directxmath-portal) 類型，以確保進行適當的資料封裝。
-   有效使用常數緩衝區的最佳方法是根據它們的更新頻率，將著色器變數組織到常數緩衝區。 例如，如果您有一些每個框架都要更新一次的制式資料，而其他制式資料只會在相機移動時更新，那麼，請考慮將該資料分隔成兩個不同的常數緩衝區。
-   您忘記套用或未正確套用的語意將是最早出現的著色器編譯 (FXC) 錯誤來源。 請仔細檢查它們！ 相關文件可能有一點混亂，因為許多較舊的頁面和範例會參考 Direct3D 11 之前不同版本的 HLSL 語意。
-   請確定您知道要將每個著色器目標設定為哪一個 Direct3D 功能層級。 功能層級9的語義與 \_ \* 11 1 的語法不同 \_ 。
-   SV \_ 位置語義會解析相關聯的後插補點資料，以座標值，其中 x 介於0和轉譯目標的寬度之間，y 介於0和轉譯目標高度之間，z 是除以原始的同質座標（w 值 (z/w) ），而 w 是1除以原來的 w 值 (1/w) 。

## <a name="related-topics"></a>相關主題


[使用方法：將簡單的 OpenGL ES 2.0 轉譯器移植到 Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[移植著色器物件](port-the-shader-config.md)

[移植頂點緩衝區與資料](port-the-vertex-buffers-and-data-config.md)

[繪製到螢幕](draw-to-the-screen.md)

 

 