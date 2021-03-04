---
title: 適合初學者在 Windows 上開始使用 NodeJS
description: 協助初學者開始在 Windows 上進行 Node.js 開發的完整指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, 了解 nodejs, windows 上的 node, 適合初學者在 windows 上使用 node, 使用 windows 上的 node 進行開發, 在 windows 上使用 nodejs 的開發人員
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: ab06ba36e2c77a105912a6a7b73abcdfa992c0ad
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823892"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>適合初學者在 Windows 上開始使用 Node.js

如果您是使用 Node.js 的新手，本指南將透過一些基本概念來協助您開始使用。

## <a name="prerequisites"></a>必要條件

本指南假設您已經完成[在原生 Windows 上設定 Node.js 開發環境](./setup-on-windows.md)的步驟，包括：

- 安裝 Node.js 版本管理員。
- 安裝 Visual Studio Code。

直接在 Windows 上安裝 Node.js 是最直接的方式，可讓您以最少的設定開始執行基本的 Node.js 作業。

當您準備好使用 Node.js 來開發適用於實際執行環境的應用程式 (通常涉及部署到 Linux 伺服器) 時，建議您[使用 WSL2 設定 Node.js 開發環境](./setup-on-wsl2.md)。 雖然可以在 Windows 伺服器上部署 Web 應用程式，但[使用 Linux 伺服器來裝載您的 Node.js 應用程式](https://azure.microsoft.com/develop/nodejs/)更為常見。

## <a name="types-of-nodejs-applications"></a>Node.js 應用程式的類型

Node.js 是主要用來建立 Web 應用程式的 JavaScript 執行階段。 換句話說，其是用來撰寫應用程式後端的 JavaScript 伺服器端實作 (儘管有許多 Node.js 架構也可以處理前端)。以下提供數個您可能使用 Node.js 建立的範例。

- **單頁應用程式 (SPA)** ：這些 Web 應用程式會在瀏覽器中運作，您不需在每次用其來取得新資料時重新載入頁面。 部分範例 SPA 包括社交網路應用程式、電子郵件或地圖應用程式、線上文字或繪圖工具等。
- **即時應用程式 (RTA)** ：這些 Web 應用程式可讓使用者在作者發佈資訊時立即接收該資訊，而不要求使用者 (或軟體) 定期檢查來源以取得更新。 部分範例 RTA 包括立即訊息應用程式或聊天室、可在瀏覽器中進行的線上多人遊戲、線上共同作業文件、社群儲存空間、視訊會議應用程式等。
- **資料串流應用程式**：這些應用程式 (或服務) 會在資料/內容抵達 (或建立) 時進行傳送，同時保持連線開啟，以視需要繼續下載進一步的資料、內容或元件。 部分範例包括影片和音訊串流應用程式。
- **REST API**：這些介面可提供資料給其他人的 Web 應用程式以進行互動。 例如，行事曆 API 服務可為其他人的當地活動網站所使用的音樂會場地提供日期和時間。
- **伺服器端轉譯的應用程式 (SSR)** ：這些 Web 應用程式可以在用戶端 (在您的瀏覽器中/前端) 和伺服器 (後端) 上執行，讓頁面能夠動態顯示 (產生 HTML) 任何已知的內容，並快速抓取不知道其可供使用的內容。 這些通常稱為「同形」或「通用」應用程式。 SSR 會利用 SPA 方法，因此不需在每次使用時重新載入。 不過，SSR 提供數個對您而言可能並不重要的優點，例如，在您的網站上建立內容會出現在 Google 搜尋結果中，以及在 Twitter 或 Facebook 等社交媒體上分享您應用程式的連結時提供預覽影像。 可能的缺點是其需要持續執行的 Node.js 伺服器。 就範例而言，支援使用者想要出現在搜尋結果和社交媒體中之活動的社交網路應用程式，可能會因 SSR 而受益，而電子郵件應用程式可能會用來作為 SPA。 您也可以執行伺服器轉譯的無 SPA 應用程式，這類應用程式可能就像 WordPress 部落格一樣。 如您所見，事情可能變得很複雜，而您只需決定重要事項即可。
- **命令列工具**：這些可讓您將重複性工作自動化，然後將您的工具散發到龐大的 Node.js 生態系統。 命令列工具的範例之一是 cURL，其適用於用戶端 URL，且可用來從網際網路 URL 下載內容。 cURL 通常可用來安裝 Node.js 之類的項目，或者，在我們的案例中是 Node.js 版本管理員。
- **硬體程式設計**：儘管不像 Web 應用程式一樣普遍，但在 IoT 的使用上，Node.js 也越來越普及，例如，從感應器、指標、發送器、馬達或任何可產生大量資料的項目收集資料。 Node.js 可以啟用資料收集、分析該資料、在裝置與伺服器之間來回通訊，並根據分析採取動作。 NPM 包含 80 個以上的套件，適用於 Arduino 控制器、raspberry pi、Intel IoT Edison、各種感應器及藍牙裝置。

## <a name="try-using-nodejs-in-vs-code"></a>嘗試在 VS Code 中使用 Node.js

1. 開啟命令列 (命令提示字元、PowerShell 或任何您慣用的方式) 並建立新目錄：`mkdir HelloNode`，然後進入該目錄：`cd HelloNode`

2. 建立名為 "app.js" 的 JavaScript 檔案，並在其中包含名為 "msg" 的變數：`echo var msg > app.js`

3. 在 VS Code 中開啟目錄和您的 app.js 檔案：`code .`

4. 新增簡單的字串變數 ("Hello World")，然後在您的 "app.js" 檔案中輸入此字串，以將字串的內容傳送至您的主控台：

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. 使用 Node.js 執行 "app.js" 檔案。 選取 [檢視] > [終端機] (或選取 Ctrl+`，使用倒單引號字元)，在 VS Code 內開啟您的終端機。 如果您需要變更預設終端機，選取下拉式功能表，然後選擇 [選取預設殼層]。

6. 在終端機中輸入：`node app.js`。 您應該會看到以下輸出："Hello World"。

> [!NOTE]
> 請注意，當您在 'app.js' 檔案中輸入 `console` 時，VS Code 會顯示與 [`console`](https://developer.mozilla.org/docs/Web/API/Console) 物件相關的支援選項，讓您可以選擇使用 IntelliSense。 使用其他 [JavaScript 物件](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects) \(翻譯不完整\) 嘗試利用 Intellisense 進行實驗。

> [!TIP]
> 如果您打算使用多個命令列 (Ubuntu、PowerShell、Windows 命令提示字元等)，或想要[自訂您的終端機](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md) \(英文\) (包括文字、背景色彩、按鍵繫結關係、多個視窗窗格等)，請試用新的 [Windows 終端機](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) \(英文\)。

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>使用 Express 設定基本的 Web 應用程式架構

Express 是最小、具彈性且精簡的 Node.js 架構，讓您能夠更輕鬆地開發 Web 應用程式來處理多種類型的要求 (例如 GET、PUT、POST 及 DELETE)。 Express 具有應用程式產生器，可自動為您的應用程式建立檔案架構。

使用 Express.js 建立專案：

1. 開啟命令列 (命令提示字元、Powershell 或任何您慣用的方式)。
2. 建立新專案資料夾：`mkdir ExpressProjects`，並進入該目錄：`cd ExpressProjects`
3. 使用 Express 建立 HelloWorld 專案範本：`npx express-generator HelloWorld --view=pug`

>[!NOTE]
> 我們在此處使用 `npx` 命令來執行 Express.js Node 套件，而不需實際加以安裝 (或根據您的想法暫時安裝)。 如果您嘗試使用 `express` 命令，或使用 `express --version` 來檢查已安裝的 Express 版本，您將會收到「找不到 Express」的回應。 如果您想要全域安裝 Express 以便重複使用，請使用：`npm install -g express-generator`。 您可以使用 `npm list`，來檢視 npm 已安裝的套件清單。 系統將依深度 (巢狀目錄深度數) 列出它們。 您所安裝的套件將會位於深度 0。 該套件的相依性將會位於深度 1，進一步的相依性則會位於深度 2，依此類推。 若要深入了解，請參閱 Stackoverflow 上的 [npx 與 npm 之間的差異？](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm) \(英文\)。

4. 使用下列命令，在 VS Code 中開啟專案，以檢查 Express 所包含的檔案和資料夾：`code .`

   Express 所產生的檔案將建立一個 Web 應用程式，其使用的架構乍看之下可能讓人有點不知所措。 您將在 VS Code 的 [Explorer] 視窗看到 (Ctrl+Shift+E 來檢視) 已產生下列檔案和資料夾：

   - `bin`. 包含會啟動您應用程式的可執行檔。 它會引發伺服器 (若未提供替代連接埠，即會在連接埠 3000 上引發)，並設定基本錯誤處理。 
   - `public`. 包含所有可公開存取的檔案，包括 JavaScript 檔案、CSS 樣式表、字型檔案、影像，以及人們連線到您網站時所需的任何其他資產。
   - `routes`. 包含適用於應用程式的所有路由處理常式。 系統會在此資料夾中自動產生 `index.js` 和 `users.js` 這兩個檔案，以作為如何分離應用程式路由設定的範例。
   - `views`. 包含您範本引擎所使用的檔案。 Express 已被設定為會在系統呼叫轉譯方法時，在這裡尋找相符的檢視。 預設的範本引擎為 Jade，但 Jade 已經由 Pug 取代，因此我們會使用 `--view` 旗標來變更檢視 (範本) 引擎。 您可以使用 `express --help` 來查看 `--view` 旗標選項，以及其他選項。
   - `app.js`. 您應用程式的起點。 它會載入所有項目，並開始處理使用者要求。 基本上，它就是將所有組件黏合在一起的膠水。
   - `package.json`. 包含專案描述、指令碼管理員，以及應用程式資訊清單。 它的主要用途是追蹤您應用程式的相依性及其各自版本。

5. 您現在必須安裝 Express 用來建置及執行 HelloWorld Express 應用程式的相依性 (針對執行伺服器等工作所使用的套件，如 `package.json` 檔案中所定義)。 在 VS Code 中，選取 [檢視] > [終端機] (或選取 Ctrl + '，使用倒單引號字元) 來開啟終端機，確定您仍位於 'HelloWorld' 專案目錄中。 使用下列命令來安裝 Express 套件相依性：

```bash
npm install
```

6. 現在您已設定多頁 Web 應用程式的架構，其可存取大量的不同 API 與 HTTP 公用程式方法及中介軟體，使其能更輕鬆地建立強固的 API。 在虛擬伺服器上，輸入下列命令來啟動 Express 應用程式：

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> 上述命令的 `DEBUG=myapp:*` 部分表示您告知 Node.js，您想要基於偵錯目的開啟記錄功能。 請記得以您的應用程式名稱取代 'myapp'。 您可以在 `package.json` 檔案中，於 "name" 屬性下方找到您的應用程式名稱。 使用 `npx cross-env` 會在任何終端機中設定 `DEBUG` 環境變數，但您也可以使用終端機特定的方式進行設定。 `npm start` 命令告知 npm，在您的 `package.json` 檔案中執行指令碼。

7. 您現在可以開啟網頁瀏覽器並前往 **localhost:3000**，以檢視執行中的應用程式。

   ![在瀏覽器中執行之 Express 應用程式的螢幕擷取畫面](../images/express-app.png)

8. 現在您的 HelloWorld Express 應用程式正在瀏覽器中本機執行，請嘗試在您的專案目錄中開啟 [檢視] 資料夾，然後選取 'index.pug' 檔案來進行變更。 開啟之後，將 `h1= title` 變更為 `h1= "Hello World!"`，然後選取 [儲存] (Ctrl+S)。 在網頁瀏覽器上重新整理 **localhost:3000** URL，以檢視您的變更。

9. 若要停止執行 Express 應用程式，在終端機中輸入：**Ctrl+C**

## <a name="try-using-a-nodejs-module"></a>嘗試使用 Node.js 模組

Node.js 具有可協助您開發伺服器端 Web 應用程式的工具，部分已經內建，還有許多其他工具可透過 npm 取得。 這些模組有助於進行許多工作：

|工具               |用途                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm、sharp          |影像操作，包括直接在您的 JavaScript 程式碼中編輯、調整大小、壓縮等操作 |
|PDFKit             |產生 PDF                                                                                            |
|validator.js       |字串驗證                                                                                         |
|imagemin、UglifyJS2|縮製                                                                                              |
|spritesmith        |產生原件工作表                                                                                   |
|winston            |記錄                                                                                                  |
|commander.js       |建立命令列應用程式                                                                       |

讓我們使用內建的作業系統模組，來取得一些有關您電腦作業系統的資訊：

1) 在您的命令列中，開啟 Node.js CLI。 輸入 `node` 之後，您將會看到 `>` 提示，如此就能知道您使用的是 Node.js

2) 若要識別您目前正在使用的作業系統 (這應該會傳回回應，讓您知道您位於 Windows 上)，輸入：`os.platform()`

3) 若要檢查您的 CPU 架構，輸入：`os.arch()`

4) 若要檢視您系統上可用的 CPU，輸入：`os.cpus()`

5) 輸入 `.exit` 或選取 Ctrl+C 兩次，即可離開 Node.js CLI。

   > [!TIP]
   > 您可以使用 Node.js OS 模組，來執行檢查平台和傳回平台特定變數之類的動作：Win32/.bat (適用於 Windows 開發)、darwin/.sh (適用於 Mac/unix、Linux、SunOS) 等 (例如 `var isWin = process.platform === "win32";`)。

## <a name="next-steps"></a>接下來的步驟

在本指南中，您已了解如何使用 Node.js 來執行的一些基本事項、嘗試使用 VS Code 中的 Node.js 命令列、使用 Express.js 建立簡單的 Web 應用程式並在網頁瀏覽器中本機執行，然後嘗試使用一些內建的 Node.js 模組。 若要深入了解如何安裝及使用一些熱門的 Node.js Web 架構，請繼續進行下一份指南，其中涵蓋 Next.js (以 React 為基礎的伺服器轉譯 Web 架構)、Nuxt.js (以 Vue 為基礎的伺服器轉譯 Web 架構)，以及 Gatsby (以 React 為基礎的靜態轉譯 Web 架構)。 您也可以跳到了解如何使用 MongoDB 或 PostgreSQL 資料庫或 Docker 容器。

- [開始使用 Windows 上的 Node.js Web 架構](./web-frameworks.md)
- [開始將 Node.js 應用程式連線到資料庫](/windows/wsl/tutorials/wsl-database)
- [開始在 Node.js 中使用 Docker 容器](./containers.md)