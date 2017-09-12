---
author: Jwmsft
Description: "XAML 提供彈性的版面配置系統來建立可回應的 UI。"
title: "使用 XAML 定義版面配置"
ms.assetid: 8D4E4162-1C9C-48F4-8A94-34976FB17079
label: Page layouts with XAML
template: detail.hbs
op-migration-status: ready
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: a491a13264a19c50affdbacded69c7ff73e99afa
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/22/2017
---
# <a name="define-page-layouts-with-xaml"></a>使用 XAML 定義頁面版面配置

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

XAML 提供彈性的版面配置系統，讓您能夠使用自動調整大小、版面配置面板、視覺狀態，甚至分開的 UI 定義來建立回應式 UI。 有了靈活的設計，您就可以讓應用程式在不同應用程式視窗尺寸、解析度、像素密度及方向的螢幕上看起來更美觀。

我們將在此處討論如何使用 XAML 屬性和版面配置面板，讓您的應用程式具有回應性及調適性。 您可以在 [UWP 應用程式設計簡介](../layout/design-and-ui-intro.md)中找到我們建置所依據的回應式 UI 設計與技術重要資訊。 您應該了解什麼是有效像素，以及了解每一種回應式設計技術：重新置放、調整大小、自動重排、顯示、取代及重新架構。

> [!NOTE]
> 您的應用程式版面配置是從您選擇的瀏覽模型開始，例如，是否使用 [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) 搭配[索引標籤和樞紐](../controls-and-patterns/tabs-pivot.md)模型，或是 [**SplitView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.aspx) 搭配[瀏覽窗格](../controls-and-patterns/navigationview.md)模型。 如需詳細資訊，請參閱 [UWP 應用程式的瀏覽設計基本知識](../layout/navigation-basics.md)。 我們將在此處討論如何讓單一頁面或元素群組的版面配置具備回應性的技術。 無論您為應用程式選擇的是哪一個瀏覽模型，此資訊都適用。

XAML 架構提供數個可用來建立回應式 UI 的最佳化層級。
- **流暢版面配置**
    使用版面配置屬性與面板來讓預設的 UI 更加流暢。

    回應式版面配置的基礎在於適當地使用版面配置屬性和面板，以進行內容的重新置放、調整大小及自動重排。 您可以在元素上設定固定大小，或者使用自動調整大小，讓父版面配置面板調整它的大小。 各種 [**Panel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.panel.aspx) 類別 (例如 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx)、[**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)、[**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) 和 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx)) 都提供不同的方式來為它們的子系調整大小和位置。

- **調適型配置**
    使用視覺狀態，根據視窗大小或其他變更，大幅更改您的 UI。

    當應用程式視窗放大或縮小的範圍超過一定數量時，您可能想要更改版面配置屬性，以重新置放、調整大小、自動重排或取代 UI 的區段。 您可以為 UI 定義不同的視覺狀態，並在視窗寬度或視窗長度超出指定閾值時套用它們。 [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx) 提供一種簡單的方法來設定套用狀態的閾值 (也稱為中斷點)。

- **量身訂做的版面配置**
    量身訂做的版面配置會針對特定的裝置系列或螢幕大小範圍進行最佳化。 在裝置系列中，版面配置仍然應該在支援的視窗大小範圍內回應變更並配合調整。
    > **注意**&nbsp;&nbsp; 使用者可藉由 [Continuum 手機版](http://go.microsoft.com/fwlink/p/?LinkID=699431)，將手機連線到螢幕、滑鼠與鍵盤。 這個功能融合了手機和桌面裝置系列之間的界限。

    量身訂做的方法包括
    - 建立自訂觸發程序

    您可以建立裝置系列的觸發程序，並修改它的 setter，與調適型觸發程序。

    - 使用不同的 XAML 檔案，針對每個裝置系列定義不同的檢視。

    您可以將個別的 XAML 檔案與同一個程式碼檔案搭配使用，為每個裝置系列定義 UI 的檢視。

    - 使用個別的 XAML 和程式碼，為每個裝置系列提供不同的實作。

    您可以提供不同的頁面實作 (XAML 和程式碼)，然後根據裝置系列、畫面大小或其他規格，瀏覽到特定實作。

## <a name="layout-properties-and-panels"></a>版面配置屬性與面板

版面配置是調整物件大小並將物件定位在 UI 的程序。 若要定位視覺物件，您必須將它們放在 Panel 或其他容器物件中。 XAML 架構提供各種 Panel 類別 (例如 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx)、[**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)、[**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) 和 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx)) 做為容器，您可以在其中放置和排列 UI 元素。

