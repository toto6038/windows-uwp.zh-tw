---
author: mtoepke
title: "定義遊戲的通用 Windows 平台 (UWP) App 架構"
description: "撰寫通用 Windows 平台 (UWP) DirectX 遊戲程式碼的第一部分，是建立讓遊戲物件與 Windows 互動的架構。"
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 遊戲, DirectX"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 82a44a3499297b3988815ad10091cd351a194cbd
ms.lasthandoff: 02/07/2017

---

#  <a name="define-the-games-universal-windows-platform-uwp-app-framework"></a>定義遊戲的通用 Windows 平台 (UWP) App 架構


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

撰寫通用 Windows 平台 (UWP) DirectX 遊戲程式碼的第一部分，是建立讓遊戲物件與 Windows 互動的架構。 這包括下列 Windows 執行階段屬性：暫停/繼續事件處理、視窗焦點以及定格，還有使用者介面的事件、互動和轉換。 我們要看看如何架構範例遊戲，以及它如何為玩家及系統互動定義高階狀態電腦。

## <a name="objective"></a>目標


-   設定 UWP DirectX 遊戲的架構，以及實作定義整體遊戲流程的狀態電腦。

## <a name="initializing-and-starting-the-view-provider"></a>初始化並啟動檢視提供者


在任何 UWP DirectX 遊戲中，您都必須取得應用程式單例 (定義執行應用程式之執行個體的 Windows 執行階段物件) 的檢視提供者，以存取所需的圖形資源。 透過 Windows 執行階段，您的 app 可以直接與圖形介面連接，但是需要指定所需的資源及處理方式。

如我們在[設定遊戲專案](tutorial--setting-up-the-games-infrastructure.md)中所討論，Microsoft Visual Studio 2015 在 **Sample3DSceneRenderer.cpp** 檔案中提供了 DirectX 基本轉譯器的實作；當您挑選 **DirectX 11 應用程式 (通用 Windows)** 範本時，該檔案便可供使用。

