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
ms.openlocfilehash: 24a2ea5288a627ed884d549ca3b6f9c6930ce34e
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315132"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>在適用于初學者的 Windows 上開始使用 node.js

如果您是使用 node.js 的新手，本指南將協助您開始使用一些基本概念。

## <a name="prerequisites"></a>必要條件

本指南假設您已完成[使用 WSL 2 設定 node.js 開發環境](./setup-on-wsl2.md)的步驟，包括：

- 安裝 Windows 10 Insider Preview 組建18932或更新版本。
- 在 Windows 上啟用 WSL 2 功能。
- 安裝 Linux 散發套件（適用于我們的範例的 Ubuntu 18.04）。 您可以使用下列方式來檢查： `wsl lsb_release -a`
- 請確定您的 Ubuntu 18.04 散發套件在 WSL 2 模式下執行。 （WSL 可以在 v1 或 v2 模式中執行散發）。您可以藉由開啟 PowerShell 並輸入下列內容來檢查： `wsl -l -v`
- 使用 PowerShell 將 Ubuntu 18.04 設定為預設散發，並搭配： `wsl -s ubuntu 18.04`

> [!NOTE]
> 雖然使用適用于 Linux 的 Windows 子系統還有一些額外的設定步驟，但使用 WSL VS Code 2 和遠端 WSL 擴充功能，將提供最流暢的 node.js 開發工作流程，並與大部分的工具、操作說明文章、教學課程和[部署環境](https://docs.microsoft.com/en-us/azure/javascript/tutorial-vscode-azure-app-service-node-01)進行比對。 不過，如果您已致力於在 Windows 上使用 node.js，請參閱我們的指南，[直接在 windows 上設定您的 node.js 開發環境](./setup-on-windows.md)。 如果您在（有點罕見）的情況下需要在 Windows 伺服器上裝載 node.js 應用程式，最常見的案例就是[使用反向 proxy](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca)。 有兩種方式可執行此動作：1）[使用 iisnode](https://harveywilliams.net/blog/installing-iisnode)或[直接](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b)。 我們不會維護這些資源，並建議[使用 Linux 伺服器來裝載您的 node.js 應用程式](https://azure.microsoft.com/en-us/develop/nodejs/)。

## <a name="types-of-nodejs-applications"></a>Node.js 應用程式的類型

Node.js 是主要用來建立 web 應用程式的 JavaScript 執行時間。 換句話說，它是用來撰寫應用程式後端的 JavaScript 伺服器端執行。 （雖然許多 node.js 架構也可以處理前端）。以下是您可能使用 node.js 建立的幾個範例。

- **單頁應用程式（spa）** ：這些是在瀏覽器中工作的 web 應用程式，不需要在每次使用它來取得新資料時重載頁面。 一些範例 Spa 包括社交網路應用程式、電子郵件或地圖應用程式、線上文字或繪圖工具等等。
- **即時應用程式（rta）** ：這些是 web 應用程式，可讓使用者在作者發行時立即接收資訊，而不是要求使用者（或軟體）定期檢查來源以取得更新。 其中一些範例 Rta 包括立即訊息應用程式或聊天室、線上多人遊戲，可以在瀏覽器中播放、線上共同作業檔、社區存放裝置、影片會議應用程式等。
- **資料串流應用程式**：這些是應用程式（或服務），會在資料/內容抵達時（或已建立）進行傳送，同時保持連線開啟狀態，以便繼續視需要下載進一步的資料、內容或元件。 其中一些範例包括影片和音訊串流應用程式。
- **REST api**：這些是提供資料給其他人的 web 應用程式以進行互動的介面。 例如，行事曆 API 服務可以為其他人的本機即時網頁所使用的音樂會地點提供日期和時間。
- **伺服器端轉譯的應用程式（SSRs）** ：這些 web 應用程式可以在用戶端（在您的瀏覽器/前端）和伺服器（後端）中執行，以允許動態顯示的頁面（產生 HTML），並快速抓取不知道可用的內容。 這些通常稱為「isomorphic」或「通用」應用程式。 SSRs 利用 SPA 方法，因此不需要在每次使用時重載。 不過，SRAs 會提供幾個對您而言不重要的權益，例如在您的網站上顯示內容時，會出現在 Google 搜尋結果中，並在您的應用程式連結于 Twitter 或 Facebook 等社交媒體上共用時提供預覽影像。 可能的缺點是它們需要一個持續執行的 node.js 伺服器。 就範例而言，支援使用者會出現在搜尋結果和社交媒體中之事件的社交網路應用程式，可能會因為 SSR 而受益，而電子郵件應用程式可能會是 SPA。 您也可以執行伺服器轉譯的無 SPA 應用程式，其為類似 WordPress 的 blog。 如您所見，專案可能會變得複雜，而您只需要決定重要事項。
- **命令列工具**：這些工具可讓您自動執行重複性工作，然後將您的工具散發到龐大的 node.js 生態系統。 命令列工具的範例是捲曲的，其適用于用戶端 URL，可用來從網際網路 URL 下載內容。 捲曲通常用來安裝 node.js 之類的專案，在我們的案例中是 node.js 版本管理員。
- **硬體程式設計**：雖然不像 web apps 一樣普遍，但 Node.js 在 IoT 使用上日益普及，例如從感應器、信標、發送器、馬達或產生大量資料的任何專案收集資料。 Node.js 可以啟用資料收集、分析資料、在裝置與伺服器之間來回通訊，以及根據分析採取動作。 NPM 包含80個以上的套件，適用于 Arduino 控制器、raspberry pi、Intel IoT Edison、各種感應器和藍牙裝置。

## <a name="try-using-nodejs-in-vs-code"></a>嘗試在 VS Code 中使用 node.js

1. 開啟您的 Ubuntu 終端機並建立新的目錄： `mkdir HelloNode`，然後輸入目錄： `cd HelloNode`

2. 建立名為 "app.config" 的空白 JavaScript 檔案： `touch app.js`

3. 在 VS Code：： `code .` 中開啟目錄和空的檔案

4. 在 node.js 中建立簡單的字串變數，並在您的 ' app.config ' 檔案中輸入此字串，將其內容傳送至主控台：

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. 若要使用 node.js 執行 "app.config" 檔案。 選取 [ **View** > **terminal** ] （或選取 Ctrl + '，並使用倒引號字元），直接在 VS Code 中開啟您的 Ubuntu 終端機。 如果您需要變更預設終端機，請選取下拉式功能表，然後選擇 [**選取預設 Shell**]。 然後選取 [ **WSL** ] 做為要與 VS Code 搭配使用的預設終端介面。

6. 在終端機中，輸入： `node app.js`。 您應該會看到輸出：「Hello World」。

> [!NOTE]
> 請注意，由於您已安裝遠端 WSL 延伸模組，您的目錄將會在 Ubuntu Linux 系統上執行的遠端環境中開啟，如 VS Code 視窗左下方的綠色索引標籤所示。 此外，請注意，當您在 ' app.config ' 檔案中輸入 `console` 時，VS Code 會顯示與[`console`](https://developer.mozilla.org/docs/Web/API/Console)物件相關的支援選項，讓您可以選擇使用 IntelliSense。 嘗試使用其他[JavaScript 物件](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects)來實驗 Intellisense。

> [!TIP]
> 如果您打算使用多個命令列（Ubuntu、PowerShell、Windows 命令提示字元等），或想要[自訂您的終端](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)機（包括文字、背景色彩、按鍵系結等），請考慮試用新的[Windows 終端](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md)機。

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>使用 Express 設定基本的 web 應用程式架構

Express 是最小、有彈性且有效率的 node.js 架構，可讓您更輕鬆地開發 web 應用程式，以處理多種類型的要求，例如 GET、PUT、POST 和 DELETE。 Express 隨附一個應用程式產生器，它會自動為您的應用程式建立檔案架構。

若要使用 Express 建立專案：

1. 開啟您的 WSL 終端機（Ubuntu 18.04）。
2. 建立新的專案資料夾： `mkdir ExpressProjects` 並輸入該目錄： `cd ExpressProjects`。
3. 使用 Express 建立 HelloWorld 專案範本： `npx express-generator HelloWorld --view=pug`。

>[!NOTE]
> 我們會使用此處的 `npx` 命令來執行 Express .js 節點套件，而不需要實際安裝它（或是根據您想要的想法來暫時安裝它）。 如果您嘗試使用 [`express`] 命令，或使用： `express --version`檢查已安裝的 Express 版本，您將會收到「找不到 Express」的回應。 如果您想要全域安裝 Express 以重複使用，請使用： `npm install -g express-generator`。 您可以使用 `npm list`，來查看 npm 已安裝的套件清單。 它們會依深度列出（嵌套目錄的數目深度）。 您安裝的套件將會位於深度0。 該套件的相依性將會位於深度1，進一步的相依性則在深度2，依此類推。 若要深入瞭解，請參閱 Stackoverflow 上的[npx 與 npm 之間的差異](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm)。

4. 藉由在 VS Code 中開啟專案，檢查 Express 所包含的檔案和資料夾，以及： `code .`

   Express 所產生的檔案將會建立一個 web 應用程式，其使用的架構一開始可能會有點困難。 您會在 VS Code **Explorer**視窗中看到（Ctrl + Shift + E），其中已產生下列檔案和資料夾：

   - `bin`。 包含啟動應用程式的可執行檔。 它會引發伺服器（如果未提供替代方法，則在埠3000上），並設定基本錯誤處理。 
   - `public`。 包含所有公開存取的檔案，包括 JavaScript 檔案、CSS 樣式表單、字型檔案、影像，以及使用者連接到您的網站時所需的任何其他資產。
   - `routes`。 包含應用程式的所有路由處理常式。 系統會在此資料夾中自動產生兩個檔案，`index.js` 和 `users.js`，以作為如何分隔應用程式路由設定的範例。
   - `views`。 包含您的範本引擎所使用的檔案。 Express 已設定為在呼叫 render 方法時，在這裡尋找相符的視圖。 預設範本引擎為 Jade，但 Jade 已被取代為 Index.pug，因此我們使用 `--view` 旗標來變更視圖（範本）引擎。 您可以使用 `express --help`來查看 `--view` 旗標選項，以及其他專案。
   - `app.js`。 應用程式的起點。 它會載入所有專案，並開始提供使用者要求。 基本上是將所有元件保存在一起的膠水。
   - `package.json`。 包含專案描述、腳本管理員和應用程式資訊清單。 其主要用途是追蹤您應用程式的相依性和其各自的版本。

5. 您現在必須安裝 Express 用來建立及執行 HelloWorld Express 應用程式的相依性（如執行伺服器等工作所使用的套件，如 `package.json` 檔案中所定義）。 在 VS Code 中，選取 [ **View** > **terminal** ] （或選取 Ctrl + '，使用倒引號字元）來開啟 WSL 終端機，請確定您仍在 ' HelloWorld ' 專案目錄中。 使用下列專案來安裝 Express 套件相依性：

```bash
npm install
```

6. 此時，您已為多頁 web 應用程式設定了架構，以存取各種不同的 Api 和 HTTP 公用程式方法及中介軟體，讓您更輕鬆地建立健全的 API。 在虛擬伺服器上啟動 Express 應用程式，方法是輸入：

```bash
DEBUG=HelloWorld:* npm start
```

> [!TIP]
> 上述命令的 `DEBUG=myapp:*` 部分表示您想要為 node.js 開啟記錄功能，以進行偵錯工具。 請記得以您的應用程式名稱取代 ' myapp '。 您可以在 [名稱] 屬性底下的 package. json 檔案中找到您的應用程式名稱。 `npm start` 命令會告訴 npm 在您的 package. json 檔案中執行腳本。

7. 您現在可以開啟網頁瀏覽器並前往： **localhost： 3000** ，以查看執行中的應用程式

   ![在瀏覽器中執行之 Express 應用程式的螢幕擷取畫面](../images/express-app.png)

8. 既然您的 HelloWorld Express 應用程式是在本機瀏覽器中執行，請在您的專案目錄中開啟 [views] 資料夾，然後選取 [index.pug] 檔案，以嘗試進行變更。 開啟之後，將 `h1= title` 變更為 `h1= "Hello World!"`，然後選取 [**儲存**] （Ctrl + S）。 在您的網頁瀏覽器上重新整理**localhost： 3000** URL，以查看您的變更。

9. 若要停止執行 Express 應用程式，請在終端機中輸入： **Ctrl + C**

## <a name="try-using-a-nodejs-module"></a>嘗試使用 Node.js 模組

Node.js 有一些工具可協助您開發伺服器端 web 應用程式，其中一些內建和更多透過 npm 提供。 這些模組可協助執行許多工：

|工具               |用於                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm，升          |圖像操作，包括編輯、調整大小、壓縮等等，直接在您的 JavaScript 程式碼中 |
|PDFKit             |產生 PDF                                                                                            |
|驗證程式 .js       |字串驗證                                                                                         |
|imagemin, UglifyJS2|縮制                                                                                              |
|spritesmith        |Sprite 表產生                                                                                   |
|winston            |記錄                                                                                                  |
|指揮 .js       |建立命令列應用程式                                                                       |

讓我們使用內建的 OS 模組來取得電腦作業系統的一些資訊：

1) 在您的 WSL （Ubuntu 18.04）終端機中，開啟 node.js CLI。 在輸入之後，您會看到 `>` 提示，讓您知道您使用的是 node.js： `node`

2) 若要識別您目前正在使用的作業系統（這應該會傳迴響應，讓您知道您是在 Windows 上），請輸入： `os.platform()`

