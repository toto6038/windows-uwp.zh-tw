---
description: 本主題示範可用於 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 與 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 物件之間轉換的輔助函式。
title: C++/WinRT 與 C++/CX 之間的互通性
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 移植, 移轉, 互通性, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: c786256efb5488fff65a8e2bdb4c5d2ca0fa181c
ms.sourcegitcommit: a9f44bbb23f0bc3ceade3af7781d012b9d6e5c9a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88180793"
---
# <a name="interop-between-cwinrt-and-ccx"></a>C++/WinRT 與 C++/CX 之間的互通性

閱讀本主題之前，請先參閱[從 C++/CX 移至 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx) 主題中的資訊。 該主題介紹兩個主要策略選項，可將您的 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 專案移植到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。

- 一次移植整個專案。 就不太大的專案而言，這是最簡單的選項。 如果您有 Windows 執行階段元件專案，則此策略是您唯一的選項。
- 逐步移植專案 (可能因程式碼基底的大小或複雜度而必須採用此選項)。 但在採用此策略時，您所執行的移植程序必須在一段時間內讓 C++/CX 和 C++/WinRT 程式碼並存於相同的專案中。 針對 XAML 專案，您的 XAML 頁面類型無論何時都必須完全是 C++/WinRT *或*完全是 C++/CX。

此相互操作主題適用於*第二種*策略&mdash;也就是您需要逐步移植專案的案例。 本主題說明兩個可用來將相同專案內的 C++/CX 物件轉換為 C++/WinRT 物件 (或反向操作) 的 Helper 函式。

當您從 C++/CX 逐步將程式碼移植到 C++/WinRT 時，這些 Helper 函式會非常有用。 或者，您也可以選擇在相同的專案中同時使用 C++/WinRT 和 C++/CX 語言投影 (無論您是否要移植)，然後使用這些 Helper 函式在兩者之間相互操作。

在閱讀本主題之後，如需如何在相同專案中支援並存的 PPL 工作和協同程式 (例如，從工作鏈結呼叫協同程式) 的說明資訊和程式碼範例，請參閱更進階的主題 [C++/WinRT 與 C++/CX 之間的非同步和相互操作](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx-async)。

## <a name="the-from_cx-and-to_cx-functions"></a>**from_cx** 和 **to_cx** 函式

以下是名為 `interop_helpers.h` 之標頭檔的原始程式碼清單，其中包含兩個轉換 Helper 函式。 下列各節將說明這些函式，以及如何在您的專案中建立和使用標頭檔。

```cppwinrt
// interop_helpers.h
#pragma once

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
```

### <a name="the-from_cx-function"></a>**from_cx** 函式

**from_cx** Helper 函式會將 C++/CX 物件轉換為對等的 C++/WinRT 物件。 函式將 C++/CX 物件轉換為其基礎 [**IUnknown**](/windows/win32/api/unknwn/nn-unknwn-iunknown) 介面指標。 然後它在該指標上呼叫 [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))，以查詢 C++/WinRT 物件的預設介面。 **QueryInterface** 是 Windows 執行階段應用程式二進位介面 (ABI)，相當於 C++/CX `safe_cast` 擴充功能。 而且，[**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) 函式會擷取 C++/WinRT 物件之基礎 **IUnknown** 介面指標的位址，使其可設為另一個值。

### <a name="the-to_cx-function"></a>**to_cx** 函式

**to_cx** Helper 函式會將 C++/WinRT 物件轉換為對等的 C++/CX 物件。 [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) 函式將指標擷取至 C++/WinRT 物件的基礎 **IUnknown** 介面。 此函式會將該指標轉換為 C++/CX 物件，然後使用 C++/CX `safe_cast` 擴充功能來查詢要求的 C++/CX 類型。

### <a name="the-interop_helpersh-header-file"></a>`interop_helpers.h` 標頭檔

若要在您的專案中使用這兩個 Helper 函式，請遵循下列步驟。

- 將新的**標頭檔 (.h)** 項目新增至您的專案，並將其命名為 `interop_helpers.h`。
- 將 `interop_helpers.h` 的內容取代為上方的程式碼清單。
- 將這些包含項目新增至 `pch.h`。

```cppwinrt
// pch.h
...
#include <unknwn.h>
// Include C++/WinRT projected Windows API headers here.
...
#include <interop_helpers.h>
```

## <a name="taking-a-ccx-project-and-adding-cwinrt-support"></a>取用 C++/CX 專案並新增 C++/WinRT 支援

