---
author: Karl-Bridge-Microsoft
Description: Use handwriting recognition and ink analysis to recognize Windows Ink strokes as text and shapes.
title: 將 Windows Ink 筆劃辨識為文字和圖案
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keywords: Windows Ink, Windows 筆跡, DirectInk, InkPresenter, InkCanvas, 手寫辨識, 使用者互動, 輸入
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 83142b0a3b24e25f8e7a922d800262f505cd8cc2
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5868536"
---
# <a name="recognize-windows-ink-strokes-as-text-and-shapes"></a>將 Windows Ink 筆劃辨識為文字和圖案

使用 Windows Ink 內建的辨識功能，將筆墨筆劃轉換為文字與形狀。

> **重要 API**：[**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)、[**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


## <a name="free-form-recognition-with-ink-analysis"></a>使用筆墨分析辨識自由格式

在這裡，我們將示範如何使用 Windows Ink 分析引擎 ([Windows.UI.Input.Inking.Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)) 分類、分析及辨識 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上的一組自由格式筆劃做為文字或圖案。 (除了文字和圖案辨識，筆墨分析可用來辨識文件結構、項目符號清單和一般繪圖。)

> [!NOTE]
> 如需基本、單行純文字的案例，例如表單輸入，請參閱[限制式手寫辨識](#constrained-handwriting-recognition)。

在此範例中，當使用者按一下按鈕表示完成繪圖時，系統便會開始辨識。

**請從[筆跡分析範例 (基本)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip) 下載此範例。**

1.  首先，先設定 UI (MainPage.xaml)。 

    UI 包含 [Recognize] 按鈕、[**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 和標準 [**Canvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas)。 按下 [Recognize] 按鈕時，會分析筆跡畫布上的所有筆墨筆劃（如果有辨識），而且在標準畫布上繪製相對應的圖案和文字。 然後從筆跡畫布刪除原始筆墨筆劃。
```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" 
                    Orientation="Horizontal" 
                    Grid.Row="0">
            <TextBlock x:Name="Header" 
                        Text="Basic ink analysis sample" 
                        Style="{ThemeResource HeaderTextBlockStyle}" 
                        Margin="10,0,0,0" />
            <Button x:Name="recognize" 
                    Content="Recognize" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid x:Name="drawingCanvas" Grid.Row="1">

            <!-- The canvas where we render the replacement text and shapes. -->
            <Canvas x:Name="recognitionCanvas" />
            <!-- The canvas for ink input. -->
            <InkCanvas x:Name="inkCanvas" />

        </Grid>
    </Grid>
```
2. 在 UI 的後置程式碼檔案中 (MainPage.xaml.cs)，新增筆墨功能和筆墨分析功能所需的命名空間類型參考：
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)
    - [Windows.UI.Input.Inking.Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)
    - [Windows.UI.Xaml.Shapes](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes)

3. 然後我們會指定我們的全域變數：
```csharp
    InkAnalyzer inkAnalyzer = new InkAnalyzer();
    IReadOnlyList<InkStroke> inkStrokes = null;
    InkAnalysisResult inkAnalysisResults = null;
```
4.  然後，設定一些基本的筆墨輸入行為：
    - [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 已設定為可將來自手寫筆、滑鼠和觸控的輸入資料解譯為筆墨筆劃 ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019))。 
    - 在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上，使用指定的 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) 來轉譯筆墨筆劃。 
    - 同時也會在 [Recognize] 按鈕上宣告適用於 click 事件的接聽程式。
