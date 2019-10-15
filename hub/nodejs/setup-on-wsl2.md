---
title: 在 WSL 2 上設定 NodeJS
description: 協助您在適用于 Linux 的 Windows 子系統（WSL）上設定 Node.js 開發環境的指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS，node.js，windows 10，microsoft，學習 NodeJS，windows 上的節點，wsl 上的節點，在 windows 上安裝節點，使用 vs code 的 NodeJS，在 windows 上以節點進行開發，在 windows 上使用 NodeJS 進行開發，在 windows 上的 wsl 上安裝節點適用于 Linux 的子系統
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 917192d782e0a44c6de7e549960161a003c646e5
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315062"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>使用 WSL 2 設定您的 node.js 開發環境

下列逐步指南可協助您使用適用于 Linux 的 Windows 子系統（WSL）設定您的 node.js 開發環境。 本指南目前要求您安裝並執行 Windows 測試人員 Preview 組建，才能安裝和使用[WSL 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/)。 WSL 2 在 WSL 1 方面具有顯著的速度和效能改進，特別是關於 node.js。 適用于 Node.js 網頁程式開發的許多 npm 模組和教學課程是針對 Linux 使用者所撰寫，並使用以 Linux 為基礎的封裝和安裝工具。 大部分的 web 應用程式也會部署在 Linux 上，因此使用 WSL 2 可確保您的開發與生產環境之間具有一致性。

> [!NOTE]
> 如果您已認可直接在 Windows 上使用 node.js，或打算使用 Windows Server 生產環境，請參閱我們的指南，[直接在 windows 上設定您的 node.js 開發環境](./setup-on-windows.md)。

## <a name="install-windows-10-insider-preview-build"></a>安裝 Windows 10 Insider preview 組建

