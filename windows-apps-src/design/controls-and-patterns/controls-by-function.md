---
Description: 提供可用於 app 的部分控制項清單 (依功能分類)。
title: 依功能分類的控制項
ms.assetid: 8DB4347B-91D6-4659-91F2-80ECF7BBB596
label: Controls by function
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8f1717a59399fb95f7b71a38ee8d2d46de4ca765
ms.sourcegitcommit: e11e0f65930665579d1f296861234893e82bf8fb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80301438"
---
# <a name="controls-by-function"></a>依功能分類的控制項

Windows 的 XAML UI 架構提供一個支援 UI 開發的龐大控制項程式庫。 部分控制項以視覺方式呈現；其餘控制項則當做其他控制項或內容 (例如影像與媒體) 的容器。 

您可以下載 [XAML UI 基本知識範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)，以查看許多 Windows UI 控制項。

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/NavigationView">開啟應用程式並查看 NavigationView 運作情形</a> 。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>


以下是可用於 App 的常用 XAML 控制項清單 (依功能分類)。

## <a name="appbars-and-commands"></a>應用程式列與命令

### <a name="app-bar"></a>應用程式列
顯示應用程式特定命令的工具列。 請參閱＜命令列＞。

參考：[AppBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) 

### <a name="app-bar-button"></a>應用程式列按鈕
使用應用程式列樣式顯示命令的按鈕。

![應用程式列按鈕圖示](images/controls/app-bar-buttons.png) 

參考：[AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)、[SymbolIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SymbolIcon)、[BitmapIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.BitmapIcon)、[FontIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FontIcon)、[PathIcon](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PathIcon) 

設計和作法︰[應用程式列和命令列控制項指南](app-bars.md) 

