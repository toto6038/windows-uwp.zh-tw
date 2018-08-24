---
author: serenaz
Description: A button gives the user a way to trigger an immediate action.
title: 按鈕
label: Buttons
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f0bf7731a8480fb4f6d81368227ad6259780381b
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/24/2018
ms.locfileid: "2836718"
---
# <a name="buttons"></a>按鈕

> [!IMPORTANT]
> 本文說明尚未發佈但可能在正式發行前大幅度修改的功能。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。 預覽功能需要的[最新的 Windows 10 內部預覽建置及 sdk （英文）](https://insider.windows.com/for-developers/)或[Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

按鈕為使用者提供觸發立即動作的方式。 特定的工作，例如導覽、 重複的動作或呈現功能表會由專用一些] 按鈕。

![按鈕的範例](images/controls/button.png)

XAML 架構提供標準按鈕] 控制項以及數個特殊的按鈕控制項。

控制項 | 說明
------- | -----------
[Button](/uwp/api/windows.ui.xaml.controls.button) | 起始立即巨集指令。 可以搭配 Click 事件或命令繫結。
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | 會引發 Click 事件持續時按下按鈕。
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | 按鈕，具有類似的超連結，用於導覽的樣式。 如需詳細資訊，請參閱[超連結](hyperlinks.md)。
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | （預覽）若要開啟 [附加的彈出式下的 v 字形具有的按鈕。
[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) | （預覽）具有兩個側邊的按鈕。 其中一邊起始動作]，並另一邊開啟功能表。
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | （預覽）具有兩個側邊的切換按鈕。 其中一邊切換開/關，與另一邊開啟功能表。

| **取得 Windows UI 文件庫** |
| - |
| DropDownButton、 SplitButton，而且 ToggleSplitButton 會包含一部分的 Windows 使用者介面文件庫包含新控制項和 UWP 應用程式的 UI 功能 NuGet 套件。 如需詳細資訊，包括安裝指示，請參閱[Windows UI 文件庫概觀 （英文）](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

| **平台 api （英文)** | **Windows 使用者介面程式庫 api （英文)** |
| - | - |
| [Click 事件](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)， [Command 屬性](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) | [DropDownButton 類別](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton) [SplitButton 類別](/uwp/api/microsoft.ui.xaml.controls.splitbutton) [ToggleSplitButton 類別](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton) |

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

若要讓使用者啟動即時的動作，例如送出表單使用 **] 按鈕**。

時動作是要瀏覽至另一個頁面，請勿使用] 按鈕請改用[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) 。 如需詳細資訊，請參閱[超連結](hyperlinks.md)。
> 例外：對於精靈瀏覽，請使用標籤為 [上一頁] 和 [下一頁] 的按鈕。 其他類型的回溯導覽或瀏覽至高層級，使用[上一步] 按鈕](../basics/navigation-history-and-backwards-navigation.md)。

使用者可能會想要重複觸發動作時，使用**RepeatButton** 。 例如，使用 RepeatButton 來遞增或遞減效能計數器的值。

使用**DropDownButton**時按鈕都不具有彈出式包含更多選項]。 預設符號提供按鈕包含彈出式視覺指示。

使用**SplitButton**時要讓使用者能夠啟動即時巨集指令或單獨選擇其他選項。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/Button">開啟應用程式並查看 Button 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

這個範例在要求位置存取權的對話方塊中使用兩個按鈕 ([允許] 和 [封鎖])。

