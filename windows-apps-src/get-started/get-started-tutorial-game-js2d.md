---
title: "入門教學課程 - 以 JavaScript 撰寫的 UWP 遊戲"
description: "以 JavaScript 和 CreateJS 撰寫的適用於 Windows 市集的簡單 UWP 遊戲"
author: jken
ms.author: jken
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 01af8254-b073-445e-af4c-e474528f8aa3
ms.openlocfilehash: 3325883b85c87753625a39d5b5634a864bd9c49a
ms.sourcegitcommit: 3e083fe2e253365e506d3e215fca8c35e5e92243
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/18/2017
---
# <a name="get-started-tutorial-a-uwp-game-in-javascript"></a>入門教學課程：以 JavaScript 撰寫的 UWP 遊戲

## <a name="a-simple-2d-uwp-game-for-the-windows-store-written-in-javascript-and-createjs"></a>以 JavaScript 和 CreateJS 撰寫的適用於 Windows 市集的簡單 2D UWP 遊戲


![Walking Dino Sprite 工作表](images/JS2D_1.png)


## <a name="introduction"></a>簡介


發佈 app 至 Windows 市集，意味著您可以分享（或賣！）給數百萬使用者 (在許多不同裝置上)。  

為了將您的 app 發佈至 Windows 市集，必須撰寫為 UWP（通用 Windows 平台）App。 不過 UWP 非常有彈性，可支援各種不同的語言版本及架構。 若要證明這點，此範例是以 JavaScript 撰寫的簡單遊戲，使用數個 CreateJS 程式庫，並示範如何繪圖 Sprite、建立遊戲迴圈、支援鍵盤與滑鼠，以及適應不同的螢幕大小。

這個專案是使用 Visual Studio 以 JavaScript 建置而成。 只要某些小變更，它也可以裝載於網站，或是適用於其他平台。 

**注意：**這並非完整（或完美！）的遊戲；設計目的是示範使用 JavaScript 和協力廠商程式庫，建立可發佈至 Windows 市集的 App。


## <a name="requirements"></a>需求

若要試用這個專案，您將需要下列各項：