```csharp
/// <summary>
/// Initialize the UI page.
/// </summary>
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen | 
            Windows.UI.Core.CoreInputDeviceTypes.Touch;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Listen for button click to initiate recognition.
        recognize.Click += RecognizeStrokes_Click;
    }
```
5.  在這個範例中，我們使用 [Recognize] 按鈕的 click 事件處理常式來執行筆墨分析。
    - 首先，在 [**InkCanvas.InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) 的 [**StrokeContainer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter.StrokeContainer) 上呼叫 [**GetStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkstrokecontainer.GetStrokes)，取得所有目前筆墨筆劃的集合。
    - 如果筆墨筆劃存在，在 InkAnalyzer 的 [**AddDataForStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer#Windows_UI_Input_Inking_Analysis_InkAnalyzer_AddDataForStrokes_Windows_Foundation_Collections_IIterable_Windows_UI_Input_Inking_InkStroke__) 呼叫中傳遞它們。
    - 我們會一直嘗試辨識繪圖與文字，但您可以使用 [**SetStrokeDataKind**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) 方法來指定您只對文字感興趣 (包括文件結構和項目符號清單)，或只對繪圖感興趣 (包括形狀辨識)。
    - 呼叫 [**AnalyzeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) 起始筆墨分析，並取得 [**InkAnalysisResult**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult)。
    - 如果 [**Status**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) 傳回狀態 **Updated**，呼叫 [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) 和 [**InkAnalysisNodeKind.InkDrawing**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) 的 [**FindNodes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes)。
    - 逐一查看這兩組的節點類型，並在辨識畫布（在筆跡畫布下方）上繪製對應文字或圖案。
    - 最後，從 InkAnalyzer 刪除辨識的節點，從筆跡畫布刪除對應的筆墨筆劃。
```csharp
/// <summary>
/// The "Analyze" button click handler.
/// Ink recognition is performed here.
/// </summary>
/// <param name="sender">Source of the click event</param>
/// <param name="e">Event args for the button click routed event</param>
private async void RecognizeStrokes_Click(object sender, RoutedEventArgs e)
    {
        inkStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (inkStrokes.Count > 0)
        {
            inkAnalyzer.AddDataForStrokes(inkStrokes);

            // In this example, we try to recognizing both 
            // writing and drawing, so the platform default 
            // of "InkAnalysisStrokeKind.Auto" is used.
            // If you're only interested in a specific type of recognition,
            // such as writing or drawing, you can constrain recognition 
            // using the SetStrokDataKind method as follows:
            // foreach (var stroke in strokesText)
            // {
            //     analyzerText.SetStrokeDataKind(
            //      stroke.Id, InkAnalysisStrokeKind.Writing);
            // }
            // This can improve both efficiency and recognition results.
            inkAnalysisResults = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (inkAnalysisResults.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes = 
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Draw primary recognized text on recognitionCanvas 
                // (for this example, we ignore alternatives), and delete 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    // Draw a TextBlock object on the recognitionCanvas.
                    DrawText(node.RecognizedText, node.BoundingRect);

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke = 
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();

                // Find all strokes that are recognized as a drawing and 
                // create a corresponding ink analysis InkDrawing node.
                var inkdrawingNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkDrawing);
                // Iterate through each InkDrawing node.
                // Draw recognized shapes on recognitionCanvas and
                // delete ink analysis data and recognized strokes.
                foreach (InkAnalysisInkDrawing node in inkdrawingNodes)
                {
                    if (node.DrawingKind == InkAnalysisDrawingKind.Drawing)
                    {
                        // Catch and process unsupported shapes (lines and so on) here.
                    }
                    // Process generalized shapes here (ellipses and polygons).
                    else
                    {
                        // Draw an Ellipse object on the recognitionCanvas (circle is a specialized ellipse).
                        if (node.DrawingKind == InkAnalysisDrawingKind.Circle || node.DrawingKind == InkAnalysisDrawingKind.Ellipse)
                        {
                            DrawEllipse(node);
                        }
                        // Draw a Polygon object on the recognitionCanvas.
                        else
                        {
                            DrawPolygon(node);
                        }
                        foreach (var strokeId in node.GetStrokeIds())
                        {
                            var stroke = inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                            stroke.Selected = true;
                        }
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
    }
```
6. 以下是在我們的辨識畫布上繪製 TextBlock 的功能。 我們使用相關的筆墨筆劃的週框筆跡畫布上設定的位置和字型大小的 TextBlock。
```csharp
/// <summary>
/// Draw ink recognition text string on the recognitionCanvas.
/// </summary>
/// <param name="recognizedText">The string returned by text recognition.</param>
/// <param name="boundingRect">The bounding rect of the original ink writing.</param>
private void DrawText(string recognizedText, Rect boundingRect)
{
    TextBlock text = new TextBlock();
    Canvas.SetTop(text, boundingRect.Top);
    Canvas.SetLeft(text, boundingRect.Left);

    text.Text = recognizedText;
    text.FontSize = boundingRect.Height;

    recognitionCanvas.Children.Add(text);
}
```
7. 以下是在我們的辨識畫布上繪製省略符號和多邊形的功能。 我們使用相關的筆墨筆劃的週框筆跡畫布上設定的位置和字型大小的圖形。
```csharp
    // Draw an ellipse on the recognitionCanvas.
    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        var points = shape.Points;
        Ellipse ellipse = new Ellipse();

        ellipse.Width = shape.BoundingRect.Width;
        ellipse.Height = shape.BoundingRect.Height;

        Canvas.SetTop(ellipse, shape.BoundingRect.Top);
        Canvas.SetLeft(ellipse, shape.BoundingRect.Left);

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        ellipse.Stroke = brush;
        ellipse.StrokeThickness = 2;
        recognitionCanvas.Children.Add(ellipse);
    }

    // Draw a polygon on the recognitionCanvas.
    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        List<Point> points = new List<Point>(shape.Points);
        Polygon polygon = new Polygon();

        foreach (Point point in points)
        {
            polygon.Points.Add(point);
        }

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        polygon.Stroke = brush;
        polygon.StrokeThickness = 2;
        recognitionCanvas.Children.Add(polygon);
    }
```

以下是這個範例的實際運作：

| 分析之前 | 分析之後 |
| --- | --- |
| ![分析之前](images/ink/ink-analysis-raw2-small.png) | ![分析之後](images/ink/ink-analysis-analyzed2-small.png) |

---

## <a name="constrained-handwriting-recognition"></a>限制式手寫辨識

在上一節 ([使用筆墨分析辨識自由格式](#free-form-recognition-with-ink-analysis)) 中，我們示範如何使用[筆墨分析 API](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis) 來分析與辨識 InkCanvas 區域內的任意筆墨筆劃。

在本節中，我們會示範如何使用 Windows Ink 手寫辨識引擎 (而非筆墨分析)，將 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上的一組筆劃轉換成文字 (根據安裝的預設語言套件)。

> [!NOTE]
> 本節中顯示的基本手寫辨識最適合單行的文字輸入案例，例如表單輸入。 如需包括文件結構、清單項目、圖形和繪圖（加上文字辨識）分析和解譯更豐富的辨識功能案例，請參閱上一節：[使用筆墨分析辨識自由格式](#free-form-recognition-with-ink-analysis)。

在此範例中，當使用者按一下按鈕表示完成書寫時，系統便會開始辨識。

**請從[筆跡手寫辨識範例](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)下載此範例。**

1.  一開始先設定 UI。

    UI 包含一個 \[辨識\] 按鈕 ([**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535))，以及一個可顯示辨識結果的區域。    
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

2. 例如，您必須先新增我們的筆墨功能所需的命名空間類型參考：
    - [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input)
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)


3.  然後設定一些基本的筆墨輸入行為。

    [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 已設定為可將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃 ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019))。 在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上，使用指定的 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) 來轉譯筆墨筆劃。 同時也會在 [辨識] 按鈕上宣告適用於 click 事件的接聽程式。
```csharp
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

4.  最後是執行基本的手寫辨識。 在這個範例中，我們使用 [辨識] 按鈕的 click 事件處理常式來執行手寫辨識。

    [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 會儲存 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 物件中的所有筆墨筆劃。 筆劃是透過 **InkPresenter** 的 [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) 屬性來公開，並使用 [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499) 方法來擷取。
```csharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    An [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) is created to manage the handwriting recognition process.

```csharp
// Create a manager for the InkRecognizer object
    // used in handwriting recognition.
    InkRecognizerContainer inkRecognizerContainer =
        new InkRecognizerContainer();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).

```csharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).

