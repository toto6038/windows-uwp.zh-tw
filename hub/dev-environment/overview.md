---
title: 設定 Windows 10 開發環境
description: 協助您設定和最佳化 Windows 開發環境的指南。 我們將讓您開始安裝語言和工具，這些語言和工具是使用 Windows 或 Windows 子系統 Linux 版進行開發所需的。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: ''
ms.localizationpriority: medium
ms.date: 07/01/2020
ROBOTS: NOINDEX
ms.openlocfilehash: 237c3e8f58e41007840cf72aa1fd65efdc5763a5
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493720"
---
# <a name="set-up-your-windows-10-development-environment"></a>設定 Windows 10 開發環境

本指南將協助您開始安裝和設定語言和工具，這些語言和工具是在 Windows 或 Windows 子系統 Linux 版上進行開發所需的。

## <a name="development-paths"></a>開發路徑

:::row:::
    :::column:::
       [![JavaScrip / NodeJS](../images/nodejs-logo.png)](https://docs.microsoft.com/windows/nodejs)<br>
        **[開始使用 NodeJS](https://docs.microsoft.com/windows/nodejs)**<br>
        在 Windows 或 Windows 子系統 Linux 版上安裝 NodeJS，並設定您的開發環境。
    :::column-end:::
    :::column:::
       [![Python](../images/python-logo.png)](https://docs.microsoft.com/windows/python)<br>
        **[開始使用 Python](https://docs.microsoft.com/windows/python)**<br>
        在 Windows 或 Windows 子系統 Linux 版上安裝 Python，並設定您的開發環境。
    :::column-end:::
    :::column:::
       [![Android](../images/android-logo.png)](https://docs.microsoft.com/windows/android)<br>
        **[開始使用 Android](https://docs.microsoft.com/windows/android)**<br>
        安裝 Android Studio，或選擇 Xamarin、React 或 Cordova 等跨平台解決方案，並在 Windows 上設定您的開發環境。
    :::column-end:::
    :::column:::
       [![Windows 桌面](../images/windows-logo.png)](https://docs.microsoft.com/windows/apps/)<br>
        **[開始使用 Windows](https://docs.microsoft.com/windows/apps/)**<br>
        使用 UWP、Win32、WPF、Windows Forms，開始建置適用於 Windows 10 的傳統型應用程式，或使用 MSIX 和 XAML Island 來開始更新和部署現有的傳統型應用程式。
    :::column-end:::
:::row-end:::

## <a name="tools-and-platforms"></a>工具與平台

:::row:::
    :::column:::
       [![WSL](../images/windows-linux-dev-env.png)](https://docs.microsoft.com/windows/wsl/)<br>
        **[Windows 子系統 Linux 版](https://docs.microsoft.com/windows/wsl/)**<br>
        使用與 Windows 完全整合的最愛 Linux 發行版本 (不再需要雙重開機)。<br>
        [安裝 WSL](https://docs.microsoft.com/windows/wsl/install-win10)
    :::column-end:::
    :::column:::
       [![Windows 終端機](../images/terminal.png)](https://docs.microsoft.com/windows/terminal/)<br>
        **[Windows 終端機](https://docs.microsoft.com/windows/terminal/)**<br>
        自訂您的終端機環境以使用多個命令列殼層。
        <br>
        [安裝終端機](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)
    :::column-end:::
    :::column:::
       [![Windows 封裝管理員](../images/winget.png)](https://docs.microsoft.com/windows/package-manager/)<br>
        **[Windows 封裝管理員](https://docs.microsoft.com/windows/package-manager/)**<br>
        使用 WinGet (完整封裝管理員) 搭配您的命令列，在 Windows 10 上安裝應用程式。<br>
        [安裝 WinGet](https://docs.microsoft.com/windows/package-manager/winget/#install-winget)
    :::column-end:::
    :::column:::
       [![PowerToys](../images/powertoys.png)](https://github.com/microsoft/PowerToys)<br>
        **[Windows PowerToys](https://github.com/microsoft/PowerToys)**<br>
        使用這組進階使用者公用程式來調整和簡化您的 Windows 體驗，以提高生產力。<br>
        [安裝 PowerToys](https://github.com/microsoft/PowerToys#installing-and-running-microsoft-powertoys)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
       [![VS Code](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        輕量型原始程式碼編輯器，內建支援 JavaScript、TypeScript、Node.js、豐富的延伸模組生態系統 (C++、C#、Java、Python、PHP、Go) 和執行階段 (例如 .Net 和 Unity)。<br>
        [安裝 VS Code](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio](../images/visualstudio.png)](https://docs.microsoft.com/visualstudio/windows/)<br>
        **[Visual Studio](https://docs.microsoft.com/visualstudio/windows/)**<br>
        一種整合式開發環境，您可以用來編輯、偵錯、建置程式碼，以及發佈應用程式，包括編譯器、IntelliSense 程式碼完成，以及許多其他功能。<br>
        [安裝 Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![Azure](../images/Azure.png)](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide)**<br>
        完整的雲端平台，可裝載現有的應用程式並簡化新的開發。 Azure 服務整合了您開發、測試、部署和管理應用程式所需的一切。<br>
        [設定 Azure 帳戶](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![.NET](../images/net.png)](https://dotnet.microsoft.com/)<br>
        **[.NET](https://docs.microsoft.com/dotnet/standard/get-started/)**<br>
        一種開放原始碼開發平台，其中包含用來建置任何類型應用程式的工具和程式庫，這些類型包括 Web、行動、傳統型、遊戲、IoT、雲端和微服務。<br>
        [安裝 .NET](https://dotnet.microsoft.com/download)
    :::column-end:::
:::row-end:::

<br>

---

<br>

![Filler 影像](../images/flashy-office.png)

## <a name="tips-for-improving-your-workflow"></a>改善工作流程的秘訣

我們收集了一些秘訣，希望能讓您的工作流程更有效率且更有樂趣。 您有其他要分享的秘訣嗎？ 使用上方的 [編輯] 按鈕來提出提取要求，或使用下方的 [意見反應] 按鈕來提出問題，我們可將其新增至清單。

* Windows 子系統 Linux 版主要是用來做為內部開發迴圈的一部分。 我們建議的範例工作流程是建立 CI/CD 管線、使用 WSL 2 在您的 Windows 電腦上安裝 Ubuntu，以及在本機使用實際的 Linux 執行個體進行開發。 在確認一切運作正常之後，您就可以將該 CI/CD 管線推送至雲端，方法是使其成為 Docker 容器，然後將該容器推送至雲端執行個體，在這裡其會在生產環境就緒的 Ubuntu VM 上執行。 如需更多使用 WSL 的方法，請觀看 [WSL 2上的 Tabs vs Spaces 劇集](https://channel9.msdn.com/Shows/Tabs-vs-Spaces/WSL2-Code-faster-on-the-Windows-Subsystem-for-Linux)。

* 如果您是同時使用 Windows 和 Windows 子系統 Linux 版，則已安裝兩個檔案系統：NTSF (Windows) 和 WSL (您的 Linux 發行版本)。 若要快速執行，請確定您的專案檔與您要使用的工具儲存在同一個系統中。 深入了解[選擇正確的檔案系統以加快執行速度](https://docs.microsoft.com/windows/wsl/compare-versions#use-the-linux-file-system-for-faster-performance)。

* 您可以改善組建速度，方法是更新您的 Windows Defender 設定，為您足夠信任的專案資料夾或檔案類型新增排除範圍，以避免掃描出安全性威脅。 深入了解如何[更新 Windows Defender 設定來改善效能](https://docs.microsoft.com/windows/android/defender-settings)。

![Windows Defender 螢幕擷取畫面](../images/windows-defender-exclusions.png)

* 您可以使用 `code .` 命令，從命令列將 VS Code 啟動至您已開啟的專案，或從 Windows 或您的 WSL 發行版本使用 `explorer.exe .` 搭配 Windows 檔案總管開啟專案目錄。 如果此動作預設為無法運作，則您可能需要將 VS Code 可執行檔新增至 PATH 環境變數。 深入了解[從命令列中啟動](https://code.visualstudio.com/docs/editor/command-line#_launching-from-command-line)。

![Windows 檔案總管螢幕擷取畫面](../images/wsl-file-explorer.png)

* 如果您是使用 Git 進行版本控制和協同作業，您可以[設定 Git 認證管理員](https://docs.microsoft.com/windows/wsl/tutorials/wsl-git#git-credential-manager-setup)，將您的權杖儲存在 Windows 認證管理員中，藉此簡化您的驗證程序。 我們也建議[新增 .gitignore 檔案](https://docs.microsoft.com/windows/wsl/tutorials/wsl-git#adding-a-git-ignore-file)至您的專案。

* 您可以使用 [Windows 終端機命令列引數](https://docs.microsoft.com/windows/terminal/command-line-arguments?tabs=powershell#multiple-panes)，將多個命令列 (例如 PowerShell、Ubuntu 和 Azure CLI) 啟動到具有多個窗格的單一視窗。 在安裝 [Windows 終端機](https://docs.microsoft.com/windows/terminal/get-started)、[WSL/Ubuntu](https://docs.microsoft.com/windows/wsl/install-win10), 和 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 之後，請在 PowerShell 輸入下列命令，以開啟具有全部三個的全新多窗格視窗：

    ```powershell
    wt -p "Command Prompt" `; split-pane -p "Windows PowerShell" `; split-pane -H wsl.exe
    ```

![Filler 影像](../images/flashy-office2.png)

## <a name="transitioning-between-mac-and-windows"></a>在 Mac 與 Windows 之間轉換

請參閱我們的指南，以[在 Mac 與 Windows](https://docs.microsoft.com/windows/dev-environment/mac-to-windows) (或 Windows 子系統 Linux 版) 開發環境之間進行轉換。 其可以協助您對應下列項目之間的差異：

* [鍵盤快速鍵](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#keyboard-shortcuts)
* [軌跡板快速鍵](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#trackpad-shortcuts)
* [終端機和殼層工具](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#terminal-and-shell)
* [應用程式和公用程式](https://docs.microsoft.com/windows/dev-environment/mac-to-windows#apps-and-utilities)

## <a name="stories-from-developers-who-have-switched"></a>已切換的開發人員所提供的劇本

我們認為聽聽其他開發人員在 Mac 和 Windows 開發環境之間切換的體驗，可能很有幫助。 大多數人發現該程序相當簡單，而意識到他們仍然可以使用最愛的 Linux 和開放原始碼工具，同時又可使用整合方式存取 Windows 生產力工具，例如 [Microsoft Office](https://www.microsoft.com/microsoft-365/products-apps-services)、[Outlook](https://www.microsoft.com/microsoft-365/outlook/email-and-calendar-software-microsoft-outlook) 和 [Teams](https://www.microsoft.com/microsoft-365/microsoft-teams/group-chat-software)。 以下是我們找到的幾篇文章和部落格文章：

* Ken Wang，[「不同思維 — 軟體開發人員從 Mac 切換到 Windows」](https://medium.com/@kenwang_57215/software-developer-switching-from-mac-to-windows-66773d331910)
* Owen Williams，[「在 2019 從 Mac 切換到 Windows 的狀態」](https://char.gd/blog/2019/the-state-of-switching-to-windows-from-mac-in-2019)
* Brent Rose，[「當我從 Mac 切換到 Windows 時發生了什麼情況」](https://www.wired.com/story/rant-switching-from-mac-to-windows/)
* Jack Franklin，[「使用 Windows 10 和 WSL 進行前端 Web 開發」](https://www.jackfranklin.co.uk/blog/frontend-development-with-windows-10/)
* Aaron Schlesinger，[「從 Mac 回到 Windows 及 WSL 2」](https://arschles.com/blog/coming-from-a-mac-to-windows-wsl-2/)
* David Heinemeier Hansson，[「二十年後回到 Windows」](https://m.signalvnoise.com/back-to-windows-after-twenty-years/)
* Ray Elenteny，[「我回到 Windows 的原因」](https://dzone.com/articles/why-i-returned-to-windows)

## <a name="tutorials-courses-and-code-samples"></a>教學課程、課程和程式碼範例

我們已在下面列出一些教學課程、課程和程式碼範例，以協助您開始著手進行一些常見的工作案例。

* [使用 React 和 Azure Cosmos DB 建立 MongoDB 應用程式](https://docs.microsoft.com/azure/cosmos-db/tutorial-develop-mongodb-react)

* [建置具有拖放功能的 Android 雙螢幕應用程式](https://docs.microsoft.com/dual-screen/android/samples)

* [使用 Xamarin.Forms 建置待辦事項清單跨平台應用程式](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo/)

* [建置 Xamarin Android 應用程式，利用 Google Play 服務來示範 Google Maps API](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo/)

* [在 Azure App Service 中使用 PostgreSQL 部署 Python (Django) Web 應用程式](https://docs.microsoft.com/azure/app-service/containers/tutorial-python-postgresql-app?tabs=bash)

* [使用 Blazor 建置您的第一個 ASP.Net Core Web 應用程式](https://docs.microsoft.com/aspnet/core/tutorials/build-your-first-blazor-app?view=aspnetcore-3.1)

* [使用 Microsoft Graph 建置 Java 應用程式](https://docs.microsoft.com/graph/tutorials/java)

* [使用 Azure AD V2 從 WPF 應用程式呼叫 ASP.NET Core Web API](https://docs.microsoft.com/samples/azure-samples/active-directory-dotnet-native-aspnetcore-v2/calling-an-aspnet-core-web-api-from-a-wpf-application-using-azure-ad-v2/?view=aspnetcore-3.1)

* [建立及部署雲端原生 ASP.NET Core 微服務](https://docs.microsoft.com/learn/modules/microservices-aspnet-core/?view=aspnetcore-3.1)

* [探索 Microsoft Learn 上的免費線上課程](https://docs.microsoft.com/learn/browse/)

![Filler 影像](../images/flashy-office3.png)

## <a name="additional-resources"></a>其他資源

* [Microsoft Edge Web 瀏覽器文件](https://docs.microsoft.com/microsoft-edge/)
* [嘗試 WebHint 來改善您的網站](https://webhint.io/)
* [Microsoft 的遊戲堆疊文件](https://docs.microsoft.com/gaming/)
