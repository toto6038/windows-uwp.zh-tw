---
author: Jwmsft
label: App bars/command bars
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a2f4e7a679ca47f2a034e19936c1115e87a2eb24
ms.openlocfilehash: c7107599529d5af5b118a46cb065106f08afe113

---

# 應用程式列與命令列

命令列 (亦稱為「應用程式列」) 可讓使用者輕鬆存取 app 的最常見工作，也可以用來顯示使用者內容特定的命令或選項，如相片選取或繪圖模式。 它們也可以用來在 app 頁面或 app 區段之間瀏覽。 命令列可用於任何瀏覽模式。

![具有圖示之命令列的範例](images/controls_appbar_icons.png)



-   [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx)
-   [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx)
-   [**AppBarToggleButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx)
-   [**AppBarSeparator**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx)

## 這是正確的控制項嗎？

CommandBar 控制項是一般用途、彈性、輕量的控制項，它可以顯示複雜的內容 (例如影像或文字區塊)，也可以顥示簡單的命令 (例如 [AppBarButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx)、[AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx) 與 [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx) 控制項)。

XAML 提供 AppBar 控制項與 CommandBar 控制項。 只有在您要升級的通用 Windows 8 應用程式使用 AppBar，而且您想要將變更幅度控制在最小的情況下，才應該使用 AppBar。 對於 Windows 10 中的新 app，建議您改為使用 CommandBar 控制項。 此文件假設您使用 CommandBar 控制項。

## 範例
Microsoft 相片 app 中擴充的命令列。

![Microsoft 相片 app 中的命令列](images/control-examples/command-bar-photos.png)

Windows Phone 上 Outlook 行事曆中的命令列

![Outlook 行事曆 app 中的命令列](images/control-examples/command-bar-calendar-phone.png)

## 結構

根據預設，命令列會顯示一排圖示按鈕與一個選擇性的 [查看更多] 按鈕，它會以省略符號 \[•••\] 表示。 這個命令列是由稍後顯示的範例程式碼建立。 顯示的命令列處於關閉、精簡狀態。

![關閉的命令列](images/command-bar-compact.png)

