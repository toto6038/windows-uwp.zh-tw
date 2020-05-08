---
description: 以宣告式 XAML 標記形式定義 UI 的做法可以極順利地從 Windows Phone Silverlight 轉譯至通用 Windows 平台 (UWP) app。
title: 將 Windows Phone Silverlight XAML 和 UI 移植到 UWP
ms.assetid: 49aade74-5dc6-46a5-89ef-316dbeabbebe
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9f6f7bddba5b6e457cb6ece4aa811f02ba3ebe8a
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730339"
---
#  <a name="porting-windowsphone-silverlight-xaml-and-ui-to-uwp"></a>將 Windows Phone Silverlight XAML 和 UI 移植到 UWP



前一個主題是[疑難排解](wpsl-to-uwp-troubleshooting.md)。

以宣告式 XAML 標記形式定義 UI 的做法可以極順利地從 Windows Phone Silverlight 轉譯至通用 Windows 平台 (UWP) app。 您會發現在更新系統資源索引鍵參考、變更某些元素類型名稱，以及將 "clr-namespace" 變更為 "using" 之後，大部分的標記都相容。 展示層中的大多數命令式程式碼 (檢視模型及操作 UI 元素的程式碼) 也會直接移植。

## <a name="a-first-look-at-the-xaml-markup"></a>XAML 標記初探

前一個主題示範如何將您的 XAML 和程式碼後置檔案複製到新的 Windows 10 Visual Studio 專案。 在 Visual Studio XAML 設計工具醒目提示的前幾個問題中，您可能會注意到的其中一個就是位於 XAML 檔案根部的 `PhoneApplicationPage` 元素不適用於通用 Windows 平台 (UWP) 專案。 在前一個主題中，您儲存了 Visual Studio 在建立 Windows 10 專案時所產生之 XAML 檔案的複本。 如果您開啟該版本的 MainPage.xaml，您會看到在根部的是 [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) 類型 (位於 [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls)命名空間中)。 因此，您可以將所有 `<phone:PhoneApplicationPage>` 元素變更為 `<Page>` (請記得使用屬性元素語法)，並且可以刪除 `xmlns:phone` 宣告。

如需更一般的方法來尋找與 Windows Phone Silverlight 類型相對應的 UWP 類型，您可以參考[命名空間與類別對應](wpsl-to-uwp-namespace-and-class-mappings.md)主題。

## <a name="xaml-namespace-prefix-declarations"></a>XAML 命名空間前置詞宣告


如果您在檢視中使用自訂類型執行個體 (可能是檢視模型執行個體或值轉換器)，則您的 XAML 標記中將會有 XAML 命名空間前置詞宣告。 這些項目的語法在 Windows Phone Silverlight 與 UWP 之間有所不同。 以下是一些範例：

```xml
    xmlns:ContosoTradingCore="clr-namespace:ContosoTradingCore;assembly=ContosoTradingCore"
    xmlns:ContosoTradingLocal="clr-namespace:ContosoTradingLocal"
```

請將 "clr-namespace" 變更為 "using"，並刪除任何組件語彙基元和分號 (將會推斷組件)。 結果如下所示：

```xml
    xmlns:ContosoTradingCore="using:ContosoTradingCore"
    xmlns:ContosoTradingLocal="using:ContosoTradingLocal"
```

您可能有由系統定義其類型的資源：

```xml
    xmlns:System="clr-namespace:System;assembly=mscorlib"
    /* ... */
    <System:Double x:Key="FontSizeLarge">40</System:Double>
```

在 UWP 中，請省略 "System" 前置詞宣告，改用 (已宣告的) "x" 前置詞：

```xml
    <x:Double x:Key="FontSizeLarge">40</x:Double>
```

## <a name="imperative-code"></a>命令式程式碼


您的檢視模型是其中一個會有參考 UI 類型之命令式程式碼的地方。 另一個地方則是任何會直接操作 UI 元素的程式碼後置檔案。 例如，您可能會發現一行類似下列的程式碼尚未編譯：