![按鈕的範例 (用於對話方塊中)](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>建立按鈕

這個範例會顯示一個按鈕，它會回應 Click 動作。

在 XAML 中建立按鈕。

```xaml
<Button Content="Subscribe" Click="SubscribeButton_Click"/>
```

或者，在程式碼中建立按鈕。

```csharp
Button subscribeButton = new Button();
subscribeButton.Content = "Subscribe";
subscribeButton.Click += SubscribeButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(subscribeButton);
```

處理 Click 事件。

```csharp
private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to subscribe to the service. For example:
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="button-interaction"></a>按鈕互動

當您以手指或手寫筆點選按鈕，或在游標位於按鈕上方時按下滑鼠左鍵，按鈕會引發 [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件。 如果按鈕有鍵盤焦點，則按下 Enter 鍵或空格鍵也會引發 Click 事件。

您通常無法處理按鈕上的低階 [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) 事件，因為按鈕本身有 Click 行為。 如需詳細資訊，請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584.aspx)。

您可以變更 [ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode) 屬性，以變更按鈕引發 Click 事件的方式。 預設 ClickMode 值是 **Release**，但您也可以將按鈕的 ClickMode 設定為 **Hover** 或 **Press**。 如果 ClickMode 是 **Hover**，則使用鍵盤或觸控無法引發 Click 事件。


### <a name="button-content"></a>按鈕內容

按鈕是 [ContentControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx)。 它的 XAML 內容屬性是 [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx)，它可讓您使用如下 XAML 語法︰`<Button>A button's content</Button>`。 您可以將任何物件設定為按鈕的內容。 如果內容是 [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx)，就會呈現於按鈕中。 如果內容是其他類型的物件，則會在按鈕中顯示它的字串表示。

按鈕的內容通常是文字。 以下是對含有文字內容的按鈕的設計建議：
-   使用簡潔、專屬、一目了然的文字，清楚說明按鈕執行的動作。 通常按鈕文字內容是一個動詞單字。
-   除非您的品牌指導方針指示您使用其他字型，否則使用預設字型。
-   對於簡短文字，避免使用 120px 的最小按鈕寬度將命令按鈕變窄。
- 對於較長文字，避免將文字限制在 26 個字元的最大長度，以使命令按鈕變寬。
-   如果按鈕的文字內容為動態 (例如經過[當地語系化](../globalizing/globalizing-portal.md))，請考慮如何調整按鈕的大小以及調整後周圍的控制項會發生什麼變化。

<table>
<tr>
<td> <b>需要修正：</b><br> 含溢位文字的按鈕。 </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>選項 1：</b><br> 增加按鈕寬度、堆疊按鈕，如果文字長度大於 26 個字元，則換行。 </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>選項 2：</b><br> 增加按鈕高度，並將文字換行。 </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

您也可以自訂形成按鈕外觀的視覺效果。 例如，您可能以圖示取代文字，或使用圖示加上文字。

如下所示，包含影像和文字的 **StackPanel** 已設定為按鈕的內容。

```xaml
<Button Click="Button_Click"
        Background="LightGray"
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Photo.png" Height="62"/>
        <TextBlock Text="Photos" Foreground="Black"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

按鈕看起來就像這樣。

![含影像和文字內容的按鈕](images/button-orange.png)

## <a name="create-a-repeat-button"></a>建立一個重複按鈕

[RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) 是一個按鈕，可以從按鈕被按下的當時到鬆開後為止，重複引發 [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件。 設定 [Delay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) 屬性以指定 RepeatButton 在它被按下之後以及在它開始重複按一下動作之前，必須等待的時間。 設定 [Interval](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) 屬性以指定重複按下動作之間的時間。 這兩個屬性的時間是以毫秒為單位來指定。

下列範例顯示兩個 RepeatButton 控制項，而且其各自的 Click 事件是用來增加或減少文字區塊中顯示的值。

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## <a name="create-a-drop-down-button"></a>建立下拉式方塊] 按鈕

> **預覽**： DropDownButton 需要[最新的 Windows 10 內部預覽建置及 sdk （英文）](https://insider.windows.com/for-developers/)或[Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton)是顯示下的 v 字形 visual 標記為已附加的彈出式包含更多選項] 按鈕。 已定義為標準按鈕與彈出式 ； 同樣的行為僅限外觀的不同。

下拉式清單] 按鈕會繼承 Click 事件，但通常不使用它。 但是，您可以使用彈出式屬性附加彈出式及叫用使用的彈出式功能表] 選項的動作。 會在按一下按鈕時彈出式會自動開啟。

> [!TIP]
> 延伸顯示的詳細資訊，請參閱[功能表和快顯功能表](menus.md)。

### <a name="example---drop-down-button"></a>範例-下拉式清單] 按鈕

本範例會示範如何建立包含命令以 RichEditBox 中的段落對齊方式彈出式下拉式方塊] 按鈕。 （如需詳細資訊和程式碼，請參閱 ＜ [Rich 編輯方塊](rich-edit-box.md)）。

![下拉式方塊和對齊方式命令按鈕](images/drop-down-button-align.png)

```xaml
<DropDownButton ToolTipService.ToolTip="Alignment">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="16" Text="&#xE8E4;"/>
    <DropDownButton.Flyout>
        <MenuFlyout Placement="BottomEdgeAlignedLeft">
            <MenuFlyoutItem Text="Left" Icon="AlignLeft" Tag="left"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Center" Icon="AlignCenter" Tag="center"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Right" Icon="AlignRight" Tag="right"
                            Click="AlignmentMenuFlyoutItem_Click"/>
        </MenuFlyout>
    </DropDownButton.Flyout>
</DropDownButton>
```

```csharp
private void AlignmentMenuFlyoutItem_Click(object sender, RoutedEventArgs e)
{
    var option = ((MenuFlyoutItem)sender).Tag.ToString();

    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the alignment to the selected paragraphs.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (option == "left")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Left;
        }
        else if (option == "center")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Center;
        }
        else if (option == "right")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Right;
        }
    }
}
```

## <a name="create-a-split-button"></a>建立分割按鈕

> **預覽**： SplitButton 需要[最新的 Windows 10 內部預覽建置及 sdk （英文）](https://insider.windows.com/for-developers/)或[Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton)包含可以個別叫用的兩個部分。 一組件的行為就像標準按鈕及會叫用 [即時運算巨集指令。 組件會叫用彈出式包含使用者可以從選擇的其他選項。

> [!NOTE]
> 分割按鈕時叫用與觸控、 行為為下拉式清單] 按鈕 ；這兩個按鈕的一半叫用彈出式。 使用其他方法的輸入，使用者可以叫用按鈕的任一半分開。

分割按鈕的一般行為是：

- 當使用者按一下 [按鈕] 組件時、 處理 Click 事件來叫用下拉式清單中目前所選取的選項。
- 開啟下拉式清單時，處理引動過程中放置到這兩個變更此選項的項目已選取，然後再叫用。 請務必因為叫用彈出式項目] 按鈕的 Click 事件不會發生使用觸控時。

> [!TIP]
> 有許多種下拉式清單中的項目向下並處理其引動過程。 如果您使用的清單檢視或 GridView，其中一個方法是處理 SelectionChanged 事件。 如果這麼做，將[SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus)設**為 false**。 這可讓使用者瀏覽使用鍵盤呼叫在每項變更的項目沒有選項。

### <a name="example---split-button"></a>範例-分割按鈕

本範例會示範如何建立用來變更 RichEditBox 中選取文字的前景色彩的分割按鈕。 （如需詳細資訊和程式碼，請參閱 ＜ [Rich 編輯方塊](rich-edit-box.md)）。

![分割按鈕選取前景色彩](images/split-button-rtb.png)

```xaml
<SplitButton ToolTipService.ToolTip="Foreground color"
             Click="BrushButtonClick">
    <Border x:Name="SelectedColorBorder" Width="20" Height="20"/>
    <SplitButton.Flyout>
        <Flyout x:Name="BrushFlyout" Placement="BottomEdgeAlignedLeft">
            <!-- Set SingleSelectionFollowsFocus="False"
                 so that keyboard navigation works correctly. -->
            <GridView ItemsSource="{x:Bind ColorOptions}" 
                      SelectionChanged="BrushSelectionChanged"
                      SingleSelectionFollowsFocus="False"
                      SelectedIndex="0" Padding="0">
                <GridView.ItemTemplate>
                    <DataTemplate>
                        <Rectangle Fill="{Binding}" Width="20" Height="20"/>
                    </DataTemplate>
                </GridView.ItemTemplate>
                <GridView.ItemContainerStyle>
                    <Style TargetType="GridViewItem">
                        <Setter Property="Margin" Value="2"/>
                        <Setter Property="MinWidth" Value="0"/>
                        <Setter Property="MinHeight" Value="0"/>
                    </Style>
                </GridView.ItemContainerStyle>
            </GridView>
        </Flyout>
    </SplitButton.Flyout>