命令列也可以顯示成關閉、最小狀態，看起來像這樣。 請參閱[開啟與關閉狀態](#open-and-closed-states)一節以取得詳細資訊。

![關閉的命令列](images/command-bar-minimal.png)

以下是處於開啟狀態的相同命令列。 標籤會識別控制項的主要部分。

![關閉的命令列](images/commandbar_anatomy_open.png)

命令列被分成 4 個主要區域︰
- [查看更多] \[•••\] 按鈕會顯示在命令列右側。 按 [查看更多] \[•••\] 按鈕有 2 種效果：顯示主要命令按鈕上的標籤，而且如果有任何次要命令存在，還會開啟溢位功能表。 在最新的 SDK 中，當沒有任何次要命令和任何隱藏標籤存在時，將不會顯示該按鈕。 [**OverflowButtonVisibility**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.overflowbuttonvisibility.aspx) 屬性可讓 App 變更這項預設的自動隱藏行為。
- 內容區域會對齊命令列左側。 如果已填入 [**Content**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx) 屬性，就會顯示內容區域。
- 主要命令區域會對齊命令列右側，就在 [查看更多] \[•••\] 按鈕右邊。 如果已填入 [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx) 屬性，就會顯示主要命令區域。  
- 只有當命令列已開啟並已填入 [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) 屬性時，才會顯示溢位功能表。 當空間有限時，新的動態溢位行為會自動將主要命令移到 SecondaryCommands 區域中。

當 [FlowDirection](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.flowdirection.aspx) 是 **RightToLeft** 時，配置會反轉。

## 建立命令列
這個範例會建立之前顯示的命令列。

```xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>

    <CommandBar.SecondaryCommands>
        <AppBarButton Icon="Like" Label="Like" Click="AppBarButton_Click"/>
        <AppBarButton Icon="Dislike" Label="Dislike" Click="AppBarButton_Click"/>
    </CommandBar.SecondaryCommands>

    <CommandBar.Content>
        <TextBlock Text="Now playing..." Margin="12,14"/>
    </CommandBar.Content>
</CommandBar>
```

## 命令與內容
CommandBar 控制項有 3 個可用來新增命令與內容的屬性：[**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx)、[**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) 與 [**Content**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx)。


### 主要動作與溢位

根據預設，新增到命令列的項目也會新增到 **PrimaryCommands** 集合。 這些命令會顯示在 [查看更多] \[•••\] 按鈕左側，就在我們稱為動作空間的區域中。 將最重要的命令 (您希望在命令列中保持可見) 放在動作空間中。 在最小的螢幕 (320 epx 寬) 上，命令列的動作空間約可容納最多 4 個項目。

您可以將命令新增到 **SecondaryCommands** 集合，而這些項目會顯示在溢位區域中。 將較不重要的命令放在溢位區域內。

預設溢位區域的樣式會與命令列不同。 您可以將 [**CommandBarOverflowPresenterStyle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.commandbaroverflowpresenterstyle.aspx) 屬性設定為以 [**CommandBarOverflowPresenter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbaroverflowpresenter.aspx) 為目標的 [Style](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx)，以調整樣式。

您能以程式設計方式控制在 PrimaryCommands 與 SecondaryCommands 之間移動命令。 

### 應用程式列按鈕

PrimaryCommands 和 SecondaryCommands 只能填入 [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx)、[**AppBarToggleButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx) 與 [**AppBarSeparator**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) 命令元素。 這些控制項最適合用在命令列中，而且它們的外觀會因為動作空間或溢位區域中是否使用控制項而變更。

應用程式列按鈕控制項是以圖示與關聯的標籤表示。 它們有兩種大小：標準和精簡。 根據預設，會顯示文字標籤。 當 [**IsCompact**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.iscompact.aspx) 屬性設定為 **true** 時，會隱藏文字標籤。 如果是用於 CommandBar 控制項中，當命令列開啟或關閉時，命令列會自動覆寫按鈕的 IsCompact 屬性。

若要將應用程式列按鈕標籤放在其圖示右邊，App 可以使用 CommandBar 的新 [**DefaultLabelPosition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx) 屬性。

```xaml
<CommandBar DefaultLabelPosition="Right">
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle"/>
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat"/>
</CommandBar>
```

以下是上述程式碼片段經由 App 繪製後看起來的樣子。

![標籤在右邊的命令列](images/app-bar-labels-on-right.png)

個別的應用程式列按鈕無法移動其標籤位置，這必須在命令列上以整體方式來完成。 應用程式列按鈕可以藉由將新的 [**LabelPosition**](https://msdn.microsoft.com/library/windows/apps/mt710920.aspx) 屬性設定為 **Collapsed**，指定一律不顯示它們的標籤。 我們建議僅限在普遍可辨識的圖解 (例如 '+') 上使用這項設定。

當您將應用程式列按鈕放在溢位功能表 (SecondaryCommands) 中時，它只會顯示為文字。 溢位中的應用程式列按鈕的 **LabelPosition** 將被忽略。 以下是相同的應用程式列切換按鈕，它在動作空間中顯示為一個主要命令 (上方)，在溢位區域中則顯示為一個次要命令 (下方)。

![成為主要和次要命令的應用程式列按鈕](images/app-bar-toggle-button-two-modes.png)

- *如果有會顯示在各頁面上的命令，最好將該命令保持在一致的位置。*
- *我們建議將 [接受]、[是]、[確定] 命令放在 [拒絕]、[否] 和 [取消] 的左邊。 一致性可讓使用者自信地在系統中移動，幫助他們將 app 瀏覽的知識從某個 app 運用到另一個 app。*

### 按鈕標籤

我們建議讓應用程式列按鈕標籤保持簡短，最好是單一字詞。 在應用程式列按鈕圖示下方放置較長的標籤會導致換行成多行，並會因此加高開啟之應用程式列的整體高度。 您可以在標籤的文字中包含選擇性連字號字元 (0x00AD)，用來提示文字分行的字元界限在何處。 在 XAML 中，您使用逸出序列來表示文字分行，就像這樣︰

```xaml
<AppBarButton Icon="Back" Label="Areally&#x00AD;longlabel"/>
```

當標籤在提示的位置換行時，它看起來像這樣。

![含換行標籤的應用程式列按鈕](images/app-bar-button-label-wrap.png)

### 其他內容

您可以設定 **Content** 屬性，以將任何 XAML 元素新增到內容區域。 若要新增多個元素，您必須將它們放在面板容器中，然後將面板變成 Content 屬性的單一子系。

當主要命令和內容都存在的時候，主要命令的優先順序較高，而且可能造成內容被裁剪。 

當 [**ClosedDisplayMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx) 為 **Compact** 時，如果內容比精簡大小的命令列還要大時，內容就會被裁剪。 您應該處理 [**Opening**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx) 和 [**Closed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) 事件，以顯示或隱藏內容區域中的 UI 部分，讓它們不被裁剪。 請參閱[開啟與關閉狀態](#open-and-closed-states)一節以取得詳細資訊。

## 開啟與關閉狀態

命令列可以開啟或關閉。 使用者可以按下 [查看更多] \[•••\] 按鈕，以在這些狀態之間切換。 您可以設定 [**IsOpen**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.isopen.aspx) 屬性，以程式設計方式在這些狀態之間切換。 開啟時，會顯示含文字標籤的主要命令按鈕，以及溢位功能表 (若有次要命令存在)，如先前所示。

您可以使用 [**Opening**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx)、[**Opened**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opened.aspx)、[**Closing**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closing.aspx) 與 [**Closed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) 事件來回應要開啟或關閉的命令列。  
- 轉換動畫開始之前，會發生 Opening 和 Closing 事件。
- 轉換完成後，發生 Opened 和 Closed 事件。

在此範例中，Opening 和 Closing 事件是用來變更命令列的不透明度。 當命令列關閉時，它會呈現半透明，因此使用者可以看到 app 背景。 當命令列開啟時，命令列會變成不透明，因此使用者可以將注意力集中在命令。

```xaml
<CommandBar Opening="CommandBar_Opening"
            Closing="CommandBar_Closing">
    <AppBarButton Icon="Accept" Label="Accept"/>
    <AppBarButton Icon="Edit" Label="Edit"/>
    <AppBarButton Icon="Save" Label="Save"/>
    <AppBarButton Icon="Cancel" Label="Cancel"/>
</CommandBar>
```

```csharp
private void CommandBar_Opening(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 1.0;
}

private void CommandBar_Closing(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 0.5;
}

```

### ClosedDisplayMode

您可以設定 [**ClosedDisplayMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx) 屬性，以控制如何顯示關閉狀態的命令列。 目前有 3 個關閉顯示模式可供選擇︰
- **Compact**：這是預設模式。 顯示內容、沒有標籤的主要命令圖示，以及 [查看更多] \[•••\] 按鈕。
- **Minimal**：只顯示精簡的一列來做為 [查看更多 ] \[•••\] 按鈕。 使用者可以按該列上的任意位置以開啟它。
- **Hidden**︰命令列關閉後就不會顯示。 這十分適合用來顯示一個含內嵌命令列的關聯式命令。 在此情況下，您必須設定 **IsOpen** 屬性或將 ClosedDisplayMode 變更為 **Minimal** 或 **Compact**，以程式設計方式來開啟命令列。

在這裡，命令列是用來放置 [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) 的簡單格式設定命令。 當焦點不在編輯方塊時，格式設定命令會干擾注意力，所以會隱藏起來。 使用編輯方塊時，命令列的 ClosedDisplayMode 會變更為 Compact，因此會顯示格式設定命令。

```xaml
<StackPanel Width="300"
            GotFocus="EditStackPanel_GotFocus"
            LostFocus="EditStackPanel_LostFocus">
    <CommandBar x:Name="FormattingCommandBar" ClosedDisplayMode="Hidden">
        <AppBarButton Icon="Bold" Label="Bold" ToolTipService.ToolTip="Bold"/>
        <AppBarButton Icon="Italic" Label="Italic" ToolTipService.ToolTip="Italic"/>
        <AppBarButton Icon="Underline" Label="Underline" ToolTipService.ToolTip="Underline"/>
    </CommandBar>
    <RichEditBox Height="200"/>
</StackPanel>
```

```csharp
private void EditStackPanel_GotFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Compact;
}

private void EditStackPanel_LostFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Hidden;
}
```

>**注意** &nbsp;&nbsp;此範例不討論如何實作編輯命令。 如需詳細資訊，請參閱 [RichEditBox](rich-edit-box.md) 文章。

雖然在某些情況下，Minimal 與 Hidden 模式很有用，但是請記住，隱藏所有動作可能造成使用者的混淆。

變更 ClosedDisplayMode 來提供更多或較少的提示給使用者，會影響周圍元素的配置。 相反地，當 CommandBar 在關閉和開啟之間轉換時，並不會影響其他元素的配置。

### IsSticky

開啟命令列後，如果使用者在控制項以外的任何地方與 app 互動，則根據預設，會關閉溢位功能表並隱藏標籤。 以這種方式關閉時，稱為「消失關閉」**。 您可以設定 [**IsSticky**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.issticky.aspx) 屬性，以控制列的關閉方式。 當命令列已相黏 (`IsSticky="true"`) 時，並無法透過消失關閉手勢關閉它。 命令列會保持開啟，直到使用者按 [查看更多] \[•••\] 按鈕，或從溢位功能表中選取項目為止。 建議您避免使用相黏命令列，因為它們不符合使用者對消失關閉的預期效果。

## 可行與禁止事項

### 放置

命令列可以放置在 app 視窗頂端、底部或內嵌在裡面。

![應用程式列放置範例 1](images/AppbarGuidelines_Placement1.png)

-   對於小型手持裝置，我們建議將命令列放置在螢幕底部以方便存取。
-   對於配備大型螢幕的裝置，如果您只要放置一個命令列，我們建議將它放置在接近視窗頂端位置。
使用 [**DiagonalSizeInInches**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.diagonalsizeininches.aspx) API 來判斷實際的螢幕大小。

命令列在單一檢視螢幕 (左側範例)，以及多檢視螢幕 (右側範例) 中可放置的畫面區域如下。 內嵌命令列可以放置在巨集指令空間中的任何地方。

![應用程式列放置範例 2](images/AppbarGuidelines_Placement2.png)

>**觸控裝置**︰如果當觸控式鍵盤或「螢幕輸入面板」(SIP) 出現時命令列必須保持可見，則您可以將命令列指派給頁面的 [BottomAppBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.bottomappbar.aspx) 屬性，如此一來，它就會在有 SIP 存在時，變成保持可見。 否則，您應該以內嵌方式將命令列放在 App 內容的相對位置。

### 動作

根據動作的可見性來決定它們在命令列中的優先順序。

-   將最重要的命令 (您希望在命令列中保持可見) 放在動作空間的前幾個位置。 在最小的螢幕 (320 epx 寬) 上，命令列的動作空間約可容納 2-4 個項目 (視畫面上的其他 UI 而定)。
-   將較不重要的命令放在列的動作空間後方，或放在溢位區域的前幾個位置。 當列有足夠的螢幕實際可用空間時，這些命令就會顯示；當沒有足夠的空間時，會納入溢位區域的下拉式功能表中。
-   將最不重要的命令放在溢位區域內。 這些命令將一律顯示在下拉式功能表中。

如果有會顯示在各頁面上的命令，最好將該命令保持在一致的位置。 我們建議將 [接受]、[是]、[確定] 命令放在 [拒絕]、[否] 和 [取消] 的左邊。 一致性可讓使用者自信地在系統中移動，幫助他們將 app 瀏覽的知識從某個 app 運用到另一個 app。

雖然您可以將所有動作放在溢位區域內，讓命令列上只顯示 [查看更多] \[•••\] 按鈕，但是請記住，隱藏所有動作可能造成使用者的混淆。

### 命令列飛出視窗

請考慮為命令使用邏輯式分組，例如在 [回覆] 功能表中放入 [回覆]、[全部回覆] 和 [轉寄]。 雖然通常應用程式列按鈕會啟動單一命令，但應用程式列按鈕也可以用來顯示包含自訂內容的 [**MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyout.aspx) 或 [**Flyout**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.aspx)。

![命令列上的飛出視窗範例](images/AppbarGuidelines_Flyouts.png)

### 溢位功能表

![包含 [更多] 區域的命令列範例](images/AppbarGuidelines_Illustration.png)

-   溢位功能表是由 [查看更多] \[•••\] 按鈕代表，也就是功能表的可見進入點。 它位於工具列最右側，與主要動作相鄰。
-   溢位區域是針對使用頻率較低的動作所配置。
-   動作可以在中斷點處，於主要動作空間與溢位功能表之間移動。 您也可以指定巨集指令保持顯示在主要巨集指令空間，而不受畫面或應用程式視窗大小影響。
-   即使在較大的螢幕上展開應用程式列時，不常用的巨集指令仍可以保留在溢位功能表內。

## 適應性

-   在直向和橫向方向中，應用程式列中可見的動作數目應該要相同，以減輕使用者的認知負擔。 在直式方向中，可用的動作應該由裝置寬度來決定。
-   在較適合單手使用的小型螢幕上，應用程式列應該位於靠近螢幕底部的位置。
-   在大型螢幕上，將應用程式列放在靠近視窗頂端的位置可讓它們更容易被注意到及找到。
-   透過設定中斷點目標，您可以在視窗大小變更時將動作移入和移出功能表。
-   透過設定螢幕對角線目標，您可以依據裝置螢幕大小修改應用程式列位置。
-   請考慮將標籤移動到應用程式列按鈕圖示的右邊，以改善可讀性。 位在底部的標籤需要使用者開啟命令列以顯示標籤，而在右邊的標籤即使命令列關閉也依然會顯示。 這項最佳化非常適合較大的視窗。

## 相關文章

**適用於設計人員** 
           [UWP app 的命令設計基本知識](../layout/commanding-basics.md)

**適用於開發人員 (XAML)** 
           [ **CommandBar** ](https://msdn.microsoft.com/library/windows/apps/dn279427)



<!--HONumber=Jul16_HO1-->


