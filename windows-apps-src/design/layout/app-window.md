---
description: 您可以使用 AppWindow 類別在不同的視窗中，查看您應用程式的不同部分。
title: 使用 AppWindow 類別來顯示應用程式的次要視窗
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 903fe20cef1e3ccaa5db4e1cffc6b6583a878376
ms.sourcegitcommit: f7c7a2ae6367e114a8b9d438963082440cd24043
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/13/2021
ms.locfileid: "107315091"
---
# <a name="show-multiple-views-with-appwindow"></a>使用 AppWindow 顯示多個視圖

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) 和其相關的 api 可讓您在次要視窗中顯示您的應用程式內容，同時仍在每個視窗中使用相同的 UI 執行緒，藉此簡化多視窗應用程式的建立。

> [!NOTE]
> AppWindow 目前為預覽狀態。 也就是您可以將使用 AppWindow 的應用程式提交至 Microsoft Store，但是已知某些平台和架構元件無法與 AppWindow 搭配使用 (請參閱[限制](/uwp/api/windows.ui.windowmanagement.appwindow#limitations))。

在這裡，我們會顯示多個視窗的案例，其中包含名為的範例應用程式 `HelloAppWindow` 。 範例應用程式會示範下列功能：

- 從主頁面取消固定控制項，然後在新視窗中開啟。
- 在新視窗中開啟頁面的新實例。
- 以程式設計方式在應用程式中調整新視窗的大小和位置。
- 將 ContentDialog 與應用程式中的適當視窗建立關聯。

![具有單一視窗的範例應用程式](images/hello-app-window-single.png)
  
> _具有單一視窗的範例應用程式_

![具有取消停駐色彩選擇器和次要視窗的範例應用程式](images/hello-app-window-multi.png)

> _具有取消停駐色彩選擇器和次要視窗的範例應用程式_

> **重要 api**： [WindowManagement 命名空間](/uwp/api/windows.ui.windowmanagement)、 [AppWindow 類別](/uwp/api/windows.ui.windowmanagement.appwindow)

## <a name="api-overview"></a>API 概觀

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)類別和[WindowManagement](/uwp/api/windows.ui.windowmanagement)命名空間中的其他 api，從 Windows 10，1903版 (SDK 18362) 開始提供。 如果您的應用程式以舊版 Windows 10 為目標，您必須 [使用 ApplicationView 來建立次要視窗](application-view.md)。 WindowManagement Api 仍在開發中，而且具有 API 參考檔中所述的 [限制](/uwp/api/windows.ui.windowmanagement.appwindow#limitations) 。

以下是一些您在 AppWindow 中用來顯示內容的重要 Api。

### <a name="appwindow"></a>AppWindow

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)類別可以用來在次要視窗中顯示 Windows 執行階段應用程式的一部分。 它在概念上類似于 [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)，但行為和存留期不同。 AppWindow 的主要功能是，每個實例都共用相同的 UI 處理執行緒 (包括其建立所在的事件發送器) ，進而簡化多視窗應用程式。

您只能將 XAML 內容連接到您的 AppWindow，不支援原生 DirectX 或全像攝影內容。 不過，您可以顯示裝載 DirectX 內容的 XAML [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) 。

### <a name="windowingenvironment"></a>WindowingEnvironment

[WindowingEnvironment](/uwp/api/windows.ui.windowmanagement.windowingenvironment) API 可讓您知道您的應用程式呈現所在的環境，以便您可以視需要調整應用程式。 它描述了環境所支援的視窗種類;例如， `Overlapped` 如果應用程式是在電腦上執行，或 `Tiled` 應用程式正在 Xbox 上執行。 它也會提供一組 DisplayRegion 物件，描述應用程式可顯示在邏輯顯示器上的區域。

### <a name="displayregion"></a>DisplayRegion

[DisplayRegion](/uwp/api/windows.ui.windowmanagement.displayregion) API 描述可在邏輯顯示器上對使用者顯示視圖的區域;例如，在桌上型電腦上，這是顯示在工作列區域以外的完整顯示器。 這不一定是與支援監視的實體顯示區域之間的1:1 對應。 同一個監視器內可以有多個顯示區域，或者，如果這些監視器在所有方面都是同質，則可以將 DisplayRegion 設定為跨越多個監視器。

### <a name="appwindowpresenter"></a>AppWindowPresenter

[AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) API 可讓您輕鬆地將視窗切換到預先定義的設定，例如 `FullScreen` 或 `CompactOverlay` 。 這些設定可讓使用者在任何支援設定的裝置上獲得一致的體驗。

### <a name="uicontext"></a>UIContext

[UICoNtext](/uwp/api/windows.ui.uicontext) 是應用程式視窗或視圖的唯一識別碼。 它是自動建立的，您可以使用 [UICoNtext](/uwp/api/windows.ui.xaml.uielement.uicontext) 屬性來取得 UICoNtext。 XAML 樹狀結構中的每個 UIElement 都有相同的 UICoNtext。

 UICoNtext 很重要，因為像是「 [目前](/uwp/api/Windows.UI.Xaml.Window.Current) 」和「模式」等 api 都 `GetForCurrentView` 依賴每個執行緒要使用的單一 ApplicationView/CoreWindow，且每個執行緒都有一個 XAML 樹狀結構。 當您使用 AppWindow 時，不會發生這種情況，因此您可以使用 UICoNtext 來識別特定的視窗。

### <a name="xamlroot"></a>XamlRoot

[XamlRoot](/uwp/api/windows.ui.xaml.xamlroot)類別會保存 XAML 元素樹狀結構、將它連接到視窗主控制項物件 (例如[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)或[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)) ，並提供大小和可見度等資訊。 您不會直接建立 XamlRoot 物件。 相反地，當您將 XAML 元素附加至 AppWindow 時，就會建立一個。 然後，您可以使用 [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) 屬性來取得 XamlRoot。

如需 UICoNtext 和 XamlRoot 的詳細資訊，請參閱 [讓程式碼可跨視窗化主機進行移植](show-multiple-views.md#make-code-portable-across-windowing-hosts)。

## <a name="show-a-new-window"></a>顯示新視窗

讓我們看看在新的 AppWindow 中顯示內容的步驟。

**若要顯示新視窗**

1. 呼叫靜態 [AppWindow. TryCreateAsync](/uwp/api/windows.ui.windowmanagement.appwindow.trycreateasync) 方法，以建立新的 [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)。

    ```csharp
    AppWindow appWindow = await AppWindow.TryCreateAsync();
    ```

1. 建立視窗內容。

    一般而言，您會建立 XAML [框架](/uwp/api/Windows.UI.Xaml.Controls.Frame)，然後將框架流覽至您已定義應用程式內容的 xaml [頁面](/uwp/api/Windows.UI.Xaml.Controls.Page) 。 如需有關框架和頁面的詳細資訊，請參閱 [兩頁之間的點對點導覽](../basics/navigate-between-two-pages.md)。

    ```csharp
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowMainPage));
    ```

    不過，您可以在 AppWindow 中顯示任何 XAML 內容，而不只是在框架和頁面中顯示。 例如，您可以只顯示單一控制項（例如 [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker)），也可以顯示裝載 DirectX 內容的 [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) 。

1. 呼叫 [ElementCompositionPreview. SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent) 方法，將 XAML 內容附加至 AppWindow。

    ```csharp
    ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);
    ```

    呼叫這個方法會建立 [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) 物件，並將它設定為指定之 UIElement 的 [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) 屬性。

    您只能針對每個 AppWindow 實例呼叫這個方法一次。 設定內容之後，此 AppWindow 實例的 SetAppWindowContent 進一步呼叫會失敗。 此外，如果您嘗試傳入 null UIElement 物件來中斷 AppWindow 內容的連接，則呼叫會失敗。

1. 呼叫 [AppWindow. TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) 方法以顯示新的視窗。

    ```csharp
    await appWindow.TryShowAsync();
    ```

## <a name="release-resources-when-a-window-is-closed"></a>當視窗關閉時釋放資源

您應該一律處理 [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow.closed) ，以釋放 XAML 資源 (AppWindow 內容) 和 AppWindow 的參考。

```csharp
appWindow.Closed += delegate
{
    appWindowContentFrame.Content = null;
    appWindow = null;
};
```

> [!TIP]
> 您應該盡可能將事件處理常式中的程式碼數量維持在 `Closed` 最小數量，以避免發生非預期的問題。

## <a name="track-instances-of-appwindow"></a>追蹤 AppWindow 的實例

根據您在應用程式中使用多個視窗的方式，您可能不需要追蹤您所建立的 AppWindow 實例。 此 `HelloAppWindow` 範例顯示一些您通常會使用 [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)的不同方式。 在這裡，我們將探討為何應該追蹤這些視窗，以及如何進行。

### <a name="simple-tracking"></a>簡單追蹤

[色彩選擇器] 視窗會裝載單一 XAML 控制項，而且與色彩選擇器互動的程式碼都位於檔案中 `MainPage.xaml.cs` 。 [色彩選擇器] 視窗只允許單一實例，而且基本上是的延伸 `MainWindow` 。 為了確保只建立一個實例，[色彩選擇器] 視窗會以頁面層級變數進行追蹤。 在建立新的 [色彩選擇器] 視窗之前，請先檢查某個實例是否存在，如果有的話，請略過建立新視窗的步驟，並在現有的視窗上直接呼叫 [TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) 。

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

### <a name="track-an-appwindow-instance-in-its-hosted-content"></a>追蹤其託管內容中的 AppWindow 實例

`AppWindowPage`視窗會裝載完整的 XAML 頁面，而與頁面互動的程式碼則位於中 `AppWindowPage.xaml.cs` 。 它允許多個實例，每個實例各自獨立。

頁面的功能可讓您操作視窗、將它設定為 `FullScreen` 或 `CompactOverlay` ，也會接聽 [AppWindow。已變更](/uwp/api/windows.ui.windowmanagement.appwindow.changed) 的事件可顯示視窗的相關資訊。 若要呼叫這些 Api， `AppWindowPage` 需要裝載它的 AppWindow 實例的參考。

如果您需要這樣做，您可以在中建立屬性， `AppWindowPage` 並在建立時將 [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) 實例指派給它。

**AppWindowPage .cs**

在中 `AppWindowPage` ，建立屬性以保存 AppWindow 參考。

```csharp
public sealed partial class AppWindowPage : Page
{
    public AppWindow MyAppWindow { get; set; }

    // ...
}
```

**MainPage.xaml.cs** \(英文\)

在中 `MainPage` ，取得頁面實例的參考，並將新建立的 AppWindow 指派給中的屬性 `AppWindowPage` 。

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

您也可能想要從應用程式的其他部分存取 [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) 實例。 例如， `MainPage` 可能會有一個 [全部關閉] 按鈕，可關閉所有已追蹤的 AppWindow 實例。

在此情況下，您應該使用 [UICoNtext](/uwp/api/windows.ui.uicontext) 唯一識別碼來追蹤 [字典](/dotnet/api/system.collections.generic.dictionary-2?view=dotnet-uwp-10.0)中的視窗實例。

**MainPage.xaml.cs** \(英文\)

在中 `MainPage` ，建立字典做為靜態屬性。 然後，在建立時將頁面加入至字典，並在頁面關閉時將其移除。 您可以在[](/uwp/api/Windows.UI.Xaml.Controls.Frame) `appWindowContentFrame.UIContext` 呼叫[ElementCompositionPreview SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent)之後，從內容框架 () 取得 UICoNtext。

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

**AppWindowPage .cs**

若要在程式碼中使用 [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) 實例 `AppWindowPage` ，請使用頁面的 [UICoNtext](/uwp/api/windows.ui.uicontext) ，從的靜態字典中取出 `MainPage` 。 您應該在頁面 [載入](/uwp/api/windows.ui.xaml.frameworkelement.loaded) 的事件處理常式中，而不是在此函式中，讓 UICoNtext 不是 null。 您可以從頁面取得 UICoNtext： `this.UIContext` 。

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
> 此 `HelloAppWindow` 範例顯示兩種在中追蹤視窗的方式 `AppWindowPage` ，但您通常會使用其中一個，而非兩者。

## <a name="request-window-size-and-placement"></a>要求視窗大小和位置

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)類別有數個方法可供您用來控制視窗的大小和位置。 如同方法名稱所暗示，系統不一定會根據環境因素，接受所要求的變更。

呼叫 [RequestSize](/uwp/api/windows.ui.windowmanagement.appwindow.requestsize) 來指定所需的視窗大小，如下所示。

```csharp
colorPickerAppWindow.RequestSize(new Size(300, 428));
```

管理視窗位置的方法名為 _RequestMove *_： [RequestMoveAdjacentToCurrentView](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttocurrentview)、 [RequestMoveAdjacentToWindow](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttowindow)、 [RequestMoveRelativeToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoverelativetodisplayregion)、 [RequestMoveToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmovetodisplayregion)。

在此範例中，此程式碼會將視窗移到視窗產生來源的主視圖旁邊。

```csharp
colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
```

若要取得目前的視窗大小和位置的相關資訊，請呼叫 [GetPlacement](/uwp/api/windows.ui.windowmanagement.appwindow.getplacement)。 這會傳回 [AppWindowPlacement](/uwp/api/windows.ui.windowmanagement.appwindowplacement) 物件，此物件會提供視窗的目前 [DisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindowplacement.displayregion)、 [位移](/uwp/api/windows.ui.windowmanagement.appwindowplacement.offset)和 [大小](/uwp/api/windows.ui.windowmanagement.appwindowplacement.size) 。

例如，您可以呼叫此程式碼，將視窗移至顯示的右上角。 此程式碼必須在視窗顯示之後呼叫;否則，呼叫 GetPlacement 所傳回的視窗大小將會是0、0，而且位移會是不正確的。

```csharp
DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
double displayRegionWidth = displayRegion.WorkAreaSize.Width;
double windowWidth = window.GetPlacement().Size.Width;
int horizontalOffset = (int)(displayRegionWidth - windowWidth);
window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
```

## <a name="request-a-presentation-configuration"></a>要求簡報設定

[AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)類別可讓您使用適用于所顯示裝置的預先定義設定來顯示[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) 。 您可以使用 [AppWindowPresentationConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresentationconfiguration) 值將視窗置於 `FullScreen` 或 `CompactOverlay` 模式中。

此範例顯示如何執行下列動作：

- 使用 [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow.changed) 變更事件，以在可用的視窗簡報變更時收到通知。
- 使用 [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow.presenter) 來取得目前的 [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)。
- 呼叫 [IsPresentationSupported](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.ispresentationsupported) 以查看是否支援特定的 [AppWindowPresentationKind](/uwp/api/windows.ui.windowmanagement.appwindowpresentationkind) 。
- 呼叫 [GetConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.getconfiguration) 來檢查目前使用的設定種類。
- 呼叫 [RequestPresentation](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.requestpresentation) 來變更目前的設定。

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

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)可讓您擁有多個具有相同 UI 執行緒的 XAML 樹狀結構。 不過，XAML 元素只能加入至 XAML 樹狀結構一次。 如果您想要將部分 UI 從某個視窗移到另一個視窗，您必須管理它在 XAML 樹狀結構中的位置。

