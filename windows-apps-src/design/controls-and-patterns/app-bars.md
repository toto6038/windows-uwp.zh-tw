---
Description: Command bars give users easy access to your app's most common tasks.
title: 命令列
label: App bars/command bars
template: detail.hbs
op-migration-status: ready
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 868b4145-319b-4a97-82bd-c98d966144db
pm-contact: yulikl
design-contact: ksulliv
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1bf828fab97a5dddc26d669b771bb831b65c140f
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8329008"
---
# <a name="command-bar"></a>命令列

命令列可讓使用者輕鬆存取您的 App 最常見的工作。 命令列可以提供 App 層級或網頁特定命令的存取權，並且可以搭配任何瀏覽模式使用。

> **重要 API**：[CommandBar 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx)、[AppBarButton 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx)、[AppBarToggleButton 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx)、[AppBarSeparator 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx)

![具有圖示之命令列的範例](images/controls_appbar_icons.png)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

CommandBar 控制項是一般用途、彈性、輕量的控制項，它可以顯示複雜的內容 (例如影像或文字區塊)，也可以顥示簡單的命令 (例如 [AppBarButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx)、[AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx) 與 [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx) 控制項)。

> [!NOTE]
> XAML 提供 [AppBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar) 控制項和 [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar) 控制項。 只有在您要升級的通用 Windows 8 應用程式使用 AppBar，而且您想要將變更幅度控制在最小的情況下，才應該使用 AppBar。 對於 Windows 10 中的新 app，建議您改為使用 CommandBar 控制項。 此文件假設您使用 CommandBar 控制項。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong> App，請按一下這裡<a href="xamlcontrolsgallery:/item/CommandBar">開啟 App 並查看 CommandBar 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Microsoft 相片 app 中擴充的命令列。

![Microsoft 相片 app 中的命令列](images/control-examples/command-bar-photos.png)

Windows Phone 上 Outlook 行事曆中的命令列

![Outlook 行事曆 app 中的命令列](images/control-examples/command-bar-calendar-phone.png)

## <a name="anatomy"></a>結構

根據預設，命令列會顯示一排圖示按鈕與一個選擇性的 [查看更多] 按鈕，它會以省略符號 \[•••\] 表示。 這個命令列是由稍後顯示的範例程式碼建立。 顯示的命令列處於關閉、精簡狀態。

![關閉的命令列](images/command-bar-compact.png)

