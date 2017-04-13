---
author: normesta
Description: "除了適用於所有 UWP app 的一般 API 外，某些擴充功能與 API 僅適用於已轉換的傳統型應用程式。 本文說明這些擴充功能及其使用方式。"
Search.Product: eADQiWindows 10XVcnh
title: "傳統型轉 UWP 橋接器應用程式擴充功能"
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.openlocfilehash: a3c29f0d36cec9a5816d5c20eb43a6634260cc43
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-uwp-bridge-app-extensions"></a>傳統型轉 UWP 橋接器：應用程式擴充功能

您可以運用各式各樣的通用 Windows 平台 (UWP) API，增強已轉換的傳統型應用程式功能。 不過，除了適用於所有 UWP app 的一般 API 外，某些擴充功能與 API 僅適用於已轉換的傳統型應用程式。 這些功能著重於諸如使用者登入時啟動處理程序，以及檔案總管整合等案例，且其設計用途在於讓原始傳統型應用程式與已轉換應用程式套件之間的轉換運作更加順暢。

本文說明這些擴充功能及其使用方式。 大部分擴充功能皆需要手動修改已轉換應用程式的資訊清單檔案，其中包含有關您應用程式所用擴充功能的宣告。 若要編輯資訊清單，請在 Visual Studio 方案中以滑鼠右鍵按一下 **Package.appxmanifest** 檔案，然後選取 [檢視程式碼]**。

## <a name="manifest-xml-namespaces"></a>資訊清單 XML 命名空間

傳統型橋接器的應用程式擴充功能需要數個不同的 XML 命名空間。 這些命名空間應該在 `<Package>` 項目中定義，位於資訊清單檔案的根目錄中，如下所示：

```xml
<Package
  ...
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  ...
  IgnorableNamespaces="uap uap2 uap3 mp rescap desktop">
```

如果您的資訊清單檔案發出編譯器錯誤，請確認已加入所有必要的命名空間，且子 XML 項目前面已加上正確的命名空間。 您也可以參考完整可運作的資訊清單檔案，在[GitHub 上的傳統型橋接器範例儲存庫](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)中。

## <a name="startup-tasks"></a>啟動工作

啟動工作能讓您的應用程式在每次使用者登入時，自動執行可執行檔。

若要宣告啟動工作，請將下列項目新增至應用程式的資訊清單︰

```XML
<desktop:Extension Category="windows.startupTask" Executable="bin\MyStartupTask.exe" EntryPoint="Windows.FullTrustApplication">
    <desktop:StartupTask TaskId="MyStartupTask" Enabled="true" DisplayName="My App Service" />
</desktop:Extension>
```
- *Extension Category* 的值應一律為 "windows.startupTask"。
- *Extension Executable* 是要啟動之 .exe 檔案的相對路徑。
- *Extension EntryPoint* 的值應一律為 "Windows.FullTrustApplication"。
- *StartupTask TaskId* 是您工作的唯一識別碼。 應用程式可使用此識別碼呼叫 [**Windows.ApplicationModel.StartupTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) 類別中的 API，運用程式設計方式啟用或停用啟動工作。
- *StartupTask Enabled* 指出工作第一次啟動時會是啟用或停用。 下次使用者登入時，將會執行已啟用的工作 (除非使用者將其停用)。
- *StartupTask DisplayName* 是出現在 [工作管理員] 中的工作名稱。 此字串可使用 ```ms-resource``` 進行當地語系化。

應用程式可宣告多個啟動工作；每個工作將會個別引發和執行。 所有啟動工作皆會顯示在 [工作管理員] 中的 **\[啟動\]** 索引標籤下方，且具有在應用程式的資訊清單中指定的名稱，以及應用程式的圖示。 [工作管理員] 會自動分析您工作的啟動影響。 使用者可以選擇透過 [工作管理員] 來手動停用應用程式的啟動工作；若使用者停用工作，則您無法以程式設計方式重新啟用該工作。

## <a name="app-execution-alias"></a>應用程式執行別名

