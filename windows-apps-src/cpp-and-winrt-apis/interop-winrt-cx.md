---
description: 本主題示範可用於 C++/CX 與 C++/WinRT 物件之間轉換的協助程式函式。
title: C++/WinRT 與 C++/CX 之間的互通性
ms.date: 10/09/2018
ms.topic: article
keywords: Windows 10，uwp、標準、c++、cpp、winrt、投影、連接埠、移轉、互通性、C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 5394443b4832864e5b46bfbf917c04f0af6d8a19
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360222"
---
# <a name="interop-between-cwinrt-and-ccx"></a>C++/WinRT 與 C++/CX 之間的互通性

策略逐漸移植程式碼，在您[ C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)專案加入[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)討論[移至C++/WinRT 從C++/CX](move-to-winrt-from-cx.md)。

本主題說明您可以使用之間進行轉換的兩個協助程式函式C++/CX 和C++/WinRT 相同專案中的物件。 您可以使用它們來使用兩個語言、 預測的程式碼的互通，或從您程式碼，您可以使用函式C++以 /CX C++/WinRT。

## <a name="fromcx-and-tocx-functions"></a>from_cx 和 to_cx 函式
下述的協助程式函式將 C++/CX 物件轉換為對等 C++/WinRT 物件。 函式將 C++/CX 物件轉換為其基礎 [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)介面指標。 然後它在該指標上呼叫 [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) 為 C++/WinRT 物件的預設介面詢問。 **QueryInterface** 是 Windows 執行階段應用程式二進位介面 (ABI) 相當於 C++/CX safe_cast 擴充功能。 且，[**Winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) 函式擷取 C++/WinRT 物件的基礎 **IUnknown** 介面指標的位址，讓它可以設定另一個值。

```cppwinrt
template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

下述的協助程式函式將 C++/WinRT 物件轉換為對等 C++/CX 物件。 [  **Winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) 函式將指標擷取至 C++/WinRT 物件的基礎 **IUnknown** 介面。 函式將該指標轉換為 C++/CX 物件之後，使用 C++/CX safe_cast 擴充功能來查詢要求的 C++/CX 類型。

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>顯示使用中的兩個協助程式函式的範例專案

若要重現，簡單的方式，逐漸移植的程式碼中的案例C++/CX 專案C++/WinRT，您可以開始建立新的專案在 Visual Studio 中使用的其中一個C++/WinRT 專案範本 (請參閱[Visual Studio支援C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。

此範例專案也會說明如何使用命名空間別名為不同的資料島，程式碼，以處理否則可能發生之間的命名空間衝突C++/WinRT 投影和C++/CX 投影。

- 建立**Visual C++**  \> **Windows Universal** > **Core 應用程式 (C++/WinRT)** 專案。
- 在 專案屬性**C /C++**  \> **一般** \> **使用 Windows 執行階段延伸模組** \> **是 (/ZW)** 。 這會開啟專案支援 C + /CX。
- 內容取代`App.cpp`下方列出的程式碼。

```cppwinrt
// App.cpp
#include "pch.h"
#include <sstream>

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows;
    using namespace Windows::ApplicationModel::Core;
    using namespace Windows::Foundation;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI;
    using namespace Windows::UI::Core;
    using namespace Windows::UI::Composition;
}

template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}

template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}

struct App : winrt::implements<App, winrt::IFrameworkViewSource, winrt::IFrameworkView>
{
    winrt::CompositionTarget m_target{ nullptr };
    winrt::VisualCollection m_visuals{ nullptr };
    winrt::Visual m_selected{ nullptr };
    winrt::float2 m_offset{};

    winrt::IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(winrt::CoreApplicationView const &)
    {
    }

    void Load(winrt::hstring const&)
    {
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        winrt::CoreWindow window = winrt::CoreWindow::GetForCurrentThread();
        window.Activate();

        winrt::CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(winrt::CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(winrt::CoreWindow const & window)
    {
        winrt::Compositor compositor;
        winrt::ContainerVisual root = compositor.CreateContainerVisual();
        m_target = compositor.CreateTargetForCurrentView();
        m_target.Root(root);
        m_visuals = root.Children();

        window.PointerPressed({ this, &App::OnPointerPressed });
        window.PointerMoved({ this, &App::OnPointerMoved });

        window.PointerReleased([&](auto && ...)
        {
            m_selected = nullptr;
        });
    }

    void OnPointerPressed(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        winrt::float2 const point = args.CurrentPoint().Position();

        for (winrt::Visual visual : m_visuals)
        {
            winrt::float3 const offset = visual.Offset();
            winrt::float2 const size = visual.Size();

            if (point.x >= offset.x &&
                point.x < offset.x + size.x &&
                point.y >= offset.y &&
                point.y < offset.y + size.y)
            {
                m_selected = visual;
                m_offset.x = offset.x - point.x;
                m_offset.y = offset.y - point.y;
            }
        }

        if (m_selected)
        {
            m_visuals.Remove(m_selected);
            m_visuals.InsertAtTop(m_selected);
        }
        else
        {
            AddVisual(point);
        }
    }

    void OnPointerMoved(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        if (m_selected)
        {
            winrt::float2 const point = args.CurrentPoint().Position();

            m_selected.Offset(
            {
                point.x + m_offset.x,
                point.y + m_offset.y,
                0.0f
            });
        }
    }

    void AddVisual(winrt::float2 const point)
    {
        winrt::Compositor compositor = m_visuals.Compositor();
        winrt::SpriteVisual visual = compositor.CreateSpriteVisual();

        static winrt::Color colors[] =
        {
            { 0xDC, 0x5B, 0x9B, 0xD5 },
            { 0xDC, 0xED, 0x7D, 0x31 },
            { 0xDC, 0x70, 0xAD, 0x47 },
            { 0xDC, 0xFF, 0xC0, 0x00 }
        };

        static unsigned last = 0;
        unsigned const next = ++last % _countof(colors);
        visual.Brush(compositor.CreateColorBrush(colors[next]));

        float const BlockSize = 100.0f;

        visual.Size(
        {
            BlockSize,
            BlockSize
        });

        visual.Offset(
        {
            point.x - BlockSize / 2.0f,
            point.y - BlockSize / 2.0f,
            0.0f,
        });

        m_visuals.InsertAtTop(visual);

        m_selected = visual;
        m_offset.x = -BlockSize / 2.0f;
        m_offset.y = -BlockSize / 2.0f;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wstringstream wstringstream;
    wstringstream << L"C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert from a C++/WinRT type to a C++/CX type.
    cx::Uri^ cx = to_cx<cx::Uri>(uri);
    wstringstream << L"C++/CX: " << cx->Domain->Data() << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());

    // Convert from a C++/CX type to a C++/WinRT type.
    winrt::Uri uri_from_cx = from_cx<winrt::Uri>(cx);
    WINRT_ASSERT(uri.Domain() == uri_from_cx.Domain());
    WINRT_ASSERT(uri == uri_from_cx);

    winrt::CoreApplication::Run(winrt::make<App>());
}
```

## <a name="important-apis"></a>重要 API
* [IUnknown 介面](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [QueryInterface 函式](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [winrt::get_abi function](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::put_abi function](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [從 C++/CX 移到 C++/WinRT](move-to-winrt-from-cx.md)
