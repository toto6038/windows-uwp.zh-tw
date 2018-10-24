---
title: 使用 MonoGame 2D 建立 UWP 遊戲
description: 簡單的 UWP 遊戲 Microsoft 網上商店，以 C# 和 MonoGame 撰寫
author: muhsinking
ms.author: mukin
ms.date: 03/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 5d5f7af2-41a9-4749-ad16-4503c64bb80c
ms.localizationpriority: medium
ms.openlocfilehash: d38465ce02e0aedf854094ede75fc33701b226a6
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "5440772"
---
# <a name="create-a-uwp-game-in-monogame-2d"></a>使用 MonoGame 2D 建立 UWP 遊戲

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-c-and-monogame"></a>適用於 Microsoft Store 的簡單 2D UWP 遊戲，以 C# 和 MonoGame 撰寫


![Walking Dino Sprite 工作表](images/JS2D_0.png)

## <a name="introduction"></a>簡介

MonoGame 是一個輕量型遊戲開發架構。 本教學課程將教導 MonoGame 遊戲開發的基礎，包括如何載入內容、繪製 Sprite、做成動畫，以及處理使用者輸入。 也會討論一些更進階的概念，如碰撞偵測和為高 DPI 螢幕而放大。 完成本教學課程可能需要 30-60 分鐘。

## <a name="prerequisites"></a>必要條件
+   Windows 10 和 Microsoft Visual Studio 2017。  [按一下這裡，以了解如何開始設定 Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up)。
+ .NET 傳統型開發架構。 如果您還沒有這安裝，您可以重新執行，Visual Studio 安裝程式，並修改您的 Visual Studio 2017 的安裝即可取得它。
+   C# 或類似物件導向程式設計語言的基本知識。 [按一下此處以了解如何開始使用 C#](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)。
+   熟悉基本電腦科學概念，如類別、方法和變數，為加分技能。

## <a name="why-monogame"></a>為什麼選擇 MonoGame？
談到遊戲開發環境，並沒有選項短缺的問題。 從完整功能引擎如 Unity，到全方位且複雜的多媒體 API 如 DirectX，可能不知從何開始。 MonoGame 是一組工具，複雜層級落在遊戲引擎與更精細的 API (如 DirectX) 之間。 它提供簡單易用的內容管線，以及建立執行於各種不同平台上之輕量遊戲所需的所有功能。 最棒的是，MonoGame 應用程式以純 C# 撰寫，而且您透過 Microsoft Store 或其他類似散發平台快速散發。

