---
title: 在適用于初學者的 Windows 上開始使用 NodeJS
description: 協助初學者在 Windows 上開始使用 node.js 開發的指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS，node.js，windows 10，microsoft，學習 NodeJS，windows 上的節點，適用于初學者的 windows 上的節點，在 windows 上以節點開發，開發人員在 windows 上使用 NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 433eb5701696f590f10d8b3276481098b9ec073d
ms.sourcegitcommit: 8f9cea69f33b06166fec22677eaa43466352c14d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/04/2020
ms.locfileid: "75657080"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>在適用于初學者的 Windows 上開始使用 node.js

如果您是使用 node.js 的新手，本指南將協助您開始使用一些基本概念。

## <a name="prerequisites"></a>先決條件

本指南假設您已完成在[原生 Windows 上設定 node.js 開發環境](./setup-on-windows.md)的步驟，包括：

- 安裝 node.js 版本管理員。
- 安裝 Visual Studio Code。

直接在 Windows 上安裝 node.js 是一種最直接的方式，可讓您開始以最少的設定來執行基本的 node.js 作業。

當您準備好使用 node.js 來開發適用于生產環境的應用程式時，通常會牽涉到部署到 Linux 伺服器，我們建議您使用[WSL2 來設定您的 node.js 開發環境](./setup-on-wsl2.md)。 雖然可以在 Windows 伺服器上部署 web 應用程式，但[使用 Linux 伺服器來裝載您的 node.js 應用程式](https://azure.microsoft.com/develop/nodejs/)更為常見。

## <a name="types-of-nodejs-applications"></a>Node.js 應用程式的類型

Node.js 是主要用來建立 web 應用程式的 JavaScript 執行時間。 換句話說，它是用來撰寫應用程式後端的 JavaScript 伺服器端執行。 （雖然許多 node.js 架構也可以處理前端）。以下是您可能使用 node.js 建立的幾個範例。

- **單頁應用程式（spa）** ：這些是在瀏覽器中工作的 web 應用程式，不需要在每次使用它來取得新資料時重載頁面。 一些範例 Spa 包括社交網路應用程式、電子郵件或地圖應用程式、線上文字或繪圖工具等等。
- **即時應用程式（rta）** ：這些是 web 應用程式，可讓使用者在作者發行時立即接收資訊，而不是要求使用者（或軟體）定期檢查來源以取得更新。 其中一些範例 Rta 包括立即訊息應用程式或聊天室、線上多人遊戲，可以在瀏覽器中播放、線上共同作業檔、社區存放裝置、影片會議應用程式等。
- **資料串流應用程式**：這些是應用程式（或服務），會在資料/內容抵達時（或已建立）進行傳送，同時保持連線開啟狀態，以便繼續視需要下載進一步的資料、內容或元件。 其中一些範例包括影片和音訊串流應用程式。
- **REST api**：這些是提供資料給其他人的 web 應用程式以進行互動的介面。 例如，行事曆 API 服務可以為其他人的本機即時網頁所使用的音樂會地點提供日期和時間。
- **伺服器端轉譯的應用程式（SSRs）** ：這些 web 應用程式可以在用戶端（在您的瀏覽器/前端）和伺服器（後端）中執行，以允許動態顯示的頁面（產生 HTML），並快速抓取不知道可用的內容。 這些通常稱為「isomorphic」或「通用」應用程式。 SSRs 利用 SPA 方法，因此不需要在每次使用時重載。 不過，SSRs 會提供幾個對您而言不重要的權益，例如在您的網站上建立內容時，會出現在 Google 搜尋結果中，並在您的應用程式連結于 Twitter 或 Facebook 等社交媒體上共用時提供預覽影像。 可能的缺點是它們需要一個持續執行的 node.js 伺服器。 就範例而言，支援使用者會出現在搜尋結果和社交媒體中之事件的社交網路應用程式，可能會因為 SSR 而受益，而電子郵件應用程式可能會是 SPA。 您也可以執行伺服器轉譯的無 SPA 應用程式，其為類似 WordPress 的 blog。 如您所見，專案可能會變得複雜，而您只需要決定重要事項。
- **命令列工具**：這些工具可讓您自動執行重複性工作，然後將您的工具散發到龐大的 node.js 生態系統。 命令列工具的範例是捲曲的，其適用于用戶端 URL，可用來從網際網路 URL 下載內容。 捲曲通常用來安裝 node.js 之類的專案，在我們的案例中是 node.js 版本管理員。
- **硬體程式設計**：雖然不像 web apps 一樣普遍，但 Node.js 在 IoT 使用上日益普及，例如從感應器、信標、發送器、馬達或產生大量資料的任何專案收集資料。 Node.js 可以啟用資料收集、分析資料、在裝置與伺服器之間來回通訊，以及根據分析採取動作。 NPM 包含80個以上的套件，適用于 Arduino 控制器、raspberry pi、Intel IoT Edison、各種感應器和藍牙裝置。

## <a name="try-using-nodejs-in-vs-code"></a>嘗試在 VS Code 中使用 node.js

1. 開啟命令列（命令提示字元、PowerShell 或您偏好的任何方式），然後建立新的目錄： `mkdir HelloNode`，然後輸入目錄： `cd HelloNode`

2. 在內建立名為 "app.config" 的 JavaScript 檔案，其中包含名為 "msg" 的變數： `echo var msg > app.js`

3. 在 VS Code：： `code .` 中開啟目錄和您的應用程式 .js 檔案

4. 新增簡單的字串變數（"Hello World"），然後在您的 "app.config" 檔案中輸入，將字串的內容傳送至您的主控台：

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. 若要使用 node.js 執行 "app.config" 檔案。 選取 [ **View** > **terminal** ] （或選取 Ctrl + '，並使用倒引號字元），在 VS Code 中開啟您的終端機。 如果您需要變更預設終端機，請選取下拉式功能表，然後選擇 [**選取預設 Shell**]。

6. 在終端機中，輸入： `node app.js`。 您應該會看到輸出：「Hello World」。

> [!NOTE]
> 請注意，當您在 ' app.config ' 檔案中輸入 `console` 時，VS Code 會顯示與[`console`](https://developer.mozilla.org/docs/Web/API/Console)物件相關的支援選項，讓您可以選擇使用 IntelliSense。 嘗試使用其他[JavaScript 物件](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects)來實驗 Intellisense。

> [!TIP]
> 如果您打算使用多個命令列（Ubuntu、PowerShell、Windows 命令提示字元等），或想要[自訂您的終端](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)機（包括文字、背景色彩、按鍵系結、多個視窗窗格等等），請嘗試新的[Windows 終端](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md)機。

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>使用 Express 設定基本的 Web 應用程式架構

Express 是最小、具彈性且精簡的 Node.js 架構，讓您能夠更輕鬆地開發 Web 應用程式來處理多種類型的要求 (例如 GET、PUT、POST 及 DELETE)。 Express 具有應用程式產生器，可自動為您的應用程式建立檔案架構。

若要使用 Express 建立專案：

1. 開啟命令列（命令提示字元、Powershell 或您偏好的任何內容）。
2. 建立新的專案資料夾： `mkdir ExpressProjects` 並輸入該目錄： `cd ExpressProjects`
3. 使用 Express 建立 HelloWorld 專案範本： `npx express-generator HelloWorld --view=pug`

>[!NOTE]
> 我們會使用此處的 `npx` 命令來執行 Express .js 節點套件，而不需要實際安裝它（或是根據您想要的想法來暫時安裝它）。 如果您嘗試使用 [`express`] 命令，或使用： `express --version`檢查已安裝的 Express 版本，您將會收到「找不到 Express」的回應。 如果您想要全域安裝 Express 以重複使用，請使用： `npm install -g express-generator`。 您可以使用 `npm list`，來查看 npm 已安裝的套件清單。 系統將依深度 (巢狀目錄深度數) 列出它們。 您所安裝的套件將會位於深度 0。 該套件的相依性將會位於深度 1，進一步的相依性則會位於深度 2，依此類推。 若要深入瞭解，請參閱 Stackoverflow 上的[npx 與 npm 之間的差異](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm)。

4. 藉由在 VS Code 中開啟專案，檢查 Express 所包含的檔案和資料夾，以及： `code .`

   Express 所產生的檔案將建立一個 Web 應用程式，而該應用程式所使用的架構一開始可能有點難以理解。 您會在 VS Code **Explorer**視窗中看到（Ctrl + Shift + E），其中已產生下列檔案和資料夾：

   - `bin`。 包含會啟動您應用程式的可執行檔。 它會引發伺服器 (若未提供替代連接埠，即會在連接埠 3000 上引發)，並設定基本錯誤處理。 
   - `public`。 包含所有可公開存取的檔案，包括 JavaScript 檔案、CSS 樣式表、字型檔案、影像，以及人們連線到您網站時所需的任何其他資產。
   - `routes`。 包含適用於應用程式的所有路由處理常式。 系統會在此資料夾中自動產生 `index.js` 和 `users.js` 這兩個檔案，以作為如何分離應用程式路由設定的範例。
   - `views`。 包含您範本引擎所使用的檔案。 Express 已被設定為會在系統呼叫轉譯方法時，在這裡尋找相符的檢視。 預設的範本引擎為 Jade，但 Jade 已經由 Pug 取代，因此我們會使用 `--view` 旗標來變更檢視 (範本) 引擎。 您可以使用 `express --help` 來查看 `--view` 旗標選項，以及其他選項。
   - `app.js`。 您應用程式的起點。 它會載入所有項目，並開始處理使用者要求。 基本上，它就是將所有組件黏合在一起的膠水。
   - `package.json`。 包含專案描述、指令碼管理員，以及應用程式資訊清單。 它的主要用途是追蹤您應用程式的相依性及其各自版本。

5. 您現在必須安裝 Express 用來建立及執行 HelloWorld Express 應用程式的相依性（如執行伺服器等工作所使用的套件，如 `package.json` 檔案中所定義）。 在 VS Code 中，藉由選取 [ **View** > **terminal** ] （或選取 Ctrl + '，使用倒引號字元）來開啟您的終端機，請確定您仍在 ' HelloWorld ' 專案目錄中。 使用下列專案來安裝 Express 套件相依性：

```bash
npm install
```

6. 現在您已設定多頁 Web 應用程式的架構，其可存取大量的不同 API 與 HTTP 公用程式方法及中介軟體，使其能更輕鬆地建立強固的 API。 在虛擬伺服器上啟動 Express 應用程式，方法是輸入：

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> 上述命令的 `DEBUG=myapp:*` 部分表示您想要為 node.js 開啟記錄功能，以進行偵錯工具。 請記得以您的應用程式名稱取代 ' myapp '。 您可以在 [名稱] 屬性下的 `package.json` 檔案中找到您的應用程式名稱。 使用 `npx cross-env` 會設定任何終端機中的 `DEBUG` 環境變數，但您也可以使用終端機特定方式來設定它。 `npm start` 命令會告訴 npm 在您的 `package.json` 檔案中執行腳本。

7. 您現在可以開啟網頁瀏覽器並前往： **localhost： 3000** ，以查看執行中的應用程式

   ![在瀏覽器上執行的 Express 應用程式螢幕擷取畫面](../images/express-app.png)

8. 既然您的 HelloWorld Express 應用程式是在本機瀏覽器中執行，請在您的專案目錄中開啟 [views] 資料夾，然後選取 [index.pug] 檔案，以嘗試進行變更。 開啟之後，將 `h1= title` 變更為 `h1= "Hello World!"`，然後選取 [**儲存**] （Ctrl + S）。 在您的網頁瀏覽器上重新整理**localhost： 3000** URL，以查看您的變更。

9. 若要停止執行 Express 應用程式，請在終端機中輸入： **Ctrl + C**

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

1) 在您的命令列中，開啟 node.js CLI。 在輸入之後，您會看到 `>` 提示，讓您知道您使用的是 node.js： `node`

2) 若要識別您目前正在使用的作業系統（這應該會傳迴響應，讓您知道您是在 Windows 上），請輸入： `os.platform()`

