---
Description: 使用 AppWindow 類別, 在不同的視窗中查看應用程式的不同部分。
title: 使用 AppWindow 類別來顯示應用程式的次要視窗
ms.date: 07/19/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9b89d9100157cf40266bb983e258aa187f65dc93
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867468"
---
# <a name="show-multiple-views-with-appwindow"></a>使用 AppWindow 顯示多個視圖

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)及其相關的 api 可讓您在次要視窗中顯示您的應用程式內容, 同時在每個視窗中繼續使用相同的 UI 執行緒, 藉此簡化多視窗應用程式的建立。

> [!NOTE]
> AppWindow 目前為預覽狀態。 這表示您可以將使用 AppWindow 的應用程式提交至存放區, 但某些平臺和架構元件已知無法與 AppWindow 搭配使用 (請參閱[限制](/uwp/api/windows.ui.windowmanagement.appwindow#limitations))。

在這裡, 我們會顯示多個視窗的一些案例, 其中`HelloAppWindow`包含名為的範例應用程式。 範例應用程式會示範下列功能:

- 從主頁面取消固定控制項, 並在新視窗中開啟它。
- 在新視窗中開啟頁面的新實例。
- 以程式設計方式調整和定位應用程式中的新視窗。
- 將 ContentDialog 與應用程式中的適當視窗建立關聯。

![使用單一視窗的範例應用程式](images/hello-app-window-single.png)
  
> _使用單一視窗的範例應用程式_

![具有取消停駐色彩選擇器和次要視窗的範例應用程式](images/hello-app-window-multi.png)

> _具有取消停駐色彩選擇器和次要視窗的範例應用程式_

> **重要 API**：[WindowManagement 命名空間](/uwp/api/windows.ui.windowmanagement), [AppWindow 類別](/uwp/api/windows.ui.windowmanagement.appwindow)

## <a name="api-overview"></a>API 概觀

從 Windows 10 版本 1903 (SDK 18362) 開始, 可以使用[WindowManagement](/uwp/api/windows.ui.windowmanagement)命名空間中的[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)類別和其他 api。 如果您的應用程式是以舊版的 Windows 10 為目標, 則必須[使用 ApplicationView 來建立次要視窗](application-view.md)。 WindowManagement Api 仍在開發中, 且具有 API 參考檔中所述的[限制](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)。

以下是一些您用來在 AppWindow 中顯示內容的重要 Api。

### <a name="appwindow"></a>AppWindow

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)類別可以用來在次要視窗中顯示 Windows 執行階段應用程式的一部分。 它在概念上類似于[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview), 但行為和存留期並不相同。 AppWindow 的主要功能是每個實例共用與它們建立所在的相同 UI 處理執行緒 (包括事件發送器), 這會簡化多視窗應用程式。

您只能將 XAML 內容連接到您的 AppWindow, 不支援原生 DirectX 或全像攝影內容。 不過, 您可以顯示裝載 DirectX 內容的 XAML [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) 。

### <a name="windowingenvironment"></a>WindowingEnvironment

[WindowingEnvironment](/uwp/api/windows.ui.windowmanagement.windowingenvironment) API 可讓您瞭解應用程式的呈現環境, 讓您可以視需要調整應用程式。 它會描述環境支援的視窗類型;例如, `Overlapped`如果應用程式是在電腦上執行, 或是`Tiled`應用程式正在 Xbox 上執行, 則為。 它也會提供一組 DisplayRegion 物件, 描述應用程式可能會顯示在邏輯顯示器上的區域。

### <a name="displayregion"></a>DisplayRegion

[DisplayRegion](/uwp/api/windows.ui.windowmanagement.displayregion) API 描述可在邏輯顯示上向使用者顯示視圖的區域;例如, 在桌上型電腦上, 這是完整顯示, 減去工作列的面積。 這不一定是與支援監視器實體顯示區域的1:1 對應。 同一個監視器中可以有多個顯示區域, 如果這些監視器在所有層面都是同質, 則可以設定 DisplayRegion 跨多個監視器。

### <a name="appwindowpresenter"></a>AppWindowPresenter

[AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) API 可讓您輕鬆地將 windows 切換為預先定義的`FullScreen`設定`CompactOverlay`, 例如或。 這些設定可讓使用者在任何支援設定的裝置上擁有一致的體驗。

### <a name="uicontext"></a>UIContext

[UICoNtext](/uwp/api/windows.ui.uicontext)是應用程式視窗或視圖的唯一識別碼。 它會自動建立, 而且您可以使用[UICoNtext](/uwp/api/windows.ui.xaml.uielement.uicontext)屬性來取出 UICoNtext。 XAML 樹狀結構中的每個 UIElement 都具有相同的 UICoNtext。

 UICoNtext 很重要, 因為像是[Window](/uwp/api/Windows.UI.Xaml.Window.Current)和`GetForCurrentView`模式之類的 api 會依賴每個執行緒的單一 ApplicationView/CoreWindow, 以使用單一 XAML 樹狀結構。 當您使用 AppWindow 時, 就不會發生這種情況, 因此您可以改用 UICoNtext 來識別特定的視窗。

### <a name="xamlroot"></a>XamlRoot

[XamlRoot](/uwp/api/windows.ui.xaml.xamlroot)類別會保存 XAML 專案樹狀結構、將它連接到視窗主機物件 (例如, [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)或[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)), 並提供大小和可見度等資訊。 您不會直接建立 XamlRoot 物件。 相反地, 當您將 XAML 元素附加至 AppWindow 時, 會建立一個。 接著, 您可以使用[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot)屬性來取出 XamlRoot。

如需 UICoNtext 和 XamlRoot 的詳細資訊, 請參閱[讓程式碼可在視窗間的主機上進行移植](show-multiple-views.md#make-code-portable-across-windowing-hosts)。

## <a name="show-a-new-window"></a>顯示新視窗

讓我們看一下在新 AppWindow 中顯示內容的步驟。

**若要顯示新視窗**

1. 呼叫靜態[AppWindow TryCreateAsync](/uwp/api/windows.ui.windowmanagement.appwindow.trycreateasync)方法, 以建立新的[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)。

    ```csharp
    AppWindow appWindow = await AppWindow.TryCreateAsync();
    ```

1. 建立視窗內容。

    一般來說, 您會建立 XAML[框架](/uwp/api/Windows.UI.Xaml.Controls.Frame), 然後流覽框架至您已定義應用程式內容的 xaml[頁面](/uwp/api/Windows.UI.Xaml.Controls.Page)。 如需框架和頁面的詳細資訊, 請參閱[兩頁之間的對等導覽](../basics/navigate-between-two-pages.md)。

    ```csharp
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowMainPage));
    ```

    不過, 您可以在 AppWindow 中顯示任何 XAML 內容, 而不只是框架和頁面。 例如, 您可以只顯示單一控制項 (例如[ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker)), 也可以顯示裝載 DirectX 內容的[SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) 。

1. 呼叫[ElementCompositionPreview. SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent)方法, 將 XAML 內容附加至 AppWindow。

    ```csharp
    ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);
    ```

    呼叫這個方法會建立[XamlRoot](/uwp/api/windows.ui.xaml.xamlroot)物件, 並將它設定為指定 UIElement 的[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot)屬性。

    您只能針對每個 AppWindow 實例呼叫這個方法一次。 設定內容之後, 此 AppWindow 實例的 SetAppWindowContent 進一步呼叫將會失敗。 此外, 如果您嘗試傳入 null UIElement 物件來中斷 AppWindow 內容的連線, 呼叫將會失敗。

1. 呼叫[AppWindow. TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync)方法以顯示新視窗。

    ```csharp
    await appWindow.TryShowAsync();
    ```

## <a name="release-resources-when-a-window-is-closed"></a>關閉視窗時釋放資源

您應該一律處理[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow.closed)的事件, 以釋放 XAML 資源 (AppWindow 內容) 和 AppWindow 的參考。

```csharp
appWindow.Closed += delegate
{
    appWindowContentFrame.Content = null;
    appWindow = null;
};
```

## <a name="track-instances-of-appwindow"></a>追蹤 AppWindow 的實例

視您在應用程式中使用多個視窗的方式而定, 您可能會需要追蹤所建立的 AppWindow 實例。 此`HelloAppWindow`範例顯示一些您通常會使用[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)的不同方式。 在這裡, 我們將探討為何應該追蹤這些視窗, 以及如何執行此動作。

### <a name="simple-tracking"></a>簡單追蹤

[色彩選擇器] 視窗主控單一 XAML 控制項, 而與色彩選擇器互動的程式碼全都位於檔案`MainPage.xaml.cs`中。 色彩選擇器視窗只允許單一實例, 而且基本上是的`MainWindow`延伸模組。 為了確保只會建立一個實例, 會使用頁面層級變數來追蹤色彩選擇器視窗。 在建立新的色彩選擇器視窗之前, 您會檢查實例是否存在, 如果有的話, 請略過建立新視窗的步驟, 並直接在現有的視窗上呼叫[TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) 。

```csharp
AppWindow colorPickerAppWindow;

// ...

private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        // ...
        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        // ...
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

### <a name="track-an-appwindow-instance-in-its-hosted-content"></a>追蹤其主控內容中的 AppWindow 實例

此`AppWindowPage`視窗裝載完整的 XAML 頁面, 而與頁面互動的程式碼位於中`AppWindowPage.xaml.cs`。 它允許多個實例, 每一個都有獨立的功能。

頁面的功能可讓您操作視窗、將其設定為`FullScreen`或`CompactOverlay`, 也會接聽[AppWindow。已變更](/uwp/api/windows.ui.windowmanagement.appwindow.changed)的事件會顯示視窗的相關資訊。 若要呼叫這些 api, `AppWindowPage`需要裝載它的 AppWindow 實例的參考。

如果這是您所需的一切, 您可以在中`AppWindowPage`建立屬性, 並在建立時將[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)實例指派給它。

**AppWindowPage.xaml.cs**

在`AppWindowPage`中, 建立屬性以保存 AppWindow 參考。

```csharp
public sealed partial class AppWindowPage : Page
{
    public AppWindow MyAppWindow { get; set; }

    // ...
}
```

**MainPage.xaml.cs**

在`MainPage`中, 取得頁面實例的參考, 並將新建立的 AppWindow 指派給中`AppWindowPage`的屬性。

```csharp
private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
{
    // Create a new window.
    AppWindow appWindow = await AppWindow.TryCreateAsync();

    // Create a Frame and navigate to the Page you want to show in the new window.
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowPage));

    // Get a reference to the page instance and assign the
    // newly created AppWindow to the MyAppWindow property.
    AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
    page.MyAppWindow = appWindow;

    // ...
}
```

### <a name="tracking-app-windows-using-uicontext"></a>使用 UICoNtext 追蹤應用程式視窗

您可能也會想要能夠從應用程式的其他部分存取[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)實例。 例如, `MainPage`可能會有 [全部關閉] 按鈕, 以關閉 AppWindow 的所有追蹤實例。

在此情況下, 您應該使用[UICoNtext](/uwp/api/windows.ui.uicontext)的唯一識別碼來追蹤[字典](/dotnet/api/system.collections.generic.dictionary-2?view=dotnet-uwp-10.0)中的視窗實例。

**MainPage.xaml.cs**

在`MainPage`中, 建立字典做為靜態屬性。 然後, 在建立時將頁面新增至字典, 並在頁面關閉時將它移除。 呼叫[ElementCompositionPreview. SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent)之後，您可以從內容[Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame) (`appWindowContentFrame.UIContext`)取得 UICoNtext。

```csharp
public sealed partial class MainPage : Page
{
    // Track open app windows in a Dictionary.
    public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
        = new Dictionary<UIContext, AppWindow>();

