---
title: 在 Windows 10 上改善開發工作流程的秘訣
description: 在 Windows 10 上改善開發工作流程的秘訣。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, 開發人員, 秘訣, 效能, WSL
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: 7d02e3b46d6938532bbc7024e8840b976b2715a6
ms.sourcegitcommit: 861c381a31e4a5fd75f94ca19952b2baaa2b72df
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2020
ms.locfileid: "92171152"
---
# <a name="tips-for-improving-performance-and-development-workflows"></a>改善效能和開發工作流程的秘訣

我們收集了一些秘訣，希望能讓您的工作流程更有效率且更有樂趣。 您有其他要分享的秘訣嗎？ 使用上方的 [編輯] 按鈕來提出提取要求，或使用下方的 [意見反應] 按鈕來提出問題，我們可將其新增至清單。

> [!NOTE]
> 如果您遇到與在 Windows 10 上進行開發相關的任何效能問題，例如：
> - 開發工具 (例如編譯器、連結器等) 在 Windows 上的執行速度比預期慢。
> - 執行階段平台 (例如 node、.NET、Python) 在 Windows 上執行的速度比其他平台慢。
> - 您的應用程式遇到與檔案 IO/網路/程序建立相關的效能問題。 
> 
> 請在 [Windows 開發人員 (WinDev) 問題存放庫](https://github.com/microsoft/WinDev)中提出問題，讓我們知道！

## <a name="use-shortcuts-to-open-a-project-in-vs-code-or-windows-file-explorer"></a>使用快速鍵在 VS Code 或 Windows 檔案總管中開啟專案

您可以使用 `code .` 命令，從命令列將 VS Code 啟動至您已開啟的專案，或從 Windows 或您的 WSL 發行版本使用 `explorer.exe .` 搭配 Windows 檔案總管開啟專案目錄。 如果此動作預設為無法運作，則您可能需要將 VS Code 可執行檔新增至 PATH 環境變數。 深入了解[從命令列中啟動](https://code.visualstudio.com/docs/editor/command-line#_launching-from-command-line)。

![Windows 檔案總管螢幕擷取畫面](../images/wsl-file-explorer.png)

## <a name="use-the-credential-manager-to-your-streamline-authentication-process"></a>使用認證管理員以簡化您的驗證程序

如果您是使用 Git 進行版本控制和協同作業，您可以[設定 Git 認證管理員](/windows/wsl/tutorials/wsl-git#git-credential-manager-setup)，將您的權杖儲存在 Windows 認證管理員中，藉此簡化您的驗證程序。 我們也建議[新增 .gitignore 檔案](/windows/wsl/tutorials/wsl-git#adding-a-git-ignore-file)至您的專案。

## <a name="use-wsl-for-testing-your-production-pipeline-before-deploying-to-the-cloud"></a>在部署至雲端之前，先使用 WSL 來測試您的生產管線

適用於 Linux 的 Windows 子系統可讓開發人員執行 GNU/Linux 環境 (包括大部分的命令列工具、公用程式和應用程式)，直接在 Windows 上執行，不需進行修改，不會造成傳統虛擬機器或 dualboot 設定的額外負荷。

WSL 是以開發人員對象為目標，旨在將開發人員做為內部開發迴圈的一部分。 假設 Sam 正在建立 CI/CD 管線 (持續整合和持續傳遞)，並想要先在本機電腦 (膝上型電腦) 上進行測試，再將其部署到雲端。 Sam 可以啟用 WSL (和 WSL 2 來改善速度和效能)，然後在本機 (膝上型電腦) 上使用正版 Linux Ubuntu 執行個體，並搭配任何慣用的 Bash 命令和工具。 在本機驗證開發管線之後，Sam 就可以將該 CI/CD 管線推送至雲端 (例如 Azure)，方法是使其成為 Docker 容器，然後將該容器推送至雲端執行個體，該執行個體會在生產環境就緒的 Ubuntu VM 上執行。

如需更多使用 WSL 的方法，請觀看 [WSL 2上的 Tabs vs Spaces 劇集](https://channel9.msdn.com/Shows/Tabs-vs-Spaces/WSL2-Code-faster-on-the-Windows-Subsystem-for-Linux)。

## <a name="improve-performance-speed-for-wsl-by-not-crossing-over-file-systems"></a>透過不跨越檔案系統來改善 WSL 的效能速度

如果您是同時使用 Windows 和 Windows 子系統 Linux 版，則已安裝兩個檔案系統：NTSF (Windows) 和 WSL (您的 Linux 發行版本)。 若要快速執行，請確定您的專案檔與您要使用的工具儲存在同一個系統中。 深入了解[選擇正確的檔案系統以加快執行速度](/windows/wsl/compare-versions#use-the-linux-file-system-for-faster-performance)。

## <a name="improve-build-speeds-by-adding-windows-defender-exclusions"></a>透過新增 Windows Defender 排除項目來改善建置速度

您可以改善組建速度，方法是更新您的 Windows Defender 設定，為您足夠信任的專案資料夾或檔案類型新增排除範圍，以避免掃描出安全性威脅。 深入了解如何[更新 Windows Defender 設定來改善效能](../android/defender-settings.md)。

![Windows Defender 螢幕擷取畫面](../images/windows-defender-exclusions.png)

## <a name="launch-all-your-command-lines-in-windows-terminal-at-once"></a>一次在 Windows 終端機中啟動所有命令列

* 您可以使用 [Windows 終端機命令列引數](/windows/terminal/command-line-arguments?tabs=powershell#multiple-panes)，將多個命令列 (例如 PowerShell、Ubuntu 和 Azure CLI) 啟動到具有多個窗格的單一視窗。 在安裝 [Windows 終端機](/windows/terminal/get-started)、[WSL/Ubuntu](/windows/wsl/install-win10), 和 [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) 之後，請在 PowerShell 輸入下列命令，以開啟具有全部三個的全新多窗格視窗：

    ```powershell
    wt -p "Command Prompt" `; split-pane -p "Windows PowerShell" `; split-pane -H wsl.exe
    ```

## <a name="share-your-tips"></a>分享您的秘訣

您是否有秘訣可以協助使用 Windows 的其他開發人員改善其工作流程？ 請[提交提取要求](https://github.com/MicrosoftDocs/windows-uwp/edit/docs/hub/dev-environment/overview.md)，將您的秘訣新增至頁面，或如果您想要新增有關特定主題的秘訣，請[記錄問題](https://github.com/MicrosoftDocs/windows-uwp/issues/new?title=&body=%0A%0A%5BEnter%20feedback%20here%5D%0A%0A%0A---%0A%23%23%23%23%20Document%20Details%0A%0A%E2%9A%A0%20*Do%20not%20edit%20this%20section.%20It%20is%20required%20for%20docs.microsoft.com%20%E2%9E%9F%20GitHub%20issue%20linking.*%0A%0A*%20ID%3A%207779352b-7b4e-dad8-7c1b-b9aba2c5e561%0A*%20Version%20Independent%20ID%3A%20a5b81b80-87a1-b6e2-8936-baf6c1a0b9c5%0A*%20Content%3A%20%5BSet%20up%20your%20Windows%2010%20development%20environment%5D(https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fwindows%2Fdev-environment%2Foverview)%0A*%20Content%20Source%3A%20%5Bhub%2Fdev-environment%2Foverview.md%5D(https%3A%2F%2Fgithub.com%2FMicrosoftDocs%2Fwindows-uwp%2Fblob%2Fdocs%2Fhub%2Fdev-environment%2Foverview.md)%0A*%20Product%3A%20**dev-environment**%0A*%20Technology%3A%20**windows-nodejs**)。

您是否有想要我們處理的效能相關問題？ 請將問題記錄在新的 [WinDev 問題存放庫](https://github.com/microsoft/windev)。

感謝開發人員。 我們正在傾聽並嘗試改善您的體驗！