此範例示範如何在主視窗和次要視窗之間移動時，重複使用 [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) 控制項。

色彩選擇器是在 XAML 中宣告，它會將 `MainPage` 它放在 `MainPage` xaml 樹狀結構中。

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

當色彩選擇器卸離以放置在新的 AppWindow 中時，您必須先從 XAML 樹狀結構中移除它， `MainPage` 方法是從其父容器中移除它。 雖然並非必要，但此範例也會隱藏父容器。

```csharp
colorPickerContainer.Children.Remove(colorPicker);
colorPickerContainer.Visibility = Visibility.Collapsed;
```

然後，您可以將它新增至新的 XAML 樹狀結構。 在這裡，您會先建立一個 [格線](/uwp/api/windows.ui.xaml.controls.grid) ，以做為 ColorPicker 的父容器，並新增 ColorPicker 做為方格的子系。  (這讓您稍後可以輕鬆地從此 XAML 樹狀結構中移除 ColorPicker。 ) 接著將方格設定為新視窗中 XAML 樹狀結構的根。

```csharp
Grid appWindowRootGrid = new Grid();
appWindowRootGrid.Children.Add(colorPicker);

// Create a new window
colorPickerAppWindow = await AppWindow.TryCreateAsync();

// Attach the XAML content to our window
ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);
```

