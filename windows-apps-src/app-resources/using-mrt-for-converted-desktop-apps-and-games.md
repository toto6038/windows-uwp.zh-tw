---
title: 針對轉換後的傳統型應用程式和遊樂場使用 MRT
description: 將您的 .NET 或 Win32 應用程式或遊戲封裝成 AppX 套件，即可利用「資源管理系統」載入為執行階段內容量身打造的 App 資源。 這個深入主題說明技術。
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, uwp, mrt, pri。 資源, 遊戲, centennial, Desktop App Converter, mui, 衛星組件
ms.localizationpriority: medium
ms.openlocfilehash: 3367cfafb2f3a8e307fd26dc6d6c19f1ece0d17e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254754"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>在舊版應用程式或遊戲中使用 Windows 10 資源管理系統

.NET 和 Win32 應用程式和遊戲通常會當地語系化為不同語言，以拓展其潛在市場範圍。 如需有關將您的 App 當地語系化的價值主張的詳細資訊，請參閱[全球化和當地語系化](../design/globalizing/globalizing-portal.md)。 藉由將您的 .NET 或 Win32 應用程式或遊戲封裝為 MSIX 或 AppX 套件，您可以利用資源管理系統來載入專為執行時間內容量身打造的應用程式資源。 這個深入主題說明技術。

將傳統 Win32 應用程式當地語系化的方法有許多種，但 Windows 8 引進了[新資源管理系統](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))，其跨程式設計語言、跨應用程式類型運作，並且提供超簡單的當地語系化功能。 此系統在本主題中將稱為「MRT」。 過去這代表「現代化資源技術」，但「現代化」一詞已停止使用。 資源管理員也稱為 MRM (現代化資源管理員) 或 PRI (套件資源索引)。

與 MSIX 型或 AppX 型部署結合（例如，從 Microsoft Store），MRT.LOG 可以自動為指定的使用者/裝置傳遞最適用的資源，以將應用程式的下載和安裝大小降到最低。 對於具有大量當地語系化內容的應用程式而言，縮減約莫數個 *GB* 的 AAA 遊戲大小有絕對的重要性。 其他 MRT 好處包括 Windows Shell 和 Microsoft Store 的當地語系化清單，當使用者慣用的語言不符合可用資源時的自動後援邏輯。

本文件描述高階 MRT 架構，並提供移植指南，以最小的程式碼變更將舊版 Win32 應用程式移至 MRT。 一旦移至 MRT，便有其他好處供開發人員運用 (例如按縮放比例或系統主題的區段資源功能)。 請注意，傳統型橋接器所處理的 UWP 應用程式和 Win32 應用程式皆適合進行 MRT 型當地語系化 (亦即「Centennial」)。

在許多情況中，您可以繼續使用您的現有當地語系化格式與原始碼，同時整合 MRT 以在執行階段解析資源，並將下載大小最小化 - 這並不是全有或全無的方法。 下表摘要說明每個階段的工作及估計成本 / 效益。 本表格不包含非當地語系化工作，例如提供高解析度或高對比的應用程式圖示。 如需提供多個資產磚、圖示等的詳細資訊，請參閱[針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md)。

<table>
<tr>
<th>Work</th>
<th>優勢</th>
<th>估計成本</th>
</tr>
<tr>
<td>當地語系化套件資訊清單</td>
<td>要在 Windows Shell 與 Microsoft Store 中顯示當地語系化的內容，需要最低工作</td>
<td>小型</td>
</tr>
<tr>
<td>使用 MRT 識別並找出資源</td>
<td>下載和安裝大小最小化的先決條件; 自動語言遞補</td>
<td>中等</td>
</tr>
<tr>
<td>組建資源套件</td>
<td>下載和安裝大小最小化的最後步驟</td>
<td>小型</td>
</tr>
<tr>
<td>移轉至 MRT 資源格式與 API</td>
<td>大幅縮小的檔案大小 (取決於現有的資源技術)</td>
<td>大型</td>
</tr>
</table>

## <a name="introduction"></a>簡介

大部分的非小型應用程式包含與應用程式碼分離之稱為*資源*的使用者介面元素 (對照在原始程式碼中製作的*硬式編碼值*)。 偏好資源甚於硬式編碼值的原因有數個 (例如讓非開發人員輕鬆編輯)，但其中一個重要原因是讓應用程式在執行階段針對相同的邏輯資源挑選不同表示法。 舉例來說，按鈕 (圖示中顯示影像) 上顯示的文字可能有所不同，端視使用者慣用語言、顯示裝置的特性，或使用者是否啟用任何輔助技術而定。

因此，主要資源管理技術的用途是在執行階段將提供邏輯或符號*資源名稱*(例如`SAVE_BUTTON_LABEL`) 的要求翻譯成一組可能*候選項目* (例如「儲存」、「Speichern」或「저장」) 的最佳實際*值* (例如「儲存」)。 MRT 提供這項功能，並讓應用程式使用稱為*限定詞*的各種不同屬性來識別出資源候選項目，例如使用者的語言、顯示縮放比例、使用者選取的主題，以及其他環境因素。 MRT 甚至支援自訂限定詞供有需要之應用程式使用 (例如，應用程式可為使用帳戶登入的使用者和來賓使用者提供不同的圖形資產，無需明確地將這個檢查加入他們的應用程式的每個部分)。 MRT 同時使用字串資源和檔案型資源，其中檔案型資源當作外部資料 (檔案本身) 的參考來實作。

### <a name="example"></a>範例

以下是簡單的應用程式範例，其有兩個上頭有文字標籤的按鍵 (`openButton`和`saveButton`)，以及一個當成標誌使用的 PNG 檔案 (`logoImage`)。 文字標籤當地語系化成英文和德文，而標誌針對正常桌面顯示 (100% 縮放比例) 和高解析度的手機 (300% 縮放比例) 進行最佳化。 請注意，此圖表呈現模型的高階、概念視圖，並不完全對應實作。

<p><img src="images\conceptual-resource-model.png"/></p>

在圖片中，應用程式碼參考三個邏輯資源名稱。 在執行階段，`GetResource`虛擬功能使用 MRT 在資源表中查詢這些資源名稱 (稱為 PRI 檔案)，並根據環境條件尋找最適當的候選項目 (使用者的語言和顯示器的縮放比例)。 對於標籤則會直接使用字串。 對於標誌圖像，字串會解譯為檔名，並從磁碟讀出檔案。 

