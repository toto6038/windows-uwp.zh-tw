---
title: 在原生視窗上設定 NodeJS
description: 協助您直接在 Windows 上設定 Node.js 開發環境的逐步指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node.js，windows 10，原生 windows，直接在 windows 上
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: bb950dcda9fddd6cf1b4b77da657f88ce2687785
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823592"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>直接在 Windows 上設定您的 Node.js 開發環境

以下是開始在原生 Windows 開發環境中使用 Node.js 的逐步指南。

## <a name="install-nvm-windows-nodejs-and-npm"></a>安裝 nvm-windows、node.js 和 npm

有多種方式可以安裝 Node.js。 由於版本變更非常快速，因此，我們建議使用版本管理員。 您可能需要根據您正在處理的不同專案需求，在多個版本之間切換。 節點版本管理員（較常稱為 nvm）是安裝多個版本 Node.js 的最受歡迎方式，但僅適用于 Mac/Linux 且不支援 Windows。 相反地，我們將逐步解說安裝 nvm 的步驟，然後使用它來安裝 Node.js 和 Node 套件管理員 (npm) 。 下一節也會涵蓋需考量的[替代版本管理員](#alternative-version-managers)。

> [!IMPORTANT]
> 安裝版本管理員之前，一律建議您從作業系統中移除任何現有的 Node.js 或 npm 安裝，因為不同類型的安裝可能導致奇怪且困惑的衝突。 這包括刪除任何現有的 nodejs 安裝目錄 (例如，可能會保留的 "C:\Program Files\nodejs" ) 。 NVM 產生的連結將不會覆寫現有的 (甚至是空的) 安裝目錄。 如需移除先前安裝的協助，請參閱 [如何從 Windows 完全移除 node.js](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows)。 ) 

1. 在網際網路瀏覽器中開啟 [windows nvm 儲存](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) 機制，然後選取 [ **立即下載** ] 連結。
2. 下載最新版本的 **nvm-setup.zip** 檔案。
3. 下載之後，開啟 zip 檔案，然後開啟 **nvm-setup.exe** 檔案。
4. NVM-Windows 安裝 wizard 將逐步引導您完成設定步驟，包括選擇將安裝 NVM Windows 和 Node.js 的目錄。

    ![NVM for Windows 安裝精靈](../images/install-nvm-for-windows-wizard.png)

5. 當安裝完成時。 開啟 PowerShell，並嘗試使用 windows-nvm 來列出目前安裝的節點版本 (應該是「無」，此時) ： `nvm ls`

    ![未顯示任何 Node 版本的 NVM 清單](../images/windows-nvm-powershell-no-node.png)

6. 安裝 Node.js (的最新版本，以測試最新的功能改進，但比 LTS 版本) 更可能發生問題： `nvm install latest`

7. 安裝最新的穩定 LTS 版 Node.js (建議的) ，方法是先查閱目前的 LTS 版本號碼是什麼： `nvm list available` ，然後使用： `nvm install <version>` (取代 `<version>` 為數字，即 `nvm install 12.14.0`) 。

    ![NVM 可用版本清單](../images/windows-nvm-list.png)

8. 列出已安裝的 Node 版本：`nvm ls` ... 現在您應該會看到您剛安裝的兩個版本。

    ![顯示已安裝節點版本的 NVM 清單](../images/windows-nvm-node-installs.png)

9. 安裝您需要的 Node.js 版本號碼之後，請輸入下列內容，以選取您要使用的版本： `nvm use <version>` (取代 `<version>` 為數字，即： `nvm use 12.9.0`) 。

10. 若要變更您要用於專案的 Node.js 版本，請建立新的專案目錄 `mkdir NodeTest` ，然後輸入目錄 `cd NodeTest` ，然後以您想 `nvm use <version>` `<version>` 要使用 (ie v 10.16.3 ') 的版本號碼取代。

11. 確認安裝的 npm 版本為： `npm --version` ，此版本號碼會自動變更為與您目前版本的 Node.js 相關聯的任何 npm 版本。

## <a name="alternative-version-managers"></a>替代版本管理員

雖然 windows nvm 目前是最受歡迎的 node 版本管理員，但是有一些替代方案可以考慮：

- [nvs](https://github.com/jasongin/nvs) \(英文\) (Node 版本切換器) 是跨平台的 `nvm` 替代方案，能夠[與 VS Code 整合](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md) \(英文\)。

- [Volta](https://github.com/volta-cli/volta#installing-volta) \(英文\) 是由 LinkedIn 小組所提供的新版本管理員，其聲稱已改良速度並提供跨平台支援。

若要安裝 Volta 做為您的版本管理員 (而不是 windows nvm) ，請移至《[入門指南》](https://docs.volta.sh/guide/getting-started)中的 **windows 安裝** 一節，然後下載並執行其 windows installer，並遵循安裝指示。

> [!IMPORTANT]
> 在安裝 Volta 之前，您必須確定 Windows 電腦上已啟用 [開發人員模式](/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers) 。

若要深入瞭解如何使用 Volta 在 Windows 上安裝多個版本的 Node.js，請參閱 [Volta](https://docs.volta.sh/guide/understanding#managing-your-toolchain)檔。

## <a name="install-your-favorite-code-editor"></a>安裝您慣用的程式碼編輯器

建議您 [安裝 VS Code](https://code.visualstudio.com)以及 [Node.js 擴充功能套件](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)，以在 Windows 上使用 Node.js 進行開發。 全部安裝，或挑選對您最實用的延伸模組。

安裝 Node.js 延伸模組套件：

1. 在 VS Code 中開啟 [延伸模組] 視窗 (Ctrl+Shift+X)。
2. 在 [擴充功能] 視窗頂端的 [搜尋] 方塊中，輸入： "Node Extension Pack" (或您要尋找之任何副檔名的名稱) 。
3. 選取 [安裝]。 安裝之後，您的延伸模組將會出現在 [ **擴充** 功能] 視窗的 [已啟用] 資料夾中。 您可以選取新擴充功能描述旁的齒輪圖示來停用、卸載或設定設定。

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

## <a name="use-windows-subsystem-for-linux-for-production"></a>使用適用于 Linux 的 Windows 子系統進行生產

直接在 Windows 上使用 Node.js 很適合用來學習及實驗您可以做的事。 當您準備好建立可供生產環境使用的 web 應用程式（這些應用程式通常會部署到以 Linux 為基礎的伺服器）時，建議您使用適用于 Linux 第2版的 Windows 子系統 (WSL 2) 來開發 Node.js web 應用程式。 許多 Node.js 套件和架構都是使用 * nix 環境所建立，且大部分的 Node.js 應用程式都部署在 Linux 上，因此在 WSL 上進行開發可確保您的開發和生產環境之間的一致性。 若要設定 WSL 開發環境，請參閱 [使用 WSL 2 設定您的 Node.js 開發環境](./setup-on-wsl2.md)。

> [!NOTE]
> 如果您在 (有點罕見) 需要在 Windows server 上裝載 Node.js 應用程式的情況，最常見的案例似乎是 [使用反向 proxy](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca)。 有兩種方式可以執行此作業： 1) [使用 iisnode](https://harveywilliams.net/blog/installing-iisnode) 或 [直接](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b)。 我們不會維護這些資源，並且建議 [使用 Linux 伺服器來裝載您的 Node.js 應用程式](/azure/app-service/app-service-web-get-started-nodejs)。