XAML 版面配置系統支援靜態與流暢版面配置。 在靜態配置中，您提供控制項明確的像素大小與位置。 當使用者變更裝置的解析度或方向時，UI 不會變更。 靜態版面配置在不同的硬體規格、畫面大小中會遭到裁剪。

流暢的版面配置可縮小、放大和自動重排，以回應裝置上的可用視覺空間。 若要建立流暢的版面配置，請針對元素、對齊方式、邊界及邊框間距使用自動或等比例調整大小，並視需要讓版面配置面板來放置其子系。 您可以指定子元素彼此間的排列關係以及與其內容和/或父元素的相對大小調整方式來排列子元素。

實際上，可以使用靜態與流暢元素的組合來建立 UI。 您仍然會在某些地方使用靜態元素與值，但請確定整體 UI 具備回應性，並可配合不同的解析度、版面配置及檢視來調整。

### <a name="layout-properties"></a>版面配置屬性

為了控制元素的大小與位置，您要設定其版面配置屬性。 下列是一些常見的版面配置屬性及其效果。

**高度和寬度**

設定 [**Height**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) 和 [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) 屬性來指定元素的大小。 您可以使用以有效像素衡量的固定值，或者可以使用自動或等比例調整大小。 若要在執行階段取得元素的大小，請使用 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.actualheight.aspx) 和 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.actualwidth.aspx) 屬性，而不是 Height 和 Width。

您可以使用自動調整大小，讓 UI 元素調整大小以符合它們的內容或父容器。 您也可以使用自動調整大小搭配方格的列與欄。 若要使用自動調整大小，請將 UI 元素的 Height 和/或 Width 設定為 **Auto**。

> [!NOTE]
> 元素是否會調整大小以符合其內容或容器，取決於它的 [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) 和 [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx) 屬性值，以及父容器如何處理調整其子系大小的方式。 如需詳細資訊，請參閱本文後續內容中的[對齊方式]()和[版面配置面板]()。

您使用等比例調整大小 (亦稱為*「星號調整」*)，按照權重比例，將可用的空間分配給方格的列和欄。 在 XAML 中，星號值的表示方法為 \* (或加權星號調整則為 *n*\*)。 例如，若要在 2 欄的版面配置中，將某一欄的寬度設定為第二欄的 5 倍，請使用 "5\*" 和 "\*" 來表示 [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.aspx) 元素中的 [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.width.aspx) 屬性。

這個範例會在具有 4 欄的 [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) 中結合固定、自動和等比例調整大小。

&nbsp;|&nbsp;|&nbsp;
------|------|------
Column_1 | **自動** | 會調整欄的大小以容納其內容。
Column_2 | * | 計算 Auto 欄之後，這個欄會分配到一部分的剩餘寬度。 Column_2 會是 Column_4 的一半寬度。
Column_3 | **44** | 此欄寬度為 44 個像素。
Column_4 | **2**\* | 計算 Auto 欄之後，這個欄會分配到一部分的剩餘寬度。 Column_4 會是 Column_2 的兩倍寬度。

預設欄的寬度是 "*"，因此不需要為第二欄明確設定這個值。

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="44"/>
        <ColumnDefinition Width="2*"/>
    </Grid.ColumnDefinitions>
    <TextBlock Text="Column 1 sizes to its conent." FontSize="24"/>
