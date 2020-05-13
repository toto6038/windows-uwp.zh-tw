---
title: 適用于側載 UWP 應用程式的代理 Windows 執行階段元件
description: 本文件討論 Windows 10 支援的以企業為目標的功能，此功能可讓方便觸控的 .NET 應用程式使用負責重要業務關鍵作業的現有程式碼。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: a3e95eae10fb06135f0fed1b92f1717f5e5fdf4d
ms.sourcegitcommit: 0f2ae8f97daac440c8e86dc07d11d356de29515c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/13/2020
ms.locfileid: "83280278"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>適用于側載 UWP 應用程式的代理 Windows 執行階段元件

本文章討論 Windows 10 所支援的企業導向功能，該功能允許方便觸控的 .NET 應用程式可使用負責重要業務關鍵作業的現有程式碼。

## <a name="introduction"></a>簡介

>**注意**  本文隨附的範例程式碼可能會下載 [Visual Studio 2015 & 2017](https://github.com/Microsoft/Brokered-WinRT-Components)。 用來建置代理 Windows 執行階段元件的 Microsoft Visual Studio 範本可以於此處下載：[以適用於 Windows 10 的通用 Windows app 為目標的 Visual Studio 2015 範本](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

Windows 包含了一個新功能，稱為*適用於側載應用程式的代理 Windows 執行階段元件*。 我們使用 IPC (處理程序間通訊) 一詞來說明在一個處理程序 (桌面元件) 執行現有桌面軟體資產，同時在 UWP App 中與此程式碼進行互動的能力。 這對企業開發人員來說是很熟悉的模型，因為資料庫應用程式和使用 Windows NT 服務的應用程式共用很類似的多個處理程序架構。

應用程式側載是這個功能很重要的元件。
企業特定應用程式在一般消費者 Microsoft Store 中沒有市場，而且公司對安全性、隱私權、發佈、設定和服務有非常特定的需求。 因此，側載模型不但是使用此功能之人員的必要項目，也是重要的實作詳細資料。

以資料為中心的應用程式是這個應用程式架構的主要對象。 例如，它預想隱藏在 SQL Server 的現有商業規則，將成為桌面元件很常見的一部分。 這當然不是桌面元件唯一可以提供的功能類型，但這個功能很大一部分需求與現有資料和商務邏輯有關。

最後，由於 .NET 運行 \# 時間和企業開發中的 C 語言有巨大的滲透，因此這項功能的開發重點在於 UWP 應用程式和桌面元件端都使用 .net。 雖然 UWP 應用程式有可能的其他語言和執行時間，隨附的範例只會說明 C \# ，而且僅限於 .net 執行時間。

## <a name="application-components"></a>應用程式元件

>**注意**  這項功能僅適用于使用 .NET。 用戶端應用程式和桌面元件都必須使用 .NET 撰寫。

**應用程式模型**

這個功能是以一般應用程式架構所建置，該架構稱為 MVVM (模型檢視檢視模型)。 因此，它假設「模型」完全位於桌面元件中。 所以，您可以立即確定桌面元件將會是「無周邊」(也就是，不包含 UI)。 檢視將會整個包含在側載企業應用程式中。 雖然此應用程式沒有規定一定要使用「檢視模型」建構，但我們預期以後這個模式將會很普遍的使用。

**桌面元件**

這個功能中的桌面元件是這個功能引進的新應用程式類型。 這個桌面元件只能以 C 撰寫 \# ，而且必須以適用于 Windows 10 的 .net 4.6 或更新版本為目標。 這個專案類型混合了以 UWP 為目標的 CLR，因為處理程序間通訊格式是由 UWP 類型和類別所組成，而且桌面元件可呼叫 .NET 執行階段類別庫的所有部分。 對 Visual Studio 專案產生的影響將會在稍後詳細討論。 這種混合式設定可在桌面元件上建置的應用程式間封送處理 UWP 類型，同時可以在桌面元件實作內呼叫桌面 CLR 程式碼。

**協定**

我們將針對 UWP 類型系統說明側載應用程式和桌面元件間的協定。 這牽涉到宣告一個或多個 \# 可代表 UWP 的 C 類別。 如需使用 C 建立 Windows 執行階段類別的特定需求，請參閱 MSDN 主題[建立 c 的 Windows 執行階段元件 \# 和 Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br230301(v=vs.140)) \# 。

>**注意**  桌面元件和側載應用程式之間的 Windows 執行階段元件合約中，目前不支援列舉。

**側載應用程式**

側載應用程式與一般 UWP app 一樣，除了一個地方不同：它使用側載而不是透過 Microsoft Store 安裝。 大部分的安裝機制都相同：資訊清單和應用程式封裝都很類似 (資訊清單還有一個相似處將在稍後詳細說明)。 一旦啟用側載，一個簡單的 PowerShell 指令碼就可以安裝所需的憑證和應用程式本身。 一般最佳做法是讓側載應用程式通過 Visual Studio [專案/市集] 功能表內含的 WACK 認證測試。

>**注意** 側載可以在 [設定] -&gt; [更新與安全性] -&gt; [適用於開發人員] 中開啟。

請特別注意，Windows 10 更新隨附的應用程式代理人機制只有 32 位元版本。 桌面元件必須是 32 位元。
側載應用程式可以是 64 位元 (前提是要同時登錄 64 位元和 32 位元 Proxy)，但這不常見。 使用一般的「中性」設定和「慣用32位」預設值，在 C 中建立側載應用程式 \# ，自然會建立32位的端載入應用程式。

**伺服器執行個體和 AppDomain**

每個側載應用程式都會收到自己的應用程式代理人伺服器執行個體 (也就是「多重執行個體」)。 伺服器程式碼在單一 AppDomain 中執行。 這可讓多個版本的程式庫在獨立的執行個體中執行。 例如，應用程式 A 需要 V1.1 版本的元件，而應用程式 B 需要 V2 版本。 將 V1.1 版本和 V2 版本的元件放在獨立的伺服器目錄，然後把應用程式指向支援正確版本的伺服器，將這兩者完全分開。

把多個應用程式指向相同的伺服器目錄，可以在多個應用程式代理人伺服器執行個體間共用伺服器程式碼實作。 雖然仍然有多個應用程式代理人伺服器執行個體，但會執行相同的程式碼。 在單一應用程式使用的所有實作元件都應該在相同的路徑中。

## <a name="defining-the-contract"></a>定義協定

使用這個功能建立應用程式的第一個步驟是，建立側載應用程式和桌面元件間的協定。 這只能使用 Windows 執行階段類型來完成。
幸運的是，使用 C 類別來宣告這些是很容易的 \# 。 不過，定義這些交談時，需要考量一些重要的效能問題，這會在稍後的章節中討論。

定義協定的順序如下所示：

**步驟 1：** 在 Visual Studio 中建立新類別程式庫。 請務必使用 [**類別庫**] 範本，而不是 [ **Windows 執行階段元件**] 範本來建立專案。

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

**步驟2：** 手動編輯專案檔，將專案的輸出類型變更為**Windows 執行階段元件**。

若要在 Visual Studio 中這麼做，在剛建立的專案上按一下滑鼠右鍵並選取 [卸載專案]，然後再按一下滑鼠右鍵並選取 [編輯 EnterpriseServer.csproj] 以開啟專案檔案 (XML 檔案) 來進行編輯。

在開啟的檔案中，搜尋 \< OutputType \> 標記，並將其值變更為 "winmdobj"。

**步驟 3：** 建立建置規則，以建立 "reference" Windows 中繼資料檔案 (.winmd 檔案)。 也就是沒有實作。

**步驟 4：** 建立建置規則，以建立 "implementation" Windows 中繼資料檔案，也就是有相同的中繼資料資訊，但也包含實作。

這會由下列指令碼完成。 將腳本加入至 [專案**屬性**] [  >  **建立事件**] 中的 [建立後事件] 命令列。

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
如上所述，側載應用程式的建置方式與任何其他 UWP app 相同，但還有一個額外的細節：在側載應用程式資訊清單中宣告 RuntimeClass 的可用性。 這可讓應用程式只要寫新項目就能存取桌面元件中的功能。 <Extension> 區段中的新資訊清單項目說明桌面元件中實作的 RuntimeClass，以及其所在位置的資訊。 在應用程式資訊清單中的這些宣告內容與針對 Windows 10 的 App 是相同的。 例如：

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

類別是 inProcessServer，因為 outOfProcessServer 類別中有多個項目不適用於這個應用程式設定。 請注意，<Path> 元件一定要包含 clrhost.dll (不過，這**不是**強制性的，且指定不同的值將會以未定義的方式失敗)。

<ActivatableClass> 區段會和應用程式套件中 Windows 執行階段元件偏好的真正同處理序 RuntimeClass 相同。 <ActivatableClassAttribute>是新的專案，且屬性 Name = "DesktopApplicationPath" 和 Type = "string" 是強制性和不變的。 值屬性指向桌面元件實作 winmd 所在的位置 (下節會有更詳盡的資訊)。 桌面元件偏好的每個 RuntimeClass 都應該有自己的 <ActivatableClass> 元素樹狀結構。 ActivatableClassId 必須符合 RuntimeClass 的完整命名空間名稱。

如＜定義協定＞一節所述，必須將專案參考連接到桌面元件參考 winmd。 Visual Studio 專案系統通常會建立相同名稱的兩層目錄結構。 在範例中，它是 EnterpriseIPCApplication \\ EnterpriseIPCApplication。 手動將參考 **winmd** 複製到第二層目錄，然後使用 [專案參考] 對話方塊 (按一下 **\[瀏覽..\]** 按鈕) 尋找並參考此 **winmd**。 在此之後，桌面元件（例如 Fabrikam）的最上層命名空間應該會在專案的 [參考] 部分中顯示為最上層節點。

>**注意** 在側載應用程式中使用 **reference winmd** 非常重要。 如果您不小心將 **implementation winmd** 帶到側載應用程式目錄並參考它，很可能會收到與「找不到 IStringable」相關的錯誤。 這是參考錯誤 **winmd** 的明顯指標。 IPC 伺服器應用程式的建置後規則 (下節會有詳細說明) 很謹慎地將這兩個 **winmd** 隔離在兩個獨立的目錄中。

環境變數（尤其是% ProgramFiles%）可以在中使用 <ActivatableClassAttribute Value="path"> 。如先前所述，應用程式代理人僅支援32位，因此 \\ ，如果應用程式是在64位作業系統上執行，% ProgramFiles% 就會解析成 C： Program Files （x86）。

## <a name="desktop-ipc-server-detail"></a>桌面 IPC 伺服器詳細資料

上面兩節說明類別宣告，以及將參考 **winmd** 傳輸到側載應用程式專案的機制。 剩下的大量桌面元件工作則牽涉到實作。 由於使用桌面元件的目的就是要能呼叫桌面程式碼 (通常用來重複使用現有的程式碼資產)，因此專案必須以特別的方式設定。
一般而言，使用 .NET 的 Visual Studio 專案使用兩種「設定檔」的其中一種。
一個用在桌面 (".NetFramework")，另一個用在 CLR (".NetCore") 的 UWP app 部分。 這個功能的桌面元件是這兩種的混合。 因此，建構參考區段時非常謹慎，以便將這兩種設定檔融合在一起。

一般 UWP app 專案不含明確的專案參考，因為隱含整個 Windows 執行個體 API 表面。
通常只會進行其他專案間參考。 不過，桌面元件專案有一個非常特別的參考集。 它會以「傳統桌面類別庫」專案的形式啟動 \\ ，因此是桌面專案。 所以，必須明確參考 Windows 執行階段 API (透過參考 **winmd** 檔案)。 新增適當的參考，如下所示。

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

正確設定參考後，下一個工作是實作伺服器功能。 請參閱 [與 Windows 執行階段元件互通的最佳做法主題（使用 C \# /VB/C + + 和 XAML 的 UWP 應用程式）](https://docs.microsoft.com/previous-versions/windows/apps/hh750311(v=win.10))。
這項工作要建立 Windows 執行階段元件 dll，以便在實作時呼叫桌面程式碼。 隨附的範例包含 Windows 執行階段中使用的主要模式：

-   方法呼叫

-   桌面元件取得的 Windows 執行階段事件來源

-   Windows 執行階段非同步作業

-   傳回基本類型的陣列

**安裝**

若要安裝應用程式，請將實作 **winmd** 複製到相關側載應用程式資訊清單指定的正確目錄：<ActivatableClassAttribute> 值="path"。 同時，複製所有相關支援檔案及 proxy/stub dll (下方將涵蓋後者的詳細資料)。 如果沒有將實作 **winmd** 複製到伺服器目錄位置，會導致所有側載應用程式呼叫變成新的，且 RuntimeClass 會擲回「類別未登錄」錯誤。 無法安裝 proxy/stub (或無法登錄) 將導致所有呼叫失敗，且不會傳回值。 後者的錯誤通常與可見例外狀況**無關**。
如果因這個設定錯誤發生例外狀況，可能會以「無效的轉型」表示。

**伺服器實作考量**

桌面 Windows 執行階段伺服器可以想像成 "worker" 或 "task" 型。 傳入伺服器的每個呼叫會在非 UI 執行緒運作，且所有程式碼必須是多執行緒感知且是安全的。 側載應用程式中哪個部分呼叫伺服器功能也很重要。 在側載應用程式中務必一律禁止從任何 UI 執行緒呼叫長時間執行程式碼。 有兩個主要方法可以達到這個目的：

1.  如果從 UI 執行緒呼叫伺服器功能，一律使用伺服器公用介面區和實作中的非同步模式。

2.  在側載應用程式中，從背景執行緒呼叫伺服器功能。

**伺服器中的 Windows 執行階段非同步作業**

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

>**注意** 寫入實作時，等待一些其他可能的長時間執行作業是很常見的。 如果是這樣，需要宣告 **Task.Run** 程式碼：

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

這個非同步方法的用戶端可像等待任何其他 Windows 執行階段非同步作業一樣等待這個作業。

**從應用程式背景執行緒呼叫伺服器功能**

由於用戶端和伺服器通常都是由同一個組織所撰寫，因此可以採用一種程式設計做法，也就是從側載應用程式中的背景執行緒進行所有伺服器呼叫。 背景執行緒可以發出從伺服器收集一或多個資料批次的直接呼叫。 擷取全部結果後，應用程式處理程序記憶體中的資料批次通常可以直接從 UI 執行緒擷取。 C \# 物件在背景執行緒與 UI 執行緒之間自然是靈活的，因此特別適用于這種呼叫模式。

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>建立和部署 Windows 執行階段 Proxy

由於 IPC 方法涉及在這兩個處理程序間封送處理 Windows 執行階段介面，因此必須使用全域登錄的 Windows 執行階段 Proxy 和虛設常式。

**在 Visual Studio 建立 Proxy**

在一般 UWP 應用程式套件內建立和註冊 proxy 和存根以供使用的程式，將在 [Windows 執行階段元件中引發事件](https://docs.microsoft.com/previous-versions/windows/apps/dn169426(v=vs.140))主題中加以說明。
本文所述的步驟比下方說明的處理程序更加複雜，因為它涉及在應用程式套件內登錄 Proxy/虛設常式 (與全域登錄不同)。

**步驟 1：** 使用桌面元件專案的方案，在 Visual Studio 中建立 Proxy/虛設常式專案：

**方案 > 新增 > 專案 > Visual C++ > Win32 主控台選取 DLL 選項。**

針對下列步驟，我們假設伺服器元件名為 **MyWinRTComponent**。

**步驟 3：** 從專案刪除所有 CPP/H 檔案。

**步驟 4：** 上面的＜定義協定＞一節包含執行 **winmdidl.exe**、**midl.exe**、**mdmerge.exe** 等的建置後命令。 這個建置後命令的其中一個 midl 步驟輸出會產生四個重要的輸出：

a) Dlldata.c

b）標頭檔（例如，MyWinRTComponent）

c） \* \_ i. c 檔案（例如，MyWinRTComponent \_ i. c）

d） \* \_ p. c 檔案（例如，MyWinRTComponent \_ p .c）

**步驟 5：** 將這四個產生的檔案新增到 "MyWinRTProxy" 專案。

**步驟 6：** 將定義檔新增到 "MyWinRTProxy" 專案 **(專案 &gt; 加入新項目 &gt; 程式碼 &gt; 模組定義檔案**)，並將內容更新為：

LIBRARY MyWinRTComponent.Proxies.dll

EXPORTS

DllCanUnloadNow PRIVATE

DllGetClassObject PRIVATE

DllRegisterServer PRIVATE

DllUnregisterServer PRIVATE

**步驟 7：** 開啟 "MyWinRTProxy" 專案的屬性：

**Comfiguration 屬性 > 一般 > 目標名稱：**

MyWinRTComponent.Proxies

**C/c + + > 預處理器定義 > 新增**

"WIN32; \_時段註冊 \_ PROXY \_ DLL」

**C/c + + > 先行編譯標頭檔：選取 [不使用先行編譯標頭檔]**

**連結器 > 一般 > 忽略匯入程式庫：選取 [是]**

**連結器 > 輸入 > 其他相依性： Add rpcrt4; runtimeobject.lib. lib**

**連結器 > Windows 中繼資料 > 產生 Windows 中繼資料：選取 [否]**

**步驟 8：** 建置 "MyWinRTProxy" 專案。

**部署 Proxy**

必須全域登錄此 Proxy。 執行這個作業最簡單的方法就是讓您的安裝處理程序呼叫 Proxy dll 上的 DllRegisterServer。 請注意，由於這個功能只支援 x86 的伺服器組建 (也就是，不支援 64 位元)，最簡單的設定是使用 32 位元伺服器、32 位元 Proxy 和 32 位元側載應用程式。 Proxy 通常會與桌面元件的實作 **winmd** 放在一起。

還有一個額外的設定步驟要執行。 為了讓側載處理程序載入和執行 Proxy，目錄必須將 ALL_APPLICATION_PACKAGES 標記為 "read / execute"。 這要透過 **icacls.exe** 命令列工具完成。 這個命令應該在實作 **winmd** 和 Proxy/虛設常式 dll 所在的目錄中執行：

*icacls./T/grant \* S-1-15-2-1： RX*

## <a name="patterns-and-performance"></a>模式和效能

仔細監視跨處理程序傳輸的效能非常重要。 跨處理程序呼叫的成本至少是同處理程序呼叫的兩倍。 建立 "chatty" 交談跨處理程序或執行大型物件 (如點陣圖) 的重複傳輸會導致未預期和不佳的應用程式效能。

下列為需考量的簡短清單：

-   應一律避免從應用程式 UI 執行緒到伺服器的同步方法呼叫。 從應用程式的背景執行緒呼叫方法，然後視需要使用 CoreWindowDispatcher 將結果傳到 UI 執行緒。

-   從應用程式 UI 執行緒呼叫非同步作業很安全，但要考慮以下討論的效能問題。

-   大量傳輸結果會降低跨處理程序的交談功能。 這通常是使用 Windows 執行階段陣列建構來執行。

-   傳回 *List<T>*，其中 *T* 是來自非同步作業或屬性擷取的物件，會產生很多跨處理程序交談。 例如，假設您傳回 *List&lt;People&gt;* 物件。 每個反覆運算傳輸都是一個跨處理程序呼叫。 每個傳回的 *People* 物件都以 Proxy 表示，每個個別物件方法或屬性的呼叫會產生跨處理程序呼叫。 *Count* 中單純的 *List&lt;People&gt;* 物件是大型物件，會導致大量緩慢的呼叫。 大量傳輸陣列中的內容結構會產生較佳的效能。 例如：

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

然後傳回*PersonStruct \[ \] * ，而不是*List &lt; PersonObject &gt; *。
這會在一個跨處理程序 "hop" 取得所有的資料。

與所有效能考量一樣，測量和測試非常重要。 在理想的情況下，應該將遙測插入各種作業以判斷作業所花的時間。 務必要測量一整個範圍：例如，側載應用程式中的特定查詢實際上花了多少時間使用所有的 *People* 物件？

另一個技巧是可變載入測試。 您可以將效能測試攔截放入應用程式，將可變延遲載入引進伺服器處理來完成此作業。 這可以模擬各種載入，以及應用程式對不同伺服器效能的反應。
此範例說明如何使用適當的非同步技術將時間延遲放入程式碼。 要插入的實際延遲時間及要放入該人工載入的隨機範圍會因應用程式設計和應用程式預期執行環境而有所不同。

## <a name="development-process"></a>開發處理序

當您變更伺服器時，必須確定所有先前執行中的執行個體已不再執行。 COM 最終將清除這個處理序，但是取消計時器的時間會比反覆開發的有效時間還長。 因此，刪除先前執行中的執行個體是開發期間的一個標準步驟。 這需要開發人員持續追蹤哪一個 dllhost 執行個體正在裝載伺服器。

您可以使用 [工作管理員] 或其他協力廠商應用程式，找出伺服器處理序並予以刪除。 命令列工具**TaskList .exe**也包含在內，而且具有彈性的語法，例如：

  
 | **命令** | **動作** |
 | ------------| ---------- |
 | Tasklist | 依建立時間的大致順序列出所有執行中的處理序，最近建立的處理序會列於底部附近。 |
 | tasklist /FI "IMAGENAME eq dllhost.exe" /M | 列出所有 dllhost.exe 執行個體的相關資訊。 /M 參數會列出已載入的模組。 |
 | tasklist /FI "PID eq 12564" /M | 如果您知道 dllhost.exe 的 PID，就可使用這個選項來加以查詢。 |

代理人伺服器的模組清單應該會在其載入模組的清單中列出 *clrhost.dll*。

## <a name="resources"></a>資源

-   [適用於 Windows 10 與 VS 2015 的代理 WinRT 元件專案範本](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

-   [傳遞可靠且可信任的 Microsoft Store 應用程式](https://blogs.msdn.com/b/b8/archive/2012/05/17/delivering-reliable-and-trustworthy-metro-style-apps.aspx)

-   [應用程式協定與延伸 (Microsoft Store 應用程式)](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))

-   [如何在 Windows 10 上側載應用程式](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)

-   [將 UWP app 部署到企業](https://blogs.msdn.com/b/windowsstore/archive/2012/04/25/deploying-metro-style-apps-to-businesses.aspx)

