---
author: QuinnRadich
Description: Use alignment, margin, and padding properties to arrange the layout of elements on a page.
title: 對齊、邊界及配置的邊框間距
ms.author: quradic
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4ce80e0ce4eb51876a0a0ecf632a7c47e8894aac
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6840129"
---
# <a name="alignment-margin-padding"></a>對齊、邊界、邊框間距

UWP 應用程式中，大部分的使用者介面 (UI) 元素從 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 類別繼承。 每個 FrameworkElement 皆有影響配置行為的維度、邊界及填補屬性。 下列指導方針提供有關如何使用這些配置屬性，確定您應用程式的 UI 可辨識且在任何操作概觀中皆容易使用。

## <a name="dimensions-height-width"></a>維度 (高度、寬度)
適當大小調整可確保所有的內容清楚可辨識。 使用者不應捲動或縮放來解讀主要的內容。

![圖表顯示維度](images/dimensions.svg)

- [**Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) 和 [**Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) 指定元素的大小。 預設值在數學上來說是 NaN (不是數字)。 您可以設定以 [有效像素](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) 衡量的固定值，或者可以使用 **Auto** 或適用於可變式行為的 [等比例調整大小](layout-panels.md#grid)。

- [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) 和 [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) 是唯讀屬性，提供執行階段元素的大小。 如果可變式配置擴大或縮小，則值在 [**SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged)事件中變更。 請注意，[**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) 不會變更 ActualHeight 和 ActualWidth 值。

- [**MinWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[**MaxWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxwidth) 和 [**MinHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minheight)/[**MaxHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxheight) 指定限制元素大小的值，同時允許可變式調整大小。

- [**FontSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.fontsize) 和其他適用於文字元素的文字屬性控制配置大小。 文字元素沒有有明確宣告維度的同時，其已經計算 ActualWidth 和 ActualHeight。 

## <a name="alignment"></a>對齊方式
對齊方式讓您的 UI 外觀看起來很棒、有組織，且平衡，也可以用來建立視覺階層與關聯性。

![此圖顯示對齊方式](images/alignment.svg)

- [**HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) 和 [**VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) 指定元素應該如何放置於其父容器內。
    - 適用於 **HorizontalAlignment** 的值為 **Left**、**Center**、**Right** 和 **Stretch**。
    - 適用於 **VerticalAlignment** 的值為 **Top**、**Center**、**Bottom** 和 **Stretch**。

- **Stretch** 是兩個屬性的預設，且元素將可填滿父容器中提供給它們的所有空間。 高度和寬度的實際數字取消一個伸展值，其取而代之做為中心值。 某些控制項 (像是按鈕) 會在其預設樣式中覆寫這個預設伸展值。

- [**HorizontalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment) 和 [**VerticalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.verticalcontentalignment) 指定如何將子元素放置於一個容器內。

- 對齊方式可能會影響配置面板中的剪裁。 例如，使用 `HorizontalAlignment="Left"`，如果內容大於 ActualWidth，則剪裁元素的右側。

- 文字元素使用 [**TextAlignment**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.textalignment)屬性。 一般而言，我們建議使用靠左對齊的預設值。 如需樣式文字的詳細資訊，請查看[印刷樣式](../style/typography.md)。

## <a name="margin-and-padding"></a>邊界及邊框間距
邊界和邊框間距屬性讓 UI 看起來不會過於雜亂或太疏鬆，且其也可以更容易使用特定輸入，像手寫筆和觸控。 以下圖示適用於容器與其內容的邊界和邊框間距的。

![Xaml 邊界及邊框間距圖表](images/xaml-layout-margins-padding.svg)

### <a name="margin"></a>邊界
[**Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin) 控制元素周圍的空白空間量。 邊界不會在 ActualHeight 和 ActualWidth 中新增像素，而且不會基於點擊測試與來源輸入事件而被視為元素的一部分。

- 邊界值可以統一或不同。 使用 `Margin="20"` 時，會對左、上、右、下的邊界上的元素一致套用 20 像素的邊界。 使用 `Margin="0,10,5,25"`，會對左、上、右、下套用此值 (依照該順序)。 

- 邊界可以累加。 若兩個元素同時指定一致的邊界為 10 個像素，且皆為方向不拘的相鄰對等元素，則元素之間的距離會是 20 個像素。

- 允許使用負值邊界。 不過，使用負值邊界經常會造成裁剪或過度繪製對等元素，因此負值邊界並不是常用的技術。

- 最後才限制邊界值，因此使用邊界時請謹慎小心，因為容器可裁剪或限制元素。 邊界值可能會是造成元素無法轉譯的原因；套用邊界，元素的維度可限制為 0。

### <a name="padding"></a>邊框間距
[**Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.padding) 控制元素的內部框線及其子內容或元素之間的空間量。 正數的邊框間距值會減少元素的內容區域。 

不同於邊界，邊框間距不是 FrameworkElement 的屬性。 有幾個類別，每一個皆定義其自身的邊框間距屬性：

-   [**Control.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding)：繼承所有 [**Control**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls) 衍生類別。 不是所有控制項都有內容，因此對於那些控制項而言，沒有必要設定屬性。 如果控制項有邊框，將會在該邊框內套用邊框間距。
-   [**Border.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.padding)：定義 [**BorderThickness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderthickness)/[**BorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderbrush) 所建立的矩形線條與 [**Child**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.child) 元素之間的空間。
-   [**ItemsPresenter.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemspresenter.padding)：影響針對項目控制項中的項目，在每個項目周圍放上指定的邊框間距。
-   [**TextBlock.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.padding) 與 [**RichTextBlock.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.padding)：展開文字元素的文字周圍的週框方塊。 這些的文字元素沒有 **Background**，因此不容易看出。 基於這個原因，使用 [**邊界**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block.margin) 改在 [**區塊**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block)容器上設定。

不論上述哪一種情況，元素也具有邊界屬性。 若同時套用邊界與邊框間距，則是兩者相加：外部容器與任何內部內容之間的外觀距離等於邊界加上邊框間距。

### <a name="example"></a>範例
讓我們看一下邊界和邊框間距在實際控制項上的效果。 以下是 Grid 內部的 TextBox，且預設的 Margin 和 Padding 值為 0。

![邊界及邊框間距為 0 的 TextBox](images/xaml-layout-textbox-no-margins-padding.svg)

以下是 TextBox 上含有 Margin 和 Padding 值的相同 TextBox 和 Grid，如這個 XAML 中所示。

```xaml
<Grid BorderBrush="Blue" BorderThickness="4" Width="200">
    <TextBox Text="This is text in a TextBox." Margin="20" Padding="24,16"/>
</Grid>
```

![含有正數邊界及邊框間距值的 TextBox](images/xaml-layout-textbox-with-margins-padding.svg)


## <a name="style-resources"></a>樣式資源
您不需要在控制項上個別設定每個屬性值。 將屬性值群組到 [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) 資源中並將 Style 套用到控制項，通常更有效率。 這尤其適用於當您需要將相同屬性值套用到許多控制項的情況。 如需有關使用樣式的詳細資訊，請參閱[XAML 樣式](../controls-and-patterns/xaml-styles.md)。

## <a name="general-recommendations"></a>一般建議
- 僅適用於特定按鍵元素的測量值，並使用適用於其他元素的可變式配置行為。 視窗寬度變更時，這可提供給 [回應式 UI](responsive-design.md)。

- 如果您使用測量值，**所有維度、邊界與邊框間距皆應以 4 epx 遞增**。 當 UWP 使用 [有效像素與縮放比例](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) 讓所有裝置與螢幕大小上的應用程式可辨識，它會以 4 的倍數縮放 UI 元素。 藉由符合整數像素，使用以 4 遞增的值，呈現最佳轉譯。

- 針對小的視窗寬度 (小於 640 像素)，我們建議您 12 epx 邊緣，以及針對較大的視窗寬度，建議 24 epx 邊緣。

![建議的邊緣](images/12-gutter.svg)

## <a name="related-topics"></a>相關主題
* [**FrameworkElement.Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height)
* [**FrameworkElement.Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width)
* [**FrameworkElement.HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment)
* [**FrameworkElement.VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)
* [**FrameworkElement.Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin)
* [**Control.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding)
