---
author: Jwmsft
Description: "您可以使用 RichEditBox 控制項來輸入和編輯包含格式化文字、超連結及影像的 RTF 文件。 您可以藉由將 IsReadOnly 屬性設定成 true，使 RichEditBox 變成唯讀。"
title: RichEditBox
ms.assetid: 4AFC0DFA-3B89-434D-9F86-4309CCFF7839
label: Rich edit box
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ae0fab5d09779ccfcc1c9b15e29921c07f0f6c6d
ms.lasthandoff: 02/07/2017

---
# <a name="rich-edit-box"></a>Rich Edit 方塊

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

您可以使用 RichEditBox 控制項來輸入和編輯包含格式化文字、超連結及影像的 RTF 文件。 您可以藉由將 IsReadOnly 屬性設定成 **true**，使 RichEditBox 變成唯讀。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**RichEditBox 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx)</li>
<li>[**Document 屬性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.document.aspx)</li>
<li>[**IsReadOnly 屬性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.isreadonly.aspx)</li>
<li>[**IsSpellCheckEnabled 屬性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.isspellcheckenabled.aspx)</li>
</ul>
</div>

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用 **RichEditBox** 顯示和編輯文字檔案。 您不會透過您使用其他標準文字輸入方塊的方式，使用 RichEditBox 取得針對您應用程式的使用者輸入內容。 但是，您會使用它來處理與您應用程式不同的文字檔案。 您通常會將輸入到 RichEditBox 的文字儲存為 .rtf 檔案。
-   如果多行文字方塊的主要目的為建立文件 (例如部落格文章或電子郵件內容)，而且這些文件需要 RTF，則使用 RTF 方塊。
-   如果您希望使用者能夠格式化文字，請使用 RTF 方塊。
-   擷取只會消耗且不會再對使用者顯示的文字時，請使用純文字輸入控制項。
-   針對所有其他案例，請使用純文字輸入控制項。

如需如何選擇正確文字控制項的詳細資訊，請參閱[文字控制項](text-controls.md)文章。

## <a name="examples"></a>範例

此 Rich Edit 方塊已在其中開啟一個 RTF 文件。 格式和檔案的按鈕不屬於 Rich Edit 方塊的一部分，但您應該至少提供最小一組樣式按鈕，並實作其動作。

![包含已開啟文件的 RTF 方塊](images/rich-edit-box.png)

## <a name="create-a-rich-edit-box"></a>建立 Rich Edit 方塊

根據預設，RichEditBox 支援拼字檢查。 若要停用拼字檢查工具，請將 [IsSpellCheckEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.isspellcheckenabled.aspx) 屬性設為 **false**。 如需詳細資訊，請參閱[拼字檢查的指導方針](spell-checking-and-prediction.md)文章。

