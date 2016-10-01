---
author: Karl-Bridge-Microsoft
Description: "將預設 InkToolbar 新增至通用 Windows 平台 (UWP) 手寫筆跡應用程式、將自訂的畫筆按鈕新增至 InkToolbar，以及將自訂的畫筆按鈕繫結到自訂的畫筆定義。"
title: "將 InkToolbar 新增至通用 Windows 平台 (UWP) 手寫筆跡應用程式"
label: Add an InkToolbar to a Universal Windows Platform (UWP) inking app
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, InkToolbar, Universal Windows Platform, UWP
translationtype: Human Translation
ms.sourcegitcommit: 743ce43be6220d96d7e9bc295ab72bdec4905edc
ms.openlocfilehash: 4c50c8ded127b2261df901d9da3210ff397b00ca

---

# 將 InkToolbar 新增至通用 Windows 平台 (UWP) 手寫筆跡應用程式

有兩個不同的控制項可協助在通用 Windows 平台 (UWP) 應用程式中使用手寫筆跡：[**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) 和 [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)。

[**InkCanvas**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) 控制項提供基本的 Windows 筆跡功能。 使用此控制項將手寫筆輸入轉譯為筆墨筆劃 (使用色彩與粗細的預設設定) 或擦去筆劃。

> 如需 InkCanvas 實作的詳細資訊，請參閱 [UWP app 中的畫筆和手寫筆互動](pen-and-stylus-interactions.md)。

做為完全透明的重疊，InkCanvas 不會提供任何用於設定筆墨筆劃屬性的內建 UI。 如果您想要變更預設的手寫筆跡體驗、讓使用者設定筆墨筆劃的屬性，以及支援其他自訂的手寫筆跡功能，您有兩個選項︰

- 在程式碼後置中，使用繫結至 InkCanvas 的基礎 [**InkPresenter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) 物件。

  InkPresenter API 支援大量的手寫筆跡體驗自訂項目。 如需更多詳細資料，請參閱 [UWP app 中的畫筆和手寫筆互動](pen-and-stylus-interactions.md)。

- 將 [**InkToolbar**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) 繫結至 InkCanvas。 根據預設，InkToolbar 會提供啟用筆跡功能與設定筆跡屬性 (例如筆觸大小、筆跡色彩及筆尖形狀) 的基本 UI。

  我們會在本主題中討論 InkToolbar。

## 重要 API

  -   [**InkCanvas 類別**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx)
  -   [**InkToolbar 類別**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)
  -   [**InkPresenter 類別**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx)
  -   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


## 預設 InkToolbar

根據預設，InkToolbar 包含用於繪圖、清除、反白顯示，以及顯示尺規的按鈕。 根據功能而定，其他設定和命令 (例如筆跡色彩、筆劃粗細、清除所有筆跡) 將會在飛出視窗中提供。

![InkToolbar](.\images\ink\ink-tools-invoked-toolbar-small.png)  
*預設 Windows Ink 工具列*

若要新增基本的預設 InkToolbar：
1. 在 MainPage.xaml 中，針對手寫筆跡表面宣告容器物件 (如這個範例中，我們使用 [格線] 控制項)。
2. 將 InkCanvas 物件宣告為容器的子系。 (InkCanvas 大小是繼承自容器。)
3. 宣告 InkToolbar 並使用 TargetInkCanvas 屬性將它繫結至 InkCanvas。
  請確定 InkToolbar 是在 InkCanvas 之後宣告。 如果不是，InkCanvas 重疊會將 InkToolbar 轉譯為無法存取。

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
        <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
    </Grid>
