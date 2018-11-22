---
author: laurenhughes
title: 使用應用程式安裝程式檔案安裝相關集合
description: 在本節中，我們將檢閱您允許透過應用程式安裝程式安裝相關集合所需採取的步驟。 我們還會逐步完成建構定義相關集合之 *.appinstaller 檔案的步驟。
ms.author: lahugh
ms.date: 1/4/2018
ms.topic: article
keywords: windows 10, uwp, 應用程式安裝程式, AppInstaller, 側載, 相關集合, 選用套件
ms.localizationpriority: medium
ms.openlocfilehash: 4caf4333bb3d442779aedac2028b0996cbd17645
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/22/2018
ms.locfileid: "7576462"
---
# <a name="install-a-related-set-using-an-app-installer-file"></a>使用應用程式安裝程式檔案安裝相關集合

如果您才剛開始使用 UWP 選用套件或相關集合，下列文章會是不錯的入門資源。 

1.  [使用選用套件擴充您的應用程式](https://blogs.msdn.microsoft.com/appinstaller/2017/04/05/uwpoptionalpackages/)
2.  [建置您的第一個選用套件](https://blogs.msdn.microsoft.com/appinstaller/2017/05/09/build-your-first-optional-package/)
3.  [從選用套件載入程式碼](https://blogs.msdn.microsoft.com/appinstaller/2017/05/11/loading-code-from-an-optional-package/)
4.  [建立相關集合的工具](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/)
5.  [選用套件及相關集合的製作](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)

現在可以使用 Windows 10 Fall Creators Update，透過應用程式安裝程式相關集合。 這樣就可以將相關集合應用程式套件散發和部署給使用者。 

## <a name="how-to-install-a-related-set"></a>如何安裝相關集合 
相關集合不是一個實體，而是主要套件與選用套件的組合。 為了可以將相關集合安裝為一個實體，我們必須能夠將主要套件與選用套件指定成為一體。 若要這樣做，我們必須建立副檔名為「.appinstaller」的 XML 檔案，以定義相關集合。 應用程式安裝程式會取用 *.appinstaller 檔案，使用者只要按一下就可以安裝所有定義的套件。 

進一步詳細說明前，我們在這裡提供一個完整的範例 *.appinstaller 檔案：

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

進行部署時，會核對 `Uri` 元素中參考的應用程式套件來驗證應用程式安裝程式檔案。 因此，`Name`、`Publisher` 和 `Version` 必須與應用程式套件資訊清單中的 [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 元素相符。 

## <a name="how-to-create-an-app-installer-file"></a>如何建立應用程式安裝程式檔案 

若要將相關集合當做一個實體來散發，您必須建立應用程式安裝程式檔案，其中包含該 [appinstaller 結構描述](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file)所需的元素。

### <a name="step-1-create-the-appinstaller-file"></a>步驟 1：建立 *.appinstaller 檔案
使用文字編輯器建立檔案 (其中包含 XML)，並將其命名為 &lt;檔名&gt;.appinstaller 

### <a name="step-2-add-the-basic-template"></a>步驟 2：新增基本範本
基本範本包含應用程式安裝程式檔案資訊。 
```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
</AppInstaller>
```

### <a name="step-3-add-the-main-package-information"></a>步驟 3：新增主要套件資訊 
如果主要應用程式套件是.appxbundle 或.msixbundle 檔案，然後使用`<MainBundle>`如下所示。 如果主要應用程式套件是.appx 或.msix 檔案，然後使用`<MainPackage>`的`<MainBundle>`中的程式碼片段。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />

</AppInstaller>
```
`<MainBundle>` 或 `<MainPackage>` 屬性中的資訊必須分別符合應用程式套件組合資訊清單或應用程式套件資訊清單中的 [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 元素。 

### <a name="step-4-add-the-optional-packages"></a>步驟 4：新增選用套件 
與主要應用程式套件屬性相似，如果選用套件可以是應用程式套件或應用程式套件組合，則 `<OptionalPackages>` 屬性中的子元素必須分別為 `<Package>` 或 `<Bundle>`。 子元素中的套件資訊必須符合套件組合或套件資訊清單中的身分識別元素。 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

### <a name="step-5-add-dependencies"></a>步驟 5：新增相依性 
在相依性元素中，您可以指定主要套件或選用套件的必要架構套件。 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

</AppInstaller>
```

### <a name="step-6-add-update-setting"></a>步驟 6：新增更新設定 
應用程式安裝程式檔案也可以指定更新設定，讓相關集合自動在發佈新的應用程式安裝程式檔案時進行更新。 **<UpdateSettings>** 是選用元素。 在**<UpdateSettings>** 內，OnLaunch 選項指定 app 啟動時應該進行更新檢查，而 HoursBetweenUpdateChecks="12 指定每隔 12 小時應該進行一次更新檢查。 如果未指定 HoursBetweenUpdateChecks，用來檢查有更新的預設間隔為 24 小時。
``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>
    
    <UpdateSettings>
        <OnLaunch HoursBetweenUpdateChecks="12" />
    </UpdateSettings>

</AppInstaller>
```

如需 XML 結構描述的所有詳細資料，請參閱[應用程式安裝程式檔案參考資料](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file)。

> [!NOTE]
> 
> 應用程式安裝程式檔案類型是 Windows 10 Fall Creators Update 中的新增項目。 舊版 Windows 10 無法支援使用應用程式安裝程式檔案來部署 UWP app。
> 也請注意，**HoursBetweenUpdateChecks**元素是 Windows 10 下一個主要更新中的新功能。
