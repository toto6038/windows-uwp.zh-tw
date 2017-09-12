---
author: Karl-Bridge-Microsoft
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 8389d1f089d507a087693879a793b95572d7860b
ms.sourcegitcommit: 5ece992c31870df4c089360ef47501bd4ce14fa9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2017
---
# <a name="managing-focus-navigation"></a>管理焦點瀏覽

有許多輸入 (即鍵盤)、協助工具如 Windows 朗讀程式、遊戲台與遙控器都共用在應用程式 UI 中移動焦點視覺效果的通用基礎機制。 請閱讀[鍵盤互動](keyboard-interactions.md)文件及[針對 Xbox 和電視進行設計](designing-for-tv.md#xy-focus-navigation-and-interaction)文件，深入了解焦點視覺效果和瀏覽。

以下提供應用於在應用程式 UI 中移動焦點的不挑輸入方式，可讓您撰寫程式碼一次，即可在多個輸入類型執行您的應用程式。

## <a name="navigation-strategies-properties-to-fine-tune-focus-movements"></a>微調焦點移動的瀏覽策略屬性

使用 XYFocus 瀏覽策略屬性，指定依據按下的方向鍵哪個控制項應該會收到焦點。 這些屬性是：

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

這些屬性有類型 **XYFocusNavigationStrategy** 的類型值。 如果設為 **Auto**，元素行為是根據元素的上階。 如果所有元素都設定為 **Auto**，則使用 **Projection**。

這些瀏覽策略適用於鍵盤、遊戲台與遙控器。

### <a name="projection"></a>Projection

Projection 策略會在瀏覽方向投影目前取得焦點元素的邊緣時，將焦點移至遇到的第一個元素。

> [!NOTE]
> 其他如先前取得焦點之元素和瀏覽方向軸鄰近性等因素會影響結果。

![projection](images/keyboard/projection.png)

*焦點會根據 A 底緣的投影在向下瀏覽時從 A 移動 D*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

NavigationDirectionDistance 策略會將焦點移至最接近瀏覽方向軸的元素。

對應到瀏覽方向的週框邊緣會延伸並投影，以識別候選目標。 系統會將碰到的第一個元素視為目標。 如果有多個候選項目，則會將最靠近的元素視為目標。 如果仍有多個候選項目，則會將最上方/最左邊的元素視為候選項目。

![瀏覽方向距離](images/keyboard/navigation-direction-distance.png)

*向下瀏覽時焦點從 A 移到 C，再從 C 移到 B。*


### <a name="rectilineardistance"></a>RectilinearDistance

RectilinearDistance 策略會根據最短 2D 距離（曼哈頓計量）將焦點移至最接近的元素。 這個距離的計算方式是將每個潛在的候選項目主要距離和次要距離相加。 在平局，如果是向上或向下方向，會選取左邊的第一個元素，如果是向左或向右方向，則選取頂端的第一個元素。

在下列影像中，顯示當 A 有焦點時，焦點移至 B，因為：

-   Distance (A, B, Down) = 10 + 0 = 10
-   Distance (A, C, Down) = 0 + 30 = 30
-   Distance (A, D, Down) 30 + 0 = 30

![直線距離](images/keyboard/rectilinear-distance.png)

*使用 RectilinearDistance 策略，當 A 焦點時，焦點移至 B*

下圖顯示瀏覽策略套用到元素本身目，而不是子元素 (不同於 XYFocusKeyboardNavigation)。

例如，使用 RectilinearDistance 策略，當 E 有焦點時，按向右鍵將焦點移到 F，但使用 Projection 策略，當 D 有焦點時，按向右鍵將焦點移到 H。

請注意，不套用主要容器策略（導覽方向距離）。 它只適用於焦點在 H、F 或 G 時。

![不同的瀏覽策略](images/keyboard/different-navigation-strategies.png)

*根據套用不同的瀏覽策略，不同的焦點行為*

下列範例模擬從 Windows 10 [開始] 功能表的磚面板行為。 在本案例中，按向上和向下鍵會使用 NavigationDirectionDistance 策略，按向左和向下鍵則會使用 Projection 策略。

```XAML
<start:TileGridView
        XYFocusKeyboardNavigation ="Default"
        XYFocusUpNavigationStrategy=" NavigationDirectionDistance "
        XYFocusDownNavigationStrategy=" NavigationDirectionDistance "
        XYFocusLeftNavigationStrategy="Projection"
        XYFocusRightNavigationStrategy="Projection"
/>
```

## <a name="move-focus-programmatically"></a>以程式設計方式移動焦點

使用 [FocusManager.TryMoveFocus](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Input.FocusManager.TryMoveFocus) 方法或[FocusManager.FindNextElement](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Input.FocusManager.FindNextFocusableElement) 方法，以程式設計方式移動焦點。

TryMoveFocus 設定焦點，就像按下導覽按鍵一樣。
FindNextElement 傳回 TryMoveFocus 想移動到的元素 (DependencyObject)。

**注意** 建議您使用**FindNextElement** 方法，而不是**FindNextFocusableElement**。 FindNextFocusableElement 嘗試傳回 UIElement，但如果下一個可設定焦點元素是超連結時，它會傳回 null，因為超連結不是 UIElement。 FindNextElement 方法傳回 DependencyObject。

### <a name="find-a-focus-candidate-in-a-scope"></a>在範圍中尋找焦點候選項目

FocusManager.FindNextElement 和 FocusManager.TryMoveFocus 可讓您自訂的下一個可設定焦點元素的搜尋行為。 您可以在特定的 UI 樹狀目錄中搜尋，或排除特定導覽區域中的元素。

本範例取自井字遊戲實作，並示範如何使用 FindNextElement 和 TryMoveFocus 方法：

```XAML
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
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="0" x:Name="Cell00" />
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="0" x:Name="Cell10"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="0" x:Name="Cell20"/>
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="1" x:Name="Cell01"/>
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="1" x:Name="Cell11"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="1" x:Name="Cell21"/>
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="2" x:Name="Cell02"/>
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="2" x:Name="Cell22"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="2" x:Name="Cell32"/>
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
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Up, options);
                    break;
                case Windows.System.VirtualKey.Down:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Down, options);
                    break;
                case Windows.System.VirtualKey.Left:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Left, options);
                    break;
                case Windows.System.VirtualKey.Right:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Right, options);
                    break;
            }
         //Note you should also consider whether is a Hyperlink, WebView, or TextBlock.
            if (candidate != null && candidate is Control)
            {
                (candidate as Control).Focus(FocusState.Keyboard);
            }
        }
```

**注意** 為了支援 RTL 案例，FocusNavigationDirection.Left 和 FocusNavigationDirection.Right 合乎邏輯（近和遠） （這符合 Xaml 的其餘部分）。

使用 FindNextElementOptions 類別，可調整焦點候選項目的搜尋。 這個物件有下列屬性：

-   **SearchRoot** DependencyObject：將候選項目搜尋範圍設定到這個 DependencyObject 的子系。 Null 值代表應用程式的整個視覺化樹狀結構。
-   **ExclusionRect** Rect：從搜尋中排除項目，這些項目在呈現時與這個矩形重疊至少一個像素。
-   **HintRect** Rect：使用可設定焦點做為參照，計算潛在的候選項目。 這個矩形可讓開發人員指定另一個參考，而不是已設定焦點項目。 矩形是「虛構」矩形，只用於計算。 它永遠不會轉換成真實矩形且新增到視覺化樹狀結構。
-   **XYFocusNavigationStrategyOverride** XYFocusNavigationStrategyOverride：用來移動焦點的瀏覽策略。 類型是有下列值的列舉：None（也就是零值）、Auto、RectilinearDistance、NavigationDirectionDistance 和 Projection。
    **重要** **SearchRoot** 不用於計算呈現的（幾何）區域，或取得區域中的候選項目。 因此，如果轉換套用至 DependencyObject 的子系，將它們放在方向區域之外，這些元素仍會被認為候選項目。

FindNextElement 的選項多載不能用於 Tab 瀏覽，它僅支援方向瀏覽。

下圖顯示這些選項。

例如，當 B 有焦點時，呼叫 FindNextElement（各種不同的選項設定為指示焦點向右移）將焦點設在 I。原因如下：

1.  因為 HintRect，參考不是 B，而是 A
2.  因為潛在的候選項目應該在 MyPanel (SearchRoot)，所以排除 C
3.  排除 F，由於它與排除矩形重疊

![瀏覽提示](images/keyboard/navigation-hints.png)

*根據瀏覽提示的焦點行為*

### <a name="no-focus-candidate-available"></a>沒有任何可以使用的焦點候選項目

當使用者嘗試使用 tab 鍵或方向鍵移動焦點，但在指定的方向中沒有可能的候選項目時，會引發 UIElement.NoFocusCandidateFound 事件。 對 FocusManager.TryMoveFocus()，不會引發這個事件。

因為這是路由事件，它會從已設定焦點元素，向上到連續父物件，一直到物件樹的根向上反昇。 如此一來，您可以在任何適當地方處理事件。

<a name="split-view-code-sample"></a>下列範例示範當使用者將焦點移至最左邊可設定焦點控制項的左邊時，開啟分割檢視的方格，如[針對電視進行設計文件](designing-for-tv.md#navigation-pane)所述。

```XAML
<Grid NoFocusCandidateFound="OnNoFocusCandidateFound">  ...</Grid>
```

```csharp
private void OnNoFocusCandidateFound (UIElement sender, NoFocusCandidateFoundEventArgs args)
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

## <a name="focus-events"></a>焦點事件

[UIElement.GotFocus](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.UIElement.GotFocus) 和 UIElement.LostFocus 事件分別會在控制項取得焦點或失去焦點時引發。 對 FocusManager.TryMoveFocus()，不會引發這個事件。

因為這是路由事件，它會從已設定焦點元素，向上到連續父物件，一直到物件樹的根向上反昇。 如此一來，您可以在任何適當地方處理事件。

**UIElement.GettingFocus** 和 **UIElement.LosingFocus** 事件也會從控制項反昇，但是在焦點變更*之前*。 這讓 app 有機會重新導向或取消焦點變更。

請注意，GettingFocus 和 LosingFocus 是同步事件，而 GotFocus 和 LostFocus 則是非同步事件。 例如，如果因為 app 呼叫 Control.Focus() 方法，移動焦點，GettingFocus 是在呼叫期間引發，但 GotFocus 則是呼叫之後引發。

UIElement 可以連接到 GettingFocus 和 LosingFocus 事件，並在焦點移動之前變更目標（使用 NewFocusedElement 屬性）。 即使已變更目標，事件仍然會反昇，而且另一個父項可以再次變更目標。

為了避免重新進入問題，如果在這些事件反昇時，您嘗試移動焦點（使用 FocusManager.TryMoveFocus 或 Control.Focus），會擲回例外狀況。

無論焦點移動的原因（例如，tab 瀏覽、XYFocus 瀏覽或程式設計），都會引發這些事件。

下圖顯示，當從 A 向右移動時，XYFocus 選擇 LVI4 為候選項目。 LVI4 然後引發 GettingFocus 事件，這時 ListView 有機會將焦點重新指派到 LVI3。

![焦點事件](images/keyboard/focus-events.png)

*焦點事件*

此程式碼範例示範如何處理 OnGettingFocus 事件，並根據先前設定的焦點，重新導向焦點。

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
            if (MyListView.SelectedIndex != -1 && IsNotAChildOf(MyListView, args.OldFocusedElement))
            {
                var selectedContainer = MyListView.ContainerFromItem(MyListView.SelectedItem);
                if (args.FocusState == FocusState.Keyboard && args.NewFocusedElement != selectedContainer)
                {
                    args.NewFocusedElement = MyListView.ContainerFromItem(MyListView.SelectedItem);
                    args.Handled = true;
                }
            }        
  }
```

此程式碼範例示範如何處理 CommandBar 物件的 OnLosingFocus 事件，並等到 DropDown 功能表已關閉，才設定焦點。

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
          if (MyCommandBar.IsOpen == true && IsNotAChildOf(MyCommandBar, args.NewFocusedElement))
          {
              args.Cancel = true;
              args.Handled = true;
          }
}
```

### <a name="losingfocus-and-gettingfocus-event-order"></a>LosingFocus 和 GettingFocus 事件順序

LosingFocus 和 GettingFocus 是同步事件，因此在這些事件反昇時不會移動焦點。 不過，因為 LostFocus 和 GotFocus 是非同步活動，並不保證處理常式執行之前不會再次移動焦點。 唯一保證是 LostFocus 處理常式是在 GotFocus 處理常式之前執行。

以下是執行順序：

1.  同步 LosingFocus：如果 LosingFocus 將焦點重設回失去焦點的元素，或設定 Cancel=true，沒有進一步事件
2.  同步 GettingFocus：如果 GettingFocus 將焦點重設回失去焦點的元素，或設定 Cancel=true，沒有進一步事件
3.  非同步 LostFocus
4.  非同步 GotFocus

## <a name="find-the-first-and-last-focusable-element-a-namefindfirstfocusableelement"></a>尋找第一個和最後一個可設定焦點元素 <a name="findfirstfocusableelement">

**FocusManager.FindFirstFocusableElement** 和 **FocusManager.FindLastFocusableElement** 方法可以讓您將焦點移至物件範圍 (UIElement 的元素樹狀結構或 TextElement 的文字樹狀結構) 中的第一個或最後一個可設定焦點項目。 當您呼叫這些方法，指定範圍。 如果引數為 null，範圍就是目前視窗。

**注意** 如果範圍中沒有任何可設定焦點元素，這些方法可以傳回 null。

<a name="popup-ui-code-sample"></a>下列程式碼範例示範如何指定 CommandBar 的按鈕具有迴繞方向行為，如[鍵盤互動](keyboard-interactions.md#popup-ui)文章所述。

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