當 [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) 關閉時，您會反轉進程。 首先，從[方格](/uwp/api/windows.ui.xaml.controls.grid)中移除[ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) ，然後將它新增為中[StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel)的子系 `MainPage` 。

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

根據預設，內容對話方塊顯示強制相對於根目錄 [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)。 當您在[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)內使用[ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog)時，您需要將對話方塊上的 XAMLROOT 手動設定為 XAML 主機的根目錄。

若要這樣做，請將 ContentDialog 的 [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) 屬性設定為與 AppWindow 中已有元素相同的 [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) 。 在此，此程式碼位於按鈕的 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件處理常式內，因此您可以使用 [ _寄件者_ ] (按一下按鈕) 來取得 XamlRoot。

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
{
    simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
}
```

如果您有一或多個 AppWindows 開啟，除了主視窗 (ApplicationView) 之外，每個視窗都可以嘗試開啟對話方塊，因為強制回應對話方塊只會封鎖它的根目錄視窗。 不過，每個執行緒一次只能開啟一個 [ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog) 。 嘗試開啟兩個 ContentDialogs 會擲回例外狀況，即使它們嘗試在不同的 AppWindows 中開啟。

為了管理這個問題，您應該至少在區塊中開啟對話方塊 `try/catch` 來攔截例外狀況，以防另一個對話方塊已開啟。

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

管理對話的另一種方式是追蹤目前開啟的對話方塊，並在嘗試開啟新的對話方塊之前關閉對話方塊。 在此，您會在中建立靜態屬性，以 `MainPage` `CurrentDialog` 供此用途使用。

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

   // ...
}
```

然後，您要檢查是否有目前開啟的對話方塊，如果有的話，則呼叫 [Hide](/uwp/api/windows.ui.xaml.controls.contentdialog.hide) 方法來關閉它。 最後，將新的對話指派給 `CurrentDialog` ，並嘗試顯示它。

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

如果不希望以程式設計方式關閉對話方塊，請不要將它指派為 `CurrentDialog` 。 在此， `MainPage` 會顯示只在使用點擊時才關閉的重要對話方塊 `Ok` 。 因為未將它指派為 `CurrentDialog` ，所以不會嘗試以程式設計方式關閉它。

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

### <a name="appwindowpagexamlcs"></a>AppWindowPage .cs

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

- [顯示多重檢視](show-multiple-views.md)
- [使用 ApplicationView 顯示多個視圖](application-view.md)
