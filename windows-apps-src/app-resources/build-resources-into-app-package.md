---
Description: Some kinds of apps (multilingual dictionaries, translation tools, etc.) need to override the default behavior of an app bundle, and build resources into the app package instead of having them in separate resource packages. This topic explains how to do that.
title: 將資源建置到您的應用程式套件，而不是資源套件
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: 8bf2d34bc3dae20750f66c9116499a17444b798c
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8938491"
---
# <a name="build-resources-into-your-app-package-instead-of-into-a-resource-pack"></a>將資源建置到您的應用程式套件，而不是資源套件

某些類型的應用程式 (多語系字典、翻譯工具等) 需要覆寫應用程式套件組合的預設行為，並將資源建置到應用程式套件中，而不是讓這些資源分散在不同的資源套件 (資源包) 中。 本主題說明如何執行這個動作。

根據預設，當您建置[應用程式套件組合 (.appxbundle)](../packaging/packaging-uwp-apps.md) 時，只會將您語言、縮放比例及 DirectX 功能層級的預設資源建置到應用程式套件中。 您的已翻譯資源 (以及針對非預設縮放比例和/或 DirectX 功能層級所量身訂做的資源) 會內建在資源套件中，並且只會下載到需要這些資源的裝置上。 如果客戶要使用語言喜好設定已設為西班牙文的裝置，從 Microsoft Store 購買您的應用程式，則只會下載並安裝您的應用程式以及西班牙文資源套件。 如果同一個使用者稍後在 **\[設定\]** 中將他們的語言喜好設定變更為法文，則會下載並安裝您應用程式的法文資源套件。 類似的情況會發生在您的縮放比例和 DirectX 功能層級所限定的資源。 對大多數的應用程式來說，這種行為提供可貴的效率，正是您和客戶所*樂見其成*的結果。

但如果您的應用程式可讓使用者從應用程式中即時 (而不是透過 **\[設定\]**) 變更語言，那麼這種預設行為並不適當。 您實際想要的是，一次就無條件地隨應用程式一起下載並安裝所有的語言資源，然後將其保留在裝置上。 您想要將所有這些資源建置到您的應用程式套件中，而不是分散至不同的資源套件。

**注意**：將資源包含在應用程式套件中，基本上會增加應用程式的大小。 這就是為什麼只有在應用程式本質上有需要時，才值得如此行。 不然，除了像平常一樣建置一般應用程式套件組合以外，就不需要執行任何動作。

您可以設定 Visual Studio，透過兩種方式之一，將資源建置到應用程式套件中。 您可以將設定檔案新增至專案，或是直接編輯專案檔案。 請使用下列選項中用起來最順手的任何選項，或是任何最適用於您的建置系統的選項。

## <a name="option-1-use-priconfigpackagingxml-to-build-resources-into-your-app-package"></a>選項 1： 使用 priconfig.packaging.xml 將資源建置到您的應用程式套件中

1. 在 Visual Studio 中，將新項目加入至您的專案。 選擇 XML 檔案，並將檔案命名為 `priconfig.packaging.xml`。
2. 在方案總管中，選取 [`priconfig.packaging.xml`]，並檢查屬性視窗。 檔案的 [建置動作] 必須設定為 [無]，而 [複製到輸出目錄] 則應設定為 [不要複製]。
3. 以此 XML 取代檔案的內容。
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Language" />
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
4. 每個 `<autoResourcePackage>` 元素都會指示 Visual Studio 自動將指定之限定詞名稱的資源分割成不同的資源套件。 這稱為*自動分割*。 就您到目前為止所處理的檔案內容來看，您還沒有實際改變 Visual Studio 的行為表現。 換句話說，Visual Studio *已經表現得好像*此檔案已經存在這些內容，因為這是預設值。 如果您不希望 Visual Studio 依據限定詞名稱自動分割，請從檔案刪除這個 `<autoResourcePackage>` 元素。 如果您想要將您所有的語言資源都建置到應用程式套件中，而不是自動分割成不同的資源套件，則檔案看起來會像這樣。
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
5. 儲存並關閉檔案，然後重新建置您的專案。