範例程式碼：[XAML 命令範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

### <a name="app-bar-separator"></a>應用程式列分隔符號
在視覺上分隔命令列中的命令群組。

參考：[AppBarSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator) 

範例程式碼：[XAML 命令範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

### <a name="app-bar-toggle-button"></a>應用程式列切換按鈕
用於在命令列中切換命令的按鈕。

參考：[AppBarToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) 

範例程式碼：[XAML 命令範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

### <a name="command-bar"></a>命令列
一個處理應用程式列按鈕元素大小調整的特殊化應用程式列。

![命令列控制項](images/command-bar-compact.png)

```xaml
<CommandBar>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
</CommandBar>
```
參考：[CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 

設計和作法︰[應用程式列和命令列控制項指南](app-bars.md)

範例程式碼：[XAML 命令範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

## <a name="buttons"></a>按鈕

### <a name="button"></a>按鈕
回應使用者輸入並引發 **Click** 事件的控制項。

![標準按鈕](images/controls/button.png)

```xaml
<Button x:Name="button1" Content="Button" 
        Click="Button_Click" />
```

參考：[Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) 

設計和作法︰[按鈕控制項指南](buttons.md) 

### <a name="hyperlink"></a>Hyperlink
請參閱＜超連結按鈕＞。

### <a name="hyperlink-button"></a>超連結按鈕
顯示為標記文字並且會在瀏覽器中開啟指定 URI 的按鈕。

![超連結按鈕](images/controls/hyperlink-button.png)

```xaml
<HyperlinkButton Content="www.microsoft.com" 
                 NavigateUri="https://www.microsoft.com"/>
```

參考：[HyperlinkButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) 

設計和作法︰[超連結控制項指南](hyperlinks.md)

### <a name="repeat-button"></a>重複按鈕
一個按鈕，從按下到放開的這段期間，會重複引發 **Click** 事件。 

![重複按鈕控制項](images/controls/repeat-button.png) 

```xaml
<RepeatButton x:Name="repeatButton1" Content="Repeat Button" 
              Click="RepeatButton_Click" />
```

參考：[RepeatButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.RepeatButton) 

設計和作法︰[按鈕控制項指南](buttons.md) 

## <a name="collectiondata-controls"></a>集合/資料控制項

### <a name="flip-view"></a>翻轉檢視
讓使用者可以逐一瀏覽項目集合 (一次瀏覽一個項目) 的控制項。

```xaml
<FlipView x:Name="flipView1" SelectionChanged="FlipView_SelectionChanged">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

參考：[FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) 

設計和作法︰[翻轉檢視控制項指南](flipview.md) 

### <a name="grid-view"></a>格線檢視
在可以垂直捲動的列和欄中顯示項目集合的控制項。

```xaml
<GridView x:Name="gridView1" SelectionChanged="GridView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</GridView>
```

參考：[GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 

設計和作法︰[清單](lists.md) 

範例程式碼：[ListView 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)

### <a name="items-control"></a>項目控制項
在資料範本指定的 UI 中顯示項目集合的控制項。 

```xaml
<ItemsControl/>
```

參考：[ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) 

### <a name="list-view"></a>清單檢視
在可以垂直捲動的清單中顯示項目集合的控制項。

```xaml
<ListView x:Name="listView1" SelectionChanged="ListView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</ListView>
```

參考：[ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 

設計和作法︰[清單](lists.md) 

範例程式碼：[ListView 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)

## <a name="date-and-time-controls"></a>日期和時間控制項

### <a name="calendar-date-picker"></a>行事曆日期選擇器
可讓使用者使用下拉式行事曆顯示畫面選取日期的控制項。

![已開啟行事曆檢視的行事曆日期選擇器](images/controls/calendar-date-picker-open.png)

```xaml
<CalendarDatePicker/>
```

參考：[CalendarDatePicker](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker) 

設計和作法︰[行事曆、日期和時間控制項](date-and-time.md)
 
### <a name="calendar-view"></a>行事曆檢視
可讓使用者選取單一或多個日期的可設定式行事曆顯示畫面。

```xaml
<CalendarView/>
```

參考：[CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 

設計和作法︰[行事曆、日期和時間控制項](date-and-time.md) 

### <a name="date-picker"></a>日期選擇器
讓使用者能夠選取日期的控制項。

![日期選擇器控制項](images/controls/date-picker.png)

```xaml
<DatePicker Header="Arrival Date"/>
```

參考：[DatePicker](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker) 

設計和作法︰[行事曆、日期和時間控制項](date-and-time.md)
 
### <a name="time-picker"></a>時間選擇器
讓使用者能夠設定時間值的控制項。

![TimePicker 控制項](images/controls/time-picker.png) 

```xaml
<TimePicker Header="Arrival Time"/>
```

參考：[TimePicker](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) 

設計和作法︰[行事曆、日期和時間控制項](date-and-time.md)

## <a name="flyouts"></a>飛出視窗

### <a name="context-menu"></a>操作功能表
請參閱＜功能表飛出視窗＞和＜快顯功能表＞。

### <a name="flyout"></a>飛出視窗
顯示一則要求使用者互動的訊息。 (與對話方塊不同的是，飛出視窗不會建立另一個視窗，也不會封鎖其他使用者互動)。

![飛出視窗控制項](images/controls/flyout.png)

```xaml
<Flyout>
    <StackPanel>
        <TextBlock Text="All items will be removed. Do you want to continue?"/>
        <Button Click="DeleteConfirmation_Click" Content="Yes, empty my cart"/>
    </StackPanel>
</Flyout>
```

參考：[飛出視窗](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout) 

設計和作法︰[飛出視窗](dialogs-and-flyouts/flyouts.md) 

### <a name="menu-flyout"></a>功能表飛出視窗
暫時顯示與使用者目前正在執行之動作相關的命令或選項清單。

![功能表飛出視窗控制項](images/controls/menu-flyout.png) 

```xaml
<MenuFlyout>
    <MenuFlyoutItem Text="Reset" Click="Reset_Click"/>
    <MenuFlyoutSeparator/>
    <ToggleMenuFlyoutItem Text="Shuffle" 
                          IsChecked="{Binding IsShuffleEnabled, Mode=TwoWay}"/>
    <ToggleMenuFlyoutItem Text="Repeat" 
                          IsChecked="{Binding IsRepeatEnabled, Mode=TwoWay}"/>
</MenuFlyout>
```

參考：[MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout)、[MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem)、[MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutSeparator)、[ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleMenuFlyoutItem) 

設計和作法︰[功能表和操作功能表](menus.md) 

範例程式碼：[XAML 操作功能表範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

### <a name="popup-menu"></a>快顯功能表
顯示您所指定命令的自訂功能表。

參考：[PopupMenu](https://docs.microsoft.com/uwp/api/Windows.UI.Popups.PopupMenu) 

設計和作法︰[對話方塊](dialogs-and-flyouts/dialogs.md) 

### <a name="tooltip"></a>工具提示
顯示元素資訊的快顯視窗。 
 
![工具提示控制項](images/controls/tool-tip.png)

```xaml
<Button Content="Button" Click="Button_Click" 
        ToolTipService.ToolTip="Click to perform action" />
```

參考：[ToolTip](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip)、[ToolTipService](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTipService) 

設計和作法︰工具提示的指導方針 

## <a name="images"></a>映像

### <a name="image"></a>影像
顯示影像的控制項。

```xaml
<Image Source="Assets/Logo.png" />
```

參考：[影像](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 

設計和作法︰[Image 和 ImageBrush](images-imagebrushes.md) 

範例程式碼：[XAML 影像範例](https://code.msdn.microsoft.com/windowsapps/0f5d56ae-5e57-48e1-9cd9-993115b027b9)

## <a name="graphics-and-ink"></a>圖形與筆墨

### <a name="inkcanvas"></a>InkCanvas
接收及顯示筆墨筆觸的控制項。

```xaml
<InkCanvas/>
```

參考：[InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 

### <a name="shapes"></a>形狀
各種保留模式圖形物件，可以使用橢圓形、長方形、直線、貝茲路徑之類的物件顯示。

![多邊形](images/controls/shapes-polygon.png) 
![路徑](images/controls/shapes-path.png) 

```xaml
<Ellipse/>
<Path/>
<Rectangle/>
```

參考：[形狀](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Shapes.Shape) 

作法：[繪製形狀](../../graphics/drawing-shapes.md) 

範例程式碼：[XAML 向量繪製範例](https://code.msdn.microsoft.com/windowsapps/Drawing-bfc39296)

## <a name="layout-controls"></a>配置控制項

### <a name="border"></a>框線
在另一個物件周圍繪製框線、背景或兩者皆繪製的容器控制項。

![兩個矩形的框線](images/controls/border.png) 

```xaml
<Border BorderBrush="Blue" BorderThickness="4" 
        Height="108" Width="64" 
        Padding="8" CornerRadius="4">
    <Canvas>
        <Rectangle Fill="Orange"/>
        <Rectangle Fill="Green" Margin="0,44"/>
    </Canvas>
</Border>
```

參考：[Border](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border)

### <a name="canvas"></a>畫布
支援將子元素以畫布左上角為起點進行絕對定位的配置面板。
 
![畫布配置面板](images/controls/canvas.png) 

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

參考：[Canvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas)
 
### <a name="grid"></a>方格
支援以列和欄排列子元素的配置面板。

![格線配置面板](images/controls/grid.png) 

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="50"/>
        <RowDefinition Height="50"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="50"/>
        <ColumnDefinition Width="50"/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```

參考：[Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)
 
### <a name="panning-scroll-viewer"></a>移動瀏覽捲動檢視器
請參閱＜捲動檢視器＞。

### <a name="relativepanel"></a>RelativePanel
一個面板，可讓您定位及排列彼此有關係或與上層面板有關係的子物件。

![相對面板配置面板](images/controls/relative-panel.png) 

```xaml
<RelativePanel>
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

參考：[RelativePanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel)

### <a name="scroll-bar"></a>捲軸
請參閱＜捲動檢視器＞。 (ScrollBar 是 ScrollViewer 的元素。 您通常不會將它做為獨立控制項)。

參考：[ScrollBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ScrollBar)
 
### <a name="scroll-viewer"></a>捲動檢視器
讓使用者移動瀏覽和縮放內容的容器控制項。

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10" 
              HorizontalScrollMode="Enabled" 
              HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

參考：[ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)

設計和作法︰[捲動和移動瀏覽控制項指南](scroll-controls.md) 

範例程式碼：[XAML 捲動、移動瀏覽和縮放範例](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)

### <a name="stack-panel"></a>堆疊面板
可以將子元素按水平或垂直方向排列到單行中的配置面板。

![堆疊面板配置控制項](images/controls/stack-panel.png) 

```xaml
<StackPanel>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue"/>
    <Rectangle Fill="Green"/>
    <Rectangle Fill="Orange"/>
</StackPanel>
```

參考：[StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel)
 
### <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid
支援以列和欄排列子元素的配置面板。 每個子元素可以橫跨多列和多欄。

![不同大小換行格線配置面板](images/controls/variable-sized-wrap-grid.png) 

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Height="80" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" Width="80" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" Height="80" Width="80" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```

參考：[VariableSizedWrapGrid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid)

### <a name="viewbox"></a>Viewbox
將內容縮放為指定大小的容器控制項。

![Viewbox 控制項](images/controls/view-box.png) 

```xaml
<Viewbox MaxWidth="25" MaxHeight="25">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="75" MaxHeight="75">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="150" MaxHeight="150">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
```

參考：[Viewbox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Viewbox)
 
### <a name="zooming-scroll-viewer"></a>縮放捲動檢視器
請參閱＜捲動檢視器＞。

## <a name="media-controls"></a>媒體控制項

### <a name="audio"></a>音訊
請參閱＜媒體元素＞。

### <a name="media-element"></a>媒體元素
播放音訊和視訊內容的控制項。

```xaml
<MediaElement x:Name="myMediaElement"/>
```

參考：[MediaElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 

設計和作法︰[媒體元素控制項指南](media-playback.md)

### <a name="mediatransportcontrols"></a>MediaTransportControls
為 MediaElement 提供播放控制項的控制項。

![具有傳輸控制項的媒體元素](images/controls/media-transport-controls.png) 

```xaml
<MediaTransportControls MediaElement="myMediaElement"/>
```

參考：[MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) 

設計和作法︰[媒體元素控制項指南](media-playback.md) 

範例程式碼：[媒體傳輸控制項範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCustomMediaTransportControls)

### <a name="video"></a>視訊
請參閱＜媒體元素＞。

## <a name="navigation"></a>瀏覽

### <a name="navigationview"></a>NavigationView

可調整的容器和有彈性的瀏覽模型，可實作左瀏覽窗格、上方瀏覽和索引標籤模式。

參考：[NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

設計和作法︰[NavigationView 控制項指南](navigationview.md)

### <a name="splitview"></a>SplitView

具有兩個檢視的容器控制項；一個檢視供主要內容使用，另一個檢視則通常用於導覽功能表。

![分割檢視控制項](images/controls/split-view.png) 

```xaml
<SplitView>
    <SplitView.Pane>
        <!-- Menu content -->
    </SplitView.Pane>
    <SplitView.Content>
        <!-- Main content -->
    </SplitView.Content>
</SplitView>
```

參考：[SplitView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SplitView) 

設計和作法︰[分割檢視控制項指南](split-view.md)

### <a name="web-view"></a>網頁檢視

裝載網頁內容的容器控制項。

```xaml
<WebView x:Name="webView1" Source="https://developer.microsoft.com" 
         Height="400" Width="800"/>
```

參考：[WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) 

設計和作法︰網頁檢視的指導方針 

範例程式碼：[XAML WebView 控制項範例](https://code.msdn.microsoft.com/windowsapps/XAML-WebView-control-sample-58ad63f7)

### <a name="semantic-zoom"></a>語意式縮放

讓使用者在項目集合的兩個檢視之間縮放的容器控制項。

```xaml
<SemanticZoom>
    <ZoomedInView>
        <GridView></GridView>
    </ZoomedInView>
    <ZoomedOutView>
        <GridView></GridView>
    </ZoomedOutView>
</SemanticZoom>
```

參考：[SemanticZoom](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 

設計和作法︰[語意式縮放控制項指南](semantic-zoom.md)

範例程式碼：[XAML GridView 群組和 SemanticZoom 範例](https://code.msdn.microsoft.com/windowsapps/groupedgridview-77c59e8e)

## <a name="progress-controls"></a>進度控制項

### <a name="progress-bar"></a>進度列
顯示一條列來指示進度的控制項。

![進度列控制項](images/controls/progress-bar-determinate.png)

顯示特定值的進度列。

```xaml
<ProgressBar x:Name="progressBar1" Value="50" Width="100"/>
```

![不確定的進度列控制項](images/controls/progress-bar-indeterminate.png)

顯示不確定進度的進度列。

```xaml
<ProgressBar x:Name="indeterminateProgressBar1" IsIndeterminate="True" Width="100"/>
```

參考：[ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) 

設計和作法︰[進度控制項指南](progress-controls.md) 

### <a name="progress-ring"></a>進度環
顯示一個環形來指示不確定進度的控制項。 

![進度環控制項](images/controls/progress-ring.png) 

```xaml
<ProgressRing x:Name="progressRing1" IsActive="True"/>
```

參考：[ProgressRing](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 

設計和作法︰[進度控制項指南](progress-controls.md) 

## <a name="text-controls"></a>文字控制項

### <a name="auto-suggest-box"></a>自動建議方塊
在使用者輸入時提供建議文字的文字輸入方塊。

![搜尋的自動建議方塊](images/controls/auto-suggest-box.png) 

參考：[AutoSuggestBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox)

設計和作法︰[文字控制項](text-controls.md)、[自動建議方塊控制項指南](auto-suggest-box.md)

範例程式碼：[AutoSuggestBox 移轉範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

### <a name="multi-line-text-box"></a>多行文字方塊
請參閱＜文字方塊＞。

### <a name="password-box"></a>密碼方塊
用於輸入密碼的控制項。

 ![密碼方塊](images/controls/password-box.png)

```xaml
<PasswordBox x:Name="passwordBox1" 
             PasswordChanged="PasswordBox_PasswordChanged" />
```

參考：[PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox) 

設計和作法︰[文字控制項](text-controls.md)、[密碼方塊控制項指南](password-box.md) 

範例程式碼：[XAML 文字顯示範例](https://code.msdn.microsoft.com/windowsapps/XAML-text-display-sample-2593ba0a)、[XAML 文字編輯範例](https://code.msdn.microsoft.com/windowsapps/XAML-text-editing-sample-fb0493ad)

### <a name="rich-edit-box"></a>Rich Edit 方塊
讓使用者能夠編輯 RTF 文件 (內容包括格式化文字、超連結及影像等) 的控制項。

```xaml
<RichEditBox />
```

參考：[RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) 

設計和作法︰[文字控制項](text-controls.md)、[Rich Edit 方塊控制項指南](rich-edit-box.md)

範例程式碼：[XAML 文字範例](https://code.msdn.microsoft.com/windowsapps/XAML-text-display-sample-2593ba0a)

### <a name="search-box"></a>搜尋方塊
請參閱＜自動建議方塊＞。

### <a name="single-line-text-box"></a>單行文字方塊
請參閱＜文字方塊＞。

### <a name="static-textparagraph"></a>靜態文字/段落
請參閱＜文字區塊＞。

### <a name="text-block"></a>文字區塊
顯示文字的控制項。

![文字區塊控制項](images/controls/text-block.png) 

```xaml
<TextBlock x:Name="textBlock1" Text="I am a TextBlock"/>
```

參考：[TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)、[RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) 

設計和作法︰[文字控制項](text-controls.md)、[文字區塊控制項指南](text-block.md)、[RTF 區塊控制項指南](rich-text-block.md)

範例程式碼：[XAML 文字範例](https://code.msdn.microsoft.com/windowsapps/XAML-text-display-sample-2593ba0a)

### <a name="text-box"></a>文字方塊
單行或多行純文字欄位。

![文字方塊控制項](images/controls/text-box.png)

```xaml
<TextBox x:Name="textBox1" Text="I am a Text Box."
         TextChanged="TextBox_TextChanged"/>
```

參考：[TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) 

設計和作法︰[文字控制項](text-controls.md)、[文字方塊控制項指南](text-box.md) 

範例程式碼：[XAML 文字範例](https://code.msdn.microsoft.com/windowsapps/XAML-text-display-sample-2593ba0a)

## <a name="selection-controls"></a>選取控制項

### <a name="check-box"></a>核取方塊
使用者可以選取或清除的控制項。

![核取方塊的三種狀態](images/templates-checkbox-states-default.png)

```xaml
<CheckBox x:Name="checkbox1" Content="CheckBox" 
          Checked="CheckBox_Checked"/>
```

參考：[CheckBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 

設計和作法︰[核取方塊控制項指南](checkbox.md) 

### <a name="combo-box"></a>下拉式方塊
使用者可以選取項目的下拉式清單。

![開啟下拉式方塊](images/controls/combo-box-open.png) 

```xaml
<ComboBox x:Name="comboBox1" Width="100"
          SelectionChanged="ComboBox_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
</ComboBox>
```

參考：[ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 

設計和作法︰[清單](lists.md) 

### <a name="list-box"></a>清單方塊
顯示使用者可以選取項目的內嵌項目清單的控制項。 

![清單方塊控制項](images/controls/list-box.png)

```xaml
<ListBox x:Name="listBox1" Width="100"
         SelectionChanged="ListBox_SelectionChanged">
    <x:String>List item 1</x:String>
    <x:String>List item 2</x:String>
    <x:String>List item 3</x:String>
</ListBox>
```

參考：[ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) 

設計和作法︰[清單](lists.md) 

### <a name="radio-button"></a>選項按鈕
允許使用者從選項群組中選取單一選項的控制項。 當選項按鈕被群組在一起時，彼此是互斥的。

![選項按鈕控制項](images/controls/radio-button.png)

```xaml
<RadioButton x:Name="radioButton1" Content="RadioButton 1" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
<RadioButton x:Name="radioButton2" Content="RadioButton 2" GroupName="Group1" 
             Checked="RadioButton_Checked" IsChecked="True"/>
<RadioButton x:Name="radioButton3" Content="RadioButton 3" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
```

參考：[RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 

設計和作法︰[選項按鈕控制項指南](radio-button.md)
 
### <a name="slider"></a>滑桿
一個控制項，透過讓使用者沿著軌跡移動 Thumb 控制項，從一定範圍內選取值。

![滑桿控制項](images/controls/slider.png)

```xaml
<Slider x:Name="slider1" Width="100" ValueChanged="Slider_ValueChanged" />
```

參考：[滑桿](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) 

設計和作法︰[滑桿控制項指南](slider.md) 

### <a name="toggle-button"></a>切換按鈕
可以在兩種狀態之間切換的按鈕。

```xaml
<ToggleButton x:Name="toggleButton1" Content="Button" 
              Checked="ToggleButton_Checked"/>
```

參考：[ToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton)

設計和作法︰[切換控制項指南](toggles.md) 

### <a name="toggle-switch"></a>切換開關
可以在兩種狀態之間切換的開關。

![切換開關控制項](images/controls/toggle-switch.png) 

```xaml
<ToggleSwitch x:Name="toggleSwitch1" Header="ToggleSwitch" 
              OnContent="On" OffContent="Off" 
              Toggled="ToggleSwitch_Toggled"/>
```

參考：[ToggleSwitch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) 

設計和作法︰[切換控制項指南](toggles.md) 
