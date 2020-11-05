---
description: 了解如何將身分識別授與非封裝的傳統型應用程式，以便您在這些應用程式中使用新式 Windows 10 功能。
title: 將身分識別授與非封裝的傳統型應用程式
ms.date: 04/23/2020
ms.topic: article
keywords: windows 10, 傳統型, 套件, 識別資料, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 30fab5da3727153b8e1f33924ffcb6177843eb4e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031161"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>將身分識別授與非封裝的傳統型應用程式

許多 Windows 10 擴充性功能都會向非 UWP 傳統型應用程式要求所要使用的[套件識別資料](/uwp/schemas/appxpackage/uapmanifestschema/element-identity)，包括背景工作、通知、動態磚和共用目標。 在這些情況下，作業系統需要身分識別，才能識別對應 API 的呼叫者。

在 Windows 10 版本 2004 之前的作業系統版本中，將身分識別授與傳統型應用程式的唯一方法，就是[將其封裝在已簽署的 MSIX 套件](/windows/msix/desktop/desktop-to-uwp-root)中。 對於這些應用程式，身分識別會指定於套件資訊清單，而 MSIX 部署管線會根據資訊清單中的資訊來處理身分識別註冊。 套件資訊清單中參考的所有內容都會出現在 MSIX 套件內。

從 Windows 10 版本 2004 開始，您可建置 *疏鬆套件* 並向您的應用程式進行註冊，以將套件識別資料授與未封裝在 MSIX 套件中的傳統型應用程式。 此支援可讓尚無法採用 MSIX 封裝進行部署的傳統型應用程式，使用需要套件識別資料的 Windows 10 擴充性功能。 如需更多背景資訊，請參閱[這篇部落格文章](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)。

若要建置並註冊可將套件識別資料授與傳統型應用程式的疏鬆套件，請遵循下列步驟。

