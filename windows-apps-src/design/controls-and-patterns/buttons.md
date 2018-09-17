---
author: QuinnRadich
Description: A button gives the user a way to trigger an immediate action.
title: 按鈕
label: Buttons
template: detail.hbs
ms.author: quradic
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
ms.openlocfilehash: 2b52f61a4bb54c3432c3e1544bb690df08c3b891
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2018
ms.locfileid: "3982145"
---
# <a name="buttons"></a>按鈕

> [!IMPORTANT]
> 本文說明尚未發佈但可能在正式發行前大幅度修改的功能。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。 預覽功能，需要的[最新的 Windows 10 Insider Preview 組建和 SDK](https://insider.windows.com/for-developers/) ] 或 [ [Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

按鈕為使用者提供觸發立即動作的方式。 一些按鈕被專門針對特定的工作，例如瀏覽、 重複的動作，或呈現功能表。

![按鈕的範例](images/controls/button.png)

XAML 架構提供標準 button 控制項，以及數個特殊的 button 控制項。

控制項 | 說明
------- | -----------
[Button](/uwp/api/windows.ui.xaml.controls.button) | 起始立即的動作。 可以搭配 Click 事件或命令繫結。
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | 引發 Click 事件持續時按下按鈕。
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | A 按鈕，具有像超連結，用來瀏覽樣式設定。 如需詳細資訊，請參閱[超連結](hyperlinks.md)。
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | （預覽）若要開啟附加飛出視窗 > 形箭號按鈕。
[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) | （預覽）具有兩個邊的按鈕。 另一側會起始動作，並在另一端，會開啟功能表。
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | （預覽）具有兩個邊切換按鈕。 另一側切換開啟/關閉，並在另一端，會開啟功能表。

| **取得 Windows UI 文件庫** |
| - |
| DropDownButton、 SplitButton，以及 ToggleSplitButton 是包含在 Windows UI 程式庫，包含新的控制項和 UI 功能適用於 UWP app 的 NuGet 套件。 如需詳細資訊，包括安裝指示，請參閱[Windows UI 文件庫的概觀](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

| **平台 Api** | **Windows 使用者介面程式庫 Api** |
| - | - |
| [Click 事件](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)，[命令屬性](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) | [DropDownButton 類別](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)， [SplitButton 類別](/uwp/api/microsoft.ui.xaml.controls.splitbutton)， [ToggleSplitButton 類別](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton) |

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

您可以使用**按鈕**，讓使用者起始直接動作，例如提交表單。

不要使用按鈕動作是瀏覽到另一個頁面。請改為使用[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) 。 如需詳細資訊，請參閱[超連結](hyperlinks.md)。
> 例外：對於精靈瀏覽，請使用標籤為 [上一頁] 和 [下一頁] 的按鈕。 對於其他類型的向後瀏覽或瀏覽到上層，使用的[返回按鈕](../basics/navigation-history-and-backwards-navigation.md)。

當使用者可能會想要重複觸發動作時，請使用**RepeatButton** 。 例如，使用 RepeatButton 遞增或遞減計數器中的值。

按鈕的飛出視窗，其中包含更多選項時，請使用**DropDownButton** 。 預設 > 形箭號提供按鈕都包含飛出視窗的視覺指示。

當您想要能夠立即起始動作，或從其他選項選擇的個別使用者，請使用**SplitButton** 。

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

## <a name="create-a-drop-down-button"></a>建立一種下拉式按鈕

> **預覽**： DropDownButton 需要的[最新的 Windows 10 Insider Preview 組建和 SDK](https://insider.windows.com/for-developers/) ] 或 [ [Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton)是顯示視覺指示器它有更多選項包含附加飛出視窗 > 形箭號按鈕。 它有相同的行為與飛出視窗; 在標準按鈕只是不同的外觀。

下拉式按鈕繼承 Click 事件，但是您通常不會使用它。 相反地，您可以使用飛出視窗屬性附加飛出視窗，叫用動作使用飛出視窗中的功能表選項。 當按一下按鈕時，會自動開啟飛出視窗。

> [!TIP]
> 如需飛出視窗的詳細資訊，請參閱[功能表和操作功能表](menus.md)。

### <a name="example---drop-down-button"></a>範例-下拉式按鈕

這個範例示範如何使用飛出視窗，其中包含段落對齊方式，在 RichEditBox 中的命令建立一種下拉式按鈕。 （如需詳細資訊和程式碼，請參閱[Rich edit 方塊](rich-edit-box.md)）。

![一種下拉式對齊命令按鈕](images/drop-down-button-align.png)

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

> **預覽**： SplitButton 需要的[最新的 Windows 10 Insider Preview 組建和 SDK](https://insider.windows.com/for-developers/) ] 或 [ [Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton)有可以個別叫用的兩個部分。 一個部分行為類似標準按鈕，並立即的動作會叫用。 其他部分會叫用的飛出視窗，其中包含其他使用者可從中選擇的選項。

> [!NOTE]
> 分割按鈕時叫用觸控式關係，做為一種下拉式按鈕; 的行為這兩個部分的按鈕叫用飛出視窗。 使用其他方法的輸入，使用者可以個別叫用按鈕的任一種半。

分割按鈕的典型行為是：

- 當使用者按下按鈕組件時，處理 Click 事件來叫用下拉式清單中目前選取的選項。
- 開啟下拉式清單時，控制代碼引動過程中的項目變更這兩個下拉選項，並叫用它。 請務必將叫用的飛出視窗項目，因為按鈕按一下使用觸控時，不會發生事件。

> [!TIP]
> 有許多方式可以向下放在下拉式功能表中的項目，並處理其引動過程。 如果您使用 ListView 或 GridView，一種方式是處理 SelectionChanged 事件。 如果這樣做，請將[SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus)設**為 false**。 這可讓使用者瀏覽使用鍵盤，不需要在每個變更項目的叫用的選項。

### <a name="example---split-button"></a>範例-分割按鈕

這個範例示範如何建立用來變更所選取的文字在 RichEditBox 中的前景色彩的分割按鈕。 （如需詳細資訊和程式碼，請參閱[Rich edit 方塊](rich-edit-box.md)）。

![分割按鈕來選取前景色彩](images/split-button-rtb.png)

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

## <a name="create-a-toggle-split-button"></a>建立切換開關分割按鈕

> **預覽**： ToggleSplitButton 需要的[最新的 Windows 10 Insider Preview 組建和 SDK](https://insider.windows.com/for-developers/) ] 或 [ [Windows UI 文件庫](https://docs.microsoft.com/uwp/toolkits/winui/)。

[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton)有可以個別叫用的兩個部分。 一個部分的行為類似的切換按鈕，可開啟或關閉。 其他部分會叫用的飛出視窗，其中包含其他使用者可從中選擇的選項。

切換分割按鈕通常用來啟用或停用的功能，當此功能有多個使用者可從中選擇的選項。 例如，在文件編輯器中，它可以用來清單上開啟或關閉，雖然下拉式清單來選擇清單的樣式。

> [!NOTE]
> 當叫用觸控式關係，分割按鈕的行為為一種下拉式按鈕。 使用其他方法的輸入，使用者可以個別叫用按鈕的任一種半。 使用觸控時，這兩個部分的按鈕叫用飛出視窗。 因此，您必須在您的飛出視窗的內容來開啟或關閉切換按鈕中包含的選項。

### <a name="differences-with-togglebutton"></a>與 ToggleButton 的差異

不同於[ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton)，ToggleSplitButton 沒有 「 不確定 」 的狀態。 如此一來，您應該牢記這些差異：

- ToggleSplitButton 沒有**IsThreeState**屬性或**未定**事件。
- [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked)屬性是只是**bool**，不是**可為 null 的布林值**。
- ToggleSplitButton 有只[IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged)事件;它不需要個別的**Checked**和**Unchecked**事件。

### <a name="example---toggle-split-button"></a>範例-切換分割按鈕

下列範例示範如何切換，分割按鈕可以用來開啟清單格式設定開啟或關閉和變更的清單，在 RichEditBox 中的樣式。 （如需詳細資訊和程式碼，請參閱[Rich edit 方塊](rich-edit-box.md)）。

![切換分割按鈕來選取清單樣式](images/toggle-split-button-open.png)

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
