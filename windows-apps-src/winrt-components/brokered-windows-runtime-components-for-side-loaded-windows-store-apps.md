---
title: 側載 UWP app 的代理 Windows 執行階段元件
description: 這份白皮書討論企業為目標的功能支援的 Windows 10，這可讓便於觸控的操作使用現有的程式碼負責重要的商務關鍵作業的.NET 應用程式。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: 24878d3c63de7df9c55f48571984b7d60d1ea240
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320365"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>側載 UWP app 的代理 Windows 執行階段元件

這篇文章討論企業為目標的功能支援的 Windows 10，這可讓便於觸控的操作使用現有的程式碼負責重要的商務關鍵作業的.NET 應用程式。

## <a name="introduction"></a>簡介

>**附註**  隨附這份文件的範例程式碼可能會下載用於 [Visual Studio 2015 和 2017年](https://aka.ms/brokeredsample)。 Microsoft Visual Studio 範本建置代理 Windows 執行階段元件可以在這裡下載：[以通用 Windows 應用程式適用於 Windows 10 為目標的 visual Studio 2015 範本](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

Windows 包含新功能，稱為 *側載應用程式的代理 Windows 執行階段元件*。 我們使用 IPC (處理程序間通訊) 一詞來說明在一個處理程序 (桌面元件) 執行現有桌面軟體資產，同時在 UWP App 中與此程式碼進行互動的能力。 這對企業開發人員來說是很熟悉的模型，因為資料庫應用程式和使用 Windows NT 服務的應用程式共用很類似的多個處理程序架構。

應用程式側載是這個功能很重要的元件。
企業特定應用程式在一般消費者 Microsoft Store 中沒有市場，而且公司對安全性、隱私權、發佈、設定和服務有非常特定的需求。 因此，側載模型不但是使用此功能之人員的必要項目，也是重要的實作詳細資料。

以資料為中心的應用程式是這個應用程式架構的主要對象。 例如，它預想隱藏在 SQL Server 的現有商業規則，將成為桌面元件很常見的一部分。 這當然不是桌面元件唯一可以提供的功能類型，但這個功能很大一部分需求與現有資料和商務邏輯有關。

最後，提供.NET 執行階段和 C 的非常龐大的滲透\#中企業應用程式開發，這項功能的語言開發使用.NET UWP 應用程式和桌面元件側邊，特別強調。 有可能會有其他語言和執行階段的 UWP 應用程式，而隨附的範例只會說明 C\#，而且僅限於.NET 執行階段。

## <a name="application-components"></a>應用程式元件

>**附註**  這項功能是專為.NET 使用。 用戶端應用程式和桌面元件都必須使用 .NET 撰寫。

**應用程式模型**

這個功能是以一般應用程式架構所建置，該架構稱為 MVVM (模型檢視檢視模型)。 因此，它假設「模型」完全位於桌面元件中。 所以，您可以立即確定桌面元件將會是「無周邊」(也就是，不包含 UI)。 檢視將會整個包含在側載企業應用程式中。 雖然此應用程式沒有規定一定要使用「檢視模型」建構，但我們預期以後這個模式將會很普遍的使用。

**桌面元件**

這個功能中的桌面元件是這個功能引進的新應用程式類型。 此桌面元件的類型只能以 C 撰寫\#和必須以.NET 4.6 或更新版本為目標的 Windows 10。 這個專案類型混合了以 UWP 為目標的 CLR，因為處理程序間通訊格式是由 UWP 類型和類別所組成，而且桌面元件可呼叫 .NET 執行階段類別庫的所有部分。 對 Visual Studio 專案產生的影響將會在稍後詳細討論。 這種混合式設定可在桌面元件上建置的應用程式間封送處理 UWP 類型，同時可以在桌面元件實作內呼叫桌面 CLR 程式碼。

**Contract**

我們將針對 UWP 類型系統說明側載應用程式和桌面元件間的協定。 這牽涉到宣告一或多個 C\#可以代表 UWP 的類別。 請參閱 MSDN 主題[Creating Windows Runtime Components in C\#和 Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br230301(v=vs.140))特定需求建立 Windows 執行階段類別使用 C\#。

>**附註**  Windows 執行階段元件之間的合約桌面元件和側載應用程式在此階段中不支援列舉。

**側載應用程式**

側載應用程式與一般 UWP app 一樣，除了一個地方不同：它使用側載而不是透過 Microsoft Store 安裝。 大部分的安裝機制都相同：資訊清單和應用程式封裝都很類似 (資訊清單還有一個相似處將在稍後詳細說明)。 一旦啟用側載，一個簡單的 PowerShell 指令碼就可以安裝所需的憑證和應用程式本身。 一般最佳做法是讓側載應用程式通過 Visual Studio [專案/市集] 功能表內含的 WACK 認證測試。

>**附註** 側載可以開啟在 [設定]-&gt;更新與安全性-&gt;適用於開發人員。

請特別注意，Windows 10 更新隨附的應用程式代理人機制只有 32 位元版本。 桌面元件必須是 32 位元。
側載應用程式可以是 64 位元 (前提是要同時登錄 64 位元和 32 位元 Proxy)，但這不常見。 建立側載應用程式，在 C 中\#自然使用一般的 「 中性 」 組態的"prefer 32 位元"的預設值建立 32 位元側載應用程式。

**伺服器執行個體和 AppDomains**

每個側載應用程式都會收到自己的應用程式代理人伺服器執行個體 (也就是「多重執行個體」)。 伺服器程式碼在單一 AppDomain 中執行。 這可讓多個版本的程式庫在獨立的執行個體中執行。 例如，應用程式 A 需要 V1.1 版本的元件，而應用程式 B 需要 V2 版本。 將 V1.1 版本和 V2 版本的元件放在獨立的伺服器目錄，然後把應用程式指向支援正確版本的伺服器，將這兩者完全分開。

把多個應用程式指向相同的伺服器目錄，可以在多個應用程式代理人伺服器執行個體間共用伺服器程式碼實作。 雖然仍然有多個應用程式代理人伺服器執行個體，但會執行相同的程式碼。 在單一應用程式使用的所有實作元件都應該在相同的路徑中。

## <a name="defining-the-contract"></a>定義協定

使用這個功能建立應用程式的第一個步驟是，建立側載應用程式和桌面元件間的協定。 這只能使用 Windows 執行階段類型來完成。
幸運的是，這些都很容易宣告使用 C\#類別。 不過，定義這些交談時，需要考量一些重要的效能問題，這會在稍後的章節中討論。

定義協定的順序如下所示：

**步驟 1:** Visual Studio 中建立新的類別庫。 請確定使用「類別程式庫」範本而非 Windows 執行階段元件範本建立專案

之後明顯會有實作，但本節只涵蓋定義處理程序間協定。 隨附的範例包含下列類別 (EnterpriseServer.cs)，雛型看起來像這樣：

```csharp
namespace Fabrikam
{
    public sealed class EnterpriseServer
    {

        public ILis<String> TestMethod(String input)
        {
            throw new NotImplementedException();
        }
        
        public IAsyncOperation<int> FindElementAsync(int input)
        {
            throw new NotImplementedException();
        }
        
        public string[] RetrieveData()
        {
            throw new NotImplementedException();
        }
        
        public event EventHandler<string> PeriodicEvent;
    }
}
```

這會定義可以從側載應用程式具現化的類別 "EnterpriseServer"。 這個類別提供 RuntimeClass 中承諾的功能。 RuntimeClass 可以用來產生側載應用程式中會包含的參考 winmd。

**步驟 2:** 編輯專案檔，以手動方式來將專案的輸出型別變更為 Windows 執行階段元件

若要在 Visual Studio 中這麼做，在剛建立的專案上按一下滑鼠右鍵並選取 [卸載專案]，然後再按一下滑鼠右鍵並選取 [編輯 EnterpriseServer.csproj] 以開啟專案檔案 (XML 檔案) 來進行編輯。

在開啟的檔案中，搜尋\<OutputType\>標記，並將其值變更為 「 winmdobj"。

**步驟 3：** 建立建置規則所建立的 「 參考 」 Windows 中繼資料檔案 （.winmd 檔案）。 也就是沒有實作。

**步驟 4：** 建立會建立一個 「 實作 」 的 Windows 中繼資料檔，也就是具有相同的中繼資料資訊，但也包含實作的建置規則。

這會由下列指令碼完成。 在專案中的**Properties** > **Build Events**，將指令碼新增到建置後事件命令列

> **注意** 根據您所針對的 Windows 版本 (Windows 10) 以及使用中的 Visual Studio 版本，指令碼會有所不同。

**Visual Studio 2015**
```cmd
    call "$(DevEnvDir)..\..\vc\vcvarsall.bat" x86 10.0.14393.0

    md "$(TargetDir)"\impl    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h   "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"

```


**Visual Studio 2017**
```cmd
    call "$(DevEnvDir)..\..\vc\auxiliary\build\vcvarsall.bat" x86 10.0.16299.0

    md "$(TargetDir)"\impl
    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"
```

參考 **winmd** 建立後 (在專案的 [目標] 資料夾底下的 [參考] 資料夾中)，會帶到 (複製到) 每個使用的側載應用程式專案並加以參考。 下節會有更詳盡的說明。 上述建置規則內含的專案結構可確保實作和參考 **winmd** 會位在建置階層完全不同的目錄中，以避免混淆。

## <a name="side-loaded-applications-in-detail"></a>側載應用程式詳細資料
如上所述，側載應用程式的建置方式與任何其他 UWP app 相同，但還有一個額外的細節：在側載應用程式資訊清單中宣告 RuntimeClass 的可用性。 這可讓應用程式只要寫新項目就能存取桌面元件中的功能。 <Extension> 區段中的新資訊清單項目說明桌面元件中實作的 RuntimeClass，以及其所在位置的資訊。 在應用程式資訊清單中的這些宣告內容與針對 Windows 10 的 App 是相同的。 例如:

```XML
<Extension Category="windows.activatableClass.inProcessServer">
    <InProcessServer>
        <Path>clrhost.dll</Path>
        <ActivatableClass ActivatableClassId="Fabrikam.EnterpriseServer" ThreadingModel="both">
            <ActivatableClassAttribute Name="DesktopApplicationPath" Type="string" Value="c:\test" />
        </ActivatableClass>
    </InProcessServer>
</Extension>
```

類別是 inProcessServer，因為 outOfProcessServer 類別中有多個項目不適用於這個應用程式設定。 請注意，<Path>元件必須永遠包含 clrhost.dll (不過這是 **不** 強制執行，並指定不同的值未定義的方式將會失敗)。

<ActivatableClass> 區段會和應用程式套件中 Windows 執行階段元件偏好的真正同處理序 RuntimeClass 相同。 <ActivatableClassAttribute> 是新的元素和屬性名稱 ="DesktopApplicationPath"且類型 ="string"會強制和非變異值。 值屬性指向桌面元件實作 winmd 所在的位置 (下節會有更詳盡的資訊)。 桌面元件偏好的每個 RuntimeClass 都應該有自己的 <ActivatableClass> 元素樹狀結構。 ActivatableClassId 必須符合 RuntimeClass 的完整命名空間名稱。

如＜定義協定＞一節所述，必須將專案參考連接到桌面元件參考 winmd。 Visual Studio 專案系統通常會建立相同名稱的兩層目錄結構。 在此範例中，它是 EnterpriseIPCApplication\\EnterpriseIPCApplication。 參考**winmd** 手動複製到這個第二個層級目錄，然後使用對話方塊中的專案參考 (按一下 **瀏覽...**   按鈕) 來找出並參考此**winmd**。 之後，桌面元件的最上層命名空間 (例如 Fabrikam) 應該在專案參考部分顯示為最上層節點。

>**附註**一定要使用 **參考 winmd** 側載應用程式中。 如果您不小心會延續 **實作 winmd** 側載應用程式目錄和參考，您可能會收到 「 找不到 IStringable 」 相關的錯誤。 這是其中一個確定登入的錯誤 **winmd** 被參考。 在 IPC 伺服器應用程式 （在下一節中詳述） 仔細的建置後規則區隔這些兩個 **winmd** 分割成個別的目錄。

環境變數 （尤其是 %programfiles%)可用於<ActivatableClassAttribute Value="path">。如先前所述，應用程式訊息代理程式僅支援 32 位元因此 %programfiles%會解析為 c:\\Program Files (x86) 如果應用程式在 64 位元作業系統上執行。

