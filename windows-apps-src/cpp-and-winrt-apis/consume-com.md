---
description: 本主題使用完整的 Direct2D 程式碼範例來示範如何使用 C++/WinRT 來取用 COM 類別和介面。
title: 使用 C++/WinRT 來使用 COM 元件
ms.date: 07/23/2018
ms.topic: article
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 COM、 元件、 類別、 介面
ms.localizationpriority: medium
ms.openlocfilehash: 16425fd6d296a4abd4ed62c0c64cd23ef1f88891
ms.sourcegitcommit: 9031a51f9731f0b675769e097aa4d914b4854e9e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2019
ms.locfileid: "58618405"
---
# <a name="consume-com-components-with-cwinrt"></a>使用 C++/WinRT 來使用 COM 元件

您可以使用的機能[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)程式庫來取用 COM 元件，例如 DirectX Api 的高效能 2d 和 3d 圖形。 C++/ WinRT 是最簡單的方式，將不會影響效能的 DirectX。 本主題使用 Direct2D 的程式碼範例示範如何使用C++/WinRT 取用 COM 類別和介面。 您當然可以混合在相同的 COM 和 Windows 執行階段程式設計C++/WinRT 專案。

在本主題的結尾，您會發現最小的 Direct2D 應用程式的完整原始程式碼的程式碼清單。 我們會隨即從該程式碼的摘錄，以及使用它們來說明如何使用 COM 元件，使用C++/使用的各種功能的 WinRT C++/WinRT 程式庫。

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>COM 智慧型指標 ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

當您使用 COM 程式設計時，您可以使用直接與介面，而不是使用物件 （即也在幕後適用於 Windows 執行階段 Api，也就是 COM 的進化版本，則為 true）。 在 COM 類別上呼叫的函式，例如，您啟用類別，取得的介面，然後再呼叫函式的介面上。 若要存取物件的狀態，您不直接; 存取其資料成員相反地，您會在介面上呼叫存取子和 mutator 函式。

若要更具體，我們的意思了互動介面*指標*。 並為此，我們受益於 COM 智慧型指標類型中是否存在C++/WinRT&mdash; [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr)型別。

```cppwinrt
#include <d2d1_1.h>
...
winrt::com_ptr<ID2D1Factory1> factory;
```

上述程式碼示範如何宣告要未初始化的智慧型指標[ **ID2D1Factory1** ](https://msdn.microsoft.com/library/Hh404596) COM 介面。 智慧型指標未初始化，因此它尚不指向**ID2D1Factory1**屬於任何實際的物件 （它並未指向介面完全） 的介面。 但有可能會這麼做;而且它有透過 COM 參考計數來管理的介面，它會指向，主控物件的存留期，以及可供您函式呼叫該介面的媒體功能 （在智慧型指標）。

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>COM 函式會傳回介面指標當做**void**

您可以呼叫[ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)寫入未初始化的智慧型指標的函式的基礎原始指標。

```cppwinrt
D2D1_FACTORY_OPTIONS options{ D2D1_DEBUG_LEVEL_NONE };
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

呼叫上述程式碼[ **D2D1CreateFactory** ](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory)函式，它會傳回**ID2D1Factory1**介面指標，其最後一個參數，已透過**void\* \*** 型別。 許多 COM 函式會傳回**void\*\***。 對於這類函式中，使用[ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)所示。

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>傳回特定的介面指標的 COM 函式

[ **D3D11CreateDevice** ](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)函式會傳回[ **ID3D11Device** ](/windows/desktop/api/d3d11/nn-d3d11-id3d11device)協力從最後一個參數，可透過的介面指標**ID3D11Device\* \*** 型別。 這樣會傳回特定的介面指標的函式，使用[ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)。

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

之前這一個示範如何呼叫未經處理的一節的程式碼範例**D2D1CreateFactory**函式。 但事實上，當此主題的程式碼範例會呼叫**D2D1CreateFactory**它會使用包裝原始的 API，協助程式函式樣板，因此程式碼範例實際上會使用[ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>COM 函式會傳回介面指標當做**IUnknown**

[ **DWriteCreateFactory** ](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory)函式會傳回其最後一個參數，已透過 DirectWrite factory 的介面指標[ **IUnknown** ](https://msdn.microsoft.com/library/windows/desktop/ms680509)型別。 對於這類函式，使用[ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)，但轉換轉換成**IUnknown**。

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>重新基座**winrt::com_ptr**

> [!IMPORTANT]
> 如果您有[ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr)的已插入擴充槽 （其內部的原始指標已經有一個目標） 和您想要重新基座，以指向不同的物件，則您必須先指派`nullptr`它&mdash;如下列程式碼範例所示。 如果沒有，則已經為插入擴充槽**com_ptr**將會繪製您注意的問題 (當您呼叫[ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)或是[ **com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)) 藉由判斷提示其內部指標不 null。

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

## <a name="handle-hresult-error-codes"></a>處理 HRESULT 錯誤碼

若要檢查的 HRESULT 值傳回從 COM 函式，並擲回例外，它代表了錯誤代碼，呼叫[ **winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)。

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>將特定的介面指標的 COM 函式

您可以呼叫[ **com_ptr::get** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function)函式來傳遞您**com_ptr**接受相同類型的特定介面指標的函式。

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>COM 函式會採用**IUnknown**介面指標

您可以呼叫[ **winrt::get_unknown** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#get_unknown-function)可用函式來傳遞您**com_ptr**函式採用**IUnknown**介面指標。

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>傳遞和傳回 COM 智慧型指標

函式的形式採用 COM 智慧型指標**winrt::com_ptr**應該這麼常數的參考，或參考。

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

傳回的函式**winrt::com_ptr**可以值。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>查詢不同的介面的 COM 智慧型指標

您可以使用[ **com_ptr::as** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function)函式來查詢不同的介面的 COM 智慧型指標。 如果查詢不成功，則函式會擲回例外狀況。

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

或者，使用[ **com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrtry_as-function)，它會傳回值，您可以檢查`nullptr`以查看查詢是否成功。

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>完整來源的最小的 Direct2D 應用程式的程式碼清單

如果您想要建置並執行此原始程式碼範例，則第一個，在 Visual Studio 中，建立新**Core 應用程式 (C++/WinRT)**。 `Direct2D` 合理專案名稱，但它可以隨意命名。 開啟`App.cpp`、 刪除整個內容，並貼上下列清單中。

```cppwinrt
#include "pch.h"
#include <d2d1_1.h>
#include <d3d11.h>
#include <dxgi1_2.h>
#include <winrt/Windows.Graphics.Display.h>

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

        char buffer[1024];
        (void)snprintf(buffer, sizeof(buffer), "Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
        ::OutputDebugStringA(buffer);
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

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>使用 COM 類型，例如 BSTR 和 VARIANT

如您所見， C++/WinRT 提供實作和呼叫 COM 介面的支援。 使用 COM 類型，例如 BSTR 和 VARIANT，總是有要在其原始形式 （搭配適當的 Api) 中使用這些選項。 或者，您可以使用例如 framework 所提供的包裝函式[Active Template Library (ATL)](/cpp/atl/active-template-library-atl-concepts)，或視覺效果C++編譯器[COM 支援](/cpp/cpp/compiler-com-support)，或甚至您自己的包裝函式。

## <a name="important-apis"></a>重要 API
* [winrt::check_hresult 函式](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr 結構範本](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown struct](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
