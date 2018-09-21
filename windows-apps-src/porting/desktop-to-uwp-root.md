---
author: normesta
Description: Create a modern Windows app package for your existing Windows Forms, WPF, or Win32 app or game. Add modern experiences for Windows 10 users and simplify deployment and monitization.
Search.Product: eADQiWindows 10XVcnh
title: 傳統型橋接器
ms.author: normesta
ms.date: 05/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.openlocfilehash: 9ded8fb8a9d391ec48b46b0795b901dc403e1f30
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/21/2018
ms.locfileid: "4111586"
---
# <a name="desktop-bridge"></a>傳統型橋接器

運用現有的傳統型應用程式，新增適用於 Windows 10 使用者的現代化體驗。 接著，透過 Microsoft Store 散佈您的應用程式，達到跨國際市場的更大覆蓋率。 您可以利用 Store 內建的功能，以更簡單的方式從您的應用程式獲利。 當然，您不一定需要使用 Store。 歡迎使用您現有的通道。

![傳統型橋接器](images/desktop-to-uwp/desktop-bridge-4.png)

傳統型橋接器是我們平台內建的基礎結構，可讓您使用現代化 Windows 應用程式套件，有效率地散佈您的 Windows Forms、WPF 或 Win32 傳統型應用程式或遊戲。

此套件可提供一個身分識別給您的應用程式，透過該身分識別，您的傳統型應用程式得以存取 Windows 通用平台 (UWP) API。 您可以使用它們來提供現代化的吸引人體驗，例如動態磚和通知。  只有當您的應用程式在 Windows 10 上執行時，才能使用簡單的條件編譯和執行階段檢查來執行 UWP 程式碼。

除了用來提供 Windows 10 體驗的程式碼，您的應用程式仍保持不變，您可以繼續將應用程式散佈給現有的 Windows 7、Windows Vista 或 Windows XP 使用者基礎。 在 Windows 10 上，您的應用程式會一如往常，繼續以完全信任使用者模式執行。

>[!IMPORTANT]
>傳統型橋接器在 Windows 10 (版本 1607) 中引進，只適用於 Visual Studio 中目標為 Windows 10 年度更新版 (10.0；組建 14393) 或更新版本的專案。

> [!NOTE]
> 請查看<a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">此系列</a>由 Microsoft Virtual Academy 發行的簡短影片。 這些影片將會逐步解說，協助您將您的傳統型應用程式帶往通用 Windows 平台 (UWP)。

## <a name="benefits"></a>權益

以下是一些為傳統型應用程式建立 Windows 應用程式套件的理由︰

:heavy_check_mark: **簡化部署**。 使用橋接器的應用程式和遊戲具備了良好的部署體驗。 此種體驗可讓使用者充滿信心的安裝和更新應用程式。 如果使用者選擇解除安裝應用程式，系統將會完全移除它，且不會留下任何痕跡。 這能縮短編寫安裝體驗的時間，並讓使用者擁有最新的更新。

:heavy_check_mark: **自動更新和授權**。 您的 App 可以參與 Microsoft Store 的內建授權和自動更新功能。 由於自動更新只會下載檔案已變更的部分，因此是一項非常可靠又有效率的機制。

:heavy_check_mark: **增加觸達對象並簡化營利**。 選擇透過 Microsoft Store 來散布，可讓您的觸達對象擴展至數百萬的 Windows10 使用者，而他們可以利用當地的付款選項來取得應用程式、遊戲，並進行 App 內購買。

:heavy_check_mark: **增加 UWP 功能**。  依照您自己的步調，您可以將 UWP 功能新增到您的應用程式套件，例如 XAML 使用者介面、動態磚更新、UWP 背景工作、應用程式服務，以及其他更多項目。

:heavy_check_mark: **跨裝置擴展使用案例**。 使用橋接器，您可以逐漸將程式碼移轉到通用 Windows 平台，以觸達所有的 Windows10 裝置類型，包括手機、Xbox One 和 HoloLens。

若要檢視其帶來優點的更完整清單，請參閱[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)。

## <a name="prepare"></a>準備

首先，準備好您的應用程式，為此請檢閱[準備封裝傳統型應用程式](desktop-to-uwp-prepare.md)文章，並在建立其 Windows 應用程式套件之前解決任何會影響應用程式的問題。 在建立套件之前，您也有可能不需要對您的應用程式進行任何變更。 然而，在某些情況下，您可能仍需要在建立套件之前調整您的應用程式。

<a id="convert" />

