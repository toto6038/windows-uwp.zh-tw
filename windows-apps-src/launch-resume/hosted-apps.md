---
description: 瞭解如何建立託管應用程式，以繼承主機應用程式的可執行檔、進入點和執行時間屬性。
title: 建立託管應用程式
ms.date: 04/23/2020
ms.topic: article
keywords: windows 10, 傳統型, 套件, 識別資料, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 205f17227b13b2da4177f42fb7773b6779bc02cb
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032911"
---
# <a name="create-hosted-apps"></a>建立託管應用程式

從 Windows 10 （2004版）開始，您可以建立 *託管應用程式* 。 裝載的應用程式與父 *主機* 應用程式共用相同的可執行檔和定義，但其外觀和行為就像系統上的個別應用程式。

裝載的應用程式適用於您想要元件 (例如可執行檔或指令碼檔案) 行為類似獨立 Windows 10 應用程式，但元件需要主機處理序才能執行的情況。 例如，PowerShell 或 Python 腳本可以作為託管應用程式來傳遞，而此應用程式需要安裝主機才能執行。 裝載的應用程式可以有自己的開始磚、身分識別，以及與 Windows 10 功能 (例如背景工作、通知、磚和共用目標) 的深度整合。

封裝資訊清單中的數個元素和屬性支援裝載的應用程式功能，可讓託管應用程式使用主機應用程式封裝中的可執行檔和定義。 當使用者執行裝載的應用程式時，OS 會自動以託管應用程式的身分識別來啟動主機可執行檔。 然後，主機可以載入視覺資產、內容或呼叫 Api 作為託管應用程式。 裝載的應用程式會取得主機和裝載應用程式之間所宣告的功能交集。 這表示託管應用程式無法要求比主機提供的功能更多的功能。

## <a name="define-a-host"></a>定義主機

*主機是裝載* 應用程式的主要可執行檔或執行時間進程。 目前，唯一支援的主機是桌面應用程式 ( 具有 *套件身分識別* 的 .Net 或 c + +/Win32) 。 UWP 應用程式目前不支援做為主機。 傳統型應用程式有幾種方式可擁有套件身分識別：

* 將套件身分識別授與傳統型應用程式的最常見方式，是將 [它封裝在 MSIX 套件中](/windows/msix)。
* 在某些情況下，您也可以選擇藉由建立 [稀疏套件](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)來授與封裝身分識別。 如果您無法採用 MSIX 封裝來部署傳統型應用程式，此選項會很有用。

在 [**uap10： HostRuntime**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntime) 延伸模組的套件資訊清單中宣告主機。 此延伸模組有一個 **Id** 屬性，此屬性必須被指派一個值，而此值也會由託管應用程式的套件資訊清單所參考。 當裝載的應用程式啟動時，主機會以託管應用程式的身分識別來啟動，而且可以從裝載的應用程式套件載入內容或二進位檔。

下列範例示範如何在封裝資訊清單中定義主機。 **Uap10： HostRuntime** 延伸模組是封裝範圍，因此會宣告為 [**package**](/uwp/schemas/appxpackage/uapmanifestschema/element-package)元素的子系。

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
| [**uap10:Extension**](/wp/schemas/appxpackage/uapmanifestschema/element-uap10-extension) | `windows.hostRuntime`類別宣告封裝範圍的延伸模組，可定義啟用裝載的應用程式時所要使用的執行時間資訊。 裝載的應用程式將會使用擴充功能中宣告的定義來執行。 使用上一個範例中所宣告的主機應用程式時，裝載的應用程式會以 **mediumIL** 信任層級上的可執行檔 **PyScriptEngine.exe** 的形式執行。<br/><br/>**可執行檔** 、 **uap10： RuntimeBehavior** 和 **uap10： TrustLevel** 屬性指定封裝中主機進程二進位檔的名稱，以及裝載的應用程式將如何執行。 例如，使用上一個範例中屬性的託管應用程式，將會以 mediumIL 信任層級的可執行檔 PyScriptEngine.exe 的形式執行。 |
| [**uap10:HostRuntime**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntime) | **Id** 屬性會在封裝中宣告此特定主機應用程式的唯一識別碼。 封裝可以有多個主應用程式，且每個應用程式都必須具有唯一 **識別碼** 的 **uap10： HostRuntime** 元素。

## <a name="declare-a-hosted-app"></a>宣告託管應用程式

