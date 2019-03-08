---
title: 語音啟用 Xbox 上的殼層 (VES)
description: 了解如何將語音控制支援新增至您在 Xbox 上的 UWP 應用程式。
ms.date: 10/19/2017
ms.topic: article
keywords: windows 10、 uwp、 xbox、 語音、 使用語音命令介面
ms.openlocfilehash: ea51216c804754e98c3bac459b79fb75dd9369cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596053"
---
# <a name="using-speech-to-invoke-ui-elements"></a>使用語音來叫用 UI 項目

語音啟用殼層 (VES) 是 Windows 語音平台，可讓應用程式內的第一級的語音體驗讓使用者使用語音來叫用螢幕上的擴充功能控制項，並將透過聽寫的文字。 VES 致力於提供所有的 Windows 殼層及裝置上的共通的端對端，請參閱 it-say it 經驗，與從應用程式開發人員所需的最小工作。  若要這麼做，它會利用 Microsoft 語音平台和 UI 自動化 (UIA) 架構。

## <a name="user-experience-walkthrough"></a>使用者體驗逐步解說 ##
以下是使用 VES Xbox 上時，使用者什麼所遇到的概觀，這應該有助於設定前一頭栽進 VE 的運作方式的詳細資料的內容。

- 使用者開啟 Xbox 主控台，並想要瀏覽以找出感興趣的應用程式：

        User: "Hey Cortana, open My Games and Apps"

- 使用者會保持在作用中接聽模式 (ALM)，這表示主控台正在接聽使用者叫用控制項可看見的畫面上，而不需要說 「 嘿 Cortana 」 每一次。  使用者現在可以切換檢視應用程式和捲動以檢視應用程式清單：

        User: "applications"

- 若要捲動檢視，使用者可以直接說：

        User: "scroll down"

- 使用者會看到他們感興趣，但忘記名稱的應用程式的封面。  使用者要求要顯示的語音提示標籤：

        User: "show labels"

- 現在，很顯然要說的內容，就可以啟動應用程式：

        User: "movies and TV"

- 若要結束主動接聽模式，使用者會告訴 Xbox 停止接聽：

        User: "stop listening"

- 更新版本起，新的作用中接聽工作階段可以開始使用：

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>UI 自動化的相依性 ##
VES 是使用者介面自動化用戶端，並依賴應用程式透過其使用者介面自動化提供者所公開的資訊。 這是在 Windows 平台上的 [朗讀程式] 功能已經使用相同的基礎結構。  使用者介面自動化可讓您以程式設計方式存取使用者介面項目，包括控制項、 其型別和哪些控制項模式實作的名稱。  當 UI 變更應用程式中，將回應 UIA 更新事件 VES，並將其重新剖析已更新的使用者介面自動化樹狀目錄中，尋找所有可採取動作的項目，可以使用這項資訊來建立語音辨識文法。 

所有的 UWP 應用程式存取使用者介面自動化架構，並可以公開 （expose） 的閘道建置在 （XAML、 DirectX/Direct3D、 Xamarin 等） 的圖形架構的獨立 UI 的相關資訊。  在某些情況下，例如 XAML，大部分繁重的工作是由 framework，可大幅減少以支援 「 朗讀程式 」 和 VES 所需的工作。

如需 UI 自動化的詳細資訊請參閱 < [UI 自動化基礎觀念](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "UI 自動化基礎觀念")。

## <a name="control-invocation-name"></a>控制項叫用的名稱 ##
VES 會採用下列啟發式來判斷什麼片語向語音辨識器，做為控制項的名稱 (亦即。 使用者必須向叫用控制項)。  這也是會顯示在語音提示標籤的片語。

來源名稱中的優先順序：

1. 如果項目具有`LabeledBy`會使用附加的屬性，VE`AutomationProperties.Name`此文字標籤。
2. `AutomationProperties.Name` 項目。  在 XAML，會使用控制項的文字內容的預設值為`AutomationProperties.Name`。
3. 如果控制項是清單項目或按鈕時，使用有效的第一個子元素會尋找 VES `AutomationProperties.Name`。

## <a name="actionable-controls"></a>可採取動作的控制項 ##
VES 會考慮控制項可採取動作，如果它會實作下列的自動化控制項模式的其中一個：

- **InvokePattern** （例如。 按鈕）-表示起始或執行單一明確動作並不會維護狀態啟動時的控制項。

- **TogglePattern** （例如。 核取方塊-） 表示的控制項可以循環瀏覽一組狀態並維護設定的狀態。