```csharp
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

**BitmapImage** 位於 Windows Phone Silverlight 的 **System.Windows.Media.Imaging**命名空間中，而同一檔案中的 using 指示詞可允許在不加命名空間限定的情況下使用 **BitmapImage**，如上述程式碼片段所示。 在這種情況下，您可以在 Visual Studio 中於類型名稱 (**BitmapImage**) 上按一下滑鼠右鍵，然後使用操作功能表上的 [**解析**] 命令將新的命名空間指示詞新增到檔案中。 在此情況下，會新增 [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) 命名空間，這是類型在 UWP 中的所在位置。 您可以移除 **System.Windows.Media.Imaging** using 指示詞，只要這樣就可以移植如上述程式碼片段中那樣的程式碼。 當您完成時，即已移除所有 Windows Phone Silverlight 命名空間。

在像這樣的簡單案例中，亦即將舊命名空間中的類型對應到新命名空間中的相同類型，您可以使用 Visual Studio 的 [**尋找和取代**] 命令對原始程式碼進行大量變更。 [**解析**] 命令是一個探索類型之新命名空間的絕佳方法。 再舉一例，您可以使用 "Windows.UI.Xaml" 來取代所有 "System.Windows"。 基本上，這將會移植所有 using 指示詞和所有參考該命名空間的完整類型名稱。

在移除所有舊的 using 指示詞並加入新的指示詞之後，您便可以使用 Visual Studio 的 [**組合管理 Using**] 命令來排序您的指示詞及移除不使用的指示詞。

有時候，修正命令式程式碼就像變更參數類型一樣，只是小事一樁。 其他時候，您將需要使用 Windows 執行階段 Api，而不是適用于 Windows 執行階段8.x 應用程式的 .NET Api。 若要識別支援的 Api，請使用此移植指南的其餘部分，並搭配[.net for Windows 執行階段8.x 應用程式總覽](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))和[Windows 執行階段參考](https://docs.microsoft.com/uwp/api/)。

而如果您只是想要到達專案建置階段，您可以將任何非必要程式碼標成註解或清除。 然後逐一查看問題，並參閱本節 (與上一個主題：[疑難排解](wpsl-to-uwp-troubleshooting.md)) 中的下列主題，直到任何建置與執行階段問題都已解決，且您的移植已完成為止。

## <a name="adaptiveresponsive-ui"></a>調適型/回應式 UI

因為您的 Windows 10 app 可以在許多不同的裝置上執行 (每個裝置都有其獨特的螢幕大小與解析度)，所以您將會想要透過最少的步驟來完成您的 app 移植，並讓您的 UI 能夠在那些裝置上呈現最佳外觀。 您可以使用調適型 Visual State Manager 功能來動態偵測視窗大小及變更回應配置，Bookstore2 案例研究主題的[調適型 UI](wpsl-to-uwp-case-study-bookstore2.md) 中已經提供此做法的範例。

## <a name="alarms-and-reminders"></a>鬧鐘和提醒

程式碼如果使用 **Alarm** 或 **Reminder** 類別，應該移植並改用 [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 類別來建立和登錄背景工作，以及在相關時間顯示快顯通知。 請參閱[背景處理](wpsl-to-uwp-business-and-data.md)和[快顯通知](#toasts)。

## <a name="animation"></a>動畫

UWP app 現在可以使用 UWP 動畫庫，做為主要畫面格動畫及 from/to 動畫的慣用替代方案。 這些動畫已設計和微調成能夠順暢執行、看起來美觀，以及讓您的應用程式看起來就像內建的應用程式一樣與 Windows 整合在一起。 請參閱[快速入門：使用動畫庫讓 UI 產生動畫效果](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))。

如果您在 UWP app 中使用主要畫面格動畫或 from/to 動畫，您可能會想要了解新平台導入的獨立式和相依式動畫之間的區別。 請參閱[最佳化動畫和媒體](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-animations-and-media)。 在 UI 執行緒上執行的動畫 (例如讓配置屬性產生動畫效果的動畫) 稱為「相依式動畫」，在新平台上執行時，除非您執行下列兩個動作之一，否則它們將不會有任何作用。 您可以將它們的動畫作用目標重新設定為不同的屬性 (例如 [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform))，讓它們變成獨立式動畫。 或者，在動畫元素上設定 `EnableDependentAnimation="True"`，以確認想要執行無法保證能夠順暢執行的動畫。 如果您使用 Blend for Visual Studio 來撰寫新的動畫，則會在必要的地方為您設定該屬性。

## <a name="back-button-handling"></a>[返回] 按鈕處理

在 Windows 10 應用程式中，您可以使用單一方式來處理返回按鈕，且返回按鈕將可以在所有裝置上運作。 在行動裝置上，按鈕會在裝置上以電容式按鈕的方式，或以殼層中的按鈕的方式提供您使用。 在傳統型裝置上，您可以在只要能夠於應用程式中進行反向瀏覽時，就在應用程式組建區塊中加入一個按鈕，而且這會顯示在視窗化應用程式的標題列中，或平板電腦模式的工作列中。 返回按鈕事件是所有裝置系列的一個通用概念，且以硬體或軟體方式實作的按鈕都可以引發相同的 [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) 事件。

下面的範例適用於所有裝置系列，且非常適合在可將相同處理套用至所有頁面以及您不需要確認瀏覽 (例如，提供未儲存變更的警告) 的情況下使用。

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide a back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

也有適用於所有裝置系列，並以程式控制方式控制現有應用程式的單一方法。

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="binding-and-compiled-bindings-with-xbind"></a>繫結，及使用 {x:Bind} 編譯的繫結

繫結的主題包括：

-   將 UI 元素繫結到「資料」(亦即繫結到檢視模型的屬性和命令)
-   將 UI 元素繫結到另一個 UI 元素
-   撰寫可觀察的檢視模型 (亦即它會在屬性值變更及命令的可用性變更時，引發通知)

所有這些方面大多仍受到支援，但有命名空間差異。 例如，**System.Windows.Data.Binding** 會對應到 [**Windows.UI.Xaml.Data.Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)、**System.ComponentModel.INotifyPropertyChanged** 會對應到 [**Windows.UI.Xaml.Data.INotifyPropertyChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)，而 **System.Collections.Specialized.INotifyPropertyChanged** 會對應到 [**Windows.UI.Xaml.Interop.INotifyCollectionChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.INotifyCollectionChanged)。

Windows Phone Silverlight app 列和 app 列按鈕無法像在 UWP app 中一樣被繫結。 您可能有命令式程式碼來建構應用程式列及其按鈕、將它們繫結到屬性和已當地語系化的字串，並處理其活動。 如果是的話，您現在可以選擇移植該命令式程式碼，方法是以繫結到屬性和命令的宣告式標記，以及以靜態資源參考，來取代該命令式程式碼，這樣會讓您的應用程式安全性漸增且更容易維護。 您可以使用 Visual Studio 或 Blend for Visual Studio 來繫結 UWP app 列按鈕並為其設定樣式，就像任何其他 XAML 元素一樣。 請注意，在 UWP app 中，您使用的類型名稱為 [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 和 [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)。

UWP app 的繫結相關功能目前有下列限制：

-   未針對資料輸入驗證以及 [**IDataErrorInfo**](https://docs.microsoft.com/dotnet/api/system.componentmodel.idataerrorinfo) 和 [**INotifyDataErrorInfo**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifydataerrorinfo) 介面提供內建支援。
-   [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 類別不包含 Windows Phone Silverlight 中提供的格式化屬性延伸。 不過，您仍然可以實作 [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter) 來提供自訂格式。
-   [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter) 方法將語言字串當作參數而不是 [**CultureInfo**](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) 物件。
-   [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 類別不提供排序和篩選的內建支援，而群組的運作方式也不一樣。 如需詳細資訊，請參閱[深入了解資料繫結](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)和[資料繫結範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)。

雖然相同的繫結功能仍普遍受到支援，但 Windows 10 提供全新且效能更好的繫結機制 (稱為「編譯的繫結」) 供您選用，它是使用 {x:Bind} 標記延伸。 請參閱[資料繫結：透過 XAML 資料繫結的全新增強功能來提升您應用程式的效能](https://channel9.msdn.com/Events/Build/2015/3-635)，以及 [x:Bind 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)。

## <a name="binding-an-image-to-a-view-model"></a>將影像繫結到檢視模型

您可以將 [**Image.Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) 屬性繫結到類型為 [**ImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageSource) 的任何檢視模型屬性。 以下是 Windows Phone Silverlight 應用程式中這類屬性的典型實作：

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

在 UWP app 中，您將使用 ms-appx [URI 配置](https://docs.microsoft.com/previous-versions/windows/apps/jj655406(v=win.10))。 如此，您便可以讓程式碼的其餘部分保持不變，您可以使用不同的 **System.Uri** 建構函式多載將 ms-appx URI 配置放在基底 URI 中，然後將路徑的其餘部分附加至其上。 例如：

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

這樣一來，檢視模型的其餘部分、影像路徑屬性中的路徑值，以及 XAML 標記中的繫結，都可以完全保持不變。

## <a name="controls-and-control-stylestemplates"></a>控制項與控制項樣式/範本

Windows Phone Silverlight app 會使用 **Microsoft.Phone.Controls** 命名空間和 **System.Windows.Controls** 命名空間中定義的控制項。 XAML UWP app 會使用 [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) 命名空間中定義的控制項。 UWP 中 XAML 控制項的架構和設計實際上與 Windows Phone Silverlight 控制項相同。 但是，目前已進行一些變更來改善該組可用的控制項，並將它們與 Windows 應用程式整合。 以下是特定範例。

| 控制項名稱 | 變更 |
|--------------|--------|
| ApplicationBar | [Page.TopAppBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.topappbar) 屬性。 |
| ApplicationBarIconButton | UWP 對等項目是 [Glyph](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon.glyph) 屬性。 PrimaryCommands 是 CommandBar 的內容屬性。 XAML 剖析器會將元素的內部 XML 解譯成其內容屬性的值。 |
| ApplicationBarMenuItem | UWP 對等項目是設為功能表項目文字的 [AppBarButton.Label](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.label)。 |
| ContextMenu (在 Windows Phone 工具組中) | 針對單一選擇飛出視窗，請使用 [[飛出視窗](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)]。 |
| ControlTiltEffect.TiltEffect 類別 | 來自 UWP 動畫庫的動畫已內建至通用控制項的預設「樣式」中。 請參閱[讓指標動作產生動畫效果](https://docs.microsoft.com/previous-versions/windows/apps/jj649432(v=win.10))。 |
| LongListSelector 搭配已分組的資料 | Windows Phone Silverlight LongListSelector 有兩種運作方式，而這兩種方式可以一起使用。 首先，可以顯示依索引鍵分組的資料，例如依第一個字母分組的名稱清單。 其次，可以在下列兩種語意式檢視之間「縮放」：已分組的項目 (例如名稱) 清單和只有群組索引鍵本身 (例如第一個字母) 的清單。 運用 UWP，您可以使用[清單和格線檢視控制項的指導方針](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists)來顯示已分組的資料。 |
| LongListSelector 搭配一般資料 | 基於效能考量，對於很長的清單，即使是一般、非分組的資料，我們也建議使用 LongListSelector，而不使用 Windows Phone Silverlight 清單方塊。 在 UWP app 中，針對長的項目清單 (不論是否可將資料加以分組)，皆建議使用 [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)。 |
| Panorama | Windows Phone Silverlight 全景控制項對應至 Windows 執行階段 8.x[應用程式中中樞控制項的指導方針](https://docs.microsoft.com/windows/uwp/controls-and-patterns/hub)，以及中樞控制項的指導方針。 <br/> 請注意，Panorama 控制項會將最後一個區段迴繞到第一個區段，且其背景影像會以和區段相對的視差方式移動。 [Hub](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) 區段不會迴繞，且不會使用視差。 |
| 樞紐 | UWP 中與 Windows Phone Silverlight Pivot 控制項相等的是 [Windows.UI.Xaml.Controls.Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)。 它適用於所有裝置系列。 |

**請注意**   ，PointerOver 視覺狀態與 Windows 10 應用程式中的自訂樣式/範本有關，但不適用於 Windows Phone Silverlight 應用程式。 還有其他原因造成現有自訂樣式/範本不適用於 Windows 10 app，包括您正在使用的系統資源索引鍵、對所使用之虛擬狀態集所做的變更，以及對 Windows 10 預設樣式/範本所做的效能改進。 我們建議您針對 Windows 10 編輯控制項之預設範本的全新副本，然後對其重新套用您的樣式與範本自訂項目。

如需 UWP 控制項的詳細資訊，請參閱[依功能分類的控制項](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-by-function)、[控制項清單](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/)，以及[控制項的指導方針](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/index)。

##  <a name="design-language-in-windows10"></a>Windows 10 中的設計語言

Windows Phone Silverlight 應用程式和 Windows 10 應用程式之間的設計語言有一些差異。 如需所有詳細資訊，請參閱[設計](https://developer.microsoft.com/windows/apps/design)。 儘管設計語言會變更，但我們的設計原則仍會保持一致：留意細節，但為了簡單起見，儘量將重點放在內容不是組件區塊、將視覺元素降至最低，並保留數位網域的驗證；使用視覺層次，特別是使用印刷格式；設計格線；以及使用流暢的動畫讓您的體驗變得更生動。

## <a name="localization-and-globalization"></a>當地語系化和全球化

針對當地語系化字串，您可以在 UWP app 專案中，重複使用來自 Windows Phone Silverlight 專案的 .resx 檔案。 請將檔案複製過去、將它新增到專案中，然後將它重新命名為 Resources.resw，以便讓查詢機制預設會尋找它。 將 [**建置動作**] 設定為 [**PRIResource**]，將 [**複製到輸出目錄**] 設定為 [**不要複製**]。 然後，您便可以藉由在 XAML 元素上指定 **X:uid** 屬性，在標記中使用這些字串。 請參閱[快速入門：使用字串資源](https://docs.microsoft.com/previous-versions/windows/apps/hh965329(v=win.10))。

Windows Phone Silverlight app 使用 **CultureInfo** 類別來協助將 app 全球化。 UWP app 使用 MRT (現代資源技術)，不論是在執行階段和 Visual Studio 設計介面中，都能動態載入 app 資源 (當地語系化、縮放及佈景主題)。 如需詳細資訊，請參閱[檔案、資料和全球化的指導方針](https://docs.microsoft.com/windows/uwp/design/usability/index)。

[**ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) 主題說明如何根據裝置系列資源選擇因素載入裝置系列特定資源。

## <a name="media-and-graphics"></a>媒體和圖形

當您閱讀 UWP 媒體和圖形的相關資料時，請記住，Windows 設計原則鼓勵大幅減少任何多餘的項目，包括圖形複雜性和雜亂度。 Windows 設計是以乾淨簡潔的視覺效果、印刷樣式及移動為代表。 如果您的 app 遵守相同的原則，它看起來就會更像內建的 app。

Windows Phone Silverlight 有一個 UWP 所沒有的 **RadialGradientBrush** 類型，雖然 UWP 有其他 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 類型。 在某些情況下，您將可藉由點陣圖獲得類似的效果。 請注意，您可以在 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) 和 XAML C++ UWP 中使用 Direct2D 來[建立放射狀漸層筆刷](https://docs.microsoft.com/windows/desktop/Direct2D/how-to-create-a-radial-gradient-brush)。

Windows Phone Silverlight 具有 **System.Windows.UIElement.OpacityMask** 屬性，但該屬性不是 UWP  [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 類型的成員。 在某些情況下，您將可藉由點陣圖獲得類似的效果。 而您可以在 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) 和 XAML C++ UWP app 中使用 Direct2D 來[建立不透明度遮罩](https://docs.microsoft.com/windows/desktop/Direct2D/opacity-masks-overview)。 但是 **OpacityMask** 的一般使用案例是使用同時適合淺色和深色佈景主題的單一點陣圖。 針對向量圖形，您可以使用佈景主題感知系統筆刷 (例如下面所述的圓形圖)。 但是製作佈景主題感知點陣圖 (例如下面所述的核取記號) 需要不同的方法。

![佈景主題感知點陣圖](images/wpsl-to-uwp-case-studies/wpsl-to-uwp-theme-aware-bitmap.png)

在 Windows Phone Silverlight app 中，技巧是使用 Alpha 遮罩 (以點陣圖形式) 做為填滿前景筆刷之 **Rectangle** 的 **OpacityMask**：

```xml
    <Rectangle Fill="{StaticResource PhoneForegroundBrush}" Width="26" Height="26">
        <Rectangle.OpacityMask>
            <ImageBrush ImageSource="/Assets/wpsl_check.png"/>
        </Rectangle.OpacityMask>
    </Rectangle>
