---
title: 入門教學課程 - 以 JavaScript 撰寫的 3D UWP 遊戲
description: 用於 Microsoft Store，以 JavaScript 搭配 three.js 撰寫的 UWP 遊戲
author: abbycar
ms.author: abigailc
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fb4249b2-f93c-4993-9e4d-57a62c04be66
ms.localizationpriority: medium
ms.openlocfilehash: 0183e19135758f73dfea9b63535437ff9b66011a
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5594182"
---
# <a name="creating-a-3d-javascript-game-using-threejs"></a>使用 three.js 建立 3D JavaScript 遊戲

## <a name="introduction"></a>簡介

對於 Web 開發人員或 JavaScript 技術達人 (Tinkerer)，以 JavaScript 開發 UWP app 可輕鬆向世人展示您的應用程式。 不用擔心學習 C# 或 C++ 等語言！

在這個範例，我們會利用 **three.js** 程式庫。 從 WebGL (用於針對網頁瀏覽器轉譯 2D 與 3D 圖形的 API) 建置這個程式庫。 **three.js** 將此複雜的 API 簡化，讓 3D 開發變得更簡單。 


進一步閱讀之前，想要一窺究竟我們將建立的應用程式嗎？ 在 CodePen 查看！

<iframe height='300' scrolling='no' title='Dino 遊戲' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpKejy/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱由 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 發佈到 <a href='https://codepen.io'>CodePen</a> 的 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpKejy/'>Dino 遊戲完稿</a>。
</iframe>

> [!NOTE] 
> 這並非完整的遊戲;設計目的是示範使用 JavaScript 和協力廠商程式庫，建立可發佈至 Microsoft Store 的 app。


## <a name="requirements"></a>需求

