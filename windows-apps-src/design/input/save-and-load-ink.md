---
Description: 支援 Windows Ink 的 Windows 應用程式可以將筆墨筆劃序列化和還原序列化為筆跡序列化格式 (ISF) 檔。 ISF 檔案是包含所有筆跡筆觸屬性和行為的其他中繼資料的 GIF 映像。 未啟用筆墨功能的 app 可以檢視靜態的 GIF 影像，包括 Alpha 色板背景透明度。
title: 儲存和抓取 Windows Ink 筆劃資料
ms.assetid: C96C9D2F-DB69-4883-9809-4A0DF7CEC506
label: Store and retrieve Windows Ink stroke data
template: detail.hbs
keywords: Windows Ink, Windows 筆跡, DirectInk, InkPresenter, InkCanvas, ISF, 筆跡序列化格式, 使用者互動, 輸入
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 66ac039774abc2322ab8b5e9a6a264af25156406
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216761"
---
# <a name="store-and-retrieve-windows-ink-stroke-data"></a>儲存和抓取 Windows Ink 筆劃資料


支援 Windows Ink 的 Windows 應用程式可以將筆墨筆劃序列化和還原序列化為筆跡序列化格式 (ISF) 檔。 ISF 檔案是包含所有筆跡筆觸屬性和行為的其他中繼資料的 GIF 映像。 未啟用筆墨功能的 app 可以檢視靜態的 GIF 影像，包括 Alpha 色板背景透明度。

> [!NOTE]
> ISF 是最簡單且易於保留格式的筆跡表示法。 它可以內嵌到二進位文件格式中 (例如 GIF 檔案)，或直接放置到剪貼簿上。

> **重要 API**：[**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)、[**Windows.UI.Input.Inking**](/uwp/api/Windows.UI.Input.Inking)

## <a name="save-ink-strokes-to-a-file"></a>將筆墨筆劃儲存為檔案

我們將在此處示範如何儲存在 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 控制項上繪製的筆墨筆劃。

**從[筆跡序列化格式 (ISF) 檔案的儲存和載入筆墨筆觸，](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)下載此範例**

1.  一開始先設定 UI。

    UI 包含 [儲存]、[載入] 和 [清除] 按鈕，以及 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)。
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnSave" 
                    Content="Save" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnLoad" 
                    Content="Load" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <InkCanvas x:Name="inkCanvas" />
        </Grid>
    </Grid>
```

2.  然後設定一些基本的筆墨輸入行為。

    [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 已設定為會將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃 ([**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes))，而且會針對按鈕上的 click 事件宣告接聽程式。
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to initiate save.
        btnSave.Click += btnSave_Click;
        // Listen for button click to initiate load.
        btnLoad.Click += btnLoad_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;
    }
```

3.  最後，我們會將筆墨儲存在 [ **儲存** ] 按鈕的 click 事件處理常式中。

    [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) 讓使用者能夠選取儲存筆劃資料的檔案和位置。

    選取檔案之後，我們會開啟已設為 [**ReadWrite**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) 的 [**IRandomAccessStream**](/uwp/api/Windows.Storage.FileAccessMode) 資料流。

    接著呼叫 [**SaveAsync**](/uwp/api/windows.ui.input.inking.iinkstrokecontainer.saveasync)，將 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) 管理的筆墨筆劃序列化至資料流。

```csharp
// Save ink data to a file.
    private async void btnSave_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Strokes present on ink canvas.
        if (currentStrokes.Count > 0)
        {
            // Let users choose their ink file using a file picker.
            // Initialize the picker.
            Windows.Storage.Pickers.FileSavePicker savePicker = 
                new Windows.Storage.Pickers.FileSavePicker();
            savePicker.SuggestedStartLocation = 
                Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
            savePicker.FileTypeChoices.Add(
                "GIF with embedded ISF", 
                new List<string>() { ".gif" });
            savePicker.DefaultFileExtension = ".gif";
            savePicker.SuggestedFileName = "InkSample";

            // Show the file picker.
            Windows.Storage.StorageFile file = 
                await savePicker.PickSaveFileAsync();
            // When chosen, picker returns a reference to the selected file.
            if (file != null)
            {
                // Prevent updates to the file until updates are 
                // finalized with call to CompleteUpdatesAsync.
                Windows.Storage.CachedFileManager.DeferUpdates(file);
                // Open a file stream for writing.
                IRandomAccessStream stream = await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
                // Write the ink strokes to the output stream.
                using (IOutputStream outputStream = stream.GetOutputStreamAt(0))
                {
                    await inkCanvas.InkPresenter.StrokeContainer.SaveAsync(outputStream);
                    await outputStream.FlushAsync();
                }
                stream.Dispose();

                // Finalize write so other apps can update file.
                Windows.Storage.Provider.FileUpdateStatus status =
                    await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);

                if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
                {
                    // File saved.
                }
                else
                {
                    // File couldn't be saved.
                }
            }
            // User selects Cancel and picker returns null.
            else
            {
                // Operation cancelled.
            }
        }
    }
```

