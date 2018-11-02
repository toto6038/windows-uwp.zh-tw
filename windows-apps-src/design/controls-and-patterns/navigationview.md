---
author: jwmsft
Description: NavigationView is an adaptive control that implements top-level navigation patterns for your app.
title: 瀏覽檢視
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8503c8cde942129c4e7ad6994afb6cd9b29f19a1
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5969656"
---
# <a name="navigation-view"></a>瀏覽檢視

NavigationView 控制項提供最上層瀏覽您的應用程式。 它可隨不同的螢幕大小，並支援_頂端_和_左_瀏覽樣式。

![頂端瀏覽](images/nav-view-header.png)<br/>
_瀏覽檢視支援頂端和左瀏覽窗格或功能表_

> **平台 Api**: [Windows.UI.Xaml.Controls.NavigationView 類別](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **Windows UI 程式庫 Api**: [Microsoft.UI.Xaml.Controls.NavigationView 類別](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> 某些功能的 NavigationView，例如_頂端_瀏覽，需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本，或[Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

NavigationView 是調適型瀏覽控制項，可適用於：

- 提供一致的瀏覽體驗，在您的 app。
- 保留較小的 windows 上的螢幕實際可用空間。
- 組織許多瀏覽類別存取。

其他瀏覽模式，請參閱[瀏覽設計基本知識](../basics/navigation-basics.md)。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/NavigationView">開啟應用程式並查看 NavigationView 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>顯示模式

> PaneDisplayMode 屬性需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本，或[Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

您可以使用 PaneDisplayMode 屬性設定不同的瀏覽樣式或 NavigationView 顯示模式。

:::row:::
    :::column:::
    ### <a name="top"></a>Top
    窗格會位於內容上方。</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![頂端瀏覽的範例](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

我們建議_頂端_瀏覽時：

- 您有 5 或較少的最上層瀏覽類別的同樣重要的是及最後會在下拉式清單溢位功能表中的類別會視為較不重要的任何其他最上層瀏覽。
- 您需要顯示在螢幕上的所有瀏覽選項。
- 您想要您的應用程式的內容更多空間。
- 圖示不能清楚說明您的應用程式瀏覽類別。

:::row:::
    :::column:::
    ### <a name="left"></a>向左
    窗格會展開並放置在左邊的內容。</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![展開左側瀏覽窗格範例](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

我們建議_左側_瀏覽時：

- 您有 5-10 同樣重要的最上層瀏覽類別。
- 您想要瀏覽類別是非常重要，與其他應用程式內容的空間較少。

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    窗格顯示只有圖示直到開啟且位於左邊的內容。</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![精簡的左瀏覽窗格的範例](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    僅 [功能表] 按鈕會顯示，直到窗格已開啟。 開啟時，它位於左邊的內容。</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![最少的左瀏覽窗格的範例](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Auto

根據預設，PaneDisplayMode 是設定為 Auto。在自動模式中，瀏覽檢視的適應 LeftMinimal 時的視窗較窄到 LeftCompact，之間，然後離開當視窗變寬。 如需詳細資訊，請參閱[調適型行為](#adaptive-behavior)。

![左側瀏覽預設的調適型行為](images/displaymode-auto.png)<br/>
_瀏覽檢視預設的調適型行為_

## <a name="anatomy"></a>結構

這些影像顯示窗格、 標頭，以及控制項的_頂端_或_左_瀏覽設定時的內容區域的版面配置。

![頂端瀏覽檢視版面配置](images/topnav-anatomy.png)<br/>
_頂端瀏覽版面配置_

![左側瀏覽檢視版面配置](images/leftnav-anatomy.png)<br/>
_左側瀏覽版面配置_

### <a name="pane"></a>窗格

您可以使用 PaneDisplayMode 屬性來放置內容上方，或在內容左邊的窗格。

NavigationView 窗格可以包含：

- [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem)物件。 用於瀏覽至特定頁面瀏覽項目。
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)物件。 適用於將瀏覽項目分組分隔符號。 設定為 0，以將轉譯的空間為分隔符號的[Opacity](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator.opacity)屬性。
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader)物件。 標頭的項目群組加上標籤。
- 選用的[AutoSuggestBox](auto-suggest-box.md)控制項，允許進行應用程式層級搜尋。 將控制項指派給[NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox)屬性。
- [應用程式設定](../app-settings/app-settings-and-data.md)的選擇性進入點。 若要隱藏設定項目，請將[IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible)屬性為**false**。

左的窗格中也包含：

- 若要切換開啟和關閉窗格功能表按鈕。 在較大的應用程式視窗上，當窗格開啟時，您可以選擇使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 屬性隱藏此按鈕。

瀏覽檢視有放在窗格的左上角的返回按鈕。 不過，它會不會自動處理向後瀏覽，並將內容新增至返回堆疊。 若要啟用向後瀏覽，請參閱[向後瀏覽](#backwards-navigation)一節。

以下是詳細的窗格結構，如上方與左邊的窗格位置。

#### <a name="top-navigation-pane"></a>上方瀏覽窗格

![瀏覽檢視上方窗格構造](images/navview-pane-anatomy-horizontal.png)

1. 標頭
1. 瀏覽項目
1. 分隔符號
1. AutoSuggestBox （選擇性）
1. 設定按鈕 （選擇性）

#### <a name="left-navigation-pane"></a>左瀏覽窗格

![瀏覽檢視左窗格構造](images/navview-pane-anatomy-vertical.png)

1. 功能表按鈕
1. 瀏覽項目
1. 分隔符號
1. 標頭
1. AutoSuggestBox （選擇性）
1. 設定按鈕 （選擇性）

#### <a name="pane-footer"></a>窗格頁尾

您可以自由格式內容中放置在窗格頁尾藉由將它新增到[PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter)屬性。

:::row:::
    :::column:::
    ![窗格頁尾上方瀏覽](images/navview-freeform-footer-top.png)<br>
     _上方窗格頁尾_<br>
    :::column-end:::
    :::column:::
    ![窗格頁尾的左瀏覽](images/navview-freeform-footer-left.png)<br>
    _左的窗格頁尾_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>窗格的標題和標頭

您可以藉由設定[PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle)屬性，窗格標頭區域中放置文字內容。 它採用字串，並顯示在 [功能表] 按鈕旁邊的文字。

若要新增非文字內容，例如影像或商標，您可以藉由將它新增到[PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader)屬性窗格的標頭中放置任何項目。

如果同時 PaneTitle 和 PaneHeader 設，內容是水平堆疊 [功能表] 按鈕，使用最接近的功能表按鈕 PaneTitle 旁邊。

:::row:::
    :::column:::
    ![窗格標頭上方瀏覽](images/navview-freeform-header-top.png)<br>
     _上方窗格標頭_<br>
    :::column-end:::
    :::column:::
    ![窗格標頭的左瀏覽](images/navview-freeform-header-left.png)<br>
    _左的窗格中的標頭_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>窗格的內容

您可以放置在窗格中的自由格式內容，藉由將它新增到[PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent)屬性。

:::row:::
    :::column:::
    ![窗格自訂內容上方瀏覽](images/navview-freeform-pane-top.png)<br>
     _上方窗格自訂內容_<br>
    :::column-end:::
    :::column:::
    ![左瀏覽窗格中自訂內容](images/navview-freeform-pane-left.png)<br>
    _左的窗格中自訂內容_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>標頭

您可以藉由設定[Header](/uwp/api/windows.ui.xaml.controls.navigationview.header)屬性新增頁面標題。

![瀏覽檢視頁首區域的範例](images/nav-header.png)<br/>
_瀏覽檢視的標頭_

頁首區域與在左的窗格中的位置，瀏覽按鈕垂直對齊，並且落在上方窗格位置窗格下方。 它具有高度固定為 52 px。 其目的是保留所選瀏覽類別的頁面標題。 頁首停駐在頁面頂端，並且做為內容區域的捲動裁剪點。

標頭是可見的任何時候 NavigationView 處於最少的顯示模式。 您可以選擇在其他用於較大視窗寬度的模式下隱藏頁首。 若要隱藏標頭，請將[AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader)屬性為**false**。

### <a name="content"></a>內容

![瀏覽檢視內容區域的範例](images/nav-content.png)<br/>
_瀏覽檢視內容_

內容區域是顯示所選瀏覽類別大部分資訊的位置。

建議您當 NavigationView 處於 [**基本**] 模式時的內容區域使用 12px 邊界，則使用 24px 邊界。

## <a name="adaptive-behavior"></a>調適性行為

根據預設，瀏覽檢視其自動變更顯示模式根據可用螢幕空間量。 [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth)與[ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth)屬性指定的顯示模式變更的中斷點。 您可以修改這些值才能自訂調適型的顯示模式行為。

### <a name="default"></a>預設值

當 PaneDisplayMode 設定為**自動**其預設值時，調適型行為是顯示：

- 展開左的窗格中較大的視窗寬度 (1008px 或更高)。
- A 向左鍵、 圖示僅限，瀏覽窗格 (LeftCompact) 上的中型視窗寬度下 (641px 到 1007px)。
- 只有功能表上的按鈕 (LeftMinimal) 小的視窗寬度 (640px 或更少)。

如需有關調適型行為的視窗大小的詳細資訊，請參閱[螢幕大小與中斷點](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

![左側瀏覽預設的調適型行為](images/displaymode-auto.png)<br/>
_瀏覽檢視預設的調適型行為_

### <a name="minimal"></a>基本

第二個常見的調適型模式是大型的視窗寬度，與只有這兩個中型和小型視窗寬度下上的功能表按鈕上使用展開左的窗格中。

我們建議您時，這個：

- 您想要更多空間較小的視窗寬度的應用程式內容。
- 您的瀏覽類別無法使用圖示清楚表示。

![左瀏覽最少的調適型行為](images/adaptive-behavior-minimal.png)<br/>
_瀏覽檢視的 「 最低 」 調適型行為_

若要設定此行為，將 CompactModeThresholdWidth 為想摺疊窗格的寬度。 在這裡，它會變更的 640 到 1007年的預設值。 您也應該設定 ExpandedModeThresholdWidth 以確保值不會發生衝突。

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>精簡

第三個常見的調適型模式是在大型的視窗寬度和 LeftCompact，圖示僅限，在這兩個中型和小型視窗寬度下瀏覽窗格上使用展開左的窗格中。

我們建議您時，這個：

- 請務必一律顯示在螢幕上的所有瀏覽選項。
- 具有圖示可以清楚表示您的瀏覽類別。

![左瀏覽精簡的調適型行為](images/adaptive-behavior-compact.png)<br/>
_瀏覽檢視 「 精簡 」 調適型行為_

若要設定此行為，將 CompactModeThresholdWidth 為 0。

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>沒有任何調適性行為

若要停用自動調整的行為，將 PaneDisplayMode 為自動以外的值。在這裡，它會設定到 LeftMinimal，，只讓 [功能表] 按鈕會顯示不論視窗寬度。

![不左瀏覽任何調適性行為](images/adaptive-behavior-none.png)<br/>
_使用設為 LeftMinimal PaneDisplayMode 瀏覽檢視_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

先前_的顯示模式_一節所說明，您可以設定為一律在頂端、 一律展開、 一律精簡，或一律最少的窗格。 您也可以管理的顯示模式自行在您的應用程式程式碼。 這個範例會顯示在下一節。

### <a name="top-to-left-navigation"></a>頂端左瀏覽

當您使用頂端瀏覽您的應用程式中時，瀏覽項目摺疊到溢位功能表為視窗寬度會減少。 當您的應用程式的視窗較窄時，它可以提供更好的使用者體驗，以切換從上方 PaneDisplayMode LeftMinimal 瀏覽，而不是讓所有項目摺疊到溢位功能表。

我們建議使用頂端瀏覽大型視窗大小，並在小型的左側瀏覽視窗調整大小的時機：

- 您有一組同樣重要的最上層瀏覽類別，以在一起，顯示，因此在此設定中的一個類別無法容納整個畫面上，如果您摺疊來授與他們同等的左瀏覽。
- 您想要保留儘可能在小型視窗大小的內容更空間。

這個範例示範如何使用[VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager)和[AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth)屬性頂端和 LeftMinimal 瀏覽之間切換。

![頂端或左邊調適型行為 1 的範例](images/navigation-top-to-left.png)

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
> 當您使用 AdaptiveTrigger.MinWindowWidth 時，當視窗寬度大於指定最小寬度時，會觸發視覺狀態。 這表示預設 XAML 定義的窄型視窗中，而 VisualState 定義當視窗變寬時套用的修改。 預設 PaneDisplayMode 瀏覽檢視為自動，因此當視窗寬度小於或等於 CompactModeThresholdWidth，LeftMinimal 瀏覽時使用。 當視窗取得更寬時，VisualState 會覆寫預設值，並用頂端瀏覽。

## <a name="navigation"></a>瀏覽

瀏覽檢視不會自動執行的任何瀏覽工作。 當使用者點選瀏覽項目上時，瀏覽檢視會顯示為已選取的項目，並引發[ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked)事件。 如果輕觸造成所選的新項目，則也會引發[SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged)事件。

您可以處理任一個事件，以執行所要求的瀏覽相關工作。 哪一個，您應該處理取決於您想要針對您的應用程式的行為。 一般而言，您會瀏覽到要求的頁面，並更新瀏覽檢視標頭，以回應這些事件。

**ItemInvoked**會引發任何時候，在使用者點選瀏覽項目，即使已選取。 （在項目也可叫用與使用滑鼠、 鍵盤或其他輸入對等的動作。 如需詳細資訊，請參閱[輸入和互動](../input/index.md)）。如果您瀏覽 ItemInvoked 處理常式中，根據預設，將會重新載入頁面，並重複的項目會新增至瀏覽堆疊。 如果您瀏覽項目叫用時，您應該禁止重新載入頁面，或確保，重複的項目不建立瀏覽上一頁堆疊中重新載入頁面時。 （請參閱程式碼範例）。

由使用者叫用不是目前所選取項目，或以程式設計方式變更選取的項目，就會引發**SelectionChanged** 。 如果使用者叫用項目，就會發生的選取項目變更，ItemInvoked 事件發生第一次。 如果選取項目變更是以程式設計方式，將不會引發 ItemInvoked。

### <a name="backwards-navigation"></a>向後瀏覽

NavigationView 具有內建的返回按鈕;但是，如同向前瀏覽，它不會向後瀏覽自動執行。 當使用者點選 [上一頁] 按鈕時，會引發[BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested)事件。 您處理此事件來執行向後瀏覽。 如需詳細資訊和程式碼範例，請參閱[瀏覽歷程記錄和向後瀏覽](../basics/navigation-history-and-backwards-navigation.md)。

在 Minimal 或 Compact 模式中，瀏覽檢視窗格是飛出視窗開啟。 在此情況下，按一下 [上一頁] 按鈕會關閉窗格，並改為引發**PaneClosing**事件。

您可以隱藏或停用 [上一頁] 按鈕設定這些屬性：

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible)： 用來顯示和隱藏 [上一頁] 按鈕。 這個屬性會接受[NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible)列舉的值，預設會設定為**Auto** 。 摺疊按鈕時, 沒有空間是配置中保留給它。
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled)： 用來啟用或停用 [上一頁] 按鈕。 您可以資料繫結至您的瀏覽框架的[CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback)屬性這個屬性。 如果**IsBackEnabled**為**false**，就不會引發**BackRequested** 。

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

這個範例示範如何使用 NavigationView 與大型視窗大小的頂端瀏覽窗格和左瀏覽窗格中的，在小型視窗大小。 它可以透過 VisualStateManager 中移除的_頂端_瀏覽設定適用於僅限左側的瀏覽。

此範例示範設定適用於許多常見的案例的瀏覽資料的建議的方式。 它也會示範如何實作向後瀏覽使用 NavigationView 的返回按鈕和鍵盤瀏覽。

此程式碼假設您的應用程式包含頁面瀏覽至下列名稱：_首頁_、 _AppsPage_、 _GamesPage_、 _MusicPage_、 _MyContentPage_，以及_SettingsPage_。 這些頁面的程式碼不會顯示。

> [!IMPORTANT]
> 應用程式的網頁的資訊會儲存在[ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple)。 此結構需要為您的應用程式專案的最低版本必須是 SDK 17763 或更高。 如果您使用的 NavigationView WinUI 版本為目標較舊版本的 Windows 10，您可以改為使用[System.ValueTuple NuGet 套件](https://www.nuget.org/packages/System.ValueTuple/)。

> [!IMPORTANT]
> 這個程式碼說明如何使用 NavigationView 的[Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)版本。 如果您改為使用 NavigationView 的平台版本，為您的應用程式專案的最低版本必須是 SDK 17763 或更高。 若要使用平台版本，移除所有參考`muxc:`。

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
> 這個程式碼說明如何使用 NavigationView 的[Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)版本。 如果您改為使用 NavigationView 的平台版本，為您的應用程式專案的最低版本必須是 SDK 17763 或更高。 若要使用平台版本，移除所有參考`muxc`。

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

## <a name="related-topics"></a>相關主題

- [NavigationView 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)
- [主要/詳細資料](master-details.md)
- [瀏覽基本知識](../basics/navigation-basics.md)
- [適用於 UWP 的 Fluent Design 概觀](../fluent-design-system/index.md)