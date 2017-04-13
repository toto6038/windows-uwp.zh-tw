---
author: normesta
Description: "開始使用傳統型轉 UWP 橋接器並將 Windows 傳統型應用程式 (例如 Win32、WPF 及 Windows Forms) 轉換為通用 Windows 平台 (UWP) App。"
Search.Product: eADQiWindows 10XVcnh
title: "傳統型轉通用 Windows 平台 (UWP) 橋接器"
ms.author: normesta
ms.date: 03/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.openlocfilehash: fcdc5ca446c1387a960838f420f05f9ca597d654
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-universal-windows-platform-uwp-bridge"></a>傳統型轉通用 Windows 平台 (UWP) 橋接器

開始使用傳統型轉 UWP 橋接器並將 Windows 傳統型應用程式轉換為通用 Windows 平台 (UWP) App。

傳統型橋接器是一組可讓您將 Windows 傳統型應用程式 (例如 Win32、Windows Forms 或 WPF) 或遊戲轉換為 UWP App 或遊戲的技術。 轉換之後，您的 Windows 傳統型應用程式就會以目標為 Windows10 Desktop 的 UWP 應用程式套件形式 (.appx 或 .appxbundle) 來封裝、提供服務及部署。

將傳統型應用程式轉換為 UWP 套件的技術分為兩個部分。 第一個部分是轉換程序，它會將您現有的二進位檔重新封裝為 UWP 套件。 您的程式碼仍然相同，只是以不同方式封裝。 第二個部分是由 Windows 年度更新版中的執行階段技術所組成，可讓 UWP 套件擁有以完全信任方式執行而不是在應用程式容器中執行的可執行檔。 這項技術也會為已轉換的 App 提供套件識別資料，需要有此識別資料才能使用某些 UWP API。

## <a name="benefits"></a>優點

以下是一些轉換 Windows 傳統型應用程式的好處：

**簡化部署**。 使用橋接器的 App 和遊戲有絕佳的部署體驗，可確保使用者能放心地安裝及更新。 如果使用者選擇解除安裝 App，系統將會完全移除它，且不會留下任何痕跡。 這能縮短編寫安裝體驗的時間，並讓使用者擁有最新的更新。

**自動更新和授權**。 您的 App 可以參與 Windows 市集的內建授權和自動更新功能。 由於自動更新只會下載檔案已變更的部分，因此是一項非常可靠又有效率的機制。

**增加觸達對象並簡化營利**。 選擇透過 Windows 市集來散發，可讓您觸達數百萬的 Windows10 使用者，而他們可以利用當地的付款選項來取得 App、遊戲，並進行 App 內購買。

**新增 UWP 功能**。  依照您自己的步調，您可以將 UWP 功能新增到您的應用程式套件，例如 XAML 使用者介面、動態磚更新、UWP 背景工作、應用程式服務，以及其他更多項目。

**跨裝置擴展使用案例**。 使用橋接器，您可以逐漸將程式碼移轉到通用 Windows 平台，以觸達所有的 Windows10 裝置類型，包括手機、Xbox One 和 HoloLens。

## <a name="prepare"></a>準備

傳統型轉 UWP 橋接器相當便於使用，因此您可能不需要針對 App 的轉換程序做出太多準備。 不過，在轉換前有一些事項和特別的情況需要注意。 請參閱[針對傳統型轉 UWP 橋接器準備 App](desktop-to-uwp-prepare.md) 文章，並在繼續之前解決任何適用於您 App 的問題。

## <a name="convert"></a>轉換

您有數個不同選項可用來轉換 App。

**Desktop App Converter (DAC)**。 DAC 是會為您自動轉換並簽署 app 的工具。 使用 DAC 相當簡便且自動化，如果您的 App 會進行許多系統修改，或者您對於安裝程式所做的一切有任何不確定性，這相當有用。 若要開始，請參閱 [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) 上的文章。

**手動轉換**。 如果您的 app 是使用 xcopy 所安裝，或是您已經熟悉安裝程式對系統所做的變更，則手動轉換可能是更簡單的選擇。 這包括建立資訊清單檔案，執行 MakeAppx.exe 工具，然後簽署應用程式套件。 如需如何手動轉換的詳細資料，請參閱[使用傳統型橋接器將您的應用程式手動轉換成 UWP](desktop-to-uwp-manual-conversion.md)。

**協力廠商安裝程式**。 數個常見的協力廠商產品和安裝程式現在支援傳統型橋接器，而且只要透過幾個步驟就能產生 MSI 安裝程式或已轉換的應用程式套件。 部分選項包括：

