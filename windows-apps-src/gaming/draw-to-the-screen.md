---
title: 繪製到螢幕
description: 最後，我們要將繪製旋轉立方體的程式碼移植到螢幕。
ms.assetid: cc681548-f694-f613-a19d-1525a184d4ab
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, DirectX, 圖形
ms.localizationpriority: medium
ms.openlocfilehash: fc93111d48f71a6ca8acad8191a2afb535fad2f0
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8328287"
---
# <a name="draw-to-the-screen"></a>繪製到螢幕




**重要 API**

-   [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)
-   [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)
-   [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)

最後，我們要將繪製旋轉立方體的程式碼移植到螢幕。

在 OpenGL ES 2.0 中，您的繪製內容定義為 EGLContext 類型，其中包含視窗與表面參數以及繪製到轉譯目標所需的資源，將用於構成顯示在視窗上的最終影像。 您使用此內容來設定圖形資源，以便將著色器管線的結果正確顯示在顯示器上。 其中一個主要資源是「背景緩衝區」(或稱為「框架緩衝區物件」)，包含最終的組合轉譯目標，可呈現在顯示器上。

使用 Direct3D 時，設定圖形資源用於繪製到顯示器的程序更易於遵循，且只需要多使用幾個 API。 (不過，Microsoft Visual Studio Direct3D 範本可大幅精簡此程序！) 若要取得內容 (呼叫 Direct3D 裝置內容)，您必須先取得 [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) 物件，然後再使用該物件建立和設定 [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) 物件。 這兩個物件可一起用於設定繪製到顯示器所需的特定資源。

簡單來說，DXGI API 包含的主要 API 可用來管理與圖形介面卡直接相關的資源，而 Direct3D 包含的 API 則可讓您在 GPU 與在 CPU 上執行的主要程式間進行互動。

為了在此範例中進行比較，以下是每個 API 的相關類型：

-   [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)：提供圖形裝置與其資源的虛擬表示法。
-   [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)：提供用來設定緩衝區和發出轉譯命令的介面。
-   [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)：交換鏈結類似於 OpenGL ES 2.0 中的背景緩衝區。 這是圖形介面卡的記憶體區域，包含最終要顯示的轉譯影像。 稱為「交換鏈結」是因為它具有數個「交換的」可寫入緩衝區，將最新的轉譯呈現至螢幕。
-   [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)：這包含容納 Direct3D 裝置繪製內容的 2D 點陣圖緩衝區，其由交換鏈結來呈現。 使用 OpenGL ES 2.0，您可以擁有多個轉譯目標，某些轉譯目標未繫結至交換鏈結，但可用於多重傳遞著色技巧。

在範本中，轉譯器物件包含下列欄位：

Direct3D 11：裝置與裝置內容宣告

``` syntax
Platform::Agile<Windows::UI::Core::CoreWindow>       m_window;

Microsoft::WRL::ComPtr<ID3D11Device1>                m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>          m_d3dContext;
Microsoft::WRL::ComPtr<IDXGISwapChain1>                      m_swapChainCoreWindow;
Microsoft::WRL::ComPtr<ID3D11RenderTargetView>          m_d3dRenderTargetViewWin;
```

這裡是如何將背景緩衝區設定為轉譯目標以及提供給交換鏈結。

