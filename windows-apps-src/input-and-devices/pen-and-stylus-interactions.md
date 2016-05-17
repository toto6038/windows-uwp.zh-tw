---
author: Karl-Bridge-Microsoft
Description: 建置通用 Windows 平台 (UWP) app，支援來自畫筆和手寫筆裝置的自訂互動，包括適用於自然書寫與繪圖體驗的數位筆墨。
title: UWP app 中的畫筆和手寫筆互動
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen and stylus interactions in UWP apps
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas
---

# UWP app 中的畫筆和手寫筆互動

最佳化您的通用 Windows 平台 (UWP)，讓手寫筆輸入能夠為您的使用者提供標準[**指標裝置**](https://msdn.microsoft.com/library/windows/apps/br225633)功能和最好的 Windows Ink 體驗。

> 注意：本主題著重在 Windows Ink 平台。 請參閱[處理指標輸入](handle-pointer-input.md)以了解一般指標輸入處理 (類似於滑鼠、觸控及觸控板)。

![觸控板](images/input-patterns/input-pen.jpg)

**重要 API**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)
-   [**Windows.UI.Input.Inking.Core**](https://msdn.microsoft.com/library/windows/apps/dn958452)

Windows 筆跡平台搭配手寫筆裝置之後，使用者就可以自然的方式建立數位手寫筆記、繪圖以及註解。 此平台支援擷取數位板輸入做為筆墨資料、產生筆墨資料、管理筆墨資料、在輸出裝置上將筆墨資料轉譯為筆劃，以及透過手寫辨識將筆墨轉換為文字。

除了在使用者書寫或繪圖時抓取畫筆的基本位置和移動，您的 app 也可以追蹤並收集整個筆劃中所使用的壓力變動量。 此資訊以及適用於筆尖形狀、大小及旋轉、筆墨色彩和用途 (一般筆墨、清除、反白顯示及選取) 的設定，可讓您提供非常類似在紙上使用筆、鉛筆或筆刷書寫或繪圖的使用者經驗。

**注意**：您的 app 也可以支援來自其他指標型裝置的筆跡輸入，包括觸控數位板和滑鼠裝置。 

筆跡平台的彈性非常大。 根據您的需求，它是專為支援各種不同層級的功能而設計。

有三個適用於筆跡平台的元件：

-   [
            **InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) - XAML UI 平台控制項，此控制項預設會接收來自畫筆的所有輸入，並顯示為筆墨筆劃或擦去筆劃。

-   [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) - 程式碼後置物件，連同 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控制項 (透過 [**InkCanvas.InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 屬性所公開) 進行具現化。 這個物件提供 **InkCanvas** 公開的所有預設筆墨功能，以及一組完整的 API 來進行其他自訂和個人化。

-   [
            **IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) - 可讓筆墨筆劃轉譯到通用 Windows app 的指定 Direct2D 裝置內容，而不是預設的 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控制項。 這樣就能完整自訂筆墨體驗。

## <span id="inkcanvas"></span><span id="INKCANVAS"></span>利用 InkCanvas 的基本筆墨功能


針對基本筆墨功能，只需在頁面上的任何地方放置 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 即可。

[
            **InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 僅支援來自畫筆的筆墨輸入。 輸入是使用適用於色彩和粗細的預設設定轉譯為筆墨筆劃，或可視為筆墨橡皮擦 (若輸入是來自使用擦掉按鈕進行修改的橡皮擦頂端或筆尖時)。

在這個範例中，[**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 會重疊背景影像。

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />            
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

這系列的影像會顯示這個 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控制項轉譯畫筆輸入的方式。

| ![含有背景影像的空白 InkCanvas](images/ink_basic_1_small.png) | ![含有筆墨筆劃的 InkCanvas](images/ink_basic_2_small.png) | ![已擦掉一個筆劃的 InkCanvas](images/ink_basic_3_small.png) |
| --- | --- | ---|
| 含有背景影像的空白 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)。 | 含有筆墨筆劃的 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)。 | 已擦掉一個筆畫的 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (請注意，此功能會擦掉整個筆劃，而不是擦掉部分筆劃)。 |

[
            **InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控制項支援的筆墨功能是由名為 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) 的程式碼後置物件所提供

針對基本的筆墨功能，您不需要考慮使用 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011)。 不過，若要在 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 上自訂和設定筆墨行為，您就必須存取其對應的 **InkPresenter** 物件。

## <span id="inkpresenter"></span><span id="INKPRESENTER"></span>使用 InkPresenter 的基本自訂


[
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) 物件是利用每個 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控制項來具現化。

除了提供其對應 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控制項的所有預設筆墨行為，[**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) 也會提供一組完整的 API 來進行額外的筆劃自訂。 這包括筆劃屬性、支援的輸入裝置類型，以及輸入是否是由物件所處理或者會傳遞到 app。

**注意**  
[
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) 不會直接具現化。 而是透過 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 的 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 屬性來存取

 

我們會在此處設定[**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)，將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃。 我們也會設定一些初始筆墨筆劃屬性，以用來將筆劃轉譯到 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)

