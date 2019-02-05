---
Description: This topic explains the general concept of qualifiers, how to use them, and the purpose of each of the qualifier names.
title: 針對語言、縮放比例、高對比及其他限定詞量身打造您的資源
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: 82dd3d20aa39ea471618e7707d066c67a6547f9f
ms.sourcegitcommit: b975c8fc8cf0770dd73d8749733ae5636f2ee296
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2019
ms.locfileid: "9058749"
---
# <a name="tailor-your-resources-for-language-scale-high-contrast-and-other-qualifiers"></a>針對語言、縮放比例、高對比及其他限定詞量身打造您的資源

本主題說明資源限定詞的一般概念、其使用方式，以及每個限定詞名稱的用途。 如需所有可能限定詞值的參考表，請參閱 [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)。

您的應用程式可以載入專為執行階段內容量身訂做的資產和資源，例如顯示語言、高對比、[顯示縮放比例](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)，以及許多其他內容。 執行此動作的方式是命名資源的檔案或資料夾，以符合對應至這些內容的限定詞名稱及限定詞值。 例如，您可能希望應用程式在高對比模式下載入不同的一組影像資產。

如需有關將您的 App 當地語系化的價值主張的詳細資訊，請參閱[全球化和當地語系化](../design/globalizing/globalizing-portal.md)。

## <a name="qualifier-name-qualifier-value-and-qualifier"></a>限定詞名稱、限定詞值和限定詞

限定詞名稱是對應至一組限定詞值地圖的索引鍵。 以下是對比的限定詞名稱及限定詞值。

| 內容 | 限定詞名稱 | 限定詞值 |
| :--------------- | :--------------- | :--------------- |
| 高對比設定 | 對比 | 標準、高、黑色、白色 |

您可以將限定詞名稱與限定詞值結合來形成一個限定詞。 `<qualifier name>-<qualifier value>` 是限定詞的格式。 `contrast-standard` 是限定詞的範例。

因此，高對比適用的一組限定詞是 `contrast-standard`、`contrast-high`、`contrast-black` 和 `contrast-white`。 限定詞名稱及限定詞值不區分大小寫。 例如，`contrast-standard` 和 `Contrast-Standard` 是相同的限定詞。

## <a name="use-qualifiers-in-folder-names"></a>在資料夾名稱中使用限定詞

以下是使用限定詞來命名包含資產檔案之資料夾的範例。 如果每個限定詞有數個資產檔案，請在資料夾名稱中使用限定詞。 如此一來，您在資料夾層級設定限定詞一次，限定詞就會套用至資料夾中的所有項目。

```console
\Assets\Images\contrast-standard\<logo.png, and other image files>
\Assets\Images\contrast-high\<logo.png, and other image files>
\Assets\Images\contrast-black\<logo.png, and other image files>
\Assets\Images\contrast-white\<logo.png, and other image files>
```

如果您為資料夾命名 (如上述範例所示)，應用程式就會使用高對比設定從適當限定詞命名的資料夾載入資源檔案。 因此，如果設定是 [高對比黑色]，則會載入 `\Assets\Images\contrast-black` 資料夾中的資源檔案。 如果設定為 [無] (也就是，電腦未使用高對比模式)，則載入 `\Assets\Images\contrast-standard` 資料夾中的資源檔案。

## <a name="use-qualifiers-in-file-names"></a>在檔案名稱中使用限定詞

您不必建立和命名資料夾，反而可以使用限定詞來命名資源檔案本身。 如果每個限定詞只有一個資源檔案，您可能會想要這樣做。 範例如下。

```console
\Assets\Images\logo.contrast-standard.png
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.contrast-black.png
\Assets\Images\logo.contrast-white.png
```

名稱含有最適合設定之限定詞的檔案就是已載入的檔案。 這個比對邏輯的運作方式對檔案名稱和資料夾名稱來說都一樣。

## <a name="reference-a-string-or-image-resource-by-name"></a>依名稱參考字串或影像資源

請參閱[從 XAML 標記參考字串資源識別碼](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-xaml-markup)、[從程式碼參考字串資源識別碼](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-code)和[從 XAML 標記和程式碼參考影像或其他資產](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)。

