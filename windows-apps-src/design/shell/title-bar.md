---
author: jwmsft
description: 自訂傳統型應用程式的標題列以符合應用程式的特質。
title: 標題列自訂
template: detail.hbs
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 標題列
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 2ebe590f98afef031ab183589fc7dcfc29cd9493
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2018
ms.locfileid: "6674049"
---
# <a name="title-bar-customization"></a>標題列自訂



應用程式在桌面視窗中執行時，您可以自訂標題列，以符合應用程式的特質。 標題列自訂 API 可讓您指定標題列元素的色彩，或將應用程式內容延伸至標題列區域，完全加以掌控。

> **重要 API**：[ApplicationView.TitleBar 屬性](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview)、[ApplicationViewTitleBar 類別](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar)、[CoreApplicationViewTitleBar 類別](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar)

## <a name="how-much-to-customize-the-title-bar"></a>可自訂標題列到什麼程度

有兩個可套用至標題列的自訂層級。

進行簡單色彩自訂時，您可以設定 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) 屬性，指定您要用於標題列元素的色彩。 在此情況下，系統繼續負責標題列所有其他層面的工作，例如繪製應用程式標題和定義可拖曳區域。

您的另一個選項是隱藏預設標題列，以您自己的 XAML 內容來取代。 例如，您可以在標題列區域中放置文字、按鈕或自訂功能表。 您也必須這個選項，才能將[壓克力](../style/acrylic.md)背景延伸至標題列區域。

當您選擇進行完整自訂時，要負責將內容置入標題列區域，而且您可以自行定義可拖曳的區域。 [返回]、[關閉]、[最小化] 及 [最大化] 系統按鈕仍然可用並且由系統來處理，但像應用程式標題這類元素則不是如此。 您必須依應用程式需要自行建立這些元素。

> [!NOTE]
> 簡單的色彩自訂適用於使用 XAML、DirectX 及 HTML 的 UWP app。 只有使用 XAML 的 UWP app，才能進行完整自訂。

## <a name="simple-color-customization"></a>簡單色彩自訂

如果您只是要自訂標題列色彩，不需弄得太花俏 (例如將索引標籤放在標題列中) 時，您可以在應用程式視窗的 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) 上設定色彩屬性。

此範例示範如何取得 ApplicationViewTitleBar 的執行個體並設定其色彩屬性。

```csharp
// using Windows.UI.ViewManagement;

var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
titleBar.ButtonForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonHoverForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonHoverBackgroundColor = Windows.UI.Colors.DarkSeaGreen;
titleBar.ButtonPressedForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonPressedBackgroundColor = Windows.UI.Colors.LightGreen;

// Set inactive window colors
titleBar.InactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.InactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonInactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonInactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
```

> [!NOTE]
> 您可以將這段程式碼放在應用程式的 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) 方法 (_App.xaml.cs_) 中，對 [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate) 的呼叫之後，或放在應用程式的第一個頁面中。

