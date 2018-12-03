---
title: 設定 DirectX 資源及顯示影像
description: 我們將在此處示範如何建立 Direct3D 裝置、交換鏈結和轉譯目標檢視，以及如何將轉譯的影像呈現到顯示器。
ms.assetid: d54d96fe-3522-4acb-35f4-bb11c3a5b064
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, directx, 資源, 影像
ms.localizationpriority: medium
ms.openlocfilehash: b650f77627e02427b2861a2e6d0df7d1fc86831a
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458360"
---
# <a name="set-up-directx-resources-and-display-an-image"></a>設定 DirectX 資源及顯示影像



我們將在此處示範如何建立 Direct3D 裝置、交換鏈結和轉譯目標檢視，以及如何將轉譯的影像呈現到顯示器。

**目標：** 設定 C++ 通用 Windows 平台 (UWP) app 中的 DirectX 資源及顯示純色。

## <a name="prerequisites"></a>先決條件


我們假設您熟悉 C++。 您還需要圖形程式設計概念的基本經驗。

**完成所需的時間：** 20 分鐘。

## <a name="instructions"></a>指示

### <a name="1-declaring-direct3d-interface-variables-with-comptr"></a>1. 使用 ComPtr 宣告 Direct3D 介面變數

由於我們使用 Windows 執行階段 C++ 範本庫 (WRL) 中的 ComPtr [智慧型指標](https://msdn.microsoft.com/library/windows/apps/hh279674.aspx)範本來宣告 Direct3D 介面變數，因此，我們可以使用非常安全的方法來管理那些變數的生命週期。 我們之後可以使用那些變數來存取 [**ComPtr class**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) 和它的成員。 例如：

```cpp
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    m_d3dDeviceContext->OMSetRenderTargets(
        1,
        m_renderTargetView.GetAddressOf(),
        nullptr // Use no depth stencil.
        );
```

如果您使用 ComPtr 宣告 [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582)，您就可以接著使用 ComPtr 的 **GetAddressOf** 方法取得 **ID3D11RenderTargetView** (\*\*ID3D11RenderTargetView) 的指標位址來傳送給 [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464)。 **OMSetRenderTargets** 會將轉譯目標繫結到[輸出合併階段](https://msdn.microsoft.com/library/windows/desktop/bb205120)，以將轉譯目標指定為輸出目標。

在範例應用程式啟動後，它會初始化並載入，然後準備執行。

### <a name="2-creating-the-direct3d-device"></a>2. 建立 Direct3D 裝置

若要使用 Direct3D API 來轉譯場景，我們必須先建立一個代表顯示卡的 Direct3D 裝置。 若要建立 Direct3D 裝置，我們將呼叫 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 函式。 我們會在 [**D3D\_FEATURE\_LEVEL**](https://msdn.microsoft.com/library/windows/desktop/ff476329) 值的陣列中指定層級 9.1 到 11.1。 Direct3D 會依序巡覽陣列，然後傳回最高的支援功能層級。 因此，為取得可用的最高功能層級，我們將按最高到最低的方式列出 **D3D\_FEATURE\_LEVEL** 陣列項目。 我們會將 [**D3D11\_CREATE\_DEVICE\_BGRA\_SUPPORT**](https://msdn.microsoft.com/library/windows/desktop/ff476107#D3D11_CREATE_DEVICE_BGRA_SUPPORT) 旗標傳送給 *Flags* 參數，讓 Direct3D 資源與 Direct2D 互通。 如果我們使用偵錯組建，則也會一併傳送 [**D3D11\_CREATE\_DEVICE\_DEBUG**](https://msdn.microsoft.com/library/windows/desktop/ff476107#D3D11_CREATE_DEVICE_DEBUG) 旗標。 如需有關對應用程式偵錯的詳細資訊，請參閱[使用偵錯層對應用程式偵錯](https://msdn.microsoft.com/library/windows/desktop/jj200584)。

我們藉由查詢 Direct3D 11 裝置和從 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 傳回的裝置內容，以取得 Direct3D 11.1 裝置 ([**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)) 和裝置內容 ([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598))。

```cpp
        // First, create the Direct3D device.

        // This flag is required in order to enable compatibility with Direct2D.
        UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
        // If the project is in a debug build, enable debugging via SDK Layers with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

        // This array defines the ordering of feature levels that D3D should attempt to create.
        D3D_FEATURE_LEVEL featureLevels[] =
        {
            D3D_FEATURE_LEVEL_11_1,
            D3D_FEATURE_LEVEL_11_0,
            D3D_FEATURE_LEVEL_10_1,
            D3D_FEATURE_LEVEL_10_0,
            D3D_FEATURE_LEVEL_9_3,
            D3D_FEATURE_LEVEL_9_1
        };

        ComPtr<ID3D11Device> d3dDevice;
        ComPtr<ID3D11DeviceContext> d3dDeviceContext;
        DX::ThrowIfFailed(
            D3D11CreateDevice(
                nullptr,                    // Specify nullptr to use the default adapter.
                D3D_DRIVER_TYPE_HARDWARE,
                nullptr,                    // leave as nullptr if hardware is used
                creationFlags,              // optionally set debug and Direct2D compatibility flags
                featureLevels,
                ARRAYSIZE(featureLevels),
                D3D11_SDK_VERSION,          // always set this to D3D11_SDK_VERSION
                &d3dDevice,
                nullptr,
                &d3dDeviceContext
                )
            );

        // Retrieve the Direct3D 11.1 interfaces.
        DX::ThrowIfFailed(
            d3dDevice.As(&m_d3dDevice)
            );

        DX::ThrowIfFailed(
            d3dDeviceContext.As(&m_d3dDeviceContext)
            );
```

### <a name="3-creating-the-swap-chain"></a>3. 建立交換鏈結

接著，我們會建立一個裝置用來轉譯和顯示的交換鏈結。 我們會宣告並初始化 [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528) 結構來描述交換鏈結。 然後我們會將交換鏈結設定為翻轉模型 (亦即交換鏈結的 **SwapEffect** 成員中設定了 [**DXGI\_SWAP\_EFFECT\_FLIP\_SEQUENTIAL**](https://msdn.microsoft.com/library/windows/desktop/bb173077#DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL) 值)，並將 **Format** 成員設定為 [**DXGI\_FORMAT\_B8G8R8A8\_UNORM**](https://msdn.microsoft.com/library/windows/desktop/bb173059#DXGI_FORMAT_B8G8R8A8_UNORM)。 我們會將 **SampleDesc** 成員指定之 [**DXGI\_SAMPLE\_DESC**](https://msdn.microsoft.com/library/windows/desktop/bb173072) 結構的 **Count** 成員設定為 1，並將 **DXGI\_SAMPLE\_DESC** 的 **Quality** 成員設定為零，因為翻轉模型不支援多重採樣消除鋸齒 (MSAA) 功能。 我們會將 **BufferCount** 成員設定為 2，以便讓交換鏈結可以使用前端緩衝區呈現到顯示裝置，並且使用背景緩衝區做為轉譯目標。

我們會透過查詢 Direct3D 11.1 裝置，取得基礎的 DXGI 裝置。 為了將耗電量降到最低 (這對以電池供電的裝置而言很重要，例如膝上型電腦和平板電腦 )，我們會呼叫 [**IDXGIDevice1::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/ff471334) 方法，並以 1 做為 DXGI 可以排入佇列的背景緩衝區框架數目上限。 這可確保應用程式只有在垂直空白後才會進行轉譯。

為了能夠在最後建立交換鏈結，我們必須從 DXGI 裝置取得父系 Factory。 我們會呼叫 [**IDXGIDevice::GetAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174531) 來取得裝置的顯示卡，然後呼叫顯示卡上的 [**IDXGIObject::GetParent**](https://msdn.microsoft.com/library/windows/desktop/bb174542) 來取得父系 Factory ([**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556))。 為了建立交換鏈結，我們會使用交換鏈結描述元和應用程式核心視窗呼叫 [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559)。

```cpp
            // If the swap chain does not exist, create it.
            DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

            swapChainDesc.Stereo = false;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.Scaling = DXGI_SCALING_NONE;
            swapChainDesc.Flags = 0;

            // Use automatic sizing.
            swapChainDesc.Width = 0;
            swapChainDesc.Height = 0;

            // This is the most common swap chain format.
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;

            // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Count = 1;
            swapChainDesc.SampleDesc.Quality = 0;

            // Use two buffers to enable the flip effect.
            swapChainDesc.BufferCount = 2;

            // We recommend using this swap effect for all applications.
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;


            // Once the swap chain description is configured, it must be
            // created on the same adapter as the existing D3D Device.

            // First, retrieve the underlying DXGI Device from the D3D Device.
            ComPtr<IDXGIDevice2> dxgiDevice;
            DX::ThrowIfFailed(
                m_d3dDevice.As(&dxgiDevice)
                );

            // Ensure that DXGI does not queue more than one frame at a time. This both reduces
            // latency and ensures that the application will only render after each VSync, minimizing
            // power consumption.
            DX::ThrowIfFailed(
                dxgiDevice->SetMaximumFrameLatency(1)
                );

            // Next, get the parent factory from the DXGI Device.
            ComPtr<IDXGIAdapter> dxgiAdapter;
            DX::ThrowIfFailed(
                dxgiDevice->GetAdapter(&dxgiAdapter)
                );

            ComPtr<IDXGIFactory2> dxgiFactory;
            DX::ThrowIfFailed(
                dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
                );

            // Finally, create the swap chain.
            CoreWindow^ window = m_window.Get();
            DX::ThrowIfFailed(
                dxgiFactory->CreateSwapChainForCoreWindow(
                    m_d3dDevice.Get(),
                    reinterpret_cast<IUnknown*>(window),
                    &swapChainDesc,
                    nullptr, // Allow on all displays.
                    &m_swapChain
                    )
                );
```

### <a name="4-creating-the-render-target-view"></a>4. 建立轉譯目標檢視

為了將圖形轉譯到視窗，我們需要建立一個轉譯目標檢視。 我們會呼叫 [**IDXGISwapChain::GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb174570) 來取得建立轉譯目標檢視時要使用的交換鏈結背景緩衝區。 我們會將背景緩衝區指定為 2D 紋理 ([**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635))。 為了建立轉譯目標檢視，我們會使用交換鏈結的背景緩衝區呼叫 [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517)。 我們必須將檢視區 ([**D3D11\_VIEWPORT**](https://msdn.microsoft.com/library/windows/desktop/ff476260)) 指定為交換鏈結的整個背景緩衝區大小，以指定繪製到整個核心視窗。 我們會在 [**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480) 的呼叫中使用檢視區，以將該檢視區繫結到管線的[點陣化階段](https://msdn.microsoft.com/library/windows/desktop/bb205125)。 點陣化階段會將向量資訊轉換成點陣影像。 在這種情況下，我們不需要進行轉換，因為我們只要顯示純色。

```cpp
        // Once the swap chain is created, create a render target view.  This will
        // allow Direct3D to render graphics to the window.

        ComPtr<ID3D11Texture2D> backBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                backBuffer.Get(),
                nullptr,
                &m_renderTargetView
                )
            );


        // After the render target view is created, specify that the viewport,
        // which describes what portion of the window to draw to, should cover
        // the entire window.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_VIEWPORT viewport;
        viewport.TopLeftX = 0.0f;
        viewport.TopLeftY = 0.0f;
        viewport.Width = static_cast<float>(backBufferDesc.Width);
        viewport.Height = static_cast<float>(backBufferDesc.Height);
        viewport.MinDepth = D3D11_MIN_DEPTH;
        viewport.MaxDepth = D3D11_MAX_DEPTH;

        m_d3dDeviceContext->RSSetViewports(1, &viewport);
```

### <a name="5-presenting-the-rendered-image"></a>5. 呈現轉譯的影像

我們會進入一個無限迴圈來不斷轉譯並顯示場景。

在這個迴圈中，我們呼叫：

1.  [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464)，將轉譯目標指定為輸出目標。
2.  [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388)，將轉譯目標清除為純色。
3.  [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576)，將轉譯的影像呈現到視窗。

由於我們之前已將最大框架延遲設定為 1，因此 Windows 通常會將轉譯迴圈速度減慢到螢幕的重新整理頻率，一般大約為 60 Hz。 Windows 會在應用程式呼叫 [**Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) 時，讓應用程式進入睡眠模式，以減慢轉譯迴圈速度。 Windows 會讓應用程式保持在睡眠模式，直到螢幕重新整理為止。

```cpp
        // Enter the render loop.  Note that UWP apps should never exit.
        while (true)
        {
            // Process events incoming to the window.
            m_window->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
        }
```

### <a name="6-resizing-the-app-window-and-the-swap-chains-buffer"></a>6. 調整 app 視窗與交換鏈結緩衝區的大小

如果 app 視窗的大小發生變更，該 app 就必須調整交換鏈結緩衝區的大小、重新建立轉譯目標檢視，然後呈現已調整大小的轉譯影像。 為了調整交換鏈結緩衝區的大小，我們會呼叫 [**IDXGISwapChain::ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577)。 在這個呼叫中，我們將緩衝區數目和緩衝區格式保持不變 (*BufferCount* 參數為 2，*NewFormat* 參數為 [**DXGI\_FORMAT\_B8G8R8A8\_UNORM**](https://msdn.microsoft.com/library/windows/desktop/bb173059#DXGI_FORMAT_B8G8R8A8_UNORM))。 我們讓交換鏈結的背景緩衝區大小與已調整大小的視窗相同。 在我們調整交換鏈結的緩衝區大小之後，我們會建立新的轉譯目標，並以類似初始化 app 時的方式呈現新的轉譯影像。

```cpp
            // If the swap chain already exists, resize it.
            DX::ThrowIfFailed(
                m_swapChain->ResizeBuffers(
                    2,
                    0,
                    0,
                    DXGI_FORMAT_B8G8R8A8_UNORM,
                    0
                    )
                );
```

## <a name="summary-and-next-steps"></a>摘要與後續步驟


我們建立了一個 Direct3D 裝置、交換鏈結和轉譯目標檢視，並將轉譯的影像呈現到顯示器。

接著，我們也在顯示器上繪製一個三角形。

[建立著色器及繪製基本型別](creating-shaders-and-drawing-primitives.md)

 

 