## <a name="desktop-ipc-server-detail"></a>桌面 IPC 伺服器詳細資料

先前的兩個區段描述類別的宣告和傳輸參考的運作原理 **winmd** 側載應用程式專案。 剩下的大量桌面元件工作則牽涉到實作。 由於使用桌面元件的目的就是要能呼叫桌面程式碼 (通常用來重複使用現有的程式碼資產)，因此專案必須以特別的方式設定。
一般而言，使用 .NET 的 Visual Studio 專案使用兩種「設定檔」的其中一種。
一個用在桌面 (".NetFramework")，另一個用在 CLR (".NetCore") 的 UWP app 部分。 這個功能的桌面元件是這兩種的混合。 因此，建構參考區段時非常謹慎，以便將這兩種設定檔融合在一起。

一般 UWP app 專案不含明確的專案參考，因為隱含整個 Windows 執行個體 API 表面。
通常只會進行其他專案間參考。 不過，桌面元件專案有一個非常特別的參考集。 它會啟動身為 「 傳統桌面\\類別庫 」 專案，因此桌面專案。 因此明確參考 Windows 執行階段 API (透過參考 **winmd** 檔案) 必須設定為。 新增適當的參考，如下所示。

```XML
<ItemGroup>
    <!-- These reference are added by VS automatically when you create a Class Library project-->
    <Reference Include="System" />
    <Reference Include="System.Core" />
    <Reference Include="System.Xml.Linq" />
    <Reference Include="System.Data.DataSetExtensions" />
    <Reference Include="Microsoft.CSharp" />
    <Reference Include="System.Data" />
    <Reference Include="System.Net.Http" />
<Reference Include="System.Xml" />
    <!-- These reference should be added manually by editing .csproj file-->

    <Reference Include="System.Runtime.WindowsRuntime, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089, processorArchitecture=MSIL">
      <HintPath>$(MSBuildProgramFiles32)\Microsoft SDKs\NETCoreSDK\System.Runtime.WindowsRuntime\4.0.10\lib\netcore50\System.Runtime.WindowsRuntime.dll</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\UnionMetadata\Facade\Windows.WinMD</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.FoundationContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.FoundationContract\1.0.0.0\Windows.Foundation.FoundationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.UniversalApiContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\1.0.0.0\Windows.Foundation.UniversalApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Connectivity.WwanContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Connectivity.WwanContract\1.0.0.0\Windows.Networking.Connectivity.WwanContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivationCameraSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ContactActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ContactActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ContactActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract\1.0.0.0\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Calls.LockScreenCallContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Calls.LockScreenCallContract\1.0.0.0\Windows.ApplicationModel.Calls.LockScreenCallContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Resources.Management.ResourceIndexerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract\1.0.0.0\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.Core.SearchCoreContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.Core.SearchCoreContract\1.0.0.0\Windows.ApplicationModel.Search.Core.SearchCoreContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.SearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.SearchContract\1.0.0.0\Windows.ApplicationModel.Search.SearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Wallet.WalletContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Wallet.WalletContract\1.0.0.0\Windows.ApplicationModel.Wallet.WalletContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Custom.CustomDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Custom.CustomDeviceContract\1.0.0.0\Windows.Devices.Custom.CustomDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Portable.PortableDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Portable.PortableDeviceContract\1.0.0.0\Windows.Devices.Portable.PortableDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.Extensions.ExtensionsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.Extensions.ExtensionsContract\1.0.0.0\Windows.Devices.Printers.Extensions.ExtensionsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.PrintersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.PrintersContract\1.0.0.0\Windows.Devices.Printers.PrintersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Scanners.ScannerDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Scanners.ScannerDeviceContract\1.0.0.0\Windows.Devices.Scanners.ScannerDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Sms.LegacySmsApiContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Sms.LegacySmsApiContract\1.0.0.0\Windows.Devices.Sms.LegacySmsApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Gaming.Preview.GamesEnumerationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Gaming.Preview.GamesEnumerationContract\1.0.0.0\Windows.Gaming.Preview.GamesEnumerationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract\1.0.0.0\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Graphics.Printing3D.Printing3DContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Graphics.Printing3D.Printing3DContract\1.0.0.0\Windows.Graphics.Printing3D.Printing3DContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Deployment.Preview.DeploymentPreviewContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Deployment.Preview.DeploymentPreviewContract\1.0.0.0\Windows.Management.Deployment.Preview.DeploymentPreviewContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Workplace.WorkplaceSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Workplace.WorkplaceSettingsContract\1.0.0.0\Windows.Management.Workplace.WorkplaceSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.AppCaptureContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.AppCaptureContract\1.0.0.0\Windows.Media.Capture.AppCaptureContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.CameraCaptureUIContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.CameraCaptureUIContract\1.0.0.0\Windows.Media.Capture.CameraCaptureUIContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Devices.CallControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Devices.CallControlContract\1.0.0.0\Windows.Media.Devices.CallControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.MediaControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.MediaControlContract\1.0.0.0\Windows.Media.MediaControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Playlists.PlaylistsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Playlists.PlaylistsContract\1.0.0.0\Windows.Media.Playlists.PlaylistsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Protection.ProtectionRenewalContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Protection.ProtectionRenewalContract\1.0.0.0\Windows.Media.Protection.ProtectionRenewalContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract\1.0.0.0\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Sockets.ControlChannelTriggerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Sockets.ControlChannelTriggerContract\1.0.0.0\Windows.Networking.Sockets.ControlChannelTriggerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.EnterpriseData.EnterpriseDataContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.EnterpriseData.EnterpriseDataContract\1.0.0.0\Windows.Security.EnterpriseData.EnterpriseDataContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.ExchangeActiveSyncProvisioning.EasContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.ExchangeActiveSyncProvisioning.EasContract\1.0.0.0\Windows.Security.ExchangeActiveSyncProvisioning.EasContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.GuidanceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.GuidanceContract\1.0.0.0\Windows.Services.Maps.GuidanceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.LocalSearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.LocalSearchContract\1.0.0.0\Windows.Services.Maps.LocalSearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.SystemManufacturers.SystemManufacturersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract\1.0.0.0\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileHardwareTokenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileHardwareTokenContract\1.0.0.0\Windows.System.Profile.ProfileHardwareTokenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileRetailInfoContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileRetailInfoContract\1.0.0.0\Windows.System.Profile.ProfileRetailInfoContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileContract\1.0.0.0\Windows.System.UserProfile.UserProfileContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileLockScreenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileLockScreenContract\1.0.0.0\Windows.System.UserProfile.UserProfileLockScreenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.ApplicationSettings.ApplicationsSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.ApplicationSettings.ApplicationsSettingsContract\1.0.0.0\Windows.UI.ApplicationSettings.ApplicationsSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.AnimationMetrics.AnimationMetricsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract\1.0.0.0\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.CoreWindowDialogsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.CoreWindowDialogsContract\1.0.0.0\Windows.UI.Core.CoreWindowDialogsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Xaml.Hosting.HostingContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Xaml.Hosting.HostingContract\1.0.0.0\Windows.UI.Xaml.Hosting.HostingContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Web.Http.Diagnostics.HttpDiagnosticsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract\1.0.0.0\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
</ItemGroup>
```