如需了解及建立檢視提供者和轉譯器的詳細資料，請參閱[如何使用 C++ 和 DirectX 設定 UWP 來顯示 DirectX 檢視](https://msdn.microsoft.com/library/windows/apps/hh465077)。

簡單的來說，您必須提供應用程式單例呼叫的 5 種方法實作：

-   [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)
-   [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509)
-   [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501)
-   [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)
-   [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523)

在 DirectX 11 應用程式 (通用 Windows) 範本中，這 5 個方法定義在 [XApp.h](#complete-sample-code-for-this-section) 的 **App** 物件中。 我們來看看它們在這個遊戲中的實作方式。

檢視提供者的 Initialize 方法

```cpp
void App::Initialize(
    _In_ CoreApplicationView^ applicationView
    )
{
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();
}
```

應用程式單例首先呼叫 **Initialize**。 因此，這個方法必須處理大部分的 UWP 遊戲基礎行為， 例如處理主視窗的啟用，以及確定遊戲可以處理突然暫停 (可能在稍後繼續) 的事件。

當遊戲應用程式初始化時，它會為控制器配置特定的記憶體，以便讓玩家開始提供輸入。 它也會為遊戲的轉譯器和狀態電腦建立新的未初始化執行個體。 我們會在[定義主要遊戲物件](tutorial--defining-the-main-game-loop.md)中詳細討論這個部分。

現在，遊戲應用程式可以處理暫停 (或繼續) 訊息，而且擁有為控制器、轉譯器及遊戲本身配置的記憶體。 但是沒有可以使用的視窗，而且遊戲還未初始化。 我們還需要完成幾個工作！

檢視提供者的 SetWindow 方法

```cpp
void App::SetWindow(
    _In_ CoreWindow^ window
    )
{
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    ShowGameInfoOverlay();
}
```

現在，透過呼叫 [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509) 的實作，應用程式單例提供一個 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 物件來代表遊戲的主視窗，並將其中的資源和事件提供給遊戲使用。 因為現在有視窗可以使用了，遊戲可以開始新增基本使用者介面元件及事件：指標 (由滑鼠及觸控控制項使用)，以及視窗重新調整大小、關閉及 DPI 變更 (如果顯示裝置變更) 的基本事件。

遊戲應用程式也會初始化控制器 (因為有視窗可以互動) 及初始化遊戲物件本身。 它可以從控制器 (觸控、滑鼠或 XBox 360 控制器) 讀取輸入。

初始化控制器之後，應用程式在畫面的左下角和右下角分別為移動和相機觸控控制項定義兩個矩形區域。 玩家使用左下角矩形 (呼叫 **SetMoveRect** 來定義) 當作前後左右移轉相機的虛擬控制鍵。 右下角矩形 (由 **SetFireRect** 方法定義) 當作發射子彈的虛擬按鈕。

現在開始慢慢成形了。

檢視提供者的 Load 方法

```cpp
void App::Load(
    Platform::String^ entryPoint
    )
{
    task<void>([this]()
    {
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // in which the m_renderer object was created because the D3D device context
        // can be accessed only on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work, so
                // the finalizer code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can  be accessed only on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}
```

設定好主視窗後，應用程式單例會呼叫 **Load**。 在範例中，這個方法使用一組非同步工作 (它的語法定義於[平行模式程式庫](https://msdn.microsoft.com/library/windows/apps/dd492418.aspx) 中) 來建立遊戲物件、載入圖形資源以及初始化遊戲的狀態電腦。 使用非同步工作模式時，Load 方法即可快速完成並允許 app 開始處理輸入。 在這個方法中，app 也會在資源檔案載入時顯示進度列。

我們將資源載入分成兩個不同的階段，因為 Direct3D 11 裝置內容的存取受限於建立裝置內容的執行緒中，而為物件建立存取 Direct3D 11 裝置則是自由不受限的執行緒。 **CreateGameDeviceResourcesAsync** 工作在與完成工作 (*FinalizeCreateGameDeviceResources*) 的不同執行緒中執行，而完成工作是在原始執行緒中執行。 我們使用 **LoadLevelAsync** 和 **FinalizeLoadLevel** 時會以類似的模式來載入關卡資源。

在建立遊戲物件和載入圖形資源之後，我們將遊戲的狀態電腦初始化成開始狀況 (例如，設定初始子彈計數、關卡級數以及物件位置)。 如果遊戲狀態表示玩家是要繼續遊戲，我們要載入目前的關卡 (遊戲暫停時玩家所在的關卡)。

在 **Load** 方法中，我們要在遊戲開始前進行任何必要的準備，像是設定任何開始狀態或全域值。 如果您想要預先擷取遊戲資料或資產，這個方法相較於 **SetWindow** 或 **Initialize** 方法更為適合。 在遊戲中為任何載入使用非同步工作，因為 Windows 強制限制遊戲必須開始處理輸入之前可以使用的時間。 如果載入需要一些時間 — 如果有許多資源 — 那麼請為您的使用者提供定期更新的進度列。

開發您自己的遊戲時，請圍繞這些方法設計啟動程式碼。 這裡是每個方法基本建議的簡單清單：

-   使用 **Initialize** 配置主要類別並連接基本事件處理常式。
-   使用 **SetWindow** 建立主視窗並連接任何視窗特定事件。
-   使用 **Load** 處理任何其餘的設定，並初始化物件和載入資源的非同步建立。 如果需要建立任何暫存的檔案或資料 (如由程序產生的資產)，也請在這裡設定。

範例遊戲建立了遊戲狀態電腦的執行個體，並將它設定為開始設定。 它處理了所有的系統及輸入事件。 它提供了可以顯示內容的視窗。 遊戲程式碼已經準備就緒可以開始執行了。

檢視提供者的 Run 方法

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::Snapped:
                if (!m_renderNeeded)
                {
                    // The app is not currently the active window, so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // Otherwise, fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close.  Make sure to save the state.
}
```

現在，我們到了遊戲 app 的遊戲部分。 執行 3 個方法並設定遊戲的關卡，接著遊戲 app 執行 **Run** 方法，就可以開始盡情享受遊戲了！

在遊戲範例中，我們開始了一個會在玩家關閉遊戲視窗時終止的 while 迴圈。 範例程式碼會在遊戲引擎狀態電腦中轉換成兩種狀態的其中一種：

-   遊戲視窗會停用 (失去焦點) 或定格。 發生這種情況時，遊戲會暫停事件處理，並且等候視窗取得焦點或取消定格。
-   否則，遊戲會更新自己的狀態，並且轉譯圖形進行顯示。

當使用者與您的遊戲互動時，您必須處理位於訊息佇列的每個事件，因此必須使用 **ProcessAllIfPresent** 選項呼叫 [**CoreWindowDispatch.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)。 其他選項會在處理訊息事件時造成延遲，這會讓您的遊戲顯得沒有回應，或是讓觸控行為變得遲緩而不敏銳。

當然，app 處於不可見、暫停或定格的狀態時，我們不希望它佔用任何資源循環來發送永遠不會傳入的訊息。 所以您的遊戲必須使用 **ProcessOneAndAllPending**，它會持續封鎖直到取得事件，然後處理該事件，並處理在處理第一個事件期間抵達處理佇列中的任何其他事件。 接著，在處理完佇列後，立即傳回 [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)。

遊戲開始執行了！ 用來轉換遊戲狀態的事件正在進行發送及處理。 圖形在遊戲迴圈循環期間進行更新。 我們希望玩家玩得很開心。 但是遊戲也有結束的時候...

...我們需要開始清理環境了。 這時就需要使用 **Uninitialize**。

檢視提供者的 Uninitialize 方法

```cpp
void App::Uninitialize()
{
}
```

在遊戲範例中，我們讓遊戲的應用程式單例在遊戲結束後清理掉所有的物件。 在 Windows 10 中，關閉應用程式視窗並不會終止應用程式的程序，而是將應用程式單例的狀態寫入到記憶體。 如需在系統必須回收此記憶體時執行任何特殊的資源清理動作，請在這個方法中放入該清理動作的程式碼。

我們在這個教學課程中重新提到這 5 個方法，請牢記它們。 現在，我們來看看遊戲引擎的整體結構及定義它的狀態電腦。

## <a name="initializing-the-game-engine-state"></a>初始化遊戲引擎狀態


由於使用者可以隨時從暫停狀態繼續 UWP 遊戲應用程式，因此應用程式可能會有任意數目的可能狀態。

遊戲範例啟動時，可能處於下列三種狀態的其中一種：

-   遊戲迴圈正在執行，而且正在進行某個關卡。
-   遊戲迴圈未執行，因為遊戲才剛結束。 (已設定高分記錄。)
-   未啟動任何遊戲，或遊戲進行到兩個關卡之間。 (高分記錄為 0。)

很明顯地，您自己的遊戲中，可能會有更多或更少的狀態。 同時，請注意您的 UWP 遊戲可以隨時終止，而在繼續進行遊戲時，玩家會希望從上一次的狀態繼續玩下去，就像沒有停止遊戲一樣。

在遊戲範例中，程式碼流程看起來像這樣。

```cpp
void App::InitializeGameState()
{
    //
    // Set up the initial state machine for handling game playing state.
    //
    if (m_game->GameActive() && m_game->LevelActive())
    {
        m_updateState = UpdateEngineState::WaitingForResources;
        // ...

    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        m_updateState = UpdateEngineState::WaitingForPress;
        // ...
    }
    else
    {
        m_updateState = UpdateEngineState::WaitingForResources;
        // ...
    }
    SetAction(GameInfoOverlayCommand::PleaseWait);
    ShowGameInfoOverlay();
}
```

初始化與冷啟動 app 較不相關，而是較著重在 qpp 終止之後重新啟動。 範例遊戲會固定儲存狀態，讓應用程式看起來像是一直在執行。 暫停狀態是指：暫停玩遊戲，但遊戲的資源仍在記憶體中。 同樣地，繼續事件是指範例遊戲從上一次暫停或終止的地方繼續。 範例遊戲在終止之後重新啟動時會正常啟動，然後判斷上次的已知狀態，讓玩家能夠立即繼續進行遊戲。

流程圖會配置遊戲範例的初始化程序的初始狀態和轉換。

![主迴圈開始前初始化及準備遊戲的程序](images/simple3dgame-appstartup.png)

根據狀態而定，會對玩家顯示不同的選項。 如果遊戲要從關卡的中途繼續，它會顯示暫停狀態，重疊則會顯示一個繼續選項。 如果是從遊戲已完成的狀態繼續，則會顯示高分記錄和玩新遊戲的選項。 最後，如果遊戲在關卡尚未開始之前繼續，則重疊會對使用者顯示開始選項。

遊戲範例不會區分遊戲本身是冷啟動 (也就是初次啟動遊戲，沒有暫停事件)，或是從暫停狀態繼續。 這是任何 UWP app 的適當設計。

## <a name="handling-events"></a>處理事件


我們的範例程式碼在 **Initialize**、**SetWindow** 以及 **Load** 中登錄了數個特定事件的處理常式。 您可能猜到這些是重要事件，因為程式碼範例在進入任何遊戲機制或圖形開發前就先做好這項工作。 您猜得沒錯！ 這些事件是提供正確 UWP app 經驗的基礎，而且因為 UWP app 可以隨時啟動、停用、調整大小、定格、解除定格、暫停或繼續，因此遊戲必須儘速登錄這些特別的事件，並透過保持遊戲經驗順暢及使用者預期的方式來處理它們。

以下是範例中的事件處理常式，以及它們處理的事件。 您可以在[這個章節的完整程式碼](#complete-sample-code-for-this-section)中找到這些事件處理常式的完整程式碼。

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
<td align="left">處理 [<strong>CoreApplicationView::Activated</strong>](https://msdn.microsoft.com/library/windows/apps/br225018)。 遊戲 app 已經被帶到前景，因此啟動主視窗。</td>
</tr>
<tr class="even">
<td align="left">OnLogicalDpiChanged</td>
<td align="left">處理 [<strong>DisplayProperties::LogicalDpiChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br226150)。 主遊戲視窗的 DPI 已變更，而且遊戲 App 據此調整其資源。
<div class="alert">
<strong>注意</strong> [<strong>CoreWindow</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404559) 座標的單位是 DIP (裝置獨立畫素)，和在 [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370987) 中相同。 因此，您必須通知 Direct2D 已變更 DPI，才能正確顯示任何 2D 資產或基本類型。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">處理 [<strong>CoreApplication::Resuming</strong>](https://msdn.microsoft.com/library/windows/apps/br205859)。 遊戲應用程式從暫停狀態還原遊戲。</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">處理 [<strong>CoreApplication::Suspending</strong>](https://msdn.microsoft.com/library/windows/apps/br205860)。 遊戲 app 將它的狀態儲存到磁碟。 它有 5 秒的時間將狀態儲存到存放區。</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">處理 [<strong>CoreWindow::VisibilityChanged</strong>](https://msdn.microsoft.com/library/windows/apps/hh701591)。 遊戲 app 已經變更可見度，可以是已經變成可見，或因為另一個 app 成為可見而變成不可見。</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">處理 [<strong>CoreWindow::Activated</strong>](https://msdn.microsoft.com/library/windows/apps/br208255)。 遊戲 app 主視窗已經停用或啟動，因此必須移除焦點並暫停遊戲，或重新取得焦點。 在這兩種情況下，重疊會指示遊戲已經暫停。</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">處理 [<strong>CoreWindow::Closed</strong>](https://msdn.microsoft.com/library/windows/apps/br208261)。 遊戲 app 關閉主視窗並暫停遊戲。</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">處理 [<strong>CoreWindow::SizeChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br208283)。 遊戲應用程式重新分配圖形資源及重疊，以容納大小變更，然後更新轉譯目標。</td>
</tr>
</tbody>
</table>

 

您自己的遊戲必須處理這些事件，因為這些是 UWP 應用程式設計的一部分。

## <a name="updating-the-game-engine"></a>更新遊戲引擎


在 **Run** 的遊戲迴圈內，範例已經實作基本狀態電腦，以處理玩家可以採取的所有主要動作。 這個狀態電腦的最高層級負責處理遊戲載入、進行特定關卡或在遊戲暫停後 (系統或玩家暫停) 繼續之前所玩的關卡。

在遊戲範例中，遊戲可能處於 3 種主要狀態 (UpdateEngineState)：

-   **Waiting for resources**。 遊戲迴圈正在循環，在資源 (尤其是圖形資源) 可用之前無法轉換。 當載入資源的非同步工作完成時，它會將狀態更新為 **ResourcesLoaded**。 這種情況通常發生在兩個關卡之間，此時關卡需要從磁碟載入新資源。 在遊戲範例中，我們模擬這個行為，因為範例此時並不需要任何額外的個別關卡資源。
-   **Waiting for press**。 遊戲迴圈正在循環，等候特定使用者輸入。 這項輸入是玩家載入遊戲、開始關卡或繼續關卡的動作。 範例程式碼將這些子狀態稱為 PressResultState 列舉值。
-   **Dynamics**。 遊戲迴圈正在執行且使用者正在玩遊戲。 使用者正在玩遊戲時，遊戲會檢查可能轉換的 3 種條件：關卡設定的時間到期、玩家完成關卡，或玩家完成所有關卡。

以下是程式碼結構。 完整程式碼在[這個章節的完整程式碼](#complete-sample-code-for-this-section)中。

用來更新遊戲引擎的狀態電腦結構

```cpp
void App::Update()
{
    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        // Waiting for initial load.  Display an update once per 60 updates.
        loadCount++;
        if ((loadCount % 60) == 0)
        {
            m_loadingCount++;
            SetGameInfoOverlay(m_gameInfoOverlayState);
        }
        break;

    case UpdateEngineState::ResourcesLoaded:
        switch (m_pressResult)
        {
        case PressResultState::LoadGame:
            // ...
            break;

        case PressResultState::PlayLevel:
            // ...
            break;

        case PressResultState::ContinueLevel:
            // ...
            break;
        }
        // ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete() || m_pressComplete)
        {
            m_pressComplete = false;

            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                // ...
                break;

            case PressResultState::PlayLevel:
                // ...
                break;

            case PressResultState::ContinueLevel:
                // ...
                break;
            }
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested() || m_pauseRequested)
        {
            // ...
        }
        else 
        {
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                // ...
                break;

            case GameState::LevelComplete:
                // ...
                break;

            case GameState::GameComplete:
                // ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // transitioning state, so enable waiting for the press event
            m_controller->WaitForPress(m_game->GameInfoOverlayUpperLeft(), m_game->GameInfoOverlayLowerRight());
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

主要遊戲狀態電腦看起來就像這樣：

![我們遊戲的主要狀態電腦](images/simple3dgame-mainstatemachine.png)

我們會在[定義主要遊戲物件](tutorial--defining-the-main-game-loop.md)中詳細討論遊戲邏輯本身。 目前來說，最重要的是您的遊戲是狀態電腦。 每個特定狀態都必須有非常具體的定義條件，而且從一個狀態到另一個狀態的轉換，必須分別根據使用者的輸入或系統的動作 (如載入資源圖形) 執行。 規劃遊戲時，先繪製出類似我們使用的圖表，確定您已解決使用者或系統在高階可能採取的所有動作。 遊戲可以很複雜，而狀態電腦是一個很強大的工具，可以視覺化這些複雜的程序，讓它變得更容易管理。

當然，就像您之前看到的，狀態電腦內還有狀態電腦。 有一個是用於控制器，可以處理玩家產生的所有可接受輸入。 在圖表中，按下的動作就是某種形式的使用者輸入。 這個狀態電腦不在乎輸入是什麼，因為它在更高階工作；它假設控制器的狀態電腦會處理影響移動及射擊行為的任何轉換，以及相關的轉譯更新。 我們會在[新增控制項](tutorial--adding-controls.md)中討論輸入狀態的管理。

## <a name="updating-the-user-interface"></a>更新使用者介面


我們需要持續通知玩家系統的狀態，允許使用者根據遊戲規則來變更高階狀態。 對於這個遊戲範例包含的大部分遊戲而言，會使用包含代表遊戲狀態和其他遊戲特定資訊 (如分數、子彈或還剩幾次機會) 的平視顯示器進行通知。 我們將這個稱為重疊，因為與主要圖形管線分開轉譯，而且放置在 3D 投影上層。 在範例遊戲中，我們使用 Direct2D API 建立這個重疊。 我們也可以使用 XAML 來建立這個重疊，如[延伸遊戲範例](tutorial-resources.md)中的討論。

使用者介面有兩個元件：

-   包含遊戲目前狀態之分數和資訊的平視顯示器。
-   暫停點陣圖，是遊戲暫停狀態期間上面重疊文字的黑色矩形。 這就是遊戲重疊。 我們會在[新增使用者介面](tutorial--adding-a-user-interface.md)中進一步討論。

不難想像，重疊也有狀態電腦。 重疊可以顯示關卡開始或遊戲結束的訊息。 基本上，它就是遊戲暫停或中斷時，用來顯示我們對使用者顯示之任何遊戲狀態資訊的畫布。

以下是遊戲範例如何建立重疊狀態電腦的結構。

```cpp
void App::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {

    case GameInfoOverlayState::Loading:
        m_renderer->InfoOverlay()->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        // ...
        break;

    case GameInfoOverlayState::LevelStart:
        // ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        // ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        // ...
        break;

    case GameInfoOverlayState::Pause:
        // ...
        break;
    }
}
```

根據遊戲本身的狀態，重疊可以顯示 6 種狀態畫面：遊戲開始時的資源載入畫面、遊戲畫面、關卡開始訊息畫面、在時間之內完成所有關卡的遊戲結束畫面、時間用完的遊戲結束畫面以及暫停功能表畫面。

將您的使用者介面與遊戲圖形管線分開，可讓您不用依賴遊戲圖形轉譯引擎獨立使用使用者介面，並大幅降低遊戲程式碼的複雜程度。

## <a name="next-steps"></a>後續步驟


這已經涵蓋遊戲範例的基本結構，並示範以 DirectX 開發 UWP 遊戲應用程式的最佳模型。 當然，還有更多內容。 我們僅逐步解說了遊戲的基本架構而已。 現在，我們要深入了解遊戲及其機制，以及如何實作這些機制以做為核心遊戲物件。 我們會在[定義主要遊戲物件](tutorial--defining-the-main-game-loop.md)中討論這個部分。

同時也該更仔細的探討範例遊戲的圖形引擎。 這個部分涵蓋在[組合轉譯管線](tutorial--assembling-the-rendering-pipeline.md)的內容中。

## <a name="complete-sample-code-for-this-section"></a>這個章節的完整範例程式碼


App.h

```cpp
///// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "Simple3DGame.h"

enum class UpdateEngineState
{
    WaitingForResources,
    ResourcesLoaded,
    WaitingForPress,
    Dynamics,
    Snapped,
    Suspended,
    Deactivated,
};

enum class PressResultState
{
    LoadGame,
    PlayLevel,
    ContinueLevel,
};

enum class GameInfoOverlayState
{
    Loading,
    GameStats,
    GameOverExpired,
    GameOverCompleted,
    LevelStart,
    Pause,
};

ref class App : public Windows::ApplicationModel::Core::IFrameworkView
{
internal:
    App();

public:
    // IFrameworkView Methods
    virtual void Initialize(_In_ Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
    virtual void SetWindow(_In_ Windows::UI::Core::CoreWindow^ window);
    virtual void Load(_In_ Platform::String^ entryPoint);
    virtual void Run();
    virtual void Uninitialize();

private:
    void InitializeGameState();

    // Event Handlers
    void OnSuspending(
        _In_ Platform::Object^ sender,
        _In_ Windows::ApplicationModel::SuspendingEventArgs^ args
        );

    void OnResuming(
        _In_ Platform::Object^ sender,
        _In_ Platform::Object^ args
        );

    void UpdateViewState();

    void OnWindowActivationChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
        );

    void OnWindowSizeChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::WindowSizeChangedEventArgs^ args
        );

    void OnWindowClosed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::CoreWindowEventArgs^ args
        );

    void OnLogicalDpiChanged(
        _In_ Platform::Object^ sender
        );

    void OnActivated(
        _In_ Windows::ApplicationModel::Core::CoreApplicationView^ applicationView,
        _In_ Windows::ApplicationModel::Activation::IActivatedEventArgs^ args
        );

    void OnVisibilityChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::VisibilityChangedEventArgs^ args
        );

    void Update();
    void SetGameInfoOverlay(GameInfoOverlayState state);
    void SetAction (GameInfoOverlayCommand command);
    void ShowGameInfoOverlay();
    void HideGameInfoOverlay();
    void SetSnapped();
    void HideSnapped();

    bool                                                m_windowClosed;
    bool                                                m_renderNeeded;
    bool                                                m_haveFocus;
    bool                                                m_visible;

    MoveLookController^                                 m_controller;
    GameRenderer^                                       m_renderer;
    Simple3DGame^                                       m_game;

    UpdateEngineState                                   m_updateState;
    UpdateEngineState                                   m_updateStateNext;
    PressResultState                                    m_pressResult;
    GameInfoOverlayState                                m_gameInfoOverlayState;
    GameInfoOverlayCommand                              m_gameInfoOverlayCommand;
    uint32                                              m_loadingCount;
};

ref class Direct3DApplicationSource : Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView();
};
```

App.cpp

```cpp
//--------------------------------------------------------------------------------------
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "App.h"

using namespace concurrency;
using namespace DirectX;
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::Foundation;
using namespace Windows::Graphics::Display;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::ViewManagement;


App::App() :
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
}

//--------------------------------------------------------------------------------------

void App::Initialize(
    _In_ CoreApplicationView^ applicationView
    )
{
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();
}

//--------------------------------------------------------------------------------------

void App::SetWindow(
    _In_ CoreWindow^ window
    )
{
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    PointerVisualizationSettings^ visualizationSettings = PointerVisualizationSettings::GetForCurrentView();
    visualizationSettings->IsContactFeedbackEnabled = false;
    visualizationSettings->IsBarrelButtonFeedbackEnabled = false;

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::Load(
    _In_ Platform::String^ /* entryPoint */
    )
{
    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and loads all the necessary resources in parallel on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work, so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can ONLY be accessed on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}

//--------------------------------------------------------------------------------------

void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::Snapped:
                if (!m_renderNeeded)
                {
                    // The App is not currently the active window, so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // Otherwise, fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close.  Make sure to save state.
}

//--------------------------------------------------------------------------------------

void App::Uninitialize()
{
}

//--------------------------------------------------------------------------------------

void App::OnWindowSizeChanged(
    _In_ CoreWindow^ window,
    _In_ WindowSizeChangedEventArgs^ /* args */
    )
{
    UpdateViewState();
    m_renderer->UpdateForWindowSizeChange();

    // The location of the GameInfoOverlay may have changed with the size change, so update the controller.
    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    if (m_updateState == UpdateEngineState::WaitingForPress)
    {
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
    }
}

//--------------------------------------------------------------------------------------

void App::OnWindowClosed(
    _In_ CoreWindow^ /* sender */,
    _In_ CoreWindowEventArgs^ /* args */
    )
{
    m_windowClosed = true;
}

//--------------------------------------------------------------------------------------

void App::OnLogicalDpiChanged(
    _In_ Platform::Object^ /* sender */
    )
{
    m_renderer->SetDpi(DisplayProperties::LogicalDpi);

    // The GameInfoOverlay may have been recreated as a result of DPI changes, so
    // regenerate the data.
    SetGameInfoOverlay(m_gameInfoOverlayState);
    SetAction(m_gameInfoOverlayCommand);
}

//--------------------------------------------------------------------------------------

void App::OnActivated(
    _In_ CoreApplicationView^ /* applicationView */,
    _In_ IActivatedEventArgs^ /* args */
    )
{
    CoreWindow::GetForCurrentThread()->Activated +=
        ref new TypedEventHandler<CoreWindow^, WindowActivatedEventArgs^>(this, &App::OnWindowActivationChanged);
    CoreWindow::GetForCurrentThread()->Activate();
}

//--------------------------------------------------------------------------------------

void App::OnVisibilityChanged(
    _In_ CoreWindow^ /* sender */,
    _In_ VisibilityChangedEventArgs^ args
    )
{
    m_visible = args->Visible;
}

//--------------------------------------------------------------------------------------

void App::InitializeGameState()
{
    // Set up the initial state machine for handling the Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::ContinueLevel;
        SetGameInfoOverlay(GameInfoOverlayState::Pause);
        SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        m_updateState = UpdateEngineState::WaitingForPress;
        m_pressResult = PressResultState::LoadGame;
        SetGameInfoOverlay(GameInfoOverlayState::GameStats);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        SetAction(GameInfoOverlayCommand::TapToContinue);
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
        SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::Update()
{
    static uint32 loadCount = 0;

    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        // Waiting for the initial load.  Display an update once per 60 updates.
        loadCount++;
        if ((loadCount % 60) == 0)
        {
            m_loadingCount++;
            SetGameInfoOverlay(m_gameInfoOverlayState);
        }
        break;

    case UpdateEngineState::ResourcesLoaded:
        switch (m_pressResult)
        {
        case PressResultState::LoadGame:
            SetGameInfoOverlay(GameInfoOverlayState::GameStats);
            break;

        case PressResultState::PlayLevel:
            SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
            break;

        case PressResultState::ContinueLevel:
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            break;
        }
        m_updateState = UpdateEngineState::WaitingForPress;
        SetAction(GameInfoOverlayCommand::TapToContinue);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        ShowGameInfoOverlay();
        m_renderNeeded = true;
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;
                m_controller->Active(false);
                m_game->LoadGame();
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();

                m_game->LoadLevelAsync().then([this]()
                {
                    m_game->FinalizeLoadLevel();
                    m_updateState = UpdateEngineState::ResourcesLoaded;

                }, task_continuation_context::use_current());
                break;

            case PressResultState::PlayLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->StartLevel();
                break;

            case PressResultState::ContinueLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->ContinueGame();
                break;
            }
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            m_game->PauseGame();
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_updateState = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            ShowGameInfoOverlay();
        }
        else
        {
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverExpired);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;

            case GameState::LevelComplete:
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;

                m_game->LoadLevelAsync().then([this]()
                {
                    m_game->FinalizeLoadLevel();
                    m_updateState = UpdateEngineState::ResourcesLoaded;

                }, task_continuation_context::use_current());
                break;

            case GameState::GameComplete:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverCompleted);
                ShowGameInfoOverlay();
                m_updateState  = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}

//--------------------------------------------------------------------------------------

void App::OnWindowActivationChanged(
    _In_ Windows::UI::Core::CoreWindow^ /* sender */,
    _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
    )
{
    if (args->WindowActivationState == CoreWindowActivationState::Deactivated)
    {
        m_haveFocus = false;

        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of Deactivated rather than going directly back into game play
            // go to the paused state waiting for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            ShowGameInfoOverlay();
            m_game->PauseGame();
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            m_renderNeeded = true;
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            ShowGameInfoOverlay();
            m_renderNeeded = true;
            break;
        }
        // Don't have focus, so shutdown input processing.
        m_controller->Active(false);
    }
    else if (args->WindowActivationState == CoreWindowActivationState::CodeActivated
        || args->WindowActivationState == CoreWindowActivationState::PointerActivated)
    {
        m_haveFocus = true;

        if (m_updateState == UpdateEngineState::Deactivated)
        {
            m_updateState = m_updateStateNext;

            if (m_updateState == UpdateEngineState::WaitingForPress)
            {
                SetAction(GameInfoOverlayCommand::TapToContinue);
                m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
            }
            else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
            {
                SetAction(GameInfoOverlayCommand::PleaseWait);
            }
        }
    }
}

//--------------------------------------------------------------------------------------

void App::OnSuspending(
    _In_ Platform::Object^ /* sender */,
    _In_ SuspendingEventArgs^ args
    )
{
    // Save application state.
    // If your application needs time to complete a lengthy operation, it can request a deferral.
    // The SuspendingOperation has a deadline time. Make sure all your operations are complete by that time!
    // If the app doesn't return from this handler within five seconds, it will be terminated.
    SuspendingOperation^ op = args->SuspendingOperation;
    SuspendingDeferral^ deferral = op->GetDeferral();

    create_task([=]()
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // Game is in the active game play state, Stop Game Timer and Pause play and save the state.
            SetAction(GameInfoOverlayCommand::None);
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            m_game->PauseGame();
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            m_updateStateNext = m_updateState;
            break;

        default:
            // If it is any other state, don't save as the next state as they are transient states and have already set m_updateStateNext
            break;
        }
        m_updateState = UpdateEngineState::Suspended;

        m_controller->Active(false);
        m_game->OnSuspending();

        deferral->Complete();
    });
}

//--------------------------------------------------------------------------------------

void App::OnResuming(
    _In_ Platform::Object^ /* sender */,
    _In_ Platform::Object^ /* args */
    )
{
    if (m_haveFocus)
    {
        m_updateState = m_updateStateNext;
    }
    else
    {
        m_updateState = UpdateEngineState::Deactivated;
    }

    if (m_updateState == UpdateEngineState::WaitingForPress)
    {
        SetAction(GameInfoOverlayCommand::TapToContinue);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
    }
    m_game->OnResuming();
    ShowGameInfoOverlay();
    m_renderNeeded = true;
}

//--------------------------------------------------------------------------------------

void App::UpdateViewState()
{
    m_renderNeeded = true;

    if (ApplicationView::Value == ApplicationViewState::Snapped)
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of SNAPPED layout rather than going directly back into game play,
            // go to the paused state and wait for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_game->PauseGame();
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            // Avoid corrupting the m_updateStateNext on a transition from Snapped -> Snapped.
            // Otherwise, just cache the current state and return to it when leaving SNAPPED layout.

            m_updateStateNext = m_updateState;
            break;

        default:
            break;
        }

        m_updateState = UpdateEngineState::Snapped;
        m_controller->Active(false);
        HideGameInfoOverlay();
        SetSnapped();
    }
    else if (ApplicationView::Value == ApplicationViewState::Filled ||
        ApplicationView::Value == ApplicationViewState::FullScreenLandscape ||
        ApplicationView::Value == ApplicationViewState::FullScreenPortrait)
    {
        if (m_updateState == UpdateEngineState::Snapped)
        {

            HideSnapped();
            ShowGameInfoOverlay();
            m_renderNeeded = true;

            if (m_haveFocus)
            {
                if (m_updateStateNext == UpdateEngineState::WaitingForPress)
                {
                    SetAction(GameInfoOverlayCommand::TapToContinue);
                    m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
                }
                else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    SetAction(GameInfoOverlayCommand::PleaseWait);
                }

                m_updateState = m_updateStateNext;
            }
            else
            {
                m_updateState = UpdateEngineState::Deactivated;
                SetAction(GameInfoOverlayCommand::None);
            }
        }
    }
}

//--------------------------------------------------------------------------------------

void App::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_renderer->InfoOverlay()->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        m_renderer->InfoOverlay()->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_renderer->InfoOverlay()->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_renderer->InfoOverlay()->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_renderer->InfoOverlay()->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_renderer->InfoOverlay()->SetPause();
        break;
    }
}

//--------------------------------------------------------------------------------------

void App::SetAction (GameInfoOverlayCommand command)
{
    m_gameInfoOverlayCommand = command;
    m_renderer->InfoOverlay()->SetAction(command);
}

//--------------------------------------------------------------------------------------

void App::ShowGameInfoOverlay()
{
    m_renderer->InfoOverlay()->ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::HideGameInfoOverlay()
{
    m_renderer->InfoOverlay()->HideGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::SetSnapped()
{
    m_renderer->InfoOverlay()->SetPause();
    m_renderer->InfoOverlay()->ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::HideSnapped()
{
    m_renderer->InfoOverlay()->HideGameInfoOverlay();
    SetGameInfoOverlay(m_gameInfoOverlayState);
}

//--------------------------------------------------------------------------------------

IFrameworkView^ DirectXAppSource::CreateView()
{
    return ref new App();
}

//--------------------------------------------------------------------------------------

[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}

//--------------------------------------------------------------------------------------
```

 

 