若要確認系統已將您的自動分割選擇納入考量，請尋找檔案 `<ProjectFolder>\obj\<ReleaseConfiguration folder>\split.priconfig.xml`，並確認其內容符合您的選擇。 如果這樣做了，即表示您已順利進行設定，可讓 Visual Studio 將您選擇的資源建置到應用程式套件中。

還有最後一個必須執行的步驟。 **但只有在您已刪除 `Language` 限定詞名稱時，才要這樣做**。 您需要將應用程式所有支援的語言的聯集指定為應用程式的語言預設值。 如需詳細資訊，請參閱[指定您的應用程式使用的預設資源](specify-default-resources-installed.md)。 如果您要將英文、西班牙文及法文資源包含在應用程式套件中，這就是您的 `priconfig.default.xml` 會包含的部分。

```xml
   <default>
      <qualifier name="Language" value="en;es;fr" />
      ...
   </default>
```

### <a name="how-does-this-work"></a>這是如何運作？

Visual Studio 在幕後啟動名為 `MakePri.exe` 的工具來產生所謂「套件資源索引」的檔案，這個檔案描述應用程式所有的資源，包括指出要依據哪些資源限定詞名稱進行自動分割。 如需此工具的詳細資訊，請參閱[使用 MakePri.exe 來手動編譯資源](compile-resources-manually-with-makepri.md)。 Visual Studio 將設定檔傳遞至 `MakePri.exe`。 `priconfig.packaging.xml` 檔案的內容會用來做為該設定檔的 `<packaging>` 元素，也就是判斷自動分割的部分。 因此，新增和編輯 `priconfig.packaging.xml`，最後都會影響 Visual Studio 為應用程式所產生的套件資源索引檔案內容，以及應用程式套件組合中套件的內容。

### <a name="using-a-different-file-name-than-priconfigpackagingxml"></a>使用與 `priconfig.packaging.xml` 不同的檔案名稱

如果您將檔案命名為 `priconfig.packaging.xml`，則 Visual Studio 將會辨識並且自動使用此名稱。 如果您指定別的名稱，就必須讓 Visual Studio 知道。 在您專案檔案中第一個 `<PropertyGroup>` 元素的開頭和結尾標記之間，加入這段 XML。

```xml
<AppxPriConfigXmlPackagingSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlPackagingSnippetPath>
```

將 `FILE-PATH-AND-NAME` 取代為您的檔案的路徑和名稱。

## <a name="option-2-use-your-project-file-to-build-resources-into-your-app-package"></a>選項 2： 使用專案檔案，將資源建置到您的應用程式套件中

這是「選項 1」的替代方法。 您了解「選項 1」的運作方式之後，就可以選擇改為執行「選項 2」，如果這樣更適合您的開發和/或建置工作流程的話。

在您專案檔案中第一個 `<PropertyGroup>` 元素的開頭和結尾標記之間，加入這段 XML。

```xml
<AppxBundleAutoResourcePackageQualifiers>Language|Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

以下是在刪除第一個限定詞名稱之後的樣子。

```xml
<AppxBundleAutoResourcePackageQualifiers>Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

儲存並關閉，然後重新建置您的專案。

還有最後一個必須執行的步驟。 **但只有在您已刪除 `Language` 限定詞名稱時，才要這樣做**。 您需要將應用程式所有支援的語言的聯集指定為應用程式的語言預設值。 如需詳細資訊，請參閱[指定您的應用程式使用的預設資源](specify-default-resources-installed.md)。 如果您要將英文、西班牙文及法文資源包含在應用程式套件中，這就是您的專案檔案會包含的部分。

```xml
<AppxDefaultResourceQualifiers>Language=en;es;fr</AppxDefaultResourceQualifiers>
```

## <a name="related-topics"></a>相關主題

* [使用 Visual Studio 封裝 UWP app](../packaging/packaging-uwp-apps.md)
* [使用 MakePri.exe 來手動編譯資源](compile-resources-manually-with-makepri.md)
* [指定您的應用程式使用的預設資源](specify-default-resources-installed.md)