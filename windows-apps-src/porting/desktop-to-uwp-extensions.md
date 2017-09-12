---
author: normesta
Description: "您可以透過預先定義的方式，使用延伸模組來將您的已封裝傳統型應用程式整合至 Windows 10。"
Search.Product: eADQiWindows 10XVcnh
title: "整合您的應用程式與 Windows 10 (傳統型橋接器)"
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.openlocfilehash: 0c3427a7b49b17fda9a3ba0680e59b134732e9fa
ms.sourcegitcommit: 38ef208ef457ce1857038c9cde3658c884d29b75
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/13/2017
---
# <a name="integrate-your-app-with-windows-10-desktop-bridge"></a>整合您的應用程式與 Windows 10 (傳統型橋接器)

透過預先定義的方式，使用延伸模組來將您的應用程式與 Windows 10 進行整合。

例如：使用延伸模組建立防火牆例外，讓您的應用程式成為某一種檔案類型的預設應用程式，或將開始畫面磚指向您應用程式的已封裝版本。 若要使用延伸模組，您只需要將一些 XML 加入您應用程式的封裝資訊清單檔案。 不需要程式碼。

本主題會描述這些延伸模組，以及您可以使用他們來執行的工作。

## <a name="transition-users-to-your-app"></a>將使用者轉換至您的應用程式

協助使用者轉換至您已封裝版本的應用程式。

