---
Description: Learn how to programmatically manage focus navigation with keyboard, gamepad, and accessibility tools in a UWP app.
title: 使用鍵盤、遊戲台、遠端控制與協助工具以程式設計方式進行焦點瀏覽
label: Programmatic focus navigation
keywords: 鍵盤, 遊戲控制器, 遙控器, 瀏覽, 瀏覽策略, 輸入, 使用者互動, 協助工具, 可用性
ms.date: 03/19/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 64a15526b32be52b0fc667dc6f8b549d6faae8f1
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8340384"
---
# <a name="programmatic-focus-navigation"></a>程式設計焦點瀏覽

![鍵盤、遠端與方向鍵](images/dpad-remote/dpad-remote-keyboard.png)

若要以程式設計方式在您的 UWP 應用程式中移動焦點，您可以使用 [FocusManager.TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 方法或[FocusManager.FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 方法。

[TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 會嘗試使用指定方向中下一個可設定為焦點元素的焦點，從元素變更焦點，[FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 則會擷取元素 (作為 [DependencyObject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject))，該元素會根據指定的瀏覽方向取得焦點 (僅限方向瀏覽，無法用來模擬 Tab 瀏覽)。

> [!NOTE]
> 我們建議使用 [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 方法，而非 [FindNextFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextFocusableElement_Windows_UI_Xaml_Input_FocusNavigationDirection_)，因為 FindNextFocusableElement 會擷取 UIElement，其會在下一個可設定為焦點的元素並非 UIElement 時傳回 Null (例如超連結 (Hyperlink) 物件)。 

## <a name="find-a-focus-candidate-within-a-scope"></a>在範圍中尋找焦點候選項目

您可以為 [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 和 [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 自訂焦點瀏覽行為，包含在特定 UI 樹狀結構中搜尋下一個焦點候選項目，或是從考量項目中排除特定元素。

此範例會使用 TicTacToe (井字) 遊戲展示 [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 和 [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) 方法。

```xaml
<StackPanel Orientation="Horizontal"
                VerticalAlignment="Center"
                HorizontalAlignment="Center" >
    <Button Content="Start Game" />
    <Button Content="Undo Movement" />
    <Grid x:Name="TicTacToeGrid" KeyDown="OnKeyDown">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="0" 
            x:Name="Cell00" />
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="0" 
            x:Name="Cell10"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="0" 
            x:Name="Cell20"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="1" 
            x:Name="Cell01"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="1" 
            x:Name="Cell11"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="1" 
            x:Name="Cell21"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="2" 
            x:Name="Cell02"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="2" 
            x:Name="Cell22"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="2" 
            x:Name="Cell32"/>
    </Grid>
</StackPanel>
```

```csharp
private void OnKeyDown(object sender, KeyRoutedEventArgs e)
{
    DependencyObject candidate = null;

    var options = new FindNextElementOptions ()
    {
        SearchRoot = TicTacToeGrid,
        NavigationStrategy = NavigationStrategyMode.Heuristic
    };

    switch (e.Key)
    {
        case Windows.System.VirtualKey.Up:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Up, options);
            break;
        case Windows.System.VirtualKey.Down:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Down, options);
            break;
        case Windows.System.VirtualKey.Left:
            candidate = FocusManager.FindNextElement(
                FocusNavigationDirection.Left, options);
            break;
        case Windows.System.VirtualKey.Right:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Right, options);
            break;
    }
    // Also consider whether candidate is a Hyperlink, WebView, or TextBlock.
    if (candidate != null && candidate is Control)
    {
        (candidate as Control).Focus(FocusState.Keyboard);
    }
}
```

使用 [FindNextElementOptions](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.findnextelementoptions) 以進一步自訂識別焦點候選項目的方式。 這個物件提供下列屬性：

- [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot) - 將搜尋焦點瀏覽候選項目的範圍設為此 DependecyObject 的下層。 Null 表示會從視覺化樹狀結構的根開始搜尋。

> [!Important] 
> 若一或多個轉換已套用至 **SearchRoot** 的子系，將其置於方向區域之外，這些元素仍然會被認為是候選項目。

- [ExclusionRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) - 會使用「虛擬」的週框識別焦點瀏覽候選項目，所有重疊的物件都會排除在瀏覽焦點之外。 這個矩形只適用於計算，而不會新增到視覺化樹狀結構。
- [HintRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) - 會使用「虛擬」的週框識別焦點瀏覽候選項目，取得最有可能取得焦點的元素。 這個矩形只適用於計算，而不會新增到視覺化樹狀結構。
- [XYFocusNavigationStrategyOverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_XYFocusNavigationStrategyOverride) - 用來移動焦點的瀏覽策略。

下圖顯示這些概念的一部分。 

當元素 B 取得焦點時，FindNextElement 會在向右巡覽時識別 I 作為焦點候選項目。 原因如下：
- 因為 [HintRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) 位於 A，因此啟動參考為 A 而非 B
- C 並非候選項目，因為 MyPanel 已識別為 [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot)
- F 並非候選項目，因為 [ExclusionRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) 與它重疊

![使用瀏覽提示自訂焦點瀏覽行為](images/keyboard/navigation-hints.png)

*使用瀏覽提示自訂焦點瀏覽行為*

## <a name="navigation-focus-events"></a>瀏覽焦點事件

### <a name="nofocuscandidatefound-event"></a>NoFocusCandidateFound event

[UIElement.NoFocusCandidateFound](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_NoFocusCandidateFound) 事件會在按下 TAB 鍵或方向鍵，並且在指定方向沒有焦點候選項目時引發。 此事件不會為 [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 引發。

因為這是路由事件，它會從已設定焦點元素，向上到連續父物件，一直到物件樹的根向上反昇。 這可讓您在任何適當地方處理事件。

<a name="split-view-code-sample"></a>

在這裡，我們顯示 Grid 如何在使用者嘗試在最左側的可設為焦點的控制項上向左移動焦點時開啟 [SplitView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview) (請參閱[為 Xbox 和 TV 設計](../devices/designing-for-tv.md#navigation-pane))。

```xaml
<Grid NoFocusCandidateFound="OnNoFocusCandidateFound">
...
</Grid>
```

```csharp
private void OnNoFocusCandidateFound (
    UIElement sender, NoFocusCandidateFoundEventArgs args)
{
    if(args.NavigationDirection == FocusNavigationDirection.Left)
    {
        if(args.InputDevice == FocusInputDeviceKind.Keyboard ||
        args.InputDevice == FocusInputDeviceKind.GameController )
            {
                OpenSplitPaneView();
            }
        args.Handled = true;
    }
}
```

### <a name="gotfocus-and-lostfocus-events"></a>GotFocus 和 LostFocus 事件
[UIElement.GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 和 [UIElement.LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) 事件會在元素取得或失去焦點時各自引發。 此事件不會為 [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 引發。

因為這些是路由事件，他們會從已設定焦點元素，向上到連續父物件，一直到物件樹的根向上反昇。 這可讓您在任何適當地方處理事件。

### <a name="gettingfocus-and-losingfocus-events"></a>GettingFocus 和 LosingFocus 事件

[UIElement.GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 和 [UIElement.LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 事件會在各自的 [UIElement.GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 和 [UIElement.LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) 事件之前引發。 

因為這些是路由事件，他們會從已設定焦點元素，向上到連續父物件，一直到物件樹的根向上反昇。 因為這會在焦點變更發生之前引發，您可以重新導向或取消焦點變更。

[GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 和 [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 為同步事件，因此焦點不會在這些事件反昇時移動。 但是，[GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 和 [LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) 為非同步事件，表示無法保證焦點不會在處理常式執行前再次移動。

或焦點透過呼叫 [Control.Focus](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_) 移動，[GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 便會在呼叫期間引發，[GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) 則會在呼叫之後引發。

焦點瀏覽目標可在 [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 和 [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 期間透過 [GettingFocusEventArgs.NewFocusedElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_NewFocusedElement) 屬性變更。 即使已變更目標，事件仍然會反昇，而且目標仍可再次變更。

若要避免重新進入問題，若您嘗試在這些事件反昇時移動焦點 (使用 [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) 或 [Control.Focus](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_))，便會擲回例外狀況。

這些事件無論移動焦點的理由為何都會引發 (包含 Tab 瀏覽、方向瀏覽和程式設計瀏覽)。

以下是焦點事件的執行順序：

1.  [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 若焦點重設回失去焦點的元素或 [TryCancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.losingfocuseventargs#Windows_UI_Xaml_Input_LosingFocusEventArgs_TryCancel) 成功，便不會引發其他事件。
2.  [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 若焦點重設回失去焦點的元素或 [TryCancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_TryCancel) 成功，便不會引發其他事件。
3.  [LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus)
4.  [GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus)

下圖顯示，當從 A 向右移動時，XYFocus 選擇 B4 為候選項目。 B4 然後引發 GettingFocus 事件，這時 ListView 有機會將焦點重新指派到 B3。

![在 GettingFocus 事件上變更焦點瀏覽](images/keyboard/focus-events.png)

*在 GettingFocus 事件上變更焦點瀏覽*

在這裡，我們顯示如何處理 [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) 事件和重新導向焦點。

```XAML
<StackPanel Orientation="Horizontal">
    <Button Content="A" />
    <ListView x:Name="MyListView" SelectedIndex="2" GettingFocus="OnGettingFocus">
        <ListViewItem>LV1</ListViewItem>
        <ListViewItem>LV2</ListViewItem>
        <ListViewItem>LV3</ListViewItem>
        <ListViewItem>LV4</ListViewItem>
        <ListViewItem>LV5</ListViewItem>
    </ListView>
</StackPanel>
```

```csharp
private void OnGettingFocus(UIElement sender, GettingFocusEventArgs args)
{
    //Redirect the focus only when the focus comes from outside of the ListView.
    // move the focus to the selected item
    if (MyListView.SelectedIndex != -1 && 
        IsNotAChildOf(MyListView, args.OldFocusedElement))
    {
        var selectedContainer = 
            MyListView.ContainerFromItem(MyListView.SelectedItem);
        if (args.FocusState == 
            FocusState.Keyboard && 
            args.NewFocusedElement != selectedContainer)
        {
            args.TryRedirect(
                MyListView.ContainerFromItem(MyListView.SelectedItem));
            args.Handled = true;
        }
    }
}
```

在這裡，我們顯示如何為 [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar) 處理 [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) 事件，並在功能表關閉時設定焦點。

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
     <AppBarButton Icon="Back" Label="Back" />
     <AppBarButton Icon="Stop" Label="Stop" />
     <AppBarButton Icon="Play" Label="Play" />
     <AppBarButton Icon="Forward" Label="Forward" />

     <CommandBar.SecondaryCommands>
         <AppBarButton Icon="Like" Label="Like" />
         <AppBarButton Icon="Share" Label="Share" />
     </CommandBar.SecondaryCommands>
 </CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (MyCommandBar.IsOpen == true && 
        IsNotAChildOf(MyCommandBar, args.NewFocusedElement))
    {
        if (args.TryCancel())
        {
            args.Handled = true;
        }
    }
}
```

## <a name="find-the-first-and-last-focusable-element"></a>尋找第一個和最後一個可設定焦點元素 

[FocusManager.FindFirstFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindFirstFocusableElement_Windows_UI_Xaml_DependencyObject_) 和 [FocusManager.FindLastFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindLastFocusableElement_Windows_UI_Xaml_DependencyObject_) 方法會將焦點移動到物件範圍 ([UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 的元素樹狀結構或 [TextElement](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.documents.textelement) 的文字樹狀結構) 中的第一個或最後一個可設定焦點元素。 在呼叫中指定的範圍 (若引數為 Null，則範圍為目前的視窗)。

若在範圍中無法識別焦點候選項目，則會傳回 Null。

在這裡，我們顯示如何指定 CommandBar 的按鈕具有迴繞方向行為 (請參閱[鍵盤互動](keyboard-interactions.md#popup-ui))。

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
    <AppBarButton Icon="Back" Label="Back" />
    <AppBarButton Icon="Stop" Label="Stop" />
    <AppBarButton Icon="Play" Label="Play" />
    <AppBarButton Icon="Forward" Label="Forward" />

    <CommandBar.SecondaryCommands>
        <AppBarButton Icon="Like" Label="Like" />
        <AppBarButton Icon="ReShare" Label="Share" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (IsNotAChildOf(MyCommandBar, args.NewFocussedElement))
    {
        DependencyObject candidate = null;
        if (args.Direction == FocusNavigationDirection.Left)
        {
            candidate = FocusManager.FindLastFocusableElement(MyCommandBar);
        }
        else if (args.Direction == FocusNavigationDirection.Right)
        {
            candidate = FocusManager.FindFirstFocusableElement(MyCommandBar);
        }
        if (candidate != null)
        {
            args.NewFocusedElement = candidate;
            args.Handled = true;
        }
    }
}
```

## <a name="related-articles"></a>相關文章

- [適用於鍵盤、遊戲台、遠端控制與協助工具的焦點瀏覽](focus-navigation.md)
- [鍵盤互動](keyboard-interactions.md)
- [鍵盤協助工具](../accessibility/keyboard-accessibility.md)