---
title: 新增 WebVR 支援至 3D Babylon.js 遊戲
description: 了解如何新增 WebVR 支援到現有的 3D Babylon.js 遊戲。
ms.date: 11/29/2017
ms.topic: article
keywords: webvr, edge, web development, babylon, babylonjs, babylon.js, javascript, web 開發
ms.localizationpriority: medium
ms.openlocfilehash: 1d8029752790e19adc5eb4266615372fb346e001
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638553"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>新增 WebVR 支援至 3D Babylon.js 遊戲

如果您使用 Babylon.js 建立 3D 遊戲，並覺得在虛擬實境 (VR) 中可能會看起來更好，則請依照本教學課程中的簡單步驟來實現這個想法。

我們將會新增 WebVR 支援到此處所示的遊戲。 請繼續並接上 Xbox 控制器試試看！


<iframe height='300' scrolling='no' title='使用 Babylon.GUI Babylon.js dino 遊戲' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱畫筆<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon.js dino 遊戲使用 Babylon.GUI</a>透過 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 上<a href='https://codepen.io'>CodePen</a>。
</iframe>

這是在平面螢幕上運作不錯的 3D 遊戲，但是在 VR 中如何呢？
本教學課程中，我們會逐步進行幾個步驟建立並使用 WebVR 執行。 我們將使用 [Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality) 耳機，可在 Microsoft Edge 中點選進入新增的 WebVR 支援。 在我們套用這些變更到遊戲後，您可以預期它也可在支援 WebVR 的其他瀏覽器/耳機組合中運作。



## <a name="prerequisites"></a>必要條件

