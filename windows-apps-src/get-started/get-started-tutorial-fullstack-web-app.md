---
author: libbymc
title: 使用 REST API 後端建立單頁 Web 應用程式
description: 使用受歡迎的 Web 技術建置適用於 Microsoft Store 的託管 Web 應用程式
keywords: hosted web app, HWA, REST API, single-page app, SPA, 託管的 Web 應用程式, 單頁應用程式
ms.author: libbymc
ms.date: 05/10/2017
ms.topic: article
ms.prod: Microsoft Edge, Azure, Visual Studio Code
ms.technology: web
ms.localizationpriority: medium
ms.openlocfilehash: 42f11cbdd749a44c4ba0d8bc1a0397a4f2882257
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2018
ms.locfileid: "3377529"
---
# <a name="create-a-single-page-web-app-with-rest-api-backend"></a>使用 REST API 後端建立單頁 Web 應用程式

**使用受歡迎的全端 Web 技術建置適用於 Microsoft Store 的託管 Web 應用程式**

![做為單頁 Web 應用程式的簡單記憶遊戲](images/fullstack.png)

這個兩部分教學課程提供快速導覽現代化的全端 Web 開發，讓您建置可在瀏覽器中運作以及做為 Microsoft Store 適用的託管 Web 應用程式的簡單記憶遊戲。 在部分 I 中，您將建置遊戲後端的簡易 REST API 服務。 藉由在做為 API 服務的雲端中裝載遊戲邏輯，您可保留遊戲狀態，讓您的使用者能夠跨不同裝置接續玩相同的遊戲執行個體。 在部分 II 中，您將建置前端 UI 為回應式配置的單頁 Web 應用程式。

