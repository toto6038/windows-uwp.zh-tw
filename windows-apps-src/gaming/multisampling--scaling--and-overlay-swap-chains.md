---
title: 交換鏈結縮放和重疊
description: 了解如何在行動裝置上建立縮放的交換鏈結以加快轉譯速度，以及使用重疊交換鏈結 (可供使用時) 來提高視覺品質。
ms.assetid: 3e4d2d19-cac3-eebc-52dd-daa7a7bc30d1
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 交換鏈縮放比例, 覆疊, directx
ms.localizationpriority: medium
ms.openlocfilehash: 77e5dac007654449fcfbe2053fda65b123e073fa
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368339"
---
# <a name="swap-chain-scaling-and-overlays"></a>交換鏈結縮放和重疊



了解如何在行動裝置上建立縮放的交換鏈結以加快轉譯速度，以及使用重疊交換鏈結 (可供使用時) 來提高視覺品質。

## <a name="swap-chains-in-directx-112"></a>DirectX 11.2 中的交換鏈結


Direct3D 11.2 讓您能夠使用交換鏈結來建立通用 Windows 平台 (UWP) app，從非原生 (降低的) 解析度加以放大，以加快填滿速率。 Direct3D 11.2 也包含適合使用硬碟覆疊進行轉譯的 API，如此，您便能在其他交換鏈結中以原生解析度來呈現 UI。 這讓您的遊戲能夠以完全原生的解析度顯示 UI，同時保有高畫面播放速率，因此能充分利用行動裝置和高 DPI 顯示器 (例如 3840 X 2160)。 本文說明如何使用覆疊交換鏈結。

Direct3D 11.2 也導入了一些新功能，可透過翻轉模型交換鏈結來減少延遲。 請參閱[透過 DXGI 1.3 交換鏈結減少延遲](reduce-latency-with-dxgi-1-3-swap-chains.md)。

## <a name="use-swap-chain-scaling"></a>使用交換鏈結縮放


當您的遊戲在下層硬體 (或者已針對省電模式最佳化的硬體) 上執行時，比起顯示器原生解析度，以較低解析度來轉譯即時遊戲內容會更有助益。 若要執行這個動作，用於轉譯遊戲內容的交換鏈結必須比原生解析度還小，或者必須使用交換鏈結的子區域。

1.  首先，以完全原生的解析度建立交換鏈結。

    ```cpp
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
    swapChainDesc.Scaling = DXGI_SCALING_STRETCH;

    // This sequence obtains the DXGI factory that was used to create the Direct3D device above.
    ComPtr<IDXGIDevice3> dxgiDevice;
    DX::ThrowIfFailed(
        m_d3dDevice.As(&dxgiDevice)
        );

    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );

    ComPtr<IDXGIFactory2> dxgiFactory;
    DX::ThrowIfFailed(
        dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
        );

    ComPtr<IDXGISwapChain1> swapChain;
    DX::ThrowIfFailed(
        dxgiFactory->CreateSwapChainForCoreWindow(
            m_d3dDevice.Get(),
            reinterpret_cast<IUnknown*>(m_window.Get()),
            &swapChainDesc,
            nullptr,
            &swapChain
            )
        );

    DX::ThrowIfFailed(
        swapChain.As(&m_swapChain)
        );
    ```

2.  接著，將來源大小設為已降低的解析度，藉以選擇要放大的交換鏈結子區域。

    DX 前景交換鏈結範例會根據下列百分比計算縮小的大小：

    ```cpp
    m_d3dRenderSizePercentage = percentage;

    UINT renderWidth = static_cast<UINT>(m_d3dRenderTargetSize.Width * percentage + 0.5f);
    UINT renderHeight = static_cast<UINT>(m_d3dRenderTargetSize.Height * percentage + 0.5f);

    // Change the region of the swap chain that will be presented to the screen.
    DX::ThrowIfFailed(
        m_swapChain->SetSourceSize(
            renderWidth,
            renderHeight
            )
        );
    ```

