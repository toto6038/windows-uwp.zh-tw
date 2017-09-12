---
author: Jwmsft
Description: "配置最上層瀏覽又同時節省螢幕實際可用空間的控制項"
title: "瀏覽檢視"
ms.assetid: 
label: Navigation view
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.openlocfilehash: 98ab8a95288e5a0225a2185f7ce037e16209d173
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2017
---
# <a name="navigation-view"></a>瀏覽檢視

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

> [!IMPORTANT]
> 本文說明尚未發佈但可能在正式發行前大幅度修改的功能。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

瀏覽檢視控制項透過可摺疊瀏覽功能表，提供 App 最上層區域的一般垂直版面配置。 此控制項設計用來實作瀏覽窗格 (或漢堡式功能表) 模式，並自動配合其版面配置調整為不同視窗大小。

> **重要 API**：[NavigationView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)、 [NavigationViewItem 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)、 [NavigationViewDisplayMode 列舉](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymode)

![NavigationView 範例](images/navview_wireframe.png)


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

NavigationView 適用於：

-  具有許多相似類型最上層瀏覽項目的 App。 例如，具有像是美式足球、棒球、籃球、足球等類別的運動 app。
-  提供跨應用程式一致的瀏覽體驗。 窗格中應該只包含瀏覽元素，不包含動作。
-  最上層瀏覽類別的中到高數字 (5-10)。
-  保留較小視窗的螢幕實際可用空間。

瀏覽檢視只是您可以使用的數個瀏覽元素其中之一；若要深入了解瀏覽模式及其他瀏覽元素，請參閱[通用 Windows 平台 (UWP) 應用程式的瀏覽設計基本知識](../layout/navigation-basics.md)。

如需示範如何使用 SplitView and ListView 建置瀏覽窗格模式的程式碼範例，請從 GitHub 下載 [XAML 瀏覽解決方案](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/XamlNavigation)。

## <a name="navigationview-parts"></a>NavigationView 組件
控制項概略分成三個區段：左側瀏覽窗格，以及右側的頁首和內容區域。

![NavigationView 區段](images/navview_sections.png)

### <a name="pane"></a>窗格

瀏覽窗格可以包含：

- 瀏覽項目，格式為 [NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)，用於瀏覽至特定頁面
- 分隔符號，格式為 [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)，用於將瀏覽項目分組
- 標頭，格式為 [NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader)，用於為項目群組加上標籤
- [應用程式設定](../app-settings/app-settings-and-data.md)的選擇性進入點。 若要隱藏設定項目，請使用 [IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_IsSettingsVisible) 屬性
- 窗格頁尾中的自由格式內容 (當新增至 [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_PaneFooter) 屬性時)

內建瀏覽 (「漢堡」) 按鈕可讓使用者開啟和關閉窗格。 在較大的應用程式視窗上，當窗格開啟時，您可以選擇使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_IsPaneToggleButtonVisible) 屬性隱藏此按鈕。

### <a name="header"></a>頁首

頁首區域與瀏覽按鈕垂直對齊，並且高度固定。 其目的是保留所選瀏覽類別的頁面標題。 頁首停駐在頁面頂端，並且做為內容區域的捲動裁剪點。

\[瀏覽檢視\] 處於 [基本] 模式時，頁者必須可見。 您可以選擇在其他用於較大視窗寬度的模式下隱藏頁首。 若要這麼做，請將 [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_AlwaysShowHeader) 屬性設定為 **false**。

### <a name="content"></a>內容

內容區域是顯示所選瀏覽類別大部分資訊的位置。 其中可以包含一個或多個元素，並且是適合其他子層級瀏覽 (例如[樞紐](tabs-pivot.md)) 的區域。

當 NavigationView 處於 [基本] 模式時，建議您在內容的側邊上使用 12px 邊界，若為其他模式則使用 24px 邊界。

## <a name="visual-style"></a>視覺樣式

<div class="microsoft-internal-note">
此控制項的紅線可在 [UNI](http://uni/DesignDepot.FrontEnd/#/ProductNav?t=Fluent%20Design%20System%7CControls%20%26%20Patterns%7CNavigationView) 上取得。<br/><br/>
</div>

瀏覽項目提供對 [已選取]、[已停用]、[指標暫留]、[已按下] 和 [已設定焦點] 視覺狀態的支援。

![NavigationView 項目狀態︰已停用、指標暫留、已按下、已設定焦點](images/navview_item-states.png)

符合硬體和軟體需求時，NavigationView 會自動在其窗格使用新的[壓克力材質](../style/acrylic.md)和[顯色顯目提示](../style/reveal.md)。


## <a name="navigationview-modes"></a>NavigationView 模式
\[NavigationView\] 窗格可以開啟或關閉，並有三種顯示模式選項：
-  **基本**：只有漢堡式按鈕保持固定，而窗格則視需要顯示和隱藏。
-  **精簡**：窗格會一律顯示為可以展開到全寬的窄條格式。
-  **展開**：窗格在內容旁邊開啟。 當啟動漢堡按鈕將窗格關閉時，其寬度會變成狹窄的銀條。

根據預設，系統會根據螢幕可用於控制項的空間量，自動選取最佳顯示模式。 (您可以覆寫此設定，如需詳細資訊，請參閱下一節)。

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
-  此模式很適合用於像平板電腦這樣的中型螢幕以及[10 英呎體驗](../input-and-devices/designing-for-tv.md)。
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

## <a name="overriding-the-default-adaptive-behavior"></a>覆寫預設的調適性行為

瀏覽檢視會根據其可用的螢幕空間量自動變更顯示模式。
[!NOTE] NavigationView 應該做為應用程式的根容器，此控制項設計用來伸展涵蓋應用程式視窗的完整寬度及高度。
您可以使用 [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_CompactModeThresholdWidth) 與 ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_ExpandedModeThresholdWidth) 屬性來覆寫瀏覽檢視變更顯示模式的寬度。 當您想要自訂顯示模式行為時，請考慮下列案例。

