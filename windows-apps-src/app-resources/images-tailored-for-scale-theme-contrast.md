---
author: stevewhims
Description: Your app can load image resource files containing images tailored for display scale factor, theme, high contrast, and other runtime contexts.
title: 載入針對縮放比例、佈景主題、高對比及其他設定量身打造的影像和資產
template: detail.hbs
ms.author: stwhi
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: 4db96cea273348b4e1bc7059446f7528ba30a645
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5824455"
---
# <a name="load-images-and-assets-tailored-for-scale-theme-high-contrast-and-others"></a>載入針對縮放比例、佈景主題、高對比及其他設定量身打造的影像和資產
您的應用程式可以載入包含針對[顯示縮放比例](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)、佈景主題、高對比及其他執行階段內容量身打造的影像資源檔案 (或其他資產檔案)。 可以從命令式程式碼或 XAML 標記中參考這些影像，例如 **Image** 的 **Source** 屬性。 也可以出現在應用程式套件資訊清單來源檔案 (`Package.appxmanifest` 檔案) 中 &mdash; 例如，在 Visual Studio 資訊清單設計工具的 \[視覺資產\] 索引標籤上做為 \[App 圖示\] 的值 &mdash; 或顯示在您的磚和快顯通知上。 您可以在影像檔案名稱中使用限定詞，並借助 [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) 選擇性動態載入這些檔案，以便載入最符合使用者顯示縮放比例、佈景主題、高對比、語言及其他內容之執行階段設定的最適當影像檔案。

影像資源是包含在影像資源檔案中。 您也可以將影像視為資產，將包含影像的檔案視為資產檔案；您可以在專案的 \Assets 資料夾中找到這些類型的資源檔案。 如需有關如何在影像資源檔案中使用限定詞的背景資訊，請參閱[針對語言、縮放比例及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md)。

