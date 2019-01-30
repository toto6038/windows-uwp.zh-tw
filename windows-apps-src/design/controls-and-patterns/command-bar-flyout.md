---
Description: Command bar flyouts give users inline access to your app's most common tasks.
title: 命令列飛出視窗
label: Command bar flyout
template: detail.hbs
ms.date: 10/2/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: dc4adf5002858b394c6908b91f14557c3e1eed7b
ms.sourcegitcommit: a60ab85e9f2f9690e0141050ec3aa51f18ec61ec
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2019
ms.locfileid: "9037340"
---
# <a name="command-bar-flyout"></a>命令列飛出視窗

命令列飛出視窗可讓您與您的 UI 畫布上的項目相關的浮動工具列中顯示命令提供使用者輕鬆存取常見的工作。

![已展開的文字命令列飛出視窗](images/command-bar-flyout-header.png)

> CommandBarFlyout 需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本，或[Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

> - **平台 Api**: [CommandBarFlyout 類別](/uwp/api/windows.ui.xaml.controls.commandbarflyout)、 [TextCommandBarFlyout 類別](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout)、 [AppBarButton 類別](/uwp/api/windows.ui.xaml.controls.appbarbutton)、 [AppBarToggleButton 類別](/uwp/api/windows.ui.xaml.controls.appbartogglebutton)、 [AppBarSeparator 類別](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>- **Windows UI 程式庫 Api**: [CommandBarFlyout 類別](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)、 [TextCommandBarFlyout 類別](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)

[CommandBar](app-bars.md)，例如 CommandBarFlyout 有**PrimaryCommands**和**SecondaryCommands**的屬性，您可用來新增命令。 您可以將命令放在集合，或兩者。 主要和次要命令會顯示的時機和方式，取決於顯示模式。

命令列飛出視窗會有兩種顯示模式：*摺疊*及*展開*。

- 在已摺疊模式中，會顯示主要的命令。 如果您的命令列飛出視窗具有主要和次要命令，[查看更多] 按鈕，這會以省略符號 \ [• • • \，] 會顯示。 這可讓使用者透過轉換為展開模式，以取得存取次要命令。
- 在展開模式中，會顯示主要和次要命令。 （如果控制項有次要的項目，它們會顯示類似於 MenuFlyout 控制項的方式）。

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用 CommandBarFlyout 控制項來顯示給使用者，例如按鈕與功能表項目，在 app 畫布上元素的內容中的一組命令。

TextCommandBarFlyout TextBox、 TextBlock、 RichEditBox、 RichTextBlock，以及 PasswordBox 在控制項中顯示文字的命令。 命令都會自動適當地設定為目前所選取的文字。 您可以使用 CommandBarFlyout 來取代文字控制項上的預設文字命令。

要顯示與內容相關命令清單項目上的，請依照下列[Contextual 集合和清單的命令功能](collection-commanding.md)中的指導方針。

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout vs MenuFlyout

若要顯示在操作功能表中的命令，您可以使用 CommandBarFlyout 或 MenuFlyout。 我們建議 CommandBarFlyout，因為它提供更多的功能，比 MenuFlyout。 您可以使用 CommandBarFlyout 僅次要命令，以取得的行為和看起來的 MenuFlyout，或使用完整的命令列飛出視窗主要和次要命令。

> 相關資訊，請參閱[飛出視窗](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)、[功能表和操作功能表](menus.md)，和[命令列](app-bars.md)。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝的<strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，按一下這裡<a href="xamlcontrolsgallery:/item/CommandBarFlyout">開啟應用程式</a>並查看 CommandBarFlyout 情形。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>主動與反應引動過程的比較

通常有兩種方式來叫用的飛出視窗或有相關聯的項目，您的 UI 畫布上的功能表：_主動式引動過程_並_反應引動過程_。

主動式引動過程中，命令時，自動出現在使用者與命令相關的項目互動。 例如，文字格式設定命令可能會跳出當使用者在文字方塊中選取的文字。 在此情況下，命令列飛出視窗不會焦點。 相反地，它會顯示與使用者互動的項目接近相關的命令。 如果使用者不會與命令互動，它們可將它關閉。

在被動引動過程中，命令會顯示以明確的使用者動作來回應要求命令;例如，以滑鼠右鍵按一下。 這會對應到傳統[操作功能表](menus.md)的概念。

您可以使用中的方式或甚至是兩個混合 CommandBarFlyout。

## <a name="create-a-command-bar-flyout"></a>建立命令列飛出視窗

這個範例示範如何建立命令列飛出視窗並主動及疑問，請使用它。 當點選影像時，飛出視窗會顯示在其摺疊模式。 當顯示為操作功能表，飛出視窗會顯示在其展開模式。 在任一情況下，使用者可以展開或摺疊飛出視窗之後開啟它。

![摺疊的命令列飛出視窗的範例](images/command-bar-flyout-img-collapsed.png)

> _摺疊的命令列飛出視窗_

![擴充的命令列飛出視窗的範例](images/command-bar-flyout-img-expanded.png)

> _擴充的命令列飛出視窗_

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

當您主動顯示關聯式命令時，只主要命令應該會顯示預設 （應該摺疊命令列飛出視窗）。 將最重要的命令放在主要命令集合和傳統上來說會在操作功能表中移到次要命令集合的其他命令。

若要主動顯示命令，您通常會處理[按一下](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)或[Tapped](/uwp/api/windows.ui.xaml.uielement.tapped)事件，以顯示命令列飛出視窗。 設定飛出視窗的[ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode)為**暫時性**或**TransientWithDismissOnPointerMoveAway**在其摺疊模式中開啟飛出視窗，而不進行對焦。

從 Windows 10 Insider Preview 中，文字控制項有**SelectionFlyout**屬性。 當您指派給這個屬性的飛出視窗時，它會自動顯示選取文字時。

### <a name="show-commands-reactively"></a>被動顯示命令

當您被動操作功能表顯示的關聯式命令時，次要命令會顯示預設 （應該展開命令列飛出視窗）。 在此情況下，命令列飛出視窗可能會有主要和次要命令或僅次要命令。

若要顯示在操作功能表中的命令，您通常指派飛出視窗的 UI 元素的[ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout)屬性。 如此一來，開啟飛出視窗由項目，來處理，而且您不需要執行任何動作。

如果您處理自行顯示飛出視窗，（例如，在上一個[RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped)事件），請設定飛出視窗的[ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode)為**標準**來開啟飛出視窗中其展開模式，並為它提供焦點。

> [!TIP]
> 如需時顯示飛出視窗，以及如何控制飛出視窗的位置選項的詳細資訊，請參閱[飛出視窗](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)。

## <a name="commands-and-content"></a>命令與內容

CommandBarFlyout 控制項有 2 個您可用來新增命令與內容的屬性： [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands)和[SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands)。

預設會將命令列項目新增至 **PrimaryCommands** 集合。 這些命令會顯示在命令列中，會顯示在摺疊或展開模式。 不同於 CommandBar，主要命令不會自動執行次要命令的溢位以及可能會被截斷。

您也可以新增到**SecondaryCommands**集合的命令。 次要命令會顯示在功能表部分控制項的而且會顯示只有在展開模式。

### <a name="app-bar-buttons"></a>應用程式列按鈕

您可以填入 PrimaryCommands 和 SecondaryCommands 直接使用[AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx)、 [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx)，以及[AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx)控制項。

應用程式列按鈕控制項可依圖示和文字標籤加以區分。 這些控制項最適合用在命令列中，以及其外觀會改變，取決於控制項在命令列還是在溢位功能表中所示。

- 應用程式列按鈕做為主要命令會顯示在命令列中使用其圖示;不會顯示的文字標籤。 我們建議您要顯示的文字說明命令，使用工具提示，如下所示。
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- 做為次要命令的應用程式列按鈕會顯示在功能表中，使用標籤和顯示圖示。

### <a name="other-content"></a>其他內容

您可以 AppBarElementContainer 中包裝它們，將其他控制項新增到命令列飛出視窗。 這可讓您新增控制項，例如[DropDownButton]()或[SplitButton]()，或新增像是[StackPanel]()來建立更複雜的 UI 容器。

若要新增到主要或次要命令的集合命令列飛出視窗，元素必須實作[ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement)介面。 AppBarElementContainer 是實作這個介面，讓您可以將元素新增到命令列，即使它不會實作介面本身的包裝函式。

在這裡，AppBarElementContainer 用來將額外的項目新增到命令列飛出視窗。 SplitButton 會新增到主要命令，以允許選擇的色彩。 StackPanel 會新增到允許縮放控制項的更複雜的版面配置的次要命令。

> [!TIP]
> 根據預設，應用程式畫布的設計元素看起來可能不正確的命令列中。 當您新增使用 AppBarElementContainer 為項目時，有一些您應該讓項目符合其他的命令列元素採取的步驟：
>
> - 覆寫預設筆刷使用[輕量型樣式設定](/design/controls-and-patterns/xaml-styles#lightweight-styling)，讓元素的背景及符合應用程式列按鈕的框線。
> - 調整大小和位置的項目。
> - 包裝 Viewbox 寬度與高度 16px 的圖示。

> [!NOTE]
> 此範例顯示只命令列飛出視窗的 UI，不會實作的任何指令，會顯示。 如需實作命令的詳細資訊，請參閱[按鈕](buttons.md)和[命令設計基本知識](../basics/commanding-basics.md)。

![命令列飛出視窗與分割按鈕](images/command-bar-flyout-split-button.png)

> _開啟 SplitButton 包含已摺疊的命令列飛出視窗_

![命令列飛出視窗與複雜的 UI](images/command-bar-flyout-custom-ui.png)

> _包含在功能表中的自訂縮放 UI 擴充的命令列飛出視窗_


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

## <a name="create-a-context-menu-with-secondary-commands-only"></a>建立僅次要命令的操作功能表

您可以使用 CommandBarFlyout 僅次要命令，為[操作功能表](menus.md)，來取代 MenuFlyout。

![僅次要命令的命令列飛出視窗](images/command-bar-flyout-context-menu.png)

> _命令列飛出視窗為操作功能表_

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

您也可以使用 CommandBarFlyout 與 DropDownButton，若要建立標準的功能表。

![命令列飛出視窗與做為一種下拉式按鈕功能表](images/command-bar-flyout-dropdown.png)

> _一種下拉式按鈕功能表中的命令列飛出視窗_

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

## <a name="command-bar-flyouts-for-text-controls"></a>文字控制項的命令列飛出視窗

[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)是包含編輯文字的命令專用的命令列飛出視窗。 每個文字控制項可顯示 TextCommandBarFlyout 自動為操作功能表 （按一下滑鼠右鍵），或選取文字時。 文字命令列飛出視窗可隨文字選取項目，只顯示相關的命令。

![摺疊的文字命令列飛出視窗](images/command-bar-flyout-text-selection.png)

> _文字命令列飛出視窗所選取的文字_

![已展開的文字命令列飛出視窗](images/command-bar-flyout-text-full.png)

> _已展開的文字命令列飛出視窗_


### <a name="available-commands"></a>可用的命令

下表顯示已包含在 TextCommandBarFlyout，並顯示這些命令。

| 命令 | 顯示... |
| ------- | -------- |
| 粗體 | 當文字控制項不是唯讀 (RichEditBox 只)。 |
| 斜體 | 當文字控制項不是唯讀 (RichEditBox 只)。 |
| 底線 | 當文字控制項不是唯讀 (RichEditBox 只)。 |
| 校訂 | 當 IsSpellCheckEnabled **，則為 true** ，而且拼字錯誤時，會選取文字。 |
| 剪下 | 當文字控制項不是唯讀的和已選取文字。 |
| 複製 | 當已選取文字。 |
| 貼上 | 當文字控制項不是唯讀的且剪貼簿具有內容。 |
| 復原 | 動作可復原時。 |
| 全選 | 當您可以選取文字。 |

### <a name="custom-text-command-bar-flyouts"></a>自訂文字命令列飛出視窗

TextCommandBarFlyout 無法自訂，並由每個文字控制項自動管理。 不過，您可以將預設 TextCommandBarFlyout 取代自訂命令。

- 若要取代預設 TextCommandBarFlyout 顯示在選取文字，您可以建立自訂的 CommandBarFlyout （或其他飛出視窗類型），並將它指派給**SelectionFlyout**屬性。 如果您將 SelectionFlyout 設為**null**，沒有任何命令會顯示選取項目上。
- 若要取代預設會顯示為操作功能表的 TextCommandBarFlyout，指派**ContextFlyout**屬性文字控制項上的自訂 CommandBarFlyout （或其他飛出視窗類型）。 如果您將 ContextFlyout 設為**null**，而不是 TextCommandBarFlyout 顯示功能表飛出視窗舊版的文字控制項中所示。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以互動式格式查看所有 XAML 控制項。
- [XAML 命令範例](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>相關文章

- [UWP 應用程式的命令設計基本知識](../basics/commanding-basics.md)
- [CommandBar 類別](https://msdn.microsoft.com/library/windows/apps/dn279427)
