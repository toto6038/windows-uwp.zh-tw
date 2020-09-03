---
description: 使用 C++/WinRT，您可以使用標準 C++ 寬字串類型呼叫 Windows 執行階段 API，或可以使用 winrt::hstring 類型。
title: C++/WinRT 中的字串處理
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 字串
ms.localizationpriority: medium
ms.openlocfilehash: 56b75710c2d259e59dac476bcb860a5e4c6938d6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154392"
---
# <a name="string-handling-in-cwinrt"></a>C++/WinRT 中的字串處理

有了 [C++/WinRT](./intro-to-using-cpp-with-winrt.md)，您可以使用 C++ 標準程式庫寬字串類型 (例如 **std::wstring**) ，來呼叫 Windows 執行階段 API (請注意：無法與縮小字串類型例如 **std::string** 共用)。 C++/WinRT 有一個稱為 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 的自訂字串類型 (在 C++/WinRT 基底程式庫中定義，也就是 `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`)。 且這是 Windows 執行階段建構函式、函式和屬性確實採用與傳回的字串類型。 但在許多情況下，&mdash;由於 **hstring** 的轉換建構函式和轉換運算子&mdash;您可以選擇是否要注意用戶端程式碼中的 **hstring**。 如果您正在*撰寫* API，更需要深入了解 **hstring**。

