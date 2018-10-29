---
author: mtoepke
title: DirectX 與 XAML 互通性
description: 您可以在通用 Windows 平台 (UWP) 遊戲中，同時使用 Extensible Application Markup Language (XAML) 與 Microsoft DirectX。
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, DirectX, XAML 互通性
ms.localizationpriority: medium
ms.openlocfilehash: 7f3a70be3dd31b0a5e4214621ab9fb4efa72cc54
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "5742178"
---
# <a name="directx-and-xaml-interop"></a>DirectX 與 XAML 互通性



您可以在通用 Windows 平台 (UWP) 遊戲或 App 中，同時使用 Extensible Application Markup Language (XAML) 與 Microsoft DirectX。 XAML 和 DirectX 的組合可讓您建置彈性的使用者介面架構，以便與 DirectX 轉譯的內容互通，而這對於具有大量圖形的 App 特別有用。 本主題說明使用 DirectX 的 UWP App 結構，並指出建置與 DirectX 搭配使用的 UWP App 時應使用的重要類型。

如果您的 App 著重在 2D 轉譯，您可能想要使用 [Win2D](https://github.com/microsoft/win2d) Windows 執行階段程式庫。 此程式庫是由 Microsoft 維護，且採用 Direct2D 核心技術。 它能大幅簡化實作 2D 圖形的使用模式，且包含本文件中所提及之某些技術的實用抽象。 如需詳細資訊，請參閱專案頁面。 此文件涵蓋的指南適用於 *「沒有」* 使用 Win2D 的 App 開發人員。

> **注意：** DirectX Api 不定義為 Windows 執行階段類型，因此您通常會使用 VisualC + + 元件延伸 (C + + /CX) 來開發與 DirectX 互通的 xamluwp XAML UWP 元件。 此外，如果您將 DirectX 呼叫包裝在個別的 Windows 執行階段中繼資料檔中，您就可以使用 C# 和 XAML 建立使用 DirectX 的 UWP App。

 

## <a name="xaml-and-directx"></a>XAML 和 DirectX

DirectX 可針對 2D 和 3D 圖形提供兩種強大的程式庫：Direct2D 和 Microsoft Direct3D。 雖然 XAML 可支援基本的 2D 基本形狀和效果，但許多應用程式 (例如模型和遊戲) 需要更複雜的圖形支援。 對於這類的應用程式，您可以使用 Direct2D 和 Direct3D 呈現局部或所有圖形，然後使用 XAML 處理其他作業。

若要實作自訂 XAML 和 DirectX 互通性，您需要了解這兩個概念：

-   共用表面是可調整大小的顯示區域，由 XAML 所定義，您可以使用 DirectX 直接在表面繪圖 (使用 [Windows::UI::Xaml::Media::ImageSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imagesource.aspx) 類型)。 針對共用表面，您不需控制新內容出現在畫面上的確切時間。 反之，共用表面的更新會同步處理至 XAML 架構的更新。
-   [交換鏈結](https://msdn.microsoft.com/library/windows/desktop/bb206356(v=vs.85).aspx)代表以最低延遲來顯示圖形的一組緩衝區。 一般來說，交換鏈結會以有別於 UI 執行序的方式，以每秒 60 畫面格的速度更新。 不過，交換鏈結會使用更多記憶體和 CPU 資源來支援快速更新，且較不易於使用，因為您必須管理多個執行序。

請考量您使用 DirectX 的目的。 用來繪製或以動畫製作在顯示視窗範圍內的單一控制項？ 是否會包含需要即時轉譯和控制的輸出，就像在遊戲中一樣？ 如果是這樣，您可能需要實作交換鏈結。 否則，使用共用表面應該就足夠。

決定 DirectX 的用途之後，您可以使用其中一個 Windows 執行階段類型，將 DirectX 轉譯納入您的 UWP app：

-   如果想製作靜態影像或在事件驅動間隔中繪製複雜的影像，請使用 [Windows::UI::Xaml::Media::Imaging::SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 在共用表面上 繪製。 這個類型會處理可調整大小的 DirectX 繪圖表面。 若要顯示在文件或 UI 元素中，您通常會使用這個類型將影像或紋理製作成點陣圖。 它不適用於即時互動 (例如需要高效能的遊戲)。 因為 **SurfaceImageSource** 物件的更新會與 XAML 使用者介面的更新同步，這會讓使用者在視覺回饋上感受到延遲，例如波動的畫面播放速率或遲緩的即時輸入回應。 不過，更新的速度仍足以進行動態控制或資料模擬！

-   如果影像大於提供的螢幕實際可用空間，且可以由使用者進行移動瀏覽及縮放，請使用 [Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050)。 這個類型可以處理大於螢幕的 DirectX 繪圖表面大小。 和 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 一樣，您會在製作複雜影像或進行動態控制時使用這個類型。 另外，也和 **SurfaceImageSource** 一樣，它不適用於需要高效能的遊戲。 一些可以使用 **VirtualSurfaceImageSource** 的 XAML 元素範例為地圖控制項，或大型的影像密集文件檢視器。

-   如果您使用 DirectX 來呈現即時更新的圖形，或處於必須在固定的低延遲間隔進行更新的情況下，請使用 [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) 類別，這樣您在重新整理圖形時就不必與 XAML 架構重新整理計時器同步。 這個類型可以讓您直接存取圖形裝置的交換鏈結 ([IDXGISwapChain1](https://msdn.microsoft.com/library/windows/desktop/hh404631))，並將 XAML 放在轉譯目標的最上層。 這個類型最適用於需要 XAML 使用者介面的遊戲和全螢幕 DirectX App。 您必須了解 DirectX 以使用此方法，包含 Microsoft DirectX Graphics Infrastructure (DXGI)、Direct2D 及 Direct3D 技術。 如需詳細資訊，請參閱 [Direct3D 11 的程式設計指南](https://msdn.microsoft.com/library/windows/desktop/ff476345)。

## <a name="surfaceimagesource"></a>SurfaceImageSource


[SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 提供用來繪圖的 DirectX 共用表面，然後將位元組合成應用程式內容。

下列是在程式碼後置中建立和更新 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 物件的基本處理程序：

1.  藉由傳遞 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 建構函式的高度和寬度，以定義共用表面的大小。 您也可以指出表面是否需要 Alpha (不透明度) 支援。

    例如：

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  取得 [ISurfaceImageSourceNativeWithD2D](https://msdn.microsoft.com/library/windows/desktop/dn302137) 的指標。 將 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 物件轉換為 [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821) (或 **IUnknown**)，並在其上呼叫 **QueryInterface** 以取得基礎 **ISurfaceImageSourceNativeWithD2D** 實作。 您使用這個實作中定義的方法來設定裝置並執行繪圖操作。

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* sisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);

    sisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **)&m_sisNativeWithD2D);
    ```

3.  藉由先呼叫 [D3D11CreateDevice](https://msdn.microsoft.com/library/windows/desktop/ff476082) 和 [D2D1CreateDevice](https://msdn.microsoft.com/library/windows/desktop/hh404272(v=vs.85).aspx)，接著將裝置和內容傳遞到 [ISurfaceImageSourceNativeWithD2D::SetDevice](https://msdn.microsoft.com/library/dn302141.aspx)，以建立 DXGI 和 D2D 裝置。 

    > [!NOTE]
    > 如果您將會從背景執行緒繪製到 **SurfaceImageSource**，您也需要確保 DXGI 裝置已啟用多執行緒存取。 基於效能考量，只有在從背景執行緒繪圖時，才必須執行此動作。

    例如：

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_sisNativeWithD2D->SetDevice(m_d2dDevice.Get());
    ```

4.  將 [ID2D1DeviceContext](https://msdn.microsoft.com/library/windows/desktop/bb174565) 物件指標提供給 [ISurfaceImageSourceNativeWithD2D::BeginDraw](https://msdn.microsoft.com/library/dn302138.aspx)，而且使用傳回的繪圖內容，在**SurfaceImageSource** 內繪製到您想要的矩形內容。 **ISurfaceImageSourceNativeWithD2D::BeginDraw** 以及繪製命令可從背景執行緒呼叫。 只會針對 *updateRect* 參數中指定要更新的區域進行繪圖。

    這個方法會傳回 *offset* 參數中已更新目標矩形的座標點 (x,y) 位移。 您可以使用此位移，判斷在哪裡使用 **ID2D1DeviceContext** 繪製更新的內容。

    ```cpp
    Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(
        updateRect, 
        &drawingContext, 
        &offset);

    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
        || beginDrawHR == DXGI_ERROR_DEVICE_RESET
        || beginDrawHR == D2DERR_RECREATE_TARGET)
    {
        // The D3D and D2D device was lost and need to be re-created.
        // Recovery steps are:
        // 1) Re-create the D3D and D2D devices
        // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the new D2D
        //    device
        // 3) Redraw the contents of the SurfaceImageSource
    }
    else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
    {
        // The devices were not lost but the entire contents of the surface
        // were. Recovery steps are:
        // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the D2D 
        //    device again
        // 2) Redraw the entire contents of the SurfaceImageSource
    }
    else 
    {
        // Draw your updated rectangle with the drawingContext
    }
    ```

5. 呼叫 [ISurfaceImageSourceNativeWithD2D::EndDraw](https://msdn.microsoft.com/library/dn302139.aspx) 以完成點陣圖。 該點陣圖可以當作 XAML [Image](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx) 或 [ImageBrush](https://msdn.microsoft.com/library/windows/apps/br210101) 的來源。 **ISurfaceImageSourceNativeWithD2D::EndDraw** 必須只能從 UI 執行緒呼叫。

    ```cpp
    m_sisNative->EndDraw();

    // ...
    // The SurfaceImageSource object's underlying 
    // ISurfaceImageSourceNativeWithD2D object contains the completed bitmap.

    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

    > [!NOTE]
    > 目前呼叫 [SurfaceImageSource::SetSource](https://msdn.microsoft.com/library/windows/apps/br243255) (繼承自 **IBitmapSource::SetSource**) 會擲回例外狀況。 不要從您的 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 物件呼叫它。

    > [!NOTE]
    > 當相關 [Window](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 隱藏時，應用程式必須避免繪製到 **SurfaceImageSource**，否則 **ISurfaceImageSourceNativeWithD2D** API 將會失敗。 若要完成此動作，請登錄為事件接聽程式，讓 [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) 事件追蹤可見度變更。

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource

當內容可能大於螢幕的可用空間時，[VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) 會擴充 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041)，因此必須虛擬化內容以達最佳呈現效果。

[VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) 不同於 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041)，它使用您實作的回呼 ([IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848337)) 在螢幕顯示表面區域時更新這些表面區域。 您不需要清除隱藏的區域，因為 XAML 架構會為您進行清除。

下列是在程式碼後置中建立和更新 [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) 物件的基本處理程序：

1.  根據您要的大小建立 [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) 執行個體。 例如：

    ```cpp
    VirtualSurfaceImageSource^ virtualSIS = 
        ref new VirtualSurfaceImageSource(2000, 2000);
    ```

2.  取得 [IVirtualSurfaceImageSourceNative](https://msdn.microsoft.com/library/windows/desktop/hh848328) 和 [ISurfaceImageSourceNativeWithD2D](https://msdn.microsoft.com/library/windows/desktop/dn302137) 的指標。 將 [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) 物件轉換為 [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821) 或 [IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)，並在其上呼叫 [QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521) 以取得基礎 **IVirtualSurfaceImageSourceNative** 和 **ISurfaceImageSourceNativeWithD2D** 實作。 您使用這些實作中定義的方法來設定裝置並執行繪圖操作。

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* vsisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);

    vsisInspectable->QueryInterface(
        __uuidof(IVirtualSurfaceImageSourceNative), 
        (void **) &m_vsisNative);

    vsisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **) &m_sisNativeWithD2D);
    ```

3.  藉由先呼叫 **D3D11CreateDevice** 和 **D2D1CreateDevice**，接著將 D2D 裝置傳遞到 **ISurfaceImageSourceNativeWithD2D::SetDevice**，以建立 DXGI 和 D2D 裝置。

    > [!NOTE]
    > 如果您將會從背景執行緒繪製到 **VirtualSurfaceImageSource**，您也需要確保 DXGI 裝置已啟用多執行緒存取。 基於效能考量，只有在從背景執行緒繪圖時，才必須執行此動作。

    例如：

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);  

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    
    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_vsisNativeWithD2D->SetDevice(m_d2dDevice.Get());

    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  呼叫 [IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848334)，將參照傳送至您實作的 [IVirtualSurfaceUpdatesCallbackNative](https://msdn.microsoft.com/library/windows/desktop/hh848336)。

    ```cpp
    class MyContentImageSource : public IVirtualSurfaceUpdatesCallbackNative
    {
    // ...
      private:
         virtual HRESULT STDMETHODCALLTYPE UpdatesNeeded() override;
    }

    // ...

    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
      // .. Perform drawing here ...
    }

    void MyContentImageSource::Initialize()
    {
      // ...
      m_vsisNative->RegisterForUpdatesNeeded(this);
      // ...
    }
    ```

    當 [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) 的區域需要更新時，架構會呼叫您的 [IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848334) 實作。

    當架構判斷需要繪製區域時 (例如當使用者進行表面的取景位置調整或縮放時)，或應用程式已經呼叫該區域上的 [IVirtualSurfaceImageSourceNative::Invalidate](https://msdn.microsoft.com/library/windows/desktop/hh848332) 後，就會進行這個動作。

5.  在 [IVirtualSurfaceImageSourceNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848337) 中，使用 [IVirtualSurfaceImageSourceNative::GetUpdateRectCount](https://msdn.microsoft.com/library/windows/desktop/hh848329) 和 [IVirtualSurfaceImageSourceNative::GetUpdateRects](https://msdn.microsoft.com/library/windows/desktop/hh848330) 方法來判斷必須繪製表面的哪些區域。

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  
            m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);

            std::unique_ptr<RECT[]> drawingBounds(
                new RECT[drawingBoundsCount]);

            m_vsisNative->GetUpdateRects(
                drawingBounds.get(), 
                drawingBoundsCount);
            
            for (ULONG i = 0; i < drawingBoundsCount; ++i)
            {
                // Drawing code here ...
            }
        }
        catch (Platform::Exception^ exception)
        {
            hr = exception->HResult;
        }

        return hr;
    }
    ```

6.  最後，針對每個必須更新的區域：

    1.  將 **ID2D1DeviceContext** 物件指標提供給 **ISurfaceImageSourceNativeWithD2D::BeginDraw**，而且使用傳回的繪圖內容，在**SurfaceImageSource** 內繪製到您想要的矩形內容。 **ISurfaceImageSourceNativeWithD2D::BeginDraw** 以及繪製命令可從背景執行緒呼叫。 只會針對 *updateRect* 參數中指定要更新的區域進行繪圖。

        這個方法會傳回 *offset* 參數中已更新目標矩形的座標點 (x,y) 位移。 您可以使用此位移，判斷在哪裡使用 **ID2D1DeviceContext** 繪製更新的內容。

        ```cpp
        Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

        HRESULT beginDrawHR = m_sisNative->BeginDraw(
            updateRect, 
            &drawingContext, 
            &offset);

        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
            || beginDrawHR == DXGI_ERROR_DEVICE_RESET 
            || beginDrawHR == D2DERR_RECREATE_TARGET)
        {
            // The D3D and D2D devices were lost and need to be re-created.
            // Recovery steps are:
            // 1) Re-create the D3D and D2D devices
            // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    new D2D device
            // 3) Redraw the contents of the VirtualSurfaceImageSource
        }
        else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
        {
            // The devices were not lost but the entire contents of the 
            // surface were lost. Recovery steps are:
            // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    D2D device again
            // 2) Redraw the entire contents of the VirtualSurfaceImageSource
        }
        else
        {
            // Draw your updated rectangle with the drawingContext
        }
        ```

    2.  將特定內容繪製到該區域，但將繪圖限制在有界限的區域，以取得較佳的效能。

    3.  呼叫 **ISurfaceImageSourceNativeWithD2D:EndDraw**。 結果是一個點陣圖。

> [!NOTE]
> 當相關 [Window](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 隱藏時，應用程式必須避免繪製到 **SurfaceImageSource**，否則 **ISurfaceImageSourceNativeWithD2D** API 將會失敗。 若要完成此動作，請登錄為事件接聽程式，讓 [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) 事件追蹤可見度變更。

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel 和遊戲


[SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) 是 Windows 執行階段類型，專門用於支援高階圖形和遊戲，您可以在此直接管理交換鏈結。 在這個案例中，您會建立自己的 DirectX 交換鏈結並管理呈現內容的顯示。

為了確保最佳的效能，[SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) 類型有部分限制：

-   每個 App 的 [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) 執行個體不能超過 4 個。
-   您應該將 DirectX 交換鏈結的高度和寬度 (在 [DXGI\_SWAP\_CHAIN\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528) 中) 設定成交換鏈結元素目前的尺寸。 如果沒有這麼做，顯示內容就會按縮放比例 (使用 **DXGI\_SCALING\_STRETCH**) 調整為最適合的大小。
-   您必須將 DirectX 交換鏈結的縮放模式 (在 [DXGI\_SWAP\_CHAIN\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528) 中) 設定為 **DXGI\_SCALING\_STRETCH**。
-   您不能將 DirectX 交換鏈結的 Alpha 模式 (在 [DXGI\_SWAP\_CHAIN\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528) 中) 設定為 **DXGI\_ALPHA\_MODE\_PREMULTIPLIED**。
-   您必須呼叫 [IDXGIFactory2::CreateSwapChainForComposition](https://msdn.microsoft.com/library/windows/desktop/hh404558) 來建立 DirectX 交換鏈結。

請根據應用程式的需求更新 [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834)，不是根據 XAML 架構的更新。 如果您需要將 **SwapChainPanel** 的更新與 XAML 架構的更新同步，請登錄 [Windows::UI::Xaml::Media::CompositionTarget::Rendering](https://msdn.microsoft.com/library/windows/apps/br228127) 事件。 否則，您必須在嘗試從 (更新 **SwapChainPanel** 的執行緒以外的) 其他執行緒更新 XAML 元素時，考量會發生的跨執行緒問題。

如果您的 **SwapChainPanel** 需要收到低延遲的指標輸入，請使用 [SwapChainPanel::CreateCoreIndependentInputSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource)。 這個方法會傳回 [CoreIndependentInputSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coreindependentinputsource) 物件，能用於以最低延遲從背景執行序取得輸入事件。 請注意，一旦呼叫這個方法，一般 XAML 指標輸入事件將不會對 **SwapChainPanel** 引發，因為所有輸入都會重新導向到背景執行序。


> **注意**   通常來說，您的 DirectX 應用程式應該橫向建立交換鏈結，並與顯示視窗大小相等 (這通常是大部分 Microsoft Store 遊戲中的原生螢幕解析度)。 這確保您的應用程式會在沒有任何可見的 XAML 重疊時使用最佳化的交換鏈結。 如果應用程式會旋轉為直向模式，則您的應用程式應該在現有的交換鏈結上呼叫 [IDXGISwapChain1::SetRotation](https://msdn.microsoft.com/library/windows/desktop/hh446801)、視需要將轉換套用到內容，然後在相同的交換鏈結上再次呼叫 [SetSwapChain](https://msdn.microsoft.com/library/windows/desktop/dn302144)。 同樣地，每當透過呼叫 [IDXGISwapChain::ResizeBuffers](https://msdn.microsoft.com/library/windows/desktop/bb174577) 調整交換鏈結大小時，您的應用程式都應該在相同的交換鏈結上再次呼叫 **SetSwapChain**。


 

下列是在程式碼後置中建立和更新 [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) 物件的基本處理程序：

1.  為應用程式取得交換鏈結面板的執行個體。 該執行個體在 XAML 中以 `<SwapChainPanel>` 標記表示。

    `Windows::UI::Xaml::Controls::SwapChainPanel^ swapChainPanel;`

    這裡是 `<SwapChainPanel>` 標記的範例。

    ```xml
    <SwapChainPanel x:Name="swapChainPanel">
        <SwapChainPanel.ColumnDefinitions>
            <ColumnDefinition Width="300*"/>
            <ColumnDefinition Width="1069*"/>
        </SwapChainPanel.ColumnDefinitions>
    …
    ```

2.  取得 [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143) 的指標。 將 [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) 物件轉換為 [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821) (或 **IUnknown**)，並在其上呼叫 **QueryInterface** 以取得基礎 **ISwapChainPanelNative** 實作。

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  建立 DXGI 裝置和交換鏈結，然後將交換鏈結傳送至 [SetSwapChain](https://msdn.microsoft.com/library/windows/desktop/dn302144) 以設定成 [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143)。

    ```cpp
    Microsoft::WRL::ComPtr<IDXGISwapChain1>               m_swapChain;    
    // ...
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
            swapChainDesc.Width = m_bounds.Width;
            swapChainDesc.Height = m_bounds.Height;
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;           // This is the most common swapchain format.
            swapChainDesc.Stereo = false; 
            swapChainDesc.SampleDesc.Count = 1;                          // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Quality = 0;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.BufferCount = 2;
            swapChainDesc.Scaling = DXGI_SCALING_STRETCH;
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // We recommend using this swap effect for all. applications
            swapChainDesc.Flags = 0;
                    
    // QI for DXGI device
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // Get the DXGI adapter.
    Microsoft::WRL::ComPtr<IDXGIAdapter> dxgiAdapter;
    dxgiDevice->GetAdapter(&dxgiAdapter);

    // Get the DXGI factory.
    Microsoft::WRL::ComPtr<IDXGIFactory2> dxgiFactory;
    dxgiAdapter->GetParent(__uuidof(IDXGIFactory2), &dxgiFactory);
    // Create a swap chain by calling CreateSwapChainForComposition.
    dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,        // Allow on any display. 
                &m_swapChain
                );
            
    m_swapChainNative->SetSwapChain(m_swapChain.Get());
    ```

4.  繪製 DirectX 交換鏈結，然後呈現它以顯示內容。

    ```cpp
    HRESULT hr = m_swapChain->Present(1, 0);
    ```

    當 Windows 執行階段配置/轉譯邏輯發出更新時，就會重新整理 XAML 元素。

## <a name="related-topics"></a>相關主題

* [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm)
* [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041)
* [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050)
* [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834)
* [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143)
* [Direct3D 11 的程式設計指南](https://msdn.microsoft.com/library/windows/desktop/ff476345)

 

 




