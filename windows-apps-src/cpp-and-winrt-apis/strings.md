---
author: stevewhims
description: 使用 C++/WinRT，您可以使用標準 C++ 寬字串類型，或可以使用 winrt::hstring 類型，呼叫 Windows 執行階段 API。
title: C++/WinRT 中的字串處理
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、字串
ms.localizationpriority: medium
ms.openlocfilehash: 72032c3c522a8434d266842a83c443889e8efc19
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5976764"
---
# <a name="string-handling-in-cwinrt"></a>C++/WinRT 中的字串處理

與[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，您可以呼叫 Windows 執行階段 Api 使用 c + + 標準程式庫寬字串類型例如**std:: wstring** (注意： 無法與縮小字串類型例如**std:: string**)。 C++/WinRT 有一個稱為 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 的自訂字串類型 (在 C++/WinRT 基底程式庫中定義，也就是 `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`)。 且這是 Windows 執行階段建構函式、函式和屬性確實採用與傳回的字串類型。 但在許多情況下&mdash;多虧了 **hstring** 的轉換建構函式和轉換運算子&mdash;您可以選擇是否要注意用戶端程式碼中的 **hstring**。 如果您正 *撰寫* API，您更需要深入了解 **hstring**。

C++ 有許多字串類型。 許多程式庫裡存在著變體，除了來自 C++ 標準程式庫的 **std::basic_string** 以外。 C++17 有字串轉換公用程式，以及 **std::basic_string_view**，來橋接所有字串類型間的間隔。  [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 提供 **std::wstring_view** 的轉換性，以提供 **std::basic_string_view** 為設計目的的互通性。

## <a name="using-stdwstring-and-optionally-winrthstring-with-uri"></a>與 Uri 搭配使用 **std::wstring** (以及選擇性的 ****winrt::hstring****) 
從 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 建構 [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri)。

```cppwinrt
public:
    Uri(winrt::hstring uri) const;
```

但是 **hstring** 有[轉換建構函式](/uwp/api/windows.foundation.uri#hstringhstring-constructor)，讓您在不知不覺中使用它。 以下是程式碼範例顯示如何從寬字串常值、寬字串檢視，以及 **std::wstring**，製作一個 **Uri**。

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

屬性存取子 [**Uri::Domain**](https://docs.microsoft.com/uwp/api/windows.foundation.uri.Domain) 類型為 **hstring**。

```cppwinrt
public:
    winrt::hstring Domain();
```

但，再試一次，請注意到該詳細資料是選擇性的，多虧了 **hstring**的[**std::wstring_view** 的轉換運算子](/uwp/api/hstring#hstringoperator-stdwstringview)。

```cppwinrt
// Access a property of type hstring, via a conversion operator to a standard type.
std::wstring domainWstring{ contosoUri.Domain() }; // L"contoso.com"
domainWstring = awUri.Domain(); // L"adventure-works.com"

// Or, you can choose to keep the hstring unconverted.
hstring domainHstring{ contosoUri.Domain() }; // L"contoso.com"
domainHstring = awUri.Domain(); // L"adventure-works.com"
```

同樣地，[**IStringable::ToString**](https://msdn.microsoft.com/library/windows/desktop/dn302136)傳回 hstring。

```cppwinrt
public:
    hstring ToString() const;
```

**Uri**實作[**IStringable**](https://msdn.microsoft.com/library/windows/desktop/dn302135)介面。

```cppwinrt
// Access hstring's IStringable::ToString, via a conversion operator to a standard type.
std::wstring tostringWstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringWstring = awUri.ToString(); // L"http://www.adventure-works.com/"

// Or you can choose to keep the hstring unconverted.
hstring tostringHstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringHstring = awUri.ToString(); // L"http://www.adventure-works.com/"
```

您可以使用 [**hstring::c_str 函式**](/uwp/api/windows.foundation.uri#hstringcstr-function) 以從 **hstring** (就像您可以從 **std::wstring** 一樣) 取得標準寬字串。

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

**hstring** 有一個成員 **std::wstring_view** 轉換運算子，且免費達成轉換。

```cppwinrt
void legacy_print(std::wstring_view view);

void Print(winrt::hstring const& hstring)
{
    legacy_print(hstring);
}
```

## <a name="winrthstring-functions-and-operators"></a>**winrt::hstring**函式和運算子
建構函式的主機、運算子、函式以及為 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 實作 Iterator。

**Hstring** 是一個範圍，讓您可以有範圍基礎 `for`，或有 `std::for_each` 來使用它。 它也提供比較運算子，用於自然且有效率地在 C++ 標準程式庫中的比較其對應項目。 且它包含了您使用 **hstring** 做為關聯容器的索引鍵所需的所有項目。

我們了解，許多 C++ 程式庫使用 **std::string**，以及單獨搭配 UTF-8 文字。 為了方便起見，我們提供可向前與向後轉換的協助程式如[**winrt::to_string**](/uwp/cpp-ref-for-winrt/to-string) 和 [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring)。

```cppwinrt
winrt::hstring w{ L"Hello, World!" };

std::string c = winrt::to_string(w);
WINRT_ASSERT(c == "Hello, World!");

w = winrt::to_hstring(c);
WINRT_ASSERT(w == L"Hello, World!");
```

如需有關 **hstring** 函式和運算子的詳細範例與資訊，請參閱 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) API 參考主題。

## <a name="the-rationale-for-winrthstring-and-winrtparamhstring"></a>**winrt::hstring**和**winrt::param::hstring** 的原理
用 **wchar_t**字元實作 Windows 執行階段，但 Windows 執行階段的應用程式二進位介面 (ABI)，不是 **std::wstring** 或 **std::wstring_view** 提供的子集。 使用這些會導致嚴重的效率不彰。 反之，C++/WinRT 提供 **winrt::hstring**，用來表示與基礎 [HSTRING](https://msdn.microsoft.com/library/windows/desktop/br205775) 一致的不可變更字串，且在類似於 **std::wstring** 的介面後面實作。 

您可能會注意到， 邏輯上應該接受 **winrt::hstring** 的 C++/ WinRT 輸入參數，確實預期 **winrt::param::hstring**。 **param** 命名空間包含一組專屬於優化輸入參數的類型，自然繫結至 C++ 標準程式庫類型，且避免複製及其它效率不彰。 您不應該直接使用這些類型。 如果您要使用最佳化自己的函式，請使用 **std::wstring_view**。

結論是您可以大致忽略特定的 Windows 執行階段字串管理，並只搭配使用您知道的效率。 且這很重要，了解在 Windows 執行階段中重度使用字串的程度。

# <a name="formatting-strings"></a>格式化字串
字串格式化的一個選項是 **std::wstringstream**。 以下是格式與顯示簡單偵錯追蹤訊息的範例。

```cppwinrt
#include <sstream>
...
void OnPointerPressed(IInspectable const&, PointerEventArgs const& args)
{
    float2 const point = args.CurrentPoint().Position();
    std::wstringstream wstringstream;
    wstringstream << L"Pointer pressed at (" << point.x << L"," << point.y << L")" << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());
}
```

## <a name="important-apis"></a>重要 API
* [winrt::hstring 結構](/uwp/cpp-ref-for-winrt/hstring)
* [winrt:: to_hstring 函式](/uwp/cpp-ref-for-winrt/to-hstring)
* [winrt:: to_string 函式](/uwp/cpp-ref-for-winrt/to-string)
