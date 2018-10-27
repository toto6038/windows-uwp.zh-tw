---
author: mtoepke
title: 支援螢幕方向 (DirectX 和 C++)
description: 在這裡，我們將討論您的 UWP DirectX app 中處理螢幕旋轉的最佳做法，以便在 windows 10 裝置的圖形硬體使用有效且更有效率。
ms.assetid: f23818a6-e372-735d-912b-89cabeddb6d4
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 螢幕方向, directx
ms.localizationpriority: medium
ms.openlocfilehash: 4ed8739f8ba7b2049af154d458ccaa831b8526a5
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5706255"
---
# <a name="supporting-screen-orientation-directx-and-c"></a>支援螢幕方向 (DirectX 和 C++)



當您處理 [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) 事件時，通用 Windows 平台 (UWP) app 可支援多個螢幕方向。 在這裡，我們將討論您的 UWP DirectX app 中處理螢幕旋轉的最佳做法，以便在 windows 10 裝置的圖形硬體使用有效且更有效率。

在您開始之前，請記住不論裝置的方向為何，圖形硬體一律會以相同的方式輸出像素資料。 Windows 10 裝置可以判斷其目前的顯示方向 （使用某種感應器，或軟體切換），並允許使用者變更顯示設定。 因為此，windows 10 本身會處理影像的旋轉，以確保它們完全 」 直立 」 根據裝置的方向。 根據預設，您的應用程式會收到某個東西的方向已經變更的通知，例如視窗大小。 當發生這種情況時，windows 10 立即旋轉，最終顯示的影像。 三個四個特定螢幕方向 （稍後討論），適用於 windows 10 會使用額外的圖形資源和運算來顯示最終影像。

對於UWP DirectX app，[**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/dn264258) 物件會提供您的應用程式可以查詢的基本顯示方向資料。 預設方向是 *「橫向」*，其中顯示器的像素寬度大於高度；另一個方向是 *「直向」*，其中顯示器以任一方向旋轉 90 度，而寬度變成小於高度。

Windows 10 定義了四個特定的顯示方向模式：

-   橫向 — 預設顯示方向為 windows 10，並會被視為基礎或識別旋轉角度 （0 度）。
-   直向 — 顯示器已經順時鐘旋轉 90 度 (或逆時鐘旋轉 270 度)。
-   橫向翻轉 — 顯示器已經旋轉 180 度 (上下顛倒)。
-   直向翻轉 — 顯示器已經順時鐘旋轉 270 度 (或逆時鐘旋轉 90 度)。

當顯示器從一個方向旋轉至另一個時，windows 10 在內部執行旋轉操作來對齊以新的方向繪製的影像，而使用者在畫面上看到直立的影像。

此外，windows 10 會顯示自動轉換動畫以建立順暢的使用者經驗，當從一個方向轉移到另一個。 當顯示方向轉移時，使用者所看到的這些轉移會以所顯示之螢幕影像的固定大小及旋轉動畫呈現。 Windows 10 會配置時間，以進行新方向的配置應用程式。

整體說來，處理螢幕方向變更的大致程序如下：

1.  使用視窗界限值與顯示方向資料的組合，讓交換鏈結對齊裝置的原生顯示方向。
2.  通知 windows 10 使用[**idxgiswapchain1:: Setrotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801)交換鏈結的方向。
3.  變更轉譯程式碼以產生對齊裝置之使用者方向的影像。

## <a name="resizing-the-swap-chain-and-pre-rotating-its-contents"></a>重新調整交換鏈結的大小並預先旋轉其內容


若要在 UWP DirectX app 中執行基本的調整顯示大小及預先旋轉其內容，可實作下列步驟：

1.  處理 [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) 事件。
2.  將交換鏈結大小調整成新的視窗大小。
3.  呼叫 [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) 來設定交換鏈結的方向。
4.  重新建立任何視窗大小相依資源，例如您的轉譯目標和其他像素資料緩衝區。

現在，讓我們看看這些步驟的更多細節。

您的第一個步驟是要註冊 [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) 事件的處理常式。 每次螢幕方向變更 (例如旋轉了顯示器) 時，就會在 app 中引發這個事件。

若要處理 [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) 事件，您需要在所需的 [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509) 方法中連結 **DisplayInformation::OrientationChanged** 的處理常式，該方法是您的檢視提供者必須實作之 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) 介面的其中一個方法。