1. [建立疏鬆套件的套件資訊清單](#create-a-package-manifest-for-the-sparse-package)
2. [建置和簽署疏鬆套件](#build-and-sign-the-sparse-package)
3. [將套件識別中繼資料新增至傳統型應用程式資訊清單](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [在執行階段註冊您的疏鬆套件](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>重要概念

下列功能可讓非封裝的傳統型應用程式取得套件識別資料。

### <a name="sparse-packages"></a>疏鬆套件

「疏鬆套件」包含套件資訊清單，但沒有其他應用程式二進位檔和內容。 疏鬆套件的資訊清單可以參考在預先決定的外部位置中套件外部的檔案。 這可讓尚無法針對整個應用程式採用 MSIX 封裝的應用程式，依照某些 Windows 10 擴充性功能的要求取得套件識別資料。

> [!NOTE]
> 使用疏鬆套件的傳統型應用程式不會收到透過 MSIX 套件完全部署的部分優點。 這些優點包括防篡改保護、在鎖定位置安裝，以及作業系統在部署、執行階段和解除安裝時的完整管理。

### <a name="package-external-location"></a>套件外部位置

為了支援疏鬆套件，套件資訊清單結構描述現在支援 [**Properties**](/uwp/schemas/appxpackage/uapmanifestschema/element-properties) 元素之下的選擇性 [**uap10:AllowExternalContent**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-allowexternalcontent) 元素。 這可讓您的套件資訊清單參考套件外部的內容 (在磁片上的特定位置)。

例如，如果您現有的非封裝傳統型應用程式在 C:\Program Files\MyDesktopApp 中安裝應用程式可執行檔和其他內容\, 您可以建立疏鬆套件，其包含資訊清單中的 **uap10:AllowExternalContent** 元素。 在應用程式的安裝過程中，或第一次使用應用程式時，您可以安裝疏鬆套件，並將 C:\Program Files\MyDesktopApp\ 宣告為應用程式將使用的外部位置。

## <a name="create-a-package-manifest-for-the-sparse-package"></a>建立疏鬆套件的套件資訊清單

您必須先建立[套件資訊清單](/uwp/schemas/appxpackage/appx-package-manifest) (名為 AppxManifest.xml 的檔案)，以宣告傳統型應用程式的套件識別中繼資料及其他必要的詳細資料，才能建置疏鬆套件。 為疏鬆套件建立套件資訊清單的最簡單方式，就是使用下面的範例，並使用[結構描述參考](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)為您的應用程式進行自訂。

確定套件資訊清單包含下列項目：

* [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 元素，可描述傳統型應用程式的身分識別屬性。
* [Properties](/uwp/schemas/appxpackage/uapmanifestschema/element-properties) 元素之下的 [**uap10:AllowExternalContent**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-allowexternalcontent) 元素。 此元素應獲派 `true` 值，該值可讓您的套件資訊清單參考套件外部的內容 (在磁片上的特定位置)。 在稍後的步驟中，您將會從在您的安裝程式或應用程式中執行的程式碼註冊疏鬆套件時，指定外部位置的路徑。 您在資訊清單中所參考的任何內容 (不在套件本身) 都應該安裝到外部位置。
* [**TargetDeviceFamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 元素的 **MinVersion** 屬性應該設定為 `10.0.19000.0` 或更新版本。
* [**Application**](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 元素的 **TrustLevel=mediumIL** 和 **RuntimeBehavior=Win32App** 屬性會宣告與疏鬆套件相關聯的傳統型應用程式，其執行方式類似於標準未封裝的傳統型應用程式，而不需登錄和檔案系統虛擬化和其他執行階段變更。

下列範例顯示疏鬆套件資訊清單 (AppxManifest.xml) 的完整內容。 此資訊清單包含需要套件識別資料的 `windows.sharetarget` 擴充功能。

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

## <a name="build-and-sign-the-sparse-package"></a>建置和簽署疏鬆套件

建立套件資訊清單之後，請使用 Windows SDK 中的 [MakeAppx.exe 工具](/windows/msix/package/create-app-package-with-makeappx-tool)來建置疏鬆套件。 因為疏鬆套件不包含資訊清單中所參考的檔案，所以您必須指定 `/nv` 選項，以略過套件的語意驗證。

下列範例示範如何從命令列建立疏鬆套件。  

```Console
MakeAppx.exe pack /d <path to directory that contains manifest> /p <output path>\MyPackage.msix /nv
```

您必須使用目標電腦上受信任的憑證來簽署疏鬆套件，才能順利將其安裝在目標電腦上。 您可以建立新的自我簽署憑證以供開發之用，並使用 Windows SDK 中提供的 [SignTool](/windows/msix/package/sign-app-package-using-signtool) 來簽署疏鬆套件。

下列範例示範如何從命令列簽署疏鬆套件。

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx /p <certificate password> <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>將套件識別中繼資料新增至傳統型應用程式資訊清單

您也必須包含 [並存應用程式資訊清單，](/windows/win32/sbscs/application-manifests)與您的傳統型應用程式，並包含 [**msix**](/windows/win32/sbscs/application-manifests#msix) 元素，其中包含可宣告應用程式身分識別屬性的屬性。 當可執行檔啟動時，作業系統會使用這些屬性的值來判斷您應用程式的身分識別。

下列範例會顯示含有 **msix** 元素的並存應用程式資訊清單。

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

**msix** 元素的屬性，必須符合您疏鬆套件的套件資訊清單中的這些值：

* **packageName** 和 **publisher** 屬性必須分別符合套件資料清單中 [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 元素的 **Name** 和 **Publisher** 屬性。
* **applicationId** 屬性必須符合套件資訊清單中 [**Application**](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 元素的 **Id** 屬性。

並存應用程式資訊清單必須存在於與傳統型應用程式的可執行檔相同的目錄中，而且依照慣例，其名稱應該與應用程式的可執行檔相同並附加 `.manifest` 副檔名。 例如，如果您應用程式的可執行檔名稱是 `ContosoPhotoStore`，則應用程式資訊清單檔名應該是 `ContosoPhotoStore.exe.manifest`。

## <a name="register-your-sparse-package-at-run-time"></a>在執行階段註冊您的疏鬆套件

若要將套件識別資料授與您的傳統型應用程式，您的應用程式必須使用 [**PackageManager**](/uwp/api/windows.management.deployment.packagemanager) 類別的 [**AddPackageByUriAsync**](/uwp/api/windows.management.deployment.packagemanager.addpackagebyuriasync) 方法來註冊疏鬆套件。 Windows 10 版本 2004 起可以取得此方法。 您可以將程式碼新增至應用程式，以在第一次執行應用程式時註冊疏鬆套件，也可以在安裝傳統型應用程式時執行程式碼來註冊該套件 (例如，如果您使用 MSI 來安裝傳統型應用程式，可以從自訂動作執行此程式碼)。

下列範例示範如何註冊疏鬆套件。 此程式碼會建立 [**AddPackageOptions**](/uwp/api/windows.management.deployment.addpackageoptions) 物件，其中包含您的套件資訊清單可參考套件外部內容的外部位置路徑。 然後，程式碼會將此物件傳遞至 **AddPackageByUriAsync** 方法，以註冊疏鬆套件。 此方法也會將已簽署疏鬆套件的位置接收為 URI。 如需更完整的範例，請參閱相關[範例](#sample)中的 `StartUp.cs` 程式碼檔案。

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

如需可完整運作的範例應用程式，以示範如何使用疏鬆套件將套件識別資料授與傳統型應用程式，請參閱 [SparesePackages](https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages) 範例。 如需建置和執行範例的詳細資訊，請參閱[這篇部落格文章](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)。

此範例包含下列項目：

* 名為 PhotoStoreDemo 之 WPF 應用程式的原始程式碼。 在啟動期間，應用程式會檢查其是否正以身分識別執行。 如果其不是以身分識別執行，則會註冊疏鬆套件，然後重新啟動應用程式。 如需執行這些步驟的程式碼，請參閱 `StartUp.cs`。
* 名為 `PhotoStoreDemo.exe.manifest` 的並存應用程式資訊清單。
* 名為 `AppxManifest.xml` 的套件資訊清單。
