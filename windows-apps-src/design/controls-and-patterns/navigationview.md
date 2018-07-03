---
author: serenaz
Description: Control that provides top-level app navigation with an automatically adapting, collapsible left navigation menu
title: 瀏覽檢視
ms.assetid: ''
label: Navigation view
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: vasriram
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c7817bf7ff60a52ea48c988bdebd6d4d2eeacdb7
ms.sourcegitcommit: 618741673a26bd718962d4b8f859e632879f9d61
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "1992147"
---
# <a name="navigation-view"></a>瀏覽檢視

瀏覽檢視控制項在應用程式中提供最上層瀏覽的可摺疊瀏覽功能表。 此控制項實作瀏覽窗格 (或漢堡式功能表) 模式，並自動配合窗格的顯示模式調整為不同視窗大小。

> **重要 API**：[NavigationView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)、[NavigationViewItem 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)、[NavigationViewDisplayMode 列舉](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymode)

![NavigationView 範例](images/navview_wireframe.png)

## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev010/player]

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

NavigationView 適用於：

-  類型相似的許多最上層瀏覽項目  (例如，具有像是美式足球、棒球、籃球、足球等類別的運動應用程式)。
-  最上層瀏覽類別的中到高數字 (5-10)。
-  提供一致的瀏覽體驗。 窗格中應該只包含瀏覽元素，不包含動作。
-  保留較小視窗的螢幕實際可用空間。

NavigationView 只是數個您可使用的瀏覽項目的其中一個。 若要深入了解其他瀏覽模式及元素，請參閱[瀏覽設計基本知識](../basics/navigation-basics.md)。

NavigationView 控制項有許多實作簡單瀏覽窗格模式的內建行為。 如果您的瀏覽還需要更複雜但 NavigationView 並不支援的行為時，您可能要考慮改用[主要/詳細資料](master-details.md)模式。

## <a name="examples"></a>範例
<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/NavigationView">開啟應用程式並查看 NavigationView 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="navigationview-sections"></a>NavigationView 區段

![NavigationView 區段](images/navview_sections.png)

### <a name="pane"></a>窗格

內建瀏覽 (「漢堡」) 按鈕可讓使用者開啟和關閉窗格。 在較大的應用程式視窗上，當窗格開啟時，您可以選擇使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 屬性隱藏此按鈕。 與漢堡旁相鄰的文字標籤是 [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) 屬性。

