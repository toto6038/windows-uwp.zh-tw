---
ms.assetid: 00ECF6C7-0970-4D5F-8055-47EA49F92C12
title: App 啟動效能的最佳做法
description: 透過改善處理啟動和啟用的方式，建立具有最佳啟動時間的通用 Windows 平台 (UWP) app。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ae37ab763b6705fbb3f341569904972ebb181412
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254678"
---
# <a name="best-practices-for-your-apps-startup-performance"></a>App 啟動效能的最佳做法


透過改善處理啟動和啟用的方式，建立具有最佳啟動時間的通用 Windows 平台 (UWP) app。

## <a name="best-practices-for-your-apps-startup-performance"></a>App 啟動效能的最佳做法

使用者對您 App 回應速度快慢的感受，部分取決於 App 啟動時間的長短。 基於這個主題的目的，App 的啟動時間是從使用者啟動 App 時算起，到使用者可以用一些有意義的方式與 App 互動時為止。 本節提供有關如何在 App 啟動時獲得較佳效能的建議。

### <a name="measuring-your-apps-startup-time"></a>評估您 App 的啟動時間

請務必先啟動 App 幾次，再實際評估其啟動時間。 這將為您的評估提供一個基準，並確保您評估出的啟動時間儘可能在合理的範圍內。

您的 UWP App 在到達您客戶的電腦上時，已經過「.NET 原生」工具鏈編譯。 「.NET 原生」是一種事先編譯技術，可將 MSIL 轉換成原生可執行機器碼。 .NET 原生 app 比其對應的 MSIL 啟動更快、使用更少的記憶體，而且消耗較少的電池電力。 使用「.NET 原生」建置的應用程式會在自訂執行階段和可於所有裝置上執行的新聚合式「.NET 核心」中以靜態方式連結，因此它們並不依存於附隨的 .NET 實作。 在您的開發電腦上，如果您是以「發行」模式建置您的 App，它預設會使用「.NET 原生」，如果您是以「偵錯」模式建置 App，則它會使用 CoreCLR。 您可以在 Visual Studio 從 \[建置\] 頁面中的 \[屬性\] \(C#\)，或 \[我的專案\] \(VB\) 中的 \[編譯\] -&gt; \[進階\]，進行這項設定。 尋找名為 \[利用 .NET 原生工具鏈進行編譯\] 的核取方塊。

當然，您應該進行代表使用者將擁有之體驗的評估。 因此，如果您不確定是否要在開發電腦上將您的 App 編譯成機器碼，您可以先執行「原生映像產生器」(Ngen.exe) 工具來預先編譯 App，再評估其啟動時間。

下列程序說明如何執行 Ngen.exe 以重新編譯 app。

**To run Ngen.exe**

1.  至少執行 app 一次，以確保 Ngen.exe 可以偵測到它。
2.  若要開啟 \[**工作排程器**\]，請執行下列其中一個動作：
    -   從 \[開始\] 畫面搜尋「工作排程器」。
    -   執行 "taskschd.msc"。
3.  在 \[**工作排程器**\] 的左窗格中，展開 \[**工作排程器程式庫**\]。
4.  展開 \[**Microsoft**\]。
5.  展開 \[**Windows**\]。
6.  選取 \[ **.NET Framework**\]。
7.  從工作清單選取 \[ **.NET Framework NGEN 4.x**\]。

    如果您是使用 64 位元電腦，也有 \[ **.NET Framework NGEN v4.x 64**\]。 如果您要建立 64 位元應用程式，請選取 \[ **.NET Framework NGEN v4.x 64**\]。

8.  從 \[**動作**\] 功能表，按一下 \[**執行**\]。

Ngen.exe 會預先編譯電腦上所有已經使用且沒有原生映像的應用程式。 如果有許多需要預先編譯的應用程式，這可能需要花很長的時間，但是後續的執行會快許多。

當您重新編譯應用程式時，就不會再使用原生映像。 而是應用程式以 Just-In-Time 的方式編譯，這表示它會在應用程式執行時進行編譯。 您必須重新執行 Ngen.exe 以取得新的原生映像。

### <a name="defer-work-as-long-as-possible"></a>盡可能將工作延長

為了改善應用程式的啟動時間，請只做絕對必須做的工作，讓使用者開始與應用程式互動。 如果您可以延遲載入其他組件，這將特別實用。 Common Language Runtime 會在第一次使用組件時載入它。 如果您可以將要載入的組件數目減至最少，就可能改善 App 的啟動時間及其記憶體的消耗量。

### <a name="do-long-running-work-independently"></a>讓長時間執行的工作獨立執行

即使您的 App 有部分無法完整運作，它仍然可以與使用者互動。 例如，如果應用程式顯示資料時需要一些時間抓取，您可以在不考慮應用程式啟動程式碼的情況下，用非同步的方式抓取資料來執行程式碼。 等到資料可供使用時，再將該資料填入 App 的使用者介面。

許多抓取資料的「通用 Windows 平台」(UWP) API 都是非同步的，因此無論如何，您可能都會以非同步的方式抓取資料。 如需有關非同步 API 的詳細資訊，請參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)。 如果您執行未使用非同步 API 的工作，則可以使用 Task 類別來進行長時間執行的工作，這樣您就不會阻礙使用者與 App 互動。 這將可使 App 在載入資料時，仍然可以回應使用者。

