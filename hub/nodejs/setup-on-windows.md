---
title: 在原生 Windows 上設定 NodeJS
description: 協助您直接在 Windows 上設定 Node.js 開發環境的指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node.js, windows 10, 原生 windows, 直接在 windows 上
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 456aac17f61ab0add3d35a48c74e151fa15e9e83
ms.sourcegitcommit: 8efeb6672f759b1ea7e3e9e2f90e764480791142
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728469"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>直接在 Windows 上設定您的 Node.js 開發環境

下列逐步指南可引導您開始在原生 Windows 開發環境中使用 Node.js。

## <a name="install-nvm-windows-nodejs-and-npm"></a>安裝 nvm-windows、node.js 和 npm

有多種方式可以安裝 Node.js。 由於版本變更非常快速，因此，我們建議使用版本管理員。 您可能需要根據您正在處理的不同專案需求，在多個版本之間切換。 Node 版本管理員 (較常稱為 nvm) 是安裝多個 Node.js 版本的最熱門方式，但僅適用 Mac/Linux，且在 Windows 上不受支援。 我們將改為逐步解說安裝 nvm-windows 的步驟，然後用其來安裝 Node.js 和 Node 套件管理員 (npm)。 下一節也會涵蓋需考量的[替代版本管理員](#alternative-version-managers)。

> [!IMPORTANT]
> 安裝版本管理員之前，一律建議您從作業系統中移除任何現有的 Node.js 或 npm 安裝，因為不同類型的安裝可能導致奇怪且困惑的衝突。 這包括刪除任何可能保留的現有 nodejs 安裝目錄 (例如 "C:\Program Files\nodejs")。 NVM 產生的符號連結將不會覆寫現有的 (甚至是空的) 安裝目錄。 如需移除先前安裝的說明，請參閱[如何從 Windows 完全移除 Node.js](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows) \(英文\)。

1. 在您的網際網路瀏覽器中，開啟 [windows-nvm 存放庫](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) \(英文\)，然後選取 [立即下載]  連結。
2. 下載最新版的 **nvm-setup.zip** 檔案。
3. 下載之後，開啟 zip 檔案，然後開啟 **nvm-setup.exe** 檔案。
4. [設定 NVM for Windows] 安裝精靈將引導您完成設定步驟，包括選擇將安裝 nvm-windows 和 Node.js 的目錄。

    ![NVM for Windows 安裝精靈](../images/install-nvm-for-windows-wizard.png)

5. 安裝完成之後。 開啟 PowerShell，並嘗試使用 windows-nvm 來列出目前已安裝的 Node 版本 (此時應該沒有任何版本)：`nvm ls`

    ![未顯示任何 Node 版本的 NVM 清單](../images/windows-nvm-powershell-no-node.png)

6. 安裝目前的 Node.js 版本 (用以測試最新的功能改進，但相較於 LTS 版本，更可能發生問題)：`nvm install latest`
7. 首先，使用 `nvm list available` 來查閱目前的 LTS 版本號碼，然後使用 `nvm install <version>` 來安裝 LTS 版本號碼 (以版本號碼取代 `<version>`，例如 `nvm install 12.14.0`)，以安裝 Node.js 的最新穩定 LTS 版本 (建議)。

    ![可用版本的 NVM 清單](../images/windows-nvm-list.png)

8. 列出已安裝的 Node 版本：`nvm ls` ... 現在您應該會看到您剛安裝的兩個版本。

    ![顯示已安裝 Node 版本的 NVM 清單](../images/windows-nvm-node-installs.png)

9. 若要檢查哪個 Node.js 版本為目前的預設值，請輸入：`node --version`
10. 若要變更您想要用於專案的 Node.js 版本，請建立新的專案目錄 `mkdir NodeTest`，並進入目錄 `cd NodeTest`，然後輸入 `nvm use <version>`，使用您想要使用的版本號碼來取代 `<version>` (例如 v10.16.3)。
11. 使用 `npm --version` 來確認已安裝的 npm 版本，此版本號碼將自動變更為與您目前 Node.js 版本相關聯的任何 npm 版本。

## <a name="alternative-version-managers"></a>替代版本管理員

雖然 windows-nvm 是目前最熱門的 Node 版本管理員，但有一些替代方案可以考慮：

- [nvs](https://github.com/jasongin/nvs) \(英文\) (Node 版本切換器) 是跨平台的 `nvm` 替代方案，能夠[與 VS Code 整合](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md) \(英文\)。

- [Volta](https://github.com/volta-cli/volta#installing-volta) \(英文\) 是由 LinkedIn 小組所提供的新版本管理員，其聲稱已改良速度並提供跨平台支援。

若要將 Volta 安裝為版本管理員 (而非 windows-nvm)，請移至其[入門指南](https://docs.volta.sh/guide/getting-started) \(英文\) 的 **Windows 安裝**小節，然後下載並執行其 Windows 安裝程式，並遵循安裝指示。

> [!IMPORTANT]
> 安裝 Volta 之前，您必須確定已在 Windows 電腦上啟用[開發人員模式](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers)。

若要深入了解如何使用 Volta 在 Windows 上安裝多個 Node.js 版本，請參閱 [Volta 文件](https://docs.volta.sh/guide/understanding#managing-your-toolchain) \(英文\)。

## <a name="install-your-favorite-code-editor"></a>安裝您慣用的程式碼編輯器

我們建議您[安裝 VS Code](https://code.visualstudio.com) \(英文\) 以及 [Node.js 延伸模組套件](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack) \(英文\)，以便在 Windows 上使用 Node.js 進行開發。 全部安裝，或挑選對您最實用的延伸模組。

安裝 Node.js 延伸模組套件：

1. 在 VS Code 中開啟 [延伸模組]  視窗 (Ctrl+Shift+X)。
2. 在 [延伸模組] 視窗頂端的搜尋方塊中，輸入：「Node 延伸模組套件」(或您要尋找之任何延伸模組的名稱)。
3. 選取 [安裝]  。 安裝完成後，您的延伸模組將出現在 [延伸模組]  視窗的 [已啟用] 資料夾中。 您可以藉由選取新延伸模組描述旁邊的齒輪圖示，來停用、解除安裝或進行設定。

您可能想要考慮的數個額外延伸模組包括：

- [適用於 Chrome 的偵錯工具](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code) \(英文\)：當您使用 Node.js 在伺服器端完成開發之後，您將需要開發並測試用戶端。 此延伸模組會整合您的 VS Code 編輯器與 Chrome 瀏覽器偵錯服務，讓工作更有效率。
- [來自其他編輯器的按鍵對應](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)：如果您從另一個文字編輯器 (例如 Atom、Sublime、Vim、eMacs、Notepad++ 等) 進行轉換，這些延伸模組有助於讓您的環境感到非常自在。
- [設定同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) \(英文\)：可讓您使用 GitHub 同步處理不同安裝之間的 VS Code 設定。 如果您在不同的電腦上工作，這有助於讓您的環境在其上保持一致。

## <a name="install-git-optional"></a>安裝 Git (選用)

如果您計畫與其他人合作，或在開放原始碼網站 (如 GitHub) 上裝載您的專案，VS Code 支援[使用 Git 進行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) \(英文\)。 VS Code 中的 [原始檔控制] 索引標籤會追蹤您所有的變更，並讓常用的 Git 命令 (add、commit、push、pull) 直接內建在 UI 中。 您首先必須安裝 Git，才能強化 [原始檔控制] 面板。

1. 從 [git-scm 網站](https://git-scm.com/download/win)下載並安裝適用於 Windows 的 Git。

2. 隨附的安裝精靈會詢問您有關 Git 安裝設定的一系列問題。 建議您使用所有預設設定，除非您有特定原因，非變更某些設定不可。

3. 如果您之前從未使用過 Git，[GitHub 指南](https://guides.github.com/)可以協助您開始使用。

4. 我們建議您將 [.gitignore 檔案](https://help.github.com/en/articles/ignoring-files) \(英文\) 新增至您的 Node 專案。 以下是[適用於 Node.js 的 GitHub 預設 gitignore 範本](https://github.com/github/gitignore/blob/master/Node.gitignore)。

## <a name="use-windows-subsystem-for-linux-for-production"></a>針對實際執行環境使用 Windows 子系統 Linux 版

直接在 Windows 上使用 Node.js，非常適合用來了解及實驗您可以執行的作業。 當您準備好建置可供實際執行環境使用的 Web 應用程式 (通常會部署到 Linux 型伺服器) 時，建議您使用 Windows 子系統 Linux 版第 2 版 (WSL 2) 來開發 Node.js Web 應用程式。 許多 Node.js 套件和架構都是以 *nix 環境作為考量，而且大部分的 Node.js 應用程式都部署於 Linux 上，因此，在 WSL 上進行開發，可確保在您的開發與實際執行環境之間具有一致性。 若要設定 WSL 開發環境，請參閱[使用 WSL 2 設定您的 Node.js 開發環境](./setup-on-wsl2.md)。

> [!NOTE]
> 如果您處於需要在 Windows 伺服器上裝載 Node.js 應用程式的 (有點罕見) 情況下，最常見的案例似乎是[使用反向 Proxy](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca) \(英文\)。 有兩種方式可以達成這個目的：1) [使用 iisnode](https://harveywilliams.net/blog/installing-iisnode) \(英文\) 或[直接](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b) \(英文\)。 我們不會維護這些資源，建議[使用 Linux 伺服器來裝載您的 Node.js 應用程式](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)。
