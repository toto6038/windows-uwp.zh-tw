---
description: 必須先將純量或陣列值包裝在參考類別物件內，才能傳遞至預期 **IInspectable** 的函式。 該包裝程序稱為「boxing」值。
title: 使用 c + +/WinRT 將值裝箱和取消 IInspectable
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, XAML, 控制項, boxing, 純量, 數值
ms.localizationpriority: medium
ms.openlocfilehash: 08c36c735b319bb1658b2d4ce745ae0fdb7bdeee
ms.sourcegitcommit: b89d3bc42713fbe4c0ada99d6f514f1304821221
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2021
ms.locfileid: "107466418"
---
# <a name="boxing-and-unboxing-values-to-iinspectable-with-cwinrt"></a>使用 c + +/WinRT 將值裝箱和取消 IInspectable

> [!NOTE]
> 您不僅可以將純量值放在一起，也可以將大部分的陣列 (，但使用 [**winrt：： box_value**](/uwp/cpp-ref-for-winrt/box-value) 和 [**winrt：： unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) 函式) 的列舉陣列除外。 您只能使用 [**winrt：： unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 函式，將純量值取消裝箱。

Windows 執行階段 (WinRT) 中，[**IInspectable 介面**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)是每個執行階段類別的根介面。 這是類似在每個 COM 介面與類別根的 [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown)；以及在每個 [一般類型系統](/dotnet/standard/base-types/common-type-system)類別根的 **System.Object** 的想法。

換言之，預期 **IInspectable** 的函式可以傳遞任何執行階段類別的執行個體。 但是您無法直接將純量值傳遞給這類函式 (例如數值或文字值) 以及陣列。 相反地，純量或陣列值必須包裝在參考類別物件內。 該包裝程序稱為「boxing」值。

> [!IMPORTANT]
> 您可以將任何可傳遞至 Windows 執行階段 API 的類型加入方塊並取消裝箱。 換句話說，Windows 執行階段型別。 數值和文字值 (字串) 和陣列是上述的一些範例。 另一個範例是 `struct` 您在 IDL 中定義的。 如果您嘗試將 `struct` 未在 IDL) 中定義的一般 c + + (，則編譯器會提醒您，您只能將 Windows 執行階段類型。 執行時間類別是 Windows 執行階段型別，但是您當然可以將執行時間類別傳遞給 Windows 執行階段的 Api，而不需要將它們裝箱。

[C + +/WinRT](./intro-to-using-cpp-with-winrt.md) 提供 [**WinRT：： box_value**](/uwp/cpp-ref-for-winrt/box-value) 函式，它接受純量或陣列值，並傳回已封裝至 **IInspectable** 的值。 若要將 **IInspectable** 取消加入純量或陣列值，可以使用 [**winrt：： unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) 函式。 若要將 **IInspectable** 取消加入純量值，也有 [**winrt：： unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 函式。

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
如果您收到 Boxed 實值，但不確定它包含哪些類型 (需要知道其類型以便 Unbox)，您可以查詢 Boxed 實值的 [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue) 介面，然後呼叫其 **類型**。 以下是程式碼範例。

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
