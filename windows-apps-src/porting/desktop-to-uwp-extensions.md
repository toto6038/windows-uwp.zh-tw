---
Description: You can use extensions to integrate your packaged desktop app with Windows 10 in predefined ways.
Search.Product: eADQiWindows 10XVcnh
title: 整合您的應用程式與 Windows 10 (傳統型橋接器)
ms.date: 04/18/2018
ms.topic: article
keywords: Windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.localizationpriority: medium
ms.openlocfilehash: 19ae09190b916fdaae68a67a2b9c11caa20d30e2
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7850667"
---
# <a name="integrate-your-packaged-desktop-application-with-windows-10"></a>您已封裝的傳統型應用程式整合至 Windows 10

使用擴充功能來整合您已封裝的傳統型應用程式與 Windows 10 中預先定義的方式。

例如，使用延伸模組建立防火牆例外、 檔案類型的預設應用程式，讓您的應用程式或開始畫面磚指向您的應用程式的已封裝版本。 若要使用延伸模組，您只需要將一些 XML 加入您應用程式的封裝資訊清單檔案。 不需要程式碼。

本主題會描述這些延伸模組，以及您可以使用他們來執行的工作。

## <a name="transition-users-to-your-app"></a>將使用者轉換至您的應用程式

協助使用者轉換至您已封裝版本的應用程式。

