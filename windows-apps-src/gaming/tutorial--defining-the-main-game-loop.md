---
title: 定義主要遊戲物件
description: 現在，我們要詳細看看遊戲範例主要物件的詳細資料，以及如何將主要物件實作的規則轉譯為與遊戲世界的互動。
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 主要物件
ms.localizationpriority: medium
ms.openlocfilehash: a3c47f3c22c41e7ca73c8a8b5d4e26dc27fab343
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367655"
---
# <a name="define-the-main-game-object"></a>定義主要遊戲物件

一旦您已配置的範例遊戲的基本架構，並實作處理的高層級的使用者和系統行為的狀態機器，您會想要檢查的規則和變成遊戲的遊戲範例的機制。 讓我們看看遊戲範例的主要物件的詳細資料，以及如何將遊戲規則轉譯成與遊戲的世界互動。

>[!Note]
>如果您尚未下載此範例的最新遊戲程式碼，請移至 [Direct3D 遊戲範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目標

了解如何套用基本開發技術，以實作遊戲規則和 UWP DirectX 遊戲的機制。

## <a name="main-game-object"></a>主要遊戲物件

在此範例遊戲中， __Simple3DGame__是主要的遊戲物件類別。 執行個體__Simple3DGame__物件的建構中__App::Load__方法。

__Simple3DGame__類別物件：
* 指定的遊戲邏輯的實作
* 包含通訊的方法：
    * 在應用程式架構中定義的狀態機器的遊戲狀態的變更。
    * 從應用程式到遊戲物件本身的遊戲狀態的變更。
    * 更新遊戲的 UI （重疊和平視顯示窗）、 動畫和物理條件 (dynamics) 的詳細資料。

    >[!Note]
    >更新的圖形由__GameRenderer__類別，其中包含方法，以取得及使用遊戲所使用的圖形裝置資源。 如需詳細資訊，請參閱[轉譯架構 i:轉譯為您簡介](tutorial--assembling-the-rendering-pipeline.md)。

* 做為定義遊戲工作階段中，層級，資料或有存留期，取決於您如何定義您的遊戲在高層級的容器。 在此情況下，遊戲狀態資料存留期間的遊戲，並會初始化一次，當使用者啟動遊戲。

若要檢視的方法和此類別物件中定義的資料，請前往[Simple3DGame 物件](#simple3dgame-object)。

## <a name="initialize-and-start-the-game"></a>初始化並啟動遊戲

當玩家啟動遊戲時，遊戲物件必須初始化它的狀態，建立和新增重疊，設定追蹤玩家表現的變數，以及具現化用來建立關卡的物件。 在此範例中，這是當新__GameMain__中建立執行個體[ __App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123)。 

遊戲的物件， __Simple3DGame__，在中建立__GameMain__建構函式。 然後初始化使用[ __Simple3DGame::Initialize__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L54-L250)方法期間[非同步建立任務__GameMain__建構函式](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L65-L74).

### <a name="simple3dgameinitialize-method"></a>Simple3DGame::Initialize 方法

遊戲的範例設定中的遊戲物件的下列元件：

* 已建立新的視訊播放物件。
* 已建立遊戲的圖形基本型別陣列，包括關卡基本型別、彈藥及障礙物陣列。
* 已建立儲存遊戲狀態資料的位置 (名為 *Game*)，並放在 [**ApplicationData::Current**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) 指定的應用程式資料設定儲存位置。
* 已建立遊戲計時器與初始的遊戲內重疊點陣圖。
* 已使用一組特定的檢視與投射參數建立新的相機。
* 輸入裝置 (控制器) 設定為與相機相同的起始傾斜度與偏航角，因此玩家在起始控制位置與相機位置之間是 1 對 1 的對應。
* 已建立玩家物件並設定為使用中。 偵測玩家的靠近牆面和障礙，並防止相機取得放入的位置時，可能會中斷訓練活動，我們會使用的球面物件。
* 已建立遊戲世界基本型別。
* 已建立圓柱形障礙物。
* 已建立目標 (**Face** 物件) 並設置編號。
* 已建立子彈。
* 已建立關卡。
* 已載入高分記錄。
* 已載入所有先前儲存的遊戲狀態。

現在遊戲擁有所有關鍵元件的執行個體：世界、玩家、障礙物、目標以及子彈。 它也擁有關卡的執行個體，代表上述所有元件的設定，以及它們在每個特定關卡的行為。 現在來看看遊戲如何建立關卡。

## <a name="build-and-load-game-levels"></a>建置和載入遊戲的層級

大部分的苦差事的層級的建構在完成__Level.h/.cpp__檔案中找到__GameLevels__範例方案的資料夾。 它著重於非常特定的實作，因為我們不會涵蓋它們這裡。 重要的是每個關卡的程式碼都會做為個別 __LevelN__ 物件來執行。 如果您想要擴充的遊戲，您可以建立**層級**物件，其採用指定的數字做為參數和隨機放置的障礙和目標。 或者，您可以將它從資源檔或甚至是網際網路載入層級的組態資料。

## <a name="define-the-game-play"></a>定義玩遊戲

到目前為止，我們已經具備組成遊戲所需的全部元件。 層級建構基本類型，從記憶體中，而且已準備好讓玩家開始與互動。

T 棒的遊戲會立即回應使用者輸入，並提供立即的意見。 這適用於任何類型的遊戲時，從 twitch-即時射擊縝密的回合式策略遊戲動作。

### <a name="simple3dgamerungame-method"></a>Simple3DGame::RunGame 方法

遊戲播放時的層級，已處於__Dynamics__狀態。 

[__GameMain::Update__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)是更新一次每個畫面的應用程式狀態，如下所示的主要更新迴圈。 在更新迴圈中，它會呼叫[ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)方法來處理工作，如果遊戲處於__Dynamics__狀態。

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
          
[__Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)處理一組定義的遊戲的遊戲迴圈目前的反覆項目的目前狀態的資料。

遊戲中的流程邏輯__RunGame__:
*  此方法會更新關卡結束前用來倒數計時 (秒) 的計時器，並測試關卡時間是否已經用完。 這是其中一個遊戲的規則： 當時間，並擁有無法擷取所有目標，是遊戲。
*  如果時間結束，此方法會設定 **TimeExpired** 遊戲狀態，並回到先前的程式碼中的 **Update** 方法。
*  如果還有時間，會輪詢移動視角控制器以更新相機位置；具體而言，就是更新從相機平面 (即玩家的視線) 正常投射的檢視角度，以及自上次輪詢控制器後該角度所移動的距離。
*  相機會依據移動視角控制器中的新資料進行更新。
*  更新動態，或是遊戲世界中與玩家控制無關的動畫及物件行為。 在這個遊戲的範例中， [ __UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856)呼叫方法來更新 pillar 障礙的動畫和目標的移動，已被引發彈藥球體的動作。 如需詳細資訊，請參閱 <<c0> [ 更新遊戲的世界](#update-the-game-world)。
*  此方法會檢查是否已達到過關的條件。 如果達到，它會計算關卡的最後分數，並檢查是否為最後一關 (共 6 關)。 如果是最後一關，此方法會傳回 **GameComplete** 遊戲狀態；否則會傳回 __LevelComplete__ 遊戲狀態。
*  如果關卡尚未完成，此方法會將遊戲狀態設為 __Active__ 並回到這一關。

## <a name="update-the-game-world"></a>更新遊戲的世界

在此範例中，當執行遊戲時， [ __Simple3DGame::UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856)呼叫方法[ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)方法 (這從呼叫[ __GameMain::Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)) 更新遊戲場景中呈現的物件。

在  __UpdateDynamics__迴圈、 呼叫方法，用於設定遊戲的世界中移動，獨立於使用者輸入中，建立沉浸式遊戲體驗，並將層級隨附*運作*。 這包括需要轉譯的圖形，然後執行中的動畫進行迴圈正視維生，覺得真實，即使沒有任何使用者輸入。 比方說，搖晃中按風向一波一波 cresting 海岸線條、 機制吸煙和自動縮放和移動的外星人 monsters 沿著樹狀結構。 它也包含物件之間的互動，像是玩家環境和世界之間的碰撞，或子彈與障礙物和目標之間的撞擊。

遊戲迴圈應該永遠保持更新遊戲的世界，不論是它根據遊戲邏輯實體的演算法，，或是否只是單純的隨機的除了遊戲是否特別暫停。 

在遊戲範例中，這個原則就稱為「動態」  ，它包含柱型障礙物的升起和落下，還有子彈發射時的動作及物理行為。 

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame::UpdateDynamics 方法 

這個方法會處理 4 組計算：

* 發射的子彈在世界的位置。
* 柱型障礙物的動畫。
* 玩家與世界界限的交集。
* 子彈與障礙物、目標、其他子彈和世界的碰撞。

障礙物動畫是在 **Animate.h/.cpp** 中定義的迴圈。 簡化的物理演算法所定義、 程式碼中提供和由一組用於遊戲的世界中，包括重力和材質屬性的全域常數參數化彈藥和任何衝突的行為。 這些全都是在遊戲世界座標空間中計算的。

### <a name="review-the-flow"></a>檢閱流程

既然我們已更新場景中的所有物件，並計算任何衝突，我們需要使用此資訊來繪製對應的視覺變更。 

在後__GameMain::Update()__ 完成目前的反覆項目遊戲迴圈 （loop），此範例立即呼叫__render （)__ 來取得更新的物件資料並產生要呈現給播放器，為新場景如下所示。 接下來，讓我們看看轉譯。

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

## <a name="render-the-game-worlds-graphics"></a>轉譯遊戲的世界圖形

建議您盡可能更新遊戲中的圖形，最多可以在每次主要遊戲迴圈反覆執行時就進行更新。 在迴圈反覆執行時，無論玩家有沒有提供輸入，都會更新遊戲。 這樣可以讓計算過的動畫和行為能夠流暢地顯示。 想像一下，如果我們有一個關於水的簡單場景，但是玩家必須按下按鈕才能流動。 這會是一個很無聊的視覺效果。 優質的遊戲看起來要很平穩流暢。

在如上所示，您應該記得範例遊戲迴圈[ __GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202)。 如果可以看到遊戲的主視窗且主視窗沒有定格或停用，遊戲會持續更新並轉譯這個更新的結果。 [__呈現__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameRenderer.cpp#L474-L624)我們所檢查的方法現在會呈現該狀態的表示法。 這是在呼叫之後立即**更新**，其中包括**RunGame**上一節中討論的更新狀態。

這個方法會繪製 3D 世界的投影，然後在上面繪製 Direct2D 重疊。 完成時，它會顯示最後的交換鏈結，包含結合的顯示緩衝區。

>[!Note]
>有範例遊戲的 Direct2D 重疊的兩種狀態： 一個遊戲的遊戲的資訊覆疊，其中包含 [暫停] 功能表中，而遊戲的十字，以及以觸控式螢幕移動了矩形的顯示位置點陣圖的顯示位置控制站。 分數文字會同時在這兩種狀態繪製。 如需詳細資訊，請參閱[轉譯架構 i:轉譯為您簡介](tutorial--assembling-the-rendering-pipeline.md)。

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

這些是中所定義的資料與方法__Simple3DGame__物件類別。

### <a name="methods"></a>方法

上定義的內部方法**Simple3DGame**包括：

-   **初始化**:設定全域變數的起始值以及初始化遊戲物件。 說明這一點[初始化並啟動遊戲](#initialize-and-start-the-game)一節。
-   **LoadGame**:初始化新的關卡並開始載入。
-   **LoadLevelAsync**:啟動一項非同步工作 (如果您不熟悉非同步工作，請參閱[平行模式程式庫](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl)) 初始化層級，然後再叫用一項非同步工作上所載入的裝置特定層級資源的轉譯器。 這個方法在另一個執行緒中執行；因此，從這個執行緒只能呼叫 [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 方法 (與 [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 方法相反)。 任何裝置內容方法都是在 **FinalizeLoadLevel** 方法中呼叫的。
-   **FinalizeLoadLevel**:在主執行緒上完成載入關卡時需要完成的所有工作。 這包括任何呼叫 Direct3D 11 裝置內容 ([**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) 方法。
-   **StartLevel**:以新關卡開始遊戲。
-   **PauseGame**:暫停遊戲。
-   **RunGame**:反覆執行遊戲迴圈。 如果遊戲狀態為 **Active**，那麼每次反覆執行遊戲迴圈時就會從 **App::Update** 呼叫它一次。
-   **OnSuspending**並**OnResuming**:分別暫停和繼續遊戲的音訊。

私用方法：

-   **LoadSavedState**並**SaveState**:分別載入和儲存遊戲的目前狀態。
-   **SaveHighScore**並**LoadHighScore**:分別儲存和載入整個遊戲的高分記錄。
-   **InitializeAmmo**:為每一輪的開始，將做為子彈的每個球體物件狀態重設回它的原始狀態。
-   **UpdateDynamics**:這是一個重要的方法，因為它會依據已經定義的動畫常式、物理特性和控制項輸入更新所有遊戲物件。 這是定義遊戲的互動核心。 說明這一點[更新遊戲的世界](#update-the-game-world)一節。

其他公用方法則是 getter 屬性，它會將遊戲和重疊的特定資訊傳回到要顯示的 app 架構。

### <a name="data"></a>資料

在程式碼範例頂端有 4 個物件，其執行個體會隨著遊戲迴圈執行而更新。

-   **MoveLookController**物件：代表使用者輸入。 如需詳細資訊，請參閱[新增控制項](tutorial--adding-controls.md)。
-   **GameRenderer**物件：代表衍生自 Direct3D 11 轉譯器**DirectXBase**類別會處理所有裝置的特定物件和其轉譯。 如需詳細資訊，請參閱 <<c0> [ 轉譯架構我](tutorial--assembling-the-rendering-pipeline.md)。
-   **音訊**物件：控制遊戲音訊的播放。 如需詳細資訊，請參閱 <<c0> [ 新增音效](tutorial--adding-sound.md)。

其餘的遊戲變數包含基本類型清單及其各自的遊戲內掛接點，以及遊戲特定資料和限制。

## <a name="next-steps"></a>後續步驟

到目前為止，您可能想知道實際轉譯引擎： 如何呼叫__呈現__更新的基本項目上的方法取得螢幕上時變成像素為單位。 這在兩個部分涵蓋&mdash;[轉譯架構 i:轉譯為您簡介](tutorial--assembling-the-rendering-pipeline.md)和[轉譯架構 II:遊戲轉譯](tutorial-game-rendering.md)。 如果您對玩家控制項如何更新遊戲狀態感到更有興趣，請參閱[新增控制項](tutorial--adding-controls.md)。
