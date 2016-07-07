---
author: Xansky
Description: "如果 app 未能提供適切的鍵盤功能操作，盲眼或行動不便的使用者將難以使用 app，或者根本無法使用。"
ms.assetid: DDAE8C4B-7907-49FE-9645-F105F8DFAD8B
title: "鍵盤協助工具"
label: Keyboard accessibility
template: detail.hbs
ms.sourcegitcommit: 50c37d71d3455fc2417d70f04e08a9daff2e881e
ms.openlocfilehash: c5b5ca247e3999850d7bf9b81347c201204db7e8

---

# 鍵盤協助工具  



如果 app 未能提供適切的鍵盤功能操作，盲眼或行動不便的使用者將難以使用 app，或者根本無法使用。

<span id="keyboard_navigation_among_UI_elements"/>
<span id="keyboard_navigation_among_ui_elements"/>
<span id="KEYBOARD_NAVIGATION_AMONG_UI_ELEMENTS"/>
## 使用鍵盤瀏覽 UI 元素  
如果想透過鍵盤操控某個控制項，該控制項必須擁有焦點，然後為了接收焦點 (不使用指標)，必須可以透過 Tab 瀏覽方式，在 UI 中找到這個控制項。 根據預設值，控制項的 Tab 順序與它們新增至設計介面、列在 XAML 或者通過程式設計新增至容器時的順序一樣。

大部分情況下，依據您在 XAML 定義控制項的方法排定的預設順序是最佳順序，因為這是螢幕助讀程式讀取控制項的順序。 不過，預設的順序不一定和肉眼觀察的順序一致。 實際的顯示位置可能取決於上層配置容器，以及可在子元素上設定而影響配置的某些屬性。 若要確定應用程式的 Tab 順序正確，請親自測試這種行為。 尤其是如果您的配置有格線隱喻或表格隱喻，則使用者閱讀的順序和 Tab 順序可能會不同。 這不一定是其本身的問題。 但在測試應用程式功能時要記得同時以可觸控 UI 和鍵盤存取 UI 的形式進行測試，確定您的 UI 適合在這兩種情況下使用。

您可以調整 XAML，讓 Tab 順序與視覺順序一致。 您也可以設定 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 屬性以覆寫預設的 Tab 順序，如以下使用欄優先 Tab 瀏覽的 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 版面配置範例所示。

XAML
```xml
<!--Custom tab order.-->
<Grid>
  <Grid.RowDefinitions>...</Grid.RowDefinitions>
  <Grid.ColumnDefinitions>...</Grid.ColumnDefinitions>

  <TextBlock Grid.Column="1" HorizontalAlignment="Center">Groom</TextBlock>
  <TextBlock Grid.Column="2" HorizontalAlignment="Center">Bride</TextBlock>

  <TextBlock Grid.Row="1">First name</TextBlock>
  <TextBox x:Name="GroomFirstName" Grid.Row="1" Grid.Column="1" TabIndex="1"/>
  <TextBox x:Name="BrideFirstName" Grid.Row="1" Grid.Column="2" TabIndex="3"/>

  <TextBlock Grid.Row="2">Last name</TextBlock>
  <TextBox x:Name="GroomLastName" Grid.Row="2" Grid.Column="1" TabIndex="2"/>
  <TextBox x:Name="BrideLastName" Grid.Row="2" Grid.Column="2" TabIndex="4"/>
</Grid>
```

您可以排除 Tab 順序中的控制項。 通常只要將控制項變成非互動就可以達成，例如，將它的 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/BR209419) 屬性設定成 **false**。 停用的控制項會自動從 Tab 順序中排除。 不過有時候，即使控制項未停用，您還是想從 Tab 順序排除控制項。 在這種情況下，您可以將 [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422) 屬性設定成 **false**。

