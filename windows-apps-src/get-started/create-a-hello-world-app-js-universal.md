---
ms.assetid: CFB3601D-3459-465F-80E2-520F57B88F62
建立 "Hello, world" app (JS)
本教學課程會教您如何使用 JavaScript 和 HTML 來建立目標是 Windows 10 上的通用 Windows 平台 (UWP) 的簡單 &\#0034;Hello, world&\#0034; app。
---
# 建立 "Hello, world" app (JS)

本教學課程會教您如何使用 JavaScript 和 HTML 來建立目標是 Windows 10 上的通用 Windows 平台 (UWP) 的簡單 "Hello, world" app。 使用 Microsoft Visual Studio 中的單一專案，您可以建置可在任何 Windows 10 裝置上執行的 app。 這裡的重點是建立可在傳統型裝置和行動裝置上順利執行的 app。

**重要** 本教學課程與 Microsoft Visual Studio 2015 與 Windows 10 搭配使用。 在較早的版本中無法正常運作。

您將在此處了解如何：

-   建立新專案
-   將 HTML 內容新增到起始頁
-   處理觸控、手寫筆以及滑鼠輸入
-   在本機桌面上和 Visual Studio 的手機模擬器上執行專案。
-   建立自己的自訂樣式
-   使用適用於 JavaScript 的 Windows Library 控制項

##開始之前...


