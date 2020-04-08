---
title: 在 WSL 2 上設定 NodeJS
description: 協助您在 Windows 子系統 Linux 版 (WSL) 上設定 Node.js 開發環境的指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, 了解 nodejs, windows 上的 Node, wsl 上的 Node, linux 或 windows 上的 Node, 在 windows 上安裝 Node, nodejs 與 vs code, 在 windows 上使用 Node 進行開發, 在 windows 上使用 nodejs 進行開發, 在 WSL 上安裝 Node, Windows 子系統 Linux 版上的 NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: c987f5bea387c630a1b9ef23c928d7a1bb8fadfc
ms.sourcegitcommit: cf4bf0ab4ea9019c1edc2bb96387ce6cedbe91dd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/10/2020
ms.locfileid: "75835375"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>使用 WSL 2 設定您的 Node.js 開發環境

下列逐步指南可協助您使用 Windows 子系統 Linux 版 (WSL) 來設定 Node.js 開發環境。 此指南目前要求您安裝並執行 Windows 測試人員預覽版，才能安裝並使用 [WSL 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) \(英文\)。 相較於 WSL 1，WSL 2 的速度和效能已有顯著改善，特別是關於 Node.js 的部分。 許多用於 Node.js Web 開發的 npm 模組和教學課程都是針對 Linux 使用者所撰寫，並使用 Linux 型封裝和安裝工具。 大部分的 Web 應用程式也會部署於 Linux 上，因此，使用 WSL 2 將確保您的開發與實際執行環境之間具有一致性。

> [!NOTE]
> 如果您已認可直接在 Windows 上使用 Node.js，或打算使用 Windows Server 實際執行環境，請參閱我們的指南，以便[直接在 Windows 上設定 Node.js 開發環境](./setup-on-windows.md)。

## <a name="install-windows-10-insider-preview-build"></a>安裝 Windows 10 Insider 預覽版

