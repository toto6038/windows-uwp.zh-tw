---
title: 遊戲台與遙控器的互動
description: 瞭解如何將您的應用程式優化，以從 Xbox 遊戲台和遠端控制進行輸入的指引、建議和建議。
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7ad6c173596c4f716f58b64f51d9068b6a689905
ms.sourcegitcommit: c5fdcc0779d4b657669948a4eda32ca3ccc7889b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2021
ms.locfileid: "102784789"
---
# <a name="gamepad-and-remote-control-interactions"></a>遊戲台與遙控器的互動

![鍵盤與遊戲台影像](images/keyboard/keyboard-gamepad.jpg)

***許多互動體驗都是在遊戲台、遠端控制和鍵盤之間共用***

在您的 Windows 應用程式中建立互動體驗，確保您的應用程式可透過電腦、膝上型電腦和平板電腦的傳統輸入類型 (滑鼠、鍵盤、觸控等) ，以及電視和 Xbox *10 英尺* 體驗的一般輸入類型（例如遊戲台和遠端控制）來使用和存取。

如需 Windows 應用程式的一般設計指導方針，請參閱 *10 英尺* 體驗的 [設計 Xbox 和電視](../devices/designing-for-tv.md)。

## <a name="overview"></a>概觀

在本主題中，我們將討論您應該在互動設計中考慮的事項 (或您不想要的方式，如果平臺會為您) ，請提供指引、建議和建議，以建立可使用的 Windows 應用程式，無論裝置、輸入類型或使用者能力和喜好設定為何。

您的應用程式應該在 *2.x 環境中* 以直覺且容易使用，因為它在 *10 英尺* 環境 (，反之亦然) 。 支援使用者的慣用裝置、讓 UI 專注于清除和 unmistakable、排列內容，讓流覽保持一致且可預測，以及為使用者提供他們想要執行的最短路徑。

> [!NOTE]
> 本主題的大部分程式碼片段都是 XAML/c #;不過，這些原則和概念適用于所有 Windows 應用程式。 如果您要開發適用于 Xbox 的 HTML/JavaScript Windows 應用程式，請參閱 GitHub 上的絕佳 [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) 程式庫。


## <a name="optimize-for-both-2-foot-and-10-foot-experiences"></a>針對2英尺和10英尺體驗進行優化

