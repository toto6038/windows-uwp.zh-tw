---
Description: Design your app so that it looks good and functions well on your television.
title: 針對 Xbox 和電視進行設計
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
keywords: Xbox, 電視, 10 英呎體驗, 遊戲台, 遙控器, 輸入, 互動
ms.date: 11/13/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 431b8912e43647bc2678aaab7efc9ec68b866d10
ms.sourcegitcommit: ff131135248c85a8a2542fc55437099d549cfaa5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "9117578"
---
# <a name="designing-for-xbox-and-tv"></a>針對 Xbox 和電視進行設計

設計您的通用 Windows 平台 (UWP) 應用程式，讓它在 Xbox One 及電視螢幕上看起來既美觀又能正常運作。

如需指導方針在*10 英呎*體驗中的 UWP 應用程式的互動體驗，請參閱[遊戲台與遙控器的互動](../input/gamepad-and-remote-interactions.md)。

## <a name="overview"></a>概觀

通用 Windows 平台可讓您創造跨多個 Windows10 裝置的絕佳體驗。
UWP 架構提供的大部分功能可讓 app 在這些裝置上使用相同的使用者介面 (UI)，無需進行額外的工作。
不過，如果要量身打造並提供最佳化的 app 以便在 Xbox One 和電視螢幕上運作良好，則需要特殊考量。

坐在房間一端的沙發上，使用遊戲台或遙控器與電視互動的體驗，稱為 **「10 英呎體驗」**。
這個名稱的由來是因為使用者通常坐在離螢幕大約 10 英呎遠的位置。
這是一個獨特的挑戰，因為我們不會稱與電腦互動是 *2 英呎*體驗。
如果您為 Xbox One 或其他輸出至電視螢幕的裝置開發 app，並使用控制器做為輸入，您就必須記住這一點。

您並非需要本篇文章中所有的步驟，才能讓 app 創造出 10 英呎體驗，但是了解這些步驟，為您的 app 做出適當的決定，可針對您的 app 特定需求，量身打造出更良好的 10 英呎體驗。
當您打算在 10 英呎環境中運作您的 app 時，請考慮下列設計原則。

### <a name="simple"></a>簡單

針對 10 英呎環境的設計會產生一些獨特的挑戰。 解析度和檢視距離會讓人們很難處理太多資訊。
請嘗試讓設計保持清晰，盡量精簡為最簡單的元件。 在電視上顯示的資訊量應該與在行動電話上 (而不是電腦上) 看到的內容差不多。

![Xbox One 主畫面](images/designing-for-tv/xbox-home-screen.png)

### <a name="coherent"></a>易懂

10 英呎環境中的 UWP app 應該直覺且易於使用。 讓焦點清楚而且易懂。
排列好內容，讓空間的移動一致而且可預測。 為使用者的動作提供最短的路徑。

![Xbox One 電影應用程式](images/designing-for-tv/xbox-movies-app.png)

_**螢幕擷取畫面中所顯示的所有電影都能在 Microsoft 電影與電視上取得。**_  

### <a name="captivating"></a>迷人

大螢幕能提供最身歷其境、類似電影的體驗。 無縫的場景、順暢的動作，以及生動活潑的色彩和排版，都能讓您的 app 提升到不同的層次。 盡量明顯而美觀。

![Xbox One 的虛擬人偶應用程式](images/designing-for-tv/xbox-avatar-app.png)

### <a name="optimizations-for-the-10-foot-experience"></a>10 英呎體驗最佳化

您現在知道了出色的 UWP app 具備 10 英呎體驗的設計原則，請仔細閱讀下列概觀，了解可最佳化您的 app 並提供絕佳使用者體驗的特定方式。