如果使用者說的是英文或德文以外的語言，或具有100% 或300% 以外的顯示比例因數，則 MRT.LOG 會根據一組回溯規則來挑選「最靠近」的相符候選項（請參閱[資源管理系統](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))以取得更多背景）。

請注意，MRT 支援為一個以上的限定詞量身打造的資源。例如，標誌圖像包含的內嵌文字也需要進行當地語系化，而標誌會有四個候選項目︰EN/Scale-100、DE/Scale-100、EN/Scale-300 和 DE/Scale-300。

### <a name="sections-in-this-document"></a>本文件章節

下列各節概述將應用程式與 MRT 整合在一起所需的概略工作。

#### <a name="phase-0-build-an-application-package"></a>階段 0︰建置應用程式套件

本節概述如何將您現有的傳統型應用程式建置為應用程式套件。 此階段不使用任何 MRT 功能。

#### <a name="phase-1-localize-the-application-manifest"></a>階段 1︰ 當地語系化應用程式資訊清單

本節概述如何當地語系化應用程式的資訊清單 (如此才能讓正確地在 Windows Shell 中顯示)，同時仍使用您的舊版資源格式與 API 以封裝及尋找資源。 

#### <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>階段 2︰使用 MRT 識別及尋找資源

本節概述如何修改的應用程式程式碼 (及可能的資源配置) 以使用 MRT 尋找資源，同時仍然使用現有的資源格式與 API 載入和取用資源。 

#### <a name="phase-3-build-resource-packs"></a>階段 3︰建置資源套件

本節概述將您的資源分成不同的*資源套件*所需的最後變更，將下載 (和安裝) 的應用程式的大小縮到最小。

### <a name="not-covered-in-this-document"></a>未涵蓋於本文件中

完成上述的階段0-3 之後，您將會有一個可提交至 Microsoft Store 的應用程式「配套」，並藉由省略不需要的資源（例如，不會說話的語言），將使用者的下載和安裝大小降到最低。 採取最後一個步驟以進一步改善應用程式的大小和功能。

#### <a name="phase-4-migrate-to-mrt-resource-formats-and-apis"></a>階段 4︰ 移轉到 MRT 資源格式與 API

這個階段超出本文件範圍，它需要將您的資源 (尤其是字串) 從舊版格式 (例如 MUI Dll 或.NET 資源組件) 移入 PRI 檔案。 這能更節省更多下載與安裝的空間。 您也可以使用其他 MRT 功能，例如依據縮放比例將下載和安裝的影像檔案最小化、協助工具設定等等。

## <a name="phase-0-build-an-application-package"></a>階段 0︰建置應用程式套件

變更您的應用程式資源之前，必須先將目前包裝和安裝技術更換為標準 UWP 封裝及部署技術。 做法有三種：

* 如果您的桌面應用程式包含複雜的安裝程式，或您使用許多 OS 擴充點，您可以使用桌面應用程式轉換器工具，從現有的應用程式安裝程式（例如 MSI）產生 UWP 檔案配置和資訊清單資訊。
* 如果您的桌面應用程式具有相對較少的檔案或簡單的安裝程式，而且沒有擴充性攔截，您可以手動建立檔案配置和資訊清單資訊。
* 如果您是從來源重建，而且想要將應用程式更新為單純的 UWP 應用程式，您可以在 Visual Studio 中建立新的專案，並依賴 IDE 為您執行大部分的工作。

