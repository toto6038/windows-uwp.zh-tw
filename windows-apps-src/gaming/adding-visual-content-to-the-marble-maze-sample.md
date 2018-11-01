---
author: eliotcowley
title: 在 Marble Maze 範例中加入視覺化內容
description: 本文件說明 Marble Maze 遊戲如何在通用 Windows 平台 (UWP) app 環境中使用 Direct3D 和 Direct2D，讓您能夠了解各種模式，以在編寫您的遊戲內容時運用這些模式。
ms.assetid: 6e43422e-e1a1-b79e-2c4b-7d5b4fa88647
ms.author: elcowle
ms.date: 09/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 範例, DirectX, 圖形
ms.localizationpriority: medium
ms.openlocfilehash: 5171578b844829ec590b654194639ed6c8ebbfe1
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5922726"
---
# <a name="adding-visual-content-to-the-marble-maze-sample"></a>在 Marble Maze 範例中加入視覺化內容




本文件說明 Marble Maze 遊戲如何在通用 Windows 平台 (UWP) app 環境中使用 Direct3D 和 Direct2D，讓您能夠了解各種模式，以編寫適合這些模式的遊戲內容。 若要了解視覺化遊戲元件如何融入 Marble Maze 的整個應用程式結構中，請參閱 [Marble Maze 應用程式結構](marble-maze-application-structure.md)。

我們會依照下列基本步驟來開發 Marble Maze 的視覺外觀：

1.  建立初始化 Direct3D 和 Direct2D 環境的基本架構。
2.  使用影像和模型編輯程式來設計遊戲中出現的 2D 與 3D 資產。
3.  請確定 2D 與 3D 資產適當載入和遊戲中出現。
4.  整合頂點和像素著色器，提高遊戲資產的視覺品質。
5.  整合遊戲邏輯，例如動畫和使用者輸入。

我們也會先專注在新增 3D 資產，然後 2D 資產。 例如，我們會先專注在核心遊戲邏輯，然後才新增功能表系統和計時器。

在開發過程中，我們也需要多次反覆執行其中的一些步驟。 例如，當我們變更網格和彈珠模型，我們也必須變更一些支援這些模型的著色器程式碼。

