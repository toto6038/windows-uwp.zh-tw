---
title: 定義主要遊戲物件
description: 現在，我們來看看範例遊戲的主要物件的詳細資料，以及它所實行的規則如何轉譯成與遊戲世界的互動。
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 主要物件
ms.localizationpriority: medium
ms.openlocfilehash: 497a1f0dc16308b4b9360aff958b94f04b6283ae
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162992"
---
# <a name="define-the-main-game-object"></a>定義主要遊戲物件

> [!NOTE]
> 本主題是使用 DirectX 教學課程系列 [建立簡單通用 Windows 平臺 (UWP) 遊戲](tutorial--create-your-first-uwp-directx-game.md) 的一部分。 該連結的主題會設定數列的內容。

一旦您配置了範例遊戲的基本架構，並且實行了可處理高階使用者和系統行為的狀態機器，您將會想要檢查將範例遊戲轉換成遊戲的規則和機制。 讓我們看看範例遊戲主要物件的詳細資料，以及如何將遊戲規則轉譯成與遊戲世界的互動。

## <a name="objectives"></a>目標

- 瞭解如何套用基本的開發技術，以實行適用于 UWP DirectX 遊戲的遊戲規則和機制。

## <a name="main-game-object"></a>主要遊戲物件

在 **Simple3DGameDX** 範例遊戲中， **Simple3DGame** 是主要的遊戲物件類別。 **Simple3DGame**的實例會透過**應用程式：： Load**方法間接建立。

以下是 **Simple3DGame** 類別的一些功能。