</Grid>
```

## 基本自訂項目

在本節中，我們會討論一些基本的 Windows Ink 工具列自訂案例。

### 指定選取的按鈕  
![在初始化時選取的鉛筆按鈕](.\images\ink\ink-tools-default-toolbar.png)  
*包含在初始化時所選取鉛筆按鈕的 Windows Ink 工具列*

根據預設，當您的 App 啟動時或是工具列初始化時，會選取第一個 (或最左邊) 按鈕。 在預設 Windows Ink 工具列中，這是鋼珠筆按鈕。

因為架構定義了內建按鈕的順序，根據預設，第一個按鈕可能不是您想要啟用的畫筆或工具。

您可以覆寫這個預設行為，並在工具列上指定選取的按鈕。

這個範例中，我們透過選取的鉛筆按鈕和啟用的鉛筆 (而不是鋼珠筆) 將預設工具列初始化。

1. 針對上一個範例的 InkCanvas 和 InkToolbar 使用 XAML 宣告。
2. 在程式碼後置中，針對 [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) 物件的 [Loaded](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) 事件設定處理常式。

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loaded += inkToolbar_Loaded;
  }
  ```

3. 在 [Loaded](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) 事件的處理常式中：
  1. 取得內建 [InkToolbarPencilButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) 的參考。

    在 [GetToolButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.gettoolbutton.aspx) 方法中傳遞 [InkToolbarTool.Pencil](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbartool.aspx) 物件會傳回 [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) 的 [InkToolbarToolButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbartoolbutton.aspx) 物件。

  2. 將 [ActiveTool](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.activetool.aspx) 設為在上一個步驟中傳回的物件。

```CSharp
/// <summary>
/// Handle the Loaded event of the InkToolbar.
/// By default, the active tool is set to the first tool on the toolbar.
/// Here, we set the active tool to the pencil button.
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void inkToolbar_Loaded(object sender, RoutedEventArgs e)
{
    InkToolbarToolButton pencilButton = inkToolbar.GetToolButton(InkToolbarTool.Pencil);
    inkToolbar.ActiveTool = pencilButton;
}
```

### 指定內建的按鈕

![初始化時包含的特定按鈕](.\images\ink\ink-tools-specific.png)  
*初始化時包含的特定按鈕*

如前所述，Windows Ink 工具列會包含一組預設的內建按鈕。 這些按鈕會以下列順序顯示 (由左至右)：

- [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx)
- [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx)
- [InkToolbarHighlighterButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarhighlighterbutton.aspx)
- [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)
- [InkToolbarRulerButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarrulerbutton.aspx)

這個範例中，我們僅會初始化內建鋼珠筆、鉛筆以及橡皮擦按鈕。

您可以使用 XAML 或程式碼後置這麼做。

**XAML**

修改第一個範例中 InkCanvas 和 InkToolbar 的 XAML 宣告。
- 新增 [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) 屬性，並將其值設定為 "[None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)"。 這會清除預設的內建按鈕集合。
- 新增您的 App 所需的特定 InkToolbar 按鈕。 現在，我們僅新增 [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx)、[InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) 和 [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)。
> [!NOTE]
> 按鈕會依照架構所定義的順序 (而非此處指定的順序) 新增至工具列。

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
        <!-- Clear the default InkToolbar buttons by setting InitialControls to None. -->
        <!-- Set the active tool to the pencil button. -->
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
                    VerticalAlignment="Top"
                    TargetInkCanvas="{x:Bind inkCanvas}"
                    InitialControls="None">
            <!--
             Add only the ballpoint pen, pencil, and eraser.
             Note that the buttons are added to the toolbar in the order
             defined by the framework, not the order we specify here.
            -->
            <InkToolbarEraserButton />
            <InkToolbarBallpointPenButton />
            <InkToolbarPencilButton/>
        </InkToolbar>
    </Grid>
</Grid>
```

**程式碼後置**
1. 針對第一個範例的 InkCanvas 和 InkToolbar 使用 XAML 宣告。

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
          <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
      </Grid>
  </Grid>
  ```

