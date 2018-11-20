---
author: joannaleecy
title: 延伸遊戲範例
description: 了解如何實作 UWP DirectX 遊戲的 XAML 重疊。
keywords: DirectX, XAML
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cb837965746eb1c2c7deab613eec239a83cac294
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7278630"
---
# <a name="extend-the-game-sample"></a>延伸遊戲範例

現在，我們已經討論基本通用 Windows 平台 (UWP) DirectX 3D 遊戲的關鍵元件。 您可以設定遊戲的架構，包含檢視提供者和轉譯管線，以及實作基本的遊戲迴圈。 您也可以建立基本的使用者介面重疊，並納入音效和實作控制項。 您已開始建立自己的遊戲，但如果您需要更多協助和資訊，請查看這些資源。

-   [DirectX 圖形與遊戲](https://msdn.microsoft.com/library/windows/desktop/ee663274)
-   [Direct3D 11 概觀](https://msdn.microsoft.com/library/windows/desktop/ff476345)
-   [Direct3D 11 參考](https://msdn.microsoft.com/library/windows/desktop/ff476147)

## <a name="using-xaml-for-the-overlay"></a>用於重疊的 XAML


有一個替代方法我們沒有深入討論，那就是在重疊使用 XAML，而不使用 [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990)。 XAML 對於 Direct2D 有許多好處，可繪製使用者介面元素。 最重要的好處是它讓您將 windows 10 的外觀及操作功能整合到您的 DirectX 遊戲更方便。 許多定義 UWP 應用程式的常用元素、樣式以及行為已緊密整合到 XAML 模型，讓遊戲開發人員需要的實作的工作變得更少。 如果您的遊戲設計具有很複雜的使用者介面，請考慮使用 XAML 而不要使用 Direct2D。

使用 XAML，我們可以讓遊戲介面看起來類似 Direct2D 的舊版本。

### <a name="xaml"></a>XAML
![XAML 重疊](./images/simple-dx-game-extend-xaml.PNG)

### <a name="direct2d"></a>Direct2D
![D2D 重疊](./images/simple-dx-game-extend-d2d.PNG)

當有類似的最終結果時，Direct2D 與 XAML 介面間的實作也有許多差異。

功能 | XAML| Direct2D
:----------|:----------- | :-----------
定義重疊 | 已在 XAML 檔案 `\*.xaml` 中定義。 一旦了解 XAML，相較於 Direct2D 建立和設定較複雜的重疊變得更容易。| 定義為 Direct2D 基本類型的集合，而 [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) 字串是手動放置和撰寫到 Direct2D 目標緩衝區。 
使用者介面元素 | XAML 使用者介面元素來自標準化元素，它們屬於 Windows 執行階段 XAML API 的一部分，包括 [**Windows::UI::Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) 和 [**Windows::UI::Xaml::Controls**](https://msdn.microsoft.com/library/windows/apps/br227716)。 處理 XAML 使用者介面元素行為的程式碼定義在程式碼後置檔案 Main.xaml.cpp 中。 | 簡單圖形可以繪製成類似矩形和省略符號。
調整視窗大小 | 自然地處理重新調整大小和檢視狀態變更事件，並依變更轉換重疊 | 需要手動指定重新繪製重疊的元件。


另一個大不相同的點包含[交換鏈結](https://docs.microsoft.com/windows/uwp/graphics-concepts/swap-chains)。 您不必附加交換鏈結到 [**Windows::UI::Core::CoreWindow**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow) 物件。 相反地，在建構新的 [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel) 物件時，由納入 XAML 的 DirectX App 關聯交換鏈結。 

下列程式碼片段顯示如何宣告 XAML 適用於 **SwapChainPanel** (在 [**DirectXPage.xaml**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml) 檔案中)。
```xml
<Page
    x:Class="Simple3DGameXaml.DirectXPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:Simple3DGameXaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">


    <SwapChainPanel x:Name="DXSwapChainPanel">

    <!-- ... XAML user controls and elements -->

    </SwapChainPanel>
</Page>
```

**SwapChainPanel** 物件設定為 App 單例在[啟動](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp#L45-L51) 時建立的目前視窗物件的 [**Content**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.Content) 屬性。

```cpp
void App::OnLaunched(_In_ LaunchActivatedEventArgs^ /* args */)
{
    m_mainPage = ref new DirectXPage();

    Window::Current->Content = m_mainPage;
    // Bring the application to the foreground so that it's visible
    Window::Current->Activate();
}
```


如果要將設定的交換鏈結附加到 XAML 定義的 [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 執行個體，就必須取得基礎原生 [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/dn302143) 介面實作的指標，並在上面呼叫 [**ISwapChainPanelNative::SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144)，將設定的交換鏈結傳遞給它。 

來自 [**DX::DeviceResources::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/Common/DeviceResources.cpp#L218-L521) 的下列程式碼片段詳述 DirectX/XAML interop：

```cpp
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

        // When using XAML interop, the swap chain must be created for composition.
        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Associate swap chain with SwapChainPanel
        // UI changes will need to be dispatched back to the UI thread
        m_swapChainPanel->Dispatcher->RunAsync(CoreDispatcherPriority::High, ref new DispatchedHandler([=]()
        {
            // Get backing native interface for SwapChainPanel
            ComPtr<ISwapChainPanelNative> panelNative;
            DX::ThrowIfFailed(
                reinterpret_cast<IUnknown*>(m_swapChainPanel)->QueryInterface(IID_PPV_ARGS(&panelNative))
                );
            DX::ThrowIfFailed(
                panelNative->SetSwapChain(m_swapChain.Get())
                );
        }, CallbackContext::Any));

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }
```

如需這個程序的詳細資訊，請參閱 [DirectX 與 XAML 互通性](directx-and-xaml-interop.md)。

## <a name="sample"></a>範例

若要下載使用 XAML 為重疊的此遊戲版本，請移至 [Direct3D 射擊遊戲範例 (XAML)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameXaml)。


與其他主題中討論的遊戲範例版本不同，XAML 版本會分別在 [App.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp) 和 [DirectXPage.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml.cpp) 檔案中定義它的架構，而不是在  [App.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/App.cpp) 和 [GameInfoOverlay.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp) 檔案中定義。