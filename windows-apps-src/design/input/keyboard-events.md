---
author: Karl-Bridge-Microsoft
Description: Respond to keystroke actions from hardware or software keyboards in your apps using both keyboard and class event handlers.
title: 鍵盤事件
ms.assetid: ac500772-d6ed-4a3a-825b-210a9c3c8f59
label: Keyboard events
template: detail.hbs
keywords: keyboard, gamepad, remote, accessibility, navigation, focus, text, input, user interactions, key up, key down, 鍵盤, 遊戲台, 遠端, 協助工具, 瀏覽, 焦點, 文字, 輸入, 使用者互動, 向上鍵, 向下鍵
ms.author: kbridge
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4a9abc16a4992dedead598f96061811c82c5a5c5
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2018
ms.locfileid: "6452634"
---
# <a name="keyboard-events"></a>鍵盤事件

## <a name="keyboard-events-and-focus"></a>鍵盤事件和焦點

硬體和觸控式鍵盤都有可能發生以下的鍵盤事件。

| 事件                                      | 描述                    |
|--------------------------------------------|--------------------------------|
| [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) | 按下按鍵時發生。  |
| [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)     | 放開按鍵時發生。 |

> [!IMPORTANT]
> 有些 Windows 執行階段控制項可在內部處理輸入事件。 在這種情況下，因為事件接聽程式不會叫用相關處理常式，所以看起來像是沒有發生輸入事件。 這個按鍵子集通常是由類別處理常式處理，為基本鍵盤協助工具提供內建支援。 例如，[**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 類別會覆寫空格鍵與 Enter 鍵的 [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982) 事件 (以及 [**OnPointerPressed**](https://msdn.microsoft.com/library/windows/apps/hh967989))，並將這些事件路由傳送到控制項的 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件。 如果按下按鍵是由控制項類別處理，就不會引發 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 與 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。  
> 這樣會提供等同叫用按鈕的內建鍵盤，類似使用手指點選或使用滑鼠按一下。 不是空格鍵或 Enter 鍵的按鍵仍會引發 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 與 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。 如需以類別為基礎的事件處理如何運作的詳細資訊，請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584) (特別是＜控制項中的輸入事件處理常式＞一節)。


您 UI 中的控制項只有在取得輸入焦點時才會產生鍵盤事件。 個別控制項會在使用者直接按一下或點選配置上的控制項時取得焦點，或是在內容區域內使用 Tab 鍵進入 Tab 順序時取得焦點。

您也可以呼叫控制項的 [**Focus**](https://msdn.microsoft.com/library/windows/apps/hh702161) 方法來強制取得焦點。 這種方式在實作快速鍵時很必要，因為 UI 載入時預設並沒有設定鍵盤焦點。 如需詳細資訊，請參閱這個主題中後面的**快速鍵範例**。

為了讓控制項接收輸入焦點，必須啟用控制項並顯示它，而且 [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/br209422) 與 [**HitTestVisible**](https://msdn.microsoft.com/library/windows/apps/br208933) 屬性值必須為 **true**。 這是大多數控制項的預設狀態。 當控制項取得輸入焦點時，它就可以引發和回應鍵盤輸入事件，如這個主題後面所述。 處理 [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) 和 [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943) 事件也可以回應取得或失去焦點的控制項。

根據預設，控制項的 Tab 順序即為它們在 Extensible Application Markup Language (XAML) 中出現的順序。 不過，使用 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/br209461) 屬性就可以修改這個順序。 如需詳細資訊，請參閱[實作鍵盤協助工具](https://msdn.microsoft.com/library/windows/apps/hh868161)。

## <a name="keyboard-event-handlers"></a>鍵盤事件處理常式


輸入事件處理常式實作一個提供下列資訊的委派：

-   事件發送者。 發送者會回報附加了事件處理常式的物件。
-   事件資料。 以鍵盤事件來說，該資料將會是 [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072) 的執行個體。 處理常式的委派是 [**KeyEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227904)。 對大多數處理常式案例來說，最相關的 **KeyRoutedEventArgs** 屬性是 [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074)，也有可能是 [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075)。
-   [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810)。 由於鍵盤事件是路由事件，因此事件資料會提供 **OriginalSource**。 如果您是刻意讓物件透過物件樹反昇，有時 **OriginalSource** (而不是發送者) 就會成為較為重要的物件。 不過這需要視您的設計而定。 如需如何使用 **OriginalSource** 而非發送者的相關資訊，請參閱這個主題的＜鍵盤路由事件＞一節，或參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584)。

### <a name="attaching-a-keyboard-event-handler"></a>附加鍵盤事件處理常式

您可以為任何物件附加鍵盤事件處理函式，只要該事件是該物件的成員即可。 這包含任何 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 衍生類別。 下列 XAML 範例示範如何為 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 附加 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件的處理常式。

```xaml
<Grid KeyUp="Grid_KeyUp">
  ...
</Grid>
```

您也可以在程式碼中附加事件處理常式。 如需詳細資訊，請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584)。

### <a name="defining-a-keyboard-event-handler"></a>定義鍵盤事件處理常式

下面的範例示範前面範例中附加的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件處理常式的不完整事件處理常式定義。

```csharp
void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    //handling code here
}
```

```vb
Private Sub Grid_KeyUp(ByVal sender As Object, ByVal e As KeyRoutedEventArgs)
    ' handling code here
End Sub
```

```c++
void MyProject::MainPage::Grid_KeyUp(
  Platform::Object^ sender,
  Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
  {
      //handling code here
  }
```

### <a name="using-keyroutedeventargs"></a>使用 KeyRoutedEventArgs

所有鍵盤事件都是使用 [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072) 代表事件資料，**KeyRoutedEventArgs** 包含下列屬性：

-   [**按鍵**](https://msdn.microsoft.com/library/windows/apps/hh943074)
-   [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075)
-   [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073)
-   [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) (繼承自 [**RoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208809))

### <a name="key"></a>按鍵

如果按下按鍵，會引發 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 事件。 同樣的，如果放開按鍵，會引發 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。 通常您接聽事件是為了處理特定的按鍵值。 若要判斷按下或放開的是哪一個按鍵，請檢查事件資料中的 [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074) 值。 **Key** 會傳回 [**VirtualKey**](https://msdn.microsoft.com/library/windows/apps/br241812) 值。 **VirtualKey** 列舉包括所有受支援的按鍵。

### <a name="modifier-keys"></a>輔助按鍵

輔助按鍵是使用者通常會與其他按鍵一起按的按鍵，如 Ctrl 或 Shift。 您的應用程式可以使用這些按鍵組合當作鍵盤快速鍵來叫用應用程式命令。

在您的 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 與 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件處理常式中使用程式碼，即可偵測快速鍵組合。 然後，您可以追蹤想了解的輔助按鍵按下狀態。 當非輔助按鍵發生鍵盤事件時，您可以同時檢查輔助按鍵是否處於按下狀態。

> [!NOTE]
> Alt 鍵以 **VirtualKey.Menu** 值表示。

 

### <a name="shortcut-keys-example"></a>快速鍵範例


以下範例示範如何實作快速鍵。 在這個範例中，使用者可以使用「播放」、「暫停」以及「停止」按鈕或 Ctrl+P、Ctrl+A 以及 Ctrl+S 鍵盤快速鍵來控制媒體的播放。 按鈕 XAML 使用工具提示以及按鈕標籤中的 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/br209081) 屬性顯示捷徑。 這個自我說明文件對於提高應用程式的可用性及協助性是很重要的。 如需詳細資訊，請參閱[鍵盤協助工具](https://msdn.microsoft.com/library/windows/apps/mt244347)。

另請注意，頁面載入時會將輸入焦點設給本身。 如果沒有這個步驟，就沒有控制項會取得初始輸入焦點，而且除非使用者手動設定輸入焦點 (例如，按 Tab 鍵或按一下控制項)，否則應用程式將無法引發輸入事件。

```xaml
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>

  </StackPanel>

</Grid>
```

```c++
//showing implementations but not header definitions
void MainPage::OnNavigatedTo(NavigationEventArgs^ e)
{
    (void) e;    // Unused parameter
    this->Loaded+=ref new RoutedEventHandler(this,&amp;MainPage::ProgrammaticFocus);
}
void MainPage::ProgrammaticFocus(Object^ sender, RoutedEventArgs^ e) {
    this->Focus(Windows::UI::Xaml::FocusState::Programmatic);
}

void KeyboardSupport::MainPage::MediaButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    FrameworkElement^ fe = safe_cast<FrameworkElement^>(sender);
    if (fe->Name == "PlayButton") {DemoMovie->Play();}
    if (fe->Name == "PauseButton") {DemoMovie->Pause();}
    if (fe->Name == "StopButton") {DemoMovie->Stop();}
}


bool KeyboardSupport::MainPage::IsCtrlKeyPressed()
{
    auto ctrlState = CoreWindow::GetForCurrentThread()->GetKeyState(VirtualKey::Control);
    return (ctrlState & CoreVirtualKeyStates::Down) == CoreVirtualKeyStates::Down;
}

void KeyboardSupport::MainPage::Grid_KeyDown(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = true;
}


void KeyboardSupport::MainPage::Grid_KeyUp(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (IsCtrlKeyPressed()) {
        if (e->Key==VirtualKey::P) { DemoMovie->Play(); }
        if (e->Key==VirtualKey::A) { DemoMovie->Pause(); }
        if (e->Key==VirtualKey::S) { DemoMovie->Stop(); }
    }
}
```

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Set the input focus to ensure that keyboard events are raised.
    this.Loaded += delegate { this.Focus(FocusState.Programmatic); };
}

private void MediaButton_Click(object sender, RoutedEventArgs e)
{
    switch ((sender as Button).Name)
    {
        case "PlayButton": DemoMovie.Play(); break;
        case "PauseButton": DemoMovie.Pause(); break;
        case "StopButton": DemoMovie.Stop(); break;
    }
}

private static bool IsCtrlKeyPressed()
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
}

private void Grid_KeyDown(object sender, KeyRoutedEventArgs e)
{
    if (IsCtrlKeyPressed())
    {
        switch (e.Key)
        {
            case VirtualKey.P: DemoMovie.Play(); break;
            case VirtualKey.A: DemoMovie.Pause(); break;
            case VirtualKey.S: DemoMovie.Stop(); break;
        }
    }
}
```

```VisualBasic
Private isCtrlKeyPressed As Boolean
Protected Overrides Sub OnNavigatedTo(e As Navigation.NavigationEventArgs)

End Sub

Private Function IsCtrlKeyPressed As Boolean
    Dim ctrlState As CoreVirtualKeyStates = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    Return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
End Function

Private Sub Grid_KeyDown(sender As Object, e As KeyRoutedEventArgs)
    If IsCtrlKeyPressed() Then
        Select Case e.Key
            Case Windows.System.VirtualKey.P
                DemoMovie.Play()
            Case Windows.System.VirtualKey.A
                DemoMovie.Pause()
            Case Windows.System.VirtualKey.S
                DemoMovie.Stop()
        End Select
    End If
End Sub

Private Sub MediaButton_Click(sender As Object, e As RoutedEventArgs)
    Dim fe As FrameworkElement = CType(sender, FrameworkElement)
    Select Case fe.Name
        Case "PlayButton"
            DemoMovie.Play()
        Case "PauseButton"
            DemoMovie.Pause()
        Case "StopButton"
            DemoMovie.Stop()
    End Select
End Sub
```

> [!NOTE]
> 在 XAML 中設定 [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/hh759762) 或 [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/hh759763) 可提供字串資訊，其中記載用於叫用該特定動作的快速鍵。 Microsoft UI 自動化用戶端 (例如朗讀程式) 會擷取此資訊，通常直接提供給使用者。
>
> 設定 **AutomationProperties.AcceleratorKey** 或 **AutomationProperties.AccessKey** 本身不會有任何動作。 您還是需要附加 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 或 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件的處理常式，才能實際在應用程式中實作鍵盤快速鍵行為。 另外，也不會自動提供便捷鍵的底線文字裝飾。 如果您希望在 UI 中顯示有底線的文字，必須以內嵌 [**Underline**](https://msdn.microsoft.com/library/windows/apps/br209982) 格式明確地為助憶鍵中的特定鍵加上文字底線。

 

## <a name="keyboard-routed-events"></a>鍵盤路由事件


有一些特定事件是路由事件，包括 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 與 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)。 路由事件使用事件反昇路由策略。 反昇路由策略表示事件源自於子物件，並接著反昇路由到物件樹中相繼的父物件。 這是處理相同事件並與相同事件資料互動的機會。

以下列 XAML 範例為例，它會處理一個 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 和兩個 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 物件的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。 在這個情況下，如果在任一 **Button** 物件具有焦點時放開按鍵，則會引發 **KeyUp** 事件。 接著，事件會反昇到父 **Canvas**。

```xaml
<StackPanel KeyUp="StackPanel_KeyUp">
  <Button Name="ButtonA" Content="Button A"/>
  <Button Name="ButtonB" Content="Button B"/>
  <TextBlock Name="statusTextBlock"/>
</StackPanel>
```

下面的範例示範如何實作前一個範例中對應的 XAML 內容的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件處理常式。

```csharp
void StackPanel_KeyUp(object sender, KeyRoutedEventArgs e)
{
    statusTextBlock.Text = String.Format(
        "The key {0} was pressed while focus was on {1}",
        e.Key.ToString(), (e.OriginalSource as FrameworkElement).Name);
}
```

請注意，前面處理常式中有用到 [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) 屬性。 這裡的 **OriginalSource** 會回報引發事件的物件。 物件不可以是 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)，因為 **StackPanel** 不是控制項而且不可以取得焦點。 在 **StackPanel** 內的兩個按鈕中，只有一個可能引發事件，但是是哪一個呢？ 如果您是在處理父物件上的事件，可以使用 **OriginalSource** 來辨別實際的事件來源物件。

### <a name="the-handled-property-in-event-data"></a>事件資料中的 Handled 屬性

視您的事件處理策略而定，您可能會只想使用一個事件處理常式來應對反昇事件。 例如，如果您有附加到其中一個 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 控制項的特定 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 處理常式，該處理常式會先使用它來處理該事件。 在這個情況下，您可能不想讓父面板也同時處理事件。 這時，您可以在事件資料中使用 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 屬性。

使用路由事件資料類別中的 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 屬性是為了報告先前在事件路由上登錄的另一個處理常式已經執行。 這會影響路由事件系統的行為。 將事件處理常式中的 **Handled** 設成 **true** 時，該事件會停止路由而不會傳送給相繼的父元素。

### <a name="addhandler-and-already-handled-keyboard-events"></a>AddHandler 和已處理的鍵盤事件

您可以使用一項特別的技術，以附加可以在已標示為處理過的事件上作用的處理常式。 這項技術使用 [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) 方法來註冊處理常式，而不是使用 XAML 屬性或語言特定的語法新增處理常式 (例如 C# 中的 +=)。

這項技術的一般限制在於 **AddHandler** API 是採用一個 [**RoutedEvent**](https://msdn.microsoft.com/library/windows/apps/br208808) 類型的參數來識別相關的路由事件。 並非所有路由事件都提供 **RoutedEvent** 識別項，因此這項考量也就影響到哪些路由事件仍然可以在 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 案例中處理。 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 與 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件在 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 上已有路由事件識別項 ([**KeyDownEvent**](https://msdn.microsoft.com/library/windows/apps/hh702416) 與 [**KeyUpEvent**](https://msdn.microsoft.com/library/windows/apps/hh702418))。 不過，其他事件 (例如 [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706)) 並沒有路由事件識別項，因此也就不能與 **AddHandler** 技術搭配使用。

### <a name="overriding-keyboard-events-and-behavior"></a>覆寫鍵盤事件和行為

您可以覆寫特定控制項 (例如 [**GridView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridView)) 的按鍵事件，為各種輸入裝置 (包括鍵盤與遊戲台) 提供一致的焦點瀏覽。

在下列範例中，我們的控制項分成子類別，並覆寫 KeyDown 行為，將焦點移至 GridView 內容按下任一方向鍵時。

```csharp
public class CustomGridView : GridView
  {
    protected override void OnKeyDown(KeyRoutedEventArgs e)
    {
      // Override arrow key behaviors.
      if (e.Key != Windows.System.VirtualKey.Left && e.Key !=
        Windows.System.VirtualKey.Right && e.Key !=
          Windows.System.VirtualKey.Down && e.Key !=
            Windows.System.VirtualKey.Up)
              base.OnKeyDown(e);
      else
        FocusManager.TryMoveFocus(FocusNavigationDirection.Down);
    }
  }
```

> [!NOTE]
> 如果使用 GridView 只是為了版面配置，請考慮使用其他控制項，例如 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsControl) 與 [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsWrapGrid)。

## <a name="commanding"></a>命令

少數 UI 元素提供命令功能的內建支援。 命令功能在相關實作中使用輸入相關路由事件。 它會叫用單一命令處理常式來處理相關的 UI 輸入，例如特定指標動作或特定快速鍵。

如果某個 UI 元素擁有命令功能，請考慮使用它的命令 API，而非任何個別輸入事件。 如需詳細資訊，請參閱 [**ButtonBase.Command**](https://msdn.microsoft.com/library/windows/apps/br227740)。

您也可實作 [**ICommand**](https://msdn.microsoft.com/library/windows/apps/br227885) 來封裝從一般事件處理常式叫用的命令功能。 透過這種方式，即使沒有可用的 **Command** 屬性，您還是可以使用命令功能。

## <a name="text-input-and-controls"></a>文字輸入和控制項

特定控制項會以它們自己的處理方式應對鍵盤事件。 例如，[**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 這個控制項的設計是擷取使用鍵盤輸入的文字，然後以視覺化方式呈現文字。 它在自己的邏輯中使用 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 與 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 來擷取按鍵輸入動作，然後如果文字真的變更了，就會引發它自己的 [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706) 事件。

一般來說，您仍然可以將 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 和 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 的處理常式加到 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)，或加到任何要用來處理文字輸入的相關控制項。 不過，基於控制項本身的設計目的，控制項可能不會對透過按鍵事件導向它的所有按鍵值都提供回應。 每個控制項都有它的特定行為。

例如，[**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736) ([**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 的基礎類別) 會處理 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)，這樣它就可以檢查空格鍵或 Enter 鍵。 **ButtonBase** 將 **KeyUp** 視為等同滑鼠左鍵，可以引發 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件。 事件的處理會在 **ButtonBase** 覆寫虛擬方法 [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983) 時完成。 在實作中，它會將 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 設定成 **true**。 以空格鍵為例，結果是接聽按鍵事件的任何父按鈕，都不會收到自己的處理常式的已處理事件。

另一個範例是 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)。 **TextBox** 並不將某些按鍵 (例如方向鍵) 視為文字，而是視為控制項 UI 特定的行為。 **TextBox** 會將這些事件案例標示為已處理。

自訂控制項可以透過覆寫 [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982) / [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983)，以實作它們自己類似的按鍵事件覆寫行為。 如果您的自訂控制項會處理特定的快速鍵，或具有類似於針對 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 所述之情況的控制項或焦點行為，就應該將這個邏輯放入您自己的 **OnKeyDown** / **OnKeyUp** 覆寫中。

## <a name="the-touch-keyboard"></a>觸控式鍵盤

文字輸入控制項會自動支援觸控式鍵盤。 當使用者使用觸控輸入將輸入焦點設為文字控制項時，觸控式鍵盤會自動出現。 當輸入焦點不在文字控制項時，觸控式鍵盤就會隱藏起來。

當觸控式鍵盤出現時，它會自動定位您的 UI，以確保取得焦點的元素保持永遠可見。 這可能會造成 UI 中其他重要的區域跑到螢幕外。 不過，您可以停用預設行為，並在觸控式鍵盤出現時調整 UI。 如需詳細資訊，請參閱[觸控式鍵盤範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)。

如果建立需要文字輸入但不是從標準輸入控制項衍生的自訂控制項，您可以實作正確的 UI 自動化控制項模式來加入對觸控式鍵盤的支援。 如需詳細資訊，請參閱[觸控式鍵盤範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)。

按下觸控式鍵盤上的按鍵，就像按下硬體鍵盤上的按鍵一樣，都會引發 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 與 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。 不過，觸控式鍵盤將不會引發 Ctrl+A、Ctrl+Z、Ctrl+X、Ctrl+C 以及 Ctrl+V 的輸入事件，因為它們是保留給輸入控制項的文字操作。

您可以設定文字控制項的輸入範圍，使其符合您預期使用者輸入的資料類型，讓使用者在您的應用程式中輸入資料時更加快速方便。 輸入範圍會提供控制項所預期之文字輸入類型的提示，讓系統可以為該輸入類型提供專用的觸控式鍵盤配置。 例如，如果文字方塊只用來輸入 4 位數 PIN，請將 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 屬性設定為 [**Number**](https://msdn.microsoft.com/library/windows/apps/hh702028)。 這會告訴系統顯示數字鍵台配置，方便使用者輸入 PIN。 如需詳細資訊，請參閱[使用輸入範圍來變更觸控式鍵盤](https://msdn.microsoft.com/library/windows/apps/mt280229)。

## <a name="related-articles"></a>相關文章

**開發人員**
* [鍵盤互動](keyboard-interactions.md)
* [識別輸入裝置](identify-input-devices.md)
* [回應觸控式鍵盤的出現](respond-to-the-presence-of-the-touch-keyboard.md)

**設計人員**
* [鍵盤設計指導方針](https://msdn.microsoft.com/library/windows/apps/hh972345)

**範例**
* [觸控式鍵盤範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
* [基本輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延遲輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [焦點視覺效果範例](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**封存範例**
* [輸入範例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [輸入：裝置功能範例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [輸入：觸控式鍵盤範例](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [回應螢幕小鍵盤外觀的範例](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [XAML 文字編輯範例](http://go.microsoft.com/fwlink/p/?LinkID=251417)
 

 