*託管應用程式* 會宣告 *主機* 上的套件相依性。 裝載的應用程式會利用主機的識別碼 (也就是主機封裝中 **uap10： HostRuntime** 延伸模組的 **ID** 屬性，) 進行啟用，而不是在它自己的封裝中指定進入點可執行檔。 裝載的應用程式通常會包含可由主機存取的內容、視覺資產、腳本或二進位檔。 裝載應用程式封裝中的 [**y**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 值應該以與主機相同的值作為目標。

裝載的應用程式套件可以進行簽署或未簽署：

* 簽署的封裝可能包含可執行檔。 這在具有二進位擴充機制的案例中很有用，這可讓主機在託管應用程式封裝中載入 DLL 或已註冊的元件。
* 未簽署的封裝只能包含非可執行檔。 當主機只需要載入影像、資產和內容或腳本檔案時，這項功能就很有用。 未簽署的套件必須 `OID` 在其 [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 元素中包含特殊值，否則將無法註冊。 這可防止未簽署的套件與簽署套件的身分識別衝突或詐騙。

若要定義託管應用程式，請在套件資訊清單中宣告下列專案：

* [**Uap10： HostRuntimeDependency**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntimedependency)元素。 這是相依 [性元素的](/uwp/schemas/appxpackage/uapmanifestschema/element-dependencies) 子系。
* [**應用程式**](/uwp/schemas/appxpackage/uapmanifestschema/element-application)元素的 **uap10： HostId** 屬性 (針對可啟動的擴充功能)  (的應用程式) 或 [**擴充**](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension)專案。

下列範例示範未簽署的託管應用程式之套件資訊清單的相關區段。

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
| [**身分識別**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) | 因為此範例中的託管應用程式封裝未簽署，所以 **發行者** 屬性必須包含 `OID.2.25.311729368913984317654407730594956997722=1` 字串。 這可確保未簽署的封裝無法偽造已簽署套件的身分識別。 |
| [**TargetDeviceFamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) | **MinVersion** 屬性必須指定10.0.19041.0 或更新版本的 OS 版本。 |
| [**uap10:HostRuntimeDependency**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntimedependency)  | 此元素元素會宣告主機應用程式封裝的相依性。 這包括主機封裝的 **名稱** 和 **發行者** ，以及它所相依的 **MinVersion** 。 這些值可在主機封裝中的 [Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 元素底下找到。 |
| [**Application**](/uwp/schemas/appxpackage/uapmanifestschema/element-application) | **Uap10： HostId** 屬性工作表示主機上的相依性。 裝載的應用程式套件必須宣告此屬性，而不是 [**應用程式**](/uwp/schemas/appxpackage/uapmanifestschema/element-application)或 [**延伸**](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension)模組專案的一般 **可執行檔** 和 **EntryPoint** 屬性。 因此，託管應用程式會從具有對應的 **HostId** 值的主機繼承 **可執行檔** 、 **EntryPoint** 和執行時間屬性。<br/><br/>**Uap10： Parameters** 屬性會指定傳遞至主機可執行檔之進入點函數的參數。 因為主機必須知道要如何處理這些參數，所以主機和託管應用程式之間會有隱含的合約。 |

## <a name="register-an-unsigned-hosted-app-package-at-run-time"></a>在執行時間註冊未簽署的託管應用程式套件

**Uap10： HostRuntime** 擴充功能的其中一個優點是，它可讓主機在執行時間動態產生託管的應用程式套件，並使用 [**>packagemanager**](/uwp/api/windows.management.deployment.packagemanager) API 來註冊它，而不需要加以簽署。 這可讓主機動態產生託管應用程式封裝的內容和資訊清單，然後進行註冊。

使用 [**>packagemanager**](/uwp/api/windows.management.deployment.packagemanager) 類別的下列方法，註冊未簽署的託管應用程式套件。 從 Windows 10 2004 版開始提供這些方法。

* [**AddPackageByUriAsync**](/uwp/api/windows.management.deployment.packagemanager.addpackagebyuriasync)：使用 *Options* 參數的 **AllowUnsigned** 屬性來註冊未簽署的 MSIX 封裝。
* [**RegisterPackageByUriAsync**](/uwp/api/windows.management.deployment.packagemanager.registerpackagebyuriasync)：執行鬆散的套件資訊清單檔案註冊。 如果封裝已簽署，則包含資訊清單的資料夾必須包含 [appxsignature.p7x](/windows/msix/overview#inside-an-msix-package) 檔案和目錄。 如果未簽署，則必須設定 *options* 參數的 **AllowUnsigned** 屬性。

### <a name="requirements-for-unsigned-hosted-apps"></a>未簽署的託管應用程式需求

* 封裝資訊清單中的 [**應用程式**](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 或 [**延伸**](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) 模組元素不能包含啟用資料，例如 **可執行檔** 、 **EntryPoint** 或 **TrustLevel** 屬性。 相反地，這些專案只能包含 **uap10： HostId** 屬性，以表示主機上的相依性和 **uap10： Parameters** 屬性。
* 封裝必須是主要套件。 它不能是配套、架構套件、資源或選用套件。

### <a name="requirements-for-a-host-that-installs-and-registers-an-unsigned-hosted-app-package"></a>安裝並註冊未簽署之託管應用程式套件的主機需求

* 主機必須有 [套件身分識別](#define-a-host)。
* 主機必須具有 **packageManagement** [限制的功能](../packaging/app-capability-declarations.md#restricted-capabilities)。
    ```xml
    <rescap:Capability Name="packageManagement" />
    ```

<!--
* If the host runs in app container (for example, it is a UWP app), it must also have the unsigned package management [custom capability](../packaging/app-capability-declarations.md#custom-capabilities) and a [Signed Custom Capability Descriptor (SCCD) file](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file).
    ```xml
    <uap4:CustomCapability Name="Microsoft.unsignedPackageManagement_cw5n1h2txyewy" />
    ```
-->

## <a name="sample"></a>範例

針對將本身宣告為主機，然後在執行時間動態註冊託管應用程式套件的完整功能範例應用程式，請參閱 [hosted app 範例](https://github.com/microsoft/AppModelSamples/tree/master/Samples/HostedApps)。

### <a name="the-host"></a>主機

主機名稱為 **PyScriptEngine** 。 這是以 c # 撰寫的包裝函式，可執行 python 腳本。 以參數執行時 `-Register` ，腳本引擎會安裝包含 python 腳本的託管應用程式。 當使用者嘗試啟動新安裝的託管應用程式時，主機會啟動並執行 **NumberGuesser** python 腳本。

主機應用程式的套件資訊清單 (PyScriptEnginePackage) 資料夾中的 package.appxmanifest 檔案，其中包含 **uap10： HostRuntime** 副檔名，可將應用程式宣告為識別碼 **PythonHost** 的主機和可執行檔 **PyScriptEngine.exe** 。  

> [!NOTE]
> 在此範例中，封裝資訊清單命名為 package.appxmanifest，它是 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)的一部分。 建立此專案時，它會 [產生名為 AppxManifest.xml的資訊清單 ](/uwp/schemas/appxpackage/uapmanifestschema/generate-package-manifest) ，並建立主機應用程式的 MSIX 套件。

### <a name="the-hosted-app"></a>託管應用程式

裝載的應用程式是由 python 腳本和套件構件（例如套件資訊清單）所組成。 它不包含任何 PE 檔案。

託管應用程式的套件資訊清單 (NumberGuesser/AppxManifest.xml 檔案) 包含下列專案：

* [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity)元素的 **發行者** 屬性包含 `OID.2.25.311729368913984317654407730594956997722=1` 不帶正負號封裝所需的識別碼。
* [**應用程式**](/uwp/schemas/appxpackage/uapmanifestschema/element-application)元素的 **uap10： HostId** 屬性將 **PythonHost** 識別為其主機。

### <a name="run-the-sample"></a>執行範例

此範例需要 Windows 10 的版本10.0.19041.0 或更新版本，以及 Windows SDK。

1. 將 [範例](https://aka.ms/hostedappsample) 下載至開發電腦上的資料夾。
2. 在 Visual Studio 中開啟 [PyScriptEngine] 方案，然後將 **PyScriptEnginePackage** 專案設定為啟始專案。
3. 建立 **PyScriptEnginePackage** 專案。
4. 在方案總管中，以滑鼠右鍵按一下 **PyScriptEnginePackage** 專案，然後選擇 [ **部署** ]。
5. 開啟命令提示字元視窗至您複製範例檔案的目錄，然後執行下列命令，將範例 **NumberGuesser** 應用程式註冊 (裝載的應用程式) 。 變更 `D:\repos\HostedApps` 為您複製範例檔案的路徑。

    ```CMD
    D:\repos\HostedApps>pyscriptengine -Register D:\repos\HostedApps\NumberGuesser\AppxManifest.xml
    ```

    > [!NOTE]
    > 您可以 `pyscriptengine` 在命令列上執行，因為範例中的主機會宣告 [**AppExecutionAlias**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias)。

6. 開啟 [ **開始** ] 功能表，然後按一下 [ **NumberGuesser** ] 以執行託管應用程式。
