---
title: Xbox 上啟用語音的 Shell （VES）
description: 瞭解如何在 Xbox 上將語音控制支援新增至 UWP 應用程式。
ms.date: 10/19/2017
ms.topic: article
keywords: windows 10，uwp，xbox，語音，啟用語音的命令介面
ms.openlocfilehash: f51ec2c93a904893dc337545f634d04affde10fd
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685183"
---
# <a name="using-speech-to-invoke-ui-elements"></a>使用語音叫用 UI 元素

啟用語音的 Shell （VES）是 Windows 語音平臺的延伸模組，可讓您在應用程式內獲得一流的語音體驗，讓使用者可以使用語音來叫用螢幕上的控制項，以及透過聽寫來插入文字。 VES 致力於在所有 Windows Shell 和裝置上提供通用的端對端體驗，並在應用程式開發人員所需的最少工作。  為了達成此目的，它利用了 Microsoft 語音平臺和 UI 自動化（UIA）架構。

## <a name="user-experience-walkthrough"></a>使用者經驗逐步解說 ##
以下概述使用者在 Xbox 上使用 VES 時的體驗，並在深入探討 VES 運作方式的詳細資料之前，應該先協助設定內容。

- 使用者開啟 Xbox 主控台，並想要流覽其應用程式，以找出感興趣的事項：

        User: "Hey Cortana, open My Games and Apps"

- 使用者會處於作用中的接聽模式（ALM），這表示主控台現在會接聽使用者，以叫用畫面上可見的控制項，而不需要每次都說「嘿 Cortana」。  使用者現在可以切換至 [觀看應用程式]，並在應用程式清單中進行流覽：

        User: "applications"

- 若要滾動此視圖，使用者可以直接說：

        User: "scroll down"

- 使用者看到感興趣的應用程式的封面，但忘記名稱。  使用者要求顯示語音提示標籤：

        User: "show labels"

- 現在已經清楚說明了，可以啟動應用程式：

        User: "movies and TV"

- 若要結束使用中的接聽模式，使用者會告訴 Xbox 停止接聽：

        User: "stop listening"

- 之後，您可以使用下列方式啟動新的有效接聽會話：

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>使用者介面自動化相依性 ##
VES 是使用者介面自動化用戶端，而且會依賴應用程式透過其 UI 自動化提供者公開的資訊。 這是 Windows 平臺上的 [朗讀程式] 功能已經使用的基礎結構。  UI 自動化可讓您以程式設計方式存取使用者介面專案，包括控制項的名稱、其類型，以及它所執行的控制項模式。  當應用程式中的 UI 變更時，VES 會回應 UIA 更新事件，並重新剖析更新的 UI 自動化樹狀目錄，以使用此資訊來建立語音辨識文法，以尋找所有可採取動作的專案。 

所有 UWP 應用程式都可以存取 UI 自動化架構，並公開與所建立之圖形架構（XAML、DirectX/Direct3D、Xamarin 等）無關的 UI 資訊。  在某些情況下（例如 XAML），大部分的繁重工作都是由架構完成，大幅減少支援朗讀程式和 VES 所需的工作量。