</Grid>
```

在 Visual Studio XAML 設計工具中，結果看起來就像這樣。

![Visual Studio 設計工具中的 4 欄格線](images/xaml-layout-grid-in-designer.png)

**大小限制**

在 UI 中使用自動調整大小時，仍然需要設置元素大小的限制。 您可以設定 [**MinWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.minwidth.aspx)/[**MaxWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxwidth.aspx) 和 [**MinHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.minheight.aspx)/[**MaxHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxheight.aspx) 屬性，來指定限制元素大小的值，同時允許流暢的調整大小。

在 Grid 中，MinWidth/MaxWidth 也可以與欄定義搭配使用，而 MinHeight/MaxHeight 可以與列定義搭配使用。

**對齊方式**

使用 [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) 和 [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx) 屬性，來指定元素應該如何放置於其父容器內。
- 適用於 **HorizontalAlignment** 的值為 **Left**、**Center**、**Right** 和 **Stretch**。
- 適用於 **VerticalAlignment** 的值為 **Top**、**Center**、**Bottom** 和 **Stretch**。

利用 **Stretch** 對齊方式，元素將可填滿父容器中提供給它們的所有空間。 Stretch 是這兩個對齊屬性的預設值。 不過，某些控制項 (像是 [**Button**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.button.aspx)) 會在其預設樣式中覆寫這個值。
任何可含有子元素的元素都能以獨特的方式來處理 HorizontalAlignment 和 VerticalAlignment 屬性的 Stretch 值。 例如，使用放置於 Grid 中之預設 Stretch 值的元素，會向兩邊延伸以填滿包含它的儲存格。 放置於 Canvas 中的相同元素會調整大小以符合其內容。 如需每個面板如何處理 Stretch 值的詳細資訊，請參閱[版面配置面板](layout-panels.md)文章。

如需詳細資訊，請參閱[對齊方式、邊界及邊框間距](alignment-margin-padding.md)文章，以及 [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) 和 [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx) 參考頁面。

控制項也具有 [**HorizontalContentAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.horizontalcontentalignment.aspx) 和 [**VerticalContentAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.verticalcontentalignment.aspx) 屬性，讓您可用來指定它們放置其內容的方式。 並非所有的控制項都能使用這些屬性。 它們只會在控制項的範本針對展示器或其中的內容區域，使用屬性做為 HorizontalAlignment/VerticalAlignment 值的來源時，影響該控制項的版面配置行為。

針對 [TextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx)、[TextBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) 及 [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx)，請使用 **TextAlignment** 屬性來控制在控制項中的文字對齊方式。

**邊界及邊框間距**

設定 [**Margin**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.margin.aspx) 屬性可控制元素周圍的空白空間量。 Margin 不會在 ActualHeight 和 ActualWidth 中新增像素，而且也不會基於點擊測試與來源輸入事件的目的而被視為元素的一部分。

設定 [**Padding**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.padding.aspx) 屬性，可控制元素的內部框線及其內容之間的空間量。 正數的 Padding 值會減少元素的內容區域。

此圖表示範如何將邊界及邊框間距套用到元素。

![邊界及邊框間距](images/xaml-layout-margins-padding.png)

適用於 Margin 和 Padding 的左、右、上及下值不需對稱，而且可將它們設為負數值。 如需詳細資訊，請參閱[對齊方式、邊界及邊框間距](alignment-margin-padding.md)，以及 [**Margin**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.margin.aspx) 或 [**Padding**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.padding.aspx) 參考頁面。

讓我們看一下 Margin 和 Padding 在實際控制項上的效果。 以下是 Grid 內部的 TextBox，且預設的 Margin 和 Padding 值為 0。

![邊界及邊框間距為 0 的 TextBox](images/xaml-layout-textbox-no-margins-padding.png)

以下是 TextBox 上含有 Margin 和 Padding 值的相同 TextBox 和 Grid，如這個 XAML 中所示。

```xaml
<Grid BorderBrush="Blue" BorderThickness="4" Width="200">
    <TextBox Text="This is text in a TextBox." Margin="20" Padding="24,16"/>
