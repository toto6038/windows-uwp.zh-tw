---
title: 在 Windows 10 上設定開發環境
description: 協助您在 Windows 上設定開發環境，並安裝偏好的工具和程式碼語言的指南。 無論您偏好使用 Python、NodeJS、VS Code、Git、Bash、Linux 工具和命令、Android Studio，我們都能運用 Windows 終端機和 WSL 等絕佳的新工具來幫助您。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: 設定 windows, 開發環境, 開發工具, 開發路徑, Microsoft, Windows, 開發人員, 秘訣, 效能, WSL, 終端機, nodejs, python
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: 3a577ca4241f102bcff2a84419a0e3523f812b74
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254632"
---
# <a name="set-up-your-development-environment-on-windows-10"></a>在 Windows 10 上設定開發環境

本指南將協助您開始安裝和設定語言和工具，這些語言和工具是在 Windows 或 Windows 子系統 Linux 版上進行開發所需的。

## <a name="development-paths"></a>開發路徑

:::row:::
    :::column:::
       [![JavaScrip NodeJS 圖示](../images/nodejs-logo.png)](../nodejs/index.yml)<br>
        **[開始使用 NodeJS](../nodejs/index.yml)**<br>
        在 Windows 或 Windows 子系統 Linux 版上安裝 NodeJS，並設定您的開發環境。
    :::column-end:::
    :::column:::
       [![Python 圖示](../images/python-logo.png)](../python/index.yml)<br>
        **[開始使用 Python](../python/index.yml)**<br>
        在 Windows 或 Windows 子系統 Linux 版上安裝 Python，並設定您的開發環境。
    :::column-end:::
    :::column:::
       [![Android 圖示](../images/android-logo.png)](/windows/android)<br>
        **[開始使用 Android](/windows/android)**<br>
        安裝 Android Studio，或選擇 Xamarin、React 或 Cordova 等跨平台解決方案，並在 Windows 上設定您的開發環境。
    :::column-end:::
    :::column:::
       [![Windows 桌面圖示](../images/windows-logo.png)](../apps/index.yml)<br>
        **[開始使用 Windows 桌面](../apps/index.yml)**<br>
        使用 UWP、Win32、WPF、Windows Forms，開始建置適用於 Windows 10 的傳統型應用程式，或使用 MSIX 和 XAML Island 來開始更新和部署現有的傳統型應用程式。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
       [![C/C++](../images/c-logo.png)](/cpp/)<br>
        **[開始使用 C++ 和 C](/cpp/)**<br>
        開始使用 C++、C 和組件來開發應用程式、服務和工具。
    :::column-end:::
    :::column:::
       [![C# 圖示](../images/csharp-logo.png)](/dotnet/csharp/)<br>
        **[開始使用 C#](/dotnet/csharp/)**<br>
        開始使用 C# 和 .NET Core 來建置應用程式。
    :::column-end:::
    :::column:::
       [![適用於 Windows 的 Docker Desktop 圖示](../images/docker-logo.png)](../dev-environment/docker/overview.md)<br>
        **[開始使用適用於 Windows 的 Docker Desktop](../dev-environment/docker/overview.md)**<br>
        透過來自 Visual Studio、VS Code、.NET、Windows 子系統 Linux 版或各種 Azure 服務的支援建立遠端開發容器。
    :::column-end:::
    :::column:::
       [![PowerShell 圖示](../images/powershell.png)](/powershell/)<br>
        **[開始使用 PowerShell](/powershell/)**<br>
        使用 PowerShell (一項命令列命令介面和指令碼語言)，開始使用跨平台工作自動化和組態管理。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
       [![Rust 圖示](../images/rust-icon.png)](./rust/index.yml)<br>
        **[Rust 入門](./rust/index.yml)**<br>
        開始使用 Rust 進行程式設計， &mdash; 包括如何使用 *windows* 安裝程式來設定 windows 的 Rust。
    :::column-end:::
:::row-end:::

## <a name="tools-and-platforms"></a>工具與平台

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
        使用 winget.exe 用戶端 (完整封裝管理員) 搭配您的命令列，在 Windows 10 上安裝應用程式。<br>
        [安裝 Windows 封裝管理員 (公開預覽)](../package-manager/winget/index.md#install-winget)
    :::column-end:::
    :::column:::
       [![PowerToys 圖示](../images/powertoys.png)](https://github.com/microsoft/PowerToys)<br>
        **[Windows PowerToys](https://github.com/microsoft/PowerToys)**<br>
        使用這組進階使用者公用程式來調整和簡化您的 Windows 體驗，以提高生產力。<br>
        [安裝 PowerToys (公開預覽)](https://github.com/microsoft/PowerToys#installing-and-running-microsoft-powertoys)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
       [![VS Code 圖示](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        輕量型原始程式碼編輯器，內建支援 JavaScript、TypeScript、Node.js、豐富的延伸模組生態系統 (C++、C#、Java、Python、PHP、Go) 和執行階段 (例如 .Net 和 Unity)。<br>
        [安裝 VS Code](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio 圖示](../images/visualstudio.png)](/visualstudio/windows/)<br>
        **[Visual Studio](/visualstudio/windows/)**<br>
        一種整合式開發環境，您可以用來編輯、偵錯、建置程式碼，以及發佈應用程式，包括編譯器、IntelliSense 程式碼完成，以及許多其他功能。<br>
        [安裝 Visual Studio](/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![Azure 圖示](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](/azure/guides/developer/azure-developer-guide)**<br>
        完整的雲端平台，可裝載現有的應用程式並簡化新的開發。 Azure 服務整合了您開發、測試、部署和管理應用程式所需的一切。<br>
        [設定 Azure 帳戶](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![.NET 圖示](../images/net.png)](https://dotnet.microsoft.com/)<br>
        **[.NET](/dotnet/standard/get-started/)**<br>
        一種開放原始碼開發平台，其中包含用來建置任何類型應用程式的工具和程式庫，這些類型包括 Web、行動、傳統型、遊戲、IoT、雲端和微服務。<br>
        [安裝 .NET](https://dotnet.microsoft.com/download)
    :::column-end:::
:::row-end:::

<br>

## <a name="run-windows-and-linux"></a>執行 Windows 和 Linux

Windows 子系統 Linux 版 (WSL) 可讓開發人員直接隨著 Windows 執行 Linux 作業系統。 這兩者會共用相同硬碟 (且可以存取彼此的檔案)，剪貼簿可自然地支援在這兩者之間複製並貼上，而不需要雙重開機。 WSL 可讓您使用 BASH，並將提供 Mac 使用者最熟悉的環境類型。
- 請在 [WSL 文件](/windows/wsl)中或透過 [Channel 9 上的 WSL 影片](https://channel9.msdn.com/Search?term=wsl&lang-en=true)深入了解。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-can-I-do-with-WSL--One-Dev-Question/player?format=ny]

您也可以使用 Windows 終端機，在具有多個索引標籤的相同視窗中或在多個窗格中開啟您喜好的所有命令列工具，不論是 PowerShell、Windows 命令提示字元、Ubuntu、Debian、Azure CLI、Oh-my-Zsh、Git Bash，或上述所有選項皆可。

請在 [Windows 終端機文件](/windows/terminal)中或透過 [Channel 9 上的 Windows 終端機影片](https://channel9.msdn.com/Search?term=windows%20terminal&lang-en=true) 深入了解。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-are-the-main-features-of-the-new-Terminal--One-Dev-Question/player?format=ny]

## <a name="transitioning-between-mac-and-windows"></a>在 Mac 與 Windows 之間轉換

請參閱我們的指南，以[在 Mac 與 Windows](./mac-to-windows.md) (或 Windows 子系統 Linux 版) 開發環境之間進行轉換。 其可以協助您對應下列項目之間的差異：

- [鍵盤快速鍵](./mac-to-windows.md#keyboard-shortcuts)
- [軌跡板快速鍵](./mac-to-windows.md#trackpad-shortcuts)
- [終端機和殼層工具](./mac-to-windows.md#command-line-shells-and-terminals)
- [應用程式和公用程式](./mac-to-windows.md#apps-and-utilities)

![辦公室影像](../images/flashy-office3.png)

## <a name="additional-resources"></a>其他資源

- [改善工作流程的秘訣](./tips.md)
- [從 Mac 切換至 Windows 的開發人員故事](./dev-stories.md)
- [熱門教學課程、課程和程式碼範例](./tutorials.md)
- [Microsoft 的遊戲堆疊文件](/gaming/)