如果您想要使用傳統型[應用程式轉換器](https://www.microsoft.com/store/p/desktopappconverter/9nblggh4skzw)，請參閱[使用桌面應用程式轉換器封裝桌面應用程式](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-run-desktop-app-converter)，以取得轉換程式的詳細資訊。 如需完整的桌面轉換器範例，請參閱「[桌面橋接器至 UWP 範例」 github](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)存放庫。

如果您想要手動建立封裝，您必須建立一個目錄結構，其中包含應用程式的所有檔案（可執行檔和內容，但不是原始程式碼）和套件資訊清單檔案（. package.appxmanifest.xml）。 如需範例，請參閱[Hello，World GitHub 範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml)，但執行名為 `ContosoDemo.exe` 的桌面可執行檔的基本套件資訊清單檔如下所示，其中反<span style="background-color: yellow">白顯示的文字</span>會由您自己的值取代。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         IgnorableNamespaces="uap mp rescap">
    <Identity Name="Contoso.Demo"
              Publisher="CN=Contoso.Demo"
              Version="1.0.0.0" />
    <Properties>
    <DisplayName>Contoso App</DisplayName>
    <PublisherDisplayName>Contoso, Inc</PublisherDisplayName>
    <Logo>Assets\StoreLogo.png</Logo>
  </Properties>
    <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
    <Resources>
    <Resource Language="en-US" />
  </Resources>
    <Applications>
    <Application Id="ContosoDemo" Executable="ContosoDemo.exe" 
                 EntryPoint="Windows.FullTrustApplication">
    <uap:VisualElements DisplayName="Contoso Demo" BackgroundColor="#777777" 
                        Square150x150Logo="Assets\Square150x150Logo.png" 
                        Square44x44Logo="Assets\Square44x44Logo.png" 
        Description="Contoso Demo">
      </uap:VisualElements>
    </Application>
  </Applications>
    <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
</Package>
```

如需套件資訊清單檔案和封裝配置的詳細資訊，請參閱[應用程式套件資訊清單](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest)。

最後，如果您使用 Visual Studio 建立新的專案，並將現有的程式碼遷移到不同的範圍，請參閱[建立 "Hello，world" 應用程式](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)。 您可以將現有的程式碼包含在新的專案中，但您可能必須進行重大的程式碼變更（特別是在使用者介面中），才能以純 UWP 應用程式的形式執行。 這些變更不在本文件範圍內。

## <a name="phase-1-localize-the-manifest"></a>第1階段：將資訊清單當地語系化

### <a name="step-11-update-strings--assets-in-the-manifest"></a>步驟1.1：在資訊清單中更新 & 資產的字串

在第0階段中，您已為應用程式建立基本套件資訊清單（. package.appxmanifest.xml）檔案（根據提供給轉換器的值、從 MSI 解壓縮，或以手動方式輸入到資訊清單中），但它不會包含當地語系化的資訊，也不會支援額外的功能，例如高解析度啟動磚資產等等。

為確保您應用程式的名稱和描述已正確當地語系化，您必須在一組資源檔中定義一些資源，並更新套件資訊清單來參考它們。

#### <a name="creating-a-default-resource-file"></a>建立預設資源檔案

第一個步驟是以預設的語言 (例如美式英文)建立預設資源檔案。 您可以使用文字編輯器，或透過 Visual Studio 中的資源設計工具，手動執行此項作業。

當您想要手動建立資源：

1. 建立名為 `resources.resw` 的 XML 檔，並將它放置在您專案的 `Strings\en-us` 子資料夾中。 如果您的預設語言不是美式英文，請使用適當的 BCP-47 程式碼。
2. 在 XML 檔案中加入下列內容，其中<span style="background-color: yellow">醒目提示文字</span>部分會以您的預設語言的適當 App 文字取代。

> [!NOTE]
> 其中一些字串的長度有所限制。 如需詳細資訊，請參閱 [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements)。

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (English)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (English)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (English)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, USA</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (EN)</value>
  </data>
</root>
```

如果您想要使用 Visual studio 的設計工具︰

1. 在專案中建立 `Strings\en-us` 資料夾（或其他語言），並使用預設名稱 [`resources.resw`]，將**新專案**加入至專案的根資料夾。 請務必選擇 [**資源檔] （. .resw）** ，而不是 [**資源字典**]-[資源字典] 是 XAML 應用程式所使用的檔案。
2. 使用設計工具，輸入下列字串 (使用相同 `Names` 但將 `Values` 取代為應用程式的適當文字)：

<img src="images\editing-resources-resw.png"/>

> [!NOTE]
> 如果您從 Visual Studio 設計工具開始，您一律可以按 `F7`直接編輯 XML。 但是，如果您以最小的 XML 檔案啟動，*設計工具將無法辨識檔案*，因為檔案遺失許多額外的中繼資料。若要修正此問題，只需將重複使用的 XSD 資訊從設計工具產生的檔案，複製到您手工編輯的 XML 檔案即可。

#### <a name="update-the-manifest-to-reference-the-resources"></a>更新資訊清單以參考資源

在 `.resw` 檔案中定義值之後，下一步就是更新資訊清單來參考資源字串。 您可以再次直接編輯 XML 檔案，或依賴 Visual Studio 資訊清單設計工具。

如果您直接編輯 XML，請開啟 `AppxManifest.xml` 檔案，對<span style="background-color: lightgreen">醒目提示值</span>進行下列變更 - 使用這個*確切*文字，而非應用程式特定的文字。 不需要使用這些確切的資源名稱，您可以自行選擇，但是您的選擇必須完全符合 &mdash; 檔案內容。 這些名稱應該符合您在 `Names` 檔案中建立的 `.resw`，其以 `ms-resource:` 配置和 `Resources/` 命名空間做為首碼。 

> [!NOTE]
> 此程式碼片段已省略資訊清單的許多元素-請勿刪除任何專案！

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package>
  <Properties>
    <DisplayName>ms-resource:Resources/PackageDisplayName</DisplayName>
    <PublisherDisplayName>ms-resource:Resources/PublisherDisplayName</PublisherDisplayName>
  </Properties>
  <Applications>
    <Application>
      <uap:VisualElements DisplayName="ms-resource:Resources/ApplicationDisplayName"
        Description="ms-resource:Resources/ApplicationDescription">
        <uap:DefaultTile ShortName="ms-resource:Resources/TileShortName">
          <uap:ShowNameOnTiles>
            <uap:ShowOn Tile="square150x150Logo" />
          </uap:ShowNameOnTiles>
        </uap:DefaultTile>
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

如果您使用 Visual Studio 的資訊清單設計工具，請開啟 package.appxmanifest.xml 檔案，然後在 [*應用程式*] 索引標籤和 [*封裝*] 索引標籤中變更反<span style="background-color: lightgreen">白顯示的值</span>：

<img src="images\editing-application-info.png"/>
<img src="images\editing-packaging-info.png"/>

### <a name="step-12-build-pri-file-make-an-msix-package-and-verify-its-working"></a>步驟1.2：組建 PRI 檔案、建立 MSIX 套件，並確認其運作正常

您現在應該可以建置 `.pri` 檔案，並且部署應用程式以確認 [開始] 功能表顯示的資訊 (您的預設語言) 正確無誤。

如果您在 Visual Studio 中建置，只要按下 `Ctrl+Shift+B` 建置專案，然後在專案上按一下滑鼠右鍵，並從內容功能表選擇 `Deploy`。

如果您要手動建立，請遵循下列步驟來建立 `MakePRI` 工具的設定檔，並產生 `.pri` 檔案本身（如需詳細資訊，請參閱[手動應用程式封裝](/windows/msix/package/manual-packaging-root)）：

1. 從 [開始] 功能表中的 [ **Visual Studio 2017** ] 或 [ **Visual Studio 2019** ] 資料夾開啟開發人員命令提示字元。
2. 切換至專案根目錄（其中包含 package.appxmanifest.xml 檔案和**字串**資料夾的目錄）。
3. 輸入下列命令，使用您專案適用的名稱取代 "contoso_demo.xml"，用您的應用程式適用的預設語言取代 "en-US" (如果適用則保留 en-US)。 請注意，XML 檔案是建立在父目錄（**而不**是在專案目錄中），因為它不是應用程式的一部分（您可以選擇任何其他您想要的目錄，但請務必在未來的命令中替代）。

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

    您可以輸入 `makepri createconfig /?` 來查看每個參數運作狀況，但請摘要說明︰
      * `/cf` 設定設定檔案（此命令的輸出）
      * `/dq` 會設定預設的限定詞，在此案例中為 language `en-US`
      * `/pv` 設定平臺版本，在此案例中為 Windows 10
      * `/o` 將它設定為覆寫輸出檔案（如果存在的話）

4. 現在您擁有設定檔，請再次執行 `MakePRI` 以確實搜尋磁碟中的資源，並封裝到 PRI 檔案。 將 "contoso_demop.xml" 取代為您在上一個步驟中使用的 XML 檔名，並確實為輸入與輸出檔指定上層目錄： 

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

    您可以輸入 `makepri new /?` 來查看每個參數運作狀況，但請簡單地說明︰
      * `/pr` 設定專案根目錄（在此案例中為目前目錄）
      * `/cf` 設定在上一個步驟中建立的設定檔案
      * `/of` 設定輸出檔 
      * `/mf` 會建立對應檔（因此我們可以在稍後的步驟中排除封裝中的檔案）
      * `/o` 將它設定為覆寫輸出檔案（如果存在的話）

5. 現在您具有內含預設語言資源 (例如 en-US) 的 `.pri` 檔案。 若要確認運作一切正常，您可以執行下列命令︰

    ```CMD
    makepri dump /if ..\resources.pri /of ..\resources /o
    ```

    您可以輸入 `makepri dump /?` 來查看每個參數運作狀況，但請簡單地說明︰
      * `/if` 設定輸入檔案名 
      * `/of` 設定輸出檔案名（`.xml` 會自動附加）
      * `/o` 將它設定為覆寫輸出檔案（如果存在的話）

6. 最後，您可以在文字編輯器中開啟 `..\resources.xml`，並確認其中有列出您的 `<NamedResource>` 值 (例如 `ApplicationDescription` 和 `PublisherDisplayName`) 以及 `<Candidate>` 您選擇的預設語言值 (檔案開頭有其他內容，請先忽略)。

您可以開啟對應檔案 `..\resources.map.txt`，確認它包含專案所需的檔案（包括不屬於專案目錄的 PRI 檔案）。 重要的是，對應檔案將*不*包含 `resources.resw` 檔案的參考，因為該檔案的內容已經嵌入 PRI 檔案。 但是它將包含像是映像檔名之類的其他資源。

#### <a name="building-and-signing-the-package"></a>建置並登入套件 

現在建置了 PRI 檔案，接著您可以建置並登入套件︰

1. 若要建立應用程式套件，請執行下列命令，以您想要建立的 MSIX/AppX 檔案的名稱取代 `contoso_demo.appx`，並確定為檔案選擇不同的目錄（此範例使用上層目錄; 它可以是任何位置，但**不**應該是專案目錄）。

    ```CMD
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

    您可以輸入 `makeappx pack /?` 來查看每個參數運作狀況，但請簡單地說明︰
      * `/m` 設定要使用的資訊清單檔
      * `/f` 設定要使用的對應檔（在上一個步驟中建立） 
      * `/p` 設定輸出封裝名稱
      * `/o` 將它設定為覆寫輸出檔案（如果存在的話）

2. 建立封裝之後，必須將其簽署。 取得簽署憑證最簡單的方式，就是在 Visual Studio 中建立空白的通用 Windows 專案，並複製它所建立的 `.pfx` 檔案，但是您可以使用 `MakeCert` 和 `Pvk2Pfx` 公用程式來手動建立一個，如[如何建立應用程式套件簽署憑證](https://docs.microsoft.com/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate)中所述。

    > [!IMPORTANT]
    > 如果您手動建立簽署憑證，請確定您將檔案放在來源專案或套件來源以外的不同目錄中，否則可能會納入封裝中，包括私密金鑰！

3. 若要登入套件，請使用下列命令。 請注意，`Publisher` 的 `Identity` 元素中所指定的 `AppxManifest.xml`，必須符合憑證的 `Subject` (這**不是**`<PublisherDisplayName>` 元素，而是對使用者顯示的當地語系化顯示名稱)。 一如往常，將 `contoso_demo...` 檔名取代為專案的適用名稱，並且 (**非常重要**) 確定 `.pfx` 檔案不在目錄的目錄中 (否則可能建立為套件的一部分，包括私用簽署金鑰！)︰

    ```CMD
    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx
    ```

    您可以輸入 `signtool sign /?` 來查看每個參數運作狀況，但請簡單地說明︰
      * `/fd` 設定檔案摘要演算法（SHA256 是 AppX 的預設值）
      * `/a` 會自動選取最佳憑證
      * `/f` 指定包含簽署憑證的輸入檔

最後，您可以按兩下 `.appx` 檔案進行安裝，若慣用好命令列則可以開啟 PowerShell 提示字元，變更到包含套件的目錄，然後輸入下列命令 (將 `contoso_demo.appx` 取代為您的套件名稱)：

```CMD
add-appxpackage contoso_demo.appx
```

如果您收到有關憑證不受信任的錯誤訊息，請確定該訊息已加入到電腦存放區中 (**不是**使用者市集)。 若要將憑證加入到電腦存放區，您可以使用命令列或 [Windows 檔案總管]。

使用命令列：

1. 以系統管理員身分執行 Visual Studio 2017 或 Visual Studio 2019 命令提示字元。
2. 切換到包含 `.cer` 檔案的目錄 (記得務必確定這是在您的來源或專案目錄之外！)
3. 輸入下列命令，以您的檔案名稱取代 `contoso_demo.cer`︰
    ```CMD
    certutil -addstore TrustedPeople contoso_demo.cer
    ```
    
    您可以執行 `certutil -addstore /?` 來查看每個參數運作狀況，但請簡單地說明︰
      * `-addstore` 將憑證新增至憑證存放區
      * `TrustedPeople` 指出要放置憑證的存放區

使用 Windows 檔案總管：

1. 瀏覽至包含 `.pfx` 檔案的資料夾
2. 按兩下`.pfx`檔案後應該會出現**憑證匯入精靈**
3. 選擇 `Local Machine`，然後按一下 `Next`
4. 接受 使用者帳戶控制管理員 提高許可權提示（如果出現的話），然後按一下 `Next`
5. 輸入私密金鑰的密碼（如果有的話），然後按一下 `Next`
6. 選取 `Place all certificates in the following store`
7. 按一下 `Browse`，並選擇 `Trusted People` 資料夾 (**非**受信任的發行者)
8. 按一下 `Next`，然後 `Finish`

將憑證加入到 `Trusted People`存放區，再次嘗試安裝套件。

您現在應該會看到您的應用程式出現在 [開始] 功能表的 [所有應用程式] 清單，附帶來自 `.resw` / `.pri` 檔案的正確資訊。 如果您看到空白字串或字串 `ms-resource:...`，表示出現錯誤，此時請再次檢查編輯內容，確定內容的正確性。 如果在 [開始] 功能表中以滑鼠右鍵按一下您的應用程式，您可以將它釘選為動態磚，並確認該處顯示的資訊也是正確的。

### <a name="step-13-add-more-supported-languages"></a>步驟 1.3︰新增更多支援的語言

對套件資訊清單進行變更並建立初始 `resources.resw` 檔案之後，新增其他語言會很容易。

#### <a name="create-additional-localized-resources"></a>建立其他當地語系化的資源

首先，建立其他當地語系化的資源值。 

在 `Strings` 資料夾，使用的適當 BCP-47 代碼 (例如，`Strings\de-DE`) 為您支援的每一種語言建立其他資料夾。 在這些的每一個資料夾內，建立 `resources.resw` 檔案 (使用 XML 編輯器或 Visual Studio 設計工具)，其包含翻譯的資源值。 假設您在某處已經有可用的當地語系化的字串，而且您只需要將它們複製到 `.resw` 檔案。本文件未涵蓋翻譯步驟。 

例如，`Strings\de-DE\resources.resw` 檔案可能呈現這樣的情況，其<span style="background-color: yellow">反白文字</span>從 `en-US` 變更而來：

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (German)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (German)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (German)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, DE</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (DE)</value>
  </data>
</root>
```

下列步驟假設您有 `de-DE` 和 `fr-FR` 兩者的新增資源，但任何語言均以遵循相同的模式。

#### <a name="update-the-package-manifest-to-list-supported-languages"></a>更新套件資訊清單以列出支援的語言

必須更新套件資訊清單，以列出應用程式支援的語言。 Desktop App Converter 會新增預設的語言，但其他的語言則必須明確地新增。 如果直接編輯 `AppxManifest.xml` 檔案，請依下列方式更新 `Resources`，依您需要的數量新增元素，並且替換您支援的<span style="background-color: yellow">適當語言</span>，確保清單中的第一個項目為預設 (遞補) 語言。 在此範例中，預設值為英文 (美國)，同時也支援德文 (德國) 和法文 (法國)︰

```xml
<Resources>
  <Resource Language="EN-US" />
  <Resource Language="DE-DE" />
  <Resource Language="FR-FR" />
</Resources>
```

如果您使用的是 Visual Studio，您應不需要做任何動作。如果查看 `Package.appxmanifest`，您應該會看到特殊的 <span style="background-color: yellow">x 產生</span>值，致使組建程序插入在您專案中找到的語言 (根據 BCP-47 代碼命名的資料夾)。 請注意，這不是實際封裝資訊清單的有效值;僅適用于 Visual Studio 專案：

```xml
<Resources>
  <Resource Language="x-generate" />
</Resources>
```

#### <a name="re-build-with-the-localized-values"></a>使用當地語系化值重建

現在您可以再次建置及部署應用程式，如果您變更 Windows 中的語言喜好設定，應該會看到出現在 [開始] 功能表中的新當地語系化值 (下方出現有關如何變更您的語言的指示)。

針對 Visual studio，您一樣只能使用 `Ctrl+Shift+B` 建置，並以滑鼠右鍵按一下專案以`Deploy`。

如果您手動建立專案，請遵循上述相同步驟，但在建立設定檔時，將其他語言 (以底線分隔) 加入至預設的限定詞清單 (`/dq`)。 例如，支援在上一個步驟加入的英文、德文和法文資源︰

```CMD
makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

這會建立一個包含可以輕鬆用於測試的所有指定語言的 PRI 檔案。 如果資源的大小總和很小，或是您只支援極少數的語言，可能您傳送的應用程式可以接受這情況。除非您因為有需要建置不同語言套件的額外工作，而需要將所安裝/下載的資源大小降到最低。

#### <a name="test-with-the-localized-values"></a>使用當地語系化值進行測試

若要測試當地語系化的新變更，您只需將慣用的 UI 語言加入到 Windows。 並不需要下載語言套件、重新開機，或者以外國語言顯示整個 Windows UI。 

1. 執行 `Settings` App (`Windows + I`)
2. 移至 `Time & language`。
3. 移至 `Region & language`。
4. 按一下 `Add a language`
5. 輸入 (或選取) 您想要的語言 (例如 `Deutsch` 或 `German`)
 * 如果有附屬語言，請選擇您想要的一種語言 (例如 `Deutsch / Deutschland`)
6. 在語言清單中選取新的語言
7. 按一下 `Set as default`

現在開啟 [開始] 功能表以搜尋您的應用程式，然後會看到選定語言的當地語系化值 (其他的應用程式可能也會呈現當地語系化)。 如果您沒有立刻看到當地語系化的名稱，請稍待數分鐘，等 [開始] 功能表的快取重新整理。 若要回復至您的母語，只需將其設定為語言清單中的預設語言。 

### <a name="step-14-localizing-more-parts-of-the-package-manifest-optional"></a>步驟1.4：當地語系化套件資訊清單的更多部分（選擇性）

封裝資訊清單的其他區段可以當地語系化。 舉例來說，如果您的應用程式處理檔案副檔名，資訊清單中就應具有 `windows.fileTypeAssociation` 副檔名，方法是完全依照所示來使用<span style="background-color: lightgreen">綠色反白文字</span> (因為它會參照資源)，然後將<span style="background-color: yellow">黃色反白文字</span>取代為應用程式特定資訊︰

```xml
<Extensions>
  <uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="default">
      <uap:DisplayName>ms-resource:Resources/FileTypeDisplayName</uap:DisplayName>
      <uap:Logo>Assets\StoreLogo.png</uap:Logo>
      <uap:InfoTip>ms-resource:Resources/FileTypeInfoTip</uap:InfoTip>
      <uap:SupportedFileTypes>
        <uap:FileType ContentType="application/x-contoso">.contoso</uap:FileType>
      </uap:SupportedFileTypes>
    </uap:FileTypeAssociation>
  </uap:Extension>
</Extensions>
```

您也可以使用 Visual Studio 資訊清單設計工具、使用 `Declarations` 索引標籤、記下<span style="background-color: lightgreen">反白值</span>以新增此資訊：

<p><img src="images\editing-declarations-info.png"/></p>

立即將對應的資源名稱加入到每一個 `.resw` 檔案，並將<span style="background-color: yellow">反白文字</span>取代為應用程式的適用文字 (務必針對*每一種支援的語言執行此動作！* )：

```xml
... existing content...
<data name="FileTypeDisplayName">
  <value>Contoso Demo File</value>
</data>
<data name="FileTypeInfoTip">
  <value>Files used by Contoso Demo App</value>
</data>
```

這接著會顯示部分的 Windows 殼層，例如檔案總管︰

<p><img src="images\file-type-tool-tip.png"/></p>

如以往建置並測試套件，執行應該顯示新的 UI 字串的任何新案例。

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>階段 2︰使用 MRT 識別及尋找資源

前一節顯示有關使用 MRT 當地語系化您的應用程式的資訊檔案，讓 Windows Shell 可以正確地顯示應用程式的名稱及其他中繼資料的方式。 不需要為此變更程式碼；它只需要使用 `.resw` 檔案，以及一些額外的工具。 本節顯示如何使用 MRT 找出使用您現有資源格式，以及使用具最少變更的現有資源處理程式碼的資源。

### <a name="assumptions-about-existing-file-layout--application-code"></a>有關現有檔案配置與應用程式程式碼的假設

由於當地語系化 Win32 傳統型應用程式的方法有很多，本文會做出一些關於需要對應到特定環境之現有應用程式結構的簡單化假設。 您可能需要針對現有的程式碼基底或資源配置做一些變更來符合 MRT 的需求，而這些大部分都不在本文件範圍內。

#### <a name="resource-file-layout"></a>資源檔案配置

本文假設您的當地語系化資源都有相同的檔案名（例如 `contoso_demo.exe.mui` 或 `contoso_strings.dll` 或 `contoso.strings.xml`），但它們放在具有 BCP-47 名稱（`en-US`、`de-DE`等等）的不同資料夾中。 這無關於您有多少資源檔案、其名稱為何、其檔案格式 / 相關的 API 為何等等。唯一重要是每個*邏輯*資源都有相同的檔名 (各位於不同的*實體*目錄)。 

以反例來看，如果您的應用程式使用具單一 `Resources` 目錄的一般檔案結構 (包含檔案 `english_strings.dll` 和 `french_strings.dll`)，它不會和 MRT 對應得很好。 比較好的結構是包含子目錄及檔案 `Resources` 和 `en\strings.dll` 的 `fr\strings.dll` 目錄。 也可以使用相同的基本檔名，但具有內嵌限定詞，例如 `strings.lang-en.dll` 和 `strings.lang-fr.dll`，但使用含語言代碼的目錄在概念上更加簡單，這也是我們著重的部分。

>[!NOTE]
> 即使您無法遵循此檔案命名慣例，仍然可以使用 MRT.LOG 和封裝的優點;這只需要更多工具。

舉例來說，應用程式在名為 <span style="background-color: yellow">ui.txt</span> 的簡單文字檔中可能有一組自訂 UI 命令 (用於按鈕標籤等) 排列在 <span style="background-color: yellow">UICommands</span> 資料夾下︰

<blockquote>
<pre>
+ ProjectRoot
|--+ Strings
|  |--+ en-US
|  |  \--- resources.resw
|  \--+ de-DE
|     \--- resources.resw
|--+ <span style="background-color: yellow">UICommands</span>
|  |--+ en-US
|  |  \--- <span style="background-color: yellow">ui.txt</span>
|  \--+ de-DE
|     \--- <span style="background-color: yellow">ui.txt</span>
|--- AppxManifest.xml
|--- ...rest of project...
</pre>
</blockquote>

#### <a name="resource-loading-code"></a>資源載入程式碼

本文假設您在程式碼中的某個時間點，想要找出包含當地語系化資源的檔案，然後將它載入，然後使用它。 用於載入資源的 API、用來擷取資源的 API 等並不重要。 在虛擬程式碼中，有三個基本步驟︰

<blockquote>
<pre>
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
</pre>
</blockquote>

在這個程序中 MRT 只需要變更前兩個步驟 - 如何判斷最佳的候選項目資源，以及如何找到它們。 您不需要變更載入或使用資源的方法 (但它會在給您需要時提供工具)。

例如，應用程式可能會使用 Win32 API `GetUserPreferredUILanguages`、CRT 函數 `sprintf` 和 Win32 API `CreateFile` 取代上述三個虛擬程式碼功能，然後手動剖析文字檔，尋找 `name=value` 配對。 (詳細資料並不重要；這只是闡述 MRT 對於處理找到的資源所使用的技術沒有任何影響)。

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>步驟 2.1︰變更程式碼以使用 MRT 尋找檔案

切換程式碼以使用 MRT 尋找資源並不難。 它需要使用少量的 WinRT 類型和數行的程式碼。 您將使用的主要類型如下所示︰

* [ResourceContext](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext)，這會封裝目前使用中的一組限定詞值 (語言、縮放比例等)
* [ResourceManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemanager) (WinRT 版本，不是 .NET 版本)，其允許從 PRI 檔案存取所有的資源
* [ResourceMap](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemap)，代表 PRI 檔案中的一組特定子集資源 (在此範例中，檔案型資源與字串資源)
* [NamedResource](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource)，代表邏輯資源和其所有可能的候選項目
* [ResourceCandidate](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcecandidate)，代表單一具體候選項目資源 

在虛擬程式碼中，您會解析特定的資源檔案名稱 (如上述範例中的 `UICommands\ui.txt`)，如下所示︰

<blockquote>
<pre>
// Get the ResourceContext that applies to this app
set resourceContext = ResourceContext.GetForViewIndependentUse()
    
// Get the current ResourceManager (there's one per app)
set resourceManager = ResourceManager.Current
    
// Get the "Files" ResourceMap from the ResourceManager
set fileResources = resourceManager.MainResourceMap.GetSubtree("Files")
    
// Find the NamedResource with the logical filename we're looking for,
// by indexing into the ResourceMap
set desiredResource = fileResources["UICommands\ui.txt"]
    
// Get the ResourceCandidate that best matches our ResourceContext
set bestCandidate = desiredResource.Resolve(resourceContext)
   
// Get the string value (the filename) from the ResourceCandidate
set absoluteFileName = bestCandidate.ValueAsString
</blockquote>
</pre>

請特別留意，程式碼**不會**要求特定的語言資料夾如 `UICommands\en-US\ui.txt`，即使檔案是以此方式存在於磁碟上。 它反而會要求*邏輯*檔名 `UICommands\ui.txt`，並且依賴 MRT 在磁碟的其中一個語言目錄尋找適當的檔案。

在這裡，範例應用程式可能會繼續使用 `CreateFile` 載入 `absoluteFileName` 並像之前一樣剖析 `name=value` 配對；在應用程式中不需要變更該邏輯。 如果您以 C# 或 C++/CX 撰寫，實際的程式碼不會比這個複雜多少 (或事實上許多中繼變數都可以省略) - 請參閱下方**載入 .NET 資源**一節。 C++/WRL 所架構的應用程式將會因使用以低階 COM 為基礎的 API 來啟動並呼叫 WinRT API，而變得更加複雜，但是您採取的基本步驟都相同 - 請參閱下方**載入 Win32 MUI 資源**一節。

#### <a name="loading-net-resources"></a>正在載入 .NET 資源

因為 .NET 具有尋找與載入資源 (稱為「衛星組件」) 的內建機制，在上述綜合範例中並無可取代的明確程式碼 - 在 .NET 中，您只需要適當目錄的資源 DLL，而且會自動幫您找出。 當應用程式使用資源套件封裝為 MSIX 或 AppX 時，目錄結構會有些不同，而不是讓資原始目錄成為主要應用程式目錄的子目錄，而是其對等的（如果使用者不會在其喜好設定中列出語言）。 

舉例來說，想一想 .NET 應用程式具有下列版面配置，其中所有檔案都存在於 `MainApp` 資料夾下︰

<blockquote>
<pre>
+ MainApp
|--+ en-us
|  \--- MainApp.resources.dll
|--+ de-de
|  \--- MainApp.resources.dll
|--+ fr-fr
|  \--- MainApp.resources.dll
\--- MainApp.exe
</pre>
</blockquote>

轉換至 AppX 之後，版面配置看起來就像這樣，假設 `en-US` 是預設的語言，而使用者的語言清單中有列出德文和法文︰

<blockquote>
<pre>
+ WindowsAppsRoot
|--+ MainApp_neutral
|  |--+ en-us
|  |  \--- <span style="background-color: yellow">MainApp.resources.dll</span>
|  \--- MainApp.exe
|--+ MainApp_neutral_resources.language_de
|  \--+ de-de
|     \--- <span style="background-color: yellow">MainApp.resources.dll</span>
\--+ MainApp_neutral_resources.language_fr
   \--+ fr-fr
      \--- <span style="background-color: yellow">MainApp.resources.dll</span>
</pre>
</blockquote>

由於當地語系化的資源已不再存在於主要可執行檔安裝位置下的子目錄中，因此內建的 .NET 資源解決方式未能成功運作。 幸好 .NET 有定義良好的機制可處理失敗的組件載入操作 - `AssemblyResolve`事件。 使用 MRT 的 .NET 應用程式註冊這個活動，並為 .NET 資源子系統提供遺失的組件。 

有關如何使用 WinRT API 尋找 .NET 使用過的衛星組件的精簡範例，如下所示。雖然您可以看到所呈現的程式碼與上述虛擬程式碼有相近的對應，但會刻意壓縮來顯示最少實作，通過的 `ResolveEventArgs` 會提供我們要尋找的組件名稱。 此程式碼 (詳細的註解與錯誤處理) 的可執行版本，可在 `PriResourceRsolver.cs`GitHub 上的 [.NET 組件解析程式**樣本**中的檔案 ](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DotNetSatelliteAssemblyDemo) 中找到。

```csharp
static class PriResourceResolver
{
  internal static Assembly ResolveResourceDll(object sender, ResolveEventArgs args)
  {
    var fullAssemblyName = new AssemblyName(args.Name);
    var fileName = string.Format(@"{0}.dll", fullAssemblyName.Name);

    var resourceContext = ResourceContext.GetForViewIndependentUse();
    resourceContext.Languages = new[] { fullAssemblyName.CultureName };

    var resource = ResourceManager.Current.MainResourceMap.GetSubtree("Files")[fileName];

    // Note use of 'UnsafeLoadFrom' - this is required for apps installed with AppX, but
    // in general is discouraged. The full sample provides a safer wrapper of this method
    return Assembly.UnsafeLoadFrom(resource.Resolve(resourceContext).ValueAsString);
  }
}
```

指定上述類別後，初期您想在應用程式的開機程式碼中某處新增下列項目 (在需要載入任何當地語系化的資源之前)︰

```csharp
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

.NET 執行階段每當找不到資源 DLL 時，就會提升 `AssemblyResolve` 事件，此時所提供的事件處理常式會透過 MRT 尋找所需的檔案，並傳回組件。

> [!NOTE]
> 如果您的應用程式已經有適用于其他用途的 `AssemblyResolve` 處理常式，您必須將資源解析程式代碼與現有的程式碼整合。

#### <a name="loading-win32-mui-resources"></a>載入 Win32 MUI 資源

載入 Win32 MUI 資源基本上相同於載入 .NET 衛星組件，但會改用 C++/CX 或 C++/WRL 程式碼。 若使用 C++/CX，符合上述 C# 程式碼的程式碼就會簡單非常多，但會使用 C++ 語言擴充功能、編譯器切換，以及您可能想要避免發生的其他執行階段竊聽。 若是如此，使用 C++/WRL 可用更詳細的程式碼代價提供影響低許多的解決方案。 雖然如此，如果您熟悉 ATL 程式設計 (或一般 COM)，WRL 便應覺得熟悉。 

下列函式範例顯示使用 C++/WRL 載入特定資源 DLL，並傳回可用於載入進一步資源的 `HINSTANCE`，方法是使用一般的 Win32 資源 API。 請注意，不像使用 .NET 執行階段要求之語言來明確初始化 `ResourceContext` 的 C# 範例，此代碼依賴使用者目前的語言。

```cpp
#include <roapi.h>
#include <wrl\client.h>
#include <wrl\wrappers\corewrappers.h>
#include <Windows.ApplicationModel.resources.core.h>
#include <Windows.Foundation.h>
   
#define IF_FAIL_RETURN(hr) if (FAILED((hr))) return hr;
    
HRESULT GetMrtResourceHandle(LPCWSTR resourceFilePath,  HINSTANCE* resourceHandle)
{
  using namespace Microsoft::WRL;
  using namespace Microsoft::WRL::Wrappers;
  using namespace ABI::Windows::ApplicationModel::Resources::Core;
  using namespace ABI::Windows::Foundation;
    
  *resourceHandle = nullptr;
  HRESULT hr{ S_OK };
  RoInitializeWrapper roInit{ RO_INIT_SINGLETHREADED };
  IF_FAIL_RETURN(roInit);
    
  // Get Windows.ApplicationModel.Resources.Core.ResourceManager statics
  ComPtr<IResourceManagerStatics> resourceManagerStatics;
  IF_FAIL_RETURN(GetActivationFactory(
    HStringReference(
    RuntimeClass_Windows_ApplicationModel_Resources_Core_ResourceManager).Get(),
    &resourceManagerStatics));
    
  // Get .Current property
  ComPtr<IResourceManager> resourceManager;
  IF_FAIL_RETURN(resourceManagerStatics->get_Current(&resourceManager));
    
  // get .MainResourceMap property
  ComPtr<IResourceMap> resourceMap;
  IF_FAIL_RETURN(resourceManager->get_MainResourceMap(&resourceMap));
    
  // Call .GetValue with supplied filename
  ComPtr<IResourceCandidate> resourceCandidate;
  IF_FAIL_RETURN(resourceMap->GetValue(HStringReference(resourceFilePath).Get(),
    &resourceCandidate));
    
  // Get .ValueAsString property
  HString resolvedResourceFilePath;
  IF_FAIL_RETURN(resourceCandidate->get_ValueAsString(
    resolvedResourceFilePath.GetAddressOf()));
    
  // Finally, load the DLL and return the hInst.
  *resourceHandle = LoadLibraryEx(resolvedResourceFilePath.GetRawBuffer(nullptr),
    nullptr, LOAD_LIBRARY_AS_DATAFILE | LOAD_LIBRARY_AS_IMAGE_RESOURCE);
    
  return S_OK;
}
```

## <a name="phase-3-building-resource-packs"></a>階段 3︰建置資源套件

既然您已經擁有包含所有資源的「肥包」(Fat Pack)，便可用兩個路徑來建置不同的主要套件和資源套件，以最小化下載並安裝的大小︰

* 取用現有的肥包，並透過[套件組合產生器工具](https://www.microsoft.com/store/apps/9nblggh43pmq)執行以自動建立資源套件。 如果您有一個已經產生肥包的組建系統，而且想要後置處理以產生資源套件時，這是優先選用的方法。
* 直接產生個人資源套件，並將這些套件建置到套件組合中。 如果您對於您的組建系統有更多控制權，而且可以直接建置套件時，這是優先選用的方法。

### <a name="step-31-creating-the-bundle"></a>步驟 3.1︰建立套件組合

#### <a name="using-the-bundle-generator-tool"></a>使用套件組合產生器工具

若要使用套件組合產生器工具，為套件建立的 PRI 檔案，需要手動更新，以移除 `<packaging>` 一節。

如果您使用 Visual Studio，請參閱[確定裝置上是否已安裝資源，不論裝置是否需要](https://docs.microsoft.com/en-us/previous-versions/dn482043(v=vs.140))，您可以建立 `priconfig.packaging.xml` 和 `priconfig.default.xml`的檔案，以取得如何將所有語言建立到主要套件的相關資訊。

如果手動編輯檔案，請依照下列步驟執行︰ 

1. 以如同以往的相同方式建立設定檔，取代路徑、檔案名稱及語言︰

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o
    ```

2. 手動開啟建立的 `.xml` 檔案，並刪除整個 `&lt;packaging&rt;` 分節 (其他全部保持不變)︰

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes" ?> 
    <resources targetOsVersion="10.0.0" majorVersion="1">
      <!-- Packaging section has been deleted... -->
      <index root="\" startIndexAt="\">
        <default>
        ...
        ...
    ```

3. 如同以往建置 `.pri` 檔案和 `.appx` 套件時，使用更新的設定檔及適當的目錄和檔案名稱 (請見上述以取得有關下列命令的詳細資訊)︰

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

4. 建立封裝之後，使用下列命令來建立配套，並使用適當的目錄和檔案名：

    ```CMD
    BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo
    ```

現在您可以移至最後一個步驟，也就是簽署（請參閱下文）。

#### <a name="manually-creating-resource-packages"></a>手動建立資源套件

手動建立資源套件需要執行一組稍有不同的命令來建置不同的 `.pri` 和 `.appx` 檔案 - 這些全都和上述用來建立肥包相似，這樣就可以將解釋的必要性降到最低。 注意︰所有命令都假設目前的目錄是包含 `AppXManifest.xml` 檔案的目錄，但所有檔案全放置在上層目錄 (必要時可以使用不同的目錄，但不可使用這些檔案損壞專案目錄)。 一如往常，將 "Contoso" 檔名取代為您自己的檔案名稱。

1. 使用下列命令來建立設定檔的名稱，**只**命名預設的語言，做為預設限定詞 - 在本案例為 `en-US`：

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

2. 為主要套件建立預設的 `.pri` 和 `.map.txt` 檔案，並針對專案中找到的每一種語言建立另一組檔案，使用的命令如下所示︰

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

3. 使用下列命令來建立主要套件 (其中包含可執行的程式碼和預設語言資源)。 一如往常，依需要變更名稱，但您應將套件放在不同的目錄，使之後建立套件組合的作業會更加容易 (此範例使用 `..\bundle` 目錄)：

    ```CMD
    makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o
    ```

4. 建立主要套件之後，每一種額外的語言使用下列命令一次 (也就是針對上一個步驟產生的語言對應檔重複執行此命令)。 輸出項目一樣須放在不同的目錄 (和主要套件一樣)。 請注意，語言可**同時**在 `/f` 選項和 `/p` 選項中指定，並使用新的 `/r` 引數 (表示需要資源封裝)︰

    ```CMD
    makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o
    ```

5. 將所有套件從套件組合目錄併入單一 `.appxbundle` 檔案。 新的 `/d` 選項指定用於套件組合所有檔案的目錄 (這就是為何 `.appx` 檔案在上一個步驟置放到不同目錄的原因)：

    ```CMD
    makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o
    ```

建立封裝的最後一個步驟是簽署。

### <a name="step-32-signing-the-bundle"></a>步驟 3.2︰簽署套件組合

一旦建立好 `.appxbundle` 檔案 (不是透過套件組合產生器工具，就是手動建立)，您將會有一支包含主要套件以及所有資源套件的單一檔案。 最後一個步驟是簽署檔案，讓 Windows 順利安裝︰

```CMD
signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle
```

這將會產生一個已簽署的 `.appxbundle` 檔案，檔案內含主要套件以及所有語言特定的資源套件。 它可以像套件檔案一樣，按兩下即可安裝應用程式，以及任何以使用者 Windows 語言喜好設定為基礎的適當語言。

## <a name="related-topics"></a>相關主題

* [針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md)