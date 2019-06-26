---
title: 新增 WebVR 支援至 3D Babylon.js 遊戲
description: 了解如何新增 WebVR 支援到現有的 3D Babylon.js 遊戲。
ms.date: 11/29/2017
ms.topic: article
keywords: webvr, edge, web development, babylon, babylonjs, babylon.js, javascript, web 開發
ms.localizationpriority: medium
ms.openlocfilehash: 1d8029752790e19adc5eb4266615372fb346e001
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "63798205"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>新增 WebVR 支援至 3D Babylon.js 遊戲

如果您使用 Babylon.js 建立 3D 遊戲，並覺得在虛擬實境 (VR) 中可能會看起來更好，則請依照此教學課程中的簡單步驟來實現這個想法。

我們將會新增 WebVR 支援到此處所示的遊戲。 請繼續並接上 Xbox 控制器試試看！


<iframe height='300' scrolling='no' title='使用 Babylon.GUI 的 Babylon.js dino 遊戲' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱由 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 在 <a href='https://codepen.io'>CodePen</a> 上發佈的 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>使用 Babylon.GUI 的 Babylon.js dino 遊戲</a>。
</iframe>

這是在平面螢幕上運作不錯的 3D 遊戲，但是在 VR 中如何呢？
在此教學課程中，我們會逐步進行幾個步驟建立並使用 WebVR 執行。 我們將使用 [Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality) 頭戴式裝置，可在 Microsoft Edge 中點選進入新增的 WebVR 支援。 在我們套用這些變更到遊戲後，您可以預期它也可在支援 WebVR 的其他瀏覽器/頭戴式裝置組合中運作。



## <a name="prerequisites"></a>必要條件

