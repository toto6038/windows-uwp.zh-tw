---
description: 本主題使用完整的 Direct2D 程式碼範例來示範如何使用 C++/WinRT 來取用 COM 類別和介面。
title: 使用 C++/WinRT 取用 COM 元件
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, COM, 元件, 類別, 介面
ms.localizationpriority: medium
ms.openlocfilehash: 6a286056fc0c44d01482e23e52df0fa80eca0515
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218518"
---
# <a name="consume-com-components-with-cwinrt"></a>使用 C++/WinRT 取用 COM 元件

您可以使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 程式庫的設備來取用 COM 元件，例如 DirectX API 的高效能 2D 和 3D 圖形。 C++/ WinRT 是使用 DirectX 最簡單的方式，而且不會影響效能。 本主題會 Direct2D 程式碼範例來示範如何使用 C++/WinRT 來取用 COM 類別和介面。 當然，您可以在相同的 C++/WinRT 專案中混合使用 COM 和 Windows 執行階段的程式設計功能。

在本主題結束時，您將可看到極簡 Direct2D 應用程式的完整原始程式碼清單。 我們將摘要選取該程式碼，並使用這些程式碼說明如何透過 C++/WinRT 使用 C++/WinRT 程式庫的各種設備來取用 COM 元件。

## <a name="com-smart-pointers-winrtcom_ptr"></a>COM 智慧型指標 ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

當您使用 COM 進行程式設計時，您應直接使用介面，而不是使用物件 (在幕後使用 Windows 執行階段 API (COM 的進化版) 也是如此)。 例如，若要在 COM 類別上呼叫函式，您可以啟用類別、重新取得介面，然後在該介面上呼叫函式。 若要存取物件的狀態，您不是直接存取其資料成員，相反地，您會在介面上呼叫存取子和更動子函式。

更具體地說，我們將討論如何與「指標」  介面互動。 為此，我們將受益於 C++/WinRT 中的 COM 智慧型指標類型&mdash;[**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 類型。

```cppwinrt
#include <d2d1_1.h>
...
winrt::com_ptr<ID2D1Factory1> factory;
```

上述程式碼會示範如何將未初始化的智慧型指標宣告為 [**ID2D1Factory1**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1factory1) COM 介面。 由於智慧型指標未初始化，因此其尚未指向屬於任何實際物件的 **ID2D1Factory1** (未指向任何介面)。 不過該指標是有能力這麼做的；而且智慧型指標可透過 COM 參考計數來管理所指介面的主控物件存留期，以及可作為函式在該介面上呼叫函式的媒介。

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>以 **void** 形式傳回介面指標的 COM 函式

您可以呼叫 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) 函式來寫入未初始化智慧型指標的基礎原始指標。

```cppwinrt
D2D1_FACTORY_OPTIONS options{ D2D1_DEBUG_LEVEL_NONE };
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

上述程式碼會呼叫 [**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory) 函式，而該函式會透過其最後一個參數 (其中有 **void\*\*** 類型) 傳回 **ID2D1Factory1** 介面指標。 許多 COM 函式會傳回 **void\*\*** 。 對於這類函式，請使用我們所示範 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)。

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>傳回特定介面指標的 COM 函式

[**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 函式會透過其倒數第三個參數 (其中有 **ID3D11Device\*\*** 類型) 來傳回 [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 介面指標。 對於像這樣傳回特定介面指標的函式，請使用 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)。

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

上一節中的程式碼範例示範如何呼叫原始 **D2D1CreateFactory** 函式。 但事實上，此主題的程式碼範例呼叫 **D2D1CreateFactory** 時會使用包裝原始 API 的協助程式函式範本，因此，程式碼範例實際上是使用 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>以 **IUnknown**形式傳回介面指標的 COM 函式

[**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) 函式會透過其最後一個參數 (其中有 [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) 類型) 傳回 DirectWrite 處理站指標。 對於這類函式，請使用 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)，但透過 reinterpret cast 轉換成 **IUnknown**。

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcom_ptr"></a>重新安置 **winrt::com_ptr**

> [!IMPORTANT]
> 如果您有已安置好的 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) (其內部原始指標已有目標)，但您想將其重新安置，以指向不同目標，那麼您必須先對其指派 `nullptr`&mdash;如下列程式碼範例所示。 如果您沒有這麼做，已安置好的 **com_ptr** 將會藉由宣稱內部指標不是 Null，來提出錯誤並引起您的注意 (當您呼叫 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) 或 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) 時)。

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

若要檢查從 COM 函式傳回的 HRESULT 值，並擲回事件中的例外 (以錯誤代碼表示)，請呼叫 [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)。

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>採用特定介面指標的 COM 函式

您可以呼叫 [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function) 函式來將 **com_ptr** 傳遞給採用同類型特定介面指標的函式。

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>採用 **IUnknown** 介面指標的 COM 函式

您可以呼叫 [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown) 免費函式來將 **com_ptr** 傳遞給採用 **IUnknown** 介面指標的函式。 如需程式碼範例，請參閱該主題。

## <a name="passing-and-returning-com-smart-pointers"></a>傳遞及傳回 COM 智慧型指標

以 **winrt::com_ptr** 形式採用 COM 智慧型指標的函式應透過常數參考或參考來執行此動作。

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

傳回 **winrt::com_ptr** 的函式應透過值來執行此動作。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>查詢不同介面的 COM 智慧型指標

您可以使用 [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) 函式來查詢不同介面的 COM 智慧型指標。 如果查詢不成功，函式會擲回例外狀況。

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

或者，使用 [**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrtry_as-function)，該函式傳回的值可讓您根據 `nullptr` 查看查詢是否成功。

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>極簡 Direct2D 應用程式的完整原始程式碼清單

如果您要建置並執行此原始程式碼範例，您必須先在 Visual Studio 中建立新的**核心應用程式 (C++/WinRT)** 。 `Direct2D` 是合適的專案名稱，但您可以隨意命名。

開啟 `pch.h`，並在加入 `windows.h` 之後，立即新增 `#include <unknwn.h>`。 這是因為我們使用 [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown)。 每當使用 **winrt::get_unknown** 時，對 `#include <unknwn.h>` 而言的確是個不錯的選擇，即使該標頭已包含在另一個標頭中。