您可以使用 RichEditBox 的 [Document](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.document.aspx) 屬性取得其內容。 RichEditBox 的內容是 [Windows.UI.Text.ITextDocument](https://msdn.microsoft.com/library/windows/apps/xaml/bb774052.aspx) 物件，與使用[Windows.UI.Xaml.Documents.Block](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.block.aspx) 物件做為其內容的 RichTextBlock 控制項不同。 ITextDocument 介面提供的方法可載入文件並儲存為資料流、抓取文字範圍、取得使用中的選取項目、復原和重做變更、設定預設格式化屬性等等。

這個範例說明如何在 RichEditBox 中編輯、載入和儲存 RTF 格式 (rtf) 檔案。

```xaml
<RelativePanel Margin="20" HorizontalAlignment="Stretch">
    <RelativePanel.Resources>
        <Style TargetType="AppBarButton">
            <Setter Property="IsCompact" Value="True"/>
        </Style>
    </RelativePanel.Resources>
    <AppBarButton x:Name="openFileButton" Icon="OpenFile"
                  Click="OpenButton_Click" ToolTipService.ToolTip="Open file"/>
    <AppBarButton Icon="Save" Click="SaveButton_Click"
                  ToolTipService.ToolTip="Save file"
                  RelativePanel.RightOf="openFileButton" Margin="8,0,0,0"/>

    <AppBarButton Icon="Bold" Click="BoldButton_Click" ToolTipService.ToolTip="Bold"
                  RelativePanel.LeftOf="italicButton" Margin="0,0,8,0"/>
    <AppBarButton x:Name="italicButton" Icon="Italic" Click="ItalicButton_Click"
                  ToolTipService.ToolTip="Italic" RelativePanel.LeftOf="underlineButton" Margin="0,0,8,0"/>
    <AppBarButton x:Name="underlineButton" Icon="Underline" Click="UnderlineButton_Click"
                  ToolTipService.ToolTip="Underline" RelativePanel.AlignRightWithPanel="True"/>

    <RichEditBox x:Name="editor" Height="200" RelativePanel.Below="openFileButton"
                 RelativePanel.AlignLeftWithPanel="True" RelativePanel.AlignRightWithPanel="True"/>
</RelativePanel>
```


```csharp
private async void OpenButton_Click(object sender, RoutedEventArgs e)
{
    // Open a text file.
    Windows.Storage.Pickers.FileOpenPicker open =
        new Windows.Storage.Pickers.FileOpenPicker();
    open.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    open.FileTypeFilter.Add(".rtf");

    Windows.Storage.StorageFile file = await open.PickSingleFileAsync();

    if (file != null)
    {
        try
        {
            Windows.Storage.Streams.IRandomAccessStream randAccStream =
        await file.OpenAsync(Windows.Storage.FileAccessMode.Read);

            // Load the file into the Document property of the RichEditBox.
            editor.Document.LoadFromStream(Windows.UI.Text.TextSetOptions.FormatRtf, randAccStream);
        }
        catch (Exception)
        {
            ContentDialog errorDialog = new ContentDialog()
            {
                Title = "File open error",
                Content = "Sorry, I couldn't open the file.",
                PrimaryButtonText = "Ok"
            };

            await errorDialog.ShowAsync();
        }
    }
}

private async void SaveButton_Click(object sender, RoutedEventArgs e)
{
    Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
    savePicker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;

    // Dropdown of file types the user can save the file as
    savePicker.FileTypeChoices.Add("Rich Text", new List<string>() { ".rtf" });

    // Default file name if the user does not type one in or select a file to replace
    savePicker.SuggestedFileName = "New Document";

    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
    if (file != null)
    {
        // Prevent updates to the remote version of the file until we
        // finish making changes and call CompleteUpdatesAsync.
        Windows.Storage.CachedFileManager.DeferUpdates(file);
        // write to file
        Windows.Storage.Streams.IRandomAccessStream randAccStream =
            await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);

        editor.Document.SaveToStream(Windows.UI.Text.TextGetOptions.FormatRtf, randAccStream);

        // Let Windows know that we're finished changing the file so the
        // other app can update the remote version of the file.
        Windows.Storage.Provider.FileUpdateStatus status = await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
        if (status != Windows.Storage.Provider.FileUpdateStatus.Complete)
        {
            Windows.UI.Popups.MessageDialog errorBox =
                new Windows.UI.Popups.MessageDialog("File " + file.Name + " couldn't be saved.");
            await errorBox.ShowAsync();
        }
    }
}

private void BoldButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        charFormatting.Bold = Windows.UI.Text.FormatEffect.Toggle;
        selectedText.CharacterFormat = charFormatting;
    }
}

private void ItalicButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        charFormatting.Italic = Windows.UI.Text.FormatEffect.Toggle;
        selectedText.CharacterFormat = charFormatting;
    }
}

private void UnderlineButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        if (charFormatting.Underline == Windows.UI.Text.UnderlineType.None)
        {
            charFormatting.Underline = Windows.UI.Text.UnderlineType.Single;
        }
        else {
            charFormatting.Underline = Windows.UI.Text.UnderlineType.None;
        }
        selectedText.CharacterFormat = charFormatting;
    }
}
```

## <a name="choose-the-right-keyboard-for-your-text-control"></a>選擇文字控制項的正確鍵盤

為協助使用者使用觸控式鍵盤或螢幕輸入面板 (SIP) 輸入資料，您可以設定文字控制項的輸入範圍，使其符合使用者要輸入的資料類型。 預設鍵盤配置通常適用於處理 RTF 文件。

如需如何使用輸入範圍的詳細資訊，請參閱[使用輸入範圍來變更觸控式鍵盤](https://msdn.microsoft.com/library/windows/apps/mt280229)。

## <a name="dos-and-donts"></a>可行與禁止事項

-   建立 RTF 方塊時，提供設定樣式按鈕並實作其動作。
-   使用與您的應用程式樣式一致的字型。
-   文字控制項的高度必須能夠容納一般輸入。
-   不要讓文字輸入控制項的高度隨著使用者輸入增加。
-   使用者只需要單行時，不要使用多行文字方塊。
-   使用純文字控制項就足夠時，不要使用 RTF 控制項。


## <a name="related-articles"></a>相關文章

* [文字控制項](text-controls.md)
- [拼字檢查指導方針](spell-checking-and-prediction.md)
- [新增搜尋](search.md)
- [文字輸入的指導方針](text-controls.md)
- [**TextBox 類別**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Windows.UI.Xaml.Controls PasswordBox 類別**](https://msdn.microsoft.com/library/windows/apps/br227519)

