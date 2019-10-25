---
title: 適用于側載 UWP 應用程式的代理 Windows 執行階段元件
description: 本檔討論 Windows 10 支援的企業目標功能，讓觸控式 .NET 應用程式可以使用現有程式碼，負責重要的業務關鍵作業。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: b28df646bb505889626ced8591c5ef9e6ece3f44
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690342"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>適用于側載 UWP 應用程式的代理 Windows 執行階段元件

本文討論 Windows 10 支援的企業目標功能，可讓觸控式 .NET 應用程式使用現有程式碼，負責重要的業務關鍵作業。

## <a name="introduction"></a>簡介

>**請注意** 本文隨附的範例程式碼可能會下載 [Visual Studio 2015 & 2017](https://aka.ms/brokeredsample)。 您可以在這裡下載 Microsoft Visual Studio 範本來建立代理 Windows 執行階段元件：以[適用于 Windows 10 的通用 Windows 應用程式為目標的 Visual Studio 2015 範本](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

Windows 包含一項新功能 *，稱為代理端載入應用程式的代理 Windows 執行階段元件*。 我們使用 IPC （處理序間通訊一詞）來描述在 UWP 應用程式中與此程式碼互動時，在一個進程（桌面元件）中執行現有桌面軟體資產的能力。 這是企業開發人員熟悉的模型，因為在 Windows 中使用 NT 服務的資料庫應用程式和應用程式共用類似的多進程架構。

應用程式的側載是這項功能的重要元件。
企業專屬的應用程式不會在一般取用者 Microsoft Store，而且公司對於安全性、隱私權、散發、設定和服務都有非常特定的需求。 因此，側載模型同時也是使用這項功能的需求，以及重要的執行詳細資料。

以資料為中心的應用程式是此應用程式架構的關鍵目標。 想像的是，現有的商務規則 ensconced （例如，在 SQL Server 中）將是桌面元件的常見部分。 這當然不是桌面元件可以好處的唯一功能類型，但這項功能的需求很大，與現有的資料和商務邏輯有關。

最後，基於企業開發的 .NET 執行時間和 C\# 語言的巨大滲透，這項功能的開發重點在於 UWP 應用程式和桌面元件端都使用 .NET。 雖然 UWP 應用程式有可能的其他語言和執行時間，隨附的範例只會說明 C\#，而且僅限於 .NET 執行時間。

## <a name="application-components"></a>應用程式元件

>**請注意** 這項功能僅適用于使用 .net。 用戶端應用程式和桌面元件都必須使用 .NET 撰寫。

**應用程式模型**

這項功能是圍繞一般的應用程式架構所建立，稱為 MVVM （模型視圖視圖模型）。 因此，假設「模型」完全存放在桌面元件中。 因此，它應該會立即明顯地看出桌面元件將會是「無周邊」（也就是不包含任何 UI）。 此視圖會完全包含在側載的企業應用程式中。 雖然此應用程式不需要以「視圖模型」結構來建立，但我們預期此模式的使用方式很常見。

**桌面元件**

這項功能中的桌面元件是此功能中引進的新應用程式類型。 這個桌面元件只能以 C\# 撰寫，而且必須以適用于 Windows 10 的 .NET 4.6 或更新版本為目標。 專案類型是 CLR 目標為 UWP 的混合式，因為處理序間通訊格式是由 UWP 類型和類別所組成，而桌面元件則可以呼叫 .NET 執行時間類別庫的所有部分。 稍後將詳細說明對 Visual Studio 專案的影響。 這種混合式設定允許在以桌面元件為基礎的應用程式之間封送處理 UWP 類型，同時允許在桌面元件執行內呼叫 desktop CLR 程式碼。

**協定**

側載應用程式與桌面元件之間的合約會以 UWP 類型系統來描述。 這牽涉到宣告一或多個可代表 UWP 的 C\# 類別。 如需使用 C\#建立 Windows 執行階段類別的特定需求，請參閱 MSDN 主題[建立 C\# 中的 Windows 執行階段元件和 Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br230301(v=vs.140)) 。

>**請注意**，桌面元件和側載應用程式之間的 Windows 執行階段元件合約中不支援  列舉。

**側載應用程式**

側載應用程式在每個方面都是一般的 UWP 應用程式，但其中一項除外：它是側載，而不是透過 Microsoft Store 安裝。 大部分的安裝機制都相同：資訊清單和應用程式封裝類似（稍後會詳細說明資訊清單的另一項）。 啟用側載後，簡單的 PowerShell 腳本就可以安裝必要的憑證和應用程式本身。 這是一般的最佳作法，即側載應用程式會傳遞 [專案/存放區] 功能表中所包含的 WACK 認證測試 Visual Studio

>**注意**可以在 [設定] 中開啟側載-&gt; 的更新 & 安全性-&gt; 供開發人員。

值得注意的一點是，隨附于 Windows 10 的應用程式代理人機制僅限32位。 桌面元件必須是32位。
側載應用程式可以是64位（前提是已註冊64位和32位 proxy），但這不是典型的。 使用一般的「中性」設定和「慣用32位」預設值，在 C\# 中建立側載應用程式，自然會建立32位的端載入應用程式。

**伺服器實例和 Appdomain**

每個側載應用程式都會收到自己的應用程式代理人伺服器實例（所謂的「多重實例」）。 伺服器程式碼會在單一 AppDomain 內執行。 這可讓您在不同的實例中執行多個版本的程式庫。 例如，應用程式 A 需要元件的 v1.1，而應用程式 B 需要 V2。 這些會在不同的伺服器目錄中具有 v1.1 和 V2 元件，並將應用程式指向任何伺服器支援所需的正確版本，以完全分隔。

藉由將多個應用程式指向相同的伺服器目錄，可以在多個應用程式代理人伺服器實例之間共用伺服器程式碼執行。 應用程式代理人伺服器仍然會有多個實例，但它們會執行相同的程式碼。 單一應用程式中使用的所有執行元件都應該出現在相同的路徑中。

## <a name="defining-the-contract"></a>定義合約

使用此功能建立應用程式的第一步，是在側載應用程式與桌面元件之間建立合約。 這必須使用 Windows 執行階段類型來獨佔完成。
幸運的是，使用 C\# 類別來宣告這些是很容易的。 不過，在定義這些交談時，有一些重要的效能考慮，在稍後的章節中會加以討論。

定義合約的順序會如下所示：

**步驟1：** 在 Visual Studio 中建立新的類別庫。 請務必使用 [**類別庫**] 範本，而不是 [ **Windows 執行階段元件**] 範本來建立專案。

執行顯然會遵循，但本節只涵蓋進程間合約的定義。 隨附的範例包含下列類別（EnterpriseServer.cs），其開始圖形如下所示：

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

這會定義可從側載應用程式具現化的類別 "EnterpriseServer"。 此類別提供 RuntimeClass 承諾的功能。 RuntimeClass 可以用來產生將包含在側載應用程式中的 reference winmd。

**步驟2：** 手動編輯專案檔，將專案的輸出類型變更為**Windows 執行階段元件**。

若要在 Visual Studio 中執行此動作，請以滑鼠右鍵按一下新建立的專案，然後選取 [卸載專案]，再以滑鼠右鍵按一下並選取 [編輯 EnterpriseServer]，以開啟專案檔（XML 檔案）來進行編輯。

在開啟的檔案中，搜尋 \<OutputType\> 標記，並將其值變更為 "winmdobj"。

**步驟3：** 建立可建立「參考」 Windows 中繼資料檔案（winmd 檔案）的組建規則。 也就是沒有任何實作為。

**步驟4：** 建立建立「執行」 Windows 中繼資料檔案的組建規則，也就是具有相同的中繼資料資訊，但也包含執行。

這會由下列腳本完成。 將腳本加入至 專案**屬性** 中的 建立後事件 命令列， > **組建事件**。

> **請注意**，腳本會根據您的目標 windows 版本（windows 10）和使用中的 Visual Studio 版本而有所不同。

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

一旦建立了 reference **winmd** （在專案的 [目標] 資料夾下的資料夾 "reference"），就會將它交給（複製）到每個取用端載入的應用程式專案，並加以參考。 下一節將進一步說明這一點。 上述組建規則中所述的專案結構，可確保實體系和 reference **winmd**會在組建階層中明確地隔離目錄，以避免混淆。

## <a name="side-loaded-applications-in-detail"></a>側載應用程式詳細資料
如先前所述，側載應用程式與任何其他 UWP 應用程式的建立方式相同，但有一個額外的詳細資料：在側載應用程式的資訊清單中宣告 RuntimeClass 的可用性。 如此一來，應用程式就可以直接撰寫新的來存取桌面元件中的功能。 [<Extension>] 區段中的新資訊清單專案描述在桌面元件中執行的 RuntimeClass，以及其所在位置的資訊。 針對以 Windows 10 為目標的應用程式，這些宣告內容在應用程式的資訊清單中是相同的。 例如：

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

類別是 inProcessServer，因為 outOfProcessServer 類別目錄中有數個專案不適用於此應用程式設定。 請注意，<Path> 元件必須一律包含 clrhost （但**不**會強制執行，而且指定不同的值將會失敗，並以未定義的方式進行）。

<ActivatableClass> 區段與應用程式套件中 Windows 執行階段元件慣用的實際同進程 RuntimeClass 相同。 <ActivatableClassAttribute> 是新的專案，且屬性 Name = "DesktopApplicationPath" 和 Type = "string" 是強制性和不變的。 Value 屬性指向桌面元件的執行 winmd 所在的位置（下一節將詳細說明此功能）。 桌面元件慣用的每個 RuntimeClass 都應該有自己的 <ActivatableClass> 元素樹狀結構。 ActivatableClassId 必須符合 RuntimeClass 的完整命名空間限定名稱。

如「定義合約」一節所述，必須對桌面元件的參考 winmd 進行專案參考。 Visual Studio 專案系統通常會建立一個具有相同名稱的兩個層級目錄結構。 在範例中，它是 EnterpriseIPCApplication\\EnterpriseIPCApplication。 參考**winmd**會手動複製到第二層目錄，然後使用 [專案參考] 對話方塊（按一下 [**流覽]。** 按鈕）來尋找並參考此**winmd**。 在此之後，桌面元件（例如 Fabrikam）的最上層命名空間應該會在專案的 [參考] 部分中顯示為最上層節點。

>**注意**在側載應用程式中使用**reference winmd**非常重要。 如果您不小心將執行**winmd 部署**到側載的應用程式目錄，並參考它，您可能會收到「找不到 IStringable」的相關錯誤。 這是一項確定已參考錯誤的**winmd** 。 IPC 伺服器應用程式中的組建後的規則（下一節詳述）會仔細將這兩個**winmd**隔離成不同的目錄。

環境變數（尤其是% ProgramFiles%）可以在 <ActivatableClassAttribute Value="path"> 中使用。如先前所述，應用程式代理人僅支援32位，因此，如果應用程式是在64位作業系統上執行，% ProgramFiles% 就會解析為 C：\\Program Files （x86）。

## <a name="desktop-ipc-server-detail"></a>桌面 IPC 伺服器詳細資料

前兩節描述類別的宣告，以及將參考**winmd**傳輸至側載應用程式專案的機制。 桌面元件中的其餘工作大部分牽涉到執行。 由於桌面元件的重點是要能夠呼叫桌面程式碼（通常是為了重複使用現有的程式碼資產），因此必須以特殊方式設定專案。
一般來說，使用 .NET 的 Visual Studio 專案會使用兩個「設定檔」其中之一。
一個適用于 desktop （".Netframework "），而另一個則以 CLR （" 的 UWP 應用程式部分為目標。NetCore "）。 此功能中的桌面元件是這兩者之間的混合式。 因此，[參考] 區段非常謹慎地加以結構化，以 blend 這兩個設定檔。

一般 UWP 應用程式專案不包含明確的專案參考，因為已隱含包含整個 Windows 執行階段 API 介面。
通常只會建立其他專案間的參考。 不過，桌面元件專案具有一組非常特殊的參考。 它會以「傳統桌面\\類別庫」專案的形式啟動，因此是桌面專案。 因此，您必須對 Windows 執行階段 API 進行明確的參考（透過**winmd**檔案的參考）。 新增適當的參考，如下所示。

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

上述參考是對這種混合式伺服器正常運作而言非常重要的 eferences 混合。 通訊協定是開啟 .csproj 檔案（如如何編輯專案 OutputType 中所述），並視需要新增這些參考。

正確設定參考之後，下一項工作就是執行伺服器的功能。 請參閱 [與 Windows 執行階段元件互通的最佳做法主題（使用 C\#/VB/C++和 XAML 的 UWP 應用程式）](https://docs.microsoft.com/previous-versions/windows/apps/hh750311(v=win.10))。
工作是建立一個可以呼叫桌面程式碼做為其實作為一部分的 Windows 執行階段元件 dll。 隨附的範例包含 Windows 執行階段中使用的主要模式：

-   方法呼叫

-   桌面元件 Windows 執行階段事件來源

-   Windows 執行階段非同步作業

-   傳回基本類型的陣列

**安裝**

若要安裝應用程式，請將執行**winmd**複製到相關聯的側載應用程式資訊清單中指定的正確目錄： <ActivatableClassAttribute>的值 = "path"。 此外，也請複製任何相關聯的支援檔案和 proxy/stub dll （下面將說明這後面的詳細資料）。 無法將執行**winmd**複製到伺服器目錄位置，會使所有側載應用程式在 RuntimeClass 上的呼叫都會擲回「類別未註冊」錯誤。 無法安裝 proxy/stub （或無法註冊）會導致所有呼叫失敗，而且沒有傳回值。 這個第二個錯誤通常**不**會與可見的例外狀況相關聯。
如果由於此設定錯誤而發現例外狀況，它們可能會參考「不正確轉換」。

**伺服器執行考慮**

桌面 Windows 執行階段伺服器可以視為「工作者」或「工作」的基礎。 伺服器的每個呼叫都是在非 UI 執行緒上運作，而且所有程式碼都必須是多執行緒感知且安全的。 側載應用程式的哪個部分呼叫伺服器的功能也很重要。 請務必避免從側載應用程式中的任何 UI 執行緒呼叫長時間執行的程式碼。 有兩種主要方式可以完成此動作：

1.  如果從 UI 執行緒呼叫伺服器功能，請一律在伺服器的公用介面區和執行中使用非同步模式。

2.  從側載應用程式中的背景執行緒呼叫伺服器的功能。

**在伺服器中 Windows 執行階段非同步**

由於應用程式模型的跨進程本質，對伺服器的呼叫會比以獨佔方式執行的程式碼具有更多的負荷。 通常可以安全地呼叫會傳回記憶體內部值的簡單屬性，因為它的執行速度會快到封鎖 UI 執行緒並不是個問題。 不過，任何包含任何排序 i/o 的呼叫（包括所有檔案處理和資料庫檢索）都可能封鎖呼叫的 UI 執行緒，並導致應用程式因無回應而終止。 此外，基於效能考慮，在此應用程式架構中不鼓勵物件的屬性呼叫。
這會在下一節中更深入討論。

正確執行的伺服器通常會透過 Windows 執行階段非同步模式來實作為直接從 UI 執行緒進行的呼叫。 這可以遵循這個模式來執行。 首先，宣告（同樣地，從隨附的範例）：

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

這會宣告會傳回整數的 Windows 執行階段非同步作業。
非同步作業的執行通常會採用下列格式：

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**注意**在撰寫執行時，通常會等待一些其他可能長時間執行的作業。 若是如此，則必須將**執行**程式碼宣告為：

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

這個非同步方法的用戶端可以等待此作業，就像任何其他 Windows 執行階段 aysnc 作業一樣。

**從應用程式背景執行緒呼叫伺服器功能**

由於用戶端和伺服器通常都是由相同的組織撰寫，因此可以採用程式設計實務，讓所有對伺服器的呼叫都是由側載應用程式中的背景執行緒進行。 從伺服器收集一或多個批次資料的直接呼叫，可以從背景執行緒進行。 當完整抓取結果時，通常會從 UI 執行緒直接抓取應用程式進程中記憶體內部的資料批次。 C @ no__t_0_ 物件在背景執行緒與 UI 執行緒之間自然是靈活的，因此特別適用于這種呼叫模式。\#

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>建立和部署 Windows 執行階段 proxy

由於 IPC 方法牽涉到在兩個進程之間封送處理 Windows 執行階段介面，因此必須使用全域註冊的 Windows 執行階段 proxy 和存根。

**在 Visual Studio 中建立 proxy**

在一般 UWP 應用程式套件內建立和註冊 proxy 和存根以供使用的程式，將在 [Windows 執行階段元件中引發事件](https://docs.microsoft.com/previous-versions/windows/apps/dn169426(v=vs.140))主題中加以說明。
本文所述的步驟比下面所述的程式更為複雜，因為它牽涉到在應用程式套件內註冊 proxy/stub （而不是全域註冊）。

**步驟1：** 使用桌面元件專案的解決方案，在 Visual Studio 中建立 Proxy/Stub 專案：

**方案 > 新增 > 專案 > Visual C++ > Win32 主控台選取 DLL 選項。**

在下列步驟中，我們假設伺服器元件稱為**MyWinRTComponent**。

**步驟3：** 刪除專案中的所有 CPP/H 檔案。

**步驟4：** 上一節「定義合約」包含執行**winmdidl**、 **midl**、 **Mdmerge**等的後置組建命令。」 這個後置組建命令的 midl 步驟其中一個輸出會產生四個重要的輸出：

a） Dlldata.c。c

b）標頭檔（例如，MyWinRTComponent）

c）\_i. c 檔案的 \*（例如，MyWinRTComponent\_i. c）

d）\_p. c 檔案的 \*（例如 MyWinRTComponent\_p. c）

**步驟5：** 將這四個產生的檔案新增至 "MyWinRTProxy" 專案。

**步驟6：** 將 def 檔案加入至 "MyWinRTProxy" 專案 **（專案 > 將新專案加入 > 程式碼 > 模組定義**檔案），並將內容更新為：

程式庫 MyWinRTComponent

多餘

DllCanUnloadNow 私用

DllGetClassObject 私用

DllRegisterServer 私用

DllUnregisterServer 私用

**步驟7：** 開啟 "MyWinRTProxy" 專案的屬性：

**Comfiguration 屬性 > 一般 > 目標名稱：**

MyWinRTComponent proxy

**> 新增C++的 C/> 預處理器定義**

32\_WINDOWS;註冊\_PROXY\_DLL」

**C/C++ > 先行編譯標頭檔：選取 [不使用先行編譯標頭檔]**

**連結器 > 一般 > 忽略匯入程式庫：選取 [是]**

**連結器 > 輸入 > 其他相依性： Add rpcrt4; runtimeobject.lib. lib**

**連結器 > Windows 中繼資料 > 產生 Windows 中繼資料：選取 [否]**

**步驟8：** 建立 "MyWinRTProxy" 專案。

**部署 proxy**

Proxy 必須是全域註冊的。 若要這麼做，最簡單的方式是將您的安裝程序呼叫 DllRegisterServer 在 proxy dll 上。 請注意，由於此功能僅支援針對 x86 所建立的伺服器（也就是沒有64位支援），最簡單的設定是使用32位伺服器、32位 proxy 和32位側載應用程式。 Proxy 通常會與桌面元件的執行**winmd**並存。

必須執行一個額外的設定步驟。 為了讓側載的進程載入及執行 proxy，目錄必須標示為「讀取/執行」以進行 ALL_APPLICATION_PACKAGES。 這是透過**icacls**命令列工具來完成。 此命令應該會在執行**winmd**和 proxy/stub dll 所在的目錄中執行：

*icacls./T/grant \*S-1-15-2-1： RX*

## <a name="patterns-and-performance"></a>模式和效能

請務必小心監視跨進程傳輸的效能。 跨進程呼叫的成本至少比同進程呼叫的兩倍。 建立「多對話」交談跨進程或執行大型物件（例如點陣圖影像）的重複傳輸，可能會導致非預期且不必要的應用程式效能。

以下是要考慮的不完整清單：

-   應該一律避免從應用程式的 UI 執行緒到伺服器的同步方法呼叫。 從應用程式中的背景執行緒呼叫方法，然後使用 CoreWindowDispatcher，在必要時將結果放入 UI 執行緒。

-   從應用程式 UI 執行緒呼叫非同步作業是安全的，但請考慮下面所討論的效能問題。

-   大量傳輸結果可減少跨進程對話。 通常會使用 Windows 執行階段陣列結構來執行這項作業。

-   傳回*清單<T>* 其中*t*是來自非同步作業或屬性提取的物件，會導致許多跨進程對話。 例如，假設您傳回一*份清單，&lt;人員&gt;* 物件。 每個反復專案傳遞都會是跨進程呼叫。 每個傳回的*人*物件都是由 proxy 所表示，而每次呼叫該個別物件上的方法或屬性，都會產生跨進程呼叫。 因此，「無害」*清單&lt;人員&gt;* *計數*很大的物件，將會造成大量的緩慢呼叫。 更好的效能結果，是因為陣列中內容的結構大量傳送。 例如：

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

然後傳回 * PersonStruct\[\]*，而不是*List&lt;PersonObject&gt;* 。
這會跨進程的「躍點」取得所有資料

就像所有的效能考慮，測量和測試很重要。 最理想的遙測應該插入各種作業，以判斷其所需的時間。 請務必測量範圍：例如，在側載應用程式中使用特定查詢的所有*人員*物件時，實際需要多久的時間？

另一個方法是變數負載測試。 這可以藉由將效能測試勾點放入應用程式中，將變數延遲載入引入伺服器處理。 這可以模擬各種負載，以及應用程式對不同伺服器效能的反應。
此範例說明如何使用適當的非同步技巧，將時間延遲放入程式碼中。 要插入的確切延遲數量以及要放入該人工智慧的隨機範圍，會因應用程式的設計和應用程式將執行的預期環境而異。

## <a name="development-process"></a>開發流程

當您對伺服器進行變更時，必須確定任何先前執行的實例已不再執行。 COM 最後會清除進程，但是取消計時器所花費的時間比在反復開發中有效率。 因此，在開發期間，終止先前執行的實例是正常的步驟。 這需要開發人員追蹤裝載伺服器的 dllhost.exe 實例。

您可以使用工作管理員或其他協力廠商應用程式來尋找及終止伺服器進程。 命令列工具 * * TaskList * * 也包括在內，而且具有彈性的語法，例如：

  
 | **命令** | **動作** |
 | ------------| ---------- |
 | 任務 | 列出所有執行中的進程，並以大致的建立時間為准，最近建立的進程接近底端。 |
 | tasklist/FI "IMAGENAME eq dllhost.exe .exe"/M | 列出所有 dllhost.exe .exe 實例的資訊。 /M 參數會列出它們已載入的模組。 |
 | tasklist/FI "PID eq 12564"/M | 您可以使用此選項來查詢 dllhost.exe （如果您知道其 PID）。 |

訊息代理程式伺服器的模組清單應該會在其已載入的模組清單中列出*clrhost。*

## <a name="resources"></a>資源

-   [適用于 Windows 10 和 VS 2015 的代理 WinRT 元件專案範本](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

-   [NorthwindRT 代理 WinRT 元件範例](https://go.microsoft.com/fwlink/p/?LinkID=397349)

-   [提供可靠且值得信賴的 Microsoft Store 應用程式](https://go.microsoft.com/fwlink/p/?LinkID=393644)

-   [應用程式合約和延伸模組（Windows Store 應用程式）](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))

-   [如何在 Windows 10 上側載應用程式](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)

-   [將 UWP 應用程式部署到企業](https://go.microsoft.com/fwlink/p/?LinkID=264770)