## <a name="actual-and-neutral-qualifier-matches"></a>實際和中性限定詞相符項目
您不需要為*每個*限定詞值提供資源檔案。 例如，若您發現高對比和標準對比都分別只需使用一個視覺資產，您可以像這樣來命名這些資產。

```console
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.png
```

第一個檔案名稱包含 `contrast-high` 限定詞。 當高對比為*開啟*狀態時，該限定詞是任何高對比設定的*實際*相符項目。 換句話說，這是密切相符項目，因此優先選用。 只有在限定詞包含*實際*值時，才會出現*實際*相符項目，正如此例的情況。 在此情況下，`high` 是 `contrast` 的*實際*值。

名稱為 `logo.png` 的檔案根本沒有對比限定詞。 缺少限定詞即是*中性*值。 如果找不到首選相符項目，則使用中性值做為遞補相符項目。 在此範例中，如果高對比為*關閉*狀態，則沒有實際相符項目。 *中性*相符項目是可找到的最佳相符項目，因此將資產 `logo.png` 載入。

如果您要將 `logo.png` 的名稱變更為 `logo.contrast-standard.png`，則檔案名稱會包含實際限定詞值。 在高對比關閉的情況下，會有包含 `logo.contrast-standard.png` 的實際相符項目，而這也就是要載入的資產檔案。 因此載入相同的檔案，雖然是在相同條件下，卻是因為相符項目不同。

如果高對比或標準對比分別都只需要一組資產，則可以使用資料夾名稱，而不使用檔案名稱。 在這種情況下，完全省略資料夾名稱就會為您提供中性相符項目。

```console
\Assets\Images\contrast-high\<logo.png, and other images to load when high contrast theme is not None>
\Assets\Images\<logo.png, and other images to load when high contrast theme is None>
```

如需限定詞比對運作方式的詳細資訊，請參閱[資源管理系統](resource-management-system.md)。

## <a name="multiple-qualifiers"></a>多個限定詞

您可以在資料夾及檔案名稱中結合限定詞。 例如，您可能會想要在高對比模式為開啟狀態*且*顯示縮放比例為 400 時載入影像資產。 其中一個這樣做的方式是巢狀資料夾。

```console
\Assets\Images\contrast-high\scale-400\<logo.png, and other image files>
```

`logo.png` 及其他要載入之檔案的設定必須與*兩個*限定詞都相符。

另一個方式是將多個限定詞結合成一個資料夾名稱。

```console
\Assets\Images\contrast-high_scale-400\<logo.png, and other image files>
```

在資料夾名稱中，您可以結合多個限定詞，並以底線來分隔。 `<qualifier1>[_<qualifier2>...]` 是格式。

您可以在檔案名稱中，以相同的格式來結合多個限定詞。

```console
\Assets\Images\logo.contrast-high_scale-400.png
```

依據您用於建立資產的工具和工作流程，或是依據您發現最容易讀取和/或管理的方式，您可以選擇適用於所有限定詞的單一命名策略，也可以結合這些限定詞以用作不同限定詞。

## <a name="alternateform"></a>AlternateForm

`alternateform` 限定詞用來針對某些特殊用途提供其他形式的資源。 這通常僅供日文應用程式開發人員用來提供其 `msft-phonetic` 值已保留的注音假名字串 (請參閱[如何準備當地語系化](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh967762)中的＜支援可排序的日文注音假名字串＞一節)。

您的目標系統或應用程式必須提供與 `alternateform` 限定詞相符的值。 不要將 `msft-` 首碼用於您自己的自訂 `alternateform` 限定詞值。

## <a name="configuration"></a>Configuration

您不太可能需要 `configuration` 限定詞名稱。 這可以用來指定只適用於特定製作階段環境的資源，例如僅供測試資源。

`configuration` 限定詞會用於載入最符合 `MS_CONFIGURATION_ATTRIBUTE_VALUE` 環境變數值的資源。 因此，您可以將變數設定為已指派給相關資源的字串值，例如 `designer` 或 `test`。

## <a name="contrast"></a>Contrast

`contrast` 限定詞用來提供最符合高對比設定的資源。

## <a name="custom"></a>Custom

您的應用程式可以設定 `custom` 限定詞的值，然後載入最符合該值的資源。 例如，您可能會想要根據應用程式的授權載入資源。 應用程式啟動時，會呼叫 [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_) 來檢查其授權並使用此授權做為 `custom` 限定詞的值，如程式碼範例中所示。