    // ...

    private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
    {
        // Create a new window.
        AppWindow appWindow = await AppWindow.TryCreateAsync();

        // Create a Frame and navigate to the Page you want to show in the new window.
        Frame appWindowContentFrame = new Frame();
        appWindowContentFrame.Navigate(typeof(AppWindowPage));

        // Attach the XAML content to the window.
        ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

        // Add the new page to the Dictionary using the UIContext as the Key.
        AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
        appWindow.Title = "App Window " + AppWindows.Count.ToString();

        // When the window is closed, be sure to release
        // XAML resources and the reference to the window.
        appWindow.Closed += delegate
        {
            MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
            appWindowContentFrame.Content = null;
            appWindow = null;
        };

        // Show the window.
        await appWindow.TryShowAsync();
    }

    private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
    {
        while (AppWindows.Count > 0)
        {
            await AppWindows.Values.First().CloseAsync();
        }
    }
    // ...
}
```

**AppWindowPage.xaml.cs**

若要在程式`AppWindowPage`代碼中使用 [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) 實例，請使用頁面的[UICoNtext](/uwp/api/windows.ui.uicontext)，從的靜態字典中`MainPage`取出它。 您應該在頁面[載入](/uwp/api/windows.ui.xaml.frameworkelement.loaded)的事件處理常式中執行此動作, 而不是在此函式中, 因此 UICoNtext 不是 null。 您可以從頁面取得 UICoNtext: `this.UIContext`。

```csharp
public sealed partial class AppWindowPage : Page
{
    AppWindow window;

