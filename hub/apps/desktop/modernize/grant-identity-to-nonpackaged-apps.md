---
Description: 瞭解如何將身分識別授與非封裝的桌面應用程式，讓您可以在這些應用程式中使用新式 Windows 10 功能。
title: 將身分識別授與非封裝的桌面應用程式
ms.date: 10/25/2019
ms.topic: article
keywords: windows 10、desktop、package、identity、MSIX、Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f355bba3087f58ed20800052371804048bc0006c
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2019
ms.locfileid: "73145613"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>將身分識別授與非封裝的桌面應用程式

<!--
> [!NOTE]
> The features described in this article require Windows 10 Insider Preview Build 10.0.19000.0 or a later release.
-->

許多 Windows 10 擴充性功能都需要從非 UWP 桌面應用程式使用[套件識別](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)，包括背景工作、通知、動態磚和共用目標。 在這些情況下，OS 需要身分識別，才能辨識對應 API 的呼叫者。

在 Windows 10 Insider preview 組建10.0.19000.0 之前的作業系統版本中，將身分識別授與桌面應用程式的唯一方式，就是將[它封裝在已簽署的 MSIX 套件中](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)。 針對這些應用程式，會在封裝資訊清單中指定身分識別，而 MSIX 部署管線會根據資訊清單中的資訊來處理身分識別註冊。 封裝資訊清單中參考的所有內容都會出現在 MSIX 套件內。

從 Windows 10 Insider Preview 組建10.0.19000.0 開始，您可以建立並向應用程式註冊*稀疏套件*，將套件識別授與未封裝在 MSIX 套件中的桌面應用程式。 這種支援可讓尚未採用 MSIX 封裝進行部署的桌面應用程式使用需要套件識別的 Windows 10 擴充性功能。 如需更多背景資訊，請參閱[這篇 blog 文章](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)。

若要建立並註冊將套件識別授與桌面應用程式的 sparse 封裝，請遵循下列步驟。

