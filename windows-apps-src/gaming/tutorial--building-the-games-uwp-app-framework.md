---
title: 定義遊戲的 UWP app 架構
description: 撰寫通用 Windows 平臺（UWP）遊戲程式碼的第一個步驟，就是建立可讓應用程式物件與 Windows 互動的架構。
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, games, directx
ms.localizationpriority: medium
ms.openlocfilehash: 5052b4e71196950b6a8a91aaa271b5550fb448b5
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409617"
---
#  <a name="define-the-games-uwp-app-framework"></a>定義遊戲的 UWP app 架構

> [!NOTE]
> 本主題是使用 DirectX 教學課程系列[建立簡單的通用 Windows 平臺（UWP）遊戲](tutorial--create-your-first-uwp-directx-game.md)的一部分。 該連結的主題會設定數列的內容。

編碼通用 Windows 平臺（UWP）遊戲的第一個步驟，是建立可讓應用程式物件與 Windows 互動的架構，包括 Windows 執行階段的功能，例如暫停-繼續事件處理、視窗焦點的變更，以及貼齊。

## <a name="objectives"></a>目標

* 設定通用 Windows 平臺（UWP） DirectX 遊戲的架構，並執行定義整體遊戲流程的狀態機器。

> [!NOTE]
> 若要遵循本主題的指示，請查看您所下載[Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/)範例遊戲的原始程式碼。

## <a name="introduction"></a>簡介

在[設定遊戲專案](tutorial--setting-up-the-games-infrastructure.md)主題中，我們引進了**WWinMain**函數以及[**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)和[**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)介面。 我們已瞭解**應用程式**類別（您可以在 Simple3DGameDX 專案中的 `App.cpp` 原始程式碼檔案中看到） **Simple3DGameDX**可同時做為*視圖提供者 factory*和*視圖提供者*。

本主題將從該處挑選，並深入瞭解遊戲中的**應用程式**類別如何執行**IFrameworkView**的方法。

## <a name="the-appinitialize-method"></a>App：： Initialize 方法

在應用程式啟動時，Windows 呼叫的第一個方法是我們的[**IFrameworkView：： Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)的執行。

您的執行應該會處理 UWP 遊戲最基本的行為，例如確定遊戲可以藉由訂閱這些事件來處理暫停（以及可能的後續恢復）事件。 我們也可以存取此處的顯示介面卡裝置，讓我們能夠建立相依于裝置的圖形資源。

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    applicationView.Activated({ this, &App::OnActivated });

    CoreApplication::Suspending({ this, &App::OnSuspending });

    CoreApplication::Resuming({ this, &App::OnResuming });

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

盡可能避免原始指標（而且幾乎一律會這麼做）。

- 對於 Windows 執行階段類型，您可以非常避免指標，而只是在堆疊上建立值。 如果您需要指標，請使用[**winrt：： com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) （我們很快就會看到此範例）。
- 若為唯一指標，請使用[**std：： unique_ptr**](/cpp/cpp/how-to-create-and-use-unique-ptr-instances)和**std：： make_unique**。
- 若為共用指標，請使用[**std：： shared_ptr**](/cpp/cpp/how-to-create-and-use-shared-ptr-instances)和**std：： make_shared**。

## <a name="the-appsetwindow-method"></a>App：： SetWindow 方法

**初始化**之後，Windows 會呼叫[**IFrameworkView：： SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)的實作為，傳遞代表遊戲主要視窗的[**CoreWindow**](/uwp/api/windows.ui.core.corewindow)物件。

在**App：： SetWindow**中，我們會訂閱與視窗相關的事件，並設定一些視窗和顯示行為。 例如，我們會建立一個滑鼠指標（透過[**CoreCursor**](/uwp/api/windows.ui.core.corecursor)類別），這可供滑鼠和觸控控制項使用。 我們也會將視窗物件傳遞至裝置相依的資源物件。