1. **[安裝最新版的 Windows 10](https://www.microsoft.com/software-download/windows10)** ：選取 [立即更新]  以下載更新小幫手。 下載之後，開啟更新小幫手以查看您目前是否正在執行最新版的 Windows，如果不是，請選取小幫手視窗內部的 [立即更新]  以更新您的電腦 (如果您執行的是最新版的 Windows 10，則此步驟是選用的)。 

    ![Windows 更新小幫手](../images/windows-update-assistant2019.png)

2. **[移至 [開始] > [設定] > [Windows 測試人員計畫]](ms-settings:windowsinsider)** ：在 [Windows 測試人員計畫] 視窗內部，選取 [開始使用]  ，然後選取 [連結帳戶]  。

    ![Windows 測試人員計畫設定](../images/windows-insider-program-settings.png)

3. **[以 Windows 測試人員身分登入](https://insider.windows.com/getting-started/#register)** \(英文\)：如果您尚未註冊測試人員計畫，將必須使用您的 [Microsoft 帳戶](https://account.microsoft.com/account)來完成註冊。

    ![Windows 測試人員註冊](../images/windows-insider-account.png)

4. 選擇接收 [快速通道]  更新或 [跳到下一個 Windows 版本]  內容。 確認並選擇 [稍後重新啟動]  。 重新啟動之前，我們必須先變更幾個額外設定。

    ![Windows 測試人員快速通道](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>啟用 Windows 子系統 Linux 版和虛擬機器平台

1. 仍在 [Windows 設定]  中，搜尋**開啟或關閉 Windows 功能**。
2. 出現 [Windows 功能]  清單之後，捲動以尋找 [虛擬機器平台]  和 [Windows 子系統 Linux 版]  、確定已核取此方塊以啟用這兩者，然後選取 [確定]  。
3. 出現提示時，重新啟動您的電腦。

    ![啟用 Windows 功能](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>安裝 Linux 發行版本

有多種 Linux 發行版本可在 WSL 上執行。 您可以在 Microsoft Store 中尋找並安裝我的最愛。 建議從使用 [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) 開始，因為它是最新、熱門且受到妥善支援。

1. 開啟此 [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) 連結、開啟 Microsoft Store，然後選取 [取得]  。 *(這是相當大的下載，因此可能需要一些時間來安裝。)*

2. 下載完成之後，從 Microsoft Store 選取 [啟動]  ，或在 [開始]  功能表中鍵入 "Ubuntu 18.04 LTS"以啟動。

3. 當您第一次執行發行版本時，系統將問您是否要建立帳戶名稱和密碼。 在此之後，預設您會以此使用者身分自動登入。 您可以選擇任何使用者名稱和密碼。 它們對您的 Windows 使用者名稱並無任何影響。

    ![Microsoft Store 中的 Linux 發行版本](../images/store-linux-distros.png)

您可以藉由輸入 `lsb_release -dc` 來檢查您目前使用的 Linux 發行版本。 若要更新您的 Ubuntu 發行版本，請使用：`sudo apt update && sudo apt upgrade`。 建議定期更新，以確保您擁有最新的套件。 Windows 不會自動處理此更新。 如需連結至 Microsoft Store 中提供的其他 Linux 發行版本、替代安裝方法或疑難排解，請參閱[適用於 Windows 10 的 Windows 子系統 Linux 版安裝指南](https://docs.microsoft.com/windows/wsl/install-win10)。

## <a name="install-wsl-2"></a>安裝 WSL 2

WSL 2 是 WSL 中[新版的架構](https://docs.microsoft.com/windows/wsl/wsl2-about)，可變更 Linux 發行版本與 Windows 互動的方式，以提升效能並新增完整的系統呼叫相容性。

1. 在 PowerShell 中，輸入命令：`wsl -l`，以檢視您已在電腦上安裝的 WSL 發行版本清單。 您現在應該會在此清單中看到 Ubuntu-18.04。
2. 現在，輸入命令：`wsl --set-version Ubuntu-18.04 2`，以將 Ubuntu 安裝設定為使用 WSL 2。
3. 確認與每個已安裝發行版本搭配使用的 WSL 版本：`wsl --list --verbose` (或 `wsl -l -v`)。

    ![Windows 子系統 Linux 版 (WSL) 設定版本](../images/wsl-versions.png)

> [!TIP]
> 您可以依照相同的指示 (使用 PowerShell)，將已安裝的任何 Linux 發行版本設定為 WSL 2，只需將 'Ubuntu-18.04' 變更為您想要設為目標的已安裝發行版本名稱。 若要變更回 WSL 1，請執行與上述相同的命令，但將 '2' 取代為 '1'。  您也可以輸入下列內容，以將 WSL 2 設定為任何新安裝之發行版本的預設值：`wsl --set-default-version 2`。

## <a name="install-nvm-nodejs-and-npm"></a>安裝 nvm、node.js 和 npm

有多種方式可以安裝 Node.js。 由於版本變更非常快速，因此，我們建議使用版本管理員。 您可能需要根據您正在處理的不同專案需求，在多個版本之間切換。 Node 版本管理員 (較常稱為 nvm) 是安裝多個 Node.js 版本的最熱門方式。 我們將逐步解說安裝 nvm 的步驟，然後用其來安裝 Node.js 和 Node 套件管理員 (npm)。 下一節也會涵蓋需考量的[替代版本管理員](#alternative-version-managers)。

> [!IMPORTANT]
> 安裝版本管理員之前，一律建議您從作業系統中移除任何現有的 Node.js 或 npm 安裝，因為不同類型的安裝可能導致奇怪且困惑的衝突。 例如，可以使用 Ubuntu 的 `apt-get` 命令安裝的 Node 版本目前已過時。 如需移除先前安裝的說明，請參閱[如何從 Ubuntu 移除 nodejs](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04) \(英文\)。

1. 開啟您的 Ubuntu 18.04 命令列。
2. 使用下列命令安裝 cURL (在命令列中，用來從網際網路下載內容的工具)：`sudo apt-get install curl`
3. 使用下列命令安裝 nvm：`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash`
4. 若要確認安裝，請輸入：`command -v nvm` ... 這應該會傳回 'nvm'，如果您收到「找不到命令」或完全沒有回應，請關閉目前的終端機並重新開啟，然後再試一次。 [在 nvm GitHub 存放庫中深入了解](https://github.com/nvm-sh/nvm) \(英文\)。
5. 列出目前已安裝的 Node 版本 (此時應該沒有任何版本)：`nvm ls`

    ![未顯示任何 Node 版本的 NVM 清單](../images/nvm-no-node.png)

6. 安裝目前的 Node.js 版本 (用以測試最新的功能改進，但較可能發生問題)：`nvm install node`
7. 安裝 Node.js 的最新穩定 LTS 版本 (建議)：`nvm install --lts`
8. 列出已安裝的 Node 版本：`nvm ls` ... 現在您應該會看到您剛安裝的兩個版本。

    ![顯示 LTS 和目前 Node 版本的 NVM 清單](../images/nvm-node-installed.png)

9. 使用下列命令來確認已安裝 Node.js 及目前預設版本：`node --version`。 然後，使用下列命令來確認您還有 npm：`npm --version` (您也可以使用 `which node` 或 `which npm` 來查看預設版本所使用的路徑)。
10. 若要變更您想要用於專案的 Node.js 版本，請建立新的專案目錄 `mkdir NodeTest` 並進入目錄 `cd NodeTest`，然後輸入 `nvm use node` 以切換至目前版本，或輸入 `nvm use --lts` 以切換至 LTS 版本。 您也可以針對已安裝的任何其他版本使用特定版本號碼，例如 `nvm use v8.2.1` (若要列出所有可用的 Node.js 版本，請使用下列命令：`nvm ls-remote`)。

> [!TIP]
> 如果您使用 NVM 來安裝 Node.js 和 NPM，則應該不需使用 SUDO 命令來安裝新套件。

> [!NOTE]
> 在發佈時，NVM v0.35.2 是最新的可用版本。 您可以查看 [GitHub 專案頁面以取得最新版的 NVM](https://github.com/nvm-sh/nvm) \(英文\)，並調整上述命令以包含最新版本。
使用 cURL 來安裝較新版的 NVM 將會取代舊版，而您使用 NVM 安裝的 Node 版本不會受到影響。 例如：`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash`

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

1. 在 VS Code 中開啟 [延伸模組]  視窗 (Ctrl+Shift+X)。

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

## <a name="install-windows-terminal-optional"></a>安裝 Windows 終端機 (選用)

新的 Windows 終端機可啟用多個索引標籤 (在命令提示字元、PowerShell 或多個 Linux 發行版本之間快速切換)、自訂按鍵繫結 (建立自己的快速鍵以開啟或關閉索引標籤、複製 + 貼上等)、Emoji ☺，以及自訂佈景主題 (色彩配置、字型樣式和大小、背景影像/柔邊/透明度)。 [深入了解](https://devblogs.microsoft.com/commandline/)。

1. [在 Microsoft Store 中取得 Windows 終端機 (預覽)](https://www.microsoft.com/store/apps/9n0dx20hk701) \(英文\)：透過 Microsoft Store 安裝，就會自動處理更新。

2. 安裝之後，開啟 Windows 終端機，然後選取 [設定]  ，以使用 `profile.json` 檔案來自訂終端機。 [深入了解如何編輯 Windows 終端機設定](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md) \(英文\)。

    ![Windows 終端機設定](../images/windows-terminal-settings.png)

## <a name="set-up-git-optional"></a>設定 Git (選用)

如果您計畫與其他人合作，或在開放原始碼網站 (如 GitHub) 上裝載您的專案，VS Code 支援[使用 Git 進行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) \(英文\)。 VS Code 中的 [原始檔控制] 索引標籤會追蹤您所有的變更，並讓常用的 Git 命令 (add、commit、push、pull) 直接內建在 UI 中。

1. Git 隨附於 Windows 子系統 Linux 版的發行版本中，不過，您必須設定自己的 Git 設定檔。 若要這樣做，請在終端機中輸入 `git config --global user.name "Your Name"`，然後輸入 `git config --global user.email "youremail@domain.com"`。 如果您還沒有 Git 帳戶，您可以[在 GitHub 上註冊一個](https://github.com/join) \(英文\)。 如果您之前從未使用過 Git，[GitHub 指南](https://guides.github.com/)可以協助您開始使用。 如果您需要編輯自己的 Git 設定，可以使用內建的文字編輯器，例如 nano：`nano ~/.gitconfig`。

2. 我們建議您將 [.gitignore 檔案](https://help.github.com/en/articles/ignoring-files) \(英文\) 新增至您的 Node 專案。 以下是[適用於 Node.js 的 GitHub 預設 gitignore 範本](https://github.com/github/gitignore/blob/master/Node.gitignore)。 如果您選擇[使用 GitHub 網站來建立新的存放庫](https://help.github.com/articles/create-a-repo) \(英文\)，有一些核取方塊可用於將具有讀我檔案的存放庫初始化、針對 Node.js 專案設定的 .gitignore 檔案，以及在需要時新增授權的選項。

## <a name="next-steps"></a>接下來的步驟

您現在已設定 Node.js 開發環境。 若要開始使用您的 Node.js 環境，請考慮嘗試下列其中一個教學課程：

- [適用於初學者的 Node.js 入門](./beginners.md)
- [開始使用 Windows 上的 Node.js Web 架構](./web-frameworks.md)
- [開始將 Node.js 應用程式連線到資料庫](./databases.md)
- [開始在 Node.js 中使用 Docker 容器](./containers.md)
