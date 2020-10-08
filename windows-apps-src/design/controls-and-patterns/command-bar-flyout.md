---
Description: 命令列飛出視窗可讓使用者以內嵌方式存取您應用程式最常見的工作。
title: 命令列飛出視窗
label: Command bar flyout
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a2f6e61373ae343d8d683d6e5f9169cc399f1594
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750544"
---
# <a name="command-bar-flyout"></a>命令列飛出視窗

命令列飛出視窗藉由在與 UI 畫布上元素相關的浮動工具列中顯示命令，讓使用者能輕鬆存取一般工作。

![展開的文字命令列飛出視窗](images/command-bar-flyout-text-full.png)

如同 [CommandBar](app-bars.md)，CommandBarFlyout 有可用來新增命令的 **PrimaryCommands** 和 **SecondaryCommands** 屬性。 您可以將命令放在集合中，或兩者之中。 顯示主要和次要命令的時機和方式取決於顯示模式。

命令列飛出視窗有兩個顯示模式：摺疊  和展開  。

- 在摺疊模式中，只會顯示主要命令。 如果您的命令列飛出視窗同時有主要與次要命令，則會顯示「查看更多」按鈕 (以省略符號 \[***\] 表示)。 這可讓使用者藉由轉換為展開模式來取得次要命令的存取權。
- 在展開模式中，主要和次要命令會同時顯示 (如果控制項只有次要項目，則會以類似 MenuFlyout 控制項的方式顯示)。

**取得 Windows UI 程式庫**

:::row:::
   :::column:::
      ![WinUI 標誌](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      此 **CommandBarFlyout** 控制項包含在 Windows UI 程式庫中；該程式庫是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](/uwp/toolkits/winui/)。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

>**Windows UI 程式庫 API**：[CommandBarFlyout 類別](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)、[TextCommandBarFlyout 類別](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)
>
>**平台 API**：[CommandBarFlyout 類別](/uwp/api/windows.ui.xaml.controls.commandbarflyout)、[TextCommandBarFlyout 類別](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout)、[AppBarButton 類別](/uwp/api/windows.ui.xaml.controls.appbarbutton)、[AppBarToggleButton 類別](/uwp/api/windows.ui.xaml.controls.appbartogglebutton)、[AppBarSeparator 類別](/uwp/api/windows.ui.xaml.controls.appbarseparator)
>
> CommandBarFlyout 需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本，或是 [Windows UI 程式庫](/uwp/toolkits/winui/)。

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用 CommandBarFlyout 控制項，在應用程式畫布上的元素內容中對使用者顯示命令集合，例如按鈕和功能表項目。

TextCommandBarFlyout 會在 TextBox、TextBlock、RichEditBox、RichTextBlock 和 PasswordBox 控制項中顯示文字命令。 這些命令會適當地自動設定為目前的文字選取範圍。 使用 CommandBarFlyout 來取代文字控制項上的預設文字命令。

若要在清單項目上顯示關聯式命令，請遵循[集合和清單的關聯式命令](collection-commanding.md)中的指導方針。

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout 與 MenuFlyout

若要在操作功能表中顯示命令，您可以使用 CommandBarFlyout 或 MenuFlyout。 我們建議 CommandBarFlyout，因為它提供的功能比 MenuFlyout 還要多。 您可以使用只有次要命令的 CommandBarFlyout 來取得 MenuFlyout 的行為和外觀，或使用同時具有主要和次要命令的完整命令列飛出視窗。

> 如需相關資訊，請參閱[飛出視窗](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)、[功能表和操作功能表](menus.md)及[命令列](app-bars.md)。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/CommandBarFlyout">開啟應用程式並查看 CommandBarFlyout 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>主動式與反應式引動過程

通常有兩種方式可叫用與 UI 畫布上元素相關聯的飛出視窗或功能表：「主動式引動過程」  和「回應式引動過程」  。

在主動式引動過程中，當使用者與命令相關聯的項目互動時，命令就會自動出現。 例如，文字格式化命令可能會在使用者選取文字方塊中的文字時跳出。 在此情況下，命令列飛出視窗不會取得焦點。 相反地，它會呈現接近使用者正在互動項目的相關命令。 如果使用者並未與命令互動，則命令會關閉。

在回應式引動過程中，顯示的命令可回應明確的使用者動作來要求命令；例如，按一下滑鼠右鍵。 這對應於[操作功能表](menus.md)的傳統概念。

您可以任一種方法使用 CommandBarFlyout，或甚至混合使用兩者。

## <a name="create-a-command-bar-flyout"></a>建立命令列飛出視窗

