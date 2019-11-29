---
Description: 建置通用 Windows 平台 (UWP) app，支援來自畫筆和手寫筆裝置的自訂互動，包括適用於自然書寫與繪圖體驗的數位筆跡。
title: UWP app 中的手寫筆互動與 Windows Ink
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen interactions and Windows Ink in UWP apps
template: detail.hbs
keywords: Windows Ink, Windows 筆跡, DirectInk, InkPresenter, InkCanvas, 手寫辨識, 使用者互動, 輸入
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 031ac1ebc8d164c99240969ce77813f36a311858
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258294"
---
# <a name="pen-interactions-and-windows-ink-in-uwp-apps"></a>UWP app 中的手寫筆互動與 Windows Ink

![Surface Pen](images/ink/hero-small.png)  
*Surface 手寫筆* (可在 [Microsoft 網上商店](https://www.microsoft.com/p/surface-pen/8zl5c82qmg6b)購買)。

## <a name="overview"></a>概觀

最佳化您的通用 Windows 平台 (UWP) app，讓手寫筆輸入能夠為您的使用者提供標準[**指標裝置**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.PointerDevice)功能和最好的 Windows Ink 體驗。

> [!NOTE]
> 本主題著重在 Windows Ink 平台。 如需了解一般指標輸入處理 (類似於滑鼠、觸控及觸控板)，請參閱[處理指標輸入](handle-pointer-input.md)。

| 影片 |   |
| --- | --- |
| <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Ink-in-Your-UWP-App/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Events/Ignite/2016/BRK2060/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> |
| *在 UWP 應用程式中使用筆墨* | *使用 Windows 畫筆和筆跡來建立更吸引人的企業應用程式* |

Windows Ink 平台搭配手寫筆裝置之後，使用者就可以自然的方式建立數位手寫筆記、繪圖以及註解。 此平台支援擷取數位板輸入做為筆墨資料、產生筆墨資料、管理筆墨資料、在輸出裝置上將筆墨資料轉譯為筆劃，以及透過手寫辨識將筆墨轉換為文字。

除了在使用者書寫或繪圖時抓取畫筆的基本位置和移動，您的 app 也可以追蹤並收集整個筆劃中所使用的壓力變動量。 此資訊以及適用於筆尖形狀、大小及旋轉、筆墨色彩和用途 (一般筆墨、清除、反白顯示及選取) 的設定，可讓您提供非常類似在紙上使用筆、鉛筆或筆刷書寫或繪圖的使用者經驗。

> [!NOTE]
> 您的 app 也可以支援來自其他指標型裝置的筆墨輸入，包括觸控數位板和滑鼠裝置。 

筆跡平台的彈性非常大。 根據您的需求，它是專為支援各種不同層級的功能而設計。

如需 Windows Ink UX 指導方針，請參閱[筆跡控制項](../controls-and-patterns/inking-controls.md)。

## <a name="components-of-the-windows-ink-platform"></a>Windows Ink 平台的元件

| Component | 描述 |
| --- | --- |
| [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) | XAML UI 平臺控制項，預設會接收並顯示畫筆的所有輸入做為筆墨筆劃或抹除筆劃。<br/>如需如何使用 InkCanvas 的詳細資訊，請參閱[將 Windows Ink 筆觸辨識為文字](convert-ink-to-text.md)與[儲存和抓取 Windows Ink 筆觸資料](save-and-load-ink.md)。 |
| [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) | 程式碼後置物件，連同 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 控制項 (透過 [**InkCanvas.InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 屬性所公開) 進行具現化。 這個物件提供 **InkCanvas** 公開的所有預設筆跡功能，以及一組完整的 API 來進行其他自訂和個人化。<br/>如需如何使用 InkPresenter 的詳細資訊，請參閱[將 Windows Ink 筆觸辨識為文字](convert-ink-to-text.md)與[儲存和抓取 Windows Ink 筆觸資料](save-and-load-ink.md)。 |
| [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) | XAML UI 平臺控制項，其中包含可自訂且可擴充的按鈕集合，可在相關聯的[**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)中啟用筆墨相關功能。<br/>如需如何使用 InkToolbar 的詳細資訊，請參閱[將 InkToolbar 新增至通用 Windows 平台 (UWP) 手寫筆跡應用程式](ink-toolbar.md)。 |
| [**IInkD2DRenderer**](https://docs.microsoft.com/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer) | 可讓筆墨筆劃轉譯到通用 Windows app 的指定 Direct2D 裝置內容，而不是預設的 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 控制項。 這樣就能完整自訂筆跡體驗。<br/>如需詳細資訊，請參閱[複雜的筆跡範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)。 |

## <a name="basic-inking-with-inkcanvas"></a>利用 InkCanvas 的基本筆跡功能

若要新增基本筆跡功能，只要將 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) UWP 平台控制項放在您的應用程式中的適當頁面上。

根據預設，[**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 僅支援來自畫筆的筆跡輸入。 輸入是使用色彩及粗細的預設設定 (黑色鋼珠筆，2 點像素粗細) 轉譯為筆墨筆劃，或可視為筆劃橡皮擦 (若輸入是來自橡皮擦筆尖，或使用橡皮擦按鈕修改的筆尖)。

> [!NOTE]
> 如果橡皮擦筆尖或按鈕不存在，InkCanvas 可以設定為將筆尖輸入做為擦去筆劃處理。

在這個範例中，[**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 會重疊背景影像。

> [!NOTE]
> InkCanvas 的預設[**高度**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Height)和[**寬度**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Width)屬性為零，除非它是自動調整其子項目大小之專案的子系，例如[StackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel)或[方格](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid)控制項。

```xaml
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

這系列的影像會顯示這個 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 控制項轉譯畫筆輸入的方式。

| ![含有背景影像的空白 InkCanvas](images/ink_basic_1_small.png) | ![含有筆墨筆劃的 InkCanvas](images/ink_basic_2_small.png) | ![已擦掉一個筆劃的 InkCanvas](images/ink_basic_3_small.png) |
| --- | --- | ---|
| 含有背景影像的空白 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)。 | 含有筆墨筆劃的 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)。 | 已擦掉一個筆畫的 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) (請注意，此功能會擦掉整個筆劃，而不是擦掉部分筆劃)。 |

[  **InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 控制項支援的筆墨功能是由名為 [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) 的程式碼後置物件所提供。

針對基本的筆墨功能，您不需要考慮使用 [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter)。 不過，若要在 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 上自訂和設定筆墨行為，您就必須存取其對應的 **InkPresenter** 物件。

## <a name="basic-customization-with-inkpresenter"></a>使用 InkPresenter 的基本自訂

[  **InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) 物件是利用每個 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 控制項來具現化。

> [!NOTE]
> [  **InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) 不會直接具現化。 而是透過 [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 的 [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 屬性來存取。 

除了提供其對應 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 控制項的所有預設筆墨行為，[**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) 也會提供一組完整的 API 來進行額外的筆劃自訂，並更細部地管理手寫筆輸入 (標準和已修改的)。 這包括筆劃屬性、支援的輸入裝置類型，以及輸入是否是由物件所處理或者會傳遞到應用程式進行處理。

> [!NOTE]
> 標準筆跡輸入 (從手寫筆筆尖或橡皮擦/按鈕) 不會使用次要硬體能供性 (例如手寫筆筒狀按鈕、滑鼠右鍵或類似的機制) 進行修改。 

根據預設，筆跡功能只支援手寫筆輸入。 我們會在此處設定[**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)，將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃。 我們也會設定一些初始筆墨筆劃屬性，以用來將筆劃轉譯到 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)。

若要啟用滑鼠和觸控筆跡，請將 [**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) 屬性的 [**InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter) 設定為您想要的 [**CoreInputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.core.coreinputdevicetypes) 值的組合。

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
}
```

您可以動態設定筆墨筆劃屬性，以適應使用者的喜好設定或 app 需求。

我們將在此處讓使用者可從筆墨色彩清單中進行選擇。

```xaml
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

```csharp
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

這些影像說明 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 如何處理和自訂畫筆輸入。

| ![含有預設黑色筆墨筆劃的 InkCanvas](images/ink-basic-custom-1-small.png) | ![含有使用者選取的紅色筆墨筆劃的 InkCanvas](images/ink-basic-custom-2-small.png) |
| --- | --- |
| 含有預設黑色筆墨筆劃的 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) | 含有使用者選取的紅色筆墨筆劃的 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) | 

若要提供筆墨和擦掉之後的功能 (例如選取筆劃)，您的 app 必須針對 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 識別出未經處理即可傳入的特定輸入，讓您的 app 來處理。

## <a name="pass-through-input-for-advanced-processing"></a>傳入輸入以進行進階處理

根據預設，[**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 會將所有輸入處理為筆墨筆劃或擦去筆劃，包括由次要硬體能供性 (例如筆身按鈕、滑鼠右鍵按鈕等) 修改的輸入。 但是，使用這些次要能供性時，使用者通常會預期一些額外的功能或已修改的行為。

在某些情況下，您可能也需要公開適用於畫筆的額外功能，而不需次要能供性 (通常不會與筆尖相關聯的功能)、其他輸入裝置類型或額外功能，或者根據應用程式 UI 中的使用者選取項目進行的某些類型修改行為。

若要支援這一點，可設定 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 來讓特定的輸入保持未處理狀態。 這個未處理的輸入接著會傳入您的 app 進行處理。

### <a name="example---use-unprocessed-input-to-implement-stroke-selection"></a>範例 - 使用未處理的輸入實作筆觸選取 

Windows Ink 平台不提供需要修改輸入 (例如筆觸選取項目) 動作的內建支援。 若要支援像這樣的功能，您必須在您的應用程式中提供自訂解決方案。 

下列程式碼範例 (所有的程式碼都位於 MainPage.xaml 和 MainPage.xaml.cs 檔案) 會逐步解說如何在使用畫筆筆身按鈕 (或滑鼠右鍵按鈕) 修改輸入時啟用筆劃選項的方法。

1.  首先，在 MainPage.xaml 中設定 UI。

    我們將在此處新增畫布 (位於 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 下方) 來繪製選取筆劃。 使用個別層級來繪製選取筆劃，讓 **InkCanvas** 及其內容保持原貌。

    ![含有基礎選取項目畫布的空白 InkCanvas](images/ink-unprocessed-1-small.png)

      ```xaml
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

      ```csharp
        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;
      ```

3.  接下來，我們會設定 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter)，將來自畫筆和滑鼠的輸入資料解譯為筆墨筆劃，並設定一些初始的筆墨筆劃屬性，以用來將筆劃轉譯到 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)。

    最重要的是，我們會使用 [InkPresenter**的**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputprocessingconfiguration)InputProcessingConfiguration[](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 屬性來表示 app 應該處理所有修改的輸入。 修改的輸入是藉由指派InkInputRightDragAction.LeaveUnprocessed[**的**InputProcessingConfiguration.RightDragAction](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkInputRightDragAction) 值來指定。 當設定此值時，[InkPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 會通過並抵達 [InkUnprocessedInput](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput) 類別 - 可讓您處理的一系列指標事件。

    我們會針對未處理的 [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed) 、[**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved) 和 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased) 事件指派接聽程式，這些事件是透過 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 來傳入。 所有的選取功能都是在這些事件的處理常式中實作的。

    最後，會針對 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) 的 [**StrokeStarted**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased) 和 [**StrokesErased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 事件指派接聽程式 。 如果開始新的筆劃或擦掉了現有的筆劃，我們就會使用這些事件的處理常式來清除 UI 選取項目。

    ![含有預設黑色筆墨筆劃的 InkCanvas](images/ink-unprocessed-2-small.png)

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

4.  接著，會針對未處理的 [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed) 、[**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved) 和 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased) 事件定義處理常式，這些事件是透過 [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) 來傳入。

    所有的選取功能都是在這些處理常式中實作的，包括套索筆劃和週框。

    ![選取套索](images/ink-unprocessed-3-small.png)

      ```csharp
        // Handle unprocessed pointer events from modified input.
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

      ```csharp
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

6.  最後，會針對 [**StrokeStarted**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) 和 [**StrokesErased**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased) InkPresenter 事件定義處理常式。

    每當偵測到新的筆劃時，這兩者只需要呼叫相同的清理函式來清除目前的選取範圍。

      ```csharp
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

## <a name="custom-ink-rendering"></a>轉譯自訂的筆跡

根據預設，筆墨輸入是在低延遲背景執行緒上處理，並在進行中轉譯，或在其繪製期間轉譯為「濕潤」狀態。 完成筆劃 (拿起畫筆或手指，或是放開滑鼠按鈕) 時，即會在 UI 執行緒上處理該筆劃，並以「烘乾」狀態轉譯到 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 層級 (在應用程式內容上方，並取代濕潤的筆墨)。

您可以透過「自訂烘乾」濕潤的筆墨筆劃來覆寫此預設行為並完全控制筆跡體驗。 雖然預設行為通常針對大部分的應用程式都已足夠，在某些案例下仍然可能會需要自訂烘乾，包括：
- 更有效率的管理大型或複雜的筆墨筆劃集合
- 在大型的筆跡畫布上支援的更有效率的平移和縮放
- 間隔筆跡和其他物件，例如形狀或文字，同時維持 Z 的順序 
- 烘乾並同步將筆跡轉換成 DirectX 形狀 (例如，點陣化的直線或形狀，再整合到應用程式內容而非分離的 **InkCanvas** 層)。

自訂烘乾必須要有 [**IInkD2DRenderer**](https://docs.microsoft.com/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer) 物件才能管理筆墨輸入，並將它轉譯到通用 Windows app 的 Direct2D 裝置內容，而不是預設的 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 控制項。

藉由呼叫 [**ActivateCustomDrying**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.activatecustomdrying) (在載入 [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 之前)，app 會建立 [**InkSynchronizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkSynchronizer) 物件，來自訂如何將筆墨筆劃以烘乾狀態轉譯到 [**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 或 [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource)。 

[  **SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) 和 [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource) 都會為您的應用程式提供 DirectX 共用表面，可在其中繪製並寫入您應用程式的內容，但 VSIS 提供比螢幕更大的虛擬表面，可進行效能更高的平移和縮放。 因為這些表面的視覺更新都是與 XAML UI 執行緒同步的，當筆跡轉譯到其中之一時，濕潤的筆跡便可同時從 InkCanvas 移除。 

您也可以將烘乾筆跡自訂成 [SwapChainPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel)，但此舉將無法保證與 UI 執行緒的同步，並且當筆跡轉譯到您的 SwapChainPanel 和筆跡從 InkCanvas 移除時將會產生延遲。

如需這項功能的完整範例，請參閱[複雜的筆跡範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)。

> [!NOTE]
> 自訂乾燥與 [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)  
> 如果您的 app 使用自訂的乾燥實作覆寫 [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) 的預設筆跡轉譯行為，InkToolbar 就不會再有轉譯的筆墨筆觸，InkToolbar 的內建清除命令也無法如預期般運作。 若要提供清除功能，就必須處理所有指標事件、對每一個筆劃執行點擊測試，並且覆寫內建的「清除所有筆跡」命令。

## <a name="other-articles-in-this-section"></a>本節中的其他文章

| 主題 | 描述 |
| --- | --- |
| [辨識筆跡筆劃](convert-ink-to-text.md) | 使用手寫辨識，將筆墨筆劃轉換為文字，或者使用自訂辨識轉換為形狀。 |
| [儲存和抓取筆跡筆劃](save-and-load-ink.md) | 使用內嵌的筆跡序列化格式 (ISF) 中繼資料，在圖形交換格式 (GIF) 檔案中儲存筆墨筆劃資料。 |
| [將 InkToolbar 新增至 UWP 墨蹟應用程式](ink-toolbar.md) | 將預設 InkToolbar 新增至通用 Windows 平台 (UWP) 手寫筆跡應用程式、將自訂的畫筆按鈕新增至 InkToolbar，以及將自訂的畫筆按鈕繫結到自訂的畫筆定義。 |

## <a name="related-articles"></a>相關文章

* [開始使用：在 UWP 應用程式中支援筆墨](../../get-started/ink-walkthrough.md)
* [處理指標輸入](handle-pointer-input.md)
* [識別輸入裝置](identify-input-devices.md)

**Api**

* [**Windows. 輸入**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input)
* [**Windows. UI. Input. 筆跡**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking)
* [**Windows. UI. Input。** ](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.Core)

**範例**
* [開始使用教學課程：在 UWP 應用程式中支援筆墨](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
* [Simple 筆墨範例（C#/C++）](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
* [複雜筆跡範例（C++）](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
* [筆墨範例（JavaScript）](https://go.microsoft.com/fwlink/p/?LinkID=620308)
* [著色書籍範例](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [家族附注範例](https://github.com/Microsoft/Windows-appsample-familynotes)
* [基本輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [低延遲輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [使用者互動模式範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) \(英文\)

**封存範例**
* [輸入：裝置功能範例](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [輸入： XAML 使用者輸入事件範例](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [XAML 捲軸、移動流覽和縮放範例](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [輸入：使用 GestureRecognizer 的手勢和操作](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