根據預設，可以擁有焦點的任何元素通常會在 Tab 順序中。 但有一個例外，就是某些文字顯示類型 (例如 [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)) 可以有焦點，剪貼簿才能使用這些類型選取文字；不過，由於靜態元素本來就不應該出現在 Tab 順序中，所以 Tab 順序也不會有這些靜態元素。 它們不是以慣用的方式互動 (它們無法被叫用，也不需要文字輸入，但支援[文字控制項模式](https://msdn.microsoft.com/library/windows/desktop/Ee671194)，此模式支援尋找和調整文字選取點)。 文字不應有當焦點設定到其上時，將啟用一些可能動作的意涵。 文字元素仍然能被輔助技術偵測，並在螢幕助讀程式中大聲讀出，但這全都是依賴以實際 Tab 順序尋找這些元素以外的技術。

無論您是調整 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) 值或者使用預設順序，都適用這些規則：

* 
            [
              **TabIndex**
            ](https://msdn.microsoft.com/library/windows/apps/BR209461) 等於 0 的 UI 元素會根據 XAML 或子集合中的宣告順序，新增到 Tab 順序。
* 
            [
              **TabIndex**
            ](https://msdn.microsoft.com/library/windows/apps/BR209461) 大於 0 的 UI 元素會根據 **TabIndex** 的值，新增到 Tab 順序。
* 
            [
              **TabIndex**
            ](https://msdn.microsoft.com/library/windows/apps/BR209461) 小於 0 的 UI 元素會新增到 Tab 順序，並顯示在任何零值的前面。 這與 HTML 處理 **tabindex** 屬性的方法有潛在的不同 (且舊版的 HTML 規格不支援負 **tabindex**)。

<span id="keyboard_navigation_within_a_UI_element"/>
<span id="keyboard_navigation_within_a_ui_element"/>
<span id="KEYBOARD_NAVIGATION_WITHIN_A_UI_ELEMENT"/>
## 使用鍵盤在 UI 元素中瀏覽  
至於複合元素，務必確認可以在內含的元素之間進行合適的內部瀏覽。 複合元素可以管理目前使用中的子元素，減少所有子元素能夠擁有焦點的負擔。 這種複合元素會包含在 Tab 順序中，而且可以自行處理鍵盤瀏覽事件。 許多複合控制項都已經在控制項的事件處理中內建一些內部瀏覽邏輯。 例如，根據預設，您可以使用方向鍵瀏覽 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)、[**GridView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview)、[**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) 和 [**FlipView**](https://msdn.microsoft.com/library/windows/apps/BR242678) 控制項。

<span id="keyboard_activation"/>
<span id="KEYBOARD_ACTIVATION"/>
## 特定控制項元素的指標動作和事件的鍵盤替代方法  
確定可以按一下的 UI 元素，同樣可以利用鍵盤呼叫它。 如果想透過鍵盤操控 UI 元素，元素必須具有焦點。 只有從 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 衍生的類別才支援焦點以及 Tab 瀏覽。

對於可以呼叫的 UI 元素，請為空格鍵和 Enter 鍵實作鍵盤事件處理常式。 如此便會有完整的基本鍵盤協助工具支援，讓使用者只利用鍵盤就可以完成基本的應用程式操作；也就是說，使用者可以使用所有的互動式 UI 元素以及啟動預設的功能。

當您想在 UI 中使用的元素沒有焦點時，可以建立自己的自訂控制項。 您必須將 [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422) 屬性設定成 **true** 才能啟用焦點，而且必須建立以焦點指示器裝飾 UI 的視覺狀態來提供已設定焦點狀態的視覺指示。 不過，利用控制項組合的方式較為容易，這樣當您編寫內容時，選擇的控制項就可以處理定位點、焦點以及 Microsoft UI 自動化對等和模式。

例如，不用處理 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 上的按下指標事件，您可以將該元素包裝到 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 裡面，以取得指標、鍵盤及焦點支援。

XAML
```xml
<!--Don't do this.-->
<Image Source="sample.jpg" PointerPressed="Image_PointerPressed"/>

<!--Do this instead.-->
<Button Click="Button_Click"><Image Source="sample.jpg"/></Button>
```

<span id="keyboard_shortcuts"/>
<span id="KEYBOARD_SHORTCUTS"/>
## 鍵盤快速鍵  
除了實作應用程式的鍵盤瀏覽以及啟用功能之外，實作應用程式功能的捷徑也是不錯的做法。 Tab 瀏覽會提供基本的鍵盤支援，不過遇到複雜的表單時，可能需要加入快速鍵的支援。 這樣可以讓應用程式更加容易操作，即使同時使用鍵盤和指標裝置的人，也是如此。

「捷徑」是一種鍵盤組合，可讓使用者更有效率地存取應用程式功能，提高工作效率。 目前有兩種捷徑：

* 「便捷鍵」是連至應用程式中某部分 UI 的捷徑。 便捷鍵包含 Alt 鍵和一個字母按鍵。
* 「快速鍵」是應用程式命令的捷徑。 您的應用程式不一定會包含準確對應到命令的 UI。 快速鍵包含 Ctrl 鍵和一個字母按鍵。

請為依賴螢幕助讀程式或其他輔助技術的使用者，提供一種便利的方法，讓他們發現應用程式的快速鍵。 使用工具提示、無障礙名稱、無障礙說明或其他螢幕上的溝通方式，與快速鍵進行溝通。 至少應在 app 的說明內容中詳細記載快速鍵。

您可以將 [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) 附加屬性設定成一個描述快速鍵的字串，就可以透過螢幕助讀程式記載便捷鍵的用法。 還有一個 [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) 附加屬性，可以用於記載無助憶鍵的快速鍵用法，雖然螢幕助讀程式通常會採用相同的方式去處理這兩個屬性。 試著使用多種方法記載快速鍵的用法，包括使用工具提示、自動化屬性以及編寫說明文件。

以下範例示範如何記載媒體播放、暫停以及停止按鈕的快速鍵。

XAML
```xml
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

> [!IMPORTANT]
> 設定 [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) 或 [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) 並不會啟用鍵盤功能。 它只會向 UI 自動化架構報告應該使用哪些按鍵，以便透過輔助技術將這類資訊傳遞給使用者。 按鍵處理的實作仍然需要在程式碼中完成，而不是在 XAML 中完成。 您仍然需要為相關控制項上的 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/BR208941) 或 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/BR208942) 事件附加處理常式，才能在您的應用程式中實際實作鍵盤快速鍵行為。 另外，也不會自動提供便捷鍵的底線文字裝飾。 如果您希望在 UI 中顯示有底線的文字，必須以內嵌 [**Underline**](https://msdn.microsoft.com/library/windows/apps/BR209982) 格式明確地為助憶鍵中的特定鍵加上文字底線。

為了簡化，上面的範例省略了字串的資源使用，例如 "Ctrl+A"。 不過當地語系化時，也必須考慮快速鍵。 因為選擇做為快速鍵的按鍵時，通常取決於元素的可見文字標籤，所以這也涉及快速鍵的當地語系化。

如需實作快速鍵的指引，請參閱《Windows 使用者體驗互動指導方針》中的[快速鍵](http://go.microsoft.com/fwlink/p/?linkid=221825)。

<span id="Implementing_a_key_event_handler"/>
<span id="implementing_a_key_event_handler"/>
<span id="IMPLEMENTING_A_KEY_EVENT_HANDLER"/>
### 實作按鍵事件處理常式  
像按鍵事件這種輸入事件，都使用一種稱為「路由事件」的事件概念。 路由事件可以透過複合控制項的子元素反昇，因此通用控制項父元素可以處理多個子元素的事件。 如果控制項包含多個複合組件，而這些組件的設計無法擁有焦點或無法成為 Tab 順序一部分，這時候很適合使用這種事件模型，為控制項定義快速鍵動作。

如需示範如何撰寫包含輔助按鍵 (例如 Ctrl 鍵) 檢查的按鍵事件處理常式的程式碼範例，請參閱[鍵盤互動](https://msdn.microsoft.com/library/windows/apps/Mt185607)。

<span id="Keyboard_navigation_for_custom_controls"/>
<span id="keyboard_navigation_for_custom_controls"/>
<span id="KEYBOARD_NAVIGATION_FOR_CUSTOM_CONTROLS"/>
## 自訂控制項的鍵盤瀏覽  
如果子元素彼此之間存在空間關係時，建議您使用方向鍵當作鍵盤快速鍵在子元素之間瀏覽。 如果樹狀檢視節點具有獨立的子項目來處理展開折疊以及節點啟動，請使用向左鍵或向右鍵，提供鍵盤展開折疊功能。 如果您有一個方向控制項可以在控制項內容中支援方向瀏覽，請使用適當的方向鍵。

通常，您是在類別邏輯包含 [**OnKeyDown**](https://msdn.microsoft.com/en-us/library/windows/apps/hh967982.aspx) 和 [**OnKeyUp**](https://msdn.microsoft.com/en-us/library/windows/apps/hh967983.aspx) 方法的覆寫，實作自訂控制項的自訂按鍵處理。

<span id="An_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="an_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="AN_EXAMPLE_OF_A_VISUAL_STATE_FOR_A_FOCUS_INDICATOR"/>
## 焦點指示器的視覺狀態範例  
我們稍早有提到任何可以讓使用者將它當作焦點的自訂控制項，都應該有視覺焦點指示器。 通常該焦點指示器就像在控制項的一般週框矩形外圍再緊接著繪製一個矩形一樣簡單。 視覺焦點的 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 是控制項範本中其餘控制項組合的對等元素，但是它一開始的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) 值是設成 **Collapsed**，因為控制項還沒有被當作焦點。 然後，當控制項確實被當作焦點時，系統就會叫用視覺狀態將焦點視覺效果的 **Visibility** 設成 **Visible**。 一旦焦點移到其他地方，系統就會呼叫另一個視覺狀態，然後 **Visibility** 就會變成 **Collapsed**。

所有預設 XAML 控制項聚焦時都會顯示適當的視覺焦點指示器 (如果可以聚焦)。 根據使用者選取的佈景主題，外觀也可能會不同 (尤其是當使用者使用高對比模式時)。如果您在 UI 使用 XAML 控制項，且沒有取代控制項範本，則不需要執行額外的動作在行為和顯示正常的控制項取得視覺焦點指示器。 不過，如果您打算重新範本化控制項，或者想了解 XAML 控制項如何提供視覺焦點指示器，本節剩餘的內容將說明如何透過 XAML 和控制項邏輯進行此工作。

下列是一些來自預設 XAML 範本的 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) XAML 範例。

XAML
```xml
<ControlTemplate TargetType="Button">
...
    <Rectangle
      x:Name="FocusVisualWhite"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualWhiteStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="1.5"/>
    <Rectangle
      x:Name="FocusVisualBlack"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualBlackStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="0.5"/>
...
</ControlTemplate>
```

到目前為止，這只是組合的部分。 若要控制焦點指示器的可見度，您需要定義切換 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) 屬性的視覺狀態。 使用 [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/Hh738505) 附加屬性就可以達到這個目的，這會套用到定義組合的 root 元素。

XAML
```xml
<ControlTemplate TargetType="Button">
  <Grid>
    <VisualStateManager.VisualStateGroups>
       <!--other visual state groups here-->
       <VisualStateGroup x:Name="FocusStates">
         <VisualState x:Name="Focused">
           <Storyboard>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualWhite"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualBlack"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
         </VisualState>
         <VisualState x:Name="Unfocused" />
         <VisualState x:Name="PointerFocused" />
       </VisualStateGroup>
     <VisualStateManager.VisualStateGroups>
<!--composition is here-->
   </Grid>
</ControlTemplate>
```

請注意如何會導致只有其中一個指定的狀態會直接調整 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992)，而其他狀態似乎是空白的。 視覺狀態的運作方式就是，只要控制項一使用來自相同 [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/BR209014) 的另一個狀態，就立即取消由先前狀態套用的所有動畫。 由於來自組合的預設 **Visibility** 是 **Collapsed**，因此表示將不會出現矩形。 控制項邏輯可透過接聽焦點事件 (例如 [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/BR208927))，並利用 [**GoToState**](https://msdn.microsoft.com/library/windows/apps/BR209025) 變更狀態，以控制此行為。 如果您使用的是預設控制項，或是根據已有該行為的控制項所自訂的控制項，則通常已為您完成這項處理。

<span id="Keyboard_accessibility_and_Windows_Phone"/>
<span id="keyboard_accessibility_and_windows_phone"/>
<span id="KEYBOARD_ACCESSIBILITY_AND_WINDOWS_PHONE"/>
## 鍵盤協助工具和 Windows Phone
Windows Phone 裝置通常不會配備專屬硬體鍵盤。 不過，軟體輸入面板 (SIP) 可以支援數個鍵盤協助工具案例。 螢幕助讀程式可以讀出來自 \[文字\] SIP 的文字輸入，包含宣告刪除。 使用者能探索他們的手指所在位置，這是因為螢幕助讀程式可以偵測到使用者正在掃描按鍵，而它會大聲讀出掃描到的按鍵名稱。 此外，部分鍵盤導向的協助工具概念也可以對應到完全不使用鍵盤的相關輔助技術。 例如，即使 SIP 未配置 Tab 鍵，朗讀程式仍然支援相當於按 Tab 鍵的觸控手勢，因此，在 UI 中透過控制項提供有用的 Tab 順序仍是一個重要的協助工具原則。 用來瀏覽複雜控制項內組件的方向鍵也可透過朗讀程式觸控手勢加以支援。 一旦焦點到達不適合用於文字輸入的控制項時，朗讀程式便支援可叫用該控制項動作的手勢。

鍵盤快速鍵通常與 Windows Phone app 無關，因為 SIP 不會包含 Ctrl 鍵或 Alt 鍵。

<span id="related_topics"/>
## 相關主題  
* [協助工具](accessibility.md)
* [鍵盤互動](https://msdn.microsoft.com/library/windows/apps/Mt185607)
* [輸入：觸控式鍵盤範例](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [回應螢幕小鍵盤外觀的範例](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [XAML 協助工具範例](http://go.microsoft.com/fwlink/p/?linkid=238570)
 



<!--HONumber=Jun16_HO5-->