* [將現有的開始畫面磚和工作列按鈕指向您已封裝的應用程式](#point)
* [使用您已封裝的應用程式開啟檔案，而非使用您的傳統型應用程式](#make)
* [使您已封裝的應用程式和一組檔案類型產生關聯。](#associate)
* [將選項新增至具有特定檔案類型的檔案操作功能表](#add)
* [使用 URL 直接開啟特定類型的檔案](#open)

<span id="point" />
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

<span id="make" />
### <a name="make-your-packaged-app-open-files-instead-of-your-desktop-app"></a>使用您已封裝的應用程式開啟檔案，而非使用您的傳統型應用程式

您可以確保使用者預設使用您新的已封裝應用程式開啟特定類型的檔案，而非使用您傳統型版本的應用程式開啟。

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
|Category |總是 ``windows.fileTypeAssociation``。
|Name |您應用程式的唯一識別碼。 此識別碼係供內部用於產生與檔案類型關聯相關聯的雜湊[程式設計識別碼 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)。 您可使用此識別碼來管理您應用程式未來版本中的變更。 |
|MigrationProgId |[程式設計識別碼 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) 描述了您想要繼承檔案關聯之傳統型應用程式的應用程式、元件，以及版本。|

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

<span id="associate" />
### <a name="associate-your-packaged-app-with-a-set-of-file-types"></a>使您已封裝的應用程式和一組檔案類型產生關聯。

您可以使您已封裝的應用程式和檔案類型副檔名產生關聯。 若使用者以滑鼠右鍵按一下檔案，並選取 **\[開啟檔案\]** 選項，您的應用程式將會出現在建議清單。

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
|Category |總是 ``windows.fileTypeAssociation``。
|Name |您應用程式的唯一識別碼。 此識別碼係供內部用於產生與檔案類型關聯相關聯的雜湊[程式設計識別碼 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)。 您可使用此識別碼來管理您應用程式未來版本中的變更。   |
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

<span id="add" />
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
|Category | 總是 ``windows.fileTypeAssociation``。
|Name |您應用程式的唯一識別碼。 |
|Verb |在檔案總管操作功能表中出現的名稱。 此字串可使用 ```ms-resource``` 進行當地語系化。|
|Id |指令動詞的唯一識別碼。 若您的 app 為 UWP app，則此識別碼會傳送至 app 做為啟用事件引數的一部分，以便能夠適當處理使用者的選擇。 若您的應用程式為完全信任的已封裝應用程式，則其會改為接收參數 (請參閱下一個項目符號)。 |
|Parameters |與指令動詞關聯的引數參數與值清單。 若您的應用程式為完全信任的已封裝應用程式，這些參數會傳遞至應用程式作為啟動應用程式時的事件引數。 您可以根據不同的指令動詞自訂您應用程式的行為。 若變數包含了檔案路徑，請以引號包圍參數。 這將可避免路徑中包含空格時可能發生的任何問題。 若您的應用程式是 UWP app，則您無法傳送參數。 應用程式會改為接收識別碼 (請參閱上一個項目符號)。|
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

<span id="open" />
### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>使用 URL 直接開啟特定類型的檔案

您可以確保使用者預設使用您新的已封裝應用程式開啟特定類型的檔案，而非使用您傳統型版本的應用程式開啟。

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
|Category |總是 ``windows.fileTypeAssociation``。
|Name |您應用程式的唯一識別碼。 |
|UseUrl |指出是否要直接從 URL 目標開啟檔案。 若您未設定這個值，每當您的應用程式嘗試使用 URL 開啟檔案時，都會使系統先將檔案下載至本機電腦。 |
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

<span id="rules" />
### <a name="create-firewall-exception-for-your-app"></a>為您的應用程式建立防火牆例外

若您的應用程式需要透過連接埠進行通訊，您可以將您的應用程式新增至防火牆例外清單。

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

## <a name="integrate-with-file-explorer"></a>與檔案總管整合

協助使用者組織您的檔案，並以熟悉的方式與他們互動。

* [定義使用者同時選取和開啟多個檔案時，您的應用程式將會如何反應](#define)
* [在檔案總管中以縮圖影像顯示檔案內容](#show)
* [在檔案總管的預覽窗格中顯示檔案內容](#preview)
* [讓使用者在檔案總管中使用種類欄位群組檔案](#enable)
* [讓檔案屬性可供搜尋、索引、屬性對話方塊，以及詳細資料窗格使用](#make-file-properties)

<span id="define" />
### <a name="define-how-your-app-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>定義使用者同時選取和開啟多個檔案時，您的應用程式將會如何反應

指定當使用者同時開啟多個檔案時，您的應用程式將會如何反應。

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
|Category |總是 ``windows.fileTypeAssociation``。
|Name |您應用程式的唯一識別碼。 |
|MultiSelectModel |請參閱下方。 |
|FileType |相關的檔案副檔名。 |

**MultSelectModel**

已封裝的傳統型應用程式具有與一般傳統型應用程式相同的三個選項。

 * ``Player``︰您的應用程式會啟動一次。 所有選取的檔案都會作為引數參數傳遞至您的應用程式。
 * ``Single``：您的應用程式會針對第一個選取的檔案啟動一次。 系統會忽略其他的檔案。
 * ``Document``：系統會針對每個所選檔案，個別啟用應用程式新的執行個體。

 您可針對不同的檔案類型和動作，設定不同的喜好設定。 例如：您可能會想要在 *Documents* 模式中開啟*「文件」*，以及在 *Player* 模式中開啟*「影像」*。

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

<span id="show" />
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
            Clsid  ="[Clsid  ]"
            Cutoff="[Cutoff]"
            Treatment="[Treatment]" />
    </FileTypeAssociation>
</Extension>
```

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|Category |總是 ``windows.fileTypeAssociation``。
|Name |您應用程式的唯一識別碼。 |
|FileType |相關的檔案副檔名。 |
|Clsid   |您應用程式的類別識別碼。 |
|Cutoff |在此定義尺寸以下不會使用縮圖影像。 請參閱[縮圖快取與調整大小](https://msdn.microsoft.com/library/windows/desktop/cc144118.aspx#cache) |
|Treatment |[縮圖裝飾](https://msdn.microsoft.com/library/windows/desktop/cc144118.aspx#adornments)會定義縮圖圖示的外觀。 |

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
              Clsid  ="20000000-0000-0000-0000-000000000001"
              Cutoff="20x20"
              Treatment="Video Sprockets" />
            </uap3:FileTypeAssociation>
         </uap::Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<span id="preview" />
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
|Category |總是 ``windows.fileTypeAssociation``。
|Name |您應用程式的唯一識別碼。 |
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

<span id="enable" />
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
|Category |總是 ``windows.fileTypeAssociation``。
|Name |您應用程式的唯一識別碼。 |
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
<span id="make-file-properties" />
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
        <DesktopPropertyHandler Clsid ="[Clsid ]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```
**Key 元素和屬性描述**

您可以在[這裡](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)找到完整的結構描述參考。

|名稱 |描述 |
|-------|-------------|
|Category |總是 ``windows.fileTypeAssociation``。
|Name |您應用程式的唯一識別碼。 |
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

<span id="start" />
## <a name="start-your-app-in-different-ways"></a>以不同的方式啟動您的應用程式

* [使用通訊協定啟動您的應用程式](#protocol)
* [使用別名啟動您的應用程式](#alias)
* [在使用者登入 Windows 時執行可執行檔](#executable)
* [從 Windows 市集接收更新後自動重新開機](#updates)

<span id="protocol" />
### <a name="start-your-app-by-using-a-protocol"></a>使用通訊協定啟動您的應用程式

通訊協定關聯支援已封裝應用程式與其他程式或系統元件間的互相操作。 若您使用通訊協定啟動已封裝的應用程式，則可指定特定參數傳送至該應用程式的啟用事件引數，讓其據以運作。 參數僅支援已封裝、完全信任的應用程式。 UWP app 無法使用參數。  

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
|Category |總是 ``windows.protocol``。
|Name |通訊協定的名稱。 |
|Parameters |應用程式啟動時，作為事件引數傳遞至您應用程式的參數與數值清單。 若變數包含了檔案路徑，請以引號包圍參數。 這將可避免路徑中包含空格時可能發生的任何問題。 |

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
<span id="alias" />
### <a name="start-your-app-by-using-an-alias"></a>使用別名啟動您的應用程式

使用者和其他處理程序可以使用別名啟動您的應用程式，而不必指定您應用程式的完整路徑。 您可以指定別名名稱。

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
|Category |總是 ``windows.appExecutionAlias``。
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

|名稱 |描述 |
|-------|-------------|
|Category |總是 ``windows.fileTypeAssociation``。
|Name |您應用程式的唯一識別碼。 此識別碼係供內部用於產生與檔案類型關聯相關聯的雜湊[程式設計識別碼 (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)。 您可使用此識別碼來管理您應用程式未來版本中的變更。   |
|FileType |您應用程式支援的檔案副檔名。 |

<span id="executable" />
### <a name="start-an-executable-file-when-users-log-into-windows"></a>在使用者登入 Windows 時執行可執行檔

啟動工作能讓您的應用程式在每次使用者登入時，自動執行可執行檔。

> [!NOTE]
> 使用者必須至少啟動您的應用程式一次，才能使您的應用程式註冊啟動工作。

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
|Category |總是 ``windows.startupTask``。|
|Executable |欲啟動之可執行檔檔案的相對路徑。 |
|TaskId |您工作的唯一識別碼。 應用程式可使用此識別碼呼叫 [Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) 類別中的 API，運用程式設計方式啟用或停用啟動工作。 |
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

<span id="updates" />
### <a name="restart-automatically-after-receiving-an-update-from-the-windows-store"></a>從 Windows 市集接收更新後自動重新開機

如果當使用者安裝更新時您的應用程式開啟中，則會關閉應用程式。

如果您希望完成更新後該應用程式重新啟動，請在每個您想要重新啟動的處理程序中呼叫 [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) 函式。

您應用程式中的每個使用中視窗都會收到 [WM_QUERYENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376890.aspx) 訊息。 此時，如有需要您的應用程式可以再次呼叫 [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) 函式以更新命令列。

當應用程式中的每個使用中視窗收到 [WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx) 訊息時，您的應用程式應該會儲存資料並關閉。

>[!NOTE]
使用中視窗也會收到 [WM_CLOSE](https://msdn.microsoft.com/library/windows/desktop/ms632617.aspx) 訊息，以防應用程式未處理 [WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx) 訊息。

此時，您的應用程式有 30 秒時間可以關閉它本身的處理程序，否則平台會強制終止。

更新完成之後，您的應用程式會重新啟動。

## <a name="work-with-other-applications"></a>與其他應用程式合作

與其他應用程式整合，啟動其他處理程序，或共用資訊。

* [在支援列印的應用程式中，將您的應用程式作為列印目標顯示](#printing)
* [與其他 Windows 應用程式共用字型](#fonts)
* [從通用 Windows 平台 (UWP) 應用程式中啟動 Win32 處理程序](#win32-process)

<span id="printing" />
### <a name="make-your-app-appear-as-the-print-target-in-applications-that-support-printing"></a>在支援列印的應用程式中，將您的應用程式作為列印目標顯示

當使用者想要從其他應用程式，例如記事本列印資料時，您可以讓您的應用程式作為列印目標，顯示在該應用程式的可用列印目標清單中。

您必須修改您的應用程式，使其能接收以 XML 文件規格 (XPS) 格式儲存的列印資料。

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
|Category |總是 ``windows.appPrinter``。
|DisplayName |您想要出現在另外一個應用程式的列印目標清單中的顯示名稱。 |
|Parameters |任何能使您的應用程式正常處理要求的必要參數。 |

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

<span id="fonts" />
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
|Category |總是 ``windows.sharedFonts``。
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
<span id="win32-process" />
### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>從通用 Windows 平台 (UWP) 應用程式中啟動 Win32 處理程序

啟動完全信任的 Win32 處理程序。

#### <a name="xml-namespaces"></a>XML 命名空間

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>此延伸模組的元素和屬性

```XML
<xtension Category="windows.fullTrustProcess" Executable="[executable file]">
  <FullTrustProcess>
    <ParameterGroup GroupId="[GroupID]" Parameters="[Parameters]"/>
  </FullTrustProcess>
</Extension>
```

|名稱 |描述 |
|-------|-------------|
|Category |總是 ``windows.fullTrustProcess``。
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
此延伸模組在您欲建立可在所有裝置上執行的通用 Windows 平台使用者介面時將會非常有用，但您也會希望您 Win32 應用程式當中的元件能繼續在完全信任的情況下執行。

您只需要為您的 Win32 應用程式建立一個傳統型橋接器套件。 然後，將此延伸模組新增至您 UWP app 的套件檔案。 此延伸模組表示您想要在傳統型橋接器套件中執行可執行檔。  若您想要讓您的 UWP app 和 Win32 應用程式互相通訊，您可以設定一或多個[應用程式服務](../launch-resume/app-services.md)。 您可以在[這裡](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/)閱讀更多關於此案例的資訊。

## <a name="next-steps"></a>後續步驟

**尋找特定問題的解答**

我們的團隊會監視這些 [StackOverflow 標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。

**提供有關本文的意見反應**

使用下方的留言區塊。
