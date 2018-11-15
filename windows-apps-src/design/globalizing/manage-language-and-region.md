---
author: stevewhims
Description: This topic defines the terms user profile language list, app manifest language list, and app runtime language list. We'll be using these terms in this topic and other topics in this feature area, so it's important to know what they mean.
title: 了解使用者設定檔語言和應用程式資訊清單語言
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
template: detail.hbs
ms.author: stwhi
ms.date: 11/08/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可當地語系化性, 當地語系化
ms.localizationpriority: medium
ms.openlocfilehash: 2215231b21700fc17b08c2149316f9a59f8d1f04
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2018
ms.locfileid: "6671014"
---
# <a name="understand-user-profile-languages-and-app-manifest-languages"></a>了解使用者設定檔語言和應用程式資訊清單語言
Windows 使用者可使用 **\[設定\]** > **\[時間與語言\]** > **\[地區與語言\]** 設定慣用顯示語言的已排序清單，或單一慣用顯示語言。 語言可具有地區性的變體。 例如，您可以選取在西班牙使用的西班牙文、在墨西哥使用的西班牙文、在美國使用的西班牙文等。

此外在 **\[設定\]** > **\[時間與語言\]** > **\[地區與語言\]** 中 (與語言分離)，使用者可指定自身位於全球的位置 (稱為地區)。 請注意，顯示語言 (及地區性變體) 設定不會決定地區設定，反之亦然。 例如，使用者目前可以居住在法國，但選擇其慣用的 Windows 顯示語言為西班牙文 (墨西哥)。

