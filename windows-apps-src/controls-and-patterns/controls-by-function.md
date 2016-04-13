---
Description: 提供可用於 app 的部分控制項清單 (依功能分類)。
title: 依功能分類的控制項
ms.assetid: 8DB4347B-91D6-4659-91F2-80ECF7BBB596
label: 依功能分類的控制項
template: detail.hbs
---
# 依功能分類的控制項

Windows 的 XAML UI 架構提供一個支援 UI 開發的龐大控制項程式庫。 部分控制項以視覺方式呈現；其餘控制項則當做其他控制項或內容 (例如影像與媒體) 的容器。 

您可以下載 [**XAML UI 基本知識範例**](http://go.microsoft.com/fwlink/p/?LinkId=619992)，以查看許多 Windows UI 控制項。 

以下是可用於 App 的常用 XAML 控制項清單 (依功能分類)。 

## 應用程式列與命令

### 應用程式列
顯示應用程式特定命令的工具列。 請參閱＜命令列＞。

參考：[AppBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.aspx) 

### 應用程式列按鈕
使用應用程式列樣式顯示命令的按鈕。

![應用程式列按鈕圖示](images/controls/app-bar-buttons.png) 

參考：[AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx)、[SymbolIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.symbolicon.aspx)、[BitmapIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.bitmapicon.aspx)、[FontIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.fonticon.aspx)、[PathIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pathicon.aspx) 

設計和作法︰[應用程式列和命令列控制項指南](app-bars.md) 

範例程式碼：[XAML 命令範例](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### 應用程式列分隔符號
在視覺上分隔命令列中的命令群組。

參考：[AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) 

範例程式碼：[XAML 命令範例](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### 應用程式列切換按鈕
用於在命令列中切換命令的按鈕。

參考：[AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx) 

範例程式碼：[XAML 命令範例](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### 命令列
一個處理應用程式列按鈕元素大小調整的特殊化應用程式列。

![命令列控制項](images/command-bar-compact.png)

```xaml
<CommandBar>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
</CommandBar>
```
參考：[CommandBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx) 

設計和作法︰[應用程式列和命令列控制項指南](app-bars.md)

範例程式碼：[XAML 命令範例](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## 按鈕

### 按鈕
回應使用者輸入並引發 **Click** 事件的控制項。

![標準按鈕](images/controls/button.png)

```xaml
<Button x:Name="button1" Content="Button" 
        Click="Button_Click" />
```

參考：[Button](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.button.aspx) 

設計和︰[按鈕控制項指南](buttons.md) 

### 超連結
請參閱＜超連結按鈕＞。

### 超連結按鈕
顯示為標記文字並且會在瀏覽器中開啟指定 URI 的按鈕。

![超連結按鈕](images/controls/hyperlink-button.png)

```xaml
<HyperlinkButton Content="www.microsoft.com" 
                 NavigateUri="http://www.microsoft.com"/>
```

參考：[HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.hyperlinkbutton.aspx) 

設計和作法︰[超連結控制項指南](hyperlinks.md)

### 重複按鈕
一個按鈕，從按下到放開的這段期間，會重複引發 **Click** 事件。 

![重複按鈕控制項](images/controls/repeat-button.png) 

```xaml
<RepeatButton x:Name="repeatButton1" Content="Repeat Button" 
              Click="RepeatButton_Click" />
```

參考：[RepeatButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.repeatbutton.aspx) 

設計和︰[按鈕控制項指南](buttons.md) 

## 集合/資料控制項

### 翻轉檢視
讓使用者可以逐一瀏覽項目集合 (一次瀏覽一個項目) 的控制項。

```xaml
<FlipView x:Name="flipView1" SelectionChanged="FlipView_SelectionChanged">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

參考：[FlipView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview.aspx) 

設計和作法︰[翻轉檢視控制項指南](flipview.md) 

### 格線檢視
在可以水平捲動的列和欄中顯示項目集合的控制項。

```xaml
<GridView x:Name="gridView1" SelectionChanged="GridView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</GridView>
```

參考：[GridView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx) 

設計和作法︰[Lists](lists.md) 

範例程式碼：[ListView 範例](http://go.microsoft.com/fwlink/p/?LinkId=619900)

### 項目控制項
在資料範本指定的 UI 中顯示項目集合的控制項。 

```xaml
<ItemsControl/>
```

參考：[ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx) 

### 清單檢視
在可以垂直捲動的清單中顯示項目集合的控制項。

```xaml
<ListView x:Name="listView1" SelectionChanged="ListView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</ListView>
```

參考：[ListView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx) 

設計和作法︰[Lists](lists.md) 

範例程式碼：[ListView 範例](http://go.microsoft.com/fwlink/p/?LinkId=619900)

## 日期和時間控制項

### 行事曆日期選擇器
可讓使用者使用下拉式行事曆顯示畫面選取日期的控制項。

![已開啟行事曆檢視的行事曆日期選擇器](images/controls/calendar-date-picker-open.png)

```xaml
<CalendarDatePicker/>
```

參考：[CalendarDatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx) 

設計和作法︰[行事曆、日期和時間控制項](date-and-time.md)
 
### 行事曆檢視
可讓使用者選取單一或多個日期的可設定式行事曆顯示畫面。

```xaml
<CalendarView/>
```

參考：[CalendarView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx) 

設計和作法︰[行事曆、日期和時間控制項](date-and-time.md) 

### 日期選擇器
讓使用者能夠選取日期的控制項。

![日期選擇器控制項](images/controls/date-picker.png)

```xaml
<DatePicker Header="Arrival Date"/>
```

參考：[DatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx) 

設計和作法︰[行事曆、日期和時間控制項](date-and-time.md)
 
### 時間選擇器
讓使用者能夠設定時間值的控制項。

![TimePicker 控制項](images/controls/time-picker.png) 

```xaml
<TimePicker Header="Arrival Time"/>
```

參考：[TimePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx) 

設計和作法︰[行事曆、日期和時間控制項](date-and-time.md)

## 飛出視窗

### 操作功能表
請參閱＜功能表飛出視窗＞和＜快顯功能表＞。

### 飛出視窗
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

參考：[Flyout](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flyout.aspx) 

設計和作法︰[操作功能表和對話方塊](dialogs-popups-menus.md) 

### 功能表飛出視窗
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

參考：[MenuFlyout](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyout.aspx)、[MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx)、[MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx)、[ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) 

設計和作法︰[操作功能表和對話方塊](dialogs-popups-menus.md) 

範例程式碼：[XAML 操作功能表範例](http://go.microsoft.com/fwlink/p/?LinkId=620021)

### 快顯功能表
顯示您所指定命令的自訂功能表。

參考：[PopupMenu](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.popups.popupmenu.aspx) 

設計和作法︰[操作功能表和對話方塊](dialogs-popups-menus.md) 

### 工具提示
顯示元素資訊的快顯視窗。 
 
![工具提示控制項](images/controls/tool-tip.png)

```xaml
<Button Content="Button" Click="Button_Click" 
        ToolTipService.ToolTip="Click to perform action" />
```

參考：[ToolTip](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.tooltip.aspx)、[ToolTipService](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.tooltipservice.aspx) 

設計和作法：工具提示的指導方針 

## 影像

### 影像
顯示影像的控制項。

```xaml
<Image Source="Assets/Logo.png" />
```

參考：[Image](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx) 

設計和作法：[Image 和 ImageBrush](images-imagebrushes.md) 

範例程式碼：[XAML 影像範例](http://go.microsoft.com/fwlink/p/?linkid=226867)

## 圖形與筆墨

### InkCanvas
接收及顯示筆墨筆觸的控制項。

```xaml
<InkCanvas/>
```

參考：[InkCanvas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.inkcanvas.aspx) 

### 形狀
各種保留模式圖形物件，可以使用橢圓形、長方形、直線、貝茲路徑之類的物件顯示。

![多邊形](images/controls/shapes-polygon.png) 
![路徑](images/controls/shapes-path.png) 

```xaml
<Ellipse/>
<Path/>
<Rectangle/>
```

參考：[Shapes](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.aspx) 

作法：[繪製形狀](../graphics/drawing-shapes.md) 

範例程式碼：[XAML 向量繪製範例](http://go.microsoft.com/fwlink/p/?linkid=226866)

## 配置控制項

### 框線
在另一個物件周圍繪製框線、背景或兩者皆繪製的容器控制項。

![兩個矩形的框線](images/controls/border.png) 

```xaml
<Border BorderBrush="Blue" BorderThickness="4" 
        Height="108" Width="64" 
        Padding="8" CornerRadius="4">
    <Canvas>
        <Rectangle Fill="Yellow"/>
        <Rectangle Fill="Green" Margin="0,44"/>
    </Canvas>
</Border>
```

參考：[Border](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.border.aspx)

### 畫布
支援將子元素以畫布左上角為起點進行絕對定位的配置面板。
 
![畫布配置面板](images/controls/canvas.png) 

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Yellow" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

參考：[Canvas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx)
 
### Grid
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
    <Rectangle Fill="Yellow" Grid.Row="1" Grid.Column="1"/>
</Grid>
```

參考：[Grid](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)
 
### 移動瀏覽捲動檢視器
請參閱＜捲動檢視器＞。

### RelativePanel
一個面板，可讓您定位及排列彼此有關係或與上層面板有關係的子物件。

![相對面板配置面板](images/controls/relative-panel.png) 

```xaml
<RelativePanel>
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

參考：[RelativePanel](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx)

### 捲軸
請參閱＜捲動檢視器＞。 (ScrollBar 是 ScrollViewer 的元素。 您通常不會將它做為獨立控制項)。

參考：[ScrollBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.scrollbar.aspx)
 
### 捲動檢視器
讓使用者移動瀏覽和縮放內容的容器控制項。

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10" 
              HorizontalScrollMode="Enabled" 
              HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

參考：[ScrollViewer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx)

設計和作法︰[捲動和移動瀏覽控制項指南](scroll-controls.md) 

範例程式碼：[XAML 捲動、移動瀏覽和縮放範例](http://go.microsoft.com/fwlink/p/?linkid=238577)

### 堆疊面板
可以將子元素按水平或垂直方向排列到單行中的配置面板。

![堆疊面板配置控制項](images/controls/stack-panel.png) 

```xaml
<StackPanel>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue"/>
    <Rectangle Fill="Green"/>
    <Rectangle Fill="Yellow"/>
</StackPanel>
```

參考：[StackPanel](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx)
 
### VariableSizedWrapGrid
支援以列和欄排列子元素的配置面板。 每個子元素可以橫跨多列和多欄。

![不同大小換行格線配置面板](images/controls/variable-sized-wrap-grid.png) 

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Height="80" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" Width="80" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Yellow" Height="80" Width="80" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```

參考：[VariableSizedWrapGrid](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx)

### Viewbox
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

參考：[Viewbox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.viewbox.aspx)
 
### 縮放捲動檢視器
請參閱＜捲動檢視器＞。

## 媒體控制項

### 音訊
請參閱＜媒體元素＞。

### 媒體元素
播放音訊和視訊內容的控制項。

```xaml
<MediaElement x:Name="myMediaElement"/>
```

參考：[MediaElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediaelement.aspx) 

設計和作法︰[媒體元素控制項指南](media-playback.md)

### MediaTransportControls
為 MediaElement 提供播放控制項的控制項。

![具有傳輸控制項的媒體元素](images/controls/media-transport-controls.png) 

```xaml
<MediaTransportControls MediaElement="myMediaElement"/>
```

參考：[MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) 

設計和作法︰[媒體元素控制項指南](media-playback.md) 

範例程式碼：[媒體傳輸控制項範例](http://go.microsoft.com/fwlink/p/?LinkId=620023)

### 影片
請參閱＜媒體元素＞。

## 瀏覽

### 中樞
可讓使用者檢視並瀏覽到不同內容區段的容器控制項。

```xaml
<Hub>
    <HubSection>
        <!--- hub section content -->
    </HubSection>
    <HubSection>
        <!--- hub section content -->
    </HubSection>
</Hub>
```

參考：[Hub](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.hub.aspx) 

設計和作法︰[中樞控制項指南](hub.md) 

範例程式碼：[XAML中樞控制項範例](http://go.microsoft.com/fwlink/p/?LinkID=309828)

### 樞紐分析
全螢幕容器和瀏覽模型也可讓您迅速在不同的樞紐分析 (檢視或篩選) 之間移動，而通常是在同一組資料。

樞紐分析控制項的樣式可設定為具有「索引標籤」版面配置。

參考：[Pivot](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) 

設計和作法︰[索引標籤和樞紐分析控制項指南](tabs-pivot.md) 

範例程式碼：[樞紐分析範例](http://go.microsoft.com/fwlink/p/?LinkId=619903&amp;clcid=0x409)

### 語意式縮放
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

參考：[SemanticZoom](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.aspx) 

設計和作法︰[語意式縮放控制項指南](semantic-zoom.md) 

範例程式碼：[XAML GridView 群組和 SemanticZoom 範例](http://go.microsoft.com/fwlink/p/?linkid=226564)

### SplitView
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

參考：[SplitView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.aspx) 

設計和作法：[分割檢視控制項指南](split-view.md)

### 網頁檢視
裝載網頁內容的容器控制項。

```xaml
<WebView x:Name="webView1" Source="http://dev.windows.com" 
         Height="400" Width="800"/>
```

參考：[WebView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx) 

設計和作法：網頁檢視的指導方針 

範例程式碼：[XAML WebView 控制項範例](http://go.microsoft.com/fwlink/p/?linkid=238582)

## 進度控制項

### 進度列
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

參考：[ProgressBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.aspx) 

設計和作法︰[進度控制項指南](progress-controls.md) 

### 進度環
顯示一個環形來指示不確定進度的控制項。 

![進度環控制項](images/controls/progress-ring.png) 

```xaml
<ProgressRing x:Name="progressRing1" IsActive="True"/>
```

參考：[ProgressRing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.aspx) 

設計和作法︰[進度控制項指南](progress-controls.md) 

## 文字控制項

### 自動建議方塊
在使用者輸入時提供建議文字的文字輸入方塊。

![搜尋的自動建議方塊](images/controls/auto-suggest-box.png) 

參考：[AutoSuggestBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx)

設計和作法︰[文字控制項](text-controls.md)、[自動建議方塊控制項指南](auto-suggest-box.md)

範例程式碼：[AutoSuggestBox 移轉範例](http://go.microsoft.com/fwlink/p/?LinkId=619996)

### 多行文字方塊
請參閱＜文字方塊＞。

### 密碼方塊
用於輸入密碼的控制項。

 ![密碼方塊](images/controls/password-box.png)

```xaml
<PasswordBox x:Name="passwordBox1" 
             PasswordChanged="PasswordBox_PasswordChanged" />
```

參考：[PasswordBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx) 

設計和作法︰[文字控制項](text-controls.md)、[密碼方塊控制項指南](password-box.md) 

範例程式碼：[XAML 文字顯示範例](http://go.microsoft.com/fwlink/p/?linkid=238579)、[XAML 文字編輯範例](http://go.microsoft.com/fwlink/p/?linkid=251417)

### Rich Edit 方塊
讓使用者能夠編輯 RTF 文件 (內容包括格式化文字、超連結及影像等) 的控制項。

```xaml
<RichEditBox />
```

參考：[RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) 

設計和作法︰[文字控制項](text-controls.md)、[Rich Edit 方塊控制項指南](rich-edit-box.md)

範例程式碼：[XAML 文字範例](http://go.microsoft.com/fwlink/p/?linkid=238578)

### 搜尋方塊
請參閱＜自動建議方塊＞。

### 單行文字方塊
請參閱＜文字方塊＞。

### 靜態文字/段落
請參閱＜文字區塊＞。

### 文字區塊
顯示文字的控制項。

![文字區塊控制項](images/controls/text-block.png) 

```xaml
<TextBlock x:Name="textBlock1" Text="I am a TextBlock"/>
```

參考：[TextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx)、[RichTextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx) 

設計和作法︰[文字控制項](text-controls.md)、[文字區塊控制項指南](text-block.md)、[RTF 區塊控制項指南](rich-text-block.md)

範例程式碼：[XAML 文字範例](http://go.microsoft.com/fwlink/p/?linkid=238578)

### 文字方塊
單行或多行純文字欄位。

![文字方塊控制項](images/controls/text-box.png) 

```xaml
<TextBox x:Name="textBox1" Text="I am a TextBox" 
         TextChanged="TextBox_TextChanged"/>
```

參考：[TextBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) 

設計和作法︰[文字控制項](text-controls.md)、[文字方塊控制項指南](text-box.md) 

範例程式碼：[XAML 文字範例](http://go.microsoft.com/fwlink/p/?linkid=238578)

## 選取控制項

### 核取方塊
使用者可以選取或清除的控制項。

![核取方塊的三種狀態](images/templates-checkbox-states-default.png)

```xaml
<CheckBox x:Name="checkbox1" Content="CheckBox" 
          Checked="CheckBox_Checked"/>
```

參考：[CheckBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.checkbox.aspx) 

設計和作法︰[核取方塊控制項指南](checkbox.md) 

### 下拉式方塊
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

參考：[ComboBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.combobox.aspx) 

設計和作法︰[Lists](lists.md) 

### 清單方塊
顯示使用者可以選取項目的內嵌項目清單的控制項。 

![清單方塊控制項](images/controls/list-box.png)

```xaml
<ListBox x:Name="listBox1" Width="100"
         SelectionChanged="ListBox_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
</ListBox>
```

參考：[ListBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listbox.aspx) 

設計和作法︰[Lists](lists.md) 

### 選項按鈕
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

參考：[RadioButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.radiobutton.aspx) 

設計和作法︰[選項按鈕控制項指南](radio-button.md)
 
### 滑桿
一個控制項，透過讓使用者沿著軌跡移動 Thumb 控制項，從一定範圍內選取值。

![滑桿控制項](images/controls/slider.png)

```xaml
<Slider x:Name="slider1" Width="100" ValueChanged="Slider_ValueChanged" />
```

參考：[Slider](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx) 

設計和作法︰[滑桿控制項指南](slider.md) 

### 切換按鈕
可以在兩種狀態之間切換的按鈕。

```xaml
<ToggleButton x:Name="toggleButton1" Content="Button" 
              Checked="ToggleButton_Checked"/>
```

參考：[ToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.togglebutton.aspx)

設計和作法︰[切換控制項指南](toggles.md) 

### 切換開關
可以在兩種狀態之間切換的開關。

![切換開關控制項](images/controls/toggle-switch.png) 

```xaml
<ToggleSwitch x:Name="toggleSwitch1" Header="ToggleSwitch" 
              OnContent="On" OffContent="Off" 
              Toggled="ToggleSwitch_Toggled"/>
```

參考：[ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.toggleswitch.aspx) 

設計和作法︰[切換控制項指南](toggles.md) 


<!--HONumber=Mar16_HO1-->