* [Flexera 的 InstallShield](http://www.flexerasoftware.com/producer/products/software-installation/installshield-software-installer)
* [FireGiant 的 WiX](https://www.firegiant.com/r/appx)
* [Caphyon 的 Advanced Installer](http://www.advancedinstaller.com/uwp-app-package)
* [Embarcadero 的 RAD Studio](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)
* [InstallAware](https://www.installaware.com/appx.htm)

如需詳細資訊，請個別瀏覽每個安裝程式的網站。

## <a name="enhance"></a>增強

您可以使用各種 UWP API 來新增如動態磚、推播通知等功能，以增強已轉換傳統型應用程式的功能。 如需完整程式碼範例，請參閱 GitHub 存放庫中的[傳統型應用程式橋接轉 UWP 範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)和[通用 Windows 平台 (UWP) App 範例](https://github.com/Microsoft/Windows-universal-samples)。 若要檢視支援的 API 完整清單，請檢閱[使用傳統型橋接器轉換之 App 所支援的 UWP API](desktop-to-uwp-supported-api.md)。

除了呼叫 UWP API 之外，您可以利用只有已轉換之 App 可存取的功能來擴充您的 App。 這些包括於使用者登入時啟動處理程序，以及檔案總管整合等案例，且其設計用途在於讓原始傳統型應用程式與完整 UWP 應用程式套件之間的轉換運作更加順暢。 如需詳細資料，請參閱[傳統型橋接器應用程式擴充功能](desktop-to-uwp-extensions.md)。

## <a name="migrate"></a>移轉

使用橋接器，您可以逐漸將舊的程式碼移轉到 UWP，同時仍保留在 Windows 桌面上執行與發佈 App 的能力 一旦您完全移轉至 UWP (且您的 App 不再包含 WPF/Win32 元件)，您就可以觸達所有 Windows 裝置，包括手機、Xbox One 和 HoloLens。

## <a name="debug"></a>偵錯

您可以使用 Visual Studio 針對您的 App 進行偵錯。 如需詳細說明，請參閱[針對使用傳統型橋接器轉換的 App 進行偵錯](desktop-to-uwp-debug.md)。

如果您對於傳統型橋接器背後的運作方式有興趣，請參閱[傳統型橋接器的幕後作業](desktop-to-uwp-behind-the-scenes.md)。

## <a name="distribute"></a>散佈

您可以使用 Windows 市集或透過側載散佈您的 App。 如需詳細資訊，請參閱[散發使用傳統型橋接器轉換的 App](desktop-to-uwp-distribute.md)。 請注意，您需要簽署 App 才能將它部署到使用者。 請參閱[簽署使用傳統型橋接器轉換的 App](desktop-to-uwp-signing.md) 以取得逐步指示。

## <a name="support-and-feedback"></a>支援與意見反應

如果您在執行轉換 App 時發生問題，您可以造訪[論壇](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop)以尋求協助。

若要提供意見反應或提出功能建議，請查看 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

## <a name="in-this-section"></a>本節內容

| 主題 | 描述 |
|-------|-------------|
| [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) | 說明如何執行 Desktop App Converter。 |
| [使用傳統型橋接器將您的 App 手動轉換為 UWP](desktop-to-uwp-manual-conversion.md) | 了解如何手動建立應用程式套件和資訊清單。 |
| [傳統型橋接器應用程式擴充功能](desktop-to-uwp-extensions.md) | 使用擴充功能來增強已轉換的 App，以啟用像是啟動工作和檔案總管整合等功能。 |
| [使用傳統型橋接器轉換之 App 所支援的 UWP API](desktop-to-uwp-supported-api.md) | 查看您可以針對已轉換的傳統型應用程式使用哪些 UWP API。 |
| [.NET 傳統型應用程式與 Visual Studio 的傳統型橋接器封裝指南](desktop-to-uwp-packaging-dot-net.md) | 設定 Visual Studio 方案，您就可以編輯、偵錯及封裝您的 .NET 應用程式。 |
| [針對使用傳統型橋接器轉換的應用程式進行偵錯](desktop-to-uwp-debug.md) | 說明可用來偵錯已轉換之 App 的選項。 |
| [簽署使用傳統型橋接器轉換的 App](desktop-to-uwp-signing.md) | 了解如何使用憑證簽署已轉換的應用程式套件。 |
| [散發使用傳統型橋接器轉換的 App](desktop-to-uwp-distribute.md) | 查看如何將已轉換的 App 散佈給使用者。  |
| [傳統型橋接器的幕後作業](desktop-to-uwp-behind-the-scenes.md) | 深入了解傳統型轉 UWP 橋接器背後的運作方式。 |
| [傳統型橋接器的已知問題](desktop-to-uwp-known-issues.md) | 列出傳統型轉 UWP 橋接器的已知問題。 |
| [傳統型應用程式橋接轉 UWP 的程式碼範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | GitHub 上示範轉換 app 功能的程式碼範例。 |
