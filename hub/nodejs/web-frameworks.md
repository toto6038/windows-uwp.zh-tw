---
title: 在 Windows 上開始使用 node.js web 架構
description: 協助您在 Windows 上開始使用 node.js web 架構的指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS，node.js，windows 10，microsoft，學習 NodeJS，windows 上的節點，wsl 上的節點，在 windows 上安裝節點，使用 vs code 的 NodeJS，在 windows 上以節點進行開發，在 windows 上使用 NodeJS 進行開發，在 windows 上的 wsl 上安裝節點適用于 Linux 的子系統
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a3c1cd980884dc50107c05207665d0c1ef88938e
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314952"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>在 Windows 上開始使用 node.js web 架構

此逐步指南可協助您開始使用 Windows 上的 node.js web 架構，包括下一篇、Nuxt 和 Gatsby。

## <a name="prerequisites"></a>必要條件

本指南假設您已完成[使用 WSL 2 設定 node.js 開發環境](./setup-on-wsl2.md)的步驟，包括：

- 安裝 Windows 10 Insider Preview 組建18932或更新版本。
- 在 Windows 上啟用 WSL 2 功能。
- 安裝 Linux 散發套件（適用于我們的範例的 Ubuntu 18.04）。 您可以使用下列方式來檢查： `wsl lsb_release -a`
- 請確定您的 Ubuntu 18.04 散發套件在 WSL 2 模式下執行。 （WSL 可以在 v1 或 v2 模式中執行散發）。您可以開啟 PowerShell 並輸入： `wsl -l -v` 來檢查此項
- 使用 PowerShell，將 Ubuntu 18.04 設定為預設散發，並使用： `wsl -s ubuntu 18.04`

## <a name="get-started-with-nextjs"></a>開始使用下一個 .js

接下來的 .js 是一種架構，可根據回應 .js、node.js、Webpack 和 Babel 來建立伺服器呈現的 JavaScript 應用程式。 基本上，它是一種專案的重複使用方法，它會特別注意最佳做法，讓您以簡單且一致的方式建立「通用」 web 應用程式，幾乎不會有任何設定。 這些「通用」伺服器呈現的 web 應用程式有時也稱為「isomorphic」，這表示程式碼會在用戶端和伺服器之間共用。

若要建立下一個 .js 專案，包括安裝下一個、回應和回應 dom：

1. 開啟您的 WSL 終端機（ie）。Ubuntu 18.04）。

2. 建立新的專案資料夾： `mkdir NextProjects` 並輸入該目錄： `cd NextProjects`。

3. 安裝下一個 .js 並建立專案（將「我的下一個應用程式」取代為您想要呼叫應用程式的任何內容）： `npm create next-app my-next-app`。

4. 安裝套件之後，請將目錄變更為新的應用程式資料夾，`cd my-next-app`，然後使用 `code .` 以在 VS Code 中開啟您的下一個 .js 專案。 這可讓您查看已針對您的應用程式建立的下一個 .js 架構。 請注意，VS Code 在 WSL 遠端環境中開啟您的應用程式（如 VS Code 視窗左下方的綠色索引標籤所示）。 這表示當您使用 VS Code 在 Windows OS 上進行編輯時，它仍會在 Linux OS 上執行您的應用程式。

    ![WSL-遠端擴充功能](../images/wsl-remote-extension.png)

5. 在下一次安裝 .js 之後，您必須知道3個命令：

    - `npm run dev` 用於執行具有熱重載的開發實例、檔案監看式和工作重新執行。
    - `npm run build`，用來編譯您的專案。
    - `npm start`，用於以生產模式啟動應用程式。

    開啟 VS Code 中整合的 WSL 終端機（**View > terminal**）。 請確定終端機路徑指向您的專案目錄（即 `~/NextProjects/my-next-app$`）。 然後嘗試使用下列程式執行新的下一個 .js 應用程式的開發實例： `npm run dev`

