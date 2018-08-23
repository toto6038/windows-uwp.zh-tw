---
author: muhsinking
Description: XAML gives you a flexible layout system to create a responsive UI.
title: 搭配 XAML 的回應式版面配置
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0b45196a83edf45a69f6b79ab82542cef6817703
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/23/2018
ms.locfileid: "2820089"
---
# <a name="responsive-layouts-with-xaml"></a>搭配 XAML 的回應式版面配置

XAML 版面配置系統提供自動調整大小、版面配置面板、視覺狀態，甚至將 UI 定義分開來建立回應式 UI。 有了回應式版面配置，您就可以讓應用程式在不同應用程式視窗尺寸、解析度、像素密度及方向的螢幕上看起來更美觀。 您也可以使用 XAML 調整位置、調整大小、顯示/隱藏、取代，或重新設計您應用程式的 UI，如同 [回應式設計技術](responsive-design.md)中討論過的。 以下，我們討論如何實作搭配有 XAML 的回應式版面配置。

## <a name="fluid-layouts-with-properties-and-panels"></a>使用屬性與面板的流暢版面配置

回應式版面配置的基礎在於適當地使用 XAML 版面配置屬性和面板，在流暢方式中進行內容的重新置放、調整大小及自動重排。 

XAML 版面配置系統支援靜態與流暢版面配置。 在靜態配置中，您提供控制項明確的像素大小與位置。 當使用者變更裝置的解析度或方向時，UI 不會變更。 靜態版面配置在不同的硬體規格、畫面大小中會遭到裁剪。 相反的，流暢的版面配置可縮小、放大和自動重排，以回應裝置上的可用視覺空間。 

實際上，可以使用靜態與流暢元素的組合來建立 UI。 您仍然會在某些地方使用靜態元素與值，但請確定整體 UI 是否回應不同的解析度、螢幕大小及檢視。

我們將在此處討論如何使用 XAML 屬性和版面配置面板，建立流暢版面配置。

### <a name="layout-properties"></a>版面配置屬性
版面配置屬性控制元素的大小與位置。 若要建立流暢版面配置，請針對元素使用自動或等比例調整大小，並視需要允許版面配置面板放置其子系。 

以下是一些常見的版面配置屬性，以及如何使用它們來建立流暢版面配置。

**高度和寬度**

[**Height**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) 和 [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) 屬性指定元素的大小。 您可以使用以有效像素衡量的固定值，或者可以使用自動或等比例調整大小。 

自動調整大小，調整 UI 元素的大小以符合它們的內容或父容器。 您也可以使用自動調整大小搭配方格的列與欄。 若要使用自動調整大小，請將 UI 元素的 Height 和/或 Width 設定為 **Auto**。

> [!NOTE]
> 元素是否會調整大小以符合其內容或容器，取決於父容器如何處理調整其子項的大小。 如需詳細資訊，請參閱本文後續內容中的 [版面配置面板](#layout-panels)。

等比例調整大小 (亦稱為 *「星號調整」*)，按照權重比例，將可用的空間分配給方格的列和欄。 在 XAML 中，星號值的表示方法為 \* (加權星號調整則為 *n*\*)。 例如，若要在 2 欄的版面配置中，將某一欄的寬度設定為第二欄的 5 倍，請使用 "5\*" 和 "\*" 來表示 [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.aspx) 元素中的 [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.width.aspx) 屬性。

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

若要在執行階段取得元素的大小，請使用唯讀 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.actualheight.aspx) 和 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.actualwidth.aspx) 屬性，而不是 Height 和 Width。

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

**可見度**

您可以將元素的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.visibility.aspx) 屬性設定為其中一個 [**Visibility** 列舉](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visibility.aspx)值，藉以顯示或隱藏該元素：**Visible** 或 **Collapsed**。 當元素是 Collapsed 時，它不會佔用 UI 版面配置中的任何空間。

您可以在程式碼或視覺狀態中變更元素的 Visibility 屬性。 當元素的 Visibility 變更時，其所有子元素也會變更。 您可以藉由顯示某一個面板，同時摺疊另一個面板，來取代 UI 的區段。