```

將這個移植到 UWP app 的最直接方式是使用 [**BitmapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.BitmapIcon)，就像這樣：

```xml
    <BitmapIcon UriSource="Assets/winrt_check.png" Width="21" Height="21"/>
```

在這裡，\_winrt 檢查 .png 是點陣圖形式的 Alpha 遮罩，就像 wpsl\_檢查一樣。 .png 是，而且也可以是相同的檔案。 不過，您可能會想要提供數種不同大小\_的 winrt 檢查，以用於不同的縮放比例因素。 如需有關該事項的詳細資訊，以及對 **Width** 和 **Height** 值所做之變更的說明，請參閱本主題中的[檢視或有效像素、檢視距離及縮放比例](#view-or-effective-pixels-viewing-distance-and-scale-factors)。

如果點陣圖的淺色和深色形式有差異，有一個適合此情況的更普遍方法，就是使用兩個影像資產：一個具有深色前景 (用於淺色佈景主題)，另一個具有淺色前景 (用於深色佈景主題)。 如需有關如何為這組點陣圖資產命名的進一步詳細資訊，請參閱[針對語言、規模和其他限定詞量身打造您的資源](../app-resources/tailor-resources-lang-scale-contrast.md)。 正確命名一組影像檔之後，您便可以在摘要中使用它們的根名稱來參考它們，就像這樣：

```xml
    <Image Source="Assets/winrt_check.png" Stretch="None"/>