</Grid>
```

![含有正數邊界及邊框間距值的 TextBox](images/xaml-layout-textbox-with-margins-padding.png)

**可見度**

您可以將元素的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.visibility.aspx) 屬性設定為其中一個 [**Visibility** 列舉](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visibility.aspx)值，藉以顯示或隱藏該元素：**Visible** 或 **Collapsed**。 當元素是 Collapsed 時，它不會佔用 UI 版面配置中的任何空間。

您可以在程式碼或視覺狀態中變更元素的 Visibility 屬性。 當元素的 Visibility 變更時，其所有子元素也會變更。 您可以藉由顯示某一個面板，同時摺疊另一個面板，來取代 UI 的區段。

> **提示**&nbsp;&nbsp;當您在 UI 中具有預設是 **Collapsed** 的元素時，仍會在啟動期間建立物件，即使它們不會顯示也一樣。 您可以延遲載入這些元素，直到藉由將 **x:DeferLoadStrategy attribute** 屬性設為 "Lazy" 來顯示它們為止。 這可以提升啟動效能。 如需詳細資訊，請參閱 [x:DeferLoadStrategy 屬性](../xaml-platform/x-deferloadstrategy-attribute.md)。

### <a name="style-resources"></a>樣式資源

您不需要在控制項上個別設定每個屬性值。 將屬性值群組到 [**Style**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx) 資源中並將 Style 套用到控制項，通常更有效率。 這尤其適用於當您需要將相同屬性值套用到許多控制項的情況。 如需有關使用樣式的詳細資訊，請參閱[設定控制項的樣式](../controls-and-patterns/styling-controls.md)。

### <a name="layout-panels"></a>版面配置面板

大部分的應用程式內容都可以歸類成數種形式的群組或階層。 您使用版面配置面板，在應用程式中群組和排列 UI 元素。 選擇版面配置面板的最重要考量是面板如何放置它的子元素以及調整其大小。 您也需要考慮重疊的子元素如何彼此交疊。

以下是在 XAML 架構中提供的面板控制項的主要功能比較。

面板控制項 | 描述
--------------|------------
[**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx) | **Canvas** 不支援流暢的 UI；您可以完全控制放置子元素及調整其大小的各方面設定。 您通常會在特殊的情況下使用它，例如，建立圖形或定義較大型調適型 UI 的小型靜態區域。 您可以使用程式碼或視覺狀態，在執行階段重新置放元素。<li>元素是使用 Canvas.Top 與 Canvas.Left 附加屬性以絕對位置的方式來放置。</li><li>圖層可以使用 Canvas.ZIndex 附加屬性明確指定。</li><li>適用於 HorizontalAlignment/VerticalAlignment 的 Stretch 值都會遭到忽略。 如果沒有明確設定元素的大小，它就會調整其大小來符合它的內容。</li><li>如果子內容大於面板，就不會以視覺化方式進行剪裁。 </li><li>子內容不會受限於面板的範圍內。</li>
[**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) | **Grid** 支援流暢地調整子元素大小。 您可以使用程式碼或視覺狀態，重新置放和自動重排元素。<li>元素是使用 Grid.Row 與 Grid.Column 附加屬性，以列和欄形式來排列。</li><li>您可以使用 Grid.RowSpan 與 Grid.ColumnSpan 附加屬性，讓元素橫跨多個列與欄。</li><li>系統會採用適用於 HorizontalAlignment/VerticalAlignment 的 Stretch 值。 如果沒有明確設定元素的大小，它會向兩邊延伸以填滿方格儲存格中的可用空間。</li><li>如果子內容大於面板，就會以視覺化方式進行剪裁。</li><li>內容大小受限於面板的範圍，因此可捲動的內容會視需要顯示捲軸。</li>
[**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) | <li>元素是以相較於面板的邊緣或中心，以及彼此相對的關係來排列。</li><li>元素是使用各種不同的附加屬性來放置，這些屬性可控制面板對齊方式、同層級對齊方式及同層級位置。 </li><li>除非用來對齊的 RelativePanel 附加屬性會造成向兩邊延伸 (例如，元素會向面板的左右邊緣對齊)，否則 HorizontalAlignment/VerticalAlignment 的 Stretch 值會遭到忽略。 如果沒有明確設定元素的大小且它不會向兩邊延伸，則它會調整大小來符合其內容。</li><li>如果子內容大於面板，就會以視覺化方式進行剪裁。</li><li>內容大小受限於面板的範圍，因此可捲動的內容會視需要顯示捲軸。</li>
[**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx) |<li>元素以垂直或水平方式堆疊到單行中。</li><li>適用於 HorizontalAlignment/VerticalAlignment 的 Stretch 值會以與 Orientation 屬性相反的方向來採用。 如果沒有明確設定元素的大小，它會向兩邊延伸以填滿可用的寬度 (或高度，如果 Orientation 是 Horizontal)。 利用 Orientation 屬性指定的方向，元素會調整大小來符合其內容。</li><li>如果子內容大於面板，就會以視覺化方式進行剪裁。</li><li>內容大小不會以 Orientation 屬性指定的方向受限於面板的範圍內，因此，可捲動內容向兩邊延伸的範圍會超過面板的範圍且不會顯示捲軸。 您必須明確限制子內容的高度 (或寬度)，讓它能夠顯示捲軸。</li>
[**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx) |<li>在列或欄中排列的元素，達到 MaximumRowsOrColumns 值時會自動換行到新列或新欄。</li><li>Orientation 屬性會指定以列或欄排列元素。</li><li>您可以使用 VariableSizedWrapGrid.RowSpan 與 VariableSizedWrapGrid.ColumnSpan 附加屬性，讓元素橫跨多個列與欄。</li><li>適用於 HorizontalAlignment/VerticalAlignment 的 Stretch 值都會遭到忽略。 元素的大小是由 ItemHeight 與 ItemWidth 屬性所指定。 如果未設定這些屬性，則第一個儲存格中的項目會調整大小以符合其內容，而所有其他的儲存格會繼承這個大小。</li><li>如果子內容大於面板，就會以視覺化方式進行剪裁。</li><li>內容大小受限於面板的範圍，因此可捲動的內容會視需要顯示捲軸。</li>

如需這些面板的詳細資訊和範例，請參閱[版面配置面板](layout-panels.md)。 另請參閱[回應技術範例](http://go.microsoft.com/fwlink/p/?LinkId=620024)。

版面配置面板可讓您將 UI 組織成控制項的邏輯群組。 將它們與適當的屬性設定搭配使用時，您可以取得自動調整大小、重新置放及自動重排 UI 元素的一些支援。 不過，大部分的 UI 版面配置需要在視窗大小有大幅變更時進一步修改。 為此，您可以使用視覺狀態。

## <a name="visual-states-and-state-triggers"></a>視覺狀態與狀態觸發程序

根據畫面大小或其他規格，使用視覺狀態來重新置放、調整大小、自動重排、顯示或取代 UI 區段。 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstate.aspx) 會定義在其處於特殊狀態時要套用到元素的屬性值。 您會在 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.aspx) 中群組視覺狀態，在符合特定條件時套用適當的 VisualState。

### <a name="set-visual-states-in-code"></a>在程式碼中設定視覺狀態

若要從程式碼套用視覺狀態，您可以呼叫 [**VisualStateManager.GoToState**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.gotostate.aspx) 方法。 例如，若要在應用程式視窗為特定大小時套用某個狀態，請處理 [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.window.sizechanged.aspx) 事件並呼叫 **GoToState** 以套用適當的狀態。

此處的 [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstategroup.aspx) 包含 2 個 VisualState 定義。 第一個是 `DefaultState`，是空的。 套用時，即會套用 XAML 頁面中定義的值。 第二個是 `WideState`，它會將 [**SplitView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.aspx) 的 [**DisplayMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.displaymode.aspx) 屬性變更為 **Inline** 並開啟窗格。 如果視窗寬度為 720 個有效像素或更大，即會在 SizeChanged 事件處理常式中套用此狀態。

```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="DefaultState">
                        <Storyboard>
                        </Storyboard>
                    </VisualState>

                <VisualState x:Name="WideState">
                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.DisplayMode"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0">
                                <DiscreteObjectKeyFrame.Value>
                                    <SplitViewDisplayMode>Inline</SplitViewDisplayMode>
                                </DiscreteObjectKeyFrame.Value>
                            </DiscreteObjectKeyFrame>
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.IsPaneOpen"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