本節說明您決定要取用現有的 C++/CX 專案、在其中新增 C++/WinRT 支援，並在該處執行移植時，所應執行的動作。 另請參閱 [C++/WinRT 的 Visual Studio 支援](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

若要在 C++/CX 專案中混用 C++/CX 和 C++/WinRT&mdash;包括在專案中使用 **from_cx** 和 **to_cx** Helper 函式&mdash;您必須手動將 C++/WinRT 支援新增至專案。

首先，請在 Visual Studio 中開啟 C++/CX 專案，並確認專案屬性 [一般]\>[目標平台版本] 設為 10.0.17134.0 (Windows 10，1803 版) 或更高版本。

### <a name="install-the-cwinrt-nuget-package"></a>安裝 C++/WinRT NuGet 套件

[Microsoft.Windows.CppWinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)提供 C++/WinRT 組建支援 (MSBuild 屬性和目標)。 若要加以安裝，請按一下功能表項目 [專案]\>[管理 NuGet 套件...]\>[瀏覽]、在搜尋方塊中輸入或貼上 **Microsoft.Windows.CppWinRT**、在搜尋結果中選取項目，然後按一下 [安裝] 以安裝適用於該專案的套件。

> [!IMPORTANT]
> 安裝 C++/WinRT NuGet 套件會導致在專案中關閉 C++/CX 的支援。 如果您要進行一次性移植，最好讓該支援關閉，讓組建訊息協助您尋找 (及移植) C++/CX 上的所有相依性 (最終將純 C++/CX 專案轉換為純 C++/WinRT 專案)。 但請參閱下一節，以取得重新加以開啟的相關資訊。

### <a name="turn-ccx-support-back-on"></a>重新開啟 C++/CX 支援

如果您要進行一次性移植，則不需要這麼做。 但是，如果您需要逐步移植，此時您必須在專案中重新開啟 C++/CX 支援。 在專案屬性中：[C/C++]\>[一般]\>[使用 Windows 執行階段擴充功能]\>[是 (/ZW)]。

或者，如果除此之外還有 XAML 專案，您可以在 Visual Studio 中使用 C++/WinRT 專案屬性頁來新增 C++/CX 支援。 在專案屬性中，瀏覽至 [通用屬性]\>[C++/WinRT]\>[專案語言]\>[C++/CX]。 這麼做會將下列屬性新增至您的 `.vcxproj` 檔案。

```xml
<syntaxhighlight lang="xml">
  <PropertyGroup Label="Globals">
    <CppWinRTProjectLanguage>C++/CX</CppWinRTProjectLanguage>
  </PropertyGroup>
</syntaxhighlight>
```

> [!IMPORTANT]
> 每當您需要透過建置將 **Midl 檔案 (.idl)** 的內容處理為 Stub 檔案時，您就必須將 [專案語言] 變更回 [C++/WinRT]。 在組建產生了這些 Stub 之後，請將 [專案語言] 變更回 [C++/CX]。

如需類似自訂選項 (可微調 `cppwinrt.exe` 工具的行為) 清單，請參閱 Microsoft.Windows.CppWinRT NuGet 套件[讀我檔案](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing)。

### <a name="include-cwinrt-header-files"></a>包含 C++/WinRT 標頭檔

您至少應在先行編譯的標頭檔 (通常是 `pch.h`) 中包含 `winrt/base.h`，如下所示。

```cppwinrt
// pch.h
...
#include <winrt/base.h>
...
```

但在絕大多數的情況下，您都需要 **winrt::Windows::Foundation** 命名空間中的類型。 而且，您可能已經知道您所需的其他命名空間。 因此，請包含對應至這些命名空間的 C++/WinRT 投影 Windows API 標頭 (此時無須明確包含 `winrt/base.h`，因為其會自動包含在內)。

```cppwinrt
// pch.h
...
#include <winrt/Windows.Foundation.h>
// Include any other C++/WinRT projected Windows API headers here.
...
```

另請參閱下一節中的程式碼範例 (*取用 C++/WinRT 專案並新增 C++/CX 支援*)，以了解使用命名空間別名 `namespace cx` 和 `namespace winrt` 的技術。 該技術也可讓您處理 C++/WinRT 投影與 C++/CX 投影之間可能產生的命名空間衝突。

### <a name="add-interop_helpersh-to-the-project"></a>將 `interop_helpers.h` 新增至專案

現在，您可以將 **from_cx** 和 **to_cx** 函式新增至您的 C++/CX 專案。 如需其操作方式的指示，請參閱前述的 [**from_cx** 和 **to_cx** 函式](#the-from_cx-and-to_cx-functions)一節。

## <a name="taking-a-cwinrt-project-and-adding-ccx-support"></a>取用 C++/WinRT 專案並新增 C++/CX 支援

本節說明您決定要建立新的 C++/WinRT 專案並在該處執行移植時所應執行的動作。

若要在 C++/WinRT 專案中混用 C++/WinRT 和 C++/CX&mdash;包括在專案中使用 **from_cx** 和 **to_cx** Helper 函式&mdash;您必須手動將 C++/CX 支援新增至專案。

- 使用一個 C++/WinRT 專案範本，在 Visual Studio 中建立新的 C++/WinRT 專案 (請參閱 [C++/WinRT 的 Visual Studio 支援](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。
- 開啟 C++/CX 的專案支援。 在專案屬性中：[C/C++]** [一般]** \> ** [使用 Windows 執行階段擴充功能]** \> ** [是 \(/ZW\)\]** \> ****。

### <a name="an-example-cwinrt-project-showing-the-two-helper-functions-in-use"></a>顯示使用中的兩個 Helper 函式的範例 C++/WinRT 專案

在本節中，您可以建立範例 C++/WinRT 專案，以示範如何使用 **from_cx** 和 **to_cx**。 該專案也說明您可以如何對不同的程式碼孤立區使用命名空間別名，以處理 C++/WinRT 投影與 C++/CX 投影之間可能產生的命名空間衝突。

- 建立 **Visual C++** \> **Windows 通用** \> **核心應用程式 (C++/WinRT)** 專案。
- 在專案屬性中：[C/C++]** [一般]** \> ** [使用 Windows 執行階段擴充功能]** \> ** [是 \(/ZW\)\]** \> ****。
- 將 `interop_helpers.h` 新增至專案。 如需其操作方式的指示，請參閱前述的 [**from_cx** 和 **to_cx** 函式](#the-from_cx-and-to_cx-functions)一節。
- 將 `App.cpp` 的內容取代為下方的程式碼清單，並在建置後執行。

`WINRT_ASSERT` 是巨集定義，而且會發展為 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

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
* [IUnknown 介面](/windows/win32/api/unknwn/nn-unknwn-iunknown)
* [QueryInterface 函式](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [winrt::get_abi 函式](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::put_abi 函式](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [從 C++/CX 移到 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx)
* [C++/WinRT 與 C++/CX 之間的非同步和相互操作](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx-async)
