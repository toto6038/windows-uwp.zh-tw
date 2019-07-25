---
description: 本主題示範如何將 C# 程式碼移植到其在 C++/WinRT 中的對等項目。
title: 從 C# 移到 C++/WinRT
ms.date: 07/15/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 連接埠, 遷移, C#
ms.localizationpriority: medium
ms.openlocfilehash: a63d38db613ebe6425a05ed20563405242ffd441
ms.sourcegitcommit: ba4a046793be85fe9b80901c9ce30df30fc541f9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/19/2019
ms.locfileid: "68328865"
---
# <a name="move-to-cwinrt-from-c"></a>從 C# 移到 C++/WinRT

本主題示範如何將 [C#](/visualstudio/get-started/csharp) 專案移植到其在 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中的對等項目。

## <a name="register-an-event-handler"></a>註冊事件處理常式

您可以在 XAML 標記中註冊事件處理常式。

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

在 C# 中，**OpenButton_Click** 方法可以是私有的，而且 XAML 仍可將其連線到 *OpenButton* 所引發的 [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件。

在 C++/WinRT 中，**OpenButton_Click** 方法在您的[實作類型](/windows/uwp/cpp-and-winrt-apis/author-apis)中必須是公用的。

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

或者，您可以讓註冊類別成為實作類別的同伴，並且讓 **OpenButton_Click** 成為私有的。

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

## <a name="making-a-class-available-to-the-binding-markup-extension"></a>讓類別可供 {Binding} 標記延伸使用

如果您想要使用 {binding} 標記延伸將資料繫結至您的資料類型，請參閱[使用 {Binding} 宣告的繫結物件](/windows/uwp/data-binding/data-binding-in-depth#binding-object-declared-using-binding)。

## <a name="making-a-data-source-available-to-xaml-markup"></a>讓資料來源可供 XAML 標記使用

在 C++/WinRT 2.0.190530.8 版和更新版本中，[**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 會建立可觀察的向量，其同時支援 **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** 和 **IObservableVector\<IInspectable\>** 。

您可以撰寫如下所示的 **Midl 檔案 (.idl)** (另請參閱[將執行階段類別分解成 Midl 檔檔案 (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl))。

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

其實作方式如下所示。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

如需詳細資訊，請參閱 [XAML 項目控制項；繫結至 C++/WinRT 集合](/windows/uwp/cpp-and-winrt-apis/binding-collection)。

## <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>讓資料來源可供 XAML 標記使用 (在 C++/WinRT 2.0.190530.8 之前)

XAML 資料繫結要求項目來源實作 **[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\>** ，以及下列其中一個介面組合。

- **IObservableVector\<IInspectable\>**
- **IBindableVector** 和 **INotifyCollectionChanged**
- **IBindableVector** 和 **IBindableObservableVector**
- **IBindableVector** 本身 (不會回應變更)
- **IVector\<IInspectable\>**
- **IBindableIterable** (會逐一查看元素並儲存至私用集合)

在執行階段無法偵測 **IVector\<T\>** 等一般介面。 每個 **IVector\<T\>** 都有不同的介面識別碼 (IID)，這是 **T**的函數。任何開發人員都可以任意擴充 **T** 集合，所以顯然 XAML 繫結程式碼永遠不會知道要查詢的完整集合。 該限制不是 C# 的問題，因為每個實作 **IEnumerable\<T\>** 的 CLR 物件都會實作 **IEnumerable**。 在 ABI 層級，這表示每個實作 **IObservableVector\<T\>** 的物件都會自動實作 **IObservableVector\<IInspectable\>** 。

C++/WinRT 不提供該保證。 如果 C++/WinRT 執行階段類別會實作 **IObservableVector\<T\>** ，我們無法假設也會提供 **IObservableVector\<IInspectable\>** 的實作。

因此，前一個範例需要如下所示。

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

其實作方式。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

如果您需要存取 *m_bookSkus* 中的物件，則必須將其 QI 回到 **Bookstore::BookSku**。

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

## <a name="tostring"></a>ToString()

C# 類型會提供 [Object.ToString](/dotnet/api/system.object.tostring) 方法。

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

C++/WinRT 不會直接提供此功能，但您可以轉向使用替代方案。

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT 也針對有限的類型數量支援 [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring)。 您必須為想要字串化的任何其他類型新增多載。

| 語言 | 將 int 字串化 | 將列舉字串化 |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

在將列舉字串化的情況下，您需要提供 **winrt::to_hstring** 的實作。

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

資料繫結通常會隱含地使用這些字串化作業。

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

這些繫結會執行所繫結屬性的 **winrt::to_hstring**。 在第二個範例 (**StatusEnum**) 的情況下，您必須提供自己的 **winrt::to_hstring** 多載，否則會收到編譯器錯誤。

## <a name="string-building"></a>字串建立

針對字串建立，C# 有內建的 [**StringBuilder**](/dotnet/api/system.text.stringbuilder) 類型。

| | C# | C++/WinRT |
|-|-|-|
| 產生器類別 | `StringBuilder builder;` | `std::wstringstream stream;` |
| 附加字串，保留 null | `builder.Append(s);` | `stream << std::wstring_view{ s };` |
| 擷取結果 | `s = builder.ToString();` | `ws = stream.str();` |

## <a name="boxing-and-unboxing"></a>Box 處理和 Unbox 處理

C# 會自動將純量 Box 處理為物件。 C++/WinRT 會要求您明確地呼叫 [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 函式。 這兩種語言都需要您明確地進行 Unbox 處理。 請參閱[使用 C++/WinRT 進行 Box 處理和 Unbox 處理](/windows/uwp/cpp-and-winrt-apis/boxing)。

在後續表格中，我們將使用下列定義。

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| 操作 | C# | C++/WinRT|
|-|-|-|
| Box 處理 | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Unbox 處理 | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

如果您嘗試將 null 指標 Unbox 處理為某個實值類型，則 C++/CX 和 C# 會引發例外狀況。 C++/WinRT 會將此視為程式設計錯誤，並且毀損。 在 C++/WinRT 中，如果您想處理物件不是您所認為類型的情況，請使用 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 函式。

| 案例 | C# | C++/WinRT|
|-|-|-|
| 進行已知整數的 Unbox 處理 |`i = (int)o;` | `i = unbox_value<int>(o);` |
| 如果 o 為 null | `System.NullReferenceException` | 毀損 |
| 如果 o 不是已 Box 處理的 int | `System.InvalidCastException` | 毀損 |
| 進行 int 的 Unbox 處理，若為 null 則使用遞補；若為其他任何項目則會毀損 | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| 可能的話，進行 int 的 Unbox 處理；其他任何項目使用遞補 | `var box = o as int?;`<br>`i = box != null ? box.Value : fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>進行字串的 Box 處理和 Unbox 處理

字串在某些方面是實值類型，而在其他方面則是參考類型。 C# 和 C++/WinRT 會以不同的方式處理字串。

ABI 類型 [**HSTRING**](/windows/win32/winrt/hstring) 是參考計數字串的指標。 但是它並非衍生自 [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)，因此在技術上並不是「物件」  。 此外, null **HSTRING** 代表空字串。 將非衍生自 **IInspectable** 的項目包裝在 [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_)內，即可完成 Box 處理，而 Windows 執行階段會以 [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) 物件形式提供標準實作 (自訂類型會回報為 [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype))。

C# 表示作為參考類型的 Windows 執行階段字串；而 C++/WinRT 會將字串投影為實值類型。 這表示已進行 Box 處理的 null 字串可以有不同的表示法 (取決於您達成的方式)。

| 操作 | C# | C++/WinRT|
|-|-|-|
| 字串類型類別 | 參考類型 | 值類型 |
| null  **HSTRING** 投影為 | `""` | `hstring{ nullptr }` |
| Null 和 `""` 相同嗎？ | 否 | 是 |
| Null 的有效性 | `s = null;`<br>`s.Length` 引發 **NullReferenceException** | `s = nullptr;`<br>`s.size() == 0` (有效) |
| 進行字串的 Box 處理 | `o = s;` | `o = box_value(s);` |
| 如果 `s` 為 `null` | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{nullptr});`<br>`o != nullptr` |
| 如果 `s` 為 `""` | `o = "";`<br>`o != null;` | `o = box_value(hstring{L""});`<br>`o != nullptr;` |
| 進行字串的 Box 處理並保留 null | `o = s;` | `o = s.empty() ? nullptr : box_value(s);` |
| 強制進行字串的 Box 處理 | `o = PropertyValue.CreateString(s);` | `o = box_value(s);` |
| 進行已知字串的 Unbox 處理 | `s = (string)o;` | `s = unbox_value<hstring>(o);` |
| 如果 `o` 為 null | `s == null; // not equivalent to ""` | 毀損 |
| 如果 `o` 不是已 Box 處理的 int | `System.InvalidCastException` | 毀損 |
| 進行字串的 Unbox 處理，若為 null 則使用遞補；若為其他任何項目則會毀損 | `s = o != null ? (string)o : fallback;` | `s = o ? unbox_value<hstring>(o) : fallback;` |
| 可能的話，進行字串的 Unbox 處理；其他任何項目使用遞補 | `var s = o as string ?? fallback;` | `s = unbox_value_or<hstring>(o, fallback);` |

在上述兩個「使用遞補進行 Unbox 處理」  案例中，null 字串可能已強制進行 Box 處理，在此情況下則不會使用遞補。 產生的值會是空字串，因為這是方塊中的內容。

## <a name="derived-classes"></a>衍生類別

為了從執行階段類別衍生，基底類別必須「可組合」  。 C# 不要求您採取任何特殊步驟，即可讓類別變為可組合，而 C++/WinRT 則會要求您採取步驟。 您可使用[未密封的關鍵字](/uwp/midl-3/intro#base-classes)，指出您希望類別可作為基底類別使用。

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

在實作標頭類別中，您必須先包含基底類別標頭檔，才可包含衍生類別的自動產生標頭。 否則，您會收到錯誤，例如「此類型當作運算式使用並不合法」。

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="consuming-objects-from-xaml-markup"></a>取用 XAML 標記中的物件

在 C# 專案中，您可以使用來自 XAML 標記的私有成員和具名元素。 但是在 C++/WinRT 中，使用 XAML [ **{x:Bind} 標記延伸**](/windows/uwp/xaml-platform/x-bind-markup-extension) 取用的所有實體都必須公開於 IDL 中。

此外，布林值的繫結會在 C# 中顯示 `true` 或 `false`，但是在 C++/WinRT 中顯示 **Windows.Foundation.IReference`1\<Boolean\>** 。

如需詳細資訊和程式碼範例，請參閱[使用標記中的物件](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup)。

## <a name="important-apis"></a>重要 API
* [winrt::single_threaded_observable_vector 函式範本](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相關主題
* [C# 教學課程](/visualstudio/get-started/csharp)
* [使用 C++/WinRT 撰寫 API](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [深入了解資料繫結](/windows/uwp/data-binding/data-binding-in-depth)
* [XAML 項目控制項；繫結至一個 C++/WinRT 集合](/windows/uwp/cpp-and-winrt-apis/binding-collection)