```csharp
private void CurrentWindow_SizeChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
{
    if (e.Size.Width >= 720)
        VisualStateManager.GoToState(this, "WideState", false);
    else
        VisualStateManager.GoToState(this, "DefaultState", false);
}
```

### <a name="set-visual-states-in-xaml-markup"></a>在 XAML 標記中設定視覺狀態

在 Windows 10 之前，VisualState 定義需要 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.storyboard.aspx) 物件來進行屬性變更，而且您必須在程式碼中呼叫 **GoToState** 來套用狀態。 如同先前範例所示。 您仍然會看到許多範例使用此語法，或者您現在可能具有會用到它的程式碼。

從 Windows 10 開始，您可以使用此處所示的簡化 [**Setter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.setter.aspx) 語法，而且可以在 XAML 標記中使用 [**StateTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.statetrigger.aspx) 來套用狀態。 您使用狀態觸發程序來建立簡單的規則，這些規則會自動觸發視覺狀態變更以回應應用程式事件。

這個範例與上一個範例相同，但會使用簡化的 **Setter** 語法 (而不是 Storyboard) 來定義屬性變更。 此外，它不會呼叫 GoToState，而是改用內建的 [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx) 狀態觸發程序來套用狀態。 當您使用狀態觸發程序時，不需要定義空的 `DefaultState`。 如果不再符合狀態觸發程序的條件，即會自動重新套用預設設定。

