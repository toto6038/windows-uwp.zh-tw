---
author: GrantMeStrength
ms.assetid: CFB3601D-3459-465F-80E2-520F57B88F62
title: "建立 &quot;Hello, world&quot; app (JS)"
description: "本教學課程會教您如何使用 JavaScript 和 HTML 來建立目標是 Windows 10 上的通用 Windows 平台 (UWP) 的簡單 &amp;\\#0034;Hello, world&amp;\\#0034; app。"
translationtype: Human Translation
ms.sourcegitcommit: 2e0965f964f6f2e10b895d99244b66458eb15903
ms.openlocfilehash: 6c81b24f7fa9abe036d4ccd22ee8fa24c011fe77

---
# 建立 "Hello, world" app (JS)

本教學課程會教您如何使用 JavaScript 和 HTML 來建立目標是 Windows 10 上的通用 Windows 平台 (UWP) 的簡單 "Hello, world" app。 使用 Microsoft Visual Studio 中的單一專案，您可以建置可在任何 Windows 10 裝置上執行的 app。

您將在此處了解如何：

-   建立目標是 **Windows 10** 和 **UWP** 的新 **Visual Studio 2015** 專案。
-   將 HTML 內容新增到起始頁
-   處理觸控、手寫筆以及滑鼠輸入
-   在本機桌面上和 Visual Studio 的手機模擬器上執行專案
-   使用適用於 JavaScript 的 Windows Library 控制項

## 開始之前...