1. [建立 sparse 封裝的封裝資訊清單](#create-a-package-manifest-for-the-sparse-package)
2. [建立和簽署 sparse 封裝](#build-and-sign-the-sparse-package)
3. [將套件身分識別中繼資料新增至您的桌面應用程式資訊清單](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [在執行時間註冊您的 sparse 封裝](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>重要概念

下列功能可讓未封裝的桌面應用程式取得套件身分識別。

### <a name="sparse-packages"></a>Sparse 封裝

*Sparse 封裝*包含套件資訊清單，但沒有其他應用程式二進位檔和內容。 稀疏封裝的資訊清單可以在預先決定的外部位置參考封裝外部的檔案。 這可讓尚未針對整個應用程式採用 MSIX 封裝的應用程式，視某些 Windows 10 擴充性功能的需求來取得套件身分識別。

> [!NOTE]
> 使用稀疏套件的桌面應用程式不會收到透過 MSIX 套件完全部署的部分優點。 這些優點包括防篡改保護、在鎖定位置安裝，以及作業系統在部署、執行時間和卸載時的完整管理。

### <a name="package-external-location"></a>封裝外部位置

為了支援 sparse 封裝，封裝資訊清單架構現在支援[ **\<屬性\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties)元素下的選擇性 **\<AllowExternalContent\>** 元素。 這可讓您的套件資訊清單在磁片上的特定位置參考套件外的內容。

例如，如果您有現有的非封裝桌面應用程式，可在 C:\Program Files\MyDesktopApp 中安裝應用程式可執行檔和其他內容\, 您可以建立包含 **\<AllowExternalContent**的 sparse 封裝\>資訊清單中的元素。 在應用程式的安裝過程中，或第一次使用應用程式時，您可以安裝 sparse 封裝，並將 C:\Program Files\MyDesktopApp\ 宣告為應用程式將使用的外部位置。

## <a name="create-a-package-manifest-for-the-sparse-package"></a>建立 sparse 封裝的封裝資訊清單

建立 sparse 封裝之前，您必須先建立[封裝資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)（名為 package.appxmanifest.xml 的檔案），以宣告桌面應用程式的套件識別中繼資料和其他必要的詳細資訊。 為 sparse 封裝建立套件資訊清單最簡單的方式，就是使用下列範例，並使用[架構參考](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)針對您的應用程式進行自訂。

請確定套件資訊清單包含下列專案：

* [ **\<身分識別\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)元素，描述桌面應用程式的身分識別屬性。
* 在[ **\<屬性\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties)專案下 **\<AllowExternalContent\>** 元素。 此元素應指派 `true`的值，這可讓您的套件資訊清單在磁片上的特定位置參考封裝外部的內容。 在稍後的步驟中，您將會在從安裝程式或應用程式中執行的程式碼註冊您的 sparse 封裝時，指定外部位置的路徑。 您在資訊清單中所參考的任何內容（不在套件本身）都應該安裝到外部位置。
* [ **\<y\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)元素的**MinVersion**屬性應設定為 `10.0.19000.0` 或更新版本。
* [ **\<應用程式\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)元素的**TrustLevel = mediumIL**和**RuntimeBehavior = Win32App**屬性會宣告與 sparse 封裝相關聯的傳統型應用程式，其執行方式類似于標準的未封裝桌上型電腦應用程式，不含登錄和檔案系統虛擬化，以及其他執行時間變更。

下列範例顯示 sparse 封裝資訊清單（Package.appxmanifest.xml）的完整內容。 此資訊清單包含需要套件識別的 `windows.sharetarget` 延伸模組。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package 
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10"
  IgnorableNamespaces="uap uap2 uap3 rescap desktop uap10">
  <Identity Name="ContosoPhotoStore" ProcessorArchitecture="x64" Publisher="CN=Contoso" Version="1.0.0.0" />
  <Properties>
    <DisplayName>ContosoPhotoStore</DisplayName>
    <PublisherDisplayName>Contoso</PublisherDisplayName>
    <Logo>Assets\storelogo.png</Logo>
    <uap10:AllowExternalContent>true</uap10:AllowExternalContent>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19000.0" MaxVersionTested="10.0.19000.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
    <rescap:Capability Name="unvirtualizedResources"/>
  </Capabilities>
  <Applications>
    <Application Id="ContosoPhotoStore" Executable="ContosoPhotoStore.exe" uap10:TrustLevel="mediumIL" uap10:RuntimeBehavior="win32App"> 
      <uap:VisualElements AppListEntry="none" DisplayName="Contoso PhotoStore" Description="Demonstrate photo app" BackgroundColor="transparent" Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png">
        <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png" Square310x310Logo="Assets\LargeTile.png" Square71x71Logo="Assets\SmallTile.png"></uap:DefaultTile>
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
      <Extensions>
        <uap:Extension Category="windows.shareTarget">
          <uap:ShareTarget Description="Send to ContosoPhotoStore">
            <uap:SupportedFileTypes>
              <uap:FileType>.jpg</uap:FileType>
              <uap:FileType>.png</uap:FileType>
              <uap:FileType>.gif</uap:FileType>
            </uap:SupportedFileTypes>
            <uap:DataFormat>StorageItems</uap:DataFormat>
            <uap:DataFormat>Bitmap</uap:DataFormat>
          </uap:ShareTarget>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

## <a name="build-and-sign-the-sparse-package"></a>建立和簽署 sparse 封裝

建立套件資訊清單之後，請使用 Windows SDK 中的[makeappx.exe](https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool)來建立 sparse 封裝。 因為稀疏封裝不包含資訊清單中所參考的檔案，所以您必須指定 `/nv` 選項，以略過封裝的語意驗證。

下列範例示範如何從命令列建立 sparse 封裝。  

```Console
MakeAppx.exe  pack  /d  <path to directory that contains manifest>  /p  <output path>\MyPackage.msix  /nv
```

您必須使用目的電腦上受信任的憑證來簽署您的 sparse 封裝，才能順利將它安裝在目的電腦上。 您可以建立新的自我簽署憑證以供開發之用，並使用 Windows SDK 中提供的[SignTool](https://docs.microsoft.com/windows/msix/package/sign-app-package-using-signtool)簽署您的 sparse 封裝。

下列範例示範如何從命令列簽署 sparse 封裝。

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx  /p <certificate password>  <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>將套件身分識別中繼資料新增至您的桌面應用程式資訊清單

您也必須在桌面應用程式中包含[並存應用程式資訊清單](https://docs.microsoft.com/windows/win32/sbscs/application-manifests)，並包含一個 **\<msix\>** 元素，其中具有可宣告應用程式身分識別屬性的屬性。 當可執行檔啟動時，OS 會使用這些屬性的值來判斷您應用程式的身分識別。

下列範例顯示具有 **\<msix\>** 元素的並存應用程式資訊清單。

```xml
<?xml version="1.0" encoding="utf-8"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity version="1.0.0.0" name="Contoso.PhotoStoreApp"/>
  <msix xmlns="urn:schemas-microsoft-com:msix.v1"
          publisher="CN=Contoso"
          packageName="ContosoPhotoStore"
          applicationId="ContosoPhotoStore"
        />
</assembly>
```

**\<msix\>** 元素的屬性必須符合您的 sparse 封裝之套件資訊清單中的這些值：

* **PackageName**和**publisher**屬性必須分別符合封裝資訊清單中[ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)元素的**名稱**和**發行者**屬性。
* **ApplicationId**屬性必須符合封裝資訊清單中[ **\<應用程式\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)元素的**Id**屬性。

並存應用程式資訊清單必須存在於與桌面應用程式的可執行檔相同的目錄中，而且依照慣例，其名稱應該與應用程式的可執行檔相同，並附加 `.manifest` 延伸模組。 例如，如果您應用程式的可執行檔名稱是 `ContosoPhotoStore`，應用程式資訊清單檔案名應該是 `ContosoPhotoStore.exe.manifest`。

## <a name="register-your-sparse-package-at-run-time"></a>在執行時間註冊您的 sparse 封裝

若要將套件識別授與您的桌面應用程式，您的應用程式必須使用[PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager)類別來註冊稀疏封裝。 您可以將程式碼新增至應用程式，以在第一次執行應用程式時註冊 sparse 封裝，或在安裝桌面應用程式時執行程式碼以註冊套件（例如，如果您使用 MSI 來安裝桌面應用程式，您可以從自訂動作執行此程式碼）。

下列範例示範如何註冊 sparse 封裝。 此程式碼會建立**AddPackageOptions**物件，其中包含您的封裝資訊清單可以參考封裝外部內容的外部位置路徑。 然後，程式碼會將此物件傳遞至**PackageManager AddPackageByUriAsync**方法，以註冊 sparse 封裝。 這個方法也會將已簽署的 sparse 封裝的位置接收為 URI。 如需更完整的範例，請參閱相關[範例](#sample)中的 `StartUp.cs` 程式碼檔案。

```csharp
private static bool registerSparsePackage(string externalLocation, string sparsePkgPath)
{
    bool registration = false;
    try
    {
        Uri externalUri = new Uri(externalLocation);
        Uri packageUri = new Uri(sparsePkgPath);

        Console.WriteLine("exe Location {0}", externalLocation);
        Console.WriteLine("msix Address {0}", sparsePkgPath);

        Console.WriteLine("  exe Uri {0}", externalUri);
        Console.WriteLine("  msix Uri {0}", packageUri);

        PackageManager packageManager = new PackageManager();

        // Declare use of an external location
        var options = new AddPackageOptions();
        options.ExternalLocationUri = externalUri;

        Windows.Foundation.IAsyncOperationWithProgress<DeploymentResult, DeploymentProgress> deploymentOperation = packageManager.AddPackageByUriAsync(packageUri, options);

        // Other progress and error-handling code omitted for brevity...
    }
}
```

## <a name="sample"></a>範例

如需可完整運作的範例應用程式，示範如何使用 sparse 封裝將套件識別授與桌面應用程式，請參閱[https://aka.ms/sparsepkgsample](https://aka.ms/sparsepkgsample)。 如需建立和執行範例的詳細資訊，請閱讀[這篇文章](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)。

這個範例包含下列各項：

* 名為 PhotoStoreDemo 之 WPF 應用程式的原始程式碼。 在啟動期間，應用程式會檢查它是否正在以身分識別執行。 如果它不是以身分識別執行，它會註冊 sparse 封裝，然後重新開機應用程式。 如需執行這些步驟的程式碼，請參閱 `StartUp.cs`。
* 名為 `PhotoStoreDemo.exe.manifest`的並存應用程式資訊清單。
* 名為 `AppxManifest.xml`的封裝資訊清單。
