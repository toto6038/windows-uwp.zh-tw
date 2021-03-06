---
title: Windows 10 上的 Web 開發
description: Windows 10 上的 網頁程式開發指南，其中包含 Microsoft 提供的工具、api 和各種資源連結。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: 網頁程式開發、網頁程式開發、windows 上的 web、api、邊緣
ms.date: 01/06/2021
ms.openlocfilehash: 1e33fbfebe037afcf64d13494b2e9eecb87389cf
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2021
ms.locfileid: "102255076"
---
# <a name="web-development-on-windows-10"></a>Windows 10 上的 Web 開發

Microsoft 為 網頁程式開發人員提供各種資源，包括支援使用 Windows 10 進行 web 程式開發的新工具和功能。 本指南涵蓋許多可用的工具，並提供可讓您在網路上為您的理想環境進行開發的 [意見](../dev-environment/overview.md#additional-resources) 反應。 如需 Api 的清單，請參閱 [網頁程式開發的 api](/apis.md)。 如需開始使用的詳細說明，請參閱 [在 Windows 10 上設定開發環境](../dev-environment/overview.md)。

## <a name="webview-devtools-pwas"></a>Web 視圖、DevTools、Pwa

:::row:::
    :::column:::
        [![Web 視圖圖示](../images/webview2.png)](https://developer.microsoft.com/microsoft-edge/webview2/)<br>
        **[Web 視圖2](https://developer.microsoft.com/microsoft-edge/webview2/)**<br>
        使用 Microsoft Edge WebView2，在您的原生應用程式中內嵌 web 內容 (HTML、CSS 和 JavaScript) 。
        [下載 Web 視圖2](https://developer.microsoft.com/microsoft-edge/webview2/#download-section)
    :::column-end:::
    :::column:::
        [![Microsoft Edge DevTools 圖示](../images/microsoftedge-devtools.png)](/microsoft-edge/devtools-guide-chromium/index.md)<br>
        **[Microsoft Edge DevTools](/microsoft-edge/devtools-guide-chromium/)**<br>
        Microsoft Edge 開發人員工具是直接內建在 Microsoft Edge 瀏覽器中的一組檢查和偵錯工具。
    若要開啟 DevTools，請將焦點放在 Microsoft Edge：
        - 以滑鼠右鍵按一下，然後檢查
        - 選取 `F12` 金鑰
        - `Ctrl` + `Shift` + `i`
    :::column-end:::
    :::column:::
       [![PWA 圖示](../images/pwa-icon.png)](/microsoft-edge/progressive-web-apps-chromium/)<br>
        **[Windows 上的漸進式 Web 應用程式](/microsoft-edge/progressive-web-apps-chromium/)**<br>
        漸進式 Web 應用程式 (Pwa) 為使用者提供自訂的原生、類似應用程式的體驗，並針對其裝置進行自訂。 它們是逐漸增強的網站，可在支援的平臺上以原生應用程式的形式運作。<br>
        [Pwa 入門](/microsoft-edge/progressive-web-apps-chromium/get-started)
    :::column-end:::
:::row-end:::

## <a name="microsoft-edge-browser"></a>Microsoft Edge 瀏覽器

:::row:::
    :::column:::
       [![Microsoft Edge 圖示](../images/microsoftedge.png)](https://www.microsoft.com/en-us/edge)<br>
        **[Microsoft Edge](https://www.microsoft.com/en-us/edge)**<br>
        新的 Microsoft Edge 是以 Chromium 為基礎，可建立更好的 web 相容性，並減少基礎 web 平臺的片段。 2020年1月15日發行，Windows、macOS、iOS 和 Android 都支援此功能。 <br>
        [安裝新的 Microsoft Edge](https://www.microsoft.com/edge)
    :::column-end:::
    :::column:::
        [![商務用 Microsoft Edge](../images/microsoftedge-enterprise.png)](/deployedge/)<br>
        **[商務用 Microsoft Edge](/deployedge/)**<br>
        Microsoft Edge 是以 Chromium 為基礎，並提供企業支援。 取得如何設定及部署多個可用通道的逐步指引。<br>
        [下載 Microsoft Edge 頻道](https://www.microsoft.com/edge/business/download)
    :::column-end:::
    :::column:::
        [![Microsoft Edge Insider 圖示](../images/microsoftedge-beta.png)](https://www.microsoftedgeinsider.com/whats-new)<br>
        **[Microsoft Edge Insider](https://www.microsoftedgeinsider.com/whats-new)**<br>
        我們每天都會為 Microsoft Edge 建立新的內容。 深入瞭解我們最近的進度，以及您可以如何參與。
        [下載 Microsoft Edge Beta 版](https://www.microsoftedgeinsider.com/)
    :::column-end:::
    :::column:::
        [![Microsoft Edge 支援圖示](../images/microsoftedge-support.png)](https://support.microsoft.com/microsoft-edge)<br>
        **[Microsoft Edge 支援](https://support.microsoft.com/microsoft-edge)**<br>
        取得自訂您的瀏覽器、新增擴充功能、追蹤預防、疑難排解等方面的協助。
        [取得 Microsoft Edge 的協助](https://support.microsoft.com/microsoft-edge)
    :::column-end:::
:::row-end:::

## <a name="debugging-testing-and-accessibility"></a>調試、測試和協助工具

:::row:::
    :::column:::
       [![VS Marketplace Edge 偵錯工具擴充功能](../images/visualstudio-edge-debugger.png)](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-edge)<br>
        **[VS Code：適用于 Microsoft Edge 的偵錯工具](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-edge/)**<br>
        使用這個 VS Code 擴充功能，在 Microsoft Edge 瀏覽器中進行 JavaScript 程式碼的偵錯工具。 也可以從 Visual Studio 中的 ASP.NET 專案使用。<br>
        [安裝 VS Code-適用于 Microsoft Edge 的偵錯工具](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-edge)
    :::column-end:::
    :::column:::
       [![虛擬機器圖示](../images/virtualmachine.png)](https://developer.microsoft.com/microsoft-edge/tools/vms/)<br>
        **[用於測試的虛擬機器](https://developer.microsoft.com/microsoft-edge/tools/vms/)**<br>
        使用您在本機下載及管理的免費 Windows 10 虛擬機器，測試 IE11 和 Microsoft Edge 舊版。<br>
        [下載虛擬機器](https://developer.microsoft.com/microsoft-edge/tools/vms/)
    :::column-end:::
    :::column:::
       [![WebHint 圖示](../images/webhint.png)](https://webhint.io/)<br>
        **[協助工具的 WebHint](https://webhint.io/)**<br>
        可自訂的 linting 工具，可協助您藉由檢查程式碼中的最佳作法和常見錯誤，來改善網站的存取性、速度、跨瀏覽器的相容性等。<br>
        [安裝 VS Code 擴充功能](https://webhint.io/docs/user-guide/extensions/vscode-webhint/)<br>
        [安裝瀏覽器擴充功能](https://webhint.io/docs/user-guide/extensions/extension-browser/)<br>
        [安裝 CLI](https://webhint.io/docs/user-guide/)
    :::column-end:::
    :::column:::
       [![WebDriver 圖示](../images/webdriver.png)](https://docs.microsoft.com/microsoft-edge/webdriver-chromium/)<br>
        **[WebDriver](https://docs.microsoft.com/microsoft-edge/webdriver-chromium/)**<br>
        在 Microsoft Edge 中使用 Microsoft WebDriver 自動化測試您的網站，以在開發週期結束迴圈。<br>
        [安裝 WebDriver](https://developer.microsoft.com/microsoft-edge/tools/webdriver/)
    :::column-end:::
:::row-end:::

## <a name="visual-studio-code-editors"></a>Visual Studio 程式碼編輯器

:::row:::
    :::column:::
       [![VS Code 圖示](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        輕量型原始程式碼編輯器，內建支援 JavaScript、TypeScript、Node.js、豐富的延伸模組生態系統 (C++、C#、Java、Python、PHP、Go) 和執行階段 (例如 .Net 和 Unity)。<br>
        [安裝 VS Code](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio 圖示](../images/visualstudio.png)](/visualstudio/windows/)<br>
        **[Visual Studio (IDE)](/visualstudio/windows/)**<br>
        一種整合式開發環境，您可以用來編輯、偵錯、建置程式碼，以及發佈應用程式，包括編譯器、IntelliSense 程式碼完成，以及許多其他功能。<br>
        [安裝 Visual Studio](/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![VS Code marketplace 圖示](../images/vs-code-marketplace.png)](https://marketplace.visualstudio.com/vscode)<br>
        **[適用于擴充功能的 VS Code Marketplace](https://marketplace.visualstudio.com/vscode)**<br>
        探索可用來自訂 Visual Studio 程式碼編輯器的許多不同擴充功能。<br>
        [安裝延伸模組](https://marketplace.visualstudio.com/vscode)
    :::column-end:::
    :::column:::
       [![Visual Studio marketplace 圖示](../images/vs-marketplace.png)](https://marketplace.visualstudio.com/vs/)<br>
        **[適用于擴充功能的 Visual Studio Marketplace](https://marketplace.visualstudio.com/vs)**<br>
        探索可用來自訂 Visual Studio 整合式開發環境的許多不同擴充功能。<br>
        [安裝延伸模組](https://marketplace.visualstudio.com/vs)
    :::column-end:::
:::row-end:::

## <a name="wsl-terminal-package-manager-docker-desktop"></a>WSL、終端機、套件管理員、Docker Desktop

:::row:::
    :::column:::
       [![WSL 圖示](../images/windows-linux-dev-env.png)](/windows/wsl/)<br>
        **[Windows 子系統 Linux 版](/windows/wsl/)**<br>
        使用與 Windows 完全整合的最愛 Linux 發行版本 (不再需要雙重開機)。<br>
        [安裝 WSL](/windows/wsl/install-win10)
    :::column-end:::
    :::column:::
       [![Windows 終端機圖示](../images/terminal.png)](/windows/terminal/)<br>
        **[Windows 終端機](/windows/terminal/)**<br>
        自訂您的終端機環境以使用多個命令列殼層。
        <br>
        [安裝終端機](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)
    :::column-end:::
    :::column:::
       [![Windows 封裝管理員圖示](../images/winget.png)](../package-manager/index.md)<br>
        **[Windows 封裝管理員](../package-manager/index.md)**<br>
        使用 winget.exe 用戶端搭配您的命令列，在 Windows 10 上安裝應用程式。<br>
        [安裝 Windows 封裝管理員 (公開預覽)](../package-manager/winget/index.md#install-winget)
    :::column-end:::
    :::column:::
       [![適用於 Windows 的 Docker Desktop 圖示](../images/docker-icon.png)](../dev-environment/docker/overview.md)<br>
        **[適用于 Windows 的 Docker Desktop](../dev-environment/docker/overview.md)**<br>
        透過來自 Visual Studio、VS Code、.NET、Windows 子系統 Linux 版或各種 Azure 服務的支援建立遠端開發容器。<br>
        [安裝適用於 Windows 的 Docker Desktop](https://docs.docker.com/docker-for-windows/install/)
    :::column-end:::
:::row-end:::

## <a name="aspnet-typescript-xamarin"></a>ASP.NET、Typescript、Xamarin

:::row:::
    :::column:::
       [![ASP.NET 圖示](../images/aspnet.png)](https://dotnet.microsoft.com/apps/aspnet)<br>
        **[ASP.NET](/aspnet/)**<br>
        使用 .NET 和 c # 來建立 web 應用程式和服務、物聯網 (IoT) 應用程式或行動後端的跨平臺架構。 在 Windows、macOS 和 Linux 上使用您最愛的開發工具。 部署到雲端或在內部部署。 在 .NET Core 上執行。<br>
        [安裝 ASP.NET](https://dotnet.microsoft.com/download)
    :::column-end:::
    :::column:::
       [![Typescript 圖示](../images/typescript-icon.png)](https://www.typescriptlang.org/)<br>
        **[Typescript](https://www.typescriptlang.org/)**<br>
        TypeScript 藉由將類型新增至語言來擴充 JavaScript。 例如，JavaScript 會提供字串、數位和物件等語言基本類型，但不會檢查您是否已一致地指派這些專案。 TypeScript 有<br>
        [在您的瀏覽器中嘗試](https://www.typescriptlang.org/play/)[安裝在本機](https://www.typescriptlang.org/#installation)
    :::column-end:::
    :::column:::
       [![Xamarin 存放庫圖示](../images/xamarin-icon.png)](/xamarin/)<br>
        **[Xamarin](/xamarin/)**<br>
        Xamarin 可讓您使用 .NET 程式碼和平台專屬的使用者介面，來建置 Android、iOS 和 macOS 原生應用程式。 Xamarin.Forms 可讓您透過以 C# 或 XAML 撰寫的共用 UI 程式碼，來建置原生應用程式。
        <br>
        [安裝 Xamarin](/xamarin/get-started/installation/)
    :::column-end:::
:::row-end:::

## <a name="open-source-contributions"></a>開放原始碼投稿

:::row:::
    :::column:::
       [![開放原始碼圖示](../images/opensource-icon.png)](https://opensource.microsoft.com/)<br>
        **[Microsoft 的開放原始碼](https://opensource.microsoft.com/)**<br>
        數以千計的 Microsoft 工程師會每天使用、參與及發行開放原始碼。 熱門專案包括 Visual Studio Code、TypeScript、.NET 和 >chakracore。<br>
        [積極參與](https://opensource.microsoft.com/collaborate)
    :::column-end:::
    :::column:::
       [![WinDev 存放庫圖示](../images/windev-repo.png)](https://github.com/microsoft/WinDev)<br>
        **[Windows 開發人員效能問題存放庫](https://github.com/microsoft/WinDev)**<br>
        無論您是在 Windows 或 Windows 上進行開發，將它當作跨平臺開發電腦使用，我們想要知道任何效能問題，而導致您的問題。
        <br>
        [提出效能問題](https://github.com/microsoft/WinDev/issues)
    :::column-end:::
    :::column:::
       [![檔圖示](../images/docs.png)](https://docs.microsoft.com/contribute/)<br>
        **[參與文件編輯](https://docs.microsoft.com/contribute/)**<br>
        Microsoft 的大部分檔集都是開放原始碼並裝載在 GitHub 上。 藉由提出問題或撰寫提取要求來參與。
        <br>
        [瞭解如何](https://docs.microsoft.com/contribute/)
    :::column-end:::
:::row-end:::

## <a name="cloud-development-with-azure"></a>使用 Azure 進行雲端開發

:::row:::
    :::column:::
       [![Azure 圖示](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](/azure/guides/developer/azure-developer-guide)**<br>
        完整的雲端平台，可裝載現有的應用程式並簡化新的開發。 Azure 服務整合了您開發、測試、部署和管理應用程式所需的一切。<br>
        [設定 Azure 帳戶](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![Azure 認知服務圖示](../images/azure-cognitive-services.png)](/azure/cognitive-services/what-are-cognitive-services)<br>
        **[Azure 認知服務](/azure/cognitive-services/what-are-cognitive-services)**<br>
        使用 REST Api 和用戶端程式庫 Sdk 的雲端式服務，可協助您在應用程式中建立認知智慧。<br>
        [試用認知服務](https://azure.microsoft.com/en-us/services/cognitive-services/)
    :::column-end:::
    :::column:::
       [![Azure 開發人員指南圖示](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[學習 Azure](/azure/guides/developer/azure-developer-guide)**<br>
        完整的雲端平台，可裝載現有的應用程式並簡化新的開發。 Azure 服務整合了您開發、測試、部署和管理應用程式所需的一切。<br>
        [設定 Azure 帳戶](https://azure.microsoft.com/free/)
    :::column-end:::
:::row-end:::

## <a name="addtional-resources"></a>其他資源

:::row:::
    :::column:::
       [![設定開發環境圖示](../images/dev-environment-icon.png)](../dev-environment/overview.md)<br>
        **[在 Windows 10 上設定開發環境](../dev-environment/overview.md)**<br>
        取得設定開發環境的說明，以使用 Python、NodeJS、c #、C、c + +、建立 Android 應用程式、建立 Windows 桌面應用程式、建立 Docker 容器、執行 PowerShell 腳本等等。
        <br>
        [開始使用](../dev-environment/overview.md)
    :::column-end:::
    :::column:::
       [![回應原生的 Windows 圖示](../images/reactnative-windows.png)](https://microsoft.github.io/react-native-windows/)<br>
        **[回應原生的 Windows + macOS](https://microsoft.github.io/react-native-windows/)**<br>
        對 Windows 10 SDK 和 macOS 10.13 SDK 帶來回應原生支援。 使用 JavaScript 為 Windows 10 所支援的所有裝置（包括電腦、平板電腦、2個內建、Xbox、混合式現實裝置等），以及 macOS 桌上型電腦和膝上型電腦生態系統建立原生 Windows 應用程式。
        <br>
        [安裝 Windows 的回應原生](https://microsoft.github.io/react-native-windows/docs/getting-started)<br>
        [安裝 macOS 的回應原生](https://microsoft.github.io/react-native-windows/docs/rnm-getting-started)
    :::column-end:::
    :::column:::
       [![學習圖示](../images/learn-icon.png)](https://docs.microsoft.com/learn/browse/?terms=web)<br>
        **[Microsoft 學習與 網頁程式開發相關的課程](https://docs.microsoft.com/learn/browse/?terms=web)**<br>
        Microsoft 學習提供免費的線上課程，以學習各種新的技能，並透過逐步指引來探索 Microsoft 產品和服務。
        <br>
        [開始學習](https://docs.microsoft.com/learn/browse/?terms=web)
    :::column-end:::
:::row-end:::

## <a name="transitioning-between-mac-and-windows"></a>在 Mac 與 Windows 之間轉換

請參閱我們的指南，以[在 Mac 與 Windows](../dev-environment/mac-to-windows.md) (或 Windows 子系統 Linux 版) 開發環境之間進行轉換。

- [鍵盤快速鍵](../dev-environment/mac-to-windows.md#keyboard-shortcuts)
- [軌跡板快速鍵](../dev-environment/mac-to-windows.md#trackpad-shortcuts)
- [終端機和殼層工具](../dev-environment/mac-to-windows.md#command-line-shells-and-terminals)
- [應用程式和公用程式](../dev-environment/mac-to-windows.md#apps-and-utilities)
- [從 Mac 切換至 Windows 的開發人員故事](../dev-environment/dev-stories.md)