```CSharp
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
}
```

您可以動態設定筆墨筆劃屬性，以適應使用者的喜好設定或 app 需求。

我們將在此處讓使用者可從筆墨色彩清單中進行選擇。

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink customization sample" 
                   VerticalAlignment="Center"
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />
        <TextBlock Text="Color:"
                   Style="{StaticResource SubheaderTextBlockStyle}"
                   VerticalAlignment="Center"
                   Margin="50,0,10,0"/>
        <ComboBox x:Name="PenColor"
                  VerticalAlignment="Center"
                  SelectedIndex="0"
                  SelectionChanged="OnPenColorChanged">
            <ComboBoxItem Content="Black"/>
            <ComboBoxItem Content="Red"/>
        </ComboBox>
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

接著處理選取色彩的變更，並據此更新筆墨筆劃屬性。

```CSharp
// Update ink stroke color for new strokes.
private void OnPenColorChanged(object sender, SelectionChangedEventArgs e)
{
    if (inkCanvas != null)
    {
        InkDrawingAttributes drawingAttributes = 
            inkCanvas.InkPresenter.CopyDefaultDrawingAttributes();

        string value = ((ComboBoxItem)PenColor.SelectedItem).Content.ToString();

        switch (value)
        {
            case "Black":
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
            case "Red":
                drawingAttributes.Color = Windows.UI.Colors.Red;
                break;
            default:
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
        };

        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
    }
}
```

這些影像說明 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 如何處理和自訂畫筆輸入

| ![含有預設黑色筆墨筆劃的 InkCanvas](images/ink-basic-custom-1-small.png) | ![含有使用者選取的紅色筆墨筆劃的 InkCanvas](images/ink-basic-custom-2-small.png) |
| --- | -- |
| 含有預設黑色筆墨筆劃的 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) | 含有使用者選取的紅色筆墨筆劃的 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) |

 

若要提供筆墨和擦掉之後的功能 (例如選取筆劃)，您的 app 必須針對 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 識別出未經處理即可傳入的特定輸入，讓您的 app 來處理。

## <span id="passthrough"></span><span id="PASSTHROUGH"></span>傳入輸入以進行進階處理