> [!NOTE]
> GIF 是儲存筆墨資料時唯一支援的檔案格式。 不過，[**LoadAsync**](/uwp/api/windows.ui.input.inking.inkmanager.loadasync) 方法 (請見下一節的示範) 可以支援用於回溯相容性的其他格式。

## <a name="load-ink-strokes-from-a-file"></a>從檔案載入筆墨筆劃

我們將在此處示範如何從檔案載入筆墨筆劃，並在 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 控制項上轉譯它們。

**從[筆跡序列化格式 (ISF) 檔案的儲存和載入筆墨筆觸，](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)下載此範例**

1.  一開始先設定 UI。

    UI 包含 [儲存]、[載入] 和 [清除] 按鈕，以及 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)。
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnSave" 
                    Content="Save" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnLoad" 
                    Content="Load" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <InkCanvas x:Name="inkCanvas" />
        </Grid>
    </Grid>
```

2.  然後設定一些基本的筆墨輸入行為。

    [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 已設定為會將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃 ([**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes))，而且會針對按鈕上的 click 事件宣告接聽程式。
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to initiate save.
        btnSave.Click += btnSave_Click;
        // Listen for button click to initiate load.
        btnLoad.Click += btnLoad_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;
    }
```

3.  最後，我們會將筆墨載入至 **載入** 按鈕的 click 事件處理常式。

    [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 讓使用者能夠選取要從中抓取已儲存之筆墨資料的檔案和位置。

    選取檔案之後，我們會開啟已設為 [**Read**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) 的 [**IRandomAccessStream**](/uwp/api/Windows.Storage.FileAccessMode) 資料流。

    接著呼叫 [**LoadAsync**](/uwp/api/windows.ui.input.inking.inkmanager.loadasync)，來讀取並還原序列化已儲存的筆墨筆劃，然後載入 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer)。 將筆劃載入 **InkStrokeContainer**，會導致 [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 立即將它們轉譯到 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)。

    > [!NOTE]
    > InkStrokeContainer 中的所有現有筆劃都會先清除，然後再載入新的筆劃。

``` csharp
// Load ink data from a file.
private async void btnLoad_Click(object sender, RoutedEventArgs e)
{
    // Let users choose their ink file using a file picker.
    // Initialize the picker.
    Windows.Storage.Pickers.FileOpenPicker openPicker =
        new Windows.Storage.Pickers.FileOpenPicker();
    openPicker.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    openPicker.FileTypeFilter.Add(".gif");
    // Show the file picker.
    Windows.Storage.StorageFile file = await openPicker.PickSingleFileAsync();
    // User selects a file and picker returns a reference to the selected file.
    if (file != null)
    {
        // Open a file stream for reading.
        IRandomAccessStream stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        // Read from file.
        using (var inputStream = stream.GetInputStreamAt(0))
        {
            await inkCanvas.InkPresenter.StrokeContainer.LoadAsync(inputStream);
        }
        stream.Dispose();
    }
    // User selects Cancel and picker returns null.
    else
    {
        // Operation cancelled.
    }
}
```

> [!NOTE]
> GIF 是儲存筆墨資料時唯一支援的檔案格式。 不過，[**LoadAsync**](/uwp/api/windows.ui.input.inking.inkmanager.loadasync) 方法可以支援用於回溯相容性的下列格式。

| 格式                    | 描述 |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| InkSerializedFormat       | 指定使用 ISF 保留的筆墨。 這是最簡單且易於保留格式的筆墨表示法。 它可以內嵌到二進位文件格式中，或直接放置到剪貼簿上。                                                                                                                                                                                                         |
| Base64InkSerializedFormat | 指定透過將 ISF 編碼為 base64 資料流來保留的筆墨。 提供這個格式是為了讓筆墨能夠直接在 XML 或 HTML 檔案中進行編碼。                                                                                                                                                                                                                                                |
| Gif                       | 指定使用 GIF 檔案保留的筆墨，這種檔案包含做為中繼資料內嵌在檔案中的 ISF。 這可讓筆墨可在沒有啟用筆墨功能的應用程式中加以檢視，並且在其返回啟用筆墨功能的應用程式時仍完全不失真。 這個格式適合用來在 HTML 檔案中傳輸筆墨內容，讓筆墨和非筆墨應用程式都可以使用該內容。 |
| Base64Gif                 | 指定使用 base64 編碼保護之 GIF 保留的筆墨。 提供這個格式是為了在 XML 或 HTML 檔案中直接編碼筆墨，以便稍後轉換為影像。 可能的使用情況是產生包含所有筆墨資訊的 XML 格式，並用來透過可延伸樣式表語言轉換 (Extensible Stylesheet Language Transformations，XSLT) 來產生 HTML。 

## <a name="copy-and-paste-ink-strokes-with-the-clipboard"></a>複製筆墨筆劃並貼上剪貼簿

我們將在此處示範如何使用剪貼簿，在 app 之間傳輸筆墨筆劃。