一些適用於影像的常見限定詞有 [scale](tailor-resources-lang-scale-contrast.md#scale)、[theme](tailor-resources-lang-scale-contrast.md#theme)、[contrast](tailor-resources-lang-scale-contrast.md#contrast) 和 [targetsize](tailor-resources-lang-scale-contrast.md#targetsize)。

## <a name="qualify-an-image-resource-for-scale-theme-and-contrast"></a>針對縮放比例、佈景主題和對比限定影像資源
`scale` 限定詞的預設值為 `scale-100`。 因此，這兩個變化對等 (皆提供縮放 100 或縮放比例 1 的影像)。

```
\Assets\Images\logo.png
\Assets\Images\logo.scale-100.png
```

您可以在資料夾名稱 (而不是檔案名稱) 中使用限定詞。 如果每個限定詞各有數個資產檔案，這會是較好的策略。 為了便於說明，這兩個變化相當於上述兩個。

```
\Assets\Images\logo.png
\Assets\Images\scale-100\logo.png
```

接下來是有關如何針對顯示縮放比例、佈景主題及高對比的不同設定提供影像資源 &mdash; 名為 `/Assets/Images/logo.png` &mdash; 變化的範例。 此範例使用資料夾命名格式。

```
\Assets\Images\contrast-standard\theme-dark
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-standard\theme-light
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-high
    \scale-100\logo.png
    \scale-200\logo.png
```

## <a name="reference-an-image-or-other-asset-from-xaml-markup-and-code"></a>從 XAML 標記和程式碼參考影像或其他資產
影像資源的名稱&mdash;或識別碼&mdash;即是移除所有限定詞之後的路徑及檔案名稱。 如果依照上一節任何範例的方式來命名資料夾和/或檔案，您會有單一影像資源，而其名稱 (與絕對路徑相同) 為 `/Assets/Images/logo.png`。 以下說明如何在 XAML 標記中使用該名稱。

```xaml
<Image x:Name="myXAMLImageElement" Source="ms-appx:///Assets/Images/logo.png"/>
```

請注意，您使用的是 `ms-appx` URI 配置，因為您要參考來自您應用程式套件的檔案。 請參閱 [URI 配置](uri-schemes.md)。 以下顯示如何在命令式程式碼中參考相同影像資源。

```csharp
this.myXAMLImageElement.Source = new BitmapImage(new Uri("ms-appx:///Assets/Images/logo.png"));
```

您可以使用 `ms-appx` 從應用程式套件載入任何任意的檔案。

```csharp
var uri = new System.Uri("ms-appx:///Assets/anyAsset.ext");
var storagefile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(uri);
```

`ms-appx-web` 配置將相同的檔案當做 `ms-appx` 來存取，但會在網頁區間中進行。

```xaml
<WebView x:Name="myXAMLWebViewElement" Source="ms-appx-web:///Pages/default.html"/>
```

```csharp
this.myXAMLWebViewElement.Source = new Uri("ms-appx-web:///Pages/default.html");
```

在這些範例示範的案例中，請使用會推斷 [UriKind](https://docs.microsoft.com/en-us/dotnet/api/system.urikind) 的 [Uri 建構函式](https://docs.microsoft.com/en-us/dotnet/api/system.uri.-ctor?view=netcore-2.0#System_Uri__ctor_System_String_)多載。 指定有效的絕對 URI (包括配置和授權單位)，或直接讓授權單位預設為應用程式的套件，如上述範例所示。

注意這些範例中的 URI 配置 ("`ms-appx`" 或 "`ms-appx-web`") 如何在後面加上 "`://`"，再後接絕對路徑。 在絕對路徑中，前置 `/` 會導致路徑從套件的根目錄開始進行解譯。

**注意**：`ms-resource` (代表[字串資源](localize-strings-ui-manifest.md)) 和 `ms-appx(-web)` (代表影像及其他資產) URI 配置會執行自動限定詞比對來尋找最適合目前內容的資源。 `ms-appdata` URI 配置 (用來載入應用程式資料) 不會執行任何這樣的自動比對，但您可以回應 [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 的內容，並使用 URI 中的完整實體檔案名稱從應用程式資料明確載入適當的資產。 如需有關應用程式資料的詳細資訊，請參閱[儲存和擷取設定及其他應用程式資料](../design/app-settings/store-and-retrieve-app-data.md)。 Web URI 配置 (例如，`http`、`https` 和 `ftp`) 也不會執行自動比對。 如需有關這種情況下該執行哪些動作的詳細資訊，請參閱[在雲端裝載和載入影像](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md#hosting-and-loading-images-in-the-cloud)。

如果影像檔案仍位於其原本在專案結構中的位置，絕對路徑會是不錯的選擇。 如果您希望可以移動影像檔案，但很在意要保留相對於其參考 XAML 標記檔案中的相同位置時，您可能需要使用相對於包含所在標記檔案的路徑，而不使用絕對路徑。 如果這樣做，您就不需要使用 URI 配置。 在這種情況下，您仍可從自動限定詞比對受益，但只是因為您是在 XAML 標記中使用相對路徑。

```xaml
<Image Source="Assets/Images/logo.png"/>
```

另請參閱[對語言、縮放比例及高對比的磚與快顯通知支援](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)。

## <a name="qualify-an-image-resource-for-targetsize"></a>針對 targetsize 限定影像資源
您可以對相同影像資源的不同變化使用 `scale` 和 `targetsize` 限定詞，但無法對單一變化的資源同時使用這兩個限定詞。 此外，您至少還需要定義一個不含 `TargetSize` 限定詞的變化。 該變化必須定義 `scale` 的值，不然就讓它預設為 `scale-100`。 因此，`/Assets/Square44x44Logo.png` 資源的這兩個變化是有效的資源。

```
\Assets\Square44x44Logo.scale-200.png
\Assets\Square44x44Logo.targetsize-24.png
```

下列兩個變化都有效。 

```
\Assets\Square44x44Logo.png // defaults to scale-100
\Assets\Square44x44Logo.targetsize-24.png
```

但這個變化就無效。

```
\Assets\Square44x44Logo.scale-200_targetsize-24.png
```

## <a name="refer-to-an-image-file-from-your-app-package-manifest"></a>從應用程式套件資訊清單參考影像檔案
如果依照上一節兩個範例之一的方式來命名資料夾和/或檔案，您會有單一應用程式圖示影像資源，而其名稱 (與相對路徑相同) 為 `Assets\Square44x44Logo.png`。 在應用程式套件資訊清單中，只需依據名稱參考資源即可。 並不需要使用任何 URI 配置。

![新增資源, 英文](images/app-icon.png)

您要做的就只是這樣，作業系統會執行自動限定詞比對來尋找最適合目前內容的資源。 如需應用程式套件資訊清單中所有可當地語系化或依此方式限定的項目清單，請參閱[可當地語系化的資訊清單項目](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)。

## <a name="qualify-an-image-resource-for-layoutdirection"></a>針對 layoutdirection 限定影像資源
請參閱[鏡像影像](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)。

## <a name="load-an-image-for-a-specific-language-or-other-context"></a>載入特定語言或其他內容的影像
如需有關將您的 App 當地語系化的價值主張的詳細資訊，請參閱[全球化和當地語系化](../design/globalizing/globalizing-portal.md)。

預設 [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) (從 [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView) 取得) 包含每個限定詞名稱表示預設執行階段內容的限定詞值 (也就是說，適用於目前使用者及電腦的設定值)。 影像檔案會&mdash;根據其名稱中的限定詞&mdash;比對該執行階段內容中的限定詞值。

不過，您有時可能會想要讓應用程式覆寫系統設定，並明確指定要在尋找所需載入的相符影像時使用的語言、縮放比例或其他限定詞值。 例如，您可能希望確切控制何時和哪些高對比影像要載入。

您可以這樣做，方式為建構新的 **ResourceContext** (而不使用預設的)、覆寫其值，然後在您的影像查閱中使用該內容物件。

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView 
resourceContext.QualifierValues["Contrast"] = "high";
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
var resourceCandidate = namedResource.Resolve(resourceContext);
var imageFileStream = resourceCandidate.GetValueAsStreamAsync().GetResults();
var bitmapImage = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
bitmapImage.SetSourceAsync(imageFileStream);
this.myXAMLImageElement.Source = bitmapImage;
```

為了在全域層級達到相同效果。您*可以*覆寫預設 **ResourceContext** 中的限定詞值。 但我們建議您改為呼叫 [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)。 您可以透過呼叫 **SetGlobalQualifierValue** 將值設定一次，然後這些值就會在每次您用來進行查閱時，於預設 **ResourceContext** 中生效。 根據預設，[**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 類別會使用預設 **ResourceContext**。

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Contrast", "high");
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
this.myXAMLImageElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
```

## <a name="updating-images-in-response-to-qualifier-value-change-events"></a>更新影像以回應限定詞值變更事件
您的執行中應用程式可以回應預設資源內容中受影響限定詞值的系統設定變更。 任何這些系統設定都會叫用 [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 上的 [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) 事件。

您可以借助 [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 預設使用的預設 **ResourceContext** 重新載入影像來回應此事件。

```csharp
public MainPage()
{
    this.InitializeComponent();

    ...

    // Subscribe to the event that's raised when a qualifier value changes.
    var qualifierValues = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
    qualifierValues.MapChanged += new Windows.Foundation.Collections.MapChangedEventHandler<string, string>(QualifierValues_MapChanged);
}

private async void QualifierValues_MapChanged(IObservableMap<string, string> sender, IMapChangedEventArgs<string> @event)
{
    var dispatcher = this.myImageXAMLElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIImages();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIImages());
    }
}

private void RefreshUIImages()
{
    var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
    this.myImageXAMLElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
}
```

## <a name="important-apis"></a>重要 API
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)
* [ResourceContext.SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>相關主題
* [針對語言、縮放比例及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md)
* [將 UI 及應用程式套件資訊清單中的字串當地語系化](localize-strings-ui-manifest.md)
* [儲存和擷取設定及其他應用程式資料](../design/app-settings/store-and-retrieve-app-data.md)
* [對語言、縮放比例及高對比的磚與快顯通知支援](tile-toast-language-scale-contrast.md)
* [可當地語系化的資訊清單項目](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [鏡像影像](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)
* [全球化和當地語系化](../design/globalizing/globalizing-portal.md)