若要試用這個專案，您將需要下列各項：
-   執行 Windows 10 目前版本的 Windows 的電腦（或虛擬機器）。
-   一份 Visual Studio 複本。 您可以從 [Visual Studio 首頁](http://visualstudio.com/)下載免費的 Visual Studio Community Edition。
這個專案使用 **three.js** JavaScript 程式庫。 **three.js** 是根據 MIT 授權發行。 這個程式庫已經在專案中 (在方案總管檢視中尋找 `js/libs`)。 您可以在 [**three.js**](https://threejs.org/) 首頁找到這個程式庫的詳細資訊。

## <a name="getting-started"></a>開始使用

app 的完整原始碼儲存在 [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js3d)。

瀏覽 GitHub、按一下綠色 \[Clone or download\] (複製或下載) 按鈕，然後選取 \[在 Visual Studio 中開啟\]，是最簡單的入門使用方式。 

![複製或下載按鈕](images/3dclone.png)

如果您不想複製專案，您可以 zip 檔案形式下載它。
一旦方案載入到 Visual Studio 之後，您會看到數個檔案，包括：
-   Images/ - 包含 UWP app 所需各種圖示的資料夾。
- css/ - 包含要使用之 CSS 的資料夾。
-   js/ - 包含 JavaScript 檔案的資料夾。 main.js 檔案是我們的遊戲，其他檔案則是協力廠商程式庫。
-   models/ - 包含 3D 模型的資料夾。 在這個遊戲，我們只有一個用於恐龍的模型。
-   index.html - 裝載遊戲轉譯器的網頁。

現在您可以執行遊戲！

按 F5 來啟動 app。 您應該會看到視窗開啟，提示您按一下螢幕。 您也會看到恐龍在背景中四處移動。 關閉遊戲，我們將開始檢查 App 及其關鍵元件。

> [!NOTE] 
> 出現錯誤？ 請務必安裝有 Web 支援的 Visual Studio。 您可以建立新的專案進行檢查，如果沒有 JavaScript 支援，您必須重新安裝 Visual Studio，並核取 \[Microsoft Web Developer Tools\] 方塊。

## <a name="walkthrough"></a>逐步解說

當您開始這個遊戲時，您會看到按一下螢幕的提示。 [指標鎖定 API](https://developer.mozilla.org/docs/Web/API/Pointer_Lock_API) 可讓您用滑鼠四處查看。 按 W、A、S、D/方向鍵來進行移動。
此遊戲的目標是遠離恐龍。 一旦恐龍夠靠近，它就會開始追您，直到您在範圍之外或太接近並輸掉遊戲。

### <a name="1-setting-up-your-initial-html-file"></a>1. 設定您的初始 HTML 檔案

在 **index.html**，您將需要新增一些 HTML 開始。 這個檔案是包含我們的 app 的預設網頁。

現在，我們將會以要使用的程式庫和用來將圖形轉譯到的 `div` (名為 `container`) 來設定它。 我們也會將它設為指向 **main.js**（遊戲程式碼）。


```html
<!DOCTYPE html>
<html lang='en'>

<head>
    <link rel="stylesheet" type="text/css" href="css/stylesheet.css" />
</head>

    <body>
        <div id='container'></div>
        <script src='js/libs/three.js'></script>
        <script src="js/controls/PointerLockControls.js"></script>
        <script src="js/main.js"></script>
    </body>

</html>
```


我們已經擁有準備就緒的簡易版 HTML，讓我們前往 **main.js**，建立一些圖形！

### <a name="2-creating-your-scene"></a>2. 建立您的場景

在逐步解說一節中，我們要新增遊戲的基礎。

我們將會從實現 `scene` 開始。 **three.js** 中的 `scene` 是相機、物件和燈號將加入的位置。 您也需要轉譯器，取得相機看到的場景，並讓它顯示。

在 **main.js**，我們建立會執行這一切的函式，稱為 `init()`，它在一些其他函式上呼叫：

```javascript
var UNITWIDTH = 90; // Width of a cubes in the maze
var UNITHEIGHT = 45; // Height of the cubes in the maze

var camera, scene, renderer;

init();
animate();

function init() {
    // Create the scene where everything will go
    scene = new THREE.Scene();

    // Add some fog for effects
    scene.fog = new THREE.FogExp2(0xcccccc, 0.0015);

    // Set render settings
    renderer = new THREE.WebGLRenderer();
    renderer.setClearColor(scene.fog.color);
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);

    // Get the HTML container and connect renderer to it
    var container = document.getElementById('container');
    container.appendChild(renderer.domElement);

    // Set camera position and view details
    camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 1, 2000);
    camera.position.y = 20; // Height the camera will be looking from
    camera.position.x = 0;
    camera.position.z = 0;

    // Add the camera
    scene.add(camera);

    // Add the walls(cubes) of the maze
    createMazeCubes();

    // Add lights to the scene
    addLights();

    // Listen for if the window changes sizes and adjust
    window.addEventListener('resize', onWindowResize, false);
}

```

我們將需要建立的其他函式包括：
- `createMazeCubes()`
- `addLights()`
- `onWindowResize()`
- `animate()` / `render()`
- 單位轉換函式

#### <a name="createmazecubes"></a>createMazeCubes()

`createMazeCubes()` 函式將簡單的立方體加入至我們的場景。 稍後我們會讓這個函式將許多立方體加入至迷宮。

```javascript
function createMazeCubes() {

  // Make the shape of the cube that is UNITWIDTH wide/deep, and UNITHEIGHT tall
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  // Make the material of the cube and set it to blue
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });
  
  // Combine the geometry and material to make the cube
  var cube = new THREE.Mesh(cubeGeo, cubeMat);

  // Add the cube to the scene
  scene.add(cube);

  // Update the cube's position
  cube.position.y = UNITHEIGHT / 2;
  cube.position.x = 0;
  cube.position.z = -100;
  // rotate the cube by 30 degrees
  cube.rotation.y = degreesToRadians(30);
}

```

#### <a name="addlights"></a>addLights()

`addLights()` 是簡單的函式，將我們的建立燈號群組並將其新增至場景。

```javascript
function addLights() {
  var lightOne = new THREE.DirectionalLight(0xffffff);
  lightOne.position.set(1, 1, 1);
  scene.add(lightOne);

  // Add a second light with half the intensity
  var lightTwo = new THREE.DirectionalLight(0xffffff, .5);
  lightTwo.position.set(1, -1, -1);
  scene.add(lightTwo);
}
```

#### <a name="onwindowresize"></a>onWindowResize()

每當我們的事件接聽程式聽到 `resize` 事件引發，就會呼叫 `onWindowResize` 函式。 當使用者調整視窗大小，發生此事件。 如果發生此事件，我們會想要確保影像依比例調整大小，而且在整個視窗中都看得到。

```javascript
function onWindowResize() {

  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();

  renderer.setSize(window.innerWidth, window.innerHeight);
}
```

#### <a name="animate"></a>animate()

最後一個需要的是 `animate()` 函式，它也會呼叫 `render()` 函式。 [`requestAnimationFrame()`](https://developer.mozilla.org/docs/Web/API/window/requestAnimationFrame) 函式用來不斷地更新我們的轉譯器。 稍後，我們會使用這些函式，以很酷的動畫更新轉譯器，像是在迷宮四處移動。

```javascript
function animate() {
    render();
    // Keep updating the renderer
    requestAnimationFrame(animate);
}

function render() {
    renderer.render(scene, camera);
}
```

#### <a name="unit-conversion-functions"></a>單位轉換函式

在 **three.js**，旋轉的測量單位是弧度。 為了簡便，我們會新增某些函式，讓我們可以輕鬆地在度與弧度之間轉換。 


```javascript
function degreesToRadians(degrees) {
  return degrees * Math.PI / 180;
}

function radiansToDegrees(radians) {
  return radians * 180 / Math.PI;
}
```

誰會記得 30 度是 .523 弧度？ 執行 `degreesToRadians(30)` 取得 `createMazeCubes()` 函式中所用的旋轉量簡單多了。

___

不少要吸收的程式碼，但現在有美麗的不少轉譯到我們的 `container`！ 請查看 CodePen 中的結果。

您可以複製和貼上此 CodePen 的所有 JavaScript，捲入到所產生的問題中，或編輯調整一些燈號以及變更某些色彩。 

<iframe height='300' scrolling='no' title='燈號、 相機、 立方體 ！' src='//codepen.io/MicrosoftEdgeDocumentation/embed/YZWygZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱手寫筆<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/YZWygZ/'>燈號，相機、 立方體 ！</a> 由 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 在<a href='https://codepen.io'>CodePen</a>。
</iframe>


### <a name="3-making-the-maze"></a>3. 製作迷宮

雖然盯著立方體讓人屏息以待，更好的是整個迷宮由立方體組成！ 建立關卡最快的方式是以 2D 陣列四處放置立方體，這在遊戲社群中是個公開秘密。
 
![使用 2D 陣列所做的迷宮](images/dinomap.png)

將 1 放置到立方體的所在位置，將 0 放置到空白空間的所在位置，可讓您以手動和簡單的方式建立/調整迷宮。

我們將舊的 `createMazeCubes()` 函式取代為使用巢狀迴圈建立及放置多個立方體的函式，來達成此目標。 我們也將會建立陣列名稱 `collidableObjects`，並將立方體加入其中，以便在本教學課程稍後進行碰撞偵測：

```javascript
var totalCubesWide; // How many cubes wide the maze will be
var collidableObjects = []; // An array of collidable objects used later

function createMazeCubes() {
  // Maze wall mapping, assuming even square
  // 1's are cubes, 0's are empty space
  var map = [
    [0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 1, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 1, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 1, 1, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ]
  ];

  // wall details
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });

  // Keep cubes within boundry walls
  var widthOffset = UNITWIDTH / 2;
  // Put the bottom of the cube at y = 0
  var heightOffset = UNITHEIGHT / 2;
  
  // See how wide the map is by seeing how long the first array is
  totalCubesWide = map[0].length;

  // Place walls where 1`s are
  for (var i = 0; i < totalCubesWide; i++) {
    for (var j = 0; j < map[i].length; j++) {
      // If a 1 is found, add a cube at the corresponding position
      if (map[i][j]) {
        // Make the cube
        var cube = new THREE.Mesh(cubeGeo, cubeMat);
        // Set the cube position
        cube.position.z = (i - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        cube.position.y = heightOffset;
        cube.position.x = (j - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        // Add the cube
        scene.add(cube);
        // Used later for collision detection
        collidableObjects.push(cube);
      }
    }
  }
    // The size of the maze will be how many cubes wide the array is * the width of a cube
    mapSize = totalCubesWide * UNITWIDTH;
}

```

現在，我們知道正在使用多少立方體（以及它們有多大），我們現在可以使用計算的 `mapSize` 變數來設定地面平面的尺寸：

```javascript
var mapSize;    // The width/depth of the maze

function createGround() {
    // Create ground geometry and material
    var groundGeo = new THREE.PlaneGeometry(mapSize, mapSize);
    var groundMat = new THREE.MeshPhongMaterial({ color: 0xA0522D, side: THREE.DoubleSide});

    var ground = new THREE.Mesh(groundGeo, groundMat);
    ground.position.set(0, 1, 0);
    // Rotate the place to ground level
    ground.rotation.x = degreesToRadians(90);
    scene.add(ground);
}
```

我們要加入迷宮的最後一個項目是周邊牆壁，圈住所有項目。 我們會使用迴圈，同時製作兩個平面（我們的牆壁），使用在 `createGround()` 中計算的 `mapSize` 變動以決定其寬度。 新牆壁也會新增到我們的 `collidableObjects` 陣列，以供未來碰撞偵測使用：

```javascript
function createPerimWalls() {
    var halfMap = mapSize / 2;  // Half the size of the map
    var sign = 1;               // Used to make an amount positive or negative

    // Loop through twice, making two perimeter walls at a time
    for (var i = 0; i < 2; i++) {
        var perimGeo = new THREE.PlaneGeometry(mapSize, UNITHEIGHT);
        // Make the material double sided
        var perimMat = new THREE.MeshPhongMaterial({ color: 0x464646, side: THREE.DoubleSide });
        // Make two walls
        var perimWallLR = new THREE.Mesh(perimGeo, perimMat);
        var perimWallFB = new THREE.Mesh(perimGeo, perimMat);

        // Create left/right wall
        perimWallLR.position.set(halfMap * sign, UNITHEIGHT / 2, 0);
        perimWallLR.rotation.y = degreesToRadians(90);
        scene.add(perimWallLR);
        // Used later for collision detection
        collidableObjects.push(perimWallLR);
        // Create front/back wall
        perimWallFB.position.set(0, UNITHEIGHT / 2, halfMap * sign);
        scene.add(perimWallFB);

        // Used later for collision detection
        collidableObjects.push(perimWallFB);

        sign = -1; // Swap to negative value
    }
}
```


不要忘記在您的 `init()` 函式的 `createMazeCubes()` 之後新增對 `createGround()` 和 `createPerimWalls` 的呼叫，讓它們進行編譯！
___

我們現在有美麗好看的迷宮，但無法真的感覺有多酷，因為我們的相機停滯在同一個地方。 現在是加強這個遊戲，並加入一些相機控制項的時候了。

歡迎測試 CodePen 中的項目，例如變更色彩或將 `init()` 函式中的 `createGround()` 標示註解來移除地面。


<iframe height='300' scrolling='no' title='迷宮建置' src='//codepen.io/MicrosoftEdgeDocumentation/embed/JWKYzG/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱由 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 發佈到 <a href='https://codepen.io'>CodePen</a> 的 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/JWKYzG/'>迷宮建置</a>。
</iframe>

### <a name="4-allowing-the-player-to-look-around"></a>4. 允許玩家四處查看

現在是進入迷宮並開始四處查看的時候了。 若要這樣做，我們將會使用 **PointerLockControls.js** 程式庫與我們的相機。

**PoinerLockControls.js** 程式庫使用滑鼠，以滑鼠移動方向來旋轉相機，允許玩家四處查看。 

首先，我們將一些新的項目加入到 **index.html** 檔案：

```html
<div id="blocker">
    <div id="instructions">
    <strong>Click to look!</strong>
    </div>
</div>

<script src="main.js"></script>
```

您也需要在本節底部 CodePen 中的所有 CSS。 它應該貼到 **stylesheet.css** 檔案。

切換回 **main.js**、加入一些新的全域變數；`controls` 用來儲存控制器，`controlsEnabled` 用來追蹤控制器狀態，而 `blocker` 用來抓取 **index.html** 中的 `blocker` 項目：

```javascript
var controls;
var controlsEnabled = false;

// HTML elements to be changed
var blocker = document.getElementById('blocker');
```


現在於 `init()` 函式，我們可以建立新的 `PoinerLockControls` 物件，將我們的 `camera` 傳遞給它，並新增 `camera` (使用 `controls.getObject()` 存取)。

```javascript
controls = new THREE.PointerLockControls(camera);
scene.add(controls.getObject());
```

現在已連接相機，但我們需要稍微讓滑鼠與控制器互動，以便我們可以四處查看。 

對於這種情形，[指標鎖定 API](https://docs.microsoft.com/microsoft-edge/dev-guide/dom/pointer-lock) 能解救這個問題，讓我們連接滑鼠移動與相機。 指標鎖定 API 也可以讓滑鼠消失，提供更沈浸式的體驗。 藉由按下 ESC，我們結束滑鼠與相機連接，讓滑鼠重新出現。 新增 `getPointerLock()` 和 `lockChange()` 函式可協助我們執行該動作。

`getPointerLock()` 函式會接聽何時發生滑鼠點選作業。 按一下後，我們轉譯的遊戲 (在 `container` 項目中) 會嘗試控制滑鼠。 我們也加入事件接聽程式，偵測何時玩家啟動或停用鎖定，然後事件接聽程式會呼叫 `lockChange()`。 

```javascript
function getPointerLock() {
  document.onclick = function () {
    container.requestPointerLock();
  }
  document.addEventListener('pointerlockchange', lockChange, false); 
}

```

我們的 `lockChange()` 函式需要停用或啟用控制項和 `blocker` 項目。 我們可以判斷指標鎖定的狀態，方法是檢查滑鼠事件的 [`pointerLockElement`](https://developer.mozilla.org/docs/Web/API/Document/pointerLockElement) 屬性目標是否設定為我們的 `container`。

```javascript
function lockChange() {
    // Turn on controls
    if (document.pointerLockElement === container) {
        // Hide blocker and instructions
        blocker.style.display = "none";
        controls.enabled = true;
    // Turn off the controls
    } else {
      // Display the blocker and instruction
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

現在，我們可以在 `init()` 函式之前新增對 `getPointerLock()` 的呼叫。
```javascript
// Get the pointer lock state
getPointerLock();
init();
animate();
```

---

我們現在有四處**查看**的功能，但是真實的「哇」因素是能夠四處**移動**。 接下來的內容使用向量，會變得有點數學化，但沒有一點數學，3D 圖形還算是什麼？

<iframe height='300' scrolling='no' title='四處查看' src='//codepen.io/MicrosoftEdgeDocumentation/embed/gmwbMo/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱由 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 發佈到 <a href='https://codepen.io'>CodePen</a> 的 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/gmwbMo/'>四處查看</a>。
</iframe>


### <a name="5-adding-player-movement"></a>5. 新增玩家移動

若要深入探討如何讓玩家移動，我們要回到微積分。 我們想要沿著特定向量（方向）將速度（移動）套用到 `camera`。

我們新增一些其他全域變數，以追蹤玩家移動的方向，並設定初始速度向量：

```javascript
// Flags to determine which direction the player is moving
var moveForward = false;
var moveBackward = false;
var moveLeft = false;
var moveRight = false;

// Velocity vector for the player
var playerVelocity = new THREE.Vector3();

// How fast the player will move
var PLAYERSPEED = 800.0;

var clock;
```

在 `init()` 函數開頭，將 `clock` 設定為新的 `Clock` 物件。 我們會使用此物件，追蹤轉譯新框架所花費的時間變更（差異）。 您也需要新增對 `listenForPlayerMovement()` 的呼叫，這會收集使用者輸入。 

```
clock = new THREE.Clock();
listenForPlayerMovement();
```

我們的 `listenForPlayerMovement()` 函式是將會翻轉方向狀態的項目。 在函式的底部，我們會有兩個等待按下並放開按鍵的事件接聽程式。 其中一個事件觸發之後，我們將會檢查這是我們想要觸發或停止移動的按鍵。

針對這個遊戲，我們已經設定讓可以玩家用 W、A、S、D 鍵或方向鍵四處移動。

```javascript
function listenForPlayerMovement() {
    
    // A key has been pressed
    var onKeyDown = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = true;
        break;

      case 37: // left
      case 65: // a
        moveLeft = true;
        break;

      case 40: // down
      case 83: // s
        moveBackward = true;
        break;

      case 39: // right
      case 68: // d
        moveRight = true;
        break;
    }
  };

  // A key has been released
    var onKeyUp = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = false;
        break;

      case 37: // left
      case 65: // a
        moveLeft = false;
        break;

      case 40: // down
      case 83: // s
        moveBackward = false;
        break;

      case 39: // right
      case 68: // d
        moveRight = false;
        break;
    }
  };

  // Add event listeners for when movement keys are pressed and released
  document.addEventListener('keydown', onKeyDown, false);
  document.addEventListener('keyup', onKeyUp, false);
}
```

我們可以判斷使用者想要移動的方向 (在其中一個全域方向旗標中儲存為 `true`)，現在是發生一些動作的時候了。 這個動作剛好採用 `animatePlayer()` 函式的形式。

這個函式會被 `animate()` 中呼叫，傳入 `delta` 以取得畫面之間的時間變更，讓我們的移動在畫面播放速率變更時同步：

```javascript
function animate() {
  render();
  requestAnimationFrame(animate);
  // Get the change in time between frames
  var delta = clock.getDelta();
  animatePlayer(delta);
}
```

現在就是有趣的部分了！ 我們的動量向量 (`playerVeloctiy`) 有三個參數 `(x, y, z)`，其中 `y` 是垂直動量。 因為這個遊戲中不執行任何跳躍，所以我們只會使用 `x` 和 `z` 參數。 一開始這個向量設為 (0, 0, 0)。

如同下方的程式碼所示，完成一系列檢查以查看哪個方向旗標翻轉為 `true`。 我們有方向之後，我們加減 `x` 和 `y`，在該方向上套用動量。 如果沒有按下移動按鍵，向量將會被設定為 `(0, 0, 0)`。


```javascript

function animatePlayer(delta) {
  // Gradual slowdown
  playerVelocity.x -= playerVelocity.x * 10.0 * delta;
  playerVelocity.z -= playerVelocity.z * 10.0 * delta;

  if (moveForward) {
    playerVelocity.z -= PLAYERSPEED * delta;
  } 
  if (moveBackward) {
    playerVelocity.z += PLAYERSPEED * delta;
  } 
  if (moveLeft) {
    playerVelocity.x -= PLAYERSPEED * delta;
  } 
  if (moveRight) {
    playerVelocity.x += PLAYERSPEED * delta;
  }
  if( !( moveForward || moveBackward || moveLeft ||moveRight)) {
    // No movement key being pressed. Stop movememnt
    playerVelocity.x = 0;
    playerVelocity.z = 0;
  }
  controls.getObject().translateX(playerVelocity.x * delta);
  controls.getObject().translateZ(playerVelocity.z * delta);
}
```

最後，我們將任何已更新的 `x` 和 `y` 值套用至相機做為轉譯，讓玩家實際移動。

---

恭喜！ 您現在擁有可以四處移動且查看、玩家控制的相機。 我們仍然會穿牆，但稍後擔心那件事。 接著我們會新增恐龍。

<iframe height='300' scrolling='no' title='可四處移動' src='//codepen.io/MicrosoftEdgeDocumentation/embed/qrbKZg/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱手寫筆<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qrbKZg/'>四處移動</a>由 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 在<a href='https://codepen.io'>CodePen</a>。
</iframe>

> [!NOTE]
> 如果您在 UWP app 中使用這些控制項，可能會遇到移動延遲和未登錄的 `keyUp` 活動。 我們正在調查此問題，並希望很快修正此部分範例！

### <a name="6-load-that-dino"></a>6. 載入該隻恐龍！

如果您複製或下載此專案存放庫，您會看到內含 `dino.json` 的 `models` 資料夾。 這個 JSON 檔案是從 Blender 製作並匯出的恐龍 3D 模型。


我們必須新增更多全域變數以載入此恐龍：

```javascript
var DINOSCALE = 20;  // How big our dino is scaled to

var clock;
var dino;
var loader = new THREE.JSONLoader();

var instructions = document.getElementById('instructions');
```

我們已經建立 `JSONLoader`，現在我們將會傳入 **dino.json** 的路徑及回呼 (具有從檔案中收集的幾何和資料)。
載入恐龍是非同步工作，這表示完全載入恐龍之前，不會轉譯任何項目。 在我們的 **index.html**，我們將 `instructions` 項目中的字串變更為 `"Loading..."`，讓玩家知道遊戲進行中。

載入恐龍之後，以遊戲的實際指示更新 `instructions` 項目，以及從 `init()` 函式結尾移動 `animate()` 函式到函式回呼結尾，如下所示：

```javascript
   // load the dino JSON model and start animating once complete
    loader.load('./models/dino.json', function (geometry, materials) {


        // Get the geometry and materials from the JSON
        var dinoObject = new THREE.Mesh(geometry, new THREE.MultiMaterial(materials));

        // Scale the size of the dino
        dinoObject.scale.set(DINOSCALE, DINOSCALE, DINOSCALE);
        dinoObject.rotation.y = degreesToRadians(-90);
        dinoObject.position.set(30, 0, -400);
        dinoObject.name = "dino";
        scene.add(dinoObject);
        
        // Store the dino
        dino = scene.getObjectByName("dino"); 

        // Model is loaded, switch from "Loading..." to instruction text
        instructions.innerHTML = "<strong>Click to Play!</strong> </br></br> W,A,S,D or arrow keys = move </br>Mouse = look around";

        // Call the animate function so that animation begins after the model is loaded
        animate();
    });
```

---

我們現在已經載入恐龍模型。 快來一探究竟！

<iframe height='300' scrolling='no' title='新增恐龍' src='//codepen.io/MicrosoftEdgeDocumentation/embed/xqOwBw/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱由 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 發佈到 <a href='https://codepen.io'>CodePen</a> 的 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/xqOwBw/'>新增恐龍</a>。
</iframe>

### <a name="7-move-that-dino"></a>7. 移動該隻恐龍！

建立遊戲 AI 會變得非常複雜，所以針對這個範例，我們會讓這隻恐龍簡單移動。 我們的恐龍會直線移動，穿牆並消失在遠方濃霧中。

若要這樣做，請先新增全域變數 `dinoVelocity`。

```javascript
var DINOSPEED = 400.0;

var dinoVelocity = new THREE.Vector3();
```
 接下來，從 `animation()` 函式呼叫 `animateDino()` 函式，而且加入下方的程式碼：

```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;

    dinoVelocity.z += DINOSPEED * delta;
    // Move the dino
    dino.translateZ(dinoVelocity.z * delta);
}
```
---

觀看恐龍走開不是很有趣，但加入碰撞偵測之後，遊戲會變得更有趣。

<iframe height='300' scrolling='no' title='移動恐龍-無碰撞' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/jBMbbL/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱由 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 發佈到 <a href='https://codepen.io'>CodePen</a> 的 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/jBMbbL/'>移動恐龍 - 無碰撞</a>。
</iframe>

### <a name="8-collision-detection-for-the-player"></a>8. 玩家碰撞偵測

我們現在可以讓玩家和恐龍四處移動，但仍有穿牆的擾人問題。 在本教學課程稍早，當我們一開始加入立方體和牆壁時，我們將其推入 `collidableObjects` 陣列。 這個陣列是我們用來得知玩家是否太接近不可穿越的項目。

我們會使用射線偵測器 (Raycaster) 以判斷交集何時即將發生。 您可能將射線偵測器想像成來自相機射向指定方向的雷射光束，回報是否碰到物件以及物件確切距離。

```javascript
var PLAYERCOLLISIONDISTANCE = 20;
```

我們會建立新函式，稱為 `detectPlayerCollision()`，如果玩家太接近可碰撞的物件，它會傳回 `true`。
為玩家，我們將套用一個射線偵測器給此物件，根據玩家移動方向來變更指向方向。

若要這樣做，我們建立 `rotationMatrix`，未定義的矩陣。 檢查移動方向時，我們會得到已定義的 `rotationMatrix`，或如果正在向前移動則為未定義。
如果已定義，`rotationMatrix` 將會套用到控制項的方向。 

然後將會建立射線偵測器，從相機開始和以 `cameraDirection` 方向射出。


```javascript
function detectPlayerCollision() {
    // The rotation matrix to apply to our direction vector
    // Undefined by default to indicate ray should coming from front
    var rotationMatrix;
    // Get direction of camera
    var cameraDirection = controls.getDirection(new THREE.Vector3(0, 0, 0)).clone();

    // Check which direction we're moving (not looking)
    // Flip matrix to that direction so that we can reposition the ray
    if (moveBackward) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(180));
    }
    else if (moveLeft) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(90));
    }
    else if (moveRight) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(270));
    }

    // Player is not moving forward, apply rotation matrix needed
    if (rotationMatrix !== undefined) {
        cameraDirection.applyMatrix4(rotationMatrix);
    }

    // Apply ray to player camera
    var rayCaster = new THREE.Raycaster(controls.getObject().position, cameraDirection);

    // If our ray hit a collidable object, return true
    if (rayIntersect(rayCaster, PLAYERCOLLISIONDISTANCE)) {
        return true;
    } else {
        return false;
    }
}
```

我們的 `detectPlayerCollision()` 函式依賴 `rayIntersect()` helper 函式。
判斷發生碰撞之前，這會取用射線偵測器和值 (表示可以多接近 `collidableObjects` 陣列中的物件)。

```javascript
function rayIntersect(ray, distance) {
    var intersects = ray.intersectObjects(collidableObjects);
    for (var i = 0; i < intersects.length; i++) {
        // Check if there's a collision
        if (intersects[i].distance < distance) {
            return true;
        }
    }
    return false;
}
```

我們已經可以判斷碰撞何時即將發生，現在可以修改 `animatePlayer()` 函式：

```javascript
function animatePlayer(delta) {
    // Gradual slowdown
    playerVelocity.x -= playerVelocity.x * 10.0 * delta;
    playerVelocity.z -= playerVelocity.z * 10.0 * delta;

    // If no collision and a movement key is being pressed, apply movement velocity
    if (detectPlayerCollision() == false) {
        if (moveForward) {
            playerVelocity.z -= PLAYERSPEED * delta;
        }
        if (moveBackward) {
            playerVelocity.z += PLAYERSPEED * delta;
        } 
        if (moveLeft) {
            playerVelocity.x -= PLAYERSPEED * delta;
        }
        if (moveRight) {
            playerVelocity.x += PLAYERSPEED * delta;
        }

        controls.getObject().translateX(playerVelocity.x * delta);
        controls.getObject().translateZ(playerVelocity.z * delta);
    } else {
        // Collision or no movement key being pressed. Stop movememnt
        playerVelocity.x = 0;
        playerVelocity.z = 0;
    }
}
```

---

我們現在有玩家碰撞偵測，請嘗試撞上一些牆壁！

<iframe height='300' scrolling='no' title='移動玩家-碰撞' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/qraOeO/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱由 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 發佈到 <a href='https://codepen.io'>CodePen</a> 的 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qraOeO/'>移動玩家 - 碰撞</a>。
</iframe>


### <a name="9-collision-detection-and-animation-for-dino"></a>9. 恐龍碰撞偵測和動畫

現在是該防止恐龍穿牆，並在它太接近可碰撞的物件時，讓它以隨機方向移動的時候了。

首先讓我們確認恐龍何時發生碰撞。 

我們需要為碰撞距離設定另一個全域變數：

```javascript
var DINOCOLLISIONDISTANCE = 55;     
```

現在，我們已經指定恐龍發生碰撞的距離，我們新增類似於 `detectPlayerCollision()` 但更簡單的函式。
`detectDinoCollision` 函式很簡單，因為我們一直有一個從恐龍前方射出的射線偵測器。 不需要像玩家碰撞偵測旋轉它。

```javascript
function detectDinoCollision() {
    // Get the rotation matrix from dino
    var matrix = new THREE.Matrix4();
    matrix.extractRotation(dino.matrix);
    // Create direction vector
    var directionFront = new THREE.Vector3(0, 0, 1);

    // Get the vectors coming from the front of the dino
    directionFront.applyMatrix4(matrix);

    // Create raycaster
    var rayCasterF = new THREE.Raycaster(dino.position, directionFront);
    // If we have a front collision, we have to adjust our direction so return true
    if (rayIntersect(rayCasterF, DINOCOLLISIONDISTANCE))
        return true;
    else
        return false;
}
```

讓我們看一下連結碰撞偵測時最終 `animateDino()` 函式的外觀：


```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;


    // If no collision, apply movement velocity
    if (detectDinoCollision() == false) {
        dinoVelocity.z += DINOSPEED * delta;
        // Move the dino
        dino.translateZ(dinoVelocity.z * delta);

    } else {
        // Collision. Adjust direction
        var directionMultiples = [-1, 1, 2];
        var randomIndex = getRandomInt(0, 2);
        var randomDirection = degreesToRadians(90 * directionMultiples[randomIndex]);

        dinoVelocity.z += DINOSPEED * delta;
        dino.rotation.y += randomDirection;
    }
}
```

我們希望恐龍以 -90、90 或 180 度轉動。 為了讓程式碼簡單易懂，上方我們建立 `directionMultiples` 陣列，在乘以 90 時，它會產生這些數字。
為了隨機選取旋轉角度，我們新增了 `getRandomInt()` helper 函式來抓取 0、1 或 2 值，這表示陣列的隨機索引。

```javascript
function getRandomInt(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min)) + min;
}
```

完成之後，我們會將陣列的隨機索引乘以 90，來取得旋轉角度（轉換成弧度）。
透過使用 `dino.rotation.y += randomDirection;` 將這個值新增至恐龍的 `y` 旋轉，恐龍現在於碰撞時可隨機轉向。


---

我們完成了！ 我們的恐龍現在有 AI，可在迷宮中四處移動！

<iframe height='300' scrolling='no' title='移動恐龍-碰撞' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/bqwMXZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱手寫筆<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/bqwMXZ/'>移動恐龍-碰撞</a>由 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 在<a href='https://codepen.io'>CodePen</a>。
</iframe>

### <a name="10-starting-the-chase"></a>10. 開始追逐

恐龍在玩家的特定距離內時，我們想要讓恐龍開始追逐玩家。 因為這只是範例，所以沒有套用恐龍追蹤玩家的任何進階演算法。 相反地，恐龍會看著玩家並走向他們。 在迷宮的開放空間部分，這個效果很好，但在牆壁擋路時恐龍會受困在現場。

在我們的 `animate()` 函式，我們會新增布林值變數，由 `triggerChase()` 傳回值所決定：

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();

    animateDino(delta);
    animatePlayer(delta);
}
```

我們的 `triggerChase` 函式會檢查玩家是否在恐龍的追逐範圍內，然後讓恐龍一律面向玩家，讓它依照玩家的方向移動。 

```javascript
function triggerChase() {
    // Check if in dino detection range of the player
    if (dino.position.distanceTo(controls.getObject().position) < 300) {
        // Set the dino target's y value to the current y value. We only care about x and z for movement.
        var lookTarget = new THREE.Vector3();
        lookTarget.copy(controls.getObject().position);
        lookTarget.y = dino.position.y;

        // Make dino face camera
        dino.lookAt(lookTarget);

        // Get distance between dino and camera with a unit offset
        // Game over when dino is the value of CATCHOFFSET units away from camera
        var distanceFrom = Math.round(dino.position.distanceTo(controls.getObject().position)) - CATCHOFFSET;
        // Alert and display distance between camera and dino
        dinoAlert.innerHTML = "Dino has spotted you! Distance from you: " + distanceFrom;
        dinoAlert.style.display = '';
        return true;
        // Not in agro range, don't start distance countdown
    } else {
        dinoAlert.style.display = 'none';
        return false;
    }
}
```

`triggerChase` 的另一半處理文字顯示，可讓玩家知道恐龍的距離。 我們也引入 `CATCHOFFSET` 指定 `0` 應該是多遠。 如果我們沒有位移，`0`將會在玩家的正上方，這並非完美結局。



```javascript
var dinoAlert = document.getElementById('dino-alert');
dinoAlert.style.display = 'none';
```

---

這時，當玩家太接近，瘋狂的恐龍會開始追蹤玩家，直到恐龍的位置在玩家上方才會停止。
最後一個步驟是，在恐龍距離 `CATCHOFFSET` 個單位遠時，新增某些遊戲結束條件。

<iframe height='300' scrolling='no' title='追逐' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpRBqR/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>請參閱由 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 發佈到 <a href='https://codepen.io'>CodePen</a> 的 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpRBqR/'>追逐</a>。
</iframe>


### <a name="11-ending-the-game"></a>11. 結束遊戲


我們從簡單的立方體經過漫長歷程，現在是結束遊戲的時候。

讓我們首先設定變數，追蹤遊戲是否結束：

```javascript
var gameOver = false;
```

現在我們需要更新 `animate()` 函式最後一次，以檢查恐龍是否太接近玩家。
如果恐龍太接近，我們將開始新的函式，稱為 `caught()`，並停止玩家和恐龍移動，如果不接近，我們將會照常繼續讓玩家和恐龍四處移動。

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();
    // Update our frames per second monitor

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();
    // If the player is too close, trigger the end of the game
    if (dino.position.distanceTo(controls.getObject().position) < CATCHOFFSET) {
        caught();
    // Player is at an undetected distance
    // Keep the dino moving and let the player keep moving too
    } else {
        animateDino(delta);
        animatePlayer(delta);
    }
}
```

如果恐龍捕捉到玩家，`caught()` 將會顯示 `blocker` 項目並更新文字以表示已輸掉遊戲。
`gameOver` 變數也設定為 `true`，現在可讓我們知道遊戲已結束。  


```javascript
function caught() {
    blocker.style.display = '';
    instructions.innerHTML = "GAME OVER </br></br></br> Press ESC to restart";
    gameOver = true;
    instructions.style.display = '';
    dinoAlert.style.display = 'none';
}
```


我們已知道遊戲是否結束，現在可以將遊戲結束檢查加入至 `lockChange()` 函式。
現在當遊戲結束後使用者按 ESC，我們可以新增 `location.reload` 重新啟動遊戲。

```javascript
function lockChange() {
    if (document.pointerLockElement === container) {
        blocker.style.display = "none";
        controls.enabled = true;
    } else {
        if (gameOver) {
            location.reload();
        }
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

---

這樣就完成了！ 這是漫長歷程，但我們已經使用 **three.js** 製作遊戲。

返回頁面頂端，可查看[遊戲完稿 CodePen](#introduction)！


## <a name="publishing-to-the-microsoft-store"></a>發佈至 Microsoft Store
現在您擁有 UWP 應用程式，就可以將它發行至 Microsoft Store （假設您已先將它改進 ！）有幾個步驟的程序。

1.  您必須[註冊](https://developer.microsoft.com/store/register)為 Windows 開發人員。
2.  您必須使用 App 提交[檢查清單](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)。
3.  必須提交 App 以取得[認證](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process)。
如需詳細資訊，請參閱[發佈您的 UWP 應用程式](https://developer.microsoft.com/store/publish-apps)。