```csharp
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

```csharp
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

## <a name="international-recognition"></a>國際性辨識

內建於 Windows Ink 平台的手寫辨識包括由 Windows 所支援廣泛地區設定和語言的一部分。

請參閱 [**InkRecognizer.Name**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkrecognizer.name.aspx) 屬性主題以取得由 [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478) 支援的語言清單。

您的應用程式可以查詢這組已安裝的手寫辨識引擎並使用其中一個，或者讓使用者選擇他們偏好的語言。

**注意：** 使用者可以看到一份已安裝的語言，方式為前往**設定]-&gt;時間與語言**。 **\[語言\]** 下方會列出已安裝的語言。

若要安裝新的語言套件並針對該語言啟用手寫辨識：

1.  移至 **\[設定\] &gt; \[時間與語言\] &gt; \[地區與語言\]**。
2.  選取 **\[新增語言\]**。
3.  從清單中選取語言，然後選擇地區版本。 語言現在會列於 **\[地區及語言\]** 頁面上。
4.  按一下語言，然後選取 **\[選項\]**。
5.  在 **[語言選項]** 頁面上，下載 **[手寫辨識引擎]** (他們也可以在此處下載完整的語言套件、語音辨識引擎和鍵盤配置)。

 

我們將在此處示範如何使用手寫辨識引擎，根據選取的辨識器來解譯 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上的一組筆墨筆劃。