此範例示範如何建立命令列飛出視窗，並以主動和回應方式使用它。 點選影像後，飛出視窗會以摺疊模式顯示。 若顯示為操作功能表，則飛出視窗會以展開模式顯示。 在任一情況下，使用者可以展開或摺疊已開啟的飛出視窗。

![摺疊的命令列飛出視窗範例](images/command-bar-flyout-img-collapsed.png)

> _摺疊的命令列飛出視窗_

![展開的命令列飛出視窗範例](images/command-bar-flyout-img-expanded.png)

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

當您主動顯示關聯式命令時，預設應該只會顯示主要命令 (命令列飛出視窗應該摺疊起來)。 將最重要的命令放在主要命令集合中，以及將傳統上納入操作功能表中的其他命令放在次要命令集合中。

若要主動顯示命令，通常會處理 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 或 [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) 事件以顯示命令列飛出視窗。 將飛出視窗的 [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) 設定為 **Transient** or **TransientWithDismissOnPointerMoveAway**，不需取得焦點，即可在其摺疊模式中開啟飛出視窗。

從 Windows 10 Insider Preview 開始，文字控制項都有 **SelectionFlyout** 屬性。 當您將飛出視窗指派給這個屬性時，它會在選取文字時自動顯示。

### <a name="show-commands-reactively"></a>以回應方式顯示命令

當您以回應方式將關聯式命令顯示為操作功能表時，預設會顯示次要命令 (命令列飛出視窗應會展開)。 在此情況下，命令列飛出視窗可能同時有主要和次要命令，或只有次要命令。

若要在操作功能表中顯示命令，通常會將飛出視窗指派給 UI 元素的 [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) 屬性。 如此一來，開啟飛出視窗是由元素處理，您不需再執行任何動作。

如果您自行處理飛出視窗顯示 (例如，在 [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) 事件上)，請將飛出視窗的 [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) 設定為 [標準]  ，以展開模式開啟飛出視窗並給予焦點。

> [!TIP]
> 如需有關顯示飛出視窗時的選項以及如何控制飛出視窗放置方式的詳細資訊，請參閱[飛出視窗](../controls-and-patterns/dialogs-and-flyouts/flyouts.md)。

## <a name="commands-and-content"></a>命令與內容

CommandBarFlyout 控制項有 2 個可用來新增命令與內容的屬性：[PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) 和 [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands)。

預設會將命令列項目新增至 **PrimaryCommands** 集合。 這些命令會顯示在命令列中，而且會同時出現在摺疊或展開模式中。 不同於 CommandBar，主要命令不會自動溢位到次要命令，且可能會被截斷。

您也可以將命令新增至 **SecondaryCommands** 集合。 次要命令會顯示在控制項的功能表部分，且只會在展開模式中出現。

### <a name="app-bar-buttons"></a>應用程式列按鈕

您可直接以 [AppBarButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)、[AppBarToggleButton](/uwp/api/Windows.UI.Xaml.Controls.AppBarToggleButton) 和 [AppBarSeparator](/uwp/api/Windows.UI.Xaml.Controls.AppBarSeparator) 控制項填入 PrimaryCommands 和 SecondaryCommands。

應用程式列按鈕控制項可依圖示和文字標籤加以區分。 這些控制項最適合在命令列中使用，其外觀會改變，取決於控制項是顯示在命令列還是溢位功能表中。

- 作為主要命令的應用程式列按鈕會以其圖示顯示在命令列中；不會顯示文字標籤。 我們建議您使用工具提示來顯示命令的文字描述，如下所示。
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- 作為次要命令的應用程式列按鈕會顯示在功能表中 (標籤和圖示兩者都可見)。

### <a name="other-content"></a>其他內容

您可以在 AppBarElementContainer 中包裝其他控制項，即可將其新增至命令列飛出視窗。 這可讓您新增 [DropDownButton](buttons.md) 或 [SplitButton](buttons.md) 之類的控制項，或新增 [StackPanel](buttons.md) 之類的容器，以建立更複雜的 UI。

元素必須實作 [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) 介面，才能夠新增至命令列飛出視窗的主要或次要命令集合。 AppBarElementContainer 是實作這個介面的包裝函式，因此您可以將元素新增至命令列，即使它不會實作介面本身。

在此，AppBarElementContainer 用來將額外的元素新增至命令列飛出視窗。 SplitButton 會新增至主要命令，以允許選取色彩。 StackPanel 會新增至次要命令，以允許縮放控制項的配置更複雜。

