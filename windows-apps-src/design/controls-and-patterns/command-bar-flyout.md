---
Description: 命令列延伸顯示讓使用者內嵌存取您的應用程式最常見的工作。
title: 命令列飛出視窗
label: Command bar flyout
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d5774b5301f7e8ce0616df72cfbf4fc81d0d0cf7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363245"
---
# <a name="command-bar-flyout"></a>命令列飛出視窗

命令列飛出視窗可讓您為使用者提供輕鬆存取一般工作，藉由在浮動工具列與 UI 畫布上的項目中顯示命令。

![展開的文字命令列飛出視窗](images/command-bar-flyout-header.png)

> CommandBarFlyout 需要 Windows 10 版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本，或有[Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

> - **平台 Api**:[CommandBarFlyout 類別](/uwp/api/windows.ui.xaml.controls.commandbarflyout)， [TextCommandBarFlyout 類別](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout)， [AppBarButton 類別](/uwp/api/windows.ui.xaml.controls.appbarbutton)， [AppBarToggleButton 類別](/uwp/api/windows.ui.xaml.controls.appbartogglebutton)， [AppBarSeparator 類別](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **Windows UI 程式庫 Api**:[CommandBarFlyout 類別](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)， [TextCommandBarFlyout 類別](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

像是[CommandBar](app-bars.md)，CommandBarFlyout 已**PrimaryCommands**並**SecondaryCommands**屬性可用來將命令加入。 您可以將命令放在集合中，或兩者。 何時及如何顯示主要和次要的命令取決於顯示模式。

命令列飛出視窗上有兩種顯示模式：*摺疊*並*展開*。

- 在摺疊模式中，會顯示主要的命令。 如果您的命令列飛出視窗有主要和次要命令，「 請查看更多的項目 」 按鈕，表示的省略符號\[• • •\]，隨即出現。 這可讓使用者取得存取權的第二個命令轉換為展開的模式。
- 在展開模式中，會顯示主要和次要的命令。 （如果控制項具有次要的項目，它們會顯示類似 MenuFlyout 控制項的方式）。

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用 CommandBarFlyout 控制項以顯示給使用者，例如按鈕和功能表項目，應用程式畫布上的項目內容中的命令集合。

TextCommandBarFlyout 會在文字方塊中，TextBlock、 RichEditBox、 RichTextBlock 和 PasswordBox 控制項中顯示的文字命令。 命令會自動適當地設定為目前文字選取範圍。 您可以使用 CommandBarFlyout 取代文字控制項的預設文字命令。

若要顯示內容清單項目上的命令會遵循中的指導方針[集合和清單的內容命令](collection-commanding.md)。

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout vs MenuFlyout

若要顯示內容功能表中的命令，您可以使用 CommandBarFlyout 或 MenuFlyout。 我們建議 CommandBarFlyout，因為它提供更多的功能比 MenuFlyout。 您可以使用 CommandBarFlyout 只有第二個命令取得行為和外觀 MenuFlyout，或使用完整命令列飛出視窗主要和次要的命令。

> 相關的資訊，請參閱[延伸顯示](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)，[功能表和操作功能表](menus.md)，並[命令列](app-bars.md)。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您有<strong style="font-weight: semi-bold">XAML 控制項陳列庫</strong>應用程式安裝，請按一下這裡可<a href="xamlcontrolsgallery:/item/CommandBarFlyout">開啟應用程式，並查看動作中的 CommandBarFlyout</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>主動式與反應式引動過程

通常有兩種方式來叫用的彈出式視窗或在 UI 畫布上的項目相關聯的功能表：_主動式引動過程_並_回應式的引動過程_。

主動式的引動過程中,，使用者與命令相關聯的項目互動時的大小時命令會自動出現。 例如，文字格式化命令可能會快顯當使用者選取文字在文字方塊中。 在此情況下，命令列飛出視窗不會不會取得焦點。 相反地，它會呈現接近使用者互動的項目相關的命令。 如果使用者不會與命令互動，它們會將它關閉。

在回應式的引動過程，命令會以明確的使用者動作的回應顯示要求的命令;例如，以滑鼠右鍵按一下。 這會對應至傳統的觀念[快顯功能表](menus.md)。

您可以使用中的方法或甚至是兩者的混合 CommandBarFlyout。

## <a name="create-a-command-bar-flyout"></a>建立命令列飛出視窗

此範例示範如何建立命令列飛出視窗並使用它，主動和被動。 點選 映像時，其摺疊模式是顯示飛出視窗。 當顯示為操作功能表，飛出視窗會顯示在其展開的模式。 在任一情況下，使用者可以展開或摺疊飛出視窗上，開啟後。

![摺疊的命令列飛出視窗的範例](images/command-bar-flyout-img-collapsed.png)

> _摺疊的命令列飛出視窗_

![展開的命令列飛出視窗的範例](images/command-bar-flyout-img-expanded.png)

> _展開的命令列飛出視窗_

```xaml
<Grid>
    <Grid.Resources>
        <CommandBarFlyout x:Name="ImageCommandsFlyout">
            <AppBarButton Icon="OutlineStar" ToolTipService.ToolTip="Favorite"/>
            <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
            <AppBarButton Icon="Share" ToolTipService.ToolTip="Share"/>
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Select all"/>
                <AppBarButton Label="Delete" Icon="Delete"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           Tapped="Image_Tapped" FlyoutBase.AttachedFlyout="{x:Bind ImageCommandsFlyout}"
           ContextFlyout="{x:Bind ImageCommandsFlyout}"/>
</Grid>
```

```csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    var flyout = FlyoutBase.GetAttachedFlyout((FrameworkElement)sender);
    var options = new FlyoutShowOptions()
    {
        // Position shows the flyout next to the pointer.
        // "Transient" ShowMode makes the flyout open in its collapsed state.
        Position = e.GetPosition((FrameworkElement)sender),
        ShowMode = FlyoutShowMode.Transient
    };
    flyout?.ShowAt((FrameworkElement)sender, options);
}
```

### <a name="show-commands-proactively"></a>主動顯示命令

當您主動顯示內容的命令時，只有主要的命令應該會顯示預設 （命令列飛出視窗應該會摺疊）。 將最重要的命令放在主要的命令集合，並會傳統上內容功能表中移至第二個命令集合的其他命令。

若要主動顯示命令，您通常會處理[按一下 ](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)或[Tapped](/uwp/api/windows.ui.xaml.uielement.tapped)顯示命令列飛出視窗上的事件。 設定飛出視窗的[ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode)要**暫時性**或是**TransientWithDismissOnPointerMoveAway**在其摺疊模式中開啟飛出視窗上，而不需要讓焦點。

從 Windows 10 Insider Preview 開始，文字控制項都有**SelectionFlyout**屬性。 當您將飛出視窗指派給這個屬性時，會自動顯示時選取的文字。

### <a name="show-commands-reactively"></a>被動顯示命令

內容相關的命令是被動的操作功能表顯示的第二個命令會顯示 （應該會展開命令列飛出視窗上） 的預設。 在此情況下，命令列飛出視窗可能主要和次要的命令或次要的命令。

若要顯示內容功能表中的命令，您通常會指派飛出視窗[ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) UI 項目的屬性。 如此一來，開啟飛出視窗由元素的方式來處理，而且您不需要執行任何動作。

如果您處理自己顯示飛出視窗 (例如，在[RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped)事件)，設定飛出視窗的[ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode)來**標準**開啟飛出視窗在其展開的模式和給予焦點。

> [!TIP]
> 如需時顯示飛出視窗以及如何控制飛出視窗放置選項的詳細資訊，請參閱[延伸顯示](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)。

## <a name="commands-and-content"></a>命令與內容

CommandBarFlyout 控制項具有可用來將命令和內容的 2 個屬性：[PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands)並[SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands)。

預設會將命令列項目新增至 **PrimaryCommands** 集合。 這些命令會顯示在命令列中，而且會出現在摺疊或展開的模式。 不同於 CommandBar，主要的命令不會自動發生溢位的第二個命令，並可能會被截斷。

您也可以新增命令**SecondaryCommands**集合。 第二個命令會顯示在控制項中功能表的一部分，並只會顯示在展開的模式。

### <a name="app-bar-buttons"></a>應用程式列按鈕

您可以填入 PrimaryCommands 和直接與 SecondaryCommands [AppBarButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)， [AppBarToggleButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton)，並[AppBarSeparator](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator)控制項。

應用程式列按鈕控制項可依圖示和文字標籤加以區分。 這些控制項適用於在命令列中使用，其外觀會變更取決於在命令列或溢位功能表是否顯示控制項。

- 應用程式列按鈕做為主要的命令會顯示在命令列中，以其圖示;不顯示的文字標籤。 我們建議您若要顯示的文字描述的命令，使用工具提示，如下所示。
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- 應用程式列按鈕做為第二個命令會顯示在功能表中，使用標籤和顯示的圖示。

### <a name="other-content"></a>其他內容

您可以裹住 AppBarElementContainer，將其他控制項加入命令列飛出視窗中。 這可讓您新增的控制項，例如[DropDownButton](buttons.md)或是[SplitButton](buttons.md)，或新增容器喜歡[StackPanel](buttons.md)建立更複雜的 UI。

項目必須實作才能將它加入至命令列飛出視窗的主要或次要的命令集合， [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement)介面。 AppBarElementContainer 是實作這個介面，因此您可以將項目新增至命令列，即使它不會實作介面本身的包裝函式。

在這裡，AppBarElementContainer 用來將額外的項目新增至命令列飛出視窗。 SplitButton 會加入至主要的命令，以允許選取的色彩。 StackPanel 會加入到次要的命令，以允許更複雜的配置，縮放控制項。

> [!TIP]
> 根據預設，專為應用程式畫布上的項目看起來可能不正確的命令列中。 當您將使用 AppBarElementContainer 的項目時，則讓項目符合其他命令列項目，您應該採取的步驟：
>
> - 覆寫的預設筆刷[輕量級樣式](/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling)進行此項目的背景和符合應用程式列按鈕的框線。
> - 調整大小和位置的項目。
> - 在使用的寬度和高度 16px Viewbox 包裝圖示。

> [!NOTE]
> 這個範例示範只命令列飛出視窗的 UI，它不會實作任何顯示的命令。 如需實作命令的詳細資訊，請參閱[ 按鈕](buttons.md)並[命令設計基本概念](../basics/commanding-basics.md)。

![命令列飛出視窗的分割按鈕](images/command-bar-flyout-split-button.png)

> _開啟的 SplitButton 摺疊的命令列飛出視窗_

![命令列飛出視窗的複雜的 UI](images/command-bar-flyout-custom-ui.png)

> _展開的命令列飛出視窗的功能表中自訂顯示比例 UI_


```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Cut" ToolTipService.ToolTip="Cut"/>
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    <AppBarButton Icon="Paste" ToolTipService.ToolTip="Paste"/>
    <!-- Alignment controls -->
    <AppBarElementContainer>
        <SplitButton ToolTipService.ToolTip="Alignment">
            <SplitButton.Resources>
                <!-- Override default brushes to make the SplitButton 
                     match other command bar elements. -->
                <Style TargetType="SplitButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="SplitButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="SplitButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrush" Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="SplitButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </SplitButton.Resources>
            <SplitButton.Content>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="AlignLeft"/>
                </Viewbox>
            </SplitButton.Content>
            <SplitButton.Flyout>
                <MenuFlyout>
                    <MenuFlyoutItem Icon="AlignLeft" Text="Align left"/>
                    <MenuFlyoutItem Icon="AlignCenter" Text="Center"/>
                    <MenuFlyoutItem Icon="AlignRight" Text="Align right"/>
                </MenuFlyout>
            </SplitButton.Flyout>
        </SplitButton>
    </AppBarElementContainer>
    <!-- end Alignment controls -->
    <CommandBarFlyout.SecondaryCommands>
        <!-- Zoom controls -->
        <AppBarElementContainer>
            <AppBarElementContainer.Resources>
                <!-- Override default brushes to make the Buttons 
                     match other command bar elements. -->
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>
                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
                <Style TargetType="TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
                <Style TargetType="Button">
                    <Setter Property="Height" Value="40"/>
                    <Setter Property="Width" Value="40"/>
                </Style>
            </AppBarElementContainer.Resources>
            <Grid Margin="12,-4">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="76"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                <Viewbox Width="16" Height="16" Margin="0,2,0,0">
                    <SymbolIcon Symbol="Zoom"/>
                </Viewbox>
                <TextBlock Text="Zoom" Margin="10,0,0,0" Grid.Column="1"/>
                <StackPanel Orientation="Horizontal" Grid.Column="2">
                    <Button ToolTipService.ToolTip="Zoom out">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomOut"/>
                        </Viewbox>
                    </Button>
                    <TextBlock Text="50%" Width="40"
                               HorizontalTextAlignment="Center"/>
                    <Button ToolTipService.ToolTip="Zoom in">
                        <Viewbox Width="16" Height="16">
                            <SymbolIcon Symbol="ZoomIn"/>
                        </Viewbox>
                    </Button>
                </StackPanel>
            </Grid>
        </AppBarElementContainer>
        <!-- end Zoom controls -->
        <AppBarSeparator/>
        <AppBarButton Label="Undo" Icon="Undo"/>
        <AppBarButton Label="Redo" Icon="Redo"/>
        <AppBarButton Label="Select all" Icon="SelectAll"/>
    </CommandBarFlyout.SecondaryCommands>
</CommandBarFlyout>
```

## <a name="create-a-context-menu-with-secondary-commands-only"></a>第二個命令中只能使用建立的內容功能表

您可以使用只有第二個命令為 CommandBarFlyout[快顯功能表](menus.md)，MenuFlyout 取代。

![只有第二個命令使用命令列飛出視窗](images/command-bar-flyout-context-menu.png)

> _為操作功能表的命令列飛出視窗_

```xaml
<Grid>
    <Grid.Resources>
        <!-- A command bar flyout with only secondary commands. -->
        <CommandBarFlyout x:Name="ContextMenu">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Copy" Icon="Copy"/>
                <AppBarButton Label="Save" Icon="Save"/>
                <AppBarButton Label="Print" Icon="Print"/>
                <AppBarSeparator />
                <AppBarButton Label="Properties"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/image1.png" Width="300"
           ContextFlyout="{x:Bind ContextMenu}"/>
</Grid>
```

您也可以使用與 DropDownButton 的 CommandBarFlyout 建立標準功能表。

![命令列飛出視窗為按鈕下拉式功能表](images/command-bar-flyout-dropdown.png)

> _在命令列飛出視窗下拉式功能表 按鈕_

```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Placeholder"/>
    <AppBarElementContainer>
        <DropDownButton Content="Mail">
            <DropDownButton.Resources>
                <!-- Override default brushes to make the DropDownButton 
                     match other command bar elements. -->
                <Style TargetType="DropDownButton">
                    <Setter Property="Height" Value="38"/>
                </Style>
                <SolidColorBrush x:Key="ButtonBackground"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed"
                                 Color="{ThemeResource SystemListMediumColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver"
                                 Color="{ThemeResource SystemListLowColor}"/>

                <SolidColorBrush x:Key="ButtonBorderBrush"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushPointerOver"
                                 Color="Transparent"/>
                <SolidColorBrush x:Key="ButtonBorderBrushChecked"
                                 Color="Transparent"/>
            </DropDownButton.Resources>
            <DropDownButton.Flyout>
                <CommandBarFlyout Placement="BottomEdgeAlignedLeft">
                    <CommandBarFlyout.SecondaryCommands>
                        <AppBarButton Icon="MailReply" Label="Reply"/>
                        <AppBarButton Icon="MailReplyAll" Label="Reply all"/>
                        <AppBarButton Icon="MailForward" Label="Forward"/>
                    </CommandBarFlyout.SecondaryCommands>
                </CommandBarFlyout>
            </DropDownButton.Flyout>
        </DropDownButton>
    </AppBarElementContainer>
    <AppBarButton Icon="Placeholder"/>
    <AppBarButton Icon="Placeholder"/>
</CommandBarFlyout>
```

## <a name="command-bar-flyouts-for-text-controls"></a>命令列的延伸顯示的文字控制項

[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)是特製化的命令列飛出視窗，其中包含用來編輯文字命令。 每個文字控制項自動顯示 TextCommandBarFlyout，為操作功能表 （按一下滑鼠右鍵），或選取的文字。 文字命令列飛出視窗調整至文字選取範圍，只顯示相關的命令。

![摺疊的文字命令列飛出視窗](images/command-bar-flyout-text-selection.png)

> _文字命令列飛出視窗上的文字選取範圍_

![展開的文字命令列飛出視窗](images/command-bar-flyout-text-full.png)

> _展開的文字命令列飛出視窗_


### <a name="available-commands"></a>可用的命令

下表顯示包含在 TextCommandBarFlyout，以及它們顯示的命令。

| 命令 | 顯示此項目... |
| ------- | -------- |
| Bold | 當文字控制項不是唯讀狀態 (RichEditBox 只)。 |
| Italic | 當文字控制項不是唯讀狀態 (RichEditBox 只)。 |
| Underline | 當文字控制項不是唯讀狀態 (RichEditBox 只)。 |
| 校訂 | 何時 IsSpellCheckEnabled **，則為 true**和選取的拼字錯誤的文字。 |
| 剪下 | 當文字控制項不是唯讀，並選取文字。 |
| 複製 | 選取文字時。 |
| 貼上 | 當文字控制項不是唯讀，且剪貼簿內容。 |
| 復原 | 可復原的動作時。 |
| 全選 | 當您可以選取文字。 |

### <a name="custom-text-command-bar-flyouts"></a>自訂文字命令列延伸顯示

TextCommandBarFlyout 無法自訂，並自動管理的每個文字控制項。 不過，您可以將預設 TextCommandBarFlyout 取代自訂命令。

- 若要取代預設文字選取範圍上所顯示的 TextCommandBarFlyout，您可以建立自訂的 CommandBarFlyout （或其他彈出式視窗類型），並將它指派給**SelectionFlyout**屬性。 如果您將設定 SelectionFlyout **null**，沒有命令會顯示在選取項目。
- 若要取代預設會顯示為 [內容] 功能表的 TextCommandBarFlyout，指派給自訂 CommandBarFlyout （或其他彈出式視窗類型） **ContextFlyout**文字控制項上的屬性。 如果您將設定 ContextFlyout **null**、 顯示的功能表彈出式視窗控制項顯示而不是 TextCommandBarFlyout 在舊版中的文字。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以互動式格式查看所有 XAML 控制項。
- [XAML 命令範例](https://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>相關文章

- [命令用於 UWP 應用程式的設計基本概念](../basics/commanding-basics.md)
- [CommandBar 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)