如果 App 在載入其部分 UI 時花費的時間特別長，請考慮在該部分新增像是「正在取得最新資料」的字串，讓使用者知道應用程式仍然在進行處理。

## <a name="minimize-startup-time"></a>將啟動時間縮到最短

除了最簡單的 App 之外，所有 App 在啟用時都需要花費一段時間來載入資源、剖析 XAML、設定資料結構，以及執行邏輯。 在這裡，我們將把啟用程序分成三個階段來加以分析。 此外，我們也會提供縮減每個階段所費時間的祕訣，以及一些可讓 App 的每個啟動階段對使用者來說更為輕鬆愉快的秘訣。

啟用期間是從使用者啟動 App 到 App 正常運作所經歷的時間。 這段時間非常重要，因為這是使用者對您 App 的第一印象。 使用者會期待從系統和應用程式得到立即和持續的回應。 如果 App 無法快速啟動，使用者便會覺得系統和 App 已損毀或設計不良。 甚至更糟，如果 App 在啟用時花費太長的時間，「處理程序生命週期管理員」(PLM) 可能就會將它刪除，或使用者可能會將它解除安裝。

### <a name="introduction-to-the-stages-of-startup"></a>啟動階段簡介

啟動包含一些活動的片段，必須正確地協調所有這些片段，才能獲得最佳的使用者體驗。 從使用者按一下您的 App 磚到開始顯示應用程式內容之間，會發生下列步驟。

-   Windows 殼層啟動處理程序，然後 Main 被呼叫。
-   建立 Application 物件。
    -   (專案範本) 建構函式呼叫 InitializeComponent，這會導致剖析 App.xaml 並建立物件。
-   引發 Application.OnLaunched 事件。
    -   (ProjectTemplate) App 程式碼建立一個 Frame 並瀏覽至 MainPage。
    -   (ProjectTemplate) MainPage 建構函式呼叫 InitializeComponent，這會導致剖析 MainPage.xaml 並建立物件。
    -   (ProjectTemplate) 呼叫 Window.Current.Activate()。
-   「XAML 平台」執行包括 Measure 與 Arrange 的「版面配置」階段。
    -   ApplyTemplate 會導致為每個控制項建立控制項範本內容，這通常會佔啟動時「版面配置」時間的大部分。
-   呼叫 Render 來建立所有視窗內容的視覺效果。
-   對「桌面視窗管理員」(DWM) 呈現 Frame。

### <a name="do-less-in-your-startup-path"></a>精簡您啟動路徑中的動作

