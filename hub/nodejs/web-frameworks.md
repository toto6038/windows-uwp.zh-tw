---
title: 開始在 Windows 上使用 Node.js Web 架構
description: 本指南可協助您開始在 Windows 上使用 Node.js Web 架構。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, 了解 nodejs, windows 上的 Node, wsl 上的 Node, linux 或 windows 上的 Node, 在 windows 上安裝 Node, nodejs 與 vs code, 在 windows 上使用 Node 進行開發, 在 windows 上使用 nodejs 進行開發, 在 WSL 上安裝 Node, Windows 子系統 Linux 版上的 NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a8ce1d08136a74504e1b3bad26feadd61b72068f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517785"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>開始在 Windows 上使用 Node.js Web 架構

此逐步指南可協助您開始在 Windows 上使用 Node.js Web 架構，包括 Next.js、Nuxt.js 及 Gatsby。

## <a name="prerequisites"></a>必要條件

本指南假設您已經完成[使用 WSL 2 設定您的 Node.js 開發環境](./setup-on-wsl2.md)的步驟，包括：

- 安裝 Windows 10 Insider Preview 組建 18932 或更新版本。
- 在 Windows 上啟用 WSL 2 功能。
- 安裝 Linux 發行版本 (本範例適用 Ubuntu 18.04)。 可透過下列方式進行檢查：`wsl lsb_release -a`
- 確保 Ubuntu 18.04 發行版本是在 WSL 2 模式下執行。 (WSL 可以在 v1 或 v2 模式中執行發行版本。)可開啟 PowerShell 並輸入下列內容，以此方式進行檢查：`wsl -l -v`
- 使用 PowerShell，將 Ubuntu 18.04 設定為預設發行版本，請透過：`wsl -s ubuntu 18.04`

## <a name="get-started-with-nextjs"></a>開始使用 Node.js

Next.js 是一種架構，可讓您以 React.js、Node.js、Webpack 及 Babel.js 為基礎，建立伺服器轉譯的 JavaScript 應用程式。 基本上這是 React 的專案樣板，其設計注重最佳做法，可透過簡單、一致的方式建立「通用」Web 應用程式，幾乎無需任何設定。 這類「通用」的伺服器轉譯 Web 應用程式有時也稱為「同構」，表示用戶端和伺服器之間會共用程式碼。

如何建立 Next.js 專案 (包括安裝 next、react 及 react-dom)：

1. 開啟 WSL 終端機 (即 Ubuntu 18.04)。

2. 建立新的專案資料夾：`mkdir NextProjects`，並進入該目錄：`cd NextProjects`。

3. 安裝 Next.js 並建立專案 (將 'my-next-app' 取代為您要為應用程式取的名稱)：`npm create next-app my-next-app`。

4. 安裝套件後，將目錄變更至新的應用程式資料夾 `cd my-next-app` 中，然後使用 `code .` 在 VS Code 中開啟 Next.js 專案。 如此即可查看為應用程式建立的 Next.js 架構。 請注意，VS Code 會在 WSL-Remote 環境中開啟您的應用程式 (如 VS Code 視窗左下方的綠色索引標籤所示)。 這表示當您在 Windows 作業系統上使用 VS Code 進行編輯時，它仍會 Linux OS 上執行您的應用程式。

    ![WSL-Remote 延伸模組](../images/wsl-remote-extension.png)

5. 安裝 Next.js 之後，3 個不可不知的命令：

    - `npm run dev`，可透過熱式重新載入、檔案監看以及工作重新執行功能，執行開發執行個體。
    - `npm run build`，用於編譯專案。
    - `npm start`，用於在實際執行模式中啟動您的應用程式。

    在 VS Code 中開啟整合式 WSL 終端機 ([檢視] > [終端機]  )。 確定終端機路徑指向您的專案目錄 (即 `~/NextProjects/my-next-app$`)。 然後嘗試使用下列項目，執行 Next.js 應用程式的開發執行個體：`npm run dev`