> [!Tip]
> 當您在 UI 中的預設**摺疊**的項目時，物件仍會在建立啟動，即使其不可見。 您可以延遲載入這些元素，直到藉由將 **x:DeferLoadStrategy attribute** 屬性設為 "Lazy" 來顯示它們為止。 這可以提升啟動效能。 如需詳細資訊，請參閱 [x:DeferLoadStrategy 屬性](../../xaml-platform/x-deferloadstrategy-attribute.md)。

### <a name="style-resources"></a>樣式資源

您不需要在控制項上個別設定每個屬性值。 將屬性值群組到 [**Style**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx) 資源中並將 Style 套用到控制項，通常更有效率。 這尤其適用於當您需要將相同屬性值套用到許多控制項的情況。 如需有關使用樣式的詳細資訊，請參閱[設定控制項的樣式](../controls-and-patterns/xaml-styles.md)。

### <a name="layout-panels"></a>版面配置面板

若要定位視覺物件，您必須將它們放在面板或其他容器物件中。 XAML 架構提供各種 Panel 類別 (例如 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx)、[**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)、[**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) 和 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx)) 做為容器，您可以在其中放置和排列 UI 元素。

選擇版面配置面板的最重要考量是面板如何放置它的子元素以及調整其大小。 您也需要考慮重疊的子元素如何彼此交疊。

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

## <a name="adaptive-layouts-with-visual-states-and-state-triggers"></a>使用視覺狀態和狀態觸發程序的彈性版面配置
使用視覺狀態，根據視窗大小或其他變更，大幅更改您的 UI。

當應用程式視窗放大或縮小的範圍超過一定數量時，您可能想要更改版面配置屬性，以重新置放、調整大小、自動重排或取代 UI 的區段。 您可以為 UI 定義不同的視覺狀態，並在視窗寬度或視窗長度超出指定閾值時套用它們。 

[**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx) 提供一種簡單的方法來設定套用狀態的閾值 (也稱為中斷點)。 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstate.aspx) 會定義在其處於特殊狀態時要套用到元素的屬性值。 您會在 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.aspx) 中群組視覺狀態，在符合特定條件時套用適當的 VisualState。

### <a name="set-visual-states-in-code"></a>在程式碼中設定視覺狀態

若要從程式碼套用視覺狀態，您可以呼叫 [**VisualStateManager.GoToState**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.gotostate.aspx) 方法。 例如，若要在應用程式視窗為特定大小時套用某個狀態，請處理 [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.window.sizechanged.aspx) 事件並呼叫 **GoToState** 以套用適當的狀態。

此處的 [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstategroup.aspx) 包含二個 VisualState 定義。 第一個是 `DefaultState`，是空的。 套用時，即會套用 XAML 頁面中定義的值。 第二個是 `WideState`，它會將 [**SplitView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.aspx) 的 [**DisplayMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.displaymode.aspx) 屬性變更為 **Inline** 並開啟窗格。 如果視窗寬度比 640 個有效像素來得大，即會在 SizeChanged 事件處理常式中套用此狀態。

> [!NOTE]
> Windows 不會針對您的應用程式提供一個偵測執行您應用程式之特定裝置的方法。 它能夠告知您執行應用程式的裝置系列 (行動、桌面等)、實際解析度，以及應用程式可用的螢幕空間量 (應用程式的視窗大小)。 我們建議為 [螢幕大小與中斷點](screen-sizes-and-breakpoints-for-responsive-design.md)定義視覺狀態。


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
    if (e.Size.Width > 640)
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
                        <AdaptiveTrigger MinWindowWidth="640" />
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

> [!Important]
> 在上述範例中，VisualStateManager.VisualStateGroups 附加屬性設在**格線**元素。 使用 StateTrigger 時，請一律確保會將 VisualStateGroups 附加到根目錄的第一個子項，讓觸發程序能夠自動生效 (此處的 **Grid** 是根 **Page** 元素的第一個子項)。

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

您可以在視覺狀態中使用 Style 資源，將一組屬性變更套用到多個控制項。 如需有關使用樣式的詳細資訊，請參閱[設定控制項的樣式](../controls-and-patterns/xaml-styles.md)。

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