當使用者完成書寫時，按一下某個按鈕即會初始辨識功能。

1.  一開始先設定 UI。

    UI 包含一個 [辨識] 按鈕、一個列出所有已安裝之手寫辨識器的下拉式方塊、[**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)，以及一個可顯示辨識結果的區域。
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

    [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 已設定為可將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃 ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019))。 在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上，使用指定的 [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) 來轉譯筆墨筆劃。

    我們會呼叫 `InitializeRecognizerList` 函式來填入辨識器下拉式方塊，其中包含已安裝的手寫辨識器清單。

    我們也會在 [辨識] 按鈕上宣告適用於 click 事件的接聽程式，以及在辨識器下拉式方塊上宣告選取範圍變更事件。
```csharp
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

    [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) 是建立來管理手寫辨識程序。 使用此物件來呼叫 [**GetRecognizers**](https://msdn.microsoft.com/library/windows/apps/br208480)，並抓取已安裝的辨識器清單來填入辨識器下拉式方塊。
```csharp
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
```csharp
// Handle recognizer change.
    private void comboInstalledRecognizers_SelectionChanged(
        object sender, SelectionChangedEventArgs e)
    {
        inkRecognizerContainer.SetDefaultRecognizer(
            (InkRecognizer)comboInstalledRecognizers.SelectedItem);
    }
```

5.  最後，會根據選取的手寫辨識器執行手寫辨識。 在這個範例中，我們使用 [辨識] 按鈕的 click 事件處理常式來執行手寫辨識。

    [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 會儲存 [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) 物件中的所有筆墨筆劃。 筆劃是透過 **InkPresenter** 的 [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) 屬性來公開，並使用 [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499) 方法來擷取。
```csharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes =
        inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).

```csharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).

```csharp
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
    
```csharp
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

## <a name="dynamic-recognition"></a>動態辨識

雖然前兩個範例需要使用者按下按鈕才會開始辨識，但您也可以使用與基本計時功能配對的筆劃輸入執行動態辨識。

在這個範例中，我們將使用與先前國際性辨識範例相同的 UI 和筆劃設定。

1. 這些全域物件 ([InkAnalyzer](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer)、[InkStroke](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstroke)、[InkAnalysisResult](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult)、[DispatcherTimer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer)) 用於我們整個應用程式中。    
```csharp
    // Stroke recognition globals.
    InkAnalyzer inkAnalyzer;
    DispatcherTimer recoTimer;
```

2.  我們針對兩個 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 筆劃事件 ([**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024) 和 [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702))新增了接聽程式，並設定含有一秒 [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244250) 間隔的基本計時器 ([**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244256))，來取代初始辨識的按鈕。    
```csharp
    public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for stroke events on the InkPresenter to 
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by 
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected += inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            inkCanvas_StrokeStarted;

        inkAnalyzer = new InkAnalyzer();

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = TimeSpan.FromSeconds(1);
        recoTimer.Tick += recoTimer_TickAsync;
    }
```