應用程式執行別名可讓您指定應用程式的關鍵字名稱。 使用者或其他處理程序可使用此關鍵字輕鬆啟動您的應用程式，就如同其位於 PATH 變中數一般 (例如從 [執行] 或命令提示字元)，而無須提供完整路徑。 例如，若您宣告別名「Foo」，則使用者可自 cmd.exe 輸入「Foo Bar.txt」，而系統會使用處於啟用事件引數一部分的「Bar.txt」路徑來啟用您的應用程式。

若要指定應用程式執行別名，請新增下列項目至您應用程式的資訊清單：

```XML
<uap3:Extension Category="windows.appExecutionAlias" Executable="exes\launcher.exe" EntryPoint="Windows.FullTrustApplication">
    <uap3:AppExecutionAlias>
        <desktop:ExecutionAlias Alias="Foo.exe" />
    </uap3:AppExecutionAlias>
</uap3:Extension>
```

- *Extension Category* 的值應一律為 "windows.appExecutionAlias"。
- *Extension Executable* 是叫用別名時要啟動的可執行檔相對路徑。
- *Extension EntryPont* 的值應一律為 "Windows.FullTrustApplication"。
- *ExecutionAlias Alias* 是您應用程式的簡稱。 其必須一律以副檔名「.exe」結尾。

針對套件中的每個應用程式，您僅可指定單一應用程式執行別名。 若有多個應用程式註冊相同的別名，系統將會叫用最後一個註冊的別名，因此請務必選擇絕對不會遭其他應用程式覆寫的唯一別名。

## <a name="protocol-associations"></a>通訊協定關聯

通訊協定關聯支援已轉換 App 與其他程式或系統元件間的互通性案例。 若您使用通訊協定啟動已轉換的 App，則可指定特定參數傳送至該 App 的啟用事件引數，讓其據以運作。 請注意，參數僅支援供已轉換、完全信任的應用程式使用；UWP app 無法使用參數。  

若要宣告通訊協定關聯，請將下列項目新增至應用程式的資訊清單：

```XML
<uap3:Extension Category="windows.protocol">
    <uap3:Protocol Name="myapp-cmd" Parameters="/p &quot;%1&quot;" />
</uap3:Extension>
```

- *Extension Category* 的值應一律為 "windows.protocol"。
- *Protocol Name* 是通訊協定的名稱。
- *Protocol Parameters* 是在啟用應用程式時會傳送至應用程式，做為事件引數之參數與值的清單。 請注意，若變數可以包含檔案路徑，您應使用引號括住值，使其在傳送包含空格的路徑時不會中斷。

## <a name="files-and-file-explorer-integration"></a>檔案與檔案總管整合

已轉換的應用程式具有各種選項，可註冊處理特定檔案類型以及整合至檔案總管。 這可讓使用者在一般工作流程當中輕鬆存取您的應用程式。

若要立即開始，請先將下列項目新增至應用程式的資訊清單︰

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="myapp">
        ... additional elements here ...
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

- *Extension Category* 的值應一律為 "windows.fileTypeAssociation"。
- *FileTypeAssociation Name* 是唯一的識別碼。 此識別碼係供內部用於產生與檔案類型關聯相關連的雜湊 ProgID。 您可使用此識別碼來管理您應用程式未來版本中的變更。 例如，若您想要變更副檔名的圖示，則可使用不同名稱將其移為新的 FileTypeAssociation。  

接著再根據需要的特定功能，將額外子元素新增至此項目。 可用的選項說明如下。

### <a name="supported-file-types"></a>支援的檔案類型

您的應用程式可指定其支援開啟特定類型的檔案。 若使用者以滑鼠右鍵按一下檔案，並選取 [開啟檔案]，您的應用程式將會出現在建議清單。

範例：

```XML
<uap:SupportedFileTypes>
    <uap:FileType>.txt</uap:FileType>
    <uap:FileType>.avi</uap:FileType>
</uap:SupportedFileTypes>
```

- *FileType* 是您應用程式支援的副檔名。

### <a name="file-context-menu-verbs"></a>檔案操作功能表動詞

使用者通常只要按兩下即可開啟檔案。 不過，當使用者以滑鼠右鍵按一下檔案時，操作功能表會顯示各種不同的選項 (稱為「動詞」)，其提供關於這些選項與檔案互動方式的額外詳細資訊，例如 [開啟]、[編輯]、[預覽] 或 [列印]。