建議您至少測試您的應用程式，以確保它們在2英尺和10英尺的案例中都能正常運作，而且所有功能都可供 Xbox [遊戲和遠端控制](#gamepad-and-remote-control)使用並可供使用。

以下是您可以將應用程式優化以在2英尺和10英尺體驗中使用的其他方式，以及所有輸入裝置 (每個連結到本主題中適當章節的) 。

> [!NOTE]
> 因為 Xbox gamepads 和遠端控制支援許多 Windows 鍵盤的行為和經驗，所以這些建議適用于這兩種輸入類型。 如需更詳細的鍵盤資訊，請參閱 [鍵盤互動](keyboard-interactions.md) 。

| 功能        | 描述           |
| -------------------------------------------------------------- |--------------------------------|
| [XY 焦點瀏覽和互動](#xy-focus-navigation-and-interaction) | **XY 焦點導覽** 可讓使用者流覽應用程式的 UI。 不過，這限制使用者只能向上、向下、向左和向右瀏覽。 本節概述處理此功能和其他考量的建議。 |
| [滑鼠模式](#mouse-mode)|針對某些類型的應用程式（例如地圖或繪圖和繪製應用程式），XY 焦點導覽並不實用或甚至可能。 在這些情況下， **滑鼠模式** 讓使用者可以自由地使用遊戲台或遙控器來流覽，就像電腦上的滑鼠一樣。|
| [焦點視覺效果](#focus-visual)  | 焦點視覺效果是一個框線，會反白顯示目前焦點的 UI 元素。 這可協助使用者快速找出其所流覽的 UI 或與其互動的 UI。  |
| [焦點佔用](#focus-engagement) | 當 UI 元素具有焦點以便與其互動時，使用者必須按下遊戲台上的 **A/Select** 按鈕或遠端控制。 |
| [硬體按鈕](#hardware-buttons) | 遊戲台和遠端控制提供非常不同的按鈕和設定。 |

## <a name="gamepad-and-remote-control"></a>遊戲台與遙控器

就像電腦的鍵盤和滑鼠，以及手機和平板電腦的觸控，遊戲台與遙控器是 10 英呎體驗的主要輸入裝置。
本節將介紹什麼是硬體按鈕，以及它們所執行的動作。
在 [XY 焦點瀏覽和互動](#xy-focus-navigation-and-interaction)和[滑鼠模式](#mouse-mode)中，您將了解使用這些輸入裝置時如何最佳化您的 app。

遊戲台和遙控器行為的原始品質取決於您的 app 支援鍵盤的程度。 要確保您的 app 可搭配遊戲台/遙控器正確運作的一個好方式是確定它可搭配電腦的鍵盤正確運作，然後使用遊戲台/遙控器測試來尋找您 UI 的弱點。

### <a name="hardware-buttons"></a>硬體按鈕

本文件將會以下圖中所提供的名稱參照按鈕。

![遊戲台與遙控器按鈕圖](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

您可以從圖中看到，遊戲台支援一些遙控器不支援的按鈕，反之亦然。 雖然您可以使用只在一種輸入裝置上支援的按鈕，讓瀏覽 UI 的速度更快，但是請注意，使用這些按鈕進行重要的互動，可能會產生使用者無法與特定部分 UI 互動的情況。

下表列出 Windows 應用程式支援的所有硬體按鈕，以及支援它們的輸入裝置。

| 按鈕                    | 遊戲台   | 遠端控制    |
|---------------------------|-----------|-------------------|
| A/選取按鈕           | 是       | 是               |
| B/返回按鈕             | 是       | 是               |
| 方向鍵 (D 鍵)   | 是       | 是               |
| 功能表按鈕               | 是       | 是               |
| 檢視按鈕               | 是       | 是               |
| X 和 Y 按鈕           | 是       | 否                |
| 左搖桿                | 是       | 否                |
| 右搖桿               | 是       | 否                |
| LT 鍵和 RT 鍵   | 是       | 否                |
| LB 鍵和 RB 鍵    | 是       | 否                |
| OneGuide 按鈕           | 否        | 是               |
| 音量按鈕             | 否        | 是               |
| 頻道按鈕            | 否        | 是               |
| 媒體控制項按鈕     | 否        | 是               |
| 靜音按鈕               | 否        | 是               |

### <a name="built-in-button-support"></a>內建按鈕支援

UWP 會自動將現有的鍵盤輸入行為對應到遊戲台和遠端控制輸入。 下表列出這些內建的對應。

| 鍵盤              | 遊戲台/遙控器                        |
|-----------------------|---------------------------------------|
| 方向鍵            | 方向鍵 (也是遊戲台的左搖桿)    |
| 空白鍵              | A/選取按鈕                       |
| Enter                 | A/選取按鈕                       |
| 逸出                | B/返回按鈕*                        |

\*當應用程式未處理 B 按鈕的 [KeyDown](/uwp/api/windows.ui.xaml.uielement.keydown) 或 [KeyUp](/uwp/api/windows.ui.xaml.uielement.keyup) 事件時，將會引發 [SystemNavigationManager BackRequested](/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) 事件，這應該會導致應用程式中的回溯導覽。 不過，您必須自行實作此功能，如下列程式碼片段所示：

```csharp
// This code goes in the MainPage class

public MainPage()
{
    this.InitializeComponent();

    // Handling Page Back navigation behaviors
    SystemNavigationManager.GetForCurrentView().BackRequested +=
        SystemNavigationManager_BackRequested;
}

private void SystemNavigationManager_BackRequested(
    object sender,
    BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = this.BackRequested();
    }
}

public Frame AppFrame { get { return this.Frame; } }

private bool BackRequested()
{
    // Get a hold of the current frame so that we can inspect the app back stack
    if (this.AppFrame == null)
        return false;

    // Check to see if this is the top-most page on the app back stack
    if (this.AppFrame.CanGoBack)
    {
        // If not, set the event to handled and go back to the previous page in the
        // app.
        this.AppFrame.GoBack();
        return true;
    }
    return false;
}
```

> [!NOTE]
> 如果使用 B 功能鍵來回復，則不會在 UI 中顯示 [上一頁] 按鈕。 如果您正使用 [瀏覽檢視](../controls-and-patterns/navigationview.md)，將會自動隱藏 [上一頁] 按鈕。 如需回溯導覽的詳細資訊，請參閱 [適用于 Windows 應用程式的流覽歷程記錄和向後流覽](../basics/navigation-history-and-backwards-navigation.md)。

Xbox one 上的 Windows 應用程式也支援按下 **功能表** 按鈕來開啟內容功能表。 如需詳細資訊，請參閱 [CommandBar 和 ContextFlyout](#commandbar-and-contextflyout)。

### <a name="accelerator-support"></a>快速鍵支援

快速鍵可用來加速瀏覽 UI。 不過可能只有某些輸入裝置才有這些按鈕，所以請記住，並非所有使用者都能使用這些功能。 事實上，「遊戲台」是目前唯一支援 Xbox One 上 Windows 應用程式加速器功能的輸入裝置。

下表列出 UWP 內建，以及您可自行實作的快速鍵支援。 請在您自訂的 UI 利用這些行為，以提供一致、友善的使用者體驗。

| 互動   | 鍵盤/滑鼠   | 遊戲台      | 內建於︰  | 建議用於： |
|---------------|------------|--------------|----------------|------------------|
| 向上一頁/向下一頁  | 向上一頁/向下一頁 | LT 鍵/RT 鍵 | [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView)、[ListBox](/uwp/api/Windows.UI.Xaml.Controls.ListBox)、[ListViewBase](/uwp/api/Windows.UI.Xaml.Controls.ListViewBase)、[ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView)、`ScrollViewer`、[Selector](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector)、[LoopingSelector](/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector)、[ComboBox](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)、[FlipView](/uwp/api/Windows.UI.Xaml.Controls.FlipView) | 支援垂直捲動的檢視
| 向左一頁/向右一頁 | 無 | LB 鍵/RB 鍵 | [ListBox](/uwp/api/Windows.UI.Xaml.Controls.ListBox)、 [ListViewBase](/uwp/api/Windows.UI.Xaml.Controls.ListViewBase)、 [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView)、 `ScrollViewer` 、 [Selector](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector)、 [LoopingSelector](/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector)、 [FlipView](/uwp/api/Windows.UI.Xaml.Controls.FlipView) | 支援水平捲動的檢視
| 放大/縮小        | CTRL +/- | LT 鍵/RT 鍵 | 無 | `ScrollViewer`、支援放大和縮小的檢視 |
| 開啟/關閉瀏覽窗格 | 無 | 檢視 | 無 | 瀏覽窗格​​ |
| 搜尋 | 無 | Y 按鈕 | 無 | App 中主要搜尋功能的快速鍵 |
| [開啟操作功能表](#commandbar-and-contextflyout) | 以滑鼠右鍵按一下 | 功能表按鈕 | [ContextFlyout](/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) | 操作功能表 |

## <a name="xy-focus-navigation-and-interaction"></a>XY 焦點瀏覽和互動

如果您的 app 支援使用鍵盤正確瀏覽焦點，這也會順利轉譯到遊戲台與遙控器。
使用箭號鍵的導覽會對應至 **D** (以及遊戲台上的 **左方杆**) ，與 UI 元素的互動會對應至 **Enter/Select** 鍵 (請參閱 [遊戲台和遠端控制](#gamepad-and-remote-control)) 。

許多事件和屬性都同時為鍵盤與遊戲台所用 &mdash; 兩者都會引發 `KeyDown` 和 `KeyUp` 事件，且兩者都只會瀏覽到具有屬性 `IsTabStop="True"` 和 `Visibility="Visible"` 的控制項。 如需鍵盤設計指導方針，請參閱[鍵盤互動](../input/keyboard-interactions.md)。

如果正確實作鍵盤支援，您的 app 應該可以正確運作，不過可能需要一些額外的工作才能支援每種狀況。 請思考您的 app 特定需求，以盡可能提供最佳的使用者體驗。

> [!IMPORTANT]
> 針對 Xbox One 上執行的 Windows 應用程式，預設會啟用滑鼠模式。 若要停用滑鼠模式及啟用 XY 焦點瀏覽，請設定 `Application.RequiresPointerMode=WhenRequested`。

### <a name="debugging-focus-issues"></a>針對焦點問題進行偵錯

[FocusManager.GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) 方法會告訴您目前焦點位於哪一個元素。 在焦點視覺效果位置可能不明顯的情況下，這會相當有用。 您可以將此資訊記錄到 Visual Studio 輸出視窗，如以下所示：

```csharp
page.GotFocus += (object sender, RoutedEventArgs e) =>
{
    FrameworkElement focus = FocusManager.GetFocusedElement() as FrameworkElement;
    if (focus != null)
    {
        Debug.WriteLine("got focus: " + focus.Name + " (" +
            focus.GetType().ToString() + ")");
    }
};
```

有三個常見的原因會導致 XY 瀏覽可能無法依照您預期的方式運作︰

* [IsTabStop](/uwp/api/windows.ui.xaml.controls.control.istabstop) 或 [Visibility](/uwp/api/windows.ui.xaml.uielement.visibility) 屬性設定錯誤。
* 取得焦點的控制項實際上比您所想的大 &mdash; XY 瀏覽看的是控制項的大小總計 ([ActualWidth](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) 和 [ActualHeight](/uwp/api/windows.ui.xaml.frameworkelement.actualheight))，而不只是呈現有趣事項的控制項部分。
* 一個可設定焦點的控制項位於另一個控制項上 &mdash; XY 瀏覽不支援重疊的控制項。

如果在修正這些問題之後，XY 瀏覽仍然未依照您預期的方式運作，您可以使用[覆寫預設的瀏覽](#overriding-the-default-navigation)中所述的方法來手動指向您要取得焦點的元素。

如果 XY 瀏覽如預期般運作但未顯示焦點視覺效果，則可能是下列其中一個問題所造成：

* 您將控制項重新樣板化而未包含焦點視覺效果。 請設定 `UseSystemFocusVisuals="True"` 或手動新增焦點視覺效果。
* 您藉由呼叫 `Focus(FocusState.Pointer)` 來移動焦點。 [FocusState](/uwp/api/Windows.UI.Xaml.FocusState) 參數可控制對焦點視覺效果產生的作用。 一般而言，您應該將此參數設定為 `FocusState.Programmatic`，如果焦點視覺效果先前是可見的，這會讓它保持可見，如果先前是隱藏的，則會保持隱藏。

本節的其餘部分將深入探討使用 XY 瀏覽時常見的設計挑戰，並提供數種解決方法。

### <a name="inaccessible-ui"></a>無法存取的 UI

因為 XY 焦點瀏覽限制使用者只能向上、向下、向左和向右移動，所以您有可能無法存取某些 UI。
下圖說明 XY 焦點瀏覽不支援的 UI 配置類型範例。
請注意，使用使用遊戲台/遙控器無法存取中間的元素，因為會優先瀏覽垂直和水平方向，中間的元素將永遠不會優先取得焦點。

![四個角有元素，且中間有無法存取的元素](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

如果因為某些原因無法重新排列 UI，請使用下一節討論的其中一個技術來覆寫預設焦點行為。

### <a name="overriding-the-default-navigation"></a>覆寫預設的瀏覽

雖然通用 Windows 平台會嘗試使方向鍵/左搖桿的瀏覽方式讓使用者感到很直覺，但是它並無法保證針對您 App 之意圖最佳化的行為。
確保瀏覽針對您的 App 最佳化的最佳方式，是先使用控制器加以測試，以確認使用者能針對 App 的案例以直覺的方式存取每個 UI 元素。 如果您的 app 案例呼叫 XY 焦點瀏覽無法達到的行為，請考慮下列各節中的下列建議並/或覆寫行為，將焦點放在合理的項目上。

下列程式碼片段示範如何覆寫 XY 焦點瀏覽行為︰

```xml
<StackPanel>
    <Button x:Name="MyBtnLeft"
            Content="Search" />
    <Button x:Name="MyBtnRight"
            Content="Delete"/>
    <Button x:Name="MyBtnTop"
            Content="Update" />
    <Button x:Name="MyBtnDown"
            Content="Undo" />
    <Button Content="Home"  
            XYFocusLeft="{x:Bind MyBtnLeft}"
            XYFocusRight="{x:Bind MyBtnRight}"
            XYFocusDown="{x:Bind MyBtnDown}"
            XYFocusUp="{x:Bind MyBtnTop}" />
</StackPanel>
```

在這個案例中，當焦點在 `Home` 按鈕上而使用者瀏覽到左邊時，焦點會移到 `MyBtnLeft` 按鈕；如果使用者瀏覽到右邊，焦點會移到 `MyBtnRight` 按鈕等等。

若要防止焦點從某個特定方向移出控制項，請使用 `XYFocus*` 屬性，以將它指向相同的控制項︰

```xml
<Button Name="HomeButton"  
        Content="Home"  
        XYFocusLeft ="{x:Bind HomeButton}" />
```

透過使用這些 `XYFocus` 屬性，當下一個焦點候選項目超出其視覺化樹狀結構時，父控制項即可強制子項目的瀏覽，除非取得焦點的子項目使用相同的 `XYFocus` 屬性。

```xml
<StackPanel Orientation="Horizontal" Margin="300,300">
    <UserControl XYFocusRight="{x:Bind ButtonThree}">
        <StackPanel>
            <Button Content="One"/>
            <Button Content="Two"/>
        </StackPanel>
    </UserControl>
    <StackPanel>
        <Button x:Name="ButtonThree" Content="Three"/>
        <Button Content="Four"/>
    </StackPanel>
</StackPanel>
```

在上述範例中，如果焦點是在 `Button` Two 且使用者向右瀏覽，則最佳的焦點候選項目將會是 `Button` Four；不過，焦點會移到 `Button` Three，因為當焦點超出其樹狀結構時，父項目 `UserControl` 會強制瀏覽到該位置。

### <a name="path-of-least-clicks"></a>最少點選次數的路徑

請嘗試讓使用者以最少的點選次數執行最常見的工作。 在下列範例中， [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 會放在 [ **播放** ] 按鈕 (這一開始會取得焦點) 和常用的元素，因此在優先順序工作之間放置不必要的元素。

![最佳的瀏覽做法提供最少點選次數的路徑](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

在下列範例中， [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 會放在 [ **播放** ] 按鈕的上方。
只要重新排列 UI，不要將不必要的元素放在優先的工作之間，即可大幅改善您 app 的可用性。

![TextBlock 移動到 [播放] 按鈕上方，不再位於優先工作之間](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### <a name="commandbar-and-contextflyout"></a>CommandBar 和 ContextFlyout

使用 [CommandBar](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 時，請記住[問題︰UI 元素位於長捲動清單/格線之後](#problem-ui-elements-located-after-long-scrolling-list-grid)中所述的捲動清單問題。 下列影像顯示一個 `CommandBar` 位於清單/格線下方的 UI 配置。 使用者必須一直向下捲動完整個清單/格線，才能到達 `CommandBar`。

![CommandBar 位於清單/格線的底部](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

如果您將 `CommandBar` *上面* 的清單/方格放在上方，該怎麼辦？ 雖然使用者在向下捲動清單/格線後，必須捲動回去才能到達 `CommandBar`，但是比起前一種設定，這種設定的瀏覽程度會少一些。 請注意，這是假設您的 app 最初的焦點是放置在 `CommandBar` 旁邊或上方；如果最初的焦點是在清單/格線下方，此方法也同樣不佳。 如果這些 `CommandBar` 專案是全域動作專案，而不需要經常存取 (例如 **同步** 按鈕) ，則可能可以接受在清單/方格上方。

雖然您無法垂直堆疊 `CommandBar` 的項目，但是如果將這些項目依捲動方向放置 (例如，放在垂直捲動清單的左邊或右邊，或是放在水平捲動清單的頂端或底部) 對您的 UI 配置而言可行，則這可能會是您想要考慮使用的另一個選項。

如果您的 App 有所含項目必須已可供使用者存取的 `CommandBar`，您可以考慮將這些項目放在 [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) 內而將它們從 `CommandBar` 中移除。 `ContextFlyout` 是 [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) 的屬性，是與該專案相關聯的 [內容功能表](../controls-and-patterns/dialogs-and-flyouts/index.md) 。 在電腦上，當您在具有 `ContextFlyout` 的元素上按一下滑鼠右鍵時，該操作功能表就會出現。 在 Xbox One 上，當焦點在這類專案上時，當您按下 **功能表** 按鈕時，就會發生這種情況。

### <a name="ui-layout-challenges"></a>UI 配置挑戰

由於 XY 焦點瀏覽的特性，有些 UI 配置更具挑戰性，而且應該依個別案例評估。 雖然沒有一種「正確」的方式，您選擇的解決方案也取決於您 app 的特定需求，不過您還是可以使用一些技術，創造絕佳的電視體驗。

為了更深入了解，讓我們看看一個假想的 App，以便說明這當中的部分問題及可克服這些問題的技術。

> [!NOTE]
> 這個假的 App 是為了說明 UI 問題和可能的解決方案，而不是為了示範您特定 App 的最佳使用者體驗。

以下是一個假想的房地產 App，此 App 顯示可供銷售的房屋清單、地圖、房地產描述，以及其他資訊。 這個 app 有三個挑戰，您可以使用下列技術克服︰

- [重新排列 UI](#ui-rearrange)
- [焦點佔用](#engagement)
- [滑鼠模式](#mouse-mode)

![假造的房地產 app](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### <a name="problem-ui-elements-located-after-long-scrolling-listgrid"></a>問題：UI 元素位於長的捲動清單/格線之後 <a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

下圖中所示的屬性 [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView) 是一份很長的捲動清單。 如果在上 *不* 需要 [engagement](#focus-engagement) `ListView` ，當使用者流覽至清單時，焦點會放在清單中的第一個專案。 若要讓使用者連線到 **上一個** 按鈕或 **[下一步]** 按鈕，他們必須逐一查看清單中的所有專案。 要求使用者瀏覽整個清單會非常痛苦&mdash;如果清單夠短，這還算可以接受&mdash;，所以您可能要考慮其他方案。

![房地產 app︰50 個項目的清單需要點選 51 次才能到達下面的按鈕](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### <a name="solutions"></a>方案

**重新排列 UI <a name="ui-rearrange"></a>**

除非您最初的焦點是放在頁面底部，否則將 UI 元素放在長捲動清單上方比放在下方更容易存取。
如果這個新的配置適用於其他裝置，請針對所有裝置系列變更配置，而不要只是針對 Xbox One 進行特殊的 UI 配置，這是比較經濟的方法。
此外，垂直捲動方向放置 UI 元素 (也就是水平放在垂直捲動清單兩側，或垂直放在水平捲動清單上下) 可更方便存取。

![房地產 app︰放置按鈕在長捲動清單上方](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

**專注參與 <a name="engagement"></a>**

當 *需要* 參與時，整個 `ListView` 會成為單一焦點目標。 使用者可以略過清單的內容，以取得下一個可設定焦點的元素。 請在[焦點佔用](#focus-engagement)中閱讀更多關於哪些控制項支援佔用，以及如何使用的內容。

![房地產 app︰設定需要佔用，只需按 1 次就可到達 [上一個/下一個] 按鈕](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### <a name="problem-scrollviewer-without-any-focusable-elements"></a>問題︰ScrollViewer 沒有任何可設定焦點的元素

由於 XY 焦點瀏覽仰賴一次僅瀏覽單一可設定 UI 元素的設計，因此沒有任何可設定焦點的元素的 [ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) (例如本範例所示的只包含文字) 可能會造成使用者無法檢視 `ScrollViewer` 中的所有內容。
如需這個案例和其他相關案例的解決方案，請參閱[焦點佔用](#focus-engagement)。

![房地產 app︰ScrollViewer 只包含文字](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### <a name="problem-free-scrolling-ui"></a>問題︰自由捲動 UI

當您的 app 需要自由捲動 UI 時 (例如繪圖介面，或本範例中的地圖)，XY 焦點瀏覽就無法運作。
在這種情況下，您可以開啟[滑鼠模式](#mouse-mode)以允許使用者自由地在 UI 元素內瀏覽。

![使用滑鼠模式的地圖 UI 元素](images/designing-for-tv/map-mouse-mode.png)

## <a name="mouse-mode"></a>滑鼠模式

如 [XY 焦點瀏覽和互動](#xy-focus-navigation-and-interaction)中所述，在 Xbox One 上，焦點是使用 XY 瀏覽系統移動，讓使用者能夠透過向上、向下、向左和向右在控制項之間移動焦點。
不過，某些控制項 (例如 [WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) 和 [MapControl](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)) 需要類似滑鼠的互動，使用者可以自由地在控制項的界限內移動指標。
還有一些 app，使用者要能夠在整個頁面移動指標，讓使用遊戲台/遙控器的使用者能夠擁有類似使用電腦滑鼠的體驗。

對於這些案例，您應該對整個頁面或對某個頁面內的某個控制項要求指標 (滑鼠模式)。
例如，您的 app 可以有一個有 `WebView` 控制項的頁面，只有在這個控制項當中才使用滑鼠模式，而在其他地方則仍使用 XY 焦點瀏覽。
若要要求指標，您可以指定是否要 **在控制項或頁面參與** 時，或是 **當頁面具有焦點** 時，想要它。

> [!NOTE]
> 不支援在控制項取得焦點時要求指標。

針對在 Xbox One 上執行的 XAML 及裝載的 Web 應用程式，預設都會針對整個 App 啟用滑鼠模式。 強烈建議您關閉這個功能，並將您的應用程式針對 XY 瀏覽進行最佳化。 若要這樣做，請將 `Application.RequiresPointerMode` 屬性設定為 `WhenRequested`，以便讓您只有在控制項或頁面要求滑鼠模式時才啟用它。

若要在 XAML app 中執行這項操作，請在您的 `App` 類別中使用下列程式碼︰

```csharp
public App()
{
    this.InitializeComponent();
    this.RequiresPointerMode =
        Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

如需詳細資訊 (包括適用於 HTML/JavaScript 的範例程式碼)，請參閱[如何停用滑鼠模式](../../xbox-apps/how-to-disable-mouse-mode.md)。

下圖顯示遊戲台/遙控器在滑鼠模式下的按鈕對應。

![遊戲台/遙控器在滑鼠模式下的按鈕對應](images/designing-for-tv/10ft_infographics_mouse-mode.png)

> [!NOTE]
> 只有在搭配使用遊戲台/遙控器的 Xbox One 上才支援滑鼠模式。 在其他裝置系列和輸入類型上，會以無訊息方式略過。

在控制項或頁面上使用 [RequiresPointer](/uwp/api/windows.ui.xaml.controls.requirespointer) 屬性，可在其上啟用滑鼠模式。 此屬性有三個可能值︰`Never` (預設值)、`WhenEngaged` 以及 `WhenFocused`。

### <a name="activating-mouse-mode-on-a-control"></a>在控制項上啟用滑鼠模式

當使用者以 `RequiresPointer="WhenEngaged"` 設定佔用控制項時，會在控制項上啟用滑鼠模式，直到使用者離開為止。 下列程式碼片段示範一個簡單的 `MapControl`，此控制項會在被佔用時啟用滑鼠模式︰

```xml
<Page>
    <Grid>
        <MapControl IsEngagementRequired="true"
                    RequiresPointer="WhenEngaged"/>
    </Grid>
</Page>
```

> [!NOTE]
> 如果一個控制項在被佔用時會啟用滑鼠模式，則它也必須使用 `IsEngagementRequired="true"` 的佔用設定，否則永遠不會啟用滑鼠模式。

當控制項處於滑鼠模式時，其巢狀結構下的控制項也會處於滑鼠模式。 此時會忽略其子項要求的模式&mdash;因為不可能父項處於滑鼠模式，而子項不是。

此外，只有在控制項取得焦點時才會檢查控制項的要求模式，因此當控制項有焦點時，模式不會動態變更。

### <a name="activating-mouse-mode-on-a-page"></a>啟用頁面上的滑鼠模式

若頁面有 `RequiresPointer="WhenFocused"` 屬性，當整個頁面取得焦點時，將啟用滑鼠模式。 下列程式碼片段示範如何提供頁面這個屬性︰

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page>
```

> [!NOTE]
> 只有在 [Page](/uwp/api/Windows.UI.Xaml.Controls.Page) 物件上才支援 `WhenFocused` 值。 如果您嘗試在控制項上設定這個值，就會擲回例外狀況。

### <a name="disabling-mouse-mode-for-full-screen-content"></a>停用全螢幕內容的滑鼠模式

當以全螢幕顯示影片或其他類型的內容時，您通常會想要隱藏游標，因為它可能會干擾使用者。 這種案例會在 App 的其他部分是使用滑鼠模式，但您希望在顯示全螢幕內容時將它關閉時發生。 若要這樣做，請將全螢幕內容置於個別的 `Page` 上，然後依照下列步驟執行。

1. 在 `App` 物件中，設定 `RequiresPointerMode="WhenRequested"`。
2. 在每個 `Page` 物件（全螢幕 *除外* `Page` ）中，設定 `RequiresPointer="WhenFocused"` 。
3. 針對全螢幕 `Page` 設定 `RequiresPointer="Never"`。

如此一來，顯示全螢幕內容時就一定不會出現游標。

## <a name="focus-visual"></a>焦點視覺效果

視覺焦點是目前有焦點的 UI 元素周圍的框線。 這可協助引導使用者輕鬆瀏覽您的 UI 而不會迷失。

透過視覺更新，以及對視覺焦點新增的許多自訂選項，開發人員可以信任單一視覺焦點能在電腦和 Xbox One 上，以及在支援鍵盤和/或遊戲台/遙控器的任何其他 Windows 10 裝置上運作良好。

雖然不同平台上可以使用相同的視覺焦點，但是使用者對於 10 英呎體驗遇到的狀況稍有不同。 您應該假設使用者不會完全注意到整個電視螢幕，因此目前聚焦的元素對使用者而言隨時都清晰可見非常重要，避免視覺搜尋遇到挫折。

此外，請務必記住，使用遊戲台或遠端控制（而 *非* 鍵盤）時，預設會顯示焦點視覺效果。 因此，即使您未實作，當您在 Xbox One 上執行您的 app 時也會顯示。

### <a name="initial-focus-visual-placement"></a>最初的視覺焦點位置

啟動 app 或瀏覽到頁面時，請將焦點放在使用者應採取動作的第一個 UI 元素上。 例如，相片 app 可以將焦點放在圖庫中的第一個項目，瀏覽到歌曲詳細檢視的音樂 app 可以將焦點放在播放按鈕，以方便播放音樂。

請嘗試將最初的焦點放在您 app 的左上方區域 (如果是由右至左的流程，則放在右上方)。 大部分的使用者一開始通常會將焦點放在這個角落，因為這是 app 內容流程通常開始的位置。

### <a name="making-focus-clearly-visible"></a>讓焦點清晰可見

螢幕上永遠要顯示一個視覺焦點讓使用者可以挑選而停駐，而不需要搜尋焦點。 同樣地，螢幕上也應該隨時有一個可設定焦點的項目 &mdash; 例如，不要使用只有文字而沒有可設定焦點元素的快顯視窗。

這個規則有一個例外狀況，就是全螢幕體驗，例如觀賞影片或檢視影像，在這些情況下並不適合顯示焦點視覺效果。

### <a name="reveal-focus"></a>顯色焦點

顯色焦點是一種光源效果，例如功能鍵，會在使用者將遊戲台或鍵盤焦點移近可設定焦點元素時，以動畫方式呈現這些元素的框線。 藉由以動畫方式呈現焦點元素框線周圍光暈，顯色焦點可讓使用者更深入瞭解焦點的位置以及對焦的位置。

預設會關閉顯色焦點。 針對 10 英呎體驗，您應該藉由在您的應用程式建構函式中設定 [Application.FocusVisualKind 屬性](/uwp/api/windows.ui.xaml.application.FocusVisualKind)來加入顯色焦點。

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

如需詳細資訊請參閱 [顯色焦點](../style/reveal-focus.md)的指導方針。

### <a name="customizing-the-focus-visual"></a>自訂焦點視覺效果

如果您想要自訂焦點視覺效果，只要修改與每個控制項的焦點視覺效果相關的屬性，即可達到此目的。 有數個這類屬性可供您利用來將您的 App 個人化。

您甚至可以不使用系統提供的焦點視覺效果，方法是使用視覺狀態來繪製自己的焦點視覺效果。 若要深入了解，請參閱 [VisualState](/uwp/api/Windows.UI.Xaml.VisualState)。

### <a name="light-dismiss-overlay"></a>消失關閉重疊

為了讓使用者注意使用者目前使用遊戲控制器或遠端控制操作的 UI 元素，UWP 會自動加入「冒煙」層，其涵蓋快顯視窗 UI 以外的區域（當應用程式正在 Xbox One 上執行時）。 這不需要任何額外的工作，但是設計您的 UI 時要記住這一點。 您可以在任何 `FlyoutBase` 上設定 `LightDismissOverlayMode` 屬性以啟用或停用煙霧層；它預設為 `Auto`，表示在 Xbox 上會啟用，在其他地方則會停用。 如需詳細資訊，請參閱[強制回應與消失關閉](../controls-and-patterns/menus.md)。

## <a name="focus-engagement"></a>焦點佔用

焦點佔用的目的是要讓您更容易使用控制器或遙控器來與 App 互動。

> [!NOTE]
> 設定焦點佔用並不會影響鍵盤或其他輸入裝置。

當 [FrameworkElement](/uwp/api/Windows.UI.Xaml.FrameworkElement) 物件上的屬性 `IsFocusEngagementEnabled` 設定為 `True` 時，它會將控制項標示為需要焦點佔用。 這表示使用者必須按 **A/Select** 按鈕來「參與」控制項並與其互動。 完成後，他們可以按下 **B/[上一步** ] 按鈕，將控制項脫開並移出它。

> [!NOTE]
> `IsFocusEngagementEnabled` 是新的 API，尚未記載。

### <a name="focus-trapping"></a>焦點受困

焦點受困是當使用者嘗試瀏覽 app 的 UI，但「受困」在控制項內時所發生的狀況，很難或甚至無法移動到控制項之外。

下列範例顯示產生焦點受困的 UI。

![按鈕在水平滑桿左邊和右邊](images/designing-for-tv/focus-engagement-focus-trapping.png)

如果使用者想要從左邊的按鈕瀏覽到右邊的按鈕，合理的狀況是假設使用者只要按下方向鍵/左搖桿向右兩次。
不過，如果 [Slider](/uwp/api/Windows.UI.Xaml.Controls.Slider) 不需要佔用，就會發生下列行為︰當使用者第一次按右鍵，焦點會移到 `Slider`，然後當使用者再按右鍵一次，`Slider` 的控點會向右移動。 使用者會一直將控點往右移動，而無法到達按鈕。

有幾種方法可以解決這個問題。 其中一個是設計不同的版面配置，類似于 [XY 焦點導覽](#xy-focus-navigation-and-interaction) 中的房地產應用程式範例，以及我們將 **上一個** 按鈕和 **下一個** 按鈕重新置放到上面的互動 `ListView` 。 垂直而非水平堆疊控制項可以解決問題，如下列影像所示。

![按鈕在水平滑桿上方和下方](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

現在，使用者可以使用方向鍵/左搖桿向上及向下瀏覽到每個控制項，當 `Slider` 有焦點時，使用者可以按左鍵和按右鍵如預期般移動 `Slider` 控點。

解決這個問題的另一種方法需要在 `Slider` 上設定佔用。 如果您設定 `IsFocusEngagementEnabled="True"`，這會導致下列行為。

![滑桿上需要焦點佔用，讓使用者可以瀏覽到右邊的按鈕](images/designing-for-tv/focus-engagement-slider.png)

當 `Slider` 需要焦點佔用時，使用者只要按方向鍵/左搖桿向右兩次，就可以到達右邊的按鈕。 這個解決方案很棒，因為它不需要調整任何 UI，就能產生預期的行為。

### <a name="items-controls"></a>項目控制項

除了 [Slider](/uwp/api/Windows.UI.Xaml.Controls.Slider) 控制項，還有其他您需要佔用的控制項，例如︰

- [ListBox](/uwp/api/Windows.UI.Xaml.Controls.ListBox)
- [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView](/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [FlipView](/uwp/api/Windows.UI.Xaml.Controls.FlipView)

這些控制項與 `Slider` 控制項不同，不會在自己本身內限制焦點，不過當它們包含大量資料時，可能會造成可用性問題。 以下是 `ListView` 包含大量資料的範例。

![有大量資料，以及頂端按鈕和底部按鈕的 ListView](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

類似 `Slider` 範例，我們來嘗試使用遊戲台/遙控器從上方按鈕瀏覽到下方按鈕。
焦點從頂端按鈕開始，按下方向鍵/搖桿會將焦點放在 `ListView` 中的第一個項目上 ("Item 1")。
當使用者再向下按一次，清單中下一個項目會取得焦點，而不是底部的按鈕。
若要到達按鈕，使用者必須先瀏覽 `ListView` 中的每個項目。
如果 `ListView` 包含大量資料，這可能相當不便，而且使用者體驗不佳。

若要解決這個問題，請將 `ListView` 上的 `IsFocusEngagementEnabled="True"` 屬性設定為需要在其上佔用。
這樣可讓使用者只要向下按，就可快速跳過 `ListView`。 不過，它們將無法在清單中滾動，或從專案中選擇專案，除非它們在有焦點時按 **A/Select** 按鈕，然後按下 **B/上一頁** 按鈕來鬆開。

![需要佔用的 ListView](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### <a name="scrollviewer"></a>ScrollViewer

[ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 與這些控制項稍有不同，其具有待考量的個別原因。 如果您有具可設定焦點內容的 `ScrollViewer`，瀏覽至 `ScrollViewer` 預設可讓您在其可設定焦點的元素之間移動。 就像在 `ListView` 中一樣，您必須捲動每個項目才能瀏覽到 `ScrollViewer` 之外。

如果沒有 `ScrollViewer` 可設定焦點的內容 &mdash; ，例如，如果它只包含 &mdash; 您可以設定的文字， `IsFocusEngagementEnabled="True"` 使用者就可以 `ScrollViewer` 使用 [ **A]/[選取**] 按鈕來參與。 參與之後，他們就可以使用 **D-pad/left** 鍵來滾動文字，然後按下 **B/上一步** 按鈕，在完成時將它鬆開。

另一種方法是在上設定，如此一來 `IsTabStop="True"` `ScrollViewer` ，使用者就不必參與控制項，只要將 &mdash; 焦點放在控制項上，然後在中沒有可設定焦點的專案時，就會使用 **D-pad/left** 來進行滾動 `ScrollViewer` 。

### <a name="focus-engagement-defaults"></a>焦點佔用預設值

有些控制項會造成焦點受困，所以其預設設定需要焦點佔用，而有些控制項預設關閉焦點佔用，但可以將它開啟獲得好處。 下表列出這些控制項，及其預設的焦點佔用行為。

| 控制               | 焦點佔用預設值  |
|-----------------------|---------------------------|
| CalendarDatePicker    | 開啟                        |
| FlipView              | 關閉                       |
| GridView              | 關閉                       |
| ListBox               | 關閉                       |
| ListView              | 關閉                       |
| ScrollViewer          | 關閉                       |
| SemanticZoom          | 關閉                       |
| 滑桿                | 開啟                        |

所有其他的 Windows 控制項都不會在發生任何行為或視覺變更時產生任何變更 `IsFocusEngagementEnabled="True"` 。

## <a name="summary"></a>摘要

您可以建立針對特定裝置或經驗優化的 Windows 應用程式，但通用 Windows 平臺也可讓您建立應用程式，以在跨裝置、2英尺和10英尺的體驗，以及輸入裝置或使用者的能力之間順利使用。 使用本文中的建議可確保您的應用程式可以在電視和電腦上正常進行。

## <a name="related-articles"></a>相關文章

- [針對 Xbox 和電視進行設計](../devices/designing-for-tv.md)
- [Windows 應用程式的裝置入門](index.md)