先前步驟只會建立一個 XAML 檔案，但不會建立相關聯的程式碼後置檔案。 而是會使用 DeviceName 限定詞 (此為檔案或資料夾名稱的一部分)，將 XAML 檢視關聯至現有的程式碼後置檔案。 這個限定詞名稱可對應到代表應用程式目前執行所在之裝置的裝置系列的字串值，例如，「電腦」、「平板電腦」和其他裝置系列的名稱 (請參閱 [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues.aspx))。

您可以將限定詞新增到檔案名稱，或者將檔案新增到具有限定詞名稱的資料夾。

**使用檔案名稱**

若要使用限定詞名稱搭配檔案，請使用下列格式：*[pageName]*.DeviceFamily-*[qualifierString]*.xaml。

讓我們看一個名為 MainPage.xaml 的檔案範例。 若要建立適用於平板電腦裝置的檢視，請將 XAML 檢視命名為 MainPage.DeviceFamily-Tablet.xaml。 若要建立適用於電腦裝置的檢視，請將檢視命名為 MainPage.DeviceFamily-Desktop.xaml。 以下是方案在 Microsoft Visual Studio 中看起來的樣子。

![含有完整檔案名稱的 XAML 檢視](images/xaml-layout-view-ex-1.png)

**使用資料夾名稱**

若要在 Visual Studio 專案中使用資料夾組織檢視，您可以使用限定詞名稱搭配資料夾。 若要這樣做，請使用下列格式來為資料夾命名：DeviceFamily-*[qualifierString]*。 在此案例中，每個 XAML 檢視檔案都具有相同名稱。 請勿在檔案名稱中包含限定詞。

以下提供一個範例，再次提醒，這適用於名為 MainPage.xaml 的檔案。 若要建立適用於平板電腦裝置的檢視，請建立名為 DeviceFamily-Tablet 的資料夾，並將名為 MainPage.xaml 的 XAML 檢視放置到其中。 若要建立適用於電腦裝置的檢視，請建立名為 "DeviceFamily-Desktop" 的資料夾，並將另一個名為 MainPage.xaml 的 XAML 檢視放置到其中。 以下是方案在 Visual Studio 中看起來的樣子。

![資料夾中的 XAML 檢視](images/xaml-layout-view-ex-2.png)

在這兩個案例中，有一個獨特的檢視適用於平板電腦裝置和電腦裝置。 如果其執行所在的裝置不符合任何裝置系列特定的檢視，即會使用預設的 MainPage.xaml 檔案。

### <a name="separate-xaml-pages-per-device-family"></a>每個裝置系列都有不同的 XAML 頁面

若要提供獨特的檢視和功能，您可以建立個別的 Page 檔案 (XAML 和程式碼)，然後在需要某個頁面時瀏覽到適當的頁面。

**將 XAML 頁面新增到應用程式**
1. 依序選取 [專案] &gt; [加入新項目]。 隨即開啟 [加入新項目] 對話方塊。
    > **提示**&nbsp;&nbsp;確定在 [方案總管] 選取的是專案而不是方案。
2. 在左窗格的 [Visual C#] 或 [Visual Basic] 下方，挑選 [XAML] 範本類型。
3. 在中央窗格，選取 [空白頁]。
4. 輸入頁面的名稱。 例如，"MainPage_Tablet"。 同時建立 MainPage_Tablet.xaml 和 MainPage_Tablet.xaml.cs/vb/cpp 程式碼檔案。
5. 按一下新增。 檔案即會新增到專案。

在執行階段，檢查應用程式執行所在的裝置系列，然後瀏覽到正確的頁面，如下。

```csharp
if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Tablet")
{
    rootFrame.Navigate(typeof(MainPage_Tablet), e.Arguments);
}
else
{
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
}
```

您也可以使用不同的準則來判斷要瀏覽到哪一個頁面。 如需更多範例，請參閱[量身打造的多個檢視](http://go.microsoft.com/fwlink/p/?LinkId=620636)範例，它會使用 [**GetIntegratedDisplaySize**](https://msdn.microsoft.com/library/windows/apps/xaml/dn904185.aspx) 函式來檢查整合式顯示器的實體大小。

## <a name="related-topics"></a>相關主題
- [教學課程：建立調適型配置](../basics/xaml-basics-adaptive-layout.md)
- [回應技術範例 (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlResponsiveTechniques)
- [狀態觸發程序範例 (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlStateTriggers)
- [量身打造的多個檢視 (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlTailoredMultipleViews)