- 文字編輯器 (例如 [Visual Studio Code](https://code.visualstudio.com/download))
- 已接到您的電腦的 Xbox 控制器
- Windows 10 Creators Update
- 具有[執行 Windows Mixed Reality 的最低需求規格](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)的電腦
- Windows Mixed Reality 裝置 (選擇性) 



## <a name="getting-started"></a>開始使用

入門的最簡單方式是造訪 [Windows-tutorials-web GitHub 存放庫](https://github.com/Microsoft/Windows-tutorials-web)，按下綠色 **\[Clone or download\]** (複製或下載) 按鈕，然後選取 **\[在 Visual Studio 中開啟\]**。

![複製或下載按鈕](images/3dclone.png)

如果您不想複製專案，您可以 zip 檔案形式下載它。
然後，您將有兩個資料夾，分別為 [before](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) 和 [after](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after)。 「before」資料夾是新增任何 VR 功能之前的遊戲，而「after」資料夾則是具有 VR 支援的已完成遊戲。

before 和 after 資料夾包含下列檔案︰
-   **紋理 /** -包含遊戲中使用的任何映像的資料夾。
-   **css /** -包含遊戲的 CSS 資料夾。
-   **js /** -包含 JavaScript 檔案的資料夾。 main.js 檔案是我們的遊戲，而其他檔案則是使用的程式庫。
-   **模型 /** -包含 3D 模型的資料夾。 對於這個遊戲，我們只有一個用於恐龍的模型。
-   **index.html** -裝載遊戲的轉譯器的網頁。 在 Microsoft Edge 中開啟此頁面會啟動遊戲。

您可以藉由在 Microsoft Edge 中開啟其各自的 index.html 檔案來測試兩個版本的遊戲。



## <a name="the-mixed-reality-portal"></a>混合實境入口

如果您熟悉 Windows Mixed Reality，而且具有相容圖形卡的電腦上安裝有 Windows 10 Creators Update，請嘗試從 Windows 10 中的 [開始] 功能表開啟**混合實境入口** App。

![混合實境入口搜尋](images/mixed-reality-portal.png)

如果您符合所有要求，則可以開啟開發人員功能，然後模擬接到您電腦的 Windows Mixed Reality 耳機。 如果您是夠幸運附近剛好有實際的耳機，請接上並執行設定。

> [!IMPORTANT]
> 混合實境入口在本教學課程中都必須一直開著。

您現在可以開始體驗 WebVR 使用 Microsoft Edge。

## <a name="2d-ui-in-a-virtual-world"></a>虛擬世界中的 2D UI

>[!NOTE]
> 抓取[**之前**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)資料夾，以取得入門範例。

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui)是 VR 友善程式庫，讓您建立簡單的互動式的使用者介面，都適用於 VR-VR 顯示。
Babylon.js，延伸模組的`GUI`程式庫是使用的 throuhout 範例，以建立 2D 的項目。


2D 文字`GUI`項目可以使用幾行，根據您想要調整的幾個屬性來建立。
下列程式碼片段已在我們[**之前**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)範例，但讓我們逐步解說中發生的事情。
我們首先請[ `AdvancedDynamicTexture` ](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture)物件來建立 GUI 將涵蓋。 此範例將此值設`CreateFullScreenUI()`，這表示我們的 UI 會涵蓋整個畫面。 具有`AdvancedDynamicTexture`建立，接著我們，讓啟動遊戲使用時出現的 2D 文字方塊`GUI.Rectanlge()`和`GUI.TextBlock()`。


此程式碼中會新增[ **main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168)。
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


此 UI 會顯示一旦建立，但是可以使用它開啟或關閉`isVisible`根據遊戲中的情況。
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>偵測耳機

它是理想的做法，以便可以支援多個案例有兩種類型的數位相機的 VR 應用程式。 對於這個遊戲，我們將會支援一個相機，需要可插入工作耳機，以及另一個不使用任何耳機。 若要判斷遊戲會使用哪一個，我們必須先檢查是否偵測到耳機。 若要這樣做，我們將使用[ `navigator.getVRDisplays()` ](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays)。


新增下列程式碼上述`window.addEventListener('DOMContentLoaded')`中**main.js**。
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


## <a name="creating-and-selecting-the-initial-camera"></a>建立並選取初始鏡頭

使用 Babylon.js，WebVR 可以快速地使用新增[ `WebVRFreeCamera` ](https://doc.babylonjs.com/classes/3.1/webvrfreecamera)。 此相機可以接受鍵盤輸入並可讓您使用 VR 眼鏡控制您的「頭部」旋轉。


### <a name="step-1-checking-for-headsets"></a>步驟 1：檢查耳機

如我們後援的相機，我們將使用[ `UniversalCamera` ](https://doc.babylonjs.com/classes/3.1/universalcamera) ，目前使用於原始的遊戲。

我們將查看我們`headset`變數，以判斷是否我們可以使用`WebVRFreeCamera`相機。

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


### <a name="step-2-activating-the-webvrfreecamera"></a>步驟 2：啟用 WebVRFreeCamera
若要在大多數瀏覽器中啟動相機，使用者必須執行一些要求虛擬體驗的互動。
我們會將連結至滑鼠點選的這項功能。


貼上 `createScene()` 中的程式碼在 `camera.applyGravity = true;` 之後。
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

遊戲中的按一下 立即建立如下所示，在提示字元，或顯示遊戲的耳機立即如果使用者已接受前的提示。

![身歷其境的提示](images/immersiveview.png)

我們也可以加入程式碼，將會顯示一段`UniversalCamera`檢視之前我們切換到我們`WebVRFreeCamera`，讓使用者查看的遊戲，而不是藍色的視窗。 

後面新增下列`engine.runRenderLoop(function () {`。
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

因為`WebVRFreeCamera`最初並不支援遊戲板，我們會將遊戲台按鈕對應至鍵盤方向鍵。 將執行此作業，探討`inputs`觀景窗的屬性。 為左類比搖桿向上、向下、向左和向右，新增相對應的程式碼，以配合方向鍵，我們的遊樂台又能動作了。


新增以下這段程式碼`scene.onPointerDown = function() {...}`呼叫。
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>步驟 4：試試看 ！

如果我們開啟**index.html**透過我們的耳機及遊戲控制器插入，藍色的遊戲視窗的左側的按一下會為 VR 模式切換我們遊戲 ！ 請繼續進行，並戴上您的眼鏡查看結果。 


<iframe height='300' scrolling='no' title='使用 Babylon.GUI-WebVR Babylon.js dino 遊戲' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱畫筆<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon.js dino 遊戲使用 Babylon.GUI-WebVR</a>透過 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 上<a href='https://codepen.io'>CodePen</a>。
</iframe>


## <a name="conclusion"></a>結論

恭喜！ 現在，您可以使用 WebVR 支援完成 Babylon.js 遊戲。 由此，您可以將學到的用來打造更好的遊戲，或關閉此組建。
