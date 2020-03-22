---
Description: 瞭解如何建立託管應用程式，以繼承主機應用程式的可執行檔、進入點和執行時間屬性。
title: 建立託管應用程式
ms.date: 01/28/2020
ms.topic: article
keywords: windows 10、desktop、package、identity、MSIX、Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 870042f3a7737e5caf646d4d14ffd49af39a079f
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108141"
---
# <a name="create-hosted-apps"></a>建立託管應用程式

從 Windows 10 版本2004開始，您可以建立*託管應用程式*。 裝載的應用程式與父*主機*應用程式共用相同的可執行檔和定義，但是其外觀和行為類似于系統上的個別應用程式。

裝載的應用程式適用于您想要元件（例如可執行檔或腳本檔案）行為類似 Windows 10 應用程式的情況，但元件需要主機進程才能執行。 例如，PowerShell 或 Python 腳本可能會以裝載應用程式的形式傳遞，需要安裝主機才能執行。 裝載的應用程式可以有自己的開始磚、身分識別，以及與 Windows 10 功能（例如背景工作、通知、磚和共用目標）的深度整合。

封裝資訊清單中的數個專案和屬性支援裝載的應用程式功能，可讓託管應用程式使用主機應用程式封裝中的可執行檔和定義。 當使用者執行裝載的應用程式時，作業系統會自動以託管應用程式的身分識別來啟動主機可執行檔。 然後主機可以將視覺資產、內容或呼叫 Api 載入為裝載的應用程式。 裝載的應用程式會取得在主機與託管應用程式之間宣告的功能交集。 這表示託管應用程式無法要求比主機所提供的更多功能。

## <a name="define-a-host"></a>定義主機

*主機*是託管應用程式的主要可執行檔或執行時間進程。 目前，唯一支援的主機是具有*套件識別*的桌面C++應用程式（.net 或/Win32）。 目前不支援將 UWP 應用程式當做主機。 桌面應用程式有數種方式可以使用套件識別：

