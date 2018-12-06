---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: 此藍圖概述的重要企業功能適用於 windows 10 和通用 Windows 平台 (UWP) app。
title: 企業版
ms.date: 08/30/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6cce98591cdaa78a887d7a5fb495e999a4ffc453
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8756742"
---
# <a name="enterprise"></a>企業版

這篇文章提供 Windows 10 應用程式提供由通用 Windows 平台 (UWP) 中的重要企業功能的概觀。

## <a name="whats-new-and-recent-for-enterprise-applications"></a>什麼是新的和新的企業應用程式

> [!div class="checklist"]
> * [Windows Template Studio](#template-studio)
> * [建立桌面樣式 Ui 控制項](#desktop-style-UI)
> * [控制項，以支援企業案例](#enterprise)
> * [Windows UI 文件庫](#UI-library)
> * [傳統型應用程式中的 UWP 控制項](#xaml-islands)
> * [.NET Standard 2.0](#standard)
> * [SQL Server 連線](#sql-server)
> * [MSIX 部署](#MSIX)

影片示範一些這些功能的詳細資料，請參閱[迅速建構的 LOB 應用程式與 UWP 和 Visual Studio](https://channel9.msdn.com/Events/Build/2018/BRK3502)。

<a id="template-studio" />

### <a name="windows-template-studio"></a>Windows Template Studio

Windows Template Studio 是加速建立新的通用 Windows 平台 (UWP) 應用程式使用精靈為基礎的體驗的 Visual Studio 2017 擴充功能。 產生的 UWP 專案的格式正確、 可讀性的程式碼會同時實作經證實的模式與最佳做法結合的最新的 Windows 10 功能。

![Windows Template Studio](images/windows-template-studio.png)

請參閱[Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio)

<a id="desktop-style-UI" />

### <a name="controls-to-create-desktop-style-uis"></a>建立桌面樣式 Ui 控制項

我們已發行新的 UWP XAML 控制項，填滿傳統桌面應用程式 UI 和 UWP UI 之間的差距。

例如，新的[功能表列](https://review.docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/menus?branch=jimwalk%2Frs5-menu-bar)、 [DropDownButton](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button)、 [SplitButton](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button)，以及[CommandBarFlyout](https://review.docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/command-bar-flyout?branch=jimwalk%2Frs5-command-bar-flyout)控制項提供您更具彈性的方式來公開命令，並[EditableComboBox](https://review.docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/combo-box?branch=rs5#make-a-combo-box-editable)讓我們在使用者輸入並未列出的值在預先定義的選項清單。

![功能表列](images/menu-bar.png)

<a id="enterprise" />

### <a name="controls-to-support-enterprise-scenarios"></a>控制項，以支援企業案例

[DataGridView](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/datagrid)提供彈性的方式，以列和欄中顯示的資料集合。

[樹狀檢視](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view)可讓展開及摺疊節點包含巢狀項目的階層式清單。 此控制項可以用來說明 UI 中的資料夾結構或巢狀關聯性。

![資料格控制](images/DataGrid.gif)


### <a name="windows-ui-library"></a>Windows UI 文件庫

Windows UI 文件庫是一組提供控制項與其他使用者介面元素用於 UWP 應用程式的 NuGet 套件。 讓您的應用程式運作方式，即使使用者沒有最新的作業系統，它也可讓使用較舊版本的 Windows 10 的下層相容性。

![Windows UI 文件庫](images/win-ui.png)

請參閱[Windows UI 程式庫 （預覽版本）](https://docs.microsoft.com/en-us/uwp/toolkits/winui/)。

<a id="xaml-islands" />

### <a name="uwp-controls-in-desktop-applications"></a>傳統型應用程式中的 UWP 控制項

Windows 10 現在可讓您在 WPF、 Windows Forms 和 c + + Win32 傳統型應用程式中使用 UWP 控制項。 這表示您可以提升外觀、 感覺與您現有的傳統型應用程式與最新的 Windows 10 UI 功能只會透過 UWP 控制項，例如 Windows Ink 和 Fluent 設計系統支援的控制項可用的功能。 此功能稱為 XAML 群島。

請參閱[傳統型應用程式中的 UWP 控制項](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)。

<a id="standard" />

### <a name="net-standard-20"></a>.NET Standard 2.0

.NET Standard 包含超過 20000 更多的 Api，比.NET Standard 1.x。 這可讓因此更容易移轉現有的.NET Framework 程式庫，然後將它們使用跨不同包括您的 UWP 應用程式的.NET 應用程式。

![net 標準](images/dot-net-standard-project-template.png)

請參閱[傳統型應用程式與 UWP app 之間共用程式碼](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate)。

<a id="sql-server" />

### <a name="sql-server-connectivity"></a>SQL Server 連線

您的 app 可以直接連接到 SQL Server 資料庫，然後使用 [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient?redirectedfrom=MSDN&view=netframework-4.7.2)命名空間中的類別儲存和擷取資料。

請參閱[在 UWP app 中使用 SQL Server 資料庫](https://docs.microsoft.com/en-us/windows/uwp/data-access/sql-server-databases)。

<a id="MSIX" />

### <a name="msix-deployment"></a>MSIX 部署

MSIX 是提供現代化的封裝體驗，以所有 Windows 應用程式的 Windows 應用程式套件格式。 MSIX 封裝格式會保留現有的應用程式套件的功能，並安裝除了啟用新的現代封裝及部署功能 Win32、 WPF 及 Windows Forms 應用程式的檔案。

MSIX 是安全、 安全且可靠，內建的封裝格式根據.msi、.appx、 APP-V 和 ClickOnce 安裝技術的組合。

![MSIX 圖示](images/MSIX-App-Package.ico)

請參閱[MSIX 文件](https://docs.microsoft.com/windows/msix/)。

<a id="distribution" />

## <a name="security"></a>安全性

Windows 10 提供一套安全性功能，讓應用程式開發人員保護他們的使用者、 公司網路的安全性和儲存在裝置上的任何商務資料的身分識別。 新增適用於 windows 10 功能是 Microsoft Passport、 使用 PIN 的無障礙輕鬆部署雙因素密碼替代方式或 Windows Hello，可提供企業級安全性，以及支援指紋、 臉部和虹膜辨識。

| 主題 | 描述 |
|-------|-------------|
| [安全開發 Windows app 的簡介](https://msdn.microsoft.com/library/windows/apps/mt622741) | 這篇簡介文章說明不同驗證階段 (包括傳輸中資料和靜態資料) 的各種 Windows 安全功能。 它也描述如何將這些階段整合到您的 app。 它涵蓋大範圍的主題，並主要目的是協助應用程式設計師，更清楚地了解可快速且簡單地讓建立通用 Windows 平台 app 的 Windows 功能。 |
| [驗證和使用者識別](https://msdn.microsoft.com/library/windows/apps/mt270184) | UWP app 有本文所述的數個使用者驗證選項。 若要用於企業，則強烈建議選用新的 Microsoft Passport 功能。 Microsoft Passport 以增強式雙因素驗證 (2FA) 取代密碼是驗證現有的認證，並建立的裝置特定認證，生物特徵辨識或 PIN 式使用者手勢所保護，導致兩者便利和高度安全的體驗。 |
| [密碼編譯](https://msdn.microsoft.com/library/windows/apps/mt270191) | 密碼編譯一節概述 UWP app 所提供的密碼編譯功能。 文章的範圍包括從如何輕鬆加密機密商業資料的簡介逐步解說，到操作密碼編譯金鑰，以及使用 MAC、雜湊和簽章這類進階主題。 |
| [Windows 資訊保護 (WIP)](wip-hub.md) | 這是一個中樞主題，從開發人員角度來探討 Windows 資訊保護 (WIP) 與檔案、緩衝區、剪貼簿、網路、背景工作的關聯，以及資料鎖定時的保護。 |

## <a name="data-binding-and-databases"></a>資料繫結和資料庫

資料繫結可讓您的 app UI 顯示來自外部來源 (例如資料庫) 的資料，以及選擇性地與該資料保持同步。 資料繫結可讓您將資料與 UI 分開考量，為 app 建構更簡單的概念模型，以及更好的可讀性、測試性和維護性。

| 主題 | 描述 |
|-------|-------------|
| [資料繫結概觀](https://msdn.microsoft.com/library/windows/apps/mt269383) | 本主題示範如何將控制項 （或其他 UI 元素） 繫結到單一項目，或將項目控制項繫結至通用 Windows 平台 (UWP) 應用程式中的項目集合。 此外，還會示範如何控制項目的呈現、根據選擇來實作詳細資料檢視，以及轉換資料以供顯示。 |
| [Entity Framework 7 for UWP](https://msdn.microsoft.com/library/windows/apps/mt592863) | 對大型資料集執行複雜查詢，可使用支援 UWP 的 Entity Framework 7 進行大幅簡化。 在這個逐步解說中，您將建置對本機 SQLite 資料庫使用 Entity Framework 執行基本資料存取的 UWP 應用程式。 |
| [SQLite 本機資料庫](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | 這個影片是使用 SQLite 的完整開發人員指南，而 SQLite 是本機 app 資料庫的建議方案。 請瀏覽 [SQLite](https://www.sqlite.org/download.html) 以下載 UWP 的最新版本，或使用 Windows 10 SDK 已隨附的版本。 |

## <a name="networking-and-data-serialization"></a>網路功能和資料序列化

企業營運 app 通常需要與各種其他系統通訊，或在各種其他系統上儲存資料。 做法通常是連線到網路服務 (使用 REST 或 SOAP 這類通訊協定)，然後序列化資料或將資料還原序列化成通用格式。 在與 WPF、WinForm 和 ASP.NET 應用程式類似的 UWP app 中，使用網路和資料序列化。 如需詳細資訊，請參閱下列文章。

| 主題 | 描述 |
|-------|-------------|
| [網路功能基本知識](https://msdn.microsoft.com/library/windows/apps/mt280233) | 這個逐步解說說明與所有 UWP app 相關的基本網路概念，而不管使用中的通訊協定為何。  |
| [哪一種網路功能技術？](https://msdn.microsoft.com/library/windows/apps/mt280235) | 適用於 UWP app 的網路功能技術快速概觀，並建議您如何選擇最適合您的 app 的技術。 |
| [XML 和 SOAP 序列化](https://msdn.microsoft.com/library/90c86ass.aspx) | XML 序列化會將物件轉換成符合特定 XML 結構描述定義語言 (XSD) 的 XML 資料流。 若要在 XML 與強型別類別之間進行轉換，您可以使用原生 [XDocument](https://msdn.microsoft.com/library/system.xml.linq.xdocument.aspx) 類別或外部程式庫。 |
| [JSON 序列化](https://msdn.microsoft.com/library/windows/apps/br240639) | JSON （JavaScript 物件標記法） 序列化是與 REST Api 進行通訊的常用格式。 UWP app 完全支援的 [Newtonsoft Json.NET](http://www.newtonsoft.com/json)。 |

## <a name="devices"></a>裝置

為了與企業營運工具 (如印表機、條碼掃描器或智慧卡讀卡機) 整合，您可能會發現有必要將外部裝置或感應器與您的 app 整合。 以下是一些功能範例，您可以使用本節所描述的技術將這些功能新增到您的 app 中。

| 主題  | 描述 |
|--------|-------------|
| [列舉裝置](https://msdn.microsoft.com/library/windows/apps/mt187355) | 本文說明如何使用 [Windows.Devices.Enumeration](https://msdn.microsoft.com/library/windows/apps/br225459) 命名空間尋找內部連接到系統、外部連接或者可透過無線或網路通訊協定偵測到的裝置。 如果您是建置任何與裝置搭配運作的 app，請從這裡開始。 |
| [列印與掃描](https://msdn.microsoft.com/library/windows/apps/mt204544) | 說明如何從您的應用程式，包括連接和使用企業裝置，例如銷售點 (POS) 系統、 收據印表機，以及高容量送紙器掃描器列印與掃描。 |
| [藍牙](https://msdn.microsoft.com/library/windows/apps/mt270288) | 除了使用傳統藍牙連線來傳送和接收資料或控制裝置，Windows 10 還可使用藍牙低功耗 (BTLE) 在背景傳送或接收指標。 使用這個項目，以在使用者接近或離開特定位置時顯示通知或啟用功能。 |
| [企業共用存放裝置](enterprise-shared-storage.md) | 在裝置鎖定案例中，了解如何在相同的應用程式內、應用程式執行個體之間或甚至應用程式之間共用資料。 |

## <a name="device-targeting"></a>裝置目標

許多使用者現在都會將他們的手機或平板電腦帶去上班，而手機或平板電腦的板型規格和螢幕大小都不同。 您可以使用通用 Windows 平台 (UWP) 撰寫在所有不同類型的裝置上流暢執行的單一企業營運 app (包括桌上型電腦電腦和 PPI 顯示器)，可讓您最接近 app 並將程式碼效率提到最高。

| 主題 | 描述 |
|-------|-------------|
| [UWP app 指南](https://msdn.microsoft.com/library/windows/apps/dn894631) | 在本簡介指南中，您將了解 Windows 10UWP 平台，包括︰裝置系列為何以及如何決定要設為目標的裝置系列、新的 UI 控制項和面板以讓您將 UI 調整為不同的裝置板型規格，以及如何了解與控制可供您的 app 使用的 API 表面。 |
| [彈性 XAML UI 程式碼範例](http://go.microsoft.com/fwlink/p/?LinkId=619992) | 這個程式碼範例顯示所有可能版面配置選項與您的應用程式，不論裝置類型的控制項，並可讓您能夠與顯示如何達成您正在尋找的任何版面配置面板互動。 除了顯示每個控制項如何回應不同的板型規格之外，app 本身也具有回應，並顯示達成彈性 UI 的各種方法。 |
| [Xamarin 主題]() | 適用於目標手機 Xamarin |

## <a name="deployment"></a>部署

您有許多將 app 發佈至組織使用者的選項。 您可以使用現有的行動裝置管理商務用 Microsoft 網上商店，或您可以側載到裝置的應用程式。 您也可以讓您的應用程式提供給一般公用發佈至 Microsoft Store。

| 主題 | 描述 |
|-------|-------------|
| [將 LOB 應用程式發佈到企業](https://msdn.microsoft.com/library/windows/apps/mt608995) | 您可以直接到企業來進行大量取得，透過企業版，Microsoft Store 發佈特定業務的應用程式，而不需要讓大眾廣泛提供應用程式。 |
| [側載 app](https://technet.microsoft.com/library/mt269549) | 當您側載 app 時，您要將簽署的 app 套件部署到裝置。 您要維護這些 app 的簽署、裝載和部署。 用於側載 app 的程序已經簡化成適用於 Windows 10。             |
| [將應用程式發佈到 Microsoft 網上商店](https://dev.windows.com/publish) | 整合的 Microsoft Store 可讓您發佈與管理所有您的應用程式，適用於所有 Windows 裝置。 透過每個市場價格、發佈和可見性控制項，以及其他選項來自訂您 app 的可用性。 |

## <a name="enterprise-uwp-samples"></a>企業 UWP 範例

| 主題 |  描述 |
|------ |--------------|
| [VanArsdel 詳細目錄範例](https://github.com/Microsoft/InventorySample) | UWP 範例應用程式，展示特定業務的案例。 範例是以建立和管理客戶、 訂單及產品為虛構公司 VanArsdel 為基礎。 |
| [客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | UWP 範例應用程式，展示對企業開發人員，例如 Azure Active Directory (AAD) 驗證、 UI 控制項 （包括資料格）、 Sqlite 和 SQL Azure 資料庫整合、 Entity Framework 和 API 的雲端服務很有用的功能。 範例是以建立和管理客戶帳戶、 訂單及產品為虛構公司 Contoso 為基礎。 |

## <a name="patterns-and-practices"></a>模式和做法

大型企業級 app 的程式碼庫會變得雜亂無章。 Prism 是建置鬆散結合、 可維護和可測試的 XAML 應用程式在 WPF、 windows 10 UWP 和 Xamarin Forms 中的架構。 Prism 提供一組設計模式的實作，這組設計模式有助於撰寫結構完善且可維護的 XAML 應用程式，包括 MVVM、相依性導入、命令、EventAggregator 和其他項目。

如需 Prism 的詳細資訊，請參閱 [GitHub 存放庫](https://github.com/PrismLibrary/Prism)。

 

 
