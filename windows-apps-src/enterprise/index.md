---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: 此藍圖概述適用於 Windows 10 和通用 Windows 平台 (UWP) 應用程式的重要企業功能。
title: 企業
ms.date: 08/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: eec1de013efce7b23cd89e81f659a5cc530638c4
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339963"
---
# <a name="enterprise"></a>企業

本文概述通用 Windows 平台 (UWP) 針對 Windows 10 應用程式而提供的重要企業功能。 如需詳盡示範其中部分功能的影片，請參閱[使用 UWP 和 Visual Studio 快速建構 LOB 應用程式](https://channel9.msdn.com/Events/Build/2018/BRK3502)。

<a id="template-studio" />

### <a name="windows-template-studio"></a>Windows Template Studio

Windows Template Studio 是 Visual Studio 2017 和 Visual Studio 2019 的擴充功能，可讓您更快速地使用精靈式體驗建立新的通用 Windows 平台 (UWP) 應用程式。 產生的 UWP 專案是是格式正確的可讀程式碼，其中包含最新的 Windows 10 功能，同時實作經過實證的模式和最佳做法。

![Windows Template Studio](images/windows-template-studio.png)

請參閱 [Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio)

<a id="desktop-style-UI" />

### <a name="controls-to-create-desktop-style-uis"></a>用來建立桌面樣式 UI 的控制項

我們發行了新的 UWP XAML 控制項，用以填補傳統桌面應用程式 UI 與 UWP UI 之間的差距。

例如，新的 [MenuBar](/windows/uwp/design/controls-and-patterns/menus)、[DropDownButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button)、[SplitButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) 和 [CommandBarFlyout](/windows/uwp/design/controls-and-patterns/command-bar-flyout) 控制項可讓您更靈活地公開命令，而 [EditableComboBox](/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) 則可讓使用者輸入預先定義的選項清單中未列出的值。

![MenuBar](images/menu-bar.png)

<a id="enterprise" />

### <a name="controls-to-support-enterprise-scenarios"></a>用來支援企業案例的控制項

[DataGridView](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/datagrid) 可讓您靈活顯示資料列和資料行中的資料集合。

[TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) 啟用提供包含巢狀項目的展開及摺疊節點的階層式清單。 此控制項可以用來說明 UI 中的資料夾結構或巢狀關聯性。

![DataGrid 控制項](images/DataGrid.gif)


### <a name="windows-ui-library"></a>Windows UI 程式庫

Windows UI 程式庫是一組 NuGet 套件，可提供 UWP 應用程式的控制項和其他使用者介面元素。 此外也提供對舊版 Windows 10 的向下相容性，因此即便使用者沒有最新的作業系統，您的應用程式仍可運作。

![Windows UI 程式庫](images/win-ui.png)

請參閱 [Windows UI 程式庫 (預覽版)](https://docs.microsoft.com/uwp/toolkits/winui/)。

<a id="xaml-islands" />

### <a name="uwp-controls-in-desktop-applications"></a>傳統型應用程式中的 UWP 控制項

Windows 10 現在可讓您在 WPF、Windows Forms 和 C++ Win32 傳統型應用程式中使用 UWP 控制項。 這表示您可以使用只能透過 UWP 控制項 (例如 Windows Ink) 和支援 Fluent Design 系統的控制項存取的最新 Windows 10 UI 功能，來增強現有傳統型應用程式的外觀與風格和功能。 這項功能稱為 XAML Islands。

請參閱[傳統型應用程式中的 UWP 控制項](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)。

<a id="standard" />

### <a name="net-standard-20"></a>.NET Standard 2.0

.NET Standard 包含的 API 比 .NET Standard 1.x 多出 20,000 餘個。 因此，要移轉現有的 .NET Framework 程式庫，然後在不同的 .NET 應用程式 (包括您的 UWP 應用程式) 之間使用這些程式庫，將比以往容易得多。

![net-standard](images/dot-net-standard-project-template.png)

請參閱[在傳統型應用程式與 UWP 應用程式之間共用程式碼](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate)。

<a id="sql-server" />

### <a name="sql-server-connectivity"></a>SQL Server 連線

您的應用程式可以直接連線到 SQL Server 資料庫，然後使用 [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) 命名空間中的類別儲存和擷取資料。

請參閱[在 UWP app 中使用 SQL Server 資料庫](https://docs.microsoft.com/en-us/windows/uwp/data-access/sql-server-databases)。

<a id="MSIX" />

### <a name="msix-deployment"></a>MSIX 部署

MSIX 是一種 Windows 應用程式套件格式，可為所有 Windows 應用程式提供現代化的封裝體驗。 MSIX 套件格式除了支援對 Win32、WPF 和 Windows Forms 應用程式的新式封裝和部署功能，也保留了現有應用程式套件和安裝檔案的功能。

MSIX 是根據 .msi、.appx、App-V 和 ClickOnce 安裝技術的組合而建置的封裝格式，不僅安全、受到保護，也具可靠性。

![MSIX 圖示](images/MSIX-App-Package.ico)

請參閱 [MSIX 文件](https://docs.microsoft.com/windows/msix/)。

<a id="distribution" />

## <a name="security"></a>安全性

Windows 10 提供一套安全性功能，讓應用程式開發人員保護其使用者的身分識別、公司網路的安全性以及裝置上儲存的任何商務資料。 Windows 10 的新增功能是 Microsoft Passport，一種可使用 PIN 或 Windows Hello 存取的易部署雙因素密碼替代方式，可提供企業級安全性，以及支援指紋、臉部和虹膜辨識。

| 主題 | 描述 |
|-------|-------------|
| [安全開發 Windows 應用程式的簡介](https://docs.microsoft.com/windows/uwp/security/intro-to-secure-windows-app-development) | 這篇簡介文章說明不同驗證階段 (包括傳輸中資料和靜態資料) 的各種 Windows 安全功能。 它也描述如何將這些階段整合到您的 app。 本文涵蓋大範圍的主題，主要目的是協助應用程式設計人員更充分地了解可快速且輕易地建立通用 Windows 平台應用程式的 Windows 功能。 |
| [驗證和使用者識別](https://docs.microsoft.com/windows/uwp/security/authentication-and-user-identity) | UWP app 有本文所述的數個使用者驗證選項。 若要用於企業，則強烈建議選用新的 Microsoft Passport 功能。 Microsoft Passport 以增強式雙因素驗證 (2FA) 取代密碼，方法是驗證現有的認證，以及建立以生物識別或 PIN 式使用者手勢所保護的裝置特定認證，以產生方便且高度安全的使用經驗。 |
| [密碼編譯](https://docs.microsoft.com/windows/uwp/security/cryptography) | 密碼編譯一節概述 UWP app 所提供的密碼編譯功能。 文章的範圍包括從如何輕鬆加密機密商業資料的簡介逐步解說，到操作密碼編譯金鑰，以及使用 MAC、雜湊和簽章這類進階主題。 |
| [Windows 資訊保護 (WIP)](wip-hub.md) | 這是一個中樞主題，從開發人員角度來探討 Windows 資訊保護 (WIP) 與檔案、緩衝區、剪貼簿、網路、背景工作的關聯，以及資料鎖定時的保護。 |

## <a name="data-binding-and-databases"></a>資料繫結和資料庫

資料繫結可讓您的 app UI 顯示來自外部來源 (例如資料庫) 的資料，以及選擇性地與該資料保持同步。 資料繫結可讓您將資料與 UI 分開考量，為 app 建構更簡單的概念模型，以及更好的可讀性、測試性和維護性。

| 主題 | 描述 |
|-------|-------------|
| [資料繫結概觀](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart) | 本主題說明如何在通用 Windows 平台 (UWP) 應用程式中將控制項 (或其他 UI 元素) 繫結到單一項目，或將項目控制項繫結到項目集合。 此外，還會示範如何控制項目的呈現、根據選擇來實作詳細資料檢視，以及轉換資料以供顯示。 |
| [Entity Framework 7 for UWP](https://docs.microsoft.com/windows/uwp/data-access/entity-framework-7-with-sqlite-for-csharp-apps) | 對大型資料集執行複雜查詢，可使用支援 UWP 的 Entity Framework 7 進行大幅簡化。 在這個逐步解說中，您將建置 UWP 應用程式，以使用 Entity Framework 對本機 SQLite 資料庫執行基本資料存取。 |
| [SQLite 本機資料庫](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | 這個影片是使用 SQLite 的完整開發人員指南，而 SQLite 是本機 app 資料庫的建議方案。 請瀏覽 [SQLite](https://www.sqlite.org/download.html) 以下載 UWP 的最新版本，或使用 Windows 10 SDK 已隨附的版本。 |

## <a name="networking-and-data-serialization"></a>網路功能和資料序列化

企業營運 app 通常需要與各種其他系統通訊，或在各種其他系統上儲存資料。 做法通常是連線到網路服務 (使用 REST 或 SOAP 這類通訊協定)，然後序列化資料或將資料還原序列化成通用格式。 在與 WPF、WinForm 和 ASP.NET 應用程式類似的 UWP app 中，使用網路和資料序列化。 如需詳細資訊，請參閱下列文章。

| 主題 | 描述 |
|-------|-------------|
| [網路功能基本知識](https://docs.microsoft.com/windows/uwp/networking/networking-basics) | 這個逐步解說說明與所有 UWP app 相關的基本網路概念，而不管使用中的通訊協定為何。  |
| [哪一種網路功能技術？](https://docs.microsoft.com/windows/uwp/networking/which-networking-technology) | 適用於 UWP app 的網路功能技術快速概觀，並建議您如何選擇最適合您的 app 的技術。 |
| [XML 和 SOAP 序列化](https://docs.microsoft.com/dotnet/framework/serialization/xml-and-soap-serialization) | XML 序列化會將物件轉換成符合特定 XML 結構描述定義語言 (XSD) 的 XML 資料流。 若要在 XML 與強型別類別之間進行轉換，您可以使用原生 [XDocument](https://docs.microsoft.com/dotnet/api/system.xml.linq.xdocument) 類別或外部程式庫。 |
| [JSON 序列化](https://docs.microsoft.com/uwp/api/Windows.Data.Json) | JSON (JavaScript 物件標記法) 序列化是與 REST API 進行通訊的常用格式。 UWP app 完全支援的 [Newtonsoft Json.NET](https://www.newtonsoft.com/json)。 |

## <a name="devices"></a>裝置

為了與企業營運工具 (如印表機、條碼掃描器或智慧卡讀卡機) 整合，您可能會發現有必要將外部裝置或感應器與您的 app 整合。 以下是一些功能範例，您可以使用本節所描述的技術將這些功能新增到您的 app 中。

| 主題  | 描述 |
|--------|-------------|
| [列舉裝置](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices) | 本文說明如何使用 [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) 命名空間尋找內部連接到系統、外部連接或者可透過無線或網路通訊協定偵測到的裝置。 如果您是建置任何與裝置搭配運作的 app，請從這裡開始。 |
| [列印與掃描](https://docs.microsoft.com/windows/uwp/devices-sensors/printing-and-scanning) | 說明如何從您的應用程式進行列印和掃描，包括連接和使用企業裝置，例如銷售點 (POS) 系統、收據印表機，以及高容量送紙器掃描器。 |
| [藍牙](https://docs.microsoft.com/windows/uwp/devices-sensors/bluetooth) | 除了使用傳統藍牙連線來傳送和接收資料或控制裝置，Windows 10 還可使用藍牙低功耗 (BTLE) 在背景傳送或接收指標。 使用這個項目，以在使用者接近或離開特定位置時顯示通知或啟用功能。 |
| [企業共用存放裝置](enterprise-shared-storage.md) | 在裝置鎖定案例中，了解如何在相同的應用程式內、應用程式執行個體之間或甚至應用程式之間共用資料。 |

## <a name="device-targeting"></a>裝置目標

許多使用者現在都會將他們的手機或平板電腦帶去上班，而手機或平板電腦的板型規格和螢幕大小都不同。 您可以使用通用 Windows 平台 (UWP) 撰寫在所有不同類型的裝置上流暢執行的單一企業營運 app (包括桌上型電腦電腦和 PPI 顯示器)，可讓您最接近 app 並將程式碼效率提到最高。

| 主題 | 描述 |
|-------|-------------|
| [UWP 應用程式指南](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) | 在本簡介指南中，您將了解 Windows 10UWP 平台，包括︰裝置系列為何以及如何決定要設為目標的裝置系列、新的 UI 控制項和面板以讓您將 UI 調整為不同的裝置板型規格，以及如何了解與控制可供您的 app 使用的 API 表面。 |
| [彈性 XAML UI 程式碼範例](https://go.microsoft.com/fwlink/p/?LinkId=619992) | 這個程式碼範例說明應用程式的所有可能版面配置選項和控制項 (不論裝置類型為何)，並可讓您與面板互動以顯示如何達成您要尋找的任何版面配置。 除了顯示每個控制項如何回應不同的板型規格之外，app 本身也具有回應，並顯示達成彈性 UI 的各種方法。 |
| [Xamarin 主題](/xamarin/) | 以手機為目標的 Xamarin |

## <a name="deployment"></a>部署

您有許多將 app 發佈至組織使用者的選項。 您可以利用商務用 Microsoft Store 或現有行動裝置管理，或是側載應用程式至裝置。 您也可藉由發佈到 Microsoft Store，將您的應用程式提供給一般大眾。

| 主題 | 描述 |
|-------|-------------|
| [將 LOB 應用程式發佈到企業](https://docs.microsoft.com/windows/uwp/publish/distribute-lob-apps-to-enterprises) | 您可以透過商務用 Microsoft Store，將企業營運應用程式直接發佈到企業來進行大量取得，而不需要讓大眾廣泛取得應用程式。 |
| [側載應用程式](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10) | 當您側載 app 時，您要將簽署的 app 套件部署到裝置。 您要維護這些 app 的簽署、裝載和部署。 用於側載 app 的程序已經簡化成適用於 Windows 10。             |
| [將應用程式發佈至 Microsoft Store](https://developer.microsoft.com/store/publish-apps) | 整合的 Windows Store 可讓您發佈與管理您為所有 Windows 裝置開發的所有應用程式。 透過每個市場價格、發佈和可見性控制項，以及其他選項來自訂您應用程式的可用性。 |

## <a name="enterprise-uwp-samples"></a>企業 UWP 範例

| 主題 |  描述 |
|------ |--------------|
| [VanArsdel 清查範例](https://github.com/Microsoft/InventorySample) | 示範企業營運案例的 UWP 範例應用程式。 此範例主要說明如何為虛構公司 VanArsdel 建立和管理客戶、訂單和產品。 |
| [客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | 這是一個 UWP 範例應用程式，示範對企業開發人員有幫助的功能，例如 Azure Active Directory (AAD) 驗證、UI 控制項 (包括資料格)、Sqlite 與 SQL Azure 資料庫整合、Entity Framework 和雲端 API 服務等。 此範例主要說明如何為虛構公司 Contoso 建立和管理客戶帳戶、訂單和產品。 |

## <a name="patterns-and-practices"></a>模式和做法

大型企業級 app 的程式碼庫會變得雜亂無章。 Prism 是一個架構，用於在 WPF、Windows 10 UWP 和 Xamarin Forms 中建置鬆散結合、可維護和可測試的 XAML 應用程式。 Prism 提供一組設計模式的實作，這組設計模式有助於撰寫結構完善且可維護的 XAML 應用程式，包括 MVVM、相依性導入、命令、EventAggregator 和其他項目。

如需 Prism 的詳細資訊，請參閱 [GitHub 存放庫](https://github.com/PrismLibrary/Prism)。

 

 