1. **[安裝最新版本的 Windows 10](https://www.microsoft.com/software-download/windows10)** ：選取 [**立即更新**] 以下載更新助理。 下載之後，請開啟 [更新助理] 以查看您目前是否正在執行最新版本的 Windows，如果沒有，請選取 [助理] 視窗內的 [**立即更新**]，以更新您的電腦。 *（如果您執行的是 Windows 10 的最新版本，則此步驟是選擇性的）。*

    ![Windows Update 助理](../images/windows-update-assistant2019.png)

2. **[移至 [開始] > 設定 > Windows 測試人員程式](ms-settings:windowsinsider)** ：在 [Windows 測試人員程式] 視窗中 **，選取 [開始**使用]，然後按一下 [**連結帳戶**]。

    ![Windows 測試人員程式設定](../images/windows-insider-program-settings.png)

3. **[註冊為 Windows 測試人員](https://insider.windows.com/getting-started/#register)** ：如果您未向測試人員計畫註冊，則必須使用您的[Microsoft 帳戶](https://account.microsoft.com/account)來執行此動作。

    ![Windows 測試人員註冊](../images/windows-insider-account.png)

4. 選擇接收**快速鈴聲**更新，或**直接跳至下一個 Windows 發行**內容。 確認並選擇**稍後重新開機**。 在重新開機之前，我們必須先變更幾個額外的設定。

    ![Windows 測試人員快速環形](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>啟用適用于 Linux 和虛擬機器平臺的 Windows 子系統

1. 仍在 [ **Windows 設定**] 中，搜尋 [**開啟或關閉 windows 功能**]。
2. [ **Windows 功能**] 清單出現之後，請流覽至 [尋找 Linux 的**虛擬機器平臺**和**Windows 子系統**]，確認已核取此方塊以啟用兩者，然後選取 **[確定]** 。
3. 出現提示時，重新啟動您的電腦。

    ![啟用 Windows 功能](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>安裝 Linux 散發套件

有數個 Linux 散發套件可在 WSL 上執行。 您可以在 Microsoft Store 中尋找並安裝我的最愛。 我們建議從[Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)開始，因為它是最新、熱門且受到妥善支援。

1. 開啟此[Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)連結，開啟 [Microsoft Store]，然後選取 [**取得**]。 *（這是相當大的下載，可能需要一些時間才能安裝）。*

2. 下載完成之後，從 [**開始**] 功能表中，選取 [從 Microsoft Store**啟動**] 或 [啟動]，方法是輸入 "Ubuntu 18.04 LTS"。

3. 當您第一次執行散發套件時，系統會要求您建立帳戶名稱和密碼。 在此之後，預設會自動以此使用者身分登入。 您可以選擇任何使用者名稱和密碼。 它們不會影響您的 Windows 使用者名稱。

    ![Microsoft Store 中的 Linux 發行版本](../images/store-linux-distros.png)

您可以藉由輸入下列內容來檢查您目前使用的 Linux 散發套件： `lsb_release -dc`。 若要更新您的 Ubuntu 散發套件，請使用： `sudo apt update && sudo apt upgrade`。 我們建議定期更新，以確保您擁有最新的套件。 Windows 不會自動處理這種更新。 如需 Microsoft Store 中可用的其他 Linux 發行版本連結、替代安裝方法或疑難排解，請參閱適用于[windows 10 的 Windows 子系統 For Linux 安裝指南](https://docs.microsoft.com/windows/wsl/install-win10)。

## <a name="install-wsl-2"></a>安裝 WSL 2

WSL 2 是 WSL 中[新版本的架構](https://docs.microsoft.com/windows/wsl/wsl2-about)，可變更 Linux 散發版本與 Windows 互動的方式、提升效能，以及增加系統呼叫的完整相容性。

1. 在 PowerShell 中，輸入命令： `wsl -l`，以查看您已在電腦上安裝的 WSL 散發套件清單。 您現在應該會在這份清單中看到 Ubuntu-18.04。
2. 現在，輸入命令： `wsl --set-version Ubuntu-18.04 2`，將您的 Ubuntu 安裝設定為使用 WSL 2。
3. 確認每個已安裝的發行版本使用的 WSL 版本為： `wsl --list --verbose` （或 `wsl -l -v`）。

    ![適用于 Linux 的 Windows 子系統設定版本](../images/wsl-versions.png)

> [!TIP]
> 您可以依照相同的指示（使用 PowerShell），將您已安裝的任何 Linux 散發套件設定為 WSL 2，只需將 ' Ubuntu-18.04 ' 變更為您想要設為目標的已安裝散發版本名稱。 若要變更回 WSL 1，請執行上述相同的命令，但將 ' 2 ' 取代為 ' 1 '。  您也可以輸入下列內容，將 WSL 2 設定為任何新安裝之散發套件的預設值： `wsl --set-default-version 2`。

## <a name="install-nvm-nodejs-and-npm"></a>安裝 nvm、node.js 和 npm

有多種方式可以安裝 node.js。 我們建議使用版本管理員，因為版本會非常快速地變更。 您可能需要根據您正在處理的不同專案的需求，在多個版本之間切換。 Node 版本管理員（較常稱為 nvm）是安裝多個 node.js 版本的最受歡迎方式。 我們會逐步解說安裝 nvm 的步驟，然後使用它來安裝 node.js 和 Node Package Manager （npm）。 在下一節中，我們也會考慮[其他版本管理員](#alternative-version-managers)。

> [!IMPORTANT]
> 在安裝版本管理員之前，一律建議您從作業系統中移除任何現有的 node.js 或 npm 安裝，因為不同類型的安裝可能會導致奇怪且困惑的衝突。 例如，可以使用 Ubuntu 的 `apt-get` 命令安裝的節點版本目前已過時。 如需移除先前安裝的說明，請參閱[如何從 ubuntu 移除 nodejs](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04)。）

1. 開啟您的 Ubuntu 18.04 命令列。
2. 安裝捲曲（命令列中用來從網際網路下載內容的工具）與： `sudo apt-get install curl`
3. 安裝 nvm，包含： `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash`
4. 若要確認安裝，請輸入： `command -v nvm` .。。這應該會傳回 ' nvm '，如果您收到「找不到命令」或完全沒有回應，請關閉目前的終端機並重新開啟，然後再試一次。 若要[深入瞭解，請 nvm github](https://github.com/nvm-sh/nvm)存放庫。
5. 列出目前已安裝的節點版本（此時應該為 [無]）： `nvm ls`

    ![未顯示節點版本的 NVM 清單](../images/nvm-no-node.png)

6. 安裝目前版本的 node.js （以測試最新的功能改進，但較可能發生問題）： `nvm install node`
7. 安裝 node.js 的最新穩定 LTS 版本（建議選項）： `nvm install --lts`
8. 列出已安裝的節點版本： `nvm ls` .。。現在您應該會看到您剛才安裝的兩個版本。

    ![顯示 LTS 和目前節點版本的 NVM 清單](../images/nvm-node-installed.png)

9. 確認已安裝 node.js，且目前的預設版本為： `node --version`。 然後，使用下列程式來確認您的 npm 是否正確：`npm --version` （您也可以使用 `which node` 或 `which npm` 查看預設版本所使用的路徑）。
10. 若要變更專案要使用的 node.js 版本，請建立新的專案目錄 `mkdir NodeTest`，並輸入目錄 `cd NodeTest`，然後輸入 `nvm use node` 來切換至目前的版本，或 `nvm use --lts` 切換至 LTS 版本。 您也可以針對您已安裝的任何其他版本使用特定的數目，例如 `nvm use v8.2.1`。 （若要列出所有可用的 node.js 版本，請使用命令： `nvm ls-remote`）。

> [!TIP]
> 如果您使用 NVM 來安裝 node.js 和 NPM，您應該不需要使用 SUDO 命令來安裝新的套件。

## <a name="alternative-version-managers"></a>替代版本管理員

雖然 nvm 目前是最受歡迎的節點版本管理員，但有幾個替代方法可以考慮：

- [n](https://www.npmjs.com/package/n#installation)是長期的 `nvm` 種替代方案，會以稍微不同的命令來完成相同的工作，並透過 `npm` 而不是 bash 腳本來安裝。
- [fnm](https://github.com/Schniz/fnm#using-a-script)是較新的版本管理員，其宣告速度會比 `nvm` 更快。 （它也會使用[Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)）。
- [Volta](https://github.com/volta-cli/volta#installing-volta)是 LinkedIn 團隊提供的新版本管理員，宣告改良的速度和跨平臺支援。
- [asdf-vm](https://asdf-vm.com/#/core-manage-asdf-vm)是一種適用于多種語言的單一 CLI，例如 ike gvm、nvm、rbenv ruby-build & pyenv （等等）。
- [nvs](https://github.com/jasongin/nvs) （節點版本切換器）是可與[VS Code 整合](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)的跨平臺 @no__t 1 替代方案。

## <a name="install-your-favorite-code-editor"></a>安裝您慣用的程式碼編輯器

我們建議使用 [**Visual Studio Code**] 搭配 node.js 開發專案的**遠端 WSL 擴充功能**。 這會將 VS Code 分割為「用戶端伺服器」架構，其中用戶端（使用者介面）會在您的 Windows 電腦和伺服器（您的程式碼、Git、外掛程式等）上執行，並從遠端執行。

- 支援以 Linux 為基礎的 Intellisense 和 linting。
- 您的專案將會在 Linux 中自動建立。
- 您可以使用在 Linux 上執行的所有擴充功能（不[起毛、NPM Intellisense、ES6 程式碼片段等等](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)）。

以終端機為基礎的文字編輯器（vim、emacs、nano）也有助於直接在主控台內進行快速變更。 （[本文](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68)會提供一項不錯的工作來說明兩者之間的差異，以及如何使用每個專案）。

> [!NOTE]
> 某些 GUI 編輯器（Atom、Sublime Text、Eclipse）可能會在存取 WSL 共用網路位置時發生問題（\\wsl $ \Ubuntu\home @ no__t-1，並會嘗試使用 Windows 工具來建立您的 Linux 檔案，這可能不是您想要的。 VS Code 中的遠端 WSL 擴充功能將會為您處理這項相容性。

若要安裝 VS Code 和遠端 WSL 擴充功能：

1. [下載並安裝適用于 Windows 的 VS Code](https://code.visualstudio.com)。 VS Code 也適用于 Linux，但適用于 Linux 的 Windows 子系統不支援 GUI 應用程式，因此我們需要將它安裝在 Windows 上。 別擔心，您仍然可以使用遠端 WSL 擴充功能，與您的 Linux 命令列和工具整合。

2. 在 VS Code 上安裝[遠端 WSL 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 這可讓您使用 WSL 做為整合式開發環境，並為您處理相容性和路徑。 [進一步瞭解](https://code.visualstudio.com/docs/remote/remote-overview)。

> [!IMPORTANT]
> 如果您已安裝 VS Code，則必須確定您有[1.35 版](https://code.visualstudio.com/updates/v1_35)或更新版本，才能安裝[遠端 WSL 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 不建議您在沒有遠端 WSL 擴充功能的 VS Code 中使用 WSL，因為您會失去自動完成、偵錯工具、linting 等的支援。有趣的事實：此 WSL 延伸模組安裝在 $HOME/.vscode-server/extensions。

### <a name="helpful-vs-code-extensions"></a>有用的 VS Code 延伸模組

雖然 VS Code 隨附許多現成可用的 node.js 開發功能，但是有一些實用的延伸可考慮在 node.js[延伸模組套件](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)中安裝。 它們包括：

- 不起毛-用來「linting」程式碼的工具。 Linting 會分析您的程式碼，並警告您可能發生的錯誤。
- npm-從命令選擇區執行 npm 腳本，並驗證在 package. json 中定義的已安裝模組。
- JavaScript （ES6）程式碼片段-在 ES6 語法中新增 JavaScript 開發的程式碼片段。
- 搜尋 node_modules-在您的專案中快速搜尋節點模組。
- NPM IntelliSense-在程式碼中新增 NPM 模組的 IntelliSense。
- 路徑 IntelliSense-自動完成程式碼中的檔案名。

全部安裝或挑選，然後選擇對您而言最有用的專案。

若要安裝 node.js 延伸模組套件：

1. 開啟 VS Code 中的 [**擴充**功能] 視窗（Ctrl + Shift + X）。

    [延伸模組] 視窗現在分為三個區段（因為您安裝了遠端 WSL 擴充功能）。
    - 「本機安裝」：安裝的擴充功能可與您的 Windows 作業系統搭配使用。
    - "WSL： Ubuntu-18.04-已安裝"：安裝的延伸模組，可用於您的 Ubuntu 作業系統（WSL）。
    - 「建議」：根據目前專案中的檔案類型，VS Code 所建議的延伸模組。

    ![本機與遠端 VS Code 擴充功能](../images/vscode-extensions-local-remote.png)

2. 在 [擴充功能] 視窗頂端的 [搜尋] 方塊中，輸入：**節點延伸**模組元件（或您要尋找之任何副檔名的名稱）。 如果您 VS Code 的本機或 WSL 實例，將會根據您目前開啟專案的位置，安裝延伸模組（或副檔名為 pack）。 您可以選取 VS Code 視窗左下角的 [遠端] 連結（以綠色顯示）。 它會提供您開啟或關閉遠端連線的選項。 在 "WSL： Ubuntu-18.04" 環境中安裝您的 node.js 延伸模組。

    ![VS Code 遠端連結](../images/wsl-remote-extension.png)

您可能想要考慮的幾個額外擴充功能包括：

- [適用于 Chrome 的偵錯工具](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code)：當您使用 node.js 在伺服器端上完成開發作業之後，您必須開發並測試用戶端。 此延伸模組整合了您的 VS Code 編輯器與 Chrome 瀏覽器的偵錯工具服務，讓專案更有效率。
- [從其他編輯器 Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)：如果您是從另一個文字編輯器（例如 Atom、Sublime、Vim、eMacs、Notepad + + 等）進行轉換，這些擴充功能可以協助您的環境直接在家裡。
- [設定同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)：可讓您使用 GitHub 在不同安裝之間同步處理您的 VS Code 設定。 如果您在不同的電腦上工作，這有助於讓您的環境在其上保持一致。

## <a name="install-windows-terminal-optional"></a>安裝 Windows 終端機（選擇性）

新的 Windows 終端機可啟用多個索引標籤（在命令提示字元、PowerShell 或多個 Linux 發行版本之間快速切換）、自訂按鍵系結（建立您自己的快速鍵以開啟或關閉索引標籤、複製 + 貼上等等）、emoji ☺和自訂主題（色彩配置、字型樣式和大小、背景影像/模糊/透明度）。 [進一步瞭解](https://devblogs.microsoft.com/commandline/)。

1. 取得[Microsoft Store 中的 Windows 終端機（預覽）](https://www.microsoft.com/store/apps/9n0dx20hk701)：透過存放區安裝，更新會自動處理。

2. 安裝之後，請開啟 Windows 終端機並選取 [**設定**]，以使用 `profile.json` 檔案來自訂您的終端機。 [深入瞭解編輯 Windows 終端機設定](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)。

    ![Windows 終端機設定](../images/windows-terminal-settings.png)

## <a name="set-up-git-optional"></a>設定 Git （選擇性）

如果您打算與其他人共同作業，或在開放原始碼網站（如 GitHub）上裝載您的專案，VS Code 支援[使用 Git 進行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的 [原始檔控制] 索引標籤會追蹤您所有的變更，並在 UI 中建立常用的 Git 命令（add、commit、push、pull）。

1. Git 隨附于適用于 Linux 的 Windows 子系統散發版本，不過，您必須設定您的 git 設定檔。 若要這樣做，請在終端機中輸入： `git config --global user.name "Your Name"`，然後 `git config --global user.email "youremail@domain.com"`。 如果您還沒有 Git 帳戶，您可以[在 GitHub 上註冊一個](https://github.com/join)。 如果您從未使用過 Git， [GitHub 指南](https://guides.github.com/)可協助您開始著手。 如果您需要編輯您的 git 設定，可以使用內建的文字編輯器（例如 nano： `nano ~/.gitconfig`）來執行此動作。

2. 我們建議您將[.gitignore](https://help.github.com/en/articles/ignoring-files)檔案新增至您的節點專案。 以下是[適用于 node.js 的 GitHub 預設 .gitignore 範本](https://github.com/github/gitignore/blob/master/Node.gitignore)。 如果您選擇[使用 GitHub 網站建立新的](https://help.github.com/articles/create-a-repo)存放庫，有一些核取方塊可用來初始化具有讀我檔案的存放庫，也就是針對 node.js 專案設定的 .gitignore 檔案，以及在需要時新增授權的選項。

## <a name="next-steps"></a>後續步驟

您現在已設定 node.js 開發環境。 若要開始使用您的 node.js 環境，請考慮嘗試下列其中一個教學課程：

- [開始使用適用于初學者的 node.js](./beginners.md)：這是一個逐步指南，可協助您開始使用 node.js 開發的新手。
- [在 Windows 上開始使用 node.js web](./web-frameworks.md)架構：此逐步指南可協助您開始在 Windows 上使用 node.js web framworks，包括下一篇、Nuxt 和 Gatsby。
- [開始將 node.js 應用程式連接到資料庫](./databases.md)：此逐步指南可協助您開始將 node.js 應用程式連接至資料庫，例如 MongoDB 或 Postgres。
- [開始使用 Docker 容器搭配 node.js](./containers.md)：此逐步指南可協助您開始使用 Docker 容器搭配您的 node.js 應用程式。