指定讓所支援的檔案類型會自動新增 [開啟] 動詞。 不過，應用程式亦可將其他的自訂動詞新增至 [檔案總管] 操作功能表。 這可讓應用程式根據使用者開啟檔案時的選擇，啟動特定的方式。

範例：

```XML
<uap2:SupportedVerbs>
    <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
    <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
</uap2:SupportedVerbs>
```

- *Verb Id* 是動詞的唯一識別碼。 若您的 app 為 UWP app，則此識別碼會傳送至 app 做為啟用事件引數的一部分，以便能夠適當處理使用者的選擇。 若您的應用程式為完全信任的已轉換應用程式，則其會改為接收參數 (請參閱下一個項目符號)。
- *Verb Parameters* 是與動詞關聯的引數參數與值清單。 若您的應用程式為完全信任的已轉換應用程式，則在啟用應用程式時這些參數會傳送至應用程式做為事件引數，因此您可針對不同的啟用動詞自訂行為。 若變數可以包含檔案路徑，您應使用引號括住值，使其在傳送包含空格的路徑時不會中斷。 請注意，若您的 app 為 UWP app，則無法傳送參數 - app 會改為接收識別碼 (請參閱上一個項目符號)。
- 
            *Verb Extended* 指定是否僅在使用者以滑鼠右鍵按一下檔案顯示操作功能表前按住 **Shift** 鍵，才會顯示動詞。 此屬性為選擇性，若未列出則其預設為 *False* (一律顯示動詞)。 您會針對每個動詞個別指定此行為 (不包括「開啟」，其一律設定為 *False*)。
- *Verb* 是要顯示在 [檔案總管] 操作功能表中的名稱。 此字串可使用 ```ms-resource``` 進行當地語系化。

### <a name="shell-context-menu-verbs"></a>殼層操作功能表動詞

目前不支援將項目新增到殼層的資料夾操作功能表。

### <a name="multiple-selection-model"></a>多重選取模型

多重選取可讓您指定應用程式如何處理同時開啟多個檔案的使用者 (例如在 [檔案總管] 中選取 10 個檔案，然後點選 [開啟])。

已轉換的傳統型應用程式具有與一般傳統型應用程式相同的三個選項。
- *Player*︰啟用一次應用程式，並傳送所有選取的檔案做為引數參數。
- *Single*︰針對第一個選取的檔案啟用一次應用程式。 系統會忽略其他的檔案。
- *Document*︰針對每個所選檔案，個別啟用應用程式新的執行個體。

您可針對不同的檔案類型和動作，設定不同的喜好設定。 例如，您可能想要在 *Documents* 模式中開啟*「文件」*，以及在 *Player* 模式中開啟*「影像」*。

若要設定您的應用程式行為，請在資訊清單中，與檔案類型及檔案啟動相關的元素新增 *MultiSelectModel* 屬性。

設定支援檔案類型的模型︰

```XML
<uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
    <uap:SupportedFileTypes>
        <uap:FileType>.txt</uap:FileType>
</uap:SupportedFileTypes>
```

設定動詞的模型：

```XML
<uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap3:Verb>
<uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap3:Verb>
```

若您的應用程式不指定多重選取選擇，且使用者開啟的檔案數目在 15 個以下，則預設值為 *Player*。 或者，若您的應用程式為已轉換應用程式，則預設值為 *Document*。 UWP app 一律會啟動為 *Player*。

### <a name="complete-example"></a>完整範例

下列是整合上述眾多檔案與 [檔案總管] 相關元素的完整案例：

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
        <uap:SupportedFileTypes>
            <uap:FileType>.txt</uap:FileType>
            <uap:FileType>.foo</uap:FileType>
    </uap:SupportedFileTypes>
    <uap2:SupportedVerbs>
            <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
            <uap3:Verb Id="Print" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
    </uap2:SupportedVerbs>
    <uap:Logo>Assets\MyExtensionLogo.png</uap:Logo>
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

## <a name="see-also"></a>另請參閱

- [應用程式套件資訊清單](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)
