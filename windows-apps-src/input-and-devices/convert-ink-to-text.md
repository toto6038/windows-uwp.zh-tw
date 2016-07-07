---
author: Karl-Bridge-Microsoft
Description: "使用手寫辨識，將筆墨筆劃轉換為文字，或者使用自訂辨識轉換為形狀。"
title: "將 Windows Ink 筆觸辨識為文字"
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, handwriting recognition
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: ac07ca76df874c670e7e38698e89de6620f73cc4

---

# 將 Windows Ink 筆觸辨識為文字

使用手寫辨識，將筆墨筆劃轉換為文字，或者使用自訂辨識轉換為形狀。

**重要 API**

-   [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


手寫辨識內建於 Windows Ink 平台，並支援一組廣泛的地區設定和語言。

針對此處的所有範例，新增筆墨功能所需的命名空間參考。 這包括 "Windows.UI.Input.Inking"。

## 基本的手寫辨識


我們將在此處示範如何使用與預設安裝的語言套件相關聯的手寫辨識引擎，來解譯 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上的一組筆劃。

當使用者完成書寫時，按一下某個按鈕即會初始辨識功能。

1.  一開始先設定 UI。

    UI 包含一個 \[辨識\] 按鈕 (InkCanvas)，以及一個可顯示辨識結果的區域。    
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel"
                    Orientation="Horizontal"
                    Grid.Row="0">
            <TextBlock x:Name="Header"
                       Text="Basic ink recognition sample"
                       Style="{ThemeResource HeaderTextBlockStyle}"
                       Margin="10,0,0,0" />
            <Button x:Name="recognize"
                    Content="Recognize"
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas"
                       Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult"
                       Grid.Row="1"
                       Margin="50,0,10,0"/>
        </Grid>
    </Grid>
```

2.  然後設定一些基本的筆墨輸入行為。

    
            [
              **InkPresenter**
            ](https://msdn.microsoft.com/library/windows/apps/dn899081) 已設定為可將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃 ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019))。 在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上，使用指定的 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) 來轉譯筆墨筆劃。 同時也會在 \[辨識\] 按鈕上宣告適用於 click 事件的接聽程式。
```    CSharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Listen for button click to initiate recognition.
        recognize.Click += Recognize_Click;
    }