    // ...
    public AppWindowPage()
    {
        this.InitializeComponent();

        Loaded += AppWindowPage_Loaded;
    }

    private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Get the reference to this AppWindow that was stored when it was created.
        window = MainPage.AppWindows[this.UIContext];

        // Set up event handlers for the window.
        window.Changed += Window_Changed;
    }
    // ...
}
```

> [!NOTE]
> 此`HelloAppWindow`範例會示範兩種追蹤`AppWindowPage`視窗的方式, 但您通常會使用其中一個, 而不是兩者。

## <a name="request-window-size-and-placement"></a>要求視窗大小和位置

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)類別有數個方法, 您可以用來控制視窗的大小和位置。 如同方法名稱所隱含, 系統不一定會根據環境因素來接受要求的變更。

呼叫[RequestSize](/uwp/api/windows.ui.windowmanagement.appwindow.requestsize)以指定所需的視窗大小, 如下所示。

```csharp
colorPickerAppWindow.RequestSize(new Size(300, 428));
```

管理視窗位置的方法名為_RequestMove *_ :[RequestMoveAdjacentToCurrentView](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttocurrentview)、 [RequestMoveAdjacentToWindow](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttowindow)、 [RequestMoveRelativeToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoverelativetodisplayregion)、 [RequestMoveToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmovetodisplayregion)。

在此範例中, 此程式碼會將視窗移到主視圖的旁邊, 而此視窗是由其衍生而來。

```csharp
colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
```

若要取得目前的視窗大小和位置的相關資訊, 請呼叫[GetPlacement](/uwp/api/windows.ui.windowmanagement.appwindow.getplacement)。 這會傳回[AppWindowPlacement](/uwp/api/windows.ui.windowmanagement.appwindowplacement)物件, 以提供目前的[DisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindowplacement.displayregion)、[位移](/uwp/api/windows.ui.windowmanagement.appwindowplacement.offset)和視窗[大小](/uwp/api/windows.ui.windowmanagement.appwindowplacement.size)。

例如, 您可以呼叫此程式碼, 將視窗移至顯示畫面的右上角。 此程式碼必須在顯示視窗之後呼叫;否則, 呼叫 GetPlacement 所傳回的視窗大小將為 0, 0, 而位移則不正確。

```csharp
DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
double displayRegionWidth = displayRegion.WorkAreaSize.Width;
double windowWidth = window.GetPlacement().Size.Width;
int horizontalOffset = (int)(displayRegionWidth - windowWidth);
window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
```

## <a name="request-a-presentation-configuration"></a>要求簡報設定

[AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)類別可讓您使用預先定義的設定來顯示[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) , 其適用于其所顯示的裝置。 您可以使用[AppWindowPresentationConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresentationconfiguration)值將視窗`FullScreen`置於或`CompactOverlay`模式。

這個範例顯示如何執行下列動作:

- 使用 [ [AppWindow 已變更](/uwp/api/windows.ui.windowmanagement.appwindow.changed)] 事件, 以在可用的視窗簡報變更時收到通知。
- 使用[AppWindow. 展示者](/uwp/api/windows.ui.windowmanagement.appwindow.presenter)屬性來取得目前的[AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)。
- 呼叫[IsPresentationSupported](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.ispresentationsupported)以查看是否支援特定的[AppWindowPresentationKind](/uwp/api/windows.ui.windowmanagement.appwindowpresentationkind) 。
- 呼叫[GetConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.getconfiguration)以檢查目前使用的設定種類。
- 呼叫[RequestPresentation](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.requestpresentation)來變更目前的設定。

```csharp
private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
{
    if (args.DidAvailableWindowPresentationsChange)
    {
        EnablePresentationButtons(sender);
    }

    if (args.DidWindowPresentationChange)
    {
        ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
    }

    if (args.DidSizeChange)
    {
        SizeText.Text = window.GetPlacement().Size.ToString();
    }
}

