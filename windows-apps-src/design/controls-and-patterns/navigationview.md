---
author: QuinnRadich
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: 瀏覽檢視
template: detail.hbs
ms.author: quradic
ms.date: 06/25/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6c75169f118e2c8ef575fa251a7badc8cfe44247
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2018
ms.locfileid: "4022216"
---
# <a name="navigation-view-preview-version"></a>瀏覽檢視 （預覽版本）

> **這是預覽版本**： 本文章說明 NavigationView 控制項仍在開發中的新版本。 若要使用它現在，您需要的[最新的 Windows 測試人員組建和 SDK](https://insider.windows.com/for-developers/) ] 或 [ [Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

NavigationView 控制項提供您的應用程式的最上層瀏覽。 它會適應各種不同的螢幕大小支援多個瀏覽樣式。

> **Windows UI 程式庫 Api**: [Microsoft.UI.Xaml.Controls.NavigationView 類別](/uwp/api/microsoft.ui.xaml.controls.navigationview)

> **平台 Api**: [Windows.UI.Xaml.Controls.NavigationView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

## <a name="get-the-windows-ui-library"></a>取得 Windows UI 文件庫

此控制項是包含在 Windows UI 程式庫，包含新的控制項和 UI 功能適用於 UWP app 的 NuGet 套件。 如需詳細資訊，包括安裝指示，請參閱[Windows UI 文件庫的概觀](https://docs.microsoft.com/uwp/toolkits/winui/)。 

## <a name="navigation-styles"></a>瀏覽樣式

NavigationView 支援：

**左瀏覽窗格或功能表**

![展開的瀏覽窗格](images/displaymode-left.png)

**上方瀏覽窗格或功能表**

![頂端瀏覽](images/displaymode-top.png)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

NavigationView 是調適型的瀏覽控制項，可適用於：

- 提供一致的瀏覽體驗，在您的 app。
- 保留較小的 windows 上的螢幕實際可用空間。
- 組織許多瀏覽類別存取。

針對其他瀏覽控制項，請[瀏覽設計基本知識](../basics/navigation-basics.md)。

如果您的瀏覽還需要更複雜但 NavigationView 並不支援的行為時，您可能要考慮改用[主要/詳細資料](master-details.md)模式。

:::row:::
    :::column:::
        ![某些映像](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    ::: 欄範圍 ="2"::: **XAML 控制項庫**<br>
        如果您有安裝的 XAML 控制項庫應用程式，請按一下<a href="xamlcontrolsgallery:/item/NavigationView">這裡</a>開啟應用程式並查看 NavigationView 的動作。

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="display-modes"></a>顯示模式

NavigationView 可以透過設定不同的顯示模式為`PaneDisplayMode`屬性：

:::row:::
    :::column:::
    ### Left
    Displays an expanded left positioned pane.
    :::column-end:::
    :::column span="2":::
    ![left nav pane expanded](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

我們建議左側瀏覽時：

- 您有同樣重要的最上層瀏覽類別的中到高數字 (5-10)。
- 您想要其他應用程式內容較少空間很明顯的瀏覽的類別。

:::row:::
    :::column:::
    ### Top
    Displays a top positioned pane.
    :::column-end:::
    :::column span="2":::
    ![top navigation](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

我們建議頂端瀏覽時：

- 您有 5 或更少同樣重要的最上層瀏覽類別，如此最後會在下拉式清單中任何額外的最上層瀏覽類別溢位功能表會視為較不重要。
- 您需要顯示在螢幕上的所有瀏覽選項。
- 您想為您的應用程式內容要更多空間。
- 圖示不能清楚說明您的應用程式瀏覽類別。

:::row:::
    :::column:::
    ### LeftCompact
    Displays a thin sliver with icons on the left.
    :::column-end:::
    :::column span="2":::
    ![nav pane compact](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### LeftMinimal
    Displays only the menu button.
    :::column-end:::
    :::column span="2":::
    ![nav pane minimal](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Auto

![gif leftnav 預設的調適型行為](images/displaymode-auto.png)

調適之間 LeftMinimal 小型螢幕上的、 LeftCompact 上中型螢幕，並在大型螢幕上的左邊。 請參閱[調適型行為](#adaptive-behavior)如需詳細資訊。

## <a name="anatomy"></a>結構

<b>左瀏覽</b><br>

![左的 NavigationView 區段](images/leftnav-anatomy.png)

<b>上方瀏覽</b><br>

![top NavigationView 區段](images/topnav-anatomy.png)

## <a name="pane"></a>窗格

窗格可以放置在最上層或左邊，透過`PanePosition`屬性。

以下是詳細的窗格結構左上窗格位置：

<b>左瀏覽</b><br>

![NavigationView 構造](images/navview-pane-anatomy-vertical.png)

1. 功能表按鈕
1. 瀏覽項目
1. 分隔符號
1. 標頭
1. AutoSuggestBox （選擇性）
1. 設定] 按鈕 （選擇性）

<b>上方瀏覽</b><br>

![NavigationView 構造](images/navview-pane-anatomy-horizontal.png)

1. 標頭
1. 瀏覽項目
1. 分隔符號
1. AutoSuggestBox （選擇性）
1. 設定] 按鈕 （選擇性）

[上一頁] 按鈕會出現在左上角的窗格中，但 NavigationView 並不會自動將內容新增至返回堆疊。 若要啟用向後瀏覽，請參閱[向後瀏覽](#backwards-navigation)一節。

NavigationView 窗格也可以包含：

1. 瀏覽項目，形式的[NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)，用於瀏覽至特定頁面。
2. 分隔符號，格式的[NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)，適用於將瀏覽項目分組。 設定為 0，以轉譯為空格分隔字元的[Opacity](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity)屬性。
3. 標頭，用於為項目群組加上標籤的[NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader)，形式。
4. 選用[AutoSuggestBox](auto-suggest-box.md) ，允許進行應用程式層級搜尋。
5. [應用程式設定](../app-settings/app-settings-and-data.md)的選擇性進入點。 若要隱藏設定項目，請使用[IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible)屬性。

左的窗格中包含：

6. 若要切換的窗格開啟和關閉功能表按鈕。 在較大的應用程式視窗上，當窗格開啟時，您可以選擇使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 屬性隱藏此按鈕。

### <a name="pane-footer"></a>窗格頁尾

窗格頁尾中的自由格式內容 (當新增至 [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) 屬性時)

:::row:::
    :::column:::
    <b>左瀏覽</b><br>
    ![窗格頁尾的左瀏覽](images/navview-freeform-footer-left.png)<br>
    :::column-end:::
    :::column:::
     <b>上方瀏覽</b><br>
    ![窗格標頭上方瀏覽](images/navview-freeform-footer-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-header"></a>窗格標頭

窗格的標頭，當新增至[PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)屬性中的自由格式內容

:::row:::
    :::column:::
    <b>左瀏覽</b><br>
    ![窗格標頭的左瀏覽](images/navview-freeform-header-left.png)<br>
    :::column-end:::
    :::column:::
     <b>上方瀏覽</b><br>
    ![窗格標頭上方瀏覽](images/navview-freeform-header-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-content"></a>窗格的內容

在窗格中，新增到[PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)屬性時的自由格式內容

:::row:::
    :::column:::
    <b>左瀏覽</b><br>
    ![窗格自訂 contentleft 瀏覽](images/navview-freeform-pane-left.png)<br>
    :::column-end:::
    :::column:::
     <b>上方瀏覽</b><br>
    ![窗格自訂內容上方瀏覽](images/navview-freeform-pane-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="visual-style"></a>視覺樣式

當符合硬體和軟體需求時，NavigationView 會自動使用[壓克力材質](../style/acrylic.md)在其窗格中，並只在其左窗格中的 [[顯色醒目提示](../style/reveal.md)。

## <a name="header"></a>標頭

![navview 標頭區域的一般的影像](images/nav-header.png)

頁首區域與在左的窗格中的位置，瀏覽按鈕垂直對齊，並且落在上方窗格位置窗格下方。 它具有高度固定為 52 px。 其目的是保留所選瀏覽類別的頁面標題。 頁首停駐在頁面頂端，並且做為內容區域的捲動裁剪點。

當 NavigationView 處於最少的顯示模式，標頭必須看得到。 您可以選擇在其他用於較大視窗寬度的模式下隱藏頁首。 若要這麼做，請將 [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) 屬性設定為 **false**。

## <a name="content"></a>內容

![navview 內容區域的一般的影像](images/nav-content.png)

內容區域是顯示所選瀏覽類別大部分資訊的位置。

當 NavigationView 處於 [基本] 模式時，建議您在內容區域使用 12px 邊界，若為其他模式則使用 24px 邊界。

## <a name="adaptive-behavior"></a>調適性行為

NavigationView 會根據其可用的螢幕空間量自動變更顯示模式。 不過，您可能想要自訂調適型的顯示模式行為。

### <a name="default"></a>預設值

NavigationView 的預設調適型行為是顯示展開左的窗格中較大的視窗寬度、 左的僅限圖示的瀏覽窗格上的中型視窗寬度，與漢堡式功能表按鈕較小的視窗寬度。 如需有關調適型行為的視窗大小的詳細資訊，請參閱[螢幕大小與中斷點](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

![gif leftnav 預設的調適型行為](images/displaymode-auto.png)

```xaml
<NavigationView />
```

### <a name="minimal"></a>基本

第二個常見的調適型模式是使用已展開的左的窗格上的大型視窗寬度，並在這兩個中型和小的視窗寬度下漢堡式功能表。

![gif leftnav 調適型行為 2](images/adaptive-behavior-minimal.png)

```xaml
<NavigationView CompactModeThresholdWidth="1008" ExpandedModeThresholdWidth="1007" />
```

我們建議時機：

- 您想要應用程式內容較小的視窗寬度的更多的空間。
- 瀏覽類別不清楚表示具有圖示。

### <a name="compact"></a>精簡

第三個常見的調適型模式是使用已展開的左的窗格上的大型視窗寬度，與左邊的僅限圖示的瀏覽窗格在這兩個中型和小的視窗寬度下。 良好的範例就是，郵件應用程式。

![gif leftnav 調適型行為 3](images/adaptive-behavior-compact.png)

```xaml
<NavigationView CompactModeThresholdWidth="0" ExpandedModeThresholdWidth="1007" />
```

我們建議時機：

- 請務必一律顯示在螢幕上的所有瀏覽選項。
- 瀏覽類別可以清楚表示具有圖示。

### <a name="no-adaptive-behavior"></a>沒有任何調適性行為

有時您可能不想要任何調適型行為完全。 您可以設定為一律展開、 一律精簡，或一律最少的窗格。

![gif leftnav 調適型行為 4](images/adaptive-behavior-none.png)

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

### <a name="top-to-left-navigation"></a>往左側瀏覽

我們建議使用頂端瀏覽大型視窗大小，並在小型的左側瀏覽視窗調整大小的時機：

- 您有一組同樣重要的最上層瀏覽類別，以在一起，顯示，因此在此設定中的一個類別不會在螢幕上顯示，如果您摺疊來授與他們同等的左瀏覽。
- 您想要保留多內容的空間，儘可能在小型視窗大小。

以下是範例：

![gif 頂端或左邊瀏覽調適型行為 1](images/navigation-top-to-left.png)

```xaml
<Grid >
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>

    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>
</Grid>

```

有時候應用程式需要不同的資料繫結至的上方窗格和左的窗格中。 通常左的窗格中包含多個瀏覽元素。

以下是範例：

![gif 頂端或左邊瀏覽調適型行為 2](images/navigation-top-to-left2.png)

```xaml
<Page >
    <Page.Resources>
        <DataTemplate x:name="navItem_top_temp" x:DataType="models:Item">
            <NavigationViewItem Background= Icon={x:Bind TopIcon}, Content={x:Bind TopContent}, Visibility={x:Bind TopVisibility} />
        </DataTemplate>

        <DataTemplate x:name="navItem_temp" x:DataType="models:Item">
            <NavigationViewItem Icon={x:Bind Icon}, Content={x:Bind Content}, Visibility={x:Bind Visibility} />
        </DataTemplate>
        
        <services:NavViewDataTemplateSelector x:Key="navview_selector" 
              NavItemTemplate="{StaticResource navItem_temp}" 
              NavItemTopTemplate="{StaticResource navItem_top_temp}" 
              NavPaneDisplayMode="{x:Bind NavigationViewControl.PaneDisplayMode}">
        </services:NavViewDataTemplateSelector>
    </Page.Resources>

    <Grid >
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <NavView x:Name='NavigationViewControl' MenuItemsSource={x:Bind items}   
                 PanePosition = "Top" MenuItemTemplateSelector="navview_selector" />
    </Grid>
</Page>

```

```csharp
ObservableCollection<Item> items = new ObservableCollection<Item>();
items.Add(new Item() {
    Content = "Aa",
    TopContent ="A",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});
items.Add(new Item() {
    Content = "Bb",
    TopContent = "B",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});
items.Add(new Item() {
    Content = "Cc",
    TopContent = "C",
    Icon = new BitmapIcon() { UriSource = new Uri("ms-appx:///testimage.jpg") },
    TopIcon = new BitmapIcon(),
    ItemVisibility = Visibility.Visible,
    TopItemVisiblity = Visibility.Visible 
});

public class NavViewDataTemplateSelector : DataTemplateSelector
    {
        public DataTemplate NavItemTemplate { get; set; }

        public DataTemplate NavItemTopTemplate { get; set; }    

     public NavigationViewPaneDisplayMode NavPaneDisplayMode { get; set; }

        protected override DataTemplate SelectTemplateCore(object item)
        {
            Item currItem = item as Item;
            if (NavPaneDisplayMode == NavigationViewPanePosition.Top)
                return NavItemTopTemplate;
            else 
                return NavItemTemplate;
        }   

    }

```

## <a name="interaction"></a>互動

當使用者點選窗格中的瀏覽項目時，NavigationView 將該項目顯示為已選取，並引發 [ItemInvoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) 事件。 如果輕觸造成不斷地選取某個新項目，NavigationView 也會引發 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) 事件。

您的應用程式負責使用適當資訊更新頁首和內容以回應此使用者互動。 此外，也建議您以程式設計方式將[焦點](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.FocusState)從瀏覽項目移至內容。 只要在載入時設定初始焦點，即可簡化使用者流程，並將預期的鍵盤焦點移動次數減少至最低。

### <a name="tabs"></a>索引標籤

在索引標籤模型中，選取項目和焦點繫結。 動作，通常會將焦點會也移選取項目。 在以下範例中，右方向鍵會將選取項目指示器從顯示放大鏡。 您可以將[SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.selectionfollowsfocus)屬性設定為 Enabled 來達成。

![只有文字的最佳 navview 的螢幕擷取畫面](images/nav-tabs.png)

以下是範例 XAML 該：

```xaml
<NavigationView PanePosition="Top" SelectionFollowsFocus="Enabled" >
   <NavigationView.MenuItems>
        <NavigationViewItem Content="Display" />
        <NavigationViewItem Content="Magnifier"  />
        <NavigationViewItem Content="Keyboard" />
    </NavigationView.MenuItems>
</NavigationView>

```

變更] 索引標籤選取項目時，換出的內容，您可以使用畫面格的[NavigateWithOptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.NavigateToType)方法搭配 FrameNavigationOptions.IsNavigationStackEnabled 設定為 False，並 NavigateOptions.TransitionInfoOverride 將設定為適當到側邊投影片動畫。 如需範例，請參閱以下[程式碼範例](#code-example)所示。

如果您想要變更預設樣式，您可以覆寫 NavigationView 的[MenuItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemcontainerstyle)屬性。 您也可以設定[MenuItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemtemplate)屬性，以指定不同的資料範本。

## <a name="backwards-navigation"></a>向後瀏覽

NavigationView 具有內建返回按鈕，您可以使用下列屬性來啟用此按鈕：

- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) 是 NavigationViewBackButtonVisible 列舉，預設值為 "Auto"。 這會用來顯示/隱藏返回按鈕。 沒有顯示此按鈕時，將會摺疊繪製返回按鈕所佔的空間。
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) 預設為 false，可以用來切換返回按鈕狀態。
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) 會在使用者按一下返回按鈕時引發。
    - 在基本/精簡模式下，當 NavigationView.Pane 以飛出視窗開啟時，按一下返回按鈕會關閉窗格，轉而引發 **PaneClosing** 事件。
    - 如果 IsBackEnabled 為 false，則不引發。

:::row:::
    :::column:::
    <b>左瀏覽</b><br>
    ![NavigationView 返回按鈕上的左瀏覽](images/leftnav-back.png)
    :::column-end:::
    :::column:::
     <b>上方瀏覽</b><br>
    ![NavigationView 的返回按鈕上方瀏覽](images/topnav-back.png)
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>程式碼範例

> [!NOTE]
> NavigationView 應該做為應用程式的根容器，因為此控制項設計是用來伸展涵蓋應用程式視窗的完整寬度及高度。
您可以使用 [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) 和 [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth) 屬性來覆寫寬度，而瀏覽檢視會因此變更顯示模式。

以下是端對端範例示範如何將 NavigationView 大的視窗大小的頂端瀏覽窗格和左側瀏覽窗格中的，在小型視窗大小。

在此範例中，我們希望使用者經常選取新的瀏覽類別，因此我們：

- [SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion)屬性設為 Enabled
- 使用未新增至瀏覽堆疊的框架瀏覽。
- 在 [ [ShoulderNavigationEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion)屬性，用來指出是否遊戲台上的向左/向右 lb 瀏覽您的應用程式的最上層瀏覽類別上保留預設值。 預設值是 「 WhenSelectionFollowsFocus 」。 可能的值是 「 一律 」 和 「 永遠不會 」。

我們也會示範如何實作向後使用 NavigationView 的返回按鈕瀏覽。

以下是這個範例會示範的錄製：

![NavigationView 端對端範例](images/nav-code-example.gif)

以下是範例程式碼：

> [!NOTE]
> 如果您使用[Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)中，則您將需要新增此工具組的參考： `xmlns:controls="using:Microsoft.UI.Xaml.Controls"`。

```xaml
<Page
    x:Class="NavigationViewSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:NavigationViewSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <NavigationView x:Name="NavView"
                    SelectionFollowsFocus="Enabled"
                    ItemInvoked="NavView_ItemInvoked"
                    IsSettingsVisible="True"
                    Loaded="NavView_Loaded"
                    BackRequested="NavView_BackRequested"
                    Header="Welcome">

            <NavigationView.MenuItems>
                <NavigationViewItem Content="Home" x:Name="home" Tag="home">
                    <NavigationViewItem.Icon>
                        <FontIcon Glyph="&#xE10F;"/>
                    </NavigationViewItem.Icon>
                </NavigationViewItem>
                <NavigationViewItemSeparator/>
                <NavigationViewItemHeader Content="Main pages"/>
                <NavigationViewItem Icon="AllApps" Content="Apps" x:Name="apps" Tag="apps"/>
                <NavigationViewItem Icon="Video" Content="Games" x:Name="games" Tag="games"/>
                <NavigationViewItem Icon="Audio" Content="Music" x:Name="music" Tag="music"/>
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

            <Frame x:Name="ContentFrame" Margin="24"/>

        </NavigationView>
    </Grid>
</Page>
```

> [!NOTE]
> 如果您使用[Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)中，則您將需要新增此工具組的參考： `using MUXC = Microsoft.UI.Xaml.Controls;`。

```csharp
// List of ValueTuple holding the Navigation Tag and the relative Navigation Page 
private readonly IList<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code behind
    NavView.MenuItems.Add(new NavigationViewItemSeparator());
    NavView.MenuItems.Add(new NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon(Symbol.Folder),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default: you need to specify it
    NavView_Navigate("home");

    // Add keyboard accelerators for backwards navigation
    var goBack = new KeyboardAccelerator { Key = VirtualKey.GoBack };
    goBack.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(goBack);

    // ALT routes here
    var altLeft = new KeyboardAccelerator
    {
        Key = VirtualKey.Left,
        Modifiers = VirtualKeyModifiers.Menu
    };
    altLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(altLeft);
}

private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{

    if (args.IsSettingsInvoked)
        ContentFrame.Navigate(typeof(SettingsPage));
    else
    {
        // Getting the Tag from Content (args.InvokedItem is the content of NavigationViewItem)
        var navItemTag = NavView.MenuItems
            .OfType<NavigationViewItem>()
            .First(i => args.InvokedItem.Equals(i.Content))
            .Tag.ToString();

        NavView_Navigate(navItemTag);
    }
}

private void NavView_Navigate(string navItemTag)
{
    var item = _pages.First(p => p.Tag.Equals(navItemTag));
    ContentFrame.Navigate(item.Page);
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
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == NavigationViewDisplayMode.Compact ||
        NavView.DisplayMode == NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag
        NavView.SelectedItem = (NavigationViewItem)NavView.SettingsItem;
    }
    else
    {
        var item = _pages.First(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));
    }
}
```

## <a name="customizing-backgrounds"></a>自訂背景

若要變更 NavigationView 主要區域的背景，請將其 `Background` 屬性設定為您慣用的筆刷。

當 NavigationView 處於 [頂端，最少或精簡] 模式時，窗格的背景會顯示應用程式內壓克力。 若要更新此行為或自訂窗格的壓克力外觀，請在 App.xaml 中覆寫這兩個佈景主題資源以進行修改。

```xaml
<Application.Resources>
    <ResourceDictionary>
        <AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewTopPaneBackground"
        BackgroundSource="Backdrop" TintColor="Yellow" TintOpacity=".6"/>
        <AcrylicBrush x:Key="NavigationViewExpandedPaneBackground"
        BackgroundSource="HostBackdrop" TintColor="Orange" TintOpacity=".8"/>
    </ResourceDictionary>
</Application.Resources>
```

## <a name="scroll-content-under-top-pane"></a>上方窗格底下的捲動內容

對於呈現順暢的外觀 + 操作中，如果您的應用程式有使用 ScrollViewer 的頁面，而且您的瀏覽窗格頂端的位置，建議讓內容捲動上方瀏覽窗格的下方。

這可以達成[CanContentRenderOutsideBounds](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.cancontentrenderoutsidebounds)屬性設定為 true 相關的 ScrollViewer 上。

![navview 捲動 navpane](images/nav-scroll-content.png)

如果您的應用程式有很長捲動的內容，您可能會想要考慮將合併的附加到上方瀏覽窗格和表單的平滑表面固定式標頭。 

![navview 捲動固定式標頭](images/nav-scroll-stickyheader.png)

您可以設定 NavigationView [ContentOverlay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay)屬性來達成。 

有時候，如果使用者已向下捲動，您可能會想要隱藏瀏覽窗格中，透過上 NavigationView 的[IsPaneVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay)屬性設定為 false 來達成。

![navview 捲動隱藏瀏覽](images/nav-scroll-hidepane.png)

## <a name="related-topics"></a>相關主題

- [NavigationView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [主要/詳細資料](master-details.md)
- [Pivot 控制項](tabs-pivot.md)
- [瀏覽基本知識](../basics/navigation-basics.md)
- [適用於 UWP 的 Fluent Design 概觀](../fluent-design-system/index.md)