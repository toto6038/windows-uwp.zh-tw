---
title: 新增 3D Babylon.js 遊戲 WebVR 支援
description: 了解如何將 WebVR 支援新增至現有的 3D Babylon.js 遊戲。
author: abbycar
ms.author: abigailc
ms.date: 11/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: webvr、 edge、 web 開發、 巴比倫、 babylonjs、 babylon.js、 javascript
ms.localizationpriority: medium
ms.openlocfilehash: 41665e8719493bb658f9926947061b1b5f81a139
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "1018648"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>新增 3D Babylon.js 遊戲 WebVR 支援

如果您已建立 3D 遊戲與 Babylon.js 思考可能看起來更好的虛擬現實 (VR) 進行的現實本教學課程中遵循的簡單的步驟。

我們將新增 WebVR 支援以這裡顯示的遊戲。 繼續並插入至試試 Xbox 控制器 ！


<iframe height='300' scrolling='no' title='使用 Babylon.GUI Babylon.js dino 遊戲' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱 Microsoft Edge 文件的畫筆<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon.js dino 遊戲使用 Babylon.GUI</a> (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) <a href='https://codepen.io'>CodePen</a>上。
</iframe>

這是適用於妥善平面] 畫面中，但功能的相關 3D 遊戲中 VR？
在本教學課程中，我們將逐步解說幾個步驟加以取得取得此並執行與 WebVR。 我們將使用便能感受到的 Microsoft Edge WebVR 新增支援[Windows 混合現實](https://developer.microsoft.com/en-us/windows/mixed-reality)耳機。 我們將這些變更套用至遊戲之後，您可預期它也支援 WebVR 其他瀏覽器/耳機組合中運作。



## <a name="prerequisites"></a>必要條件

- 文字編輯器 （例如[Visual Studio 程式碼](https://code.visualstudio.com/download)）
- 會您的電腦插入擴充座 Xbox 控制器
- Windows 10 Creators Update
- [若要執行 Windows 混合現實的最小必要的規格](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)電腦
- Windows 混合現實裝置 （選用） 



## <a name="getting-started"></a>開始使用

若要開始的最簡單方式是以造訪[Windows 教學課程網站 GitHub repo](https://github.com/Microsoft/Windows-tutorials-web)中，按綠色**複製或下載**] 按鈕，然後選取 [**在 Visual Studio 中開啟**。

![複製或下載按鈕](images/3dclone.png)

如果您不想複製專案，您可以 zip 檔案形式下載它。
然後需要兩個資料夾、[之前](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)和[之後](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after)。 "Before"資料夾是我們遊戲之前新增任何 VR 功能，且"之後"資料夾 VR 支援完成的遊戲。

前與後資料夾包含下列檔案：
-   **材質 /** -內含所使用的遊戲任何圖像的資料夾。
-   **css /** -內含的遊戲 CSS 的資料夾。
-   **js /** -內含 JavaScript 檔案的資料夾。 Main.js 檔案是我們遊戲，及其他檔案所使用的文件庫。
-   **模型 /** -內含 3D 模型的資料夾。 針對此遊戲我們必須只有一個模型、 恐龍。
-   **index.html** -主控的遊戲轉譯器的網頁。 在 Microsoft Edge 中開啟此頁面啟動的遊戲。

您可以藉由開啟其各自的 index.html 檔案中 Microsoft Edge 測試兩個版本的遊戲。



## <a name="the-mixed-reality-portal"></a>混合的現實入口網站

如果您還不熟悉 Windows 混合現實且 Windows 10 建立者更新安裝電腦上的相容的圖形卡，嘗試從 Windows 10] 中的 [開始] 功能表中開啟**混合現實入口網站**應用程式。

![混合的現實入口網站搜尋](images/mixed-reality-portal.png)

如果您符合所有需求，您可以再開啟開發人員功能和模擬 Windows 混合現實耳機插入擴充座您的電腦。 如果您是真足夠讓附近實際耳機，它插與執行安裝程式。

> [!IMPORTANT]
> 混合的現實入口網站，必須開啟在所有的時間期間本教學課程。

您現在準備好體驗 WebVR Microsoft 邊緣。

## <a name="2d-ui-in-a-virtual-world"></a>在虛擬世界 2D UI

>[!NOTE]
> 取得[**之前**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)資料夾取得 starter 範例。

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) VR 更容易文件庫，讓您建立簡單、 互動式的使用者介面 （英文） 適用於 VR 該工作及非 VR 會顯示。
Babylon.js、 延伸模組的`GUI`文件庫會使用的 throuhout 建立 2D 項目範例。


2D 文字`GUI`元素可以建立根據您想要調整的多少屬性的幾行。
下列程式碼片段是已在我們[**之前**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)範例中，但我們有什麼新鮮事逐步解說。
我們先進行[`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture)建立 GUI 會涵蓋的物件。 本範例將此值設`CreateFullScreenUI()`，這表示我們 UI 會涵蓋整個螢幕。 使用`AdvancedDynamicTexture`建立，我們然後進行 2D 文字] 方塊中出現時啟動遊戲使用`GUI.Rectanlge()`和`GUI.TextBlock()`。


這段程式碼會新增[**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168)內。
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


此 UI 看之後建立，但可使用切換開啟或關閉`isVisible`根據近日遊戲中。
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>偵測耳機

它是很好的作法，讓可支援多個案例有兩種類型的照相機 VR 應用程式。 針對此遊戲，我們將支援一個相機工作耳機，插入與其他組織所需使用任何耳機。 若要決定哪一個遊戲會使用，我們必須先檢查以查看是否已偵測到耳機。 若要這樣做，我們將使用[`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays)。


新增上述這段程式碼`window.addEventListener('DOMContentLoaded')` **main.js**中。
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

使用中儲存的資訊`headset`變數，我們現在可以選擇最適合使用者的相機。


## <a name="creating-and-selecting-the-initial-camera"></a>建立和選取初始相機

使用 Babylon.js、 WebVR 可以新增快速使用[`WebVRFreeCamera`](http://doc.babylonjs.com/classes/3.1/webvrfreecamera)。 此相機約鍵盤輸入與可讓您使用 VR 耳機以控制 「 標題 」 旋轉角度。


### <a name="step-1-checking-for-headsets"></a>步驟 1： 檢查耳機

針對我們後援攝影機，我們將使用[`UniversalCamera`](https://doc.babylonjs.com/classes/3.1/universalcamera)的目前使用中的原始的遊戲。

我們將檢查我們`headset`變數來判斷是否可以使用我們`WebVRFreeCamera`相機。

取代`camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);`與下列程式碼。
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


### <a name="step-2-activating-the-webvrfreecamera"></a>步驟 2： 啟動 WebVRFreeCamera
若要啟動此相機大多數的瀏覽器中，使用者必須執行某些要求虛擬經驗的互動。
我們將連接資訊至按滑鼠動作的這項功能。


貼上的程式碼內`createScene()`後運作`camera.applyGravity = true;`。
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

遊戲中的按一下 [立即建立類似下列提示或遊戲中顯示耳機立即如果使用者已接受之前先提示。

![沈浸式提示](images/immersiveview.png)

我們也可以新增程式碼會顯示一段`UniversalCamera`檢視之前我們切換到我們`WebVRFreeCamera`，讓使用者可以查看而不是藍色視窗的遊戲。 

新增後的下列`engine.runRenderLoop(function () {`。
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

由於`WebVRFreeCamera`最初不支援遊戲板，我們將鍵盤方向鍵以對應我們遊戲台] 按鈕。 我們將由深入進行`inputs`相機的屬性。 向上、 向下、 左、 以及從右向符合下方向鍵以新增的左類比固定相對應的代碼，我們遊戲台是在 [動作。


新增下列這段程式碼`scene.onPointerDown = function() {...}`呼叫。
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>步驟 4： 試試看 ！

如果我們開啟**index.html**我們耳機，遊戲控制器插入擴充座藍色的遊戲視窗上左的 click 會切換我們遊戲 VR 模式 ！ 繼續並放在您耳機取出結果。 


<iframe height='300' scrolling='no' title='使用 Babylon.GUI-WebVR Babylon.js dino 遊戲' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱 Microsoft Edge 文件的畫筆<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon.js dino 遊戲使用 Babylon.GUI-WebVR</a> (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) <a href='https://codepen.io'>CodePen</a>上。
</iframe>


## <a name="conclusion"></a>總結

恭喜！ 您現在已完成 Babylon.js 遊戲 WebVR 支援。 從這裡您可能需要您已建立更好的遊戲或關閉此建置學會。