## <a name="package"></a>套件

以下是一些您可以用來為您的應用程式建立 Windows 應用程式套件的工具。

### <a name="desktop-app-converter"></a>Desktop App Converter

雖然此工具的名稱中出現「Converter」(轉換器) 這個詞，但它並不會轉換您的應用程式。 您的應用程式會保持不變。 不過，這項工具會為您產生 Windows 應用程式套件。 此工具在您需要進行大量系統修改，或是您不確定您的安裝程式會進行何種工作的情況下將會非常方便。

Desktop App Converter 會將您安裝程式的動作轉譯成您應用程式已封裝版本將使用的虛擬檔案及登錄系統。 Desktop App Converter 也會為您完成其他幾個項目。 以下是一些範例。

:heavy_check_mark: 自動註冊您的預覽處理常式、縮圖處理常式、屬性處理常式、防火牆規則、URL 旗標。

:heavy_check_mark: 自動註冊檔案類型對應，讓使用者可以在檔案總管中使用 **\[類型\]** 欄位群組檔案。

:heavy_check_mark: 註冊您的公用 COM 伺服器。

: heavy_check_mark： 產生憑證，您可以用來執行您的應用程式。

:heavy_check_mark: 驗證您的應用程式是否違反傳統型橋接器和 Microsoft Store 的需求。

另一個使用 Desktop App Converter 的更好理由是如果您使用 Visual Studio 以外的開發環境來維護您的應用程式。 即使您的應用程式沒有安裝程式，您還是可以使用 Desktop App Converter。

請參閱[使用 Desktop App Converter 封裝應用程式 (傳統型橋接器)](desktop-to-uwp-run-desktop-app-converter.md)

### <a name="visual-studio"></a>Visual Studio

如果您使用 Visual Studio 來維護應用程式，而且您的應用程式沒有安裝程式或您的安裝程式不會執行太多複雜的工作，請考慮將改為使用 Visual Studio。

使用 Visual Studio 建立套件非常簡單。 您只需加入封裝專案、參考桌面專案，然後按 F5 對您的應用程式進行偵錯。 不需進行任何手動調整。 這個有效率的全新體驗可以大幅改善舊版 Visual Studio 中所提供的體驗。 以下是您能使用它來做到的一些其他事情。

:heavy_check_mark: 自動產生視覺資產。

:heavy_check_mark: 使用視覺化設計工具來變更您的資訊清單。

:heavy_check_mark: 使用精靈產生您的套件。

:heavy_check_mark: 從 Windows 開發人員中心儀表板中您已保留的名稱，輕鬆指派身分識別給您的應用程式。

請參閱[使用 Visual Studio 封裝 .NET 應用程式 (傳統型橋接器)](desktop-to-uwp-packaging-dot-net.md)

### <a name="third-party-installer"></a>協力廠商安裝程式

 一些受歡迎的協力廠商產品及安裝程式現在也支援傳統型橋接器。 您可以使用他們來產生 MSI 安裝程式，或是透過只按幾下來完成應用程式套件。 雖然我們不會提供關於這些工具使用方式的說明文件，您還是可以瀏覽他們的網站以深入了解。

#### <a name="advanced-installer"></a>Advanced Installer