3.  建立檢視區以符合交換鏈結的子區域。

    ```cpp
    // In Direct3D, change the Viewport to match the region of the swap
    // chain that will now be presented from.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        static_cast<float>(renderWidth),
        static_cast<float>(renderHeight)
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

4.  如果使用了 Direct2D，就需要調整旋轉轉換以適用來源區域。

## <a name="create-a-hardware-overlay-swap-chain-for-ui-elements"></a>針對 UI 元素建立硬體重疊交換鏈結


使用交換鏈結縮放時有一個繼承的缺點，便是 UI 也會縮小，因此可能使它變得模糊且難以使用。 在含有覆疊交換鏈結之硬體支援的裝置上，藉由從即時遊戲內容分離出來的交換鏈結中以原生解析度來轉譯 UI，即可完全解決這個問題。 請注意，此技術僅能套用到 [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) 交換鏈結 - 無法與 XAML Interop 搭配使用。

使用下列步驟來建立前景交換鏈結，以使用硬體重疊功能。 這些步驟會在先針對即時遊戲內容建立交換鏈結之後執行，如前所述。

1.  首先，判斷 DXGI 介面卡是否支援覆疊。 從交換鏈結取得 DXGI 輸出介面卡：

    ```cpp
    ComPtr<IDXGIAdapter> outputDxgiAdapter;
    DX::ThrowIfFailed(
        dxgiFactory->EnumAdapters(0, &outputDxgiAdapter)
        );

    ComPtr<IDXGIOutput> dxgiOutput;
    DX::ThrowIfFailed(
        outputDxgiAdapter->EnumOutputs(0, &dxgiOutput)
        );

    ComPtr<IDXGIOutput2> dxgiOutput2;
    DX::ThrowIfFailed(
        dxgiOutput.As(&dxgiOutput2)
        );
    ```

    如果輸出介面卡針對 [**SupportsOverlays**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgioutput2-supportsoverlays) 傳回 True，就表示 DXGI 介面卡支援覆疊。

    ```cpp
    m_overlaySupportExists = dxgiOutput2->SupportsOverlays() ? true : false;
    ```
    
    > **附註**  如果 DXGI 配接器支援重疊，請繼續下一個步驟。 如果裝置不支援覆疊，使用多個交換鏈結進行轉譯將會失效。 可改為在和即時遊戲內容相同的交換鏈結中，以降低的解析度來轉譯 UI。

     

2.  使用 [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) 建立前景交換鏈結。 必須設定下列選項， [ **DXGI\_交換\_鏈結\_DESC1** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1)提供給*pDesc*參數：

    -   指定[ **DXGI\_交換\_鏈結\_旗標\_前景\_層**](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag)交換鏈結旗標，表示前景交換鏈結。
    -   使用[ **DXGI\_ALPHA\_模式\_PREMULTIPLIED** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ne-dxgi1_2-dxgi_alpha_mode) alpha 模式旗標。 前景交換鏈結一律是預乘的。
    -   設定[ **DXGI\_調整\_NONE** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ne-dxgi1_2-dxgi_scaling)旗標。 前景交換鏈結一律會以原生解析度執行。

    ```cpp
     foregroundSwapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER;
     foregroundSwapChainDesc.Scaling = DXGI_SCALING_NONE;
     foregroundSwapChainDesc.AlphaMode = DXGI_ALPHA_MODE_PREMULTIPLIED; // Foreground swap chain alpha values must be premultiplied.
    ```

    > **附註**  設定[ **DXGI\_交換\_鏈結\_旗標\_前景\_層**](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag)再每隔交換鏈結會調整大小的時間。

    ```cpp
    HRESULT hr = m_foregroundSwapChain->ResizeBuffers(
        2, // Double-buffered swap chain.
        static_cast<UINT>(m_d3dRenderTargetSize.Width),
        static_cast<UINT>(m_d3dRenderTargetSize.Height),
        DXGI_FORMAT_B8G8R8A8_UNORM,
        DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER // The FOREGROUND_LAYER flag cannot be removed with ResizeBuffers.
        );
    ```

3.  使用兩個交換鏈結時，將框架延遲上限提高為 2，讓 DXGI 介面卡有時間同時顯示這兩個交換鏈結 (在相同的 VSync 間隔內)。

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

4.  前景交換鏈結一律使用預乘的 Alpha。 呈現框架之前，您可以預期每個像素的色彩值都已經與 Alpha 值相乘。 例如，50% Alpha 的 100% 白色 BGRA 像素會設為 (0.5, 0.5, 0.5, 0.5)。

    可以在 輸出合併階段完成 alpha premultiplication 步驟，藉由套用應用程式的 blend 狀態 (請參閱[ **ID3D11BlendState**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11blendstate)) 與[ **D3D11\_呈現\_目標\_BLEND\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_blend_desc)結構的**SrcBlend**欄位設定為**D3D11\_SRC\_ALPHA**。 含有預乘 Alpha 值的資產也可以使用。

    如果 Alpha 預乘步驟尚未完成，前景交換鏈結上的色彩將會較預期的明亮。

5.  根據前景交換鏈結是否已建立而定，UI 元素的 Direct2D 繪製表面可能需要與前景交換鏈結產生關聯。

    建立轉譯目標檢視：

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

    建立 Direct2D 繪製表面：

    ```cpp
    if (m_foregroundSwapChain)
    {
        // Create a Direct2D target bitmap for the foreground swap chain.
        ComPtr<IDXGISurface2> dxgiForegroundSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiForegroundSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiForegroundSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }
    else
    {
        // Create a Direct2D target bitmap for the swap chain.
        ComPtr<IDXGISurface2> dxgiSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());
    ```

    ```cpp
    // Create a render target view of the swap chain's back buffer.
    ComPtr<ID3D11Texture2D> backBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
        );

    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
            backBuffer.Get(),
            nullptr,
            &m_d3dRenderTargetView
            )
        );
    ```

6.  將前景交換鏈結與用於即時遊戲內容的縮放交換鏈結一起呈現。 由於已將這兩個交換鏈結的畫面格延遲設為 2，, 因此 DXGI 可以在相同的 VSync 間隔內呈現它們兩個。

    ```cpp
    // Present the contents of the swap chain to the screen.
    void DX::DeviceResources::Present()
    {
        // The first argument instructs DXGI to block until VSync, putting the application
        // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
        // frames that will never be displayed to the screen.
        HRESULT hr = m_swapChain->Present(1, 0);

        if (SUCCEEDED(hr) && m_foregroundSwapChain)
        {
            m_foregroundSwapChain->Present(1, 0);
        }

        // Discard the contents of the render targets.
        // This is a valid operation only when the existing contents will be entirely
        // overwritten. If dirty or scroll rects are used, this call should be removed.
        m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());
        if (m_foregroundSwapChain)
        {
            m_d3dContext->DiscardView(m_d3dForegroundRenderTargetView.Get());
        }

        // Discard the contents of the depth stencil.
        m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

        // If the device was removed either by a disconnection or a driver upgrade, we
        // must recreate all device resources.
        if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            HandleDeviceLost();
        }
        else
        {
            DX::ThrowIfFailed(hr);
        }
    }
    ```

 

 




