---
title: 管理遊戲流程
description: 了解如何初始化遊戲狀態、處理事件，以及設定遊戲更新迴圈。
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, games, directx
ms.localizationpriority: medium
ms.openlocfilehash: 181eca743a9ccdc76ebfc1302e8bb04d85a32269
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409627"
---
# <a name="game-flow-management"></a>管理遊戲流程

> [!NOTE]
> 本主題是使用 DirectX 教學課程系列[建立簡單的通用 Windows 平臺（UWP）遊戲](tutorial--create-your-first-uwp-directx-game.md)的一部分。 該連結的主題會設定數列的內容。

遊戲現在有一個視窗，已註冊一些事件處理常式，並以非同步方式載入了資產。 本主題說明遊戲狀態的使用方式、如何管理特定的重要遊戲狀態，以及如何為遊戲引擎建立更新迴圈。 然後，我們將瞭解使用者介面流程，最後進一步瞭解 UWP 遊戲所需的事件處理常式。

## <a name="game-states-used-to-manage-game-flow"></a>用於管理遊戲流程的遊戲狀態

我們使用遊戲狀態來管理遊戲流程。

當**Simple3DGameDX**範例遊戲在機器上首次執行時，它會處於未啟動任何遊戲的狀態。 遊戲執行的後續時間，可以是任何一種狀態。

- 未啟動任何遊戲，或遊戲在層級之間（高分數為零）。
- 遊戲迴圈正在執行，而且位於層級的中間。
- 遊戲迴圈未執行，因為遊戲已完成（高分具有非零的值）。

您的遊戲可以擁有所需的最多狀態。 但請記住，它可以隨時終止。 當它繼續時，使用者預期它會繼續處於終止時的狀態。

## <a name="game-state-management"></a>遊戲狀態管理

因此，在遊戲初始化期間，您必須支援冷啟動遊戲，以及在停止遊戲後繼續遊戲。 **Simple3DGameDX**範例一律會儲存其遊戲狀態，以提供絕對不會停止的印象。

為了回應暫止事件，遊戲已暫止，但遊戲的資源仍在記憶體中。 同樣地，系統會處理 resume 事件，以確保範例遊戲會在暫停或終止時，以其所在的狀態收取。 根據狀態而定，會對玩家顯示不同的選項。 

- 如果遊戲繼續進行中，則會顯示 [已暫停]，而重迭會提供 [繼續] 選項。
- 如果遊戲在遊戲完成的狀態下繼續，則會顯示 [高分數] 和 [播放新遊戲的選項]。
- 最後，如果遊戲在層級開始之前繼續進行，則重迭會向使用者顯示啟動選項。

範例遊戲並不區分遊戲是否正在冷啟動、第一次啟動而沒有暫停事件，或是從暫停狀態繼續。 這是任何 UWP app 的適當設計。

