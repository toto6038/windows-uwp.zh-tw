---
author: stevewhims
description: 以宣告式 XAML 標記形式定義 UI 的做法可以極順利地從 WindowsPhone Silverlight 轉譯至通用 Windows 平台 (UWP) 應用程式。
title: 移植 WindowsPhone Silverlight XAML 與 UI 到 UWP
ms.assetid: 49aade74-5dc6-46a5-89ef-316dbeabbebe
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d219a09ccca74c9fc513b7510c40ce0b90ad9f52
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7553805"
---
#  <a name="porting-windowsphone-silverlight-xaml-and-ui-to-uwp"></a>移植 WindowsPhone Silverlight XAML 與 UI 到 UWP



前一個主題是[疑難排解](wpsl-to-uwp-troubleshooting.md)。

以宣告式 XAML 標記形式定義 UI 的做法可以極順利地從 WindowsPhone Silverlight 轉譯至通用 Windows 平台 (UWP) 應用程式。 您會發現在更新系統資源索引鍵參考、變更某些元素類型名稱，以及將 "clr-namespace" 變更為 "using" 之後，大部分的標記都相容。 展示層中的大多數命令式程式碼 (檢視模型及操作 UI 元素的程式碼) 也會直接移植。

## <a name="a-first-look-at-the-xaml-markup"></a>XAML 標記初探

前一個主題示範如何將 XAML 和程式碼後置複製到新的 windows 10 Visual Studio 專案檔案。 在 Visual Studio XAML 設計工具醒目提示的前幾個問題中，您可能會注意到的其中一個就是位於 XAML 檔案根部的 `PhoneApplicationPage` 元素不適用於通用 Windows 平台 (UWP) 專案。 在前一個主題中，您可以儲存一份 Visual Studio 中建立 windows 10 專案時所產生的 XAML 檔案。 如果您開啟該版本的 MainPage.xaml，您會看到在根部的是 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 類型 (位於 [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716)命名空間中)。 因此，您可以將所有 `<phone:PhoneApplicationPage>` 元素變更為 `<Page>` (請記得使用屬性元素語法)，並且可以刪除 `xmlns:phone` 宣告。

如需更多一般方法，來尋找 WindowsPhone Silverlight 類型相對應的 UWP 類型，您可以參考[命名空間與類別對應](wpsl-to-uwp-namespace-and-class-mappings.md)。

## <a name="xaml-namespace-prefix-declarations"></a>XAML 命名空間前置詞宣告


如果您在檢視中使用自訂類型執行個體 (可能是檢視模型執行個體或值轉換器)，則您的 XAML 標記中將會有 XAML 命名空間前置詞宣告。 這些項目的語法不同 WindowsPhone Silverlight 與 UWP 之間。 以下是一些範例：

```xml
    xmlns:ContosoTradingCore="clr-namespace:ContosoTradingCore;assembly=ContosoTradingCore"
    xmlns:ContosoTradingLocal="clr-namespace:ContosoTradingLocal"
```

請將 "clr-namespace" 變更為 "using"，並刪除任何組件語彙基元和分號 (將會推斷組件)。 結果看起來就像這樣：

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

**BitmapImage**是**System.Windows.Media.Imaging**命名空間中 WindowsPhone Silverlight，並使用相同的檔案中的指示詞可讓**BitmapImage**不如上述的程式碼片段所示的命名空間限定的情況下使用。 在這種情況下，您可以在 Visual Studio 中於類型名稱 (**BitmapImage**) 上按一下滑鼠右鍵，然後使用操作功能表上的 [**解析**] 命令將新的命名空間指示詞新增到檔案中。 在此情況下，會新增 [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) 命名空間，這是類型在 UWP 中的所在位置。 您可以移除 **System.Windows.Media.Imaging** using 指示詞，只要這樣就可以移植如上述程式碼片段中那樣的程式碼。 當您完成後時，您將會移除所有 WindowsPhone Silverlight 命名空間。

在像這樣的簡單案例中，亦即將舊命名空間中的類型對應到新命名空間中的相同類型，您可以使用 Visual Studio 的 [**尋找和取代**] 命令對原始程式碼進行大量變更。 [**解析**] 命令是一個探索類型之新命名空間的絕佳方法。 再舉一例，您可以使用 "Windows.UI.Xaml" 來取代所有 "System.Windows"。 基本上，這將會移植所有 using 指示詞和所有參考該命名空間的完整類型名稱。