private void EnablePresentationButtons(AppWindow window)
{
    // Check whether the current AppWindowPresenter supports CompactOverlay.
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
    {
        // Show the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Collapsed;
    }

    // Check whether the current AppWindowPresenter supports FullScreen?
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
    {
        // Show the FullScreen button...
        fullScreenButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the FullScreen button...
        fullScreenButton.Visibility = Visibility.Collapsed;
    }
}

private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
        fullScreenButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}

private void FullScreenButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
        compactOverlayButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}
```

## <a name="reuse-xaml-elements"></a>重複使用 XAML 元素

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)可讓您擁有多個具有相同 UI 執行緒的 XAML 樹狀結構。 不過, XAML 元素只能加入至 XAML 樹狀結構一次。 如果您想要將某個部分的 UI 從一個視窗移至另一個視窗, 您必須管理它在 XAML 樹狀結構中的位置。

這個範例示範如何重複使用[ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker)控制項, 同時將它移到主視窗和次要視窗之間。

色彩選擇器是在的 xaml `MainPage`中宣告, 它會將`MainPage`它放在 xaml 樹狀結構中。

```xaml
<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
    <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
</StackPanel>
```

當色彩選擇器卸離以放在新的 AppWindow 中時, 您必須先`MainPage`從 XAML 樹狀結構中移除它, 方法是將它從其父容器移除。 雖然並非必要, 但此範例也會隱藏父容器。

```csharp
colorPickerContainer.Children.Remove(colorPicker);
colorPickerContainer.Visibility = Visibility.Collapsed;
```

然後您可以將它加入至新的 XAML 樹狀結構。 在這裡, 您會先建立一個[格線](/uwp/api/windows.ui.xaml.controls.grid), 做為 ColorPicker 的父容器, 並加入 ColorPicker 做為方格的子系。 (這可讓您稍後輕鬆地從這個 XAML 樹狀結構移除 ColorPicker)。接著, 您可以在新視窗中將方格設定為 XAML 樹狀結構的根。

```csharp
Grid appWindowRootGrid = new Grid();
appWindowRootGrid.Children.Add(colorPicker);