```

在 Windows Phone Silverlight 中，**UIElement.Clip** 屬性可以是您可使用 **Geometry** 來表示的任何形狀，並且通常在 **StreamGeometry** 迷你語言的 XAML 標記中會序列化。 在 UWP 中，[**Clip**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip) 屬性的類型是 [**RectangleGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RectangleGeometry)，因此您只能裁剪出矩形區域。 允許使用迷你語言來定義矩形會太寬鬆。 因此，若要移植標記中的裁剪區域，請取代 **Clip** 屬性語法並將它轉換成屬性元素語法，就像下面這樣：

```xml
    <UIElement.Clip>
        <RectangleGeometry Rect="10 10 50 50"/>
    </UIElement.Clip>
```

請注意，您可以在 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) 與 XAML C++ UWP app 中使用 Direct2D，以[使用任意幾何圖形做為圖層中的遮罩](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-layers-overview)。

## <a name="navigation"></a>瀏覽

在 Windows Phone Silverlight app 中瀏覽到某個頁面時，您會使用「統一資源識別項」(URI) 定址配置：

```csharp
    NavigationService.Navigate(new Uri("/AnotherPage.xaml", UriKind.Relative)/*, navigationState*/);
```

在 UWP app 中，您則會呼叫 [**Frame.Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) 方法，並指定目的地頁面的類型 (由頁面之 XAML 標記定義的 **x:Class** 屬性所定義)：


```csharp
    // In a page:
    this.Frame.Navigate(typeof(AnotherPage)/*, parameter*/);

    // In a view model, perhaps inside an ICommand implementation:
    var rootFrame = Windows.UI.Xaml.Window.Current.Content as Windows.UI.Xaml.Controls.Frame;
    rootFrame.Navigate(typeof(AnotherPage)/*, parameter*/);