```csharp
public void SetLicenseLevel(BrandID brand)
{
    if (brand == BrandID.Premium)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Premium", ResourceQualifierPersistence.LocalMachine);
    }
    else if (brand == BrandID.Standard)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", " Standard", ResourceQualifierPersistence.LocalMachine);
    }
    else
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Trial", ResourceQualifierPersistence.LocalMachine);
    }
}
```

在此案例中，您接著會指定包含限定詞 `custom-premium`、`custom-standard` 及 `custom-trial` 的資源名稱。

## <a name="devicefamily"></a>DeviceFamily

您不太可能需要 `devicefamily` 限定詞名稱。 您應該盡可能避免使用此限定詞，因為另有更方便且穩健的技術可供您使用。 這些技術在[偵測執行您 app 的平台](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on)和[版本調適型程式碼](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)中有說明。

但在不得已時，還是可以使用 devicefamily 限定詞來命名含有 XAML 檢視表 (XAML 檢視表是一個包含 UI 版面配置及控制項的 XAML 檔案) 的資料夾。

```console
\devicefamily-desktop\<MainPage.xaml, and other markup files to load when running on a desktop computer>
\devicefamily-mobile\<MainPage.xaml, and other markup files to load when running on a phone>
```

或者，也可以命名檔案。

```console
\MainPage.devicefamily-desktop.xaml
\MainPage.devicefamily-mobile.xaml
```

在這兩種情況下，每一份 `MainPage.[<qualifier>].xaml` 複本都會有共同的 `MainPage.xaml.cs`，就名稱、位置和內容而言，這在您的專案中皆保持不變。

您也可以使用 devicefamily 限定詞來命名資源檔案 (`.resw`) 或資料夾。 例如，當應用程式在行動裝置系列上執行時，UI 元素 `<TextBlock x:Uid="DeviceFriendlyName"/>` 會使用 `Resources.devicefamily-mobile.resw` 檔案中的定義的文字及前景資源 (如果其中包含的話)。

```xml
<data name="DeviceFriendlyName.Foreground">
    <value>Red</value>
</data>
<data name="DeviceFriendlyName.Text">
    <value>Mobile device</value>
</data>
```

如需有關使用資源檔案的詳細資訊，請參閱[當地語系化您的 UI 字串](localize-strings-ui-manifest.md)。

## <a name="dxfeaturelevel"></a>DXFeatureLevel

您不太可能需要 `dxfeaturelevel` 限定詞名稱。 這是設計用來與 Direct3D 遊戲資產搭配，使系統載入舊版資源以符合當時的特定舊版硬體設定。 但現在使用該硬體設定的情形並不普遍，建議您不要使用這個限定詞。

## <a name="homeregion"></a>HomeRegion

