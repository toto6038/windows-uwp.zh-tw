---
author: stevewhims
description: 本主題會使用完整的 Direct2D 程式碼範例會示範如何使用 C + + 使用 COM 類別和介面 WinRT。
title: 使用 DirectX 及其他 COM Api 與 C + + WinRT
ms.author: stwhi
ms.date: 07/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 COM、 元件、 類別、 介面
ms.localizationpriority: medium
ms.openlocfilehash: b87eb90ed5ecf731cc851e81e81ad016956e5fea
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/23/2018
ms.locfileid: "2815570"
---
# <a name="consume-directx-and-other-com-apis-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>使用 DirectX 及其他 COM Api 與[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

您可以使用設備的 C + + 使用 COM 元件，例如 DirectX Api 高效能 2d 及 3d 圖形 WinRT 文件庫。 C + + WinRT 是使用 DirectX 兼顧效能的最簡單方式。 本主題會使用 Direct2D 程式碼範例會示範如何使用 C + + 使用 COM 類別和介面 WinRT。 當然，可以混合 COM 和 Windows Runtime 程式設計內相同的 C + + WinRT 專案。

本主題的結尾，您會發現的最少的 Direct2D 應用程式的完整來源程式碼清單。 我們將拿起摘錄從該程式碼和用以說明如何使用 COM 元件使用 C + + WinRT 使用各種設備的 C + + WinRT 文件庫。

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>COM 智慧型指標 ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

當您設計程式與 COM 時，則可以搭配直接與介面而不是處理 （亦的也在幕後 for Windows Runtime Api，亦即發展的 COM，則為 true） 的物件。 COM 類別上呼叫函數，例如您啟動類別，取得介面上一步] 並接著該介面上呼叫函數。 若要存取物件的狀態，您不直接; 存取其資料成員而您可以呼叫 accessor 和更動子函數介面上。

若要更具體，我們所說的相關互動介面*指標*。 與的我們獲益是否存在 COM 智慧型指標類型在 C + + WinRT&mdash; [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)類型。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
```

上面的程式碼示範如何宣告[**ID2D1Factory1**](https://msdn.microsoft.com/library/Hh404596) COM 介面初始化智慧型指標。 智慧型指標是初始化，因此尚未指向屬於任何實際物件 （它未指向介面所有） **ID2D1Factory1**介面。 但有可能會這麼做;與 （正在智慧型指標） 已透過 COM 參考計數管理其指向、 介面主控物件的存留期以及會依據您的功能呼叫該介面 medium 的能力。

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>傳回為介面指標的 COM 函數**void\ * \ ***

您可以呼叫[**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)函數將寫入初始化智慧型指標的基礎原始的指標。

```cppwinrt
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

上面的程式碼呼叫[**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory)函數，透過具有其最後一個參數傳回**ID2D1Factory1**介面指標**void\ * \ *** 類型。 許多 COM 函數會傳回**void\ * \ ***。 這類的功能，請使用[**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)所示。

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>傳回特定介面指標的 COM 函數