> [!TIP]
> 根據預設，專為應用程式畫布設計的元素在命令列中可能看起來不太合適。 當您使用 AppBarElementContainer 新增元素時，您應該採取一些步驟，讓該元素符合其他命令列元素：
>
> - 以[輕量級樣式](./xaml-styles.md#lightweight-styling)覆寫預設筆刷，讓此元素的背景和框線符合應用程式列按鈕。
> - 調整元素的大小和位置。
> - 在寬度和高度均為 16px 的 Viewbox 中包裝圖示。

> [!NOTE]
> 這個範例只示範命令列飛出視窗 UI，並不會實作任何顯示的命令。 如需實作命令的詳細資訊，請參閱 [Buttons](buttons.md) 和[命令設計基本知識](../basics/commanding-basics.md)。

![具有分割按鈕的命令列飛出視窗](images/command-bar-flyout-split-button.png)

> _具有開啟 SplitButton 的摺疊命令列飛出視窗_

![具有複雜 UI 的命令列飛出視窗](images/command-bar-flyout-custom-ui.png)

> _功能表中有自訂縮放 UI 的展開命令列飛出視窗_


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

## <a name="create-a-context-menu-with-secondary-commands-only"></a>建立只有次要命令的操作功能表

您可以使用只有次要命令的 CommandBarFlyout 作為[操作功能表](menus.md)，以取代 MenuFlyout。

![只有次要命令的命令列飛出視窗](images/command-bar-flyout-context-menu.png)

> _命令列飛出視窗即操作功能表_

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

您也可以使用具有 DropDownButton 的 CommandBarFlyout 來建立標準功能表。

![具有下拉式按鈕功能表的命令列飛出視窗](images/command-bar-flyout-dropdown.png)

> _命令列飛出視窗中的下拉式按鈕功能表_

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

[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) 是特製化的命令列飛出視窗，其中包含用來編輯文字的命令。 每個文字控制項都會將 TextCommandBarFlyout 顯示為操作功能表 (按一下滑鼠右鍵)，或在選取文字後顯示。 文字命令列飛出視窗會將文字選取範圍調整為只顯示相關的命令。

![摺疊的文字命令列飛出視窗](images/command-bar-flyout-text-selection.png)

> _文字選取範圍上的文字命令列飛出視窗_

![展開的文字命令列飛出視窗](images/command-bar-flyout-text-full.png)

> _展開的文字命令列飛出視窗_


### <a name="available-commands"></a>可用的命令

下表顯示 TextCommandBarFlyout 中包含的命令，以及其顯示時間。

| 命令 | 顯示... |
| ------- | -------- |
| 粗體 | 當文字控制項不是唯讀狀態 (僅限 RichEditBox)。 |
| 斜體 | 當文字控制項不是唯讀狀態 (僅限 RichEditBox)。 |
| Underline | 當文字控制項不是唯讀狀態 (僅限 RichEditBox)。 |
| 校訂 | 當 IsSpellCheckEnabled 為 **true** 並已選取拼字錯誤的文字時。 |
| 剪下 | 當文字控制項不是唯讀狀態並已選取文字時。 |
| 複製 | 已選取文字時。 |
| 貼上 | 當文字控制項不是唯讀狀態且剪貼簿有內容時。 |
| 復原 | 具有可復原的動作時。 |
| 全選 | 可以選取文字時。 |

### <a name="custom-text-command-bar-flyouts"></a>自訂文字命令列飛出視窗

TextCommandBarFlyout 無法加以自訂，並由每個文字控制項自動管理。 不過，您可以自訂命令取代預設 TextCommandBarFlyout。

- 若要取代文字選取範圍上顯示的預設 TextCommandBarFlyout，您可以建立自訂 CommandBarFlyout (或其他飛出視窗類型)，並將它指派給 **SelectionFlyout** 屬性。 如果您將 SelectionFlyout 設定為 **null**，則選取範圍上沒有顯示任何命令。
- 若要取代顯示為操作功能表的預設 TextCommandBarFlyout，請將自訂 CommandBarFlyout (或其他飛出視窗類型) 指派給文字控制項上的 **ContextFlyout** 屬性。 如果您將 ContextFlyout 設定為 **null**，則會顯示舊版文字控制項中顯示的功能表飛出視窗，而不會顯示 TextCommandBarFlyout。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。
- [XAML 命令範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCommanding)

## <a name="related-articles"></a>相關文章

- [Windows 應用程式的命令設計基本知識](../basics/commanding-basics.md)
- [CommandBar 類別](/uwp/api/Windows.UI.Xaml.Controls.CommandBar)