```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <!-- VisualState to be triggered when the
                             window width is >=720 effective pixels. -->
                        <AdaptiveTrigger MinWindowWidth="720" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="mySplitView.DisplayMode" Value="Inline"/>
                        <Setter Target="mySplitView.IsPaneOpen" Value="True"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

> **重要**&nbsp;&nbsp;在先前範例中，已在 **Grid** 元素上設定 VisualStateManager.VisualStateGroups 附加屬性。 使用 StateTrigger 時，請一律確保會將 VisualStateGroups 附加到根目錄的第一個子項，讓觸發程序能夠自動生效 (此處的 **Grid** 是根 **Page** 元素的第一個子項)。

### <a name="attached-property-syntax"></a>附加屬性語法

在 VisualState 中，您通常會設定控制項屬性的值，或者為包含該控制項之面板的其中一個附加屬性設定值。 設定附加屬性時，請使用括號將附加屬性名稱括起來。

這個範例示範如何在名為 `myTextBox` 的 TextBox 上，設定 [**RelativePanel.AlignHorizontalCenterWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanel.aspx) 附加屬性。 第一個 XAML 會使用 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.objectanimationusingkeyframes.aspx) 語法，而第二個會使用 **Setter** 語法。

```xaml
<!-- Set an attached property using ObjectAnimationUsingKeyFrames. -->
<ObjectAnimationUsingKeyFrames
    Storyboard.TargetProperty="(RelativePanel.AlignHorizontalCenterWithPanel)"
    Storyboard.TargetName="myTextBox">
    <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
</ObjectAnimationUsingKeyFrames>