```

您會在 WMAppManifest.xml 中定義 Windows Phone Silverlight app 的啟動頁：

```xml
    <DefaultTask Name="_default" NavigationPage="MainPage.xaml" />
```

在 UWP app 中，您將使用命令式程式碼來定義啟動頁。 以下是一些來自 App.xaml.cs 並說明做法的程式碼：

```csharp
    if (!rootFrame.Navigate(typeof(MainPage), e.Arguments))
```

URI 對應和片段瀏覽是 URI 瀏覽技術，因此不適用於並非以 URI 為基礎的 UWP 瀏覽。 URI 對應的存在是為了因應以 URI 字串識別目標頁面而具有的弱式型別本質，該方式會使得頁面在移到不同的資料夾，繼而移到不同的相對路徑時，導致脆弱易損和可維護性問題。 UWP app 使用以類型為基礎的瀏覽，這是一種強型別並受編譯器檢查的瀏覽，沒有 URI 對應所要解決的問題。 片段瀏覽的使用案例是將一些內容傳遞給目標頁面，讓頁面能夠將其內容的特定片段捲動到檢視中，將以其他方式顯示。 在您呼叫 [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) 方法時傳遞瀏覽參數，也可達成相同的目標。

如需詳細資訊，請參閱[瀏覽](https://docs.microsoft.com/windows/uwp/layout/navigation-basics)。

## <a name="resource-key-reference"></a>資源索引鍵參考

設計語言已針對 Windows 10 進化，因此某些系統樣式也已經改變，且許多一統資源索引鍵已經移除或重新命名。 Visual Studio 中的 XAML 標記編輯器會醒目提示無法解析的資源索引鍵參考。 例如，XAML 標記編輯器會在樣式索引鍵 `PhoneTextNormalStyle` 的參考加上紅色波浪底線。 如果未修正該參考，當您嘗試將應用程式部署到模擬器或裝置上時，它將會立即終止。 因此，請務必注意 XAML 標記的正確性。 您會發現 Visual Studio 是攔截這類問題的絕佳工具。

另請參閱下面的[文字](#text)。

## <a name="status-bar-system-tray"></a>狀態列 (系統匣)

系統匣 (在 XAML 標記中以 `shell:SystemTray.IsVisible` 設定) 現在稱為狀態列，並且預設會顯示。 您可以在命令式程式碼中，藉由呼叫 [**Windows.UI.ViewManagement.StatusBar.ShowAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.statusbar.showasync) 和 [**HideAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.statusbar.hideasync) 方法來控制其可見度。

## <a name="text"></a>Text

文字 (或印刷樣式) 是 UWP app 中的一個重要層面，在移植時，您可以重新檢閱檢視的視覺設計，以確保它們不會與新的設計語言產生違和感。 請使用這些插圖說明來找出可用的 UWP  **TextBlock** 系統樣式。 尋找與您使用的 Windows Phone Silverlight 樣式相對應的樣式。 您也可以選擇建立自己的通用樣式，然後從 Windows Phone Silverlight 系統樣式將屬性複製到這些通用樣式中。

![適用於 Windows 10 app 的系統 textblock 樣式](images/label-uwp10stylegallery.png)

適用於 Windows 10 應用程式的系統 TextBlock 樣式

在 Windows Phone Silverlight 應用程式中，預設字型系列為 Segoe WP。 在 Windows 10 應用程式中，預設字型系列為 Segoe UI。 因此，您應用程式中的字型標準可能會看起來不一樣。 如果您想要重新產生 Windows Phone Silverlight 文字的外觀，您可以使用像是 [**LineHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.lineheight) 和 [**LineStackingStrategy**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy) 這樣的屬性設定您自己的標準。 如需詳細資訊，請參閱[字型的指導方針](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts)和[設計 UWP app](https://developer.microsoft.com/windows/apps/design)。

## <a name="theme-changes"></a>佈景主題變更

在 Windows Phone Silverlight 應用程式中，預設佈景主題為深色。 Windows 10 裝置的預設佈景主題已經變更，但是您可以透過在 App.xaml 中宣告要求的佈景主題來控制所使用的佈景主題。 例如，若要在所有裝置上使用深色佈景主題，可將 `RequestedTheme="Dark"` 新增到根應用程式元素。

## <a name="tiles"></a>Tiles

UWP app 磚的行為與 Windows Phone Silverlight app 的「動態磚」類似，雖然仍有些微差異。 例如，程式碼如果會呼叫 **Microsoft.Phone.Shell.ShellTile.Create** 方法來建立次要磚，應該移植並改成呼叫 [**SecondaryTile.RequestCreateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestcreateasync)。 以下是移植前和移植後的對照範例，首先是 Windows Phone Silverlight 版本：


```csharp
    var tileData = new IconicTileData()
    {
        Title = this.selectedBookSku.Title,
        WideContent1 = this.selectedBookSku.Title,
        WideContent2 = this.selectedBookSku.Author,
        SmallIconImage = this.SmallIconImageAsUri,
        IconImage = this.IconImageAsUri
    };

    ShellTile.Create(this.selectedBookSku.NavigationUri, tileData, true);