- 包含遊戲邏輯的執行。
- 包含傳達這些詳細資料的方法。
  - 將遊戲狀態變更為應用程式架構中定義的狀態機器。
  - 從應用程式到遊戲物件本身的遊戲狀態變更。
  - 更新遊戲 UI 的詳細資料 (重迭和列印頭顯示) 、動畫和物理)  (。
  > [!NOTE]
  > 圖形的更新是由 **GameRenderer** 類別所處理，該類別包含取得和使用遊戲所使用之圖形裝置資源的方法。 如需詳細資訊，請參閱轉譯 [架構 I：轉譯簡介](tutorial--assembling-the-rendering-pipeline.md)。
- 可作為定義遊戲會話、層級或存留期之資料的容器，取決於您在高階定義遊戲的方式。 在此情況下，遊戲狀態資料適用于遊戲的存留期，並且會在使用者啟動遊戲時初始化一次。

若要查看這個類別所定義的方法和資料，請參閱下面 [的 Simple3DGame 類別](#the-simple3dgame-class) 。

## <a name="initialize-and-start-the-game"></a>初始化和啟動遊戲

當玩家啟動遊戲時，遊戲物件必須初始化它的狀態，建立和新增重疊，設定追蹤玩家表現的變數，以及具現化用來建立關卡的物件。 在此範例中，這是在**應用程式：： Load**中建立**GameMain**實例時完成的。

**Simple3DGame**類型的遊戲物件是在**GameMain：： GameMain**函式中建立的。 然後，它會使用 **Simple3DGame：： Initialize** 方法，在 **GameMain：： ConstructInBackground** 引發和遺忘協同程式（從 **GameMain：： GameMain**呼叫）進行初始化。

### <a name="the-simple3dgameinitialize-method"></a>Simple3DGame：： Initialize 方法

範例遊戲會在遊戲物件中設定這些元件。

- 已建立新的視訊播放物件。
- 已建立遊戲的圖形基本型別陣列，包括關卡基本型別、彈藥及障礙物陣列。
- 已建立儲存遊戲狀態資料的位置 (名為 *Game*)，並放在 [**ApplicationData::Current**](/uwp/api/windows.storage.applicationdata.current) 指定的應用程式資料設定儲存位置。
- 已建立遊戲計時器與初始的遊戲內重疊點陣圖。
- 已使用一組特定的檢視與投射參數建立新的相機。
- 輸入裝置 (控制器) 設定為與相機相同的起始傾斜度與偏航角，因此玩家在起始控制位置與相機位置之間是 1 對 1 的對應。
- 已建立玩家物件並設定為使用中。 我們會使用球體物件來偵測玩家與牆和障礙物的距離，並讓相機進入可能會中斷深度的位置。
- 已建立遊戲世界基本型別。
- 已建立圓柱形障礙物。
- 已建立目標 (**Face** 物件) 並設置編號。
- 已建立子彈。
- 已建立關卡。
- 已載入高分記錄。
- 已載入所有先前儲存的遊戲狀態。

遊戲現在具有 &mdash; 世界、玩家、障礙、目標和 ammo 球體的所有重要元件的實例。 它也擁有關卡的執行個體，代表上述所有元件的設定，以及它們在每個特定關卡的行為。 現在讓我們看看遊戲如何建立這些層級。

## <a name="build-and-load-game-levels"></a>組建和載入遊戲等級

在 `Level[N].h/.cpp` 範例方案的 **GameLevels** 資料夾中找到的檔案中，會進行層級結構的大部分繁重工作。 因為它著重于非常特定的執行，所以我們不會在此討論。 重要的是，每個層級的程式碼都是以不同的 **層級 [N]** 物件來執行。 如果您想要擴充遊戲，您可以建立一個 **層級 [N]** 物件，該物件會接受指派的數位作為參數，並隨機放置障礙物和目標。 或者，您可以讓它從資源檔（甚至是網際網路）載入層級的設定資料。

## <a name="define-the-gameplay"></a>定義遊戲

到目前為止，我們已經擁有開發遊戲所需的所有元件。 層級已在原始物件的記憶體中建立，並已準備好讓玩家開始與互動。

最佳的遊戲會立即對玩家輸入做出反應，並提供立即的意見反應。 這種情況適用于任何類型的遊戲，從 twitch 行動、即時的玩射擊到審慎、輪流的策略遊戲。

### <a name="the-simple3dgamerungame-method"></a>Simple3DGame：： RunGame 方法

當遊戲等級正在進行時，遊戲會處於「 **動態** 」狀態。 

**GameMain：： Update** 是在每個畫面格更新應用程式狀態一次的主要更新迴圈，如下所示。 Update 迴圈會呼叫 **Simple3DGame：： RunGame** 方法，以在遊戲處於「 **動態** 」狀態時處理工作。

```cppwinrt
// Updates the application state once per frame.
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update();

    switch (m_updateState)
    {
    ...
    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            ...
        }
        else
        {
            // When the player is playing, work is done by Simple3DGame::RunGame.
            GameState runState = m_game->RunGame();
            switch (runState)
            {
                ...
```
          
**Simple3DGame：： RunGame** 會處理一組資料，以定義遊戲迴圈目前反復專案的目前狀態。

以下是 **Simple3DGame：： RunGame**中的遊戲流程邏輯。

- 方法會更新計時器，以在完成層級之前將計數減少，並測試以查看層級的時間是否已過期。 當時間結束時，這是遊戲的其中一項規則 &mdash; ，如果並非所有目標都已在使用中，則會進行遊戲。
- 如果時間已用盡，則方法會設定 **TimeExpired** 遊戲狀態，並回到先前程式碼中的 **Update** 方法。
- 如果保留時間，則會輪詢移動外觀控制器以取得相機位置的更新;具體而言，就是從相機平面投射的觀點， (播放程式) 的角度，以及從上一次輪詢控制器以來移動角度的距離。
- 相機會依據移動視角控制器中的新資料進行更新。
- 更新動態，或是遊戲世界中與玩家控制無關的動畫及物件行為。 在這個範例遊戲中，會呼叫 **Simple3DGame：： UpdateDynamics** 方法，以更新已引發之 ammo 球體的動作、要件障礙的動畫以及目標的移動。 如需詳細資訊，請參閱 [更新遊戲世界](#update-the-game-world)。
- 方法會檢查是否符合層級的成功完成準則。 如果是，它會完成層級的分數，並檢查這是否為最後一層 (6) 。 如果是最後一個層級，則此方法會傳回 **GameState：： GameComplete** 遊戲狀態;否則，它會傳回 **GameState：： LevelComplete** 遊戲狀態。
- 如果層級未完成，則方法會將遊戲狀態設為 **GameState：： Active**，然後傳回。

## <a name="update-the-game-world"></a>更新遊戲世界

在此範例中，當遊戲執行時，會從**Simple3DGame：： RunGame**方法呼叫**Simple3DGame：： UpdateDynamics**方法 (從**GameMain：： Update**) 呼叫該方法，以更新在遊戲場景中轉譯的物件。

迴圈（例如 **UpdateDynamics** ）會呼叫任何方法，這些方法是用來設定運動中的遊戲世界（與玩家輸入無關），以建立沉浸式遊戲體驗，並讓層級保持運作。 這包括需要轉譯的圖形，以及執行的動畫迴圈，即使沒有播放程式輸入，也能帶出動態世界。 在您的遊戲中，這可能包括 swaying 的樹狀結構、沿著支援腳步線的波浪 cresting、機械吸煙，以及外部程式碼延展和四處移動。 它也包含物件之間的互動，像是玩家環境和世界之間的碰撞，或子彈與障礙物和目標之間的撞擊。

除非遊戲特別暫停，否則遊戲迴圈應該繼續更新遊戲世界;這是以遊戲邏輯、實體演算法，還是單純的隨機方式來決定。

在範例遊戲中，此原則稱為 *dynamics*，它包含要件障礙的上升和下降，以及 ammo 球體的移動和實體行為（當它們被引發且在移動時）。

### <a name="the-simple3dgameupdatedynamics-method"></a>Simple3DGame：： UpdateDynamics 方法 

這個方法會處理這四組計算。

- 發射的子彈在世界的位置。
- 柱型障礙物的動畫。
- 玩家與世界界限的交集。
- 子彈與障礙物、目標、其他子彈和世界的碰撞。

障礙的動畫會在 **動畫 .h** 的程式碼檔中定義的迴圈中進行。 Ammo 和任何衝突的行為是由程式碼中提供的簡化物理演算法所定義，並由遊戲世界的一組全域常數（包括引力和材質屬性）進行參數化。 這些全都是在遊戲世界座標空間中計算的。

### <a name="review-the-flow"></a>檢查流程

既然我們已更新場景中的所有物件，並計算出任何衝突，就需要使用此資訊來繪製對應的視覺效果變更。 

在 **GameMain：： Update** 完成遊戲迴圈的目前反復專案之後，此範例會立即呼叫 **GameRenderer：： Render** 來取得更新的物件資料，並產生新的場景來呈現給播放程式，如下所示。

```cppwinrt
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
                ...
                // Otherwise, fall through and do normal processing to perform rendering.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                    CoreProcessEventsOption::ProcessAllIfPresent);
                // GameMain::Update calls Simple3DGame::RunGame. If game is in Dynamics
                // state, uses Simple3DGame::UpdateDynamics to update game world.
                Update();
                // Render is called immediately after the Update loop.
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="render-the-game-worlds-graphics"></a>呈現遊戲世界的圖形

我們建議遊戲中的圖形經常更新，在理想的情況下，通常會與主要遊戲迴圈逐一查看的方式完全相同。 當迴圈進行迴圈時，遊戲世界的狀態會更新，不論是否有播放程式輸入。 這可讓您順暢地顯示所計算的動畫和行為。 假設我們只有一個水的簡單場景，只要玩家按下按鈕就會移動。 這並不切合實際，好的遊戲隨時都有流暢且流暢的外觀。

回想一下如上述 **GameMain：： Run**所示的範例遊戲迴圈。 如果遊戲的主視窗是可見的，而未貼齊或停用，則遊戲會繼續更新並轉譯該更新的結果。 我們接著檢查的 **GameRenderer：： Render** 方法會呈現該狀態的標記法。 這會在呼叫 **GameMain：： update**之後立即完成，其中包括用來更新狀態的 **Simple3DGame：： RunGame** ，如上一節中所述。

**GameRenderer：： Render** 繪製3d 世界的投影，然後在其上方繪製 Direct2D 重迭。 完成時，它會顯示最後的交換鏈結，包含結合的顯示緩衝區。

> [!NOTE]
> 範例遊戲的 Direct2D 重迭有兩種狀態 &mdash; ，其中的遊戲顯示包含 [暫停] 功能表點陣圖的遊戲資訊重迭，另一個則是遊戲顯示十字線以及觸控線移動外觀控制器的矩形。 分數文字會同時在這兩種狀態繪製。 如需詳細資訊，請參閱[轉譯架構 I：轉譯簡介](tutorial--assembling-the-rendering-pipeline.md)。

### <a name="the-gamerendererrender-method"></a>GameRenderer：： Render 方法

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            ...
            for (auto&& object : m_game->RenderObjects())
            {
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud.Render(m_game);
        }

        if (m_gameInfoOverlay.Visible())
        {
            d2dContext->DrawBitmap(
                m_gameInfoOverlay.Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        ...
    }
}
```

## <a name="the-simple3dgame-class"></a>Simple3DGame 類別

這些是 **Simple3DGame** 類別所定義的方法和資料成員。

### <a name="member-functions"></a>成員函數

**Simple3DGame**所定義的公用成員函式包含下列各項。

- **初始化**。 設定全域變數的起始值，並初始化遊戲物件。 這會在 [ [初始化並啟動遊戲](#initialize-and-start-the-game) ] 一節中討論。
- **LoadGame**。 初始化新的層級，並開始載入它。
- **LoadLevelAsync**。 初始化層級的協同程式，然後再叫用轉譯器上的另一個協同程式，以載入裝置特定層級的資源。 這個方法在另一個執行緒中執行；因此，從這個執行緒只能呼叫 [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 方法 (與 [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 方法相反)。 任何裝置內容方法都是在 **FinalizeLoadLevel** 方法中呼叫的。 如果您不熟悉非同步程式設計，請參閱 [使用 c + +/WinRT 的並行和非同步作業](../cpp-and-winrt-apis/concurrency.md)。
- **FinalizeLoadLevel**。 在主執行緒上完成載入關卡時需要完成的所有工作。 這包括任何呼叫 Direct3D 11 裝置內容 ([**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) 方法。
- **StartLevel**。 開始新層級的遊戲。
- **PauseGame**。 暫停遊戲。
- **RunGame**。 反覆執行遊戲迴圈。 如果遊戲狀態為 **Active**，那麼每次反覆執行遊戲迴圈時就會從 **App::Update** 呼叫它一次。
- **OnSuspending** 和 **OnResuming**。 分別暫停/繼續遊戲的音訊。

以下是私用成員函式。

- **LoadSavedState** 和 **SaveState**。 分別載入/儲存遊戲的目前狀態。
- **LoadHighScore** 和 **SaveHighScore**。 分別在遊戲中載入/儲存高分。
- **InitializeAmmo**。 為每一輪的開始，將做為子彈的每個球體物件狀態重設回它的原始狀態。
- **UpdateDynamics**。 這是很重要的方法，因為它會根據現成的動畫常式、物理和控制項輸入來更新所有遊戲物件。 這是定義遊戲的互動核心。 這會在「 [更新遊戲世界](#update-the-game-world) 」一節中討論。

其他公用方法為屬性存取子，可將遊戲和覆迭特定資訊傳回給應用程式架構以供顯示。

### <a name="data-members"></a>資料成員

當遊戲迴圈執行時，會更新這些物件。

- **MoveLookController** 物件。 表示播放玩家輸入。 如需詳細資訊，請參閱[新增控制項](tutorial--adding-controls.md)。
- **GameRenderer** 物件。 代表 Direct3D 11 轉譯器，其會處理所有裝置特定物件和其轉譯。 如需詳細資訊，請參閱轉譯 [架構 I](tutorial--assembling-the-rendering-pipeline.md)。
- **音訊** 物件。 控制遊戲的音訊播放。 如需詳細資訊，請參閱 [新增音效](tutorial--adding-sound.md)。

其餘的遊戲變數包含基本專案清單、其各自的遊戲中數量，以及遊戲播放特定的資料和條件約束。

## <a name="next-steps"></a>後續步驟

我們還在討論實際的轉譯引擎如何對 &mdash; 更新的基本專案**Render**的轉譯方法進行呼叫，以在您的螢幕上變成圖元。 這些層面涵蓋于兩個部分 &mdash; [：轉譯架構 I：](tutorial--assembling-the-rendering-pipeline.md)轉譯和轉譯[架構 II 的簡介：遊戲](tutorial-game-rendering.md)轉譯。 如果您對玩家控制項如何更新遊戲狀態感興趣，請參閱 [新增控制項](tutorial--adding-controls.md)。