> [!NOTE]
> 與本文件對應的範例程式碼可以在 [DirectX Marble Maze 遊戲範例](http://go.microsoft.com/fwlink/?LinkId=624011)中找到。

 
針對處理 DirectX 和視覺化遊戲內容方面，也就是初始化 DirectX 圖庫、載入場景資源及更新和呈現場景，以下是本文件討論的一些重點：

-   新增遊戲內容通常需要許多步驟。 往往也需要反覆執行這些步驟。 遊戲開發人員通常先專注在新增 3D 遊戲內容，然後再新增 2D 內容。
-   盡可能支援各種圖形硬體來吸引更多客戶，讓他們盡情享受。
-   清楚區隔設計階段和執行階段格式。 建構您的設計階段資產來發揮最大彈性和快速反覆處理內容。 格式化和壓縮您的資產，盡可能在執行階段有效率地載入並呈現。
-   在 UWP App 中建立 Direct3D 和 Direct2D 裝置的方式，和在 Windows 傳統型應用程式中進行的方式很像。 有個重要的差別在於交換鏈結與輸出視窗關聯的方式。
-   當您設計遊戲時，請確定您選擇的網格格式支援您的主要案例。 例如，如果遊戲需要碰撞，請確定您可以從網格中取得碰撞資料。
-   先更新所有場景物件再呈現物件，將遊戲邏輯和呈現邏輯區分開來。
-   您通常會繪製 3D 場景物件，並再任何 2D 物件，會出現在場景最前面。
-   同步處理繪圖到垂直空白，以確定遊戲不浪費時間繪製事實上永遠不會出現在顯示器上的畫面。 *垂直空白*是一個畫面完成到監視器繪製之後的時間和下一個畫面開始。

## <a name="getting-started-with-directx-graphics"></a>DirectX 圖形入門


當我們規劃 Marble Maze 通用 Windows 平台 (UWP) 遊戲時，我們選擇 c + + 和 Direct3D 11.1 是因為它們是建立掌控呈現和高效能的 3D 遊戲的絕佳選擇。 DirectX 11.1 支援 DirectX 9 到 DirectX 11 的硬體，因此您不需要為過去的每一版 DirectX 重寫程式碼，所以能協助您更有效率地吸引更多的客戶。

Marble Maze 使用 Direct3D 11.1 來呈現 3D 遊戲資產，也就是彈珠和迷宮。 Marble Maze 也使用 Direct2D、 DirectWrite 及 Windows 影像處理元件 (WIC) 來繪製 2D 遊戲資產，例如功能表和計時器。

遊戲開發需要規劃。 如果您是 DirectX 圖形的新手，我們建議您閱讀[DirectX： 入門](directx-getting-started.md)來自行熟悉建立 UWP DirectX 遊戲的基本概念。 當您閱讀本文件和研究 Marble Maze 原始程式碼時，您可以參考 DirectX 圖形更深入資訊的下列資源：

-   [Direct3D 11 圖形](https://msdn.microsoft.com/library/windows/desktop/ff476080)： 描述 Direct3D 11，一種功能強大、 硬體加速 3D 圖形 API 來轉譯 3D 幾何在 Windows 平台。
-   [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990)： 描述 Direct2D，這是硬體加速的 2D 圖形 API，其提供高效能和高品質來呈現 2D 幾何、 點陣圖和文字。
-   [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038)： 描述支援轉譯高品質文字的 DirectWrite。
-   [Windows 影像處理元件](https://msdn.microsoft.com/library/windows/desktop/ee719902)： 描述 wic，一種可延伸的平台，提供數位影像的低階 API。

### <a name="feature-levels"></a>功能層級

Direct3D 11 引進了名為*功能層級*」 的開發架構。 功能層級是一組妥善定義的 GPU 功能。 使用功能層級將遊戲的目標設定在舊版 Direct3D 硬體上執行。 Marble Maze 支援功能層級 9.1，原因是它不需要較高層級的進階功能。 建議您盡可能擴大硬體支援範圍，並調整遊戲內容，讓使用高階或低階電腦的客戶，都能盡情享受您的遊戲。 如需功能層級的詳細資訊，請參閱[舊版硬體上的 Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476872)。

## <a name="initializing-direct3d-and-direct2d"></a>初始化 Direct3D 和 Direct2D


「裝置」代表顯示卡。 在 UWP App 中建立 Direct3D 和 Direct2D 裝置的方式，和在 Windows 傳統型應用程式中進行的方式很像。 主要差異是如何將 Direct3D 交換鏈結連接至視窗系統。

**DeviceResources** 類別是管理 Direct3D 和 Direct2D 的基礎。 這個類別處理一般基礎結構，而不是遊戲特定資產。 Marble Maze 會定義**MarbleMazeMain**類別來處理遊戲特定資產，它包含可讓它存取 Direct3D 和 Direct2D 的**DeviceResources**物件的參考。

在初始化期間， **DeviceResources**建構函式會建立與裝置無關的資源及 Direct3D 和 Direct2D 裝置。

```cpp
// Initialize the Direct3D resources required to run. 
DX::DeviceResources::DeviceResources() :
    m_screenViewport(),
    m_d3dFeatureLevel(D3D_FEATURE_LEVEL_9_1),
    m_d3dRenderTargetSize(),
    m_outputSize(),
    m_logicalSize(),
    m_nativeOrientation(DisplayOrientations::None),
    m_currentOrientation(DisplayOrientations::None),
    m_dpi(-1.0f),
    m_deviceNotify(nullptr)
{
    CreateDeviceIndependentResources();
    CreateDeviceResources();
}
```

**DeviceResources** 類別會分割這項功能，因此能更輕鬆順應環境改變。 例如，當視窗大小變更時，它會呼叫 **CreateWindowSizeDependentResources** 方法。

###  <a name="initializing-the-direct2d-directwrite-and-wic-factories"></a>初始化 Direct2D、DirectWrite 及 WIC Factory

**DeviceResources::CreateDeviceIndependentResources** 方法會建立 Direct2D、DirectWrite 及 WIC 的 Factory。 在 DirectX 圖形中，Factory 是建立圖形資源的起點。 Marble Maze 指定 **D2D1\_FACTORY\_TYPE\_SINGLE\_THREADED** 的原因是，它會在主執行緒上執行所有繪製。

```cpp
// These are the resources required independent of hardware. 
void DX::DeviceResources::CreateDeviceIndependentResources()
{
    // Initialize Direct2D resources.
    D2D1_FACTORY_OPTIONS options;
    ZeroMemory(&options, sizeof(D2D1_FACTORY_OPTIONS));

#if defined(_DEBUG)
    // If the project is in a debug build, enable Direct2D debugging via SDK Layers.
    options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

    // Initialize the Direct2D Factory.
    DX::ThrowIfFailed(
        D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            __uuidof(ID2D1Factory2),
            &options,
            &m_d2dFactory
            )
        );

    // Initialize the DirectWrite Factory.
    DX::ThrowIfFailed(
        DWriteCreateFactory(
            DWRITE_FACTORY_TYPE_SHARED,
            __uuidof(IDWriteFactory2),
            &m_dwriteFactory
            )
        );

    // Initialize the Windows Imaging Component (WIC) Factory.
    DX::ThrowIfFailed(
        CoCreateInstance(
            CLSID_WICImagingFactory2,
            nullptr,
            CLSCTX_INPROC_SERVER,
            IID_PPV_ARGS(&m_wicFactory)
            )
        );
}
```

###  <a name="creating-the-direct3d-and-direct2d-devices"></a>建立 Direct3D 和 Direct2D 裝置

**DeviceResources::CreateDeviceResources** 方法會呼叫 [D3D11CreateDevice](https://msdn.microsoft.com/library/windows/desktop/ff476082) 來建立代表 Direct3D 顯示卡的裝置物件。 因為 Marble Maze 支援功能層級 9.1 及以上的**deviceresources:: Createdeviceresources**方法**featureLevels**陣列指定層級 9.1 到 11.1。 Direct3D 會依序瀏覽清單，並將第一個可用的功能層級提供給 App。 因此**D3D\_FEATURE\_LEVEL**陣列項目會從最高列到最低，以便在應用程式將會取得可用的最高功能層級。 **DeviceResources::CreateDeviceResources** 方法會查詢從 **D3D11CreateDevice** 傳回的 Direct3D 11 裝置，以取得 Direct3D 11.1 裝置。

```cpp
// This flag adds support for surfaces with a different color channel ordering
// than the API default. It is required for compatibility with Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
    if (DX::SdkLayersAvailable())
    {
        // If the project is in a debug build, enable debugging via SDK Layers 
        // with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
    }
#endif

// This array defines the set of DirectX hardware feature levels this app will support.
// Note the ordering should be preserved.
// Don't forget to declare your application's minimum required feature level in its
// description.  All applications are assumed to support 9.1 unless otherwise stated.
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
ComPtr<ID3D11DeviceContext> context;

HRESULT hr = D3D11CreateDevice(
    nullptr,                    // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,   // Create a device using the hardware graphics driver.
    0,                          // Should be 0 unless the driver is D3D_DRIVER_TYPE_SOFTWARE.
    creationFlags,              // Set debug and Direct2D compatibility flags.
    featureLevels,              // List of feature levels this app can support.
    ARRAYSIZE(featureLevels),   // Size of the list above.
    D3D11_SDK_VERSION,          // Always set this to D3D11_SDK_VERSION for UWP apps.
    &device,                    // Returns the Direct3D device created.
    &m_d3dFeatureLevel,         // Returns feature level of device created.
    &context                    // Returns the device immediate context.
    );

if (FAILED(hr))
{
    // If the initialization fails, fall back to the WARP device.
    // For more information on WARP, see:
    // http://go.microsoft.com/fwlink/?LinkId=286690
    DX::ThrowIfFailed(
        D3D11CreateDevice(
            nullptr,
            D3D_DRIVER_TYPE_WARP, // Create a WARP device instead of a hardware device.
            0,
            creationFlags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &device,
            &m_d3dFeatureLevel,
            &context
            )
        );
}

// Store pointers to the Direct3D 11.1 API device and immediate context.
DX::ThrowIfFailed(
    device.As(&m_d3dDevice)
    );

DX::ThrowIfFailed(
    context.As(&m_d3dContext)
    );
```

**DeviceResources::CreateDeviceResources** 方法接著會建立 Direct2D 裝置。 Direct2D 使用 Microsoft DirectX Graphics Infrastructure (DXGI) 來與 Direct3D 相互溝通。 DXGI 可在圖形執行階段之間共用視訊記憶體表面。 Marble Maze 使用 Direct3D 裝置的基礎 DXGI 裝置，從 Direct2D Factory 建立 Direct2D 裝置。

```cpp
// Create the Direct2D device object and a corresponding context.
ComPtr<IDXGIDevice3> dxgiDevice;
DX::ThrowIfFailed(
    m_d3dDevice.As(&dxgiDevice)
    );

DX::ThrowIfFailed(
    m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
    );

DX::ThrowIfFailed(
    m_d2dDevice->CreateDeviceContext(
        D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
        &m_d2dContext
        )
    );
```

如需 DXGI 及 Direct2D 和 Direct3D 之間互通性的詳細資訊，請參閱 [DXGI 概觀](https://msdn.microsoft.com/library/windows/desktop/bb205075)與 [Direct2D 和 Direct3D 互通性概觀](https://msdn.microsoft.com/library/windows/desktop/dd370966)。

### <a name="associating-direct3d-with-the-view"></a>建立 Direct3D 與檢視的關聯

**DeviceResources::CreateWindowSizeDependentResources** 方法會建立依存於特定視窗大小的圖形資源，例如交換鏈結，以及 Direct3D 和 Direct2D 呈現目標。 DirectX UWP app 與傳統型應用程式間有個重要差異，就是交換鏈結與輸出視窗建立關聯的方式。 交換鏈結負責顯示裝置要在監視器上呈現的緩衝區。 [Marble Maze 應用程式結構](marble-maze-application-structure.md)描述 UWP 應用程式的視窗系統有兩點不同於傳統型應用程式。 因為 UWP 應用程式不使用[HWND](https://msdn.microsoft.com/library/windows/desktop/aa383751)物件，Marble Maze 必須使用[idxgifactory2:: Createswapchainforcorewindow](https://msdn.microsoft.com/library/windows/desktop/hh404559)方法將裝置輸出關聯至檢視。 下列範例顯示 **DeviceResources::CreateWindowSizeDependentResources** 方法中負責建立交換鏈結的部分。

```cpp
// Obtain the final swap chain for this window from the DXGI factory.
DX::ThrowIfFailed(
    dxgiFactory->CreateSwapChainForCoreWindow(
        m_d3dDevice.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &m_swapChain
        )
    );
```

為了將耗電量降到最低，這在由電池供電的裝置上很重要 (例如膝上型電腦和平板電腦)，**DeviceResources::CreateWindowSizeDependentResources** 方法會呼叫 [IDXGIDevice1::SetMaximumFrameLatency](https://msdn.microsoft.com/library/windows/desktop/ff471334) 方法，以確保只在垂直空白之後才呈現遊戲。 與垂直空白進行同步處理會進一步說明此文件中[顯示場景](#presenting-the-scene)的區段中所述。

```cpp
// Ensure that DXGI does not queue more than one frame at a time. This both 
// reduces latency and ensures that the application will only render after each
// VSync, minimizing power consumption.
DX::ThrowIfFailed(
    dxgiDevice->SetMaximumFrameLatency(1)
    );
```

**DeviceResources::CreateWindowSizeDependentResources** 方法以適合大部分遊戲的方式來初始化圖形資源。

> [!NOTE]
> *檢視*」 一詞會有不同的意義在 Windows 執行階段和 Direct3D 中。 在 Windows 執行階段中，檢視是指應用程式使用者介面設定的集合，包括顯示區域和輸入行為及用來處理的執行緒。 當您建立檢視時，您可以指定所需的組態和設定。 [Marble Maze 應用程式結構](marble-maze-application-structure.md)中會說明設定 App 檢視的程序。
> 在 Direct3D 中，「檢視」一詞有多種意義。 資源檢視可定義資源可以存取的子資源。 例如，當紋理物件與著色器資源檢視相關聯時，該著色器稍後可存取紋理。 資源檢視的一項優點是在呈現管線的不同階段中，您可以採取不同的方式來解譯資料。 如需資源檢視的詳細資訊，請參閱[資源檢視](https://msdn.microsoft.com/library/windows/desktop/ff476900#Views)。
> 在檢視轉換或檢視轉換矩陣的環境下使用時，檢視是指相機的位置和方向。 檢視轉換會依相機的位置和方向來重新定位全世界的物件。 如需檢視轉換的詳細資訊，請參閱[檢視轉換 (Direct3D 9)](https://msdn.microsoft.com/library/windows/desktop/bb206342)。 本主題會進一步說明 Marble Maze 如何使用資源和矩陣檢視。

 

## <a name="loading-scene-resources"></a>載入場景資源


Marble Maze 使用**BasicLoader.h**中宣告， **BasicLoader**類別，來載入紋理和著色器。 Marble Maze 使用**SDKMesh**類別來載入迷宮和彈珠的 3D 網格。

為了確保應用程式具有好的回應能力，Marble Maze 會以非同步方式或在背景載入場景資源。 當資產在背景中載入時，您的遊戲可以回應視窗事件。 這個程序會在本指南的[在背景中載入遊戲資產](marble-maze-application-structure.md#loading-game-assets-in-the-background)中詳細說明。

###  <a name="loading-the-2d-overlay-and-user-interface"></a>載入 2D 覆疊和使用者介面

在 Marble Maze 中，覆疊是出現在螢幕最上面的影像。 覆疊一律出現在場景的最前面。 在 Marble Maze 中，覆疊包含 Windows 標誌和文字字串**DirectX Marble Maze 遊戲範例**。 重疊的管理是由**SampleOverlay.h**中定義**SampleOverlay**類別，來執行。 雖然我們在 Direct3D 範例中使用覆疊，但您可以調整這段程式碼，在場景最前面顯示任何影像。

覆疊有一個重要概念，因為內容不會變更，所以 **SampleOverlay** 類別在初始化期間會將內容繪製 (或快取) 到 [ID2D1Bitmap1](https://msdn.microsoft.com/library/windows/desktop/hh404349) 物件。 在繪製階段，**SampleOverlay** 類別只需要將點陣圖繪製到螢幕。 如此一來，就不必為每一個畫面執行高度耗費資源的常式 (例如文字繪製)。

使用者介面 (UI) 所組成 2D 元件，例如功能表和抬頭顯示器 (Hud)，會出現在場景最前面。 Marble Maze 會定義下列 UI 元素：

-   使用者啟動遊戲或檢視高分記錄的功能表項目。
-   在遊戲開始前倒數三秒的計時器。
-   追蹤已完遊戲時間的計時器。
-   列出最快完成時間的表格。
-   當遊戲暫停時顯示**暫停**的文字。

Marble Maze 在**UserInterface.h**中定義遊戲特有的 UI 元素。 Marble Maze 定義 **ElementBase** 類別做為所有 UI 元素的基底類別。 **ElementBase** 類別定義 UI 元素的大小、位置、對齊和可見度等屬性。 它也控制如何更新和呈現元素。

```cpp
class ElementBase
{
public:
    virtual void Initialize() { }
    virtual void Update(float timeTotal, float timeDelta) { }
    virtual void Render() { }

    void SetAlignment(AlignType horizontal, AlignType vertical);
    virtual void SetContainer(const D2D1_RECT_F& container);
    void SetVisible(bool visible);

    D2D1_RECT_F GetBounds();

    bool IsVisible() const { return m_visible; }

protected:
    ElementBase();

    virtual void CalculateSize() { }

    Alignment       m_alignment;
    D2D1_RECT_F     m_container;
    D2D1_SIZE_F     m_size;
    bool            m_visible;
};
```

**UserInterface** 類別 (可管理使用者介面) 提供 UI 元素的通用基底類別，因此只需要保留 **ElementBase** 物件的集合，簡化 UI 管理並提供可重複使用的使用者介面管理員。 Marble Maze 會定義衍生自 **ElementBase** 的類別，以實作遊戲特有的行為。 例如，**HighScoreTable** 會定義計分排行榜的行為。 如需這些類別的詳細資訊，請參閱原始程式碼。

> [!NOTE]
> 由於 XAML 可讓您更輕鬆地建立複雜的使用者介面，例如模擬和戰略遊戲中的介面，請考慮是否要使用 XAML 來定義您的 UI。 如需如何使用 XAML 開發使用者介面的 DirectX UWP 遊戲中的資訊，請參閱[延伸遊戲範例中](tutorial-resources.md)，指的是 DirectX 3D 射擊遊戲範例。

 

###  <a name="loading-shaders"></a>載入著色器

Marble Maze 使用 **BasicLoader::LoadShader** 方法從檔案載入著色器。

著色器是現今遊戲中 GPU 程式設計的基本單位。 幾乎所有 3D 圖形處理都是透過著色器，不論是模型轉換和場景光源，還是更複雜的幾何處理，範圍從人物膚色到鑲嵌驅動。 如需著色器程式設計模型的相關資訊，請參閱 [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561)。

Marble Maze 使用頂點和像素著色器。 頂點著色器一律在一個輸入頂點上運算，然後產生一個頂點做為輸出。 像素著色器接受數值、紋理資料、內插的各頂點值和其他資料，以產生像素色彩做為輸出。 因為著色器一次只轉換一個元素，如果圖形硬體提供多個著色器管線，則可以平行處理多組元素。 GPU 可用的平行管線數目可能明顯大於 CPU 可用的數目。 因此，即使是基本著色器，也可以大幅改進輸送量。

**Marblemazemain:: Loaddeferredresources**方法會載入一個頂點著色器和一個像素著色器載入覆疊之後。 這些著色器的設計階段版本分別定義於**BasicVertexShader.hlsl**和**BasicPixelShader.hlsl**。 Marble Maze 在呈現階段會將這些著色器套用至彈珠和迷宮。

Marble Maze 專案包含 .hlsl (設計階段格式) 和 .cso (執行階段格式) 版本的著色器檔案。 在建置時，Visual Studio 會使用 fxc.exe 效果編譯器，將 .hlsl 原始程式檔編譯成 .cso 二進位著色器。 如需效果編譯器工具的詳細資訊，請參閱[效果編譯器工具](https://msdn.microsoft.com/library/windows/desktop/bb232919)。

頂點著色器使用提供的模型、檢視和投影矩陣，轉換輸入幾何。 輸入幾何的位置資料會進行兩次轉換和輸出：一次是在螢幕空間 (為了呈現)，第二次是在世界空間 (為了讓像素著色器執行光源計算)。 表面標準向量會轉換成世界空間，像素著色器也會使用此空間來提供光源。 紋理座標會原封不動地傳遞至像素著色器。

```hlsl
sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

像素著色器接受頂點著色器的輸出做為輸入。 這個著色器會執行光源計算，以模擬一盞在迷宮上方盤旋並對準彈珠位置的柔邊聚光燈。 直接面向燈光的表面，光源最強。 擴散元件會隨著標準表面逐漸垂直於光線而趨近於零，氣氛光會隨著標準點偏離光線而減少。 越靠近彈珠的點 (因此越靠近聚光燈中央) 會越亮。 不過，會針對彈珠底下的點調整光源，以模擬柔和陰影。 在真實環境中，白色彈珠之類的物體會將聚光燈擴散地反射到場景中的其他物體。 彈珠明亮的那半面大致是這樣的情形。 其他照明因素則是相對於彈珠的角度和距離。 產生的像素色彩由取樣紋理與光源計算結果構成。

```hlsl
float4 main(sPSInput input) : SV_TARGET
{
    float3 lightDirection = float3(0, 0, -1);
    float3 ambientColor = float3(0.43, 0.31, 0.24);
    float3 lightColor = 1 - ambientColor;
    float spotRadius = 50;

    // Basic ambient (Ka) and diffuse (Kd) lighting from above.
    float3 N = normalize(input.norm);
    float NdotL = dot(N, lightDirection);
    float Ka = saturate(NdotL + 1);
    float Kd = saturate(NdotL);

    // Spotlight.
    float3 vec = input.worldPos - marblePosition;
    float dist2D = sqrt(dot(vec.xy, vec.xy));
    Kd = Kd * saturate(spotRadius / dist2D);

    // Shadowing from ball.
    if (input.worldPos.z > marblePosition.z)
        Kd = Kd * saturate(dist2D / (marbleRadius * 1.5));

    // Diffuse reflection of light off ball.
    float dist3D = sqrt(dot(vec, vec));
    float3 V = normalize(vec);
    Kd += saturate(dot(-V, N)) * saturate(dot(V, lightDirection))
        * saturate(marbleRadius / dist3D);

    // Final composite.
    float4 diffuseTexture = Texture.Sample(Sampler, input.tex);
    float3 color = diffuseTexture.rgb * ((ambientColor * Ka) + (lightColor * Kd));
    return float4(color * lightStrength, diffuseTexture.a);
}
```

> [!WARNING]
> 編譯的像素著色器包含 32 個算術指令和 1 個紋理指令。 這個著色器在桌上型電腦和更高階的平板電腦上也能正常執行。 不過，低階電腦可能無法處理這個著色器及提供互動式畫面播放速率。 請考量目標使用者一般會使用的硬體，並設計符合該硬體功能的著色器。

 

**Marblemazemain:: Loaddeferredresources**方法使用**basicloader:: Loadshader**方法來載入著色器。 下列範例載入的是頂點著色器。 這個著色器的執行階段格式為**BasicVertexShader.cso**。 **m\_vertexShader** 成員變數是 [ID3D11VertexShader](https://msdn.microsoft.com/library/windows/desktop/ff476641) 物件。

```cpp
BasicLoader^ loader = ref new BasicLoader(m_deviceResources->GetD3DDevice());

D3D11_INPUT_ELEMENT_DESC layoutDesc [] =
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "NORMAL", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "TANGENT", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 32, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};
m_vertexStride = 44; // must set this to match the size of layoutDesc above

Platform::String^ vertexShaderName = L"BasicVertexShader.cso";
loader->LoadShader(
    vertexShaderName,
    layoutDesc,
    ARRAYSIZE(layoutDesc),
    &m_vertexShader,
    &m_inputLayout
    );
```

**m\_inputLayout** 成員變數是 [ID3D11InputLayout](https://msdn.microsoft.com/library/windows/desktop/ff476575) 物件。 輸入配置物件會封裝輸入組合語言 (IA) 階段的輸入狀態。 IA 階段有一項工作是使用系統產生的值 (也稱為 *「語意」*)，只處理尚未處理的基本類別或頂點，以提高著色器的效率。

使用 [ID3D11Device::CreateInputLayout](https://msdn.microsoft.com/library/windows/desktop/ff476512) 方法，從輸入元素描述的陣列來建立輸入配置。 這個陣列包含一或多個輸入元素，每個輸入元素描述來自一個端點緩衝區的一個頂點資料元素。 整組輸入元素描述會描述所有將繫結至 IA 階段的端點緩衝區中的所有頂點資料元素。 

**layoutDesc**上述的程式碼片段示範 Marble Maze 使用配置描述。 配置描述會描述包含四個頂點資料元素的頂點緩衝區。 陣列中每一個項目最重要的部分就是語意名稱、日期格式和位元組位移。 例如，**POSITION** 元素指定物件空間中的頂點位置。 它以位元組位移 0 為起點，且包含三個浮點元件 (總計 12 個位元組)。 **NORMAL** 元素指定標準向量。 它以位元組位移 12 為起點，因為在配置中它會緊接著 **POSITION** 出現，而這需要 12 個位元組。 **NORMAL** 元素包含一個四元件、32 位元不帶正負號的整數。

比較輸入配置與頂點著色器所定義的 **sVSInput** 結構，如下列範例所示。 **sVSInput** 結構定義 **POSITION**、**NORMAL** 及 **TEXCOORD0** 元素。 DirectX 執行階段會將配置中的每個元素對應至著色器所定義的輸入結構。

```hlsl
struct sVSInput
{
    float3 pos : POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
};

struct sPSInput
{
    float4 pos : SV_POSITION;
    float3 norm : NORMAL;
    float2 tex : TEXCOORD0;
    float3 worldPos : TEXCOORD1;
};

sPSInput main(sVSInput input)
{
    sPSInput output;
    float4 temp = float4(input.pos, 1.0f);
    temp = mul(temp, model);
    output.worldPos = temp.xyz / temp.w;
    temp = mul(temp, view);
    temp = mul(temp, projection);
    output.pos = temp;
    output.tex = input.tex;
    output.norm = mul(float4(input.norm, 0.0f), model).xyz;
    return output;
}
```

[語意](https://msdn.microsoft.com/library/windows/desktop/bb509647)文件會進一步說明每一種可用的語意。

> [!NOTE]
> 在配置中，您可以指定不會用來讓多個著色器共用相同的版面配置的其他元件。 例如，著色器不使用 **TANGENT** 元素。 如果您想要試用標準貼圖這類技術，可以使用 **TANGENT** 元素。 您可以使用標準貼圖，也稱為「凹凸貼圖」，在物件的表面建立凹凸效果。 如需凹凸貼圖的詳細資訊，請參閱[凹凸貼圖 (Direct3D 9)](https://msdn.microsoft.com/library/windows/desktop/bb172379)。

 

如需輸入組件階段的詳細資訊，請參閱[輸入組合階段](https://msdn.microsoft.com/library/windows/desktop/bb205116)和[輸入組合階段入門](https://msdn.microsoft.com/library/windows/desktop/bb205117)。

本文件稍後的[呈現場景](#rendering-the-scene)一節描述使用頂點和像素著色器來呈現場景的程序。

### <a name="creating-the-constant-buffer"></a>建立常數緩衝區

Direct3D 緩衝區會將一組資料集合起來。 常數緩衝區是可將資料傳遞給著色器的一種緩衝區。 Marble Maze 使用常數緩衝區來保留模型 (或「世界」) 檢視，以及作用中場景物件的投影矩陣。

下列範例示範如何**marblemazemain:: Loaddeferredresources**方法建立常數緩衝區，稍後將會保留矩陣資料。 這個範例會建立 **D3D11\_BUFFER\_DESC** 結構，此結構使用 **D3D11\_BIND\_CONSTANT\_BUFFER** 旗標來指定用途為常數緩衝區。 這個範例接著會將該結構傳遞給 [ID3D11Device::CreateBuffer](https://msdn.microsoft.com/library/windows/desktop/ff476501) 方法。 **m\_constantBuffer** 變數是 [ID3D11Buffer](https://msdn.microsoft.com/library/windows/desktop/ff476351) 物件。

```cpp
// Create the constant buffer for updating model and camera data.
D3D11_BUFFER_DESC constantBufferDesc = {0};

// Multiple of 16 bytes
constantBufferDesc.ByteWidth = ((sizeof(ConstantBuffer) + 15) / 16) * 16;

constantBufferDesc.Usage               = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags           = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags      = 0;
constantBufferDesc.MiscFlags           = 0;

// This will not be used as a structured buffer, so this parameter is ignored.
constantBufferDesc.StructureByteStride = 0;

DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &constantBufferDesc,
        nullptr,    // leave the buffer uninitialized
        &m_constantBuffer
        )
    );
```

**Marblemazemain:: Update**方法之後會更新**ConstantBuffer**物件，一個用於迷宮，另一個用於彈珠。 **Marblemazemain:: Render**方法接著會繫結每個**ConstantBuffer**物件的常數緩衝區呈現每個物件之前。 下列範例示範**ConstantBuffer**結構，也就是在**MarbleMazeMain.h**。

```cpp
// Describes the constant buffer that draws the meshes.
struct ConstantBuffer
{
    XMFLOAT4X4 model;
    XMFLOAT4X4 view;
    XMFLOAT4X4 projection;

    XMFLOAT3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

若要更清楚地了解常數緩衝區如何對應到著色器程式碼，比較**ConstantBuffer**結構中**MarbleMazeMain.h** **ConstantBuffer**常數緩衝區**與 BasicVertexShader.hlsl 中頂點著色器所定義**:

```hlsl
cbuffer ConstantBuffer : register(b0)
{
    matrix model;
    matrix view;
    matrix projection;
    float3 marblePosition;
    float marbleRadius;
    float lightStrength;
};
```

**ConstantBuffer** 結構的配置符合 **cbuffer** 物件。 **cbuffer** 變數指定暫存器 b0，這表示常數緩衝區資料儲存在暫存器 0。 **Marblemazemain:: Render**方法在啟動常數緩衝區時會指定暫存器 0。 本文件稍後會進一步說明此程序。

如需常數緩衝區的詳細資訊，請閱讀 [Direct3D 11 的緩衝區簡介](https://msdn.microsoft.com/library/windows/desktop/ff476898)。 如需 register 關鍵字的相關資訊，請參閱 [register](https://msdn.microsoft.com/library/windows/desktop/dd607359)。

###  <a name="loading-meshes"></a>載入網格

Marble Maze 使用 SDK 網格做為執行階段格式，原因是此格式提供基本方法來載入範例應用程式的網格資料。 在實際用途上，您應該使用符合遊戲特定需求的網格格式。

**Marblemazemain:: Loaddeferredresources**方法在載入頂點和像素著色器之後，會載入網格資料。 網格是一組頂點資料，通常包含位置、標準資料、色彩、材質和紋理座標等資訊。 網格通常是在 3D 製作軟體中建立及維護個別應用程式程式碼從檔案中。 彈珠和迷宮便是遊戲使用的兩個網格例子。

Marble Maze 使用 **SDKMesh** 類別來管理網格。 這個類別是在**SDKMesh.h**中宣告的。 **SDKMesh** 提供方法來載入、轉譯及終結網格資料。

> [!IMPORTANT]
> Marble Maze 使用 SDK 網格格式，並提供僅供說明**SDKMesh**類別。 雖然 SDK 網格格式有助於學習及建立原型，但卻是很基本的格式，可能不符合大多數遊戲開發的需求。 建議您使用符合遊戲特定需求的網格格式。

 

下列範例示範如何**marblemazemain:: Loaddeferredresources**方法會使用**sdkmesh:: Create**方法來載入迷宮和彈珠的網格資料。

```cpp
// Load the meshes.
DX::ThrowIfFailed(
    m_mazeMesh.Create(
        m_deviceResources->GetD3DDevice(),
        L"Media\\Models\\maze1.sdkmesh",
        false
        )
    );

DX::ThrowIfFailed(
    m_marbleMesh.Create(
        m_deviceResources->GetD3DDevice(),
        L"Media\\Models\\marble2.sdkmesh",
        false
        )
    );
```

###  <a name="loading-collision-data"></a>載入碰撞資料

雖然本節的重點不在於 Marble Maze 如何在彈珠和迷宮之間實作物理模擬，但請注意，載入網格時會讀取物理系統的網格幾何。

```cpp
// Extract mesh geometry for the physics system.
DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_walls",
        m_collision.m_wallTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_Floor",
        m_collision.m_groundTriList
        )
    );

DX::ThrowIfFailed(
    ExtractTrianglesFromMesh(
        m_mazeMesh,
        "Mesh_floorSides",
        m_collision.m_floorTriList
        )
    );

m_physics.SetCollision(&m_collision);
float radius = m_marbleMesh.GetMeshBoundingBoxExtents(0).x / 2;
m_physics.SetRadius(radius);
```

大部分載入碰撞資料的方式取決於您所使用的執行階段格式。 如需 Marble Maze 如何從 SDK 網格檔案載入碰撞幾何的詳細資訊，請參閱原始程式碼中的**MarbleMazeMain::ExtractTrianglesFromMesh**方法。

## <a name="updating-game-state"></a>更新遊戲狀態


Marble Maze 會先更新所有場景物件再呈現物件，以區隔遊戲邏輯和呈現邏輯。

[Marble Maze 應用程式結構](marble-maze-application-structure.md)描述主要遊戲迴圈。 場景更新 (遊戲迴圈的一部分) 是在處理 Windows 事件和輸入之後和呈現場景之前進行。 **Marblemazemain:: Update**方法會處理 UI 和遊戲的更新。

### <a name="updating-the-user-interface"></a>更新使用者介面

**Marblemazemain:: Update**方法呼叫**userinterface:: Update**方法來更新 UI 的狀態。

```cpp
UserInterface::GetInstance().Update(
    static_cast<float>(m_timer.GetTotalSeconds()), 
    static_cast<float>(m_timer.GetElapsedSeconds()));
```

**UserInterface::Update** 方法會更新 UI 集合的每一個元素。

```cpp
void UserInterface::Update(float timeTotal, float timeDelta)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        (*iter)->Update(timeTotal, timeDelta);
    }
}
```

衍生自**ElementBase** （在**UserInterface.h**中定義） 的類別會實作**Update**方法來執行特定的行為。 例如，**StopwatchTimer::Update** 方法會以提供的時間量來更新耗用時間，並更新稍後會顯示的文字。

```cpp
void StopwatchTimer::Update(float timeTotal, float timeDelta)
{
    if (m_active)
    {
        m_elapsedTime += timeDelta;

        WCHAR buffer[16];
        GetFormattedTime(buffer);
        SetText(buffer);
    }

    TextElement::Update(timeTotal, timeDelta);
}
```

###  <a name="updating-the-scene"></a>更新場景

**Marblemazemain:: Update**方法更新遊戲根據目前的狀態電腦 ( **GameState**，儲存在**m_gameState**) 的狀態。 當遊戲處於作用中狀態 (**GameState::InGameActive**) 時，Marble Maze 會更新相機來追蹤彈珠、 更新檢視矩陣組常數緩衝區，並更新物理模擬。

下列範例示範如何**marblemazemain:: Update**方法更新相機的位置。 Marble Maze 使用 **m\_resetCamera** 變數來表示必須將相機重設為位於彈珠正上方。 當遊戲啟動或彈珠掉落迷宮底下時，都會重設相機。 當主功能表或高分顯示畫面在作用中時，會將相機設定在固定位置。 否則，Marble Maze 會使用 *timeDelta* 參數，將相機位置插入目前位置和目標位置之間。 目標位置稍高於彈珠前方。 使用已耗用的畫面時間可讓相機逐步追蹤 (或「追逐」) 彈珠。

```cpp
static float eyeDistance = 200.0f;
static XMFLOAT3A eyePosition = XMFLOAT3A(0, 0, 0);

// Gradually move the camera above the marble.
XMFLOAT3A targetEyePosition;
XMStoreFloat3A(
    &targetEyePosition, 
    XMLoadFloat3A(&marblePosition) - (XMLoadFloat3A(&g) * eyeDistance));

if (m_resetCamera)
{
    eyePosition = targetEyePosition;
    m_resetCamera = false;
}
else
{
    XMStoreFloat3A(
        &eyePosition, 
        XMLoadFloat3A(&eyePosition) 
            + ((XMLoadFloat3A(&targetEyePosition) - XMLoadFloat3A(&eyePosition)) 
                * min(1, static_cast<float>(m_timer.GetElapsedSeconds()) * 8)
            )
    );
}

// Look at the marble. 
if ((m_gameState == GameState::MainMenu) || (m_gameState == GameState::HighScoreDisplay))
{
    // Override camera position for menus.
    XMStoreFloat3A(
        &eyePosition, 
        XMLoadFloat3A(&marblePosition) + XMVectorSet(75.0f, -150.0f, -75.0f, 0.0f));

    m_camera->SetViewParameters(
        eyePosition, 
        marblePosition, 
        XMFLOAT3(0.0f, 0.0f, -1.0f));
}
else
{
    m_camera->SetViewParameters(eyePosition, marblePosition, XMFLOAT3(0.0f, 1.0f, 0.0f));
}
```

下列範例示範如何**marblemazemain:: Update**方法更新彈珠和迷宮的常數緩衝區。 迷宮的模型 (或「世界」) 矩陣仍然是「單位矩陣」。 單位矩陣是除了主對角線 (元素全部是 1) 以外全由 0 組成的平方矩陣。 彈珠的模型矩陣所根據的是其位置矩陣乘以其旋轉矩陣。

```cpp
// Update the model matrices based on the simulation.
XMStoreFloat4x4(&m_mazeConstantBufferData.model, XMMatrixIdentity());

XMStoreFloat4x4(
    &m_marbleConstantBufferData.model, 
    XMMatrixTranspose(
        XMMatrixMultiply(
            marbleRotationMatrix, 
            XMMatrixTranslationFromVector(XMLoadFloat3A(&marblePosition))
        )
    )
);

// Update the view matrix based on the camera.
XMFLOAT4X4 view;
m_camera->GetViewMatrix(&view);
m_mazeConstantBufferData.view = view;
m_marbleConstantBufferData.view = view;
```

**Marblemazemain:: Update**方法如何讀取使用者輸入和模擬彈珠動作的相關資訊，請參閱[加入輸入和互動 Marble Maze 範例](adding-input-and-interactivity-to-the-marble-maze-sample.md)。

## <a name="rendering-the-scene"></a>呈現場景


呈現場景時，通常包括下列步驟。

1.  設定目前呈現目標深度樣板緩衝區。
2.  清除呈現和樣板檢視。
3.  準備頂點和像素著色器來進行繪製。
4.  呈現 3D 場景中的物件。
5.  呈現您想要在場景最前面顯示任何 2D 物件。
6.  將所呈現的影像顯示到監視器。

**Marblemazemain:: Render**方法會繫結呈現目標和深度樣板檢視、 清除這些檢視、 繪製場景，以及然後繪製覆疊。

###  <a name="preparing-the-render-targets"></a>準備呈現目標

在呈現場景之前，您必須設定目前呈現目標深度樣板緩衝區。 如果場景不一定會繪製螢幕上的每個像素，也請清除呈現檢視和樣板檢視。 Marble Maze 會清除每個畫面的呈現檢視和樣板檢視，以確保沒有前一個畫面殘留下來的任何可見成品。

下列範例示範如何**marblemazemain:: Render**方法會呼叫[id3d11devicecontext:: omsetrendertargets](https://msdn.microsoft.com/library/windows/desktop/ff476464)方法以將轉譯目標和深度樣板緩衝區設為目前使用的。

```cpp
auto context = m_deviceResources->GetD3DDeviceContext();

// Reset the viewport to target the whole screen.
auto viewport = m_deviceResources->GetScreenViewport();
context->RSSetViewports(1, &viewport);

// Reset render targets to the screen.
ID3D11RenderTargetView *const targets[1] = 
    { m_deviceResources->GetBackBufferRenderTargetView() };

context->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());

// Clear the back buffer and depth stencil view.
context->ClearRenderTargetView(
    m_deviceResources->GetBackBufferRenderTargetView(), 
    DirectX::Colors::Black);

context->ClearDepthStencilView(
    m_deviceResources->GetDepthStencilView(), 
    D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 
    1.0f, 
    0);
```

[
              ID3D11RenderTargetView
            ](https://msdn.microsoft.com/library/windows/desktop/ff476582) 和 [ID3D11DepthStencilView](https://msdn.microsoft.com/library/windows/desktop/ff476377) 介面支援 Direct3D 10 及更新版本所提供的紋理檢視機制。 如需紋理檢視的詳細資訊，請參閱[紋理檢視 (Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb205128)。 [
              OMSetRenderTargets
            ](https://msdn.microsoft.com/library/windows/desktop/ff476464) 方法會準備 Direct3D 管線的輸出合併階段。 如需輸出合併階段的詳細資訊，請參閱[輸出合併階段](https://msdn.microsoft.com/library/windows/desktop/bb205120)。

### <a name="preparing-the-vertex-and-pixel-shaders"></a>準備頂點著色器和像素著色器

在呈現場景物件之前，請執行下列步驟，準備頂點和像素著色器來進行繪製：

1.  將著色器輸入配置設定為目前配置。
2.  將頂點和像素著色器設定為目前著色器。
3.  使用您必須傳遞給著色器的資料來更新任何常數緩衝區。

> [!IMPORTANT]
> Marble Maze 的所有 3D 物件都使用一對頂點和像素著色器。 如果遊戲使用一對以上的著色器，則每次您繪製的物件使用不同的著色器時，您都必須執行上述步驟。 為了降低變更著色器狀態所引起的額外負荷，建議您將所有使用相同著色器的物件的呈現呼叫聚集在一起。

 

本文件的[載入著色器](#loading-shaders)一節描述建立頂點著色器時如何建立輸入配置。 下列範例示範如何**marblemazemain:: Render**方法會使用[id3d11devicecontext:: iasetinputlayout](https://msdn.microsoft.com/library/windows/desktop/ff476454)方法來將這個配置設為目前配置。

```cpp
m_deviceResources->GetD3DDeviceContext()->IASetInputLayout(m_inputLayout.Get());
```

下列範例示範如何**marblemazemain:: Render**方法會使用的[id3d11devicecontext:: vssetshader](https://msdn.microsoft.com/library/windows/desktop/ff476493)和[id3d11devicecontext:: pssetshader](https://msdn.microsoft.com/library/windows/desktop/ff476472)方法來將頂點和像素著色器設定為目前著色器，分別。

```cpp
// Set the vertex shader stage state.
m_deviceResources->GetD3DDeviceContext()->VSSetShader(
    m_vertexShader.Get(),   // use this vertex shader
    nullptr,                // don't use shader linkage
    0);                     // don't use shader linkage

m_deviceResources->GetD3DDeviceContext()->PSSetShader(
    m_pixelShader.Get(),    // use this pixel shader
    nullptr,                // don't use shader linkage
    0);                     // don't use shader linkage

m_deviceResources->GetD3DDeviceContext()->PSSetSamplers(
    0,                          // starting at the first sampler slot
    1,                          // set one sampler binding
    m_sampler.GetAddressOf());  // to use this sampler
```

**Marblemazemain:: Render**設定著色器及其輸入的配置之後，它會使用[id3d11devicecontext:: updatesubresource](https://msdn.microsoft.com/library/windows/desktop/ff476486)方法以更新常數緩衝區模型、 檢視和迷宮的投影矩陣。 **UpdateSubresource** 方法會將 CPU 記憶體中的矩陣資料複製到 GPU 記憶體。 前面說過**ConstantBuffer**結構的 model 和 view 元件會更新**marblemazemain:: Update**方法中。 **Marblemazemain:: Render**方法接著會呼叫的[id3d11devicecontext:: vssetconstantbuffers](https://msdn.microsoft.com/library/windows/desktop/ff476491)和[id3d11devicecontext:: pssetconstantbuffers](https://msdn.microsoft.com/library/windows/desktop/ff476470)方法，將這個常數緩衝區設為目前緩衝區。

```cpp
// Update the constant buffer with the new data.
m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    nullptr,
    &m_mazeConstantBufferData,
    0,
    0);

m_deviceResources->GetD3DDeviceContext()->VSSetConstantBuffers(
    0,                                  // starting at the first constant buffer slot
    1,                                  // set one constant buffer binding
    m_constantBuffer.GetAddressOf());   // to use this buffer

m_deviceResources->GetD3DDeviceContext()->PSSetConstantBuffers(
    0,                                  // starting at the first constant buffer slot
    1,                                  // set one constant buffer binding
    m_constantBuffer.GetAddressOf());   // to use this buffer
```

**Marblemazemain:: Render**方法會執行類似的步驟來準備要呈現的彈珠。

### <a name="rendering-the-maze-and-the-marble"></a>呈現迷宮和彈珠

在您啟動目前著色器之後，您可以繪製場景物件。 **Marblemazemain:: Render**方法會呼叫**sdkmesh:: Render**方法來呈現迷宮網格。

```cpp
m_mazeMesh.Render(
    m_deviceResources->GetD3DDeviceContext(), 
    0, 
    INVALID_SAMPLER_SLOT, 
    INVALID_SAMPLER_SLOT);
```

**Marblemazemain:: Render**方法會執行類似的步驟來呈現彈珠。

如本文件先前所述，提供 **SDKMesh** 類別是為了便於示範，但我們不建議將它用於正式的遊戲中。 但請注意，**SDKMesh::RenderMesh** 方法 (由 **SDKMesh::Render** 呼叫) 不僅使用 [ID3D11DeviceContext::IASetVertexBuffers](https://msdn.microsoft.com/library/windows/desktop/ff476456) 和 [ID3D11DeviceContext::IASetIndexBuffer](https://msdn.microsoft.com/library/windows/desktop/ff476453) 方法來設定目前的頂點和索引緩衝區 (定義網格)，也使用 [ID3D11DeviceContext::DrawIndexed](https://msdn.microsoft.com/library/windows/desktop/ff476410) 方法來繪製緩衝區。 如需如何使用頂點和索引緩衝區的詳細資訊，請參閱 [Direct3D 11 的緩衝區簡介](https://msdn.microsoft.com/library/windows/desktop/ff476898)。

### <a name="drawing-the-user-interface-and-overlay"></a>繪製使用者介面和覆疊

在繪製 3D 場景物件之後, Marble Maze 會繪製場景前出現的 2D UI 元素。

**Marblemazemain:: Render**方法最後會繪製使用者介面和覆疊。

```cpp
// Draw the user interface and the overlay.
UserInterface::GetInstance().Render(m_deviceResources->GetOrientationTransform2D());

m_deviceResources->GetD3DDeviceContext()->BeginEventInt(L"Render Overlay", 0);
m_sampleOverlay->Render();
m_deviceResources->GetD3DDeviceContext()->EndEvent();
```

**UserInterface::Render** 方法會使用 [ID2D1DeviceContext](https://msdn.microsoft.com/library/windows/desktop/hh404479) 物件來繪製 UI 元素。 這個方法會設定繪圖狀態、繪製所有作用中的 UI 元素，然後還原先前的繪圖狀態。

```cpp
void UserInterface::Render(D2D1::Matrix3x2F orientation2D)
{
    m_d2dContext->SaveDrawingState(m_stateBlock.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(orientation2D);

    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if ((*iter)->IsVisible())
            (*iter)->Render();
    }

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        DX::ThrowIfFailed(hr);
    }

    m_d2dContext->RestoreDrawingState(m_stateBlock.Get());
}
```

**SampleOverlay::Render** 方法會使用類似的技術來繪製覆疊點陣圖。

###  <a name="presenting-the-scene"></a>顯示場景

在繪製所有 2D 與 3D 場景物件之後, Marble Maze 會將轉譯的影像呈現到監視器。 它會將繪圖同步處理到垂直空白，以確定不浪費時間繪製事實上永遠不會出現在顯示器上的畫面。 Marble Maze 在顯示場景時也會處理裝置變更。

**Marblemazemain:: Render**方法會傳回之後，遊戲迴圈會呼叫**DX::DeviceResources::Present**方法來將轉譯的影像傳送到監視器或顯示器。 **DX::DeviceResources::Present**方法會呼叫[idxgiswapchain::](https://msdn.microsoft.com/library/windows/desktop/bb174576)來執行顯示作業，如下列範例所示：

```cpp
// The first argument instructs DXGI to block until VSync, putting the application
// to sleep until the next VSync. This ensures we don't waste any cycles rendering
// frames that will never be displayed to the screen.
HRESULT hr = m_swapChain->Present(1, 0);
```

在此範例中，**m\_swapChain** 是 [IDXGISwapChain1](https://msdn.microsoft.com/library/windows/desktop/hh404631) 物件。 本文件的[初始化 Direct3D 和 Direct2D](#initializing-direct3d-and-direct2d) 一節會描述這個物件的初始化。

[Idxgiswapchain::](https://msdn.microsoft.com/library/windows/desktop/hh446797)， *SyncInterval*，第一個參數指定呈現框架之前要等待的垂直空白的數。 Marble Maze 指定 1，所以會等待到下一個垂直空白。

[Idxgiswapchain::](https://msdn.microsoft.com/library/windows/desktop/bb174576)方法會傳回錯誤碼，表示裝置已移除或故障。 在此情況下，Marble Maze 會重新初始化裝置。

```cpp
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
```

## <a name="next-steps"></a>後續步驟


如需使用輸入裝置時要牢記的一些重要做法的相關資訊，請參閱[在 Marble Maze 範例中加入輸入和互動](adding-input-and-interactivity-to-the-marble-maze-sample.md)。 本文件會討論 Marble Maze 如何支援觸控、 加速計、 Xbox 控制器和滑鼠輸入。

## <a name="related-topics"></a>相關主題


* [在 Marble Maze 範例中加入輸入和互動](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Marble Maze 應用程式結構](marble-maze-application-structure.md)
* [使用 C++ 和 DirectX 開發 Marble Maze (UWP 遊戲)](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