上述參考非常謹慎地混合參考，對此混合式伺服器的正常運作非常重要。 通訊協定就是用來開啟 .csproj 檔案 (就像如何編輯專案 OutputType 中所述)，然後視需要新增這些參考。

正確設定參考後，下一個工作是實作伺服器功能。 請參閱 MSDN 主題 [與 Windows 執行階段元件的互通性的最佳做法 (使用 C 的 UWP 應用程式\#/VB/C++和 XAML)](https://docs.microsoft.com/previous-versions/windows/apps/hh750311(v=win.10))。
這項工作要建立 Windows 執行階段元件 dll，以便在實作時呼叫桌面程式碼。 隨附的範例包含 Windows 執行階段中使用的主要模式：

-   方法呼叫

-   桌面元件取得的 Windows 執行階段事件來源

-   Windows 執行階段非同步作業

-   傳回基本類型的陣列

**安裝**

若要安裝應用程式，複製 實作 **winmd** 相關聯的側載應用程式的資訊清單中指定正確的目錄：<ActivatableClassAttribute>的值 = 「 路徑 」。 同時，複製所有相關支援檔案及 proxy/stub dll (下方將涵蓋後者的詳細資料)。 無法複製實作 **winmd** 伺服器目錄位置，就會造成所有側載應用程式的呼叫上 RuntimeClass 新增將會擲回 「 類別未登錄 」 的錯誤。 無法安裝 proxy/stub (或無法登錄) 將導致所有呼叫失敗，且不會傳回值。 此第二個錯誤是經常 **未** 可見的例外狀況相關聯。
如果因這個設定錯誤發生例外狀況，可能會以「無效的轉型」表示。

**伺服器實作考量**

桌面 Windows 執行階段伺服器可以想像成 "worker" 或 "task" 型。 傳入伺服器的每個呼叫會在非 UI 執行緒運作，且所有程式碼必須是多執行緒感知且是安全的。 側載應用程式中哪個部分呼叫伺服器功能也很重要。 在側載應用程式中務必一律禁止從任何 UI 執行緒呼叫長時間執行程式碼。 有兩個主要方法可以達到這個目的：

1.  如果從 UI 執行緒呼叫伺服器功能，一律使用伺服器公用介面區和實作中的非同步模式。

2.  在側載應用程式中，從背景執行緒呼叫伺服器功能。

**在伺服器中的 Windows 執行階段非同步**

由於應用程式模型的跨處理程序特性，伺服器呼叫比使用只在同處理程序執行的程式碼負荷更重。 通常呼叫傳回記憶體值的簡單屬性很安全，因為執行速度夠快，所以不會有阻擋 UI 執行緒的問題。 不過，所有涉及任何 I/O 形式的呼叫 (這包含所有檔案處理和資料庫擷取) 都可能阻擋呼叫 UI 執行緒，導致應用程式因無回應而終止。 此外，基於效能的原因，不建議在這個應用程式架構呼叫物件上的屬性。
下節將針對這個部分提供更詳盡的說明。

正確實作的伺服器通常會實作透過 Windows 執行階段非同步模式直接從 UI 執行緒進行的呼叫。 依照下列模式進行，可實作這個作業。 首先，宣告 (還是隨附的範例)：

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

這會宣告傳回整數的 Windows 執行階段非同步作業。
非同步作業的實作通常以下列形式進行：

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**注意** 寫入實作時，等待一些其他可能的長時間執行作業是很常見的。 如果是的話 **Task.Run** 的程式碼需要宣告：

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

這個非同步方法的用戶端可像等待任何其他 Windows 執行階段非同步作業一樣等待這個作業。

**從應用程式的背景執行緒呼叫伺服器功能**

由於用戶端和伺服器通常都是由同一個組織所撰寫，因此可以採用一種程式設計做法，也就是從側載應用程式中的背景執行緒進行所有伺服器呼叫。 背景執行緒可以發出從伺服器收集一或多個資料批次的直接呼叫。 擷取全部結果後，應用程式處理程序記憶體中的資料批次通常可以直接從 UI 執行緒擷取。 C\#物件自然敏捷式軟體開發背景執行緒之間，而且讓 UI 執行緒會特別適用於這種呼叫模式。

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>建立和部署 Windows 執行階段 Proxy

由於 IPC 方法涉及在這兩個處理程序間封送處理 Windows 執行階段介面，因此必須使用全域登錄的 Windows 執行階段 Proxy 和虛設常式。

**在 Visual Studio 中建立 proxy**

建立和註冊 proxy 和虛設常式是一般的 UWP 應用程式套件內使用的程序詳述於本主題 [在 Windows 執行階段元件中引發事件](https://docs.microsoft.com/previous-versions/windows/apps/dn169426(v=vs.140))。
本文所述的步驟比下方說明的處理程序更加複雜，因為它涉及在應用程式套件內登錄 Proxy/虛設常式 (與全域登錄不同)。

**步驟 1:** 使用桌面元件專案的方案，Visual Studio 中建立的 Proxy/Stub 專案：

**解決方案 > 新增 > 專案 > Visual C++ > Win32 主控台選取 [DLL] 選項。**

下列步驟中，我們假設的伺服器元件稱為 **MyWinRTComponent**。

**步驟 3：** 從專案刪除所有 CPP/H 檔案。

**步驟 4：** 上一節 「 定義合約 」 包含所要執行建置後命令 **winmdidl.exe**， **midl.exe**， **mdmerge.exe**，依此類推。 這個建置後命令的其中一個 midl 步驟輸出會產生四個重要的輸出：

a) Dlldata.c

b) 一個標頭檔 (例如 MyWinRTComponent.h)

c） 的\* \_i.c 檔案 (例如 MyWinRTComponent\_i.c)

d） 的\* \_p.c 檔案 (例如 MyWinRTComponent\_p.c)

**步驟 5：** 將這四個產生的檔案新增至"MyWinRTProxy 」 專案中。

**步驟 6：** 將.def 檔案新增至 「 MyWinRTProxy 」 專案 **(專案 > 加入新項目 > 程式碼 > 模組定義檔**) 並更新為內容：

LIBRARY MyWinRTComponent.Proxies.dll

EXPORTS

DllCanUnloadNow PRIVATE

DllGetClassObject PRIVATE

DllRegisterServer PRIVATE

DllUnregisterServer PRIVATE

**步驟 7：** 開啟 「 MyWinRTProxy 」 專案的屬性：

**設定屬性 > 一般 > 目標名稱：**

MyWinRTComponent.Proxies

**C /C++ > 前置處理器定義 > 新增**

"WIN32;\_WINDOWS;REGISTER\_PROXY\_DLL"

**C /C++ > 先行編譯標頭：選取 [不使用先行編譯標頭]**

**連結器 > 一般 > 忽略匯入程式庫：選取 [是]**

**連結器 > 輸入 > 其他相依性：新增 rpcrt4.lib;runtimeobject.lib**

**連結器 > Windows 中繼資料 > 產生 Windows 中繼資料：Select "No"**

**步驟 8:** 建置 「 MyWinRTProxy 」 專案。

**部署 proxy**

必須全域登錄此 Proxy。 執行這個作業最簡單的方法就是讓您的安裝處理程序呼叫 Proxy dll 上的 DllRegisterServer。 請注意，由於這個功能只支援 x86 的伺服器組建 (也就是，不支援 64 位元)，最簡單的設定是使用 32 位元伺服器、32 位元 Proxy 和 32 位元側載應用程式。 Proxy 通常會與桌面元件的實作 **winmd** 放在一起。

還有一個額外的設定步驟要執行。 為了讓側載處理程序載入和執行 Proxy，目錄必須將 ALL_APPLICATION_PACKAGES 標記為 "read / execute"。 這要透過 **icacls.exe** 命令列工具完成。 這個命令應該在實作 **winmd** 和 Proxy/虛設常式 dll 所在的目錄中執行：

*icacls。/T /grant \*S-1-15-2-1:RX*

## <a name="patterns-and-performance"></a>模式和效能

仔細監視跨處理程序傳輸的效能非常重要。 跨處理程序呼叫的成本至少是同處理程序呼叫的兩倍。 建立 "chatty" 交談跨處理程序或執行大型物件 (如點陣圖) 的重複傳輸會導致未預期和不佳的應用程式效能。

下列為需考量的簡短清單：

-   應一律避免從應用程式 UI 執行緒到伺服器的同步方法呼叫。 從應用程式的背景執行緒呼叫方法，然後視需要使用 CoreWindowDispatcher 將結果傳到 UI 執行緒。

-   從應用程式 UI 執行緒呼叫非同步作業很安全，但要考慮以下討論的效能問題。

-   大量傳輸結果會降低跨處理程序的交談功能。 這通常是使用 Windows 執行階段陣列建構來執行。

-   傳回 *清單<T>*  位置 *T* 是來自非同步作業或屬性擷取的物件，會導致大量的跨處理序的對話。 例如，假設您返回*清單&lt;人&gt;*  物件。 每個反覆運算傳輸都是一個跨處理程序呼叫。 每個 *人* 傳回的物件由 proxy 和每個呼叫的方法，或在該個別物件上的屬性將會導致跨處理序呼叫。 因此 「 無害 」 *清單&lt;人&gt;*  物件位置 *計數* 大會導致大量的慢速呼叫。 大量傳輸陣列中的內容結構會產生較佳的效能。 例如:

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

然後傳回 * PersonStruct\[\]* 而非*清單&lt;PersonObject&gt;* 。
這會在一個跨處理程序 "hop" 取得所有的資料。

與所有效能考量一樣，測量和測試非常重要。 在理想的情況下，應該將遙測插入各種作業以判斷作業所花的時間。 務必要測量一整個範圍：例如，側載應用程式中的特定查詢實際上花了多少時間使用所有的 *People* 物件？

另一個技巧是可變載入測試。 您可以將效能測試攔截放入應用程式，將可變延遲載入引進伺服器處理來完成此作業。 這可以模擬各種載入，以及應用程式對不同伺服器效能的反應。
此範例說明如何使用適當的非同步技術將時間延遲放入程式碼。 要插入的實際延遲時間及要放入該人工載入的隨機範圍會因應用程式設計和應用程式預期執行環境而有所不同。

## <a name="development-process"></a>開發處理序

當您變更伺服器時，必須確定所有先前執行中的執行個體已不再執行。 COM 最終將清除這個處理序，但是取消計時器的時間會比反覆開發的有效時間還長。 因此，刪除先前執行中的執行個體是開發期間的一個標準步驟。 這需要開發人員持續追蹤哪一個 dllhost 執行個體正在裝載伺服器。

您可以使用 [工作管理員] 或其他協力廠商應用程式，找出伺服器處理序並予以刪除。 命令列工具 **TaskList.exe **也會併入及有彈性的語法，例如：

  
 | **命令** | **動作** |
 | ------------| ---------- |
 | Tasklist | 依建立時間的大致順序列出所有執行中的處理序，最近建立的處理序會列於底部附近。 |
 | tasklist /FI "IMAGENAME eq dllhost.exe" /M | 列出所有 dllhost.exe 執行個體的相關資訊。 /M 參數會列出已載入的模組。 |
 | tasklist /FI "PID eq 12564" /M | 如果您知道 dllhost.exe 的 PID，就可使用這個選項來加以查詢。 |

代理人伺服器的模組清單應該會列出 *clrhost.dll* 載入模組的清單中。

## <a name="resources"></a>資源

-   [適用於 Windows 10 和 VS 2015 的代理的 WinRT 元件專案範本](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

-   [NorthwindRT 代理的 WinRT 元件範例](https://go.microsoft.com/fwlink/p/?LinkID=397349)

-   [提供可靠且值得信賴的 Microsoft Store 應用程式](https://go.microsoft.com/fwlink/p/?LinkID=393644)

-   [應用程式協定與延伸 （Windows 市集應用程式）](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))

-   [如何在 Windows 10 的側載應用程式](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)

-   [UWP 應用程式部署到企業](https://go.microsoft.com/fwlink/p/?LinkID=264770)

