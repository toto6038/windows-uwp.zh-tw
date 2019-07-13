---
Description: NavigationView 一種調適型控制項，用於實作應用程式最上層的瀏覽模式。
title: 瀏覽檢視
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e00c9860ca2aa8661581de265fff106c45b30ab5
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319404"
---
# <a name="navigation-view"></a>瀏覽檢視

NavigationView 控制項提供您的應用程式最上層瀏覽。 其可配合各種不同的螢幕大小，並且同時支援「頂端」  和「左側」  的瀏覽樣式。

![頂端瀏覽](images/nav-view-header.png)<br/>
_瀏覽檢視支援頂端和左側瀏覽窗格或功能表_

> **平台 API**：[Windows.UI.Xaml.Controls.NavigationView 類別](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **Windows UI 程式庫 API**：[Microsoft.UI.Xaml.Controls.NavigationView 類別](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> NavigationView 的一些功能 (例如_頂端_瀏覽) 需要 Windows 10 版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本，或 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

NavigationView 是調適型瀏覽控制項，適用於：

- 提供跨應用程式一致的瀏覽體驗。
- 保留較小視窗的螢幕實際可用空間。
- 安排對於許多瀏覽類別的存取。

關於其他瀏覽模式，請參閱[瀏覽設計基本概念](../basics/navigation-basics.md)。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/NavigationView">開啟應用程式並查看 NavigationView 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>顯示模式

> PaneDisplayMode 屬性需要 Windows 10 1809 版 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本，或 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/) (英文)。

您可以使用 PaneDisplayMode 屬性來設定 NavigationView 的不同瀏覽樣式或顯示模式。

:::row:::
    :::column:::
    ### <a name="top"></a>上層
    窗格位於內容上方。</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![頂端瀏覽的範例](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

對於下列情況建議_頂端_瀏覽：

- 您有 5 個以內同樣重要的最上層瀏覽類別，以及其他任何最上層瀏覽類型，因此下拉式溢位功能表被視為較不重要。
- 您需要在螢幕上顯示所有的瀏覽選項。
- 您想要您的應用程式內容有更多空間。
- 圖示無法清楚地描述您的應用程式瀏覽類別。

:::row:::
    :::column:::
    ### <a name="left"></a>Left
    窗格會展開，並位於內容的左側。</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![展開左側瀏覽窗格的範例](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

對於下列情況建議_左側_瀏覽：

- 有 5 個至 10 個同樣重要的最上層瀏覽類別。
- 您想要瀏覽類別相當顯著，而且其他應用程式內容的空間較少。

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    窗格只會在開啟時顯示圖示，並位於內容的左側。</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![精簡左側瀏覽窗格的範例](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    只有在窗格開啟時，功能表按鈕才會顯示。 這開啟時位於內容的左側。</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![基本左側瀏覽窗格的範例](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>自動

PaneDisplayMode 預設設定為 [自動]。在自動模式中，瀏覽檢視會切換為 LeftMinimal (視窗變窄時)、LeftCompact 和 Left (視窗變寬時)。 如需詳細資訊，請參閱[調適型行為](#adaptive-behavior)一節。

![左側瀏覽預設調適型行為](images/displaymode-auto.png)<br/>
_瀏覽檢視預設調適型行為_

## <a name="anatomy"></a>結構

這些圖片顯示對於_頂端_或_左側_瀏覽設定時窗格、頁首和控制項內容區域的配置。

![頂端瀏覽檢視配置](images/topnav-anatomy.png)<br/>
_頂端瀏覽配置_

![左側瀏覽檢視配置](images/leftnav-anatomy.png)<br/>
_左側瀏覽配置_

### <a name="pane"></a>窗格

您可以使用 PaneDisplayMode 屬性，將窗格放置於內容上方或內容左側。

NavigationView 窗格可以包含：

- [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem) 物件。 瀏覽至特定頁面的瀏覽項目。
- [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator) 物件。 瀏覽項目分組的分隔符號。 設定[不透明度](/uwp/api/windows.ui.xaml.uielement.opacity)屬性設為 0 會將分隔符號呈現為空格。
- [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader) 物件。 標記項目群組的標頭。
- 選用的 [AutoSuggestBox](auto-suggest-box.md) 控制項，允許進行應用程式層級搜尋。 將控制項指派為 [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox) 屬性。
- [應用程式設定](../app-settings/app-settings-and-data.md)的選擇性進入點。 若要隱藏設定項目，請將 [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) 屬性設定為 **false**。

左窗格也包含：

- 切換開啟和關閉窗格的功能表按鈕。 在較大的應用程式視窗上，當窗格開啟時，您可以選擇使用 [IsPaneToggleButtonVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible) 屬性隱藏此按鈕。

瀏覽檢視有放置在窗格左上角的返回按鈕。 不過，這會自動處理向後瀏覽，並將內容新增至返回堆疊。 若要啟用向後瀏覽，請參閱[向後瀏覽](#backwards-navigation)一節。

以下是頂端和左側窗格位置的詳細的窗格結構。

#### <a name="top-navigation-pane"></a>頂端瀏覽窗格

![瀏覽檢視頂端窗格結構](images/navview-pane-anatomy-horizontal.png)

1. 標頭
1. 瀏覽項目
1. 分隔符號
1. AutoSuggestBox (選用)
1. 設定按鈕 (選用)

#### <a name="left-navigation-pane"></a>左側瀏覽窗格

![瀏覽檢視左側窗格結構](images/navview-pane-anatomy-vertical.png)

1. 功能表按鈕
1. 瀏覽項目
1. 分隔符號
1. 標頭
1. AutoSuggestBox (選用)
1. 設定按鈕 (選用)

#### <a name="pane-footer"></a>窗格頁尾

您可以將自由格式內容新增至 [PaneFooter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter) 屬性，在窗格頁尾中放置自由格式內容。

:::row:::
    :::column:::
    ![窗格頁尾頂端瀏覽](images/navview-freeform-footer-top.png)<br>
     _頂端窗格頁尾_<br>
    :::column-end:::
    :::column:::
    ![窗格頁尾左側瀏覽](images/navview-freeform-footer-left.png)<br>
    _左側窗格頁尾_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>窗格標題和頁首

您也可以設定 [PaneTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle) 屬性，在窗格標頭區域中放置文字內容。 其會使用字串，並顯示功能表按鈕旁邊的文字。

若要新增非文字內容，例如圖片或標誌，您可以將任何元素新增至 [PaneHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader) 屬性，以便置於窗格的頁首。

如果已設定 PaneTitle 和 PaneHeader，內容會在功能表按鈕旁邊水平堆疊，而且 PaneTitle 最接近功能表按鈕。

:::row:::
    :::column:::
    ![窗格頁首頂端瀏覽](images/navview-freeform-header-top.png)<br>
     _頂端窗格頁首_<br>
    :::column-end:::
    :::column:::
    ![窗格頁首左側瀏覽](images/navview-freeform-header-left.png)<br>
    _左側窗格頁首_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>窗格內容

您可以將自由格式內容新增至 [PaneCustomContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent) 屬性，在窗格中放置自由格式內容。

:::row:::
    :::column:::
    ![窗格自訂內容頂端瀏覽](images/navview-freeform-pane-top.png)<br>
     _頂端窗格自訂內容_<br>
    :::column-end:::
    :::column:::
    ![窗格自訂內容左側瀏覽](images/navview-freeform-pane-left.png)<br>
    _左側窗格自訂內容_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>標頭

您可以設定 [Header](/uwp/api/windows.ui.xaml.controls.navigationview.header) 屬性新增頁面標題。

![瀏覽檢視頁首區域的範例](images/nav-header.png)<br/>
_瀏覽檢視頁首_

頁首區域與左側窗格位置的導覽按鈕垂直對齊，而且導覽按鈕在左側窗格位置，並位於頂端窗格位置中的窗格下方。 這有 52 px 的固定高度。 其目的是保留所選瀏覽類別的頁面標題。 頁首停駐在頁面頂端，並且做為內容區域的捲動裁剪點。

頁首始終顯示，NavigationView 為基本顯示模式。 您可以選擇在其他用於較大視窗寬度的模式下隱藏頁首。 若要隱藏頁首，請將 [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) 屬性設定為 **false**。

### <a name="content"></a>內容

![瀏覽檢視內容區域的範例](images/nav-content.png)<br/>
_瀏覽檢視內容_

內容區域是顯示所選瀏覽類別大部分資訊的位置。

當 NavigationView 處於 [基本]  模式時，建議您在內容區域使用 12px 邊界，若為其他模式則使用 24px 邊界。

## <a name="adaptive-behavior"></a>調適性行為

瀏覽檢視預設會根據其可用的螢幕空間量自動變更顯示模式。 [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) 和 [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) 屬性會指定顯示模式變更的中斷點。 您可以修改這些值，以自訂調適性顯示模式行為。

### <a name="default"></a>預設值

PaneDisplayMode 是設定為 **Auto** 的預設值，調適性行為會顯示：

- 大型視窗寬度 (1008px 或更大) 的展開左側窗格。
- 中等視窗寬度 (641px 至 1007px) 的左側僅圖示瀏覽窗格 (LeftCompact)。
- 小型視窗寬度 (640px 或更小) 的僅功能表按鈕 (LeftMinimal)。

如需調適性行為視窗大小的詳細資訊，請參閱[螢幕大小和中斷點](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

![左側瀏覽預設調適型行為](images/displaymode-auto.png)<br/>
_瀏覽檢視預設調適型行為_

### <a name="minimal"></a>基本

第二個常見的調適性模式是使用大型視窗寬度的展開左側窗格，以及中等和小型視窗寬度的功能表按鈕。

我們建議在下列情況時使用：

- 您想要較小視窗寬度的應用程式內容有更多空間。
- 您的瀏覽類別無法以圖示清楚表示。

![左側瀏覽基本調適型行為](images/adaptive-behavior-minimal.png)<br/>
_瀏覽檢視「基本」調適型行為_

若要設定此行為，請將 CompactModeThresholdWidth 設訂為想要窗格摺疊的寬度。 在其中，這是從預設的 640 變更為 1007。 您也應該設定 ExpandedModeThresholdWidth 以確保值不衝突。

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>精簡

第三個常見的調適性模式是使用大型視窗寬度的展開左側窗格，以及中等和小型視窗寬度的 LeftCompact 僅圖示瀏覽窗格。

我們建議在下列情況時使用：

- 務必在螢幕上顯示所有瀏覽選項。
- 您的瀏覽類別能夠以圖示清楚表示。

![左側瀏覽精簡調適型行為](images/adaptive-behavior-compact.png)<br/>
_瀏覽檢視「精簡」調適型行為_

若要設定此行為，請將 CompactModeThresholdWidth 設定為 0。

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>無調適性行為

若要停用調適性行為，請將 PaneDisplayMode 設定為 Auto 以外的值。在其中，這是設定為 LeftMinimal，因此，無論視窗的寬度為何，功能表按鈕都會顯示。

![左側瀏覽無調適型行為](images/adaptive-behavior-none.png)<br/>
_PaneDisplayMode 設定為 LeftMinimal 的瀏覽檢視_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

如先前在_顯示模式_小節中所述，您可以設定窗格始終在頂端、始終展開、始終精簡，或始終基本。 您也可以自己的應用程式程式碼中自行管理顯示模式。 下一節將顯示這個的範例。

### <a name="top-to-left-navigation"></a>頂端至左側瀏覽

您在應用程式中使用頂端瀏覽時，瀏覽項目會在視窗寬度減少時摺疊為溢位功能表。 應用程式視窗變窄時，將 PaneDisplayMode 從 Top 切換為 LeftMinimal 瀏覽，而不是將所有項目摺疊為溢位功能表，可提供較佳的使用者體驗。

在下列情況時，建議對於大型視窗大小使用頂端瀏覽，並對於小型視窗大小使用左側瀏覽：

- 有一組同樣重要的最上層瀏覽類別需要一起顯示，以便在此組中的一個類別無法在畫面上完整顯示時，可以摺疊為左側瀏覽來呈現同等重要性。
- 您想要對於小型視窗大小盡可能保留更多內容空間。

此範例顯示如何使用 [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) 和 [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) 切換頂端和 LeftMinimal 瀏覽屬性。

![頂端或左側調適性行為 1 的範例](images/navigation-top-to-left.png)

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
> 您使用 AdaptiveTrigger.MinWindowWidth 時，在視窗超出指定最小寬度的情況下，會觸發視覺狀態。 這表示預設 XAML 定義窄視窗，而且 VisualState 定義視窗變寬時套用的修改。 瀏覽檢視的預設 PaneDisplayMode 為 Auto，因此，視窗寬度小於或等於 CompactModeThresholdWidth 時，會使用 LeftMinimal 瀏覽。 視窗變寬時，VisualState 會覆寫預設值，並使用頂端瀏覽。

## <a name="navigation"></a>瀏覽

瀏覽檢視不會自動執行任何瀏覽工作。 使用者點選瀏覽項目時，瀏覽檢視會將該項目顯示為已選取，並引發 [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked) 事件。 如果點選的動作導致不斷選取到某個新項目，也會引發 [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) 事件。

您可以處理任何一個事件來執行與要求的瀏覽相關的工作。 您應該處理哪一個取決於您的應用程式所需的行為。 一般而言，您會瀏覽至要求的頁面，並更新瀏覽檢視頁首，以回應這些事件。

只要使用者點選瀏覽項目，即使是已選取的瀏覽項目，都會引發 **ItemInvoked**。 (也可以使用滑鼠、鍵盤或其他輸入，透過對等的動作叫用項目。 如需詳細資訊，請參閱[輸入和互動](../input/index.md)。)如果您在 ItemInvoked 處理常式中瀏覽，預設將重新載入頁面，而且重複的項目將新增至瀏覽堆疊。 如果您在叫用項目時瀏覽，您應該不允許重新載入頁面，或確定頁面重新載入時並未在瀏覽 backstack 中建立重複項目。 (參考程式碼範例。)

**SelectionChanged** 可由使用者叫用目前未選取的項目來引發，也可透過程式設計方式變更選取的項目來引發。 如果由於使用者叫用項目而發生選取變更，會先發生 ItemInvoked 事件。 如果以程式設計方式進行選取變更，不會引發 ItemInvoked。

### <a name="backwards-navigation"></a>向後瀏覽

NavigationView 有內建的返回按鈕；但是，和向前瀏覽一樣，這不會自動執行向後瀏覽。 使用者點選返回按鈕時，會引發 [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) 事件。 您可以處理這個事件來執行向後瀏覽。 如需詳細資訊和程式碼範例，請參閱[瀏覽歷程記錄和向後瀏覽](../basics/navigation-history-and-backwards-navigation.md)。

在基本或精簡模式中，瀏覽檢視窗格是以飛出視窗開啟。 在此情況下，按一下返回按鈕會關閉窗格，並引發 **PaneClosing** 事件。

您可以設定這些屬性來隱藏或停用返回按鈕：

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible)：用來顯示和隱藏返回按鈕。 這個屬性會使用 [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible)列舉型別的值，並且預設設定為 **Auto**。 按鈕摺疊時，不會在配置中保留按鈕的空間。
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled)：用來啟用或停用返回按鈕。 您可以利用資料繫結將此屬性繫結至瀏覽畫面的 [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) 屬性。 如果 **IsBackEnabled** 是 **false**，不會引發 **BackRequested**。

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

此範例會示範如何對於大型視窗大小的頂端瀏覽窗格和小型視窗大小的左側瀏覽窗格使用 NavigationView。 這可移除 VisualStateManager 中的 _top_ 瀏覽設定，而調適為僅左側瀏覽。

此範例示範對於許多常見情況設定適用瀏覽資料可用的建議方式。 這也會示範如何使用 NavigationView 的返回按鈕和鍵盤瀏覽來實作向後瀏覽。

此程式碼假設您的應用程式包含將瀏覽的頁面，這些頁面的名稱如下：_HomePage_、_AppsPage_、_GamesPage_、_MusicPage_、_MyContentPage_ 和 _SettingsPage_。 這些頁面的程式碼不會顯示。

> [!IMPORTANT]
> 應用程式頁面的資訊會儲存在 [ValueTuple](https://docs.microsoft.com/dotnet/api/system.valuetuple) 中。 此結構會要求應用程式專案的最小版本必須是 SDK 17763 或以上。 如果您使用 NavigationView 的 WinUI 版本鎖定舊版 Windows 10 為目標，您可以改為使用 [System.ValueTuple NuGet 套件](https://www.nuget.org/packages/System.ValueTuple/)。

> [!IMPORTANT]
> 此程式碼顯示如何使用 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)版本的 NavigationView。 如果您改為使用平台版本的 NavigationView，應用程式專案的最小版本必須是 SDK 17763 或以上。 若要使用平台版本，請移除 `muxc:` 的所有參考。

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
> 此程式碼顯示如何使用 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)版本的 NavigationView。 如果您改為使用平台版本的 NavigationView，應用程式專案的最小版本必須是 SDK 17763 或以上。 若要使用平台版本，請移除 `muxc` 的所有參考。

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

以下是上述 C# 程式碼範例的 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/index) 版 **NavView_ItemInvoked** 處理常式。 C++/WinRT 處理常式的技巧需要先儲存 (在 [**NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem) 的標籤中) 要瀏覽的頁面所用的完整類型名稱。 在處理常式中，您會對該值進行 unbox 處理，將它變成 [**Windows::UI::Xaml::Interop::TypeName**](/uwp/api/windows.ui.xaml.interop.typename) 物件，並使用它來瀏覽至目的地頁面。 不需要 C# 範例中名稱為 `_pages` 的對應變數，而且您將能夠建立單元測試，確認標記內的值屬於有效類型。 另請參閱[使用 C++/WinRT，Boxing 和 unboxing 純量數值到 IInspectable](/windows/uwp/cpp-and-winrt-apis/boxing)。

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

### <a name="pane-backgrounds"></a>窗格背景

根據顯示模式，NavigationView 窗格預設使用不同的背景：

- 窗格在左側展開而與內容並排時 (在左側模式中)，會顯示純灰色。
- 窗格會在開始時使用應用程式內壓克力做為內容頂端的重疊 (在頂端、基本或精簡模式中)。

若要修改窗格背景，您可以覆寫轉譯每個模式的背景所用的 XAML 佈景主題資源。 (會使用這項技巧，而非使用單獨的 PaneBackground 屬性，以支援不同的顯示模式下的不同背景。)

下表顯示會在每個顯示模式中使用哪個佈景主題資源。

| 顯示模式 | 佈景主題資源 |
| ------------ | -------------- |
| Left | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| 上層 | NavigationViewTopPaneBackground |

此範例示範如何覆寫 App.xaml 中的佈景主題資源。 您覆寫佈景主題資源時，應該至少一律提供「預設」和「高對比」資源字典，並且視需要提供「亮色調」或「暗色調」資源的字典。 如需詳細資訊，請參閱 [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)。

> [!IMPORTANT]
> 此程式碼顯示如何使用 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)版本的 AcrylicBrush。 如果您改為使用平台版本的 AcrylicBrush，應用程式專案的最小版本必須是 SDK 16299 或以上。 若要使用平台版本，請移除 `muxm:` 的所有參考。

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
- [適用於 UWP 的 Fluent Design 概觀](/windows/apps/fluent-design-system)
