---
title: 'Xbox 上的語音啟用 Shell (VES) '
description: 瞭解如何使用已啟用語音的 Shell)  (的通用 Windows 平臺，在 Xbox 上將語音控制支援新增至您的 (UWP) 應用程式。
ms.date: 10/19/2017
ms.topic: article
keywords: windows 10、uwp、xbox、語音、語音功能 shell
ms.openlocfilehash: 38afa2473dd74ab580cf38cc21d1f2b192f9b72a
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304650"
---
# <a name="using-speech-to-invoke-ui-elements"></a>使用語音來叫用 UI 元素

啟用語音的 Shell (VES) 是 Windows 語音平臺的延伸模組，可在應用程式內啟用一流的語音體驗，讓使用者可以使用語音來叫用螢幕控制項，以及透過聽寫插入文字。 VES 致力於在所有 Windows Shell 和裝置上提供常見的端對端體驗，並提供應用程式開發人員所需的最少投入時間。  為了達成此目的，它會利用 Microsoft Speech Platform 和消費者介面自動化 (UIA) 架構。

## <a name="user-experience-walkthrough"></a>使用者經驗逐步解說 ##
以下是在 Xbox 上使用 VES 時，使用者會遇到什麼體驗的總覽，並且應該先協助設定內容，再深入瞭解如何執行 VES 的詳細資料。

- 使用者開啟 Xbox 主控台，並想要流覽其應用程式以尋找感興趣的內容：

        User: "Hey Cortana, open My Games and Apps"

- 使用者停留在主動接聽模式 (ALM) ，這表示主控台現在正在接聽使用者來叫用畫面上可見的控制項，而不需要每次「嗨 Cortana」。  使用者現在可以切換以查看應用程式，並在應用程式清單中進行滾動：

        User: "applications"

- 若要滾動查看，使用者可以直接說：

        User: "scroll down"

- 使用者會看到他們感興趣的應用程式封面，但忘記名稱。  使用者要求顯示語音提示標籤：

        User: "show labels"

- 現在您可以清楚說，可以啟動應用程式：

        User: "movies and TV"

- 若要結束主動接聽模式，使用者可告知 Xbox 停止聆聽：

        User: "stop listening"

- 之後，可以使用下列方式啟動新的作用中接聽會話：

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>使用者介面自動化相依性 ##
VES 是消費者介面自動化用戶端，而且會依賴應用程式透過其消費者介面自動化提供者公開的資訊。 這與 Windows 平臺上的 [朗讀程式] 功能已使用相同的基礎結構。  消費者介面自動化可讓您以程式設計方式存取使用者介面元素，包括控制項的名稱、其類型，以及它所執行的控制項模式。  當應用程式中的 UI 變更時，VES 將會回應 UIA 更新事件，並重新剖析更新的消費者介面自動化樹狀結構，以找出所有可採取動作的專案，並使用這項資訊來建立語音辨識文法。 

所有 UWP 應用程式都可以存取消費者介面自動化架構，並且可以公開有關 UI 的資訊，而這些資訊與根據 (XAML、DirectX/Direct3D、Xamarin 等 ) 建立的圖形架構無關。  在某些情況下（例如 XAML），大部分的繁重工作都是由架構來完成，大幅減少支援朗讀程式和 VES 所需的工作。

如需消費者介面自動化的詳細資訊，請參閱 [消費者介面自動化基礎](/dotnet/framework/ui-automation/ui-automation-fundamentals "UI 自動化基礎觀念")。

## <a name="control-invocation-name"></a>控制項調用名稱 ##
VES 採用下列啟發學習法來決定要使用語音辨識器註冊哪一個片語，作為控制項名稱 (ie。使用者必須說的，才能叫用控制項) 。  這也是要顯示在語音提示標籤中的片語。

依優先順序排列的名稱來源：

1. 如果專案具有 `LabeledBy` 附加屬性，則 VES 會使用 `AutomationProperties.Name` 此文字標籤的。
2. `AutomationProperties.Name` 專案的。  在 XAML 中，控制項的文字內容將作為的預設值使用 `AutomationProperties.Name` 。
3. 如果控制項是專案1或按鈕，則 VES 會尋找具有有效的第一個子項目 `AutomationProperties.Name` 。

## <a name="actionable-controls"></a>可操作的控制項 ##
如果控制項執行下列其中一個 Automation 控制項模式，則會將控制項視為可採取動作：

- **InvokePattern** (例如 按鈕) -代表在啟用時起始或執行單一、明確動作和不維持狀態的控制項。

- **TogglePattern** (例如 核取方塊) -表示可以在一組狀態之間迴圈的控制項，並在設定之後維護狀態。

- **SelectionItemPattern** (例如 下拉式方塊) -表示作為可選取子專案集合之容器的控制項。

- **ExpandCollapsePattern** (例如 下拉式方塊) -表示以視覺化方式展開來顯示內容和折迭以隱藏內容的控制項。

- **ScrollPattern** (例如 List) -代表可作為子項目集合之可滾動容器的控制項。

## <a name="scrollable-containers"></a>可滾動的容器 ##
針對可滾動的容器（支援 ScrollPattern），VES 會接聽語音命令，例如「向左滾動」、「向右滾動」等等。當使用者觸發其中一個命令時，就會使用適當的參數叫用 Scroll。  捲軸命令會根據和屬性的值插入 `HorizontalScrollPercent` `VerticalScrollPercent` 。  比方說，如果 `HorizontalScrollPercent` 大於0，將會加入「左滾」，如果小於100，則會加入「向右滾動」等等。

## <a name="narrator-overlap"></a>朗讀程式重迭 ##
「朗讀程式」應用程式也是消費者介面自動化用戶端，並使用 `AutomationProperties.Name` 屬性做為針對目前選取的 UI 元素所讀取之文字的其中一個來源。  為了提供更好的協助工具體驗，許多應用程式開發人員都已使用較長的描述性文字來進行屬性的多載，其 `Name` 目標是在朗讀朗讀程式時提供詳細資訊和內容。  不過，這會造成兩個功能之間的衝突： VES 需要符合或完全符合控制項可見文字的簡短片語，而朗讀程式從較長、更具描述性的片語中獲益，以提供更好的內容。

若要解決這個問題，請從 Windows 10 Creators Update 開始，朗讀程式也已更新，以查看 `AutomationProperties.HelpText` 屬性。  如果這個屬性不是空的，除了之外，朗讀程式還會說出其內容 `AutomationProperties.Name` 。  如果 `HelpText` 是空的，則 [朗讀程式] 只會讀取名稱的內容。  這可讓您在需要時使用較長的描述性字串，但會在屬性中維護較短的語音辨識易記片語 `Name` 。

![](images/ves_narrator.jpg)

如需詳細資訊，請參閱 [UI 中協助工具支援的自動化屬性](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ff400332(v=vs.95) "UI 中協助工具支援的 Automation 屬性")。

## <a name="active-listening-mode-alm"></a> (ALM) 的主動式接聽模式 ##
### <a name="entering-alm"></a>進入 ALM ###
在 Xbox 上，VES 不會不斷地接聽語音輸入。  使用者需要明確地進入主動接聽模式，方法是說：

- "嗨 Cortana、select 或
- 「嗨 Cortana，請選擇」

另外還有其他幾個 Cortana 命令，也會在完成時讓使用者保持作用中狀態，例如「嗨 Cortana、登入」或「嗨 Cortana，前往 home」。 

進入 ALM 將會有下列效果：

- Cortana 重迭會顯示在右上角，告訴使用者他們會看到的內容。  當使用者說話時，語音辨識器所辨識的片語片段也會顯示在此位置中。
- VES 會剖析 UIA 樹狀結構、尋找所有可操作的控制項、在語音辨識文法中註冊其文字，以及開始連續接聽會話。

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>離開 ALM ###
當使用者使用語音與 UI 互動時，系統將會保留在 ALM 中。  有兩種方式可以離開 ALM：

- 使用者明確指出、「停止接聽」或
- 如果在輸入 ALM 或自上次正面辨識後的17秒內沒有正面辨識，將會發生超時狀況

## <a name="invoking-controls"></a>叫用控制項 ##
在 ALM 中，使用者可以使用語音與 UI 互動。  如果已正確設定 UI (具有符合可見文字) 的名稱屬性，則使用語音來執行動作應該是順暢的自然體驗。  使用者應該能夠直接說出它們在螢幕上看到的內容。

## <a name="overlay-ui-on-xbox"></a>Xbox 上的重迭 UI ##
針對控制項衍生的名稱 VES 可能與 UI 中的實際可見文字不同。  這可能是因為控制項的 `Name` 屬性，或是 `LabeledBy` 明確設定為不同字串的附加元素所致。  或者，控制項沒有 GUI 文字，而只有圖示或影像元素。

在這些情況下，使用者需要一種方式來查看必須說的，才能叫用這類控制項。  因此，在使用中的接聽之後，您可以藉由說出 [顯示標籤] 來顯示語音提示。  這會導致語音提示標籤顯示在每個可採取動作的控制項之上。

有100標籤的限制，因此，如果應用程式的 UI 具有比100更具可操作的控制項，將會有一些不會顯示語音提示標籤的部分。  在此情況下選擇的標籤不具決定性，因為它相依于目前 UI 的結構和組合，作為 UIA 樹狀結構中的第一個列舉。

一旦顯示語音提示標籤，就不會有任何命令可以隱藏它們，直到發生下列其中一個事件時，才會顯示這些標籤：

- 使用者叫用控制項
- 使用者離開目前的場景
- 使用者說：「停止接聽」
- 主動式接聽模式超時

## <a name="location-of-voice-tip-labels"></a>語音提示標籤的位置 ##
語音提示標籤會在控制項的 BoundingRectangle 中水準和垂直置中。  當控制項較小且緊密地分組時，其他標籤可能會重迭/被他人遮蔽，而且 VES 會嘗試將這些標籤分開來分開，並確保它們是可見的。  不過，這不保證會在100% 的時間內運作。  如果有非常擁擠的 UI，可能會導致某些標籤被他人遮蔽。 請使用 [顯示標籤] 檢查您的 UI，以確保有足夠的聲音提示可見空間。

![](images/ves_labels.png)

## <a name="combo-boxes"></a>下拉式方塊 ##
展開下拉式方塊中的每個個別專案時，下拉式方塊中的每個個別專案都會取得自己的語音提示標籤，而且通常會在下拉式清單的現有控制項上方。  為了避免呈現混亂而令人困惑的標籤 muddle (其中的下拉式方塊專案標籤會與下拉式方塊後方的控制項標籤混合) 當下拉式方塊展開時，只會顯示其子專案的標籤; 所有其他語音提示標籤將會隱藏。  然後，使用者可以選取其中一個下拉式專案或 [關閉] 下拉式方塊。

- 折迭下拉式方塊上的標籤：

    ![](images/ves_combo_closed.png)

- 展開的下拉式方塊上的標籤：

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>可滾動的控制項 ##
針對可滾動的控制項，捲軸命令的聲音提示將會以控制項的每個邊緣為中心。  語音秘訣只會顯示為可採取動作的捲軸方向，因此例如，如果無法使用垂直捲動條，將不會顯示「向上快移」和「向下滾動」。  當有多個可捲動區域存在時，會使用序數來區分它們 (例如。 ) 的「向右滾動1」、「向右滾動2」等等。

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>去除混淆 ##
當多個 UI 元素有相同的名稱，或語音辨識器符合多個候選項目時，VES 將會進入去除混淆模式。  在此模式中，會顯示相關元素的語音提示標籤，讓使用者可以選取正確的專案。 使用者可以藉由說出 [取消] 來取消去除混淆模式。

例如：

- 在進行混淆之前的主動式接聽模式;使用者說：「我不清楚」：

    ![](images/ves_disambig1.png) 

- 這兩個按鈕相符;已開始消除混淆：

    ![](images/ves_disambig2.png) 

- 選擇 [選取 2] 時顯示 click 動作：

    ![](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>範例 UI ##
以下是以 XAML 為基礎的 UI 範例，以各種方式設定 AutomationProperties.Name：

    <Page
        x:Class="VESSampleCSharp.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:VESSampleCSharp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Button x:Name="button1" Content="Hello World" HorizontalAlignment="Left" Margin="44,56,0,0" VerticalAlignment="Top"/>
            <Button x:Name="button2" AutomationProperties.Name="Launch Game" Content="Launch" HorizontalAlignment="Left" Margin="44,106,0,0" VerticalAlignment="Top" Width="99"/>
            <TextBlock AutomationProperties.Name="Day of Week" x:Name="label1" HorizontalAlignment="Left" Height="22" Margin="168,62,0,0" TextWrapping="Wrap" Text="Select Day of Week:" VerticalAlignment="Top" Width="137"/>
            <ComboBox AutomationProperties.LabeledBy="{Binding ElementName=label1}" x:Name="comboBox" HorizontalAlignment="Left" Margin="310,57,0,0" VerticalAlignment="Top" Width="120">
                <ComboBoxItem Content="Monday" IsSelected="True"/>
                <ComboBoxItem Content="Tuesday"/>
                <ComboBoxItem Content="Wednesday"/>
                <ComboBoxItem Content="Thursday"/>
                <ComboBoxItem Content="Friday"/>
                <ComboBoxItem Content="Saturday"/>
                <ComboBoxItem Content="Sunday"/>
            </ComboBox>
            <Button x:Name="button3" HorizontalAlignment="Left" Margin="44,156,0,0" VerticalAlignment="Top" Width="213">
                <Grid>
                    <TextBlock AutomationProperties.Name="Accept">Accept Offer</TextBlock>
                    <TextBlock Margin="0,25,0,0" Foreground="#FF5A5A5A">Exclusive offer just for you</TextBlock>
                </Grid>
            </Button>
        </Grid>
    </Page>


使用上述範例，就是 UI 的外觀和沒有語音提示標籤的樣子。
 
- 在主動接聽模式中，不會顯示標籤：

    ![](images/ves_alm_nolabels.png) 

- 在主動接聽模式中，在使用者顯示 [顯示標籤] 之後：

    ![](images/ves_alm_labels.png) 

在的案例中 `button1` ，XAML 會 `AutomationProperties.Name` 使用控制項的可見文字內容中的文字來自動填入屬性。  這就是為什麼即使沒有明確的設定，也會有語音提示標籤 `AutomationProperties.Name` 。

在 `button2` 中，我們會明確地將設定 `AutomationProperties.Name` 為控制項文字以外的內容。

使用 `comboBox` 時，我們使用 `LabeledBy` 屬性做為 `label1` 自動化的來源 `Name` ，並 `label1` 將設定 `AutomationProperties.Name` 為比螢幕上轉譯的更自然的片語 ( "Day of week"，而不是 "Select day of week" ) 。

最後，使用 `button3` ，VES `Name` 會從第一個子項目抓取，因為 `button3` 本身沒有 `AutomationProperties.Name` 集合。

## <a name="see-also"></a>另請參閱
- [UI 自動化基礎](/dotnet/framework/ui-automation/ui-automation-fundamentals "UI 自動化基礎觀念")
- [UI 中協助工具支援的 Automation 屬性](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ff400332(v=vs.95) "UI 中協助工具支援的 Automation 屬性")
- [常見問題集](frequently-asked-questions.md)
- [Xbox One 上的 UWP](index.md)