在這個程式碼範例中，[**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) 的事件處理常式是稱為 **OnOrientationChanged** 的方法 。 當 **DisplayInformation::OrientationChanged** 引發時，會接著呼叫名為 **SetCurrentOrientation** 的方法，該方法接著呼叫 **CreateWindowSizeDependentResources**。

```cpp
void App::SetWindow(CoreWindow^ window)
{
  // ... Other UI event handlers assigned here ...
  
    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);

  // ...
}
}
```

```cpp
void App::OnOrientationChanged(DisplayInformation^ sender, Object^ args)
{
    m_deviceResources->SetCurrentOrientation(sender->CurrentOrientation);
    m_main->CreateWindowSizeDependentResources();
}

// This method is called in the event handler for the OrientationChanged event.
void DX::DeviceResources::SetCurrentOrientation(DisplayOrientations currentOrientation)
{
    if (m_currentOrientation != currentOrientation)
    {
        m_currentOrientation = currentOrientation;
        CreateWindowSizeDependentResources();
    }
}
```

接著調整新螢幕方向的交換鏈結大小，以及將它備妥以在執行轉譯時旋轉圖形管線內容。 在這個範例中，**DirectXBase::CreateWindowSizeDependentResources** 方法會處理呼叫 IDXGISwapChain::ResizeBuffers、設定 3D 和 2D 旋轉矩陣、呼叫 SetRotation，及重新建立資源。

```cpp
void DX::DeviceResources::CreateWindowSizeDependentResources() 
{
    // Clear the previous window size specific context.
    ID3D11RenderTargetView* nullViews[] = {nullptr};
    m_d3dContext->OMSetRenderTargets(ARRAYSIZE(nullViews), nullViews, nullptr);
    m_d3dRenderTargetView = nullptr;
    m_d2dContext->SetTarget(nullptr);
    m_d2dTargetBitmap = nullptr;
    m_d3dDepthStencilView = nullptr;
    m_d3dContext->Flush();

    // Calculate the necessary render target size in pixels.
    m_outputSize.Width = DX::ConvertDipsToPixels(m_logicalSize.Width, m_dpi);
    m_outputSize.Height = DX::ConvertDipsToPixels(m_logicalSize.Height, m_dpi);
    
    // Prevent zero size DirectX content from being created.
    m_outputSize.Width = max(m_outputSize.Width, 1);
    m_outputSize.Height = max(m_outputSize.Height, 1);

    // The width and height of the swap chain must be based on the window's
    // natively-oriented width and height. If the window is not in the native
    // orientation, the dimensions must be reversed.
    DXGI_MODE_ROTATION displayRotation = ComputeDisplayRotation();

    bool swapDimensions = displayRotation == DXGI_MODE_ROTATION_ROTATE90 || displayRotation == DXGI_MODE_ROTATION_ROTATE270;
    m_d3dRenderTargetSize.Width = swapDimensions ? m_outputSize.Height : m_outputSize.Width;
    m_d3dRenderTargetSize.Height = swapDimensions ? m_outputSize.Width : m_outputSize.Height;

    if (m_swapChain != nullptr)
    {
        // If the swap chain already exists, resize it.
        HRESULT hr = m_swapChain->ResizeBuffers(
            2, // Double-buffered swap chain.
            lround(m_d3dRenderTargetSize.Width),
            lround(m_d3dRenderTargetSize.Height),
            DXGI_FORMAT_B8G8R8A8_UNORM,
            0
            );

        if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
        {
            // If the device was removed for any reason, a new device and swap chain will need to be created.
            HandleDeviceLost();

            // Everything is set up now. Do not continue execution of this method. HandleDeviceLost will reenter this method 
            // and correctly set up the new device.
            return;
        }
        else
        {
            DX::ThrowIfFailed(hr);
        }
    }
    else
    {
        // Otherwise, create a new one using the same adapter as the existing Direct3D device.
        DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

        swapChainDesc.Width = lround(m_d3dRenderTargetSize.Width); // Match the size of the window.
        swapChainDesc.Height = lround(m_d3dRenderTargetSize.Height);
        swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
        swapChainDesc.Stereo = false;
        swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
        swapChainDesc.SampleDesc.Quality = 0;
        swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
        swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
        swapChainDesc.Flags = 0;    
        swapChainDesc.Scaling = DXGI_SCALING_NONE;
        swapChainDesc.AlphaMode = DXGI_ALPHA_MODE_IGNORE;

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

        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForCoreWindow(
                m_d3dDevice.Get(),
                reinterpret_cast<IUnknown*>(m_window.Get()),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }

    // Set the proper orientation for the swap chain, and generate 2D and
    // 3D matrix transformations for rendering to the rotated swap chain.
    // Note the rotation angle for the 2D and 3D transforms are different.
    // This is due to the difference in coordinate spaces.  Additionally,
    // the 3D matrix is specified explicitly to avoid rounding errors.

    switch (displayRotation)
    {
    case DXGI_MODE_ROTATION_IDENTITY:
        m_orientationTransform2D = Matrix3x2F::Identity();
        m_orientationTransform3D = ScreenRotation::Rotation0;
        break;

    case DXGI_MODE_ROTATION_ROTATE90:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(90.0f) *
            Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
        m_orientationTransform3D = ScreenRotation::Rotation270;
        break;

    case DXGI_MODE_ROTATION_ROTATE180:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(180.0f) *
            Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
        m_orientationTransform3D = ScreenRotation::Rotation180;
        break;

    case DXGI_MODE_ROTATION_ROTATE270:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(270.0f) *
            Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
        m_orientationTransform3D = ScreenRotation::Rotation90;
        break;

    default:
        throw ref new FailureException();
    }


    //SDM: only instance of SetRotation
    DX::ThrowIfFailed(
        m_swapChain->SetRotation(displayRotation)
        );

    // Create a render target view of the swap chain back buffer.
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

    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT, 
        lround(m_d3dRenderTargetSize.Width),
        lround(m_d3dRenderTargetSize.Height),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
            &depthStencilDesc,
            nullptr,
            &depthStencil
            )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2D);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
            depthStencil.Get(),
            &depthStencilViewDesc,
            &m_d3dDepthStencilView
            )
        );
    
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);

    // Create a Direct2D target bitmap associated with the
    // swap chain back buffer and set it as the current target.
    D2D1_BITMAP_PROPERTIES1 bitmapProperties = 
        D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_PREMULTIPLIED),
            m_dpi,
            m_dpi
            );

    ComPtr<IDXGISurface2> dxgiBackBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiBackBuffer))
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiBackBuffer.Get(),
            &bitmapProperties,
            &m_d2dTargetBitmap
            )
        );

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());

    // Grayscale text anti-aliasing is recommended for all UWP apps.
    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

}

```