> [!TIP]
> Windows 社群工具組提供可讓您在 XAML 中設定這些色彩屬性的延伸模組。 如需詳細資訊，請參閱 [Windows 社群工具組文件](https://docs.microsoft.com/windows/uwpcommunitytoolkit/extensions/viewextensions)。

有幾個需要在設定標題列色彩時注意的事項：

- 按鈕背景色彩不會套用至 [關閉] 按鈕暫留及按下狀態。 [關閉] 按鈕在這些狀態下永遠使用系統定義的色彩。
- 使用系統 [返回] 按鈕時，按鈕色彩屬性會套用至該按鈕 (請參閱[瀏覽歷程記錄和向後瀏覽](../basics/navigation-history-and-backwards-navigation.md))。
- 將色彩屬性設定為 **null** 會將其重設為預設系統色彩。
- 您無法設定透明色彩。 將會忽略色彩的 Alpha 色板。

Windows 提供選項讓使用者將選取的[輔色](https://docs.microsoft.com/windows/uwp/style/color#accent-color)套用至標題列。 如果您設定任何標題列色彩，我們建議您明確設定所有色彩。 這樣可確保不會因為使用者定義色彩設定而出現非預期的色彩組合。

## <a name="full-customization"></a>完整自訂

當您選擇進行完整標題列自訂時，應用程式的工作區會延伸到涵蓋整個視窗，包括標題列區域。 您要負責整個視窗的繪製和輸入處理，但標題按鈕除外，這些按鈕會重疊在應用程式的畫布上面。

若要隱藏預設標題列，並將您的內容延伸至標題列區域中，請將 [CoreApplicationViewTitleBar.ExtendViewIntoTitleBar](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar) 屬性設定為 **true**。

此範例示範如何取得 CoreApplicationViewTitleBar，以及將 ExtendViewIntoTitleBar 屬性設定為 **true**。 這可以在應用程式的 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) 方法 (_App.xaml.cs_)，或在應用程式的第一個頁面中完成。

```csharp
// using Windows.ApplicationModel.Core;

// Hide default title bar.
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;
```

> [!TIP]
> 將應用程式關閉並重新啟動時，此設定會持續保存。 在 Visual Studio 中，如果您將 ExtendViewIntoTitleBar 設定為 **true**，接著又要還原成預設值時，您必須明確將其設為 **false**，再執行應用程式來覆寫保存的設定。

### <a name="draggable-regions"></a>可拖曳的區域

標題列的可拖曳區域定義使用者是否可以按一下後進行拖曳，在視窗中四處移動 (而不只是應用程式的畫布內拖曳內容)。 您可以呼叫 [Window.SetTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.settitlebar) 方法並傳入定義可拖曳區域的 UIElement，來指定可拖曳的區域 (UIElement 通常是包含其他元素的面板)。

以下說明如何將內容方格設定為可拖曳的標題列區域。 此程式碼會放入應用程式第一個頁面的 XAML 及程式碼後置中。 如需完整的程式碼，請參閱[完整自訂範例](./title-bar.md#full-customization-example)一節。

```xaml
<Grid x:Name="AppTitleBar" Background="Transparent">
    <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
    <!-- Using padding columns instead of Margin ensures that the background
         paints the area under the caption control buttons (for transparent buttons). -->
    <Grid.ColumnDefinitions>
        <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
        <ColumnDefinition/>
        <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
    </Grid.ColumnDefinitions>
    <Image Source="Assets/Square44x44Logo.png" 
           Grid.Column="1" HorizontalAlignment="Left" 
           Width="20" Height="20" Margin="12,0"/>
    <TextBlock Text="Custom Title Bar" 
               Grid.Column="1" 
               Style="{StaticResource CaptionTextBlockStyle}" 
               Margin="44,8,0,0"/>
</Grid>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;

    // Set XAML element as a draggable region.
    AppTitleBar.Height = coreTitleBar.Height;
    Window.Current.SetTitleBar(AppTitleBar);
}
```

UIElement (`AppTitleBar`) 是應用程式 XAML 的一部分。 您可以在不會變更的根頁面中宣告並設定標題列，或是在應用程式可瀏覽到的每個頁面中設定或宣告標題列區域。 如果是在每個頁面中設定，您必須確保可拖曳的區域會在使用者四處瀏覽應用程式時保持一致。

應用程式正在執行時，您可以呼叫 SetTitleBar 切換到新的標題列元素。 您也可以將 **null** 做為參數傳遞給 SetTitleBar 來還原為預設拖曳行為  (如需詳細資訊，請參閱「預設可拖曳的區域」)。

> [!IMPORTANT]
> 您指定的可拖曳區域必須可進行點擊測試，這表示您可能需要為某些元素設定透明背景筆刷。 如需詳細資訊，請參閱 [VisualTreeHelper.FindElementsInHostCoordinates](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates) 的備註。
>
>例如，如果您將方格定義為可拖曳的區域，請設定 `Background="Transparent"` 使該方格可拖曳。
>
>此方格不可拖曳 (但其中顯示的元素則可以)：`<Grid x:Name="AppTitleBar">`。
>
>此方格看起來一樣，但整個方格可拖曳：`<Grid x:Name="AppTitleBar" Background="Transparent">`。

#### <a name="default-draggable-region"></a>預設可拖曳的區域

如果您沒有指定可拖曳的區域，則會將寬度同視窗、高度同標題按鈕且沿著視窗上邊緣放置的矩形設定為預設可拖曳的區域。

如果您確實定義了可拖曳的區域，系統就會將可拖曳的區域縮減為標題按鈕大小的小區域，置於標題按鈕左側 (或右側，如果標題按鈕在視窗左側)。 這樣可確保使用者永遠都有一致可拖曳的區域。

### <a name="system-caption-buttons"></a>系統標題按鈕

系統會將應用程式視窗的左上角或右上角保留給系統標題按鈕 ([返回]、[最小化]、[最大化]、關閉])。 系統繼續擁有標題控制項區域的控制權，以保證提供最基本的拖曳、最小化，最大化及關閉視窗功能。 對於由左到右的語言，系統會在右上角繪製 [關閉]，而由右到左的語言則是在左上角繪製。

標題控制項區域的尺寸及位置是由 CoreApplicationViewTitleBar 類別傳達，這樣就可以在標題列 UI 的版面配置中加以處理。 每個側邊保留區域的寬度是由 [SystemOverlayLeftInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayLeftInset) 或 [SystemOverlayRightInset](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayRightInset) 屬性指定，而其高度則由 [Height](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.Height) 屬性指定。

您可以在這些屬性定義的標題控制項區域下方繪製內容 (例如應用程式背景)，但不應該放置您預期使用者可以與之互動的任何使用者介面。 該使用者介面不會收到任何輸入，因為對標題控制項的輸入是由系統處理。

您可以處理 [LayoutMetricsChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.LayoutMetricsChanged) 事件來回應標題按鈕大小的變更。 例如，此情況會在系統 [返回] 按鈕顯示或隱藏時發生。 處理這個事件來驗證和更新取決於標題列大小的 UI 項目位置。

此範例示範如何調整標題列的版面配置來處理像顯示或隱藏系統 [返回] 按鈕這樣的變更。 `AppTitleBar`、`LeftPaddingColumn` 和 `RightPaddingColumn` 是在先前顯示的 XAML 中宣告。

```csharp
private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}
```

### <a name="interactive-content"></a>互動式內容

您可以將互動式控制項 (例如按鈕、功能表或搜尋方塊) 放在應用程式的上半部，因此這些控制項看起來好像在標題列中。 不過，有一些規則您必須遵循，才能確保互動式元素收到使用者輸入。
- 您必須呼叫 SetTitleBar，才能將區域定義為可拖曳的標題列區域。 如果沒有這樣做，系統會將預設可拖的曳區域設定在頁面頂端。 系統接著處理對此區域的所有使用者輸入，並防止輸入到達您的控制項。
- 請將互動式控制項放置在透過呼叫 SetTitleBar (使用較高 Z 軸順序) 所定義之可拖曳區域頂端的上方。 不要讓互動式控制項成為傳遞至 SetTitleBar 之 UIElement 的子系。 將元素傳遞給 SetTitleBar 之後，系統將其視為系統標題列，並處理所有對該元素的指標輸入。

`TitleBarButton` 元素在這裡的 Z 軸順序比 `AppTitleBar` 的高，因此會接收使用者輸入。

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition />
    </Grid.RowDefinitions>

    <Grid x:Name="AppTitleBar" Background="Transparent">
        <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
        <!-- Using padding columns instead of Margin ensures that the background
             paints the area under the caption control buttons (for transparent buttons). -->
        <Grid.ColumnDefinitions>
            <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
            <ColumnDefinition/>
            <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
        </Grid.ColumnDefinitions>
        <Image Source="Assets/Square44x44Logo.png"
               Grid.Column="1" HorizontalAlignment="Left"
               Width="20" Height="20" Margin="12,0"/>
        <TextBlock Text="Custom Title Bar"
                   Grid.Column="1"
                   Style="{StaticResource CaptionTextBlockStyle}"
                   Margin="44,8,0,0"/>
    </Grid>

    <!-- This Button has a higher z-order than AppTitleBar, 
         so it receives user input. -->
    <Button x:Name="TitleBarButton" Content="Button in the title bar"
        HorizontalAlignment="Right"/>
</Grid>
```

### <a name="transparency-in-caption-buttons"></a>標題按鈕的透明度

將 ExtendViewIntoTitleBar 設定為 **true** 時，您可以讓標題按鈕的背景變透明，以允許應用程式背景顯露出來。 通常可將背景設定為 [Colors.Transparent](https://docs.microsoft.com/uwp/api/windows.ui.colors.Transparent) 以取得完全透明度。 如需部分透明度，請設定您將屬性設定成之[色彩](https://docs.microsoft.com/uwp/api/windows.ui.color)的 Alpha 色板。

下列 ApplicationViewTitleBar 屬性可以設為透明：

- ButtonBackgroundColor
- ButtonHoverBackgroundColor
- ButtonPressedBackgroundColor
- ButtonInactiveBackgroundColor

按鈕背景色彩不會套用至 [關閉] 按鈕暫留及按下狀態。 [關閉] 按鈕在這些狀態下永遠使用系統定義的色彩。

所有其他色彩屬性將會繼續忽略 Alpha 色板。 如果 ExtendViewIntoTitleBar 設定為 **false**，則永遠忽略所有 ApplicationViewTitleBar 色彩屬性的 Alpha 色板。

### <a name="full-screen-and-tablet-mode"></a>全螢幕和平板電腦模式

應用程式在_全螢幕_或_平板電腦模式_下執行時，系統會隱藏標題列和標題控制項按鈕。 不過，使用者可以叫用標題列，使之顯示為應用程式 UI 上面的覆疊。
您可以處理 [CoreApplicationViewTitleBar.IsVisibleChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.IsVisibleChanged) 事件，以便在隱藏或叫用標題列時收到通知，並視需要顯示或隱藏自訂標題列內容。

此範例示範如何處理 IsVisibleChanged 以顯示或隱藏先前所述的 `AppTitleBar` 元素。

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

>[!NOTE]
>只有在應用程式支援_全螢幕_模式時，才能進入該模式。 如需詳細資訊，請參閱 [ApplicationView.IsFullScreenMode](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.IsFullScreenMode)。 [_平板電腦模式_](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet)是支援的硬體上的使用者選項，因此使用者可以選擇在平板電腦模式下執行任何應用程式。

## <a name="full-customization-example"></a>完整自訂範例

```xaml
<Page
    x:Class="CustomTitleBar.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:CustomTitleBar"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition />
        </Grid.RowDefinitions>

        <Grid x:Name="AppTitleBar" Background="Transparent">
            <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
            <!-- Using padding columns instead of Margin ensures that the background
                 paints the area under the caption control buttons (for transparent buttons). -->
            <Grid.ColumnDefinitions>
                <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
                <ColumnDefinition/>
                <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
            </Grid.ColumnDefinitions>
            <Image Source="Assets/Square44x44Logo.png" 
                   Grid.Column="1" HorizontalAlignment="Left" 
                   Width="20" Height="20" Margin="12,0"/>
            <TextBlock Text="Custom Title Bar" 
                       Grid.Column="1" 
                       Style="{StaticResource CaptionTextBlockStyle}" 
                       Margin="44,8,0,0"/>
        </Grid>

        <!-- This Button has a higher z-order than MyTitleBar, 
             so it receives user input. -->
        <Button x:Name="TitleBarButton" Content="Button in the title bar"
                HorizontalAlignment="Right"/>
    </Grid>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Hide default title bar.
    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    UpdateTitleBarLayout(coreTitleBar);

    // Set XAML element as a draggable region.
    Window.Current.SetTitleBar(AppTitleBar);

    // Register a handler for when the size of the overlaid caption control changes.
    // For example, when the app moves to a screen with a different DPI.
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

## <a name="dos-and-donts"></a>可行與禁止注意事項

- 務必讓視窗何時處於使用中或非使用中狀態的情況變得顯而易見。 至少要在標題列中變更文字、圖示和按鈕的色彩。
- 務必沿著應用程式畫布的上邊緣定義可拖曳的區域。 符合系統標題列的放置方式會讓使用者較容易尋找。
- 務必在應用程式的畫布上定義符合視覺標題列 (若有) 的可拖曳區域。

## <a name="related-articles"></a>相關文章

- [壓克力](../style/acrylic.md)
- [色彩](../style/color.md)