## <a name="get-the-code"></a>取得程式碼
如果您不想逐步完成教學課程的工作，只是想要看運作中的 MonoGame，[請按一下此處取得完成的 App](https://github.com/Microsoft/Windows-appsample-get-started-mg2d)。

在 Visual Studio 2017 中，開啟專案，然後按**F5**來執行範例。 第一次進行此動作時，可能需要一段時間，因為 Visual Studio 需要擷取您安裝中遺失的任何 NuGet 套件。

如果已經這樣做，請略過有關設定 MonoGame 查看程式碼逐步解說的下一節。

**注意：** 這個範例中建立的遊戲並不完整（也根本談不上有趣）。 其唯一目的是要示範 MonoGame 中 2D 開發的所有核心概念。 歡迎將此程式碼做成更好的遊戲，或在您已經掌握基本知識之後從頭開始！

## <a name="set-up-monogame-project"></a>設定 MonoGame 專案
1. 從 [MonoGame.net](http://www.monogame.net/) 安裝適用於 Visual Studio 的 **MonoGame 3.6**

2. 啟動 Visual Studio 2017。

3. 移至 **\[檔案\] -> \[新增\] -> \[專案\]**

4. 在 Visual C# 專案範本底下，選取 **\[MonoGame\]** 和 **\[MonoGame Windows 10 通用專案\]**

5. 將專案命名為 “MonoGame2D”，然後選取 \[確定\]。 建立專案後，它可能看起來有許多錯誤，這些錯誤在您第一次執行專案並安裝任何遺失 NuGet 套件後應該會消失。

6. 請確認 **\[x86\]** 和 **\[本機電腦\]** 設為目標平台，然後按 **F5** 建置並執行空白專案。 如果您依照上述步驟執行，在專案完成建置之後您應該會看到空白藍色視窗。

## <a name="method-overview"></a>方法概觀
現在您已經建立專案，請從 **\[方案總管\]** 開啟 **Game1.cs** 檔案。 這是處理大量遊戲邏輯的地方。 當您建立新的 MonoGame 專案，這裡會自動產生許多重要的方法。 我們來快速檢閱這些方法：

**public Game1()** 建構函式。 本教學課程中，我們完全不會變更此方法。

**protected override void Initialize()** 我們在這裡初始化任何使用的類別變數。 在遊戲開始時，呼叫這個方法一次。

**protected override void LoadContent()** 這個方法在遊戲開始之前載入內容（例如， 紋理、音訊、字型）到記憶體。 如同 Initialize，當應用程式開始時，呼叫這個方法一次。

**protected override void UnloadContent()** 此方法用來卸除非內容管理員內容。 我們完全不會使用此方法。

**protected override void Update(GameTime gameTIme)** 針對每個遊戲迴圈循環，呼叫這個方法一次。 我們在這裡更新遊戲中所使用之任何物件或變數的狀態。 這包含像是物件的位置、速度或色彩。 這也是處理使用者輸入的地方。 簡言之，除了在螢幕上繪製物件，這個方法處理遊戲邏輯的所有部分。
**protected override void Draw(GameTime gameTime)** 這是在螢幕上繪製物件的地方，使用 Update 方法所提供的位置。

## <a name="draw-a-sprite"></a>繪製 Sprite
您已經執行全新 MonoGame 專案，並找到很棒的藍天—我們來加入一些綠地。
在 MonoGame，2D 藝術以 Sprite 的形式新增到應用程式。 Sprite 只是做為單一實體操作的電腦圖形。 Sprite 可以移動、縮放、塑型、做成動畫並結合，建立 2D 空間中可以想像的任何項目。

### <a name="1-download-a-texture"></a>1. 下載紋理
為了示範目的，第一個 Sprite 會很無聊。 [按一下這裡以下載此無特色的綠色矩形](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/grass.png)。

### <a name="2-add-the-texture-to-the-content-folder"></a>2. 將紋理新增至 Content 資料夾
- 開啟 **\[方案總管\]**
- 以滑鼠右鍵按一下 **Content** 資料夾中的 **Content.mgcb**，然後選取 **\[開啟方式\]**。 從快顯功能表中選取 **\[Monogame Pipeline\]**，然後選取 **\[確定\]**。
- 在新視窗中，以滑鼠右鍵按一下 **Content** 項目，然後選取 **\[加入\] -> \[現有項目\]**。
- 在檔案瀏覽器中找出並選取綠色矩形
- 將此項目命名為 “grass.png”，然後選取 **\[加入\]**。

### <a name="3-add-class-variables"></a>3. 新增類別變數
若要載入此影像做為 Sprite 紋理，請開啟 **Game1.cs**，並新增下列類別變數。

```CSharp
const float SKYRATIO = 2f/3f;
float screenWidth;
float screenHeight;
Texture2D grass;
```

SKYRATIO 變數告訴我們在場景中天空相對於草地佔多少比例—在本案例中，三分之二。 **screenWidth** 和 **screenHeight** 將追蹤應用程式視窗大小，而 **grass** 是我們將儲存綠色矩形的地方。

### <a name="4-initialize-class-variables-and-set-window-size"></a>4. 初始化類別變數及設定視窗大小
**screenWidth** 和 **screenHeight** 變數仍然需要初始化，因此新增此程式碼至 **Initialize** 方法：

```CSharp
ApplicationView.PreferredLaunchWindowingMode = ApplicationViewWindowingMode.FullScreen;

screenHeight = (float)ApplicationView.GetForCurrentView().VisibleBounds.Height;
screenWidth = (float)ApplicationView.GetForCurrentView().VisibleBounds.Width;

this.IsMouseVisible = false;
```

除了螢幕的寬度與高度，我們也將 App 的視窗模式設定為 **Fullscreen**，並讓滑鼠隱藏。

### <a name="5-load-the-texture"></a>5. 載入紋理
若要將紋理載入到 grass 變數，新增下列程式碼至 **LoadContent** 方法：

```CSharp
grass = Content.Load<Texture2D>("grass");
```

### <a name="6-draw-the-sprite"></a>6. 繪製 Sprite
若要繪製矩形，新增下列程式碼行至 **Draw** 方法：

```CSharp
GraphicsDevice.Clear(Color.CornflowerBlue);
spriteBatch.Begin();
spriteBatch.Draw(grass, new Rectangle(0, (int)(screenHeight * SKYRATIO),
  (int)screenWidth, (int)screenHeight), Color.White);
spriteBatch.End();
```

我們在這裡使用 **spriteBatch.Draw** 方法，將特定的紋理放置在 Rectangle 物件的框線中。 **Rectangle** 是由其左上角和右下角的 x 和 y 座標定義。 使用我們之前定義的 **screenWidth**、**screenHeight** 和 **SKYRATIO** 變數，我們在螢幕底部三分之一處繪製綠色矩形紋理。 如果您現在執行程式，您應該會看到之前的藍色背景被綠色矩形部分覆蓋。

![綠色矩形](images/monogame-tutorial-1.png)

## <a name="scale-to-high-dpi-screens"></a>配合高 DPI 螢幕調整大小
如果您在高像素密度監視器（例如 Surface Pro 或 Surface Studio 電腦螢幕）上執行 Visual Studio，您可能會發現從上述步驟建立的綠色矩形不會覆蓋螢幕底部三分之一處。 它可能懸空在螢幕左下角的上方。 為了修正此問題，並整合遊戲在所有裝置的體驗，我們將需要建立可相對於螢幕像素密度來縮放某些值的方法：

```CSharp
public float ScaleToHighDPI(float f)
{
  DisplayInformation d = DisplayInformation.GetForCurrentView();
  f *= (float)d.RawPixelsPerViewPixel;
  return f;
}
```

接下來使用此程式碼取代 **Initialize** 方法中的 **screenHeight** 和 **screenWidth** 初始設定：

```CSharp
screenHeight = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Height);
screenWidth = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Width);
```

如果您現在正在使用高 DPI 螢幕，並嘗試執行 App，您應該會看到綠色矩形如預期般覆蓋螢幕底部三分之一處。

## <a name="build-the-spriteclass"></a>建置 SpriteClass
開始以動畫顯示 Sprite 之前，我們會建立新類別，稱為 “SpriteClass”，可讓我們減少 Sprite 操作的表面層級複雜性。

### <a name="1-create-a-new-class"></a>1. 建立新類別
在 **\[方案總管\]**，以滑鼠右鍵按一下 **MonoGame2D (通用 Windows)**，然後選取 **\[加入\] -> \[類別\]**。 將此類別命名為 “SpriteClass.cs”，然後選取 **\[加入\]**。

### <a name="2-add-class-variables"></a>2. 新增類別變數
新增此程式碼至您剛建立的類別：

```CSharp
public Texture2D texture
{
  get;
}

public float x
{
  get;
  set;
}

public float y
{
  get;
  set;
}

public float angle
{
  get;
  set;
}

public float dX
{
  get;
  set;
}

public float dY
{
  get;
  set;
}

public float dA
{
  get;
  set;
}

public float scale
{
  get;
  set;
}
```

我們在這裡設定繪製及以動畫顯示 Sprite 所需要的類別變數。 **x** 和 **y** 變數代表 Sprite 在平面上目前的位置，而 **angle** 變數是 Sprite 的目前角度 (0 為直立，90 是順時針方向傾斜 90 度)。 請務必注意，針對此類別，**x** 和 **y** 代表 Sprite 的**中心**座標（預設原點是左上角）。 這會讓旋轉 Sprite 變得更容易，因為 Sprite 會圍繞任何提供的原點旋轉，而圍繞中心旋轉給我們一致的旋轉動作。

在此之後，我們有 **dX**、**dY** 和 **dA**，它們分別是 **x**、**y** 和 **angle** 變數的每秒變更率。

### <a name="3-create-a-constructor"></a>3. 建立建構函式
在建立 **SpriteClass** 執行個體時，我們從 **Game1.cs** 為建構函式提供圖形裝置、相對於專案資料夾的紋理路徑，以及想要的紋理縮放比例 (相對於其原始尺寸)。 我們將會在開始遊戲之後，在 update 方法中設定類別變數的其餘部分。

```CSharp
public SpriteClass (GraphicsDevice graphicsDevice, string textureName, float scale)
{
  this.scale = scale;
  if (texture == null)
  {
    using (var stream = TitleContainer.OpenStream(textureName))
    {
      texture = Texture2D.FromStream(graphicsDevice, stream);
    }
  }
}
```

### <a name="4-update-and-draw"></a>4 .更新和繪圖
仍有兩個需要新增到 SpriteClass 宣告的方法：

```CSharp
public void Update (float elapsedTime)
{
  this.x += this.dX * elapsedTime;
  this.y += this.dY * elapsedTime;
  this.angle += this.dA * elapsedTime;
}

public void Draw (SpriteBatch spriteBatch)
{
  Vector2 spritePosition = new Vector2(this.x, this.y);
  spriteBatch.Draw(texture, spritePosition, null, Color.White, this.angle, new Vector2(texture.Width/2, texture.Height/2), new Vector2(scale, scale), SpriteEffects.None, 0f);
}
```

**Update** SpriteClass 方法在 Game1.cs 的 **Update** 方法中呼叫，用來更新 Sprite **x**、**y** 和 **angle** 值，依據各自的變更率。

**Draw** 方法在 Game1.cs 的 **Draw** 方法中呼叫，用來繪製遊戲視窗中的 Sprite。

## <a name="user-input-and-animation"></a>使用者輸入與動畫
我們現在已建置 SpriteClass，我們會使用它來建立兩個新遊戲物件，第一個是玩家可透過使用方向鍵和空格鍵控制的虛擬人偶。 第二個是玩家必須避免的物件

### <a name="1-get-the-textures"></a>1. 取得紋理
針對玩家的虛擬人偶，我們會使用 Microsoft 自己的忍者貓 (暴龍是其坐騎)。 [按一下這裡以下載影像](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/ninja-cat-dino.png)。

現在，對於玩家需要避免的障礙， 忍者貓與肉食恐龍這兩個最討厭什麼？ 吃蔬菜！ [按一下這裡以下載影像](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/broccoli.png)。

如同之前的綠色矩形，透過**MonoGame Pipeline** 新增這些影像至 **Content.mgcb**，將它們分別命名為 “ninja-cat-dino.png” 和 “broccoli.png”。

### <a name="2-add-class-variables"></a>2. 新增類別變數
將下列程式碼新增到 **Game1.cs** 中的類別變數清單：

```CSharp
SpriteClass dino;
SpriteClass broccoli;

bool spaceDown;
bool gameStarted;

float broccoliSpeedMultiplier;
float gravitySpeed;
float dinoSpeedX;
float dinoJumpY;
float score;

Random random;
```

**dino** 和 **broccoli** 是我們的 SpriteClass 變數。 **dino** 會保留玩家虛擬人偶，而 **broccoli** 保留花椰菜障礙。

**spaceDown** 追蹤空格鍵是否正在按住，而非按下並放開。

**gameStarted** 告訴我們使用者是否第一次開始遊戲。

**broccoliSpeedMultiplier** 決定花椰菜障礙在螢幕上移動的速度。

**gravitySpeed** 決定玩家虛擬人偶在跳躍後加速向下的速度。

**dinoSpeedX** 和 **dinoJumpY** 決定玩家虛擬人偶移動和跳躍的速度。
score 追蹤玩家已成功閃避多少障礙物。

最後，**random** 用於將某些隨機性加入到花椰菜障礙的行為。

### <a name="3-initialize-variables"></a>3. 初始化變數
接下來，我們需要初始化這些變數。 新增下列程式碼到 Initialize 方法中：

```CSharp
broccoliSpeedMultiplier = 0.5f;
spaceDown = false;
gameStarted = false;
score = 0;
random = new Random();
dinoSpeedX = ScaleToHighDPI(1000f);
dinoJumpY = ScaleToHighDPI(-1200f);
gravitySpeed = ScaleToHighDPI(30f);
```

請注意，最後三個變數需要為高 DPI 裝置縮放，因為它們以像素為單位指定變更率。

### <a name="4-construct-spriteclasses"></a>4. 建構 SpriteClasses
我們將在 **LoadContent** 方法中建構 SpriteClass 物件。 在那裡新增此程式碼：

```CSharp
dino = new SpriteClass(GraphicsDevice, "Content/ninja-cat-dino.png", ScaleToHighDPI(1f));
broccoli = new SpriteClass(GraphicsDevice, "Content/broccoli.png", ScaleToHighDPI(0.2f));
```

花椰菜影像比遊戲中所需顯示大小大很多，所以我們會將它縮小為原始大小的 0.2 倍。

### <a name="5-program-obstacle-behavior"></a>5. 設計障礙行為
我們想要花椰菜從螢幕外某處冒出，並瞄準玩家虛擬人偶，因此玩家必須躲它。 若要完成這些工作，新增這個方法至 **Game1.cs** 類別：

```CSharp
public void SpawnBroccoli()
{
  int direction = random.Next(1, 5);
  switch (direction)
  {
    case 1:
      broccoli.x = -100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 2:
      broccoli.y = -100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
    case 3:
      broccoli.x = screenWidth + 100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 4:
      broccoli.y = screenHeight + 100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
  }

  if (score % 5 == 0) broccoliSpeedMultiplier += 0.2f;

  broccoli.dX = (dino.x - broccoli.x) * broccoliSpeedMultiplier;
  broccoli.dY = (dino.y - broccoli.y) * broccoliSpeedMultiplier;
  broccoli.dA = 7f;
}
```

方法的第一部分決定冒出花椰菜物件的螢幕外點，使用兩個隨機數字。

第二部分決定花椰菜移動速度，這是由目前分數決定。 當玩家成功閃避每五個花椰菜，移動速度會更快。

第三個部分設定花椰菜 Sprite 移動的方向。 當冒出花椰菜時，它瞄準玩家虛擬人偶 (恐龍)。 我們也提供值為 7f 的 **dA**，讓花椰菜在追逐玩家時在空中旋轉。

### <a name="6-program-game-starting-state"></a>6. 設計遊戲開始狀態
移到處理鍵盤輸入之前，我們需要方法來設定我們所建立兩個物件的初始遊戲狀態。 與其 App 執行時遊戲立即開始，我們希望使用者按空格鍵手動開始遊戲。 新增下列程式碼，設定動畫物件的初始狀態，並將分數重設：

```CSharp
public void StartGame()
{
  dino.x = screenWidth / 2;
  dino.y = screenHeight * SKYRATIO;
  broccoliSpeedMultiplier = 0.5f;
  SpawnBroccoli();  
  score = 0;
}
```

### <a name="7-handle-keyboard-input"></a>7.處理鍵盤輸入
接下來，我們需要新的方法來處理透過鍵盤使用者輸入。 將這個方法新增至 **Game1.cs**：

```CSharp
void KeyboardHandler()
{
  KeyboardState state = Keyboard.GetState();

  // Quit the game if Escape is pressed.
  if (state.IsKeyDown(Keys.Escape))
  {
    Exit();
  }

  // Start the game if Space is pressed.
  if (!gameStarted)
  {
    if (state.IsKeyDown(Keys.Space))
    {
      StartGame();
      gameStarted = true;
      spaceDown = true;
      gameOver = false;
    }
    return;
  }            
  // Jump if Space is pressed
  if (state.IsKeyDown(Keys.Space) || state.IsKeyDown(Keys.Up))
  {
    // Jump if the Space is pressed but not held and the dino is on the floor
    if (!spaceDown && dino.y >= screenHeight * SKYRATIO - 1) dino.dY = dinoJumpY;

    spaceDown = true;
  }
  else spaceDown = false;

  // Handle left and right
  if (state.IsKeyDown(Keys.Left)) dino.dX = dinoSpeedX * -1;

  else if (state.IsKeyDown(Keys.Right)) dino.dX = dinoSpeedX;
  else dino.dX = 0;
}
```

上述我們有四個連續 if 陳述式：

第一個在按下 **Escape** 鍵時結束遊戲。

第二個在按下**空格鍵**而且遊戲尚未開始時開始遊戲。

第三個在按下**空格鍵**時，藉由變更其 **dY** 屬性，讓恐龍虛擬人偶跳躍。 請注意，除非在地面 (dino.y = screenHeight * SKYRATIO)，否則玩家無法跳躍，如果按住 (而不是按一次) 空格鍵玩家也無法跳躍。 這會阻止恐龍在遊戲一開始立即跳躍 (利用開始遊戲的相同按鍵動作搭順風車)。

最後，最後一個 if/else 子句會檢查正在按下向左或向右鍵，從而變更恐龍的 **dX** 屬性。

**挑戰：** 您可以讓上述鍵盤處理方法不只適用於方向鍵，也能用於 WASD 輸入配置嗎？

### <a name="8-add-logic-to-the-update-method"></a>8. 將邏輯加入至 Update 方法
接下來，我們需要新增所有這些部分的邏輯至 **Game1.cs** 中的 **Update**方法：

```CSharp
float elapsedTime = (float)gameTime.ElapsedGameTime.TotalSeconds;
KeyboardHandler(); // Handle keyboard input
// Update animated SpriteClass objects based on their current rates of change
dino.Update(elapsedTime);
broccoli.Update(elapsedTime);

// Accelerate the dino downward each frame to simulate gravity.
dino.dY += gravitySpeed;

// Set game floor so the player does not fall through it
if (dino.y > screenHeight * SKYRATIO)
{
  dino.dY = 0;
  dino.y = screenHeight * SKYRATIO;
}

// Set game edges to prevent the player from moving offscreen
if (dino.x > screenWidth - dino.texture.Width/2)
{
  dino.x = screenWidth - dino.texture.Width/2;
  dino.dX = 0;
}
if (dino.x < 0 + dino.texture.Width/2)
{
  dino.x = 0 + dino.texture.Width/2;
  dino.dX = 0;
}

// If the broccoli goes offscreen, spawn a new one and iterate the score
if (broccoli.y > screenHeight+100 || broccoli.y < -100 || broccoli.x > screenWidth+100 || broccoli.x < -100)
{
  SpawnBroccoli();
  score++;
}
```

### <a name="9-draw-spriteclass-objects"></a>9. 繪製 SpriteClass 物件
最後，將下列程式碼新增到 **Game1.cs** 的 **Draw** 方法，在 **spriteBatch.Draw** 的最後一個呼叫之後：

```CSharp
broccoli.Draw(spriteBatch);
dino.Draw(spriteBatch);
```

在 MonoGame，**spriteBatch.Draw** 的新呼叫將會在任何先前呼叫上繪圖。 這表示該花椰菜和恐龍 Sprite 都會在現有草地 Sprite 上繪製，讓它們無論其位置都永遠不會隱藏在草地背後。

現在請嘗試執行遊戲，並使用方向鍵與空格鍵來移動恐龍。 如果您依照上述步驟執行，您應該可以讓您的虛擬人偶在遊戲視窗中移動，而且花椰菜的速度應該不斷增加。

![玩家虛擬人偶和障礙](images/monogame-tutorial-2.png)

## <a name="render-text-with-spritefont"></a>以 SpriteFont 轉譯文字
使用上方的程式碼，我們在幕後追蹤玩家的分數，但我們實際上不告訴玩家分數為何。 我們在 App 啟動時也有非直覺的簡介—玩家會看到藍色及綠色視窗，但無法得知需要按空格鍵來開始遊戲。

若要修正這些問題，我們將使用一種新 MonoGame 物件，稱為 **SpriteFonts**。

### <a name="1-create-spritefont-description-files"></a>1. 建立 SpriteFont 描述檔案
在 **\[方案總管\]**，尋找 **Content** 資料夾。 在此資料夾，以滑鼠右鍵按一下 **Content.mgcb** 檔案，並選取**\[開啟方式\]**。 從快顯功能表中選取 **\[MonoGame Pipeline\]**，然後按 **\[確定\]**。 在新視窗中，以滑鼠右鍵按一下 **\[內容\]** 項目，然後選取 **\[加入\] -> \[新增項目\]**。 選取 **\[SpriteFont 描述\]**，將它命名為 “Score”，然後按 **\[確定\]**。 接著，使用相同的程序，新增另一個名為 “GameState” 的 SpriteFont 描述。

### <a name="2-edit-descriptions"></a>2. 編輯描述
在 **MonoGame Pipeline**，以滑鼠右鍵按一下 **Content** 資料夾並選取 **\[開啟檔案位置\]**。 您應該會看見一個內含剛建立 SpriteFont 描述檔案 (以及您目前已新增到 Content 資料夾的任何影像) 的資料夾。 您現在可以關閉並儲存 MonoGame Pipeline 視窗。 從 **\[檔案總管\]**，使用文字編輯器（Visual Studio、NotePad++、Atom 等）開啟這兩個描述檔案。

每個描述包含一些描述 SpriteFont 的值。 我們要做一些變更：

在 **Score.spritefont**，將 **<Size>** 值從 12 變更為 36。

在**GameState.spritefont**，將**<Size>** 值從 12 變更為 72，而將 **<FontName>** 值從 Arial 變更為 Agency。 Agency 是另一種隨附於 Windows 10 電腦的標準字型，將會在我們的簡介畫面新增一些獨特風格。

### <a name="3-load-spritefonts"></a>3. 載入 SpriteFonts
回到 Visual Studio，我們首先要為簡介啟動顯示畫面新增新的紋理。 [按一下這裡以下載影像](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/start-splash.png)。

如先前一樣，以滑鼠右鍵按一下 Content 項目，並選取 **\[加入\] -> \[現有項目\]**，將紋理新增至專案。 將新項目命名為 “start-splash.png”。

接下來，將下列類別變數新增至 **Game1.cs**：

```CSharp
Texture2D startGameSplash;
SpriteFont scoreFont;
SpriteFont stateFont;
```

接著將這些程式碼行新增至 **LoadContent** 方法：

```CSharp
startGameSplash = Content.Load<Texture2D>("start-splash");
scoreFont = Content.Load<SpriteFont>("Score");
stateFont = Content.Load<SpriteFont>("GameState");
```

### <a name="4-draw-the-score"></a>4. 繪製分數
移至 **Game1.cs** 的 **Draw** 方法，將下列程式碼新增至 **spriteBatch.End();** 之前

```CSharp
spriteBatch.DrawString(scoreFont, score.ToString(),
new Vector2(screenWidth - 100, 50), Color.Black);
```

上述的程式碼使用我們建立的 Sprite 描述 (Arial Size 36)，在螢幕右上角附近繪製玩家的目前分數。

### <a name="5-draw-horizontally-centered-text"></a>5. 繪製水平置中的文字
建立遊戲時，您通常要繪圖水平或垂直置中的文字。 若要將文字水平置中，新增此程式碼到 **Draw** 方法，在 **spriteBatch.End();** 之前

```CSharp
if (!gameStarted)
{
  // Fill the screen with black before the game starts
  spriteBatch.Draw(startGameSplash, new Rectangle(0, 0,
  (int)screenWidth, (int)screenHeight), Color.White);

  String title = "VEGGIE JUMP";
  String pressSpace = "Press Space to start";

  // Measure the size of text in the given font
  Vector2 titleSize = stateFont.MeasureString(title);
  Vector2 pressSpaceSize = stateFont.MeasureString(pressSpace);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, title,
  new Vector2(screenWidth / 2 - titleSize.X / 2, screenHeight / 3),
  Color.ForestGreen);
  spriteBatch.DrawString(stateFont, pressSpace,
  new Vector2(screenWidth / 2 - pressSpaceSize.X / 2,
  screenHeight / 2), Color.White);
  }
```

首先，我們建立兩個 String，對於我們希望繪製的每一行文字各一個。 接下來，使用 **SpriteFont.MeasureString(String)** 方法，我們衡量在列印時每一行的寬度與高度。 這會以 **Vector2** 物件形式提供大小，其中 **X** 屬性包含寬度，而 **Y** 屬性包含高度。

最後，我們繪製每一行。 若要將文字水平置中，我們將其位置向量的 **X** 值等於 **screenWidth / 2 - textSize.X / 2**

**挑戰：** 如何變更上述程序，使文字水平垂直置中？

嘗試執行遊戲。 您是否看到簡介啟動顯示畫面？ 每次花椰菜重新冒出時，分數會向上計數嗎？

![簡介啟動顯示畫面](images/monogame-tutorial-3.png)

## <a name="collision-detection"></a>碰撞偵測
我們有追著您四處跑的花椰菜，也有每次新花椰菜冒出時便會向上計數的分數，但實際上卻沒有輸掉遊戲的方法。 我們需要知道恐龍和花椰菜 Sprite 是否相碰撞，而且當這樣做，宣告遊戲結束。

### <a name="1-rectangular-collision"></a>1. 矩形碰撞
當遊戲中偵測到碰撞，物件通常會簡化以減少相關複雜的數學運算。 為了偵測玩家虛擬人偶和花椰菜障礙之間碰撞，我們會將兩者視為矩形。

開啟 **SpriteClass.cs** 並新增類別變數：

```CSharp
const float HITBOXSCALE = .5f;
```

這個值代表碰撞偵測對玩家如何「寬恕」。 值為 .5f 時，恐龍所擁有可以與花椰菜碰撞的矩形邊緣—通常稱為 “hitbox”—將是紋理完整大小的一半。 這樣會造成兩個紋理角落碰撞，但影像的任何部分實際上看起來未碰觸的幾個情形。 歡迎依據您的個人品味調整這個值。

接下來，新增新的方法至 **SpriteClass.cs**：

```CSharp
public bool RectangleCollision(SpriteClass otherSprite)
{
  if (this.x + this.texture.Width * this.scale * HITBOXSCALE / 2 < otherSprite.x - otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y + this.texture.Height * this.scale * HITBOXSCALE / 2 < otherSprite.y - otherSprite.texture.Height * otherSprite.scale / 2) return false;
  if (this.x - this.texture.Width * this.scale * HITBOXSCALE / 2 > otherSprite.x + otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y - this.texture.Height * this.scale * HITBOXSCALE / 2 > otherSprite.y + otherSprite.texture.Height * otherSprite.scale / 2) return false;
  return true;
}
```

這個方法會偵測兩個矩形物件是否已碰撞。 演算法運作方式是，測試查看矩形的任何側邊之間是否有間距。 如果有任何間距，表示沒有碰撞，如果無間距存在，表示必定有碰撞。

### <a name="2-load-new-textures"></a>2. 載入新紋理

然後開啟 **Game1.cs** 並新增兩個新類別變數，一個用來儲存遊戲結束 Sprite 紋理，一個布林值用來追蹤遊戲狀態：

```CSharp
Texture2D gameOverTexture;
bool gameOver;
```

然後，在 **Initialize** 方法中初始化 **gameOver**：

```CSharp
gameOver = false;
```

最後，將紋理載入到 **LoadContent** 方法中的 **gameOverTexture**：

```CSharp
gameOverTexture = Content.Load<Texture2D>("game-over");
```

### <a name="3-implement-game-over-logic"></a>3. 實作「遊戲結束」邏輯
新增此程式碼至 **Update** 方法，在 **KeyboardHandler** 方法呼叫之後：

```CSharp
if (gameOver)
{
  dino.dX = 0;
  dino.dY = 0;
  broccoli.dX = 0;
  broccoli.dY = 0;
  broccoli.dA = 0;
}
```

這會造成遊戲結束時停止所有動作，凍結恐龍和花椰菜 sprites 目前的位置。

接著，在 **Update** 方法結尾處，在 **base.Update(gameTime)** 之前，加入這行程式碼：

```CSharp
if (dino.RectangleCollision(broccoli)) gameOver = true;
```

這會呼叫我們在 **SpriteClass** 建立的 **RectangleCollision**方法，並在傳回 true 時將遊戲標示為結束。

### <a name="4-add-user-input-for-resetting-the-game"></a>4. 新增使用者輸入以重設遊戲
新增此程式碼至 **KeyboardHandler** 方法，讓使用者按 Enter 鍵重設遊戲：

```CSharp
if (gameOver && state.IsKeyDown(Keys.Enter))
{
  StartGame();
  gameOver = false;
}
```

### <a name="5-draw-game-over-splash-and-text"></a>5. 繪製遊戲結束啟動顯示畫面和文字
最後，繪圖，新增此程式碼至 Draw 方法，就在 **spriteBatch.Draw** 的第一個方法呼叫之後（這應該是繪製草地紋理的呼叫）。

```CSharp
if (gameOver)
{
  // Draw game over texture
  spriteBatch.Draw(gameOverTexture, new Vector2(screenWidth / 2 - gameOverTexture.Width / 2, screenHeight / 4 - gameOverTexture.Width / 2), Color.White);

  String pressEnter = "Press Enter to restart!";

  // Measure the size of text in the given font
  Vector2 pressEnterSize = stateFont.MeasureString(pressEnter);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, pressEnter, new Vector2(screenWidth / 2 - pressEnterSize.X / 2, screenHeight - 200), Color.White);
}
```

我們在此使用之前相同的方法以繪製水平置中文字（重複使用簡介啟動顯示畫面使用的字型），以及將 **gameOverTexture** 置中在上半部視窗。

大功告成！ 請再次嘗試執行遊戲。 如果您依照上述步驟執行，遊戲現在應該會在恐龍與花椰菜碰撞時結束，而且系統應該會提示玩家按 Enter 鍵重新啟動遊戲。

![遊戲結束](images/monogame-tutorial-4.png)

## <a name="publish-to-the-microsoft-store"></a>發佈到 Microsoft 網上商店
由於我們這個遊戲建置為 UWP app，它可將這個專案發佈至 Microsoft Store。 程序有幾個步驟。

您必須[註冊](https://developer.microsoft.com/en-us/store/register)為 Windows 開發人員。

您必須使用 [App 提交檢查清單](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions)。

必須提交 App 以取得[認證](https://docs.microsoft.com/en-us/windows/uwp/publish/the-app-certification-process)。

如需詳細資訊，請參閱[發佈您的 UWP 應用程式](https://developer.microsoft.com/en-us/store/publish-apps)。