2. 在程式碼後置中，針對 [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) 物件的 [Loading](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.loading.aspx) 事件設定處理常式。

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loading += inkToolbar_Loading;
  }
  ```

3. 將 [InitialControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) 設定為 "[None](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)"。
4. 建立應用程式所需按鈕的物件參考。 現在，我們僅新增 [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx)、[InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx) 和 [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)。
  > [!NOTE]
  > 按鈕會依照架構所定義的順序 (而非此處指定的順序) 新增至工具列。

5. 將按鈕[新增](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.dependencyobjectcollection.add.aspx)到 InkToolbar。

  ```csharp
  /// <summary>
  /// Handles the Loading event of the InkToolbar.
  /// Here, we identify the buttons to include on the InkToolbar.
  /// </summary>
  /// <param name="sender">The InkToolbar</param>
  /// <param name="args">The InkToolbar event data.
  /// If there is no event data, this parameter is null</param>
  private void inkToolbar_Loading(FrameworkElement sender, object args)
  {
      // Clear all built-in buttons from the InkToolbar.
      inkToolbar.InitialControls = InkToolbarInitialControls.None;

      // Add only the ballpoint pen, pencil, and eraser.
      // Note that the buttons are added to the toolbar in the order
      // defined by the framework, not the order we specify here.
      InkToolbarBallpointPenButton ballpoint = new InkToolbarBallpointPenButton();
      InkToolbarPencilButton pencil = new InkToolbarPencilButton();
      InkToolbarEraserButton eraser = new InkToolbarEraserButton();
      inkToolbar.Children.Add(eraser);
      inkToolbar.Children.Add(ballpoint);
      inkToolbar.Children.Add(pencil);
  }
  ```

<!--
### Support touch input
By default, the InkToolbar supports both pen and mouse input, you have to enable support for touch input.
-->

## 自訂按鈕和手寫筆跡功能

您可以自訂和延伸透過 InkToolbar 所提供按鈕 (以及相關聯的手寫筆跡功能) 的集合。

InkToolbar 包含兩個不同群組的按鈕類型︰

1. 包含內建繪圖、清除以及醒目提示按鈕的「工具」按鈕群組。 在此處新增自訂的畫筆與工具。
> **注意**&nbsp;&nbsp;功能選項互斥。

2. 包含內建尺規按鈕的「切換」按鈕群組。 在此處新增自訂的切換。
> **注意**&nbsp;&nbsp;功能不會互斥，因此可以和其他使用中的工具同時使用。

根據您的應用程式和所需的手寫筆跡功能，您可以將下列任何按鈕 (繫結到您自訂的筆跡功能) 新增到 InkToolbar：

- 自訂畫筆 – 適用於由主機應用程式定義的筆跡調色盤和筆尖屬性 (例如圖形、旋轉和大小) 的畫筆。
- 自訂工具 – 由主機應用程式定義的非畫筆工具。
- 自訂切換 – 將應用程式定義功能的狀態設定為開啟或關閉。 開啟時，功能會與使用中的工具搭配使用。

> **注意**&nbsp;&nbsp;您無法變更內建按鈕的顯示順序。 預設的顯示順序是︰鋼珠筆、鉛筆、螢光筆、橡皮擦和尺規。 自訂畫筆會附加到最後一個預設畫筆，自訂工具按鈕會新增到最後一個畫筆按鈕與橡皮擦按鈕之間，而自訂切換按鈕會新增到尺規按鈕之後。 (自訂按鈕會以指定的順序新增。)

### 自訂畫筆

![自訂書法筆按鈕](.\images\ink\ink-tools-custompen.png)  
*自訂書法筆按鈕*

這個範例中，我們會定義具有寬筆尖的自訂畫筆，支援基本的書法筆墨筆劃。 我們也會自訂按鈕飛出視窗上所顯示調色盤中的筆刷集合。

**程式碼後置**

首先，我們換定義我們的自訂畫筆並在程式碼後置中指定繪圖屬性。 稍後我們會從 XAML 參考此自訂畫筆。

1. 在 [方案總管] 中的專案上按一下滑鼠右鍵，然後選取 [加入] &gt; [新增項目]。
2. 在 [Visual C#] -&gt; [程式碼] 底下，新增新的類別檔案，並命名為 CalligraphicPen.cs。
3. 在 Calligraphic.cs 中，使用下列項目取代預設 using 區塊：
```csharp
using System.Numerics;
using Windows.UI;
using Windows.UI.Input.Inking;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
```
4. 指定 CalligraphicPen 類別是衍生自 [InkToolbarCustomPen](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.aspx)。
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```
5. 覆寫 [CreateInkDrawingAttributesCore](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore.aspx) 以指定您自己的筆刷和筆觸大小。
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```
6. 建立 [InkDrawingAttributes](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.aspx) 物件，並設定[筆尖形狀](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentip.aspx)、[筆尖旋轉](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentiptransform.aspx)、[筆觸大小](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.size.aspx)和[筆跡色彩](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.color.aspx)。
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
        InkDrawingAttributes inkDrawingAttributes =
          new InkDrawingAttributes();
        inkDrawingAttributes.PenTip = PenTipShape.Circle;
        inkDrawingAttributes.Size =
          new Windows.Foundation.Size(strokeWidth, strokeWidth * 20);
        SolidColorBrush solidColorBrush = brush as SolidColorBrush;
        if (solidColorBrush != null)
        {
            inkDrawingAttributes.Color = solidColorBrush.Color;
        }
        else
        {
            inkDrawingAttributes.Color = Colors.Black;
        }

        Matrix3x2 matrix = Matrix3x2.CreateRotation(45);
        inkDrawingAttributes.PenTipTransform = matrix;

        return inkDrawingAttributes;
    }
}
```