- **SelectionItemPattern** （例如。 下拉式方塊）-代表可作為可選取子項目集合的容器控制項。

- **ExpandCollapsePattern** （例如。 下拉式方塊）-表示以視覺化方式展開來顯示內容和摺疊以隱藏內容的控制項。

- **ScrollPattern** （例如。 清單）-代表可當成子項目集合捲動式容器控制項。

## <a name="scrollable-containers"></a>可捲動的容器 ##
適用於可捲動的容器 VES，ScrollPattern 接聽語音支援 「 向左捲動 」、 「 向右捲動 」 等類似的命令，並叫用捲軸以適當的參數時使用者會觸發其中一個命令。  捲動命令已插入的值為基礎`HorizontalScrollPercent`和`VerticalScrollPercent`屬性。  比方說，如果`HorizontalScrollPercent`是大於 0，"捲軸向左"會新增，如果它小於 100，"向右捲動 」 將會新增，依此類推。

## <a name="narrator-overlap"></a>朗讀程式重疊 ##
「 朗讀程式 」 應用程式也是使用者介面自動化用戶端，並使用`AutomationProperties.Name`屬性做為其中一個來源讀取之目前選取的 UI 項目的文字。  若要提供較佳的協助工具體驗許多應用程式開發人員都訴諸多載`Name`具有長時間的描述性文字，旨在提供更多的資訊與當讀取器 [朗讀程式] 的內容屬性。  不過，這會導致兩個功能之間的衝突：VES 必須簡短片語的比對或接近的控制項，朗讀程式獲益更長、 更具描述性的片語，以提供較佳的內容時的可見文字。

若要解決此問題，從 Windows 10 Creators Update 開始，[朗讀程式] 已更新為也查看`AutomationProperties.HelpText`屬性。  如果這個屬性不是空的 [朗讀程式] 將說出它的內容，除了`AutomationProperties.Name`。  如果`HelpText`是空的朗讀程式只會讀取內容的名稱。  這可讓在需要時，使用再描述性字串，但是會維護較短，語音辨識易記的句子中`Name`屬性。

![](images/ves_narrator.jpg)

如需詳細資訊，請參閱[UI 中的協助工具支援的 Automation Properties](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "的協助工具支援，在 UI 自動化屬性")。

## <a name="active-listening-mode-alm"></a>主動接聽模式 (ALM) ##
### <a name="entering-alm"></a>輸入 ALM ###
在 Xbox 上 VES 不斷接聽語音輸入。  使用者必須輸入主動接聽模式明確地說：

- 「 嘿 Cortana，選取"，或
- 「 嘿 Cortana，請選取項目 」

有數個其他也將留在完成時，比方說 「 嘿 Cortana，登入 」 或 「 嘿 Cortana，前往首頁 」 的作用中接聽使用者的 Cortana 命令。 

輸入 ALM，將會有下列結果：

- 在右上角，告訴使用者他們可以說他們看到的內容中，將會顯示 Cortana 覆疊。  當使用者正在讀出時，語音辨識器所辨識的片語片段也會顯示在這個位置。
- VES 剖析，UIA 樹狀目錄、 尋找所有可採取動作的控制項、 語音辨識文法中註冊其文字和啟動持續接聽工作階段。

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>現有的 ALM ###
系統會保留在 ALM 中，雖然使用者與 UI 使用語音互動。  有兩種方式可結束 ALM:

- 使用者明確地說，「 停止接聽 」，或
- 如果找不到正數辨識的輸入 ALM，或自最後一個正數辨識 17 秒內，會發生逾時

## <a name="invoking-controls"></a>叫用控制項 ##
在使用語音 UI 的 ALM 使用者可以互動。  如果 UI 已設定正確 （比對的可見文字的名稱屬性），則使用語音來執行動作應該是流暢的自然體驗。  使用者應該能夠把它們在螢幕上所看到的內容。

## <a name="overlay-ui-on-xbox"></a>覆疊在 Xbox 上的 UI ##
VES 名稱衍生而來的控制項可能會不同於 UI 中的實際可見文字。  這可能是因為`Name`屬性的控制項或附加`LabeledBy`項目明確設定為不同的字串。  或者，您也可以在控制項並沒有 GUI 文字，但是只有圖示或影像項目。

在這些情況下，使用者需要查看哪些項目要可說是以叫用這類控制項的方式。  因此，一次在作用中接聽，語音提示可以顯示依所說 [顯示標籤]。  這會導致語音提示標籤會出現在每個可採取動作的控制項頂端。

