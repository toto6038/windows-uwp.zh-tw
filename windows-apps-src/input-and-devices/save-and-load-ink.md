---
author: Karl-Bridge-Microsoft
Description: "支援 Windows Ink 的 UWP app 可以將筆跡筆觸序列化和還原序列化至筆跡序列化格式 (ISF) 檔案。 ISF 檔案是包含所有筆跡筆觸屬性和行為的其他中繼資料的 GIF 映像。 未啟用筆墨功能的 app 可以檢視靜態的 GIF 影像，包括 Alpha 色板背景透明度。"
title: "儲存和抓取 Windows Ink 筆觸資料"
ms.assetid: C96C9D2F-DB69-4883-9809-4A0DF7CEC506
label: Store and retrieve Windows Ink stroke data
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, ISF, Ink Serialized Format
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: cdef00304e1835532eceb8e51fecc8045f2ff300

---

# 儲存和抓取 Windows Ink 筆觸資料


支援 Windows Ink 的 UWP app 可以將筆跡筆觸序列化和還原序列化至筆跡序列化格式 (ISF) 檔案。 ISF 檔案是包含所有筆跡筆觸屬性和行為的其他中繼資料的 GIF 映像。 未啟用筆墨功能的 app 可以檢視靜態的 GIF 影像，包括 Alpha 色板背景透明度。


**重要 API**

-   [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)



**注意**  
ISF 是最簡單且易於保留格式的筆跡表示法。 它可以內嵌到二進位文件格式中 (例如 GIF 檔案)，或直接放置到剪貼簿上。

 

## 將筆墨筆劃儲存為檔案


我們將在此處示範如何儲存在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控制項上繪製的筆墨筆劃。