C++ 有許多字串類型。 許多程式庫裡存在著變體，除了來自 C++ 標準程式庫的 **std::basic_string** 以外。 C++17 有字串轉換公用程式，以及 **std::basic_string_view**，來橋接所有字串類型間的間隔。  [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 提供 **std::wstring_view** 的轉換性，以提供 **std::basic_string_view** 為設計目的的互通性。

## <a name="using-stdwstring-and-optionally-winrthstring-with-uri"></a>將 **std::wstring** (並選擇性地使用 **winrt::hstring**) 與 **Uri** 搭配使用
[**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) 建構自 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。

```cppwinrt
public:
    Uri(winrt::hstring uri) const;
```

但是 **hstring** 有[轉換建構函式](/uwp/cpp-ref-for-winrt/hstring#hstringhstring-constructor)，讓您在不知不覺中使用它。 以下是程式碼範例顯示如何從寬字串常值、寬字串檢視，以及 **std::wstring**，製作一個 **Uri**。

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <string_view>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    using namespace std::literals;

    winrt::init_apartment();

    // You can make a Uri from a wide string literal.
    Uri contosoUri{ L"http://www.contoso.com" };

    // Or from a wide string view.
    Uri contosoSVUri{ L"http://www.contoso.com"sv };

    // Or from a std::wstring.
    std::wstring wideString{ L"http://www.adventure-works.com" };
    Uri awUri{ wideString };
}
```

屬性存取子 [**Uri::Domain**](/uwp/api/windows.foundation.uri.Domain) 的類型為 **hstring**。

```cppwinrt
public:
    winrt::hstring Domain();
```

但是，由於 **hstring** 對 [**std::wstring_view** 的轉換運算子](/uwp/cpp-ref-for-winrt/hstring#hstringoperator-stdwstring_view)，可注意到該詳細資料是選擇性的。

```cppwinrt
// Access a property of type hstring, via a conversion operator to a standard type.
std::wstring domainWstring{ contosoUri.Domain() }; // L"contoso.com"
domainWstring = awUri.Domain(); // L"adventure-works.com"

// Or, you can choose to keep the hstring unconverted.
hstring domainHstring{ contosoUri.Domain() }; // L"contoso.com"
domainHstring = awUri.Domain(); // L"adventure-works.com"
```

同樣地，[**IStringable::ToString**](/windows/desktop/api/windows.foundation/nf-windows-foundation-istringable-tostring) 傳回 hstring。

```cppwinrt
public:
    hstring ToString() const;
```

**Uri** 實作 [**IStringable**](/windows/desktop/api/windows.foundation/nn-windows-foundation-istringable) 介面。

```cppwinrt
// Access hstring's IStringable::ToString, via a conversion operator to a standard type.
std::wstring tostringWstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringWstring = awUri.ToString(); // L"http://www.adventure-works.com/"

// Or you can choose to keep the hstring unconverted.
hstring tostringHstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringHstring = awUri.ToString(); // L"http://www.adventure-works.com/"
```

您可以使用 [**hstring::c_str 函式**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) 以從 **hstring** 取得標準寬字串 (就像您可以從 **std::wstring** 一樣)。

```cppwinrt
#include <iostream>
std::wcout << tostringHstring.c_str() << std::endl;
```
如果您有 **hstring**，然後您可以從其製作 **Uri**。

```cppwinrt
Uri awUriFromHstring{ tostringHstring };
```

請考慮採用 **hstring** 的方法。

```cppwinrt
public:
    Uri CombineUri(winrt::hstring relativeUri) const;
```

所有您方才看到的選項也適用於此類案例。

```cppwinrt
std::wstring contact{ L"contact" };
contosoUri = contosoUri.CombineUri(contact);
    
std::wcout << contosoUri.ToString().c_str() << std::endl;
```

**hstring** 有一個成員 **std::wstring_view** 轉換運算子，而且可以免費進行轉換。

```cppwinrt
void legacy_print(std::wstring_view view);

void Print(winrt::hstring const& hstring)
{
    legacy_print(hstring);
}
```

## <a name="winrthstring-functions-and-operators"></a>**winrt::hstring** 函式和運算子
為 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 實作許多建構函式、運算子、函式和迭代器。

**hstring** 是一個範圍，讓您可以與範圍架構的 `for` 或與 `std::for_each` 搭配使用。 它也提供比較運算子，可以自然且有效率地與 C++ 標準程式庫中的對應項目進行比較。 而且它包含您使用 **hstring** 做為關聯容器之金鑰所需的所有項目。

我們了解，許多 C++ 程式庫使用 **std::string**，以及單獨搭配 UTF-8 文字。 為了方便起見，我們提供可向前與向後轉換的協助程式，例如 [**winrt::to_string**](/uwp/cpp-ref-for-winrt/to-string) 和 [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring)。

`WINRT_ASSERT` 是巨集定義，而且會發展為 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

```cppwinrt
winrt::hstring w{ L"Hello, World!" };

std::string c = winrt::to_string(w);
WINRT_ASSERT(c == "Hello, World!");

w = winrt::to_hstring(c);
WINRT_ASSERT(w == L"Hello, World!");
```

如需有關 **hstring** 函式和運算子的詳細範例與資訊，請參閱 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) API 參考主題。

## <a name="the-rationale-for-winrthstring-and-winrtparamhstring"></a>**winrt::hstring** 和 **winrt::param::hstring** 的原理
用 **wchar_t** 字元實作 Windows 執行階段，但 Windows 執行階段的應用程式二進位介面 (ABI) 不是 **std::wstring** 或 **std::wstring_view** 提供的子集。 使用這些會導致嚴重的效率不彰。 反之，C++/WinRT 提供 **winrt::hstring**，用來表示與基礎 [HSTRING](/windows/desktop/WinRT/hstring) 一致的不可變更字串，且在類似於 **std::wstring** 的介面後面實作。 

您可能會注意到， 邏輯上應該接受 **winrt::hstring** 的 C++/ WinRT 輸入參數，實際預期為 **winrt::param::hstring**。 **param** 命名空間包含一組專門用於最佳化輸入參數的類型，以自然繫結至 C++ 標準程式庫類型，並避免複製及其它效率不彰。 您不應該直接使用這些類型。 如果要對自己的函式使用最佳化，請使用 **std::wstring_view**。 另請參閱[將參數傳入 ABI 界限](./pass-parms-to-abi.md)。

結論是您可以大致忽略特定的 Windows 執行階段字串管理，並只搭配使用您知道的效率。 了解在 Windows 執行階段中重度使用字串的程度，這一點很重要。

## <a name="formatting-strings"></a>格式化字串
字串格式化的一個選項是 **std::wostringstream**。 以下是格式與顯示簡單偵錯追蹤訊息的範例。

```cppwinrt
#include <sstream>
#include <winrt/Windows.UI.Input.h>
#include <winrt/Windows.UI.Xaml.Input.h>
...
void MainPage::OnPointerPressed(winrt::Windows::UI::Xaml::Input::PointerRoutedEventArgs const& e)
{
    winrt::Windows::Foundation::Point const point{ e.GetCurrentPoint(nullptr).Position() };
    std::wostringstream wostringstream;
    wostringstream << L"Pointer pressed at (" << point.X << L"," << point.Y << L")" << std::endl;
    ::OutputDebugString(wostringstream.str().c_str());
}
```

## <a name="the-correct-way-to-set-a-property"></a>設定屬性的正確方式

您可以藉由將值傳遞至 setter 函式來設定屬性。 以下是範例。

```cppwinrt
// The right way to set the Text property.
myTextBlock.Text(L"Hello!");
```

下列是不正確的程式碼。 其會進行編譯，但只是修改由 **Text()** 存取子函式傳回的暫存 **winrt::hstring**，然後捨棄結果。

```cppwinrt
// *Not* the right way to set the Text property.
myTextBlock.Text() = L"Hello!";
```

## <a name="important-apis"></a>重要 API
* [winrt::hstring 結構](/uwp/cpp-ref-for-winrt/hstring)
* [winrt::to_hstring 函式](/uwp/cpp-ref-for-winrt/to-hstring)
* [winrt::to_string 函式](/uwp/cpp-ref-for-winrt/to-string)