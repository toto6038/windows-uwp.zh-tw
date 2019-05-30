---
description: 提供在執行階段物件圖形中指定繫結來源的相對關係的方法。
title: RelativeSource 標記延伸
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 716ca61fc9925846377157d215ca3326191915b7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371143"
---
# <a name="relativesource-markup-extension"></a>{RelativeSource} 標記延伸


提供在執行階段物件圖形中指定繫結來源的相對關係的方法。

## <a name="xaml-attribute-usage-self-mode"></a>XAML 屬性用法 (Self 模式)

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## <a name="xaml-attribute-usage-templatedparent-mode"></a>XAML 屬性用法 (TemplatedParent 模式)

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 詞彙 | 描述 |
|------|-------------|
| {RelativeSource Self} | 產生的 [<strong>Mode</strong>](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.relativesource.mode) 值是 <strong>Self</strong>。 目標元素應做為這個繫結的來源。 在同一個元素中繫結一個元素的屬性到另一個元素時，這個值很有用。 |
| {RelativeSource TemplatedParent} | 產生 [<strong>ControlTemplate</strong>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)，以套用來做為這個繫結的來源。 在範本層級將執行階段資訊套用到繫結時，這個值很有用。 | 

## <a name="remarks"></a>備註

[  **Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 可以將 [**Binding.RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource) 設成 **Binding** 物件元素上的屬性，或是設成 [{Binding} 標記延伸](binding-markup-extension.md)內的元件。 這就是為什麼顯示兩個不同 XAML 語法的緣故。

**RelativeSource** 與 [{Binding} 標記延伸](binding-markup-extension.md)類似。  它也是可以傳回本身的執行個體的標記延伸，並且支援實質上會傳遞引數到建構函式的字串型建構。 在此案例中，所傳遞的引數為 [**Mode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.relativesource.mode) 值。

將元素的一個屬性繫結到同一元素的另一個屬性時，**Self** 模式就很有用，且這也是不需命名然後再自我參考元素的 [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) 繫結的變化型。 如果您將一個元素的屬性繫結到同一元素的另一個屬性，這兩個屬性必須使用相同的屬性類型，或者是您也必須在要轉換值的繫結中使用 [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter)。 例如，您可以使用 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 做為 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 的來源而不需轉換，但是如果要使用 [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) 做為 [**Visibility**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Visibility) 的來源，就需要轉換器。

這裡提供一個範例。 這個 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 使用一個 [{Binding} 標記延伸](binding-markup-extension.md)，這樣便能讓它的 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 和 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 一律相等，且會轉譯成正方形。 只有 Height 是設定成固定值。 針對這個 **Rectangle**，它的預設 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 為 **null**，而不是 **this**。 因此，若要建立資料內容來源以做為物件本身 (並啟用繫結至它的其他屬性)，我們會在 {Binding} 標記延伸用法中使用 `RelativeSource={RelativeSource Self}` 引數。

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

另一個 `RelativeSource={RelativeSource Self}` 的用法，是將物件的 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 設定到其本身的做法。  例如，您可能會看到這項技術，在某些 SDK 範例的位置[**頁**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)類別已擴充加上自己的資料繫結 已經提供已準備好要移至檢視模型的自訂屬性例如： `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**附註**  XAML 使用量**RelativeSource**顯示它適用的使用狀況： 設定的值[ **Binding.RelativeSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource)在 XAML 做為繫結運算式的一部分。 理論上，如果設定值為 [**RelativeSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.RelativeSource) 的屬性，也可以有其他用法。

## <a name="related-topics"></a>相關主題

* [XAML 概觀](xaml-overview.md)
* [深入了解資料繫結](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)
* [{Binding} 標記延伸](binding-markup-extension.md)
* [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**RelativeSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.RelativeSource)

