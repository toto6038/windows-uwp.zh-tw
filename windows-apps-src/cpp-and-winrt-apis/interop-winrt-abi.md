---
description: 本主題示範如何在應用程式二進位介面 (ABI) 與 C++/WinRT 物件之間轉換。
title: C++/WinRT 與 ABI 之間的互通性
ms.date: 11/30/2018
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 移植, 移轉, 互通性, ABI
ms.localizationpriority: medium
ms.openlocfilehash: a1745f9ad98ed8dac2e54e17d18467981eafdcec
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "66360231"
---
# <a name="interop-between-cwinrt-and-the-abi"></a>C++/WinRT 與 ABI 之間的互通性

本主題示範如何在 SDK 應用程式二進位介面 (ABI) 與 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 物件之間轉換。 您可以使用這些技術，在搭配使用這兩種程式設計方式與 Windows 執行階段的程式碼之間進行互通，也可以在將程式碼從 ABI 逐漸移動到 C++/WinRT 時，使用這些技術。

## <a name="what-is-the-windows-runtime-abi-and-what-are-abi-types"></a>什麼是 Windows 執行階段 ABI，以及有哪些 ABI 類型？
Windows 執行階段類別 (執行階段類別) 其實是抽象概念。 這個抽象概念定義可讓各種不同程式設計語言與物件互動的二進位介面 (應用程式二進位介面或 ABI)。 無論程式設計語言為何，與 Windows 執行階段物件的用戶端程式碼互動會發生在最低層級，用戶端語言建構會轉譯成物件 ABI 的呼叫。

"%WindowsSdkDir%Include\10.0.17134.0\winrt" 資料夾中的 Windows SDK 標頭 (視需要調整案例中的 SDK 版本號碼)，是 Windows 執行階段 ABI 標頭檔案。 這些標頭是由 MIDL 編譯器產生。 這是包括其中一個標頭的範例。

```cpp
#include <windows.foundation.h>
```

以下是您在特定 SDK 標頭中找到的其中一個 ABI 類型的簡化範例。 請注意，**ABI** 命名空間；**Windows::Foundation**，以及其他 Windows 命名空間，均由 **ABI** 命名空間內的 SDK 標頭來宣告。

```cpp
namespace ABI::Windows::Foundation
{
    IUriRuntimeClass : public IInspectable
    {
    public:
        /* [propget] */ virtual HRESULT STDMETHODCALLTYPE get_AbsoluteUri(/* [retval, out] */__RPC__deref_out_opt HSTRING * value) = 0;
        ...
    }
}
```

**IUriRuntimeClass** 是 COM 介面。 不僅如此，&mdash;由於其基底是 **IInspectable**&mdash;**IUriRuntimeClass** 是 Windows 執行階段介面。 請注意，**HRESULT** 會傳回類型，而不是引發例外狀況。 並且使用構件，例如 **HSTRING** 控點 (完成時，將該控點設回 `nullptr` 是很好的做法)。 這樣可了解 Windows 執行階段在應用程式二進位層級的樣子，亦即 COM 程式設計的層級。

Windows 執行階段是根據元件物件模型 (COM) API。 您可以使用該方式存取 Windows 執行階段，或者透過「語言投影」  存取。 投影會隱藏 COM 的詳細資訊，並針對特定語言提供更自然的程式設計體驗。

例如，如果您在 "%WindowsSdkDir%Include\10.0.17134.0\cppwinrt\winrt" 資料夾中尋找 (同樣地，視需求調整您案例的 SDK 版本號碼)，然後您會找到 C++/WinRT 語言投影標頭。 每一個 Windows 命名空間有一個標頭，就像每一個 Windows 命名空間有一個 ABI 標頭。 以下是包括 C++/WinRT 標頭之一的範例。

```cppwinrt
#include <winrt/Windows.Foundation.h>
```

而且從該標頭，以下是 (簡化) C++/WinRT，相當於我們看到的該 ABI 類型。

```cppwinrt
namespace winrt::Windows::Foundation
{
    struct Uri : IUriRuntimeClass, ...
    {
        winrt::hstring AbsoluteUri() const { ... }
        ...
    };
}
```

這裡的介面是新式的標準 C++。 其會取消 **HRESULT** (C++/WinRT 視需要引發例外狀況)。 而且，存取子函式會傳回簡單的字串物件，其會在範圍的結尾清除。

本主題適用的情況，是希望與在應用程式二進位介面 (ABI) 層運作的程式碼互通或移植的情況。