| 功能        | 描述           |
| -------------------------------------------------------------- |--------------------------------|
| [調整 UI 元素大小](#ui-element-sizing)  | 通用 Windows 平台使用[縮放與有效像素](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)，根據檢視距離來調整 UI。 了解如何調整大小並套用到整個 UI，可協助最佳化 10 英呎環境的 app。  |
|  [電視安全區域](#tv-safe-area) | UWP 預設會自動避免在電視不安全的區域 (接近螢幕邊緣的區域) 中顯示任何 UI。 不過，這會產生一種「被框住」的效果，UI 看起來就像信箱一樣。 為了讓您的 app 能真正融入電視螢幕，您要加以修改，讓 app 在支援的電視能延伸到螢幕的邊緣。 |
| [色彩](#colors)  |  UWP 支援色彩佈景主題，優先採用系統佈景主題的 app 在 Xbox One 上將會預設為 **「深色」**。 如果您的 app 有特定的色彩佈景主題，您應該考慮到有些色彩不適合電視，應盡量避免使用。 |
| [音效](../style/sound.md)    | 音效聲音在 10 英呎體驗中扮演關鍵角色，其為使用者提供身歷其境的體驗與回應。 在 Xbox One 上執行 app 時，UWP 可提供針對通用控制項自動開啟音效的功能。 深入了解關於 UWP 內建音效支援及如何善用的詳細資訊。    |
| [UI 控制項的指導方針](#guidelines-for-ui-controls)  |  提供數種可針對多部裝置良好運作的 UI 控制項，但當在電視上使用時具有特定考量。 深入了解有關針對 10 英呎體驗進行設計時使用這些控制項的一些最佳做法。 |
| [適用於 Xbox 的自訂視覺狀態觸發程序](#custom-visual-state-trigger-for-xbox) | 若要針對 10 英呎體驗量身打造您的 UWP app，建議您使用自訂的 *「視覺狀態觸發程序」*，在 App 偵測到它已在 Xbox 主機上啟動時變更配置。 |

除了上述的設計和配置考量，還有數個建置您的應用程式時，您應該考慮的[遊戲台與遙控器的互動](../input/gamepad-and-remote-interactions.md)最佳化。

| 功能        | 描述           |
| -------------------------------------------------------------- |--------------------------------|
| [XY 焦點瀏覽和互動](../input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction) | **XY 焦點瀏覽**可讓使用者四處瀏覽您的應用程式 UI。 不過，這限制使用者只能向上、向下、向左和向右瀏覽。 本節概述處理此功能和其他考量的建議。 |
| [滑鼠模式](../input/gamepad-and-remote-interactions.md#mouse-mode)|XY 焦點瀏覽不可行，或甚至可行，某些類型的應用程式，例如地圖或繪圖和繪製應用程式。 在這些情況下，**滑鼠模式**可讓使用者與遊戲台或遙控器，就像在電腦上的滑鼠自由地瀏覽。|
| [視覺焦點](../input/gamepad-and-remote-interactions.md#focus-visual)  | 焦點視覺效果是在目前取得焦點的 UI 元素會反白顯示的框線。 這可協助使用者快速找出的 UI 瀏覽或互動。  |
| [焦點佔用](../input/gamepad-and-remote-interactions.md#focus-engagement) | 焦點佔用需要使用者按**A/選取**] 按鈕上的遊戲台或遙控器，當 UI 項目有焦點時才能與它互動。 |
| [硬體按鈕](../input/gamepad-and-remote-interactions.md#hardware-buttons) | 遊戲台與遙控器提供極不同的按鈕和設定。 |

> [!NOTE]
> 本主題中大部分的程式碼片段都是以 XAML/C# 所撰寫，不過，原則和概念則適用於所有 UWP app。 如果您正在開發適用於 Xbox 的 HTML/JavaScript UWP app，請查看 GitHub 上絕佳的 [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) 媒體櫃。

## <a name="ui-element-sizing"></a>調整 UI 元素大小

因為在 10 英呎環境中的 app 使用者會使用遙控器或遊戲台，而且坐在離螢幕數英呎遠的位置，所以您的設計必須納入一些特別的 UI 考量。
請確定 UI 有適當的內容密度，也不要太雜亂，讓使用者可以輕鬆瀏覽和選取元素。 請記住︰重點是簡單。

### <a name="scale-factor-and-adaptive-layout"></a>縮放比例與調適型配置

**「縮放比例」** 有助於確保 UI 元素以適合 app 執行裝置的大小顯示。
在桌面上，您可以在 **\[設定\] &gt; \[系統\] &gt; \[顯示\]** 中找到這個設定，以滑動值表示。
手機上也有這個相同的設定 (如果裝置支援)。

![變更文字、app 與其他項目的大小](images/designing-for-tv/ui-scaling.png)

Xbox One 上沒有這類系統設定。不過，如果要適當調整 UWP UI 元素的大小以適用於電視，預設會針對 XAML app 將它們調整為 **200%**，以及針對 HTML app 調整為 **150%**。
只要 UI 元素能針對其他裝置適當調整大小，就能針對電視適當調整大小。
Xbox One 以 1080p (1920 x 1080 像素) 呈現您的 app。 因此，從電腦等其他裝置帶入 app 時，請確定採用[調適型技術](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)，以 100% 縮放比例讓 UI 呈現 960 x 540 像素的最佳外觀 (或針對 HTML app 以 100% 縮放比例呈現 1280 x 720 像素的最佳外觀)。

針對 Xbox 的設計與針對電腦的設計稍有不同，因為您只需考慮 1920 x 1080 一種解析度。
如果使用者的電視解析度比較高，就沒什麼關係&mdash;UWP app 一律會縮放至 1080p。

但無論電視解析度為何，當您的 app 在 Xbox One 上執行時，也會提取 200% (或針對 HTML app 提取 150%) 的正確資產大小。

### <a name="content-density"></a>內容密度

設計 app 時請記住，使用者會從遠距離檢視 UI 而且使用遙控器或遊戲控制器與 UI 互動，比起使用滑鼠或觸控輸入要花費更多的時間瀏覽。

#### <a name="sizes-of-ui-controls"></a>UI 控制項的大小

互動式 UI 元素的高度至少要有 32 epx (有效像素)。 這是常見 UWP 控制項的預設值，以縮放比例 200% 使用時，可確保從遠距離也能看見 UI 元素，並協助減少內容密度。

![縮放比例 100% 和 200% 的 UWP 按鈕](images/designing-for-tv/button-100-200.png)

#### <a name="number-of-clicks"></a>點選次數

使用者從電視螢幕一邊瀏覽到另一邊時，以不超過 **「點選六次」** 的原則來簡化您的 UI。 同樣地，這裡適用 **「簡單」** 的原則。 

![跨 6 個圖示](images/designing-for-tv/six-clicks.png)

### <a name="text-sizes"></a>文字大小

若要從遠距離看見您的 UI，請使用下列重要規則︰

* 主要的文字和閱讀內容︰最小 15 epx
* 非關鍵的文字和補充內容︰最小 12 epx

在 UI 中使用較大文字時，請挑選不會限制螢幕實際可用空間太多、佔用其他內容可能填入空間的大小。

### <a name="opting-out-of-scale-factor"></a>不使用縮放比例

我們建議您的 app 充分利用縮放比例支援，針對每個裝置類型進行縮放，可協助 app 在所有裝置上正確執行。
不過，您也可以不使用這個行為，以縮放比例 100% 來設計所有 UI。 請注意，您無法將縮放比例變更為 100% 以外的值。

針對 XAML app，您可以使用下列程式碼片段，選擇不使用縮放比例︰

```csharp
bool result =
    Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` 將會通知您是否成功選擇不要採用。

如需詳細資訊 (包括適用於 HTML/JavaScript 的範例程式碼)，請參閱[如何關閉縮放比例](../../xbox-apps/disable-scaling.md)。

請務必將本主題中所述的 *「有效」* 像素值加倍來得到 *「實際」* 像素值 (或針對 HTML app 乘以 1.5)，藉以計算出 UI 元素的適當大小。

## <a name="tv-safe-area"></a>電視安全區域

由於歷史上以及技術上的理由，並非所有的電視都以相同的方式顯示螢幕邊緣的內容。 UWP 預設會避免在電視不安全區域中顯示任何 UI 內容，改為只繪製頁面背景。

下列影像中，電視不安全區域以藍色區域表示。

![電視不安全區域](images/designing-for-tv/tv-unsafe-area.png)

您可以將背景設定為靜態或佈景主題色彩，或設定為影像，如以下程式碼片段示範。

### <a name="theme-color"></a>佈景主題色彩

```xml
<Page x:Class="Sample.MainPage"
      Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"/>
```

### <a name="image"></a>影像

```xml
<Page x:Class="Sample.MainPage"
      Background="\Assets\Background.png"/>
```

這是您的 app 不進行額外工作的外觀。

![電視安全區域](images/designing-for-tv/tv-safe-area.png)

這不是最佳的結果，因為它讓 app 產生「被框住」的效果，某些 UI (例如瀏覽窗格和格線) 似乎被裁切掉。 不過，您可以進行最佳化，延伸部份 UI 到螢幕邊緣，讓 app 有更多電影效果。

### <a name="drawing-ui-to-the-edge"></a>將 UI 繪製到邊緣

我們建議您使用特定 UI 元素，延伸到螢幕邊緣，讓使用者感覺更投入。 這些包括 [ScrollViewers](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)、[瀏覽窗格](../controls-and-patterns/navigationview.md)以及 [CommandBars](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)。

但同樣重要的是，互動式元素和文字則一律要避開螢幕邊緣，以確保在一些電視上不會被裁切掉。 我們建議您在螢幕邊緣的 5% 以內，只繪製不重要的視覺效果。 如[調整 UI 元素大小](#ui-element-sizing)所述，遵循 Xbox One 主機預設縮放比例 200% 的 UWP app 會使用 960 x 540 epx 的區域，所以在您的 app UI 中，您應該避免在下列區域中放置重要的 UI：

- 從頂端和底端算起 27 epx
- 從左邊和右邊算起 48 epx

下列小節說明如何將您的 UI 延伸到螢幕邊緣。

#### <a name="core-window-bounds"></a>核心視窗界限

對於只針對 10 英呎體驗的 UWP app，使用核心視窗界限是比較簡單的選項。

在 `App.xaml.cs` 的 `OnLaunched` 方法中，新增下列程式碼：

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

有了這行程式碼，app 視窗會延伸到螢幕邊緣，所以您必須將所有互動式和重要 UI 移入稍早所述的電視安全區域中。 暫時性 UI (例如操作功能表和開啟的[下拉式方塊](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)) 會自動留在電視安全區域內。

![核心視窗界限](images/designing-for-tv/core-window-bounds.png)

#### <a name="pane-backgrounds"></a>窗格背景

瀏覽窗格通常會繪製在螢幕邊緣附近，所以背景應該要延伸到電視不安全區域，以免產生不適當的間距。 若要這麼做，只要將瀏覽窗格的背景色彩變更為應用程式的背景色彩即可。

使用先前所述的核心視窗界限將可讓您將 UI 繪製在螢幕的邊緣，但是您應該接著在 [SplitView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SplitView) 的內容上使用正值邊界，以便讓它保持在電視安全區域內。

![延伸到螢幕邊緣的瀏覽窗格](images/designing-for-tv/tv-safe-areas-2.png)

現在，瀏覽窗格的背景已經延伸到螢幕邊緣，同時瀏覽項目也保留在電視安全區域內。
`SplitView` 的內容 (在此例中為項目的格線) 已經延伸到螢幕底部，因此看起來是持續往下而沒有被截斷，而格線頂端也仍然在電視安全區域內。 (若要深入了解如何執行這項操作，請參閱[捲動清單和格線的尾端](#scrolling-ends-of-lists-and-grids))。

下列程式碼片段可達到這個效果︰

```xml
<SplitView x:Name="RootSplitView"
           Margin="48,0,48,0">
    <SplitView.Pane>
        <ListView x:Name="NavMenuList"
                  ContainerContentChanging="NavMenuItemContainerContentChanging"
                  ItemContainerStyle="{StaticResource NavMenuItemContainerStyle}"
                  ItemTemplate="{StaticResource NavMenuItemTemplate}"
                  ItemInvoked="NavMenuList_ItemInvoked"
                  ItemsSource="{Binding NavMenuListItems}"/>
    </SplitView.Pane>
    <Frame x:Name="frame"
           Navigating="OnNavigatingToPage"
           Navigated="OnNavigatedToPage"/>
</SplitView>
```

[CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 是另一個常放在 app 一邊或多邊附近的窗格範例，也因此其背景應該延伸到電視螢幕邊緣。 它通常也包含一個 **\[更多\]** 按鈕 (在右邊以 "..." 代表)，此按鈕應該留在電視安全區域中。 以下是可達到所需的互動和視覺效果的幾個不同策略。

**選項 1**︰將 `CommandBar` 背景色彩變更為透明或與頁面背景相同的色彩︰

```xml
<CommandBar x:Name="topbar"
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

這麼做會讓 `CommandBar` 看起來就像位在與頁面其餘部分相同的背景上，因此背景會順暢地延伸到螢幕邊緣。

**選項 2**︰新增一個填滿色彩與 `CommandBar` 背景相同的背景矩形，並將它置於 `CommandBar` 底下且延伸到頁面的其餘部分︰

```xml
<Rectangle VerticalAlignment="Top"
            HorizontalAlignment="Stretch"      
            Fill="{ThemeResource SystemControlBackgroundChromeMediumBrush}"/>
<CommandBar x:Name="topbar"
            VerticalAlignment="Top"
            HorizontalContentAlignment="Stretch">
            ...
</CommandBar>
```

> [!NOTE]
> 如果使用這個方法，請注意 **\[更多\]** 按鈕會視需要變更所開啟之 `CommandBar` 的高度，以便在 `AppBarButton` 的圖示之下顯示其標籤。 建議您將標籤移到其圖示的 *「右側」*，以避免發生調整大小的情況。 如需詳細資訊，請參閱 [CommandBar 標籤](#commandbar-labels)。

這兩種方法也適用於本節所列的其他類型的控制項。

#### <a name="scrolling-ends-of-lists-and-grids"></a>捲動清單和格線的尾端

清單和格線經常會包含一個螢幕無法同時容納的多個項目。 發生這種情況時，我們建議您將清單或格線延伸到螢幕邊緣。 水平捲動清單和格線時應該向右邊延伸，垂直捲動時應該向下延伸。

![電視安全區域格線截斷](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

雖然清單或格線會像這樣延伸，但是請務必讓視覺焦點及其關聯的項目保留在電視安全區域內。

![捲動格線焦點應保留在電視安全區域內](images/designing-for-tv/scrolling-grid-focus.png)

UWP 具有可將視覺焦點保留在 [VisibleBounds](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.visiblebounds) 內的功能，但是您必須新增邊框間距，以確保清單/格線項目可以捲動到安全區域的視野範圍內。 具體來說，您要對 [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 或 [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 的 [ItemsPresenter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsPresenter) 新增正邊界，如下列程式碼片段所示︰

```xml
<Style x:Key="TitleSafeListViewStyle"
       TargetType="ListView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="ListView">
                <Border BorderBrush="{TemplateBinding BorderBrush}"
                        Background="{TemplateBinding Background}"
                        BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer"
                                  TabNavigation="{TemplateBinding TabNavigation}"
                                  HorizontalScrollMode="{TemplateBinding ScrollViewer.HorizontalScrollMode}"
                                  HorizontalScrollBarVisibility="{TemplateBinding ScrollViewer.HorizontalScrollBarVisibility}"
                                  IsHorizontalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsHorizontalScrollChainingEnabled}"
                                  VerticalScrollMode="{TemplateBinding ScrollViewer.VerticalScrollMode}"
                                  VerticalScrollBarVisibility="{TemplateBinding ScrollViewer.VerticalScrollBarVisibility}"
                                  IsVerticalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsVerticalScrollChainingEnabled}"
                                  IsHorizontalRailEnabled="{TemplateBinding ScrollViewer.IsHorizontalRailEnabled}"
                                  IsVerticalRailEnabled="{TemplateBinding ScrollViewer.IsVerticalRailEnabled}"
                                  ZoomMode="{TemplateBinding ScrollViewer.ZoomMode}"
                                  IsDeferredScrollingEnabled="{TemplateBinding ScrollViewer.IsDeferredScrollingEnabled}"
                                  BringIntoViewOnFocusChange="{TemplateBinding ScrollViewer.BringIntoViewOnFocusChange}"
                                  AutomationProperties.AccessibilityView="Raw">
                        <ItemsPresenter Header="{TemplateBinding Header}"
                                        HeaderTemplate="{TemplateBinding HeaderTemplate}"
                                        HeaderTransitions="{TemplateBinding HeaderTransitions}"
                                        Footer="{TemplateBinding Footer}"
                                        FooterTemplate="{TemplateBinding FooterTemplate}"
                                        FooterTransitions="{TemplateBinding FooterTransitions}"
                                        Padding="{TemplateBinding Padding}"
                                        Margin="0,27,0,27"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

您可以將先前的程式碼片段放在頁面或 App 資源中，然後再透過下列方式存取它︰

```xml
<Page>
    <Grid>
        <ListView Style="{StaticResource TitleSafeListViewStyle}"
                  ... />
```

> [!NOTE]
> 這個程式碼片段是特別針對 `ListView`；如果是 `GridView` 樣式，請將 [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) 和 [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 的 [TargetType](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) 屬性都設定為 `GridView`。

更多更精細地控制如何項目會被帶入檢視中，如果您的應用程式的目標是版本 1803年或更新版本，您可以使用[UIElement.BringIntoViewRequested 事件](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)。 您可以將它放[ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) **ListView**的/**GridView**攔截它之前沒有內部**ScrollViewer** ，如下列程式碼片段所示：

```xaml
<GridView x:Name="gridView">
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid Orientation="Horizontal"
                           BringIntoViewRequested="ItemsWrapGrid_BringIntoViewRequested"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

```cs
// The BringIntoViewRequested event is raised by the framework when items receive keyboard (or Narrator) focus or 
// someone triggers it with a call to UIElement.StartBringIntoView.
private void ItemsWrapGrid_BringIntoViewRequested(UIElement sender, BringIntoViewRequestedEventArgs args)
{
    if (args.VerticalAlignmentRatio != 0.5)  // Guard against our own request
    {
        args.Handled = true;
        // Swallow this request and restart it with a request to center the item.  We could instead have chosen
        // to adjust the TargetRect’s Y and Height values to add a specific amount of padding as it bubbles up, 
        // but if we just want to center it then this is easier.

        // (Optional) Account for sticky headers if they exist
        var headerOffset = 0.0;
        var itemsWrapGrid = sender as ItemsWrapGrid;
        if (gridView.IsGrouping && itemsWrapGrid.AreStickyGroupHeadersEnabled)
        {
            var header = gridView.GroupHeaderContainerFromItemContainer(args.TargetElement as GridViewItem);
            if (header != null)
            {
                headerOffset = ((FrameworkElement)header).ActualHeight;
            }
        }

        // Issue a new request
        args.TargetElement.StartBringIntoView(new BringIntoViewOptions()
        {
            AnimationDesired = true,
            VerticalAlignmentRatio = 0.5, // a normalized alignment position (0 for the top, 1 for the bottom)
            VerticalOffset = headerOffset, // applied after meeting the alignment ratio request
        });
    }
}
```

## <a name="colors"></a>色彩 

預設中，通用 Windows 平台調整您的 App 電視安全範圍色彩 (請參閱 [電視安全色彩](#tv-safe-colors)獲得詳細資訊)，以便您的 App 在任何電視中看起來都很美。 此外，您的 app 可以採取一些色彩設定的增強功能，來改善電視上的視覺體驗。

### <a name="application-theme"></a>應用程式佈景主題

您可以根據適合 app 的方式選擇 **「應用程式佈景主題」** (深色或淺色)，或者選擇不使用佈景主題。 請在[色彩佈景主題](../style/color.md)中閱讀更多有關佈景主題的一般建議。

UWP 也能讓 app 根據執行的裝置所提供的系統設定，動態設定佈景主題。
雖然 UWP 一律會優先採用使用者指定的佈景主題設定，但是每個裝置也會提供適當的預設佈景主題。
由於 Xbox One 的 *「媒體」* 體驗要求比 *「生產力」* 體驗要求還高，因此預設為深色的系統佈景主題。
如果您的 app 佈景主題是根據系統設定，在 Xbox One 上就會預設為深色。

### <a name="accent-color"></a>輔色

UWP 提供一個很方便的方式可以公開使用者從其系統設定選取的 **「輔色」**。

使用者可以在 Xbox One 上選取使用者的色彩，就如同在電腦上選取輔色一樣。
只要您的 app 透過筆刷或色彩資源呼叫這些輔色，就會採用使用者在系統設定中選取的色彩。 請注意，Xbox One 上的輔色依使用者 (不是依系統) 而定。

另外，Xbox One 上的使用者色彩組與電腦、手機和其他裝置上的不同。

只要您的 app 使用筆刷資源 (例如 **SystemControlForegroundAccentBrush**) 或色彩資源 (**SystemAccentColor**)，或直接透過 [UIColorType.Accent*](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType) API 改為呼叫輔色，這些色彩就會取代為適合電視的 Xbox One 可用輔色。 高對比的筆刷色彩也會從系統 (就像在電腦和手機上一樣) 提取。

若要深入了解一般的輔色，請參閱[輔色](../style/color.md#accent-color)。

### <a name="color-variance-among-tvs"></a>電視之間的色彩差異

針對電視設計時，請注意色彩顯示的方式依照所呈現的電視而相當不同。 不要認為色彩會完全像在監視器上顯示的一樣。 如果您的 app 依賴細微的色彩差異來區分 UI，色彩可能會混合在一起，而讓使用者感到困惑。 無論使用的電視為何，請試著使用足以讓使用者能夠清楚分辨的色彩。

### <a name="tv-safe-colors"></a>電視安全色彩

色彩的 RGB 值代表紅色、綠色及藍色的濃度。 電視無法處理極端的濃度值&mdash;它們可以在某些電視上會產生奇異的帶狀效果，或者看起來像褪色一樣。 此外，高濃度色彩可能會導致開花的效果 (附近的像素開始繪製相同的色彩)。 雖然不同思想學派對於何謂電視安全色彩有不同的意見，但是 16-235 (或以十六進位表示的 10-EB) 的 RGB 值內的色彩通常可安全地用於電視。

![電視安全色彩範圍](images/designing-for-tv/tv-safe-colors-2.png)

在過去，Xbox app 必須量身打造他們的顏色落在「電視安全」色彩的範圍內；不過，從秋季版 Creators Update 開始，Xbox One 自動調整在電視安全範圍內的完整內容。 這表示大部分的 app 開發人員不需要擔心電視安全色彩。

> [!IMPORTANT]
> 已在電視安全色彩範圍中的影片內容，當使用 [媒體基礎](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk)播放時，不會套用此色彩調整影響。

如果您正使用 DirectX 11 或 DirectX 12 開發 app 以及建立您自己的交換鏈結轉譯 UI 或影片，可以指定您藉由撥打 [IDXGISwapChain3::SetColorSpace1](https://docs.microsoft.com/windows/desktop/api/dxgi1_4/nf-dxgi1_4-idxgiswapchain3-setcolorspace1)來使用的色彩空間，這會讓系統知道是否需要調整色彩。

## <a name="guidelines-for-ui-controls"></a>UI 控制項的指導方針

提供數種可針對多部裝置良好運作的 UI 控制項，但當在電視上使用時具有特定考量。 深入了解有關針對 10 英呎體驗進行設計時使用這些控制項的一些最佳做法。

### <a name="pivot-control"></a>Pivot 控制項

[樞紐](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)可讓您在 App 內透過選取不同的標頭或索引標籤，快速瀏覽檢視。 控制項會在成為焦點的標頭加上底線，以便在使用遊戲台/遙控器時，更加凸顯目前選取的標頭。

![樞紐底線](images/designing-for-tv/pivot-underline.png)

您可以將 [Pivot.IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.isheaderitemscarouselenabledproperty) 屬性設定為 `true`，如此就能讓樞紐一律保持在相同位置上，而不會讓選取的樞紐標頭總是移到第一個位置。 對於大螢幕顯示器 (例如電視)，這會是更好的體驗，因為標頭換行可能會讓使用者分心。 如果不能在螢幕上同時顯示所有的樞紐標頭，即會提供捲軸，讓客戶能夠看見其他標頭。不過，您應該確定它們全都會顯示於螢幕上以提供最佳體驗。 如需詳細資訊，請參閱[索引標籤和樞紐](/windows/uwp/design/controls-and-patterns/pivot)。

### <a name="navigation-pane-a-namenavigation-pane-"></a>瀏覽窗格 <a name="navigation-pane" />

瀏覽窗格 (也稱為 *「漢堡式功能表」*) 是 UWP app 中常用的瀏覽控制項。 它一般會是一個含有清單樣式功能表的窗格，功能表中有數個可供選擇的選項，可將使用者帶到不同的頁面。 這個窗格通常一開始會摺疊起來以節省空間，使用者按一下按鈕即可將它開啟。

使用滑鼠和觸控可以很容易存取瀏覽窗格，但使用遊戲台/遙控器遠端則較難存取瀏覽窗格，因為使用者必須瀏覽到按鈕才能開啟窗格。 因此，理想的做法是讓 **「檢視」** 按鈕開啟瀏覽窗格，以及讓使用者能夠一路瀏覽到頁面左邊來開啟窗格。 如何實作這個設計模式的程式碼範例位於[程式設計焦點瀏覽](../input/focus-navigation-programmatic.md#split-view-code-sample)文件。 這將讓使用者非常容易存取窗格的內容。 如需有關瀏覽窗格在不同螢幕大小中如何運作的詳細資訊，以及適用於遊戲台/遙控器瀏覽的最佳做法，請參閱[瀏覽窗格](../controls-and-patterns/navigationview.md)。

### <a name="commandbar-labels"></a>CommandBar 標籤

在 [CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 上，理想的做法是將標籤放在圖示右邊，以便使 CommandBar 高度最小化並保持一致。 您可以將 [CommandBar.DefaultLabelPosition](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.defaultlabelpositionproperty) 屬性設定為 `CommandBarDefaultLabelPosition.Right` 來達到此目的。

![標籤在圖示右邊的 CommandBar](images/designing-for-tv/commandbar.png)

設定此屬性也會使標籤變成一律顯示，這非常適用於 10 英呎體驗，因為它可將使用者的點選次數降到最低。 這也是一個可供其他裝置類型遵循的絕佳模型。

### <a name="tooltip"></a>工具提示

[Tooltip](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip) 控制項是為了當使用者將滑鼠停留在元素上，或者點選並按住元素上的圖形時，在 UI 中提供更多資訊。 對於遊戲台和遙控器，當元素取得焦點一小段時間之後，`Tooltip` 就會出現在螢幕上停留一小段時間，然後消失。 如果使用了太多 `Tooltip`，這種行為可能會讓使用者分心。 針對電視進行設計時，請嘗試避免使用 `Tooltip`。

### <a name="button-styles"></a>按鈕樣式

雖然標準的 UWP 按鈕能在電視上順利運作，但是一些按鈕視覺樣式更能讓 UI 吸引注意，您可以針對所有平台考慮這些樣式，特別是在 10 英呎體驗中，更需要清楚傳達焦點所在。 若要深入了解這些樣式，請參閱[按鈕](../controls-and-patterns/buttons.md)。

### <a name="nested-ui-elements"></a>巢狀的 UI 元素

巢狀的 UI 會公開容器 UI 元素內部包含的巢狀可動作項目，其中巢狀項目與容器項目都可以各自獨立取得焦點。

對於某些輸入類型，巢狀的 UI 能夠良好運作，但對於依賴 XY 導覽的遊戲台與遙控器，則不一定能夠良好運作。 請務必遵循本主題中的指導方針，以確保您的 UI 能夠針對 10 英呎環境最佳化，讓使用者能夠輕鬆地存取所有可互動的元素。 一個常見的解決方案是將巢狀的 UI 元素放入`ContextFlyout`。

如需巢狀 UI 的詳細資訊，請參閱[清單項目中的巢狀 UI](../controls-and-patterns/nested-ui.md)。

### <a name="mediatransportcontrols"></a>MediaTransportControls

[MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) 元素提供預設的播放體驗，讓使用者可以與其媒體互動 (播放、暫停、開啟隱藏式輔助字幕等等)。 這個控制項是 [MediaPlayerElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 的屬性，且支援兩種配置選項：*「單列」* 和 *「雙列」*。 在單列配置中，滑桿與播放按鈕都會在同一列上，且播放/暫停按鈕會在滑桿左邊。 在雙列配置中，滑桿會自成一列，而播放按鈕則位於下方另一列上。 針對 10 英呎體驗設計時，應使用雙列配置，因為它能為控制器提供較佳的瀏覽。 若要啟用雙列配置，請在 `MediaPlayerElement` 的 [TransportControls](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) 屬性中的 `MediaTransportControls` 元素上設定 `IsCompact="False"`。

```xml
<MediaPlayerElement x:Name="mediaPlayerElement1"  
                    Source="Assets/video.mp4"
                    AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls IsCompact="False"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```  

如需深入了解將媒體加入 App，請參閱[媒體播放](../controls-and-patterns/media-playback.md)。

> ![NOTE] `MediaPlayerElement` 只能在 Windows10 版本 1607 及更新版本中取得。 如果您是針對先前版本的 Windows10 開發 App，便必須改為使用 [MediaElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement)。 上述建議也適用於 `MediaElement`，且 `TransportControls` 屬性也是以同樣的方式存取。

### <a name="search-experience"></a>搜尋體驗

搜尋內容是 10 英呎體驗中其中一個最常執行的功能。 如果您的 app 提供搜尋體驗，使用者能夠使用遊戲台上的 **Y** 按鈕做為快速鍵來快速存取會很有幫助。

大部分的客戶應該已經熟悉這個快速鍵，但是如果您願意，可以在 UI 新增視覺化 **Y** 字符，表示客戶可以使用此按鈕來存取搜尋功能。 如果您新增此提示，請務必使用 **Segoe Xbox MDL2 Symbol** 字型中的符號 (針對 XAML app 為 `&#xE3CC;`，針對 HTML app 則為 `\E426`)，以提供與 Xbox 殼層和其他 app 的一致性。

> [!NOTE]
> 因為 **Segoe Xbox MDL2 Symbol** 字型只能在 Xbox 上使用，所以此符號將無法顯示於您的電腦上。 不過，一旦將它部署到 Xbox 之後，就能顯示於電視上。

因為 **Y** 按鈕只能在遊戲台上使用，所以請務必提供其他方法來存取搜尋，例如 UI 中的按鈕。 否則，部分客戶可能無法存取此功能。

在 10 英呎體驗中，讓客戶使用全螢幕的搜尋體驗通常比較容易，因為顯示器的空間有限。 無論您有全螢幕或部分螢幕 (「就地」搜尋)，我們建議當使用者開啟搜尋體驗時，就出現已經開啟的螢幕小鍵盤，供客戶輸入搜尋字詞。

## <a name="custom-visual-state-trigger-for-xbox"></a>適用於 Xbox 的自訂視覺狀態觸發程序

若要針對 10 英呎體驗量身打造您的 UWP app，建議您在 App 偵測到它已在 Xbox 主機上啟動時變更配置。 若要這麼做，其中一個方法是使用 *「視覺狀態觸發程序」*。 當您想要在 **Blend for Visual Studio** 中進行編輯時，視覺狀態觸發程序最為實用。 下列程式碼片段示範如何建立適用於 Xbox 的視覺狀態觸發程序：

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <triggers:DeviceFamilyTrigger DeviceFamily="Windows.Xbox"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="RootSplitView.OpenPaneLength"
                        Value="368"/>
                <Setter Target="RootSplitView.CompactPaneLength"
                        Value="96"/>
                <Setter Target="NavMenuList.Margin"
                        Value="0,75,0,27"/>
                <Setter Target="Frame.Margin"
                        Value="0,27,48,27"/>
                <Setter Target="NavMenuList.ItemContainerStyle"
                        Value="{StaticResource NavMenuItemContainerXboxStyle}"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

若要建立觸發程序，請將下列類別新增到您的 App。 這是上面的 XAML 程式碼所參考的類別︰

```csharp
class DeviceFamilyTrigger : StateTriggerBase
{
    private string _currentDeviceFamily, _queriedDeviceFamily;

    public string DeviceFamily
    {
        get
        {
            return _queriedDeviceFamily;
        }

        set
        {
            _queriedDeviceFamily = value;
            _currentDeviceFamily = AnalyticsInfo.VersionInfo.DeviceFamily;
            SetActive(_queriedDeviceFamily == _currentDeviceFamily);
        }
    }
}
```

新增自訂觸發程序之後，每當您的 App 偵測到它在 Xbox One 主機上執行時，就會自動進行您在 XAML 程式碼中指定的配置修改。

另一個您可以檢查 App 是否正在 Xbox 上執行並接著進行適當調整的方式是透過程式碼。 您可以使用下列簡單變數來檢查您的 App 是否正在 Xbox 上執行︰

```csharp
bool IsTenFoot = (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily ==
                    "Windows.Xbox");
```

接著，您可以在這項檢查之後，在程式碼區塊中適當調整 UI。 

## <a name="summary"></a>摘要

針對 10 英呎經驗的設計已納入特殊考量，使其有別於其他所有平台的設計。 您當然可以直接將 UWP app 移植至 Xbox One 且它亦可運作，但它不一定是針對 10 英呎體驗最佳化，且會讓使用者感到失望。 遵循本文所述的指導方針，以確定 app 在電視上仍可呈現優異效果。

## <a name="related-articles"></a>相關文章

- [通用 Windows 平台 (UWP) 應用程式的裝置基本資訊](index.md)
- [遊戲台與遙控器的互動](../input/gamepad-and-remote-interactions.md)
- [UWP app 中的音效](../style/sound.md)
