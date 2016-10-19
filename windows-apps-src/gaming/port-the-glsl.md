---
author: mtoepke
title: "移植 GLSL"
description: "一旦將建立和設定緩衝區與著色器物件的程式碼移過去之後，就可以將這些著色器內部的程式碼從 OpenGL ES 2.0 的 GL 著色器語言 (GLSL) 移植到 Direct3D 11 的高階著色器語言 (HLSL)。"
ms.assetid: 0de06c51-8a34-dc68-6768-ea9f75dc57ee
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 883f4423f72f044435ffc0ee9eccdcd5b0d63bfa

---

# 移植 GLSL


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [HLSL 語意](https://msdn.microsoft.com/library/windows/desktop/bb205574)
-   [著色器常數 (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581)

一旦將建立和設定緩衝區與著色器物件的程式碼移過去之後，就可以將這些著色器內部的程式碼從 OpenGL ES 2.0 的 GL 著色器語言 (GLSL) 移植到 Direct3D 11 的高階著色器語言 (HLSL)。

在 OpenGL ES 2.0 中，著色器會在使用像是 **gl\_Position**、**gl\_FragColor** 或 **gl\_FragData\[n\]** (其中 n 是特定轉譯目標的索引) 的內建函式執行之後傳回資料。 在 Direct3D 中，沒有特定的內建函式，而且著色器會傳回資料做為其各自 main() 函式的傳回類型。

您想要在著色器階段之間插補的資料 (例如，頂點位置或法向量) 是藉由使用 **varying** 宣告來處理。 但是，Direct3D 沒有這個宣告；而是必須使用 [HLSL 語意](https://msdn.microsoft.com/library/windows/desktop/bb205574)來標示您要在著色器階段之間傳送的任何資料。 所選擇的特定語意表示資料的用途及意義。 例如，您要將在片段著色器之間插補的頂點資料宣告為：

`float4 vertPos : POSITION;`

或

`float4 vertColor : COLOR;`

其中 POSITION 是用來表示頂點位置資料的語意。 POSITION 也是一個特殊案例，因為在插補之後，像素著色器便無法存取它。 因此，您必須使用 SV\_POSITION 來指定像素著色器的輸入，而系統會將插補的頂點資料放入該變數。

`float4 position : SV_POSITION;`

語意可以在著色器的主體 (main) 方法中進行宣告。 針對像素著色器，主體方法中需要有表示轉譯目標的 SV\_TARGET\[n\] (不含數字尾碼預設值的 SV\_TARGET，以轉譯目標索引 0)。

另請注意，需要有頂點著色器，才能輸出 SV\_POSITION 系統值語意。 這個語意可以將頂點位置資料解析成座標值，其中 x 介於 -1 與 1 之間、y 介於 -1 與 1 之間、z 會除以原始同質座標 w 值 (z/w)，而 w 是 1 除以原始 w 值 (1/w)。 像素著色器會使用 SV\_POSITION 系統值語意來擷取螢幕上的像素位置，其中 x 介於 0 與轉譯目標寬度之間，而 y 介於 0 與轉譯目標高度之間 (每個都會位移 0.5)。 功能層級 9\_x 像素著色器無法從 SV\_POSITION 值中讀取。

常數緩衝區必須使用 **cbuffer** 來宣告，而且必須與特定的起始暫存器產生關聯，以供查詢。

Direct3D 11：HLSL 常數緩衝區宣告

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

此處的常數緩衝區使用暫存器 b0 來保留封裝的緩衝區。 所有的暫存器都是以 b\# 形式來參考。 如需關於常數緩衝區、暫存器及資料封裝的 HLSL 實作的詳細資訊，請參閱[著色器常數 (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581)。

指示
------------

### 步驟 1：移植頂點著色器

在我們的簡單 OpenGL ES 2.0 範例中，頂點著色器會有三個輸入：常數模型-檢視-投影 4x4 矩陣，以及兩個 4 座標的向量。 這兩個向量包含頂點位置及其色彩。 著色器會將位置向量轉換成透視座標，並將它指派給 gl\_Position 內建函式以進行點陣化。 同時也會將頂點色彩複製到一個變化變數，以便在點陣化期間進行插補。

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

### 步驟 2：移植片段著色器

我們在 GLSL 中的片段著色器範例相當簡單：使用插補的色彩值來提供 gl\_FragColor 內建函式。 OpenGL ES 2.0 會將它寫入預設的轉譯目標。

OpenGL ES 2.0：立方體物件的片段著色器 (GLSL)

``` syntax
varying vec4 destColor;

void main()
{
  gl_FragColor = destColor;
} 
```

Direct3D 幾乎一樣簡單。 唯一的顯著差異在於像素著色器的主體函式必須傳回值。 由於色彩是一個 4 座標 (RGBA) 的浮點值，您可以指示 float4 做為傳回類型，然後指定預設轉譯目標做為 SV\_TARGET 系統值語意。

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

## 上一步


[移植頂點緩衝區與資料](port-the-vertex-buffers-and-data-config.md) 下一步
---------

[繪製到螢幕](draw-to-the-screen.md) 備註
-------

了解 HLSL 語意與常數緩衝區的封裝，除了可為您省掉一些令人頭痛的偵錯工作，還能提供最佳化的時機。 如果有機會，請完整閱讀[變數語法 (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509706)、[Direct3D 11 中的緩衝區簡介](https://msdn.microsoft.com/library/windows/desktop/ff476898)以及[使用方法：建立常數緩衝區](https://msdn.microsoft.com/library/windows/desktop/ff476896)。 如果沒有機會，那麼此處提供一些有關語意與常數緩衝區的入門提示，請謹記在心。

-   總是重複檢查轉譯器的 Direct3D 設定程式碼，以確定常數緩衝區的結構符合 HLSL 中的 cbuffer struct 宣告，且這兩個宣告中的元件純量類型是相符的。
-   在轉譯器的 C++ 程式碼中，於常數緩衝區宣告中使用 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) 類型，以確保進行適當的資料封裝。
-   有效使用常數緩衝區的最佳方法是根據它們的更新頻率，將著色器變數組織到常數緩衝區。 例如，如果您有一些每個框架都要更新一次的制式資料，而其他制式資料只會在相機移動時更新，那麼，請考慮將該資料分隔成兩個不同的常數緩衝區。
-   您忘記套用或未正確套用的語意將是最早出現的著色器編譯 (FXC) 錯誤來源。 請仔細檢查它們！ 相關文件可能有一點混亂，因為許多較舊的頁面和範例會參考 Direct3D 11 之前不同版本的 HLSL 語意。
-   請確定您知道要將每個著色器目標設定為哪一個 Direct3D 功能層級。 功能層級 9\_\* 的語意與 11\_1 的語意不同。
-   SV\_POSITION 語意可以將相關聯的插補後位置資料解析為座標值，其中 x 介於 0 與轉譯目標寬度之間、y 介於 0 與轉譯目標高度之間、z 會除以原始同質座標 w 值 (z/w)，而 w 是 1 除以原始 w 值 (1/w)。

## 相關主題


[使用方法：將簡單的 OpenGL ES 2.0 轉譯器移植到 Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[移植著色器物件](port-the-shader-config.md)

[移植頂點緩衝區與資料](port-the-vertex-buffers-and-data-config.md)

[繪製到螢幕](draw-to-the-screen.md)

 

 







<!--HONumber=Aug16_HO3-->


