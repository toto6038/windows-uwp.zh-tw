---
author: joannaleecy
title: 管理遊戲流程
description: 了解如何初始化遊戲狀態、處理事件，以及設定遊戲更新迴圈。
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, games, directx, 遊戲
ms.localizationpriority: medium
ms.openlocfilehash: 610b794c0ded6791e93c14d8960366132afd973b
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6841659"
---
# <a name="game-flow-management"></a>管理遊戲流程

遊戲現在會有一個視窗，非同步登錄兩個事件處理常式，並且載入資產登記。 本章節說明有關使用遊戲狀態、如何管理特定的主要遊戲狀態，以及如何建立遊戲引擎的更新迴圈。 然後，我們將會了解使用者介面流程，且最終能了解事件處理常式，以及 UWP 遊戲所需的事件詳細資訊。

>[!Note]
>如果您尚未下載此範例的最新遊戲程式碼，請移至 [Direct3D 遊戲範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="game-states-used-to-manage-game-flow"></a>用於管理遊戲流程的遊戲狀態

我們使用遊戲狀態來管理遊戲流程。 您的遊戲可能會有任意數目的可能狀態，因為使用者可以隨時從暫停狀態繼續 UWP 遊戲應用程式。

對於此遊戲範例，當啟動時，其可能處於下列三種狀態的其中一種：
* 遊戲迴圈正在執行，而且正在進行某個關卡。
* 遊戲迴圈未執行，因為遊戲才剛結束。 (已設定高分記錄)
* 未啟動任何遊戲，或遊戲進行到兩個關卡之間。 (高分記錄為 0)

您可以定義所需狀態數量，視遊戲的需求而定。 同時，請注意您的 UWP 遊戲可以隨時終止，而在繼續進行遊戲時，玩家會希望從上一次的狀態繼續玩下去，就像沒有停止遊戲一樣。

## <a name="game-states-initialization"></a>遊戲狀態初始化

在遊戲初始化時，您不只專注於冷啟動遊戲，也要在暫停或終止之後重新啟動。 範例遊戲永遠儲存遊戲狀態，其外觀停留在執行中狀態。 

在暫停狀態中，遊戲暫停，但遊戲的資源仍在記憶體中。 

同樣地，繼續事件是要確定範例遊戲從上一次暫停或終止的地方繼續。 範例遊戲在終止之後重新啟動時會正常啟動，然後判斷上次的已知狀態，讓玩家能夠立即繼續進行遊戲。

根據狀態而定，會對玩家顯示不同的選項。 

* 如果遊戲要從關卡的中途繼續，它會顯示暫停狀態，重疊則會顯示一個繼續選項。
* 如果是從遊戲已完成的狀態繼續，則會顯示高分記錄和玩新遊戲的選項。
* 最後，如果遊戲在關卡尚未開始之前繼續，則重疊會對使用者顯示開始選項。

遊戲範例不會區分遊戲本身是冷啟動 (初次啟動遊戲，沒有暫停事件)，或是從暫停狀態繼續。 這是任何 UWP app 的適當設計。

在此範例中，遊戲狀態的初始化發生在 [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method)。

這是協助將流程視覺化的流程圖，其涵蓋初始化與更新迴圈。

* 當您檢查目前遊戲狀態時，初始化開始於 __[開始]__ 節點。 如需遊戲程式碼，請移至 [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method)。
* 如需更新迴圈的詳細資訊，請移至[更新遊戲引擎](#update-game-engine)。 如需遊戲程式碼，請移至 [__App::Update__](#appupdate-method)。

![我們遊戲的主要狀態電腦](images/simple-dx-game-flow-statemachine.png)

### <a name="gamemaininitializegamestate-method"></a>GameMain::InitializeGameState 方法

__InitializeGameState__ 方法從 [__GameMain__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L32-L131)建構函式類別呼叫，當 __GameMain__ 中類別物件建立於 [__App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123) 方法時，便會呼叫該方法。

```cpp

GameMain::GameMain(...)
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...

    create_task([this]()
    {
        ...

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState(); //Initialization of game states occurs here.
        
        ...
    
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
        
    }, task_continuation_context::use_current());
}

```

```cpp

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle of a level.
        // We are waiting for the user to continue the game.
        //...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        //...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
        m_uiControl->SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    m_uiControl->ShowGameInfoOverlay();
}

```

## <a name="update-game-engine"></a>更新遊戲引擎

在 [__App::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L127-L130) 方法中，它會呼叫 [__GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202)。 在此方法內，範例已經實作基本狀態電腦，以處理玩家可以採取的所有主要動作。 這個狀態電腦的最高層級負責處理遊戲載入、進行特定關卡或在遊戲暫停後 (系統或玩家暫停) 繼續之前所玩的關卡。

在遊戲範例中，遊戲可能處於 3 種主要狀態 (__UpdateEngineState__)：

1. __Waiting for resources__：遊戲迴圈正在循環，在資源 (尤其是圖形資源) 可用之前無法轉換。 當載入資源的非同步工作完成時，它會將狀態更新為 __ResourcesLoaded__。 當層級從磁碟、遊戲伺服器或雲端後端載入新資源時，這通常在層級之間發生。 在遊戲範例中，我們模擬這個行為，因為範例此時並不需要任何額外的個別關卡資源。
2. __Waiting for press__：遊戲迴圈正在循環，等候特定使用者輸入。 這項輸入是玩家載入遊戲、開始關卡或繼續關卡的動作。 範例程式碼將這些子狀態稱為 __PressResultState__ 列舉值。
3. 在 __Dynamics__ 中：遊戲迴圈正在執行且使用者正在玩遊戲。 當使用者在玩遊戲時，遊戲會檢查可能轉換的 3 種條件： 
    * __TimeExpired__：關卡設定的時間到期
    * __LevelComplete__：玩家完成關卡 
    * __LevelComplete__：玩家完成所有關卡

您的遊戲只在一種電腦包含多個較小狀態的電腦的狀態。 針對每個特定狀態，它必須以極特定的準則定義。 如何從一個狀態轉換到另一個狀態，必須分別根據使用者的輸入或系統的動作 (如載入資源圖形) 執行。 規劃您的遊戲的同時，請思考繪出整個遊戲流程，確保您已處理了所有使用者或系統可能採取的動作。 遊戲可以很複雜，所以狀態電腦是一個很強大的工具，可以協助將這些複雜程序視覺化，讓它變得更容易管理。

我們來看看以下更新迴圈的程式碼。

### <a name="appupdate-method"></a>App::Update 方法

用來更新遊戲引擎的狀態電腦結構

```cpp
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        //...
        break;

    case UpdateEngineState::ResourcesLoaded:
        //...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            //...
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when the player is playing, the work is handled by this Simple3DGame::RunGame method.
            switch (runState)
            {
            case GameState::TimeExpired:
                //...
                break;

            case GameState::LevelComplete:
                //...
                break;

            case GameState::GameComplete:
                //...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event
            m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded
            m_controller->Active(false);
        }
        break;
    }
}
```

## <a name="update-user-interface"></a>更新使用者介面

我們需要持續通知玩家系統的狀態，允許遊戲狀態根據玩家的動作和定玩家規則來變更。 包括此遊戲範例的許多遊戲，通常會利用使用者介面 (UI) 元素來向玩家顯示這項資訊。 UI 包含遊戲狀態的表示法，以及其他遊戲特有的資訊，例如分數或子彈，或是剩餘的機會次數。 UI 也稱為重疊，因為與主要圖形管線分開轉譯，而且放置在 3D 投影上層。 有些 UI 資訊也會呈現為平視顯示 (HUD)，讓使用者不必他們視線移開主要遊戲區即可獲得這些資訊。 在範例遊戲中，我們使用 Direct2D API 建立這個重疊。 我們也可以使用 XAML 來建立這個重疊，如[延伸遊戲範例](tutorial-resources.md)中的討論。

使用者介面有兩個元件：

-   HUD，包含遊戲目前狀態之分數和資訊。
-   暫停點陣圖，是遊戲暫停狀態期間上面重疊文字的黑色矩形。 這就是遊戲重疊。 我們會在[新增使用者介面](tutorial--adding-a-user-interface.md)中進一步討論。

不難想像，重疊也有狀態電腦。 重疊可以顯示關卡開始或遊戲結束的訊息。 基本上，它就是遊戲暫停或中斷時，用來顯示我們對使用者顯示之任何遊戲狀態資訊的畫布。

經過轉譯的覆疊可以是六個畫面的其中一個，視根據遊戲狀態而定： 
1. 遊戲開始的資源載入畫面
2. 遊戲統計播放畫面
3. 關卡開始訊息畫面
4. 在時間之內完成所有關卡的遊戲結束畫面
5. 時間用完的遊戲結束畫面
6. 暫停功能表畫面

將您的使用者介面與遊戲圖形管線分開，可讓您不用依賴遊戲圖形轉譯引擎獨立使用使用者介面，並大幅降低遊戲程式碼的複雜程度。

以下是遊戲範例如何建立重疊狀態電腦的結構。

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        //...
        break;

    case GameInfoOverlayState::LevelStart:
        //...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        //...
        break;

    case GameInfoOverlayState::GameOverExpired:
        //...
        break;

    case GameInfoOverlayState::Pause:
        //...
        break;
    }
}
```

## <a name="events-handling"></a>事件處理

我們的範例程式碼在 App.cpp 中的 **Initialize**、**SetWindow** 以及 **Load** 中登錄了數個特定事件的處理常式。 以下是需要在新增遊戲機制或開始圖形開發之前運作的重要事件。 這些事件是正確 UWP app 體驗的基礎。 因為 UWP app 可以隨時啟動、停用、調整大小、定格、解除定格、暫停或繼續，因此遊戲必須儘速登錄這些事件，並透過保持遊戲經驗順暢及使用者預期的方式來處理它們。

這些是此範例使用的事件處理常式，以及它們處理的事件。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">事件處理常式</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">OnActivated</td>
<td align="left">處理 <a href="https://msdn.microsoft.com/library/windows/apps/br225018"><strong>CoreApplicationView::Activated</strong></a>。 遊戲 app 已經被帶到前景，因此啟動主視窗。</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left">處理 <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>Graphics::Display::DisplayInformation::DpiChanged</strong></a>。 已變更顯示器的 DPI，而遊戲會隨之調整其資源。
<div class="alert">
<strong>注意：</strong>[<strong>CoreWindow</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh404559)座標是在[direct2d](https://msdn.microsoft.com/library/windows/desktop/dd370987)的 Dip （裝置獨立像素）。 因此，您必須通知 Direct2D 已變更 DPI，才能正確顯示任何 2D 資產或基本類型。
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left">處理 <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>Graphics::Display::DisplayInformation::OrientationChanged</strong></a>。 顯示器方向有所變更，而轉譯需要更新。</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated</td>
<td align="left">處理 <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>Graphics::Display::DisplayInformation::DisplayContentsInvalidated</strong></a>。 顯示器需要重新繪製，而您的遊戲需要再試一次經過轉譯。</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">處理 <a href="https://msdn.microsoft.com/library/windows/apps/br205859"><strong>CoreApplication::Resuming</strong></a>。 遊戲應用程式從暫停狀態還原遊戲。</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">處理 <a href="https://msdn.microsoft.com/library/windows/apps/br205860"><strong>CoreApplication::Suspending</strong></a>。 遊戲 app 將它的狀態儲存到磁碟。 它有 5 秒的時間將狀態儲存到存放區。</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">處理<a href="https://msdn.microsoft.com/library/windows/apps/hh701591"><strong>CoreWindow::VisibilityChanged</strong></a>。 遊戲 app 已經變更可見度，可以是已經變成可見，或因為另一個 app 成為可見而變成不可見。</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">處理 <a href="https://msdn.microsoft.com/library/windows/apps/br208255"><strong>CoreWindow::Activated</strong></a>。 遊戲 app 主視窗已經停用或啟動，因此必須移除焦點並暫停遊戲，或重新取得焦點。 在這兩種情況下，重疊會指示遊戲已經暫停。</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">處理 <a href="https://msdn.microsoft.com/library/windows/apps/br208261"><strong>CoreWindow::Closed</strong></a>。 遊戲 app 關閉主視窗並暫停遊戲。</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">處理 <a href="https://msdn.microsoft.com/library/windows/apps/br208283"><strong>CoreWindow::SizeChanged</strong></a>。 遊戲應用程式重新分配圖形資源及重疊，以容納大小變更，然後更新轉譯目標。</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>後續步驟

本主題中，我們已討論如何使用遊戲狀態管理整體遊戲流量，以及遊戲的多個不同狀態的電腦所組成。 我們也已經了解如何更新 UI，以及管理主應用程式鍵事件處理常式。 現在，我們已經準備好深入轉譯迴圈、遊戲及其機制。
 
您可以瀏覽其他以任何順序構成此遊戲的元件：
* [定義主要遊戲物件](tutorial--defining-the-main-game-loop.md)
* [轉譯架構 I：轉譯簡介](tutorial--assembling-the-rendering-pipeline.md)
* [轉譯架構 II：遊戲轉譯](tutorial-game-rendering.md)
* [新增使用者介面](tutorial--adding-a-user-interface.md)
* [新增控制項](tutorial--adding-controls.md)
* [新增音效](tutorial--adding-sound.md)