在移除所有舊的 using 指示詞並加入新的指示詞之後，您便可以使用 Visual Studio 的 [**組合管理 Using**] 命令來排序您的指示詞及移除不使用的指示詞。

有時候，修正命令式程式碼就像變更參數類型一樣，只是小事一樁。 其他時候，您將需要使用 UWP Api，而不是.NET Api 適用於 Windows 執行階段 8.x 應用程式。 若要識別哪些 Api 受支援，請使用搭配[.NET 適用於 Windows 執行階段 8.x 應用程式的概觀](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)與[Windows 執行階段參考](https://msdn.microsoft.com/library/windows/apps/br211377)此移植指南的其餘部分。

而如果您只是想要到達專案建置階段，您可以將任何非必要程式碼標成註解或清除。 然後逐一查看問題，並參閱本節 (與上一個主題：[疑難排解](wpsl-to-uwp-troubleshooting.md)) 中的下列主題，直到任何建置與執行階段問題都已解決，且您的移植已完成為止。

## <a name="adaptiveresponsive-ui"></a>調適型/回應式 UI

因為您的 windows 10 應用程式可以在許多不同的各種裝置上執行 — 每一個都有它自己的螢幕大小與解析度 — 您會想要透過最少的步驟，來移植您的應用程式，您會想要量身打造您的 UI 以呈現最佳外觀在那些裝置上。 您可以使用調適型 Visual State Manager 功能來動態偵測視窗大小及變更回應配置，Bookstore2 案例研究主題的[調適型 UI](wpsl-to-uwp-case-study-bookstore2.md) 中已經提供此做法的範例。

## <a name="alarms-and-reminders"></a>鬧鐘和提醒

程式碼如果使用 **Alarm** 或 **Reminder** 類別，應該移植並改用 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 類別來建立和登錄背景工作，以及在相關時間顯示快顯通知。 請參閱[背景處理](wpsl-to-uwp-business-and-data.md)和[快顯通知](#toasts)。

## <a name="animation"></a>動畫

UWP app 現在可以使用 UWP 動畫庫，做為主要畫面格動畫及 from/to 動畫的慣用替代方案。 這些動畫已設計和微調成能夠順暢執行、看起來美觀，以及讓您的應用程式看起來就像內建的應用程式一樣與 Windows 整合在一起。 請參閱[快速入門：使用動畫庫讓 UI 產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)。

如果您在 UWP app 中使用主要畫面格動畫或 from/to 動畫，您可能會想要了解新平台導入的獨立式和相依式動畫之間的區別。 請參閱[最佳化動畫和媒體](https://msdn.microsoft.com/library/windows/apps/mt204774)。 在 UI 執行緒上執行的動畫 (例如讓配置屬性產生動畫效果的動畫) 稱為「相依式動畫」，在新平台上執行時，除非您執行下列兩個動作之一，否則它們將不會有任何作用。 您可以將它們的動畫作用目標重新設定為不同的屬性 (例如 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980))，讓它們變成獨立式動畫。 或者，在動畫元素上設定 `EnableDependentAnimation="True"`，以確認想要執行無法保證能夠順暢執行的動畫。 如果您使用 Blend for Visual Studio 來撰寫新的動畫，則會在必要的地方為您設定該屬性。

## <a name="back-button-handling"></a>[返回] 按鈕處理

在 windows 10 app 中，您可以使用單一方式來處理返回按鈕，它將會在所有裝置上運作。 在行動裝置上，按鈕會在裝置上以電容式按鈕的方式，或以殼層中的按鈕的方式提供您使用。 在傳統型裝置上，您可以在只要能夠於應用程式中進行反向瀏覽時，就在應用程式組建區塊中加入一個按鈕，而且這會顯示在視窗化應用程式的標題列中，或平板電腦模式的工作列中。 返回按鈕事件是所有裝置系列的一個通用概念，且以硬體或軟體方式實作的按鈕都可以引發相同的 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 事件。

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

所有這些方面大多仍受到支援，但有命名空間差異。 例如，**System.Windows.Data.Binding** 會對應到 [**Windows.UI.Xaml.Data.Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)、**System.ComponentModel.INotifyPropertyChanged** 會對應到 [**Windows.UI.Xaml.Data.INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/br209899)，而 **System.Collections.Specialized.INotifyPropertyChanged** 會對應到 [**Windows.UI.Xaml.Interop.INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/hh702001)。

如同他們可以在 UWP app 中，無法繫結 WindowsPhone Silverlight 應用程式列和應用程式列按鈕。 您可能有命令式程式碼來建構應用程式列及其按鈕、將它們繫結到屬性和已當地語系化的字串，並處理其活動。 如果是的話，您現在可以選擇移植該命令式程式碼，方法是以繫結到屬性和命令的宣告式標記，以及以靜態資源參考，來取代該命令式程式碼，這樣會讓您的應用程式安全性漸增且更容易維護。 您可以使用 Visual Studio 或 Blend for Visual Studio 來繫結 UWP app 列按鈕並為其設定樣式，就像任何其他 XAML 元素一樣。 請注意，在 UWP app 中，您使用的類型名稱為 [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/dn279427) 和 [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244)。

UWP app 的繫結相關功能目前有下列限制：

-   未針對資料輸入驗證以及 [**IDataErrorInfo**](https://msdn.microsoft.com/library/system.componentmodel.idataerrorinfo.aspx) 和 [**INotifyDataErrorInfo**](https://msdn.microsoft.com/library/system.componentmodel.inotifydataerrorinfo.aspx) 介面提供內建支援。
-   [**繫結**](https://msdn.microsoft.com/library/windows/apps/br209820)的類別不包含的格式化屬性延伸的 WindowsPhone Silverlight 中提供。 不過，您仍然可以實作 [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903) 來提供自訂格式。
-   [
              **IValueConverter**
            ](https://msdn.microsoft.com/library/windows/apps/br209903) 方法將語言字串當作參數而不是 [**CultureInfo**](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx) 物件。
-   [
              **CollectionViewSource**
            ](https://msdn.microsoft.com/library/windows/apps/br209833) 類別不提供排序和篩選的內建支援，而群組的運作方式也不一樣。 如需詳細資訊，請參閱[深入了解資料繫結](https://msdn.microsoft.com/library/windows/apps/mt210946)和[資料繫結範例](http://go.microsoft.com/fwlink/p/?linkid=226854)。

雖然大多仍受到支援相同的繫結功能，windows 10 提供新的選項，且更多的高效能繫結機制，稱為編譯的繫結，使用 {X:bind} 標記延伸。 請參閱[資料繫結：透過 XAML 資料繫結的全新增強功能來提升您應用程式的效能](http://channel9.msdn.com/Events/Build/2015/3-635)，以及 [x:Bind 範例](http://go.microsoft.com/fwlink/p/?linkid=619989)。

## <a name="binding-an-image-to-a-view-model"></a>將影像繫結到檢視模型

您可以將 [**Image.Source**](https://msdn.microsoft.com/library/windows/apps/br242760) 屬性繫結到類型為 [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/br210107) 的任何檢視模型屬性。 以下是 WindowsPhone Silverlight 應用程式中的這類屬性的典型實作：

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

在 UWP app 中，您將使用 ms-appx [URI 配置](https://msdn.microsoft.com/library/windows/apps/jj655406)。 如此，您便可以讓程式碼的其餘部分保持不變，您可以使用不同的 **System.Uri** 建構函式多載將 ms-appx URI 配置放在基底 URI 中，然後將路徑的其餘部分附加至其上。 就像這樣：

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

這樣一來，檢視模型的其餘部分、影像路徑屬性中的路徑值，以及 XAML 標記中的繫結，都可以完全保持不變。

## <a name="controls-and-control-stylestemplates"></a>控制項與控制項樣式/範本

WindowsPhone Silverlight 應用程式使用**Microsoft.Phone.Controls**命名空間和**System.Windows.Controls**命名空間中定義的控制項。 XAML UWP app 會使用 [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) 命名空間中定義的控制項。 架構和 UWP 中 XAML 控制項的設計是幾乎與 WindowsPhone Silverlight 控制項相同。 但是，目前已進行一些變更來改善該組可用的控制項，並將它們與 Windows 應用程式整合。 以下是特定範例。

| 控制項名稱 | 變更 |
|--------------|--------|
| ApplicationBar | [Page.TopAppBar](https://msdn.microsoft.com/library/windows/apps/hh702575) 屬性。 |
| ApplicationBarIconButton | UWP 對等項目是 [Glyph](https://msdn.microsoft.com/library/windows/apps/dn279538) 屬性。 PrimaryCommands 是 CommandBar 的內容屬性。 XAML 剖析器會將元素的內部 XML 解譯成其內容屬性的值。 |
| ApplicationBarMenuItem | UWP 對等項目是設為功能表項目文字的 [AppBarButton.Label](https://msdn.microsoft.com/library/windows/apps/dn279261)。 |
| ContextMenu (在 Windows Phone 工具組中) | 針對單一選擇飛出視窗，請使用 [[飛出視窗](https://msdn.microsoft.com/library/windows/apps/dn279496)]。 |
| ControlTiltEffect.TiltEffect 類別 | 來自 UWP 動畫庫的動畫已內建至通用控制項的預設「樣式」中。 請參閱[讓指標動作產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)。 |
| LongListSelector 搭配已分組的資料 | 兩種方式，可用於音樂會 WindowsPhone Silverlight LongListSelector 函式。 首先，可以顯示依索引鍵分組的資料，例如依第一個字母分組的名稱清單。 其次，可以在下列兩種語意式檢視之間「縮放」：已分組的項目 (例如名稱) 清單和只有群組索引鍵本身 (例如第一個字母) 的清單。 運用 UWP，您可以使用[清單和格線檢視控制項的指導方針](https://msdn.microsoft.com/library/windows/apps/mt186889)來顯示已分組的資料。 |
| LongListSelector 搭配一般資料 | 基於效能考量，對於很長的清單，我們建議使用 LongListSelector，而不是 WindowsPhone Silverlight 清單方塊，即使的一般、 非分組的資料。 在 UWP app 中，針對長的項目清單 (不論是否可將資料加以分組)，皆建議使用 [GridView](https://msdn.microsoft.com/library/windows/apps/br242705)。 |
| Panorama | WindowsPhone Silverlight Panorama 控制項對應至[Windows 執行階段 8.x app hub 控制項的指導方針](https://msdn.microsoft.com/library/windows/apps/dn449149)與指導方針中樞控制項。 <br/> 請注意，Panorama 控制項會將最後一個區段迴繞到第一個區段，且其背景影像會以和區段相對的視差方式移動。 [Hub](https://msdn.microsoft.com/library/windows/apps/dn251843) 區段不會迴繞，且不會使用視差。 |
| 樞紐分析 | UWP WindowsPhone Silverlight Pivot 控制項相等的是[Windows.UI.Xaml.Controls.Pivot](https://msdn.microsoft.com/library/windows/apps/dn608241)。 它適用於所有裝置系列。 |

**注意：**  PointerOver 視覺狀態是相關的自訂樣式/範本在 windows 10 應用程式，但不是在 WindowsPhone Silverlight 應用程式。 還有其他原因造成現有自訂樣式/範本可能適用於 windows 10 應用程式，包括您使用的系統資源索引鍵組視覺狀態和效能改進的 windows 10 預設樣式所做的變更 /範本 \]。 我們建議您針對 windows 10 編輯控制項的預設範本的全新副本，並重新套用樣式與範本自訂項目。

如需 UWP 控制項的詳細資訊，請參閱[依功能分類的控制項](https://msdn.microsoft.com/library/windows/apps/mt185405)、[控制項清單](https://msdn.microsoft.com/library/windows/apps/mt185406)，以及[控制項的指導方針](https://msdn.microsoft.com/library/windows/apps/dn611856)。

##  <a name="design-language-in-windows10"></a>Windows 10 中的設計語言

有一些在設計語言中 WindowsPhone Silverlight 應用程式和 windows 10 應用程式之間的差異。 如需所有詳細資訊，請參閱[設計](http://dev.windows.com/design)。 儘管設計語言會變更，但我們的設計原則仍會保持一致：留意細節，但為了簡單起見，儘量將重點放在內容不是組件區塊、將視覺元素降至最低，並保留數位網域的驗證；使用視覺層次，特別是使用印刷格式；設計格線；以及使用流暢的動畫讓您的體驗變得更生動。

## <a name="localization-and-globalization"></a>當地語系化和全球化

針對當地語系化字串，您可以重新將.resx 檔案從您的 WindowsPhone Silverlight 專案使用您的 UWP 應用程式專案中。 請將檔案複製過去、將它新增到專案中，然後將它重新命名為 Resources.resw，以便讓查詢機制預設會尋找它。 將 [**建置動作**] 設定為 [**PRIResource**]，將 [**複製到輸出目錄**] 設定為 [**不要複製**]。 然後，您便可以藉由在 XAML 元素上指定 **X:uid** 屬性，在標記中使用這些字串。 請參閱[快速入門：使用字串資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329)。

WindowsPhone Silverlight 應用程式使用**CultureInfo**類別來協助將 app 全球化。 UWP app 使用 MRT (現代資源技術)，不論是在執行階段和 Visual Studio 設計介面中，都能動態載入 app 資源 (當地語系化、縮放及佈景主題)。 如需詳細資訊，請參閱[檔案、資料和全球化的指導方針](https://msdn.microsoft.com/library/windows/apps/dn611859)。

[
              **ResourceContext.QualifierValues**
            ](https://msdn.microsoft.com/library/windows/apps/br206071) 主題說明如何根據裝置系列資源選擇因素載入裝置系列特定資源。

## <a name="media-and-graphics"></a>媒體和圖形

當您閱讀 UWP 媒體和圖形的相關資料時，請記住，Windows 設計原則鼓勵大幅減少任何多餘的項目，包括圖形複雜性和雜亂度。 Windows 設計是以乾淨簡潔的視覺效果、印刷樣式及移動為代表。 如果您的 app 遵守相同的原則，它看起來就會更像內建的 app。

WindowsPhone Silverlight 具有**RadialGradientBrush**類型不是出現在 UWP 中，儘管其他[**筆刷**](/uwp/api/Windows.UI.Xaml.Media.Brush)類型。 在某些情況下，您將可藉由點陣圖獲得類似的效果。 請注意，您可以在 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 和 XAML C++ UWP 中使用 Direct2D 來[建立放射狀漸層筆刷](https://msdn.microsoft.com/library/windows/desktop/dd756679)。

WindowsPhone Silverlight 有**System.Windows.UIElement.OpacityMask**屬性，但該屬性不是 UWP [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)類型的成員。 在某些情況下，您將可藉由點陣圖獲得類似的效果。 而您可以在 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 和 XAML C++ UWP app 中使用 Direct2D 來[建立不透明度遮罩](https://msdn.microsoft.com/library/windows/desktop/ee329947)。 但是 **OpacityMask** 的一般使用案例是使用同時適合淺色和深色佈景主題的單一點陣圖。 針對向量圖形，您可以使用佈景主題感知系統筆刷 (例如下面所述的圓形圖)。 但是製作佈景主題感知點陣圖 (例如下面所述的核取記號) 需要不同的方法。

![佈景主題感知點陣圖](images/wpsl-to-uwp-case-studies/wpsl-to-uwp-theme-aware-bitmap.png)

在 WindowsPhone Silverlight 應用程式中，技巧是將 alpha 遮罩 （以點陣圖形式） 做為**OpacityMask** **矩形**填滿前景筆刷：

```xml
    <Rectangle Fill="{StaticResource PhoneForegroundBrush}" Width="26" Height="26">
        <Rectangle.OpacityMask>
            <ImageBrush ImageSource="/Assets/wpsl_check.png"/>
        </Rectangle.OpacityMask>
    </Rectangle>
```

將這個移植到 UWP app 的最直接方式是使用 [**BitmapIcon**](https://msdn.microsoft.com/library/windows/apps/dn279306)，就像這樣：

```xml
    <BitmapIcon UriSource="Assets/winrt_check.png" Width="21" Height="21"/>
```

這裡的 winrt\_check.png 是點陣圖形式的 Alpha 遮罩 ，就像 wpsl\_check.png，而且很可能是相同的檔案。 不過，您可能會想要提供數個不同大小的 winrt\_check.png 以供不同的縮放比例使用。 如需有關該事項的詳細資訊，以及對 **Width** 和 **Height** 值所做之變更的說明，請參閱本主題中的[檢視或有效像素、檢視距離及縮放比例](#view-or-effective-pixels-viewing-distance-and-scale-factors)。

如果點陣圖的淺色和深色形式有差異，有一個適合此情況的更普遍方法，就是使用兩個影像資產：一個具有深色前景 (用於淺色佈景主題)，另一個具有淺色前景 (用於深色佈景主題)。 如需如何命名這組點陣圖資產的進一步詳細資訊，請參閱[量身打造您的語言、 縮放比例及其他限定詞的資源](../app-resources/tailor-resources-lang-scale-contrast.md)。 正確命名一組影像檔之後，您便可以在摘要中使用它們的根名稱來參考它們，就像這樣：

```xml
    <Image Source="Assets/winrt_check.png" Stretch="None"/>
```

在 WindowsPhone Silverlight **UIElement.Clip**屬性可以是任何形狀，您可以快速**幾何**，並會序列化**StreamGeometry**迷你語言中的 XAML 標記。 在 UWP 中，[**Clip**](https://msdn.microsoft.com/library/windows/apps/br208919) 屬性的類型是 [**RectangleGeometry**](https://msdn.microsoft.com/library/windows/apps/br210259)，因此您只能裁剪出矩形區域。 允許使用迷你語言來定義矩形會太寬鬆。 因此，若要移植標記中的裁剪區域，請取代 **Clip** 屬性語法並將它轉換成屬性元素語法，就像下面這樣：

```xml
    <UIElement.Clip>
        <RectangleGeometry Rect="10 10 50 50"/>
    </UIElement.Clip>
```

請注意，您可以在 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 與 XAML C++ UWP app 中使用 Direct2D，以[使用任意幾何圖形做為圖層中的遮罩](https://msdn.microsoft.com/library/windows/desktop/dd756654)。

## <a name="navigation"></a>瀏覽

當您瀏覽到 WindowsPhone Silverlight 應用程式中的頁面時，您可以使用 「 統一資源識別項 (URI) 定址配置：

```csharp
    NavigationService.Navigate(new Uri("/AnotherPage.xaml", UriKind.Relative)/*, navigationState*/);
```

在 UWP app 中，您則會呼叫 [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 方法，並指定目的地頁面的類型 (由頁面之 XAML 標記定義的 **x:Class** 屬性所定義)：


```csharp
    // In a page:
    this.Frame.Navigate(typeof(AnotherPage)/*, parameter*/);

    // In a view model, perhaps inside an ICommand implementation:
    var rootFrame = Windows.UI.Xaml.Window.Current.Content as Windows.UI.Xaml.Controls.Frame;
    rootFrame.Navigate(typeof(AnotherPage)/*, parameter*/);
```

您可以在 WMAppManifest.xml 中定義的 WindowsPhone Silverlight 應用程式的啟動頁：

```xml
    <DefaultTask Name="_default" NavigationPage="MainPage.xaml" />
```

在 UWP app 中，您將使用命令式程式碼來定義啟動頁。 以下是一些來自 App.xaml.cs 並說明做法的程式碼：

```csharp
    if (!rootFrame.Navigate(typeof(MainPage), e.Arguments))
```

URI 對應和片段瀏覽是 URI 瀏覽技術，因此不適用於並非以 URI 為基礎的 UWP 瀏覽。 URI 對應的存在是為了因應以 URI 字串識別目標頁面而具有的弱式型別本質，該方式會使得頁面在移到不同的資料夾，繼而移到不同的相對路徑時，導致脆弱易損和可維護性問題。 UWP app 使用以類型為基礎的瀏覽，這是一種強型別並受編譯器檢查的瀏覽，沒有 URI 對應所要解決的問題。 片段瀏覽的使用案例是將一些內容傳遞給目標頁面，讓頁面能夠將其內容的特定片段捲動到檢視中，將以其他方式顯示。 在您呼叫 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 方法時傳遞瀏覽參數，也可達成相同的目標。

如需詳細資訊，請參閱[瀏覽](https://msdn.microsoft.com/library/windows/apps/mt187344)。

## <a name="resource-key-reference"></a>資源索引鍵參考

適用於 windows 10 的設計語言已進化許多，因此某些系統樣式也已經改變，和許多系統資源索引鍵已經移除或重新命名。 Visual Studio 中的 XAML 標記編輯器會醒目提示無法解析的資源索引鍵參考。 例如，XAML 標記編輯器會在樣式索引鍵 `PhoneTextNormalStyle` 的參考加上紅色波浪底線。 如果未修正該參考，當您嘗試將應用程式部署到模擬器或裝置上時，它將會立即終止。 因此，請務必注意 XAML 標記的正確性。 您會發現 Visual Studio 是攔截這類問題的絕佳工具。

另請參閱下面的[文字](#text)。

## <a name="status-bar-system-tray"></a>狀態列 (系統匣)

系統匣 (在 XAML 標記中以 `shell:SystemTray.IsVisible` 設定) 現在稱為狀態列，並且預設會顯示。 您可以在命令式程式碼中，藉由呼叫 [**Windows.UI.ViewManagement.StatusBar.ShowAsync**](https://msdn.microsoft.com/library/windows/apps/dn610343) 和 [**HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339) 方法來控制其可見度。

## <a name="text"></a>文字

文字 (或印刷樣式) 是 UWP app 中的一個重要層面，在移植時，您可以重新檢閱檢視的視覺設計，以確保它們不會與新的設計語言產生違和感。 請使用這些插圖說明來找出可用的 UWP  **TextBlock** 系統樣式。 找出感與您用 WindowsPhone Silverlight 樣式對應。 或者，您可以建立自己的通用樣式，並將內容從 WindowsPhone Silverlight 系統樣式複製到那些。

![適用於 Windows 10 app 的系統 textblock 樣式](images/label-uwp10stylegallery.png)

適用於 windows 10 app 的系統 TextBlock 樣式

在 WindowsPhone Silverlight 應用程式中，預設字型系列為 Segoe WP。 在 windows 10 app 中，預設字型系列為 Segoe UI。 因此，您應用程式中的字型標準可能會看起來不一樣。 如果您想要重現 WindowsPhone Silverlight 文字的外觀，您可以設定您自己的標準使用[**LineHeight**](https://msdn.microsoft.com/library/windows/apps/br209671)和[**LineStackingStrategy**](https://msdn.microsoft.com/library/windows/apps/br244362)等屬性。 如需詳細資訊，請參閱[字型的指導方針](https://msdn.microsoft.com/library/windows/apps/hh700394.aspx)和[設計 UWP app](http://dev.windows.com/design)。

## <a name="theme-changes"></a>佈景主題變更

WindowsPhone Silverlight 應用程式中，預設佈景主題預設是深色。 適用於 windows 10 裝置，預設佈景主題已經變更，但是您可以控制藉由宣告要求的佈景主題，在 App.xaml 中使用的佈景主題。 例如，若要在所有裝置上使用深色佈景主題，可將 `RequestedTheme="Dark"` 新增到根應用程式元素。

## <a name="tiles"></a>磚

適用於 UWP app 的磚的行為與類似 WindowsPhone Silverlight 應用程式，動態磚，雖然仍有些微差異。 例如，程式碼如果會呼叫 **Microsoft.Phone.Shell.ShellTile.Create** 方法來建立次要磚，應該移植並改成呼叫 [**SecondaryTile.RequestCreateAsync**](https://msdn.microsoft.com/library/windows/apps/br230606)。 以下是一個前和移植後的範例，首先 WindowsPhone Silverlight 版本：


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

應用程式碼如果使用 **Microsoft.Phone.Shell.ShellTile.Update** 方法或 **Microsoft.Phone.Shell.ShellTileSchedule** 類別來更新磚，應該移植並改用 [**TileUpdateManager**](https://msdn.microsoft.com/library/windows/apps/br208622)、[**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628)、[**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) 和 (或) [**ScheduledTileNotification**](https://msdn.microsoft.com/library/windows/apps/hh701637) 類別。

如需有關磚、快顯通知、徽章、橫幅及通知的詳細資訊，請參閱[建立磚](https://msdn.microsoft.com/library/windows/apps/xaml/hh868260)和[使用磚、徽章以及快顯通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259)。 如需有關用於 UWP 磚之視覺資產大小的詳細資訊，請參閱[磚與快顯通知視覺資產](https://msdn.microsoft.com/library/windows/apps/hh781198)。

## <a name="toasts"></a>快顯通知

應用程式碼如果使用 **Microsoft.Phone.Shell.ShellToast** 類別來顯示快顯通知，應該移植並改用 [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642)、[**ToastNotifier**](https://msdn.microsoft.com/library/windows/apps/br208653)、[**ToastNotification**](https://msdn.microsoft.com/library/windows/apps/br208641) 和 (或) [**ScheduledToastNotification**](https://msdn.microsoft.com/library/windows/apps/br208607) 類別。 請注意，在行動裝置上，「快顯通知」就面向消費者的詞彙而言就是「橫幅」。

請參閱[使用磚、徽章以及快顯通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259)。

## <a name="view-or-effective-pixels-viewing-distance-and-scale-factors"></a>檢視或有效像素、檢視距離及縮放比例

WindowsPhone Silverlight 應用程式和 windows 10 應用程式不同它們大小離的實際實體大小的 UI 元素的配置和解析度抽取與裝置的方式。 WindowsPhone Silverlight 應用程式使用檢視像素來執行此動作。 使用 windows 10，檢視像素的概念有已經精簡為有效像素的。 以下是該字詞的說明、 它的意義，以及它所提供的額外值。

「解析度」一詞是指像素密度 (而非一般認為的像素計數) 的度量。 「有效解析度」係指在特定檢視距離與裝置之實體像素大小 (像素密度為實體像素大小的倒數) 的差異下，構成影像或字符的實體像素解析成視覺的方式。 有效解析度以使用者為中心，是建置體驗時很好的衡量標準。 藉由了解所有因素及控制 UI 元素的大小，您就能讓使用者享有良好的體驗。

對 WindowsPhone Silverlight 應用程式中，所有手機螢幕都都正好是 480 檢視像素寬、 無一例外，無論有多個實體像素畫面，也不論其像素密度或實體大小為何。 這表示使用**影像**項目`Width="48"`將會完全任何可以執行 WindowsPhone Silverlight 應用程式之手機螢幕寬度的 1/10。

Windows 10 應用程式中，它是** 是所有裝置一些情況固定的有效像素寬的數目。 這可能相當顯而易見，因為 UWP app 能夠在廣泛的各式裝置上執行。 不同的裝置有不同數量的有效像素範圍，範圍從適用於最小型裝置的 320 epx 到適用於中等大小之監視器的 1024 epx，還有遠遠超過此寬度的更高像素。 您只需一如往常繼續使用自動調整大小元素與動態配置面板。 在某些情況下，也會將 XAML 標記中的 UI 元素屬性設定為固定大小。 縮放比例會根據您的應用程式是在何種裝置上執行，以及使用者所做的顯示設定，自動套用到該應用程式。 此外，該縮放比例還可用來讓大小固定的任何 UI 元素持續在各種不同螢幕大小上，為使用者呈現大約是固定大小的觸控 (及讀取) 目標。 和動態配置一起使用，您的 UI 將不只會在不同裝置上進行光學縮放，還將改為執行必要的動作，以便將適當數量的內容放到可用空間中。

因為 480 之前的固定的寬度在檢視中像素手機尺寸螢幕，而且該值現在在有效像素中通常較小，法則是將 WindowsPhone Silverlight 應用程式標記中的任何維度乘以 0.8。

為使您的應用程式在所有顯示器上都能有最佳體驗，建議您在某個大小範圍內個別建立適用於各個特定縮放比例的點陣圖資產。 提供 100%、200% 及 400% 的縮放比例 (並以此順序做為其優先順序)，可讓您在大部分情況下利用所有的中繼縮放係數獲得絕佳的結果。

**注意：** 如果，如無論原因為何，您無法資產建立一個以上的大小，請建立縮放比例 100%的資產。 在 Microsoft Visual Studio 中，UWP app 的預設專案範本只會提供一種大小的商標資產 (磚影像和標誌)，但其縮放比例不是 100%。 在為您自己的應用程式製作資產時，請遵循本節中的指導方針，提供 100%、 200%及 400% 的大小，並使用資產套件。

如果您有複雜的圖檔，您可以用更多大小來提供您的資產。 如果您開始使用向量藝術，則使用任何縮放比例來產生高品質的資產，相對來說就容易許多。

我們不建議您嘗試支援所有的縮放比例，但適用於 windows 10 app 的縮放比例的完整清單是 100%、 125%、 150%、 200%、 250%、 300%及 400%。 若有提供，市集將會為每個裝置挑選大小正確的資產，同時只會下載那些資產。 市集會根據裝置的 DPI 來選取要下載的資產。

如需詳細資訊，請參閱[適用於 UWP App 的回應式設計入門](https://msdn.microsoft.com/library/windows/apps/dn958435)。

## <a name="window-size"></a>視窗大小

在 UWP app 中，您可以使用命令式程式碼來指定最小大小 (寬度與高度)。 預設的最小大小是 500x320epx，這也是可接受的最小大小下限。 可接受的最小大小上限是 500x500epx。

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

下一個主題是 [I/O、裝置與 app 模型的移植](wpsl-to-uwp-input-and-sensors.md)。

## <a name="related-topics"></a>相關主題

* [命名空間與類別對應](wpsl-to-uwp-namespace-and-class-mappings.md)