我們會使用一些最受歡迎的 Web 技術，包括 [Node.js](https://nodejs.org/en/) 執行階段和用於伺服器端開發的 [Express](http://expressjs.com/)、[Bootstrap](http://getbootstrap.com/) UI 架構、[Pug](https://www.npmjs.com/package/pug) 範本引擎和用於建置 RESTful API 的 [Swagger](http://swagger.io/tools/)。 您也可以體驗 [Azure 入口網站](https://ms.portal.azure.com/)的雲端裝載以及使用 [Visual Studio Code](https://code.visualstudio.com/) 編輯器。

## <a name="prerequisites"></a>必要條件

如果您的電腦上尚未有這些資源，請遵循這些下載連結︰

 - [Node.js](https://nodejs.org/en/download/) - 務必要選取選項以新增 Node 到您的 PATH。

 - [Express 產生器](http://expressjs.com/en/starter/generator.html)- 在您安裝 Node 後，透過執行下列來安裝 Express： `npm install express-generator -g`

 - [Visual Studio Code](https://code.visualstudio.com/)

如果您想要完成在 Microsoft Azure 上裝載 API 服務和記憶遊戲應用程式的最後步驟，您需要[建立免費的 Azure 帳戶](https://azure.microsoft.com/en-us/free/) (若尚未這麼做的話)。

如果您決定要放棄 (或延後) Azure 部分，只要略過部分 I 及 II 的最後區段，其中涵蓋 Azure 裝載和封裝 Microsoft Store 適用的應用程式。 您建置的 API 服務與 Web 應用程式仍會在您的電腦上本機執行 (分別從 `http://localhost:8000` 到 `http://localhost:3000`)。

## <a name="part-i-build-a-rest-api-backend"></a>部分 I︰建置 REST API 後端

我們將第一次建置簡單的記憶遊戲 API 以啟動我們的記憶遊戲 Web 應用程式。 我們將使用 [Swagger](http://swagger.io/) 來定義我們 API，並產生 Scaffolding 程式碼和 Web UI 以進行手動測試。

如果您想要略過此部分並直接移至[部分 II︰建置單頁 Web 應用程式](#part-ii-build-a-single-page-web-appl)，以下是[部分 I 已完成的程式碼](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend)。請依照 *README* 指示安裝程式碼並在本機執行，或查看 *5. 在 Azure 上裝載 API 服務，並啟用 CORS* 從 Azure 執行它。

### <a name="game-overview"></a>遊戲概觀

*記憶*(也稱為[*集中注意力*](https://en.wikipedia.org/wiki/Concentration_(game))和[*配爾曼記憶*](https://en.wikipedia.org/wiki/Pelmanism_(system))) 是簡單的遊戲，由一副卡片配對所組成。 卡片面朝下放在桌上，然後由玩家檢查卡片值，一次兩張以找出相符者。 每次翻卡後將卡片再次面朝下翻回，除非找到配對相符者，兩張卡片才能從遊戲中剔除。 遊戲目標是以最少的回合數找出所有卡片配對。

基於說明目的，我們將會使用非常簡單的遊戲結構：單一遊戲，單一玩家。 不過，遊戲邏輯是在伺服器端 (而非用戶端) 保留遊戲狀態，以便您可以在不同的裝置上繼續玩相同牌局。

記憶遊戲的資料結構只包含一個陣列的 JavaScript 物件，每個代表單一卡片，陣列中的索引做為卡片的 ID。 在伺服器上，每張卡片物件都有一個值與 **cleared** 旗標。 例如，2 個相符項目 (4 張卡片) 的遊戲板可能會隨機產生並像這樣序列化。

```json
[
    { "cleared":"false",
        "value":"0",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"0",
    }
]
```

當遊戲板陣列傳遞到用戶端時，值機碼會從陣列中移除，以避免作弊 (例如，使用 F12 瀏覽器工具檢查 HTTP 回應本文)。 以下是相同的新遊戲如何仰賴呼叫 **GET /game** REST 端點的用戶端：

```json
[{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"}]
```

談到端點，記憶遊戲服務將包含三個 REST API。

#### <a name="post-new"></a>POST /new
初始化指定大小 (相符項目數) 的新遊戲板。

| 參數 | 說明 |
|-----------|-------------|
| int *size* |要洗牌到遊戲板的配對數。 範例： `http://memorygameapisample/new?size=2`|

| 回應 | 說明 |
|----------|-------------|
| 200 OK | 所要求大小的新記憶遊戲已經就緒。|
| 400 BAD REQUEST| 所要求的大小超出可接受的範圍。|


#### <a name="get-game"></a>GET /game
擷取記憶遊戲板的目前狀態。

*無參數*

| 回應 | 說明 |
|----------|-------------|
| 200 OK | 傳回 JSON 陣列的卡片物件。 每張卡都有 **cleared** 屬性，指出是否已找到其配對。 相符的卡片也會回報它們的 **value**。 範例： `[{"cleared":"false"},{"cleared":"false"},{"cleared":"true","value":1},{"cleared":"true","value":1}]`|

#### <a name="put-guess"></a>PUT /guess
指定要顯示的卡片，並檢查先前已經翻過的相符卡片。

| 參數 | 說明 |
|-----------|-------------|
| int *card* | 要顯示之卡片的卡片 ID (遊戲板陣列中的索引)。 每個完成的「猜測」包含兩個指定的卡片 (亦即，使用有效且唯一的 *card* 值兩次呼叫 **/guess**)。 範例： `http://memorygameapisample/guess?card=0`|

| 回應 | 說明 |
|----------|-------------|
| 200 OK | 傳回指定之卡片含有 **id** 和 **value** 的 JSON。 範例： `[{"id":0,"value":1}]`|
| 400 BAD REQUEST |  指定的卡片發生錯誤。 詳細資訊請查看 HTTP 回應本文。|

### <a name="1-spec-out-the-api-and-generate-code-stubs"></a>1. 指出 API 並產生程式碼端

我們將使用 [Swagger](http://swagger.io/) 將我們的記憶遊戲 API 設計轉換為有用的 Node.js 伺服器程式碼。 以下是您可以定義我們的[記憶遊戲 API 為 Swagger 中繼資料](https://github.com/Microsoft/Windows-tutorials-web/blob/master/Single-Page-App-with-REST-API/backend/api.json)的方式。 我們會使用這個來產生伺服器程式碼端。

1. 建立新資料夾 (例如您的本機 *GitHub* 目錄)，然後下載包含我們的記憶遊戲 API 定義的 [**api.json**](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/api.json?token=ACEfklXAHTeLkHYaI5plV20QCGuqC31cks5ZFhVIwA%3D%3D) 檔。 請確定您的資料夾名稱未包含任何空格。

2. 開啟您最愛的殼層 ([或使用 Visual Studio Code 的整合式終端機！](https://code.visualstudio.com/docs/editor/integrated-terminal)) 到該資料夾，然後為您的全域 (**-g**) Node 環境執行下列 Node Package Manager (NPM) 命令，以安裝 [Yeoman](http://yeoman.io/) (yo) 程式碼 Scaffolding 工具和 Swagger 產生器︰

    ```
    npm install -g yo
    npm install -g generator-swaggerize
    ```

3. 現在，我們可以使用 Swagger 產生伺服器 Scaffolding 程式碼︰

    ```
    yo swaggerize
    ```

4. **swaggerize** 命令會詢問您幾個問題。
    - Swagger 文件的路徑 (或 URL)︰**api.json**
    - 架構：**express**
    - 您想要如何稱呼這個專案 (YourFolderNameHere)：**[enter]**

    依您的喜好回答一切；資訊是大部分是提供 *package.json* 檔與您的連絡人資訊，以便您可以發佈您的程式碼為 NPM 套件。

5. 最後，為您的新專案和 [Swagger UI](http://swagger.io/swagger-ui/) 支援安裝所有相依性 (在 *package.json* 中列出)。

    ```
    npm install
    npm install swaggerize-ui
    ```

    立即啟動 VS Code 與 **\[檔案\]** > **\[開啟資料夾...\]**，並移至 MemoryGameAPI 目錄。 這是您剛建立的 Node.js API 伺服器！ 它使用受歡迎的 [ExpressJS](http://expressjs.com/en/4x/api.html) Web 應用程式架構來構建和執行您的專案。

### <a name="2-customize-the-server-code-and-setup-debugging"></a>2. 自訂伺服器程式碼與設定偵錯

您的專案根目錄中的 *server.js* 檔是做為您伺服器的「主要」功能。 在 VS Code 中開啟它並複製下列到其中。 修改自所產生程式碼的程式行會註解進一步的解釋。

```javascript
'use strict';

var port = process.env.PORT || 8000; // Better flexibility than hardcoding the port

var Http = require('http');
var Express = require('express');
var BodyParser = require('body-parser');
var Swaggerize = require('swaggerize-express');
var SwaggerUi = require('swaggerize-ui'); // Provides UI for testing our API
var Path = require('path');

var App = Express();
var Server = Http.createServer(App);

App.use(function(req, res, next) {  // Enable cross origin resource sharing (for app frontend)
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', 'GET,PUT,POST,OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization, Content-Length, X-Requested-With');

    // Prevents CORS preflight request (for PUT game_guess) from redirecting
    if ('OPTIONS' == req.method) {
      res.send(200);
    }
    else {
      next(); // Passes control to next (Swagger) handler
    }
});

App.use(BodyParser.json());
App.use(BodyParser.urlencoded({
    extended: true
}));

App.use(Swaggerize({
    api: Path.resolve('./config/swagger.json'),
    handlers: Path.resolve('./handlers'),
    docspath: '/swagger'   //  Hooks up the testing UI
}));

App.use('/', SwaggerUi({    // Serves the testing UI from our base URL
  docs: '/swagger'          //
}));

Server.listen(port, function () {  // Starts server with our modfied port settings
 });
```

是時候執行您的伺服器了！ 讓我們設定 Visual Studio Code 以進行 Node 偵錯。 選取 **\[偵錯\]** 面板圖示 (Ctrl+Shift+D)，然後選取齒輪圖示 (開啟 launch.json)，再修改「設定」為此：

```json
"configurations": [
    {
        "type": "node",
        "request": "launch",
        "name": "Launch Program",
        "program": "${workspaceRoot}/server.js"
    }
]
```

現在按下 F5 並將您的瀏覽器開啟至 [http://localhost:8000](http://localhost:8000)。 頁面應會為我們的記憶遊戲 API 開啟至 Swagger UI，而您可以從該處為每個方法展開詳細資料和輸入欄位。 您甚至可以嘗試呼叫 API，雖然其回應將只包含模擬的資料 (由 [Swagmock](https://www.npmjs.com/package/swagmock) 模組提供)。 是時候加入我們的遊戲邏輯，讓這些 API 成真。

### <a name="3-set-up-your-route-handlers"></a>3. 設定您的路由處理常式

Swagger 檔案 (config\swagger.json) 會指示我們的伺服器如何處理各種用戶端 HTTP 要求，方式是對應它所定義的每個 URL 路徑至處理常式檔案 (在 \handlers 中)，以及針對該處理常式檔案中的 `operationId` (函式) 的該路徑定義的每一種方法 (例如 **GET**、**POST**)。

在我們程式的這一層中，我們在傳送各種用戶端要求至我們的資料模型之前，將新增一些輸入檢查。 下載 (或是複製並貼上)︰

 - 此 [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/game.js?token=ACEfkvhw6BUnkeSsZgnzVe086T5WLwjfks5ZFhW5wA%3D%3D) 程式碼到您的 **handlers\game.js** 檔案
 - 此 [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/guess.js?token=ACEfkswel02rHVr0e61bVsNdpv_i1Rtuks5ZFhXPwA%3D%3D) 程式碼到您的 **handlers\guess.js** 檔案
 - 此 [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/new.js?token=ACEfkgk2QXJeRc0aaLzY5ulFsAR4Juidks5ZFhXawA%3D%3D) 程式碼到您的 **handlers\new.js** 檔案

 您可以瀏覽這些檔案中的註解，以取得有關變更的更多詳細資料，但基本上它們檢查基本輸入錯誤 (例如，用戶端要求小於一個符合項目的新遊戲)，並視需要傳送描述性錯誤訊息。 處理常式也路由有效的用戶端要求至其對應的資料檔 (在 \data 中) 以進一步處理。 讓我們接下來進行下一步。

### <a name="4-set-up-your-data-model"></a>4. 設定您的資料模型

是時候使用我們的記憶遊戲板的真實資料模型，換出預留位置的資料模擬服務。

我們程式的這一層代表記憶卡本身，並提供大量的遊戲邏輯，包括新遊戲的「洗牌」、找出相符的卡片配對以及追蹤遊戲狀態。 複製並貼上：

 - 此 [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/game.js?token=ACEfksAceJNQmhF82aHjQTx78jILYKfCks5ZFhX4wA%3D%3D) 程式碼到您的 **data\game.js** 檔案
 - 此 [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/guess.js?token=ACEfkvY69Zr1AZQ4iXgfCgDxeinT21bBks5ZFhYBwA%3D%3D) 程式碼到您的 **data\guess.js** 檔案
 - 此 [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/new.js?token=ACEfkiqeDN0HjZ4-gIKRh3wfVZPSlEmgks5ZFhYPwA%3D%3D) 程式碼到您的 **data\new.js** 檔案

為清楚明瞭，我們將我們的遊戲板儲存在我們的 Node 伺服器上的全域變數 (`global.board`) 中。 但實際上，您會使用雲端儲存空間 (像是 Google [Cloud Datastore](https://cloud.google.com/datastore/) 或 Azure [DocumentDB](https://azure.microsoft.com/en-us/services/documentdb/))，讓此加入同時支援多個遊樂和玩家的可用記憶遊戲 API 服務。

請確定您已儲存 VS Code 中的所有變更，再次啟動您的伺服器 (VS Code 中的 F5 或殼層的 `npm start`，然後瀏覽至 [http://localhost:8000](http://localhost:8000)) 來測試遊戲 API。

每次您按下 **\[試試看！\]**  按鈕 (**/game**、**/guess** 或 **/new** 作業之一)，檢查以下產生的**回應本文**和**回應碼**以確認一切是否如預期般運作。

嘗試： 

1. 建立新的 `size=2` 遊戲。

    ![從 Swagger UI 開始新的記憶遊戲](images/swagger_new.png)

2. 猜測幾個值。

    ![從 Swagger UI 猜測卡片](images/swagger_guess.png)

3. 依照遊戲進度檢查遊戲板。

    ![從 Swagger UI 檢查遊戲狀態](images/swagger_game.png)

如果一切看起來良好，您的 API 服務已準備好裝載到 Azure 上了！ 如果您遇到問題，請嘗試在 \data\game.js 中註解下列程式行。

```javascript
for (var i=0; i < board.length; i++){
    var card = {};
    card.cleared = board[i].cleared;

    // Only reveal cleared card values
    //if ("true" == card.cleared) {        // To debug the board, comment out this line
        card.value = board[i].value;
    //}                                    // And this line
    currentBoardState.push(card);
}
```

藉由此項變更，**GET /game** 方法會傳回所有所有卡片值 (包括尚未清除的值)。 這是您在建置記憶遊戲前端時，可保留的實用偵錯駭客。

### <a name="5-optional-host-your-api-service-on-azure-and-enable-cors"></a>5. (選擇性) 在 Azure 上裝載 API 服務並啟用 CORS

Azure 文件將逐步引導您完成：

 - [向 Azure 入口網站註冊新的 *API App*](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#createapiapp)
 - [設定適用於 API 應用程式的 Git 部署](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git)，以及
 - [部署 API 應用程式的程式碼到 Azure](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git)

註冊您的 App 時，請嘗試區分您的 *App 名稱* (避免命名與 *http://memorygameapi.azurewebsites.net* URL 上的其他請求變異衝突)。

如果您已經進行到此並且 Azure 現在已供您的 Swagger UI 使用，最後一個步驟則是記憶遊戲後端部分。 從 [Azure 入口網站](https://portal.azure.com)，選取您剛建立的 *App Service*，然後選取或搜尋**CORS** (跨原始來源資源共用) 選項。 在 **\[允許的來源\]** 下，新增星號 (`*`)，然後按一下 **\[儲存\]**。 這可讓您在本機電腦上開發時從您的記憶遊戲前端，跨原始來源呼叫您的 API 服務。 一旦您完成記憶遊戲前端並將其部署到 Azure，您就可以Web 應用程式的特定 URL 取代此項目。

### <a name="going-further"></a>更進一步

為了讓記憶遊戲 API 成為生產應用程式可用的後端服務，您會想要延伸程式碼為支援多個玩家和遊戲。 因此，您可能需要為您的 API 探查[驗證](http://swagger.io/docs/specification/authentication/) (用於管理玩家身分識別)、[NoSQL 資料庫](https://docs.microsoft.com/en-us/azure/documentdb/) (用於追蹤遊戲和玩家)，以及一些基本[單元測試](https://apigee.com/about/blog/developer/swagger-test-templates-test-your-apis)。

以下是一些可讓您更進一步的實用資源︰

 - [使用 Visual Studio Code 的進階 Node.js 偵錯](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

 - [Azure Web + 行動裝置版文件](https://docs.microsoft.com/en-us/azure/#pivot=services&panel=web)

 - [Azure DocumentDB 文件](https://docs.microsoft.com/en-us/azure/documentdb/index)

## <a name="part-ii-build-a-single-page-web-application"></a>部分 II︰ 建置單頁 web 應用程式

既然您已經從部分 I 建置 (或[下載](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend)) [REST API 後端](#part-i-build-a-rest-api-backend)，您就可以開始使用 [Node](https://nodejs.org/en/)、[Express](http://expressjs.com/) 和 [Bootstrap](http://getbootstrap.com/) 建立單頁記憶遊戲前端。

本教學課程的部分 II 可讓您的體驗︰ 

* [Node.js](https://nodejs.org/en/)︰建立託管您的遊戲的伺服器
* [jQuery](http://jquery.com/)：JavaScript 程式庫
* [Express](http://expressjs.com/)︰用於應用程式 Web 架構
* [Pug](https://pugjs.org/)：(之前為 Jade) 用於範本化引擎
* [Bootstrap](http://getbootstrap.com/)︰用於回應式配置
* [Visual Studio Code](https://code.visualstudio.com/)︰用於程式碼編寫、Markdown 檢視及偵錯

### <a name="1-create-a-nodejs-application-by-using-express"></a>1. 使用 Express 建立 Node.js 應用程式

讓我們開始使用 Express 建立 Node.js 專案。

1. 開啟命令提示字元，然後瀏覽至您想要用來儲存遊戲的目錄。 
2. 使用 Express 產生器建立使用範本化引擎 *Pug* 稱為 *memory* 的新應用程式。

    ```
    express --view=pug memory
    ```

3. 在記憶目錄中，安裝 package.json 檔案中所列的相依性。 package.json 檔案是建立在您專案的根目錄中。 此檔案包含您的 Node.js 應用程式所需的模組。  

    ```
    cd memory
    npm install
    ```

    執行此命令之後，您應該會看到一個稱為 node_modules 的資料夾，其中包含此應用程式所需的所有模組。 

4. 現在，執行您的應用程式。

    ```
    npm start
    ```

5. 移至 [http://localhost:3000/](http://localhost:3000/) 檢視您的應用程式。

    ![http://localhost:3000/ 的螢幕擷取畫面](./images/express.png)

6. 編輯 memory\routes 目錄中的 index.js 來變更您的 Web 應用程式的預設標題。 變更以下程式行中的 `Express` 為 `Memory Game` (或您選擇的另一個標題)。

    ``` javascript
    res.render('index', { title: 'Express' });
    ```

7. 若要重新整理應用程式以查看您的新標題，請按下 **Crtl + C**、命令提示字元中的 **Y**來停止您的應用程式，再使用 `npm start` 重新開機。

### <a name="2-add-client-side-game-logic-code"></a>2. 新增用戶端遊戲邏輯程式碼
您可以在[開始](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start)資料夾中尋找這一半教學課程所需的檔案。 如果您遺失了，完成的程式碼就在[最終](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Final)資料夾中。 

1. 複製[開始](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start)資料夾內的 scripts.js 檔並貼到 memory\public\javascripts 中。 此檔案包含執行遊戲所需的所有遊戲邏輯，包括︰

    * 建立/開始新的遊戲。
    * 還原儲存在伺服器上的遊戲。
    * 根據使用者的選取繪製遊戲板和卡片。
    * 翻轉卡片。

2. 在 script.js 中，讓我們從修改 `newGame()` 函式開始。 此函式︰

    * 處理使用者選取的遊戲大小。
    * 從伺服器擷取[遊戲板陣列](#part-i-build-a-rest-api-backend)。
    * 呼叫 `drawGameBoard()` 將遊戲板放到畫面上。

    在 `// Add code from Part 2.2 here` 註解之後新增 `newGame()` 內的以下程式行。

    ``` javascript
    // extract game size selection from user
    var size = $("#selectGameSize").val();

    // parse game size value
    size = parseInt(size, 10);
    ```

    此程式碼會使用 `id="selectGameSize"` (我們稍後將會建立) 從下拉式功能表擷取值，並將它儲存在變數 (`size`) 中。  我們使用 [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) 函式來剖析來自下拉清單的字串值以傳回整數，這樣我們就可以所要求遊戲牌局的 `size` 傳遞到伺服器。 

    我們使用本教學課程部分 I 中建立的 [`/new`](#part-i-build-a-rest-api-backend) 方法張貼遊戲板的大小到伺服器。 方法傳回單一 JSON 陣列的卡片與指出卡片是否相符的 `true/false` 值。 

3. 接著，填入還原上次玩過的遊戲的 `restoreGame()` 函式。 為清楚明瞭，應用程式一律會載入上次玩過的遊戲。 如果伺服器上沒有儲存遊戲，請使用下拉式功能表開始新的遊戲。 

    將此程式碼複製並貼到 `restoreGame()`。

   ``` javascript 
   // reset the game
   gameBoardSize = 0;
   cardsFlipped = 0;

   // fetch the game state from the server 
   $.get("http://localhost:8000/game", function (response) {
       // store game board size
       gameBoardSize = response.length;

       // draw the game board
       drawGameBoard(response);
   });
   ```

    遊戲現在將會從伺服器擷取遊戲狀態。 如需有關此步驟中使用的 [`/game`](#part-i-build-a-rest-api-backend) 方法的詳細資訊，請參閱本教學課程部分 I。 如果您使用 Azure (或其他服務) 裝載後端 API，請以您的生產 URL 取代上述 *localhost* 位址。

4. 現在，我們想要建立 `drawGameBoard()` 函式。  此函式︰

    * 偵測遊戲的大小，並套用特定 CSS 樣式。
    * 以 HTML 產生卡片。
    * 新增卡片到頁面。

    將此程式碼複製並貼到 *scripts.js* 中的 `drawGameBoard()` 函式：

    ``` javascript
    // create output
    var output = "";
    // detect board size CSS class
    var css = "";
    switch (board.length / 4) {
        case 1:
            css = "rows1";
            break;
        case 2:
            css = "rows2";
            break;
        case 3:
            css = "rows3";
            break;
        case 4:
            css = "rows4";
            break;   
    }
    // generate HTML for each card and append to the output
    for (var i = 0; i < board.length; i++) {
        if (board[i].cleared == "true") {
            // if the card has been cleared apply the .flip class
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards flip matched\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\">" + lookUpGlyphicon(board[i].value) + "</div>\
                </div></div>";
        } else {
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\"></div>\
                </div></div>";
        }
    }
    // place the output on the page
    $("#game-board").html(output);
    ```

5. 接下來，我們需要完成 `flipCard()` 函式。  這個函式處理大部分的遊戲邏輯，包括使用教學課程部分 I 中所開發的 [`/guess`](#part-i-build-a-rest-api-backend) 方法，從伺服器取得所選卡片的值。 如果您是雲端裝載 REST API 後端，別忘了以您的生產 URL 取代 *localhost* 位址。

    在 `flipCard()` 函式中，取消註解此程式碼：

    ``` javascript
    // post this guess to the server and get this card's value
    $.ajax({
        url: "http://localhost:8000/guess?card=" + selectedCards[0],
        type: 'PUT',
        success: function (response) {
            // display first card value
            $("#" + selectedCards[0] + " .back").html(lookUpGlyphicon(response[0].value));

            // store the first card value
            selectedCardsValues.push(response[0].value);
        }
    });
    ```

> [!TIP] 
> 如果您使用 Visual Studio Code，請選取您想要取消註解的所有程式碼行，並按下 Crtl + K、U

在此我們使用部分 I 建立的 [`jQuery.ajax()`](http://api.jquery.com/jQuery.ajax/) 和 **PUT** [`/guess`](#part-i-build-a-rest-api-backend) 方法。 

此程式碼以下列順序執行。

* 使用者選取的第一個卡片的 `id` 新增至 selectedCards[] 陣列做為第一個值︰ `selectedCards[0]` 
* 使用 [`/guess`](#part-i-build-a-rest-api-backend) 方法將 `selectedCards[0]` 中的值 (`id`) 張貼到伺服器
* 伺服器會以該卡的 `value` (整數) 回應
* [Bootstrap glyphicon](http://getbootstrap.com/components/) 會新增到卡片的背面，而卡片的 `id` 是 `selectedCards[0]`
* 第一個卡片的 `value` (從伺服器) 會儲存在 `selectedCardsValues[]` 陣列中︰`selectedCardsValues[0]`。 

使用者的第二個猜測會遵照相同的邏輯。 如果使用者選取的卡片有相同的識別碼 (例如，`selectedCards[0] == selectedCards[1]`)，則卡片相符！ CSS 類別`.matched` 會新增至相符的卡片 (將其轉為綠色)，而卡片會維持翻轉狀態。

現在，我們需要新增邏輯以檢查使用者是否符合所有卡片並贏得遊戲。 在 `flipCard()` 函式的內部，新增下列程式碼行到 `//check if the user won the game` 註解下。 

``` javascript
if (cardsFlipped == gameBoardSize) {
    setTimeout(function () {
        output = "<div id=\"playAgain\"><p>You Win!</p><input type=\"button\" onClick=\"location.reload()\" value=\"Play Again\" class=\"btn\" /></div>";
        $("#game-board").html(output);
    }, 1000);
}   
```

如果翻轉的卡片數符合遊戲板的大小 (例如，`cardsFlipped == gameBoardSize`)，表示已沒有要翻轉的卡片，而使用者已贏得遊戲。 我們將會新增一些簡單 HTML到 `id="game-board"` 的 `div`，讓使用者知道他們已贏得遊戲並可再次玩遊戲。  

### <a name="3-create-the-user-interface"></a>3. 建立使用者介面 
現在，我們透過建立使用者介面來看看此程式碼的所有動作。 本教學課程中，我們使用範本化引擎 [Pug](https://pugjs.org/)(正式 Jade)。  *Pug* 是撰寫 HTML 的全新、區分空白字元的語法。 這裡提供一個範例。 

```
body
    h1 Memory Game
    #container
        p We love tutorials!
```

becomes

``` html
<body>
    <h1>Memory Game</h1>
    <div id="container">
        <p>We love tutorials!</p>
    </div>
</body>
```


1. 以 [開始] 資料夾中提供的 layout.pug 檔案取代 memory\views 中的 layout.pug 檔案。 在 layout.pug 內，您會看到連至下列項目的連結︰

    * Bootstrap
    * jQuery
    * 自訂 CSS 檔案
    * 我們剛剛完成修改的 JavaScript 檔案

2. 開啟目錄 memory\views 中名為 index.pug 的檔案。
此檔案延伸 layout.pug 檔案，並將轉譯我們的遊戲。 在 layout.pug 內，貼上下列程式碼行︰

    ```
    extends layout
    block content  
        div
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board
                script restoreGame();
    ```

> [!TIP] 
> 記住︰Pug 會區分空白字元。 請確定您的所有縮排都正確！

### <a name="4-use-bootstraps-grid-system-to-create-a-responsive-layout"></a>4. 使用 Bootstrap 的格線系統來建立回應式配置
Bootstrap 的[方格系統](http://getbootstrap.com/css/#grid)是可變式方格系統，可縮放方格為裝置的檢視區變更。 此遊戲中的卡片使用 Bootstrap 預先定義的方格系統類別配置，包括︰
* `.container-fluid`︰指定方格的可變式容器
* `.row-fluid`︰指定可變式列
* `.col-xs-3`︰指定欄數

Bootstrap 的方格系統允許方格系統摺疊到一個垂直欄，就像您在行動裝置上看到的導覽功能表。  不過，因為我們希望我們的遊戲都要有欄位，所以我們使用預先定義的類別 `.col-xs-3`，這會一直讓方格保留水平。 

方格系統最多允許 12 欄。 既然我們希望我們的遊戲只有 4 欄，我們會使用類別 `.col-xs-3`。 此類別指定我們的每個欄必須橫跨先前所提及 12 個可用欄中其中 3 欄的寬度。 下圖顯示 12 欄方格和 4 欄方格，就像此遊戲中所使用的。

![12 欄與 4 欄的 Bootstrap 方格](./images/grid.png)

1. 開啟 scripts.js，並尋找 `drawGameBoard()` 函式。  在我們為每張卡片產生 HTML 的程式碼區塊中，您可以找出具有 `class="col-xs-3"` 的 `div` 元素嗎？ 

2. 在 index.pug 內，我們新增先前所提及預先定義的 Bootstrap 類別，來建立我們流動配置。  變更 index.pug 為下列。

    ```
    extends layout

    block content   

        .container-fluid
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board.row-fluid 
                script restoreGame();
    ```

### <a name="5-add-a-card-flip-animation-with-css-transforms"></a>5. 使用 CSS 轉換新增卡片翻轉動畫
使用 [開始] 資料夾的 style.css 檔取代 memory\public\stylesheets 中的 style.css 檔。

使用 [CSS 轉換](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide/css/transforms)新增翻轉動作可提供卡片逼真的 3D 翻轉動作。 遊戲中的卡片是使用下列 HTML 結構建立並以程式設計方式新增到遊戲板 (在之前顯示的`drawGameBoard()` 函式中)。

``` html
<div class="flipContainer">
    <div class="cards">
        <div class="front"></div>
        <div class="back"></div>
    </div>
</div>
```

1. 若要開始，請提供[透視效果](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)給動畫的父容器 (`.flipContainer`)。  這可提供其子項目的深度錯覺效果︰值越大，離使用者越遠的項目就會出現。 讓我們在 style.css 中新增下列透視效果至 `.flipContainer` 類別。

    ``` css
    perspective: 1000px; 
    ```

2. 立即在 style.css 中新增下列屬性至 `.cards` 類別。 `.cards``div` 是實際做翻轉動畫的元素，會顯示卡片的正面或背面。 

    ``` css
    transform-style: preserve-3d;
    transition-duration: 1s;
    ```

    [`transform-style`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-style) 屬性建立 3D 呈現內容，而 `.cards` 類別 (`.front`和`.back`) 的子項是 3D 空間的成員。 新增 [`transition-duration`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-duration) 屬性會指定動畫結束的秒數。 

3.  使用 [`transform`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) 屬性，我們可以沿著 Y 軸旋轉卡片。  新增下列 CSS 到 `cards.flip`。

    ``` css
    transform: rotateY(180deg);
    ```

    使用 [`.toggleClass()`](http://api.jquery.com/toggleClass/) 在 `flipCard` 函式中切換開啟和關閉 `cards.flip` 中定義的樣式。 

    ``` javascript
    $(card).toggleClass("flip");
    ```

    現在當使用者按下卡片，卡片就會 180 度旋轉。

### <a name="6-test-and-play"></a>6. 測試和進行遊戲
恭喜！ 您已完成建立 Web 應用程式！ 我們來測試看看。 

1. 在您的記憶目錄中開啟命令提示字元，並輸入下列命令︰ `npm start`

2. 在您的瀏覽器中，移至 [http://localhost:3000/](http://localhost:3000/) 並玩遊戲！

3. 如果您遇到任何錯誤，您可以在鍵盤上按下 F5 鍵並輸入 `Node.js`，使用 Visual Studio Code 的 Node.js 偵錯工具。 如需在 Visual Studio Code 中偵錯的詳細資訊，請參閱此[文章](http://code.visualstudio.com/docs/editor/debugging#_launch-configurations)。 

    您也可以比較您的程式碼與最終資料夾中提供的程式碼。

4. 若要停止遊戲，請在命令提示字元中輸入︰**Ctrl + C**，**Y**。 

### <a name="going-further"></a>更進一步

您現在可以部署您的應用程式到 Azure (或任何其他雲端裝載服務)，跨不同的裝置外形規格 (例如行動裝置、平板電腦和桌上型電腦) 進行測試。 (別忘了也要跨不同的瀏覽器進行測試！) 一旦您的 App 準備好進入生產，您可以輕鬆地將其封裝為*通用 Windows 平台* (UWP) 的*主控 Web 應用程式* (HWA)，以及從 Microsoft Store 進行散發。

在 Microsoft Store 中發佈的基本步驟如下︰

 1. 建立 [Windows 開發人員](https://developer.microsoft.com/en-us/store/register)帳戶
 2. 使用 App 提交[檢查清單](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions)
 3. 提交您的 App [進行認證](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process)

以下是一些可讓您更進一步的實用資源︰

 - [將您的應用程式開發專案部署至 Azure 網站](https://docs.microsoft.com/azure/cosmos-db/documentdb-nodejs-application#_Toc395783182)

 - [將您的 Web 應用程式轉換為通用 Windows 平台 (UWP) app](https://docs.microsoft.com/en-us/windows/uwp/porting/hwa-create-windows)

 - [發佈 Windows 應用程式](https://developer.microsoft.com/en-us/store/publish-apps)