-   [通用 Windows app 是什麼](whats-a-uwp.md)？
-   [Windows 10 的新功能](https://dev.windows.com/whats-new-windows-10-dev-preview)？
-   若要完成這個教學課程，您需要 Windows 10 與 Visual Studio 2015。 [開始設定](get-set-up.md)。
-   我們亦假設您使用的是 Visual Studio 中預設的視窗配置。 如果您變更預設配置，您可以使用 [視窗]**** 功能表中的 [重設視窗配置]**** 命令重設它。

## 步驟 1：在 Visual Studio 中建立新專案

讓我們建立名為 `HelloWorld` 的新應用程式。 方法如下：
1.  啟動 Visual Studio 2015。

2.  從 [檔案]**** 功能表中，選取 [新增] &gt; [專案]**** 來開啟 [新增專案]** 對話方塊。

3.  從左邊的範本清單中，展開 [已安裝] &gt; [範本] &gt; [JavaScript] &gt; [Windows]****，然後選擇 [通用]**** 來查看 UWP 專案範本的清單。 選擇 [WinJS 應用程式 (通用 Windows)]****。

    ![[新增專案] 視窗 ](images/winjs-tut-newproject.png)

    在本教學課程中，我們使用 [WinJS 應用程式]**** 範本。 這個範本會建立能夠編譯和執行的最基本 UWP app，但不包含使用者介面控制項或資料。 在此教學課程系列中，您會將控制項和資料新增到 app。

   (如果您沒有看到這些選項，請確定您已經安裝「通用 Windows 應用程式開發工具」。 如需詳細資訊，請參閱[開始設定](get-set-up.md)。)

4.  在 [名稱]**** 文字方塊中，輸入 "HelloWorld"。
5.  按一下 [確定]**** 來建立專案。
6.  系統會要求您選取要支援的 Windows [目標版本]**** 和 [最小版本]****。 使用預設設定即可，因此請按一下 [確定]****。

    Visual Studio 會建立您的專案，然後在 [方案總管]**** 中顯示。

    ![HelloWorld 專案的 Visual Studio 方案總管](images/winjs-tut-helloworld.png)

雖然 [WinJS 應用程式]**** 是最基本的範本，但是仍然包含少數檔案：

-   一個資訊清單檔案 (package.appxmanifest)，說明 app 的名稱、描述、磚、起始頁、啟動顯示畫面等，以及列示其中包含的檔案。
-   在 [開始] 功能表上顯示的一組標誌影像 (images/Square150x150Logo.scale-200.png、images/Square44x44Logo.scale-200.png 和 images/Wide310x150Logo.scale-200.png)。
-   在 Windows 市集中代表您的 app 的影像 (images/StoreLogo.png)。
-   app 啟動時顯示的啟動顯示畫面 (images/SplashScreen.scale-200.png)。
-   app 啟動時執行的起始頁 (index.html) 和伴隨的 JavaScript 檔案 (main.js)。

若要檢視和編輯檔案，按兩下 [方案總管]**** 中的檔案。

這些檔案對於所有使用 JavaScript 的 UWP app 都是必要的。 您在 Visual Studio 中建立的任何專案都包含這些檔案。

## 步驟 2：啟動 app


到目前為止，您已經建立了一個非常簡單的 App。 您可以趁現在建置、部署和啟動您的應用程式，並看看它的外觀。 您可以在本機電腦、模擬器或遠端裝置上進行應用程式的偵錯。 以下是在 Visual Studio 中的目標裝置功能表。

![用於偵錯應用程式的裝置目標下拉式清單](images/uap-debug.png)

### 在傳統型裝置上啟動應用程式

根據預設，應用程式會在本機電腦上執行。 目標裝置功能表提供從傳統型裝置系列的裝置偵錯應用程式的數個選項。

-   **模擬器**
-   **本機電腦 
**
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

![電腦上的 HelloWorld app](images/helloworld-1-winjs.png)

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
-   **模擬器 <SDK version> WVGA 4 吋 512MB**
-   **模擬器 <SDK version> WVGA 4 吋 1GB**
-   等等... (其他組態中的各種模擬器)

(如果您沒有看到模擬器，請確定您已經安裝「通用 Windows 應用程式開發工具」。 如需詳細資訊，請參閱[開始設定](get-set-up.md)。)

在小螢幕和記憶體有限的裝置上測試您的 app 是不錯的想法，因此，請使用 [模擬器 10.0.14393.0 WVGA 4 inch 512MB]**** 選項。

**在行動裝置模擬器上開始偵錯**

1.  在 [標準]**** 工具列上的目標裝置功能表 (![開始偵錯功能表](images/startdebug-full.png)) 中，選擇 [模擬器 10.0.14393.0 WVGA 4 inch 512MB]****。
2.  按一下工具列中的 [開始偵錯]**** 按鈕 (![開始偵錯按鈕](images/startdebug-sm.png))。

   –或–

   在 [偵錯]**** 功能表中，按一下 [開始偵錯]****。


Visual Studio 會啟動選取的模擬器，然後部署和啟動您的 App。 在初始啟動時，模擬器可能會需要一些時間來啟動。 您可能會看到關於 HyperV 的錯誤，按一下 [重試]**** 應該可以解決這個問題。 在行動裝置模擬器上，App 看起來會像這樣。

![行動裝置上最初的應用程式畫面](images/helloworld-1-winjs-phone.png)

## 步驟 3：修改起始頁

Visual Studio 為您建立的其中一個檔案是 **index.html**，也就是您 app 的起始頁。 當 app 執行時，它會顯示起始頁的內容。 起始頁也包含 app 的程式碼檔案及樣式表的參考。 以下是 Visual Studio 為您建立的起始頁：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>HelloWorld</title>

    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>

    <!-- HelloWorld references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/main.js"></script>
</head>
<body class="win-type-body">
    <div>Content goes here!</div>
</body>
</html>
```

讓我們將一些新的內容新增到您的 default.html 檔案。 就像您將內容新增到任何其他 HTML 檔案的方式一樣，您要將內容新增到 [body](https://msdn.microsoft.com/library/windows/apps/Hh453011) 元素內。 您可以使用 HTML5 元素建立 app (只有[少數例外](https://msdn.microsoft.com/library/windows/apps/Hh465380))。 這表示您可以使用如 [h1](https://msdn.microsoft.com/library/windows/apps/Hh441078)、[p](https://msdn.microsoft.com/library/windows/apps/Hh453431)、[button](https://msdn.microsoft.com/library/windows/apps/Hh453017)、[div](https://msdn.microsoft.com/library/windows/apps/Hh453133) 以及 [img](https://msdn.microsoft.com/library/windows/apps/Hh466114) 之類的 HTML5 元素。

**修改起始頁**

1.  使用 "Hello, world!" 做為第一層標題、詢問使用者名稱的一些文字、接受使用者名稱的 **input** 元素、**button** 以及 **div** 元素，取代 **body** 元素中的現有內容。 將識別碼指派給 **input**、**button** 和 **div**。

 ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
    </body>
 ```

2.  在本機電腦上執行 app。 它的外觀如下。

![包含新內容的 HelloWorld app](images/helloworld-2-winjs.png)

   您可以在 **input** 元素中輸入，但是在這個時候，按一下 **button** 並不會執行任何動作。 某些物件，例如 **button**，可以在特定事件發生時傳送訊息。 這些事件訊息讓您有機會採取動作來回應事件。 您可以將回應事件的程式碼放置在事件處理常式方法中。

   在後續步驟中，我們要為顯示個人化問候語的 **button** 建立一個事件處理常式。 我們要將事件處理常式程式碼新增到 main.js 檔案。

## 步驟 4：建立事件處理常式

當我們建立新專案時，Visual Studio 會為我們建立 /js/main.js 檔案。 這個檔案包含了處理 app 週期的程式碼。 您也在這裡撰寫其他的程式碼，為 index.html 檔案提供互動功能。

開啟 main.js 檔案。

開始新增我們自己的程式碼之前，我們先來看看檔案中最前面和最後面的幾行程式碼：

```javascript
(function () {
    "use strict";

     // Omitted code

 })();
```

您或許好奇這裡發生了什麼事。 這幾行程式碼將 main.js 程式碼的剩餘部分包裝在自我執行的匿名函式中。 自我執行的匿名函式可以更容易避免命名衝突或避免意外修改到不想修改的值。 它也能夠將非必要的識別碼排除在全域命名空間外，進而提升效能。 它看起來有一點奇怪，但卻是很好的程式設計做法。

下一行程式碼會開啟 JavaScript 程式碼的 [strict 模式](https://msdn.microsoft.com/library/windows/apps/br230269.aspx)。 Strict 模式為程式碼提供額外的錯誤檢查。 例如，它會防止您使用隱含宣告的變數或將值指派給唯讀屬性。

看看 main.js 中其餘的程式碼。 它會處理您 app 的 [activated](https://msdn.microsoft.com/library/windows/apps/BR212679) 和 [checkpoint](https://msdn.microsoft.com/library/windows/apps/BR229839) 事件。 稍後我們會詳細說明這些事件。 現在，只需要知道 app 啟動時會觸發 **activated** 事件就可以了。

```javascript
   (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    var isFirstActivation = true;

    app.onactivated = function (args) {
          if (args.detail.kind === activation.ActivationKind.voiceCommand) {
            // TODO: Handle relevant ActivationKinds. For example, if your app can be started by voice commands,
            // this is a good place to decide whether to populate an input field or choose a different initial view.
        }
          else if (args.detail.kind === activation.ActivationKind.launch) {
            // A Launch activation happens when the user launches your app via the tile
            // or invokes a toast notification by clicking or tapping on the body.
              if (args.detail.arguments) {
                // TODO: If the app supports toasts, use this value from the toast payload to determine where in the app
                // to take the user in response to them invoking a toast notification.
              }
              else if (args.detail.previousExecutionState === activation.ApplicationExecutionState.terminated) {
                // TODO: This application had been suspended and was then terminated to reclaim memory.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                // Note: You may want to record the time when the app was last suspended and only restore state if they've returned after a short period.
            }
        }

        if (!args.detail.prelaunchActivated) {
            // TODO: If prelaunchActivated were true, it would mean the app was prelaunched in the background as an optimization.
            // In that case it would be suspended shortly thereafter.
            // Any long-running operations (like expensive network or disk I/O) or changes to user state which occur at launch
            // should be done here (to avoid doing them in the prelaunch case).
            // Alternatively, this work can be done in a resume or visibilitychanged handler.
        }

        if (isFirstActivation) {
            // TODO: The app was activated and had not been running. Do general startup initialization here.
            document.addEventListener("visibilitychange", onVisibilityChanged);
            args.setPromise(WinJS.UI.processAll());
        }

        isFirstActivation = false;
    };
```

讓我們為您的 [button](https://msdn.microsoft.com/library/windows/apps/Hh453017) 定義一個事件處理常式。 我們新的事件處理常式會從 `nameInput` [input](https://msdn.microsoft.com/library/windows/apps/Hh453271) 控制項取得使用者的名稱，並使用它將問候語輸出到您在上一節中建立的 `greetingOutput` **div** 元素。

### 使用適用於觸控、滑鼠及手寫筆輸入的事件

在 UWP app 中，您不需要擔心觸控、滑鼠以及其他指標輸入形式之間的差異。 您可以只使用您知道的事件 (例如 [click](https://msdn.microsoft.com/library/windows/apps/Hh441312))，而這些事件適用於所有輸入形式。

**提示** 您的 app 也可以使用新的 *MSPointer\** 和 *MSGesture\** 事件 (適用於觸控、滑鼠以及手寫筆輸入)，而且可以提供觸發事件之裝置的其他資訊。 如需詳細資訊，請參閱[回應使用者互動](https://msdn.microsoft.com/library/windows/apps/Hh700412)以及[手勢、操作以及互動](https://msdn.microsoft.com/library/windows/apps/Hh761498)。

讓我們繼續建立事件處理常式。

**建立事件處理常式**

1.  在 main.js 中，就在 [**app.oncheckpoint**](https://msdn.microsoft.com/library/windows/apps/BR229839) 事件處理常式之後及呼叫 [**app.start**](https://msdn.microsoft.com/library/windows/apps/BR229705) 之前，建立名為 `buttonClickHandler` 的 [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312) 事件處理常式函式，它接受名為 `eventInfo` 的單一參數。
```javascript
    function buttonClickHandler(eventInfo) {

        }
```

2.  在事件處理常式中，從 `nameInput` [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) 控制項擷取使用者的名稱，並使用它來建立問候語。 使用 `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 顯示結果。
```javascript
    function buttonClickHandler(eventInfo) {
            var userName = document.getElementById("nameInput").value;
            var greetingString = "Hello, " + userName + "!";
            document.getElementById("greetingOutput").innerText = greetingString;
        }
 ```

您已將事件處理常式新增到 main.js。 現在您需要登錄它。

## 步驟 5：在 app 啟動時登錄事件處理常式


目前我們唯一需要做的事是在按鈕上登錄事件處理常式。 登錄事件處理常式的建議做法是從我們的程式碼呼叫 [addEventListener](https://msdn.microsoft.com/library/windows/apps/Hh441145)。 登錄事件處理常式的理想時機是 app 啟用時。 幸運的是，Visual Studio 已經為我們在 main.js 檔案中產生一些程式碼，用來處理 app 的啟用。


在 [onactivated](https://msdn.microsoft.com/library/windows/apps/BR212679) 處理常式中，程式碼會檢查發生了哪一種類型的啟用。 啟用有許多不同的類型。 例如，當使用者啟動您的應用程式以及當使用者想要開啟與您應用程式相關的檔案時，您的應用程式就會啟用。 (如需詳細資訊，請參閱[應用程式週期](https://msdn.microsoft.com/library/windows/apps/Mt243287)。)

我們只針對 [launch](https://msdn.microsoft.com/library/windows/apps/BR224693) 啟用。 只要 app 不在執行中，然後使用者啟用它，app 就被「啟動」**了。 無論 app 是過去已經關閉或者是第一次啟動，都會呼叫 [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975)。 **WinJS.UI.processAll** 包含在 [setPromise](https://msdn.microsoft.com/library/windows/apps/JJ215609) 方法的呼叫中，可以確保在 app 頁面準備好前都不會關閉啟動顯示畫面。

**提示** **WinJS.UI.processAll** 函式會掃描您的 default.html 檔案，尋找 WinJS 控制項並初始化它們。 到目前為止，我們還沒有新增任何這些控制項，但是最好保留這個程式碼，萬一您之後想要新增就可以使用。

如果要登錄非 WinJS 控制項的事件處理常式，最好緊接在呼叫 **WinJS.UI.processAll** 之後。

**登錄事件處理常式**

-   在 main.js 的 [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 事件處理常式中，擷取 `helloButton`，並使用 [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145) 登錄 [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312) 事件的事件處理常式。 將這個程式碼新增到對 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 的呼叫之後。

```javascript
   app.onactivated = function (args) {
           // Omitted code
           if (isFirstActivation) {
              document.addEventListener("visibilitychange", onVisibilityChanged);
              args.setPromise(WinJS.UI.processAll());

              // Add your code to retrieve the button and register the event handler.
              var helloButton = document.getElementById("helloButton");
              helloButton.addEventListener("click", buttonClickHandler, false);
            }

```    



執行 App。 在文字方塊中輸入您的名稱並按一下按鈕時，app 會顯示個人化問候語。

**注意** 如果您想知道為什麼我們使用 [addEventListener](https://msdn.microsoft.com/library/windows/apps/Hh441145) 在程式碼中登錄事件而不是在 HTML 中設定 [onclick](https://msdn.microsoft.com/library/windows/apps/Hh441312) 事件，請參閱[撰寫基本 app 的程式碼](https://msdn.microsoft.com/library/windows/apps/Hh780660)以取得詳細的說明。

## 步驟 6：新增適用於 JavaScript 的 Windows Library 控制項


除了標準 HTML 控制項，您的 app 可以使用[適用於 JavaScript 的 Windows Library](https://msdn.microsoft.com/library/windows/apps/BR229782) 中的任何控制項，例如 [WinJS.UI.DatePicker](https://msdn.microsoft.com/library/windows/apps/BR211681)、[WinJS.UI.FlipView](https://msdn.microsoft.com/library/windows/apps/BR211711)、[WinjS.UI.ListView](https://msdn.microsoft.com/library/windows/apps/BR211837)、[WinJS.UI.Rating](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項。

和 HTML 控制項不同，WinJS 控制項沒有專用的標記元素：例如，您無法透過加入 `<rating />` 元素建立 [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項。 若要新增 WinJS 控制項，要建立 **div** 元素，並使用 [data-win-control](https://msdn.microsoft.com/library/windows/apps/Hh440969) 屬性指定所需的控制項類型。 若要新增 **Rating** 控制項，您要將屬性設定為 "WinJS.UI.Rating"。

**將 Rating 控制項新增至您的 app。**

1.  在您的 index.html 檔案中，將 [label](https://msdn.microsoft.com/library/windows/apps/Hh453321) 與 [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項新增到 `greetingOutput` **div** 後方。

    ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
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

2.  在本機電腦上執行 app。 注意新的 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項。

   ![含有適用於 JavaScript 之 Windows Library 控制項的 Hello, world app](images/helloworld-4-winjs.png)

> 為使 **Rating** 載入，您的頁面必須呼叫 [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975)。 因為我們的 app 正在使用其中一種 Visual Studio 範本，因此我們的 main.js 已包含對 **WinJS.UI.processAll** 的呼叫 (如之前所述)，所以不必新增任何程式碼。

現在，按一下 **Rating** 控制項會變更評等，但不會執行其他動作。 讓我們使用事件處理常式在使用者變更評分時執行一些動作。

## 步驟 7：為適用於 JavaScript 的 Windows Library 控制項登錄事件處理常式


為 WinJS 控制項登錄事件處理常式，與為標準 HTML 控制項登錄事件處理常式稍有不同。 之前我們提過 **onactivated** 事件處理常式會呼叫 **WinJS.UI.processAll** 方法，以在您的標記中初始化 WinJS。 **WinJS.UI.processAll** 呼叫是包含在對 **setPromise** 方法的呼叫中，就像這樣：

```javascript
            args.setPromise(WinJS.UI.processAll());           
```

如果 **Rating** 是標準的 HTML 控制項，在此呼叫後，您可以將事件處理常式新增到 **WinJS.UI.processAll**。 但是 WinJS 控制項則較為複雜，就像我們的 **Rating** 一樣。 因為 **WinJS.UI.processAll** 會為我們建立 **Rating** 控制項，因此在 **WinJS.UI.processAll** 完成處理之前，我們無法將事件處理常式新增到 **Rating**。

如果 **WinJS.UI.processAll** 是一般方法，我們可以在呼叫它以後便登錄 **Rating** 事件處理常式。 但 **WinJS.UI.processAll** 方法是非同步的，因此，遵循該方法的任何程式碼可能會在 **WinJS.UI.processAll** 完成之前執行。 那麼該怎麼辦呢？ 我們使用 [Promise](https://msdn.microsoft.com/library/windows/apps/BR211867) 物件接收 **WinJS.UI.processAll** 完成時的通知。

與所有 WinJS 非同步方法一樣，**WinJS.UI.processAll** 會傳回 **Promise** 物件。 **Promise** 是未來會發生某些事的「承諾」；當那件事發生時，表示 **Promise** 已完成。

[Promise](https://msdn.microsoft.com/library/windows/apps/BR211867) 物件有一個 [then](https://msdn.microsoft.com/library/windows/apps/BR229728) 方法，會將 "completed" (已完成的) 函式做為參數。 **Promise** 會在完成時呼叫此函式。

透過將您的程式碼新增到 "completed" 函式，並將它傳遞到 **Promise** 物件的 **then** 方法，便可確保 **WinJS.UI.processAll** 完成後會執行您的程式碼。

**輸出使用者選取的評分值**

1.  在您的 index.html 檔案中，建立一個 [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 元素來顯示評分值，並為它提供 **id** "ratingOutput"。

    ```html
        <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
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

2.  在我們的 index.js 檔案中，為 **Rating** 控制項的 [change](https://msdn.microsoft.com/library/windows/apps/BR211891) 事件 (名為 `ratingChanged`) 建立事件處理常式。 [eventInfo](https://msdn.microsoft.com/library/windows/apps/Hh465776) 參數包含一個提供新使用者評分的 **detail.tentativeRating** 屬性。 抓取此值並顯示在輸出 **div** 中。

    ```javascript
        function ratingChanged(eventInfo) {

            var ratingOutput = document.getElementById("ratingOutput");
            ratingOutput.innerText = eventInfo.detail.tentativeRating;
        }
```

3.  新增呼叫至 [then](https://msdn.microsoft.com/library/windows/apps/BR229728) 方法，並將 `completed` 函式傳送給它，以在呼叫 [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975) 的 [onactivated](https://msdn.microsoft.com/library/windows/apps/BR212679) 事件處理常式中更新程式碼。 在 `completed` 函式中，抓取裝載 [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項的 `ratingControlDiv` 元素。 然後使用 [winControl](https://msdn.microsoft.com/library/windows/apps/Hh770814) 屬性抓取實際的 **Rating** 控制項。 (此範例以內嵌方式定義 `completed` 函式)。

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

4.  您可以在呼叫 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 後為 HTML 控制項登錄事件處理常式，也可以在您的 `completed` 函式中登錄它們。 為了簡化工作，我們將所有事件處理程式登錄移到 [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) 事件處理常式內。

    下列是更新後的 [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 事件處理常式：

    ```javascript
    (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    var isFirstActivation = true;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.voiceCommand) {
            // TODO: Handle relevant ActivationKinds. For example, if your app can be started by voice commands,
            // this is a good place to decide whether to populate an input field or choose a different initial view.
        }
        else if (args.detail.kind === activation.ActivationKind.launch) {
            // A Launch activation happens when the user launches your app via the tile
            // or invokes a toast notification by clicking or tapping on the body.
            if (args.detail.arguments) {
                // TODO: If the app supports toasts, use this value from the toast payload to determine where in the app
                // to take the user in response to them invoking a toast notification.
            }
            else if (args.detail.previousExecutionState === activation.ApplicationExecutionState.terminated) {
                // TODO: This application had been suspended and was then terminated to reclaim memory.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                // Note: You may want to record the time when the app was last suspended and only restore state if they've returned after a short period.
            }
        }

        if (!args.detail.prelaunchActivated) {
            // TODO: If prelaunchActivated were true, it would mean the app was prelaunched in the background as an optimization.
            // In that case it would be suspended shortly thereafter.
            // Any long-running operations (like expensive network or disk I/O) or changes to user state which occur at launch
            // should be done here (to avoid doing them in the prelaunch case).
            // Alternatively, this work can be done in a resume or visibilitychanged handler.
        }

        if (isFirstActivation) {
            // TODO: The app was activated and had not been running. Do general startup initialization here.
            document.addEventListener("visibilitychange", onVisibilityChanged);

            args.setPromise(WinJS.UI.processAll().then(function completed() {
                var ratingControlDiv = document.getElementById("ratingControlDiv");
                var ratingControl = ratingControlDiv.winControl;
                ratingControl.addEventListener("change",ratingChanged, false);
            }));

            var helloButton = document.getElementById("helloButton");
            helloButton.addEventListener("click", buttonClickHandler, false);

        }

        isFirstActivation = false;
    };

    ```        

    執行 App。 當您選取評分值時，它會在 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控制項下方輸出數值。

![電腦上完成的 Hello world app](images/helloworld-5-winjs.png)

## 摘要

恭喜您！您已經使用 JavaScript 和 HTML 建立`適用於 Windows 10 和 UWP 的第一個 app 了！



<!--HONumber=Aug16_HO3-->


