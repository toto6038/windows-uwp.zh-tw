---
author: stevewhims
description: 本主題使用完整的 Direct2D 程式碼範例，示範如何使用 C + + /winrt 取用 COM 類別和介面。
title: 使用 C++/WinRT 來使用 COM 元件
ms.author: stwhi
ms.date: 07/23/2018
ms.topic: article
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 COM、 元件、 類別、 介面
ms.localizationpriority: medium
ms.openlocfilehash: a07c9877a6d6d953e578d927959b1202444ede21
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6859259"
---
# <a name="consume-com-components-with-cwinrt"></a>使用 C++/WinRT 來使用 COM 元件

您可以使用的設備[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)程式庫來使用 COM 元件，例如 DirectX Api 高效能的 2d 和 3d 圖形。 C + + /winrt 是最簡單的方式使用 DirectX 又無須犧牲效能。 本主題使用 Direct2D 程式碼範例，示範如何使用 C + + /winrt 取用 COM 類別和介面。 您當然可以混合 COM 和 Windows 執行階段程式設計內相同的 C + + /winrt 專案。

本主題的結尾，您會發現最少的 Direct2D 應用程式的完整來源的程式碼清單。 我們會提起節錄該程式碼，並使用它們來說明如何使用 COM 元件使用 C + + WinRT 使用各種不同的設備的 C + + /winrt 程式庫。

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>COM 智慧型指標 ([**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

當您計畫以 COM 時，您可以使用直接與介面，而不是使用的物件 （也在幕後適用於 Windows 執行階段 Api，這是 COM 的進化版，則為 true 的）。 COM 類別上呼叫函式，例如，您啟動類別，取得介面回來，並接著在該介面上呼叫函式。 若要存取物件的狀態，您不直接; 存取其資料成員相反地，您會在介面上呼叫存取子和更動子函式。

若要更具體，我們正在討論與介面*指標*互動。 為此，我們實惠的存在 COM 智慧型指標型別在 C + + /winrt&mdash; [**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)類型。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
```

上述的程式碼顯示如何宣告[**ID2D1Factory1**](https://msdn.microsoft.com/library/Hh404596) COM 介面未初始化的智慧型指標。 智慧型指標是未初始化，，因此它還不指至屬於先前依照任何實際的物件 （它不指向介面完全） **ID2D1Factory1**介面。 但它有可能這樣做;和 （正在智慧型指標） 已透過 COM 參考計數管理，它指向，介面的擁有者物件的存留期，並將其所依據的呼叫的函式在該介面的媒體的能力。

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>COM 函式，傳回**void**為介面指標

您可以呼叫[**com_ptr:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)函式將寫入的未初始化智慧型指標的基礎原始指標。

```cppwinrt
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

上述的程式碼會呼叫[**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory)函式，傳回透過其最後一個參數，其中有**ID2D1Factory1**介面指標**void\ * \ *** 類型。 許多 COM 函式會傳回**void\ * \ ***。 針對這類功能，請使用[**com_ptr:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) ，如所示。

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>COM 函式，傳回特定介面指標

透過其 antepenultimate 參數，其中有[**D3D11CreateDevice**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory)函式會傳回的[**ID3D11Device**](https://msdn.microsoft.com/library/Hh404596)介面指標**ID3D11Device\ * \ *** 類型。 對於函式，像這樣傳回特定介面指標，使用[**com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)。

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

前一個區段的程式碼範例示範如何呼叫原始**D2D1CreateFactory**函式。 但事實上，當本主題中的程式碼範例呼叫**D2D1CreateFactory**時，它會使用原始的 API，換行的協助程式函式範本，因此在程式碼範例實際使用[**com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>COM 函式，傳回做為**IUnknown**介面指標

[**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory)函式會傳回透過其最後一個參數，其中具有[**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)類型 DirectWrite factory 介面指標。 針對這類函式，使用[**com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)，但重新解譯轉換成可**IUnknown**。

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>重新座位**winrt:: com_ptr**

> [!IMPORTANT]
> 如果您有已經安裝[**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) （其內部的原始指標已經有一個目標） 和您想要重新座位它指向不同的物件，則您必須先指派`nullptr`它&mdash;在下列程式碼範例所示。 如果沒有，則已經安裝**com_ptr**將會繪製問題 （當您呼叫[**com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) [**:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)） 注意到藉由宣告其內部指標不 null。

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

若要檢查從 COM 函式，傳回的 HRESULT 值並擲回例外狀況，它可以代表錯誤程式碼，呼叫[**winrt:: check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)。

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>採取的特定介面指標的 COM 函式

您可以呼叫[**comptr:: get**](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function)函式，將您**com_ptr**傳遞給一相同類型的特定介面指標的函式。

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>採取**IUnknown**介面指標的 COM 函式

您可以呼叫[**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function)可用函式，將您**com_ptr**傳遞給**IUnknown**介面指標的函式。

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>傳遞和傳回 COM 智慧型指標

**Winrt:: com_ptr**的形式的 COM 智慧型指標的函式應該這樣由常數的參考，或參考。

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

**Winrt:: com_ptr**傳回的函式應該執行此動作的值。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>查詢不同的介面的 COM 智慧型指標

您可以使用[**com_ptr:: as**](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function)函式來查詢不同的介面的 COM 智慧型指標。 如果不成功查詢函式會擲回例外狀況。

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

或者，使用[**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#comptrtryas-function)，這會傳回值，您可以檢查針對`nullptr`以查看查詢是否成功。

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>完整來源的最少的 Direct2D 應用程式的程式碼清單

如果您想要建置並執行此來源的程式碼範例，則第一個，在 Visual Studio 中，建立一個新**核心應用程式 (C + + /winrt)**。 `Direct2D` 合理專案名稱，但您可以將它命名您喜歡的任何項目。 開啟`App.cpp`、 刪除整個內容，以及貼上下列清單中。

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

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>使用 COM 類型，例如 BSTR 和變體

如您所見，C + + /winrt 提供支援實作和呼叫 COM 介面。 適用於使用 COM 類型，例如 BSTR 和變體，沒有一律要在其原始形式 （連同適當的 Api) 中使用這些選項。 或者，您可以使用包裝函式所提供的架構，例如[作用中的範本程式庫 (ATL)](/cpp/atl/active-template-library-atl-concepts)，或透過 Visual c + + 編譯器[COM 的支援](/cpp/cpp/compiler-com-support)，或甚至是您自己的包裝函式。

## <a name="important-apis"></a>重要 API
* [winrt::check_hresult 函式](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr 結構範本](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown 結構](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