-   本教學課程直接進入建立簡單通用 app 的步驟。 因此在開始本教學課程之前，我們強烈建議您閱讀並了解 [Windows 10 的新功能](https://dev.windows.com/whats-new-windows-10-dev-preview)和[什麼是通用 Windows 應用程式](whats-a-uwp.md)的概觀資訊。
-   若要完成這個教學課程，您需要 Windows 10 與 Visual Studio 2015。 如需詳細資訊，請參閱[開始設定](get-set-up.md)。
-   我們亦假設您使用的是 Visual Studio 中預設的視窗配置。 如果您變更預設配置，您可以使用 [視窗]**** 功能表中的 [重設視窗配置]**** 命令重設它。

##步驟 1：在 Visual Studio 中建立新專案


讓我們建立名為 `HelloWorld` 的新應用程式。 方法如下：

1.  啟動 Visual Studio 2015。

    隨即顯示 Visual Studio 2015 開始畫面。

    (以下將 Visual Studio 2015 簡稱為 Visual Studio 。)

2.  在 [檔案]**** 功能表上，選取 [新增]**** > [專案]****。

    隨即顯示 [新增專案]**** 對話方塊。 對話方塊的左窗格能夠讓您選取要顯示的範本類型。

3.  在左窗格中，依序展開 [已安裝] > [範本] > [JavaScript] > [Windows]****，然後選取 [通用]**** 範本群組。 對話方塊的中央窗格會顯示 Universal Windows Platform (UWP) app 的專案範本清單。

    ![[新增專案] 視窗 ](images/js-tut-newproject.png)

    在本教學課程中，我們使用 [空白應用程式]**** 範本。 這個範本會建立能夠編譯和執行的最基本 UWP app，但不包含使用者介面控制項或資料。 在此教學課程系列中，您會將控制項和資料新增到 app。

4.  在中央窗格中，選取 [空白應用程式 (通用 Windows)]**** 範本。

    [空白應用程式]**** 範本可以建立能夠編譯、執行的最基本 UWP 應用程式，但不包含使用者介面控制項或資料。 在本教學課程中，您會將控制項新增到 app。

5.  在 [名稱]**** 文字方塊中，輸入 "HelloWorld"。
6.  按一下 [確定]**** 來建立專案。

    Visual Studio 會建立您的專案，然後在 [方案總管]**** 中顯示。

    ![HelloWorld 專案的 Visual Studio 方案總管](images/js-tut-helloworld.png)

雖然 [空白應用程式]**** 是最基本的範本，但是仍然包含少數檔案：

-   一個資訊清單檔案 (package.appxmanifest)，說明 app 的名稱、描述、磚、起始頁、啟動顯示畫面等，以及列示其中包含的檔案。
-   在 [開始] 功能表上顯示的一組標誌影像 (images/Square150x150Logo.scale-200.png、images/Square44x44Logo.scale-200.png 和 images/Wide310x150Logo.scale-200.png)。
-   在 Windows 市集中代表您的 app 的影像 (images/StoreLogo.png)。
-   app 啟動時顯示的啟動顯示畫面 (images/SplashScreen.scale-200.png)。
-   應用程式啟動時執行的起始頁 (default.html) 和伴隨的 JavaScript 檔案 (default.js)。

若要檢視和編輯檔案，按兩下 [方案總管]**** 中的檔案。

這些檔案對於所有使用 JavaScript 的 UWP app 都是必要的。 您在 Visual Studio 中建立的任何專案都包含這些檔案。

##步驟 2：啟動 app


到目前為止，您已經建立了一個非常簡單的 app。 您可以趁現在建置、部署和啟動您的應用程式，並看看它的外觀。 您可以在本機電腦、模擬器或遠端裝置上進行應用程式的偵錯。 以下是在 Visual Studio 中的目標裝置功能表。

![用於偵錯應用程式的裝置目標下拉式清單](images/uap-debug.png)

### 在傳統型裝置上啟動應用程式

根據預設，app 會在本機電腦上執行。 目標裝置功能表提供從傳統型裝置系列的裝置偵錯應用程式的數個選項。

-   **模擬器**
-   **本機電腦**
-   **遠端電腦**

**在本機電腦上開始偵錯**

1.  在目標裝置功能表 (![開始偵錯功能表](images/startdebug-full.png)) 的 [標準]**** 工具列上，確定已選取 [本機電腦]****。 (這是預設選項)。
2.  按一下工具列上的 [開始偵錯]**** 按鈕 (![開始偵錯按鈕](images/startdebug-sm.png))。

   –或–

   在 [偵錯]**** 功能表中，按一下 [開始偵錯]****。

   –或–

   按 F5。

應用程式會在視窗中開啟，預設啟動顯示畫面會先顯示。 啟動顯示畫面是由影像 (SplashScreen.png) 和背景色彩 (在 app 的資訊清單檔案中指定) 所定義。

啟動顯示畫面消失之後，接著就出現您的應用程式。 它包含一個黑色畫面，上面有「內容在此處出現」的文字。

![電腦上的 HelloWorld app](images/helloworld-1-js.png)

按下 Windows 鍵以開啟 [開始]**** 功能表，然後顯示所有 app。 請注意，在本機部署應用程式會在 [開始] ****功能表上新增磚。 若要再次執行應用程式 (不在偵錯模式)，請點選或按一下 [開始]**** 功能表中的磚。

應用程式還沒有太多功能—但是，恭喜您，您已經建置第一個 UWP app 了！

**停止偵錯**

-   按一下工具列中的 [停止偵錯]**** 按鈕 (![停止偵錯按鈕](images/stopdebug.png))。

   –或–

   在 [偵錯]**** 功能表中，按一下 [停止偵錯]****。

   –或–

   關閉應用程式視窗。

### 在行動裝置模擬器上啟動 app

您的應用程式會在所有 Windows 10 裝置上執行，因此，我們來看看它在 Windows Phone 上的外觀如何。

除了在傳統型裝置上偵錯的選項外，Visual Studio 還提供在連接到電腦的實體行動裝置或在行動裝置模擬器上部署和偵錯應用程式的選項。 您可以為有不同記憶體和顯示器組態的裝置選擇不同的模擬器。

-   **裝置**
-   **模擬器 <SDK version> WVGA 4 inch 512MB**
-   **模擬器 <SDK version> WVGA 4 inch 1GB**
-   等等... (其他組態中的各種模擬器)

在小螢幕和記憶體有限的裝置上測試您的應用程式是不錯的想法，因此，請使用 [模擬器 10.0.10240.0 WVGA 4 inch 512MB]**** 選項。
**在行動裝置模擬器上開始偵錯**

1.  在目標裝置功能表 (![開始偵錯功能表](images/startdebug-full.png)) 的 [標準]**** 工具列上，選擇 [模擬器 10.0.10240.0 WVGA 4 inch 512MB]****。
2.  按一下工具列中的 [開始偵錯]**** 按鈕 (![開始偵錯按鈕](images/startdebug-sm.png))。

   –或–

   在 [偵錯]**** 功能表中，按一下 [開始偵錯]****。

   
Visual Studio 會啟動選取的模擬器，然後部署和啟動您的應用程式。 在行動裝置模擬器上，應用程式看起來會像這樣。

![行動裝置上最初的應用程式畫面](images/helloworld-1-js-phone.png)

## 步驟 3：修改起始頁

Visual Studio 為您建立的其中一個檔案是 default.html，也就是您 app 的起始頁。 當 app 執行時，它會顯示起始頁的內容。 起始頁也包含 app 的程式碼檔案及樣式表的參考。 以下是 Visual Studio 為您建立的起始頁：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>HelloWorld</title>

    <!-- WinJS references -->
    <link href="WinJS/css/ui-dark.css" rel="stylesheet" />
    <script src="WinJS/js/base.js"></script>
    <script src="WinJS/js/ui.js"></script>

    <!-- HelloWorld references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/default.js"></script>
</head>
<body class="win-type-body">
    <p>Content goes here</p>
</body>
</html>
```

讓我們將一些新的內容新增到您的 default.html 檔案。 就像您將內容新增到任何其他 HTML 檔案的方式一樣，您要將內容新增到 [**body**](https://msdn.microsoft.com/library/windows/apps/Hh453011) 元素內。 您可以使用 HTML5 元素建立應用程式 (只有[少數例外](https://msdn.microsoft.com/library/windows/apps/Hh465380))。 這表示您可以使用如 [**h1**](https://msdn.microsoft.com/library/windows/apps/Hh441078)、[**p**](https://msdn.microsoft.com/library/windows/apps/Hh453431)、[**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017)、[**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 以及 [**img**](https://msdn.microsoft.com/library/windows/apps/Hh466114) 之類的 HTML5 元素。

**修改起始頁**

1.  使用 "Hello, world!" 做為第一層標題、詢問使用者名稱的一些文字、接受使用者名稱的 [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) 元素、[**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017) 以及 [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 元素，取代 [**body**](https://msdn.microsoft.com/library/windows/apps/Hh453011) 元素中的現有內容。 將識別碼指派給 **input**、**button** 和 **div**。

 ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What' s your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
    </body>
 ```

2.  在本機電腦上執行 app。 它的外觀如下。

![包含新內容的 HelloWorld app](images/helloworld-2-js.png)

   您可以在 [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) 元素中輸入，但是在這個時候，按一下 [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017) 並不會執行任何動作。 某些物件，例如 **button**，可以在特定事件發生時傳送訊息。 這些事件訊息讓您有機會採取動作來回應事件。 您可以將回應事件的程式碼放置在事件處理常式方法中。

   在後續步驟中，我們要為顯示個人化問候語的 [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017) 建立一個事件處理常式。 我們要將事件處理常式程式碼新增到 default.js 檔案。

##步驟 4：建立事件處理常式

當我們建立新專案時，Visual Studio 會為我們建立 /js/default.js 檔案。 這個檔案包含了處理 app 週期的程式碼。 您也在這裡撰寫其他的程式碼，為 default.html 檔案提供互動功能。

開啟 default.js 檔案。

開始新增我們自己的程式碼之前，我們先來看看檔案中最前面和前後面的幾行程式碼：

```javascript
(function () {
    "use strict";

     // Omitted code 

 })(); 
```

您或許好奇這裡發生了什麼事。 這幾行程式碼將 default.js 程式碼的剩餘部分包裝在自我執行的匿名函式中。 自我執行的匿名函式可以更容易避免命名衝突或避免意外修改到不想修改的值。 它也能夠將非必要的識別碼排除在全域命名空間外，進而提升效能。 它看起來有一點奇怪，但卻是很好的程式設計做法。

下一行程式碼會開啟 JavaScript 程式碼的 [strict 模式](https://msdn.microsoft.com/en-us/library/windows/apps/br230269.aspx)。 Strict 模式為程式碼提供額外的錯誤檢查。 例如，它會防止您使用隱含宣告的變數或將值指派給唯讀屬性。

看看 default.js 中其餘的程式碼。 它會處理您應用程式的 [**activated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 和 [**checkpoint**](https://msdn.microsoft.com/library/windows/apps/BR229839) 事件。 稍後我們會詳細說明這些事件。 現在，只需要知道應用程式啟動時會觸發 **activated** 事件就可以了。

```javascript
   (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
            args.setPromise(WinJS.UI.processAll());
        }
    };

    app.oncheckpoint = function (args) {
        // TODO: This application is about to be suspended. Save any state that needs to persist across suspensions here.
        // You might use the WinJS.Application.sessionState object, which is automatically saved and restored across suspension.
        // If you need to complete an asynchronous operation before your application is suspended, call args.setPromise().
    };

    app.start();
})();
```

讓我們為您的 [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017) 定義一個事件處理常式。 我們新的事件處理常式會從 `nameInput` [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) 控制項取得使用者的名稱，並使用它將問候語輸出到您在上一節中建立的 `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 元素。

### 使用適用於觸控、滑鼠及手寫筆輸入的事件

在 UWP app 中，您不需要擔心觸控、滑鼠以及其他指標輸入形式之間的差異。 您可以只使用您知道的事件 (例如 [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312))，而這些事件適用於所有輸入形式。

**提示** 您的應用程式也可以使用新的 *MSPointer\** 和 *MSGesture\** 事件 (適用於觸控、滑鼠以及手寫筆輸入)，而且可以提供觸發事件之裝置的其他資訊。 如需詳細資訊，請參閱[回應使用者互動](https://msdn.microsoft.com/library/windows/apps/Hh700412)以及[手勢、操作以及互動](https://msdn.microsoft.com/library/windows/apps/Hh761498)。

讓我們繼續建立事件處理常式。

**建立事件處理常式**

1.  在 default.js 中，就在 [**app.oncheckpoint**](https://msdn.microsoft.com/library/windows/apps/BR229839) 事件處理常式之後及呼叫 [**app.start**](https://msdn.microsoft.com/library/windows/apps/BR229705) 之前，建立名為 `buttonClickHandler` 的 [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312) 事件處理常式函式，它接受名為 `eventInfo` 的單一參數。
```javascript
    function buttonClickHandler(eventInfo) {
     
        }
    ```

2.  Inside our event handler, retrieve the user's name from the `nameInput` [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) control and use it to create a greeting. Use the `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) to display the result.
```javascript
    function buttonClickHandler(eventInfo) {
            var userName = document.getElementById("nameInput").value;
            var greetingString = "Hello, " + userName + "!";
            document.getElementById("greetingOutput").innerText = greetingString; 
        }
 ```

您已將事件處理常式新增到 default.js。 現在您需要登錄它。

## 步驟 5：在 app 啟動時登錄事件處理常式


目前我們唯一需要做的事是在按鈕上登錄事件處理常式。 登錄事件處理常式的建議做法是從我們的程式碼呼叫 [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145)。 登錄事件處理常式的理想時機是應用程式啟用時。 幸運的是，Visual Studio 已經為我們在 default.js 檔案中產生一些程式碼，用來處理應用程式的啟用：[**app.onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 事件處理常式。 我們來看看這個程式碼。

```javascript
    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
            args.setPromise(WinJS.UI.processAll());
        }
    };
```

在 [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 處理常式中，程式碼會檢查發生了哪一種類型的啟用。 啟用有許多不同的類型。 例如，當使用者啟動您的應用程式以及當使用者想要開啟與您應用程式相關的檔案時，您的應用程式就會啟用。 如需詳細資訊，請參閱[應用程式週期](https://msdn.microsoft.com/library/windows/apps/Mt243287)。

我們只針對 [**launch**](https://msdn.microsoft.com/library/windows/apps/BR224693) 啟用。 只要應用程式不在執行中，然後使用者啟用它，應用程式就被「啟動」**了。

```javascript
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
```

如果啟用屬於 Launch 啟用，程式碼會檢查應用程式上次執行時的關機方式。

```javascript
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
```

接著它會呼叫 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975)。

```javascript
            args.setPromise(WinJS.UI.processAll());
        }
    };
```    

無論應用程式是過去已經關機或者是第一次啟動，都會呼叫 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975)。 **WinJS.UI.processAll** 包含在 [**setPromise**](https://msdn.microsoft.com/library/windows/apps/JJ215609) 方法的呼叫中，可以確保在應用程式頁面準備好前都不會關閉啟動顯示畫面。

**提示** [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 函式會掃描您的 default.html 檔案，尋找 WinJS 控制項並初始化它們。 到目前為止，我們還沒有新增任何這些控制項，但是最好保留這個程式碼，萬一您之後想要新增就可以使用。

如果要登錄非 WinJS 控制項的事件處理常式，最好緊接在呼叫 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 之後。

**登錄事件處理常式**

-   在 default.js 的 [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 事件處理常式中，擷取 `helloButton`，並使用 [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145) 登錄 [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312) 事件的事件處理常式。 將這個程式碼新增到對 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 的呼叫之後。

```javascript
   app.onactivated = function (args) {
            if (args.detail.kind === activation.ActivationKind.launch) {
                if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                    // TODO: This application has been newly launched. Initialize your application here.
                } else {
                    // TODO: This application was suspended and then terminated.
                    // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                }
                args.setPromise(WinJS.UI.processAll());

             // Retrieve the button and register our event handler. 
                var helloButton = document.getElementById("helloButton");
                helloButton.addEventListener("click", buttonClickHandler, false);
            }
        };
```    

我們已更新的 default.js 檔案的完整程式碼如下：

```javascript
   (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
            args.setPromise(WinJS.UI.processAll());

            // Retrieve the button and register our event handler. 
            var helloButton = document.getElementById("helloButton");
            helloButton.addEventListener("click", buttonClickHandler, false);
        }
    };

    app.oncheckpoint = function (args) {
        // TODO: This application is about to be suspended. Save any state that needs to persist across suspensions here.
        // You might use the WinJS.Application.sessionState object, which is automatically saved and restored across suspension.
        // If you need to complete an asynchronous operation before your application is suspended, call args.setPromise().
    };

    function buttonClickHandler(eventInfo) {
        var userName = document.getElementById("nameInput").value;
        var greetingString = "Hello, " + userName + "!";
        document.getElementById("greetingOutput").innerText = greetingString;
    }

    app.start();
})();
```

執行應用程式。 在文字方塊中輸入您的名稱並按一下按鈕時，app 會顯示個人化問候語。 以下是在本機電腦及在模擬器中的外觀。

![HelloWorld app 的個人化問候語](images/helloworld-3-js.png)

![HelloWorld app 的個人化問候語](images/helloworld-3-js-phone.png)

**注意** 如果您想知道為什麼我們使用 [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145) 在程式碼中登錄事件而不是在 HTML 中設定 [**onclick**](https://msdn.microsoft.com/library/windows/apps/Hh441312) 事件，請參閱[撰寫基本 app 的程式碼](https://msdn.microsoft.com/library/windows/apps/Hh780660)以取得詳細的說明。

## 步驟 6：新增適用於 JavaScript 的 Windows Library 控制項


除了標準 HTML 控制項，您的 app 可以使用適用於 JavaScript 的 Windows Library 中的任何控制項，例如 [**WinJS.UI.DatePicker**](https://msdn.microsoft.com/library/windows/apps/BR211681)、[**WinJS.UI.FlipView**](https://msdn.microsoft.com/library/windows/apps/BR211711)、[**WinjS.UI.ListView**](https://msdn.microsoft.com/library/windows/apps/BR211837)、[**WinJS.UI.Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項。

和 HTML 控制項不同，WinJS 控制項沒有專用的標記元素；例如，您無法透過加入 `<rating />` 元素建立 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項。 若要新增 WinJS 控制項，要建立 [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 元素，並使用 [**data-win-control**](https://msdn.microsoft.com/library/windows/apps/Hh440969) 屬性指定所需的控制項類型。 若要新增 **Rating** 控制項，您要將屬性設定為 "WinJS.UI.Rating"。

讓我們將 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項新增至您的應用程式。

1.  在您的 default.html 檔案中，將 [**label**](https://msdn.microsoft.com/library/windows/apps/Hh453321) 與 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項新增到 `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 後方。

```html
        <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What' s your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
        <label for="ratingControlDiv">
            Rate this greeting:
        </label>
        <div id="ratingControlDiv" data-win-control="WinJS.UI.Rating">
        </div>
    </body>
```

    For the [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) to load, your page must call [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975). Because our app is using one of the Visual Studio templates, your default.js already includes a call to **WinJS.UI.processAll**, as described earlier, so you don't have to add any code.

2.  在本機電腦上執行 app。 注意新的 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項。

   ![含有適用於 JavaScript 之 Windows Library 控制項的 Hello, world 應用程式](images/helloworld-4-JS.png)

現在，按一下 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項會變更評等，但不會執行其他動作。 讓我們使用事件處理常式在使用者變更評分時執行一些動作。

## 步驟 7：為適用於 JavaScript 的 Windows Library 控制項登錄事件處理常式


為 WinJS 控制項登錄事件處理常式，與為標準 HTML 控制項登錄事件處理常式稍有不同。 之前我們提過 [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 事件處理常式會呼叫 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 方法，以在您的標記中初始化 WinJS。 **WinJS.UI.processAll** 是包含在 [**setPromise**](https://msdn.microsoft.com/library/windows/apps/JJ215609) 方法的呼叫中。

```javascript
            args.setPromise(WinJS.UI.processAll());           
```

如果 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 是標準的 HTML 控制項，在此呼叫後，您可以將事件處理常式新增到 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975)。 但是 WinJS 控制項則較為複雜，就像我們的 **Rating** 一樣。 因為 **WinJS.UI.processAll** 會為我們建立 **Rating** 控制項，因此在 **WinJS.UI.processAll** 完成處理之前，我們無法將事件處理常式新增到 **Rating**。

如果 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 是一般方法，我們可以在呼叫它以後便登錄 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 事件處理常式。 但 **WinJS.UI.processAll** 方法是非同步的，因此，遵循該方法的任何程式碼可能會在 **WinJS.UI.processAll** 完成之前執行。 那麼該怎麼辦呢？ 我們使用 [**Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867) 物件接收 **WinJS.UI.processAll** 完成時的通知。

與所有 WinJS 非同步方法一樣，[**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 會傳回 [**Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867) 物件。 **Promise** 是未來會發生某些事的「承諾」；當那件事發生時，表示 **Promise** 已完成。

[
            **Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867) 物件有一個 [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) 方法，會將「完成的」函式做為參數。 **Promise** 會在完成時呼叫此函式。

透過將您的程式碼新增到「完成的」函式，並將它傳遞到 [**Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867) 物件的 [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) 方法，便可確保 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 完成後會執行您的程式碼。

1.  讓我們在使用者選取評分時輸出評分值。 在您的 default.html 檔案中，建立一個 [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 元素來顯示評分值，並為它提供 **id** "ratingOutput"。
```html
        <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What' s your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
        <label for="ratingControlDiv">
            Rate this greeting:
        </label>
        <div id="ratingControlDiv" data-win-control="WinJS.UI.Rating">
        </div>
        <div id="ratingOutput"></div>
    </body>
```

2.  在我們的 default.js 檔案中，為 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項的 [**change**](https://msdn.microsoft.com/library/windows/apps/BR211891) 事件 (名為 `ratingChanged`) 建立事件處理常式。 [
            **eventInfo**](https://msdn.microsoft.com/library/windows/apps/Hh465776) 參數包含一個提供新使用者評分的 **detail.tentativeRating** 屬性。 抓取此值並顯示在輸出 [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 中。

```javascript
        function ratingChanged(eventInfo) {

            var ratingOutput = document.getElementById("ratingOutput");
            ratingOutput.innerText = eventInfo.detail.tentativeRating; 
        }
    ```

3.  Update the code in the [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) event handler that calls [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) by adding a call to the [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) method and passing it a `completed` function. In the `completed` function, retrieve the `ratingControlDiv` element that hosts the [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) control. Then use the [**winControl**](https://msdn.microsoft.com/library/windows/apps/Hh770814) property to retrieve the actual **Rating** control. (This example defines the `completed` function inline.)

```javascript
           args.setPromise(WinJS.UI.processAll().then(function completed() {

                    // Retrieve the div that hosts the Rating control.
                    var ratingControlDiv = document.getElementById("ratingControlDiv");

                    // Retrieve the actual Rating control.
                    var ratingControl = ratingControlDiv.winControl;

                    // Register the event handler. 
                    ratingControl.addEventListener("change", ratingChanged, false);

                }));
    ```

4.  While it's fine to register event handlers for HTML controls after the call to [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975), it's also OK to register them inside your `completed` function. For simplicity, let's go ahead and move all your event handler registrations inside the [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) event handler.

Here's the updated [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) event handler:

```javascript
       app.onactivated = function (args) {
            if (args.detail.kind === activation.ActivationKind.launch) {
                if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                    // TODO: This application has been newly launched. Initialize your application here.
                } else {
                    // TODO: This application was suspended and then terminated.
                    // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                }
                args.setPromise(WinJS.UI.processAll().then(function completed() {

                    // Retrieve the div that hosts the Rating control.
                    var ratingControlDiv = document.getElementById("ratingControlDiv");

                    // Retrieve the actual Rating control.
                    var ratingControl = ratingControlDiv.winControl;

                    // Register the event handler. 
                    ratingControl.addEventListener("change", ratingChanged, false);

                    // Retrieve the button and register our event handler. 
                    var helloButton = document.getElementById("helloButton");
                    helloButton.addEventListener("click", buttonClickHandler, false);

                }));

            }
        };
```        

5.  執行應用程式。 當您選取評分值時，它會在 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項下方輸出數值。

![電腦上完成的 Hello world app](images/helloworld-5-js.png)

![手機上完成的 Hello world app](images/helloworld-5-js-phone.png)

## 摘要

恭喜您！您已經使用 JavaScript 和 HTML 建立`適用於 Windows 10 和 UWP 的第一個 app 了！



<!--HONumber=Mar16_HO1-->