6. 本機開發伺服器將會啟動，當專案頁面完成建置後，您的終端機會顯示「編譯成功 - [http://localhost:3000](http://localhost:3000) 已就緒」。 選取此 localhost 連結，以在 Web 瀏覽器中開啟新的 Next.js 應用程式。

    ![於 localhost:3000 執行的 Next.js 應用程式](../images/next-app.png)

7. 在 VS Code 編輯器中開啟 `pages/index.js` 檔案。 尋找頁面標題 `<h1 className='title'>Welcome to Next.js!</h1>` 並變更為 `<h1 className='title'>This is my new Next.js app!</h1>`。 在您的 Web 瀏覽器仍開啟 localhost:3000 的情況下，儲存您的變更。請注意，熱式重新載入功能會自動編譯並在瀏覽器中更新變更。

8. 接下來看看 Next.js 如何處理錯誤。 移除 `</h1>` 結尾標記，標題看起來就會如下所示：`<h1 className='title'>This is my new Next.js app!`。 儲存這項變更。請注意，瀏覽器中會顯示「無法編譯」的錯誤，而且會在終端機中告知應使用 `<h1>` 的結尾標記。 取代 `</h1>` 結尾標記並儲存，將會重新載入頁面。

您可以使用 F5 鍵，或前往功能表列的 [檢視] > [偵錯]  (Ctrl+Shift+D) 和 [檢視] > [偵錯主控台]  (Ctrl+Shift+Y)，搭配使用 VS Code 的偵錯程式與 Next.js 應用程式。 在 [偵錯] 視窗中選取齒輪圖示，即會建立啟動組態 (`launch.json`) 檔案，以儲存偵測設定的詳細資訊。 若要深入了解，請參閱 [VS Code 偵錯](https://code.visualstudio.com/docs/nodejs/nodejs-debugging) (英文)。

![VS Code 偵錯視窗與 launch.json 組態圖示](../images/vscode-debug-launch-configuration.png)

若要深入了解 Next.js，請參閱 [Next.js 文件](https://nextjs.org/docs) (英文)。

## <a name="get-started-with-nuxtjs"></a>開始使用 Nuxt.js

Nuxt.js 是一種架構，可讓您以 Vue.js、Node.js、Webpack 及 Babel.js 為基礎，建立伺服器轉譯的 JavaScript 應用程式。 該架構是受到 Next.js 所啟發。 基本這是 Vue 的專案樣板。 就像 Next.js 一樣，其設計注重最佳做法，可透過簡單、一致的方式建立「通用」Web 應用程式，幾乎無需任何設定。 這類「通用」的伺服器轉譯 Web 應用程式有時也稱為「同構」，表示用戶端和伺服器之間會共用程式碼。

如何建立 Nuxt.js 專案 (牽涉到要安裝何種整合式伺服器端架構、使用者介面架構、測試架構、模式、模組和 Linter 等各種問題)：

1. 開啟 WSL 終端機 (即 Ubuntu 18.04)。

2. 建立新的專案資料夾：`mkdir NuxtProjects`，並進入該目錄：`cd NuxtProjects`。

3. 安裝 Nuxt.js 並建立專案 (將 'my-nuxt-app' 取代為您要為應用程式取的名稱)：`npm create nuxt-app my-nuxt-app`

4. Nuxt.js 安裝程式會詢問您以下問題：
    - 專案名稱：my-nuxtjs-app
    - 專案描述：我的 Nuxt.js 應用程式描述。
    - 作者姓名：我要使用我的 GitHub 別名。
    - 選擇封裝管理員：Yarn 或 **Npm** - 本文以 NPM 為例。
    - 選擇使用者介面架構：None、Ant Design Vue、 Bootstrap Vue 等。 此範例會選擇 **Vuetify**，不過 Vue 社群有製作一篇不錯的[使用者介面架構比較摘要](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr) (英文)，可幫助您選擇最適合專案的選項。
    - 選擇自訂的伺服器架構：None、AdonisJs、Express、Fastify 等。 此範例會選擇 **None**，但是您可以在 Dev.to 網站找到 [2019-2020 伺服器架構比較](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm) (英文)。
    - 選擇 Nuxt.js 模組 (使用空白鍵來選取模組，或如果不需要任何模組，只要按 Enter 即可)：Axios (用於簡化 HTTP 要求) 或 [PWA support](https://pwa.nuxtjs.org/) (用於新增服務程式、manifest.json 檔案等)。 此範例不用新增模組。
    - 選擇 linting 工具：**ESLint**、Prettier、Lint 分段檔案。 選擇 **ESLint** (此工具可分析您的程式碼，並警告您可能發生的錯誤)。
    - 選擇測試架構：**None**、Jest、AVA。 這次選擇 **None**，因為本快速入門不會進行測試。
    - 選擇轉譯模式：**通用 (SSR)** 或單頁應用程式 (SPA)。 此範例會選擇 [通用 (SSR)]  ，但是 [Nuxt.js 文件](https://nuxtjs.org/guide#server-rendered-universal-ssr-) (英文) 有指出一些差異 -- 為了進行靜態裝載，SSR 需要執行 Node.js 伺服器，以對您的應用程式與 SPA 進行伺服器轉譯。
    - 選擇開發工具：**jsconfig.json** (建議用於 VS Code，以讓 Intellisense 程式碼完成運作)

5. 建立專案後，輸入 `cd my-nuxtjs-app` 以進入您的 Nuxt.js 專案目錄，然後輸入 `code .`，以在 VS Code WSL-Remote 環境中開啟專案。

    ![WSL-Remote 延伸模組](../images/wsl-remote-extension.png)

6. 安裝 Nuxt.js 之後，3 個不可不知的命令：

    - `npm run dev`，可透過熱式重新載入、檔案監看以及工作重新執行功能，執行開發執行個體。
    - `npm run build`，用於編譯專案。
    - `npm start`，用於在實際執行模式中啟動您的應用程式。

    在 VS Code 中開啟整合式 WSL 終端機 ([檢視] > [終端機]  )。 確定終端機路徑指向您的專案目錄 (即 `~/NuxtProjects/my-nuxt-app$`)。 然後嘗試使用下列項目，執行 Nuxt.js 應用程式的開發執行個體：`npm run dev`

6. 本機開發伺服器即會啟動 (顯示某種用戶端和伺服器編譯的酷炫進度列)。 專案建置完成後，終端機會顯示「編譯成功」以及編譯所花費的時間。 將您的 Web 瀏覽器指向 [http://localhost:3000](http://localhost:3000)，以開啟新的 Nuxt.js 應用程式。

    ![於 localhost:3000 執行的 Nuxt.js 應用程式](../images/nuxt-app.png)

7. 在 VS Code 編輯器中開啟 `pages/index.vue` 檔案。 尋找頁面標題 `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>` 並變更為 `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`。 在您的 Web 瀏覽器仍開啟 localhost:3000 的情況下，儲存您的變更。請注意，熱式重新載入功能會自動編譯並在瀏覽器中更新變更。

8. 接下來看看 Nuxt.js 如何處理錯誤。 移除 `</v-card-title>` 結尾標記，標題看起來就會如下所示：`<v-card-title class="headline">This is my new Nuxt.js app!`。 儲存這項變更。請注意，瀏覽器中會顯示編譯錯誤，而且會在終端機中告知缺少 `<v-card-title>` 的結尾標記，以及程式碼中發生錯誤的行號。 取代 `</v-card-title>` 結尾標記並儲存，將會重新載入頁面。

您可以使用 F5 鍵，或前往功能表列的 [檢視] > [偵錯]  (Ctrl+Shift+D) 和 [檢視] > [偵錯主控台]  (Ctrl+Shift+Y)，搭配使用 VS Code 的偵錯程式與 Nuxt.js 應用程式。 在 [偵錯] 視窗中選取齒輪圖示，即會建立啟動組態 (`launch.json`) 檔案，以儲存偵測設定的詳細資訊。 若要深入了解，請參閱 [VS Code 偵錯](https://code.visualstudio.com/docs/nodejs/nodejs-debugging) (英文)。

![VS Code 偵錯視窗與 launch.json 組態圖示](../images/vscode-debug-launch-configuration.png)

若要深入了解 Nuxt.js，請參閱 [Nuxt.js 指南](https://nuxtjs.org/guide) (英文)。

## <a name="get-started-with-gatsbyjs"></a>開始使用 Gatsby.js

Gatsby.js 是以 React.js 為基礎的靜態網站產生器架構，而非 Next.js 和 Nuxt.js 這類的伺服器轉譯。 靜態網站產生器會在建置時產生靜態 HTML。 不需要伺服器。 Next.js 和 Nuxt.js 會在執行階段產生 HTML (每次收到新請求時)， 需要伺服器才能執行。 Gatsby 也會指定如何 (透過 GraphQL) 處理應用程式中的資料，而 Next.js 和 Nuxt.js 則由您自己決定。

如何建立 Gatsby.js 專案：

1. 開啟 WSL 終端機 (即 Ubuntu 18.04)。
2. 建立新專案資料夾：`mkdir GatsbyProjects`，並進入該目錄：`cd GatsbyProjects`
3. 使用 npm 安裝 Gatsby CLI：`npm install -g gatsby-cli`。 安裝後，使用 `gatsby --version` 檢查版本。
4. 建立您的 Gatsby.js 專案：`gatsby new my-gatsby-app`
5. 安裝套件後，將目錄變更到新的應用程式資料夾 `cd my-gatsby-app` 中，然後使用 `code .` 在 VS Code 中開啟 Gatsby 專案。 如此即可使用 VS Code 的 [檔案總管]，查看為應用程式建立的 Gatsby.js 架構。 請注意，VS Code 會在 WSL-Remote 環境中開啟您的應用程式 (如 VS Code 視窗左下方的綠色索引標籤所示)。 這表示當您在 Windows 作業系統上使用 VS Code 進行編輯時，它仍會 Linux OS 上執行您的應用程式。

    ![WSL-Remote 延伸模組](../images/wsl-remote-extension.png)

6. 安裝 Gatsby 之後，3 個不可不知的命令：

    - `gatsby develop`，可透過熱式重新載入開發執行個體。
    - `gatsby build`，用於建立正式組建。
    - `gatsby serve`，用於在實際執行模式中啟動您的應用程式。

    在 VS Code 中開啟整合式 WSL 終端機 ([檢視] > [終端機]  )。 確定終端機路徑指向您的專案目錄 (即 `~/GatsbyProjects/my-gatsby-app$`)。 然後嘗試使用下列程式，執行新應用程式的開發執行個體：`gatsby develop`

7. 當新 Gatsby 專案完成編譯之後，您的終端機會顯示「您現在可以在瀏覽器中檢視 gatsby-starter-default。 [http://localhost:8000/](http://localhost:8000/)。」 選取此 localhost 連結，以在 Web 瀏覽器中檢視您建置的新專案。

> [!NOTE]
> 您會發現終端機的輸出結果也會告知您可以「檢視 GraphiQL，這是瀏覽器內用 IDE，以探索網站的資料和結構描述：[http://localhost:8000/___graphql](http://localhost:8000/___graphql)。」 GraphQL 會將您的 API 合併至 Gatsby 內建的自編文件 IDE (GraphiQL)。 除了探索網站的資料和結構描述以外，也可以執行 GraphQL 作業，例如查詢、變動和訂閱。 如需詳細資訊，請參閱 [GraphiQL 簡介](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/) (英文)。

8. 在 VS Code 編輯器中開啟 `src/pages/index.js` 檔案。 尋找頁面標題 `<h1 >Hi people</h1>` 並變更為 `<h1 >Hi (Your Name)!</h1>`。 在您的 Web 瀏覽器仍開啟 localhost:8000 的情況下，儲存您的變更。請注意，熱式重新載入功能會自動編譯並在瀏覽器中更新變更。

    ![於 localhost:3000 執行的 Gatsby.js 應用程式](../images/gatsby-app.png)

9. 接下來看看 Next.js 如何處理錯誤。 移除 `</h1>` 結尾標記，標題看起來就會如下所示：`<h1>Hi (Your Name)!`。 儲存這項變更。請注意，瀏覽器中會顯示「無法編譯」的錯誤，而且會在終端機中告知應使用 `<h1>` 的結尾標記。 取代 `</h1>` 結尾標記並儲存，將會重新載入頁面。

您可以使用 F5 鍵，或前往功能表列的 [檢視] > [偵錯]  (Ctrl+Shift+D) 和 [檢視] > [偵錯主控台]  (Ctrl+Shift+Y)，搭配使用 VS Code 的偵錯程式與 Next.js 應用程式。 在 [偵錯] 視窗中選取齒輪圖示，即會建立啟動組態 (`launch.json`) 檔案，以儲存偵測設定的詳細資訊。 若要深入了解，請參閱 [VS Code 偵錯](https://code.visualstudio.com/docs/nodejs/nodejs-debugging) (英文)。

![VS Code 偵錯視窗與 launch.json 組態圖示](../images/vscode-debug-launch-configuration.png)

若要深入了解 Gatsby，請參閱 [Gatsby.js 文件](https://www.gatsbyjs.org/docs/) (英文)。