根據預設，[**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 會將所有輸入處理為筆墨筆劃或擦去筆劃。 這包括透過次要硬體能供性所修改的輸入，例如畫筆筆身按鈕、滑鼠右鍵按鈕或類似按鈕。

使用這些次要能供性時，使用者通常會預期一些額外的功能或已修改的行為。

在某些情況下，您可能需要公開適用於畫筆的基本筆墨功能，而不需次要能供性 (通常不會與筆尖相關聯的功能)、其他輸入裝置類型或額外功能，或者根據 App UI 中的使用者選取項目來修改的行為。

若要支援這一點，可設定 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 來讓特定的輸入保持未處理狀態。 這個未處理的輸入接著會傳入您的 app 進行處理。

下列程式碼範例會逐步解說如何在使用畫筆筆身按鈕 (或滑鼠右鍵按鈕) 修改輸入時啟用筆劃選項的方法。

在這個範例中，我們使用 MainPage.xaml 和 MainPage.xaml.cs 檔案來裝載所有程式碼。

1.  首先，在 MainPage.xaml 中設定 UI。

    我們將在此處新增畫布 (位於 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 下方) 來繪製選取筆劃。 使用個別層級來繪製選取筆劃，讓 **InkCanvas** 及其內容保持原貌。

    ![含有基礎選取項目畫布的空白 InkCanvas](images/ink-unprocessed-1-small.png)
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Advanced ink customization sample" 
                       VerticalAlignment="Center"
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
        </StackPanel>
        <Grid Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
        </Grid>
    </Grid>
```

2.  在 MainPage.xaml.cs 中，我們會宣告數個全域變數，持續參考各個層面的 UI 選取項目。 具體而言，選取套索筆劃和週框會將選取的筆劃反白顯示。
```    CSharp
// Stroke selection tool.
    private Polyline lasso;
    // Stroke selection area.
    private Rect boundingRect;
```

3.  接下來，我們會設定 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)，將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃，並設定一些初始的筆墨筆劃屬性，以用來將筆劃轉譯到 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)

    最重要的是，我們會使用 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 的 [**InputProcessingConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948764) 屬性來表示 app 應該處理所有修改的輸入。 修改的輸入是藉由指派 [**InkInputRightDragAction.LeaveUnprocessed**](https://msdn.microsoft.com/library/windows/apps/dn948760) 的 **InputProcessingConfiguration.RightDragAction** 值來指定

    接著，會針對未處理的 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712) 、[**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711) 和 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) 事件指派接聽程式，這些事件是透過 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 來傳入。 所有的選取功能都是在這些事件的處理常式中實作的。

    最後，會針對 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 的 [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) 和 [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767) 事件指派接聽程式 。 如果開始新的筆劃或擦掉了現有的筆劃，我們就會使用這些事件的處理常式來清除 UI 選取項目。

    ![含有預設黑色筆墨筆劃的 InkCanvas](images/ink-unprocessed-2-small.png)
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

4.  接著，會針對未處理的 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712) 、[**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711) 和 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) 事件定義處理常式，這些事件是透過 [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) 來傳入

    所有的選取功能都是在這些處理常式中實作的，包括套索筆劃和週框。

    ![選取套索](images/ink-unprocessed-3-small.png)
```    CSharp
// Handle unprocessed pointer events from modifed input.
    // The input is used to provide selection functionality.
    // Selection UI is drawn on a canvas under the InkCanvas.
    private void UnprocessedInput_PointerPressed(
        InkUnprocessedInput sender, PointerEventArgs args)
    {
        // Initialize a selection lasso.
        lasso = new Polyline()
        {
            Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
            StrokeThickness = 1,
            StrokeDashArray = new DoubleCollection() { 5, 2 },
        };

        lasso.Points.Add(args.CurrentPoint.RawPosition);

        selectionCanvas.Children.Add(lasso);
    }

    private void UnprocessedInput_PointerMoved(
        InkUnprocessedInput sender, PointerEventArgs args)
    {
        // Add a point to the lasso Polyline object.
        lasso.Points.Add(args.CurrentPoint.RawPosition);
    }

    private void UnprocessedInput_PointerReleased(
        InkUnprocessedInput sender, PointerEventArgs args)
    {
        // Add the final point to the Polyline object and 
        // select strokes within the lasso area.
        // Draw a bounding box on the selection canvas 
        // around the selected ink strokes.
        lasso.Points.Add(args.CurrentPoint.RawPosition);

        boundingRect = 
            inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                lasso.Points);

        DrawBoundingRect();
    }
