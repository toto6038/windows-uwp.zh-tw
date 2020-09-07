---
title: 按鈕
description: 了解如何使用按鈕，讓使用者有辦法觸發立即動作，並了解特定工作的特殊按鈕。
label: Buttons
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 18007214cfd54edda5b2ba23aed241b85a0e7199
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157182"
---
# <a name="buttons"></a>按鈕

按鈕讓使用者得以觸發立即動作。 某些按鈕專門用於特定工作，例如瀏覽、重複的動作，或呈現功能表。

![按鈕的範例](images/controls/button.png)

[Extensible Application Markup Language (XAML)](../../xaml-platform/xaml-overview.md) 架構提供標準按鈕控制項，以及數個特製化的按鈕控制項。

控制 | 說明
------- | -----------
[Button](/uwp/api/windows.ui.xaml.controls.button) | 起始立即動作的按鈕。 可以搭配 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件或 [Command](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) 繫結使用。
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | 按下時會接連引發 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件的按鈕。
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | 樣式類似超連結且用於瀏覽的按鈕。 如需有關超連結的詳細資訊，請參閱[超連結](hyperlinks.md)。
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | ![WinUI 標誌](images/winui-logo-16x16.png) 用於開啟附加飛出視窗的按鈕 (具有＞形箭號)。
[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) | ![WinUI 標誌](images/winui-logo-16x16.png) 具有兩端的按鈕。 一端可起始動作，另一端可開啟功能表。
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | ![WinUI 標誌](images/winui-logo-16x16.png) 具有兩端的切換按鈕。 一端可開啟/關閉，另一端可開啟功能表。
[ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton) | 可以開啟或關閉的按鈕。

**取得 Windows UI 程式庫**

|  |  |
| - | - |
| ![WinUI 標誌](images/winui-logo-64x64.png) | **DropDownButton**、**SplitButton** 和 **ToggleSplitButton** 包含在 Windows UI 程式庫中；該程式庫是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](/uwp/toolkits/winui/)。 |