// Create a new window
colorPickerAppWindow = await AppWindow.TryCreateAsync();

// Attach the XAML content to our window
ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);
```

當[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)關閉時, 您會反轉進程。 首先, 移除[方格](/uwp/api/windows.ui.xaml.controls.grid)中的[ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) , 然後將它新增為中`MainPage` [StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel)的子系。

```csharp
// When the window is closed, be sure to release XAML resources
// and the reference to the window.
colorPickerAppWindow.Closed += delegate
{
    appWindowRootGrid.Children.Remove(colorPicker);
    appWindowRootGrid = null;
    colorPickerAppWindow = null;

    colorPickerContainer.Children.Add(colorPicker);
    colorPickerContainer.Visibility = Visibility.Visible;
};
```

```csharp
private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    ColorPickerContainer.Visibility = Visibility.Collapsed;

    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        ColorPickerContainer.Children.Remove(colorPicker);

        Grid appWindowRootGrid = new Grid();
        appWindowRootGrid.Children.Add(colorPicker);

        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
        colorPickerAppWindow.RequestSize(new Size(300, 428));
        colorPickerAppWindow.Title = "Color picker";

        // Attach the XAML content to our window
        ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

        // When the window is closed, be sure to release XAML resources
        // and the reference to the window.
        colorPickerAppWindow.Closed += delegate
        {
            appWindowRootGrid.Children.Remove(colorPicker);
            appWindowRootGrid = null;
            colorPickerAppWindow = null;

            ColorPickerContainer.Children.Add(colorPicker);
            ColorPickerContainer.Visibility = Visibility.Visible;
        };
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