如需 UI 自動化的詳細資訊，請參閱[Ui 自動化基本](https://msdn.microsoft.com/library/ms753107(v=vs.110).aspx "UI 自動化基礎觀念")概念。

## <a name="control-invocation-name"></a>控制項調用名稱 ##
VES 會採用下列啟發學習法來判斷要向語音辨識器註冊哪個片語作為控制項的名稱（例如，使用者需要對其叫用控制項的意義）。  這也是要顯示在語音提示標籤中的片語。

依優先順序排列的名稱來源：

1. 如果元素具有 `LabeledBy` 附加屬性，則 VES 會使用此文字標籤的 `AutomationProperties.Name`。
2. 元素的 `AutomationProperties.Name`。  在 XAML 中，控制項的文字內容將用來做為 `AutomationProperties.Name`的預設值。
3. 如果控制項是「專案」或「按鈕」，則 VES 會尋找具有有效 `AutomationProperties.Name`的第一個子項目。

## <a name="actionable-controls"></a>可操作的控制項 ##
當控制項執行下列其中一個 Automation 控制項模式時，VES 會將它視為可採取動作：

- **InvokePattern** （例如 按鈕）-代表啟動或執行單一明確動作的控制項，並在啟用時不維護狀態。

- **TogglePattern** （例如 核取方塊）-代表可以迴圈一組狀態並維護一組狀態的控制項。

- **SelectionItemPattern** （例如 下拉式方塊）-代表可作為可選取之子專案集合容器的控制項。

- **ExpandCollapsePattern** （例如 下拉式方塊）-表示以視覺化方式展開來顯示內容和折迭以隱藏內容的控制項。

- **ScrollPattern** （例如 List）-表示做為子專案集合之可滾動容器的控制項。

## <a name="scrollable-containers"></a>可滾動的容器 ##
對於支援 ScrollPattern 的可滾動容器，VES 會接聽語音命令，例如 "scroll left"、"scroll right" 等等。當使用者觸發其中一個命令時，會叫用具有適當參數的 Scroll。  捲軸命令會根據 `HorizontalScrollPercent` 的值和 `VerticalScrollPercent` 屬性來插入。  例如，如果 `HorizontalScrollPercent` 大於0，則會新增「向左滾動」，如果小於100，則會加入「向右」，依此類推。

## <a name="narrator-overlap"></a>朗讀程式重迭 ##
[朗讀程式] 應用程式也是使用者介面自動化用戶端，並使用 [`AutomationProperties.Name`] 屬性做為目前所選 UI 專案所讀取之文字的其中一個來源。  為了提供更好的協助工具體驗，許多應用程式開發人員已使用長描述性文字來多載 `Name` 屬性，其目標是在朗讀程式閱讀時提供詳細資訊和內容。  不過，這會造成兩個功能之間的衝突： VES 需要符合或符合控制項可見文字的簡短片語，而朗讀程式則從較長的更具描述性的片語中獲益，以提供更佳的內容。

若要解決此問題，從 Windows 10 建立者更新開始，朗讀程式已更新為同時查看 `AutomationProperties.HelpText` 屬性。  如果此屬性不是空的，則除了 `AutomationProperties.Name`以外，朗讀程式也會說出其內容。  如果 `HelpText` 是空的，則朗讀程式只會讀取名稱的內容。  這將可讓您在需要時使用較長的描述性字串，但會在 `Name` 屬性中維護較短的語音辨識易記片語。

![](images/ves_narrator.jpg)

如需詳細資訊，請參閱[UI 中協助工具支援的自動化屬性](https://msdn.microsoft.com/library/ff400332(vs.95).aspx "UI 中協助工具支援的自動化屬性")。

## <a name="active-listening-mode-alm"></a>主動接聽模式（ALM） ##
### <a name="entering-alm"></a>進入 ALM ###
在 Xbox 上，VES 不會持續接聽語音輸入。  使用者需要明確地進入主動接聽模式，方法是說：

- 「嗨，Cortana，請選取」，或
- 「嗨，Cortana，進行選擇」

有數個其他 Cortana 命令也會讓使用者在完成時保持作用中的接聽，例如「嘿 Cortana、登入」或「嘿 Cortana，前往首頁」。 

輸入 ALM 會有下列效果：

- Cortana 重迭會顯示在右上角，告知使用者他們可以說出他們看到的內容。  當使用者說話時，語音辨識器所辨識的片語片段也會顯示在此位置中。
- VES 會剖析 UIA 樹狀結構、尋找所有可操作的控制項、在語音辨識文法中註冊其文字，並啟動連續接聽會話。

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>即將結束 ALM ###
當使用者使用語音與 UI 互動時，系統將會保留在 ALM 中。  有兩種方式可以結束 ALM：

- 使用者明確顯示「停止接聽」，或
- 如果在進入 ALM 或上次正向辨識後的17秒內沒有正辨識，就會發生超時

## <a name="invoking-controls"></a>叫用控制項 ##
在 ALM 中，使用者可以使用語音與 UI 互動。  如果 UI 設定正確（名稱屬性符合可見文字），使用語音來執行動作應該是順暢且自然的體驗。  使用者應該可以只說他們在畫面上看到的內容。

## <a name="overlay-ui-on-xbox"></a>Xbox 上的重迭 UI ##
為控制項衍生的名稱 VES 可能與 UI 中的實際可見文字不同。  這可能是因為控制項的 `Name` 屬性或明確設定為不同字串的附加 `LabeledBy` 元素所造成。  或者，控制項沒有 GUI 文字，而是只有圖示或影像元素。

在這些情況下，使用者需要一種方式來查看要叫用這類控制項所需說出的內容。  因此，在作用中接聽後，可以藉由說出「顯示標籤」來顯示語音提示。  這會導致語音提示標籤出現在每個可操作的控制項之上。

有100標籤的限制，因此，如果應用程式的 UI 具有比100更多的可操作控制，將會有一些不會顯示語音提示標籤的部分。  在此情況下選擇的標籤不具決定性，因為它相依于目前 UI 的結構和組合，做為 UIA 樹狀目錄中的第一個列舉。

當語音提示標籤顯示時，沒有可隱藏它們的命令，它們會保持可見狀態，直到發生下列其中一個事件為止：

- 使用者叫用控制項
- 使用者流覽離開目前場景
- 使用者說：「停止接聽」
- 主動接聽模式時間超時

## <a name="location-of-voice-tip-labels"></a>語音秘訣標籤的位置 ##
語音秘訣標籤會水準和垂直置中在控制項的 BoundingRectangle 內。  當控制項變小且緊密分組時，標籤可能會重迭/被其他人遮蔽，而 VES 會嘗試將這些標籤分開推送，並確保它們可見。  不過，這不保證會在100% 的時間內生效。  如果有非常擁擠的 UI，可能會導致有些標籤被其他人遮蔽。 請使用 [顯示標籤] 來檢查您的 UI，以確保有足夠的空間可供語音提示可見度之用。

![](images/ves_labels.png)

## <a name="combo-boxes"></a>下拉式方塊 ##
當下拉式方塊展開時，下拉式方塊中的每個個別專案都會取得自己的語音提示標籤，而這些方塊通常會位於 [現有控制項後方] 下拉式清單的頂端。  若要避免呈現混亂且令人困惑的標籤 muddle （其中下拉式方塊專案標籤與下拉式方塊後面的控制項標籤混合），當下拉式方塊展開時，只會顯示其子專案的標籤; 所有其他的聲音提示標籤都會隱藏起來。  然後，使用者可以選取其中一個下拉式專案或 [關閉] 下拉式方塊。

- 折迭下拉式方塊上的標籤：

    ![](images/ves_combo_closed.png)

- 展開的下拉式方塊上的標籤：

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>可滾動控制項 ##
針對可滾動的控制項，捲軸命令的語音秘訣會以控制項的每個邊緣為中心。  只會顯示可採取動作的語音提示，例如，如果無法使用垂直捲動，將不會顯示「向上快移」和「向下滾動」。  當有多個可捲動區域存在時，VES 會使用序數來區別它們（例如， "Scroll right 1"、"Scroll right 2" 等）。

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>去除混淆 ##
當多個 UI 元素的名稱相同，或語音辨識器符合多個候選項目時，VES 會進入去除去除模式。  在此模式中，將會顯示相關元素的語音提示標籤，讓使用者可以選取正確的專案。 使用者可以藉由說「取消」來取消去除混淆模式。

例如：

- 在使用中接聽模式時，去除混淆之前：使用者說：「我不清楚」。

    ![](images/ves_disambig1.png) 

- 兩個按鈕都相符;消除混淆已開始：

    ![](images/ves_disambig2.png) 

- 選擇 [選取 2] 時，顯示按一下動作：

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


使用上述範例時，UI 看起來會像，而不使用語音提示標籤。
 
- 在主動接聽模式中，沒有顯示標籤：

    ![](images/ves_alm_nolabels.png) 

- 在主動接聽模式中，在使用者顯示「顯示標籤」之後：

    ![](images/ves_alm_labels.png) 

在 `button1`的情況下，XAML 會使用控制項可見文字內容中的文字，自動填入 `AutomationProperties.Name` 屬性。  這就是為什麼沒有明確的 `AutomationProperties.Name` 設定了語音秘訣標籤的原因。

在 `button2`中，我們會將 `AutomationProperties.Name` 明確設定為控制項文字以外的內容。

使用 `comboBox`，我們使用 `LabeledBy` 屬性來參考 `label1` 做為自動化 `Name`的來源，而在 `label1` 我們將 `AutomationProperties.Name` 設定為比螢幕上轉譯的更自然片語（「一周中的日」，而不是「選取的星期幾」）。

最後，透過 `button3`，VES 會從第一個子專案抓取 `Name`，因為 `button3` 本身並沒有 `AutomationProperties.Name` 設定。

## <a name="see-also"></a>請參閱
- [UI 自動化基礎](https://msdn.microsoft.com/library/ms753107(v=vs.110).aspx "UI 自動化基礎觀念")
- [UI 中協助工具支援的自動化屬性](https://msdn.microsoft.com/library/ff400332(vs.95).aspx "UI 中協助工具支援的自動化屬性")
- [常見問題集](frequently-asked-questions.md)
- [Xbox One 上的 UWP](index.md)