讓您的啟動程式碼路徑中不含第一個框架所不需的一切項目。

-   如果您有包含控制項的使用者 dll 是第一個框架期間所不需要的，請考慮延遲載入它們。
-   如果您的 UI 有部分倚賴來自雲端的資料，則請分割該 UI。 先顯示不依賴雲端資料的 UI，再以非同步方式顯示雲端相依 UI。 您也應該考慮在本機快取資料，以便讓應用程式離線工作，或不受緩慢的網路連線影響。
-   如果您的 UI 正在等待資料，請顯示進度 UI。
-   請小心處理涉及許多組態檔剖析的 App 設計，或是由程式碼動態產生的 UI。

### <a name="reduce-element-count"></a>縮減元素計數

XAML App 中的啟動效能與您在啟動期間建立的元素數目直接相關。 建立的元素越少，您 App 啟動時所花費的時間將會越短。 做為概略的基準，請將建立每個元素所需花費的時間視為 1 毫秒。

-   項目控制項中所使用範本的影響最大，因為它們會重複很多次。 請參閱 [ListView 與 GridView UI 最佳化](optimize-gridview-and-listview.md)。
-   UserControl 和控制項範本將會擴充，因此也應將這些納入考量。
-   如果您建立任何不會顯示在螢幕上的 XAML，則您應該要有正當的理由確定這些 XAML 片段是否應該在啟動時建立。

