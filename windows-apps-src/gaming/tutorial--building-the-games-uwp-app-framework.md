---
title: 定義遊戲的 UWP 應用程式架構
description: 撰寫通用 Windows 平台 (UWP) DirectX 遊戲程式碼的第一部分，是建立讓遊戲物件與 Windows 互動的架構。
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, games, directx, 遊戲
ms.localizationpriority: medium
ms.openlocfilehash: 175009773f7969adbaf36a036e733443f593467f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620553"
---
#  <a name="define-the-uwp-app-framework"></a>定義 UWP app 架構

建置架構讓您的遊戲物件與 Windows 互動，包括 Windows 執行階段屬性式的暫停-恢復事件處理、變更視窗對焦和貼齊等。

若要設定此架構，首先取得檢視提供者，以便 App 單例 (定義執行中 App 之執行個體的 Windows 執行階段物件) 可以存取所需的圖形資源。 透過 Windows 執行階段，您的遊戲也可以直接與圖形介面連接，讓您可指定所需的資源及處理方式。

檢視提供者物件會實作 __IFrameworkView__ 介面，其中包含一系列需要設定以建立此遊戲範例的方法。

您將需要實作這五個 App 單例所呼叫的方法：
* [__初始化__](#initialize-the-view-provider)
* [__SetWindow__](#configure-the-window-and-display-behaviors)
* [__負載__](#load-method-of-the-view-provider)
* [__執行__](#run-method-of-the-view-provider)
* [__解除初始化__](#uninitialize-method-of-the-view-provider)

__Initialize__ 方法是在應用程式啟動時呼叫。 __SetWindow__ 方法是在 __Initialize__ 之後呼叫。 然後呼叫 __Load__ 方法。 __Run__ 方法是遊戲執行中。 遊戲結束時，呼叫 __Uninitialize__ 方法。 如需詳細資訊，請參閱 [__IFrameworkView__ API 參照](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview)。 

>[!Note]
>如果您尚未下載此範例的最新遊戲程式碼，請移至 [Direct3D 遊戲範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此為 UWP 功能範例的大集合的一部分。 如需下載範例方法的指示，請參閱[從 GitHub 取得 UWP 範例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目標

設定 通用 Windows 平台 (UWP) DirectX 遊戲的架構，以及實作定義整體遊戲流程的狀態電腦。

## <a name="define-the-view-provider-factory-and-view-provider-object"></a>定義檢視提供者 Factory 和檢視提供者物件

讓我們來看看 __App.cpp__ 中的 __main__ loop。 

在此步驟中，我們建立檢視的 Factory (實作 __IFrameworkViewSource__)，其轉為建立定義檢視的檢視提供者物件的執行個體 (實作 __IFrameworkView__)。

### <a name="main-method"></a>主要方法

如果您載入了 GitHub 範例程式碼，請建立新的 __DirectXApplicationSource__。 (如果您使用原始的 DirectX 範本，請使用 __Direct3DApplicationSource__) 這是實作 __IFrameworkViewSource__ 的檢視提供者 Factory。 檢視提供者 Factory 的 __IFrameworkViewSource__ 介面有已定義的單一方法 __CreateView__。

在 __CoreApplication::Run__ 中，傳遞 __Direct3DApplicationSource__ 或 __DirectXApplicationSource__ 時，__CreateView__ 方法是由 App 單例呼叫。

__CreateView__ 會傳回您的 App 物件新執行個體的參照，該物件實作其為此範例中 __App__ 類別物件的 __IFrameworkView__。 __App__ 類別物件是檢視提供者物件。

```cpp
// The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto directXApplicationSource = ref new DirectXApplicationSource();
    CoreApplication::Run(directXApplicationSource);
    return 0;
}

//--------------------------------------------------------------------------------------

IFrameworkView^ DirectXApplicationSource::CreateView()
{
    return ref new App();
}
```

## <a name="initialize-the-view-provider"></a>初始化檢視提供者

建立檢視提供者物件後，App 單例會在應用程式啟動時呼叫 [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 方法。 因此，這個方法必須處理大部分的 UWP 遊戲基礎行為， 例如處理主視窗的啟用，以及確定遊戲可以處理突然暫停 (可能在稍後繼續) 的事件。

此時，遊戲 App 可以處理暫停 (或繼續) 的訊息。 但是仍沒有可以使用的視窗，而且遊戲還未初始化。 我們還需要完成幾個工作！

### <a name="appinitialize-method"></a>App::Initialize 方法

在此方法中，建立各種事件處理常式，以供啟動，暫停、繼續遊戲。

取得裝置資源的存取權。 __make_shared__ 功能是用來在第一次建立記憶體資源時建立 __shared_ptr__。 使用 __make_shared__ 的好處是異常安全。 它也會使用相同的呼叫來為配置記憶體控制項封鎖和資源，因此降低建構負擔。

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    // Register event handlers for app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="configure-the-window-and-display-behaviors"></a>設定視窗，並顯示行為

現在，讓我們看看 [__SetWindow__](https://msdn.microsoft.com/library/windows/apps/hh700509) 的實作。 在 __SetWindow__ 方法中，您設定視窗並顯示行為。

### <a name="appsetwindow-method"></a>App::SetWindow 方法

App 單例提供一個 [__CoreWindow__](https://msdn.microsoft.com/library/windows/apps/br208225) 物件來代表遊戲的主視窗，並將其中的資源和事件提供給遊戲使用。 現在，有視窗可供使用，遊戲可以開始加入基本 UI 元件和事件。

然後，使用 __CoreCursor__ 方法建立指標，可由兩個滑鼠和觸控式控制項使用。

最後，建立調整視窗大小、關閉和變更 DPI (如果顯示裝置變更) 的基本事件。 如需有關事件的詳細資訊，請移至[事件處理](tutorial-game-flow-management.md#events-handling)。

```cpp
void App::SetWindow(
    CoreWindow^ window
    )
{
    // Codes added to modify the original DirectX template project
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    PointerVisualizationSettings^ visualizationSettings = PointerVisualizationSettings::GetForCurrentView();
    visualizationSettings->IsContactFeedbackEnabled = false;
    visualizationSettings->IsBarrelButtonFeedbackEnabled = false;
    // --end of codes added
    
    m_deviceResources->SetWindow(window);

    window->Activated +=
        ref new TypedEventHandler<CoreWindow^, WindowActivatedEventArgs^>(this, &App::OnWindowActivationChanged);

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);
        
    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    DisplayInformation^ currentDisplayInformation = DisplayInformation::GetForCurrentView();

    currentDisplayInformation->DpiChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDpiChanged);

    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);
    
    // Codes added to modify the original DirectX template project
    currentDisplayInformation->StereoEnabledChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnStereoEnabledChanged);
    // --end of codes added
    
    DisplayInformation::DisplayContentsInvalidated +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDisplayContentsInvalidated);
}
```

## <a name="load-method-of-the-view-provider"></a>檢視提供者的 Load 方法

設定好主視窗後，應用程式單例會呼叫 [__Load__](https://msdn.microsoft.com/library/windows/apps/hh700501)。 這個方法使用一組非同步工作來建立遊戲物件、載入圖形資源以及初始化遊戲的狀態電腦。 如果您想要預先擷取遊戲資料或資產，這個方法相較於 **SetWindow** 或 **Initialize** 更為適合。 

因為 Windows 強制限制遊戲必須開始處理輸入之前可以使用的時間，藉由使用非同步工作模式，您需要設計  __Load__ 方法以快速完成，如此才能開始處理輸入。 如果載入需要一些時間，或者如果有許多資源，那麼請為您的使用者提供定期更新的進度列。 此方法也用來在遊戲開始前進行任何必要的準備，像是設定任何開始狀態或全域值。

如果您是非同步程式設計和工作平行概念的新手，請移至 [C++ 中的非同步程式設計](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。

### <a name="appload-method"></a>App::Load 方法

建立包含載入工作 __GameMain__ 類別。

```cpp
void App::Load(
    Platform::String^ entryPoint
    )
{
        if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
````

### <a name="gamemain-constructor"></a>GameMain 建構函式

* 建立和初始化遊戲轉譯器 如需詳細資訊，請參閱[轉譯架構 i:轉譯為您簡介](tutorial--assembling-the-rendering-pipeline.md)。
* 建立和初始化 Simple3Dgame 物件。 如需詳細資訊，請參閱[定義主要遊戲物件](tutorial--defining-the-main-game-loop.md)。    
* 建立遊戲 UI 控制項物件，並顯示遊戲資訊重疊，以在資源檔案載入時顯示進度列。 如需詳細資訊，請參閱[新增使用者介面](tutorial--adding-a-user-interface.md)。
* 建立控制器，使其可以從控制器 (觸控、滑鼠或 XBox 無線控制器) 讀取輸入。 如需詳細資訊，請參閱[新增控制項](tutorial--adding-controls.md)。
* 初始化控制器之後，我們在畫面的左下角和右下角分別為移動和相機觸控控制項定義兩個矩形區域。 玩家使用左下角矩形 (呼叫 **SetMoveRect** 來定義) 當作前後左右移轉相機的虛擬控制鍵。 右下角矩形 (由 **SetFireRect** 方法定義) 當作發射子彈的虛擬按鈕。
* 使用 __create_task__ 和 __create_task::then__ 中斷資源載入為兩個不同的階段。 因為 Direct3D 11 裝置內容的存取被限於裝置內容建立的執行緒，而存取用於建立物件的 Direct3D 11 裝置則是無限制執行緒，這表示 **CreateGameDeviceResourcesAsync** 工作可以在從完成工作 (*FinalizeCreateGameDeviceResources*，其在原始執行緒上執行) 的獨立執行緒上執行。 我們使用 **LoadLevelAsync** 和 **FinalizeLoadLevel** 時會以類似的模式來載入關卡資源。

```cpp
GameMain::GameMain(const std::shared_ptr<DX::DeviceResources>& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = ref new GameRenderer(m_deviceResources);
    m_game = ref new Simple3DGame();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = ref new MoveLookController(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameConstants::TouchRectangleSize, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and in parallel all the necessary resources are loaded on other threads.
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
            // In the middle of a game so spin up the async task to load the level.
            return m_game->LoadLevelAsync().then([this]()
            {
                // The m_game object may need to deal with D3D device context work so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D
                // device context can ONLY be accessed on a single thread.
                m_game->FinalizeLoadLevel();
                m_game->SetCurrentLevelToSavedState();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
        else
        {
            // The game is not in the middle of a level so there aren't any level
            // resources to load.  Creating a no-op task so that in both cases
            // the same continuation logic is used.
            return create_task([]()
            {
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        // Since Game loading is an async task, the app visual state
        // may be too small or not have focus.  Put the state machine
        // into the correct state to reflect these cases.

        if (m_deviceResources->GetLogicalSize().Width < GameConstants::MinPlayableWidth)
        {
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::TooSmall;
            m_controller->Active(false);
            m_uiControl->HideGameInfoOverlay();
            m_uiControl->ShowTooSmall();
            m_renderNeeded = true;
        }
        else if (!m_haveFocus)
        {
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::Deactivated;
            m_controller->Active(false);
            m_uiControl->SetAction(GameInfoOverlayCommand::None);
            m_renderNeeded = true;
        }
    }, task_continuation_context::use_current());
}
```

## <a name="run-method-of-the-view-provider"></a>檢視提供者的 Run 方法

先前的三種方法：__初始化__， __SetWindow__，以及__負載__設定階段。 現在，遊戲可以開始進行 **Run** 方法，開始享受其樂趣吧！ 用來在遊戲狀態間轉換的事件已發送及處理。 圖形在遊戲迴圈循環期間進行更新。

### <a name="apprun"></a>App::Run

啟動一個決定玩家何時關閉遊戲視窗的 __while__ 迴圈。

範例程式碼會在遊戲引擎狀態電腦中轉換成兩種狀態的其中一種：
    * __停用__:遊戲視窗會停用 (失去焦點) 或定格。 發生這種情況時，遊戲會暫停事件處理，並且等候視窗取得焦點或取消定格。
    * __TooSmall__:在遊戲更新自己的狀態，並轉譯顯示圖形。

當使用者與您的遊戲互動時，您必須處理位於訊息佇列的每個事件，因此必須使用 **ProcessAllIfPresent** 選項呼叫 [**CoreWindowDispatch.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)。 其他選項會在處理訊息事件時造成延遲，這會讓您的遊戲顯得沒有回應，或是讓觸控行為變得遲緩而不敏銳。

當遊戲處於不可見、暫停或定格的狀態時，您不希望它佔用任何資源循環來發送永遠不會傳入的訊息。 在此狀況中，您的遊戲必須使用 **ProcessOneAndAllPending**，它會持續封鎖直到取得事件，然後處理該事件，並處理在處理第一個事件期間抵達處理佇列中的任何其他事件。 [**ProcessEvents** ](https://msdn.microsoft.com/library/windows/apps/br208215)立即傳回處理完佇列之後。

```cpp
void App::Run()
{
    m_main->Run();
}

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
                if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    WaitingForResourceLoading();
                    m_renderNeeded = true;
                }
                else if (m_updateStateNext == UpdateEngineState::ResourcesLoaded)
                {
                    // In the device lost case, we transition to the final waiting state
                    // and make sure the display is updated.
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
                    m_updateStateNext = UpdateEngineState::WaitingForPress;
                    m_uiControl->ShowGameInfoOverlay();
                    m_renderNeeded = true;
                }

                if (!m_renderNeeded)
                {
                    // The App is not currently the active window and not in a transient state so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
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

## <a name="uninitialize-method-of-the-view-provider"></a>檢視提供者的 Uninitialize 方法

當使用者最後結束遊戲工作階段時，我們需要進行清除。 這時就需要使用 **Uninitialize**。

在 Windows 10 中，關閉應用程式視窗不終止應用程式的程序，但改為它的應用程式的單一狀態將寫入記憶體。 如需在系統必須回收此記憶體時執行任何特殊的動作，包括任何特殊的資源清理，請在這個方法中放入該清理動作的程式碼。

### <a name="app-uninitialize"></a>應用程式：Uninitialize

```cpp
void App::Uninitialize()
{
}
```

## <a name="tips"></a>提示

開發您自己的遊戲時，請圍繞這些方法設計啟動程式碼。 這裡是每個方法基本建議的簡單清單：

-   使用 **Initialize** 配置主要類別並連接基本事件處理常式。
-   使用 **SetWindow** 建立主視窗並連接任何視窗特定事件。
-   使用 **Load** 處理任何其餘的設定，並初始化物件和載入資源的非同步建立。 如果需要建立任何暫存的檔案或資料 (如由程序產生的資產)，也請在這裡設定。


## <a name="next-steps"></a>後續步驟

這包含 UWP 遊戲和 DirectX 的基本結構。 請記住這五個方法，因為我們會在此逐步解說的其他部分中參照它們。 接下來，讓我們深入了解如何管理遊戲狀態和事件處理，保持遊戲在[遊戲流程管理](tutorial-game-flow-management.md)下執行。