</SplitButton>
```

```csharp
public sealed partial class MainPage : Page
{
    // Color options that are bound to the grid in the split button flyout.
    private List<SolidColorBrush> ColorOptions = new List<SolidColorBrush>();
    private SolidColorBrush CurrentColorBrush = null;

    public MainPage()
    {
        this.InitializeComponent();

        // Add color brushes to the collection.
        ColorOptions.Add(new SolidColorBrush(Colors.Black));
        ColorOptions.Add(new SolidColorBrush(Colors.Red));
        ColorOptions.Add(new SolidColorBrush(Colors.Orange));
        ColorOptions.Add(new SolidColorBrush(Colors.Yellow));
        ColorOptions.Add(new SolidColorBrush(Colors.Green));
        ColorOptions.Add(new SolidColorBrush(Colors.Blue));
        ColorOptions.Add(new SolidColorBrush(Colors.Indigo));
        ColorOptions.Add(new SolidColorBrush(Colors.Violet));
        ColorOptions.Add(new SolidColorBrush(Colors.White));
    }

    private void BrushButtonClick(object sender, object e)
    {
        // When the button part of the split button is clicked,
        // apply the selected color.
        ChangeColor();
    }

    private void BrushSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        // When the flyout part of the split button is opened and the user selects
        // an option, set their choice as the current color, apply it, then close the flyout.
        CurrentColorBrush = (SolidColorBrush)e.AddedItems[0];
        SelectedColorBorder.Background = CurrentColorBrush;
        ChangeColor();
        BrushFlyout.Hide();
    }

    private void ChangeColor()
    {
        // Apply the color to the selected text in a RichEditBox.
        Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
        if (selectedText != null)
        {
            Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
            charFormatting.ForegroundColor = CurrentColorBrush.Color;
            selectedText.CharacterFormat = charFormatting;
        }
    }
}
```

## <a name="create-a-toggle-split-button"></a>建立切換分割按鈕

> **預覽**： ToggleSplitButton 需要[最新的 Windows 10 內部預覽建置及 sdk （英文）](https://insider.windows.com/for-developers/)或[Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton)包含可以個別叫用的兩個部分。 一組件的行為與切換按鈕可開啟或關閉類似。 組件會叫用彈出式包含使用者可以從選擇的其他選項。

切換分割按鈕通常用於啟用或停用功能時功能有多個使用者可以從選擇的選項。 例如，在文件編輯器中，它無法用以開啟清單開啟或關閉，而下拉式清單用來選擇清單的樣式。

> [!NOTE]
> 使用觸控叫用時分割按鈕的行為為下拉式清單] 按鈕。 使用其他方法的輸入，使用者可以叫用按鈕的任一半分開。 觸控與按鈕的兩個一半叫用彈出式。 因此，您必須包含一個選項來開啟或關閉切換按鈕彈出式內容中。

### <a name="differences-with-togglebutton"></a>ToggleButton 的差異

不同於[ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton)，ToggleSplitButton 沒有未定狀態。 因此，您應請牢記下列差異：

- ToggleSplitButton 沒有**IsThreeState**屬性或**未定**事件。
- [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked)屬性是剛**bool**，不是**null bool**。
- ToggleSplitButton 具有僅[IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged)事件 ；它沒有個別的**檢查**及**未核取**事件。

### <a name="example---toggle-split-button"></a>範例-切換分割按鈕

下列範例顯示如何分割按鈕切換無法用來開啟或關閉、 格式設定的清單和變更清單、 RichEditBox 中的樣式。 （如需詳細資訊和程式碼，請參閱 ＜ [Rich 編輯方塊](rich-edit-box.md)）。

![切換分割按鈕選取清單樣式](images/toggle-split-button-open.png)

```xaml
<ToggleSplitButton x:Name="ListButton"
                   ToolTipService.ToolTip="List style"
                   Click="ListButton_Click"
                   IsCheckedChanged="ListStyleButton_IsCheckedChanged">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="16" Text="&#xE8FD;"/>
    <ToggleSplitButton.Flyout>
        <Flyout Placement="BottomEdgeAlignedLeft">
            <ListView x:Name="ListStylesListView"
                      SelectionChanged="ListStylesListView_SelectionChanged" 
                      SingleSelectionFollowsFocus="False">
                <StackPanel Tag="bullet" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7C8;"/>
                    <TextBlock Text="Bullet" Margin="8,0"/>
                </StackPanel>
                <StackPanel Tag="alpha" Orientation="Horizontal">
                    <TextBlock Text="A" FontSize="24" Margin="2,0"/>
                    <TextBlock Text="Alpha" Margin="8"/>
                </StackPanel>
                <StackPanel Tag="numeric" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xF146;"/>
                    <TextBlock Text="Numeric" Margin="8,0"/>
                </StackPanel>
                <TextBlock Tag="none" Text="None" Margin="28,0"/>
            </ListView>
        </Flyout>
    </ToggleSplitButton.Flyout>