```

和 UWP 對等物件：

```csharp
    var tile = new SecondaryTile(
        this.selectedBookSku.Title.Replace(" ", string.Empty),
        this.selectedBookSku.Title,
        this.selectedBookSku.ArgumentString,
        this.IconImageAsUri,
        TileSize.Square150x150);

    await tile.RequestCreateAsync();
```

應用程式碼如果使用 **Microsoft.Phone.Shell.ShellTile.Update** 方法或 **Microsoft.Phone.Shell.ShellTileSchedule** 類別來更新磚，應該移植並改用 [**TileUpdateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdateManager)、[**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater)、[**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification) 和 (或) [**ScheduledTileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledTileNotification) 類別。

如需有關磚、快顯通知、徽章、橫幅及通知的詳細資訊，請參閱[建立磚](https://docs.microsoft.com/previous-versions/windows/apps/hh868260(v=win.10))和[使用磚、徽章以及快顯通知](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10))。 如需有關用於 UWP 磚之視覺資產大小的詳細資訊，請參閱[磚與快顯通知視覺資產](https://docs.microsoft.com/previous-versions/windows/apps/hh781198(v=win.10))。

## <a name="toasts"></a>快顯通知

應用程式碼如果使用 **Microsoft.Phone.Shell.ShellToast** 類別來顯示快顯通知，應該移植並改用 [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager)、[**ToastNotifier**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier)、[**ToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification) 和 (或) [**ScheduledToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification) 類別。 請注意，在行動裝置上，「快顯通知」就面向消費者的詞彙而言就是「橫幅」。

請參閱[使用磚、徽章以及快顯通知](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10))。

## <a name="view-or-effective-pixels-viewing-distance-and-scale-factors"></a>檢視或有效像素、檢視距離及縮放比例

Windows Phone Silverlight 應用程式和 Windows 10 應用程式在從裝置的實際實體大小和解析度抽取 UI 元素的大小與配置時的方式上是不一樣的。 Windows Phone Silverlight 應用程式是使用檢視像素來執行此作業。 Windows 10 的檢視像素概念則已經精簡為有效向素。 以下是該字詞的說明、 它的意義，以及它所提供的額外值。

「解析度」一詞是指像素密度 (而非一般認為的像素計數) 的度量。 「有效解析度」係指在特定檢視距離與裝置之實體像素大小 (像素密度為實體像素大小的倒數) 的差異下，構成影像或字符的實體像素解析成視覺的方式。 有效解析度以使用者為中心，是建置體驗時很好的衡量標準。 藉由了解所有因素及控制 UI 元素的大小，您就能讓使用者享有良好的體驗。

對 Windows Phone Silverlight app 來說，不論手機螢幕實體像素是多少，也不論其像素密度或實體大小為何，所有手機螢幕寬度都正好是 480 檢視像素，無一例外。 這表示 **Image** 元素如果是 `Width="48"`，將正好是任何可執行 Windows Phone Silverlight app 之手機螢幕寬度的 1/10。

在 Windows 10 應用程式中，*並非*所有裝置都有固定的有效圖元數。 這可能相當顯而易見，因為 UWP app 能夠在廣泛的各式裝置上執行。 不同的裝置有不同數量的有效像素範圍，範圍從適用於最小型裝置的 320 epx 到適用於中等大小之監視器的 1024 epx，還有遠遠超過此寬度的更高像素。 您只需一如往常繼續使用自動調整大小元素與動態配置面板。 在某些情況下，也會將 XAML 標記中的 UI 元素屬性設定為固定大小。 縮放比例會根據您的應用程式是在何種裝置上執行，以及使用者所做的顯示設定，自動套用到該應用程式。 此外，該縮放比例還可用來讓大小固定的任何 UI 元素持續在各種不同螢幕大小上，為使用者呈現大約是固定大小的觸控 (及讀取) 目標。 和動態配置一起使用，您的 UI 將不只會在不同裝置上進行光學縮放，還將改為執行必要的動作，以便將適當數量的內容放到可用空間中。

因為 480 之前是手機尺寸螢幕之檢視像素中的固定寬度，而且該值現在在有效像素中通常較小，所以縮圖的規則是將您 Windows Phone Silverlight 應用程式標記中的任何維度乘以 0.8。

為使您的應用程式在所有顯示器上都能有最佳體驗，建議您在某個大小範圍內個別建立適用於各個特定縮放比例的點陣圖資產。 提供 100%、200% 及 400% 的縮放比例 (並以此順序做為其優先順序)，可讓您在大部分情況下利用所有的中繼縮放係數獲得絕佳的結果。

**請注意**  ，如果您基於任何原因而無法建立多個大小的資產，則建立100% 規模的資產。 在 Microsoft Visual Studio 中，UWP app 的預設專案範本只會提供一種大小的商標資產 (磚影像和標誌)，但其縮放比例不是 100%。 在為您自己的應用程式製作資產時，請遵循本節中的指導方針，提供 100%、 200%及 400% 的大小，並使用資產套件。

如果您有複雜的圖檔，您可以用更多大小來提供您的資產。 如果您開始使用向量藝術，則使用任何縮放比例來產生高品質的資產，相對來說就容易許多。

不建議您嘗試支援所有縮放比例，但 Windows 10 應用程式的完整縮放比例清單為 100%、125%、150%、200%、250%、300% 及 400%。 若有提供，市集將會為每個裝置挑選大小正確的資產，同時只會下載那些資產。 市集會根據裝置的 DPI 來選取要下載的資產。

如需詳細資訊，請參閱[適用於 UWP App 的回應式設計入門](https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design)。

## <a name="window-size"></a>視窗大小

在 UWP app 中，您可以使用命令式程式碼來指定最小大小 (寬度與高度)。 預設的最小大小是 500x320epx，這也是可接受的最小大小下限。 可接受的最小大小上限是 500x500epx。

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

下一個主題是 [I/O、裝置與 app 模型的移植](wpsl-to-uwp-input-and-sensors.md)。

## <a name="related-topics"></a>相關主題

* [命名空間與類別對應](wpsl-to-uwp-namespace-and-class-mappings.md)