3) 若要檢查您的 CPU 架構，請輸入： `os.arch()`

4) 若要查看您系統上可用的 Cpu，請輸入： `os.cpus()`

5) 輸入 `.exit` 或選取 Ctrl+C 兩次，即可離開 Node.js CLI。

   > [!TIP]
   > 您可以使用 node.js OS 模組來執行檢查平臺和傳回平臺特定變數之類的動作： Win32/.bat 用於 Windows 開發、達爾文/. sh （適用于 Mac/unix、Linux、SunOS 等等）（例如 `var isWin = process.platform === "win32";`）。

## <a name="next-steps"></a>接下來的步驟

在本指南中，您已瞭解如何使用 node.js 來執行的一些基本事項，並嘗試使用 VS Code 中的 node.js 命令列，使用 node.js 建立簡單的 web 應用程式，並在網頁瀏覽器的本機執行，然後嘗試使用一些內建的 node.js 模組。 若要深入瞭解如何安裝及使用一些熱門的 node.js web 架構，請繼續進行下一個指南，其中包含下一篇 .js （以回應方式呈現的伺服器轉譯 web 架構）、Nuxt （以 Vue 為基礎的伺服器呈現的 web 架構）和 Gatsby （a根據回應而靜態轉譯的 web 架構。 您也可以跳到了解如何使用 MongoDB 或于 postgresql 資料庫或 Docker 容器。

- [在 Windows 上開始使用 node.js web 架構](./web-frameworks.md)
- [開始將 node.js 應用程式連接到資料庫](./databases.md)
- [開始在 node.js 中使用 Docker 容器](./containers.md)