若要支援剪貼簿功能，內建的 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) 剪下和複製命令會要求選取一或多個筆墨筆劃。

在這個範例中，我們將在使用畫筆筆身按鈕 (或滑鼠右鍵按鈕) 修改輸入時啟用筆劃選取項目。 如需如何實作筆劃選取項目的完整範例，請參閱[畫筆和手寫筆互動](pen-and-stylus-interactions.md)中的＜傳入輸入以進行進階處理＞。

**從剪貼簿的[儲存和載入筆墨筆劃](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)下載此範例**

1.  一開始先設定 UI。

    UI 包含 [剪下]、[複製]、[貼上] 和 [清除] 按鈕，以及 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 和選取項目畫布。
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="tbHeader" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnCut" 
                    Content="Cut" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnCopy" 
                    Content="Copy" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnPaste" 
                    Content="Paste" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="20,0,10,0"/>
        </StackPanel>
        <Grid x:Name="gridCanvas" Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
        </Grid>
    </Grid>
```

2.  然後設定一些基本的筆墨輸入行為。

    [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 已設定為可將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃 ([**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes))。 此處也會宣告按鈕上適用於 click 事件的接聽程式，以及適用於選取功能的指標和筆劃事件。

    如需如何實作筆劃選取項目的完整範例，請參閱[畫筆和手寫筆互動](pen-and-stylus-interactions.md)中的＜傳入輸入以進行進階處理＞。
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to cut ink strokes.
        btnCut.Click += btnCut_Click;
        // Listen for button click to copy ink strokes.
        btnCopy.Click += btnCopy_Click;
        // Listen for button click to paste ink strokes.
        btnPaste.Click += btnPaste_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;

        // By default, the InkPresenter processes input modified by 
        // a secondary affordance (pen barrel button, right mouse 
        // button, or similar) as ink.
        // To pass through modified input to the app for custom processing 
        // on the app UI thread instead of the background ink thread, set 
        // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
        inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
            InkInputRightDragAction.LeaveUnprocessed;

        // Listen for unprocessed pointer events from modified input.
        // The input is used to provide selection functionality.
        inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
            UnprocessedInput_PointerPressed;
        inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
            UnprocessedInput_PointerMoved;
        inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
            UnprocessedInput_PointerReleased;

        // Listen for new ink or erase strokes to clean up selection UI.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            StrokeInput_StrokeStarted;
        inkCanvas.InkPresenter.StrokesErased +=
            InkPresenter_StrokesErased;
    }
```

3.  最後，在新增筆觸選取範圍支援之後，我們會在 **剪**下、 **複製**和 **貼** 上按鈕的 click 事件處理常式中，執行剪貼簿功能。

    針對剪下功能，我們會先在 [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.copyselectedtoclipboard) 的 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) 上呼叫 [**CopySelectedToClipboard**](/uwp/api/Windows.UI.Input.Inking.InkPresenter)。

    接著呼叫 [**DeleteSelected**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.deleteselected) 來移除筆墨畫布上的筆劃。

    最後，從選取項目畫布中刪除所有選取筆劃。
    
```csharp
private void btnCut_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
        ClearSelection();
    }
```
```csharp
// Clean up selection UI.
    private void ClearSelection()
    {
        var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        foreach (var stroke in strokes)
        {
            stroke.Selected = false;
        }
        ClearDrawnBoundingRect();
    }

    private void ClearDrawnBoundingRect()
    {
        if (selectionCanvas.Children.Any())
        {
            selectionCanvas.Children.Clear();
            boundingRect = Rect.Empty;
        }
    }
```

針對複製功能，我們只需在 [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.copyselectedtoclipboard) 的 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) 上呼叫 [**CopySelectedToClipboard**](/uwp/api/Windows.UI.Input.Inking.InkPresenter)。


```csharp
private void btnCopy_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
    }
```

針對貼上，我們呼叫 [**CanPasteFromClipboard**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.canpastefromclipboard) 確認剪貼簿上的內容可貼到筆跡畫布。

若可以的話，我們便會呼叫 [**PasteFromClipboard**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.pastefromclipboard) 將剪貼簿的筆墨筆劃插入 [**IntPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) 的 [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer)，接著便會將筆劃轉譯到筆跡畫布。

```csharp
private void btnPaste_Click(object sender, RoutedEventArgs e)
    {
        if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
        {
            inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                new Point(0, 0));
        }
        else
        {
            // Cannot paste from clipboard.
        }
    }
```

## <a name="related-articles"></a>相關文章

* [畫筆和手寫筆互動](pen-and-stylus-interactions.md)

**主題範例**
* [儲存和從筆跡序列化格式 (ISF) 檔案載入筆墨筆劃](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [儲存和從剪貼簿載入筆墨筆劃](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)

**其他範例**
* [簡單的筆跡範例 (C#/C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
* [複雜的筆跡範例 (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
* [筆跡範例 (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
* [開始教學課程：在您的 Windows 應用程式中支援筆墨](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
* [著色本範例](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [家庭記事本範例](https://github.com/Microsoft/Windows-appsample-familynotes)