* 將套件識別授與桌面應用程式的最常見方式，是將[它封裝在 MSIX 套件中](https://docs.microsoft.com/windows/msix)。
* 在某些情況下，您也可以選擇藉由建立[sparse 封裝](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)來授與封裝身分識別。 如果您無法採用 MSIX 封裝來部署桌面應用程式，這個選項就很有用。

主機是在其套件資訊清單中，由**uap10： HostRuntime**延伸模組所宣告。 此延伸模組有一個**Id**屬性，必須指派一個值給裝載的應用程式的套件資訊清單也會參考它。 啟動裝載的應用程式時，會在裝載的應用程式的身分識別下啟動主機，並且可以從主控的應用程式套件載入內容或二進位檔。

下列範例示範如何在封裝資訊清單中定義主機。 **Uap10： HostRuntime**延伸模組是封裝範圍，因此會宣告為[package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-package)元素的子系。

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Extensions>
    <uap10:Extension Category="windows.hostRuntime"  
        Executable="PyScriptEngine\PyScriptEngine.exe"  
        uap10:RuntimeBehavior="packagedClassicApp"  
        uap10:TrustLevel="mediumIL">
      <uap10:HostRuntime Id="PythonHost" />
    </uap10:Extension>
  </Extensions>

</Package>
```

請記下下列元素的重要詳細資料。

| 元素              | 詳細資料 |
|----------------------|-------|
| **uap10：擴充功能** | `windows.hostRuntime` 類別目錄會宣告一個封裝範圍的延伸模組，以定義啟動託管應用程式時所要使用的執行時間資訊。 裝載的應用程式將會以延伸模組中宣告的定義來執行。 使用在上一個範例中宣告的主機應用程式時，裝載的應用程式將會在**mediumIL**信任層級以可執行檔**PyScriptEngine**的形式執行。<br/><br/>**可執行檔**、 **uap10： RuntimeBehavior**和**uap10： TrustLevel**屬性會指定封裝中主機進程二進位檔的名稱，以及裝載應用程式的執行方式。 例如，使用上一個範例中屬性的託管應用程式，將會在 mediumIL 信任層級以可執行檔 PyScriptEngine 的形式執行。 |
| **uap10： HostRuntime** | **Id**屬性會宣告封裝中此特定主機應用程式的唯一識別碼。 一個封裝可以有多個主應用程式，而且每個都必須有一個具有唯一**識別碼**的**uap10： HostRuntime**元素。

## <a name="declare-a-hosted-app"></a>宣告託管應用程式

*託管應用程式*會宣告*主機*上的封裝相依性。 裝載的應用程式會利用主機的識別碼（也就是主機套件中**uap10： HostRuntime**擴充功能的**id**屬性）來啟用，而不是在自己的套件中指定進入點可執行檔。 裝載的應用程式通常包含可由主機存取的內容、視覺資產、腳本或二進位檔。 裝載的應用程式封裝中的[y](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)值應該以與主機相同的值為目標。

裝載的應用程式套件可以是簽署或不帶正負號：

* 已簽署的套件可能包含可執行檔。 這在具有二進位延伸機制的案例中很有用，這可讓主機在裝載的應用程式封裝中載入 DLL 或已註冊的元件。
* 未簽署的封裝只能包含無法執行檔。 這在主機只需要載入影像、資產和內容或腳本檔案的案例中很有用。 未簽署的套件必須在其[Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)元素中包含特殊的 `OID` 值，否則將無法註冊。 這可防止不帶正負號的套件與或詐騙已簽署套件的身分識別相互衝突。

若要定義託管應用程式，請在封裝資訊清單中宣告下列專案：

* **Uap10： HostRuntimeDependency**元素。 這是相依[性元素的](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-dependencies)子系。
* [應用程式](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)專案的**uap10： HostId**屬性（適用于應用程式）或[Extension](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension)元素（適用于可啟動的擴充功能）。

下列範例示範不帶正負號之託管應用程式之套件資訊清單的相關區段。

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Identity Name="NumberGuesserManifest"
    Publisher="CN=AppModelSamples, OID.2.25.311729368913984317654407730594956997722=1"
    Version="1.0.0.0" />

  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19041.0" MaxVersionTested="10.0.19041.0" />
    <uap10:HostRuntimeDependency Name="PyScriptEnginePackage" Publisher="CN=AppModelSamples" MinVersion="1.0.0.0"/>
  </Dependencies>

  <Applications>
    <Application Id="NumberGuesserApp"  
      uap10:HostId="PythonHost"  
      uap10:Parameters="-Script &quot;NumberGuesser.py&quot;">
    </Application>
  </Applications>

</Package>
```

請記下下列元素的重要詳細資料。

| 元素              | 詳細資料 |
|----------------------|-------|
| [**2x2**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) | 因為此範例中的裝載應用程式封裝不帶正負號，所以**發行者**屬性必須包含 `OID.2.25.311729368913984317654407730594956997722=1` 字串。 這可確保未簽署的套件無法欺騙已簽署套件的身分識別。 |
| [**Y**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) | **MinVersion**屬性必須指定10.0.19041.0 或更新版本的 OS。 |
| **uap10:HostRuntimeDependency** | 此元素元素會宣告主機應用程式套件的相依性。 這是由主機套件的**名稱**和**發行者**，以及它所依賴的**MinVersion**所組成。 這些值可以在主機封裝中的[Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)元素下找到。 |
| **應用程式** | **Uap10： HostId**屬性會表示主機上的相依性。 裝載的應用程式套件必須宣告這個屬性，而不是[應用程式](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)或[延伸](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension)元素的一般**可執行檔**和**EntryPoint**屬性。 因此，託管應用程式會從主機繼承具有對應的**HostId**值的**可執行檔**、 **EntryPoint**和執行時間屬性。<br/><br/>**Uap10： Parameters**屬性會指定傳遞至主機可執行檔之進入點函式的參數。 因為主機必須知道該如何處理這些參數，所以主機和裝載應用程式之間有隱含的合約。 |

## <a name="register-an-unsigned-hosted-app-package-at-run-time"></a>在執行時間註冊不帶正負號的託管應用程式套件

**Uap10： HostRuntime**延伸模組的其中一個優點是，它可讓主機在執行時間以動態方式產生託管應用程式套件，並使用[PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) API 來註冊它，而不需要簽署它。 這可讓主機以動態方式產生託管應用程式套件的內容和資訊清單，然後進行註冊。

使用[PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager)類別的下列方法，註冊未簽署的託管應用程式封裝。 從 Windows 10 版本2004開始提供這些方法。

* **AddPackageByUriAsync**：使用*Options*參數的**AllowUnsigned**屬性，註冊未簽署的 MSIX 套件。
* **RegisterPackageByUriAsync**：執行鬆散封裝資訊清單檔案註冊。 如果封裝已簽署，則包含資訊清單的資料夾必須包含[appxsignature.p7x](https://docs.microsoft.com/windows/msix/overview#inside-an-msix-package)檔案和目錄。 如果不帶正負號，必須設定*options*參數的**AllowUnsigned**屬性。

### <a name="requirements-for-unsigned-hosted-apps"></a>未簽署託管應用程式的需求

* 封裝資訊清單中的[應用程式](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)或[延伸](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension)模組元素不能包含啟用資料，例如**可執行檔**、 **EntryPoint**或**TrustLevel**屬性。 相反地，這些元素只能包含**uap10： HostId**屬性來表示主機上的相依性，以及**uap10： Parameters**屬性。
* 封裝必須是主要的封裝。 它不能是組合、架構封裝、資源或選用的封裝。

### <a name="requirements-for-a-host-that-installs-and-registers-an-unsigned-hosted-app-package"></a>安裝及註冊不帶正負號之託管應用程式套件的主機需求

* 主機必須有[套件身分識別](#define-a-host)。
* 主機必須具有**packageManagement** [限制的功能](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)。
    ```xml
    <rescap:Capability Name="packageManagement" />
    ```

<!--
* If the host runs in app container (for example, it is a UWP app), it must also have the unsigned package management [custom capability](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#custom-capabilities) and a [Signed Custom Capability Descriptor (SCCD) file](https://docs.microsoft.com/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file).
    ```xml
    <uap4:CustomCapability Name="Microsoft.unsignedPackageManagement_cw5n1h2txyewy" />
    ```
-->

## <a name="sample"></a>範例

如需將本身宣告為主機，然後在執行時間動態註冊託管應用程式套件的完整功能範例應用程式，請參閱[託管應用程式範例](https://aka.ms/hostedappsample)。

### <a name="the-host"></a>主機

主機名稱為**PyScriptEngine**。 這是在中C#撰寫的包裝函式，會執行 python 腳本。 以 `-Register` 參數執行時，腳本引擎會安裝包含 python 腳本的託管應用程式。 當使用者嘗試啟動新安裝的裝載應用程式時，主機會啟動並執行**NumberGuesser** python 腳本。

主機應用程式的套件資訊清單（PyScriptEnginePackage 資料夾中的 package.appxmanifest.xml 檔案）包含**uap10： HostRuntime**延伸模組，它會將應用程式宣告為識別碼**PythonHost**和可執行檔**PyScriptEngine**的主機。  

> [!NOTE]
> 在此範例中，封裝資訊清單的名稱是 package.appxmanifest.xml，而它是[Windows 應用程式封裝專案](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)的一部分。 建立此專案時，它會[產生名為 package.appxmanifest.xml 的資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/generate-package-manifest)，並建立主機應用程式的 MSIX 套件。

### <a name="the-hosted-app"></a>裝載的應用程式

裝載的應用程式是由 python 腳本和封裝成品（例如封裝資訊清單）所組成。 它不包含任何 PE 檔案。

裝載應用程式的套件資訊清單（NumberGuesser/Package.appxmanifest.xml 檔案）包含下列專案：

* [Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)元素的**發行者**屬性包含不帶正負號封裝所需的 `OID.2.25.311729368913984317654407730594956997722=1` 識別碼。
* [Application](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)元素的**uap10： HostId**屬性會將**PythonHost**識別為其主機。

### <a name="run-the-sample"></a>執行範例

此範例需要 Windows 10 的10.0.19041.0 或更新版本，以及 Windows SDK。

1. 將[範例](https://aka.ms/hostedappsample)下載到開發電腦上的資料夾。
2. 在 Visual Studio 中開啟 [PyScriptEngine] 方案，並將**PyScriptEnginePackage**專案設定為啟始專案。
3. 建立**PyScriptEnginePackage**專案。
4. 在方案總管中，以滑鼠右鍵按一下**PyScriptEnginePackage**專案，然後選擇 [**部署**]。
5. 在您複製範例檔案的目錄中開啟 [命令提示字元] 視窗，然後執行下列命令來註冊範例**NumberGuesser**應用程式（託管應用程式）。 將 `D:\repos\HostedApps` 變更為您複製範例檔案的路徑。

    ```CMD
    D:\repos\HostedApps>pyscriptengine -Register D:\repos\HostedApps\NumberGuesser\AppxManifest.xml
    ```

    > [!NOTE]
    > 您可以在命令列上執行 `pyscriptengine`，因為範例中的主機會宣告[AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias)。

6. 開啟 [**開始**] 功能表，然後按一下 [ **NumberGuesser** ] 以執行裝載的應用程式。