```

3.  最後是執行基本的手寫辨識。 在這個範例中，我們使用 \[辨識\] 按鈕的 click 事件處理常式來執行手寫辨識。

    
            [
              **InkPresenter**
            ](https://msdn.microsoft.com/library/windows/apps/dn899081) 會儲存 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 物件中的所有筆墨筆劃。 筆劃是透過 **InkPresenter** 的 [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) 屬性來公開，並使用 [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499) 方法來擷取。
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    An [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) is created to manage the handwriting recognition process.
```    CSharp
// Create a manager for the InkRecognizer object
    // used in handwriting recognition.
    InkRecognizerContainer inkRecognizerContainer =
        new InkRecognizerContainer();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (var result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates = result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the click handler example, in full.
```    CSharp
// Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // Create a manager for the InkRecognizer object
            // used in handwriting recognition.
            InkRecognizerContainer inkRecognizerContainer =
                new InkRecognizerContainer();

            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (var result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates = result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog = new Windows.UI.Popups.MessageDialog("You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
```

## 國際性辨識


Windows 支援的完整語言子集可用於手寫辨識。

如需 [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478) 支援的語言清單，請參閱 [**InkRecognizer.Name**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkrecognizer.name.aspx) 屬性。

您的 app 可以查詢這組已安裝的手寫辨識引擎，並使用其中一個，或者讓使用者選擇他們偏好的語言。

**注意**  
使用者可以移至 \[設定\] - \[時間與語言\]，來查看已安裝的語言清單。 \[語言\] 下方會列出已安裝的語言。

若要安裝新的語言套件並針對該語言啟用手寫辨識：

1.  移至 \[設定\]  \[時間與語言\]  \[地區與語言\]。
2.  選取 \[新增語言\]。
3.  從清單中選取語言，然後選擇地區版本。 語言現在會列於 \[地區及語言\] 頁面上。
4.  按一下語言，然後選取 \[選項\]。
5.  在 \[語言選項\] 頁面上，下載 \[手寫辨識引擎\] (他們也可以在此處下載完整的語言套件、語音辨識引擎和鍵盤配置)。

 

我們將在此處示範如何使用手寫辨識引擎，根據選取的辨識器來解譯 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上的一組筆墨筆劃。

當使用者完成書寫時，按一下某個按鈕即會初始辨識功能。

1.  一開始先設定 UI。

    UI 包含一個 \[辨識\] 按鈕、一個列出所有已安裝之手寫辨識器的下拉式方塊、InkCanvas，以及一個可顯示辨識結果的區域。
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel"
                    Orientation="Horizontal"
                    Grid.Row="0">
            <TextBlock x:Name="Header"
                       Text="Advanced international ink recognition sample"
                       Style="{ThemeResource HeaderTextBlockStyle}"
                       Margin="10,0,0,0" />
            <ComboBox x:Name="comboInstalledRecognizers"
                     Margin="50,0,10,0">
                <ComboBox.ItemTemplate>
                    <DataTemplate>
                        <StackPanel Orientation="Horizontal">
                            <TextBlock Text="{Binding Name}" />
                        </StackPanel>
                    </DataTemplate>
                </ComboBox.ItemTemplate>
            </ComboBox>
            <Button x:Name="buttonRecognize"
                    Content="Recognize"
                    IsEnabled="False"
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas"
                       Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult"
                       Grid.Row="1"
                       Margin="50,0,10,0"/>
        </Grid>
    </Grid>
```

2.  然後設定一些基本的筆墨輸入行為。

    
            [
              **InkPresenter**
            ](https://msdn.microsoft.com/library/windows/apps/dn899081) 已設定為可將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃 ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019))。 在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上，使用指定的 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) 來轉譯筆墨筆劃。

    我們會呼叫 `InitializeRecognizerList` 函式來填入辨識器下拉式方塊，其中包含已安裝的手寫辨識器清單。

    我們也會在 \[辨識\] 按鈕上宣告適用於 click 事件的接聽程式，以及在辨識器下拉式方塊上宣告選取範圍變更事件。
```    CSharp
 public MainPage()
     {
         this.InitializeComponent();

         // Set supported inking device types.
         inkCanvas.InkPresenter.InputDeviceTypes =
             Windows.UI.Core.CoreInputDeviceTypes.Mouse |
             Windows.UI.Core.CoreInputDeviceTypes.Pen;

         // Set initial ink stroke attributes.
         InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
         drawingAttributes.Color = Windows.UI.Colors.Black;
         drawingAttributes.IgnorePressure = false;
         drawingAttributes.FitToCurve = true;
         inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

         // Populate the recognizer combo box with installed recognizers.
         InitializeRecognizerList();

         // Listen for combo box selection.
         comboInstalledRecognizers.SelectionChanged +=
             comboInstalledRecognizers_SelectionChanged;

         // Listen for button click to initiate recognition.
         buttonRecognize.Click += Recognize_Click;
     }
```

3.  我們會在辨識器下拉式方塊中填入已安裝的手寫辨識器清單。

    
            [
              **InkRecognizerContainer**
            ](https://msdn.microsoft.com/library/windows/apps/br208479) 是建立來管理手寫辨識程序。 使用此物件來呼叫 [**GetRecognizers**](https://msdn.microsoft.com/library/windows/apps/br208480)，並抓取已安裝的辨識器清單來填入辨識器下拉式方塊。
```    CSharp
// Populate the recognizer combo box with installed recognizers.
    private void InitializeRecognizerList()
    {
        // Create a manager for the handwriting recognition process.
        inkRecognizerContainer = new InkRecognizerContainer();
        // Retrieve the collection of installed handwriting recognizers.
        IReadOnlyList<InkRecognizer> installedRecognizers =
            inkRecognizerContainer.GetRecognizers();
        // inkRecognizerContainer is null if a recognition engine is not available.
        if (!(inkRecognizerContainer == null))
        {
            comboInstalledRecognizers.ItemsSource = installedRecognizers;
            buttonRecognize.IsEnabled = true;
        }
    }
```

4.  如果辨識器下拉式方塊選取項目變更，請更新手寫辨識器。

    使用 [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479)，根據從辨識器下拉式方塊中選取的辨識器來呼叫 [**SetDefaultRecognizer**](https://msdn.microsoft.com/library/windows/apps/hh920328)。
```    CSharp
// Handle recognizer change.
    private void comboInstalledRecognizers_SelectionChanged(
        object sender, SelectionChangedEventArgs e)
    {
        inkRecognizerContainer.SetDefaultRecognizer(
            (InkRecognizer)comboInstalledRecognizers.SelectedItem);
    }
```

5.  最後，會根據選取的手寫辨識器執行手寫辨識。 在這個範例中，我們使用 \[辨識\] 按鈕的 click 事件處理常式來執行手寫辨識。

    
            [
              **InkPresenter**
            ](https://msdn.microsoft.com/library/windows/apps/dn899081) 會儲存 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 物件中的所有筆墨筆劃。 筆劃是透過 **InkPresenter** 的 [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) 屬性來公開，並使用 [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499) 方法來擷取。
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes =
        inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates =
            result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the click handler example, in full.
```    CSharp
// Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes =
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (InkRecognitionResult result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates =
                            result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog =
                    new Windows.UI.Popups.MessageDialog(
                        "You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
```

## 動態手寫辨識


先前的兩個範例要求使用者按下按鈕來開始辨識。 您的 app 也可以使用與基本計時函式配對的筆劃輸入來執行動態辨識。

在這個範例中，我們將使用與先前國際性辨識範例相同的 UI 和筆劃設定。

1.  如同先前範例，[**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 已設定為會將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃 ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019))，而且會在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上，使用指定的 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) 來轉譯筆墨筆劃。

    我們針對兩個 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 筆劃事件 ([**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024) 和 [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702))新增了接聽程式，並設定含有一秒 [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) 間隔的基本計時器 ([**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250))，來取代初始辨識的按鈕。    
```    CSharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Populate the recognizer combo box with installed recognizers.
        InitializeRecognizerList();

        // Listen for combo box selection.
        comboInstalledRecognizers.SelectionChanged +=
            comboInstalledRecognizers_SelectionChanged;

        // Listen for stroke events on the InkPresenter to
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected +=
            inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            inkCanvas_StrokeStarted;

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = new TimeSpan(0, 0, 1);
        recoTimer.Tick += recoTimer_Tick;
    }

    // Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }    
```

2.  以下是我們在第一個步驟中針對這三個事件新增的處理常式。

    [**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024)  
    當使用者提起他們的畫筆或手指，或是放開滑鼠按鈕來停止筆墨輸入時，即會啟動辨識計時器。 一秒之後若無任何筆墨輸入，即會初始辨識。

    [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)  
    如果新的筆劃在下一個計時器 tick 事件之前開始，請停止計時器，因為新的筆劃很可能是延續自單一手寫輸入項。

    [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256)  
    一秒之後若無任何筆墨輸入，請呼叫辨識函式。
```    CSharp
// Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }
```

3.  最後，會根據選取的手寫辨識器執行手寫辨識。 在這個範例中，我們使用 [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250) 的 [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) 事件處理常式來初始手寫辨識功能。

    
            [
              **InkPresenter**
            ](https://msdn.microsoft.com/library/windows/apps/dn899081) 會儲存 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 物件中的所有筆墨筆劃。 筆劃是透過 **InkPresenter** 的 [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) 屬性來公開，並使用 [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499) 方法來擷取。
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates = result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the recognition function, in full.
```    CSharp
// Respond to timer Tick and initiate recognition.
    private async void Recognize_Tick()
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (InkRecognitionResult result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates = result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog = new Windows.UI.Popups.MessageDialog("You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }

        // Stop the dynamic recognition timer.
        recoTimer.Stop();
    }
```

## 相關文章

* [畫筆和手寫筆互動](pen-and-stylus-interactions.md)

**範例**
* [筆跡範例](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [簡單的筆跡範例](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [複雜的筆跡範例](http://go.microsoft.com/fwlink/p/?LinkID=620314)
 

 



<!--HONumber=Jun16_HO5-->


