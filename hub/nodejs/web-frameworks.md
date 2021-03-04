---
title: 開始使用 Windows 上的 Node.js Web 架構
description: 協助您在 Windows 上開始使用 Node.js web 架構的逐步指南，包括 Next.js、Nuxt.js 和 Gatsby。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, 了解 nodejs, windows 上的 Node, wsl 上的 Node, linux 或 windows 上的 Node, 在 windows 上安裝 Node, nodejs 與 vs code, 在 windows 上使用 Node 進行開發, 在 windows 上使用 nodejs 進行開發, 在 WSL 上安裝 Node, Windows 子系統 Linux 版上的 NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: af23ec1374d5fce727579171113402536221e5f2
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823602"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>開始使用 Windows 上的 Node.js Web 架構

協助您在 Windows 上開始使用 Node.js web 架構的逐步指南，包括 Next.js、Nuxt.js 和 Gatsby。

## <a name="prerequisites"></a>必要條件

本指南假設您已經完成[使用 WSL 2 設定您的 Node.js 開發環境](./setup-on-wsl2.md)的步驟，包括：

- 安裝 Windows 10 Insider Preview 組建 18932 或更新版本。
- 在 Windows 上啟用 WSL 2 功能。
- 安裝 Linux 發行版本 (我們的範例適用 Ubuntu 18.04)。 您可以使用下列方式檢查： `wsl lsb_release -a`
- 確保 Ubuntu 18.04 發行版本是在 WSL 2 模式下執行。  (WSL 可以在 v1 或 v2 模式中執行散發。 ) 您可以開啟 PowerShell 並輸入下列內容來檢查： `wsl -l -v`
- 使用 PowerShell，將 Ubuntu 18.04 設定為您的預設散發，以及： `wsl -s ubuntu 18.04`

## <a name="get-started-with-nextjs"></a>開始使用 Node.js

Next.js 是一種架構，可根據 React.js、Node.js、Webpack 和 Babel.js 來建立伺服器呈現的 JavaScript 應用程式。 基本上，這是一個專案重複使用的反應，並強調最佳作法，讓您以簡單且一致的方式建立「通用」 web 應用程式，幾乎沒有任何設定。 這些「通用」伺服器呈現的 web 應用程式有時也稱為「isomorphic」，這表示在用戶端和伺服器之間共用程式碼。

若要建立 Next.js 專案，其中包括「下一步」、「回應」和「回應 dom」：

1. 開啟 WSL 終端機 (即 Ubuntu 18.04)。

2. 建立新的專案資料夾： `mkdir NextProjects` ，然後輸入該目錄： `cd NextProjects` 。

3. 安裝 Next.js 並建立專案 (將「我的下一個應用程式」取代為您想要用來呼叫應用程式的任何專案) ： `npm create next-app my-next-app` 。

4. 安裝套件之後，請將目錄變更為新的應用程式資料夾， `cd my-next-app` 然後使用 `code .` 在 VS Code 中開啟您的 Next.js 專案。 這可讓您查看已為您的應用程式建立的 Next.js framework。 請注意，VS Code 會在 WSL-Remote 環境中開啟您的應用程式 (如 VS Code 視窗) 左下方的綠色索引標籤中所示。 這表示，當您使用 VS Code 在 Windows OS 上進行編輯時，仍在 Linux 作業系統上執行您的應用程式。

    ![WSL-Remote 擴充功能](../images/wsl-remote-extension.png)

5. 安裝 Next.js 之後，您需要知道3個命令：

    - `npm run dev` 用於執行具有熱重載、檔案監看和工作重新執行的開發實例。
    - `npm run build` 用於編譯您的專案。
    - `npm start` 在生產模式下啟動您的應用程式。

    開啟整合于 VS Code (**View > terminal**) 的 WSL 終端機。 請確定終端機路徑指向您的專案目錄 (ie。 `~/NextProjects/my-next-app$`) 。 然後使用下列程式嘗試執行新 Next.js 應用程式的開發實例： `npm run dev`

