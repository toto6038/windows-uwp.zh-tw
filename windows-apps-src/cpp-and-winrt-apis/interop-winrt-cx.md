---
author: stevewhims
description: 本主題示範可用於 C++/CX 與 C++/WinRT 物件之間轉換的協助程式函式。
title: C++/WinRT 與 C++/CX 之間的互通性
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10，uwp、標準、c++、cpp、winrt、投影、連接埠、移轉、互通性、C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 02aa86231cd611bd20a386d3da2f9d2b6dc5df66
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2018
ms.locfileid: "2885755"
---
# <a name="interop-between-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-and-ccx"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 與 C++/CX 之間的互通性
本主題示範可用於 [C + + / CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 與 C++/WinRT 物件之間轉換的協助程式函式。 您可用於 interop 之間使用兩種語言、 預測的程式碼或函數可以用逐漸移動時您的程式碼從 C + + CX C + WinRT (請參閱[移至 C + + WinRT 從 C + + CX](move-to-winrt-from-cx.md))。

## <a name="fromcx-and-tocx-functions"></a>from_cx 和 to_cx 函式
下述的協助程式函式將 C++/CX 物件轉換為對等 C++/WinRT 物件。 函式將 C++/CX 物件轉換為其基礎 [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)介面指標。 然後它在該指標上呼叫 [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) 為 C++/WinRT 物件的預設介面詢問。 **QueryInterface** 是 Windows 執行階段應用程式二進位介面 (ABI) 相當於 C++/CX safe_cast 擴充功能。 且，[**Winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) 函式擷取 C++/WinRT 物件的基礎 **IUnknown** 介面指標的位址，讓它可以設定另一個值。

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

下述的協助程式函式將 C++/WinRT 物件轉換為對等 C++/CX 物件。 [**Winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) 函式將指標擷取至 C++/WinRT 物件的基礎 **IUnknown** 介面。 函式將該指標轉換為 C++/CX 物件之後，使用 C++/CX safe_cast 擴充功能來查詢要求的 C++/CX 類型。

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="code-example"></a>程式碼範例
以下是一個程式碼範例 (根據 C++/CX**空白應用程式**專案範本) 示範使用中的兩個協助程式函式。 它也會示範您可以如何對不同孤立區使用命名空間別名，處理 C++/WinRT 投影與 C++/CX 投影之間可能產生的命名空間衝突。

```cppwinrt
// MainPage.xaml.cpp

#include "pch.h"
#include "MainPage.xaml.h"
#include <winrt/Windows.Foundation.h>
#include <sstream>

using namespace InteropExample;

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows::Foundation;
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

MainPage::MainPage()
{
    InitializeComponent();

    winrt::init_apartment(winrt::apartment_type::single_threaded);

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
}
```

## <a name="important-apis"></a>重要 API
* [IUnknown 介面](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [winrt::get_abi 函式](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::put_abi 函式](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
