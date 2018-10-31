---
author: mtoepke
title: 通用 Windows 平台 (UWP) app 中的多重取樣
description: 了解如何在以 Direct3D 建立的通用 Windows 平台 (UWP) 應用程式中使用多重取樣。
ms.assetid: 1cd482b8-32ff-1eb0-4c91-83eb52f08484
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 多重取樣, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 7b967ae1709849bbe5bc944b00d9e30f22052aeb
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5888641"
---
# <a name="span-iddevgamingmultisamplingmulti-sampleantialiasinginwindowsstoreappsspan-multisampling-in-universal-windows-platform-uwp-apps"></a><span id="dev_gaming.multisampling__multi-sample_anti_aliasing__in_windows_store_apps"></span>通用 Windows 平台 (UWP) app 中的多重取樣



了解如何在以 Direct3D 建立的通用 Windows 平台 (UWP) app 中使用多重取樣。 多重取樣 (也稱為多重取樣消除鋸齒) 是一種用來減少鋸齒邊緣外觀的圖形技術。 這項技術的運作方式，是透過在最終的轉譯目標中繪製比實際更多的像素，然後將數值平均，以維持特定像素中「部分」邊緣的外觀。 如需多重取樣在 Direct3D 中的實際運作方式的詳細說明，請參閱[多重取樣消除鋸齒點陣化規則](https://msdn.microsoft.com/library/windows/desktop/cc627092#Multisample)。

## <a name="multisampling-and-the-flip-model-swap-chain"></a>多重取樣與翻轉模型交換鏈結


使用 DirectX 的 UWP app 必須使用翻轉模型交換鏈結。 翻轉模型交換鏈結不直接支援多重取樣，但多重取樣仍能以不同方式套用，方法是將場景轉譯到多重取樣的轉譯目標檢視，然後將多重取樣的轉譯目標解析為背景緩衝區後再呈現。 本文說明將多重取樣新增至 UWP app 所需的步驟。

### <a name="how-to-use-multisampling"></a>如何使用多重取樣

Direct3D 功能層級保證支援特定的基本取樣計數功能，並且保證提供可支援多重取樣的某些緩衝區格式。 圖形裝置支援的格式和取樣計數範圍，通常比基本所需的要多。 多重取樣支援可以在執行階段決定，方法是檢查是否有以特定 DXGI 格式進行多重取樣的功能支援，然後檢查每種支援格式可用的取樣計數。

1.  呼叫 [**ID3D11Device::CheckFeatureSupport**](https://msdn.microsoft.com/library/windows/desktop/ff476497) 以找出哪些 DXGI 格式可與多重取樣一起使用。 提供您的遊戲可用的轉譯目標格式。 轉譯目標與解析目標必須使用相同格式，因此，請同時檢查 [**D3D11\_FORMAT\_SUPPORT\_MULTISAMPLE\_RENDERTARGET**](https://msdn.microsoft.com/library/windows/desktop/ff476134) 與 **D3D11\_FORMAT\_SUPPORT\_MULTISAMPLE\_RESOLVE**。

    **功能層級 9:** 雖然功能層級 9 裝置[保證支援多重取樣的轉譯目標格式](https://msdn.microsoft.com/library/windows/desktop/ff471324#MultiSample_RenderTarget)，多重取樣解析目標不保證支援。 因此，嘗試使用本主題中描述的多重取樣技術之前，這項檢查是必須的。

    下列程式碼會檢查所有 DXGI\_FORMAT 值的多重取樣支援：

    ```cpp
    // Determine the format support for multisampling.
    for (UINT i = 1; i < DXGI_FORMAT_MAX; i++)
    {
        DXGI_FORMAT inFormat = safe_cast<DXGI_FORMAT>(i);
        UINT formatSupport = 0;
        HRESULT hr = m_d3dDevice->CheckFormatSupport(inFormat, &formatSupport);

        if ((formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RESOLVE) &&
            (formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RENDERTARGET)
            )
        {
            m_supportInfo->SetFormatSupport(i, true);
        }
        else
        {
            m_supportInfo->SetFormatSupport(i, false);
        }
    }
    ```

2.  對於每種支援的格式，可呼叫 [**ID3D11Device::CheckMultisampleQualityLevels**](https://msdn.microsoft.com/library/windows/desktop/ff476499) 以查詢取樣計數支援。

    下列程式碼會檢查支援的 DXGI 格式的取樣大小支援：

    ```cpp
    // Find available sample sizes for each supported format.
    for (unsigned int i = 0; i < DXGI_FORMAT_MAX; i++)
    {
        for (unsigned int j = 1; j < MAX_SAMPLES_CHECK; j++)
        {
            UINT numQualityFlags;

            HRESULT test = m_d3dDevice->CheckMultisampleQualityLevels(
                (DXGI_FORMAT) i,
                j,
                &numQualityFlags
                );

            if (SUCCEEDED(test) && (numQualityFlags > 0))
            {
                m_supportInfo->SetSampleSize(i, j, 1);
                m_supportInfo->SetQualityFlagsAt(i, j, numQualityFlags);
            }
        }
    }
    ```

    > **注意：** 使用[**ID3D11Device2::CheckMultisampleQualityLevels1**](https://msdn.microsoft.com/library/windows/desktop/dn280494)而如果您需要檢查的多重取樣支援並排資源的緩衝區。

     

3.  建立含有所需之取樣計數的緩衝區和轉譯目標檢視。 使用相同的 DXGI\_FORMAT、寬度及高度做為交換鏈結，但指定大於 1 的取樣計數，並使用多重取樣材質維度 (例如 **D3D11\_RTV\_DIMENSION\_TEXTURE2DMS**)。 如有需要，您可以重建含有適合多重取樣的新設定的交換鏈結。

    下列程式碼會建立多重取樣的轉譯目標：

    ```cpp
    float widthMulti = m_d3dRenderTargetSize.Width;
    float heightMulti = m_d3dRenderTargetSize.Height;

    D3D11_TEXTURE2D_DESC offScreenSurfaceDesc;
    ZeroMemory(&offScreenSurfaceDesc, sizeof(D3D11_TEXTURE2D_DESC));

    offScreenSurfaceDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
    offScreenSurfaceDesc.Width = static_cast<UINT>(widthMulti);
    offScreenSurfaceDesc.Height = static_cast<UINT>(heightMulti);
    offScreenSurfaceDesc.BindFlags = D3D11_BIND_RENDER_TARGET;
    offScreenSurfaceDesc.MipLevels = 1;
    offScreenSurfaceDesc.ArraySize = 1;
    offScreenSurfaceDesc.SampleDesc.Count = m_sampleSize;
    offScreenSurfaceDesc.SampleDesc.Quality = m_qualityFlags;

    // Create a surface that's multisampled.
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &offScreenSurfaceDesc,
        nullptr,
        &m_offScreenSurface)
        );

    // Create a render target view. 
    CD3D11_RENDER_TARGET_VIEW_DESC renderTargetViewDesc(D3D11_RTV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
        m_offScreenSurface.Get(),
        &renderTargetViewDesc,
        &m_d3dRenderTargetView
        )
        );
    ```

4.  深度緩衝區必須有相同的寬度、高度、取樣計數及材質維度，才能符合多重取樣的轉譯目標。

    下列程式碼會建立多重取樣的深度緩衝區：

    ```cpp
    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT,
        static_cast<UINT>(widthMulti),
        static_cast<UINT>(heightMulti),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL,
        D3D11_USAGE_DEFAULT,
        0,
        m_sampleSize,
        m_qualityFlags
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &depthStencilDesc,
        nullptr,
        &depthStencil
        )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
        depthStencil.Get(),
        &depthStencilViewDesc,
        &m_d3dDepthStencilView
        )
        );
    ```

5.  現在是建立檢視區的好時機，因為檢視區寬度與高度也必須符合轉譯目標。

    下列程式碼會建立檢視區：

    ```cpp
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        widthMulti / m_scalingFactor,
        heightMulti / m_scalingFactor
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

6.  將每個框架轉譯為多重取樣的轉譯目標。 在轉譯完成後，先呼叫 [**ID3D11DeviceContext::ResolveSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476474) 再呈現框架。 這會指示 Direct3D 執行多重取樣作業、計算用於顯示的每個像素值，並將結果放在背景緩衝區。 背景緩衝區便會包含可呈現的最終消除鋸齒影像。

    下列程式碼會在呈現框架之前先解析子資源：

    ```cpp
    if (m_sampleSize > 1)
    {
        unsigned int sub = D3D11CalcSubresource(0, 0, 1);

        m_d3dContext->ResolveSubresource(
            m_backBuffer.Get(),
            sub,
            m_offScreenSurface.Get(),
            sub,
            DXGI_FORMAT_B8G8R8A8_UNORM
            );
    }

    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    hr = m_swapChain->Present(1, 0);
    ```

 

 




