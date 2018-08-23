---
author: serenaz
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: 瀏覽檢視
template: detail.hbs
ms.author: sezhen
ms.date: 08/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4c0857005d584b1fde0eb52a6ab0ef5ec29eaf44
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/23/2018
ms.locfileid: "2810908"
---
# <a name="navigation-view-preview-version"></a>導覽檢視 （預覽版本）

> **這是預覽版本**： 本文說明 NavigationView 控制項仍處於開發的新版本。 若要使用它現在，您需要的[最新的 Windows 內部組建及 sdk （英文）](https://insider.windows.com/for-developers/)或[Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

NavigationView 控制項提供您的應用程式的最上層導覽。 它會適應螢幕大小支援各種多個導覽樣式。

> **Windows 使用者介面程式庫 Api**： [Microsoft.UI.Xaml.Controls.NavigationView 類別](/uwp/api/microsoft.ui.xaml.controls.navigationview)

> **平台 Api**： [Windows.UI.Xaml.Controls.NavigationView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

## <a name="get-the-windows-ui-library"></a>取得 Windows UI 文件庫

此控制項是 Windows UI 文件庫包含新控制項和 UWP 應用程式的 UI 功能 NuGet 套件的一部分。 如需詳細資訊，包括安裝指示，請參閱[Windows UI 文件庫概觀 （英文）](https://docs.microsoft.com/uwp/toolkits/winui/)。 

## <a name="navigation-styles"></a>瀏覽樣式

NavigationView 支援：

**左的功能窗格或功能表**

![展開的巡覽窗格](images/displaymode-left.png)

**上方導覽列中的窗格或功能表**

![上方導覽列](images/displaymode-top.png)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

NavigationView 是適用於調適型的導覽控制項：

- 提供您的應用程式整個網站導覽體驗一致。
- 保存在較小的 windows 螢幕面積。
- 將組織內許多導覽類別來存取。

其他的導覽控制項，請參閱[導覽設計基礎 （英文）](../basics/navigation-basics.md)。

如果您的瀏覽還需要更複雜但 NavigationView 並不支援的行為時，您可能要考慮改用[主要/詳細資料](master-details.md)模式。

:::row:::
    :::column:::
        ![一些圖像](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    ::: 欄跨越 ="2"::: **XAML 控制項庫 （英文)**<br>
        如果您有安裝的 XAML 控制項圖庫應用程式，請按一下<a href="xamlcontrolsgallery:/item/NavigationView">這裡</a>來開啟應用程式並查看 NavigationView 運作中。

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="display-modes"></a>顯示模式

透過不同的顯示模式可以設定 NavigationView`PaneDisplayMode`屬性：

:::row:::
    :::column:::
    ### Left
    Displays an expanded left positioned pane.
    :::column-end:::
    :::column span="2":::
    ![left nav pane expanded](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

我們建議左側的導覽時：

- 您必須中型-高 (5-10) 同樣也請務必最上層導覽類別數目。
- 所需非常顯著導覽類別具有較少的其他應用程式內容的空間。

:::row:::
    :::column:::
    ### Top
    Displays a top positioned pane.
    :::column-end:::
    :::column span="2":::
    ![top navigation](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

我們建議上方導覽時：

- 您有 5 或更少同樣也請務必最上層導覽類別如此最後的下拉式清單中任何其他的最上層導覽類別溢位] 功能表中被視為較不重要。
- 您要顯示在畫面上的所有導覽選項。
- 所需更多空間的應用程式內容。
- 圖示不能清楚描述您的應用程式導覽類別。

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

![gif leftnav 預設調適型行為](images/displaymode-auto.png)

小型的畫面上的 LeftMinimal、 LeftCompact 之間適應中型畫面及左大螢幕上。 請參閱[調適型行為](#adaptive-behavior)] 區段中的詳細資訊。

## <a name="anatomy"></a>結構

<b>左的巡覽</b><br>

![左的 NavigationView 章節](images/leftnav-anatomy.png)

<b>上方的巡覽</b><br>

![熱門 NavigationView 章節](images/topnav-anatomy.png)

## <a name="pane"></a>窗格

[] 窗格中可以置於上方或左邊，透過`PanePosition`屬性。

以下是左緣和上窗格位置詳細的窗格分析：

<b>左的巡覽</b><br>

![NavigationView 分析](images/navview-pane-anatomy-vertical.png)

1. 功能表按鈕
1. 導覽項目
1. 分隔符號
1. 標頭
1. AutoSuggestBox （選用）
1. （選用） 設定] 按鈕

<b>上方的巡覽</b><br>

![NavigationView 分析](images/navview-pane-anatomy-horizontal.png)

1. 標頭
1. 導覽項目
1. 分隔符號
1. AutoSuggestBox （選用）
1. （選用） 設定] 按鈕

[上一頁] 按鈕會出現在上方的左上角的窗格，但 NavigationView 不會自動新增內容至後堆疊。 若要啟用回溯導覽，請參閱[回溯導覽](#backwards-navigation)] 區段中。

也可包含 NavigationView 窗格：

1. 導覽中的項目、 瀏覽至特定頁面[NavigationViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)、 的表單。
2. 分隔符號、 [NavigationViewItemSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)，格式為分組導覽項目。 將[不透明度](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity)屬性設定為 0 轉譯為空間的分隔符號。
3. 顯示標籤的項目群組的[NavigationViewItemHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitemheader)、 表單中的標題。
4. 選用[AutoSuggestBox](auto-suggest-box.md)可用於應用程式層級搜尋。
5. [應用程式設定](../app-settings/app-settings-and-data.md)的選擇性進入點。 若要隱藏設定項目，請使用[IsSettingsVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible)屬性。

左的窗格包含：

6. 若要切換窗格中開啟] 及 [關閉] 功能表] 按鈕。 在較大的應用程式視窗上，當窗格開啟時，您可以選擇使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 屬性隱藏此按鈕。

### <a name="pane-footer"></a>窗格頁尾

窗格頁尾中的自由格式內容 (當新增至 [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) 屬性時)

:::row:::
    :::column:::
    <b>左的巡覽</b><br>
    ![巡覽左邊的窗格頁尾](images/navview-freeform-footer-left.png)<br>
    :::column-end:::
    :::column:::
     <b>上方的巡覽</b><br>
    ![窗格標題的上方巡覽](images/navview-freeform-footer-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-header"></a>窗格標頭

自由格式內容窗格的標題上時新增至[PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)屬性

:::row:::
    :::column:::
    <b>左的巡覽</b><br>
    ![窗格標題的左邊的巡覽](images/navview-freeform-header-left.png)<br>
    :::column-end:::
    :::column:::
     <b>上方的巡覽</b><br>
    ![窗格標題的上方巡覽](images/navview-freeform-header-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="pane-content"></a>窗格中的內容

自由格式的窗格中，新增至[PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)屬性時的內容

:::row:::
    :::column:::
    <b>左的巡覽</b><br>
    ![窗格中的自訂 contentleft 巡覽](images/navview-freeform-pane-left.png)<br>
    :::column-end:::
    :::column:::
     <b>上方的巡覽</b><br>
    ![窗格自訂內容上方巡覽](images/navview-freeform-pane-top.png)<br>
    :::column-end:::
:::row-end:::

### <a name="visual-style"></a>視覺樣式

當符合硬體和軟體需求時、 NavigationView 會自動使用[Acrylic 材料](../style/acrylic.md)其窗格中，並只能在其左窗格中[顯示醒目提示](../style/reveal.md)。

## <a name="header"></a>標頭

![navview 的標頭] 區域中的一般影像](images/nav-header.png)

頁首區域在左的窗格中的位置、 導覽按鈕會垂直對齊並位於下方窗格處於上方窗格的位置。 它具有 52 固定的高度像素。 其目的是保留所選瀏覽類別的頁面標題。 頁首停駐在頁面頂端，並且做為內容區域的捲動裁剪點。

標頭必須可見時 NavigationView 處於最小的顯示模式。 您可以選擇在其他用於較大視窗寬度的模式下隱藏頁首。 若要這麼做，請將 [AlwaysShowHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) 屬性設定為 **false**。

## <a name="content"></a>內容

![navview 一般影像的內容區域](images/nav-content.png)

內容區域是顯示所選瀏覽類別大部分資訊的位置。

當 NavigationView 處於 [基本] 模式時，建議您在內容區域使用 12px 邊界，若為其他模式則使用 24px 邊界。

## <a name="adaptive-behavior"></a>調適性行為

NavigationView 會根據其可用的螢幕空間量自動變更顯示模式。 不過，您可能會想要自訂調適型的顯示模式行為。

### <a name="default"></a>預設值

NavigationView 預設調適型行為是在小型視窗寬度顯示延伸的左的窗格上大型視窗寬度、 左側的僅限圖示的巡覽窗格在中型視窗寬度與漢堡功能表] 按鈕。 如需視窗大小調適型行為的詳細資訊，請參閱[螢幕大小和中斷點](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

![gif leftnav 預設調適型行為](images/displaymode-auto.png)

```xaml
<NavigationView />
```

### <a name="minimal"></a>基本

第二個常見調適型模式是使用大型視窗寬度與這兩個的中型及小型視窗寬度漢堡] 功能表上的擴充的左的窗格。

![gif leftnav 調適型行為 2](images/adaptive-behavior-minimal.png)

```xaml
<NavigationView CompactModeThresholdWidth="1008" ExpandedModeThresholdWidth="1007" />
```

建議您遵循此情況：

- 您需在較小的視窗寬度的應用程式內容的更多空間。
- 瀏覽類別不能以圖示清楚代表。

### <a name="compact"></a>精簡

第三個一般調適型模式是使用延伸的左的窗格上大型視窗寬度以及兩個的中型及小型視窗寬度的左邊的僅限圖示的巡覽窗格。 良好的範例是郵件應用程式。

![gif leftnav 調適型行為 3](images/adaptive-behavior-compact.png)

```xaml
<NavigationView CompactModeThresholdWidth="0" ExpandedModeThresholdWidth="1007" />
```

建議您遵循此情況：

- 請務必一律顯示在畫面上的所有導覽選項。
- 以圖示都可以代表清楚的導覽類別。

### <a name="no-adaptive-behavior"></a>沒有調適型行為

有時您不可以完全需任何調適型行為。 您可以設定為永遠展開、 永遠 compact 或一律最少的窗格。

![gif leftnav 調適型行為 4](images/adaptive-behavior-none.png)

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

### <a name="top-to-left-navigation"></a>由上到左側的導覽

我們建議使用大型視窗大小及小型在左導覽列上的上方導覽列中視窗調整大小的時機：

- 您有一組同等重要的最上層導覽類別要顯示在一起，如此這組中的一個類別不會符合在畫面上，如果您摺疊至左邊瀏覽至授與相等的重要性。
- 您想要保留為更加內容空間儘可能在小型視窗大小。

以下是範例：

![gif 上方或左邊的巡覽調適型行為 1](images/navigation-top-to-left.png)

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

有時應用程式需要不同的資料繫結至頂端窗格和左邊的窗格。 通常的左的窗格包含多個導覽元素。

以下是範例：

![gif 上方或左邊的巡覽調適型行為 2](images/navigation-top-to-left2.png)

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

在索引標籤模型中，已繫結選取項目和重點。 動作的通常班次焦點會也移動選取項目。 在下列範例中，右 arrowing 會移選取項目標記顯示至 [放大鏡]。 您可以將[SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.selectionfollowsfocus)屬性設定為 [已啟用來達成。

![只有文字上方 navview 的螢幕擷取畫面](images/nav-tabs.png)

以下是範例 XAML，如：

```xaml
<NavigationView PanePosition="Top" SelectionFollowsFocus="Enabled" >
   <NavigationView.MenuItems>
        <NavigationViewItem Content="Display" />
        <NavigationViewItem Content="Magnifier"  />
        <NavigationViewItem Content="Keyboard" />
    </NavigationView.MenuItems>
</NavigationView>

```

若要更換內容時變更] 索引標籤上選取範圍時，您可以使用圖文框的[NavigateWithOptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.NavigateToType)方法搭配 FrameNavigationOptions.IsNavigationStackEnabled 設為 False，與 NavigateOptions.TransitionInfoOverride 設為適當端到側邊投影片動畫。 如需範例，請參閱下列的[程式碼範例](#code-example)。

如果您想要變更預設的樣式，您可以覆寫 NavigationView 的[MenuItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemcontainerstyle)屬性。 您也可以設定指定不同的資料範本的[MenuItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.menuitemtemplate)屬性。

## <a name="backwards-navigation"></a>向後瀏覽

NavigationView 具有內建返回按鈕，您可以使用下列屬性來啟用此按鈕：

- [**IsBackButtonVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible) 是 NavigationViewBackButtonVisible 列舉，預設值為 "Auto"。 這會用來顯示/隱藏返回按鈕。 沒有顯示此按鈕時，將會摺疊繪製返回按鈕所佔的空間。
- [**IsBackEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled) 預設為 false，可以用來切換返回按鈕狀態。
- [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) 會在使用者按一下返回按鈕時引發。
    - 在基本/精簡模式下，當 NavigationView.Pane 以飛出視窗開啟時，按一下返回按鈕會關閉窗格，轉而引發 **PaneClosing** 事件。
    - 如果 IsBackEnabled 為 false，則不引發。

:::row:::
    :::column:::
    <b>左的巡覽</b><br>
    ![NavigationView 上一頁] 按鈕上的左巡覽](images/leftnav-back.png)
    :::column-end:::
    :::column:::
     <b>上方的巡覽</b><br>
    ![在上方的巡覽 NavigationView 上一頁] 按鈕](images/topnav-back.png)
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>程式碼範例

> [!NOTE]
> NavigationView 應該做為應用程式的根容器，因為此控制項設計是用來伸展涵蓋應用程式視窗的完整寬度及高度。
您可以使用 [CompactModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.CompactModeThresholdWidth) 和 [ExpandedModeThresholdWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ExpandedModeThresholdWidth) 屬性來覆寫寬度，而瀏覽檢視會因此變更顯示模式。

以下是如何將 NavigationView 合併使用大型視窗大小上的上方導覽窗格與在小型視窗大小的左的窗格的端對端範例。

在這個範例中，我們預期使用者經常選取新的瀏覽類別與因此我們：

- 將[SelectionFollowsFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion)屬性設定為 [已啟用
- 使用未將新增至導覽堆疊的圖文框巡覽。
- 保留預設值上[ShoulderNavigationEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PanePostion)屬性，是用來指出是否遊戲台上的左/右柱瀏覽您的應用程式的最上層導覽類別。 預設值為"WhenSelectionFollowsFocus"。 其他可能的值一律""和"永不"。

我們也會示範如何實作回溯導覽配備 NavigationView 的 [上一頁] 按鈕。

以下是錄製的範例示範如何：

![NavigationView 端對端範例](images/nav-code-example.gif)

以下是範例程式碼：

> [!NOTE]
> 如果您使用[Windows 使用者介面文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)中，則您需要將參照新增至 toolkit: `xmlns:controls="using:Microsoft.UI.Xaml.Controls"`。

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
> 如果您使用[Windows 使用者介面文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)中，則您需要將參照新增至 toolkit: `using MUXC = Microsoft.UI.Xaml.Controls;`。

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

窗格的背景會顯示在應用程式 acrylic NavigationView 位於頂端，最少或壓縮模式時。 若要更新此行為或自訂窗格的壓克力外觀，請在 App.xaml 中覆寫這兩個佈景主題資源以進行修改。

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

## <a name="scroll-content-under-top-pane"></a>在頂端窗格下捲軸內容

基於一致外觀 + 風格，如果您的應用程式使用 ScrollViewer 的頁面，而且在功能窗格時頂端位於，建議具有內容的捲軸頂端的巡覽窗格下方。

這可藉由[CanContentRenderOutsideBounds](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.cancontentrenderoutsidebounds)屬性設定為 true 相關 ScrollViewer 上達成。

![navview 捲軸 navpane](images/nav-scroll-content.png)

如果您的應用程式有很長捲動內容，您可能會想要考慮合併附加至頂端的巡覽窗格與表單順利表面自黏標頭。 

![navview 捲軸自黏標頭](images/nav-scroll-stickyheader.png)

您可以設定 NavigationView [ContentOverlay](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay)屬性來達成。 

有時，如果使用者向下捲動，您可能會想要隱藏的巡覽] 窗格中，可達到的 NavigationView [IsPaneVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.ContentOverlay)屬性設定為 false。

![navview 捲軸隱藏巡覽](images/nav-scroll-hidepane.png)

## <a name="related-topics"></a>相關主題

- [NavigationView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [主要/詳細資料](master-details.md)
- [Pivot 控制項](tabs-pivot.md)
- [瀏覽基本知識](../basics/navigation-basics.md)
- [適用於 UWP 的 Fluent Design 概觀](../fluent-design-system/index.md)