命令列也可以顯示成關閉、最小狀態，看起來像這樣。 請參閱[開啟與關閉狀態](#open-and-closed-states)一節以取得詳細資訊。

![關閉的命令列](images/command-bar-minimal.png)

以下是處於開啟狀態的相同命令列。 標籤會識別控制項的主要部分。

![關閉的命令列](images/commandbar_anatomy_open.png)

命令列分為 4 個主要區域︰
- 內容區域對齊命令列左側。 如果已填入 [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx) 屬性，就會顯示內容區域。
- 主要命令區域對齊命令列右側。 如果已填入 [PrimaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx) 屬性，就會顯示主要命令區域。  
- [查看更多] \[•••\] 按鈕顯示在命令列右側。 按 [查看更多] \[•••\] 按鈕會顯示主要命令標籤，如果有任何次要命令，還會開啟溢位功能表。 沒有主要命令標籤和次要標籤存在時，不會顯示此按鈕。 若要變更預設行為，請使用 [OverflowButtonVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.overflowbuttonvisibility.aspx) 屬性。
- 只有在命令列開啟且 [SecondaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) 屬性已填入時，才會顯示溢位功能表。 當空間有限時，主要命令會移到 SecondaryCommands 區域中。 若要變更預設行為，請使用 [IsDynamicOverflowEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled.aspx) 屬性。

當 [FlowDirection](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.flowdirection.aspx) 是 **RightToLeft** 時，配置會反轉。

## <a name="create-a-command-bar"></a>建立命令列
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
        <AppBarButton Label="Like" Click="AppBarButton_Click"/>
        <AppBarButton Label="Dislike" Click="AppBarButton_Click"/>
    </CommandBar.SecondaryCommands>

    <CommandBar.Content>
        <TextBlock Text="Now playing..." Margin="12,14"/>
    </CommandBar.Content>
</CommandBar>
```

## <a name="commands-and-content"></a>命令與內容
CommandBar 控制項有 3 個可用來新增命令及內容的屬性：[PrimaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx)、[SecondaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) 和 [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx)。


### <a name="commands"></a>命令

預設會將命令列項目新增至 **PrimaryCommands** 集合。 您應該依照命令的重要性來新增命令，讓最重要的命令永遠都看得到。 命令列寬度變更 (例如使用者調整應用程式視窗大小) 時，主要命令會動態地在命令列與溢位功能表之間往中斷點移動。 若要變更此預設行為，請使用 [IsDynamicOverflowEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled.aspx) 屬性。 

在最小的螢幕 (320 epx 寬) 上，命令列最多容納 4 個主要命令。 

您也可以將命令新增至 **SecondaryCommands** 集合，這些命令會顯示在溢位功能表中。

![包含 [更多] 區域及圖示的命令列範例](images/appbar_rs2_overflow_icons.png)

您可以透過程式設計方式，視需要在 PrimaryCommands 與 SecondaryCommands 之間移動命令。

- *如果有慣常在各頁面顯示的命令，最好讓這個命令保持在一致的位置。*
- *我們建議將 [接受]、[是]、[確定] 命令放在 [拒絕]、[否] 和 [取消] 的左邊。 一致性可讓使用者有自信地在系統中四處移動，並幫助他們在應用程式上運用從其他應用程式學到的應用程式瀏覽知識。*

### <a name="app-bar-buttons"></a>應用程式列按鈕

PrimaryCommands 和 SecondaryCommands 都只能填入 [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx)、[AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx) 和 [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) 命令元素。 

應用程式列按鈕控制項可依圖示和文字標籤加以區分。 這些控制項最適合在命令列中使用，其外觀會改變，取決於控制項是在命令列還是在溢位功能表中使用。

溢位功能表中圖示的大小為 16x16px，這小於主要命令區域中圖示的大小 (20x20px)。 如果您使用 SymbolIcon、FontIcon 或 PathIcon，當命令進入次要命令區域時，圖示將會自動縮放至正確大小，但逼真度並不會降低。 

### <a name="button-labels"></a>按鈕標籤
AppBarButton [IsCompact](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.IsCompact) 屬性決定是否顯示標籤。 在 CommandBar 控制項中，當命令列開啟或關閉時，命令列會自動覆寫按鈕的 IsCompact 屬性。

若要放置應用程式列按鈕標籤，請使用 CommandBar 的 [DefaultLabelPosition](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx) 屬性。

```xaml
<CommandBar DefaultLabelPosition="Right">
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle"/>
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat"/>
</CommandBar>
```

![標籤在右邊的命令列](images/app-bar-labels-on-right.png)

在較大的視窗上，請考慮將標籤移動到應用程式列按鈕圖示的右邊，以改善可讀性。 位在底部的標籤需要使用者開啟命令列以顯示標籤，而在右邊的標籤即使命令列關閉也依然會顯示。

在溢位功能表中，預設會將標籤放置在圖示右邊，並忽略 **LabelPosition**。 您可以將 [CommandBarOverflowPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar.CommandBarOverflowPresenterStyle) 屬性設定為以 [CommandBarOverflowPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbaroverflowpresenter) 為目標的 Style，以調整樣式。 

按鈕標籤應簡短，最好只有一個字。 圖示下方較長的標籤會換行成多行，並增加所開啟之命令列的整體高度。 您可以在標籤的文字中包含選擇性連字號字元 (0x00AD)，用來提示文字分行的字元界限在何處。 在 XAML 中，您使用逸出序列來表示文字分行，就像這樣︰

```xaml
<AppBarButton Icon="Back" Label="Areally&#x00AD;longlabel"/>
```

當標籤在提示的位置換行時，它看起來像這樣。

![含換行標籤的應用程式列按鈕](images/app-bar-button-label-wrap.png)

### <a name="command-bar-flyouts"></a>命令列飛出視窗

請考慮為命令使用邏輯式分組，例如在 [回覆] 功能表中放入 [回覆]、[全部回覆] 和 [轉寄]。 雖然通常應用程式列按鈕會啟動單一命令，但應用程式列按鈕也可以用來顯示包含自訂內容的 [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyout.aspx) 或 [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.aspx)。

![命令列上的飛出視窗範例](images/AppbarGuidelines_Flyouts.png)

### <a name="other-content"></a>其他內容

您可以設定 **Content** 屬性，以將任何 XAML 元素新增到內容區域。 若要新增多個元素，您必須將它們放在面板容器中，然後將面板變成 Content 屬性的單一子系。

啟用動態溢位時，內容不會剪短，因為主要命令可以移入溢位功能表。 未啟用時，優先顯示主要命令，並可能導致內容遭裁剪。

當 [ClosedDisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx) 為 **Compact** 時，如果內容比精簡大小的命令列還要大時，內容就會被裁剪。 您應該處理 [Opening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx) 和 [Closed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) 事件，以顯示或隱藏內容區域中的 UI 部分，讓它們不被裁剪。 請參閱[開啟與關閉狀態](#open-and-closed-states)一節以取得詳細資訊。


## <a name="open-and-closed-states"></a>開啟與關閉狀態

可以開啟或關閉命令列。 開啟時，顯示含文字標籤的主要命令按鈕，如果有次要命令存在，還會開啟溢位功能表。

使用者可以按下 [查看更多] \[•••\] 按鈕，以在這些狀態之間切換。 您可以設定 [IsOpen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.isopen.aspx) 屬性，以程式設計方式在這些狀態之間切換。 

您可以使用 [Opening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx)、[Opened](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opened.aspx)、[Closing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closing.aspx) 和 [Closed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) 事件來回應要開啟或關閉的命令列。  
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

### <a name="issticky"></a>IsSticky

如果使用者在命令列開啟時與應用程式的其他部分互動，則命令列會自動關閉。 這稱為*消失關閉*。 您可以設定 [IsSticky](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.issticky.aspx) 屬性來控制消失關閉行為。 當 `IsSticky="true"` 時，命令列會保持開啟，直到使用者按 [查看更多] \[•••\] 按鈕，或從溢位功能表中選取項目為止。 

建議您避免使用相黏命令列，因為它們不符合使用者對消失關閉的預期效果。

### <a name="display-mode"></a>顯示模式

您可以設定 [ClosedDisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx) 屬性來控制命令列在關閉狀態下的顯示方式。 目前有 3 個關閉顯示模式可供選擇︰
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

>**注意**&nbsp;&nbsp;此範例不討論如何實作編輯命令。 如需詳細資訊，請參閱 [RichEditBox](rich-edit-box.md) 文章。

雖然在某些情況下，Minimal 與 Hidden 模式很有用，但是請記住，隱藏所有動作可能造成使用者的混淆。

變更 ClosedDisplayMode 來提供更多或較少的提示給使用者，會影響周圍元素的配置。 相反地，當 CommandBar 在關閉和開啟之間轉換時，並不會影響其他元素的配置。

## <a name="placement"></a>放置
命令列可以放置在 app 視窗頂端、底部或內嵌在裡面。

![應用程式列放置範例 1](images/AppbarGuidelines_Placement1.png)

-   對於小型手持式裝置，我們建議將命令列放置在螢幕底部以方便存取。
-   若是有較大型螢幕的裝置，將命令列放在接近視窗頂端的位置會讓這些命令列更顯眼且更容易找到。

使用 [DiagonalSizeInInches](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.diagonalsizeininches.aspx) API 來判斷實際的螢幕大小。

命令列在單一檢視螢幕 (左側範例)，以及多檢視螢幕 (右側範例) 中可放置的畫面區域如下。 內嵌命令列可以放置在巨集指令空間中的任何地方。

![應用程式列放置範例 2](images/AppbarGuidelines_Placement2.png)

>**觸控裝置**︰如果當觸控式鍵盤或「螢幕輸入面板」(SIP) 出現時命令列必須保持可見，則您可以將命令列指派給頁面的 [BottomAppBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.bottomappbar.aspx) 屬性，如此一來，它就會在有 SIP 存在時，變成保持可見。 否則，您應該以內嵌方式將命令列放在應用程式內容的相對位置。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。
- [XAML 命令範例](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>相關文章

* [UWP 應用程式的命令設計基本知識](../basics/commanding-basics.md)
* [CommandBar 類別](https://msdn.microsoft.com/library/windows/apps/dn279427)