Caphyon 提供免費、GUI 式的傳統型應用程式封裝工具，協助您透過只按幾下來為您的應用程式產生 Windows 應用程式套件。 它可以使用任何安裝程式。即使是以無訊息 (silent) 模式執行的安裝程式也能，並能執行驗證檢查以確定應用程式適合封裝。
Desktop App Converter 也整合了 Hyper-V 和 [VMWare](http://www.vmware.com/)。 這表示，您可以使用您自己的虛擬電腦，而不需要下載大小可能超過 3 GB 的相對應[Docker](https://docs.docker.com/) 映像。

<img width="20%" src="images/desktop-to-uwp/Advanced_Installer_Vertical.png">

您可以使用 [Advanced Installer](http://www.advancedinstaller.com/) 從現有的專案產生 MSI 和 [Windows 應用程式套件](http://www.advancedinstaller.com/uwp-app-package.html)。 您也可以使用 Advanced Installer 匯入您使用 Microsoft Desktop App Converter 產生的 Windows 應用程式套件。 匯入之後，你就可以使用專為 UWP app 設計的視覺工具維持他們。

Advanced Installer 也提供了適用於 Visual Studio 2017 及 2015 的延伸模組，可用於[建置和偵錯傳統型橋接器應用程式](http://www.advancedinstaller.com/debug-desktop-bridge-apps.html)。

請查看[這個影片](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be)以獲得快速概觀。

> [!TIP]
> 請務必查看最近發行的[進階安裝程式 Express 版](https://www.advancedinstaller.com/express-edition.html)。

#### <a name="cloudhouse-compatibility-containers"></a>Cloudhouse Compatibility Containers

對於其企業營運應用程式不相容於 Windows 10 和 Windows 10 S 的企業客戶，Cloudhouse Compatibility Containers 可讓 Windows XP 和 7 應用程式在 Windows 10 上執行，並轉換為在 通用 Windows 平台 (UWP) 上執行，然後透過商務用 Microsoft Store 或 Microsoft InTune 提供，而無須更動原始碼。 請註冊以取得[免費試用](http://www.cloudhouse.com/free-trial)。

<img width="20%" src="images/desktop-to-uwp/cloudhouse-container-logo.png">

Cloudhouse 提供 Auto Packager，可將企業營運應用程式封裝為現今應用程式所執行作業系統 (例如，Windows XP) 上的[相容性容器](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications)，以及[準備轉換](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905)為 UWP。 接著可將容器整合至 Microsoft Desktop App Converter 工具以轉換為新的 Windows 應用程式套件格式。

Auto Packager 使用安裝/擷取和執行階段分析來建立應用程式的容器，其中包括應用程式檔案、登錄、執行階段及相依性，以及能讓應用程式在 Windows 10 上執行所需的相容性及重新導向引擎。 容器提供應用程式及其執行階段的隔離，因此它們不會影響或與其他在使用者裝置上執行的應用程式相衝突。

如需進一步了解如何透過商務用 Microsoft Store 提供商務應用程式，請閱讀我們的[發行部落格](http://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp)。

#### <a name="firegiant"></a>FireGiant

[FireGiant Appx 擴充功能](https://www.firegiant.com/products/wix-expansion-pack/appx)可讓您從相同的 WiX 原始碼同時建立 Windows 應用程式套件和 MSI 套件。 每次組建時，您可以針對 Windows 10 中的傳統型橋接器搭配 Windows 應用程式套件和舊版 Windows 與 MSI。

<img width="20%" src="images/desktop-to-uwp/FG3rdPartyLogo.png">

FireGiant Appx 擴充功能使用靜態分析及 WiX 專案的智慧型模擬，建立不需磁碟空間和容器或虛擬機器之執行階段額外負荷的 Windows 應用程式套件。

由於 FireGiant Appx 擴充功能不會透過執行來轉換您的安裝程式，因此您可以維持 WiX 安裝程式，而不需要將其重複轉換成 Windows 應用程式套件。 在不同版本 Windows 上的所有使用者都能取得最新改進，您不必擔心 MSI 和 Windows 應用程式套件無法同步。

請查看此[影片](https://www.youtube.com/watch?v=AFBpdBiAYQE)，了解 FireGiant CEO Rob Mensching 如何使用幾行程式碼就建立熱門開放原始碼 7-Zip 壓縮工具的 Appx (Windows 應用程式套件) 版本，接著如何在相同 WiX 原始碼中進行變更，改進 Windows 應用程式和 MSI 套件。

#### <a name="installaware"></a>InstallAware

具有[追蹤記錄](https://www.installaware.com/press-room.htm)的 Install**Aware** 可快速支援 Microsoft 創新，從單一來源建置 [Windows 應用程式套件 (傳統型橋接器)](https://www.installaware.com/appx-builder.htm)、App-V (Application Virtualization)、MSI (Windows Installer)，以及 EXE (原生程式碼) 套件。

<img width="20%" src="images/desktop-to-uwp/installaware.png">

Install**Aware** 提供適用於 Visual Studio 版本 2012-2017 的免費 Install**Aware** 擴充功能。 使用它們，直接從 [Visual Studio 工具列](https://www.installaware.com/visual-studio-installer-2015.htm)點按一下即可建立 Windows 應用程式套件。

您也可以使用 Package**Aware** (無須快照的設定擷取) 或 Database Import Wizard (適用於所有 MSI 安裝程式與 MSM 合併模組) 匯入任何設定，即使您沒有該設定的原始碼。 您可以使用 [GUI 工具](https://www.installaware.com/scripting-two-way-integrated-ide.htm)，以視覺化方式或透過指令碼維護與增強匯入。

[進階 APPX 建立選項](https://www.installaware.com/mhtml5/desktop/appx.htm)可協助您針對 Microsoft Store 提交，或產生已簽署的 Windows 應用程式套件二進位檔以便側載散佈給使用者。 您甚至可以透過單一來源建立 **WSA** (Windows Server 應用程式) 安裝程式套件並鎖定部署至 **Nano Server**，以及除了 GUI 以外，還能獲得[命令列自動化](https://www.installaware.com/scripting-automation-interface.htm)的完整支援。

Install**Aware** 也[以開放原始碼的方式提供](https://www.installaware.com/gnu.asp)** APPX 建立器程式庫**，加上範例命令列小程式，一起以 GNU Affero GPL 授權提供。 這些專供開放原始碼平台 (例如 WiX) 搭配使用。

#### <a name="installshield"></a>InstallShield

InstallShield 提供單一解決方案，用以開發 MSI 和 EXE 安裝程式、建立通用 Windows 平台 (UWP) 與 Windows Server 應用程式 (WSA) 套件，以及使用最少指令碼、程式設計和重做即可虛擬化應用程式。

<img width="20%" src="images/desktop-to-uwp/InstallShield-logo.jpg">

數秒內即可掃描您的 InstallShield 專案，自動找出的應用程式和 UWP 與 WSA 套件之間潛在的相容性問題，節省數小時的調查工作。

從現有的 InstallShield 專案建置 UWP 應用程式套件，替 Microsoft Store 做好準備並簡化您軟體在 Windows 10 上的安裝體驗。 可同時建置 Windows Installer 和 UWP 應用程式套件，支援您客戶想要的全部部署案例。 從您現有的 InstallShield 專案建置 WSA 套件，支援 Nano Server 與 Windows Server 2016 部署。

在模組中開發安裝，讓部署及維護更加輕鬆，然後在建置階段將元件和相依性合併至適用於 Microsoft Store 的單一 UWP 應用程式套件。 對於 Store 外的直接散佈，使用 Suite/Advanced UI 安裝程式將您的 UWP 應用程式套件和其他相依性一起組合起來。

請閱讀此[電子書](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0)以深入了解。

#### <a name="pace-suite"></a>PACE Suite

[PACE Suite](https://pacesuite.com/) 是一種應用程式封裝工具，您可用來將傳統型應用程式轉換成通用 Windows 平台。

<img width="20%" src="images/desktop-to-uwp/PACE.png">

使用 PACE Suite，就不需要準備特殊封裝環境或安裝其他 Windows SDK 元件。 PACE Suite 可以在 Windows 10 或 Windows Server 2016 的標準封裝環境中獨立建置 Windows 應用程式套件。 請查看此[圖解範例](https://pacesuite.com/convert-exe-to-appx/)以了解 PACE Suite 如何將安裝程式重新封裝為 Windows 應用程式套件。

除了建立 Windows 應用程式套件，您也可以使用 PACE Suite 建立 Windows Installer 套件 (MSI)、修補程式 (MSP)、轉換 (MST) 和 APP-V 套件。 編撰 MSI 時，PACE Suite 可協助管理升級、權限設定、自訂動作、指令碼及其他項目。 您也可以將您的應用程式直接發佈至 System Center Configuration Manager。

若要檢視所有應用程式封裝功能，請參閱 [PACE Suite 功能](https://pacesuite.com/features/)。

#### <a name="rad-studio"></a>RAD Studio

請參閱 [RAD Studio by Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

#### <a name="raypack-studio"></a>RayPack Studio

Raynet 的封裝解決方案 [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio) 支援傳統型橋接器，做為有效率且易於設定之轉換及重新封裝架構的幾個可能途徑之一。

<img width="20%" src="images/desktop-to-uwp/RaynetLogo_v3.png">

現有虛擬環境 (VMWare Workstation、Hyper-V) 無需冗長環境設定，即可用於執行自動化/大量轉換。 此製作公司的元件 ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)) 可以進行預先轉換檢測和相容性測試來確認軟體符合轉換的條件。 此外，使用者現在可以執行與各種 Windows 10 版本 (包括年度更新版和 Creators Update) 的完整碰撞及相容性檢查。

除了不能建立 Windows 10 APPX/UWP 格式的軟體套件之外，RayPack Studio 也可以用來建立傳統 Windows Installer 套件 (MSI)、修補程式 (MSP)、轉換 (MST) 和 App-V 套件。 此外，此解決方案還隨附一組適用於企業專業軟體封裝的軟體產品及元件。 除了封裝軟體和虛擬化之外，RayPack Studio 也顧慮到所有封裝相關工作上的需要：軟體應用程式與套件的衝突及相容性檢查 ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad))、軟體評估 ([RayEval](https://raynet.de/Raynet-Products/RayEval)) 和品質保證 ([RayQC](https://raynet.de/Raynet-Products/RayQC))。

如果與 [RayFlow](https://raynet.de/Raynet-Products/RayFlow) 這個 Raynet 的企業工作流程系統搭配使用，使用者就可以從安排封裝順序一直到評估、分析、封裝、品質保證、使用者接受度測試和部署，有效率地在整個企業應用程式生命週期中處理軟體。 所有套件和格式都可以直接儲存和部署到 SCCM 或其他解決方案中。 RayFlow 會追蹤和管理整個應用程式生命週期流程。 此外，還可以整合任何訂單系統，例如 ServiceNow。 Raynet 憑藉其供應服務提供者的工具，在世界各地建立軟體封裝廠。

為了讓自己確信，請取得 Raynet 的 RayPack Studio and RayFlow [免費試用授權](https://raynet.de/contact?init=license)。 如需詳細資訊，請瀏覽 [www.raynet.de](https://raynet.de/home)。

**相關連結**：

* Raynet：[https://raynet.de/home](https://raynet.de/home)
* RayPack Studio：[https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow：[https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval：[https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC：[https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced：[https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* 免費試用授權：[https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)

### <a name="manual-packaging"></a>手動封裝

您還有一個最後選擇，就是不使用上述任一工具來轉換您的應用程式。 若您希望細微控制您的轉換，您可以建立一個資訊清單檔，然後執行 **MakeAppx.exe** 工具來建立您的 Windows 應用程式套件。

請參閱[手動封裝應用程式 (傳統型橋接器)](desktop-to-uwp-manual-conversion.md)。

## <a name="integrate"></a>整合

如果您的應用程式需要與系統整合 (例如：建立防火牆規則)，請在應用程式的封裝資訊清單中描述這些項目，系統會替您完成其餘的工作。 針對大部分的工作，您完全不需要撰寫任何程式碼。 只需在資訊清單中提供一些 XML，您就可以執行一些工作，像是在使用者登入時執行處理程序、將您的應用程式與檔案總管整合，以及將您的應用程式加入在其他應用程式中出現的列印目標清單。

請參閱[整合您的應用程式與 Windows 10 (Windows 傳統型橋接器)](desktop-to-uwp-extensions.md)。

## <a name="enhance"></a>增強

封裝您的應用程式之後，您就可以使用一些功能，例如：動態磚，以及推播通知等美化您的應用程式。 有些功能可大幅改善您應用程式的參與程度，而且新增他們只需要花費您一點時間。 一些增強功能則可能需要多一點程式碼。

請參閱[增強您的 Windows 10 傳統型應用程式](desktop-to-uwp-enhance.md)。

## <a name="extend"></a>擴充

有些 Windows 10 體驗 (例如：具有觸控功能的 UI 頁面) 必須在現代化應用程式容器中執行。 一般而言，您應該先判斷是否可以使用 UWP API 透過[增強](desktop-to-uwp-enhance.md)現有的傳統型應用程式來新增體驗。 如果您必須使用 UWP 元件來達成體驗，則可以將 UWP 專案加入您的方案並使用應用程式服務在您的傳統型應用程式和 UWP 元件之間通訊。

請參閱[使用現代化 UWP 元件擴充您的傳統型應用程式](desktop-to-uwp-extend.md)。

## <a name="migrate"></a>移轉

沒有工具可將傳統型應用程式轉換為 UWP app 時，您可以重複使用許多現有的程式碼，而這樣還會降低建置應用程式的成本。 做法是盡量將最多的商務邏輯移到 .NET Standard 2.0 程式庫中。

.NET Standard 2.0 包含大量增加的 .NET API 以及您最愛 NuGet 套件和協力廠商程式庫的相容性填充碼。

將您的程式碼移轉至 .NET Standard 程式庫，然後建立通用 Windows 平台 (UWP) app，將適用範圍擴及所有 Windows 10 裝置。

請參閱[在傳統型應用程式與 UWP app 之間共用程式碼](desktop-to-uwp-migrate.md)


## <a name="test"></a>測試

當您準備好在逼真的設定下測試您的應用程式以準備散布時，我們建議您簽署您的應用程式並安裝它。 請參閱[測試您的應用程式](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-debug#test-your-app)。

>[!IMPORTANT]
> 如果您計劃將您的應用程式發行至 Microsoft Store，請確定應用程式可在執行 Windows 10 (S 模式) 的裝置上正確運作。 這是 Microsoft Store 的要求條件。 請參閱[針對 Windows 10 S 模式測試您的 Windows 應用程式](desktop-to-uwp-test-windows-s.md)。

## <a name="validate"></a>驗證

為了讓您的 app 能順利在 Microsoft Store 上發行或成為 [Windows 認證](http://go.microsoft.com/fwlink/p/?LinkID=309666)，請在送出以進行認證之前，先在本機進行驗證和測試。

若您正在使用 DAC 封裝您的應用程式，您可以使用新的 ``-Verify`` 旗標來驗證您的套件是否違反傳統型橋接器和 Store 的需求。 請參閱[封裝應用程式，簽署應用程式，並準備提交 Store](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters)。

若您正在使用 Visual Studio，您可以從 **\[建立應用程式套件\]** 精靈中驗證您的應用程式。 請參閱[建立應用程式套件上傳檔案](../packaging/packaging-uwp-apps.md#create-an-app-package-upload-file)。

若要手動執行工具，請參閱 [Windows 應用程式認證套件](../debug-test-perf/windows-app-certification-kit.md)。

若要檢閱 Windows 應用程式認證用來驗證您應用程式的測試清單，請參閱 [Windows 傳統型橋接器應用程式測試](../debug-test-perf/windows-desktop-bridge-app-tests.md)。

## <a name="distribute"></a>散發

您可以透過將您的應用程式發行至我們的 Microsoft Store，或是側載至其他系統來散佈您的應用程式。

請參閱[散佈封裝的傳統型應用程式 (傳統型橋接器)](desktop-to-uwp-distribute.md)。

## <a name="support-and-feedback"></a>支援與意見反應

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

## <a name="in-this-section"></a>本節內容

| 主題 | 描述 |
|-------|-------------|
| [準備封裝應用程式](desktop-to-uwp-prepare.md) | 提供項目清單，可讓您在封裝傳統型應用程式前檢閱。 |
| [使用 Desktop App Converter 封裝應用程式 (傳統型橋接器)](desktop-to-uwp-run-desktop-app-converter.md) | 說明如何執行 Desktop App Converter。 |
| [手動封裝應用程式 (傳統型橋接器)](desktop-to-uwp-manual-conversion.md) | 了解如何手動建立應用程式套件和資訊清單。 |
| [使用 Visual Studio 封裝 .NET 應用程式 (傳統型橋接器)](desktop-to-uwp-packaging-dot-net.md)| 告訴您如何使用 Visual Studio 封裝傳統型應用程式。 |
| [整合您的應用程式與 Windows 10 (傳統型橋接器)](desktop-to-uwp-extensions.md) | 使用封裝專案的封裝資訊清單檔案中描述的工作，將您的應用程式整合至 Windows 10。 |
| [增強您的 Windows 10 傳統型應用程式](desktop-to-uwp-enhance.md)| 使用 UWP API 新增適用於 Windows 10 使用者的現代化體驗。 |
| [UWP API 適用於已封裝的傳統型應用程式 (傳統型橋接器)](desktop-to-uwp-supported-api.md) | 查看您可以針對已封裝的傳統型應用程式使用哪些 UWP API。 |
| [使用現代化 UWP 元件擴充您的傳統型應用程式](desktop-to-uwp-extend.md)| 新增必須在 UWP app 容器中執行的進階體驗。 使用 App 服務，將您的傳統型應用程式連接至 UWP 處理程序。|
| [執行、偵錯以及測試封裝的傳統型橋接器應用程式 (傳統型橋接器)](desktop-to-uwp-debug.md) | 說明可用來偵錯已封裝之應用程式的選項。 |
| [散佈封裝的傳統型應用程式 (傳統型橋接器)](desktop-to-uwp-distribute.md) | 查看如何將已轉換的應用程式散佈給使用者。  |
| [傳統型橋接器的幕後作業 (傳統型橋接器)](desktop-to-uwp-behind-the-scenes.md) | 深入了解傳統型橋接器背後的運作方式。 |
| [已知問題 (傳統型橋接器)](desktop-to-uwp-known-issues.md) | 列出傳統型橋接器的已知問題。 |
| [傳統型橋接器程式碼範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | GitHub 上示範轉換應用程式功能的程式碼範例。 |