6. 本機程式開發伺服器將會啟動，一旦您的專案頁面建立完成後，您的終端機會顯示「已順利完成編譯」 [http://localhost:3000](http://localhost:3000) 。 選取此 localhost 連結，以在網頁瀏覽器中開啟新的 Next.js 應用程式。

    ![在 localhost：3000中執行的 Next.js 應用程式](../images/next-app.png)

7. `pages/index.js`在 VS Code 編輯器中開啟檔案。 尋找頁面標題 `<h1 className='title'>Welcome to Next.js!</h1>` ，並將其變更為 `<h1 className='title'>This is my new Next.js app!</h1>` 。 如果您的網頁瀏覽器仍開啟 localhost：3000，請儲存您的變更，並注意熱重載功能會自動在瀏覽器中編譯和更新您的變更。

8. 讓我們來看看 Next.js 處理錯誤的方式。 移除 `</h1>` 結束記號，讓您的標題代碼現在看起來像這樣： `<h1 className='title'>This is my new Next.js app!` 。 儲存這項變更，並注意「無法編譯」錯誤會顯示在您的瀏覽器中，而在您的終端機中，讓您知道預期的結束記號 `<h1>` 。 取代 `</h1>` 結束記號、儲存，然後將重載頁面。

您可以藉由選取 F5 鍵，或移至 **> debug** (Ctrl + Shift + D) ，並在功能表列中 **查看 > Debug 主控台** (Ctrl + shift + Y) ，來將 VS Code 的偵錯工具與您的 Next.js 應用程式搭配使用。 如果您選取 [偵錯工具] 視窗中的齒輪圖示， `launch.json` 將會為您建立啟動設定 () 檔案，以儲存偵錯工具的詳細資料。 若要深入瞭解，請參閱 [VS Code 調試](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)程式。

![VS Code 偵錯工具視窗和設定上的 launch.js圖示](../images/vscode-debug-launch-configuration.png)

若要深入瞭解 Next.js，請參閱 [Next.js](https://nextjs.org/docs)檔。

## <a name="get-started-with-nuxtjs"></a>開始使用 Nuxt.js

Nuxt.js 是一種架構，可根據 Vue.js、Node.js、Webpack 和 Babel.js 來建立伺服器呈現的 JavaScript 應用程式。 它是 Next.js 的靈感。 基本上是 Vue 專案的重複專案。 就像 Next.js 一樣，它是以最佳作法來設計，可讓您以簡單且一致的方式建立「通用」 web 應用程式，幾乎沒有任何設定。 這些「通用」伺服器呈現的 web 應用程式有時也稱為「isomorphic」，這表示在用戶端和伺服器之間共用程式碼。

若要建立 Nuxt.js 專案，這將包括回答一連串有關您想要安裝之整合式伺服器端架構、UI 架構、測試架構、模式、模組和 linter 的問題：

1. 開啟 WSL 終端機 (即 Ubuntu 18.04)。

2. 建立新的專案資料夾： `mkdir NuxtProjects` ，然後輸入該目錄： `cd NuxtProjects` 。

3. 安裝 Nuxt.js 並建立專案 (使用您想要用來呼叫應用程式的任何專案來取代「我的 nuxt 應用程式」) ： `npm create nuxt-app my-nuxt-app`

4. Nuxt.js 安裝程式現在會詢問您下列問題：
    - 專案名稱：我的 nuxtjs-應用程式
    - 專案描述：我的 Nuxt.js 應用程式的描述。
    - 作者名稱：我使用 GitHub 別名。
    - 選擇套件管理員： Yarn 或 **Npm** -我們在範例中使用 Npm。
    - 選擇 UI 架構：無、Ant 設計 Vue、啟動程式 Vue 等等。 讓我們選擇此範例中的 **Vuetify** ，但 Vue 社區建立了 [比較這些 UI](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr) 架構的絕佳摘要，以協助您選擇最適合您的專案。
    - 選擇自訂伺服器架構：無、AdonisJs、Express、Fastify 等等。 在此範例中，我們選擇 [ **無** ]，但您可以在 Dev.to 網站上找到 [2019-2020 server framework 的比較](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm) 。
    - 選擇 Nuxt.js 模組 (使用空格鍵選取模組，或只在不需要任何) 時輸入： Axios (以簡化 HTTP 要求，) 或 [PWA 支援](https://pwa.nuxtjs.org/) 新增服務工作者、 (檔案等 manifest.js。 我們不會在此範例中新增模組。
    - 選擇 linting 工具： **ESLint**、美化、不起毛的分段檔。 讓我們選擇 **ESLint** (工具來分析您的程式碼，並警告您可能發生的錯誤) 。
    - 選擇測試架構： **None**、JEST、AVA。 讓我們選擇 [ **無** ]，因為我們不會在本快速入門中討論測試。
    - 選擇轉譯模式： **通用 (SSR)** 或單一頁面應用程式 (SPA) 。 讓我們為範例選擇 **通用 (SSR)** ，但 [Nuxt.js](https://nuxtjs.org/guide#server-rendered-universal-ssr-) 檔指出一些差異--SSR 需要執行 Node.js 伺服器來伺服器轉譯您的應用程式和 SPA 以進行靜態裝載。
    - 選擇開發工具： (建議的 VS Code **jsconfig.js** ，讓 Intellisense 程式碼完成運作) 

5. 一旦建立您的專案，請輸入 `cd my-nuxtjs-app` 您的 Nuxt.js 專案目錄，然後輸入 `code .` 以在 VS Code WSL-Remote 環境中開啟專案。

    ![WSL-Remote 擴充功能](../images/wsl-remote-extension.png)

6. 安裝 Nuxt.js 之後，您需要知道3個命令：

    - `npm run dev` 用於執行具有熱重載、檔案監看和工作重新執行的開發實例。
    - `npm run build` 用於編譯您的專案。
    - `npm start` 在生產模式下啟動您的應用程式。

    開啟整合于 VS Code (**View > terminal**) 的 WSL 終端機。 請確定終端機路徑指向您的專案目錄 (ie。 `~/NuxtProjects/my-nuxt-app$`) 。 然後使用下列程式嘗試執行新 Nuxt.js 應用程式的開發實例： `npm run dev`

6. 本機程式開發伺服器將會開始 (針對用戶端和伺服器編譯) 顯示某種酷炫的進度列。 當您的專案建立完成後，您的終端機會顯示「已成功編譯」，以及編譯所需的時間。 將您的網頁瀏覽器指向以 [http://localhost:3000](http://localhost:3000) 開啟新的 Nuxt.js 應用程式。

    ![在 localhost：3000中執行的 Nuxt.js 應用程式](../images/nuxt-app.png)

7. `pages/index.vue`在 VS Code 編輯器中開啟檔案。 尋找頁面標題 `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>` ，並將其變更為 `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>` 。 如果您的網頁瀏覽器仍開啟 localhost：3000，請儲存您的變更，並注意熱重載功能會自動在瀏覽器中編譯和更新您的變更。

8. 讓我們來看看 Nuxt.js 處理錯誤的方式。 移除 `</v-card-title>` 結束記號，讓您的標題代碼現在看起來像這樣： `<v-card-title class="headline">This is my new Nuxt.js app!` 。 儲存這項變更，並請注意，編譯錯誤會顯示在您的瀏覽器中，而在您的終端機中，讓您知道遺漏的結束記號 `<v-card-title>` ，以及在程式碼中可以找到錯誤的行號。 取代 `</v-card-title>` 結束記號、儲存，然後將重載頁面。

您可以藉由選取 F5 鍵，或移至 **> debug** (Ctrl + Shift + D) ，並在功能表列中 **查看 > Debug 主控台** (Ctrl + shift + Y) ，來將 VS Code 的偵錯工具與您的 Nuxt.js 應用程式搭配使用。 如果您選取 [偵錯工具] 視窗中的齒輪圖示， `launch.json` 將會為您建立啟動設定 () 檔案，以儲存偵錯工具的詳細資料。 若要深入瞭解，請參閱 [VS Code 調試](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)程式。

![VS Code 偵錯工具視窗和設定上的 launch.js圖示](../images/vscode-debug-launch-configuration.png)

若要深入瞭解 Nuxt.js，請參閱  [Nuxt.js 指南](https://nuxtjs.org/guide)。

## <a name="get-started-with-gatsbyjs"></a>開始使用 Gatsby.js

Gatsby.js 是以 React.js 為基礎的靜態網站產生器架構，而不是伺服器呈現，例如 Next.js 和 Nuxt.js。 靜態網站產生器會在組建時間產生靜態 HTML。 它不需要伺服器。 Next.js 和 Nuxt.js 每次) 都有新的要求時，就會在執行時間 (產生 HTML。 他們需要伺服器才能執行。 Gatsby 也會指定如何處理應用程式中的資料， (使用 GraphQL) ，而 Next.js 和 Nuxt.js 將該決策留給您。

若要建立 Gatsby.js 專案：

1. 開啟 WSL 終端機 (即 Ubuntu 18.04)。
2. 建立新專案資料夾：`mkdir GatsbyProjects`，並進入該目錄：`cd GatsbyProjects`
3. 使用 npm 來安裝 Gatsby CLI： `npm install -g gatsby-cli` 。 安裝之後，請使用來檢查版本 `gatsby --version` 。
4. 建立您的 Gatsby.js 專案： `gatsby new my-gatsby-app`
5. 安裝套件之後，請將目錄變更為新的應用程式資料夾， `cd my-gatsby-app` 然後使用 `code .` 在 VS Code 中開啟您的 Gatsby 專案。 這可讓您查看使用 VS Code 的 [檔案瀏覽器] 為您的應用程式建立的 Gatsby.js framework。 請注意，VS Code 會在 WSL-Remote 環境中開啟您的應用程式 (如 VS Code 視窗) 左下方的綠色索引標籤中所示。 這表示，當您使用 VS Code 在 Windows OS 上進行編輯時，仍在 Linux 作業系統上執行您的應用程式。

    ![WSL-Remote 擴充功能](../images/wsl-remote-extension.png)

6. 安裝 Gatsby 之後，您需要知道3個命令：

    - `gatsby develop` 用於執行具有熱重載的開發實例。
    - `gatsby build` 用於建立生產組建。
    - `gatsby serve` 在生產模式下啟動您的應用程式。

    開啟整合于 VS Code (**View > terminal**) 的 WSL 終端機。 請確定終端機路徑指向您的專案目錄 (ie。 `~/GatsbyProjects/my-gatsby-app$`) 。 然後使用下列程式嘗試執行新應用程式的開發實例： `gatsby develop`

7. 當您的新 Gatsby 專案完成編譯之後，您的終端機會顯示「您現在可以在瀏覽器中查看 Gatsby-starter-預設值。 [http://localhost:8000/](http://localhost:8000/)." 選取此 localhost 連結以查看您在網頁瀏覽器中建立的新專案。

> [!NOTE]
> 您將會注意到，您的終端機輸出也會讓您知道，您可以「查看瀏覽器中的 GraphiQL，以流覽您的網站資料和架構：」 [http://localhost:8000/___graphql](http://localhost:8000/___graphql) 。 GraphQL 會將您的 Api 整合到自我記錄 IDE 中， () GraphiQL 內建于 Gatsby 中。 除了流覽您的網站資料和架構之外，您還可以執行 GraphQL 作業，例如查詢、突變和訂閱。 如需詳細資訊，請參閱 [GraphiQL 簡介](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/)。

8. `src/pages/index.js`在 VS Code 編輯器中開啟檔案。 尋找頁面標題 `<h1 >Hi people</h1>` ，並將其變更為 `<h1 >Hi (Your Name)!</h1>` 。 如果您的網頁瀏覽器仍開啟 localhost：8000，請儲存您的變更，並注意熱重載功能會自動在瀏覽器中編譯和更新您的變更。

    ![在 localhost：3000中執行的 Gatsby.js 應用程式](../images/gatsby-app.png)

9. 讓我們來看看 Next.js 處理錯誤的方式。 移除 `</h1>` 結束記號，讓您的標題代碼現在看起來像這樣： `<h1>Hi (Your Name)!` 。 儲存這項變更，並注意「無法編譯」錯誤會顯示在您的瀏覽器中，而在您的終端機中，讓您知道預期的結束記號 `<h1>` 。 取代 `</h1>` 結束記號、儲存，然後將重載頁面。

您可以藉由選取 F5 鍵，或移至 **> debug** (Ctrl + Shift + D) ，並在功能表列中 **查看 > Debug 主控台** (Ctrl + shift + Y) ，來將 VS Code 的偵錯工具與您的 Next.js 應用程式搭配使用。 如果您選取 [偵錯工具] 視窗中的齒輪圖示， `launch.json` 將會為您建立啟動設定 () 檔案，以儲存偵錯工具的詳細資料。 若要深入瞭解，請參閱 [VS Code 調試](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)程式。

![VS Code 偵錯工具視窗和設定上的 launch.js圖示](../images/vscode-debug-launch-configuration.png)

若要深入瞭解 Gatsby，請參閱  [Gatsby.js](https://www.gatsbyjs.org/docs/)檔。