[**D3D11CreateDevice**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory)函數會傳回[**ID3D11Device**](https://msdn.microsoft.com/library/Hh404596)介面指標透過其 antepenultimate 參數，具有**ID3D11Device\ * \ *** 類型。 傳回特定介面指標像這樣的功能，使用[**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)。

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

在前一個] 區段中的程式碼範例顯示如何呼叫原始**D2D1CreateFactory**函數。 但事實上，當本主題的程式碼範例會呼叫**D2D1CreateFactory**，它會使用 helper 函數範本換行未經處理的 API，並讓程式碼範例會實際使用[**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>傳回為介面指標的 COM 函數**IUnknown\ * \ ***

[**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory)函數會傳回 DirectWrite 原廠介面指標透過其最後一個參數，具有**IUnknown\ * \ *** 類型。 這類函數使用[**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)，但重新解譯轉換的至**IUnknown\ * \ ***。

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>重新座位**winrt::com_ptr**

> [!IMPORTANT]
> 如果您必須已經安裝[**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) （其內部的原始指標已經有目標） 與您想要重新座位它以指到不同的物件，則您必須先指派`nullptr`它&mdash;下面的程式碼範例所示。 如果您不要然後已經安裝**com_ptr**將繪出問題到注意 （當您呼叫[**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)或[**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)） 來判斷其內部的指標不 null。

```cppwinrt
winrt::com_ptr<ID2D1SolidColorBrush> brush;
...
    brush.put()
...
brush = nullptr; // Important because we're about to re-seat
target->CreateSolidColorBrush(
    color_orange,
    D2D1::BrushProperties(0.8f),
    brush.put()));
```

## <a name="handle-hresult-error-codes"></a>處理 HRESULT 錯誤代碼

若要檢查的 HRESULT 值傳回 COM 函式和擲回例外，它所代表的錯誤碼，呼叫[**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)。

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>COM 函數所採用的特定介面指標

您可以呼叫[**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function)函數，以將您**com_ptr**傳遞給函數會針對相同類型的特定介面指標。

```cppwinrt
... ExampleFunction(
    winrt::com_ptr<ID2D1Factory1> const& factory,
    winrt::com_ptr<IDXGIDevice> const& dxdevice)
{
    ...
    winrt::check_hresult(factory->CreateDevice(dxdevice.get(), ...));
    ...
}
```

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>取得**IUnknown**介面指標的 COM 函數

您可以呼叫[**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function)免費函數，以將您**com_ptr**傳遞給函數會**IUnknown**介面指標。

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>傳遞和傳回 COM 智慧型指標

利用 COM 智慧型指標的格式**winrt::com_ptr**函數應達成常數的參考，或是參考 （英文）。

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

會傳回**winrt::com_ptr**函數應該進行值。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>查詢 COM 智慧型指標不同的介面

您可以使用[**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function)函數來查詢 COM 智慧型指標不同的介面。 如果查詢不會成功函數會擲回例外狀況。

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

或者，使用[**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#comptrtryas-function)，它會傳回一個值，您可以檢查針對`nullptr`以查看是否已成功的查詢。

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>完整的來源程式碼清單的最少的 Direct2D 應用程式

如果您想要建立及執行這個來源的程式碼範例則第一筆、 Visual Studio 中，建立新**核心應用程式 (C + + WinRT)**。 `Direct2D` project 的合理名稱但是它可以您想要的任何項目名稱。 開啟`App.cpp`、 刪除其整個內容，並在下方的清單貼上。

```cppwinrt
#include "pch.h"

using namespace winrt;

using namespace Windows;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::UI;
using namespace Windows::UI::Core;
using namespace Windows::Graphics::Display;

namespace
{
    winrt::com_ptr<ID2D1Factory1> CreateFactory()
    {
        D2D1_FACTORY_OPTIONS options{};

#ifdef _DEBUG
        options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

        winrt::com_ptr<ID2D1Factory1> factory;

        winrt::check_hresult(D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            options,
            factory.put()));

        return factory;
    }

    HRESULT CreateDevice(D3D_DRIVER_TYPE const type, winrt::com_ptr<ID3D11Device>& device)
    {
        WINRT_ASSERT(!device);

        return D3D11CreateDevice(
            nullptr,
            type,
            nullptr,
            D3D11_CREATE_DEVICE_BGRA_SUPPORT,
            nullptr, 0,
            D3D11_SDK_VERSION,
            device.put(),
            nullptr,
            nullptr);
    }

    winrt::com_ptr<ID3D11Device> CreateDevice()
    {
        winrt::com_ptr<ID3D11Device> device;
        HRESULT hr{ CreateDevice(D3D_DRIVER_TYPE_HARDWARE, device) };

        if (DXGI_ERROR_UNSUPPORTED == hr)
        {
            hr = CreateDevice(D3D_DRIVER_TYPE_WARP, device);
        }

        winrt::check_hresult(hr);
        return device;
    }

    winrt::com_ptr<ID2D1DeviceContext> CreateRenderTarget(
        winrt::com_ptr<ID2D1Factory1> const& factory,
        winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(factory);
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<ID2D1Device> d2device;
        winrt::check_hresult(factory->CreateDevice(dxdevice.get(), d2device.put()));

        winrt::com_ptr<ID2D1DeviceContext> target;
        winrt::check_hresult(d2device->CreateDeviceContext(D2D1_DEVICE_CONTEXT_OPTIONS_NONE, target.put()));
        return target;
    }

    winrt::com_ptr<IDXGIFactory2> GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<IDXGIAdapter> adapter;
        winrt::check_hresult(dxdevice->GetAdapter(adapter.put()));

        winrt::com_ptr<IDXGIFactory2> factory;
        winrt::check_hresult(adapter->GetParent(__uuidof(factory), factory.put_void()));
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        winrt::check_hresult(swapchain->GetBuffer(0, __uuidof(surface), surface.put_void()));

        D2D1_BITMAP_PROPERTIES1 const props{ D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_IGNORE)) };

        winrt::com_ptr<ID2D1Bitmap1> bitmap;

        winrt::check_hresult(target->CreateBitmapFromDxgiSurface(surface.get(),
            props,
            bitmap.put()));

        target->SetTarget(bitmap.get());
    }

    winrt::com_ptr<IDXGISwapChain1> CreateSwapChainForCoreWindow(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIFactory2> const factory{ GetDxgiFactory(device) };

        DXGI_SWAP_CHAIN_DESC1 props{};
        props.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
        props.SampleDesc.Count = 1;
        props.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        props.BufferCount = 2;
        props.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;

        winrt::com_ptr<IDXGISwapChain1> swapChain;

        winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
            device.get(),
            winrt::get_unknown(CoreWindow::GetForCurrentThread()),
            &props,
            nullptr, // all or nothing
            swapChain.put()));

        return swapChain;
    }

    constexpr D2D1_COLOR_F color_white{ 1.0f,  1.0f,  1.0f,  1.0f };
    constexpr D2D1_COLOR_F color_orange{ 0.92f,  0.38f,  0.208f,  1.0f };
}

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::com_ptr<ID2D1Factory1> m_factory;
    winrt::com_ptr<ID2D1DeviceContext> m_target;
    winrt::com_ptr<IDXGISwapChain1> m_swapChain;
    winrt::com_ptr<ID2D1SolidColorBrush> m_brush;
    float m_dpi{};

    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const&)
    {
    }

    void Load(hstring const&)
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };

        window.SizeChanged([&](auto&&...)
        {
            if (m_target)
            {
                ResizeSwapChainBitmap();
                Render();
            }
        });

        DisplayInformation const display{ DisplayInformation::GetForCurrentView() };
        m_dpi = display.LogicalDpi();

        display.DpiChanged([&](DisplayInformation const& display, IInspectable const&)
        {
            if (m_target)
            {
                m_dpi = display.LogicalDpi();
                m_target->SetDpi(m_dpi, m_dpi);
                CreateDeviceSizeResources();
                Render();
            }
        });

        m_factory = CreateFactory();
        CreateDeviceIndependentResources();
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };
        window.Activate();

        Render();
        CoreDispatcher const dispatcher{ window.Dispatcher() };
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const&) {}

    void Draw()
    {
        m_target->Clear(color_white);

        D2D1_SIZE_F const size{ m_target->GetSize() };
        D2D1_RECT_F const rect{ 100.0f, 100.0f, size.width - 100.0f, size.height - 100.0f };
        m_target->DrawRectangle(rect, m_brush.get(), 100.0f);

        WINRT_TRACE("Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
    }

    void Render()
    {
        if (!m_target)
        {
            winrt::com_ptr<ID3D11Device> const device{ CreateDevice() };
            m_target = CreateRenderTarget(m_factory, device);
            m_swapChain = CreateSwapChainForCoreWindow(device);

            CreateDeviceSwapChainBitmap(m_swapChain, m_target);

            m_target->SetDpi(m_dpi, m_dpi);

            CreateDeviceResources();
            CreateDeviceSizeResources();
        }

        m_target->BeginDraw();
        Draw();
        m_target->EndDraw();

        HRESULT const hr{ m_swapChain->Present(1, 0) };

        if (S_OK != hr && DXGI_STATUS_OCCLUDED != hr)
        {
            ReleaseDevice();
        }
    }

    void ReleaseDevice()
    {
        m_target = nullptr;
        m_swapChain = nullptr;

        ReleaseDeviceResources();
    }

    void ResizeSwapChainBitmap()
    {
        WINRT_ASSERT(m_target);
        WINRT_ASSERT(m_swapChain);

        m_target->SetTarget(nullptr);

        if (S_OK == m_swapChain->ResizeBuffers(0, // all buffers
            0, 0, // client area
            DXGI_FORMAT_UNKNOWN, // preserve format
            0)) // flags
        {
            CreateDeviceSwapChainBitmap(m_swapChain, m_target);
            CreateDeviceSizeResources();
        }
        else
        {
            ReleaseDevice();
        }
    }

    void CreateDeviceIndependentResources()
    {
    }

    void CreateDeviceResources()
    {
        winrt::check_hresult(m_target->CreateSolidColorBrush(
            color_orange,
            D2D1::BrushProperties(0.8f),
            m_brush.put()));
    }

    void CreateDeviceSizeResources()
    {
    }

    void ReleaseDeviceResources()
    {
        m_brush = nullptr;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(App());
}
```

## <a name="important-apis"></a>重要 API
* [winrt::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown 結構](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