6. 本機開發伺服器將會啟動，一旦您的專案頁面完成建立之後，您的終端機會顯示「已成功地在[http://localhost:3000](http://localhost:3000)」上編譯。 選取此 localhost 連結，以在網頁瀏覽器中開啟新的 node.js 應用程式。

    ![您在 localhost：3000中執行的下一個 .js 應用程式](../images/next-app.png)

7. 在您的 VS Code 編輯器中開啟 `pages/index.js` 檔案。 找出頁面標題 `<h1 className='title'>Welcome to Next.js!</h1>`，並將其變更為 `<h1 className='title'>This is my new Next.js app!</h1>`。 在您的網頁瀏覽器仍然開啟至 localhost：3000時，儲存您的變更，並注意熱重載功能會自動在瀏覽器中編譯及更新您的變更。

8. 我們來看一下，接下來的 .js 如何處理錯誤。 移除 `</h1>` 結束記號，讓您的標題程式碼現在看起來像這樣： `<h1 className='title'>This is my new Next.js app!`。 儲存這項變更，請注意，您的瀏覽器中會顯示「無法編譯」錯誤，而在您的終端機中，讓您知道應該會有 `<h1>` 的結束記號。 取代 `</h1>` 結束記號、儲存，且頁面將會重載。

您可以使用 VS Code 的偵錯工具搭配您的下一個 .js 應用程式，方法是選取 F5 鍵，或移至 **> Debug** （Ctrl + Shift + D），並在功能表列中查看 **> 偵錯主控台**（Ctrl + shift + Y）。 如果您在 [偵錯工具] 視窗中選取齒輪圖示，將會為您建立啟動設定（`launch.json`）檔案，以儲存偵錯工具的安裝詳細資料。 若要深入瞭解，請參閱[VS Code 的調試](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations)。

![VS Code 的 [debug] 視窗和 [啟動]。 json 設定圖示](../images/vscode-debug-launch-configuration.png)

若要深入瞭解下一個 .js，請參閱[下一個 .js](https://nextjs.org/docs)檔。

## <a name="get-started-with-nuxtjs"></a>開始使用 Nuxt

Nuxt 是一種架構，可用於根據 Vue、node.js、Webpack 和 Babel 建立伺服器呈現的 JavaScript 應用程式。 它是由下一個 .js 所啟發。 基本上，它是 Vue 的專案樣板。 就像下一的 .js 一樣，它會特別注意最佳做法，並可讓您以簡單且一致的方式建立「通用」 web 應用程式，幾乎不會有任何設定。 這些「通用」伺服器呈現的 web 應用程式有時也稱為「isomorphic」，這表示程式碼會在用戶端和伺服器之間共用。

若要建立 Nuxt 專案，其中包括回答一系列有關您想要安裝之整合式伺服器端架構、UI 架構、測試架構、模式、模組和 linter 的問題：

1. 開啟您的 WSL 終端機（ie）。Ubuntu 18.04）。

2. 建立新的專案資料夾： `mkdir NuxtProjects` 並輸入該目錄： `cd NuxtProjects`。

3. 安裝 Nuxt 並建立專案（將「我的 Nuxt 應用程式」取代為您想要呼叫應用程式的任何內容）： `npm create nuxt-app my-nuxt-app`

4. Nuxt 安裝程式現在會詢問您下列問題：
    - 專案名稱：我的 nuxtjs-應用程式
    - 專案描述：我的 Nuxt 應用程式的描述。
    - 作者姓名：我使用我的 GitHub 別名。
    - 選擇 [套件管理員]：Yarn 或**Npm** -我們在範例中使用 Npm。
    - 選擇 UI 架構：無、Ant 設計 Vue、啟動程式 Vue 等等。 在此範例中，我們將選擇**Vuetify** ，但是 Vue 的社區建立了一個比較好的[摘要](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr)，以協助您選擇最適合您的專案。
    - 選擇自訂伺服器架構：None、AdonisJs、Express、Fastify 等等。 讓我們在此範例中選擇 [**無**]，但您可以在 Dev.to 網站上找到[2019-2020 server framework 的比較](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm)。
    - 選擇 [Nuxt] 模組（使用空格鍵選取模組，或只在您不想要的情況中輸入）：Axios （用於簡化 HTTP 要求）或[PWA 支援](https://pwa.nuxtjs.org/)（用於新增服務背景工作、資訊清單 json 檔案等）。 讓我們不要新增此範例的模組。
    - 選擇 [linting 工具]：**ESLint**、美化、不起毛的分段檔案。 讓我們選擇 [ **ESLint** ] （用來分析程式碼的工具，並警告您可能發生的錯誤）。
    - 選擇測試架構：**None**、JEST、AVA。 讓我們選擇 [**無**]，因為我們不會在本快速入門中討論測試。
    - 選擇轉譯模式：**通用（SSR）** 或單一頁面應用程式（SPA）。 我們在範例中選擇**通用（SSR）** ，但是[Nuxt](https://nuxtjs.org/guide#server-rendered-universal-ssr-)檔指出一些差異--SSR 需要執行的 node.js 伺服器，以伺服器呈現您的應用程式和 SPA 以進行靜態裝載。
    - 選擇 [開發工具： **jsconfig** ] （建議 VS Code，讓 Intellisense 程式碼完成運作）

5. 建立專案之後，`cd my-nuxtjs-app` 輸入您的 Nuxt 專案目錄，然後輸入 `code .` 以在 VS Code WSL-遠端環境中開啟專案。

    ![WSL-遠端擴充功能](../images/wsl-remote-extension.png)

6. 安裝 Nuxt 之後，您必須知道3個命令：

    - `npm run dev` 用於執行具有熱重載的開發實例、檔案監看式和工作重新執行。
    - `npm run build`，用來編譯您的專案。
    - `npm start`，用於以生產模式啟動應用程式。

    開啟 VS Code 中整合的 WSL 終端機（**View > terminal**）。 請確定終端機路徑指向您的專案目錄（即 `~/NuxtProjects/my-nuxt-app$`）。 然後，使用下列程式嘗試執行新 Nuxt 應用程式的開發實例： `npm run dev`

6. 本機程式開發伺服器將會啟動（針對用戶端和伺服器所編譯的部分，顯示一些酷炫的進度列）。 當您的專案完成建立之後，您的終端機會顯示「編譯成功」，以及編譯所需的時間。 將網頁瀏覽器指向[http://localhost:3000](http://localhost:3000)以開啟新的 Nuxt 應用程式。

    ![您在 localhost：3000中執行的 Nuxt 應用程式](../images/nuxt-app.png)

7. 在您的 VS Code 編輯器中開啟 `pages/index.vue` 檔案。 找出頁面標題 `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>`，並將其變更為 `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`。 在您的網頁瀏覽器仍然開啟至 localhost：3000時，儲存您的變更，並注意熱重載功能會自動在瀏覽器中編譯及更新您的變更。

8. 讓我們看看 Nuxt 如何處理錯誤。 移除 `</v-card-title>` 結束記號，讓您的標題程式碼現在看起來像這樣： `<v-card-title class="headline">This is my new Nuxt.js app!`。 儲存這項變更，並請注意，編譯錯誤會顯示在您的瀏覽器中，而在您的終端機中，讓您知道 `<v-card-title>` 的結束記號遺失，以及可在程式碼中找到錯誤的行號。 取代 `</v-card-title>` 結束記號、儲存，且頁面將會重載。

您可以藉由選取 F5 鍵，或移至 **> Debug** （Ctrl + Shift + D）並在功能表列中查看 **> 偵錯主控台**（Ctrl + shift + Y）來使用 VS Code 的偵錯工具與您的 Nuxt。 如果您在 [偵錯工具] 視窗中選取齒輪圖示，將會為您建立啟動設定（`launch.json`）檔案，以儲存偵錯工具的安裝詳細資料。 若要深入瞭解，請參閱[VS Code 的調試](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations)。

![VS Code 的 [debug] 視窗和 [啟動]。 json 設定圖示](../images/vscode-debug-launch-configuration.png)

若要深入瞭解 Nuxt，請參閱[Nuxt 指南](https://nuxtjs.org/guide)。

## <a name="get-started-with-gatsbyjs"></a>開始使用 Gatsby

Gatsby 是以 Nuxt 為基礎的靜態網站產生器架構，而不是像下一個 .js 和的伺服器呈現。 靜態網站產生器會在組建時間產生靜態 HTML。 它不需要伺服器。 接下來的 .js 和 Nuxt 會在執行時間產生 HTML （每次新的要求都在中）。 他們需要執行伺服器。 Gatsby 也會決定如何處理您應用程式中的資料（使用 GraphQL），而下一個 .js 和 Nuxt 則留給您決定。

若要建立 Gatsby 專案：

1. 開啟您的 WSL 終端機（ie）。Ubuntu 18.04）。
2. 建立新的專案資料夾： `mkdir GatsbyProjects` 並輸入該目錄： `cd GatsbyProjects`
3. 使用 npm 來安裝 Gatsby CLI： `npm install -g gatsby-cli`。 安裝之後，請使用 `gatsby --version` 來檢查版本。
4. 建立 Gatsby 專案： `gatsby new my-gatsby-app`
5. 安裝套件之後，請將目錄變更為新的應用程式資料夾，`cd my-gatsby-app`，然後使用 `code .`，在 VS Code 中開啟您的 Gatsby 專案。 這可讓您查看已使用 VS Code 的檔案瀏覽器為應用程式建立的 Gatsby 架構。 請注意，VS Code 在 WSL 遠端環境中開啟您的應用程式（如 VS Code 視窗左下方的綠色索引標籤所示）。 這表示當您使用 VS Code 在 Windows OS 上進行編輯時，它仍會在 Linux OS 上執行您的應用程式。

    ![WSL-遠端擴充功能](../images/wsl-remote-extension.png)

6. 安裝 Gatsby 之後，您必須知道3個命令：

    - `gatsby develop`，用來執行具有熱重載的開發實例。
    - `gatsby build`，用於建立生產組建。
    - `gatsby serve`，用於以生產模式啟動應用程式。

    開啟 VS Code 中整合的 WSL 終端機（**View > terminal**）。 請確定終端機路徑指向您的專案目錄（即 `~/GatsbyProjects/my-gatsby-app$`）。 然後嘗試使用下列程式執行新應用程式的開發實例： `gatsby develop`

7. 當您的新 Gatsby 專案完成編譯之後，您的終端機會會顯示「您現在可以在瀏覽器中看到 Gatsby-starter-default。 [http://localhost:8000/](http://localhost:8000/)」。 選取此 localhost 連結，即可查看在網頁瀏覽器中建立的新專案。

> [!NOTE]
> 您會注意到您的終端機輸出也會讓您知道，您可以「View GraphiQL，這是一個瀏覽器內的 IDE，以探索您網站的資料和架構： [http://localhost:8000/___graphql](http://localhost:8000/___graphql)」。 GraphQL 會將您的 Api 合併為內建于 Gatsby 的自我記錄 IDE （GraphiQL）。 除了探索您網站的資料和架構之外，您還可以執行查詢、變化和訂閱等 GraphQL 作業。 如需詳細資訊，請參閱[GraphiQL 簡介](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/)。

8. 在您的 VS Code 編輯器中開啟 `src/pages/index.js` 檔案。 找出頁面標題 `<h1 >Hi people</h1>`，並將其變更為 `<h1 >Hi (Your Name)!</h1>`。 在您的網頁瀏覽器仍然開啟至 localhost：8000時，儲存您的變更，並注意熱重載功能會自動在瀏覽器中編譯及更新您的變更。

    ![您在 localhost：3000中執行的 Gatsby 應用程式](../images/gatsby-app.png)

9. 我們來看一下，接下來的 .js 如何處理錯誤。 移除 `</h1>` 結束記號，讓您的標題程式碼現在看起來像這樣： `<h1>Hi (Your Name)!`。 儲存這項變更，請注意，您的瀏覽器中會顯示「無法編譯」錯誤，而在您的終端機中，讓您知道應該會有 `<h1>` 的結束記號。 取代 `</h1>` 結束記號、儲存，且頁面將會重載。

您可以使用 VS Code 的偵錯工具搭配您的下一個 .js 應用程式，方法是選取 F5 鍵，或移至 **> Debug** （Ctrl + Shift + D），並在功能表列中查看 **> 偵錯主控台**（Ctrl + shift + Y）。 如果您在 [偵錯工具] 視窗中選取齒輪圖示，將會為您建立啟動設定（`launch.json`）檔案，以儲存偵錯工具的安裝詳細資料。 若要深入瞭解，請參閱[VS Code 的調試](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations)。

![VS Code 的 [debug] 視窗和 [啟動]。 json 設定圖示](../images/vscode-debug-launch-configuration.png)

若要深入瞭解 Gatsby，請參閱[Gatsby](https://www.gatsbyjs.org/docs/)檔。