## <a name="show-a-dialog-box"></a>顯示對話方塊

根據預設，內容對話方塊顯示強制相對於根目錄 [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)。 當您在[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)內使用[ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog)時, 您必須手動將對話方塊上的 XamlRoot 設定為 XAML 主機的根目錄。

若要這麼做，請將 ContentDialog 的[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) 屬性設定為相同 [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) 已在 AppWindow 中的元素。 在這裡, 此程式碼位於按鈕的[Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)事件處理常式內, 因此您可以使用傳送者 (按一下按鈕) 來取得 XamlRoot。

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
{
    simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
}
```

如果除了主視窗 (ApplicationView) 之外, 還有一或多個 AppWindows 開啟, 每個視窗都可以嘗試開啟對話方塊, 因為強制回應對話方塊只會封鎖其根視窗。 不過, 每個執行緒一次只能開啟一個[ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog) 。 嘗試開啟兩個 ContentDialogs 會擲回例外狀況，即使它們嘗試在不同的 AppWindows 中開啟。

若要進行管理, 您至少應該在`try/catch`區塊中開啟對話方塊來攔截例外狀況, 以防另一個對話方塊已開啟。

```csharp
try
{
    ContentDialogResult result = await simpleDialog.ShowAsync();
}
catch (Exception)
{
    // The dialog didn't open, probably because another dialog is already open.
}
```

管理對話方塊的另一種方式是追蹤目前開啟的對話方塊, 並在嘗試開啟新的對話方塊之前將它關閉。 在這裡, 您會在中`MainPage` `CurrentDialog`建立靜態屬性, 以供此用途使用。

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

   // ...
}
```

然後, 您會檢查目前是否有開啟的對話方塊, 如果有的話, 請呼叫[Hide](/uwp/api/windows.ui.xaml.controls.contentdialog.hide)方法將它關閉。 最後, 將新的對話指派`CurrentDialog`給, 並嘗試顯示它。

```csharp
private async void DialogButton_Click(object sender, RoutedEventArgs e)
{
    ContentDialog simpleDialog = new ContentDialog
    {
        Title = "Content dialog",
        Content = "Dialog box for " + window.Title,
        CloseButtonText = "Ok"
    };

    if (MainPage.CurrentDialog != null)
    {
        MainPage.CurrentDialog.Hide();
    }
    MainPage.CurrentDialog = simpleDialog;

    // Use this code to associate the dialog to the appropriate AppWindow by setting
    // the dialog's XamlRoot to the same XamlRoot as an element that is already
    // present in the AppWindow.
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
    {
        simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
    }

    try
    {
        ContentDialogResult result = await simpleDialog.ShowAsync();
    }
    catch (Exception)
    {
        // The dialog didn't open, probably because another dialog is already open.
    }
}
```