[Visual Studio 即時視覺化樹狀結構](https://devblogs.microsoft.com/visualstudio/introducing-the-ui-debugging-tools-for-xaml/)視窗會顯示樹狀結構中每個節點的子元素計數。

![即時視覺化樹狀結構。](images/live-visual-tree.png)

**使用延遲**。 將元素摺疊或將其不透明度設定為 0 並不會防止建立該元素。 使用 x:Load 或 x:DeferLoadStrategy，您便可延遲載入某個 UI，而在需要時才載入它。 這是一個相當好的方式，可延遲處理在啟動畫面期間不顯示的 UI，讓您在需要時才載入該 UI，或是隨著延遲邏輯一起載入。 若要觸發載入，您只需要針對該元素呼叫 FindName。 如需範例和詳細資訊，請參閱 [x:Load 屬性](../xaml-platform/x-load-attribute.md)和 [x:DeferLoadStrategy 屬性](https://docs.microsoft.com/windows/uwp/xaml-platform/x-deferloadstrategy-attribute)。

**虛擬化**。 如果您的 UI 中有清單或重複器內容，則強烈建議您使用 UI 虛擬化。 如果不將清單 UI 虛擬化，代價就是會在最前面建立所有元素，而這會導致啟動變慢。 請參閱 [ListView 與 GridView UI 最佳化](optimize-gridview-and-listview.md)。

應用程式效能不僅僅與原始效能相關，也與感受相關。 變更操作順序以讓視覺層面先發生，通常會讓使用者感覺操作速度較快。 當內容出現在螢幕上時，使用者會認為應用程式已載入。 通常，應用程式在啟動過程中需要執行多項作業，但並非所有作業都是顯示 UI 時所需的作業，因此應該將這些作業延遲或將優先順序排在 UI 之後。

本主題討論來自動畫/電視的「第一個畫面」，並且做為要多久時間使用者才會看到內容的一種評估方式。

### <a name="improve-startup-perception"></a>改善啟動感受

讓我們使用一個簡單的線上遊戲範例來認明每個啟動階段，並使用不同的技巧在整個過程中持續回應使用者。 就這個範例而言，第一個啟用階段是從使用者點選遊戲的磚到遊戲開始執行其程式碼為止。 在這段期間，系統沒有任何內容可向使用者顯示，甚至無法指出已啟動正確的遊戲。 但是提供啟動顯示畫面即可將該內容提供給系統。 遊戲接著會在開始執行程式碼時，以自己的 UI 取代靜態的啟動顯示畫面，藉此通知使用者第一個啟用階段已經完成。

第二個啟用階段包括進行遊戲重要結構的建立和初始化。 如果 App 在第一個啟用階段後能夠以可用的資料快速建立其初始 UI，第二個階段就會相當微不足道，而您將可以立即顯示 UI。 否則，我們建議 App 在初始化時顯示載入頁面。

載入頁面的外觀由您決定，它可以簡單到只是顯示一個進度列或進度環。 關鍵點在於讓 App 在產生回應之前指出它正在執行工作。 在遊戲的例子中，雖然遊戲想要顯示初始畫面，但需要從磁碟將一些影像和聲音載入記憶體才能顯示 UI。 這些工作需花費幾秒的時間才能完成，因此 App 會以載入畫面 (顯示與遊戲主題相關的簡單動畫) 取代啟動顯示畫面，讓使用者得知進度。

第三個階段會在遊戲取得建立互動式 UI (將取代載入頁面) 所需的一組最基本資料後開始。 此時，線上遊戲唯一可用的資訊是從磁碟載入的應用程式內容。 遊戲會隨附足夠的內容來建立互動式 UI；不過因為它是線上遊戲，所以必須先連線到網際網路並下載一些額外資訊，才能開始運作。 在它取得運作所需的一切資訊之前，使用者可以與 UI 進行互動，但是需要從網路取得額外資料的功能應該提供回應，讓使用者知道內容仍在載入中。 App 可能需要一些時間才能完整運作，因此儘快提供使用者可用的功能相當重要。

現在，我們已指出線上遊戲的三個啟用階段，讓我們將它們與實際程式碼連結在一起。

### <a name="phase-1"></a>階段 1

應用程式啟動前，它需要告知系統要做為啟動顯示畫面的項目。 若要這樣做，請在應用程式資訊清單的 SplashScreen 元素中提供影像和背景色彩，如範例所示。 Windows 會在 App 開始啟用程序後顯示這個畫面。

```xml
<Package ...>
  ...
  <Applications>
    <Application ...>
      <VisualElements ...>
        ...
        <SplashScreen Image="Images\splashscreen.png" BackgroundColor="#000000" />
        ...
      </VisualElements>
    </Application>
  </Applications>
</Package>
```

如需詳細資訊，請參閱[新增啟動顯示畫面](https://docs.microsoft.com/windows/uwp/launch-resume/add-a-splash-screen)。

請只使用 App 的建構函式來初始化對應用程式非常重要的資料結構。 只有第一次執行應用程式時需要呼叫建構函式，不需要在每次啟用應用程式時呼叫它。 例如，不會為已執行、置於背景然後透過搜尋協定啟用的應用程式呼叫建構函式。

### <a name="phase-2"></a>階段 2

啟用應用程式有數個原因，而處理每個原因的方法也不相同。 您可以覆寫 [**OnActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated)、[**OnCachedFileUpdaterActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.oncachedfileupdateractivated)、[**OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)、[**OnFileOpenPickerActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileopenpickeractivated)、[**OnFileSavePickerActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfilesavepickeractivated)、[**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched)、[**OnSearchActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onsearchactivated) 以及 [**OnShareTargetActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onsharetargetactivated) 方法來處理啟用的每個原因。 應用程式必須在這些方法進行的其中一個動作是建立 UI，將它指派給 [**Window.Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.content)，然後呼叫 [**Window.Activate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.activate)。 此時，啟動顯示畫面會被 app 建立的 UI 所取代。 這個視覺畫面可能是載入畫面或應用程式實際的 UI (如果啟用時有足夠的資訊建立 UI 的話)。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public partial class App : Application
> {
>     // A handler for regular activation.
>     async protected override void OnLaunched(LaunchActivatedEventArgs args)
>     {
>         base.OnLaunched(args);
> 
>         // Asynchronously restore state based on generic launch.
> 
>         // Create the ExtendedSplash screen which serves as a loading page while the
>         // reader downloads the section information.
>         ExtendedSplash eSplash = new ExtendedSplash();
> 
>         // Set the content of the window to the extended splash screen.
>         Window.Current.Content = eSplash;
> 
>         // Notify the Window that the process of activation is completed
>         Window.Current.Activate();
>     }
> 
>     // a different handler for activation via the search contract
>     async protected override void OnSearchActivated(SearchActivatedEventArgs args)
>     {
>         base.OnSearchActivated(args);
> 
>         // Do an asynchronous restore based on Search activation
> 
>         // the rest of the code is the same as the OnLaunched method
>     }
> }
> 
> partial class ExtendedSplash : Page
> {
>     // This is the UIELement that's the game's home page.
>     private GameHomePage homePage;
> 
>     public ExtendedSplash()
>     {
>         InitializeComponent();
>         homePage = new GameHomePage();
>     }
> 
>     // Shown for demonstration purposes only.
>     // This is typically autogenerated by Visual Studio.
>     private void InitializeComponent()
>     {
>     }
> }
> ```
> ```vb
>     Partial Public Class App
>     Inherits Application
> 
>     ' A handler for regular activation.
>     Protected Overrides Async Sub OnLaunched(ByVal args As LaunchActivatedEventArgs)
>         MyBase.OnLaunched(args)
> 
>         ' Asynchronously restore state based on generic launch.
> 
>         ' Create the ExtendedSplash screen which serves as a loading page while the
>         ' reader downloads the section information.
>         Dim eSplash As New ExtendedSplash()
> 
>         ' Set the content of the window to the extended splash screen.
>         Window.Current.Content = eSplash
> 
>         ' Notify the Window that the process of activation is completed
>         Window.Current.Activate()
>     End Sub
> 
>     ' a different handler for activation via the search contract
>     Protected Overrides Async Sub OnSearchActivated(ByVal args As SearchActivatedEventArgs)
>         MyBase.OnSearchActivated(args)
> 
>         ' Do an asynchronous restore based on Search activation
> 
>         ' the rest of the code is the same as the OnLaunched method
>     End Sub
> End Class
> 
> Partial Friend Class ExtendedSplash
>     Inherits Page
> 
>     Public Sub New()
>         InitializeComponent()
> 
>         ' Downloading the data necessary for 
>         ' initial UI on a background thread.
>         Task.Run(Sub() DownloadData())
>     End Sub
> 
>     Private Sub DownloadData()
>         ' Download data to populate the initial UI.
> 
>         ' Create the first page. 
>         Dim firstPage As New MainPage()
> 
>         ' Add the data just downloaded to the first page
> 
>         ' Replace the loading page, which is currently 
>         ' set as the window's content, with the initial UI for the app
>         Window.Current.Content = firstPage
>     End Sub
> 
>     ' Shown for demonstration purposes only.
>     ' This is typically autogenerated by Visual Studio.
>     Private Sub InitializeComponent()
>     End Sub
> End Class 
> ```

在啟用處理常式中顯示載入頁面的應用程式會開始在背景建立 UI。 建立該元素之後，會發生元素的 [**FrameworkElement.Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) 事件。 在事件處理常式中，您可以將視窗的內容 (目前的載入畫面) 取代成新建立的首頁。

對於具有延長式初始化期間的應用程式來說，顯示載入頁面是非常重要的。 除了可提供使用者啟用程序的回應之外，如果在啟用程序開始的 15 秒內未呼叫 [**Window.Activate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.activate)，這個程序就會終止。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> partial class GameHomePage : Page
> {
>     public GameHomePage()
>     {
>         InitializeComponent();
> 
>         // add a handler to be called when the home page has been loaded
>         this.Loaded += ReaderHomePageLoaded;
> 
>         // load the minimal amount of image and sound data from disk necessary to create the home page.        
>     }
>     
>     void ReaderHomePageLoaded(object sender, RoutedEventArgs e)
>     {
>         // set the content of the window to the home page now that it's ready to be displayed.
>         Window.Current.Content = this;
>     }
> 
>     // Shown for demonstration purposes only.
>     // This is typically autogenerated by Visual Studio.
>     private void InitializeComponent()
>     {
>     }
> }
> ```
> ```vb
>     Partial Friend Class GameHomePage
>     Inherits Page
> 
>     Public Sub New()
>         InitializeComponent()
> 
>         ' add a handler to be called when the home page has been loaded
>         AddHandler Me.Loaded, AddressOf ReaderHomePageLoaded
> 
>         ' load the minimal amount of image and sound data from disk necessary to create the home page.        
>     End Sub
> 
>     Private Sub ReaderHomePageLoaded(ByVal sender As Object, ByVal e As RoutedEventArgs)
>         ' set the content of the window to the home page now that it's ready to be displayed.
>         Window.Current.Content = Me
>     End Sub
> 
>     ' Shown for demonstration purposes only.
>     ' This is typically autogenerated by Visual Studio.
>     Private Sub InitializeComponent()
>     End Sub
> End Class
> ```

如需使用延長式啟動顯示畫面的範例，請參閱[啟動顯示畫面範例](https://code.msdn.microsoft.com/windowsapps/Splash-screen-sample-89c1dc78)。

### <a name="phase-3"></a>階段 3

應用程式顯示 UI 時並不代表使用者已經可以使用完整的應用程式功能。 以我們的遊戲為例，UI 會與需要從網際網路取得資料的功能預留位置一起顯示。 此時，遊戲會下載讓應用程式可以完整運作所需的額外資料，並透過取得的資料逐漸啟用應用程式功能。

有時，啟用所需的大多數內容會與應用程式封裝在一起。 簡單的遊戲就是其中一例。 這讓啟用程序變得相當簡單。 但是許多程式 (例如，新聞閱讀程式和相片檢視器) 必須從網路取得資訊才能正常運作。 這個資料可能很大，並且需要花費相當長的時間下載。 應用程式在啟用程序期間取得這個資料的方式會大幅影響使用者對應用程式效能的感受。

如果應用程式嘗試在啟用的階段 1 或 2 下載功能所需的整個資料集，而且應用程式必須顯示長達數分鐘的載入頁面或 (更糟的是) 啟動顯示畫面。 這會讓應用程式看起來像是沒有反應，或是被系統終止。 我們建議 app 在階段 2 中先下載最少的資料量來顯示與預留位置元素一起顯示的互動式 UI，然後在階段 3 中逐漸載入取代預留位置元素的資料。 如需處理資料的詳細資訊，請參閱[最佳化 ListView 與 GridView](optimize-gridview-and-listview.md)。

app 對每個啟動階段的反應方式完全取決於您，但是請盡可能提供使用者更多回應 (資料載入時的啟動顯示畫面、載入畫面、UI)，這會讓使用者感覺 app 和整個系統執行起來是快速的。

### <a name="minimize-managed-assemblies-in-the-startup-path"></a>將啟動路徑中的 Managed 組件減到最少

可重複使用的程式碼通常會以專案中包含的模組 (DLL) 形式出現。 載入這些模組需要存取磁碟，因此您可以想像這樣做會增加大量負擔。 這不但對冷啟動有非常大的影響，也會影響暖啟動。 如果是 C# 和 Visual Basic，CLR 會視需要載入組件，嘗試將這類負擔盡可能延後。 也就是說，CLR 只會在執行方法參照模組時載入模組。 因此，請僅在啟動程式碼中參照啟動應用程式所需的必要組件，讓 CLR 不要載入不必要的模組。 如果啟動路徑中有含有非必要參照的未使用程式碼路徑，您可以將這些程式碼路徑移到其他方法以避免不必要的載入。

減少模組載入的另外一個方法是合併您的應用程式模組。 載入一個大組件所需的時間通常比載入兩個小組件的時間短。 但這並非永遠可行，只有在不會對開發人員生產力或重複使用程式碼造成實質差異的情況下，才可以合併模組。 您可以使用工具 (例如 [PerfView](https://www.microsoft.com/download/details.aspx?id=28567) 或 [Windows 效能分析程式 (WPA)](https://docs.microsoft.com/previous-versions/windows/desktop/xperf/windows-performance-analyzer--wpa-)) 來找出啟動時載入的模組。

### <a name="make-smart-web-requests"></a>聰明的 Web 要求

您可以透過在本機封裝應用程式內容 (包括 XAML、影像，以及任何其他對應用程式很重要的檔案) 來大幅提升應用程式的載入時間。 磁碟操作的速度比網路操作快。 如果 App 在初始化時需要某個特定的檔案，您可以從磁碟載入 (而不是從遠端伺服器抓取) 該檔案，以減少整體的啟動時間。

## <a name="journal-and-cache-pages-efficiently"></a>有效率地記錄日誌和快取頁面

「框架」控制項可提供瀏覽功能。 它提供「頁面」瀏覽 (Navigate 方法)、瀏覽日誌 (BackStack/ForwardStack 屬性、GoForward/GoBack 方法)、「頁面」快取 (Page.NavigationCacheMode)，以及序列化支援 (GetNavigationState 方法)。

「框架」相關效能方面要注意的主要是圍繞著日誌和頁面快取。

**框架日誌記錄**。 當您使用 Frame.Navigate() 來瀏覽到頁面時，目前頁面的 PageStackEntry 會被新增到 Frame.BackStack 集合。 PageStackEntry 相對較小，但 BackStack 集合並沒有內建的大小限制。 使用者有可能以迴圈方式瀏覽，而讓這個集合無限地成長。

PageStackEntry 也包含傳遞給 Frame.Navigate() 方法的參數。 建議使用基本可序列化類型 (例如整數或字串) 的參數，以便讓 Frame.GetNavigationState() 方法能夠運作。 但該參數有可能參考負責更大量工作集或其他資源的物件，使得 BackStack 中的每個項目更加耗費資源。 例如，您有可能使用 StorageFile 做為參數，因而導致 BackStack 讓不限數目的檔案保持開啟。

因此，建議您讓瀏覽參數維持在較小的狀態，並限制 BackStack 的大小。 BackStack 是一個標準的向量 (在 C# 中為 IList，在 C++/CX 中為 Platform::Vector)，因此可以藉由移除項目來進行刪減。

**頁面快取**。 根據預設，當您使用 Frame.Navigate 方法來瀏覽到頁面時，該頁面的新執行個體將會具現化。 同樣地，如果您接著使用 Frame.GoBack 往回瀏覽到上一頁，就會配置上一頁的新執行個體。

不過，「框架」有提供一個可避免這些具現化的選擇性頁面快取。 若要將頁面放入快取中，請使用 Page.NavigationCacheMode 屬性。 將該模式設定為 Required 會強制快取頁面，將它設定為 Enabled 則可允許快取頁面。 預設的快取大小是 10 頁，但是可以使用 Frame.CacheSize 屬性來覆寫該預設值。 將會快取所有 Required 頁面，如果數量比 CacheSize Required 頁面少，則也可以快取 Enabled 頁面。

頁面快取可藉由避免具現化來協助改善效能，進而提升瀏覽效能。 頁面快取可能因過度快取，進而影響到工作集，而導致效能降低。

因此，建議針對您的應用程式適當地使用頁面快取。 例如，假設您有一個會在「框架」中顯示項目清單的 App，而當您點選某個項目時，框架會瀏覽到該項目的詳細資料頁面。 清單頁面或許就應該設定為要快取。 如果所有項目的詳細資料頁面都相同，則或許也應該快取該頁面。 但是，如果詳細資料頁面是較異質的頁面，則將快取維持關閉可能較好。
