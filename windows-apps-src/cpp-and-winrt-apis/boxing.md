---
description: 在傳遞至需要 **IInspectable** 的函示之前，必須將純量數值包裝在參考資料類別物件中。 該包裝程序稱為「boxing」  值。
title: 使用 C++/WinRT，Boxing 和 unboxing 純量數值到 IInspectable
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, XAML, 控制項, boxing, 純量, 數值
ms.localizationpriority: medium
ms.openlocfilehash: 29263260217de154f1a942d37d1e18fece15e3d0
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "79038091"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrt"></a>使用 C++/WinRT，Boxing 和 unboxing 純量數值到 IInspectable
 
Windows 執行階段 (WinRT) 中，[**IInspectable 介面**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)是每個執行階段類別的根介面。 這是類似在每個 COM 介面與類別根的 [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)；以及在每個[一般類型系統](https://docs.microsoft.com/dotnet/standard/base-types/common-type-system)類別根的 **System.Object** 的想法。

換言之，預期 **IInspectable** 的函式可以傳遞任何執行階段類別的執行個體。 但您無法直接將純量數值 (例如數字或文字值) 傳遞至此類函式。 而是需要將純量數值包裝於參考類別物件中。 該包裝程序稱為「boxing」  值。

> [!IMPORTANT]
> 您可以對任何可傳至 Windows 執行階段 API 的類型進行 Box 和 Unbox 處理。 換句話說，即 Windows 執行階段類型。 以上提供的是數值和文字值 (字串) 的範例。 另一個範例是您在 IDL 中定義的 `struct`。 如果您嘗試對一般 C++ `struct` (未定義於 IDL 中) 進行 Box 處理，則編譯器會提醒您，您只能對 Windows 執行階段類型進行 Box 處理。 執行階段類別是一種 Windows 執行階段類型，但您理當可將執行階段類別傳至 Windows 執行階段 API，而不需要進行其 Box 處理。

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 提供 [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 函式，該函式採用純量數值，並傳回經過 Box 處理到 **IInspectable** 的值。 針對將 **IInspectable** Unbox 回純量數值，有 [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) 和 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 函式。

## <a name="examples-of-boxing-a-value"></a>Boxing 值的範例
[**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) 存取子函式傳回 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)，即為純量數值。 我們可以 box 該 **hstring** 值，並將其傳遞至預期 **IInspectable** 的函式，如下所示。

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(winrt::xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

若要設定 XAML [**按鈕**](/uwp/api/windows.ui.xaml.controls.button) 的內容屬性，您可以呼叫 [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?) 更動子函式。 若要將內容屬性設定為字串值，您可以使用此程式碼。

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

首先，[**hstring**](/uwp/cpp-ref-for-winrt/hstring) 轉換建構函式將字串常值轉換成 **hstring**。 然後叫用採用 **hstring** 的 **winrt::box_value** 多載。

## <a name="examples-of-unboxing-an-iinspectable"></a>Unboxing IInspectable 的範例
在您自己的預期 **IInspectable** 函式中，可以使用 [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) 來 unbox，而且可以使用 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 來 unbox 預設值。

```cppwinrt
void Unbox(winrt::Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="determine-the-type-of-a-boxed-value"></a>判斷 Boxed 實值的類型
如果您收到 Boxed 實值，但不確定它包含哪些類型 (需要知道其類型以便 Unbox)，您可以查詢 Boxed 實值的 [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue) 介面，然後呼叫其**類型**。 以下是程式碼範例。

`WINRT_ASSERT` 是巨集定義，而且會發展為 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

```cppwinrt
float pi = 3.14f;
auto piInspectable = winrt::box_value(pi);
auto piPropertyValue = piInspectable.as<winrt::Windows::Foundation::IPropertyValue>();
WINRT_ASSERT(piPropertyValue.Type() == winrt::Windows::Foundation::PropertyType::Single);
```

## <a name="important-apis"></a>重要 API
* [IInspectable 介面](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [winrt::box_value 函式範本](/uwp/cpp-ref-for-winrt/box-value)
* [winrt::hstring 結構](/uwp/cpp-ref-for-winrt/hstring)
* [winrt::unbox_value 函式範本](/uwp/cpp-ref-for-winrt/unbox-value)
* [winrt::unbox_value_or 函式範本](/uwp/cpp-ref-for-winrt/unbox-value-or)
