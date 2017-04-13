---
author: mtoepke
title: "OpenGL ES 2.0 著色器管線與 Direct3D 的比較"
description: "在概念上來說，Direct3D 11 著色器管線與 OpenGL ES 2.0 中的著色器管線非常相似。"
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 遊戲, OpenGL, Direct3D, 著色器管線"
ms.openlocfilehash: 20d02d9b9724c0cfd8120d4d38fa476b9efa3bb3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="compare-the-opengl-es-20-shader-pipeline-to-direct3d"></a>OpenGL ES 2.0 著色器管線與 Direct3D 的比較


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [輸入組合階段](https://msdn.microsoft.com/library/windows/desktop/bb205116)
-   [頂點著色器階段](https://msdn.microsoft.com/library/windows/desktop/bb205146#Vertex_Shader_Stage)
-   [像素著色器階段](https://msdn.microsoft.com/library/windows/desktop/bb205146#Pixel_Shader_Stage)

在概念上來說，Direct3D 11 著色器管線與 OpenGL ES 2.0 中的著色器管線非常相似。 不過在 API 設計方面，建立與管理著色器階段的主要元件則分為兩個主要的介面，[**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 和 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)。 本主題嘗試將常見的 OpenGL ES 2.0 著色器管線 API 模式對應到這些介面中的 Direct3D 11 對應項。

## <a name="reviewing-the-direct3d-11-shader-pipeline"></a>檢閱 Direct3D 11 著色器管線


著色器物件是使用 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 介面上的方法建立的，如 [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) 與 [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)。

Direct3D 11 圖形管線由 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 介面的執行個體管理，並具有下列階段：

-   [輸入組合階段](https://msdn.microsoft.com/library/windows/desktop/bb205116)。 輸入組合階段提供資料 (三角形、線與點) 給管線。 支援此階段的 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 方法會以 "IA" 開頭。
-   [頂點著色器階段](https://msdn.microsoft.com/library/windows/desktop/bb205146#Vertex_Shader_Stage) - 頂點著色器階段處理頂點，通常執行轉換、貼圖與光照等作業。 頂點著色器一律採用單一輸入頂點，並產生單一輸出頂點。 支援此階段的 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 方法會以 "VS" 開頭。
-   [資料流輸出階段](https://msdn.microsoft.com/library/windows/desktop/bb205121) - 資料流輸出階段會在點陣化的過程中，將基本型別資料從管線串流到記憶體。 資料可串流輸出及/或傳遞進入點陣化。 串流輸出到記憶體的資料可重新循環回管線，做為輸入資料或從 CPU 讀回。 支援此階段的 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 方法會以 "SO" 開頭。
-   [點陣化階段](https://msdn.microsoft.com/library/windows/desktop/bb205125) - 點陣化會裁剪基本型別、準備像素著色器的基本型別，以及決定如何叫用像素著色器。 您也可告知管線沒有像素著色器 (使用 [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) 將像素著色器階段設為 NULL) 並停用深度與樣板測試 (在 [**D3D11\_DEPTH\_STENCIL\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476110) 將 DepthEnable 與 StencilEnable 設為 FALSE)，以停用點陣化。 停用時，不會更新與點陣化相關的管線計數器。
-   [像素著色器階段](https://msdn.microsoft.com/library/windows/desktop/bb205146#Pixel_Shader_Stage) - 像素著色器階段會接收基本型別的插補資料，並產生每一像素資料，如色彩。 支援此階段的 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 方法會以 "PS" 開頭。
-   [輸出合併階段](https://msdn.microsoft.com/library/windows/desktop/bb205120) - 輸出合併階段將不同類型的輸出資料 (像素著色器值、深度與樣板資訊) 與轉譯目標和深度/樣板緩衝區的內容結合，以產生最終的管線結果。 支援此階段的 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 方法會以 "OM" 開頭。

(還有幾何著色器、輪廓著色器、鑲嵌與網域著色器，但因為在 OpenGL ES 2.0 中沒有類似項目，我們將不會在此討論。) 如需這些階段的方法的完整清單，請參閱 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 和 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 參考頁面。 **ID3D11DeviceContext1** 擴充了 Direct3D 11 的 **ID3D11DeviceContext**。

## <a name="creating-a-shader"></a>建立著色器


在 Direct3D 中，不會在編譯和載入著色器資源之前先建立著色器資源；而是在載入 HLSL 時建立資源。 因此，沒有正好與 glCreateShader 類似的函式，這個函式可建立特定類型的初始化著色器資源 (如 GL\_VERTEX\_SHADER or GL\_FRAGMENT\_SHADER)。 著色器是在以特定函式 (如 [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) 與 [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)) 載入 HLSL 後建立，並使用類型與編譯的 HLSL 做為參數。

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | 成功載入編譯的著色器物件後呼叫 [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) 與 [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)，將 CSO 傳遞給它們做為緩衝區。 |

 

## <a name="compiling-a-shader"></a>編譯著色器


Direct3D 著色器必須在通用 Windows 平台 (UWP) app 中預先編譯為編譯的著色器物件 (.cso) 檔案，並使用其中一個 Windows 執行階段檔案 API 載入。 (傳統型 app 可在執行階段從文字檔或字串編譯著色器。) CSO 檔案是從您 Microsoft Visual Studio 專案中的任何 .hlsl 檔所建置，且可保留相同的名稱，只要有 .cso 副檔名即可。 出貨時，請確認它們包含在您的套件中！

| OpenGL ES 2.0                          | Direct3D 11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | 不適用。 在 Visual Studio 中將著色器編譯為 .cso 檔案，並將它們包含在您的套件中。                                                                                     |
| 使用 glGetShaderiv 取得編譯狀態 | 不適用。 如果編譯過程中發生任何錯誤，請參閱 Visual Studio 的 FX 編譯器 (FXC) 的編譯結果。 如果編譯成功，會建立對應的 CSO 檔案。 |

 

## <a name="loading-a-shader"></a>載入著色器


如同建立著色器一節所說明，當對應的 CSO 檔案載入緩衝區並傳遞至下表的其中一個方法時，Direct3D 11 便會建立著色器。

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | 成功載入編譯的著色器物件後呼叫 [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) 與 [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)。 |

 

## <a name="setting-up-the-pipeline"></a>設定管線


OpenGL ES 2.0 擁有「著色器程式」物件，包含多個可執行的著色器。 個別著色器會附加至著色器程式物件。 不過，在 Direct3D 11 中，您會直接使用轉譯內容 ([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)) 並在其上建立著色器。

| OpenGL ES 2.0   | Direct3D 11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| glCreateProgram | 不適用。 Direct3D 11 不使用著色器程式物件抽象概念。                          |
| glLinkProgram   | 不適用。 Direct3D 11 不使用著色器程式物件抽象概念。                          |
| glUseProgram    | 不適用。 Direct3D 11 不使用著色器程式物件抽象概念。                          |
| glGetProgramiv  | 使用您建立至 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 的參考。 |

 

使用靜態 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 方法建立 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 與 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/dn280493) 的執行個體。

``` syntax
Microsoft::WRL::ComPtr<ID3D11Device1>          m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>  m_d3dContext;

// ...

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for Windows Store apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &m_d3dContext // Returns the device's immediate context.
);
```

## <a name="setting-the-viewports"></a>設定檢視區


在 Direct3D 11 設定檢視區的方法與在 OpenGL ES 2.0 設定檢視區的方法十分類似。 在 Direct3D 11，使用已設定的 [**CD3D11\_VIEWPORT**](https://msdn.microsoft.com/library/windows/desktop/jj151722) 呼叫 [**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480)。

Direct3D 11：設定檢視區。

``` syntax
CD3D11_VIEWPORT viewport(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );
m_d3dContext->RSSetViewports(1, &viewport);
```

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| glViewport    | [**CD3D11\_VIEWPORT**](https://msdn.microsoft.com/library/windows/desktop/jj151722)、[**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480) |

 

## <a name="configuring-the-vertex-shaders"></a>設定頂點著色器


在 Direct3D 11，是在載入著色器時設定頂點著色器。 Uniform 使用 [**ID3D11DeviceContext1::VSSetConstantBuffers1**](https://msdn.microsoft.com/library/windows/desktop/hh446795) 做為常數緩衝區傳遞。

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524)                       |
| glGetShaderiv、glGetShaderSource | [**ID3D11DeviceContext1::VSGetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476489)                       |
| glGetUniformfv、glGetUniformiv   | [**ID3D11DeviceContext1::VSGetConstantBuffers1**](https://msdn.microsoft.com/library/windows/desktop/hh446793)。 |

 

## <a name="configuring-the-pixel-shaders"></a>設定像素著色器


在 Direct3D 11，是在載入著色器時設定像素著色器。 Uniform 使用 [**ID3D11DeviceContext1::PSSetConstantBuffers1.**](https://msdn.microsoft.com/library/windows/desktop/hh404649) 做為常數緩衝區傳遞。

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)                         |
| glGetShaderiv、glGetShaderSource | [**ID3D11DeviceContext1::PSGetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476468)                       |
| glGetUniformfv、glGetUniformiv   | [**ID3D11DeviceContext1::PSGetConstantBuffers1**](https://msdn.microsoft.com/library/windows/desktop/hh404645)。 |

 

## <a name="generating-the-final-results"></a>產生最終結果


管線完成時，即可將著色器階段的結果繪製到背景緩衝區。 在 Direct3D 11 的做法與在 Open GL ES 2.0 中相同，這個動作牽涉到呼叫繪製命令，在背景緩衝區中將結果輸出為色彩對應，然後傳送該背景緩衝區至顯示畫面。

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glDrawElements | [**ID3D11DeviceContext1::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407)、[**ID3D11DeviceContext1::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) (或 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 上的其他 Draw\* 方法)。 |
| eglSwapBuffers | [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)                                                                                                                                                                              |

 

## <a name="porting-glsl-to-hlsl"></a>將 GLSL 移植到 HLSL


除了複雜類型支援與部分整體語法之外，GLSL 與 HLSL 並沒有太大的不同。 許多開發人員發現，將共用 OpenGL ES 2.0 指示與定義更名為 HLSL 的對等項目，是在這兩者中進行移植最簡單的方法。 請注意，Direct3D 使用著色器模型版本來呈現圖形介面支援之 HLSL 的功能集；OpenGL 則有不同的 HLSL 版本規格。 下表嘗試提供一些約略的概念，讓您了解在其他版本方面針對 Direct3D 11 與 OpenGL ES 2.0 所定義的著色器語言功能集。

| 著色器語言           | GLSL 功能版本                                                                                                                                                                                                      | Direct3D 著色器模型 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Direct3D 11 HLSL          | ~4.30。                                                                                                                                                                                                                    | SM 5.0                |
| 適用於 OpenGL ES 2.0 的 GLSL ES | 1.40。 適用於 OpenGL ES 2.0 的 GLSL ES 較舊實作版本可使用 1.10 到 1.30。 使用 glGetString(GL\_SHADING\_LANGUAGE\_VERSION) 或 glGetString(SHADING\_LANGUAGE\_VERSION) 檢查您的原始程式碼加以判斷。 | ~SM 2.0               |

 

如需這兩個著色器語言之差異與共用語法對應的詳細資訊，請閱讀 [GLSL-to-HLSL 參考](glsl-to-hlsl-reference.md)。

## <a name="porting-the-opengl-intrinsics-to-hlsl-semantics"></a>將 OpenGL 內建移植到 HLSL 語意


Direct3D 11 HLSL 語意是 Uniform 或屬性名稱這類字串，用來識別在應用程式與著色器程式間傳遞的值。 雖然它們可以是任何可能的字串，但最佳做法是使用可指出用法的字串，如 POSITION 或 COLOR。 您可在建構常數緩衝區或緩衝區輸入配置時，指派這些語意。 您也可以在語意後附加 0 到 7 的數字，在類似的值使用不同的登錄。 例如：COLOR0、COLOR1、COLOR2...

以 "SV\_" 開頭的語意是由您的著色器程式寫入的系統值語意；您的應用程式本身 (在 CPU 上執行) 無法修改這些語意。 一般來說，這些語意包含的值為圖形管線中來自其他著色器階段的輸入或輸出，或者是完全由 GPU 產生的值。

此外，使用 SV\_ 語意指定著色器階段的輸入或輸出時，會有不同的行為。 例如，SV\_POSITION (輸出) 包含在頂點著色器階段期間轉換的頂點資料，而 SV\_POSITION (輸入) 包含在點陣化期間插補的像素位置值。

以下為共用 OpenGL ES 2.0 著色器內建的一些對應：

| OpenGL 系統值 | 使用這個 HLSL 語意                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| gl\_Position        | POSITION(n)，適用於頂點緩衝區資料。 SV\_POSITION 為像素著色器提供像素位置，且無法由您的 app 寫入。                                        |
| gl\_Normal          | NORMAL(n)，適用於頂點緩衝區提供的一般資料。                                                                                                                 |
| gl\_TexCoord\[n\]   | TEXCOORD(n)，適用於提供給著色器的紋理 UV (在某些 OpenGL 文件中為 ST) 座標資料。                                                                       |
| gl\_FragColor       | COLOR(n)，適用於提供給著色器的 RGBA 色彩資料。 請注意，此語意的處理方式與座標資料相同；該語意僅協助您識別其為色彩資料。 |
| gl\_FragData\[n\]   | SV\_Target\[n\]，適用於從像素著色器寫入目標紋理或其他像素緩衝區。                                                                               |

 

您用來編寫語意程式碼的方法與在 OpenGL ES 2.0 中使用內建不同。 在 OpenGL 中，您不需要任何設定或宣告即可直接存取許多內建；但在 Direct3D，您必須宣告特定常數緩衝區中的欄位才能使用特殊語意，或者將欄位宣告為著色器的 **main()** 方法的傳回值。

以下為常數緩衝區定義中使用的語意範例：

```cpp
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR0;
};

// The position is interpolated to the pixel value by the system. The per-vertex color data is also interpolated and passed through the pixel shader. 
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

這個程式碼定義一對簡單的常數緩衝區

以下為用來定義片段著色器傳回值的語意範例：

```cpp
// A pass-through for the (interpolated) color data.
float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color,1.0f);
}
```

在此案例中，SV\_TARGET 是轉譯目標的位置，當著色器執行完成時，會將像素色彩 (定義為四個浮動值的向量) 寫入此位置。

如需在 Direct3D 使用語意的詳細資訊，請閱讀 [HLSL 語意](https://msdn.microsoft.com/library/windows/desktop/bb509647)。

 

 




