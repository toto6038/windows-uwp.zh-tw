---
author: normesta
Description: Shows how to manually package a Windows desktop application (like Win32, WPF, and Windows Forms) for Windows 10.
Search.Product: eADQiWindows 10XVcnh
title: 手動封裝應用程式 （傳統型橋接器）
ms.author: normesta
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
ms.openlocfilehash: 29fd7faec6f78b5f01469e7bfa6c01a8831f07eb
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5641148"
---
# <a name="package-a-desktop-application-manually"></a>手動封裝的傳統型應用程式

本主題示範如何不使用工具例如 Visual Studio 或 Desktop App Converter (DAC) 封裝您的應用程式。

若要手動封裝應用程式，您必須先建立封裝資訊清單檔案，然後執行命令列工具來產生 Windows 應用程式套件。

如果您使用 xcopy 命令安裝您的應用程式，或您已熟悉您的應用程式安裝程式對系統的變更，請考慮手動封裝，並想要對程序更細微的控制。

若您不確定您的安裝程式會對系統做出什麼變更，或者您想要使用自動化的工具產生您的封裝資訊清單，請考慮[這些](desktop-to-uwp-root.md#convert)選項。

>[!IMPORTANT]
>若要建立 Windows 應用程式套件的傳統型應用程式的能力 （傳統型橋接器，稱為 Windows 10，版本 1607 開始，引進了否則和它只能在專案中目標為 Windows 10 年度更新版 (10.0;組建 14393） 或更新版本在 Visual Studio 中的。

## <a name="first-prepare-your-application"></a>首先，準備您的應用程式

開始建立您的應用程式套件之前，先檢閱本指南：[準備封裝傳統型應用程式](desktop-to-uwp-prepare.md)。

## <a name="create-a-package-manifest"></a>建立封裝資訊清單

建立一個檔案，並將其命名為 **appxmanifest.xml**，然後將此 XML 新增至其中。

這是一個包含了您套件所需之元素和屬性的基礎範本。 我們會在下一節添加數值。

```XML
<?xml version="1.0" encoding="utf-8"?>
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
  <Identity Name="" Version="" Publisher="" ProcessorArchitecture="" />
    <Properties>
       <DisplayName></DisplayName>
       <PublisherDisplayName></PublisherDisplayName>
             <Description></Description>
      <Logo></Logo>
    </Properties>
    <Resources>
      <Resource Language="" />
    </Resources>
      <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="" MaxVersionTested="" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
    <Applications>
      <Application Id="" Executable="" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements DisplayName="" Description=""   Square150x150Logo=""
                   Square44x44Logo=""   BackgroundColor="" />
      </Application>
     </Applications>
  </Package>
```

## <a name="fill-in-the-package-level-elements-of-your-file"></a>填寫您檔案中的套件層級元素

使用足以描述您套件的資訊填寫這個範本。

### <a name="identity-information"></a>識別資訊

以下是一個屬性以預留位置文字填上的 **Identity** 元素範例。 您可以將 ``ProcessorArchitecture`` 屬性設定為 ``x64`` 或 ``x86``。

```XML
<Identity Name="MyCompany.MySuite.MyApp"
          Version="1.0.0.0"
          Publisher="CN=MyCompany, O=MyCompany, L=MyCity, S=MyState, C=MyCountry"
                ProcessorArchitecture="x64">
```
> [!NOTE]
> 如果您已經保留了您在 Windows 市集中的應用程式名稱，您可以使用 Windows 開發人員中心儀表板取得名稱和發行者。 如果您打算側載至其他系統應用程式，您可以提供您自己的這些名稱，只要您用來簽署您的應用程式的發行者名稱，依您選擇符合憑證的名稱。

### <a name="properties"></a>屬性

[Properties](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-properties) 元素有 3 個必要的子元素。 以下是以預留位置文字填上元素的 **Properties** 節點範例。 **DisplayName**是您上傳至 microsoft store 的應用程式市集中，保留的應用程式的名稱。

```XML
<Properties>
  <DisplayName>MyApp</DisplayName>
  <PublisherDisplayName>MyCompany</PublisherDisplayName>
  <Logo>images\icon.png</Logo>
</Properties>
```

### <a name="resources"></a>資源

以下是 [Resource](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-resources) 節點的範例。

```XML
<Resources>
  <Resource Language="en-us" />
</Resources>
```
### <a name="dependencies"></a>相依性

針對您建立的封裝的傳統型應用程式，一律設定``Name``屬性``Windows.Desktop``。

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>功能
針對傳統型應用程式，您建立的封裝，如您必須新增``runFullTrust``功能。

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>填寫應用程式層級元素

使用足以描述您應用程式的資訊填寫這個範本。

### <a name="application-element"></a>應用程式元素

針對您建立的封裝的傳統型應用程式``EntryPoint``Application 元素的屬性永遠都是``Windows.FullTrustApplication``。

```XML
<Applications>
  <Application Id="MyApp"     
        Executable="MyApp.exe" EntryPoint="Windows.FullTrustApplication">
   </Application>
</Applications>
```

### <a name="visual-elements"></a>視覺元素

以下是 [VisualElements](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements) 節點的範例。

```XML
<uap:VisualElements
    BackgroundColor="#464646"
    DisplayName="My App"
    Square150x150Logo="images\icon.png"
    Square44x44Logo="images\small_icon.png"
    Description="A useful description" />
```
<a id="target-based-assets" />

## <a name="optional-add-target-based-unplated-assets"></a>(選用) 新增以目標為基礎的 Unplated 資產

以目標為基礎的資產適用於圖示和磚，會顯示在 Windows 工作列、工作檢視、ALT + TAB 及貼齊小幫手上，以及開始畫面磚的右下角。 您也可以在[這裡](https://docs.microsoft.com/windows/uwp/design/style/app-icons-and-logos#unplated-assets)閱讀更多資訊。

1. 取得正確的 44x44 影像，並將它們複製到包含您的影像 (也就是資產) 的資料夾。

2. 針對每個 44x44 影像，在相同的資料夾中建立一個複本，然後將 **.targetsize-44_altform-unplated** 附加至檔案名稱。 每個圖示應該會有兩個複本，每個均以特定方式命名。 例如：完成此程序之後，您的「資產 (assets)」資料夾可能會包含 **MYAPP_44x44.png** 和 **MYAPP_44x44.targetsize-44_altform-unplated.png**。

   > [!NOTE]
   > 在這個範例中，名為 **MYAPP_44x44.png** 的圖示將會是您 Windows 應用程式套件的 ``Square44x44Logo`` 商標屬性中參考的圖示。

3.  在 Windows 應用程式套件中，將您正在製作的每個圖示之 ``BackgroundColor`` 設定為透明。

4. 繼續前往下一小節，產生新的套件資源索引檔案。

<a id="make-pri" />

### <a name="generate-a-package-resource-index-pri-file"></a>產生套件資源索引 (PRI) 檔案

如果您在上，一節中所述建立以目標為基礎的資產，或您修改的任一視覺資產，您的應用程式在建立套件之後，您必須產生新的 PRI 檔案。

1.  開啟**適用於 VS 2017 的開發人員命令提示字元**。

    ![開發人員命令提示字元](images/desktop-to-uwp/developer-command-prompt.png)

2.  將目錄變更為套件的根資料夾，然後執行命令 ``makepri createconfig /cf priconfig.xml /dq en-US`` 來建立 priconfig.xml 檔案。

5.  使用 ``makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml`` 命令來建立 resources.pri 檔案。

    例如，您的應用程式的命令可能看起來像這樣： ``makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``。

6.  使用下一個步驟中的指示封裝您的 Windows 應用程式套件。

<a id="make-appx" />

## <a name="generate-a-windows-app-package"></a>產生 Windows 應用程式套件

使用 **MakeAppx.exe** 來為您的專案產生 Windows 應用程式套件。 此項目已隨附於 Windows 10 SDK，如果您已安裝 Visual Studio，則可透過您 Visual Studio 版本的「開發人員命令提示字元」輕鬆存取。

請參閱[使用 MakeAppx.exe 工具建立應用程式套件](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)

## <a name="run-the-packaged-app"></a>執行封裝後的應用程式

您可以執行您的應用程式進行測試，在本機而不需要取得憑證和進行簽署。 您只需要執行此 PowerShell cmdlet：

```Add-AppxPackage –Register AppxManifest.xml```

若要更新應用程式的 .exe 或.dll 檔案，使用新檔案來取代套件中現有的檔案、提高 AppxManifest.xml 的版本號碼，然後再次執行上述命令即可。

> [!NOTE]
> 已封裝的應用程式一律會以互動式使用者的身分執行，並安裝您已封裝的應用程式上任何磁碟機必須格式化為 NTFS 格式。

## <a name="next-steps"></a>後續步驟

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

**逐步執行程式碼/找出並修正問題**

請參閱[執行、 偵錯以及測試封裝的傳統型應用程式](desktop-to-uwp-debug.md)

**簽署您的應用程式並散布它**

請參閱[散發的已封裝的傳統型應用程式](desktop-to-uwp-distribute.md)