3) 若要檢查您的 CPU 架構，請輸入： `os.arch()`

4) 若要查看您系統上可用的 Cpu，請輸入： `os.cpus()`

5) 輸入 `.exit` 或選取 Ctrl + C 兩次，以離開 node.js CLI。

   > [!TIP]
   > 您可以使用 node.js OS 模組來執行檢查平臺和傳回平臺特定變數之類的動作： Win32/.bat 用於 Windows 開發、達爾文/. sh （適用于 Mac/unix、Linux、SunOS 等等）（例如 `var isWin = process.platform === "win32";`）。

## <a name="next-steps"></a>後續步驟

在本指南中，您已瞭解如何使用 node.js 來執行的一些基本事項，並嘗試使用 VS Code 中的 node.js 命令列，使用 node.js 建立簡單的 web 應用程式，並在網頁瀏覽器的本機執行，然後嘗試使用一些內建的 node.js 模組。 若要深入瞭解如何安裝及使用一些熱門的 node.js web 架構，請繼續進行下一個指南，其中包含下一篇 .js （以回應方式呈現的伺服器轉譯 web 架構）、Nuxt （以 Vue 為基礎的伺服器呈現的 web 架構）和 Gatsby （a根據回應而靜態轉譯的 web 架構。 您也可以跳到了解如何使用 MongoDB 或于 postgresql 資料庫或 Docker 容器。

- [在 Windows 上開始使用 node.js web 架構](./web-frameworks.md)
- [開始將 node.js 應用程式連接到資料庫](./databases.md)
- [開始在 node.js 中使用 Docker 容器](./containers.md)