會限制為 100 的標籤，因此，應用程式的 UI 更容易執行大於 100 的控制項是否會有一些不會顯示的語音提示標籤。  選擇哪一個標籤會在此情況下不具決定性，因為這取決於結構和目前的使用者介面的組合，第一次列舉，UIA 樹狀目錄中。

一旦語音提示標籤會顯示沒有任何命令，以將其隱藏，它們會保持可見的情況，直到發生下列事件其中一項：

- 使用者叫用控制項
- 使用者瀏覽離開目前的場景
- 使用者講的是，[停止接聽]
- 主動接聽模式逾時

## <a name="location-of-voice-tip-labels"></a>語音提示標籤的位置 ##
語音提示標籤水平和垂直置中對齊控制項的 BoundingRectangle。  當控制項都是小型且緊密的群組時，這些標籤可以重疊/會變得模糊其他人和 VES 會嘗試將這些標籤相隔推送至加以區隔，並確保它們會顯示。  不過，這不保證可以運作 100%的時間。  如果有非常擁擠的 UI，可能會導致某些遮蔽其他人的標籤。 請檢閱您的 UI 與 「 顯示標籤 」，確保有足夠的空間，如語音提示可見性。

![](images/ves_labels.png)

## <a name="combo-boxes"></a>下拉式方塊 ##
下拉式方塊展開下拉式方塊中的每個個別項目時取得其自己的語音提示標籤與這些通常是下拉式清單背後的現有控制項之上。  若要避免呈現雜亂且令人困惑混淆的標籤 （下拉式方塊項目標籤混合使用後下拉式方塊控制項的標籤） 下拉式方塊會顯示其子項目; 展開只有標籤時 將隱藏所有其他的語音提示標籤。  使用者可以再選取其中一個下拉式清單項目或者 「 關閉 」 的下拉式方塊。

- 摺疊的下拉式方塊中的標籤：

    ![](images/ves_combo_closed.png)

- 擴充的下拉式方塊中的標籤：

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>可捲動的控制項 ##
可捲動的控制項的捲動命令的語音提示將會在每個控制項的邊緣上置中對齊。  語音提示，才會顯示的捲軸方向，採取動作，例如如果垂直捲動無法使用，「 向上捲動 」 和 「 向下捲動"將不會顯示。  VES 有多個可捲動區域時將使用序數區分它們 （例如。 「 捲動右 1 」、 「 捲動右 2 」 等等。)。

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>去除混淆 ##
當多個 UI 項目有相同的名稱，或語音辨識器比對多個候選項目時，VE 會進入去除混淆模式。  在此模式語音提示標籤會顯示的項目相關，因此使用者可以選取正確。 使用者可以取消去除混淆模式，說出"cancel"。

例如：

- 在主動接聽模式中之前去除混淆;使用者說 「 我 I 模稜兩可 」:

    ![](images/ves_disambig1.png) 

- 比對; 這兩個按鈕去除混淆開始：

    ![](images/ves_disambig2.png) 

- 顯示已選擇 「 選取 2 」 時，按一下 動作：

    ![](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>範例 UI ##
以下是範例的 XAML 形式的 UI，以各種方式設定 AutomationProperties.Name:

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


使用這裡的上述的範例是不含語音提示標籤與 UI 的外觀。
 
- 在主動接聽模式，而不要顯示的標籤：

    ![](images/ves_alm_nolabels.png) 

- 在主動接聽模式中之後使用者講的是 [顯示標籤]:

    ![](images/ves_alm_labels.png) 

若是`button1`，XAML 自動填入`AutomationProperties.Name`屬性使用中控制項的顯示文字內容的文字。  這就是為什麼沒有語音提示標籤即使沒有明確`AutomationProperties.Name`設定。

具有`button2`，明確地設定`AutomationProperties.Name`以外的控制項的文字。

與`comboBox`，我們使用了`LabeledBy`屬性來參考`label1`做為來源的自動化`Name`，然後在`label1`我們設定`AutomationProperties.Name`更自然的句子比要呈現的畫面上 ("Day of Week"而不是比 「 選取星期幾"）。

最後，使用`button3`，VE 抓取`Name`從第一個子項目之後`button3`本身並沒有`AutomationProperties.Name`設定。

## <a name="see-also"></a>請參閱
- [UI 自動化基礎觀念](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "UI 自動化基礎觀念")
- [在 UI 中的協助工具支援的自動化屬性](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "的協助工具支援，在 UI 自動化屬性")
- [常見問題集](frequently-asked-questions.md)
- [在 Xbox One UWP](index.md)