在儲存目前的視窗高度和寬度值以供下次呼叫這個方法時使用之後，請將顯示界限的裝置獨立像素 (DIP) 值轉換成像素。 在這個範例中，您會呼叫 **ConvertDipsToPixels**，這是一個執行下列程式碼的簡單函式：

` floor((dips * dpi / 96.0f) + 0.5f);`

您可以增加 0.5f 來確保四捨五入到最接近的整數值。

題外話，[**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 座標一律是以 DIP 定義。 如需 windows 10 和舊版 Windows，DIP 定義為 1/96 英吋，並且對齊到*最高*的 OS 的定義。 當顯示方向旋轉成直向模式時，app 會翻轉 **CoreWindow** 的寬度和高度，而轉譯目標大小 (界限) 必須跟著變更。 由於 Direct3D 的座標一律使用實體像素，因此您必須先從 **CoreWindow** 的 DIP 值轉換成整數像素值，再將這些值傳送給 Direct3D 來設定交換鏈結。

按照處理程序，您所做的工作會比只調整交換鏈結大小來得多一些：您實際上是先旋轉影像的 Direct2D 和 Direct3D 元件，再結合它們來進行呈現，並且告知交換鏈結您已經以新方向轉譯結果。 以下是這個處理程序更多的細節，如 **DX::DeviceResources::CreateWindowSizeDependentResources** 的程式碼範例所示：

-   判斷顯示器的新方向。 如果顯示器已經從橫向翻轉成直向 (反之亦然)，請切換顯示界限的高度和寬度值 — 從 DIP 值變更成像素。

-   然後，檢查看看是否已經建立交換鏈結。 如果尚未建立，請呼叫 [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559) 來建立它。 否則，請呼叫 [**IDXGISwapchain:ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577)，將現有交換鏈結的緩衝區大小調整成新的顯示大小。 雖然您不需要為旋轉事件調整交換鏈結大小 — 畢竟您輸出的是已經由轉譯管線旋轉的內容 — 但是，還是有其他需要調整大小的大小變更事件 (例如貼齊和填滿事件)。

-   接著，請設定適當的 2D 或 3D 矩陣轉換，將圖形管線中的像素或頂點轉譯至交換鏈結時，分別套用像素和頂點。 我們有 4 個可能的旋轉矩陣：

    -   橫向 (DXGI\_MODE\_ROTATION\_IDENTITY)
    -   直向 (DXGI\_MODE\_ROTATION\_ROTATE270)
    -   橫向翻轉 (DXGI\_MODE\_ROTATION\_ROTATE180)
    -   直向翻轉 (DXGI\_MODE\_ROTATION\_ROTATE90)

    根據提供的 windows 10 （例如[**displayinformation:: Orientationchanged**](https://msdn.microsoft.com/library/windows/apps/dn264268)的結果） 來判斷顯示方向資料選取正確矩陣，且會乘以每個像素 (Direct2D) 或頂點的座標(Direct3D) 中場景，有效地旋轉它們來對齊螢幕的方向。 (請注意，在 Direct2D 中，螢幕原點被定義為左上角，而在 Direct3D 中，原點則被定義為視窗的邏輯中心。)

> **注意：** 如需有關用於旋轉，以及如何定義它們的 2d 轉換的詳細資訊，請參閱[定義螢幕旋轉 (2d) 的矩陣](#appendix-a-applying-matrices-for-screen-rotation-2-d)。 如需有關用於旋轉的 3D 轉換，請參閱[定義螢幕旋轉的矩陣 (3D)](#appendix-b-applying-matrices-for-screen-rotation-3-d)。

 

現在，這裡是重點所在：呼叫 [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) 並將更新的旋轉矩陣提供給它，如下：

`m_swapChain->SetRotation(rotation);`

您也需要將選取的旋轉矩陣儲存在轉譯方法於計算新投影時能夠取得它的地方。 當您轉譯最終的 3D 投影或結合最終的 2D 配置時，將會使用這個矩陣。 (它不自動為您套用它。)

接著，請為旋轉的 3D 檢視建立新的轉譯目標，以及為檢視建立新的深度樣板緩衝區。 呼叫 [**ID3D11DeviceContext:RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480) 來為旋轉的場景設定 3D 轉譯檢視區。

最後，如果您有 2D 影像要旋轉或配置，請使用 [**ID2D1DeviceContext::CreateBitmapFromDxgiSurface**](https://msdn.microsoft.com/library/windows/desktop/hh404482) 將 2D 轉譯目標建立成可寫入的點陣圖，以供調整大小的交換鏈結使用，然後將新配置結合，以供更新的方向使用。 設定轉譯目標上您需要的任何屬性，例如消除鋸齒模式 (如程式碼範例中所見)。

現在，呈現交換鏈結。

## <a name="reduce-the-rotation-delay-by-using-corewindowresizemanager"></a>使用 CoreWindowResizeManager 降低旋轉延遲


根據預設，windows 10 提供一個簡短但明顯的任何應用程式，無論應用程式模型或語言，來完成影像旋轉的時間。 不過，可能的情況是，當您的應用程式使用這裡描述的其中一項技術執行旋轉運算時，會在這個時間範圍結束前完成運算。 您會想要取回那些時間並完成旋轉動畫，是嗎？ 這就是 [**CoreWindowResizeManager**](https://msdn.microsoft.com/library/windows/apps/jj215603) 派上用場的地方。

以下是 [**CoreWindowResizeManager**](https://msdn.microsoft.com/library/windows/apps/jj215603) 的用法：當 [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) 事件被引發時，請呼叫該事件之處理常式內的 [**CoreWindowResizeManager::GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh404170) 以取得 **CoreWindowResizeManager** 的執行個體，然後在新方向的配置完成並呈現時，呼叫 [**NotifyLayoutCompleted**](https://msdn.microsoft.com/library/windows/apps/jj215605) 讓 Windows 知道它可以完成旋轉動畫並顯示應用程式畫面。

[**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) 事件處理常式中的程式碼看起來如下：

```cpp
CoreWindowResizeManager^ resizeManager = Windows::UI::Core::CoreWindowResizeManager::GetForCurrentView();

// ... build the layout for the new display orientation ...

resizeManager->NotifyLayoutCompleted();
```

當使用者旋轉顯示器的方向時，windows 10 顯示一個動畫獨立於您的應用程式的意見反應給使用者。 該動畫包含三個部分，執行順序如下：

-   Windows 10 壓縮原始影像。
-   Windows 10 會將映像保留重建新配置所花的時間。 這是您會想要縮減的時間範圍，因為您的應用程式可能不需要全部的時間。
-   當配置時間範圍過了之後，或收到配置完成的通知時，Windows 會旋轉影像，然後淡入與淡出至新方向。

做為建議第三個項目符號中，當應用程式呼叫[**NotifyLayoutCompleted**](https://msdn.microsoft.com/library/windows/apps/jj215605)，windows 10 會停止逾時時間、 完成旋轉動畫，並傳回控制項至您的應用程式，這以新的顯示方向繪製。 整體的影響就是您的應用程式現在感覺比較有彈性和有回應，並且運作起來較有效率。

## <a name="appendix-a-applying-matrices-for-screen-rotation-2-d"></a>附錄 A：套用螢幕旋轉的矩陣 (2D)


在[重新調整交換鏈結的大小並預先旋轉其內容](#resizing-the-swap-chain-and-pre-rotating-its-contents) (以及 [DXGI 交換鏈結旋轉範例](http://go.microsoft.com/fwlink/p/?linkid=257600)) 的範例程式碼中，您可能已經注意到 Direct2D 輸出與 Direct3D 輸出有個別的旋轉矩陣。 讓我們先來看看 2D 矩陣。

不能在 Direct2D 和 Direct3D 內容套用相同旋轉矩陣的理由有兩個：

-   一、兩者使用不同的笛卡兒座標模型。 Direct2D 使用慣用右手規則，其中 Y 座標是從原點向上以正值遞增。 不過，Direct3D 是使用慣用左手規則，其中 Y 座標是從原點向右以正值遞增。 結果就是螢幕座標的原點會位在 Direct2D 的左上方，而螢幕 (投影平面) 的原點則是位在 Direct3D 的左下方。 (如需詳細資訊，請參閱 [3D 座標系統](https://msdn.microsoft.com/library/windows/apps/bb324490.aspx)。)

    ![direct3d 座標系統。](images/direct3d-origin.png)![direct2d 座標系統。](images/direct2d-origin.png)

-   二、必須明確指定 3D 旋轉矩陣，才能避免進位誤差。

交換鏈結會假設原點位於左下方，因此您必須執行旋轉，將慣用右手的 Direct2D 座標系統與交換鏈結所使用的慣用左手座標系統對齊。 更明確地說，您需要將旋轉矩陣乘以轉移矩陣 (Translation Matrix) 來算出旋轉的座標系統原點，然後將影像從 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 的座標空間轉換成交換鏈結的座標空間，將影像重新定位在新慣用左手的方向之下。 當 Direct2D 轉譯目標與交換鏈結相連時，您的應用程式也必須同樣地套用這項轉換。 不過，如果您的應用程式是繪製到並未與交換鏈結直接關聯的中繼介面上，則請勿套用這項座標空間轉換。

從四種可能的旋轉中選取正確矩陣的程式碼可能看起來如下 (請注意轉到新座標系統原點的轉移)：

```cpp
   
// Set the proper orientation for the swap chain, and generate 2D and
// 3D matrix transformations for rendering to the rotated swap chain.
// Note the rotation angle for the 2D and 3D transforms are different.
// This is due to the difference in coordinate spaces.  Additionally,
// the 3D matrix is specified explicitly to avoid rounding errors.

switch (displayRotation)
{
case DXGI_MODE_ROTATION_IDENTITY:
    m_orientationTransform2D = Matrix3x2F::Identity();
    m_orientationTransform3D = ScreenRotation::Rotation0;
    break;

case DXGI_MODE_ROTATION_ROTATE90:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(90.0f) *
        Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
    m_orientationTransform3D = ScreenRotation::Rotation270;
    break;

case DXGI_MODE_ROTATION_ROTATE180:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(180.0f) *
        Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
    m_orientationTransform3D = ScreenRotation::Rotation180;
    break;

case DXGI_MODE_ROTATION_ROTATE270:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(270.0f) *
        Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
    m_orientationTransform3D = ScreenRotation::Rotation90;
    break;

default:
    throw ref new FailureException();
}
    
```

在有了 2D 影像的正確旋轉矩陣和原點之後，請在您呼叫 [**ID2D1DeviceContext::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/dd371768) 和 [**ID2D1DeviceContext::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/dd371924) 之間以 [**ID2D1DeviceContext::SetTransform**](https://msdn.microsoft.com/library/windows/desktop/dd742857) 呼叫來設定它。

**警告** Direct2D 沒有轉換堆疊。 如果您應用程式的繪圖程式碼中也使用了 [**ID2D1DeviceContext::SetTransform**](https://msdn.microsoft.com/library/windows/desktop/dd742857)，則這個矩陣必須在後續乘以任何其他您已經套用的轉換。

 

```cpp
    ID2D1DeviceContext* context = m_deviceResources->GetD2DDeviceContext();
    Windows::Foundation::Size logicalSize = m_deviceResources->GetLogicalSize();

    context->SaveDrawingState(m_stateBlock.Get());
    context->BeginDraw();

    // Position on the bottom right corner.
    D2D1::Matrix3x2F screenTranslation = D2D1::Matrix3x2F::Translation(
        logicalSize.Width - m_textMetrics.layoutWidth,
        logicalSize.Height - m_textMetrics.height
        );

    context->SetTransform(screenTranslation * m_deviceResources->GetOrientationTransform2D());

    DX::ThrowIfFailed(
        m_textFormat->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_TRAILING)
        );

    context->DrawTextLayout(
        D2D1::Point2F(0.f, 0.f),
        m_textLayout.Get(),
        m_whiteBrush.Get()
        );

    // Ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = context->EndDraw();
```

下次呈現交換鏈結時，就會將您的 2D 影像旋轉成符合新的顯示方向。

## <a name="appendix-b-applying-matrices-for-screen-rotation-3-d"></a>附錄 B：套用螢幕旋轉的矩陣 (3D)


在[重新調整交換鏈結的大小並預先旋轉其內容](#resizing-the-swap-chain-and-pre-rotating-its-contents) (以及 [DXGI 交換鏈結旋轉範例](http://go.microsoft.com/fwlink/p/?linkid=257600)) 的範例程式碼中，我們為每個可能的螢幕方向定義了特定的轉換矩陣。 現在，讓我們看看旋轉 3D 場景的矩陣。 如先前一樣，您需要為 4 個可能方向中的每個方向建立一組矩陣。 為了防止進位誤差以至於輕微的視覺不自然感，請在程式碼中明確宣告這些矩陣。

您需要按照下列方式設定這些 3D 旋轉矩陣。 下列程式碼範例中示範的矩陣是標準的旋轉矩陣，用於 0、90、180 及 270 度的頂點 (定義相機 3D 場景空間中的各點) 旋轉。 計算 2D 場景投影時，會將場景中每個頂點的 \[x, y, z\] 座標值乘以這個旋轉矩陣。

```cpp
   
// 0-degree Z-rotation
static const XMFLOAT4X4 Rotation0( 
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 90-degree Z-rotation
static const XMFLOAT4X4 Rotation90(
    0.0f, 1.0f, 0.0f, 0.0f,
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 180-degree Z-rotation
static const XMFLOAT4X4 Rotation180(
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, -1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 270-degree Z-rotation
static const XMFLOAT4X4 Rotation270( 
    0.0f, -1.0f, 0.0f, 0.0f,
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );            
    }
```

您需要使用 [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) 呼叫來設定交換鏈結上的旋轉類型，如下：

`   m_swapChain->SetRotation(rotation);`

現在，在您的轉譯方法中，實作一些類似下列的程式碼：

``` syntax
struct ConstantBuffer // This struct is provided for illustration.
{
    // Other constant buffer matrices and data are defined here.

    float4x4 projection; // Current matrix for projection
} ;
ConstantBuffer  m_constantBufferData;          // Constant buffer resource data

// ...

// Rotate the projection matrix as it will be used to render to the rotated swap chain.
m_constantBufferData.projection = mul(m_constantBufferData.projection, m_rotationTransform3D);
```

現在，當您呼叫轉譯方法時，它會將目前的旋轉矩陣 (如類別變數 **m\_orientationTransform3D** 所指定) 乘以目前的投影矩陣，然後將該項運算的結果指派為您轉譯器的新投影矩陣。 呈現交換鏈結來查看更新了顯示方向的場景。

 

 




