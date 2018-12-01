---
description: 在傳遞至期待 **IInspectable** 的函式之前，需要將純量數值包裝在參考資料類別物件中。 該包裝處理程序稱為*boxing*值。
title: 'Boxing 和 unboxing 純量數值到使用 C++/WinRT 的 IInspectable '
ms.date: 04/10/2018
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、XAML、控制項、boxing、純量、數值
ms.localizationpriority: medium
ms.openlocfilehash: 5c86d1ac8ce83ea092ce0e2730ea0e9d4a201b94
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8350389"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrt"></a>Boxing 和 unboxing 純量數值到使用 C++/WinRT 的 IInspectable 
 
Windows 執行階段 (WinRT) 中，[**IInspectable 介面**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)是每個執行階段類別的根介面。 這是類似在每個 COM 介面與類別根的 [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)；以及在每個[一般類型系統](https://docs.microsoft.com/dotnet/standard/base-types/common-type-system)類別根的 **System.Object** 的想法。

換言之，預期 **IInspectable** 的函示可以傳遞任何執行階段類別的執行個體。 但您無法直接將純量數值，例如數字或文字值，傳遞至這項函式。 而是需要將純量數值包裝於參考類別物件中。 該包裝處理程序稱為*boxing*值。

[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)提供[**winrt:: box_value**](/uwp/cpp-ref-for-winrt/box-value)函式，它會將純量數值到**IInspectable**傳回 boxed 的值。 針對 unboxing **IInspectable**返回純量數值，有 [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) 和 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 函式。

## <a name="examples-of-boxing-a-value"></a>Boxing 值的範例
[**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) 存取子函式傳回[**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)，即為純量數值。 我們可以 box 該 **hstring** 值，並將它傳遞至預期 **IInspectable** 的函式，如下所示。

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

首先，[**hstring**](/uwp/cpp-ref-for-winrt/hstring) 轉換建構函式將字串常值轉換成 **hstring**。 然後叫用取得 **hstring** 的 **winrt::box_value** 多載。

## <a name="examples-of-unboxing-an-iinspectable"></a>Unboxing IInspectable 的範例
在您自己的預期 **IInspectable** 的函式裡，可以使用 [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) 來 Unbox，且可以使用[**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 來 unbox 預設值。

```cppwinrt
void Unbox(winrt::Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="determine-the-type-of-a-boxed-value"></a>判斷 Boxed 實值型別
如果您收到 Boxed 實值，但不確定它包含哪些類型 (需要知道其類型以將它 Unbox)，然後您就可以查詢 Boxed 實值的 [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue)介面，然後呼叫其**類型**。 以下是程式碼範例。

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
