---
title: OpenGL ES 2.0 著色器管線與 Direct3D 的比較
description: 在概念上來說，Direct3D 11 著色器管線與 OpenGL ES 2.0 中的著色器管線非常相似。
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, OpenGL, Direct3D, 著色器管線
ms.localizationpriority: medium
ms.openlocfilehash: 7a35102fed9993ca37afa1d1f47850427235ed49
ms.sourcegitcommit: cbd900f350569a3901086a44b2d5007bb6fb7bed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2019
ms.locfileid: "72276290"
---
# <a name="compare-the-opengl-es-20-shader-pipeline-to-direct3d"></a>OpenGL ES 2.0 著色器管線與 Direct3D 的比較




**重要 API**

-   [輸入組譯階段](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)
-   [頂點-著色器階段](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85))
-   [圖元著色器階段](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85))

在概念上來說，Direct3D 11 著色器管線與 OpenGL ES 2.0 中的著色器管線非常相似。 不過在 API 設計方面，建立與管理著色器階段的主要元件則分為兩個主要的介面，[**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 和 [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)。 本主題嘗試將常見的 OpenGL ES 2.0 著色器管線 API 模式對應到這些介面中的 Direct3D 11 對應項。

## <a name="reviewing-the-direct3d-11-shader-pipeline"></a>檢閱 Direct3D 11 著色器管線


著色器物件是使用 [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 介面上的方法建立的，如 [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 與 [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)。

Direct3D 11 圖形管線由 [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 介面的執行個體管理，並具有下列階段：

-   [輸入組合階段](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)。 輸入組合階段提供資料 (三角形、線與點) 給管線。 支援此階段的[**ID3D11DeviceCoNtext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)方法前面會加上 "IA"。
-   [頂點著色器階段](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85)) - 頂點著色器階段處理頂點，通常執行轉換、貼圖與光照等作業。 頂點著色器一律採用單一輸入頂點，並產生單一輸出頂點。 支援此階段的[**ID3D11DeviceCoNtext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)方法前面會加上 "VS"。
-   [資料流輸出階段](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-stream-stage) - 資料流輸出階段會在點陣化的過程中，將基本型別資料從管線串流到記憶體。 資料可串流輸出及/或傳遞進入點陣化。 串流輸出到記憶體的資料可重新循環回管線，做為輸入資料或從 CPU 讀回。 支援此階段的[**ID3D11DeviceCoNtext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)方法前面會加上 "SO"。
-   [點陣化階段](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage) - 點陣化會裁剪基本型別、準備像素著色器的基本型別，以及決定如何叫用像素著色器。 您可以通知管線沒有圖元著色器（將圖元著色器階段設定為 Null with [**ID3D11DeviceCoNtext：:P ssetshader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader)），並停用深度和樣板測試（將 DepthEnable 和 StencilEnable 設定為 FALSE，以[**停用點陣化）D3D11 @ no__t-4DEPTH @ no__t-5STENCIL @ no__t-6DESC**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_depth_stencil_desc)）。 停用時，不會更新與點陣化相關的管線計數器。
-   [像素著色器階段](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85)) - 像素著色器階段會接收基本型別的插補資料，並產生每一像素資料，如色彩。 支援此階段的[**ID3D11DeviceCoNtext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)方法前面會加上 "PS"。
-   [輸出合併階段](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage) - 輸出合併階段將不同類型的輸出資料 (像素著色器值、深度與樣板資訊) 與轉譯目標和深度/樣板緩衝區的內容結合，以產生最終的管線結果。 支援此階段的[**ID3D11DeviceCoNtext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)方法前面會加上 "OM"。

（幾何著色器、輪廓著色器、tesselators 和網域著色器還有一些階段，因為在 OpenGL ES 2.0 中沒有雷同，所以我們不會在這裡討論）。如需這些階段之方法的完整清單，請參閱[**ID3D11DeviceCoNtext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)和[**ID3D11DeviceCoNtext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)參考頁面。 **ID3D11DeviceContext1** 擴充了 Direct3D 11 的 **ID3D11DeviceContext**。

## <a name="creating-a-shader"></a>建立著色器


在 Direct3D 中，不會在編譯和載入著色器資源之前先建立著色器資源；而是在載入 HLSL 時建立資源。 因此，沒有直接的類似函式可 glCreateShader，它會建立特定類型的初始化著色器資源（例如，GL @ no__t-0VERTEX @ no__t-1SHADER 或 GL @ no__t-2FRAGMENT @ no__t-3SHADER）。 著色器是在以特定函式 (如 [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 與 [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)) 載入 HLSL 後建立，並使用類型與編譯的 HLSL 做為參數。

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | 成功載入編譯的著色器物件後呼叫 [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 與 [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)，將 CSO 傳遞給它們做為緩衝區。 |

 

## <a name="compiling-a-shader"></a>編譯著色器


Direct3D 著色器必須先行編譯為通用 Windows 平臺（UWP）應用程式中已編譯的著色器物件（cso）檔案，並使用其中一個 Windows 執行階段檔案 Api 進行載入。 （桌面應用程式可以在執行時間從文字檔或字串編譯著色器）。CSO 檔案是從 Microsoft Visual Studio 專案中的任何 hlsl 檔案建立而成，而且只保留相同的名稱，而且只包含在.... 副檔名。 出貨時，請確認它們包含在您的套件中！

| OpenGL ES 2.0                          | Direct3D 11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | 不適用。 在 Visual Studio 中將著色器編譯為 .cso 檔案，並將它們包含在您的套件中。                                                                                     |
| 使用 glGetShaderiv 取得編譯狀態 | 不適用。 如果編譯過程中發生任何錯誤，請參閱 Visual Studio 的 FX 編譯器 (FXC) 的編譯結果。 如果編譯成功，會建立對應的 CSO 檔案。 |

 

## <a name="loading-a-shader"></a>載入著色器


如同建立著色器一節所說明，當對應的 CSO 檔案載入緩衝區並傳遞至下表的其中一個方法時，Direct3D 11 便會建立著色器。

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | 成功載入編譯的著色器物件後呼叫 [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 與 [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)。 |

 

## <a name="setting-up-the-pipeline"></a>設定管線


OpenGL ES 2.0 擁有「著色器程式」物件，包含多個可執行的著色器。 個別著色器會附加至著色器程式物件。 不過，在 Direct3D 11 中，您會直接使用轉譯內容 ([**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) 並在其上建立著色器。

| OpenGL ES 2.0   | Direct3D 11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| glCreateProgram | 不適用。 Direct3D 11 不使用著色器程式物件抽象概念。                          |
| glLinkProgram   | 不適用。 Direct3D 11 不使用著色器程式物件抽象概念。                          |
| glUseProgram    | 不適用。 Direct3D 11 不使用著色器程式物件抽象概念。                          |
| glGetProgramiv  | 使用您建立至 [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 的參考。 |

 

使用靜態 [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 方法建立 [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 與 [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2) 的執行個體。

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
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for UWP apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &m_d3dContext // Returns the device's immediate context.
);
```

## <a name="setting-the-viewports"></a>設定檢視區


在 Direct3D 11 設定檢視區的方法與在 OpenGL ES 2.0 設定檢視區的方法十分類似。 在 Direct3D 11 中，使用已設定的[**CD3D11 @ no__t-4VIEWPORT**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85))呼叫[**ID3D11DeviceCoNtext：： RSSetViewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) 。

Direct3D 11：設定視口。

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
| glViewport    | [**CD3D11 @ no__t-2VIEWPORT**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85))， [ **ID3D11DeviceCoNtext：： RSSetViewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) |

 

## <a name="configuring-the-vertex-shaders"></a>設定頂點著色器


在 Direct3D 11，是在載入著色器時設定頂點著色器。 Uniform 使用 [**ID3D11DeviceContext1::VSSetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vssetconstantbuffers1) 做為常數緩衝區傳遞。

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader)                       |
| glGetShaderiv、glGetShaderSource | [**ID3D11DeviceCoNtext1::VSGetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vsgetshader)                       |
| glGetUniformfv、glGetUniformiv   | [**ID3D11DeviceCoNtext1：： VSGetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vsgetconstantbuffers1)。 |

 

## <a name="configuring-the-pixel-shaders"></a>設定像素著色器


在 Direct3D 11，是在載入著色器時設定像素著色器。 Uniform 使用 [**ID3D11DeviceContext1::PSSetConstantBuffers1.** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-pssetconstantbuffers1) 做為常數緩衝區傳遞。

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)                         |
| glGetShaderiv、glGetShaderSource | [**ID3D11DeviceCoNtext1：:P SGetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-psgetshader)                       |
| glGetUniformfv、glGetUniformiv   | [**ID3D11DeviceCoNtext1：:P sgetconstantbuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-psgetconstantbuffers1)。 |

 

## <a name="generating-the-final-results"></a>產生最終結果


管線完成時，即可將著色器階段的結果繪製到背景緩衝區。 在 Direct3D 11 的做法與在 Open GL ES 2.0 中相同，這個動作牽涉到呼叫繪製命令，在背景緩衝區中將結果輸出為色彩對應，然後傳送該背景緩衝區至顯示畫面。

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glDrawElements | [**ID3D11DeviceCoNtext1：:D raw**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw)、 [**ID3D11DeviceCoNtext1：:D Rawindexed**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) （或[**ID3D11DeviceCoNtext1**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)上的其他 Draw @ no__t-4 方法）。 |
| eglSwapBuffers | [**IDXGISwapChain1：:P resent1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)                                                                                                                                                                              |

 

## <a name="porting-glsl-to-hlsl"></a>將 GLSL 移植到 HLSL


除了複雜類型支援與部分整體語法之外，GLSL 與 HLSL 並沒有太大的不同。 許多開發人員發現，將共用 OpenGL ES 2.0 指示與定義更名為 HLSL 的對等項目，是在這兩者中進行移植最簡單的方法。 請注意，Direct3D 使用著色器模型版本來呈現圖形介面支援之 HLSL 的功能集；OpenGL 則有不同的 HLSL 版本規格。 下表嘗試提供一些約略的概念，讓您了解在其他版本方面針對 Direct3D 11 與 OpenGL ES 2.0 所定義的著色器語言功能集。

| 著色器語言           | GLSL 功能版本                                                                                                                                                                                                      | Direct3D 著色器模型 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Direct3D 11 HLSL          | ~4.30。                                                                                                                                                                                                                    | SM 5.0                |
| 適用於 OpenGL ES 2.0 的 GLSL ES | 1.40。 適用於 OpenGL ES 2.0 的 GLSL ES 較舊實作版本可使用 1.10 到 1.30。 請使用 glGetString （GL @ no__t-0SHADING @ no__t-1LANGUAGE @ no__t-2VERSION）或 glGetString （網底 @ no__t-3LANGUAGE @ no__t-4VERSION）檢查您的原始程式碼，以判斷它。 | ~SM 2.0               |

 

如需這兩個著色器語言之差異與共用語法對應的詳細資訊，請閱讀 [GLSL-to-HLSL 參考](glsl-to-hlsl-reference.md)。

## <a name="porting-the-opengl-intrinsics-to-hlsl-semantics"></a>將 OpenGL 內建移植到 HLSL 語意


Direct3D 11 HLSL 語意是 Uniform 或屬性名稱這類字串，用來識別在應用程式與著色器程式間傳遞的值。 雖然它們可以是任何可能的字串，但最佳做法是使用可指出用法的字串，如 POSITION 或 COLOR。 您可在建構常數緩衝區或緩衝區輸入配置時，指派這些語意。 您也可以在語意後附加 0 到 7 的數字，在類似的值使用不同的登錄。 例如: COLOR0、COLOR1、COLOR2 。

前面加上 "SV @ no__t-0" 的語義，是由您的著色器程式寫入的系統值語義;您的應用程式本身（在 CPU 上執行）無法修改它們。 一般來說，這些語意包含的值為圖形管線中來自其他著色器階段的輸入或輸出，或者是完全由 GPU 產生的值。

此外，當 SV @ no__t-0 語義用來指定著色器階段的輸入或輸出時，會有不同的行為。 例如，SV @ no__t-0POSITION （輸出）包含在端點著色器階段期間轉換的頂點資料，而 SV @ no__t-1POSITION （input）包含在點陣化期間插入的圖元位置值。

以下為共用 OpenGL ES 2.0 著色器內建的一些對應：

| OpenGL 系統值 | 使用這個 HLSL 語意                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| gl @ no__t-0Position        | POSITION(n)，適用於頂點緩衝區資料。 SV @ no__t-0POSITION 提供圖元著色器的圖元位置，而且無法由您的應用程式寫入。                                        |
| gl @ no__t-0Normal          | NORMAL(n)，適用於頂點緩衝區提供的一般資料。                                                                                                                 |
| gl @ no__t-0TexCoord @ no__t-1n @ no__t-2   | TEXCOORD(n)，適用於提供給著色器的紋理 UV (在某些 OpenGL 文件中為 ST) 座標資料。                                                                       |
| gl @ no__t-0FragColor       | COLOR(n)，適用於提供給著色器的 RGBA 色彩資料。 請注意，此語意的處理方式與座標資料相同；該語意僅協助您識別其為色彩資料。 |
| gl @ no__t-0FragData @ no__t-1n @ no__t-2   | SV @ no__t-0Target @ no__t-1n @ no__t-2，用於從圖元著色器寫入目標材質或其他圖元緩衝區。                                                                               |

 

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

在此情況下，SV @ no__t-0TARGET 是轉譯目標的位置，其圖元色彩（定義為具有四個 float 值的向量）會在著色器完成執行時寫入至。

如需在 Direct3D 使用語意的詳細資訊，請閱讀 [HLSL 語意](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)。

 

 




