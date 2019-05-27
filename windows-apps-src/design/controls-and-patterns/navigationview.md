---
Description: NavigationView 是實作您的應用程式的最上層瀏覽模式自適性控制項。
title: 瀏覽檢視
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1a396377eb332052ae7f238a23865f2b7dc0aa16
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984176"
---
# <a name="navigation-view"></a>瀏覽檢視

NavigationView 控制項提供您的應用程式的最上層導覽。 它可配合各種不同的螢幕大小，並且同時支援_頂端_並_左_導覽樣式。

![上方導覽](images/nav-view-header.png)<br/>
_瀏覽檢視支援頂端和左側瀏覽窗格或功能表_

> **平台 Api**:[Windows.UI.Xaml.Controls.NavigationView 類別](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **Windows UI 程式庫 Api**:[Microsoft.UI.Xaml.Controls.NavigationView 類別](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> NavigationView，某些功能這類_頂端_導覽中，需要 Windows 10 版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本，或有[Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

NavigationView 是適用於自動調整的瀏覽控制項：

- 提供一致的巡覽體驗，在您的應用程式。
- 保留在較小的 windows 上的螢幕畫面。
- 組織對許多瀏覽類別的詳細資訊。

其他瀏覽模式中，請參閱 <<c0> [ 瀏覽設計基本概念](../basics/navigation-basics.md)。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/NavigationView">開啟應用程式並查看 NavigationView 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>顯示模式

> PaneDisplayMode 屬性需要 Windows 10 版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本，或有[Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

您可以使用 PaneDisplayMode 屬性來設定不同的瀏覽樣式，或針對 NavigationView 中顯示模式。

:::row:::
    :::column:::
    ### <a name="top"></a>上層
    [] 窗格位於上述內容。</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![上方導覽的範例](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

我們建議_頂端_瀏覽時：

- 您有 5 或較少的最上層導覽類別同樣重要的是，以及任何其他的最上層導覽，最後會溢位下拉式功能表中的類別會被視為較不重要。
- 您要顯示在畫面上的所有導覽選項。
- 您想為您的應用程式內容的更多空間。
- 圖示無法清楚地描述您的應用程式瀏覽類別。

:::row:::
    :::column:::
    ### <a name="left"></a>Left
    窗格會展開，並位於左側的內容。</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![展開左側的導覽窗格的範例](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

我們建議_左_瀏覽時：

- 您有 5-10 同樣重要的最上層導覽類別。
- 您想要非常顯著，使用較少的空間給其他應用程式內容的瀏覽類別。

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    窗格會顯示只有圖示開啟，並位於左側的內容之前。</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![Compact 的左側的導覽窗格的範例](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    直到窗格會開啟，則會顯示功能表按鈕。 開啟時，它位於左側的內容。</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![最小的左側的導覽窗格的範例](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>自動

根據預設，PaneDisplayMode 設定為 Auto。在自動模式中，導覽檢視的適應之間 LeftMinimal 當視窗變窄時，要 LeftCompact，，然後由左而右，當視窗變寬。 如需詳細資訊，請參閱 <<c0> [ 適應性行為](#adaptive-behavior)一節。

![左側瀏覽預設自動調整行為](images/displaymode-auto.png)<br/>
_瀏覽檢視預設自動調整行為_

## <a name="anatomy"></a>結構

這些映像顯示 窗格、 頁首和設定控制項的內容區域的版面配置_頂端_或是_左_瀏覽。

![上方導覽檢視版面配置](images/topnav-anatomy.png)<br/>
_上方導覽版面配置_

![左側瀏覽檢視版面配置](images/leftnav-anatomy.png)<br/>
_左側瀏覽版面配置_

### <a name="pane"></a>窗格

您可以使用 PaneDisplayMode 屬性放置內容上方或左側的 [內容] 窗格。

NavigationView 窗格可以包含：

- [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem)物件。 瀏覽至特定頁面的瀏覽項目。
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)物件。 群組導覽項目分隔符號。 設定[不透明度](/uwp/api/windows.ui.xaml.uielement.opacity)屬性設為 0 來呈現為空格分隔符號。
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader)物件。 標記項目群組的標頭。
- 選擇性[AutoSuggestBox](auto-suggest-box.md)控制項，以允許應用程式層級搜尋。 將指定的控制項[NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox)屬性。
- [應用程式設定](../app-settings/app-settings-and-data.md)的選擇性進入點。 若要隱藏的設定項目，設定[IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible)屬性設**false**。

左的窗格也包含：

- 若要切換開啟和關閉窗格功能表按鈕。 在較大的應用程式視窗上，當窗格開啟時，您可以選擇使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 屬性隱藏此按鈕。

導覽檢視已放置在窗格的左上角的返回按鈕。 不過，它會自動處理向後巡覽，並將內容新增至上一頁堆疊。 若要啟用向後導覽，請參閱[向後巡覽](#backwards-navigation)一節。

以下是詳細的窗格結構針對框線和左窗格的位置。

#### <a name="top-navigation-pane"></a>上方導覽窗格

![瀏覽檢視上方窗格結構](images/navview-pane-anatomy-horizontal.png)

1. 標頭
1. 瀏覽項目
1. 分隔符號
1. AutoSuggestBox （選擇性）
1. （選擇性） 設定 按鈕

#### <a name="left-navigation-pane"></a>左側瀏覽窗格

![瀏覽檢視左窗格中的結構](images/navview-pane-anatomy-vertical.png)

1. 功能表按鈕
1. 瀏覽項目
1. 分隔符號
1. 標頭
1. AutoSuggestBox （選擇性）
1. （選擇性） 設定 按鈕

#### <a name="pane-footer"></a>窗格頁尾

您可以放置自由格式 窗格的頁尾中的內容將它加入至[PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)屬性。

:::row:::
    :::column:::
    ![窗格頁尾上方瀏覽](images/navview-freeform-footer-top.png)<br>
     _在上方窗格頁尾_<br>
    :::column-end:::
    :::column:::
    ![左側的導覽中窗格頁尾](images/navview-freeform-footer-left.png)<br>
    _左的窗格的頁尾_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>窗格標題和標頭

您也可以設定窗格標頭區域中放置文字內容[PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle)屬性。 它會使用字串，並顯示 [功能表] 按鈕旁邊的文字。

若要新增非文字內容，例如映像或標誌，您可以將任何項目置於窗格的標頭將它加入至[PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)屬性。

如果設定了 PaneTitle 和 PaneHeader，內容是旁邊 PaneTitle 最接近的功能表按鈕的功能表按鈕，水平堆疊。

:::row:::
    :::column:::
    ![窗格標頭頂端的導覽區](images/navview-freeform-header-top.png)<br>
     _在上方窗格的標頭_<br>
    :::column-end:::
    :::column:::
    ![窗格標頭的左側瀏覽](images/navview-freeform-header-left.png)<br>
    _左的窗格的標頭_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>窗格的內容

您可以放置自由格式 窗格中的內容將它加入至[PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)屬性。

:::row:::
    :::column:::
    ![窗格中自訂內容最上層導覽](images/navview-freeform-pane-top.png)<br>
     _在上方窗格的自訂內容_<br>
    :::column-end:::
    :::column:::
    ![左側導覽窗格中自訂的內容](images/navview-freeform-pane-left.png)<br>
    _左的窗格的自訂內容_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>標頭

您可以藉由設定中新增頁面標題[標頭](/uwp/api/windows.ui.xaml.controls.navigationview.header)屬性。

![瀏覽檢視標頭區域的範例](images/nav-header.png)<br/>
_瀏覽檢視標頭_

標頭區域與瀏覽按鈕，在左的窗格的位置，以垂直方式對齊，並位於下方的窗格中的上方窗格的位置。 它有固定的高度 52 的像素。 其目的是保留所選瀏覽類別的頁面標題。 頁首停駐在頁面頂端，並且做為內容區域的捲動裁剪點。

標頭會的隨時 NavigationView 為最小的顯示模式顯示。 您可以選擇在其他用於較大視窗寬度的模式下隱藏頁首。 若要隱藏標頭，設定[AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader)屬性設**false**。

### <a name="content"></a>內容

![瀏覽檢視內容區域的範例](images/nav-content.png)<br/>
_瀏覽檢視內容_

內容區域是顯示所選瀏覽類別大部分資訊的位置。

NavigationView 中時，我們會建議您的內容區域的 12px 邊界**最小**其他方式的模式和 24px 邊界。

## <a name="adaptive-behavior"></a>調適性行為

根據預設，[導覽] 檢視會自動變更其根據的螢幕空間可供其使用的顯示模式。 [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth)並[ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth)屬性指定的顯示模式變更中斷點。 您可以修改這些值，以自訂的自動調整的顯示模式行為。

### <a name="default"></a>預設

當 PaneDisplayMode 設為其預設值**自動**，適應性行為是要示範：

- 在較大的視窗寬度展開左的窗格 (1008px 或更新版本)。
- 保留，圖示僅限、 瀏覽窗格 (LeftCompact) 中的視窗寬度 (以 1007px 641px)。
- 只有功能表上的按鈕 (LeftMinimal) 小視窗的寬度 (640px 小於或等於)。

如需有關適應性行為的視窗大小的詳細資訊，請參閱[螢幕大小和中斷點](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

![左側瀏覽預設自動調整行為](images/displaymode-auto.png)<br/>
_瀏覽檢視預設自動調整行為_

### <a name="minimal"></a>基本

第二個常見的自動調整模式是使用較大的視窗寬度 」 和 「 僅 功能表上的按鈕，這兩個中型和小型的視窗寬度的展開左的窗格。

我們建議時：

- 您會想更多的空間較小的視窗寬度上的應用程式內容。
- 您的瀏覽類別無法清楚地表示具有圖示。

![左側瀏覽最少的適應性行為](images/adaptive-behavior-minimal.png)<br/>
_瀏覽檢視 「 最小 」，適應性行為_

若要設定此行為，請將 CompactModeThresholdWidth 設要摺疊窗格的寬度。 在這裡，它會從 640 到 1007年的預設值變更。 您也應該設定 ExpandedModeThresholdWidth 以確保不發生衝突的值。

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>精簡

第三個常見的自動調整模式是在較大的視窗寬度 」 和 「 LeftCompact，圖示僅限，在這兩個中型和小型的視窗寬度的導覽窗格上使用擴充的左的窗格。

我們建議時：

- 請務必一律會顯示在畫面上的所有導覽選項。
- 您的瀏覽類別可以使用圖示來清楚地表示。

![左側瀏覽 compact 適應性行為](images/adaptive-behavior-compact.png)<br/>
_瀏覽檢視 「 壓縮 」 適應性行為_

若要設定此行為，請設定 CompactModeThresholdWidth 設為 0。

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>沒有自動調整行為

若要停用自動調整行為，請設定 PaneDisplayMode 自動以外的值。在這裡，它會設定至 LeftMinimal，因此，只有 [功能表] 按鈕會顯示不論視窗的寬度。

![不左瀏覽任何自動調整行為](images/adaptive-behavior-none.png)<br/>
_設定為 LeftMinimal PaneDisplayMode，[導覽] 檢視_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

如先前所述_顯示模式_ 區段中，您可以設定必須永遠保持在最上層，永遠展開，一律精簡，或永遠最小的窗格。 您也可以管理的顯示模式自己應用程式程式碼。 這個範例會顯示下一節。

### <a name="top-to-left-navigation"></a>左側瀏覽至頂端

當您可以使用上方導覽應用程式中時，瀏覽項目摺疊成溢位功能表成視窗寬度減少。 當您的應用程式視窗變窄時，它可以提供較佳的使用者體驗，若要切換由上至下 PaneDisplayMode LeftMinimal 導覽，而不是讓所有項目摺疊成溢位功能表。

我們建議使用較大的視窗大小和小左方導覽頂端瀏覽的時間範圍的時機：

- 如果在此集合中的一個類別目錄不符合在畫面上，您摺疊來為其提供同樣重要的左方瀏覽，您會有一組同樣重要的最上層導覽類別一起顯示。
- 您想要保留為盡可能地小視窗大小很多內容的空格。

此範例示範如何使用[VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager)並[AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth)切換頂端和 LeftMinimal 瀏覽屬性。

![範例中的上方或左方適應性行為 1](images/navigation-top-to-left.png)

```xaml
<Grid >
    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>

```

> [!TIP]
> 當您使用 AdaptiveTrigger.MinWindowWidth 時，當視窗超出指定的最小寬度時，會觸發的可見狀態。 這表示預設的 XAML 定義窄 視窗中，而且 VisualState 定義視窗變寬時，會套用的修改。 預設 PaneDisplayMode 導覽檢視為自動，因此當視窗寬度小於或等於 CompactModeThresholdWidth，LeftMinimal 導覽會使用。 當視窗變寬時，VisualState 覆寫預設值，並會使用上方導覽。

## <a name="navigation"></a>巡覽

導覽檢視不會自動執行任何瀏覽工作。 當使用者點選瀏覽項目上時，瀏覽 檢視顯示為已選取該項目，並引發[ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked)事件。 如果新的項目被選取，會產生 tap [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged)也會引發事件。

您可以處理其中任何一個事件來執行要求的瀏覽至相關的工作。 您應該處理取決於您要為您的應用程式的行為。 一般而言，您會瀏覽至要求的頁面，並更新導覽檢視標頭，以回應這些事件。

**ItemInvoked**會引發使用者點選瀏覽項目中，任何時候，即使已選取。 （項目也會叫用對等的動作，使用滑鼠、 鍵盤或其他輸入。 如需詳細資訊，請參閱 <<c0> [ 輸入和互動](../input/index.md)。)如果您瀏覽 ItemInvoked 處理常式中，根據預設，將重新載入頁面，並重複的項目新增至導覽堆疊。 如果您瀏覽項目叫用時，您應該不允許重新載入頁面，或確定，重複項目不會在瀏覽 backstack 重新載入頁面時。 （請參閱程式碼範例。）

**SelectionChanged**由使用者叫用不目前選取的項目，或以程式設計方式變更選取的項目可能會引發。 如果使用者叫用項目，就會發生選取範圍變更時，會先發生 ItemInvoked 事件。 如果以程式設計方式選取範圍變更時，不會引發 ItemInvoked。

### <a name="backwards-navigation"></a>向後瀏覽

NavigationView 有一個內建的上一頁按鈕;但是，如同向前巡覽，而不會執行向後瀏覽自動。 當使用者點選 [上一頁] 按鈕[BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested)就會引發事件。 您處理這個事件來執行向後巡覽。 如需更多的資訊和程式碼範例，請參閱 <<c0> [ 瀏覽歷程記錄及向後巡覽](../basics/navigation-history-and-backwards-navigation.md)。

在最少或精簡模式中，瀏覽檢視窗格中則是以飛出視窗開啟項目。 在此情況下，按一下 [上一頁] 按鈕會關閉窗格並引發**PaneClosing**事件改。

您可以隱藏或停用 [上一頁] 按鈕，藉由設定這些屬性：

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible)： 用來顯示和隱藏 [上一頁] 按鈕。 這個屬性可接受的值[NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible)列舉型別，且設定為**自動**預設。 按鈕摺疊時，會不保留任何空間，在配置中。
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled)： 用來啟用或停用 [上一頁] 按鈕。 您可以此屬性為資料繫結[CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback)巡覽框架屬性。 **BackRequested**如果不會引發**IsBackEnabled**是**false**。

:::row:::
    :::column:::
        ![Navigation view back button in the left navigation pane](images/leftnav-back.png)<br/>
        _The back button in the left navigation pane_
    :::column-end:::
    :::column:::
        ![Navigation view back button in the top navigation pane](images/topnav-back.png)<br/>
        _The back button in the top navigation pane_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>程式碼範例

此範例會示範如何使用 NavigationView 上方導覽窗格中，較大的視窗大小和小視窗大小的左側瀏覽窗格。 它可適用於僅限左側的導覽藉由移除_頂端_VisualStateManager 中的瀏覽設定。

此範例示範設定適用於許多常見案例的瀏覽資料的建議的方式。 它也會示範如何實作向後巡覽及 NavigationView 的上一頁按鈕和鍵盤瀏覽。

此程式碼假設您的應用程式包含使用瀏覽至下列名稱的頁面：_首頁_， _AppsPage_， _GamesPage_， _MusicPage_， _MyContentPage_，以及_設定_. 不會顯示這些頁面的程式碼。

> [!IMPORTANT]
> 應用程式的頁面相關的資訊會儲存在[ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple)。 此結構會要求您的應用程式專案的最小版本必須 17763 或更新版本的 SDK。 如果您使用 NavigationView 的 WinUI 版本以舊版的 Windows 10 為目標時，您可以使用[System.ValueTuple NuGet 套件](https://www.nuget.org/packages/System.ValueTuple/)改。

> [!IMPORTANT]
> 此程式碼示範如何使用[Windows 的 UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)NavigationView 的版本。 如果您改為使用 NavigationView 的平台版本，您的應用程式專案的最小版本必須是 17763 或更新版本的 SDK。 若要使用的平台版本，移除所有參考`muxc:`。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Grid>
    <muxc:NavigationView x:Name="NavView"
                         Loaded="NavView_Loaded"
                         ItemInvoked="NavView_ItemInvoked"
                         BackRequested="NavView_BackRequested">
        <muxc:NavigationView.MenuItems>
            <muxc:NavigationViewItem Tag="home" Icon="Home" Content="Home"/>
            <muxc:NavigationViewItemSeparator/>
            <muxc:NavigationViewItemHeader x:Name="MainPagesHeader"
                                           Content="Main pages"/>
            <muxc:NavigationViewItem Tag="apps" Content="Apps">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB3C;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="games" Content="Games">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7FC;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="music" Icon="Audio" Content="Music"/>
        </muxc:NavigationView.MenuItems>

        <muxc:NavigationView.AutoSuggestBox>
            <!-- See AutoSuggestBox documentation for
                 more info about how to implement search. -->
            <AutoSuggestBox x:Name="NavViewSearchBox" QueryIcon="Find"/>
        </muxc:NavigationView.AutoSuggestBox>

        <ScrollViewer>
            <Frame x:Name="ContentFrame" Padding="12,0,12,24" IsTabStop="True"
                   NavigationFailed="ContentFrame_NavigationFailed"/>
        </ScrollViewer>
    </muxc:NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavView.CompactModeThresholdWidth}"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <!-- Remove the next 3 lines for left-only navigation. -->
                    <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    <Setter Target="NavViewSearchBox.Width" Value="200"/>
                    <Setter Target="MainPagesHeader.Visibility" Value="Collapsed"/>
                    <!-- Leave the next line for left-only navigation. -->
                    <Setter Target="ContentFrame.Padding" Value="24,0,24,24"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

> [!IMPORTANT]
> 此程式碼示範如何使用[Windows 的 UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)NavigationView 的版本。 如果您改為使用 NavigationView 的平台版本，您的應用程式專案的最小版本必須是 17763 或更新版本的 SDK。 若要使用的平台版本，移除所有參考`muxc`。

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

private void ContentFrame_NavigationFailed(object sender, NavigationFailedEventArgs e)
{
    throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
}

// List of ValueTuple holding the Navigation Tag and the relative Navigation Page
private readonly List<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code.
    NavView.MenuItems.Add(new muxc.NavigationViewItemSeparator());
    NavView.MenuItems.Add(new muxc.NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon((Symbol)0xF1AD),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    // Add handler for ContentFrame navigation.
    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default, so load home page.
    NavView.SelectedItem = NavView.MenuItems[0];
    // If navigation occurs on SelectionChanged, this isn't needed.
    // Because we use ItemInvoked to navigate, we need to call Navigate
    // here to load the home page.
    NavView_Navigate("home", new EntranceNavigationTransitionInfo());

    // Add keyboard accelerators for backwards navigation.
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

private void NavView_ItemInvoked(muxc.NavigationView sender,
                                 muxc.NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.InvokedItemContainer != null)
    {
        var navItemTag = args.InvokedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

<!-- NavView_SelectionChanged is not used in this example, but is shown for completeness.
     You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
     but not both. -->
private void NavView_SelectionChanged(muxc.NavigationView sender,
                                      muxc.NavigationViewSelectionChangedEventArgs args)
{
    if (args.IsSettingsSelected == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.SelectedItemContainer != null)
    {
        var navItemTag = args.SelectedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

private void NavView_Navigate(string navItemTag, NavigationTransitionInfo transitionInfo)
{
    Type _page = null;
    if (navItemTag == "settings")
    {
        _page = typeof(SettingsPage);
    }
    else
    {
        var item = _pages.FirstOrDefault(p => p.Tag.Equals(navItemTag));
        _page = item.Page;
    }
    // Get the page type before navigation so you can prevent duplicate
    // entries in the backstack.
    var preNavPageType = ContentFrame.CurrentSourcePageType;

    // Only navigate if the selected page isn't currently loaded.
    if (!(_page is null) && !Type.Equals(preNavPageType, _page))
    {
        ContentFrame.Navigate(_page, null, transitionInfo);
    }
}

private void NavView_BackRequested(muxc.NavigationView sender,
                                   muxc.NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender,
                         KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed.
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == muxc.NavigationViewDisplayMode.Compact ||
         NavView.DisplayMode == muxc.NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
        NavView.SelectedItem = (muxc.NavigationViewItem)NavView.SettingsItem;
        NavView.Header = "Settings";
    }
    else if (ContentFrame.SourcePageType != null)
    {
        var item = _pages.FirstOrDefault(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<muxc.NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));

        NavView.Header =
            ((muxc.NavigationViewItem)NavView.SelectedItem)?.Content?.ToString();
    }
}
```

以下是[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/index)新版**NavView_ItemInvoked**處理常式，從C#上述程式碼範例。 技術C++/WinRT 處理常式需要您先儲存 (中的標記[ **NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem)) 的完整類型名稱，您要瀏覽的頁面。 在處理常式中的值進行 unbox 處理，將它變成[ **Windows::UI::Xaml::Interop::TypeName** ](/uwp/api/windows.ui.xaml.interop.typename)物件，並使用它來瀏覽至 [目的地] 頁面。 對應變數，名為無須`_pages`中看到C#範例中，而且您將能夠建立單元測試，以確認您的標記內的值屬於有效的型別。 另請參閱[Boxing 和 unboxing 的純量值，以與 IInspectable C++/WinRT](/windows/uwp/cpp-and-winrt-apis/boxing)。

```cppwinrt
void MainPage::NavView_ItemInvoked(Windows::Foundation::IInspectable const & /* sender */, Windows::UI::Xaml::Controls::NavigationViewItemInvokedEventArgs const & args)
{
    if (args.IsSettingsInvoked())
    {
        // Navigate to Settings.
    }
    else if (args.InvokedItemContainer())
    {
        Windows::UI::Xaml::Interop::TypeName pageTypeName;
        pageTypeName.Name = unbox_value<hstring>(args.InvokedItemContainer().Tag());
        pageTypeName.Kind = Windows::UI::Xaml::Interop::TypeKind::Primitive;
        ContentFrame().Navigate(pageTypeName, nullptr);
    }
}
```

## <a name="navigation-view-customization"></a>瀏覽檢視自訂

### <a name="pane-backgrounds"></a>窗格的背景

根據預設，NavigationView 窗格會使用不同的背景，根據的顯示模式：

- 窗格會是灰色純色展開左側的 （中左模式） 的內容並存。
- 窗格會使用應用程式內 acrylic 時開啟 （在頂端，最小，或精簡模式） 的內容上的重疊影像。

若要修改窗格背景，您可以覆寫用來呈現每個模式背景的 XAML 佈景主題資源。 （這項技術會使用而不是單一的 PaneBackground 屬性以不同的顯示模式支援不同的背景）。

下表顯示哪個佈景主題資源會在每一種顯示模式。

| 顯示模式 | 佈景主題資源 |
| ------------ | -------------- |
| Left | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| 上層 | NavigationViewTopPaneBackground |

此範例示範如何覆寫 App.xaml 中的佈景主題資源。 當您覆寫佈景主題資源時，您應該一律視需要提供最少的 「 預設 」 和 「 高對比 」 資源字典，字典 「 亮色調 」 或 「 暗色調 」 資源。 如需詳細資訊，請參閱 < [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)。

> [!IMPORTANT]
> 此程式碼示範如何使用[Windows 的 UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)AcrylicBrush 版本。 如果您改為使用 AcrylicBrush 的平台版本，您的應用程式專案的最小版本必須是 16299 或更新版本的 SDK。 若要使用的平台版本，移除所有參考`muxm:`。

```xaml
<Application
    <!-- ... -->
    xmlns:muxm="using:Microsoft.UI.Xaml.Media">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
                <ResourceDictionary>
                    <ResourceDictionary.ThemeDictionaries>
                        <ResourceDictionary x:Key="Default">
                            <!-- The "Default" theme dictionary is used unless a specific
                                 light, dark, or high contrast dictionary is provided. These
                                 resources should be tested with both the light and dark themes,
                                 and specific light or dark resources provided as needed. -->
                            <muxm:AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="LightSlateGray"
                                   TintOpacity=".6"/>
                            <muxm:AcrylicBrush x:Key="NavigationViewTopPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="{ThemeResource SystemAccentColor}"
                                   TintOpacity=".6"/>
                            <LinearGradientBrush x:Key="NavigationViewExpandedPaneBackground"
                                     StartPoint="0.5,0" EndPoint="0.5,1">
                                <GradientStop Color="LightSlateGray" Offset="0.0" />
                                <GradientStop Color="White" Offset="1.0" />
                            </LinearGradientBrush>
                        </ResourceDictionary>
                        <ResourceDictionary x:Key="HighContrast">
                            <!-- Always include a "HighContrast" dictionary when you override
                                 theme resources. This empty dictionary ensures that the 
                                 default high contrast resources are used when the user
                                 turns on high contrast mode. -->
                        </ResourceDictionary>
                    </ResourceDictionary.ThemeDictionaries>
                </ResourceDictionary>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

## <a name="related-topics"></a>相關主題

- [NavigationView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [主要/詳細資料](master-details.md)
- [瀏覽基本知識](../basics/navigation-basics.md)
- [Fluent 設計 UWP 概觀](/windows/apps/fluent-design-system)