## <a name="converting-to-and-from-abi-types-in-code"></a>在程式碼中轉換為 ABI 類型和從 ABI 類型轉換
為了安全和簡單起見，對於雙向轉換，只需使用 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)、[**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function)，與 [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)。 以下是一個程式碼範例 (根據**主控台應用程式**專案範本)，也會示範您可以如何對不同孤立區使用命名空間別名，處理 C++/WinRT 投影與 ABI 之間可能產生的命名空間衝突。

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");

    // Convert to an ABI type.
    winrt::com_ptr<abi::IStringable> ptr{ uri.as<abi::IStringable>() };

    // Convert from an ABI type.
    uri = ptr.as<winrt::Uri>();
    winrt::IStringable uriAsIStringable{ ptr.as<winrt::IStringable>() };
}
```

**as** 函式的實作會呼叫 [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))。 如果您想要僅呼叫 [**AddRef**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref) 的較低層級轉換，您可以使用 [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-to-abi) 和 [**winrt::copy_from_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi) 輔助函式。 下一個程式碼範例將這些較低層級轉換新增至上述的程式碼範例。

```cppwinrt
int main()
{
    // The code in main() already shown above remains here.

    // Lower-level conversions that only call AddRef.

    // Convert to an ABI type.
    ptr = nullptr;
    winrt::copy_to_abi(uri, *ptr.put_void());

    // Convert from an ABI type.
    uri = nullptr;
    winrt::copy_from_abi(uri, ptr.get());
    ptr = nullptr;
}
```

以下是其他類似地低層級轉換技術，但使用原始指標至 ABI 介面類型 (由 Windows SDK 標頭定義)。

```cppwinrt
    // The code in main() already shown above remains here.

    // Copy to an owning raw ABI pointer with copy_to_abi.
    abi::IStringable* owning{ nullptr };
    winrt::copy_to_abi(uri, *reinterpret_cast<void**>(&owning));

    // Copy from a raw ABI pointer.
    uri = nullptr;
    winrt::copy_from_abi(uri, owning);
    owning->Release();
```

針對最低層級轉換，其只能複製位址，您可以使用 [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi)、[**winrt::detach_abi**](/uwp/cpp-ref-for-winrt/detach-abi) 和 [**winrt::attach_abi**](/uwp/cpp-ref-for-winrt/attach-abi) 輔助函式。

```cppwinrt
    // The code in main() already shown above remains here.

    // Lowest-level conversions that only copy addresses

    // Convert to a non-owning ABI object with get_abi.
    abi::IStringable* non_owning{ static_cast<abi::IStringable*>(winrt::get_abi(uri)) };
    WINRT_ASSERT(non_owning);

    // Avoid interlocks this way.
    owning = static_cast<abi::IStringable*>(winrt::detach_abi(uri));
    WINRT_ASSERT(!uri);
    winrt::attach_abi(uri, owning);
    WINRT_ASSERT(uri);
```

## <a name="convertfromabi-function"></a>convert_from_abi 函式
這項輔助函式以最低額外負荷將原始 ABI 介面指標轉換成對等 C++/WinRT 物件。

```cppwinrt
template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr };

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

函式只需呼叫 [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))，來查詢要求 C++/WinRT 類型的預設介面。

如我們所了解，輔助函式不需要從 C++/WinRT 物件轉換至對等 ABI 介面指標。 只要使用 [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (或 [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)) 成員函式為要求的介面詢問。 **as** 和 **try_as** 函式傳回包裝要求 ABI 類型的 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 物件。

## <a name="code-example-using-convertfromabi"></a>使用 convert_from_abi 的程式碼範例
以下是示範實務上此輔助函式的程式碼範例。

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

namespace sample
{
    template <typename T>
    T convert_from_abi(::IUnknown* from)
    {
        T to{ nullptr };

        winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

        return to;
    }
    inline auto put_abi(winrt::hstring& object) noexcept
    {
        return reinterpret_cast<HSTRING*>(winrt::put_abi(object));
    }
}

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wcout << "C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert to an ABI type.
    winrt::com_ptr<abi::IUriRuntimeClass> ptr = uri.as<abi::IUriRuntimeClass>();
    winrt::hstring domain;
    winrt::check_hresult(ptr->get_Domain(sample::put_abi(domain)));
    std::wcout << "ABI: " << domain.c_str() << std::endl;

    // Convert from an ABI type.
    winrt::Uri uri_from_abi = sample::convert_from_abi<winrt::Uri>(ptr.get());

    WINRT_ASSERT(uri.Domain() == uri_from_abi.Domain());
    WINRT_ASSERT(uri == uri_from_abi);
}
```

## <a name="important-apis"></a>重要 API
* [AddRef 函式](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref)
* [QueryInterface 函式](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [winrt::attach_abi 函式](/uwp/cpp-ref-for-winrt/attach-abi)
* [winrt::com_ptr 結構範本](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::copy_from_abi 函式](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [winrt::copy_to_abi 函式](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [winrt::detach_abi 函式](/uwp/cpp-ref-for-winrt/detach-abi)
* [winrt::get_abi 函式](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::Windows::Foundation::IUnknown::as 成員函式](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as 成員函式](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)