我們將在[遊戲流程管理](tutorial-game-flow-management.md#event-handling)主題中詳細討論處理事件。

```cppwinrt
void SetWindow(CoreWindow const& window)
{
    //CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();

    window.PointerCursor(CoreCursor(CoreCursorType::Arrow, 0));

    PointerVisualizationSettings visualizationSettings{ PointerVisualizationSettings::GetForCurrentView() };
    visualizationSettings.IsContactFeedbackEnabled(false);
    visualizationSettings.IsBarrelButtonFeedbackEnabled(false);

    m_deviceResources->SetWindow(window);

    window.Activated({ this, &App::OnWindowActivationChanged });

    window.SizeChanged({ this, &App::OnWindowSizeChanged });

    window.Closed({ this, &App::OnWindowClosed });

    window.VisibilityChanged({ this, &App::OnVisibilityChanged });

    DisplayInformation currentDisplayInformation{ DisplayInformation::GetForCurrentView() };

    currentDisplayInformation.DpiChanged({ this, &App::OnDpiChanged });

    currentDisplayInformation.OrientationChanged({ this, &App::OnOrientationChanged });

    currentDisplayInformation.StereoEnabledChanged({ this, &App::OnStereoEnabledChanged });

    DisplayInformation::DisplayContentsInvalidated({ this, &App::OnDisplayContentsInvalidated });
}
```

## <a name="the-appload-method"></a>應用程式：： Load 方法

現在已設定主視窗，我們會呼叫[**IFrameworkView：： Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)的執行。 **載入**是預先提取遊戲資料或資產的最佳位置，而不是**初始化**和**SetWindow**。

```cppwinrt
void Load(winrt::hstring const& /* entryPoint */)
{
    if (!m_main)
    {
        m_main = winrt::make_self<GameMain>(m_deviceResources);
    }
}
```

如您所見，實際的工作會委派給我們在這裡所做的**GameMain**物件的函式。 **GameMain**類別定義于和中 `GameMain.h` `GameMain.cpp` 。

### <a name="the-gamemaingamemain-constructor"></a>GameMain：： GameMain 函數

**GameMain**的函式（以及它所呼叫的其他成員函式）會開始一組非同步載入作業來建立遊戲物件、載入圖形資源，以及初始化遊戲的狀態機器。 在遊戲開始之前，我們也會執行任何必要的準備工作，例如設定任何起始狀態或全域值。

Windows 在開始處理輸入之前，會對您的遊戲所能採取的時間施加限制。 因此使用非同步，就像我們在這裡所做的一樣，這表示**載入**作業會在背景中繼續進行時快速傳回。 如果載入需要很長的時間，或是有許多資源，則為您的使用者提供經常更新的進度列，是個不錯的主意。 

如果您不熟悉非同步程式設計，請參閱[使用 c + +/WinRT 的並行和非同步作業](/windows/uwp/cpp-and-winrt-apis/concurrency)。

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);
    m_game = std::make_shared<Simple3DGame>();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = std::make_shared<MoveLookController>(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(GameUIConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameUIConstants::TouchRectangleSize, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    auto lifetime = get_strong();

    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        // In the middle of a game so spin up the async task to load the level.
        co_await m_game->LoadLevelAsync();

        // The m_game object may need to deal with D3D device context work so
        // again the finalize code needs to run in the same thread
        // context as the m_renderer object was created because the D3D
        // device context can ONLY be accessed on a single thread.
        m_game->FinalizeLoadLevel();
        m_game->SetCurrentLevelToSavedState();
        m_updateState = UpdateEngineState::ResourcesLoaded;
    }
    else
    {
        // The game is not in the middle of a level so there aren't any level
        // resources to load.
    }

    // Since Game loading is an async task, the app visual state
    // may be too small or not have focus. Put the state machine
    // into the correct state to reflect these cases.

    if (m_deviceResources->GetLogicalSize().Width < GameUIConstants::MinPlayableWidth)
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
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    ...
}
```

以下是由函式所啟動之工作順序的大綱。

- 建立並初始化**GameRenderer**類型的物件。 如需詳細資訊，請參閱[轉譯架構 I：轉譯簡介](tutorial--assembling-the-rendering-pipeline.md)。
- 建立並初始化**Simple3DGame**類型的物件。 如需詳細資訊，請參閱[定義主要遊戲物件](tutorial--defining-the-main-game-loop.md)。
- 建立遊戲 UI 控制項物件，並顯示 [遊戲資訊重迭]，以在載入資源檔時顯示進度列。 如需詳細資訊，請參閱[新增使用者介面](tutorial--adding-a-user-interface.md)。
- 建立控制器物件，以從控制器（觸控、滑鼠或 Xbox 無線控制器）讀取輸入。 如需詳細資訊，請參閱[新增控制項](tutorial--adding-controls.md)。
- 分別在 [移動] 和 [相機觸控] 控制項的螢幕左下方和右下角定義兩個矩形區域。 播放程式會使用左下角矩形（定義于**SetMoveRect**的呼叫中）做為虛擬控制板，以便向前和向後移動相機，以及側邊。 右下角矩形（由**SetFireRect**方法定義）是用來引發 ammo 的虛擬按鈕。
- 使用協同程式將資源載入分成不同的階段。 Direct3D 裝置內容的存取權僅限於建立裝置內容的執行緒;雖然在建立物件時可以自由執行緒存取 Direct3D 裝置。 因此， **GameRenderer：： CreateGameDeviceResourcesAsync**協同程式可以與完成工作（**GameRenderer：： FinalizeCreateGameDeviceResources**）在不同的執行緒上執行，此工作會在原始執行緒上執行。
- 我們使用類似的模式，透過**Simple3DGame：： LoadLevelAsync**和**Simple3DGame：： FinalizeLoadLevel**載入層級資源。

我們會在下一個主題中看到更多**GameMain：： InitializeGameState** （[遊戲流程管理](tutorial-game-flow-management.md)）。

## <a name="the-apponactivated-method"></a>App：： OnActivated 方法

接下來，會引發[**CoreApplicationView：：已啟用**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated)事件。 因此，系統會呼叫您擁有的任何**OnActivated**事件處理常式（例如我們的**App：： OnActivated**方法）。

```cppwinrt
void OnActivated(CoreApplicationView const& /* applicationView */, IActivatedEventArgs const& /* args */)
{
    CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();
}
```

我們在此所做的唯一工作是啟用主要[**CoreWindow**](/uwp/api/windows.ui.core.corewindow)。 或者，您也可以選擇在**App：： SetWindow**中執行此動作。

## <a name="the-apprun-method"></a>App：： Run 方法

**Initialize**、 **SetWindow**和**Load**已設定階段。 現在，遊戲已啟動並執行中，我們會呼叫[**IFrameworkView：： Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)的實作為。

```cppwinrt
void Run()
{
    m_main->Run();
}
```

同樣地，工作會委派給**GameMain**。

### <a name="the-gamemainrun-method"></a>GameMain：： Run 方法

**GameMain：： Run**是遊戲的主要迴圈;您可以在中找到它 `GameMain.cpp` 。 基本的邏輯是，當遊戲的視窗維持開啟狀態時，分派所有事件、更新計時器，然後轉譯和呈現圖形管線的結果。 此外，在此也會分派和處理用來在遊戲狀態之間轉換的事件。

這裡的程式碼也關心「遊戲引擎」狀態機器中的兩個狀態。

- **UpdateEngineState：:D eactivated**。 這表示遊戲視窗已停用（已失去焦點）或已進行快照。 
- **UpdateEngineState：： TooSmall**。 這表示工作區太小，無法在中呈現遊戲。

在上述任一種狀態中，遊戲都會暫停事件處理，並等候視窗聚焦、unsnap 或調整大小。

當使用者與您的遊戲互動時，您必須處理位於訊息佇列的每個事件，因此必須使用 **ProcessAllIfPresent** 選項呼叫 [**CoreWindowDispatch.ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents)。 其他選項可能會造成處理訊息事件的延遲，這可能會讓您的遊戲感覺無回應，或導致觸控行為變慢。

當遊戲未顯示、暫止或尚未進行快照時，您不會想要讓它使用任何資源迴圈來分派永遠不會抵達的訊息。 在此情況下，您的遊戲必須使用 [ **ProcessOneAndAllPending** ] 選項。 該選項會封鎖直到取得事件，然後在處理第一個事件時處理該事件（以及任何其他到達進程佇列的其他專案）。 然後， **ProcessEvents**會在處理完佇列之後立即傳回。

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
                    CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="the-appuninitialize-method"></a>App：：解除初始化方法

遊戲結束時，會呼叫[**IFrameworkView：：解除初始化**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)的執行。 這是我們執行清除的機會。 關閉應用程式視窗並不會終止應用程式的進程;但改為將應用程式的狀態寫入記憶體。 如果在系統回收此記憶體時必須進行任何特殊動作（包括任何特殊的資源清除），請將該清除的程式碼放在取消**初始化**中。

在我們的案例中， **App：：解除初始化**是無作用的。

```cpp
void Uninitialize()
{
}
```

## <a name="tips"></a>提示

開發您自己的遊戲時，請根據本主題中所述的方法來設計您的啟動程式碼。 以下是每個方法的基本建議的簡單列表。

- 使用**Initialize**來配置您的主要類別，並連接基本的事件處理常式。
- 使用**SetWindow**來訂閱任何視窗特定的事件，並將您的主視窗傳遞至裝置相依的資源物件，讓它可以在建立交換鏈時使用該視窗。
- 使用**Load**來處理任何剩餘的設定，以及起始物件的非同步建立和資源的載入。 如果您需要建立任何暫存檔案或資料，例如 cti 產生的資產，則也請在這裡執行此動作。

## <a name="next-steps"></a>後續步驟

本主題涵蓋了使用 DirectX 之 UWP 遊戲的一些基本結構。 建議您記住這些方法，因為我們將在稍後的主題中回頭說明。

在下一個主題「 &mdash; [遊戲流程管理](tutorial-game-flow-management.md)」中， &mdash; 我們將深入探討如何管理遊戲狀態和事件處理，以便讓遊戲流動。
