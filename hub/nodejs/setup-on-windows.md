---
title: 在原生視窗上設定 NodeJS
description: 協助您在 Windows 上直接設定 node.js 開發環境的指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node.js、windows 10、原生 windows，直接在 windows 上
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: eaeee6e2d55bcb9221d88bd87ebeafc7c45d0a5d
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315082"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>直接在 Windows 上設定 node.js 開發環境

以下是可讓您開始在原生 Windows 開發環境中使用 node.js 的逐步指南。

> [!NOTE]
> 雖然在 Windows 上使用 node.js 的確是可行的選項，但我們通常建議使用適用于 Linux 的 Windows 子系統（WSL）來開發 node.js web 應用程式。 許多 node.js 套件和架構都是以 * nix 環境為考慮，而且大部分的 node.js 應用程式都是部署在 Linux 上，因此在 WSL 上進行開發可確保您的開發與生產環境之間的一致性。 若要設定 WSL 開發環境，請參閱[使用 WSL 2 來設定您的 node.js 開發環境](./setup-on-wsl2.md)。

## <a name="install-nvm-windows-nodejs-and-npm"></a>安裝 nvm-windows、node.js 和 npm

有多種方式可以安裝 node.js。 我們建議使用版本管理員，因為版本會非常快速地變更。 您可能需要根據您正在處理的不同專案的需求，在多個版本之間切換。 Node 版本管理員（較常稱為 nvm）是安裝多個 node.js 版本的最受歡迎方式，但僅適用于 Mac/Linux 且不受 Windows 支援。 相反地，我們將逐步解說安裝 nvm 的步驟，然後使用它來安裝 node.js 和 Node Package Manager （npm）。 在下一節中，我們也會考慮[其他版本管理員](#alternative-version-managers)。

> [!IMPORTANT]
> 在安裝版本管理員之前，一律建議您從作業系統中移除任何現有的 node.js 或 npm 安裝，因為不同類型的安裝可能會導致奇怪且困惑的衝突。 這包括刪除可能保留的任何現有 nodejs 安裝目錄（例如 "C:\Program Files\nodejs"）。 NVM 產生的符號將不會覆寫現有的（甚至是空的）安裝目錄。 如需移除先前安裝的說明，請參閱[如何從 Windows 完全移除 node.js](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows)。）

1. 在您的網際網路瀏覽器中開啟[windows nvm 存放庫](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows)，然後選取 [**立即下載**] 連結。
2. 下載最新版本的**nvm-setup .zip**檔案。
3. 下載之後，請開啟 zip 檔案，然後開啟**nvm-setup**檔案。
4. [設定-NVM-Windows 安裝程式] 會引導您完成設定步驟，包括選擇將安裝 NVM-Windows 和 node.js 的目錄。

    ![適用于 Windows 安裝精靈的 NVM](../images/install-nvm-for-windows-wizard.png)

5. 安裝完成之後。 開啟 PowerShell 並嘗試使用 windows nvm 來列出目前已安裝的節點版本（此時應該為 [無]）： `nvm ls`

    ![未顯示節點版本的 NVM 清單](../images/windows-nvm-powershell-no-node.png)

6. 安裝目前版本的 node.js （以測試最新的功能改進，但較可能會遇到與 LTS 版本不同的問題）： `nvm install latest`
7. 請先查閱目前的 LTS 版本號碼，以安裝 Node.js 的最新穩定 LTS 版本（建議）： `nvm list available`，然後使用下列程式碼安裝 LTS 版本號碼： `nvm install <version>` （將 `<version>` 取代為數字，即 `nvm install 10.16.3`）。

    ![NVM 可用版本的清單](../images/windows-nvm-list.png)

8. 列出已安裝的節點版本： `nvm ls` .。。現在您應該會看到您剛才安裝的兩個版本。

    ![顯示已安裝節點版本的 NVM 清單](../images/windows-nvm-node-installs.png)

9. 若要檢查哪個 node.js 版本目前為預設值，請輸入： `node --version`
10. 若要變更專案要使用的 node.js 版本，請建立新的專案目錄 `mkdir NodeTest`，並輸入目錄 `cd NodeTest`，然後輸入 `nvm use <version>`，並以您想要使用的版本號碼取代 `<version>` （ie v 10.16.3 '）。
11. 確認安裝了哪個版本的 npm： `npm --version`，此版本號碼會自動變更為與您目前的 node.js 版本關聯的任何 npm 版本。

## <a name="alternative-version-managers"></a>替代版本管理員

雖然 windows nvm 目前是最受歡迎的節點版本管理員，但是還有一些替代方式可考慮：

- [nvs](https://github.com/jasongin/nvs) （節點版本切換器）是可與[VS Code 整合](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)的跨平臺 @no__t 1 替代方案。
- 
- [Volta](https://github.com/volta-cli/volta#installing-volta)是 LinkedIn 團隊提供的新版本管理員，宣告改良的速度和跨平臺支援。

若要將 Volta 安裝為版本管理員（而非 windows nvm），請移至其[消費者入門指南](https://docs.volta.sh/guide/getting-started)的 [ **windows 安裝**] 區段，然後下載並執行其 windows installer，並遵循安裝指示。

> [!IMPORTANT]
> 在安裝 Volta 之前，您必須先確定已在您的 Windows 電腦上啟用[開發人員模式](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers)。

若要深入瞭解如何使用 Volta 在 Windows 上安裝多個 node.js 版本，請參閱[Volta](https://docs.volta.sh/guide/understanding#managing-your-toolchain)檔。

## <a name="install-your-favorite-code-editor"></a>安裝您慣用的程式碼編輯器

我們建議您[安裝 VS Code](https://code.visualstudio.com)和[Node.js 延伸模組套件](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)，以便在 Windows 上使用 node.js 進行開發。

Node.js 延伸模組套件包含：

- 不起毛-用來「linting」程式碼的工具。 Linting 會分析您的程式碼，並警告您可能發生的錯誤。
- npm-從命令選擇區執行 npm 腳本，並驗證在 package. json 中定義的已安裝模組。
- JavaScript （ES6）程式碼片段-在 ES6 語法中新增 JavaScript 開發的程式碼片段。
- 搜尋 node_modules-在您的專案中快速搜尋節點模組。
- NPM IntelliSense-在程式碼中新增 NPM 模組的 IntelliSense。
- 路徑 IntelliSense-自動完成程式碼中的檔案名。

全部安裝或挑選，然後選擇對您而言最有用的專案。

若要安裝 node.js 延伸模組套件：

1. 開啟 VS Code 中的 [**擴充**功能] 視窗（Ctrl + Shift + X）。
2. 在 [擴充功能] 視窗頂端的 [搜尋] 方塊中，輸入：「節點延伸模組套件」（或您要尋找之任何副檔名的名稱）。
3. 選取 [**安裝**]。 安裝之後，您的擴充功能就會出現在 [**延伸**模組] 視窗的 [已啟用] 資料夾中。 您可以藉由選取新擴充功能的描述旁的齒輪圖示，來停用、卸載或設定設定。

您可能想要考慮的幾個額外擴充功能包括：

- [適用于 Chrome 的偵錯工具](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code)：當您使用 node.js 在伺服器端上完成開發作業之後，您必須開發並測試用戶端。 此延伸模組整合了您的 VS Code 編輯器與 Chrome 瀏覽器的偵錯工具服務，讓專案更有效率。
- [從其他編輯器 Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)：如果您是從另一個文字編輯器（例如 Atom、Sublime、Vim、eMacs、Notepad + + 等）進行轉換，這些擴充功能可以協助您的環境直接在家裡。
- [設定同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)：可讓您使用 GitHub 在不同安裝之間同步處理您的 VS Code 設定。 如果您在不同的電腦上工作，這有助於讓您的環境在其上保持一致。

## <a name="install-git-optional"></a>安裝 Git （選擇性）

如果您打算與其他人共同作業，或在開放原始碼網站（如 GitHub）上裝載您的專案，VS Code 支援[使用 Git 進行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的 [原始檔控制] 索引標籤會追蹤您所有的變更，並在 UI 中建立常用的 Git 命令（add、commit、push、pull）。 您必須先安裝 Git，才能開啟 [原始檔控制] 面板。

1. 從[git-scm 網站](https://git-scm.com/download/win)下載並安裝 Git for Windows。

2. 其中包含安裝精靈，會詢問您有關 Git 安裝設定的一系列問題。 我們建議使用所有預設設定，除非您有特定原因要變更某個專案。

3. 如果您從未使用過 Git， [GitHub 指南](https://guides.github.com/)可協助您開始著手。

4. 我們建議您將[.gitignore](https://help.github.com/en/articles/ignoring-files)檔案新增至您的節點專案。 以下是[適用于 node.js 的 GitHub 預設 .gitignore 範本](https://github.com/github/gitignore/blob/master/Node.gitignore)。
