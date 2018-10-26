---
author: stevewhims
Description: If your app doesn't have resources that match the particular settings of a customer device, then the app's default resources are used. This topic explains how to specify what those default resources are.
title: 指定您的應用程式使用的預設資源
template: detail.hbs
ms.author: stwhi
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: daa40656c72812e19c7f6f5fa71e50c2206670af
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5546531"
---
# <a name="specify-the-default-resources-that-your-app-uses"></a>指定您的應用程式使用的預設資源

如果您的 App 沒有符合客戶裝置特定設定的資源，則會使用 App 的預設資源。 本主題說明如何指定這些預設資源的內容。

當客戶從 Microsoft Store 安裝您的 App 時，會依據 App 的可用資源比對客戶裝置上的設定。 執行這個比對後，就只需要下載並安裝適合該使用者的資源。 例如，使用最適合使用者語言喜好設定和裝置解析度及 DPI 設定的字串與影像。 例如，`200` 是 `scale` 的預設值，但是您可以視需要覆寫該預設值。

即使是未納入其本身資源套件 (例如專為高對比設定量身訂做的影像) 中的資源，您還是可以指定如果找不到符合使用者設定的資源時，應用程式在執行階段應使用哪些預設資源。 例如，`standard` 是 `contrast` 的預設值，但是您可以視需要覆寫該預設值。

這些預設值是以預設資源限定詞值的形式來指定。 如需有關何謂資源限定詞、其用法及用途的說明，請參閱[針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md)。

您可以使用兩種方式的其中一種來設定這些預設值的內容。 您可以將設定檔案新增至專案，或是直接編輯專案檔案。 請使用下列選項中用起來最順手的任何選項，或是任何最適用於您的建置系統的選項。

## <a name="option-1-use-priconfigdefaultxml-to-specify-default-qualifier-values"></a>選項 1： 使用 priconfig.default.xml 來指定預設限定詞值

1. 在 Visual Studio 中，將新項目加入至您的專案。 選擇 XML 檔案，並將檔案命名為 `priconfig.default.xml`。
2. 在方案總管中，選取 [`priconfig.default.xml`]，並檢查屬性視窗。 檔案的 [建置動作] 必須設定為 [無]，而 [複製到輸出目錄] 則應設定為 [不要複製]。
3. 以此 XML 取代檔案的內容。
   ```xml
   <default>
      <qualifier name="Language" value="LANGUAGE-TAG(S)" />
      <qualifier name="Contrast" value="standard" />
      <qualifier name="Scale" value="200" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
   
   **注意**：`LANGUAGE-TAG(S)` 這個值必須與您的應用程式預設語言保持同步。 如果這是單一 [BCP-47 語言標記](http://go.microsoft.com/fwlink/p/?linkid=227302)，則應用程式的預設語言必須是相同的標記。 如果是以逗號分隔的語言標記清單，則應用程式的預設語言必須是清單中的第一個標記。 您可以在應用程式封裝資訊清單來源檔案 (`Package.appxmanifest`) 的 **\[應用程式\]** 索引標籤上的 **\[預設語言\]** 中的，設定應用程式的預設語言。

4. 每個 `<qualifier>` 元素都會指定 Visual Studio 要使用什麼值做為每個限定詞名稱的預設值。 就您到目前為止所處理的檔案內容來看，您還沒有實際改變 Visual Studio 的行為表現。 換言之，Visual Studio *已經表現得好像*此檔案已經存在這些內容，因為這是預設值。 因此要將預設值覆寫成您自己的預設值，就必須變更檔案中的值。 以下是檔案在您編輯了前三個值的情況下可能呈現的外觀範例。
   ```xml
   <default>
      <qualifier name="Language" value="de-DE" />
      <qualifier name="Contrast" value="black" />
      <qualifier name="Scale" value="400" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
5. 儲存並關閉檔案，然後重新建置您的專案。

若要確認系統已將您覆寫的預設值納入考量，請尋找檔案 `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml`，並確認其內容符合您的覆寫。 如果這樣做了，即表示您已成功設定應用程式預設使用之資源的限定詞值。 如果找不到使用者設定的相符項目，則會使用資料夾或檔案名稱包含您在此所設定之預設資源限定詞值的資源。

### <a name="how-does-this-work"></a>這是如何運作？

Visual Studio 在幕後啟動名為 `MakePri.exe` 的工具來產生所謂套件資源索引 (PRI) 的檔案，這個檔案描述應用程式所有的資源，包括指出哪些是預設資源。 如需此工具的詳細資訊，請參閱[使用 MakePri.exe 來手動編譯資源](compile-resources-manually-with-makepri.md)。 Visual Studio 將設定檔傳遞至 `MakePri.exe`。 `priconfig.default.xml` 檔案的內容會用來做為該設定檔的 `<default>` 元素，也就是指定視為預設值之限定詞值集的部分。 因此，新增和編輯 `priconfig.default.xml`，最後都會影響 Visual Studio 為應用程式所產生並且包含在其應用程式套件中的套件資源索引檔案內容。

**注意**：只要您變更 `<qualifier name="Language" ... />` 元素的值，就必須與您應用程式的預設語言同步處理該變更。 這是為了讓已在應用程式 PRI 檔案中建立索引的語言資源，符合應用程式的資訊清單預設語言。 `<qualifier name="Language" ... />` 元素的值會根據 `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` 的內容覆寫資訊清單的值，但是這個檔案與您的應用程式的資訊清單必須相符。

### <a name="using-a-different-file-name-than-priconfigdefaultxml"></a>使用與 `priconfig.default.xml` 不同的檔案名稱

如果您將檔案命名為 `priconfig.default.xml`，則 Visual Studio 將會辨識並且自動使用此名稱。 如果您指定別的名稱，就必須讓 Visual Studio 知道。 在您專案檔案中第一個 `<PropertyGroup>` 元素的開頭和結尾標記之間，加入這段 XML。

```xml
<AppxPriConfigXmlDefaultSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlDefaultSnippetPath>
```

將 `FILE-PATH-AND-NAME` 取代為您的檔案的路徑和名稱。

## <a name="option-2-use-your-project-file-to-specify-default-qualifier-values"></a>選項 2： 使用您的專案檔案來指定預設限定詞值

這是「選項 1」的替代方法。 您了解「選項 1」的運作方式之後，就可以選擇改為執行「選項 2」，如果這樣更適合您的開發和/或建置工作流程的話。

在您專案檔案中第一個 `<PropertyGroup>` 元素的開頭和結尾標記之間，加入這段 XML。

```xml
<AppxDefaultResourceQualifiers>Language=LANGUAGE-TAG(S)|Contrast=standard|Scale=200|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

以下是這個檔案在您編輯前三個值過後可能呈現的外觀範例。

```xml
<AppxDefaultResourceQualifiers>Language=de-DE|Contrast=black|Scale=400|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

儲存並關閉，然後重新建置您的專案。

**注意**：只要您變更 `Language=` 值，就必須使用資訊清單設計工具 (藉由開啟 `Package.appxmanifest`)，與您應用程式的預設語言同步處理該變更。

## <a name="related-topics"></a>相關主題

* [針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md)
* [BCP-47 語言標記](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [使用 MakePri.exe 來手動編譯資源](compile-resources-manually-with-makepri.md)
