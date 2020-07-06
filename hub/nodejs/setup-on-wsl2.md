---
title: 在 WSL 2 上設定 NodeJS
description: 協助您在 Windows 子系統 Linux 版 (WSL) 上設定 Node.js 開發環境的指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, 了解 nodejs, windows 上的 Node, wsl 上的 Node, linux 或 windows 上的 Node, 在 windows 上安裝 Node, nodejs 與 vs code, 在 windows 上使用 Node 進行開發, 在 windows 上使用 nodejs 進行開發, 在 WSL 上安裝 Node, Windows 子系統 Linux 版上的 NodeJS
ms.localizationpriority: medium
ms.date: 06/09/2020
ms.openlocfilehash: 494db609db577bd2b199f828fcf80e80a5c8c624
ms.sourcegitcommit: 22ed0d4edad5e6bab352e641cf86cf455cf83825
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2020
ms.locfileid: "85133971"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>使用 WSL 2 設定您的 Node.js 開發環境

下列逐步指南可協助您使用 Windows 子系統 Linux 版 (WSL) 來設定 Node.js 開發環境。

我們建議您安裝並執行更新的 WSL 2，因為更新版在效能速度和系統呼叫相容性方面有重大改進，並且包含可執行 [Docker Desktop](https://docs.docker.com/docker-for-windows/wsl-tech-preview/#download) 的功能。 許多用於 Node.js Web 開發的 npm 模組和教學課程都是針對 Linux 使用者所撰寫，並使用 Linux 型封裝和安裝工具。 大部分的 Web 應用程式也會部署於 Linux 上，因此，使用 WSL 2 將確保您的開發與實際執行環境之間具有一致性。

> [!NOTE]
> 如果您已認可直接在 Windows 上使用 Node.js，或打算使用 Windows Server 實際執行環境，請參閱我們的指南，以便[直接在 Windows 上設定 Node.js 開發環境](./setup-on-windows.md)。

## <a name="install-wsl-2"></a>安裝 WSL 2

若要啟用並安裝 WSL 2，請遵循 [WSL 安裝文件](https://docs.microsoft.com/windows/wsl/install-win10)中的步驟。 這些步驟會包含選擇 Linux 發行版本 (例如 Ubuntu)。

一旦您安裝了 WSL 2 和 Linux 發行版本，請開啟 Linux 發行版本 (位於 Windows 的 [開始] 功能表中)，然後使用下列命令來檢查版本和代號：`lsb_release -dc`。

我們建議您定期更新 Linux 發行版本 (包括安裝後立即更新)，以確保您擁有最新的套件。 Windows 不會自動處理此更新。 若要更新您的發行版本，請使用下列命令：`sudo apt update && sudo apt upgrade`。  

## <a name="install-windows-terminal-optional"></a>安裝 Windows 終端機 (選用)

新的 Windows 終端機可啟用多個索引標籤 (在多個 Linux 命令列、Windows 命令提示字元、PowerShell、Azure CLI 等之間快速切換)、建立自訂按鍵繫結 (開啟或關閉索引標籤、複製+貼上等的快速鍵)、使用搜尋功能及自訂佈景主題 (色彩配置、字型樣式和大小、背景影像/柔邊/透明度)。 [深入了解](https://docs.microsoft.com/windows/terminal)。

1. [在 Microsoft Store 中取得 Windows 終端機 (預覽)](https://www.microsoft.com/store/apps/9n0dx20hk701) \(英文\)：透過 Microsoft Store 安裝，就會自動處理更新。

2. 安裝之後，開啟 Windows 終端機，然後選取 [設定]，以使用 `settings.json` 檔案來自訂終端機。

    ![Windows 終端機設定](../images/windows-terminal-settings.png)

## <a name="install-nvm-nodejs-and-npm"></a>安裝 nvm、node.js 和 npm

有多種方式可以安裝 Node.js。 由於版本變更非常快速，因此，我們建議使用版本管理員。 您可能需要根據您正在處理的不同專案需求，在多個版本之間切換。 Node 版本管理員 (較常稱為 nvm) 是安裝多個 Node.js 版本的最熱門方式。 我們將逐步解說安裝 nvm 的步驟，然後用其來安裝 Node.js 和 Node 套件管理員 (npm)。 下一節也會涵蓋需考量的[替代版本管理員](#alternative-version-managers)。

> [!IMPORTANT]
> 安裝版本管理員之前，一律建議您從作業系統中移除任何現有的 Node.js 或 npm 安裝，因為不同類型的安裝可能導致奇怪且困惑的衝突。 例如，可以使用 Ubuntu 的 `apt-get` 命令安裝的 Node 版本目前已過時。 如需移除先前安裝的說明，請參閱[如何從 Ubuntu 移除 nodejs](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04) \(英文\)。

1. 開啟您的 Ubuntu 18.04 命令列。
2. 使用下列命令安裝 cURL (在命令列中，用來從網際網路下載內容的工具)：`sudo apt-get install curl`
3. 使用下列命令安裝 nvm：`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`

> [!NOTE]
> 在發佈時，NVM v0.35.3 是最新的可用版本。 您可以查看 [GitHub 專案頁面以取得最新版的 NVM](https://github.com/nvm-sh/nvm) \(英文\)，並調整上述命令以包含最新版本。
使用 cURL 來安裝較新版的 NVM 將會取代舊版，而您使用 NVM 安裝的 Node 版本不會受到影響。 例如：`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash`

4. 若要確認安裝，請輸入：`command -v nvm` ... 這應該會傳回 'nvm'，如果您收到「找不到命令」或完全沒有回應，請關閉目前的終端機並重新開啟，然後再試一次。 [在 nvm GitHub 存放庫中深入了解](https://github.com/nvm-sh/nvm) \(英文\)。
5. 列出目前已安裝的 Node 版本 (此時應該沒有任何版本)：`nvm ls`

    ![未顯示任何 Node 版本的 NVM 清單](../images/nvm-no-node.png)

6. 安裝目前的 Node.js 版本 (用以測試最新的功能改進，但較可能發生問題)：`nvm install node`
7. 安裝 Node.js 的最新穩定 LTS 版本 (建議)：`nvm install --lts`
8. 列出已安裝的 Node 版本：`nvm ls` ... 現在您應該會看到您剛安裝的兩個版本。

    ![顯示 LTS 和目前 Node 版本的 NVM 清單](../images/nvm-node-installed.png)

9. 使用下列命令來確認已安裝 Node.js 及目前預設版本：`node --version`。 然後，使用下列命令來確認您還有 npm：`npm --version` (您也可以使用 `which node` 或 `which npm` 來查看預設版本所使用的路徑)。
10. 若要變更您想要用於專案的 Node.js 版本，請建立新的專案目錄 `mkdir NodeTest` 並進入目錄 `cd NodeTest`，然後輸入 `nvm use node` 以切換至目前版本，或輸入 `nvm use --lts` 以切換至 LTS 版本。 您也可以針對已安裝的任何其他版本使用特定版本號碼，例如 `nvm use v8.2.1` (若要列出所有可用的 Node.js 版本，請使用下列命令：`nvm ls-remote`)。

如果您使用 NVM 來安裝 Node.js 和 NPM，則應該不需使用 SUDO 命令來安裝新套件。

## <a name="alternative-version-managers"></a>替代版本管理員

雖然 nvm 是目前最熱門的 Node 版本管理員，但有幾個替代方案可以考慮：

- [n](https://www.npmjs.com/package/n#installation) \(英文\) 是長期的 `nvm` 替代方案，它會以稍微不同的命令來完成相同工作，並透過 `npm` 而非 Bash 指令碼來安裝。
- [fnm](https://github.com/Schniz/fnm#using-a-script) \(英文\) 是較新的版本管理員，其聲稱速度比 `nvm` 快很多 (其也會使用 [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops) \(英文\))。
- [Volta](https://github.com/volta-cli/volta#installing-volta) \(英文\) 是由 LinkedIn 小組所提供的新版本管理員，其聲稱已改良速度並提供跨平台支援。
- [asdf-vm](https://asdf-vm.com/#/core-manage-asdf-vm) \(英文\) 是適用於多種語言的單一 CLI，例如 ike gvm、nvm、rbenv & pyenv (及更多語言) 合而為一。
- [nvs](https://github.com/jasongin/nvs) \(英文\) (Node 版本切換器) 是跨平台的 `nvm` 替代方案，能夠[與 VS Code 整合](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md) \(英文\)。

## <a name="install-your-favorite-code-editor"></a>安裝您慣用的程式碼編輯器

我們建議使用 **Visual Studio Code** 搭配適用於 Node.js 專案的 **Remote-WSL 延伸模組**。 這會將 VS Code 分割為「用戶端-伺服器」架構，其中用戶端 (使用者介面) 會在您的 Windows 電腦上執行，而伺服器 (您的程式碼、Git、外掛程式等) 會在遠端執行。

- 支援 Linux 型 Intellisense 和 Lint 分析。
- 您的專案將會在 Linux 中自動建置。
- 您可以使用在 Linux 上執行的所有延伸模組 ([ES Lint、NPM Intellisense、ES6 程式碼片段等](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack) \(英文\))。

終端機型文字編輯器 (vim、emacs、nano) 也有助於直接在主控台內進行快速變更 ([這篇文章](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68)完整說明了其中差異，並稍微解釋如何使用每一項)。

> [!NOTE]
> 某些 GUI 編輯器 (Atom、Sublime Text、Eclipse) 可能會在存取 WSL 共用網路位置 (\\wsl$\Ubuntu\home\) 時遇到問題，並將嘗試使用 Windows 工具來建置您的 Linux 檔案，但這可能不是您想要的。 VS Code 中的 Remote-WSL 延伸模組將為您處理此相容性。

安裝 VS Code 和 Remote-WSL 延伸模組：

1. [下載並安裝 VS Code for Windows](https://code.visualstudio.com)。 VS Code 也適用於 Linux，但 Windows 子系統 Linux 版不支援 GUI 應用程式，因此我們需要將它安裝在 Windows 上。 別擔心，您仍然可以使用 Remote - WSL 延伸模組，與您的 Linux 命令列和工具整合。

2. 在 VS Code 上安裝 [Remote - WSL 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 這可讓您使用 WSL 做為整合式開發環境，並為您處理相容性和路徑。 [深入了解](https://code.visualstudio.com/docs/remote/remote-overview)。

> [!IMPORTANT]
> 如果您已安裝 VS Code，則必須確定您具有 [1.35 May 版本](https://code.visualstudio.com/updates/v1_35)或更新版本，才能安裝 [Remote - WSL 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 若沒有 Remote - WSL 延伸模組，不建議您使用 WSL，因為您會失去自動完成、偵錯、linting 等的支援。有趣的事實：此 WSL 延伸模組安裝在 $HOME/.vscode-server/extensions 中。

### <a name="helpful-vs-code-extensions"></a>實用的 VS Code 延伸模組

雖然 VS Code 隨附許多現成可用的 Node.js 開發功能，但 [Node.js 延伸模組套件](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)中提供了一些您可考慮安裝的實用延伸模組。 全部安裝，或挑選對您最實用的延伸模組。

安裝 Node.js 延伸模組套件：

1. 在 VS Code 中開啟 [延伸模組] 視窗 (Ctrl+Shift+X)。

    [延伸模組] 視窗現在分為三個區段 (因為您安裝了 Remote-WSL 延伸模組)。
    - 「本機 - 已安裝」：安裝來與您 Windows 作業系統搭配使用的延伸模組。
    - 「WSL:Ubuntu-18.04 - 已安裝」：安裝來與您 Ubuntu 作業系統 (WSL) 搭配使用的延伸模組。
    - 「建議」：VS Code 根據目前專案中的檔案類型所建議的延伸模組。

    ![本機與遠端 VS Code 延伸模組](../images/vscode-extensions-local-remote.png)

2. 在 [延伸模組] 視窗頂端的搜尋方塊中，輸入：**Node 延伸模組套件** (或您要尋找之任何延伸模組的名稱)。 系統將根據您目前開啟專案的位置，為 VS Code 的本機或 WSL 執行個體安裝此延伸模組。 您可以藉由選取 VS Code 視窗左下角的遠端連結 (以綠色顯示) 來告知。 其將提供您開啟或關閉遠端連線的選項。 在 "WSL:Ubuntu-18.04" 環境中安裝您的 Node.js 延伸模組。

    ![VS Code 遠端連結](../images/wsl-remote-extension.png)

您可能想要考慮的數個額外延伸模組包括：

- [適用於 Chrome 的偵錯工具](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code) \(英文\)：當您使用 Node.js 在伺服器端完成開發之後，您將需要開發並測試用戶端。 此延伸模組會整合您的 VS Code 編輯器與 Chrome 瀏覽器偵錯服務，讓工作更有效率。
- [來自其他編輯器的按鍵對應](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)：如果您從另一個文字編輯器 (例如 Atom、Sublime、Vim、eMacs、Notepad++ 等) 進行轉換，這些延伸模組有助於讓您的環境感到非常自在。
- [設定同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) \(英文\)：可讓您使用 GitHub 同步處理不同安裝之間的 VS Code 設定。 如果您在不同的電腦上工作，這有助於讓您的環境在其上保持一致。

## <a name="set-up-git-optional"></a>設定 Git (選用)

若要在 WSL 上針對 NodeJS 專案設定 Git，請參閱 WSL 文件中的[針對 Linux 在 Windows 子系統上開始使用 Git](https://docs.microsoft.com/windows/wsl/tutorials/wsl-git) 一文。

## <a name="next-steps"></a>接下來的步驟

您現在已設定 Node.js 開發環境。 若要開始使用您的 Node.js 環境，請考慮嘗試下列其中一個教學課程：

- [適用於初學者的 Node.js 入門](./beginners.md)
- [開始使用 Windows 上的 Node.js Web 架構](./web-frameworks.md)
- [開始將 Node.js 應用程式連線到資料庫](./databases.md)
- [開始在 Node.js 中使用 Docker 容器](./containers.md)
