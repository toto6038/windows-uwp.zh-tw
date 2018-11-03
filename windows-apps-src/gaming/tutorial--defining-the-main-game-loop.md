---
author: joannaleecy
title: 定義主要遊戲物件
description: 現在，我們要詳細看看遊戲範例主要物件的詳細資料，以及如何將主要物件實作的規則轉譯為與遊戲世界的互動。
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 主要物件
ms.localizationpriority: medium
ms.openlocfilehash: b94d7139f35b3a18edd66af9959a0958d0bdcbc1
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/03/2018
ms.locfileid: "5992039"
---
# <a name="define-the-main-game-object"></a>定義主要遊戲物件

一旦您已經舖範例遊戲的基本架構，並實作了處理高階使用者及系統行為的狀態電腦，您會想要檢查規則與機制搖身一變遊戲的遊戲範例。 讓我們看看遊戲範例主要物件的詳細資料，以及如何將遊戲規則翻譯成與遊戲世界的互動。

>[!Note]
>如果您尚未下載此範例的最新遊戲程式碼，請移至 [Direct3D 遊戲範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目標

了解如何套用基本的開發技術來實作遊戲規則和機制 UWP DirectX 遊戲。

## <a name="main-game-object"></a>主要遊戲物件

在此範例遊戲中， __Simple3DGame__是主要遊戲物件類別。 __App:: load__方法中建構__Simple3DGame__物件的執行個體。

__Simple3DGame__類別物件：
* 指定遊戲邏輯的實作
* 包含通訊的方法：
    * 在應用程式架構中定義的狀態電腦遊戲狀態的變更。
    * 應用程式到遊戲物件本身的遊戲狀態的變更。
    * 更新遊戲的 UI （重疊以及平視顯示器）、 動畫和物理特性 （動態） 的詳細資料。

    >[!Note]
    >更新的圖形是由__GameRenderer__類別處理，其中包含方法來取得，並使用遊戲所使用的圖形裝置資源。 如需詳細資訊，請參閱[轉譯架構 i： 轉譯簡介](tutorial--assembling-the-rendering-pipeline.md)。

* 做為容器，定義遊戲工作階段、 關卡，資料或存留期，根據您定義您的遊戲在高階的方式。 在此情況下，遊戲狀態資料的存留期，遊戲，而會初始化一次當使用者啟動遊戲。

若要檢視方法與此類別物件中所定義的資料，請移至[Simple3DGame 物件](#simple3dgame-object)。

## <a name="initialize-and-start-the-game"></a>初始化並啟動遊戲

當玩家啟動遊戲時，遊戲物件必須初始化它的狀態，建立和新增重疊，設定追蹤玩家表現的變數，以及具現化用來建立關卡的物件。 在此範例中，這是當[__app:: load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123)中建立新的__GameMain__執行個體。 

在遊戲物件__Simple3DGame__，被建立__GameMain__建構函式。 接著，它會初始化[非同步建立__GameMain__建構函式中的工作](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L65-L74)期間使用[__simple3dgame:: initialize__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L54-L250)方法。

### <a name="simple3dgameinitialize-method"></a>Simple3dgame:: initialize 方法

遊戲範例會設定遊戲物件中的下列元件：

* 已建立新的視訊播放物件。
* 已建立遊戲的圖形基本型別陣列，包括關卡基本型別、彈藥及障礙物陣列。
* 已建立儲存遊戲狀態資料的位置 (名為 *Game*)，並放在 [**ApplicationData::Current**](https://msdn.microsoft.com/library/windows/apps/br241619) 指定的應用程式資料設定儲存位置。
* 已建立遊戲計時器與初始的遊戲內重疊點陣圖。
* 已使用一組特定的檢視與投射參數建立新的相機。
* 輸入裝置 (控制器) 設定為與相機相同的起始傾斜度與偏航角，因此玩家在起始控制位置與相機位置之間是 1 對 1 的對應。
* 已建立玩家物件並設定為使用中。 我們使用一個球形物件來偵測玩家的牆壁和障礙物的鄰近性，並保留相機從開始放在可能妨礙遊戲進行的位置。
* 已建立遊戲世界基本型別。
* 已建立圓柱形障礙物。
* 已建立目標 (**Face** 物件) 並設置編號。
* 已建立子彈。
* 已建立關卡。
* 已載入高分記錄。
* 已載入所有先前儲存的遊戲狀態。

現在遊戲擁有所有關鍵元件的執行個體：世界、玩家、障礙物、目標以及子彈。 它也擁有關卡的執行個體，代表上述所有元件的設定，以及它們在每個特定關卡的行為。 現在來看看遊戲如何建立關卡。

## <a name="build-and-load-game-levels"></a>建置和載入遊戲關卡

建構關卡繁重的工作的大部分都是範例方案的__GameLevels__資料夾中找到的__Level.h/.cpp__檔案中完成。 因為它著重在非常特殊的實作，我們將不會涵蓋這些以下。 重要的是每個關卡的程式碼都會做為個別 __LevelN__ 物件來執行。 如果您想要擴充遊戲，您可以建立一個**層級**物件，並會隨機放置障礙物和目標可指派的數字做為參數。 或者，您可以從資源檔案或甚至是網際網路，載入關卡設定資料。

## <a name="define-the-game-play"></a>定義遊戲玩法

到目前為止，我們已經具備組成遊戲所需的全部元件。 關卡已經從基本類型，記憶體中建構，並準備好讓玩家開始與互動。

此最好的遊戲玩家的輸入，立即回應，以及提供立即的回饋。 這適用於任何類型的遊戲時，從 twitch action、 即時的第一人稱第三人稱射擊回合型戰略遊戲。

### <a name="simple3dgamerungame-method"></a>Simple3DGame::RunGame 方法

播放層級，當遊戲處於__Dynamics__狀態時。 

[__Gamemain:: Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)是主要的更新迴圈會更新一次每個畫面的應用程式狀態，如下所示。 在更新迴圈中，它會呼叫[__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)方法來處理工作，如果遊戲處於__Dynamics__狀態。

```cpp
// Updates the application state once per frame.
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
        //...

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when playing a level, the game is in the Dynamics state. Work is handled by Simple3DGame::RunGame method.
            switch (runState)
            {
                
      //...
```
          
[__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)處理定義目前的遊戲迴圈反覆運算的遊戲的目前狀態的資料集。

遊戲流程邏輯__RunGame__中：
*  此方法會更新關卡結束前用來倒數計時 (秒) 的計時器，並測試關卡時間是否已經用完。 這是其中一項遊戲規則： 當時間用完並所有目標有不被擷取都畫面時，它是遊戲結束。
*  如果時間結束，此方法會設定 **TimeExpired** 遊戲狀態，並回到先前的程式碼中的 **Update** 方法。
*  如果還有時間，會輪詢移動視角控制器以更新相機位置；具體而言，就是更新從相機平面 (即玩家的視線) 正常投射的檢視角度，以及自上次輪詢控制器後該角度所移動的距離。
*  相機會依據移動視角控制器中的新資料進行更新。
*  更新動態，或是遊戲世界中與玩家控制無關的動畫及物件行為。 在這個遊戲範例中，呼叫[__UpdateDynamics()__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856)方法來更新有已經引發子彈的動作、 柱型障礙物的動畫與目標的移動。 如需詳細資訊，請參閱[更新遊戲世界](#update-the-game-world)。
*  此方法會檢查是否已達到過關的條件。 如果達到，它會計算關卡的最後分數，並檢查是否為最後一關 (共 6 關)。 如果是最後一關，此方法會傳回 **GameComplete** 遊戲狀態；否則會傳回 __LevelComplete__ 遊戲狀態。
*  如果關卡尚未完成，此方法會將遊戲狀態設為 __Active__ 並回到這一關。

## <a name="update-the-game-world"></a>更新遊戲世界

在此範例中，執行遊戲時， [__Simple3DGame::UpdateDynamics()__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856)方法呼叫[__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)方法 （這稱為 「 從[__gamemain:: Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)） 來更新轉譯遊戲場景中的物件。

在__UpdateDynamics__迴圈中，呼叫方法，可用來設定遊戲世界中的動作，獨立玩家的輸入，建立身歷其境的遊戲體驗並進行層級*成形*。 這包含需要經過轉譯的圖形，與執行中的動畫執行迴圈以帶有關一個真實、 覺得真實，即使沒有任何玩家輸入。 例如，在 wind、 搖晃分批 cresting 沿著線條搖擺的樹、 拍吸煙以及外星怪獸自動縮放和四處移動樹狀結構。 它也包含物件之間的互動，像是玩家環境和世界之間的碰撞，或子彈與障礙物和目標之間的撞擊。

遊戲迴圈應該一律保持更新遊戲世界，或是否根據遊戲邏輯，實體的演算法，無論是只是單純的隨機，除了遊戲特別暫停。 

在遊戲範例中，這個原則就稱為 *「動態」*，它包含柱型障礙物的升起和落下，還有子彈發射時的動作及物理行為。 

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame::UpdateDynamics 方法 

這個方法會處理 4 組計算：

* 發射的子彈在世界的位置。
* 柱型障礙物的動畫。
* 玩家與世界界限的交集。
* 子彈與障礙物、目標、其他子彈和世界的碰撞。

障礙物動畫是在 **Animate.h/.cpp** 中定義的迴圈。 子彈和任何碰撞的行為會由簡化的物理演算法定義、 提供的程式碼中，並由一組全域常數遊戲世界中，包括重力和材料屬性參數化。 這些全都是在遊戲世界座標空間中計算的。

### <a name="review-the-flow"></a>檢閱流程

現在，我們更新場景中的所有物件，並計算了任何碰撞，我們需要使用此資訊來繪製對應的視覺變更。 

__GameMain::Update()__ 完成的遊戲迴圈目前的反覆項目後，範例會立即呼叫__Render()__ 取得更新的物件資料並產生一個新的場景呈現給使用者，如下所示。 接下來，我們來看看轉譯。

```cpp
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::TooSmall:
                // ...
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update(); // GameMain::Update calls Simple3DGame::RunGame() if game is in Dynamics state, uses Simple3DGame::UpdateDynamics() to update game world.
                m_renderer->Render(); //Render() is called immediately after the Update() loop
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // exiting due to window close.  Make sure to save state.
}
```

## <a name="render-the-game-worlds-graphics"></a>轉譯遊戲世界的圖形

建議您盡可能更新遊戲中的圖形，最多可以在每次主要遊戲迴圈反覆執行時就進行更新。 在迴圈反覆執行時，無論玩家有沒有提供輸入，都會更新遊戲。 這樣可以讓計算過的動畫和行為能夠流暢地顯示。 想像一下，如果我們有一個關於水的簡單場景，但是玩家必須按下按鈕才能流動。 這會是一個很無聊的視覺效果。 優質的遊戲看起來要很平穩流暢。

重新呼叫範例遊戲的迴圈，如上方[__gamemain:: Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202)所示。 如果可以看到遊戲的主視窗且主視窗沒有定格或停用，遊戲會持續更新並轉譯這個更新的結果。 我們正在檢查的[__轉譯__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameRenderer.cpp#L474-L624)的方法目前轉譯代表這個狀態。 這是**更新**，其中包括**RunGame**更新狀態，這已在上一節中所討論的呼叫之後立即完成。

這個方法會繪製 3D 世界的投影，然後在上面繪製 Direct2D 重疊。 完成時，它會顯示最後的交換鏈結，包含結合的顯示緩衝區。

>[!Note]
>有兩個範例遊戲的 Direct2D 重疊狀態： 一種是遊戲顯示包含暫停功能表中，與一種是遊戲顯示十字觸控螢幕移動視角的矩形的點陣圖遊戲資訊重疊控制器。 分數文字會同時在這兩種狀態繪製。 如需詳細資訊，請參閱[轉譯架構 I：轉譯簡介](tutorial--assembling-the-rendering-pipeline.md)。

### <a name="gamerendererrender-method"></a>GameRenderer::Render 方法

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
   
        // ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            //...
            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); // Renders the 3D objects
            }

        //...
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw(); //Start drawing the overlays
        
        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud->Render(m_game); // Renders number of hits, shots, and time
        }

        if (m_gameInfoOverlay->Visible())
        {
            d2dContext->DrawBitmap(     // Renders the game overlay
                m_gameInfoOverlay->Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        //...
    }
}
```

## <a name="simple3dgame-object"></a>Simple3DGame 物件

這些是方法__Simple3DGame__物件類別中定義的資料。

### <a name="methods"></a>方法

**Simple3DGame**上定義的內部方法包括：

-   **初始化**： 設定全域變數的起始值以及初始化遊戲物件。 此內容將涵蓋的[初始化和啟動遊戲](#initialize-and-start-the-game)一節。
-   **LoadGame**： 初始化新的關卡並開始載入。
-   **LoadLevelAsync**： 開始非同步工作 （如果您是使用非同步工作，您不熟悉，請參閱[平行模式程式庫](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl)） 來初始化關卡，然後叫用非同步工作來載入裝置特定關卡資源轉譯器上的。 這個方法在另一個執行緒中執行；因此，從這個執行緒只能呼叫 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) 方法 (與 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 方法相反)。 任何裝置內容方法都是在 **FinalizeLoadLevel** 方法中呼叫的。
-   **FinalizeLoadLevel**： 任何載入關卡時需要完成工作在主執行緒上執行的動作。 這包括任何呼叫 Direct3D 11 裝置內容 ([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)) 方法。
-   **StartLevel**： 以新關卡開始遊戲。
-   **PauseGame**： 暫停遊戲。
-   **RunGame**： 遊戲迴圈反覆執行。 如果遊戲狀態為 **Active**，那麼每次反覆執行遊戲迴圈時就會從 **App::Update** 呼叫它一次。
-   **OnSuspending**和**OnResuming**： 分別暫停和繼續遊戲的音訊。

私用方法：

-   **LoadSavedState**和**SaveState**： 載入和分別儲存的遊戲，目前的狀態。
-   **SaveHighScore**和**LoadHighScore**： 分別儲存和載入高分整個遊戲的。
-   **InitializeAmmo**： 重設為其原始狀態子彈用於每一輪的開始每個球體物件的狀態。
-   **UpdateDynamics**： 這是一個重要的方法，因為它會更新依據已經定義的動畫常式、 物理特性和控制項輸入的所有遊戲物件。 這是定義遊戲的互動核心。 此內容將涵蓋[更新遊戲世界](#update-the-game-world)] 區段中。

其他公用方法則是 getter 屬性，它會將遊戲和重疊的特定資訊傳回到要顯示的 app 架構。

### <a name="data"></a>資料

在程式碼範例頂端有 4 個物件，其執行個體會隨著遊戲迴圈執行而更新。

-   **MoveLookController**物件： 代表玩家輸入。 如需詳細資訊，請參閱[新增控制項](tutorial--adding-controls.md)。
-   **GameRenderer**物件： 代表衍生自處理所有裝置特定物件及其轉譯**DirectXBase**類別的 Direct3D 11 轉譯器。 如需詳細資訊，請參閱[轉譯架構 I](tutorial--assembling-the-rendering-pipeline.md)。
-   **音訊**物件： 控制遊戲的音訊播放。 如需詳細資訊，請參閱[加入聲音](tutorial--adding-sound.md)。

其餘的遊戲變數包含基本類型清單及其各自的遊戲內掛接點，以及遊戲特定資料和限制。

## <a name="next-steps"></a>後續步驟

現在，您或許好奇實際的轉譯引擎： 如何在更新基本型別上__轉譯__方法的呼叫取得轉變成像素在螢幕上。 這兩個部分會說明&mdash;[轉譯架構 i： 轉譯簡介](tutorial--assembling-the-rendering-pipeline.md)並[轉譯架構 II： 遊戲轉譯](tutorial-game-rendering.md)。 如果您對玩家控制項如何更新遊戲狀態感到更有興趣，請參閱[新增控制項](tutorial--adding-controls.md)。