**XAML**

接下來，我們會在 MainPage.xaml 中新增對自訂畫筆的必要參考。

1. 我們會宣告一個本機頁面資源字典，它會建立對 CalligraphicPen.cs 中所定義自訂畫筆 (`CalligraphicPen`) 的參考，以及自訂畫筆 (`CalligraphicPenPalette`) 支援的[筆刷集合](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.UI.Xaml.Media.BrushCollection.aspx)。
```xaml
<Page.Resources>
    <!-- Add the custom CalligraphicPen to the page resources. -->
    <local:CalligraphicPen x:Key="CalligraphicPen" />
    <!-- Specify the colors for the palette of the custom pen. -->
    <BrushCollection x:Key="CalligraphicPenPalette">
        <SolidColorBrush Color="Blue" />
        <SolidColorBrush Color="Red" />
    </BrushCollection>
</Page.Resources>
```
2. 然後，我們會使用子系 [InkToolbarCustomPenButton](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompenbutton.aspx) 元素新增 InkToolbar。

  自訂畫筆按鈕包含兩個在頁面資源中宣告的靜態資源參考︰`CalligraphicPen` 和 `CalligraphicPenPalette`。

  我們也會為自訂畫筆按鈕 ([SymbolIcon](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.symbolicon.aspx)) 指定筆觸大小滑桿 ([MinStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth.aspx)、[MaxStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth.aspx) 和 [SelectedStrokeWidth](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty.aspx))、所選筆刷 ([SelectedBrushIndex](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex.aspx))，以及圖示的範圍。
```xaml
<Grid Grid.Row="1">
    <InkCanvas x:Name="inkCanvas" />
    <InkToolbar x:Name="inkToolbar"
                VerticalAlignment="Top"
                TargetInkCanvas="{x:Bind inkCanvas}">
        <InkToolbarCustomPenButton
            CustomPen="{StaticResource CalligraphicPen}"
            Palette="{StaticResource CalligraphicPenPalette}"
            MinStrokeWidth="1" MaxStrokeWidth="3" SelectedStrokeWidth="2"
            SelectedBrushIndex ="1">
            <SymbolIcon Symbol="Favorite" />
            <InkToolbarCustomPenButton.ConfigurationContent>
                <InkToolbarPenConfigurationControl />
            </InkToolbarCustomPenButton.ConfigurationContent>
        </InkToolbarCustomPenButton>
    </InkToolbar>
</Grid>
```

<!--
### Custom toggle

Enable touch inking

>**Note**&nbsp;&nbsp;InkToolbar supports pen and mouse input and can be configured to recognize touch input.
-->


## 相關文章

* [畫筆和手寫筆互動](pen-and-stylus-interactions.md)

**範例**
* [筆跡範例](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [簡單的筆跡範例](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [複雜的筆跡範例](http://go.microsoft.com/fwlink/p/?LinkID=620314)



<!--HONumber=Aug16_HO3-->