-  **經常瀏覽**：如果您預期使用者會有點經常在 App 區域之間瀏覽，請考慮讓窗格在較小的視窗寬度下仍保留在檢視中。 包含 [歌曲]/[專輯]/[演出者] 瀏覽區域的 [音樂] App 可能會選擇 280px 窗格寬度，並在 App 視窗寬度大於 560px 時保持在 [展開] 模式。
```xaml
<NavigationView OpenPaneLength=”280” CompactModeThresholdWidth="560" ExpandedModeThresholdWidth=”560”/>
```
-    **很少瀏覽** 如果您預期使用者會極少在應用程式區域之間瀏覽，請考慮讓窗格在較大的視窗寬度下保持隱藏。 包含多個版面配置的 [小算盤] App 可能會選擇保持在 [基本] 模式中，即使 App 已在 1080p 顯示器上最大化。
```xaml
<NavigationView CompactModeThresholdWidth=”1920” ExpandedModeThresholdWidth=”1920”/>
```
-    **圖示去除混淆** 如果應用程式的瀏覽區域沒有用心設計有意義的圖示，請避免使用 [精簡] 模式。 包含 [收藏]/[相簿]/[資料夾] 瀏覽區域的影像檢視 App 可能會選擇在窄小及中等寬度下以 [基本] 模式顯示 NavigationView，而在寬大寬度下以 [展開] 模式來顯示。
```xaml
<NavigationView CompactModeThresholdWidth=”1008”/>
```

## <a name="interaction"></a>互動

當使用者點選窗格中的瀏覽類別時，NavigationView 將該項目顯示為已選取，並引發 [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_ItemInvoked) 事件。 如果輕觸造成不斷地選取某個新項目，NavigationView 也會引發 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview#Windows_UI_Xaml_Controls_NavigationView_SelectionChanged) 事件。 您的應用程式負責使用適當資訊更新頁首和內容以回應此使用者互動。 此外，也建議您以程式設計方式將焦點從瀏覽項目移至內容。 只要在載入時設定初始焦點，即可簡化使用者流程，並將預期的鍵盤焦點移動次數減少至最低。

## <a name="how-to-use-navigationview"></a>如何使用 NavigationView

以下是示範如何將 NavigationView 納入您的 App 的簡單範例。

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
                    Loaded="NavView_Loaded">
    <!-- Load NavigationViewItems in NavView_Loaded. -->

        <NavigationView.HeaderTemplate>
            <DataTemplate>
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Style="{StaticResource TitleTextBlockStyle}"
                           FontSize="28"
                           VerticalAlignment="Center"
                           Margin="12,0"
                           Text="Welcome"/>
                    <CommandBar Grid.Column="1"
                            HorizontalAlignment="Right"
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

        <Frame x:Name="ContentFrame">
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
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Apps", Icon = new SymbolIcon(Symbol.AllApps), Tag = "apps" });
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Games", Icon = new SymbolIcon(Symbol.Video), Tag = "games" });
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "Music", Icon = new SymbolIcon(Symbol.Audio), Tag = "music" });
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem()
        { Text = "My content", Icon = new SymbolIcon(Symbol.Folder), Tag = "content" });

    foreach (NavigationViewItem item in NavView.MenuItems)
    {
        if (item.Tag.ToString() == "play")
        {
            NavView.SelectedItem = item;
            break;
        }
    }
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        ContentFrame.Navigate(typeof(SettingsPage));
    }
    else
    {
        switch ((args.InvokedItem as NavigationViewItem).Tag)
        {
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
}
```

## <a name="navigation"></a>瀏覽

NavigationView 不會自動在 App 的標題列中顯示 [返回] 按鈕，也不會將內容新增至返回堆疊。 控制項不會自動回應軟體或硬體返回按鈕的按壓動作。 如需本主題，以及如何將瀏覽支援到新增至 App 的詳細資訊，請參閱[瀏覽歷程記錄和向後瀏覽](../layout/navigation-history-and-backwards-navigation.md)一節。


## <a name="related-topics"></a>相關主題

* [NavigationView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
* [主要/詳細資料](master-details.md)
* [Pivot 控制項](tabs-pivot.md)
* [瀏覽基本知識](../layout/navigation-basics.md)
 

 