開啟 `App.cpp`、刪除整個內容，然後貼入下列清單。

下列程式碼會在適當情況下使用 [winrt::com_ptr::capture 函式](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcapture-function)。 `WINRT_ASSERT` 是巨集定義，而且會發展為 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

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
        factory.capture(adapter, &IDXGIAdapter::GetParent);
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        surface.capture(swapchain, &IDXGISwapChain1::GetBuffer, 0);

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
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>使用 COM 類型 (例如 BSTR 和 VARIANT)

如您所見，C++/WinRT 會提供實作和呼叫 COM 介面的支援。 若要使用 COM 類型 (例如 BSTR 和 VARIANT)，我們建議您使用 [Windows Implementation Libraries (WIL)](https://github.com/Microsoft/wil) 所提供的包裝函式，例如 **wil::unique_bstr** 和 **wil::unique_variant** (這些函式會管理資源存留期)。

[WIL](https://github.com/Microsoft/wil) 會取代 Active Template Library (ATL) 等架構和 Visual C++ 編譯器的 COM 支援。 我們建議您用此包裝函式來覆寫自己的包裝函式，或是以原始形式使用 BSTR 和 VARIANT 等 COM 類型 (搭配適當的 API)。

## <a name="avoiding-namespace-collisions"></a>避免命名空間衝突

自由使用 using 指示詞是 C++/WinRT 中常見的作法 (如本主題中列出的程式碼所示)。 不過，在某些情況下，這可能會導致將衝突名稱匯入全域命名空間的問題。 以下是範例。

C++/WinRT 包含名為 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown) 的類型；而 COM 會定義名為 [ **::IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) 的類型。 因此，在 C++/WinRT 專案中，請考慮下列取用 COM 標頭的程式碼。

```cppwinrt
using namespace winrt::Windows::Foundation;
...
void MyFunction(IUnknown*); // error C2872:  'IUnknown': ambiguous symbol
```

非限定名稱 IUnknown  在全域命名空間中發生衝突，因此造成「符號模稜兩可」  的編譯器錯誤。 您可以改為將 C++/WinRT 版的名稱隔離至 **winrt** 命名空間，如下所示。

```cppwinrt
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

或者，如果您想要利用 `using namespace winrt` 的方便性，您也可以這麼做。 您只需要對全域版本的 *IUnknown* 限定資格即可，如下所示。

```cppwinrt
using namespace winrt;
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(::IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

當然，這適用於任何 C++/WinRT 命名空間。

```cppwinrt
namespace winrt
{
    using namespace Windows::Storage;
    using namespace Windows::System;
}
```

接著，舉例來說，您就可以直接將 **winrt::Windows::Storage::StorageFile** 參考為 **winrt::StorageFile**。

## <a name="important-apis"></a>重要 API
* [winrt::check_hresult 函式](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr 結構範本](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown 結構](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