3.  接著我們會定義我們在第一個步驟中宣告的 InkPresenter 事件 (我們也會覆寫 [**OnNavigatingFrom**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) 頁面事件來管理我們的計時器)。

    - [**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024)  
    將筆墨筆劃 ([**AddDataForStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.adddataforstrokes)) 新增至 InkAnalyzer，然後當使用者停止筆墨輸入 (提起他們的畫筆或手指，或是放開滑鼠按鈕) 時啟動辨識計時器。 一秒之後若無任何筆墨輸入，即會初始辨識。  

        使用 [**SetStrokeDataKind**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) 方法來指定您只對文字感興趣 (包括文件結構和項目符號清單)，或只對繪圖感興趣 (包括形狀辨識)。

    - [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)  
    如果新的筆劃在下一個計時器 tick 事件之前開始，請停止計時器，因為新的筆劃很可能是延續自單一手寫輸入項。
```csharp
    // Handler for the InkPresenter StrokeStarted event.
    // Don't perform analysis while a stroke is in progress.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }    
    // Handler for the InkPresenter StrokesCollected event.
    // Stop the timer and add the collected strokes to the InkAnalyzer.
    // Start the recognition timer when the user stops inking (by 
    // lifting their pen or finger, or releasing the mouse button).
    // If ink input is not detected after one second, initiate recognition.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Stop();
        // If you're only interested in a specific type of recognition,
        // such as writing or drawing, you can constrain recognition 
        // using the SetStrokDataKind method, which can improve both 
        // efficiency and recognition results.
        // In this example, "InkAnalysisStrokeKind.Writing" is used.
        foreach (var stroke in args.Strokes)
        {
            inkAnalyzer.AddDataForStroke(stroke);
            inkAnalyzer.SetStrokeDataKind(stroke.Id, InkAnalysisStrokeKind.Writing);
        }
        recoTimer.Start();
    }    
    // Override the Page OnNavigatingFrom event handler to 
    // stop our timer if user leaves page.
    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        recoTimer.Stop();
    } 
```

4.  最後執行手寫辨識。 在這個範例中，我們使用 [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244256) 的 [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244250) 事件處理常式來初始手寫辨識功能。
    - 呼叫 [**AnalyzeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) 起始筆墨分析，並取得 [**InkAnalysisResult**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult)。
    - 如果 [**Status**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) 傳回的狀態為 \[已更新\]****，則會為 [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) 的節點類型呼叫 [**FindNodes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes)。
    - 逐一查看節點，並顯示所辨識的文字。
    - 最後，從 InkAnalyzer 刪除辨識的節點，從筆跡畫布刪除對應的筆墨筆劃。
```csharp
    private async void recoTimer_TickAsync(object sender, object e)
    {
        recoTimer.Stop();
        if (!inkAnalyzer.IsAnalyzing)
        {
            InkAnalysisResult result = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (result.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Display the primary recognized text (for this example, 
                // we ignore alternatives), and then delete the 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    string recognizedText = node.RecognizedText;
                    // Display the recognition candidates.
                    recognitionResult.Text = recognizedText;

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke =
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
        else
        {
            // Ink analyzer is busy. Wait a while and try again.
            recoTimer.Start();
        }
    }
```

## <a name="related-articles"></a>相關文章

* [畫筆和手寫筆互動](pen-and-stylus-interactions.md)

**主題範例**
* [筆跡分析範例 (基本) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [筆跡手寫辨識範例 (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)

**其他範例**
* [簡單的筆跡範例 (C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [複雜的筆跡範例 (C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [筆跡範例 (JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [入門教學課程：UWP 應用程式中的支援筆跡](https://aka.ms/appsample-ink)
* [著色本範例](https://aka.ms/cpubsample-coloringbook)
* [家庭記事本範例](https://aka.ms/cpubsample-familynotessample)


 
