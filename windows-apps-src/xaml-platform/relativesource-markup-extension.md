---
description: 提供在執行階段物件圖形中指定繫結來源的相對關係的方法。
title: RelativeSource 標記延伸
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d8d81adab37f942f804fadb3d8d1ec0cefd20380
ms.sourcegitcommit: 249100d990cd5cf2854c59fa66803b7f83d5db96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2021
ms.locfileid: "105938963"
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
| {RelativeSource Self} | 產生的 [<strong>Mode</strong>](/uwp/api/windows.ui.xaml.data.relativesource.mode) 值是 <strong>Self</strong>。 目標元素應做為這個繫結的來源。 在同一個元素中繫結一個元素的屬性到另一個元素時，這個值很有用。 |
| {RelativeSource TemplatedParent} | 產生 [<strong>ControlTemplate</strong>](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)，以套用來做為這個繫結的來源。 在範本層級將執行階段資訊套用到繫結時，這個值很有用。 |

## <a name="remarks"></a>備註

[**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) 可以將 [**Binding.RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource) 設成 **Binding** 物件元素上的屬性，或是設成 [{Binding} 標記延伸](binding-markup-extension.md)內的元件。 這就是為什麼顯示兩個不同 XAML 語法的緣故。

**RelativeSource** 與 [{Binding} 標記延伸](binding-markup-extension.md)類似。  它也是可以傳回本身的執行個體的標記延伸，並且支援實質上會傳遞引數到建構函式的字串型建構。 在此案例中，所傳遞的引數為 [**Mode**](/uwp/api/windows.ui.xaml.data.relativesource.mode) 值。

將元素的一個屬性繫結到同一元素的另一個屬性時，**Self** 模式就很有用，且這也是不需命名然後再自我參考元素的 [**ElementName**](/uwp/api/windows.ui.xaml.data.binding.elementname) 繫結的變化型。 如果您將一個元素的屬性繫結到同一元素的另一個屬性，這兩個屬性必須使用相同的屬性類型，或者是您也必須在要轉換值的繫結中使用 [**Converter**](/uwp/api/windows.ui.xaml.data.binding.converter)。 例如，您可以使用 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 做為 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 的來源而不需轉換，但是如果要使用 [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) 做為 [**Visibility**](/uwp/api/Windows.UI.Xaml.Visibility) 的來源，就需要轉換器。

以下為範例。 這個 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 使用一個 [{Binding} 標記延伸](binding-markup-extension.md)，這樣便能讓它的 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 和 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 一律相等，且會轉譯成正方形。 只有 Height 是設定成固定值。 針對這個 **Rectangle**，它的預設 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 為 **null**，而不是 **this**。 因此，若要建立資料內容來源以做為物件本身 (並啟用繫結至它的其他屬性)，我們會在 {Binding} 標記延伸用法中使用 `RelativeSource={RelativeSource Self}` 引數。

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

另一個 `RelativeSource={RelativeSource Self}` 的用法，是將物件的 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 設定到其本身的做法。  例如，您可能會在某些 SDK 範例中看到這項技術，其中 [**頁面**](/uwp/api/Windows.UI.Xaml.Controls.Page) 類別已擴充為自訂屬性，該屬性已經針對自己的資料系結提供現成的視圖模型，例如： `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**注意****RelativeSource** 的 XAML 用法僅顯示它的預期用法：在 XAML 中設定 [**Binding.RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource) 的值以做為繫結運算式的一部分。 理論上，如果設定值為 [**RelativeSource**](/uwp/api/Windows.UI.Xaml.Data.RelativeSource) 的屬性，也可以有其他用法。

## <a name="related-topics"></a>相關主題

* [XAML 概觀](xaml-overview.md)
* [深入了解資料繫結](../data-binding/data-binding-in-depth.md)
* [{Binding} 標記延伸](binding-markup-extension.md)
* [**綁定**](/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**RelativeSource**](/uwp/api/Windows.UI.Xaml.Data.RelativeSource)