---
author: eliotcowley
title: Marble Maze 應用程式結構
description: DirectX 通用 Windows 平台 (UWP) 應用程式的結構與傳統型應用程式結構不同。
ms.assetid: 6080f0d3-478a-8bbe-d064-73fd3d432074
ms.author: elcowle
ms.date: 09/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 樣本, directx, 結構
ms.localizationpriority: medium
ms.openlocfilehash: 1272200bf128443c82807aec9df5559f207819e1
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6975357"
---
# <a name="marble-maze-application-structure"></a>Marble Maze 應用程式結構




DirectX 通用 Windows 平台 (UWP) 應用程式的結構與傳統型應用程式結構不同。 Windows 執行階段提供介面 (例如 [[Windows::UI::Core::ICoreWindow](https://msdn.microsoft.com/library/windows/desktop/aa383751)](https://msdn.microsoft.com/library/windows/apps/br208296))，讓您能夠使用更現代化的物件導向方式來開發 UWP 應用程式，而不是使用控制代碼類型 (例如 [HWND](https://msdn.microsoft.com/library/windows/desktop/ms632679)) 和函式 (例如 CreateWindow) 來進行開發。 文件的這一節示範如何建構 Marble Maze App 程式碼。

> [!NOTE]
> 與本文件對應的範例程式碼可以在 [DirectX Marble Maze 遊戲範例](http://go.microsoft.com/fwlink/?LinkId=624011)中找到。

 
## 
以下是本文件所討論在建構遊戲程式碼時的一些重點：

-   在初始化階段，設定遊戲所使用的執行階段和程式庫元件，並載入遊戲專用的資源。
-   UWP app 必須在啟動後的 5 秒內開始處理事件。 因此，載入應用程式時，只需載入必要資源。 遊戲應該在背景中載入大量資源，並顯示進度畫面。
-   在遊戲迴圈中，回應 Windows 事件、讀取使用者輸入、更新場景物件，以及轉譯場景。
-   使用事件處理常式來回應視窗事件 (這些處理常式會取代 Windows 傳統型應用程式的視窗訊息)。
-   使用狀態機器來控制遊戲邏輯的流程和順序。

##  <a name="file-organization"></a>檔案組織


Marble Maze 中的某些元件只要稍加修改 (甚至不需修改)，就可在任何遊戲中重複使用。 您可以針對自己的遊戲，適度調整這些檔案提供的組織和概念。 下表簡短說明重要的原始程式碼檔。

| 檔案                                      | 描述                                                                                                                                                                          |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| App.h、App.cpp               | 定義 **App** 和 **DirectXApplicationSource** 類別，封裝應用程式的檢視 (視窗、執行緒及事件)。                                                     |
| Audio.h、Audio.cpp                         | 定義管理音訊資源的 **Audio** 類別。                                                                                                                          |
| BasicLoader.h、BasicLoader.cpp             | 定義 **BasicLoader** 類別，提供公用程式方法來協助您載入紋理、網格和著色器。                                                                  |
| BasicMath.h                                | 定義結構和函式來協助您使用向量和矩陣資料及計算。 這其中有許多函式與 HLSL 著色器類型相容。                     |
| BasicReaderWriter.h、BasicReaderWriter.cpp | 定義 **BasicReaderWriter** 類別，在 UWP 應用程式中使用 Windows 執行階段來讀取和寫入檔案資料。                                                                    |
| BasicShapes.h、BasicShapes.cpp             | 定義 **BasicShapes** 類別，提供公用程式方法來建立基本圖形，例如立方體和球體 (Marble Maze 實作不會使用這些檔案)。 |                                                                                  |
| Camera.h、Camera.cpp                       | 定義 **Camera** 類別，提供相機的位置和方向。                                                                                               |
| Collision.h、Collision.cpp                 | 管理彈珠與其他物件 (例如迷宮) 之間的碰撞資訊。                                                                                                       |
| DDSTextureLoader.h、DDSTextureLoader.cpp   | 定義 **CreateDDSTextureFromMemory** 函式，從記憶體緩衝區載入 .dds 格式的紋理。                                                              |
| DirectXHelper.h             | 定義對許多 DirectX UWP app 有幫助的 DirectX 協助程式函式。                                                                            |
| LoadScreen.h、LoadScreen.cpp               | 定義 **LoadScreen** 類別，在應用程式初始化期間顯示載入畫面。                                                                                         |
| MarbleMazeMain.h、MarbleMazeMain.cpp               | 定義 **MarbleMazeMain** 類別，管理遊戲專用資源並定義許多遊戲邏輯。                                                                          |
| MediaStreamer.h、MediaStreamer.cpp         | 定義 **MediaStreamer** 類別，使用媒體基礎來協助遊戲管理音訊資源。                                                                            |
| PersistentState.h、PersistentState.cpp     | 定義 **PersistentState** 類別，在備份存放區讀取和寫入基本資料型別。                                                                      |
| Physics.h、Physics.cpp                     | 定義 **Physics** 類別，在彈珠和迷宮之間實作物理模擬。                                                                              |
| Primitives.h                               | 定義遊戲使用的幾何類型。                                                                                                                                   |
| SampleOverlay.h、SampleOverlay.cpp         | 定義 **SampleOverlay** 類別，提供常用的 2D 和使用者介面資料和作業。                                                                               |
| SDKMesh.h、SDKMesh.cpp                     | 定義 **SDKMesh** 類別，載入並呈現 SDK Mesh (.sdkmesh) 格式的網格。                                                                                |
| StepTimer.h               | 定義 **StepTimer** 類別，提供簡單的方法來取得總時間和耗用時間。
| UserInterface.h、UserInterface.cpp         | 定義與使用者介面相關的功能，例如功能表系統和計分排行榜。                                                                        |

 

##  <a name="design-time-versus-run-time-resource-formats"></a>設計階段與執行階段的資源格式


請盡可能使用執行階段格式，而非設計階段格式，以更有效率地載入遊戲資源。

*設計階段*格式是您在設計資源時所使用的格式。 一般而言，3D 設計工具可處理設計階段格式。 有些設計階段格式也是以文字為基礎的，因此可讓您在任何文字編輯器中加以修改。 設計階段格式可能很冗長，所包含的資訊可能超出遊戲所需。 *「執行階段」* 格式是由遊戲讀取的二進位格式。 執行階段格式通常較為精簡，載入時比對應的設計階段格式更有效率。 這就是為什麼大部分遊戲會在執行階段使用執行階段資產。

雖然您的遊戲可直接讀取設計階段格式，但使用不同的執行階段格式有許多好處。 由於執行階段格式通常較為精簡，因此所需的磁碟空間較少，而且在網路上傳輸的時間較短。 此外，執行階段格式通常表示為記憶體對應的資料結構。 因此，它們可以更快速地載入記憶體，例如，比 XML 型文字檔更快。 最後，因為個別的執行階段格式通常是以二進位編碼，所以更不易遭到使用者修改。

例如，HLSL 著色器就是使用不同設計階段和執行階段格式的一種資源。 Marble Maze 使用 .hlsl 做為設計階段格式，並使用 .cso 做為執行階段格式。 .hlsl 檔案保存著色器的原始程式碼；.cso 檔案保存對應的著色器位元組程式碼。 當您離線轉換 .hlsl 檔案並隨遊戲提供 .cso 檔案時，就不需要在遊戲載入時，將 HLSL 來源檔案轉換成位元組程式碼。

基於教學用途，Marble Maze 專案包含許多資源的設計階段格式和執行階段格式，不過，您只需要在遊戲的來源專案中維護設計階段格式，因為您可以在需要時再將它們轉換成執行階段格式。 本文件示範如何將設計階段格式轉換成執行階段格式。

##  <a name="application-life-cycle"></a>應用程式生命週期


Marble Maze 遵循一般 UWP 應用程式的生命週期。 如需 UWP 應用程式生命週期的詳細資訊，請參閱 [應用程式生命週期](https://msdn.microsoft.com/library/windows/apps/mt243287)。

當 UWP 遊戲初始化時，它通常會初始化執行階段元件，例如 Direct3D、Direct2D，以及它使用的任何輸入、音訊或物理程式庫。 它也會載入遊戲開始之前所需的遊戲專用資源。 這個初始化會在遊戲工作階段期間發生一次。

初始化之後，遊戲通常會執行 *「遊戲迴圈」*。 在這個迴圈中，遊戲通常會執行四個動作：處理 Windows 事件、收集輸入、更新場景物件，以及呈現場景。 當遊戲更新場景時，會將目前的輸入狀態套用到場景物件，並模擬物理事件，例如物件碰撞。 遊戲也可以執行其他活動，例如播放音效或經由網路傳送資料。 當遊戲呈現場景時，它會擷取場景的目前狀態，並將它繪製到顯示裝置。 下列章節將進一步說明這些活動。

##  <a name="adding-to-the-template"></a>新增到範本


**DirectX 11 應用程式 (通用 Windows)** 範本會建立一個可利用 Direct3D 呈現的核心視窗。 此範本也包含 **DeviceResources** 類別，該類別會建立在 UWP 應用程式中呈現 3D 內容需要的所有 Direct3D 裝置資源。

**App** 類別會建立 **MarbleMazeMain** 類別物件、開始載入資源、執行迴圈以更新計時器，並針對每個畫面呼叫 **MarbleMazeMain::Render** 轉譯方法。 **App::OnWindowSizeChanged**、**App::OnDpiChanged** 和 **App::OnOrientationChanged** 方法會各自呼叫 **MarbleMazeMain::CreateWindowSizeDependentResources** 方法，而 **App::Run** 方法則呼叫 **MarbleMazeMain::Update** 和 **MarbleMazeMain::Render** 方法。

下列範例示範 **App::SetWindow** 方法在何處建立 **MarbleMazeMain** 類別物件。 **DeviceResources** 類別會傳送到該方法，這樣它才能使用 Direct3D 物件進行轉譯。

```cpp
    m_main = std::unique_ptr<MarbleMazeMain>(new MarbleMazeMain(m_deviceResources));
```

**App** 類別也會開始為遊戲載入延遲的資源。 如需詳細資訊，請參閱下一節。

此外，**App** 類別還會設定 [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow) 事件的事件處理常式。 呼叫這些事件的處理常式時，它們會將輸入傳送到 **MarbleMazeMain** 類別。

## <a name="loading-game-assets-in-the-background"></a>在背景中載入遊戲資產


為了確保遊戲可以在啟動後的 5 秒內回應視窗事件，建議您以非同步方式或在背景中載入遊戲資產。 當資產在背景中載入時，您的遊戲可以回應視窗事件。

> [!NOTE]
> 您也可以顯示已備妥的主功能表，並讓其餘資產繼續在背景中載入。 在所有資源都載入之前，如果使用者從功能表中選取選項，您可以指出場景資源仍在載入中，例如顯示進度列。

 

即使遊戲包含的資產相當少，最好還是以非同步方式載入資產，這樣做有兩個理由。 其中一個理由是很難保證您的所有資源在所有裝置和所有組態上都能快速載入。 另外，儘早納入非同步載入，您的程式碼就可以隨著您新增功能而調整。

非同步資產載入是從 **App::Load** 方法開始。 這個方法使用 [task](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class) 類別在背景中載入遊戲資產。

```cpp
    task<void>([=]()
    {
        m_main->LoadDeferredResources(true, false);
    });
```

**MarbleMazeMain** 類別會定義 *m\_deferredResourcesReady* 旗標，以表示非同步載入完成。 **MarbleMazeMain::LoadDeferredResources** 方法會在載入遊戲資源後設定這個旗標。 應用程式的更新 (**MarbleMazeMain::Update**) 和呈現 (**MarbleMazeMain::Render**) 階段會檢查這個旗標。 如果設定了這個旗標，遊戲就會繼續正常執行。 如果尚未設定這個旗標，遊戲就會顯示正在載入的畫面。

如需 UWP 應用程式的非同步程式設計的詳細資訊，請參閱 [C++ 的非同步程式設計](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。

> [!TIP]
> 如果您使用 Windows 執行階段 C++ 程式庫 (也就是 DLL) 來撰寫遊戲程式碼，請考慮是否閱讀[使用 C++ 建立 UWP app 的非同步作業](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)，以了解如何建立可供應用程式和其他程式庫使用的非同步作業。

 

## <a name="the-game-loop"></a>遊戲迴圈


**App::Run** 方法會執行主要遊戲迴圈 (**MarbleMazeMain::Update**)。 每個畫面都會呼叫這個方法。

為了協助從遊戲特定程式碼中抽出檢視和視窗程式碼，我們實作 **App::Run** 方法，將更新和呈現呼叫轉送到 **MarbleMazeMain** 物件。

下列範例示範包含主要遊戲迴圈的 **App::Run** 方法。 該遊戲迴圈會更新時間總計和畫面時間變數，然後更新和呈現場景。 這樣也可確保只有在視窗可見時才會呈現內容。

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->
                ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->
                ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }

    // The app is exiting so do the same thing as if the app were being suspended.
    m_main->OnSuspending();

#ifdef _DEBUG
    // Dump debug info when exiting.
    DumpD3DDebug();
#endif //_DEGBUG
}
```

## <a name="the-state-machine"></a>狀態機器


遊戲通常包含 *「狀態機器」*(也稱為 *「有限狀態機器」* 或 FSM)，可控制遊戲邏輯的流程和順序。 狀態機器包含一定數目的狀態，也能夠在這些狀態之間切換。 狀態機器通常從 *「初始」* 狀態開始、轉換成一或多個 *「中繼」* 狀態，而且可能會以 *「終止」* 狀態結束。

遊戲迴圈通常會使用狀態機器，以執行目前遊戲狀態的特定邏輯。 Marble Maze 會定義 **GameState** 列舉，此列舉定義遊戲的每個可能的狀態。

```cpp
enum class GameState
{
    Initial,
    MainMenu,
    HighScoreDisplay,
    PreGameCountdown,
    InGameActive,
    InGamePaused,
    PostGameResults,
};
```

例如，**MainMenu** 狀態定義主功能表出現，遊戲沒在進行。 相反地，**InGameActive** 狀態定義遊戲在進行中，不顯示功能表。 **MarbleMazeMain** 類別定義 **m\_gameState** 成員變數來保留進行中的遊戲狀態。

**MarbleMazeMain::Update** 和 **MarbleMazeMain::Render** 方法使用 switch 陳述式執行目前狀態的邏輯。 下列範例示範 switch 陳述式在 **MarbleMazeMain::Update** 方法中可能的樣子 (移除詳細資料以說明結構)。

```cpp
switch (m_gameState)
{
case GameState::MainMenu:
    // Do something with the main menu. 
    break;

case GameState::HighScoreDisplay:
    // Do something with the high-score table. 
    break;

case GameState::PostGameResults:
    // Do something with the game results. 
    break;

case GameState::InGamePaused:
    // Handle the paused state. 
    break;
}
```

當遊戲邏輯或呈現取決於特定遊戲狀態時，我們會在此文件中強調。

## <a name="handling-app-and-window-events"></a>處理應用程式和視窗事件


Windows 執行階段提供物件導向的事件處理系統，讓您可以更輕鬆地管理 Windows 訊息。 若要在應用程式中使用事件，您必須提供事件處理常式 (或事件處理方法) 來回應事件。 您還必須向事件來源登錄事件處理常式。 這個程序通常稱為「事件連接」。

### <a name="supporting-suspend-resume-and-restart"></a>支援暫停、繼續及重新啟動

當使用者離開 Marble Maze 時，或當 Windows 進入低電源狀態時，Marble Maze 會暫停。 當使用者將遊戲移至前景時，或當 Windows 脫離低電源狀態時，遊戲會繼續。 您通常不需要關閉應用程式。 當應用程式處於暫停狀態，且 Windows 需要應用程式所使用的資源 (例如記憶體) 時，Windows 可以終止應用程式。 當應用程式即將暫停或繼續時，Windows 會通知應用程式，但是，當應用程式即將終止時，Windows 並不會通知應用程式。 因此，當 Windows 通知即將暫停應用程式時，應用程式必須能夠儲存任何必要資料，以便於應用程式重新啟動時還原目前的使用者狀態。 如果應用程式有大量使用者狀態需要耗費高度資源才能儲存，則即使是在應用程式接獲暫停通知之前，您也可能需要經常儲存狀態。 基於兩個理由，Marble Maze 會回應暫停和繼續通知：

1.  當應用程式暫停時，遊戲會儲存目前的遊戲狀態並暫停音訊播放。 當應用程式繼續時，遊戲會繼續音訊播放。
2.  當應用程式關閉而後來又重新啟動時，遊戲會從先前的狀態繼續執行。

Marble Maze 執行下列工作來支援暫停和繼續：

-   在遊戲的關鍵點，它會將狀態儲存至永續性儲存體，例如當使用者到達關卡時。
-   它會將狀態儲存至永續性儲存體，以回應暫停通知。
-   它會從永續性儲存體載入狀態，以回應繼續通知。 在啟動期間，它也會載入先前的狀態。

為了支援暫停和繼續，Marble Maze 定義 **PersistentState** 類別 (請參閱 **PersistentState.h** 和 **PersistentState.cpp**)。 這個類別使用 [Windows::Foundation::Collections::IPropertySet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IPropertySet) 介面來讀取和寫入屬性。 **PersistentState** 類別提供方法在備份存放區中讀取和寫入基本資料型別 (例如，**bool**、**int**、**float**、[XMFLOAT3](https://msdn.microsoft.com/library/windows/desktop/ee419475) 和 [Platform::String](https://docs.microsoft.com/cpp/cppcx/platform-string-class))。

```cpp
ref class PersistentState
{
internal:
    void Initialize(
        _In_ Windows::Foundation::Collections::IPropertySet^ settingsValues,
        _In_ Platform::String^ key
        );

    void SaveBool(Platform::String^ key, bool value);
    void SaveInt32(Platform::String^ key, int value);
    void SaveSingle(Platform::String^ key, float value);
    void SaveXMFLOAT3(Platform::String^ key, DirectX::XMFLOAT3 value);
    void SaveString(Platform::String^ key, Platform::String^ string);

    bool LoadBool(Platform::String^ key, bool defaultValue);
    int  LoadInt32(Platform::String^ key, int defaultValue);
    float LoadSingle(Platform::String^ key, float defaultValue);

    DirectX::XMFLOAT3 LoadXMFLOAT3(
        Platform::String^ key, 
        DirectX::XMFLOAT3 defaultValue);

    Platform::String^ LoadString(
        Platform::String^ key, 
        Platform::String^ defaultValue);

private:
    Platform::String^ m_keyName;
    Windows::Foundation::Collections::IPropertySet^ m_settingsValues;
};
```

**MarbleMazeMain** 類別會保留 **PersistentState** 物件。 **MarbleMazeMain** 建構函式會初始化此物件，並提供本機應用程式資料存放區做為備份資料存放區。

```cpp
m_persistentState = ref new PersistentState();

m_persistentState->Initialize(
    Windows::Storage::ApplicationData::Current->LocalSettings->Values,
    "MarbleMaze");
```

當彈珠通過關卡或目標時 (在 **MarbleMazeMain::Update** 方法中)，以及當視窗失去焦點時 (在 **MarbleMazeMain::OnFocusChange** 方法中)，Marble Maze 會儲存其狀態。 如果遊戲保留大量狀態資料，建議您以類似方式偶爾將狀態儲存至永續性儲存體，因為您只有幾秒鐘可回應暫停通知。 因此，應用程式在收到暫停通知時，只需要儲存已變更的狀態資料。

為回應暫停和繼續通知，**MarbleMazeMain** 類別會定義在暫停和繼續時呼叫的 **SaveState** 與 **LoadState** 方法。 **MarbleMazeMain::OnSuspending** 方法會處理暫停事件，**MarbleMazeMain::OnResuming** 方法則處理繼續事件。

**MarbleMazeMain::OnSuspending** 方法會儲存遊戲狀態並暫停音訊。

```cpp
void MarbleMazeMain::OnSuspending()
{
    SaveState();
    m_audio.SuspendAudio();
}
```

**MarbleMazeMain::SaveState** 方法會儲存遊戲狀態值，例如彈珠的目前位置和速度、最近的關卡，以及計分排行榜。

```cpp
void MarbleMazeMain::SaveState()
{
    m_persistentState->SaveXMFLOAT3(":Position", m_physics.GetPosition());
    m_persistentState->SaveXMFLOAT3(":Velocity", m_physics.GetVelocity());

    m_persistentState->SaveSingle(
        ":ElapsedTime", 
        m_inGameStopwatchTimer.GetElapsedTime());

    m_persistentState->SaveInt32(":GameState", static_cast<int>(m_gameState));
    m_persistentState->SaveInt32(":Checkpoint", static_cast<int>(m_currentCheckpoint));

    int i = 0;
    HighScoreEntries entries = m_highScoreTable.GetEntries();
    const int bufferLength = 16;
    char16 str[bufferLength];

    m_persistentState->SaveInt32(":ScoreCount", static_cast<int>(entries.size()));

    for (auto iter = entries.begin(); iter != entries.end(); ++iter)
    {
        int len = swprintf_s(str, bufferLength, L"%d", i++);
        Platform::String^ string = ref new Platform::String(str, len);

        m_persistentState->SaveSingle(
            Platform::String::Concat(":ScoreTime", string), 
            iter->elapsedTime);

        m_persistentState->SaveString(
            Platform::String::Concat(":ScoreTag", string), 
            iter->tag);
    }
}
```

當遊戲繼續時，只需要繼續播放音訊。 因為狀態已載入記憶體中，它不需要從永續性儲存體載入狀態。

[在 Marble Maze 範例中加入音訊](adding-audio-to-the-marble-maze-sample.md)文件中會解說遊戲如何暫停和繼續播放音訊。

為了支援重新啟動，**MarbleMazeMain** 建構函式 (在啟動期間呼叫) 會呼叫 **MarbleMazeMain::LoadState** 方法。 **MarbleMazeMain::LoadState** 方法會讀取狀態，並將狀態套用至遊戲物件。 如果遊戲已暫停 (paused)，這個方法也會將目前的遊戲狀態設定為暫停 (paused)，或當遊戲已暫停 (suspended) 時，設定為使用中。 我們暫停 (pause) 遊戲，以避免發生未預期的活動而讓使用者感到意外。 當遊戲暫停 (suspended) 時，如果遊戲不是在進行遊戲的狀態，它也會移至主功能表。

```cpp
void MarbleMazeMain::LoadState()
{
    XMFLOAT3 position = m_persistentState->LoadXMFLOAT3(
        ":Position", 
        m_physics.GetPosition());

    XMFLOAT3 velocity = m_persistentState->LoadXMFLOAT3(
        ":Velocity", 
        m_physics.GetVelocity());

    float elapsedTime = m_persistentState->LoadSingle(":ElapsedTime", 0.0f);

    int gameState = m_persistentState->LoadInt32(
        ":GameState", 
        static_cast<int>(m_gameState));

    int currentCheckpoint = m_persistentState->LoadInt32(
        ":Checkpoint", 
        static_cast<int>(m_currentCheckpoint));

    switch (static_cast<GameState>(gameState))
    {
    case GameState::Initial:
        break;

    case GameState::MainMenu:
    case GameState::HighScoreDisplay:
    case GameState::PreGameCountdown:
    case GameState::PostGameResults:
        SetGameState(GameState::MainMenu);
        break;

    case GameState::InGameActive:
    case GameState::InGamePaused:
        m_inGameStopwatchTimer.SetVisible(true);
        m_inGameStopwatchTimer.SetElapsedTime(elapsedTime);
        m_physics.SetPosition(position);
        m_physics.SetVelocity(velocity);
        m_currentCheckpoint = currentCheckpoint;
        SetGameState(GameState::InGamePaused);
        break;
    }

    int count = m_persistentState->LoadInt32(":ScoreCount", 0);

    const int bufferLength = 16;
    char16 str[bufferLength];

    for (int i = 0; i < count; i++)
    {
        HighScoreEntry entry;
        int len = swprintf_s(str, bufferLength, L"%d", i);
        Platform::String^ string = ref new Platform::String(str, len);

        entry.elapsedTime = m_persistentState->LoadSingle(
            Platform::String::Concat(":ScoreTime", string), 
            0.0f);

        entry.tag = m_persistentState->LoadString(
            Platform::String::Concat(":ScoreTag", string), 
            L"");

        m_highScoreTable.AddScoreToTable(entry);
    }
}
```

> [!IMPORTANT]
> Marble Maze 不會區分冷啟動 (也就是第一次啟動，先前沒有暫停事件) 和從暫停狀態中繼續。 建議所有 UWP 應用程式都採用此設計。

如需有關應用程式資料的詳細資訊，請參閱[儲存及擷取設定和其他應用程式資料](https://msdn.microsoft.com/library/windows/apps/mt299098)。

##  <a name="next-steps"></a>後續步驟


如需使用視覺化資源時應該牢記的一些重要做法的相關資訊，請參閱[在 Marble Maze 範例中加入視覺化內容](adding-visual-content-to-the-marble-maze-sample.md)。

## <a name="related-topics"></a>相關主題

* [在 Marble Maze 範例中加入視覺化內容](adding-visual-content-to-the-marble-maze-sample.md)
* [Marble Maze 範例基礎觀念](marble-maze-sample-fundamentals.md)
* [使用 C++ 和 DirectX 開發 Marble Maze (UWP 遊戲)](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