* [將現有的開始畫面磚和工作列按鈕指向您已封裝的應用程式](#point)
* [請開啟檔案，而非您傳統型應用程式的已封裝的應用程式](#make)
* [將您已封裝的應用程式與一組檔案類型產生關聯](#associate)
* [將選項新增至具有特定檔案類型的檔案操作功能表](#add)
* [使用 URL 直接開啟特定類型的檔案](#open)

<a id="point" />

### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>將現有的開始畫面磚和工作列按鈕指向您已封裝的應用程式

您的使用者可能會將您的傳統型應用程式釘選至工作列或 \[開始\] 功能表。 您可以將這些捷徑指向您新的已封裝應用程式。

#### <a name="xml-namespace"></a>XML 命名空間

http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.desktopAppMigration">
    <DesktopAppMigration>
        <DesktopApp AumId="[your_app_aumid]" />
        <DesktopApp ShortcutPath="[path]" />
    </DesktopAppMigration>
</Extension>

```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-rescap3-desktopappmigration)找到完整的結構描述參考。

|名稱 | 描述 |
|-------|-------------|
|類別 |總是 ``windows.desktopAppMigration``。
|AumID |您已封裝應用程式之應用程式使用者模型識別碼。 |
|ShortcutPath |啟動您傳統型版本應用程式之 .lnk 檔案的路徑。 |

#### <a name="example"></a>範例

```XML
<Package
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="rescap3">
  <Applications>
    <Application>
      <Extensions>
        <rescap3:Extension Category="windows.desktopAppMigration">
          <rescap3:DesktopAppMigration>
            <rescap3:DesktopApp AumId="[your_app_aumid]" />
            <rescap3:DesktopApp ShortcutPath="%USERPROFILE%\Desktop\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%APPDATA%\Microsoft\Windows\Start Menu\Programs\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\[my_app_folder]\[my_app].lnk"/>
         </rescap3:DesktopAppMigration>
        </rescap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>相關範例

[具有轉換/移轉/解除安裝的 WPF 圖片檢視器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make" />

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>請開啟檔案，而非您傳統型應用程式的已封裝的應用程式

您可以確保使用者開啟特定類型的檔案，而非開啟您的應用程式的傳統型版本的預設開啟新的已封裝應用程式。

若要完成這項工作，您可以指定您想要繼承檔案關聯的每個應用程式的[程式設計識別碼 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)。

#### <a name="xml-namespaces"></a>XML 命名空間

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.fileTypeAssociation">
<FileTypeAssociation Name="[AppID]">
         <MigrationProgIds>
            <MigrationProgId>"[ProgID]"</MigrationProgId>
        </MigrationProgIds>
    </FileTypeAssociation>
</Extension>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.fileTypeAssociation``。
|名稱 |您應用程式的唯一識別碼。 此識別碼係供內部用於產生與檔案類型關聯相關聯的雜湊[程式設計識別碼 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)。 您可使用此識別碼來管理您應用程式未來版本中的變更。 |
|MigrationProgId |[程式設計識別碼 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)描述應用程式、 元件，以及您要繼承檔案關聯的傳統型應用程式的版本。|

#### <a name="example"></a>範例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap3, rescap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <rescap3:MigrationProgIds>
              <rescap3:MigrationProgId>Foo.Bar.1</rescap3:MigrationProgId>
              <rescap3:MigrationProgId>Foo.Bar.2</rescap3:MigrationProgId>
            </rescap3:MigrationProgIds>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>相關範例

[具有轉換/移轉/解除安裝的 WPF 圖片檢視器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate" />

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>將您已封裝的應用程式與一組檔案類型產生關聯

您可以使用檔案類型副檔名產生關聯您已封裝的應用程式。 如果使用者以滑鼠右鍵按一下檔案，然後選取 [**開啟**] 選項，您的應用程式會出現在建議清單。

#### <a name="xml-namespace"></a>XML 命名空間

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[file extension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.fileTypeAssociation``。
|名稱 |您應用程式的唯一識別碼。 此識別碼係供內部用於產生與檔案類型關聯相關聯的雜湊[程式設計識別碼 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)。 您可使用此識別碼來管理您應用程式未來版本中的變更。   |
|FileType |您應用程式支援的檔案副檔名。 |

#### <a name="example"></a>範例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap:SupportedFileTypes>
            <uap:FileType>.txt</uap:FileType>
            <uap:FileType>.avi</uap:FileType>
            </uap:SupportedFileTypes>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>相關範例

[具有轉換/移轉/解除安裝的 WPF 圖片檢視器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="add" />

### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>將選項新增至具有特定檔案類型的檔案操作功能表

在大部分的情況下，使用者會藉由在檔案上按兩下以進行開啟。 若使用者以滑鼠右鍵按一下檔案，就會出現各種不同的選項。

您可以將選項加入該功能表。 這些選項可為使用者提供更多與您檔案互動的方式，例如：列印、編輯，或預覽檔案。

#### <a name="xml-namespaces"></a>XML 命名空間

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedVerbs>
           <Verb Id="[ID]" Extended="[Extended]" Parameters="[parameters]">"[verb label]"</Verb>
        </SupportedVerbs>
    </FileTypeAssociation>
</Extension>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|類別 | 總是 ``windows.fileTypeAssociation``。
|名稱 |您應用程式的唯一識別碼。 |
|Verb |在檔案總管操作功能表中出現的名稱。 此字串可使用 ```ms-resource``` 進行當地語系化。|
|Id |指令動詞的唯一識別碼。 如果您的應用程式是 UWP app，這被傳遞至您的應用程式做為其啟用事件引數的一部分讓它能夠適當處理使用者的選擇。 如果您的應用程式為完全信任的已封裝應用程式，它參數會改為接收 （請參閱下一個項目符號）。 |
|Parameters |與指令動詞關聯的引數參數與值清單。 如果您的應用程式為完全信任的已封裝應用程式，這些參數會傳遞至應用程式做為事件引數，應用程式啟動時。 您可以自訂您的應用程式根據不同的啟用動詞的行為。 若變數包含了檔案路徑，請以引號包圍參數。 這將可避免路徑中包含空格時可能發生的任何問題。 如果您的應用程式是 UWP app，您無法傳送參數。 應用程式會改為接收識別碼 (請參閱上一個項目符號)。|
|Extended |指定是否僅在使用者以滑鼠右鍵按一下檔案顯示操作功能表前按住 **Shift** 鍵，才會顯示指令動詞。 此屬性為選擇性，若未列出則其預設值為 **False** (一律顯示動詞)。 您會針對每個動詞個別指定此行為 (不包括「開啟」，其一律設定為 **False**)。|

#### <a name="example"></a>範例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
              <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
            </uap2:SupportedVerbs>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>相關範例

[具有轉換/移轉/解除安裝的 WPF 圖片檢視器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open" />

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>使用 URL 直接開啟特定類型的檔案

您可以確保使用者開啟特定類型的檔案，而非開啟您的應用程式的傳統型版本的預設開啟新的已封裝應用程式。

#### <a name="xml-namespaces"></a>XML 命名空間

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" UseUrl="true" Parameters="%1">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.fileTypeAssociation``。
|名稱 |您應用程式的唯一識別碼。 |
|UseUrl |指出是否要直接從 URL 目標開啟檔案。 如果您沒有設定此值，會嘗試透過您的應用程式使用的 URL 原因先將檔案下載到系統在本機開啟的檔案。 |
|Parameters |選用參數。 |
|FileType |相關的檔案副檔名。 |

#### <a name="example"></a>範例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
      <Application>
        <Extensions>
          <uap:Extension Category="windows.fileTypeAssociation">
            <uap3:FileTypeAssociation Name="documenttypes" UseUrl="true" Parameters="%1">
              <uap:SupportedFileTypes>
                <uap:FileType>.txt</uap:FileType>
                <uap:FileType>.doc</uap:FileType>
              </uap:SupportedFileTypes>
            </uap3:FileTypeAssociation>
          </uap:Extension>
        </Extensions>
      </Application>
    </Applications>
</Package>
```

## <a name="perform-setup-tasks"></a>執行安裝工作

* [為您的應用程式建立防火牆例外](#rules)
* [將您 DLL 檔案放置在套件的任何資料夾](#load-paths)

<a id="rules" />

### <a name="create-firewall-exception-for-your-app"></a>為您的應用程式建立防火牆例外

如果您的應用程式需要透過連接埠進行通訊，您可以新增應用程式清單的防火牆例外狀況。

#### <a name="xml-namespace"></a>XML 命名空間

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.firewallRules">
  <FirewallRules Executable="[executable file name]">
    <Rule
      Direction="[Direction]"
      IPProtocol="[Protocol]"
      LocalPortMin="[LocalPortMin]"
      LocalPortMax="LocalPortMax"
      RemotePortMin="RemotePortMin"
      RemotePortMax="RemotePortMax"
      Profile="[Profile]"/>
  </FirewallRules>
</Extension>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-firewallrules)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|Category |總是 ``windows.firewallRules``|
|Executable |您想要新增到防火牆例外清單的可執行檔名稱 |
|Direction |指定規則為輸入或輸出規則 |
|IPProtocol |通訊協定 |
|LocalPortMin |本機連接埠數字範圍中較低的數字。 |
|LocalPortMax |本機連接埠數字範圍中最高的數字。 |
|RemotePortMax |遠端連接埠數字範圍中較低的數字。 |
|RemotePortMax |遠端連接埠數字範圍中最高的數字。 |
|Profile |網路類型 |

#### <a name="example"></a>範例

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Extensions>
    <desktop2:Extension Category="windows.firewallRules">
      <desktop2:FirewallRules Executable="Contoso.exe">
          <desktop2:Rule Direction="in" IPProtocol="TCP" Profile="all"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="domain"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="public"/>
          <desktop2:Rule Direction="out" IPProtocol="UDP" LocalPortMin="1339" LocalPortMax="1340" RemotePortMin="15"
                         RemotePortMax="19" Profile="domainAndPrivate"/>
          <desktop2:Rule Direction="out" IPProtocol="GRE" Profile="private"/>
      </desktop2:FirewallRules>
  </desktop2:Extension>
</Extensions>
</Package>
```

<a id="load-paths" />

### <a name="place-your-dll-files-into-any-folder-of-the-package"></a>將您 DLL 檔案放置在套件的任何資料夾

使用延伸模組來識別這些資料夾。 如此一來，系統可以找出並載入您放置的檔案。 將此延伸模組視為 _%PATH%_ 環境變數的替代。

如果您未使用此延伸模組，系統會依序搜尋程序的套件相依性圖形、套件根資料夾，然後搜尋系統目錄 (_%SystemRoot%\system32_)。 若要深入了解，請參閱 [Windows 應用程式的搜尋順序](https://msdn.microsoft.com/library/windows/desktop/ms682586.aspx#_search_order_for_windows_store_apps)。

每個套件只可以包含下列其中一個延伸模組。 這表示您可以將其中一個新增到您的主要套件，並在每個[選用套件及相關集合](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)中新增一個。

#### <a name="xml-namespace"></a>XML 命名空間

http://schemas.microsoft.com/appx/manifest/uap/windows10/6

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

在應用程式資訊清單的套件層級宣告此延伸模組。

```XML
<Extension Category="windows.loaderSearchPathOverride">
  <LoaderSearchPathOverride>
    <LoaderSearchPathEntry FolderPath="[path]"/>
  </LoaderSearchPathOverride>
</Extension>

```

|名稱 | 描述 |
|-------|-------------|
|類別 |總是 ``windows.loaderSearchPathOverride``。
|FolderPath | 包含 dll 檔案的資料夾路徑。 指定相對於套件根資料夾的路徑。 您可以在一個延伸模組中指定最多 5 個路徑。 如果您想要系統在套件根資料夾中搜尋檔案，請使用空字串代表其中一個路徑。 不包含重複路徑，並請確定路徑不包含前置和結尾斜線或反斜線。 <br><br> 系統不會搜尋子資料夾，因此請務必明確列出包含要系統載入的 DLL 檔案的每個資料夾。|

#### <a name="example"></a>範例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/6"
  IgnorableNamespaces="uap6">
  ...
    <Extensions>
      <uap6:Extension Category="windows.loaderSearchPathOverride">
        <uap6:LoaderSearchPathOverride>
          <uap6:LoaderSearchPathEntry FolderPath=""/>
          <uap6:LoaderSearchPathEntry FolderPath="folder1/subfolder1"/>
          <uap6:LoaderSearchPathEntry FolderPath="folder2/subfolder2"/>
        </uap6:LoaderSearchPathOverride>
      </uap6:Extension>
    </Extensions>
...
</Package>
```

## <a name="integrate-with-file-explorer"></a>與檔案總管整合

協助使用者組織您的檔案，並以熟悉的方式與他們互動。

* [定義使用者選取和同時開啟多個檔案時，您的應用程式的運作方式](#define)
* [在檔案總管中以縮圖影像顯示檔案內容](#show)
* [在檔案總管的預覽窗格中顯示檔案內容](#preview)
* [讓使用者在檔案總管中使用種類欄位群組檔案](#enable)
* [讓檔案屬性可供搜尋、索引、屬性對話方塊，以及詳細資料窗格使用](#make-file-properties)
* [讓雲端服務中的檔案出現在檔案總管](#cloud-files)

<a id="define" />

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>定義使用者選取和同時開啟多個檔案時，您的應用程式的運作方式

指定當使用者同時開啟多個檔案時，您的應用程式的行為。

#### <a name="xml-namespaces"></a>XML 命名空間

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" MultiSelectModel="[SelectionModel]">
        <SupportedVerbs>
            <Verb Id="Edit" MultiSelectModel="[SelectionModel]">Edit</Verb>
        </SupportedVerbs>
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
</Extension>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.fileTypeAssociation``。
|名稱 |您應用程式的唯一識別碼。 |
|MultiSelectModel |請參閱下方。 |
|FileType |相關的檔案副檔名。 |

**MultSelectModel**

已封裝的傳統型應用程式具有與一般傳統型應用程式相同的三個選項。

* ``Player``： 您的應用程式會啟動一次。 所有選取的檔案都會傳遞至您的應用程式做為引數參數。
* ``Single``： 您的應用程式會啟動一次，針對第一個選取的檔案。 系統會忽略其他的檔案。
* ``Document``： 針對每個所選檔案啟用新，個別的執行個體應用程式。

 您可針對不同的檔案類型和動作，設定不同的喜好設定。 例如：您可能會想要在 *Documents* 模式中開啟 *「文件」*，以及在 *Player* 模式中開啟 *「影像」*。

#### <a name="example"></a>範例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap3:Verb>
              <uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap3:Verb>
            </uap2:SupportedVerbs>
            <uap:SupportedFileTypes>
              <uap:FileType>.txt</uap:FileType>
            </uap:SupportedFileTypes>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

若使用者開啟 15 個或較少數量的檔案，**MultiSelectModel** 屬性的預設選項將會設定為 *Player*。 否則，預設值將會設定為 *Document*。 UWP app 一律會以 *Player* 模式啟動。

<a id="show" />

### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>在檔案總管中以縮圖影像顯示檔案內容

讓使用者在檔案圖示以中圖示、大圖示，或超大圖示顯示時，可透過縮圖影像檢視檔案內容。

#### <a name="xml-namespace"></a>XML 命名空間

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <ThumbnailHandler
            Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.fileTypeAssociation``。
|名稱 |您應用程式的唯一識別碼。 |
|FileType |相關的檔案副檔名。 |
|Clsid   |您應用程式的類別識別碼。 |

#### <a name="example"></a>範例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap2:SupportedFileTypes>
            <desktop2:ThumbnailHandler
              Clsid  ="20000000-0000-0000-0000-000000000001"  />
            </uap3:FileTypeAssociation>
         </uap::Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="preview" />

### <a name="show-file-contents-in-the-preview-pane-of-file-explorer"></a>在檔案總管的預覽窗格中顯示檔案內容

讓使用者可在檔案總管中的預覽窗格預覽檔案內容。

#### <a name="xml-namespace"></a>XML 命名空間

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <DesktopPreviewHandler Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.fileTypeAssociation``。
|名稱 |您應用程式的唯一識別碼。 |
|FileType |相關的檔案副檔名。 |
|Clsid   |您應用程式的類別識別碼。 |

#### <a name="example"></a>範例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
                </uap2SupportedFileTypes>
              <desktop2:DesktopPreviewHandler Clsid ="20000000-0000-0000-0000-000000000001" />
           </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="enable" />

### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>讓使用者在檔案總管中使用種類欄位群組檔案

您可以透過**種類**欄位使您的檔案類型與一或多個預先定義的值產生關聯。

在檔案總管中，使用者可以使用該欄位群組這些檔案。 系統元件也會根據不同用途使用此欄位，例如：編製索引。

如需有關**種類**欄位以及您可以使用於此欄位中的詳細資訊，請參閱[使用種類名稱](https://msdn.microsoft.com/library/windows/desktop/cc144136.aspx)。

#### <a name="xml-namespaces"></a>XML 命名空間

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <KindMap>
            <Kind value="[KindValue]">
        </KindMap>
    </FileTypeAssociation>
</Extension>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.fileTypeAssociation``。
|名稱 |您應用程式的唯一識別碼。 |
|FileType |相關的檔案副檔名。 |
|value |一個有效的 [Kind 值](https://msdn.microsoft.com/en-us/library/windows/desktop/cc144136.aspx#kind_hierarchy) |

#### <a name="example"></a>範例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap, rescap">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
           <uap:FileTypeAssociation Name="Contoso">
             <uap:SupportedFileTypes>
               <uap:FileType>.m4a</uap:FileType>
               <uap:FileType>.mta</uap:FileType>
             </uap:SupportedFileTypes>
             <rescap:KindMap>
               <rescap:Kind value="Item">
               <rescap:Kind value="Communications">
               <rescap:Kind value="Task">
             </rescap:KindMap>
          </uap:FileTypeAssociation>
      </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="make-file-properties" />

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>讓檔案屬性可供搜尋、索引、屬性對話方塊，以及詳細資料窗格使用

#### <a name="xml-namespace"></a>XML 命名空間

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>.bar</FileType>
        </SupportedFileTypes>
        <DesktopPropertyHandler Clsid ="[Clsid]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.fileTypeAssociation``。
|名稱 |您應用程式的唯一識別碼。 |
|FileType |相關的檔案副檔名。 |
|Clsid  |您應用程式的類別識別碼。 |

#### <a name="example"></a>範例

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap:SupportedFileTypes>
            <desktop2:DesktopPropertyHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="cloud-files" />

### <a name="make-files-from-your-cloud-service-appear-in-file-explorer"></a>讓雲端服務中的檔案出現在檔案總管

登錄您的應用程式中實作的處理常式。 您也可以新增操作功能表選項，使用者在 [檔案總管] 中的雲端式檔案上按一下滑鼠右鍵時出現這些選項。

#### <a name="xml-namespace"></a>XML 命名空間

* http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.cloudfiles" >
    <CloudFiles IconResource="[Icon]">
        <CustomStateHandler Clsid ="[Clsid]"/>
        <ThumbnailProviderHandler Clsid ="[Clsid]"/>
        <ExtendedPropertyhandler Clsid ="[Clsid]"/>
        <CloudFilesContextMenus>
            <Verb Id ="Command3" Clsid= "[GUID]">[Verb Label]</Verb>
        </CloudFilesContextMenus>
    </CloudFiles>
</Extension>

```

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.cloudfiles``。
|iconResource |圖示，表示您的雲端檔案提供者服務。 在 [檔案總管] 瀏覽窗格中，會顯示此圖示。  使用者選擇此圖示，以顯示您的雲端服務中的檔案。 |
|CustomStateHandler Clsid |實作 CustomStateHandler 的應用程式的類別 ID。 系統會使用這個類別 ID 要求雲端檔案的自訂狀態和欄。 |
|ThumbnailProviderHandler Clsid |實作 ThumbnailProviderHandler 的應用程式的類別 ID。 系統會使用這個類別 ID 要求雲端檔案的縮圖影像。 |
|ExtendedPropertyHandler Clsid |實作 ExtendedPropertyHandler 的應用程式的類別 ID。  系統會使用這個類別 ID 要求雲端檔案的擴充屬性。 |
|Verb |為您的雲端服務所提供的檔案，顯示在 [檔案總管] 操作功能表中的名稱。 |
|Id |指令動詞的唯一識別碼。 |

#### <a name="example"></a>範例

```XML
<Package
    xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
    IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <Extension Category="windows.cloudfiles" >
            <CloudFiles IconResource="images\Wide310x150Logo.png">
                <CustomStateHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ThumbnailProviderHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ExtendedPropertyhandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <desktop:CloudFilesContextMenus>
                    <desktop:Verb Id ="keep" Clsid=
                       "20000000-0000-0000-0000-000000000001">
                       Always keep on this device</desktop:Verb>
                </desktop:CloudFilesContextMenus>
            </CloudFiles>
          </Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="start" />

## <a name="start-your-application-in-different-ways"></a>以不同方式啟動您的應用程式

* [使用通訊協定啟動您的應用程式](#protocol)
* [透過使用別名啟動您的應用程式](#alias)
* [在使用者登入 Windows 時執行可執行檔](#executable)
* [讓使用者將裝置連接至電腦時，啟動您的應用程式](#autoplay)
* [從 Microsoft Store 接收更新後自動重新開機](#updates)

<a id="protocol" />

### <a name="start-your-application-by-using-a-protocol"></a>使用通訊協定啟動您的應用程式

通訊協定關聯支援已封裝應用程式與其他程式或系統元件間的互相操作。 使用通訊協定啟動已封裝應用程式時，您可以指定特定參數傳送至其啟用事件引數，讓其據以運作。 參數僅支援已封裝、完全信任的應用程式。 UWP app 無法使用參數。

#### <a name="xml-namespace"></a>XML 命名空間

http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension
    Category="windows.protocol">
  <Protocol
      Name="[Protocol name]"
      Parameters="[Parameters]" />
</Extension>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.protocol``。
|Name |通訊協定的名稱。 |
|Parameters |之參數與值，以啟動應用程式時，您的應用程式做為事件引數傳遞的清單。 若變數包含了檔案路徑，請以引號包圍參數。 這將可避免路徑中包含空格時可能發生的任何問題。 |

### <a name="example"></a>範例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap3:Extension
          Category="windows.protocol">
          <uap3:Protocol
            Name="myapp-cmd"
            Parameters="/p &quot;%1&quot;" />
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="alias" />

### <a name="start-your-application-by-using-an-alias"></a>透過使用別名啟動您的應用程式

使用者和其他處理程序可以使用別名啟動您的應用程式，而不必指定您的應用程式的完整路徑。 您可以指定別名名稱。

#### <a name="xml-namespaces"></a>XML 命名空間

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension
    Category="windows.appExecutionAlias"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
    <AppExecutionAlias>
        <desktop:ExecutionAlias Alias="[AliasName]" />
    </AppExecutionAlias>
</Extension>
```

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.appExecutionAlias``。
|Executable |叫用別名時欲啟動之可執行檔的相對路徑。 |
|Alias |適用於您應用程式的簡短名稱。 其必須一律以副檔名「.exe」結尾。 針對套件中的每個應用程式，您僅可指定單一應用程式執行別名。 若有多個應用程式註冊相同的別名，系統將會叫用最後一個註冊的別名，因此請務必選擇絕對不會遭其他應用程式覆寫的唯一別名。
|

#### <a name="example"></a>範例

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap3, desktop">
  ...
  <uap3:Extension
        Category="windows.appExecutionAlias"
        Executable="exes\launcher.exe"
        EntryPoint="Windows.FullTrustApplication">
      <uap3:AppExecutionAlias>
        <desktop:ExecutionAlias Alias="Contoso.exe" />
      </uap3:AppExecutionAlias>
  </uap3:Extension>
...
</Package>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)找到完整的結構描述參考。

<a id="executable" />

### <a name="start-an-executable-file-when-users-log-into-windows"></a>在使用者登入 Windows 時執行可執行檔

啟動工作能讓您的應用程式每次使用者登入時，自動執行可執行檔。

> [!NOTE]
> 使用者必須啟動您的應用程式至少一次登錄此啟動工作。

您的應用程式可以宣告多個啟動工作。 每個工作都是獨立啟動的。 所有啟動工作皆會顯示在「工作管理員」中的 **\[啟動\]** 索引標籤下方，且具有您在應用程式的資訊清單中指定的名稱，以及應用程式的圖示。 工作管理員會自動分析您工作的啟動影響。

使用者可以使用工作管理員手動停用您應用程式的啟動工作。 若使用者停用工作，您將無法以程式設計的方式重新啟用它。

#### <a name="xml-namespace"></a>XML 命名空間

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension
    Category="windows.startupTask"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
  <StartupTask
      TaskId="[TaskID]"
      Enabled="true"
      DisplayName="[DisplayName]" />
</Extension>
```

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.startupTask``。|
|Executable |欲啟動之可執行檔檔案的相對路徑。 |
|TaskId |您工作的唯一識別碼。 使用此識別碼，您的應用程式可以呼叫 Api 以程式設計方式啟用或停用啟動工作[Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask)類別中。 |
|Enabled |指出工作第一次啟動時會是啟用或停用。 下次使用者登入時，將會執行已啟用的工作 (除非使用者將其停用)。 |
|DisplayName |出現在工作管理員中工作的名稱。 您可以透過使用 ```ms-resource``` 來當地語系化此字串。 |

#### <a name="example"></a>範例

```XML
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <desktop:Extension
          Category="windows.startupTask"
          Executable="bin\MyStartupTask.exe"
          EntryPoint="Windows.FullTrustApplication">
        <desktop:StartupTask
          TaskId="MyStartupTask"
          Enabled="true"
          DisplayName="My App Service" />
        </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
 </Package>
```

<a id="autoplay" />

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>讓使用者將裝置連接至電腦時，啟動您的應用程式

當使用者將裝置連接至電腦時，「 自動播放 」 可以呈現您的應用程式成為一個選項。

#### <a name="xml-namespace"></a>XML 命名空間

http://schemas.microsoft.com/appx/manifest/desktop/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.autoPlayHandler">
  <AutoPlayHandler>
    <InvokeAction ActionDisplayName="[action string]" ProviderDisplayName="[name of your app/service]">
      <Content ContentEvent="[Content event]" Verb="[any string]" DropTargetHandler="[Clsid]" />
      <Content ContentEvent="[Content event]" Verb="[any string]" Parameters="[Initialization parameter]"/>
      <Device DeviceEvent="[Device event]" HWEventHandler="[Clsid]" InitCmdLine="[Initialization parameter]"/>
    </InvokeAction>
  </AutoPlayHandler>
```

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.autoPlayHandler``。
|ActionDisplayName |字串，表示使用者可以在連接到電腦的裝置上執行的動作 (例如：「匯入檔案」或「播放影片」)。 |
|ProviderDisplayName | 字串，表示您的應用程式或服務 (例如: 「 Contoso 影片播放程式 」)。 |
|ContentEvent |內容事件的名稱，該事件導致向使用者提示您的``ActionDisplayName``與``ProviderDisplayName``。 將磁碟區裝置 (例如，相機記憶卡、隨身碟或 DVD) 插入電腦時，就會引發內容事件。 您可以在[這裡](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference)找到那些事件的完整清單。  |
|Verb |Verb 設定會識別它的值會傳遞至您的應用程式針對選取的選項。 您可以為自動播放事件指定多個啟動動作，並使用 \[動詞\] 設定判斷使用者為您 app 選取的選項。 您可以檢查傳遞至 app 啟動事件引數的 verb 屬性，以判斷使用者選取的選項。 您可以在 \[動詞\] 設定使用保留字 open 以外的任何值。 |
|DropTargetHandler |應用程式實作[IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)介面的類別 ID。 抽取式媒體中的檔案將會傳遞至[IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)實作的[Drop](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop?view=visualstudiosdk-2017#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__)方法。  |
|Parameters |您不需要為所有內容事件實作[IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)介面。 對於任何內容事件，您可以提供命令列參數，而不實作[IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)介面。 對這些事件，「 自動播放 」 會使用這些命令列參數啟動您的應用程式。 您可以在應用程式的初始化程式碼中剖析這些參數，以判斷自動播放是否啟動應用程式，然後提供您的自訂實作。 |
|DeviceEvent |裝置事件的名稱，該事件導致向使用者提示您的``ActionDisplayName``與``ProviderDisplayName``。 將裝置連接到電腦時，就會引發裝置事件。 裝置事件以字串``WPD``開頭，[這裡](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference)列出這些裝置事件。 |
|HWEventHandler |應用程式實作[IHWEventHandler](https://msdn.microsoft.com/library/windows/desktop/bb775492.aspx)介面的類別 ID。 |
|InitCmdLine |您想要傳遞到[IHWEventHandler](https://msdn.microsoft.com/library/windows/desktop/bb775492.aspx)介面的[Initialize](https://msdn.microsoft.com/en-us/library/windows/desktop/bb775495.aspx)方法的字串參數。 |

### <a name="example"></a>範例

```XML
<Package
  xmlns:desktop3="http://schemas.microsoft.com/appx/manifest/desktop/windows10/3"
  IgnorableNamespaces="desktop3">
  <Applications>
    <Application>
      <Extensions>
        <desktop3:Extension Category="windows.autoPlayHandler">
          <desktop3:AutoPlayHandler>
            <desktop3:InvokeAction ActionDisplayName="Import my files" ProviderDisplayName="ms-resource:AutoPlayDisplayName">
              <desktop3:Content ContentEvent="ShowPicturesOnArrival" Verb="show" DropTargetHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8"/>
              <desktop3:Content ContentEvent="PlayVideoFilesOnArrival" Verb="play" Parameters="%1" />
              <desktop3:Device DeviceEvent="WPD\ImageSource" HWEventHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8" InitCmdLine="/autoplay"/>
            </desktop3:InvokeAction>
          </desktop3:AutoPlayHandler>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="updates" />

### <a name="restart-automatically-after-receiving-an-update-from-the-microsoft-store"></a>從 Microsoft Store 接收更新後自動重新開機

如果您的應用程式開啟時使用者安裝更新它，就會關閉應用程式。

如果您想要完成更新之後重新啟動該應用程式，請在每個您想要重新啟動的處理序中呼叫[RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx)函式。

您的應用程式中每個使用中視窗收到[WM_QUERYENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376890.aspx)的訊息。 到目前為止，您的應用程式可以呼叫一次以更新命令列，如有必要的[RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx)函式。

當您的應用程式中每個使用中視窗收到[WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx)訊息時，您的應用程式應該儲存資料，並關閉。

>[!NOTE]
您使用中的 windows 也收到[WM_CLOSE](https://msdn.microsoft.com/library/windows/desktop/ms632617.aspx)訊息，以防應用程式不會處理[WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx)訊息。

到目前為止，您的應用程式關閉它本身的處理程序有 30 秒時間或平台會強制終止。

更新完成後，重新啟動您的應用程式。

## <a name="work-with-other-applications"></a>與其他應用程式合作

與其他應用程式整合，啟動其他處理程序，或共用資訊。

* [讓您的應用程式作為列印目標支援應用程式中顯示列印](#printing)
* [與其他 Windows 應用程式共用字型](#fonts)
* [從通用 Windows 平台 (UWP) 應用程式中啟動 Win32 處理程序](#win32-process)

<a id="printing" />

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>讓您的應用程式作為列印目標支援應用程式中顯示列印

當使用者想要從另一個應用程式，例如記事本列印資料時，您可以讓您的應用程式作為列印目標應用程式的可用列印目標清單中出現。

您必須修改您的應用程式，使其能接收以 XML 文件規格 (XPS) 格式列印資料。

#### <a name="xml-namespaces"></a>XML 命名空間

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.appPrinter">
    <AppPrinter
        DisplayName="[DisplayName]"
        Parameters="[Parameters]" />
</Extension>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-appprinter)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.appPrinter``。
|DisplayName |您想要出現在另外一個應用程式的列印目標清單中的顯示名稱。 |
|Parameters |應用程式正常處理要求時所需要的任何參數。 |

#### <a name="example"></a>範例

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Applications>
  <Application>
    <Extensions>
      <desktop2:Extension Category="windows.appPrinter">
        <desktop2:AppPrinter
          DisplayName="Send to Contoso"
          Parameters="/insertdoc %1" />
      </desktop2:Extension>
    </Extensions>
  </Application>
</Applications>
</Package>
```

您可以在[這裡](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/PrintToPDF)尋找使用此延伸模組的範例

<a id="fonts" />

### <a name="share-fonts-with-other-windows-applications"></a>與其他 Windows 應用程式共用字型

與其他 Windows 應用程式共用您的自訂字型。

#### <a name="xml-namespaces"></a>XML 命名空間

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.sharedFonts">
    <SharedFonts>
      <Font File="[FontFile]" />
    </SharedFonts>
  </Extension>
```

您可以在[這裡](https://review.docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.sharedFonts``。
|File |包含您想要共用之字型的檔案。 |

#### <a name="example"></a>範例

```XML
<Package
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
  IgnorableNamespaces="uap4">
  <Applications>
    <Application>
      <Extensions>
        <uap4:Extension Category="windows.sharedFonts">
          <uap4:SharedFonts>
            <uap4:Font File="Fonts\JustRealize.ttf" />
            <uap4:Font File="Fonts\JustRealizeBold.ttf" />
          </uap4:SharedFonts>
        </uap4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="win32-process" />

### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>從通用 Windows 平台 (UWP) 應用程式中啟動 Win32 處理程序

啟動完全信任的 Win32 處理程序。

#### <a name="xml-namespaces"></a>XML 命名空間

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<Extension Category="windows.fullTrustProcess" Executable="[executable file]">
  <FullTrustProcess>
    <ParameterGroup GroupId="[GroupID]" Parameters="[Parameters]"/>
  </FullTrustProcess>
</Extension>
```

|名稱 |描述 |
|-------|-------------|
|類別 |總是 ``windows.fullTrustProcess``。
|GroupID |您要傳遞給可執行檔的一個可識別一組參數的字串。 |
|Parameters |您想要傳遞給可執行檔的參數。 |

#### <a name="example"></a>範例

```XML
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap=
"http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10">
  ...
  <Capabilities>
      <rescap:Capability Name="runFullTrust"/>
  </Capabilities>
  <Applications>
    <Application>
      <Extensions>
          <desktop:Extension Category="windows.fullTrustProcess" Executable="fulltrustprocess.exe">
              <desktop:FullTrustProcess>
                  <desktop:ParameterGroup GroupId="SyncGroup" Parameters="/Sync"/>
                  <desktop:ParameterGroup GroupId="OtherGroup" Parameters="/Other"/>
              </desktop:FullTrustProcess>
           </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

此延伸模組可能會很有用，如果您想要建立在所有裝置執行的通用 Windows 平台使用者介面，但您想要的 Win32 應用程式繼續執行，完全信任的元件。

只需要建立您的 Win32 應用程式的 Windows 應用程式套件。 然後，將此延伸模組新增至您 UWP app 的套件檔案。 此延伸模組表示您想要在 Windows 應用程式套件中啟動之可執行檔。  若您想要讓您的 UWP app 和 Win32 應用程式互相通訊，您可以設定一或多個[應用程式服務](../launch-resume/app-services.md)。 您可以在[這裡](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/)閱讀更多關於此案例的資訊。

## <a name="next-steps"></a>後續步驟

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