> **Windows UI 程式庫 API：** [DropDownButton 類別](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)、[SplitButton 類別](/uwp/api/microsoft.ui.xaml.controls.splitbutton)、[ToggleSplitButton 類別](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton)
>
> **平台 API：** [Click 事件](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)、[Command 屬性](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用 **Button** 控制項讓使用者起始立即動作，例如提交表單。

如果動作是瀏覽到另一個頁面，請不要使用 **Button** 控制項，改為使用 [HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton)控制項。 如需有關超連結的詳細資訊，請參閱[超連結](hyperlinks.md)。

> [!IMPORTANT]
> 對於精靈瀏覽，請使用標籤為 [上一頁]  和 [下一頁]  的按鈕。 對於其他類型的反向瀏覽或瀏覽到上一層，請使用[返回按鈕](../basics/navigation-history-and-backwards-navigation.md)。

當使用者想要重複觸發動作時，請使用 **RepeatButton** 控制項。 例如，使用 **RepeatButton** 控制項使計數器中的值遞增或遞減。

當按鈕具有包含更多選項的飛出視窗時，請使用 **DropDownButton** 控制項。 預設＞形箭號會提供按鈕包含飛出視窗的視覺指標。

當您想讓使用者能夠起始立即動作，或獨立選擇其他選項時，請使用 **SplitButton** 控制項。

當您希望使用者能夠立即在兩個互斥的狀態之間切換，且按鈕最符合您的 UI 需求時，請使用 **ToggleButton** 控制項。 除非按鈕能為您的 UI 帶來幫助，否則使用 [AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton)、[CheckBox](checkbox.md)、[RadioButton](radio-button.md) 或 [ToggleSwitch](toggles.md)可能是更佳的選擇。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>，請按一下這裡<a href="xamlcontrolsgallery:/item/Button">開啟應用程式並查看 Button 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

這個範例在要求位置存取權的對話方塊中使用兩個按鈕 ([允許]  和 [封鎖]  )。

![按鈕的範例 (用於對話方塊中)](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>建立按鈕

這個範例會顯示一個按鈕，它會回應 Click (按一下) 動作。

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

處理 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件。

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

當您以手指或手寫筆點選 **Button** 控制項，或在游標位於按鈕上方時按下滑鼠左鍵，按鈕會引發 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件。 如果按鈕有鍵盤焦點，則按下 Enter 鍵或空格鍵也會引發 **Click** 事件。

您通常無法處理 **Button** 物件上的低階 [PointerPressed](/uwp/api/windows.ui.xaml.uielement.pointerpressed) 事件，因為按鈕本身有 **Click** 行為。 如需詳細資訊，請參閱[事件與路由事件概觀](../../xaml-platform/events-and-routed-events-overview.md)。

您可以變更 [ClickMode](/uwp/api/windows.ui.xaml.controls.clickmode) 屬性，以變更按鈕引發 **Click** 事件的方式。 預設 **ClickMode** 值是 **Release**，但您也可以將按鈕的 **ClickMode** 值設定為 **Hover** 或 **Press**。 如果 **ClickMode** 是 **Hover**，則使用鍵盤或觸控方式並不能引發 **Click** 事件。


### <a name="button-content"></a>按鈕內容

**Button**是 [ContentControl](/uwp/api/Windows.UI.Xaml.Controls.ContentControl) 類別的內容控制項。 它的 XAML 內容屬性是 [Content](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)，它可讓您使用如下 XAML 語法︰`<Button>A button's content</Button>`。 您可以將任何物件設定為按鈕的內容。 如果內容是 [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) 物件，就會呈現於按鈕中。 如果內容是其他類型的物件，則會在按鈕中顯示它的字串表示。

按鈕的內容通常是文字。 當您設計該文字時，請使用下列建議：

-  使用簡潔、專屬、一目了然的文字，清楚說明按鈕執行的動作。 通常按鈕文字是一個動詞單字。

-  除非您的品牌指導方針指示您使用其他字型，否則使用預設字型。

-  對於簡短文字，避免使用 120px 的最小按鈕寬度將命令按鈕變窄。

- 對於較長文字，避免將文字限制在 26 個字元的最大長度，以使命令按鈕變寬。

-  如果按鈕的文字內容為動態 (也就是經過[當地語系化](../globalizing/globalizing-portal.md))，請考慮如何調整按鈕的大小以及調整後周圍的控制項會發生什麼變化。

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

[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) 控制項是一個按鈕，可以從按鈕被按下的當時到鬆開後為止，重複引發 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件。 設定 [Delay](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.delay) 屬性以指定 **RepeatButton** 控制項在它被按下之後以及在它開始重複按一下動作之前，必須等待的時間。 設定 [Interval](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton.interval) 屬性以指定重複按下動作之間的時間。 這兩個屬性的時間皆是以毫秒為單位來指定。

下列範例顯示兩個 **RepeatButton** 控制項，而且其各自的 **Click** 事件是用來增加或減少文字區塊中顯示的值。

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

## <a name="create-a-drop-down-button"></a>建立下拉式按鈕

> **DropDownButton** 需要 [Windows UI 程式庫](/uwp/toolkits/winui/)或 Windows 10 版本 1809 (SDK 17763) 或更新版本。 若要下載最新的 SDK，請參閱 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)；若要下載舊版的 SDK，請參閱 [Windows SDK 和模擬器封存](https://developer.microsoft.com/windows/downloads/sdk-archive)。

[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) 是一個按鈕，其將＞形箭號顯示為視覺指標，具有包含許多選項的附加飛出視窗。 它的行為與具有飛出視窗的標準 **Button** 控制項相同，只有外觀不同。

下拉式按鈕會繼承 **Click** 事件，但您通常不會使用它。 相反地，您可以使用 **Flyout** 屬性來附加飛出視窗，並使用飛出視窗中的功能表選項來叫用動作。 按一下此按鈕時，飛出視窗就會自動開啟。
請務必指定飛出視窗的 [Placement](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement)，以確保按鈕相關的所需放置方式。 預設放置演算法可能不會在所有情況下產生預計的放置方式。

> [!TIP]
> 如需飛出視窗的詳細資訊，請參閱[功能表和操作功能表](menus.md)。

### <a name="example---drop-down-button"></a>範例 - 下拉式按鈕

此範例示範如何建立具有飛出視窗的下拉式按鈕，其中包含 **RichEditBox** 控制項中段落對齊的命令。 (如需詳細資訊和程式碼，請參閱 [Rich Edit 方塊](rich-edit-box.md))。

![具有對齊命令的下拉式按鈕](images/drop-down-button-align.png)

```xaml
<DropDownButton ToolTipService.ToolTip="Alignment">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="14" Text="&#xE8E4;"/>
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

 > [!IMPORTANT]
 > **SplitButton** 需要 [Windows UI 程式庫](/uwp/toolkits/winui/)或 Windows 10 版本 1809 (SDK 17763) 或更新版本。 若要下載最新的 SDK，請參閱 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)；若要下載舊版的 SDK，請參閱 [Windows SDK 和模擬器封存](https://developer.microsoft.com/windows/downloads/sdk-archive)。

[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) 控制項有可以分別叫用的兩個組件。 一個組件的行為類似標準按鈕，並且會叫用立即動作。 另一個組件會叫用飛出視窗，其中包含使用者可以選擇的其他選項。

> [!NOTE]
> 以觸控方式叫用時，分割按鈕時的行為如同下拉式按鈕，按鈕的兩半可叫用飛出視窗。 使用其他輸入方法，使用者可以分別叫用按鈕的任一半。

分割按鈕的典型行為如下：

- 當使用者按一下按鈕組件時，處理 **Click** 事件以叫用下拉式清單中目前已選取的選項。

- 當下拉式清單開啟時，處理下拉式清單中項目的引動過程，兩者均可變更已選取的選項，然後再叫用它。 務必叫用飛出視窗項目，因為按鈕 **Click** 事件不會在使用觸控時發生。

> [!TIP]
> 有許多種方式可將項目放入下拉式清單並處理其引動過程。 如果您使用 **ListView** 或 **GridView**，其中一種方式就是處理 **SelectionChanged** 事件。 如果這樣做，請將 [SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus) 設定為 **false**。 這可讓使用者使用鍵盤來瀏覽選項，而不需在每次變更時叫用項目。

### <a name="example---split-button"></a>範例 - 分割按鈕

此範例示範如何建立一個分割按鈕，用來變更 **RichEditBox** 控制項中所選文字的前景色彩。 (如需詳細資訊和程式碼，請參閱 [Rich Edit 方塊](rich-edit-box.md))。
分割按鈕的飛出視窗使用 [BottomEdgeAlignedLeft](/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode) 作為其 [Placement](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement) 屬性的預設值。 您無法覆寫此值。

![用來選取前景色彩的分割按鈕](images/split-button-rtb.png)

```xaml
<SplitButton ToolTipService.ToolTip="Foreground color"
             Click="BrushButtonClick">
    <Border x:Name="SelectedColorBorder" Width="20" Height="20"/>
    <SplitButton.Flyout>
        <Flyout x:Name="BrushFlyout">
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

> [!NOTE]
> **ToggleSplitButton** 需要 [Windows UI 程式庫](/uwp/toolkits/winui/)或 Windows 10 版本 1809 (SDK 17763) 或更新版本。 若要下載最新的 SDK，請參閱 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)；若要下載舊版的 SDK，請參閱 [Windows SDK 和模擬器封存](https://developer.microsoft.com/windows/downloads/sdk-archive)。

[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) 控制項有可以分別叫用的兩個組件。 一個組件的行為類似可以開或關的切換按鈕。 另一個組件會叫用飛出視窗，其中包含使用者可以選擇的其他選項。

如果功能有使用者可從中選擇的多個選項時，切換分割按鈕通常用來啟用或停用該功能。 例如，在文件編輯器中，它可用來開啟或關閉清單，而下拉式清單則用來選擇清單的樣式。

> [!NOTE]
> 以觸控方式叫用時，切換分割按鈕的行為如同下拉式按鈕。 使用其他輸入方法，使用者可以切換並分別叫用按鈕的兩半。 使用觸控，按鈕的兩半可叫用飛出視窗。 因此，您必須在飛出視窗內容中包含用以開啟或關閉的選項。


### <a name="differences-with-togglebutton"></a>與 ToggleButton 的差異

不同於 [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton)，**ToggleSplitButton** 沒有不確定的狀態。 因此，您應該記住下列差異：

- **ToggleSplitButton** 沒有 **IsThreeState** 屬性或 **Indeterminate**事件。
- [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked) 屬性只是布林值，而不是 **Nullable<bool>** 。
- **ToggleSplitButton** 只有 [IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged) 事件，並沒有個別的 **Checked** 和 **Unchecked** 事件。


### <a name="example---toggle-split-button"></a>範例 - 切換分割按鈕

下列範例會示範切換分割按鈕如何用來開啟或關閉清單格式調整，以及變更 **RichEditBox** 控制項中的樣式清單。 (如需詳細資訊和程式碼，請參閱 [Rich Edit 方塊](rich-edit-box.md))。
切換分割按鈕的飛出視窗使用 [BottomEdgeAlignedLeft](/uwp/api/windows.ui.xaml.controls.primitives.flyoutplacementmode) 作為其 [Placement](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.placement) 屬性的預設值。 您無法覆寫此值。

![用於選取清單樣式的切換分割按鈕](images/toggle-split-button-open.png)

```xaml
<ToggleSplitButton x:Name="ListButton"
                   ToolTipService.ToolTip="List style"
                   Click="ListButton_Click"
                   IsCheckedChanged="ListStyleButton_IsCheckedChanged">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="14" Text="&#xE8FD;"/>
    <ToggleSplitButton.Flyout>
        <Flyout>
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
    - [取消]

- 一次只對使用者顯示一或兩個按鈕，例如，[接受]  和 [取消]  。 如果需要對使用者顯示更多動作，請考慮使用[核取方塊](checkbox.md)或[選項按鈕](radio-button.md)，使用者可以利用它們選取動作，只要一個命令按鈕即可觸發這些動作。

- 對於需要在應用程式內多個頁面上提供的動作，請不要在多個頁面上複製按鈕，而是考慮改用[應用程式底部列](app-bars.md)。


### <a name="recommended-single-button-layout"></a>建議使用的單一按鈕配置

如果您的配置只需要一個按鈕，此按鈕應根據其容器內容靠左或靠右對齊。

  - 只有一個按鈕的對話方塊必須讓按鈕**靠右對齊**。 如果對話方塊只包含一個按鈕，請確保按鈕執行的是安全、不具破壞性的動作。 如果您使用 [ContentDialog](./dialogs-and-flyouts/index.md) 並指定單一按鈕，此按鈕將會自動靠右對齊。

    ![對話方塊內的按鈕](images/pushbutton_doc_dialog.png)

  - 如果您的按鈕會在容器 UI 中出現 (例如，在快顯通知、飛出視窗或的清單檢視項目中)，就必須讓按鈕在容器內**靠右對齊**。

    ![容器內的按鈕](images/pushbutton_doc_container.png)

  - 在包含單一按鈕 (例如，設定頁面底部的 [套用]  按鈕) 的頁面中，您應該讓按鈕**靠左對齊**。 這樣可確保按鈕對齊其餘的網頁內容。

    ![頁面上的按鈕](images/pushbutton_doc_page.png)


## <a name="back-buttons"></a>返回按鈕

返回按鈕是一種系統提供的 UI 元素，可透過使用者的返回堆疊或瀏覽歷程記錄啟用向後瀏覽。 您不需要自行建立返回按鈕，但您可能必須花一點心力來提供良好的向後瀏覽體驗。 如需詳細資訊，請參閱 [Windows 應用程式的瀏覽歷程記錄和向後瀏覽](../basics/navigation-history-and-backwards-navigation.md)。


## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫](https://github.com/Microsoft/Xaml-Controls-Gallery)：此範例會以互動式格式顯示所有 XAML 控制項。


## <a name="related-articles"></a>相關文章

- [Button 類別](/uwp/api/windows.ui.xaml.controls.button)
- [選項按鈕](radio-button.md)
- [核取方塊](checkbox.md)
- [切換開關](toggles.md)