- 文字編輯器 (例如 [Visual Studio Code](https://code.visualstudio.com/download))
- 已接到您的電腦的 Xbox 控制器
- Windows 10 Creators Update
- 具有[執行 Windows Mixed Reality 的最低需求規格](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)的電腦
- Windows Mixed Reality 裝置 (選用) 



## <a name="getting-started"></a>開始使用

入門的最簡單方式是造訪 [Windows-tutorials-web GitHub 存放庫](https://github.com/Microsoft/Windows-tutorials-web)，按下綠色 [複製或下載]  按鈕，然後選取 [在 Visual Studio 中開啟]  。

![[複製或下載] 按鈕](images/3dclone.png)

如果您不想複製專案，您可以 zip 檔案形式下載它。
然後，您將有兩個資料夾，分別為 [before](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) 和 [after](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after)。 「before」資料夾是新增任何 VR 功能之前的遊戲，而「after」資料夾則是具有 VR 支援的已完成遊戲。

before 和 after 資料夾包含下列檔案︰
-   **textures/** - 包含遊戲中所使用的任何影像的資料夾。
-   **css/** - 包含遊戲 CSS 的資料夾。
-   **js/** - 包含 JavaScript 檔案的資料夾。 main.js 檔案是我們的遊戲，而其他檔案則是使用的程式庫。
-   **models/** - 包含 3D 模型的資料夾。 對於這個遊戲，我們只有一個用於恐龍的模型。
-   **index.html** - 裝載遊戲轉譯器的網頁。 在 Microsoft Edge 中開啟此頁面會啟動遊戲。

您可以在 Microsoft Edge 中開啟其各個 index.html 檔案來測試遊戲的兩個版本。



## <a name="the-mixed-reality-portal"></a>混合實境入口

如果您熟悉 Windows Mixed Reality，而且具有相容圖形卡的電腦上安裝有 Windows 10 Creators Update，請嘗試從 Windows 10 中的 [開始] 功能表開啟 [混合實境入口]  應用程式。

![混合實境入口搜尋](images/mixed-reality-portal.png)

如果您符合所有要求，則可以開啟開發人員功能，然後模擬接到您電腦的 Windows Mixed Reality 頭戴式裝置。 如果您是夠幸運附近剛好有實際的頭戴式裝置，請接上並執行設定。

> [!IMPORTANT]
> 混合實境入口在此教學課程中都必須一直開著。

您現在可以使用 Microsoft Edge 體驗 WebVR。

## <a name="2d-ui-in-a-virtual-world"></a>虛擬世界中的 2D UI

>[!NOTE]
> 抓取 [**before**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) 資料夾以取得入門範例。

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) 是一個 VR 友善程式庫，讓您能夠建立簡單、互動式的使用者介面，適用於 VR 和非 VR 顯示器。
作為 Babylon.js 的延伸模組，在整個範例中使用 `GUI` 程式庫來建立 2D 元素。


可以使用幾行來建立 2D 文字 `GUI` 元素，具體取決於您想要調整的屬性數量。
下列程式碼片段已存在我們的 [**before**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) 範例中，但讓我們逐步解說發生了什麼事情。
我們首先建立一個 [`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) 物件來確定 GUI 將涵蓋的內容。 該範例將此設定為 `CreateFullScreenUI()`，這表示我們的 UI 將涵蓋整個畫面。 建立 `AdvancedDynamicTexture` 後，我們會建立一個 2D 文字方塊，它會在使用 `GUI.Rectanlge()` 和 `GUI.TextBlock()` 開始遊戲時出現。


此程式碼已新增至 [**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168) 中。
```javascript
// GUI
var advancedTexture = BABYLON.GUI.AdvancedDynamicTexture.CreateFullscreenUI("UI");

// Start UI
startUI = new BABYLON.GUI.Rectangle("start");
startUI.background = "black"
startUI.alpha = .8;
startUI.thickness = 0;
startUI.height = "60px";
startUI.width = "400px";
advancedTexture.addControl(startUI); 
var tex2 = new BABYLON.GUI.TextBlock();
tex2.text = "Stay away from the dinosaur! \n Plug in an Xbox controller and press A to start";
tex2.color = "white";
startUI.addControl(tex2); 
```


此 UI 會在建立後顯示，但可以使用 `isVisible` 開啟或關閉，具體取決於遊戲中的情況。
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>偵測頭戴式裝置

對於 VR 應用程式這是很好的做法，有兩種類型的相機，以便可支援多個案例。 對於這個遊戲，我們將會支援一個相機，需要可插入工作頭戴式裝置，以及另一個不使用任何頭戴式裝置。 若要判斷遊戲會使用哪一個，我們必須先檢查是否偵測到頭戴式裝置。 若要這樣做，我們將使用 [`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays)。


在 **main.js** 中的 `window.addEventListener('DOMContentLoaded')` 上方新增此程式碼。
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

透過儲存在 `headset` 變數中的資訊，我們現在可以選擇最適合使用者的相機。


## <a name="creating-and-selecting-the-initial-camera"></a>建立並選取初始相機

使用 Babylon.js，WebVR 可透過使用 [`WebVRFreeCamera`](https://doc.babylonjs.com/classes/3.1/webvrfreecamera) 快速加入。 此相機可以接受鍵盤輸入並可讓您使用 VR 頭戴式裝置控制您的「頭部」旋轉。


### <a name="step-1-checking-for-headsets"></a>步驟 1：檢查頭戴式裝置

對於我們的遞補相機，我們會目前原始遊戲中使用的 [`UniversalCamera`](https://doc.babylonjs.com/classes/3.1/universalcamera)。

我們將會檢查我們 `headset` 變數，以判斷是否可以使用 `WebVRFreeCamera` 相機。

以下列程式碼取代 `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);`。
```javascript
        if(headset){
            // Create a WebVR camera with the trackPosition property set to false so that we can control movement with the gamepad
            camera = new BABYLON.WebVRFreeCamera("vrcamera", new BABYLON.Vector3(0, 14, 0), scene, true, { trackPosition: false });
            camera.deviceScaleFactor = 1;
        } else {
            // No headset, use universal camera
            camera = new BABYLON.UniversalCamera("camera", new BABYLON.Vector3(0, 18, -45), scene);
        }
```


### <a name="step-2-activating-the-webvrfreecamera"></a>步驟 2：啟動 WebVRFreeCamera
若要在大多數瀏覽器中啟動相機，使用者必須執行一些要求虛擬體驗的互動。
我們將這個功能與按一下滑鼠連結。


在 `camera.applyGravity = true;` 之後，於 `createScene()` 函式內貼上程式碼。
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

現在遊戲中按一下會建立如下列的提示，或者如果使用者之前已接受提示，會立即在頭戴式裝置中顯示遊戲。

![沈浸式提示](images/immersiveview.png)

我們也可以加入一段程式碼，在切換到我們的 `WebVRFreeCamera` 之前顯示 `UniversalCamera` 檢視，允許使用者查看遊戲而不是藍色視窗。 

在 `engine.runRenderLoop(function () {` 之後新增下列值。
```javascript
            if (headset) {
                if (!(headset.isPresenting)) {
                    var camera2 = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);
                    scene.activeCamera = camera2;
                } else {
                    scene.activeCamera = camera;
                }
            }
```

### <a name="step-3-adding-gamepad-support"></a>步驟 3：新增遊戲台支援

因為 `WebVRFreeCamera` 最初不支援遊戲台，我們會將遊戲台按鈕對應鍵盤方向鍵。 我們會深入相機的 `inputs` 屬性來進行此動作。 為左類比搖桿向上、向下、向左和向右，新增相對應的程式碼，以配合方向鍵，我們的遊戲台又能動作了。


在 `scene.onPointerDown = function() {...}` 呼叫下方新增此程式碼。
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>步驟 4：試試看吧！

如果我們使用頭戴式裝置開啟 **index.html** 且遊戲控制器有插入，在藍色遊戲視窗上按下左鍵會將我們的遊戲切換為 VR 模式！ 請繼續進行，並戴上您的頭戴式裝置查看結果。 


<iframe height='300' scrolling='no' title='使用 Babylon.GUI 的 Babylon.js dino 遊戲 - WebVR' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱由 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 在 <a href='https://codepen.io'>CodePen</a> 上發佈的 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>使用 Babylon.GUI 的 Babylon.js dino 遊戲 - WebVR</a>。
</iframe>


## <a name="conclusion"></a>結論

恭喜！ 現在，您可以使用 WebVR 支援完成 Babylon.js 遊戲。 由此，您可以將學到的用來打造更好的遊戲，或關閉此組建。