內建的返回按鈕會出現在窗格的左上角。 NavigationView 控制項不會自動將內容新增至返回堆疊，但要啟用向後瀏覽，請參閱[向後瀏覽](#backwards-navigation)一節。

NavigationView 窗格也可以包含：

- 瀏覽項目，格式為 [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)，用於瀏覽至特定頁面
- 分隔符號，格式為 [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)，用於將瀏覽項目分組
- 標頭，格式為 [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader)，用於為項目群組加上標籤
- 選用的 [AutoSuggestBox](auto-suggest-box.md)，允許進行應用程式層級搜尋
- [應用程式設定](../app-settings/app-settings-and-data.md)的選擇性進入點。 若要隱藏設定項目，請使用 [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) 屬性
- 窗格頁尾中的自由格式內容 (當新增至 [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) 屬性時)

#### <a name="visual-style"></a>視覺樣式

NavigationView 提供對 [已選取]、[已停用]、[指標暫留]、[已按下] 和 [已設定焦點] 視覺狀態的支援。

![NavigationView 項目狀態︰已停用、指標暫留、已按下、已設定焦點](images/navview_item-states.png)

符合硬體和軟體需求時，NavigationView 會自動在其窗格使用新的[壓克力材質](../style/acrylic.md)和[顯色顯目提示](../style/reveal.md)。

### <a name="header"></a>頁首

頁首區域與瀏覽按鈕垂直對齊，並且高度固定為 52 px。 其目的是保留所選瀏覽類別的頁面標題。 頁首停駐在頁面頂端，並且做為內容區域的捲動裁剪點。

\[瀏覽檢視\] 處於 [基本] 模式時，頁者必須可見。 您可以選擇在其他用於較大視窗寬度的模式下隱藏頁首。 若要這麼做，請將 [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) 屬性設定為 **false**。

### <a name="content"></a>內容

內容區域是顯示所選瀏覽類別大部分資訊的位置。 

當 NavigationView 處於 [基本] 模式時，建議您在內容區域使用 12px 邊界，若為其他模式則使用 24px 邊界。

## <a name="navigationview-display-modes"></a>NavigationView 顯示模式
NavigationView 窗格可以開啟或關閉，並有三種顯示模式選項：
-  **基本**：只有漢堡式按鈕保持固定，而窗格則視需要顯示和隱藏。
-  **精簡**：窗格會一律顯示為可以展開到全寬的窄條格式。
-  **展開**：窗格在內容旁邊開啟。 當啟動漢堡按鈕將窗格關閉時，其寬度會變成狹窄的銀條。

根據預設，系統會根據螢幕可用於控制項的空間量，自動選取最佳顯示模式  (您可以[覆寫](#overriding-the-default-adaptive-behavior)此設定)。

### <a name="minimal"></a>基本

![[基本] 模式下的 NavigationView，顯示關閉和開啟的窗格。](images/navview_minimal.png)

-  關閉時，預設會隱藏窗格，只顯示瀏覽按鈕。
-  可在節省螢幕空間的情況下提供隨選瀏覽功能。 適合手機和平板手機上的應用程式。
-  按下按瀏覽按鈕會開啟和關閉窗格，此窗格將繪製成頁首和內容上方的重疊。 內容不會自動重排。
-  開啟時，窗格為暫時性，並且可以使用關閉手勢來關閉，例如進行選取、按返回按鈕，或點選窗格外部。
-  窗格重疊開啟時，就可以看到選取的項目。
-  符合需求時，開啟的窗格的背景會是[應用程式內壓克力](../style/acrylic.md#acrylic-blend-types)。
-  依預設，NavigationView 在其整體寬度小於 640px 時，處於 [基本] 模式。

### <a name="compact"></a>精簡

![[精簡] 模式下的 NavigationView，顯示關閉和開啟的窗格。](images/navview_compact.png)

-  關閉時，可看到垂直窄條狀窗格只有顯示圖示和瀏覽按鈕。
-  雖然使用螢幕的少量實際可用空間，仍可看出部分所選位置。
-  此模式很適合用於像平板電腦這樣的中型螢幕以及 [10 英呎體驗](../devices/designing-for-tv.md)。
-  按下按瀏覽按鈕會開啟和關閉窗格，此窗格將繪製成頁首和內容上方的重疊。 內容不會自動重排。
-  頁首不是必要項，可隱藏來為內容提供更多垂直空間。
-  選取的項目會顯示視覺指示器來醒目提示使用者在瀏覽樹狀目錄中的位置。
-  符合需求時，窗格的背景會是[應用程式內壓克力](../style/acrylic.md#acrylic-blend-types)。
-  依預設，NavigationView 在其整體寬度介於 641px 至 1007px 之間時，處於 [精簡] 模式。

### <a name="expanded"></a>展開

![[展開] 模式下的 NavigationView，顯示開啟的窗格](images/navview_expanded.png)

-  窗格預設會保持開啟。 這個模式適合用於較大的螢幕。
-  窗格與首頁和內容並排繪製，而內容會在其可用空間中自動重排。
-  當使用瀏覽按鈕關閉窗格時，窗格會顯示成與頁首和內容並列的窄銀條。
-  頁首不是必要項，可隱藏來為內容提供更多垂直空間。
-  選取的項目會顯示視覺指示器來醒目提示使用者在瀏覽樹狀目錄中的位置。
-  符合需求時，窗格的背景是使用[背景壓克力](../style/acrylic.md#acrylic-blend-types)來繪製。
-  依預設，NavigationView 在其整體寬度大於 1007px 時，處於 [展開] 模式。

### <a name="overriding-the-default-adaptive-behavior"></a>覆寫預設的調適性行為

NavigationView 會根據其可用的螢幕空間量自動變更顯示模式。

> [!NOTE]
> NavigationView 應該做為應用程式的根容器，因為此控制項設計是用來伸展涵蓋應用程式視窗的完整寬度及高度。
您可以使用 [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) 和 [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth) 屬性來覆寫寬度，而瀏覽檢視會因此變更顯示模式。

當您想要自訂顯示模式行為時，請考慮下列案例。

- **經常瀏覽**：如果您預期使用者會有點經常在 App 區域之間瀏覽，請考慮讓窗格在較小的視窗寬度下仍保留在檢視中。 包含 [歌曲]/[專輯]/[演出者] 瀏覽區域的 [音樂] App 可能會選擇 280px 窗格寬度，並在 App 視窗寬度大於 560px 時保持在 [展開] 模式。
```xaml
<NavigationView OpenPaneLength="280" CompactModeThresholdWidth="560" ExpandedModeThresholdWidth="560"/>
```

- **很少瀏覽** 如果您預期使用者會極少在應用程式區域之間瀏覽，請考慮讓窗格在較大的視窗寬度下保持隱藏。 包含多個版面配置的 [小算盤] App 可能會選擇保持在 [基本] 模式中，即使 App 已在 1080p 顯示器上最大化。
```xaml
<NavigationView CompactModeThresholdWidth="1920" ExpandedModeThresholdWidth="1920"/>
```

- **圖示去除混淆** 如果應用程式的瀏覽區域沒有用心設計有意義的圖示，請避免使用 [精簡] 模式。 包含 [收藏]/[相簿]/[資料夾] 瀏覽區域的影像檢視 App 可能會選擇在窄小及中等寬度下以 [基本] 模式顯示 NavigationView，而在寬大寬度下以 [展開] 模式來顯示。
```xaml
<NavigationView CompactModeThresholdWidth="1008"/>
```

## <a name="interaction"></a>互動

當使用者點選窗格中的瀏覽項目時，NavigationView 將該項目顯示為已選取，並引發 [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) 事件。 如果輕觸造成不斷地選取某個新項目，NavigationView 也會引發 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) 事件。 

您的應用程式負責使用適當資訊更新頁首和內容以回應此使用者互動。 此外，也建議您以程式設計方式將[焦點](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.FocusState)從瀏覽項目移至內容。 只要在載入時設定初始焦點，即可簡化使用者流程，並將預期的鍵盤焦點移動次數減少至最低。

## <a name="backwards-navigation"></a>向後瀏覽
NavigationView 具有內建返回按鈕，您可以使用下列屬性來啟用此按鈕：
- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) 是 NavigationViewBackButtonVisible 列舉，預設值為 "Auto"。 這會用來顯示/隱藏返回按鈕。 沒有顯示此按鈕時，將會摺疊繪製返回按鈕所佔的空間。
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) 預設為 false，可以用來切換返回按鈕狀態。
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) 會在使用者按一下返回按鈕時引發。
    - 在基本/精簡模式下，當 NavigationView.Pane 以飛出視窗開啟時，按一下返回按鈕會關閉窗格，轉而引發 **PaneClosing** 事件。
    - 如果 IsBackEnabled 為 false，則不引發。

![NavigationView 返回按鈕](../basics/images/back-nav/NavView.png)

## <a name="code-example"></a>程式碼範例

以下是示範如何將 NavigationView 納入您的 App 的簡單範例。 

我們示範如何使用 NavigationView 的返回按鈕來實作向後瀏覽。 請注意，若要使用的 NavigationView 返回瀏覽屬性，您需要 [Windows 10 Insider Preview (v10.0.17110.0 所引進)](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK)。

我們還會示範使用 `x:Uid` 進行瀏覽項目內容字串當地語系化。 如需當地語系化的詳細資訊，請參閱[當地語系化 UI 中的字串](../../app-resources/localize-strings-ui-manifest.md)。

```xaml
<Page
    x:Class="NavigationViewSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:NavigationViewSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <NavigationView x:Name="NavView"
                    ItemInvoked="NavView_ItemInvoked"
                    Loaded="NavView_Loaded"
                    BackRequested="NavView_BackRequested">

        <NavigationView.MenuItems>
            <NavigationViewItem x:Uid="HomeNavItem" Content="Home" Tag="home">
                <NavigationViewItem.Icon>
                    <FontIcon Glyph="&#xE10F;"/>
                </NavigationViewItem.Icon>
            </NavigationViewItem>
            <NavigationViewItemSeparator/>
            <NavigationViewItemHeader Content="Main pages"/>
            <NavigationViewItem x:Uid="AppsNavItem" Icon="AllApps" Content="Apps" Tag="apps"/>
            <NavigationViewItem x:Uid="GamesNavItem" Icon="Video" Content="Games" Tag="games"/>
            <NavigationViewItem x:Uid="MusicNavItem" Icon="Audio" Content="Music" Tag="music"/>
        </NavigationView.MenuItems>

        <NavigationView.AutoSuggestBox>
            <AutoSuggestBox x:Name="ASB" QueryIcon="Find"/>
        </NavigationView.AutoSuggestBox>

        <NavigationView.HeaderTemplate>
            <DataTemplate>
                <Grid Margin="24,10,0,0">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Style="{StaticResource TitleTextBlockStyle}"
                           FontSize="28"
                           VerticalAlignment="Center"
                           Text="Welcome"/>
                    <CommandBar Grid.Column="1"
                            HorizontalAlignment="Right"
                            VerticalAlignment="Top"
                            DefaultLabelPosition="Right"
                            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
                        <AppBarButton Label="Refresh" Icon="Refresh"/>
                        <AppBarButton Label="Import" Icon="Import"/>
                    </CommandBar>
                </Grid>
            </DataTemplate>
        </NavigationView.HeaderTemplate>

        <NavigationView.PaneFooter>
            <HyperlinkButton x:Name="MoreInfoBtn"
                             Content="More info"
                             Click="More_Click"
                             Margin="12,0"/>
        </NavigationView.PaneFooter>

        <Frame x:Name="ContentFrame" Margin="24">
            <Frame.ContentTransitions>
                <TransitionCollection>
                    <NavigationThemeTransition/>
                </TransitionCollection>
            </Frame.ContentTransitions>
        </Frame>

    </NavigationView>
</Page>
```

```csharp
private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // you can also add items in code behind
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem()
    { Content = "My content", Icon = new SymbolIcon(Symbol.Folder), Tag = "content" });

    // set the initial SelectedItem 
    foreach (NavigationViewItemBase item in NavView.MenuItems)
    {
        if (item is NavigationViewItem && item.Tag.ToString() == "home")
        {
            NavView.SelectedItem = item;
            break;
        }
    }
            
    ContentFrame.Navigated += On_Navigated;

    // add keyboard accelerators for backwards navigation
    KeyboardAccelerator GoBack = new KeyboardAccelerator();
    GoBack.Key = VirtualKey.GoBack;
    GoBack.Invoked += BackInvoked;
    KeyboardAccelerator AltLeft = new KeyboardAccelerator();
    AltLeft.Key = VirtualKey.Left;
    AltLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(GoBack);
    this.KeyboardAccelerators.Add(AltLeft);
    // ALT routes here
    AltLeft.Modifiers = VirtualKeyModifiers.Menu;
    
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{  
    if (args.IsSettingsInvoked)
    {
        ContentFrame.Navigate(typeof(SettingsPage));
    }
    else
    {
        // find NavigationViewItem with Content that equals InvokedItem
        var item = sender.MenuItems.OfType<NavigationViewItem>().First(x => (string)x.Content == (string)args.InvokedItem);
        NavView_Navigate(item as NavigationViewItem);
    }
}

private void NavView_Navigate(NavigationViewItem item)
{
    switch (item.Tag)
    {
        case "home":
            ContentFrame.Navigate(typeof(HomePage));
            break;

        case "apps":
            ContentFrame.Navigate(typeof(AppsPage));
            break;

        case "games":
            ContentFrame.Navigate(typeof(GamesPage));
            break;

        case "music":
            ContentFrame.Navigate(typeof(MusicPage));
            break;

        case "content":
            ContentFrame.Navigate(typeof(MyContentPage));
            break;
    }           
}

private void NavView_BackRequested(NavigationView sender, NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    bool navigated = false;

    // don't go back if the nav pane is overlayed
    if (NavView.IsPaneOpen && (NavView.DisplayMode == NavigationViewDisplayMode.Compact || NavView.DisplayMode == NavigationViewDisplayMode.Minimal))
    {
        return false;
    }
    else
    {
        if (ContentFrame.CanGoBack)
        {
            ContentFrame.GoBack();
            navigated = true;
        }
    }
    return navigated;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        NavView.SelectedItem = NavView.SettingsItem as NavigationViewItem;
    }
    else 
    {
        Dictionary<Type, string> lookup = new Dictionary<Type, string>()
        {
            {typeof(HomePage), "home"},
            {typeof(AppsPage), "apps"},
            {typeof(GamesPage), "games"},
            {typeof(MusicPage), "music"},
            {typeof(MyContentPage), "content"}    
        };

        String stringTag = lookup[ContentFrame.SourcePageType];

        // set the new SelectedItem  
        foreach (NavigationViewItemBase item in NavView.MenuItems)
        {
            if (item is NavigationViewItem && item.Tag.Equals(stringTag))
            {
                item.IsSelected = true;
                break;
            }
        }        
    }
}
```

## <a name="customizing-backgrounds"></a>自訂背景

若要變更 NavigationView 主要區域的背景，請將其 `Background` 屬性設定為您慣用的筆刷。

當 NavigationView 位於 Minimal 或 Compact 模式，且背景壓克力位於展開模式，窗格的背景會顯示應用程式內壓克力。 若要更新此行為或自訂窗格的壓克力外觀，請在 App.xaml 中覆寫這兩個佈景主題資源以進行修改。

```xaml
<Application.Resources>
    <ResourceDictionary>
        <AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewExpandedPaneBackground"
        BackgroundSource="HostBackdrop" TintColor="Orange" TintOpacity=".8"/>
    </ResourceDictionary>
</Application.Resources>
```

## <a name="extending-your-app-into-the-title-bar"></a>將應用程式延伸至標題列

為了讓您應用程式的視窗內有融合、流暢的外觀，我們建議將 NavigationView 及其壓克力窗格向上延伸到應用程式的標題列區域中。 這可避免標題列、純色 NavigationView 內容及 NavigationView 窗格壓克力所建立的圖形在視覺上顯得無趣。

若要這樣做，請在 App.xaml.cs 中新增下列程式碼。

```csharp
//draw into the title bar
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;

//remove the solid-colored backgrounds behind the caption controls and system back button
var viewTitleBar = ApplicationView.GetForCurrentView().TitleBar;
viewTitleBar.ButtonBackgroundColor = Colors.Transparent;
viewTitleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
viewTitleBar.ButtonForegroundColor = (Color)Resources["SystemBaseHighColor"];
```

標題列中的繪圖會產生隱藏應用程式標題的副作用。 若要協助使用者，請新增您自己的 TextBlock 來還原標題。 將下列標記新增至包含 NavigationView 的根頁面。

```xaml
<Grid>
    <TextBlock x:Name="AppTitle"
        xmlns:appmodel="using:Windows.ApplicationModel"
        Text="{x:Bind appmodel:Package.Current.DisplayName}"
        Style="{StaticResource CaptionTextBlockStyle}"
        IsHitTestVisible="False"
        Canvas.ZIndex="1"/>
    

    <NavigationView Canvas.ZIndex="0" ... />

</Grid>
```

您還需要根據返回按鈕的可見度調整 AppTitle 的邊界。 當應用程式處於全螢幕模式時，您必須移除返回箭號的間距，即使 TitleBar 為其保留空間，也是如此。

```csharp
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
Window.Current.SetTitleBar(AppTitle);
coreTitleBar.ExtendViewIntoTitleBar = true;

void UpdateAppTitle()
{
    var full = (ApplicationView.GetForCurrentView().IsFullScreenMode);
    var left = 12 + (full ? 0 : CoreApplication.GetCurrentView().TitleBar.SystemOverlayLeftInset);
    AppTitle.Margin = new Thickness(left, 8, 0, 0);
}

Window.Current.CoreWindow.SizeChanged += (s, e) => UpdateAppTitle();
coreTitleBar.LayoutMetricsChanged += (s, e) => UpdateAppTitle();
```

如需有關自訂標題列的詳細資訊，請參閱[標題列自訂](../shell/title-bar.md)。

## <a name="related-topics"></a>相關主題

- [NavigationView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [主要/詳細資料](master-details.md)
- [Pivot 控制項](tabs-pivot.md)
- [瀏覽基本知識](../basics/navigation-basics.md)
- [適用於 UWP 的 Fluent Design 概觀](../fluent-design-system/index.md)