* 執行 Windows 10 目前版本的 Windows 的電腦（或虛擬機器）。
* 一份 Visual Studio 複本。 您可以從 [Visual Studio 首頁](http://visualstudio.com)下載免費的 Visual Studio Community Edition。

這個專案利用 CreateJS JavaScript 架構。 CreateJS 是一套免費工具，根據 MIT 授權，可讓您輕鬆地建立以 Sprite 為基礎的遊戲。 CreateJS 程式庫已存在專案中 (在方案總管檢視中尋找 *js/easeljs-0.8.2.min.js*，以及 *js/preloadjs-0.6.2.min.js*)。 您可以在 [CreateJS 首頁](http://www.createjs.com)找到有關 CreateJS 的資訊。


## <a name="getting-started"></a>開始使用

app 的完整原始碼儲存在 [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js2d)。

瀏覽 GitHub、按一下綠色 **\[Clone or download\]** (複製或下載) 按鈕，然後選取 **\[在 Visual Studio 中開啟\]**，是最簡單的入門使用方式。 

![複製存放庫](images/JS2D_2.png)

您也可以下載專案為 zip 檔案，或使用任何可搭配 [GitHub 專案](https://msdn.microsoft.com/en-us/windows/uwp/get-started/get-uwp-app-samples) 的其他標準方式。

一旦方案載入到 Visual Studio 之後，您會看到數個檔案，包括：

* Images/ - 包含 UWP app 所需各種圖示以及遊戲 SpriteSheet 和某些其他點陣圖的資料夾。
* js/ - 包含 JavaScript 檔案的資料夾。 main.js 檔案是我們的遊戲，其他檔案是 EaselJS 和 PreloadJS。
* index.html - 包含畫布物件（裝載遊戲圖形）的網頁。

現在您可以執行遊戲！

按 **F5** 以開始執行 app。 您應該會看到視窗開啟，以及我們熟悉的恐龍站在田園 (或稀疏) 風景中。 我們現在將會檢查 app、解釋一些重要的部分，並在過程中解除鎖定其餘部分功能。

![只是日常的恐龍與坐在其背上的忍者貓](images/JS2D_3.png)

**注意：**出現錯誤？ 請務必安裝有 Web 支援的 Visual Studio。 您可以建立新的專案進行檢查，如果沒有 JavaScript 支援，您必須重新安裝 Visual Studio，並核取 *\[Microsoft Web Developer Tools\]* 方塊。

## <a name="walkthough"></a>逐步解說

如果您按 F5 開始遊戲時，您可能想知道發生什麼事。 答案「並不多」，因為許多程式碼目前已標示註解。 到目前為止，您只會看到恐龍，以及按下空格鍵的無力請求。 

### <a name="1-setting-the-stage"></a>1. 設定舞台

如果您打開並檢查 **index.html**，您會看到它幾乎是空白的。 這個檔案是預設網頁，包含我們的 app，而且只做兩件重要的事。 首先，它包含 **EaselJS** 和 **PreloadJS** CreateJS 程式庫的 JavaScript 原始碼，以及 **main.js**（我們自己的來源程式碼檔案）。
第二，它會定義 &lt;canvas&gt; 標記，這是我們所有的圖形將會出現的位置。 &lt;canvas&gt; 是標準 HTML5 文件元件。 我們為其提供名稱 (gameCanvas)，以便在 **main.js** 中的程式碼可以參考它。 順帶一提，若要從頭撰寫 JavaScript 遊戲，您也需要複製 **EaselJS** 和 **PreloadJS** 檔案到您的方案，並建立畫布物件。

EaselJS 為我們提供新的物件，稱為*舞台*。 舞台連結到畫布，用來顯示影像和文字。 想要顯示在舞台上的任何物件，必須先加入為舞台的子系，如下：

```
    stage.addChild(myObject);
```

您會看到該程式碼行在 **main.js** 中顯示數次

說到這個檔案，現在是開啟 **main.js** 的好時機。

### <a name="2-loading-the-bitmaps"></a>2. 載入點陣圖

EaselJS 為我們提供許多不同類型的圖形物件。 我們可以建立簡單形狀（例如藍色矩形用於天空），或點陣圖（例如我們將要新增的雲朵）、文字物件和 Sprite。 Sprite 使用 (SpriteSheet)[http://createjs.com/docs/easeljs/classes/SpriteSheet.html]：包含多個影像的單一點陣圖。 例如我們使用這個 SpriteSheet 儲存恐龍動畫的不同畫面：

![Walking Dino Sprite 工作表](images/JS2D_4.png)

我們讓恐龍走路，藉由在此程式碼中定義不同畫面和畫面應該動畫顯示的速度：

```
    // Define the animated dino walk using a spritesheet of images,
    // and also a standing-still state, and a knocked-over state.
    var data = {
        images: [loader.getResult("dino")],
        frames: { width: 373, height: 256 },
        animations: {
            stand: 0,
            lying: { 
                frames: [0, 1],
                speed: 0.1
            },
            walk: {
                frames: [0, 1, 2, 3, 2, 1],
                speed: 0.4
            }
        }
    }

    var spriteSheet = new createjs.SpriteSheet(data);
    dino_walk = new createjs.Sprite(spriteSheet, "walk");
    dino_stand = new createjs.Sprite(spriteSheet, "stand");
    dino_lying = new createjs.Sprite(spriteSheet, "lying");

```

現在，我們新增一些小的毛毛雲朵到舞台。 一旦遊戲執行，雲朵會飄過螢幕上。 雲的影像已在方案中的 *images* 資料夾。

瀏覽 **main.js** 直到找到 **init()** 函式。 遊戲開始時呼叫這個函式，它是我們開始設定所有圖形物件的位置。

找出下列程式碼，並從參照雲影像的該行移除註解 (\\)。

```
 manifest = [
        { src: "walkingDino-SpriteSheet.png", id: "dino" },
        { src: "barrel.png", id: "barrel" },
        { src: "fluffy-cloud-small.png", id: "cloud" },
    ];
```

JavaScript 在載入資源時 (例如影像) 需要一點協助，所以我們使用可以預先載入影像的 CreateJS 程式庫功能，稱為 [LoadQueue](http://www.createjs.com/docs/preloadjs/classes/LoadQueue.html)。 我們無法確定載入影像需要多久，所以我們使用 LoadQueue 來處理它。 當有可用的影像時，佇列會告訴我們影像已就緒。 若要這樣做，我們先建立可列出所有影像的新物件，然後我們建立 LoadQueue 物件。 在下面的程式碼，您會看到如何設定此物件，以便在一切已準備就緒時呼叫 **loadingComplete()** 函式。

```
    // Now we create a special queue, and finally a handler that is
    // called when they are loaded. The queue object is provided by preloadjs.

    loader = new createjs.LoadQueue(false);
    loader.addEventListener("complete", loadingComplete);
    loader.loadManifest(manifest, true, "../images/");
```    

當呼叫 **loadingComplete()** 函式，影像會載入且可供使用。 您會看到建立雲朵的已標示註解區段，現在可以使用它們的點陣圖。 移除註解，讓它看起來像這樣：

```
    // Create some clouds to drift by..
    for (var i = 0; i < 3; i++) {
        cloud[i] = new createjs.Bitmap(loader.getResult("cloud"));
        cloud[i].x = Math.random()*1024; // Random start location
        cloud[i].y = 64 + i * 48;
        stage.addChild(cloud[i]);
    }
```
此程式碼會建立三個雲物件 (每個使用我們預先載入的影像)、定義其位置，然後將其新增至舞台。

再次執行 app（按下 F5），您會看到雲朵會出現。

### <a name="3-moving-the-clouds"></a>3. 移動雲朵

現在我們要讓雲朵移動。 移動雲朵 - 事實上移動任何項目 - 的秘密是，設定每秒重複呼叫多次的 [ticker](http://www.createjs.com/docs/easeljs/classes/Ticker.html) 函式。 每次呼叫這個函式，它便會在稍微不同的地方重新繪製圖形。

<p data-height="500" data-theme-id="23761" data-slug-hash="vxZVRK" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="CreateJS - Animating clouds" data-preview="true" data-editable="true" class="codepen">請參閱由 Microsoft Edge Docs (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) 發佈到 <a href="http://codepen.io">CodePen</a> 的 Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/vxZVRK/">CreateJS - 以動畫顯示雲朵</a>。</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
 
若要這樣做的程式碼已在 **main.js** 檔案中，由 CreateJS 程式庫 EaselJS 所提供的。 它的外觀如下：

```
    // Set up the game loop and keyboard handler.
    // The keyword 'tick' is required to automatically animated the sprite.
    createjs.Ticker.timingMode = createjs.Ticker.RAF;
    createjs.Ticker.addEventListener("tick", gameLoop);
```

此程式碼將在介於每秒 30 到 60 個畫面之間呼叫 **gameLoop()** 函式。 確切速度視電腦速度而定。

尋找 **gameLoop()** 函式，向下到結尾，您會看到 **animateClouds()** 函式。 編輯它，讓它沒有標示註解。

```
    // Move clouds
    animateClouds();
```

如果您檢視這個函式的定義，您會看到它依次變更每個雲朵的 x 座標。 如果 x 座標在螢幕邊緣外，它會重設為最右邊。 每個雲朵也會以些許不同的速度移動。

```
function animate_clouds()
{
    // Move the cloud sprites across the sky. If they get to the left edge, 
    // move them over to the right.

    for (var i = 0; i < 3; i++) {    
        cloud[i].x = cloud[i].x - (i+1);
        if (cloud[i].x < -128)
            cloud[i].x = width + 128;
    }
}
```

如果您目前執行 app 時，您會看到雲朵已經開始飄移。 最後，我們有動作！

### <a name="4-adding-keyboard-and-mouse-input"></a>4. 新增鍵盤和滑鼠輸入

不能與人互動的遊戲不是遊戲。 所以我們允許玩家使用鍵盤或滑鼠執行一些動作。 回到 **loadingComplete()** 函式，您將會看到以下。 移除註解。

```
    // This code will call the method 'keyboardPressed' is the user presses a key.
    this.document.onkeydown = keyboardPressed;

    // Add support for mouse clicks
    stage.on("stagemousedown", mouseClicked);
```

現在，我們有兩個函式，當玩家按下鍵盤按鍵或按一下滑鼠時呼叫它們。 這兩個事件都會呼叫 **userDidSomething()**，這個函式會查看 gamestate 變數，決定遊戲目前執行的動作，以及因此接下來需要發生的動作。

Gamestate 是遊戲中使用的常見設計模式。 發生的所有項目，都是在 ticker 計時器所呼叫的 **gameLoop()** 函式中發生。 gameLoop() 追蹤正在玩遊戲狀態中、處於「遊戲結束狀態」或「準備玩遊戲狀態」或作者使用變數定義的其他狀態。 這個 state 變數在切換陳述式中進行測試，它會定義要呼叫哪些其他的功能。 因此如果狀態設定為 "playing"，就會呼叫可讓恐龍跳躍以及讓桶子移動的函式。 如果恐龍被殺死，gamestate 變數將設為 "game over state"，而且「遊戲結束！」 訊息將會顯示。 如果您對遊戲設計模式感興趣，[遊戲程式設計模式](http://gameprogrammingpatterns.com/)這本書非常有幫助。

請再次嘗試執行 app，最後您就可以開始玩遊戲。 按空格鍵（或按一下滑鼠，或輕觸螢幕）即可開始遊戲。 

您會看到桶子滾向您的方向：適時按空格鍵或再按一下，恐龍就會跳起。 時間錯誤，而且遊戲就會結束。

桶子動畫顯示的方式和雲朵相同（雖然每次變得更快），我們會檢查恐龍和桶子的位置，確定它們尚未碰撞：

```
 // Very simple check for collision between dino and barrel
                if ((barrel.x > 220 && barrel.x < 380)
                    &&
                    (!jumping))
                {
                    barrel.x = 380;
                    GameState = GameStateEnum.GameOver;
                }
```

如果恐龍不跳躍而且桶子在附近，程式碼會將 state 變數變更為 *GameOver* 狀態。 正如您所想，*GameOver* 會停止遊戲。

我們遊戲的主要機制已完成。

### <a name="5-resizing-support"></a>5. 調整大小支援

我們幾乎完成了！ 但在完成之前，還要先處理一個擾人的問題。 執行遊戲時，請嘗試調整視窗大小。 您會看到遊戲很快變得非常混亂，因為物件不再位於其位置。 我們可以處理這個問題，方式是為視窗調整大小事件 (當玩家調整視窗大小或當裝置從橫向旋轉至直向時所產生) 建立處理常式。

若要這樣做的程式碼已存在（事實上，當遊戲第一次開始時我們呼叫它，以確定預設視窗的大小正常運作，因為 UWP app 啟動時，您就無法確定視窗大小為何）。

只要取消註解這行程式碼，即可在螢幕大小事件引發時呼叫函式：

```
    // This code makes the app call the method 'resizeGameWindow' if the user resizes the current window.
     window.addEventListener('resize', resizeGameWindow);
```

如果您再次執行 app 時，您現在應該可以調整視窗大小，以取得較好的結果。

## <a name="publishing-to-the-windows-store"></a>發佈到 Windows 市集

現在您擁有 UWP 應用程式，可以將它發行至 Windows 市集（假設您已先將它改進！） 

程序有幾個步驟。

1. 您必須[註冊](https://developer.microsoft.com/en-us/store/register)為 Windows 開發人員。
2. 您必須使用 App 提交[檢查清單](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)。
3. 必須提交 App 以取得[認證](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process)。

如需詳細資訊，請參閱[發佈 Windows 市集應用程式](https://developer.microsoft.com/en-us/store/publish-apps)。

## <a name="suggestions-for-other-features"></a>其他功能的建議。

接下來該怎麼辦？ 以下是幾個功能建議，可新增至您的（即將）獲獎的應用程式。

1. 音效。 CreateJS 程式庫在 [SoundJS](http://www.createjs.com/soundjs) 程式庫中包含對音效的支援。
2. 遊戲台支援。 有 [API 可用](https://gamedevelopment.tutsplus.com/tutorials/using-the-html5-gamepad-api-to-add-controller-support-to-browser-games--cms-21345)。
3. 讓您的遊戲變得更更好！ 這個部分操之在您，但有許多線上資源可以使用。 

## <a name="other-links"></a>其他連結

* [使用 JavaScript 製作簡單的 Windows 遊戲](https://www.sitepoint.com/creating-a-simple-windows-8-game-with-javascript-game-basics-createjseaseljs/)
* [挑選 HTML/JS 遊戲引擎](https://html5gameengine.com/)
* [在以 JS 為基礎的遊戲中使用 CreateJS](https://blogs.msdn.microsoft.com/cbowen/2012/09/19/using-createjs-in-your-javascript-based-windows-8-game/)
* [LinkedIn Learning 上的遊戲開發課程](https://www.linkedin.com/learning/topics/game-development)
