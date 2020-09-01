---
title: EGL 程式碼與 DXGI 和 Direct3D 的比較
description: DirectX 圖形介面 (DXGI) 和數個 Direct3D API 都可提供與 EGL 相同的角色。 本主題可以協助您從 EGL 的觀點來了解 DXGI 和 Direct3D 11。
ms.assetid: 90f5ecf1-dd5d-fea3-bed8-57a228898d2a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP, egl, dxgi, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: a9e521810c5220d412afb98d25a00f5ac499af1b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165212"
---
# <a name="compare-egl-code-to-dxgi-and-direct3d"></a>EGL 程式碼與 DXGI 和 Direct3D 的比較




**重要 API**

-   [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)
-   [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)
-   [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)

DirectX 圖形介面 (DXGI) 和數個 Direct3D API 都可提供與 EGL 相同的角色。 本主題可以協助您從 EGL 的觀點來了解 DXGI 和 Direct3D 11。

DXGI 和 Direct3D 如同 EGL 都提供方法來設定圖形資源、取得著色器要繪製的轉譯內容，以及在視窗中顯示結果。 但是，DXGI 和 Direct3D 有更多的選項，而從 EGL 移植時需要投入更多心力才能正確設定。

> **注意**   本指南是以 EGL 1.4 的 Khronos 群組 open 規格為基礎，可在這裡找到： [Khronos 原生平臺圖形介面 (EGL 1.4 版-年4月6日 2011) \[ PDF \] ](https://www.khronos.org/registry/egl/specs/eglspec.1.4.20110406.pdf)。 本指導方針中未涵蓋其他平台與開發語言特定的語法差異。

 

## <a name="how-does-dxgi-and-direct3d-compare"></a>與 DXGI 和 Direct3D 相比的結果如何？


EGL 優於 DXGI 和 Direct3D 的最大優點是它能夠以相對簡單的方式開始繪製到視窗表面。 這是因為 OpenGL ES 2.0 — 以及 EGL — 是由多個平台提供者所實作的規格，而 DXGI 和 Direct3D 是硬體廠商驅動程式必須遵循的單一參考。 這表示 Microsoft 必須實作一組 API，盡可能啟用範圍最廣泛的廠商功能組合，而不是專注於特定廠商所提供的功能子集，或是藉由將廠商特定的設定命令組合成更簡單的 API 所提供的功能子集。 另一方面，Direct3D 提供單一 API 組合來涵蓋範圍非常廣泛的圖形硬體平台與功能層級，並為對該平台有使用經驗的開發人員提供更多彈性。

就像 EGL，DXGI 和 Direct3D 提供 API 來進行下列行為：

-   取得、讀取和寫入框架緩衝區 (在 DXGI 中稱為「交換鏈結」)。
-   將框架緩衝區與 UI 視窗產生關聯。
-   取得和設定要在其中繪製的轉譯內容。
-   針對特定轉譯內容發出命令到圖形管線。
-   建立和管理著色器資源，並將它們與轉譯內容產生關聯。
-   轉譯成特定的轉譯目標 (例如紋理)。
-   使用圖形資源轉譯的結果來更新視窗的顯示表面。

若要查看設定圖形管線的基本 Direct3D 程序，請查看 Microsoft Visual Studio 2015 中的 DirectX 11 app (通用 Windows) 範本。 該範本中的基本轉譯類別可提供設定 Direct3D 11 圖形基礎結構和在其上設定基本資源的良好基準，並且支援通用 Windows 平台 (UWP) app 功能 (例如，螢幕旋轉)。

相較於 Direct3D 11，EGL 具有很少 API，而且如果您不熟悉平台特有的命名方式與專業用語，瀏覽後者就是一項挑戰。 這裡提供可協助您找到方向的簡單概觀。

首先，檢閱基本 EGL 物件到 Direct3D 介面的對應：

| EGL 抽象概念 | 類似的 Direct3D 表示法                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EGLDisplay**  | 在 Direct3D (針對 UWP app) 中，可以透過 [**Windows::UI::CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) API (或公開 HWND 的 **ICoreWindowInterop** 介面) 來取得顯示控制代碼。 介面卡與硬體設定是以 [**IDXGIAdapter**](/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) 與 [**IDXGIDevice1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2) COM 介面來分別設定。                                                                                                                                                                                                                                                           |
| **EGLSurface**  | 在 Direct3D 中，緩衝區與其他視窗資源 (可見或螢幕上不可見的) 都是透過特定的 DXGI 介面來建立與設定，其中包含 [**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) (這是一種 Factory 模式實作，可用來取得像是 [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) (顯示緩衝區)) 的 DXGI 資源。 代表圖形裝置及其資源的 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 可以使用 [**D3D11Device::CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 來取得。 針對轉譯目標，請使用 [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) 介面。 |
| **EGLContext**  | 在 Direct3D 中，您可以使用 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 介面，設定並發出命令給圖形管線。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **EGLConfig**   | 在 Direct3D 11 中，您可以使用 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 介面上的方法來建立和設定圖形資源，例如，緩衝區、紋理、樣板及著色器。                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

 

現在，以下是在 UWP app 的 DXGI 與 Direct3D 中設定簡易圖形顯示、資源及內容最基本的程序。

1.  透過呼叫 [**CoreWindow::GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) 以取得 app 核心 UI 執行緒的 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 物件控制代碼。
2.  針對 UWP app，從含有 [**IDXGIFactory2::CreateSwapChainForCoreWindow**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) 的 [**IDXGIAdapter2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiadapter2) 取得交換鏈結，然後將您在步驟 1 取得的 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 參考傳送給它。 隨後您將收到 [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) 執行個體。 將它的範圍設定為您的轉譯器物件和其轉譯的執行緒。
3.  呼叫 [**D3D11Device::CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 方法以取得 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 與 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 執行個體。 以及將它們的範圍設定為您的轉譯器物件。
4.  使用您轉譯器的 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 物件上的方法來建立著色器、材質及其他資源。
5.  使用您轉譯器的 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 物件上的方法來定義緩衝區、執行著色器及管理管線階段。
6.  當管線已執行且框架繪製到背景緩衝區後，請使用 [**IDXGISwapChain1::Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) 將它呈現到螢幕。

如果要更詳細地檢驗此程序，請檢閱 [DirectX 圖形入門](/windows/desktop/getting-started-with-directx-graphics)。 本文的其餘部分涵蓋許多基本圖形管線設定和管理的一般步驟。
> **注意**   Windows 桌面應用程式有不同的 Api 可取得 Direct3D 交換鏈，例如[**D3D11Device：： CreateDeviceAndSwapChain**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdeviceandswapchain)，且不使用[**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)物件。

 

## <a name="obtaining-a-window-for-display"></a>取得要顯示的視窗


在這個範例中，會針對 Microsoft Windows 平台特定的視窗資源，將 HWND 傳送給 eglGetDisplay。 其他像是 Apple 的 iOS (Cocoa) 與 Google 的 Android 的平台對於視窗資源都有不同的控制代碼或參照，而且可能有完全不同的呼叫語法。 取得顯示之後，您要將它初始化、設定慣用的設定，以及利用您可以繪製到其中的背景緩衝區來建立表面。

取得顯示並以 EGL 來設定它。

``` syntax
// Obtain an EGL display object.
EGLDisplay display = eglGetDisplay(GetDC(hWnd));
if (display == EGL_NO_DISPLAY)
{
  return EGL_FALSE;
}

// Initialize the display
if (!eglInitialize(display, &majorVersion, &minorVersion))
{
  return EGL_FALSE;
}

// Obtain the display configs
if (!eglGetConfigs(display, NULL, 0, &numConfigs))
{
  return EGL_FALSE;
}

// Choose the display config
if (!eglChooseConfig(display, attribList, &config, 1, &numConfigs))
{
  return EGL_FALSE;
}

// Create a surface
surface = eglCreateWindowSurface(display, config, (EGLNativeWindowType)hWnd, NULL);
if (surface == EGL_NO_SURFACE)
{
  return EGL_FALSE;
}
```

在 Direct3D 中，UWP app 的主視窗是由 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 物件來表示，您可以在針對 Direct3D 建構的「檢視提供者」的初始化程序中呼叫 [**CoreWindow::GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread)，從 app 物件中取得這個物件。 (如果您是使用 Direct3D-XAML 互通性，則使用 XAML 架構的檢視提供者。) 建立 Direct3D 檢視提供者的程序涵蓋於[如何設定 app 以顯示檢視](/previous-versions/windows/apps/hh465077(v=win.10))中。

取得 Direct3D 的 CoreWindow。

``` syntax
CoreWindow::GetForCurrentThread();
```

一旦取得 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 參考之後，就必須啟用視窗，這個視窗會執行主物件的 **Run** 方法，並開始進行視窗事件處理。 之後，請建立 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 和 [**ID3D11DeviceCoNtext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)，並使用它們來取得基礎 [**IDXGIDevice1**](/windows/desktop/api/dxgi/nn-dxgi-idxgidevice1) 和 [**IDXGIAdapter**](/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) ，讓您可以取得 [**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) 物件，以根據您的 [**DXGI \_ 交換 \_ 鏈 \_ DESC1**](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) 設定建立交換鏈資源。

在 Direct3D 的 CoreWindow 上設定 DXGI 交換鏈結。

``` syntax
// Called when the CoreWindow object is created (or re-created).
void SimpleDirect3DApp::SetWindow(CoreWindow^ window)
{
  // Register event handlers with the CoreWindow object.
  // ...

  // Obtain your ID3D11Device1 and ID3D11DeviceContext1 objects
  // In this example, m_d3dDevice contains the scoped ID3D11Device1 object
  // ...

  ComPtr<IDXGIDevice1>  dxgiDevice;
  // Get the underlying DXGI device of the Direct3D device.
  m_d3dDevice.As(&dxgiDevice);

  ComPtr<IDXGIAdapter> dxgiAdapter;
  dxgiDevice->GetAdapter(&dxgiAdapter);

  ComPtr<IDXGIFactory2> dxgiFactory;
  dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2), 
    &dxgiFactory);

  DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
  swapChainDesc.Width = static_cast<UINT>(m_d3dRenderTargetSize.Width); // Match the size of the window.
  swapChainDesc.Height = static_cast<UINT>(m_d3dRenderTargetSize.Height);
  swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
  swapChainDesc.Stereo = false;
  swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
  swapChainDesc.SampleDesc.Quality = 0;
  swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
  swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
  swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
  swapChainDesc.Flags = 0;

  // ...

  Windows::UI::Core::CoreWindow^ window = m_window.Get();
  dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr, // Allow on all displays.
    &m_swapChainCoreWindow);
}
```

當您準備好框架之後，請呼叫 [**IDXGISwapChain1::Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) 方法來顯示該框架。

請注意，在 Direct3D 11 中，並沒有與 EGLSurface 完全相同的抽象概念。 (雖然有 [**IDXGISurface1**](/windows/desktop/api/dxgi/nn-dxgi-idxgisurface1)，但用法完全不同。) 最接近的概念近似項目是 [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) 物件，我們可以使用這個物件來指派紋理 ([**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)) 做為著色器管線將繪製到其中的背景緩衝區。

在 Direct3D 11 中針對交換鏈結設定背景緩衝區

``` syntax
ComPtr<ID3D11RenderTargetView>    m_d3dRenderTargetViewWin; // scoped to renderer object

// ...

ComPtr<ID3D11Texture2D> backBuffer2;
    
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&backBuffer2));

m_d3dDevice->CreateRenderTargetView(
  backBuffer2.Get(),
  nullptr,
    &m_d3dRenderTargetViewWin);
```

每當建立視窗或變更視窗大小時，呼叫這個程式碼是一個很好的做法。 在轉譯期間，先利用 [**ID3D11DeviceContext1::OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 來設定轉譯目標檢視，然後再設定任何其他子資源，像是頂點緩衝區或著色器。

``` syntax
// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

## <a name="creating-a-rendering-context"></a>建立轉譯內容


在 EGL 1.4 中，所謂的「顯示」是代表一組視窗資源。 您通常可以藉由提供一組屬性給顯示物件並取得傳回的表面，以設定顯示的「表面」。 您可以透過建立該內容並將它繫結到表面和顯示，建立用來顯示表面內容的內容。

呼叫流程通常看起來類似這樣：

-   以顯示或視窗資源的控制代碼來呼叫 eglGetDisplay，並取得顯示物件。
-   使用 eglInitialize 來初始化顯示。
-   取得可用的顯示設定，並使用 eglGetConfigs 和 eglChooseConfig 選取其中一個顯示設定。
-   使用 eglCreateWindowSurface 建立視窗表面。
-   使用 eglCreateContext 建立繪製的顯示內容。
-   使用 eglMakeCurrent，將顯示內容繫結到顯示與表面。

在上一節中，我們已建立 EGLDisplay 和 EGLSurface，現在要使用 EGLDisplay 來建立內容，並將該內容與顯示產生關聯，使用已設定的 EGLSurface 來將輸出參數化。

使用 EGL 1.4 取得轉譯內容

```cpp
// Configure your EGLDisplay and obtain an EGLSurface here ...
// ...

// Create a drawing context from the EGLDisplay
context = eglCreateContext(display, config, EGL_NO_CONTEXT, contextAttribs);
if (context == EGL_NO_CONTEXT)
{
  return EGL_FALSE;
}   
   
// Make the context current
if (!eglMakeCurrent(display, surface, surface, context))
{
  return EGL_FALSE;
}
```

Direct3D 11 中的轉譯內容是透過 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 物件來表示，其代表介面卡並讓您能夠建立像是緩衝區和著色器的 Direct3D 資源，同時也可透過 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 物件來表示，這讓您能夠管理圖形管線並執行著色器。

請注意 Direct3D 功能層級！ 這些都可以用來支援較舊的 Direct3D 硬體平台，範圍涵蓋 DirectX 9.1 到 DirectX 11。 許多使用低電量圖形硬體的平台 (例如平板電腦) 只能存取 DirectX 9.1 功能，而較舊的支援圖形硬體的範圍則涵蓋 9.1 到 11。

使用 DXGI 與 Direct3D 建立轉譯內容

```cpp

// ... 

UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;
ComPtr<IDXGIDevice> dxgiDevice;

D3D_FEATURE_LEVEL featureLevels[] = 
{
        D3D_FEATURE_LEVEL_11_1,
        D3D_FEATURE_LEVEL_11_0,
        D3D_FEATURE_LEVEL_10_1,
        D3D_FEATURE_LEVEL_10_0,
        D3D_FEATURE_LEVEL_9_3,
        D3D_FEATURE_LEVEL_9_2,
        D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> d3dContext;

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
  &d3dContext // Returns the device immediate context.
);
```

## <a name="drawing-into-a-texture-or-pixmap-resource"></a>繪製到紋理或 Pixmap 資源


若要使用 OpenGL ES 2.0 繪製到紋理，請設定像素緩衝區或 PBuffer。 當您成功為它設定並建立 EGLSurface 之後，可以為它提供轉譯內容，並執行著色器管線以繪製到紋理。

使用 OpenGL ES 2.0 繪製到像素緩衝區

``` syntax
// Create a pixel buffer surface to draw into
EGLConfig pBufConfig;
EGLint totalpBufAttrs;

const EGLint pBufConfigAttrs[] =
{
    // Configure the pBuffer here...
};
 
eglChooseConfig(eglDsplay, pBufConfigAttrs, &pBufConfig, 1, &totalpBufAttrs);
EGLSurface pBuffer = eglCreatePbufferSurface(eglDisplay, pBufConfig, EGL_TEXTURE_RGBA); 
```

在 Direct3D 11 中，您可以建立 [**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d) 資源並為它設定轉譯目標。 使用 [**D3D11 轉譯 \_ \_ 目標 \_ 視圖 \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_view_desc)設定呈現目標。 當您在裝置內容上呼叫 [**>id3d11devicecoNtext：:D 原始**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) 方法 (或類似的繪圖作業 \*) 使用此轉譯目標時，結果會繪製成材質。

使用 Direct3D 11 繪製到紋理

``` syntax
ComPtr<ID3D11Texture2D> renderTarget1;

D3D11_RENDER_TARGET_VIEW_DESC renderTargetDesc = {0};
// Configure renderTargetDesc here ...

m_d3dDevice->CreateRenderTargetView(
  renderTarget1.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);

// Later, in your render loop...

// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

如果這個紋理已與 [**ID3D11ShaderResourceView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview) 產生關聯，就能傳送到著色器。

## <a name="drawing-to-the-screen"></a>繪製到螢幕


一旦您已使用 EGLContext 來設定緩衝區並更新資料之後，就可以執行繫結到它的著色器，並使用 glDrawElements 將結果繪製到背景緩衝區。 您可以呼叫 eglSwapBuffers 來顯示背景緩衝區。

Open GL ES 2.0：繪製到螢幕。

``` syntax
glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
```

在 Direct3D 11 中，您可以設定緩衝區，並將著色器繫結到 [**IDXGISwapChain::Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)。 然後，您可以呼叫其中一個[**ID3D11DeviceCoNtext1：:D 原始**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) \* 方法來執行著色器，並將結果繪製至設定為交換鏈後置緩衝區的轉譯目標。 在此之後，您只需呼叫 **IDXGISwapChain::Present1**，就能將背景緩衝區呈現到顯示。

Direct3D 11：繪製到螢幕。

``` syntax

m_d3dContext->DrawIndexed(
        m_indexCount,
        0,
        0);

// ...

m_swapChainCoreWindow->Present1(1, 0, &parameters);
```

## <a name="releasing-graphics-resources"></a>釋放圖形資源


在 EGL 中，您可以藉由將 EGLDisplay 傳送到 eglTerminate 來釋放視窗資源。

使用 EGL 1.4 終止顯示

```cpp
EGLBoolean eglTerminate(eglDisplay);
```

在 UWP app 中，您可以使用 [**CoreWindow::Close**](/uwp/api/windows.ui.core.corewindow.close) 來關閉 CoreWindow，即使這只適用於次要的 UI 視窗也一樣。 您無法關閉主要的 UI 執行緒及其相關聯的 CoreWindow；而是透過作業系統讓它們到期。 然而，在關閉次要的 CoreWindow 時，會引發 [**CoreWindow::Closed**](/uwp/api/windows.ui.core.corewindow.closed) 事件。

## <a name="api-reference-mapping-for-egl-to-direct3d-11"></a>EGL 與 Direct3D 11 的 API 參照對應


| EGL API                          | 類似 Direct3D 11 API 或行為                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| eglBindAPI                       | N/A。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglBindTexImage                  | 呼叫 [**ID3D11Device::CreateTexture2D**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createtexture2d) 以設定 2D 紋理。                                                                                                                                                                                                                                                                                                                                                                                          |
| eglChooseConfig                  | Direct3D 未提供預設的框架緩衝區設定組合。 交換鏈結的設定                                                                                                                                                                                                                                                                                                                                                                                           |
| eglCopyBuffers                   | 若要複製緩衝區資料，請呼叫 [**ID3D11DeviceContext::CopyStructureCount**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copystructurecount)。 若要複製資源，請呼叫 [**ID3DDeviceCOntext::CopyResource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copyresource)。                                                                                                                                                                                                                                                      |
| eglCreateContext                 | 透過呼叫 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 以建立 Direct3D 裝置內容，這樣會將控制代碼傳回到 Direct3D 裝置和預設的 Direct3D 直接內容 ([**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) 物件)。 您也可以在傳回的 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 物件上呼叫 [**ID3D11Device2::CreateDeferredContext**](/windows/desktop/api/d3d11_2/nf-d3d11_2-id3d11device2-createdeferredcontext2)，以建立 Direct3D 延遲內容。 |
| eglCreatePbufferFromClientBuffer | 所有的緩衝區都會當成 Direct3D 子資源來讀取與寫入，例如 [**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)。 使用像是 [**ID3D11DeviceContext1:CopyResource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copyresource) 的方法，從某個相容的子資源複製到另一個子資源。                                                                                                                                                                                                     |
| eglCreatePbufferSurface          | 若要建立不包含交換鏈結的 Direct3D 裝置，請呼叫靜態 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 方法。 針對 Direct3D 轉譯目標檢視，呼叫 [**ID3D11Device::CreateRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview)。                                                                                                                                                                                                                               |
| eglCreatePixmapSurface           | 若要建立不包含交換鏈結的 Direct3D 裝置，請呼叫靜態 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 方法。 針對 Direct3D 轉譯目標檢視，呼叫 [**ID3D11Device::CreateRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview)。                                                                                                                                                                                                                               |
| eglCreateWindowSurface           | 取得 [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) (用於顯示緩衝區) 和 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) (用於圖形裝置及其資源的虛擬介面)。 使用 **ID3D11Device1** 來定義 [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview)，您可以用它來建立要疑供給 **IDXGISwapChain1** 的框架緩衝區。                                                                                         |
| eglDestroyContext                | N/A。 使用 [**ID3D11DeviceContext::DiscardView1**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-discardview1) 來移除轉譯目標檢視。 若要關閉父項 [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)，請將執行個體設為 Null，並等候平台回收它的資源。 您無法直接摧毀裝置內容。                                                                                                                                                |
| eglDestroySurface                | N/A。 當平台關閉 UWP app 的 CoreWindow 時，就會清理圖形資源。                                                                                                                                                                                                                                                                                                                                                                                                 |
| eglGetCurrentDisplay             | 呼叫 [**CoreWindow::GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) 以取得目前主應用程式視窗的參考。                                                                                                                                                                                                                                                                                                                                                         |
| eglGetCurrentSurface             | 這是目前的 [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview)。 這通常會將範圍設定為您的轉譯器物件。                                                                                                                                                                                                                                                                                                                                                         |
| eglGetError                      | 由 DirectX 介面上大部分方法所傳回的 HRESULT 來取得錯誤。 如果方法未傳回 HRESULT，請呼叫 [**GetLastError**](/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror)。 若要將系統錯誤轉換成 HRESULT 值，請使用 [** \_ \_ WIN32**](/windows/desktop/api/winerror/nf-winerror-hresult_from_win32)宏的 hresult   。                                                                                                                                                                                                  |
| eglInitialize                    | 呼叫 [**CoreWindow::GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) 以取得目前主應用程式視窗的參考。                                                                                                                                                                                                                                                                                                                                                         |
| eglMakeCurrent                   | 使用 [**ID3D11DeviceContext1::OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 來設定要在目前內容上繪製的轉譯目標。                                                                                                                                                                                                                                                                                                                                  |
| eglQueryContext                  | N/A。 但是，您可能會從 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 執行個體取得轉譯目標，以及一些設定資料 (請參閱可用方法清單的連結)。                                                                                                                                                                                                                                                                                           |
| eglQuerySurface                  | N/A。 然而，您可能會從 [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) 執行個體上的方法，取得檢視區和目前圖形硬體的相關資料 (請參閱可用方法清單的連結)。                                                                                                                                                                                                                                                                               |
| eglReleaseTexImage               | N/A。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglReleaseThread                 | 針對一般 GPU 多執行緒，請參閱[多執行緒](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro)。                                                                                                                                                                                                                                                                                                                                                                              |
| eglSurfaceAttrib                 | 使用 [**D3D11 \_ 轉譯 \_ 目標 \_ 視圖 \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_view_desc) 來設定 Direct3D 轉譯目標視圖，                                                                                                                                                                                                                                                                                                                                                               |
| eglSwapBuffers                   | 使用[**IDXGISwapChain1::Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)。                                                                                                                                                                                                                                                                                                                                                                                                                     |
| eglSwapInterval                  | 請參閱 [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)。                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| eglTerminate                     | 用來顯示圖形管線輸出的 CoreWindow 是由作業系統所管理。                                                                                                                                                                                                                                                                                                                                                                                          |
| eglWaitClient                    | 針對共用的表面，請使用 IDXGIKeyedMutex。 針對一般 GPU 多執行緒，請參閱[多執行緒](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro)。                                                                                                                                                                                                                                                                                                                                    |
| eglWaitGL                        | 針對共用的表面，請使用 IDXGIKeyedMutex。 針對一般 GPU 多執行緒，請參閱[多執行緒](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro)。                                                                                                                                                                                                                                                                                                                                    |
| eglWaitNative                    | 針對共用的表面，請使用 IDXGIKeyedMutex。 針對一般 GPU 多執行緒，請參閱[多執行緒](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro)。                                                                                                                                                                                                                                                                                                                                    |

 

 

 