</ToggleSplitButton>
```

```csharp
private void ListStyleButton_IsCheckedChanged(ToggleSplitButton sender, ToggleSplitButtonIsCheckedChangedEventArgs args)
{
    // Use the toggle button to turn the selected list style on or off.
    if (((ToggleSplitButton)sender).IsChecked == true)
    {
        // On. Apply the list style selected in the drop down to the selected text.
        var listStyle = ((FrameworkElement)(ListStylesListView.SelectedItem)).Tag.ToString();
        ApplyListStyle(listStyle);
    }
    else
    {
        // Off. Make the selected text not a list,
        // but don't change the list style selected in the drop down.
        ApplyListStyle("none");
    }
}

private void ListStylesListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var listStyle = ((FrameworkElement)(e.AddedItems[0])).Tag.ToString();

    if (ListButton.IsChecked == true)
    {
        // Toggle button is on. Turn it off...
        if (listStyle == "none")
        {
            ListButton.IsChecked = false;
        }
        else
        {
            // or apply the new selection.
            ApplyListStyle(listStyle);
        }
    }
    else
    {
        // Toggle button is off. Turn it on, which will apply the selection
        // in the IsCheckedChanged event handler.
        ListButton.IsChecked = true;
    }
}

private void ApplyListStyle(string listStyle)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the list style to the selected text.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (listStyle == "none")
        {  
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.None;
        }
        else if (listStyle == "bullet")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Bullet;
        }
        else if (listStyle == "numeric")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Arabic;
        }
        else if (listStyle == "alpha")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.UppercaseEnglishLetter;
        }
        selectedText.ParagraphFormat = paragraphFormatting;
    }
}
```

## <a name="recommendations"></a>建議

- 確定使用者能清楚了解按鈕的用途和狀態。
- 同樣的決定 (例如確認對話方塊) 有多個按鈕時，依下列順序顯示認可按鈕 (其中，[執行] 和 [不執行] 是主要指令的特定回應)：
    - 確定/[執行]/是
    - [不執行]/否
    - 取消
- 一次只對使用者顯示一或兩個按鈕，例如，[接受] 和 [取消]。 如果需要對使用者顯示更多動作，請考慮使用[核取方塊](checkbox.md)或[選項按鈕](radio-button.md)，使用者可以利用它們選取動作，只要一個命令按鈕即可觸發這些動作。
- 對於需要在 app 內多個頁面上提供的動作，請不要在多個頁面上複製按鈕，而是考慮改用[底部應用程式列](app-bars.md)。

### <a name="recommended-single-button-layout"></a>建議使用的單一按鈕配置

如果您的配置只需要一個按鈕，此按鈕應根據其容器內容靠左或靠右對齊。

- 只有一個按鈕的對話方塊必須讓按鈕**靠右對齊**。 如果對話方塊只包含一個按鈕，請確保按鈕執行的是安全、不具破壞性的動作。 如果您使用 [ContentDialog](dialogs.md) 並指定單一按鈕，此按鈕將會自動靠右對齊。

![對話方塊內的按鈕](images/pushbutton_doc_dialog.png)

- 如果您的按鈕會在容器 UI 中出現 (例如，在快顯通知、飛出視窗或的清單檢視項目中)，就必須讓按鈕在容器內**靠右對齊**。

![容器內的按鈕](images/pushbutton_doc_container.png)

- 在包含單一按鈕 (例如，設定頁面底部的 [套用] 按鈕) 的頁面中，您應該讓按鈕**靠左對齊**。 這樣可確保按鈕對齊其餘的網頁內容。

![頁面上的按鈕](images/pushbutton_doc_page.png)

## <a name="back-buttons"></a>返回按鈕

返回按鈕是一種系統提供的 UI 元素，可透過使用者的返回堆疊或瀏覽歷程記錄啟用向後瀏覽。 您不需要自行建立返回按鈕，但您可能必須花一點心力來提供良好的向後瀏覽體驗。 如需詳細資訊，請參閱[歷程記錄和向後瀏覽](../basics/navigation-history-and-backwards-navigation.md)。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [Button 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
- [選項按鈕](radio-button.md)
- [核取方塊](checkbox.md)
- [切換開關](toggles.md)