<!-- Set an attached property using Setter. -->
<Setter Target="myTextBox.(RelativePanel.AlignHorizontalCenterWithPanel)" Value="True"/>
```

### <a name="custom-state-triggers"></a>自訂狀態觸發程序

您可以擴充 [**StateTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.statetrigger.aspx) 類別，針對各種案例建立自訂觸發程序。 例如，可以建立 StateTrigger，根據輸入類型來觸發不同狀態，然後在輸入類型為觸控時，增加控制項四周的邊界。 或是建立 StateTrigger，以根據應用程式執行所在的裝置系列來套用不同的狀態。 如需如何建置自訂觸發程序並使用它們從單一 XAML 檢視中建立最佳化 UI 體驗的範例，請參閱[狀態觸發程序範例](http://go.microsoft.com/fwlink/p/?LinkId=620025)。

### <a name="visual-states-and-styles"></a>視覺狀態與樣式

您可以在視覺狀態中使用 Style 資源，將一組屬性變更套用到多個控制項。 如需有關使用樣式的詳細資訊，請參閱[設定控制項的樣式](../controls-and-patterns/styling-controls.md)。

在這個來自狀態觸發程序範例的簡化 XAML 中，Style 資源被套用到 Button，以調整滑鼠或觸控輸入的大小和邊界。 如需自訂狀態觸發程序的完整程式碼和定義，請參閱[狀態觸發程序範例](http://go.microsoft.com/fwlink/p/?LinkId=620025)。

```xaml
<Page ... >
    <Page.Resources>
        <!-- Styles to be used for mouse vs. touch/pen hit targets -->
        <Style x:Key="MouseStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="5" />
            <Setter Property="Height" Value="20" />
            <Setter Property="Width" Value="20" />
        </Style>
        <Style x:Key="TouchPenStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="15" />
            <Setter Property="Height" Value="40" />
            <Setter Property="Width" Value="40" />
        </Style>
    </Page.Resources>

    <RelativePanel>
        <!-- ... -->
        <Button Content="Color Palette Button" x:Name="MenuButton">
            <Button.Flyout>
                <Flyout Placement="Bottom">
                    <RelativePanel>
                        <Rectangle Name="BlueRect" Fill="Blue"/>
                        <Rectangle Name="GreenRect" Fill="Green" RelativePanel.RightOf="BlueRect" />
                        <!-- ... -->
                    </RelativePanel>
                </Flyout>
            </Button.Flyout>
        </Button>
        <!-- ... -->
    </RelativePanel>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="InputTypeStates">
            <!-- Second set of VisualStates for building responsive UI optimized for input type.
                 Take a look at InputTypeTrigger.cs class in CustomTriggers folder to see how this is implemented. -->
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- This trigger indicates that this VisualState is to be applied when MenuButton is invoked using a mouse. -->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Mouse" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource MouseStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource MouseStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- Multiple trigger statements can be declared in the following way to imply OR usage.
                         For example, the following statements indicate that this VisualState is to be applied when MenuButton is invoked using Touch OR Pen.-->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Touch" />
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Pen" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Page>
```

## <a name="tailored-layouts"></a>量身訂做的版面配置

當您大幅變更不同裝置上的 UI 版面配置時，會發現更簡便的方式是使用為該裝置量身訂做的版面配置來定義個別 UI，而不是調整單一 UI。 如果功能在各個裝置上都一樣，您就可以定義個別的 XAML 檢視來共用同一個程式碼檔案。 如果檢視和功能在不同裝置上有顯著的差異，則可定義個別的 Page，然後選擇要在應用程式載入時瀏覽到哪一個 Page。

### <a name="separate-xaml-views-per-device-family"></a>每個裝置系列都有不同的 XAML 檢視

使用 XAML 檢視，建立不同的 UI 定義來共用相同的程式碼後置。 您可以針對每個裝置系列提供獨特的 UI 定義。 請依照下列步驟來將 XAML 檢視新增到應用程式。

**將 XAML 檢視新增到應用程式**
1. 依序選取 [專案] &gt; [加入新項目]。 隨即開啟 [加入新項目] 對話方塊。
    > **提示**&nbsp;&nbsp;確定在 [方案總管] 中選取的是資料夾或專案，而不是方案。
2. 在左窗格的 [Visual C#] 或 [Visual Basic] 下方，挑選 [XAML] 範本類型。
3. 在中央窗格，挑選 [XAML 檢視]。
4. 輸入檢視的名稱。 檢視必須以正確方式命名。 如需命名的詳細資訊，請參閱本節的其餘部分。
5. 按一下 [新增]。 檔案即會新增到專案。

先前步驟只會建立一個 XAML 檔案，但不會建立相關聯的程式碼後置檔案。 而是會使用 DeviceName 限定詞 (此為檔案或資料夾名稱的一部分)，將 XAML 檢視關聯至現有的程式碼後置檔案。 這個限定詞名稱可對應到代表應用程式目前執行所在之裝置的裝置系列的字串值，例如，"Desktop"、"Mobile" 和其他裝置系列的名稱 (請參閱 [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues.aspx))。

您可以將限定詞新增到檔案名稱，或者將檔案新增到具有限定詞名稱的資料夾。

**使用檔案名稱**

若要使用限定詞名稱搭配檔案，請使用下列格式：*[pageName]*.DeviceFamily-*[qualifierString]*.xaml。

讓我們看一個名為 MainPage.xaml 的檔案範例。 若要建立適用於行動裝置的檢視，請將 XAML 檢視命名為 MainPage.DeviceFamily-Mobile.xaml。 若要建立適用於電腦裝置的檢視，請將檢視命名為 MainPage.DeviceFamily-Desktop.xaml。 以下是方案在 Microsoft Visual Studio 中看起來的樣子。

![含有完整檔案名稱的 XAML 檢視](images/xaml-layout-view-ex-1.png)

**使用資料夾名稱**

若要在 Visual Studio 專案中使用資料夾組織檢視，您可以使用限定詞名稱搭配資料夾。 若要這樣做，請使用下列格式來為資料夾命名：DeviceFamily-*[qualifierString]*。 在此案例中，每個 XAML 檢視檔案都具有相同名稱。 請勿在檔案名稱中包含限定詞。

以下提供一個範例，再次提醒，這適用於名為 MainPage.xaml 的檔案。 若要建立適用於行動裝置的檢視，請建立名為 DeviceFamily-Mobile 的資料夾，並將名為 MainPage.xaml 的 XAML 檢視放置到其中。 若要建立適用於電腦裝置的檢視，請建立名為 "DeviceFamily-Desktop" 的資料夾，並將另一個名為 MainPage.xaml 的 XAML 檢視放置到其中。 以下是方案在 Visual Studio 中看起來的樣子。

![資料夾中的 XAML 檢視](images/xaml-layout-view-ex-2.png)

在這兩個案例中，有一個獨特的檢視適用於行動裝置和電腦裝置。 如果其執行所在的裝置不符合任何裝置系列特定的檢視，即會使用預設的 MainPage.xaml 檔案。

### <a name="separate-xaml-pages-per-device-family"></a>每個裝置系列都有不同的 XAML 頁面

若要提供獨特的檢視和功能，您可以建立個別的 Page 檔案 (XAML 和程式碼)，然後在需要某個頁面時瀏覽到適當的頁面。

**將 XAML 頁面新增到應用程式**
1. 依序選取 [專案] &gt; [加入新項目]。 隨即開啟 [加入新項目] 對話方塊。
    > **提示**&nbsp;&nbsp;確定在 [方案總管] 選取的是專案而不是方案。
2. 在左窗格的 [Visual C#] 或 [Visual Basic] 下方，挑選 [XAML] 範本類型。
3. 在中央窗格，選取 [空白頁]。
4. 輸入頁面的名稱。 例如，"MainPage_Mobile"。 同時建立 MainPage_Mobile.xaml 和 MainPage_Mobile.xaml.cs/vb/cpp 程式碼檔案。
5. 按一下 [新增]。 檔案即會新增到專案。

在執行階段，檢查應用程式執行所在的裝置系列，然後瀏覽到正確的頁面，如下。

```csharp
if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
{
    rootFrame.Navigate(typeof(MainPage_Mobile), e.Arguments);
}
else
{
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
}
```

您也可以使用不同的準則來判斷要瀏覽到哪一個頁面。 如需更多範例，請參閱[量身打造的多個檢視](http://go.microsoft.com/fwlink/p/?LinkId=620636)範例，它會使用 [**GetIntegratedDisplaySize**](https://msdn.microsoft.com/library/windows/apps/xaml/dn904185.aspx) 函式來檢查整合式顯示器的實體大小。

## <a name="sample-code"></a>範例程式碼
*   [XAML UI 基本知識範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    以互動式格式查看所有 XAML 控制項。