針對 UWP app，語言是以 [BCP-47 語言標記](http://go.microsoft.com/fwlink/p/?linkid=227302)表示的。 例如，BCP-47 語言標記 "en-US" 會對應到 **\[設定\]** 中的英文 (美國)。 適當的 UWP API 會接受並傳回 BCP-47 語言標記的字串表示。

另請參閱 [IANA 語言子標記登錄](http://go.microsoft.com/fwlink/p/?linkid=227303)。

以下三節會定義「使用者設定檔語言清單」、「應用程式資訊清單語言清單」及「應用程式執行階段語言清單」等三個術語。 我們會在本主題及位於此功能區域的其他主題中使用這些術語，因此請務必了解他們的意義。

## <a name="user-profile-language-list"></a>使用者設定檔語言清單
使用者設定檔語言清單是由使用者在 **\[設定\]** > **\[時間與語言\]** > **\[地區與語言\]** > **\[語言\]** 中設定之清單的名稱。 在程式碼中，您可以使用 [**GlobalizationPreferences.Languages**](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages) 屬性將使用者設定檔語言清單作為唯讀字串清單存取，其中每個字串都是單一 [BCP-47 語言標記](http://go.microsoft.com/fwlink/p/?linkid=227302)，例如 "en-US" 或 "ja-JP"。

```csharp
    IReadOnlyList<string> userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;
```

## <a name="app-manifest-language-list"></a>應用程式資訊清單語言清單
應用程式資訊清單語言清單是您應用程式宣告 (或將宣告) 支援之語言的清單。 這個清單會隨著您的應用程式在開發生命週期中向當地語系化邁進時逐漸擴大。

清單會在編譯階段進行判斷，但您有兩個選項可控制該判斷發生的方式。 其中一個選項是讓 Visual Studio 從您專案中的檔案判斷該清單。 若要執行此作業，首先請在您應用程式封裝資訊清單來源檔案 (`Package.appxmanifest`) 中的 **\[應用程式\]** 索引標籤上設定您應用程式的 **\[預設語言\]**。 然後，請確認相同的檔案包含此設定 (預設為已包含)。

```xml
  <Resources>
    <Resource Language="x-generate" />
  </Resources>
```

每次 Visual Studio 產生您的建置應用程式封裝資訊清單檔案 (`AppxManifest.xml`) 時，便會將來源檔案中的單一 `Resource` 元素展開至其在您專案中找到的所有語言識別碼的聯集 (請參閱[針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](../../app-resources/tailor-resources-lang-scale-contrast.md))。 例如，若您已開始當地語系化，並且您擁有資料夾或檔案名稱包含 "en-US"、"ja-JP" 和 "fr-FR" 的字串、影像和/或檔案資源，則您的建置 `AppxManifest.xml` 檔案便會包含下列項目 (清單中的第一個項目為您設定的預設語言)。

```xml
  <Resources>
    <Resource Language="EN-US" />
    <Resource Language="JA-JP" />
    <Resource Language="FR-FR" />
  </Resources>
```

其他選項便是使用 `<Resource>` 元素的展開清單取代您應用程式封裝資訊清單來源檔案 (`Package.appxmanifest`) 中的單一 "x-generate" `<Resource>` 元素 (請記得將預設語言列在第一個)。 您可能會需要為該選項進行更多維護工作，但若您使用的是自訂建置系統，則可能會是適合您的選項。

若要開始執行，您的應用程式資訊清單語言清單只會包含一種語言。 可能是 zh-TW。 但最後&mdash;在您手動設定資訊清單後，或是將已翻譯的資源新增到您的專案後&mdash;該清單便會成長。

當您的應用程式位於 Microsoft Store 中時，應用程式資訊清單語言清單中的語言便會顯示給客戶。 如需取得 Microsoft Store 支援的特定 BCP-47 語言標記清單，請參閱[支援語言](../../publish/supported-languages.md)。

在程式碼中，您可以使用 [**ApplicationLanguages.ManifestLanguages**](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages) 屬性將應用程式資訊清單語言清單作為唯讀字串清單存取，其中每個字串都是單一 BCP-47 語言標記。

```csharp
    IReadOnlyList<string> userLanguages = Windows.Globalization.ApplicationLanguages.ManifestLanguages;
```

## <a name="app-runtime-language-list"></a>應用程式執行階段語言清單
第三種重要語言清單便是我們方才描述的兩種清單的交集。 在執行階段時，您應用程式宣告支援的語言清單 (應用程式資訊清單語言清單) 會和使用者宣告喜好設定的語言清單 (使用者設定檔語言清單) 進行比較。 應用程式執行階段語言清單便會設為此交集 (若交集並非空白)，或是應用程式的預設語言 (若交集為空白)。

更具體而言，應用程式執行階段語言清單是由這些項目組成的。

1.  **(選擇性) 主要語言覆寫**。 [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride) 是給予使用者自身獨立語言選擇之應用程式，或是因具備某些強力理由而需要覆寫預設語言選擇之應用程式的簡易覆寫設定。 若要深入了解，請參閱[應用程式資源和當地語系化範例](http://go.microsoft.com/fwlink/p/?linkid=231501)。
2.  **受應用程式支援的使用者語言**。 這是經過應用程式資訊清單語言清單篩選後的使用者設定檔語言清單。 依應用程式支援的語言篩選使用者的語言可在軟體開發套件 (SDK)、類別庫、相依架構套件及應用程式之間維持一致性。
3.  **如果第 1 項和第 2 項為空白，則為預設語言或應用程式所支援的第一個語言。**。 如果使用者設定檔語言清單並未包含任何應用程式支援的語言，則應用程式執行階段語言會是應用程式支援的第一個語言。

在程式碼中，您可以使用 [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) 屬性存取應用程式執行階段語言清單，其形式為包含 BCP-47 語言標記分號分隔清單的字串。

```csharp
    string runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["Language"];
```

您也可以將其作為唯讀字串清單存取，其中每一個都包含單一 BCP-47 語言標記。 您可以使用 [**ResourceContext.Languages**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages) 屬性或 [**ApplicationLanguages.Languages**](/uwp/api/windows.globalization.applicationlanguages.Languages) 屬性執行此作業。

```csharp
    IReadOnlyList<string> runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().Languages;

    runtimeLanguages = Windows.Globalization.ApplicationLanguages.Languages;
```

應用程式執行階段語言清單會判斷 Windows 為您的應用程式載入的資源，以及用來格式化日期、時間、數字和其他元件的語言。 請參閱[全球化您的日期/時間/數字格式](use-global-ready-formats.md)。

**注意**：若使用者設定檔語言和應用程式資訊清單語言為彼此的地區性變體，則會使用使用者的地區性變體作為應用程式執行階段語言。 例如，如果使用者慣用的是 en-GB，同時應用程式支援 en-US，則應用程式執行階段語言便會是 en-GB。 這確保日期、時間及數字的格式化方式會更貼近使用者的預期 (en-GB)，但是仍然會在應用程式支援的語言 (en-US) 中載入當地語系化資源 (由於語言比對的緣故)。

## <a name="qualify-resource-files-with-their-language"></a>使用他們的語言限定資源檔案
使用語言資源限定詞命名您的資源檔案或其資料夾。 若要深入了解資源限定詞，請參閱[針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](../../app-resources/tailor-resources-lang-scale-contrast.md))。 資源檔案可以是單一影像或其他資產檔案，或是包含字串資源的容器資源檔案&mdash;例如資源檔 (.resw)&mdash;。

**注意**：即使是您應用程式預設語言的資源也需要使用其語言進行限定。 例如，您應用程式的預設語言為英文 (美國)，請也限定您的 en-US 資源，如同 `\Assets\Images\en-US\logo.png`。 

- Windows 會執行複雜的比對，包含例如跨地區性的變體，像是 en-US 和 en-GB。 因此請視需要包含或省略地區子標記。 請參閱[資源管理系統如何比對語言標記](../../app-resources/how-rms-matches-lang-tags.md)。
- 如果語言沒有定義抑制指令碼值，則包含指令碼。 請參閱 [IANA 語言子標記登錄](http://go.microsoft.com/fwlink/p/?linkid=227303)，以取得語言標記的詳細資料。 例如，使用 zh-Hant、zh-Hant-TW 或 zh-Hans，不使用 zh-CN 或 zh-TW。
- 如果語言只有單一標準方言，則不需要新增地區。 在某些情況下，一般標記是合理的，例如使用 ja (而非 ja-JP) 標示資產。
- 某些工具和其他元件 (如電腦翻譯工具) 可能會找到特定語言標記 (如地區方言資訊)，這有助於了解資料。

有時候可能不需要當地語系化所有資源。

- 針對像是 UI 字串這類包含所有語言的資源，請使用其語言標記他們。 確認所有字串都存在於預設語言中。
- 對於含有整個應用程式的語言集之子集的資源 (部分當地語系化)，請指定資產確實含有的語言集，並確定這些資源全都使用預設語言。 例如，若您的應用程式含有西班牙文的完整資源集合，則您可能並未將您應用程式所有的 UI 都當地語系化成卡達隆尼亞文。 對於優先使用卡達隆尼亞文、其次為西班牙文的使用者來說，未以卡達隆尼亞文提供的資源會以西班牙文顯示。
- 對於某些語言有特定例外，但其他所有語言則對應至某個共通資源的資源來說，請使用未定語言標記 'und' 標示要應用在所有語言的資源。 Windows 會將 'und' 語言標記解譯為萬用字元 (類似 '\*')，即它會比對任何其他特定的相符項目後的最上層應用程式語言。 例如，如果芬蘭文的少數資源不相同，但所有語言的其餘資源均相同，則芬蘭文資源應以芬蘭文語言標記標示，其餘則應以 'und' 標示。
- 對於以語言的指令碼 (而非語言) 為基礎的資源 (例如字型或文字高度)，請使用含有指定之指令碼 'und-&lt;script&gt;' 的未定語言標記。 例如，拉丁字型使用 `und-Latn\\fonts.css`，斯拉夫字型則使用 `und-Cryl\\fonts.css`。

## <a name="set-the-http-accept-language-request-header"></a>設定 HTTP Accept-Language 要求標頭
請考慮您呼叫的 Web 服務是否擁有與您應用程式相同程度的當地語系化。 在典型 Web 要求和 XMLHttpRequest (XHR) 中，從 UWP app 和傳統型應用程式提出的 HTTP 要求，會使用標準的 HTTP Accept-Language 要求標頭。 根據預設，HTTP 標頭會設為使用者設定檔語言清單。 清單中的每個語言都可進一步展開，以包含中性語言和加權 (q)。 例如，fr-FR 和 en-US 的使用者語言清單會產生 fr-FR、fr、en-US、en ("fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3") 的 HTTP Accept-Language 要求標頭。 但如果您的天氣應用程式 (舉例) 以法文 (法國) 顯示 UI，但使用者喜好設定清單中的第一個語言為德文，您便需要向服務明確要求法文 (法國)，以和您的應用程式保持一致。

## <a name="apis-in-the-windowsglobalization-namespace"></a>Windows.Globalization 命名空間中的 API
通常，[**Windows.Globalization**](/uwp/api/windows.globalization?branch=live) 命名空間中的 API 會使用應用程式執行階段語言清單來判斷語言。 如果沒有找到具有相符格式的語言，就會使用使用者地區設定。 這是用於系統時鐘的相同地區設定。 使用者地區設定可以從 **\[設定\]** > **\[時間與語言\]** > **\[地區與語言\]** > **\[其他日期、時間及區域設定\]** > **\[地區: 變更日期、時間或數字格式\]** 中取得。 **Windows.Globalization** API 也具有指定要使用的語言清單的覆寫，可取代應用程式執行階段語言清單。

使用 [**Language**](/uwp/api/windows.globalization.language?branch=live) 類別，您可以檢查關於特定語言的詳細資料，例如語言的指令碼、顯示名稱和原生名稱。

## <a name="use-geographic-region-when-appropriate"></a>適當使用地理區域
在 **\[設定\]** > **\[時間與語言\]** > **\[地區與語言\]** > **\[國家或地區\]** 中，使用者可指定自身在世界中的位置。 您可以使用此設定，而非語言，來選擇要向使用者顯示的內容。 例如，新聞應用程式可能會預設顯示此地區的內容。

在程式碼中，您可以使用 [**GlobalizationPreferences.HomeGeographicRegion**](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion) 屬性存取此設定。

使用 [**GeographicRegion**](/uwp/api/windows.globalization.geographicregion?branch=live) 類別，您可以檢查關於特定地區的詳細資料，例如其顯示名稱、原生名稱，以及使用的貨幣。

## <a name="examples"></a>範例
下表包含一些範例，說明使用者在各種語言與地區設定下，在您應用程式 UI 中會看見的內容。

<table border="1">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">應用程式資訊清單語言清單</th>
<th align="left">使用者設定檔語言清單</th>
<th align="left">應用程式的主要語言覆寫 (選擇性)</th>
<th align="left">應用程式執行階段語言清單</th>
<th align="left">使用者在應用程式中看見的內容</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">英文 (英國) (預設值)； 德文 (德國)</td>
<td align="left">英文 (英國)</td>
<td align="left">無</td>
<td align="left">英文 (英國)</td>
<td align="left">UI：英文 (英國)<br>日期/時間/數字：英文 (英國)</td>
</tr>
<tr>
<td align="left">德文 (德國) (預設值)；法文 (法國)；義大利文 (義大利)</td>
<td align="left">法文 (奧地利)</td>
<td align="left">無</td>
<td align="left">法文 (奧地利)</td>
<td align="left">UI：法文 (法國) (從法文 (奧地利) 後援)<br>日期/時間/數字：法文 (奧地利)</td>
</tr>
<tr>
<td align="left">英文 (美國) (預設值)；法文 (法國)；英文 (英國)</td>
<td align="left">英文 (加拿大)；法文 (加拿大)</td>
<td align="left">無</td>
<td align="left">英文 (加拿大)；法文 (加拿大)</td>
<td align="left">UI：英文 (美國) (從英文 (加拿大) 後援)<br>日期/時間/數字：英文 (加拿大)</td>
</tr>
<tr>
<td align="left">西班牙文 (西班牙) (預設值)；西班牙文 (墨西哥)；西班牙文 (拉丁美洲)；葡萄牙文 (巴西)</td>
<td align="left">英文 (美國)</td>
<td align="left">無</td>
<td align="left">西班牙文 (西班牙)</td>
<td align="left">UI：西班牙文 (西班牙) (使用預設值，因為英文沒有可用的後援)<br>日期/時間/數字：西班牙文 (西班牙)</td>
</tr>
<tr>
<td align="left">卡達隆尼亞文 (預設值)；西班牙文 (西班牙)；法文 (法國)</td>
<td align="left">卡達隆尼亞文；法文 (法國)</td>
<td align="left">無</td>
<td align="left">卡達隆尼亞文；法文 (法國)</td>
<td align="left">UI：大部分卡達隆尼亞文和部分法文 (法國)，因為不是所有字串都是卡達隆尼亞文<br>日期/時間/數字：卡達隆尼亞文</td>
</tr>
<tr>
<td align="left">英文 (英國) (預設值)；法文 (法國)；德文 (德國)</td>
<td align="left">德文 (德國)； 英文 (英國)</td>
<td align="left">英文 (英國) (使用者在應用程式的 UI 中選擇)</td>
<td align="left">英文 (英國)；德文 (德國)</td>
<td align="left">UI：英文 (英國) (語言覆寫)<br>日期/時間/數字：英文 (英國)</td>
</tr>
</tbody>
</table>

## <a name="important-apis"></a>重要 API
* [GlobalizationPreferences.Languages](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages)
* [ApplicationLanguages.ManifestLanguages](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages)
* [PrimaryLanguageOverride](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)
* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [ResourceContext.Languages](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages)
* [ApplicationLanguages.Languages](/uwp/api/windows.globalization.applicationlanguages.Languages)
* [Windows.Globalization](/uwp/api/windows.globalization?branch=live)
* [Language](/uwp/api/windows.globalization.language?branch=live)
* [GlobalizationPreferences.HomeGeographicRegion](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion)
* [GeographicRegion](/uwp/api/windows.globalization.geographicregion?branch=live)

## <a name="related-topics"></a>相關主題
* [BCP-47 語言標記](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [IANA 語言子標記登錄](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [支援的語言](../../publish/supported-languages.md)
* [全球化您的日期/時間/數字格式](use-global-ready-formats.md)
* [資源管理系統如何比對語言標記](../../app-resources/how-rms-matches-lang-tags.md)

## <a name="samples"></a>範例
* [應用程式資源和當地語系化範例](http://go.microsoft.com/fwlink/p/?linkid=231501)