如果不希望以程式設計方式關閉對話方塊, 請不要將它指派為`CurrentDialog`。 這裡顯示一個重要的對話方塊, 只有在按下滑鼠`Ok`按鍵時才會關閉。 `MainPage` 因為未將它指派為`CurrentDialog`, 所以不會嘗試以程式設計方式關閉它。

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

    // ...
    private async void DialogButton_Click(object sender, RoutedEventArgs e)
    {
        ContentDialog importantDialog = new ContentDialog
        {
            Title = "Important dialog",
            Content = "This dialog can only be dismissed by clicking Ok.",
            CloseButtonText = "Ok"
        };

        if (MainPage.CurrentDialog != null)
        {
            MainPage.CurrentDialog.Hide();
        }
        // Do not track this dialog as the MainPage.CurrentDialog.
        // It should only be closed by clicking the Ok button.
        MainPage.CurrentDialog = null;

        try
        {
            ContentDialogResult result = await importantDialog.ShowAsync();
        }
        catch (Exception)
        {
            // The dialog didn't open, probably because another dialog is already open.
        }
    }
    // ...
}
```

## <a name="complete-code"></a>完整程式碼

### <a name="mainpagexaml"></a>MainPage.xaml

```xaml
<Page
    x:Class="HelloAppWindow.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button x:Name="NewWindowButton" Content="Open new window" 
                    Click="ShowNewWindowButton_Click" Margin="0,12"/>
            <Button Content="Open dialog" Click="DialogButton_Click" 
                    HorizontalAlignment="Stretch"/>
            <Button Content="Close all" Click="CloseAllButton_Click" 
                    Margin="0,12" HorizontalAlignment="Stretch"/>
        </StackPanel>

<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
            <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
        </StackPanel>
    </Grid>
</Page>

```

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=402352&clcid=0x409

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        AppWindow colorPickerAppWindow;

        // Track open app windows in a Dictionary.
        public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
            = new Dictionary<UIContext, AppWindow>();

        // Track the last opened dialog so you can close it if another dialog tries to open.
        public static ContentDialog CurrentDialog { get; set; } = null;

        public MainPage()
        {
            this.InitializeComponent();
        }

        private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
        {
            // Create a new window.
            AppWindow appWindow = await AppWindow.TryCreateAsync();

            // Create a Frame and navigate to the Page you want to show in the new window.
            Frame appWindowContentFrame = new Frame();
            appWindowContentFrame.Navigate(typeof(AppWindowPage));

            // Get a reference to the page instance and assign the
            // newly created AppWindow to the MyAppWindow property.
            AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
            page.MyAppWindow = appWindow;
            page.TextColorBrush = new SolidColorBrush(colorPicker.Color);

            // Attach the XAML content to the window.
            ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

            // Add the new page to the Dictionary using the UIContext as the Key.
            AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
            appWindow.Title = "App Window " + AppWindows.Count.ToString();

            // When the window is closed, be sure to release XAML resources
            // and the reference to the window.
            appWindow.Closed += delegate
            {
                MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
                appWindowContentFrame.Content = null;
                appWindow = null;
            };

            // Show the window.
            await appWindow.TryShowAsync();
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog importantDialog = new ContentDialog
            {
                Title = "Important dialog",
                Content = "This dialog can only be dismissed by clicking Ok.",
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            // Do not track this dialog as the MainPage.CurrentDialog.
            // It should only be closed by clicking the Ok button.
            MainPage.CurrentDialog = null;

            try
            {
                ContentDialogResult result = await importantDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
        {
            // Create the color picker window.
            if (colorPickerAppWindow == null)
            {
                colorPickerContainer.Children.Remove(colorPicker);
                colorPickerContainer.Visibility = Visibility.Collapsed;

                Grid appWindowRootGrid = new Grid();
                appWindowRootGrid.Children.Add(colorPicker);

                // Create a new window
                colorPickerAppWindow = await AppWindow.TryCreateAsync();
                colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
                colorPickerAppWindow.RequestSize(new Size(300, 428));
                colorPickerAppWindow.Title = "Color picker";

                // Attach the XAML content to our window
                ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

                // Make sure to release the reference to this window, 
                // and release XAML resources, when it's closed
                colorPickerAppWindow.Closed += delegate
                {
                    appWindowRootGrid.Children.Remove(colorPicker);
                    appWindowRootGrid = null;
                    colorPickerAppWindow = null;

                    colorPickerContainer.Children.Add(colorPicker);
                    colorPickerContainer.Visibility = Visibility.Visible;
                };
            }
            // Show the window.
            await colorPickerAppWindow.TryShowAsync();
        }

        private void ColorPicker_ColorChanged(ColorPicker sender, ColorChangedEventArgs args)
        {
            NewWindowButton.Background = new SolidColorBrush(args.NewColor);
        }

        private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
        {
            while (AppWindows.Count > 0)
            {
                await AppWindows.Values.First().CloseAsync();
            }
        }
    }
}

```

