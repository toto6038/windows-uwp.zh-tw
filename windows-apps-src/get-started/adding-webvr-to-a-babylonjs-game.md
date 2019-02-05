---
title: 新增 WebVR 至 3D Babylon.js game 的支援
description: 了解如何將 WebVR 支援新增至現有的 3D Babylon.js game。
ms.date: 11/29/2017
ms.topic: article
keywords: webvr，edge，web 開發、 巴比倫、 babylonjs、 babylon.js javascript
ms.localizationpriority: medium
ms.openlocfilehash: 1d8029752790e19adc5eb4266615372fb346e001
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2019
ms.locfileid: "9050105"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>新增 WebVR 至 3D Babylon.js game 的支援

如果您已經使用 Babylon.js 建立 3D 遊戲，並以為可能看起來更虛擬實境 (VR) 中，請在這個教學課程中進行，但實際上依照簡單的步驟。

我們將如下所示的遊戲中新增 WebVR 支援。 請繼續進行並插入 Xbox 控制器來試試看 ！


<iframe height='300' scrolling='no' title='使用 Babylon.GUI Babylon.js dino 遊戲' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱由 Microsoft Edge Docs 手寫筆<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>使用 Babylon.GUI Babylon.js dino 遊戲</a>(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 在<a href='https://codepen.io'>CodePen</a>。
</iframe>

這可以是關於在一般的畫面，但什麼可運作的 3D 遊戲中為 VR 嗎？
在本教學課程中，我們將逐步解說幾個步驟它會取得此和透過 WebVR 執行。 我們會使用[Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality)頭戴式裝置，可以點選到 WebVR 在 Microsoft Edge 中的已新增支援。 我們將這些變更套用至遊戲之後，您可以預期它也支援 WebVR 其他瀏覽器/耳機組合中搭配使用。



## <a name="prerequisites"></a>必要條件

- 文字編輯器 （例如[Visual Studio Code](https://code.visualstudio.com/download))
- Xbox 控制器插入到您的電腦
- Windows 10 Creators Update
- 有[執行 Windows Mixed Reality 的最小必要的規格](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)的電腦
- Windows Mixed Reality 裝置 （選用） 



## <a name="getting-started"></a>開始使用

若要開始使用的最簡單方式是造訪[Windows 教學課程 web GitHub 存放庫](https://github.com/Microsoft/Windows-tutorials-web)中，按下的綠色**複製或下載**按鈕，然後選取 **[在 Visual Studio 中開啟**。

![複製或下載按鈕](images/3dclone.png)

如果您不想複製專案，您可以 zip 檔案形式下載它。
這樣您就有兩個資料夾、[之前](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)和[之後](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after)。 「 之前 」 資料夾是我們的遊戲，才能加入任何 VR 功能，與 「 之後 」 資料夾是 VR 支援完成的遊戲。

之前與之後資料夾包含這些檔案：
-   **紋理 /** -包含遊戲中使用任何映像的資料夾。
-   **css /** -包含遊戲之 CSS 的資料夾。
-   **js /** -包含 JavaScript 檔案的資料夾。 Main.js 檔案是我們的遊戲和其他檔案是用的程式庫。
-   **模型 /** -包含 3D 模型的資料夾。 針對此遊戲中，我們已經只有一個模型中，用於恐龍的模型。
-   **index.html** -裝載遊戲的轉譯器的網頁。 在 Microsoft Edge 中開啟這個頁面會啟動遊戲。

您可以藉由在 Microsoft Edge 中開啟其各自的 index.html 檔案來測試遊戲的這兩個版本。



## <a name="the-mixed-reality-portal"></a>在混合的實境入口網站

如果您正在使用 Windows Mixed Reality 不熟悉，且具備相容的圖形卡，在電腦上安裝 Windows 10 Creators Update，請嘗試從 Windows 10 中的 [開始] 功能表開啟**混合實境入口網站**應用程式。

![混合的實境入口網站搜尋](images/mixed-reality-portal.png)

如果您滿足所有需求，然後可以開啟 [開發人員功能，並模擬 Windows Mixed Reality 頭戴式裝置插入到您的電腦。 如果您是幸運的是足以有附近，實際頭戴式裝置插入它，並執行安裝程式。

> [!IMPORTANT]
> 混合實境入口必須是在開啟所有的時間期間本教學課程。

現在，您準備好要體驗 WebVR 與 Microsoft Edge。

## <a name="2d-ui-in-a-virtual-world"></a>虛擬世界中的 2D UI

>[!NOTE]
> 抓取[**之前**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)資料夾，以取得入門範例。

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) VR 方便的程式庫，讓您建立簡單，互動式的使用者介面該適用於 VR 和非 VR 顯示。
一個延伸模組至 Babylon.js，`GUI`程式庫是使用的 throuhout 建立 2D 元素的範例。


2D 的文字`GUI`可利用幾行取決於您想要調整多少屬性建立項目。
下列程式碼片段是已經在我們[**之前**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)範例中，但是讓我們逐步解說發生了什麼。
我們先讓[`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture)建立 GUI 將涵蓋的物件。 此範例將此設定為`CreateFullScreenUI()`，這表示我們的 UI 將涵蓋整個螢幕。 使用`AdvancedDynamicTexture`建立，我們建立的然後啟動遊戲使用時出現的 2D 文字方塊`GUI.Rectanlge()`和`GUI.TextBlock()`。


在[**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168)中新增這個程式碼。
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


這個 UI 會顯示後建立，但是可以使用切換開啟或關閉`isVisible`根據遊戲中發生了什麼。
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>偵測耳機

它是不錯的做法，適用於 VR 應用程式，以便可以支援多個的案例有兩種類型的相機。 針對此遊戲，我們將會支援一個相機工作中，已連接耳機，另一個需要使用任何頭戴式裝置。 若要判斷哪一個遊戲會使用，我們必須先檢查以查看是否已偵測到耳機。 若要這樣做，我們會使用[`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays)。


新增此程式碼上述`window.addEventListener('DOMContentLoaded')` **main.js**中。
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

儲存在其資訊`headset`變數，我們將現在能夠選擇最適合使用者的相機。


## <a name="creating-and-selecting-the-initial-camera"></a>建立和選取初始相機

使用 Babylon.js、 WebVR 可以新增快速藉由使用[`WebVRFreeCamera`](https://doc.babylonjs.com/classes/3.1/webvrfreecamera)。 這個相機可以使用鍵盤輸入，並可讓您使用 VR 耳機來控制您的 「 head 」 旋轉。


### <a name="step-1-checking-for-headsets"></a>步驟 1： 檢查耳機

我們後援的相機，我們會使用[`UniversalCamera`](https://doc.babylonjs.com/classes/3.1/universalcamera)，目前使用原始的遊戲中。

我們會檢查我們`headset`變數，以判斷我們是否可以使用`WebVRFreeCamera`相機。

取代`camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);`具有下列程式碼。
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


### <a name="step-2-activating-the-webvrfreecamera"></a>步驟 2： 啟用 WebVRFreeCamera
若要啟用此相機在大部分的瀏覽器中，使用者必須執行一些要求虛擬體驗的互動。
我們會將這項功能，最多滑鼠按一下。


貼上內的程式碼`createScene()`函式之後`camera.applyGravity = true;`。
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

在遊戲中的按一下現在會建立下列項目，例如在提示字元或顯示遊戲頭戴式裝置中立即如果使用者已經接受之前的提示。

![身歷其境的命令提示字元](images/immersiveview.png)

我們也可以新增一段程式碼，將會顯示`UniversalCamera`我們切換到之前，先檢視我們`WebVRFreeCamera`，讓使用者能看看遊戲，而不是藍色視窗。 

新增下列項目之後`engine.runRenderLoop(function () {`。
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

### <a name="step-3-adding-gamepad-support"></a>步驟 3： 新增遊戲台支援

因為`WebVRFreeCamera`一開始不支援遊戲台，我們會將我們的遊戲台按鈕對應到鍵盤方向鍵。 我們將會執行此動作的深入`inputs`相機的屬性。 藉由新增左邊類比搖桿的對應程式碼，向上、 向下、 左、 和右方向鍵和相符，我們的遊戲台是回情形。


新增此程式碼下方`scene.onPointerDown = function() {...}`呼叫。
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>步驟 4： 試試看 ！

如果我們會使用我們的頭戴式裝置開啟**index.html** ，且電源遊戲控制器，藍色遊戲視窗上的按一下左會為 VR 模式切換我們的遊戲 ！ 請繼續進行並置於耳機，以查看結果。 


<iframe height='300' scrolling='no' title='使用 Babylon.GUI-WebVR Babylon.js dino 遊戲' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱由 Microsoft Edge Docs 手寫筆<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>使用 Babylon.GUI-WebVR Babylon.js dino 遊戲</a>(<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 在<a href='https://codepen.io'>CodePen</a>。
</iframe>


## <a name="conclusion"></a>總結

恭喜！ 您現在可以完整 Babylon.js game WebVR 支援。 接下來，您可以採取您已經學會建置更好的遊戲，或關閉這一個組建。