```

5.  為了推斷出 PointerReleased 事件處理常式，我們清除了所有內容的選取項目層級 (套索筆劃)，然後在套索區域所圍繞的筆墨筆劃四周繪製單一週框。

    ![選取週框](images/ink-unprocessed-4-small.png)
```    CSharp
// Draw a bounding rectangle, on the selection canvas, encompassing 
    // all ink strokes within the lasso area.
    private void DrawBoundingRect()
    {
        // Clear all existing content from the selection canvas.
        selectionCanvas.Children.Clear();

        // Draw a bounding rectangle only if there are ink strokes 
        // within the lasso area.
        if (!((boundingRect.Width == 0) || 
            (boundingRect.Height == 0) || 
            boundingRect.IsEmpty))
        {
            var rectangle = new Rectangle()
            {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                StrokeThickness = 1,
                StrokeDashArray = new DoubleCollection() { 5, 2 },
                Width = boundingRect.Width,
                Height = boundingRect.Height
            };

            Canvas.SetLeft(rectangle, boundingRect.X);
            Canvas.SetTop(rectangle, boundingRect.Y);

            selectionCanvas.Children.Add(rectangle);
        }
    }
```

6.  最後，會針對 [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) 和 [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767) InkPresenter 事件定義處理常式。

    每當偵測到新的筆劃時，這兩者只需要呼叫相同的清理函式來清除目前的選取範圍。
```    CSharp
// Handle new ink or erase strokes to clean up selection UI.
    private void StrokeInput_StrokeStarted(
        InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
    {
        ClearSelection();
    }

    private void InkPresenter_StrokesErased(
        InkPresenter sender, InkStrokesErasedEventArgs args)
    {
        ClearSelection();
    }
```

7.  以下這個函式會在開始新的筆劃或清除現有的筆劃時，從選取的畫布中移除所有 UI 選取項目。
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

## <span id="iinkd2drenderer"></span><span id="IINKD2DRENDERER"></span>轉譯自訂的筆墨


根據預設，筆墨輸入是在低延遲背景執行緒上處理，並在其繪製期間轉譯為「濕潤」狀態。 完成筆劃 (拿起畫筆或手指，或是放開滑鼠按鈕) 時，即會在 UI 執行緒上處理該筆劃，並以「烘乾」狀態轉譯到 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 層級 (在應用程式內容上方，並取代濕潤的筆墨)。

筆跡平台可讓您覆寫這個行為，並以自訂烘乾筆墨輸入完整自訂筆墨經驗。

自訂烘乾必須要有 [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) 物件才能管理筆墨輸入，並將它轉譯到通用 Windows app 的 Direct2D 裝置內容，而不是預設的 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 控制項。

藉由呼叫 [**ActivateCustomDrying**](https://msdn.microsoft.com/library/windows/apps/dn922012) (在載入 [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) 之前)，app 會建立 [**InkSynchronizer**](https://msdn.microsoft.com/library/windows/apps/dn903979) 物件，來自訂如何將筆墨筆劃以烘乾狀態轉譯到 [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) 或 [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)。 例如，筆墨筆劃會被點陣化並整合到應用程式內容，而不是做為個別的 **InkCanvas** 層。

如需這項功能的完整範例，請參閱[複雜的筆跡範例](http://go.microsoft.com/fwlink/p/?LinkID=620314)


## 本節中的其他文章 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[辨識筆墨筆劃](convert-ink-to-text.md)</p></td>
<td align="left"><p>使用手寫辨識，將筆墨筆劃轉換為文字，或者使用自訂辨識轉換為形狀。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[儲存和擷取筆墨筆劃](save-and-load-ink.md)</p></td>
<td align="left"><p>使用內嵌的筆跡序列化格式 (ISF) 中繼資料，在圖形交換格式 (GIF) 檔案中儲存筆墨筆劃資料。</p></td>
</tr>
</tbody>
</table>

 


## <span id="related_topics"></span>相關文章


* [處理指標輸入](handle-pointer-input.md)
* [識別輸入裝置](identify-input-devices.md)

**範例**
* [筆跡範例](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [簡單的筆跡範例](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [複雜的筆跡範例](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [基本輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延遲輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [使用者互動模式範例](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦點視覺效果範例](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**封存範例**
* [輸入：裝置功能範例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [輸入：XAML 使用者輸入事件範例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [XAML 捲動、移動瀏覽和縮放範例](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [輸入：使用 GestureRecognizer 處理手勢與操作](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 

 






<!--HONumber=May16_HO2-->