### <a name="appwindowpagexaml"></a>AppWindowPage .xaml

```xaml
<Page
    x:Class="HelloAppWindow.AppWindowPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <TextBlock x:Name="TitleTextBlock" Text="Hello AppWindow!" FontSize="24" HorizontalAlignment="Center" Margin="24"/>

        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button Content="Open dialog" Click="DialogButton_Click"
                    Width="200" Margin="0,4"/>
            <Button Content="Move window" Click="MoveWindowButton_Click"
                    Width="200" Margin="0,4"/>
            <ToggleButton Content="Compact Overlay" x:Name="compactOverlayButton" Click="CompactOverlayButton_Click"
                          Width="200" Margin="0,4"/>
            <ToggleButton Content="Full Screen" x:Name="fullScreenButton" Click="FullScreenButton_Click"
                          Width="200" Margin="0,4"/>
            <Grid>
                <TextBlock Text="Size:"/>
                <TextBlock x:Name="SizeText" HorizontalAlignment="Right"/>
            </Grid>
            <Grid>
                <TextBlock Text="Presentation:"/>
                <TextBlock x:Name="ConfigText" HorizontalAlignment="Right"/>
            </Grid>
        </StackPanel>
    </Grid>
</Page>
```

### <a name="appwindowpagexamlcs"></a>AppWindowPage.xaml.cs

```csharp
using System;
using Windows.Foundation;
using Windows.Foundation.Metadata;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=234238

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class AppWindowPage : Page
    {
        AppWindow window;

        public AppWindow MyAppWindow { get; set; }

        public SolidColorBrush TextColorBrush { get; set; } = new SolidColorBrush(Colors.Black);

        public AppWindowPage()
        {
            this.InitializeComponent();

            Loaded += AppWindowPage_Loaded;
        }

        private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
        {
            // Get the reference to this AppWindow that was stored when it was created.
            window = MainPage.AppWindows[this.UIContext];

            // Set up event handlers for the window.
            window.Changed += Window_Changed;

            TitleTextBlock.Foreground = TextColorBrush;
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog simpleDialog = new ContentDialog
            {
                Title = "Content dialog",
                Content = "Dialog box for " + window.Title,
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            MainPage.CurrentDialog = simpleDialog;

            // Use this code to associate the dialog to the appropriate AppWindow by setting
            // the dialog's XamlRoot to the same XamlRoot as an element that is already 
            // present in the AppWindow.
            if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
            {
                simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
            }

            try
            {
                ContentDialogResult result = await simpleDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
        {
            if (args.DidAvailableWindowPresentationsChange)
            {
                EnablePresentationButtons(sender);
            }

            if (args.DidWindowPresentationChange)
            {
                ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
            }

            if (args.DidSizeChange)
            {
                SizeText.Text = window.GetPlacement().Size.ToString();
            }
        }

        private void EnablePresentationButtons(AppWindow window)
        {
            // Check whether the current AppWindowPresenter supports CompactOverlay.
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
            {
                // Show the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Collapsed;
            }

            // Check whether the current AppWindowPresenter supports FullScreen?
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
            {
                // Show the FullScreen button...
                fullScreenButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the FullScreen button...
                fullScreenButton.Visibility = Visibility.Collapsed;
            }
        }

        private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
                fullScreenButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void FullScreenButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
                compactOverlayButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void MoveWindowButton_Click(object sender, RoutedEventArgs e)
        {
            DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
            double displayRegionWidth = displayRegion.WorkAreaSize.Width;
            double windowWidth = window.GetPlacement().Size.Width;
            int horizontalOffset = (int)(displayRegionWidth - windowWidth);
            window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
        }
    }
}
```

## <a name="related-topics"></a>相關主題

- [顯示多個視圖](show-multiple-views.md)
- [使用 ApplicationView 顯示多個視圖](application-view.md)