在此範例中，遊戲狀態的初始化會發生在[**GameMain：： InitializeGameState**](#the-gamemaininitializegamestate-method)中（該方法的大綱會在下一節中顯示）。

以下是可協助您將流程視覺化的流程圖。 同時涵蓋初始化和更新迴圈。

- 當您檢查目前遊戲狀態時，初始化開始於 **[開始]** 節點。 針對遊戲程式碼，請參閱下一節中的[**GameMain：： InitializeGameState**](#the-gamemaininitializegamestate-method) 。
* 如需更新迴圈的詳細資訊，請移至[更新遊戲引擎](#update-game-engine)。 針對遊戲代碼，請移至[**GameMain：： Update**](#the-gamemainupdate-method)。

![我們遊戲的主要狀態電腦](images/simple-dx-game-flow-statemachine.png)

### <a name="the-gamemaininitializegamestate-method"></a>GameMain：： InitializeGameState 方法

**GameMain：： InitializeGameState**方法是透過**GameMain**類別的「函式」間接呼叫，這是在**應用程式：： Load**中建立**GameMain**實例的結果。

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();
    ...
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        ...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        ...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        ...
    }
    m_uiControl->ShowGameInfoOverlay();
}
```

## <a name="update-game-engine"></a>更新遊戲引擎

**App：： run**方法會呼叫**GameMain：： run**。 在**GameMain：： Run**中，是用來處理使用者可以採取之所有主要動作的基本狀態機器。 此狀態機器的最高等級會處理載入遊戲、播放特定層級，或在遊戲暫停（由系統或使用者）之後繼續執行層級。

在範例遊戲中，有3個主要狀態（由**UpdateEngineState**列舉代表）可供遊戲使用。

1. **UpdateEngineState：： WaitingForResources**。 遊戲迴圈正在循環，在資源 (尤其是圖形資源) 可用之前無法轉換。 當非同步資源載入工作完成時，我們會將狀態更新為**UpdateEngineState：： ResourcesLoaded**。 當層級從磁片、遊戲伺服器或雲端後端載入新資源時，通常會發生這種情況。 在範例遊戲中，我們會模擬此行為，因為此範例在當時並不需要任何額外的每一層資源。
2. **UpdateEngineState：： WaitingForPress**。 遊戲迴圈正在循環，等候特定使用者輸入。 此輸入是用來載入遊戲、啟動層級，或繼續層級的播放動作。 範例程式碼會透過**PressResultState**列舉來參考這些子狀態。
3. **UpdateEngineState：:D ynamics**。 遊戲迴圈正在執行且使用者正在玩遊戲。 當使用者在玩遊戲時，遊戲會檢查可能轉換的 3 種條件： 
 - **GameState：： TimeExpired**。 層級的時間限制到期。
 - **GameState：： LevelComplete**。 播放程式完成層級。
 - **GameState：： GameComplete**。 播放程式完成所有層級。

遊戲只是包含多個較小狀態機器的狀態機器。 每個特定狀態都必須以非常特定的準則來定義。 從某個狀態轉換到另一個狀態時，必須以個別的使用者輸入或系統動作（例如圖形資源載入）為基礎。

在規劃您的遊戲時，請考慮繪製整個遊戲流程，以確保您已解決使用者或系統可以採取的所有可能動作。 遊戲可能非常複雜，因此狀態機器是一種功能強大的工具，可協助您將此複雜性視覺化，並使其更容易管理。

讓我們看一下更新迴圈的程式碼。

### <a name="the-gamemainupdate-method"></a>GameMain：： Update 方法

這是用來更新遊戲引擎之狀態機器的結構。

```cppwinrt
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update(); 

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        ...
        break;

    case UpdateEngineState::ResourcesLoaded:
        ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            ...
        }
        break;

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
            case GameState::TimeExpired:
                ...
                break;

            case GameState::LevelComplete:
                ...
                break;

            case GameState::GameComplete:
                ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(
                m_renderer->GameInfoOverlayUpperLeft(),
                m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller
            // until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}
```

## <a name="update-the-user-interface"></a>更新使用者介面

我們需要持續通知玩家系統的狀態，允許遊戲狀態根據玩家的動作和定玩家規則來變更。 許多遊戲（包括此範例遊戲）通常會使用使用者介面（UI）元素，將此資訊提供給播放者。 UI 包含遊戲狀態的標記法，以及其他播放特定的資訊，例如分數、ammo，或剩餘的機會數。 UI 也稱為「重迭」，因為它會與主要圖形管線分開轉譯，並放在3D 投影上。

有些 UI 資訊也會顯示為標題顯示（抬頭顯示器），讓使用者可以看到該資訊，而不需要完全離開主要遊戲區域。 在範例遊戲中，我們會使用 Direct2D Api 來建立此重迭。 或者，我們可以使用 XAML 來建立這項重迭，我們將在[擴充範例遊戲](tutorial-resources.md)中加以討論。

使用者介面有兩個元件。

- 包含遊戲目前狀態之分數和資訊的抬頭顯示器。
- 暫停點陣圖，是遊戲暫停狀態期間上面重疊文字的黑色矩形。 這就是遊戲重疊。 我們會在[新增使用者介面](tutorial--adding-a-user-interface.md)中進一步討論。

不難想像，重疊也有狀態電腦。 重迭可以顯示層級開始或遊戲訊息。 這基本上是一個畫布，我們可以在遊戲暫停或暫止時，輸出想要顯示給播放程式之遊戲狀態的任何相關資訊。

轉譯的重迭可以是這六個畫面的其中一個，視遊戲的狀態而定。

1. 開始遊戲時的資源載入進度畫面。
2. 遊戲統計資料畫面。
3. 層級啟動訊息畫面。
4. 當所有層級都完成但沒有時間執行時，遊戲畫面。
5. 當時間用盡時的遊戲畫面。
6. 暫停功能表畫面。

將您的使用者介面與遊戲的圖形管線隔開，可讓您在遊戲的圖形轉譯引擎之外獨立作業，並大幅降低遊戲程式碼的複雜度。

以下是範例遊戲如何為重迭的狀態機器進行結構說明。

```cppwinrt
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        ...
        break;

    case GameInfoOverlayState::LevelStart:
        ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        ...
        break;

    case GameInfoOverlayState::Pause:
        ...
        break;
    }
}
```

## <a name="event-handling"></a>事件處理

如我們在[定義遊戲的 UWP 應用程式架構](tutorial--building-the-games-uwp-app-framework.md)主題中所見，**應用程式**類別的許多視圖提供者方法會註冊事件處理常式。 這些方法必須在加入遊戲機制或開始圖形開發之前，正確地處理這些重要的事件。

適當處理問題的事件是 UWP 應用程式體驗的基礎。 由於 UWP 應用程式可以隨時啟動、停用、調整大小、貼齊、unsnapped、暫停或繼續，遊戲必須儘快註冊這些事件，並以讓播放程式的體驗保持順暢且可預測的方式處理。

這些是此範例中所使用的事件處理常式，以及它們所處理的事件。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">事件處理常式</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">OnActivated</td>
<td align="left">處理 <a href="/uwp/api/windows.applicationmodel.core.coreapplicationview.activated"><strong>CoreApplicationView::Activated</strong></a>。 遊戲 app 已經被帶到前景，因此啟動主視窗。</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left">處理 <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>Graphics::Display::DisplayInformation::DpiChanged</strong></a>。 已變更顯示器的 DPI，而遊戲會隨之調整其資源。
<div class="alert">
<strong>注意</strong> <a href="/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow"><strong>CoreWindow</strong></a>座標是以與裝置無關的圖元（Dip）來進行<a href="/windows/desktop/Direct2D/direct2d-overview">Direct2D</a>。 因此，您必須通知 Direct2D 已變更 DPI，才能正確顯示任何 2D 資產或基本類型。
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left">處理 <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>Graphics::Display::DisplayInformation::OrientationChanged</strong></a>。 顯示器方向有所變更，而轉譯需要更新。</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated</td>
<td align="left">處理 <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>Graphics::Display::DisplayInformation::DisplayContentsInvalidated</strong></a>。 顯示器需要重新繪製，而您的遊戲需要再試一次經過轉譯。</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">處理 <a href="/uwp/api/windows.applicationmodel.core.coreapplication.resuming"><strong>CoreApplication::Resuming</strong></a>。 遊戲應用程式從暫停狀態還原遊戲。</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">處理 <a href="/uwp/api/windows.applicationmodel.core.coreapplication.suspending"><strong>CoreApplication::Suspending</strong></a>。 遊戲 app 將它的狀態儲存到磁碟。 它有 5 秒的時間將狀態儲存到存放區。</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">處理<a href="/uwp/api/windows.ui.core.corewindow.visibilitychanged"><strong>CoreWindow::VisibilityChanged</strong></a>。 遊戲 app 已經變更可見度，可以是已經變成可見，或因為另一個 app 成為可見而變成不可見。</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">處理 <a href="/uwp/api/windows.ui.core.corewindow.activated"><strong>CoreWindow::Activated</strong></a>。 遊戲 app 主視窗已經停用或啟動，因此必須移除焦點並暫停遊戲，或重新取得焦點。 在這兩種情況下，重疊會指示遊戲已經暫停。</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">處理 <a href="/uwp/api/windows.ui.core.corewindow.closed"><strong>CoreWindow::Closed</strong></a>。 遊戲 app 關閉主視窗並暫停遊戲。</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">處理 <a href="/uwp/api/windows.ui.core.corewindow.sizechanged"><strong>CoreWindow::SizeChanged</strong></a>。 遊戲應用程式重新分配圖形資源及重疊，以容納大小變更，然後更新轉譯目標。</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>後續步驟

在本主題中，我們已瞭解如何使用遊戲狀態來管理整體遊戲流程，而且遊戲是由多個不同的狀態機器所組成。 我們也已瞭解如何更新 UI，以及如何管理重要的應用程式事件處理常式。 現在我們已經準備好深入探討轉譯迴圈、遊戲和其機制。
 
您可以依照任何順序，逐一流覽本遊戲的其餘主題。

- [定義主要遊戲物件](tutorial--defining-the-main-game-loop.md)
- [轉譯架構 I：轉譯簡介](tutorial--assembling-the-rendering-pipeline.md)
- [轉譯架構 II：遊戲轉譯](tutorial-game-rendering.md)
- [新增使用者介面](tutorial--adding-a-user-interface.md)
- [加入控制項](tutorial--adding-controls.md)
- [加入聲音](tutorial--adding-sound.md)