``` syntax
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(backBuffer));
m_d3dDevice->CreateRenderTargetView(
  backBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

Direct3D 執行階段會為 [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)明確建立 [**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343)，將紋理表示為「背景緩衝區」，讓交換鏈結用於顯示。

您可以在 Direct3D 範本的自訂 **CreateDeviceResources** 與 **CreateWindowSizeDependentResources** 方法中，找到 Direct3D 裝置與裝置內容的初始化與設定以及轉譯目標。

如需與 EGL 及 EGLContext 類型相關的 Direct3D 裝置內容的詳細資訊，請閱讀[將 EGL 程式碼移植到 DXGI 與 Direct3D](moving-from-egl-to-dxgi.md)。

## <a name="instructions"></a>指示

### <a name="step-1-rendering-the-scene-and-displaying-it"></a>步驟 1：轉譯場景並顯示

更新立方體資料後 (在此案例中，是延著 Y 軸將它稍微旋轉)，Render 方法會將檢視區設為繪製內容 (EGLContext) 的維度。 此內容包含將會使用設定的顯示器 (EGLDisplay) 顯示於視窗表面 (EGLSurface) 的色彩緩衝區。 此時，範例會更新頂點資料屬性、重新繫結索引緩衝區、繪製立方體，並在著色管線繪製到顯示介面的色彩緩衝區內交換。

OpenGL ES 2.0：轉譯框架用於顯示

``` syntax
void Render(GraphicsContext *drawContext)
{
  Renderer *renderer = drawContext->renderer;

  int loc;
   
  // Set the viewport
  glViewport ( 0, 0, drawContext->width, drawContext->height );
   
   
  // Clear the color buffer
  glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
  glEnable(GL_DEPTH_TEST);


  // Use the program object
  glUseProgram (renderer->programObject);

  // Load the a_position attribute with the vertex position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_position");
  glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), 0);
  glEnableVertexAttribArray(loc);

  // Load the a_color attribute with the color position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_color");
  glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
  glEnableVertexAttribArray(loc);

  // Bind the index buffer
  glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);

  // Load the MVP matrix
  glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);

  // Draw the cube
  glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

  eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
}
```

在 Direct3D 11 中的程序非常類似。 (我們假設您是使用 Direct3D 範本的檢視區與轉譯目標設定。)

-   呼叫 [**ID3D11DeviceContext1::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/hh446790) 以更新常數緩衝區 (在此案例中為 model-view-projection 矩陣)。
-   使用 [**ID3D11DeviceContext1::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) 設定頂點緩衝區。
-   使用 [**ID3D11DeviceContext1::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) 設定索引緩衝區。
-   使用 [**ID3D11DeviceContext1::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455) 設定特定的三角形拓撲 (三角形清單)。
-   使用 [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) 設定頂點緩衝區的輸入配置。
-   使用 [**ID3D11DeviceContext1::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) 繫結頂點著色器。
-   使用 [**ID3D11DeviceContext1::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) 繫結片段著色器。
-   透過著色器傳送索引後的頂點，並使用 [**ID3D11DeviceContext1::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) 將色彩結果輸出到轉譯目標緩衝區。
-   使用 [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) 顯示轉譯目標緩衝區。

Direct3D 11：轉譯框架用於顯示

``` syntax
void RenderObject::Render()
{
  // ...

  // Only update shader resources that have changed since the last frame.
  m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    NULL,
    &m_constantBufferData,
    0,
    0);

  // Set up the IA stage corresponding to the current draw operation.
  UINT stride = sizeof(VertexPositionColor);
  UINT offset = 0;
  m_d3dContext->IASetVertexBuffers(
    0,
    1,
    m_vertexBuffer.GetAddressOf(),
    &stride,
    &offset);

  m_d3dContext->IASetIndexBuffer(
    m_indexBuffer.Get(),
    DXGI_FORMAT_R16_UINT,
    0);

  m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
  m_d3dContext->IASetInputLayout(m_inputLayout.Get());

  // Set up the vertex shader corresponding to the current draw operation.
  m_d3dContext->VSSetShader(
    m_vertexShader.Get(),
    nullptr,
    0);

  m_d3dContext->VSSetConstantBuffers(
    0,
    1,
    m_constantBuffer.GetAddressOf());

  // Set up the pixel shader corresponding to the current draw operation.
  m_d3dContext->PSSetShader(
    m_pixelShader.Get(),
    nullptr,
    0);

  m_d3dContext->DrawIndexed(
    m_indexCount,
    0,
    0);

    // ...

  m_swapChainCoreWindow->Present1(1, 0, &parameters);
}

```

呼叫 [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) 後，您的框架就會輸出到設定的顯示器。

## <a name="previous-step"></a>上一步


[移植 GLSL](port-the-glsl.md)

## <a name="remarks"></a>備註

此範例並未討論關於設定裝置資源的複雜性，尤其是通用 Windows 平台 (UWP) DirectX app。 建議您檢閱完整的範本程式碼，特別是執行視窗與裝置資源設定和管理的部分。 UWP app 除了支援暫停/繼續事件之外，也必須支援旋轉事件，而範本示範了處理介面遺失或顯示參數變更的最佳做法。

## <a name="related-topics"></a>相關主題


* [使用方法：將簡單的 OpenGL ES 2.0 轉譯器移植到 Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [移植著色器物件](port-the-shader-config.md)
* [移植 GLSL](port-the-glsl.md)
* [繪製到螢幕](draw-to-the-screen.md)

 

 