`homeregion` 限定詞對應於使用者的國家或地區設定。 這代表使用者的住家位置。 值包括任何有效 [BCP-47 區域標記](https://go.microsoft.com/fwlink/p/?linkid=227302)。 也就是，任何兩個字母的 **ISO 3166-1 alpha-2** 區域代碼，加上一組代表組成區域的三位數 **ISO 3166-1 數字**地理代碼 (請參閱[聯合國統計司 M49 區域分類編碼](https://go.microsoft.com/fwlink/p/?linkid=247929))。 「選取的經濟及其他群組」的代碼無效。

## <a name="language"></a>Language

`language` 限定詞對應於顯示語言設定。 值包括任何有效 [BCP-47 語言標記](https://go.microsoft.com/fwlink/p/?linkid=227302)。 如需語言清單，請參閱 [IANA 語言子標記登錄](https://go.microsoft.com/fwlink/p/?linkid=227303)。

如果希望應用程式支援不同的顯示語言，而且您的程式碼或 XAML 標記也含有字串常值時，請將這些字串從程式碼/標記中移入資源檔案 (`.resw`)。 您可以接著針對應用程式支援的每一種語言建立該資源檔案的翻譯複本。

您通常會使用 `language` 限定詞來命名包含資源檔案 (`.resw`) 的資料夾。

```console
\Strings\language-en\Resources.resw
\Strings\language-ja\Resources.resw
```

您可以省略 `language` 限定詞的 `language-` 部分 (也就是，限定詞名稱)。 您不可使用其他類型的限定詞這樣做，而且也只有在資料夾名稱中才做得到。

```console
\Strings\en\Resources.resw
\Strings\ja\Resources.resw
```

您不必命名資料夾，反而可以使用 `language` 限定詞來命名資源檔案本身。

```console
\Strings\Resources.language-en.resw
\Strings\Resources.language-ja.resw
```

請參閱[當地語系化您的 UI 字串](localize-strings-ui-manifest.md)，取得更多資訊來了解如何使用字串資源讓應用程式可當地語系化，以及如何在應用程式中參考字串資源。

## <a name="layoutdirection"></a>LayoutDirection

`layoutdirection` 限定詞對應於顯示語言設定的配置方向。 例如，可能需要針對阿拉伯文或希伯來文等由右至左的語言建立影像的鏡像。 如果 UI 中的配置面板及影像設定了 [FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) 屬性，便會適當地對應至配置方向 (請參閱[調整配置和字型並支援 RTL](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md))。 不過，`layoutdirection` 限定詞較適用於無法以簡單翻轉來達到滿意效果的情況，而且可讓您以更通用的方式回應特定閱讀順序的方向和文字對齊。

## <a name="scale"></a>縮放比例

Windows 會根據 DPI (每英吋點數) 以及裝置的檢視距離，自動選取每個螢幕的縮放比例。 請參閱[有效像素與縮放比例](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)。 您應該建立幾種建議大小 (至少 100、200 以及 400) 的影像，以便 Windows 可以選擇完美大小或者可以使用最接近的大小，並調整它。 因此，Windows 可以找出包含正確大小的實體檔案，以顯示縮放比例，您使用 `scale` 限定詞。 資源的縮放比例的相符項目會是 [DisplayInformation.ResolutionScale](/uwp/api/windows.graphics.display.displayinformation.ResolutionScale) 的值或次大的縮放資源。

以下是在資料夾層級設定限定詞的範例。

```console
\Assets\Images\scale-100\<logo.png, and other image files>
\Assets\Images\scale-200\<logo.png, and other image files>
\Assets\Images\scale-400\<logo.png, and other image files>
```

而此範例則是在檔案層級進行設定。

```console
\Assets\Images\logo.scale-100.png
\Assets\Images\logo.scale-200.png
\Assets\Images\logo.scale-400.png
```

如需有關針對 `scale` 及 `targetsize` 限定資源的詳細資訊，請參閱[針對 targetsize 限定影像資源](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize)。

## <a name="targetsize"></a>TargetSize

`targetsize` 限定詞主要用來指定要顯示在 [檔案總管] 中的[檔案類型關聯圖示](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh127427)或[通訊協定圖示](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/bb266530)。 限定詞值表示以原始 (實體) 像素為單位的正方形影像側邊長度。 載入的會是符合 [檔案總管] 中 [檢視] 設定值的資源，如果完全相符的項目，則載入含有次大值的資源。

在應用程式封裝資訊清單設計工具的 [視覺資產] 索引標籤中，您可以定義資產來表示應用程式圖示 (`/Assets/Square44x44Logo.png`) 的數個 `targetsize` 限定詞值大小。

如需有關針對 `scale` 及 `targetsize` 限定資源的詳細資訊，請參閱[針對 targetsize 限定影像資源](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize)。

## <a name="theme"></a>Theme

`theme` 限定詞用來提供最符合預設應用程式模式設定的資源，或是應用程式使用 [Application.RequestedTheme](/uwp/api/windows.ui.xaml.application.requestedtheme) 的覆寫。

## <a name="important-apis"></a>重要 API

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)

## <a name="related-topics"></a>相關主題

* [有效像素與縮放比例](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)
* [資源管理系統](resource-management-system.md)
* [如何準備當地語系化](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh967762)
* [偵測執行您 app 的平台](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on)
* [裝置系列概觀](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)
* [當地語系化您的 UI 字串](localize-strings-ui-manifest.md)
* [BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302)
* [聯合國統計司 M49 區域分類編碼](https://go.microsoft.com/fwlink/p/?linkid=247929)
* [IANA 語言子標記登錄](https://go.microsoft.com/fwlink/p/?linkid=227303)
* [調整配置和字型並支援 RTL](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)