1.  一開始先設定 UI。

    UI 包含 \[儲存\]、\[載入\] 和 \[清除\] 按鈕，以及 InkCanvas。
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

    [
              **InkPresenter**
            ](https://msdn.microsoft.com/library/windows/apps/dn899081) 已設定為會將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃 ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019))，而且會針對按鈕上的 click 事件宣告接聽程式。
```    CSharp
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

3.  最後，會在 \[儲存\] 按鈕的 click 事件處理常式中儲存筆劃。

    [
              **FileSavePicker**
            ](https://msdn.microsoft.com/library/windows/apps/br207871) 讓使用者能夠選取儲存筆劃資料的檔案和位置。

    選取檔案之後，我們會開啟已設為 [**ReadWrite**](https://msdn.microsoft.com/library/windows/apps/br241635) 的 [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731) 資料流。

    接著呼叫 [**SaveAsync**](https://msdn.microsoft.com/library/windows/apps/br242114)，將 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 管理的筆墨筆劃序列化至資料流。
```    CSharp
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
                    // File couldn&#39;t be saved.
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

[!NOTE]  
GIF 是唯一可用來儲存筆墨資料的支援格式。 不過，[**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/hh701607) 方法 (請見下一節的示範) 可以支援用於回溯相容性的其他格式。

 

## 從檔案載入筆墨筆劃


我們將在此處示範如何從檔案載入筆墨筆劃，並在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控制項上轉譯它們。

1.  一開始先設定 UI。

    UI 包含 \[儲存\]、\[載入\] 和 \[清除\] 按鈕，以及 InkCanvas。
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

    [
              **InkPresenter**
            ](https://msdn.microsoft.com/library/windows/apps/dn899081) 已設定為會將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃 ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019))，而且會針對按鈕上的 click 事件宣告接聽程式。
```    CSharp
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

3.  最後，會在 \[載入\] 按鈕的 click 事件處理常式中載入筆墨。

    [
              **FileOpenPicker**
            ](https://msdn.microsoft.com/library/windows/apps/br207847) 讓使用者能夠選取要從中抓取已儲存之筆墨資料的檔案和位置。

    選取檔案之後，我們會開啟已設為 [**Read**](https://msdn.microsoft.com/library/windows/apps/br241635) 的 [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731) 資料流。

    接著呼叫 [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/hh701607)，來讀取並還原序列化已儲存的筆墨筆劃，然後載入 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492)。 將筆劃載入 **InkStrokeContainer**，會導致 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 立即將它們轉譯到 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)。
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
            await inkCanvas.InkPresenter.StrokeContainer.LoadAsync(stream);
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

**注意**  
GIF 是唯一可用來儲存筆墨資料的支援格式。 不過，[**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/hh701607) 方法可以支援用於回溯相容性的下列格式。

| 格式                    | 說明 |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| InkSerializedFormat       | 指定使用 ISF 保留的筆墨。 這是最簡單且易於保留格式的筆墨表示法。 它可以內嵌到二進位文件格式中，或直接放置到剪貼簿上。                                                                                                                                                                                                         |
| Base64InkSerializedFormat | 指定透過將 ISF 編碼為 base64 資料流來保留的筆墨。 提供這個格式是為了讓筆墨能夠直接在 XML 或 HTML 檔案中進行編碼。                                                                                                                                                                                                                                                |
| GIF                       | 指定使用 GIF 檔案保留的筆墨，這種檔案包含做為中繼資料內嵌在檔案中的 ISF。 這可讓筆墨可在沒有啟用筆墨功能的應用程式中加以檢視，並且在其返回啟用筆墨功能的應用程式時仍完全不失真。 這個格式適合用來在 HTML 檔案中傳輸筆墨內容，讓筆墨和非筆墨應用程式都可以使用該內容。 |
| Base64Gif                 | 指定使用 base64 編碼保護之 GIF 保留的筆墨。 提供這個格式是為了在 XML 或 HTML 檔案中直接編碼筆墨，以便稍後轉換為影像。 可能的使用情況是產生包含所有筆墨資訊的 XML 格式，並用來透過可延伸樣式表語言轉換 (Extensible Stylesheet Language Transformations，XSLT) 來產生 HTML。 

## 複製筆墨筆劃並貼上剪貼簿


我們將在此處示範如何使用剪貼簿，在 app 之間傳輸筆墨筆劃。

若要支援剪貼簿功能，內建的 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 剪下和複製命令會要求選取一或多個筆墨筆劃。

在這個範例中，我們將在使用畫筆筆身按鈕 (或滑鼠右鍵按鈕) 修改輸入時啟用筆劃選取項目。 如需如何實作筆劃選取項目的完整範例，請參閱[畫筆和手寫筆互動](pen-and-stylus-interactions.md)中的[傳入輸入以進行進階處理](pen-and-stylus-interactions.md#passthrough)。

1.  一開始先設定 UI。

    UI 包含 \[剪下\]、\[複製\]、\[貼上\] 和 \[清除\] 按鈕，以及 InkCanvas 和選取項目畫布。
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

    [
              **InkPresenter**
            ](https://msdn.microsoft.com/library/windows/apps/dn899081) 已設定為可將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃 ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019))。 此處也會宣告按鈕上適用於 click 事件的接聽程式，以及適用於選取功能的指標和筆劃事件。

    如需如何實作筆劃選取項目的完整範例，請參閱[畫筆和手寫筆互動](pen-and-stylus-interactions.md)中的[傳入輸入以進行進階處理](pen-and-stylus-interactions.md#passthrough)。
```    CSharp
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

3.  最後，在新增筆劃選取項目支援之後，我們會在 \[剪下\]、\[複製\] 和 \[貼上\] 按鈕的 click 事件處理常式中實作剪貼簿功能。

    針對剪下功能，我們會先在 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) 的 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 上呼叫 [**CopySelectedToClipboard**](https://msdn.microsoft.com/library/windows/apps/br244232)。

    接著呼叫 [**DeleteSelected**](https://msdn.microsoft.com/library/windows/apps/br244233) 來移除筆墨畫布上的筆劃。

    最後，從選取項目畫布中刪除所有選取筆劃。
```    CSharp
private void btnCut_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
        ClearSelection();
    }
```
```    CSharp
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

    For copy, we simply call [**CopySelectedToClipboard**](https://msdn.microsoft.com/library/windows/apps/br244232) on the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) of the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011).
```    CSharp
private void btnCopy_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
    }
```

    For paste, we call [**CanPasteFromClipboard**](https://msdn.microsoft.com/library/windows/apps/br208495) to ensure that the content on the clipboard can be pasted to the ink canvas.

    If so, we call [**PasteFromClipboard**](https://msdn.microsoft.com/library/windows/apps/br208503) to insert the clipboard ink strokes into the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) of the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011), which then renders the strokes to the ink canvas.
```    CSharp
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

## 相關文章

* [畫筆和手寫筆互動](pen-and-stylus-interactions.md)

**範例**
* [筆跡範例](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [簡單的筆跡範例](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [複雜的筆跡範例](http://go.microsoft.com/fwlink/p/?LinkID=620314)
 

 







<!--HONumber=Jun16_HO4-->


