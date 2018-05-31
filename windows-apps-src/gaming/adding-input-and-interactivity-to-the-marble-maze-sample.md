---
author: eliotcowley
title: 在 Marble Maze 範例中加入輸入和互動
description: 深入了解您在使用輸入裝置時應該牢記的一些重要做法。
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
ms.author: elcowle
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10 , UWP, 遊戲, 輸入, 範例
ms.localizationpriority: medium
ms.openlocfilehash: 2be43690726112d8597747ee51dd94baf0f40f0e
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817043"
---
# <a name="adding-input-and-interactivity-to-the-marble-maze-sample"></a>在 Marble Maze 範例中加入輸入和互動




通用 Windows 平台 (UWP) 遊戲可在各種裝置上執行，例如桌上型電腦、膝上型電腦和平板電腦。 裝置可以具備需多輸入和控制機制。 本文件說明使用輸入裝置時需要牢記的重要做法，並示範 Marble Maze 如何運用這些做法。

> [!NOTE]
> 與本文件對應的範例程式碼可以在 [DirectX Marble Maze 遊戲範例](http://go.microsoft.com/fwlink/?LinkId=624011)中找到。

 
以下是本文件所討論在遊戲中使用輸入時的一些重點：

-   盡可能支援多種輸入裝置，讓您的遊戲兼顧客戶更廣泛的各種偏好和能力。 雖然遊戲控制器和感應器的使用並非必要，但強烈建議使用它來增強玩家體驗。 我們已設計遊戲控制器和感應器 API 來協助您更輕鬆地整合這些輸入裝置。

-   若要初始化觸控，您必須登錄視窗事件，例如在指標啟動、釋放和移動時。 若要初始化加速計，請在初始化應用程式時建立 [Windows::Devices::Sensors::Accelerometer](https://msdn.microsoft.com/library/windows/apps/br225687) 物件。 Xbox 控制器不需要初始化。

-   對於單人遊戲，請考慮是否要合併來自所有可能的 Xbox 控制器的輸入。 如此一來，您就不需要追蹤哪項輸入來自哪個控制器。 或者，您也可以像我們在此範例中，只要追蹤從最近新增的控制器的輸入。

-   在處理輸入裝置之前先處理 Windows 事件。

-   Xbox 控制器和加速計支援輪詢。 也就是說，您可以在需要時輪詢資料。 對於觸控，請將觸控事件記錄在可供輸入處理程式碼使用的資料結構中。

-   考慮是否將輸入值正規化為一般格式。 這樣做可以簡化遊戲的其他元件解譯輸入的方式 (例如物理模擬)，也能更輕鬆地撰寫可在不同螢幕解析度下執行的遊戲。

## <a name="input-devices-supported-by-marble-maze"></a>Marble Maze 支援的輸入裝置


Marble Maze 支援以 Xbox 控制器、滑鼠及觸控來選取選單項目，也支援以 Xbox 控制器、滑鼠、觸控和加速計來控制遊戲進行。 Marble Maze 使用 [Windows::Gaming::Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) API 來輪詢控制器的輸入。 觸控可讓應用程式追蹤並回應指尖輸入。 加速計是測量沿著 X 軸、Y 軸和 Z 軸所施加力量的感應器。 您可以使用 Windows 執行階段來輪詢加速計裝置的目前狀態，以及透過 Windows 執行階段事件處理機制來接收觸控事件。

> [!NOTE]
> 本文件使用「觸控」來表示觸控輸入和滑鼠輸入兩者，使用「指標」來表示任何使用指標事件的裝置。 由於觸控和滑鼠會使用標準指標事件，因此，您可以使用任一裝置來選取選單項目和控制遊戲進行。

 

> [!NOTE]
> 套件資訊清單將 **Landscape** 設為僅遊戲支援的旋轉方式，以防止當您旋轉裝置讓彈珠滾動時改變方向。 在 Visual Studio **\[方案總管\]** 中，按兩下套件資訊清單檔案 **Package.appxmanifest**。

 

## <a name="initializing-input-devices"></a>初始化輸入裝置


Xbox 控制器不需要初始化。 若要初始化觸控，您必須登錄視窗事件，像是啟動 (例如玩家按下滑鼠按鈕或觸碰螢幕)、釋放和移動指標等事件。 若要初始化加速計，您必須在初始化應用程式時建立 [Windows::Devices::Sensors::Accelerometer](https://msdn.microsoft.com/library/windows/apps/br225687) 物件。

下列範例顯示 **App::SetWindow** 方法如何登錄 [Windows::UI::Core::CoreWindow::PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerPressed)、[Windows::UI::Core::CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerReleased) 和 [Windows::UI::Core::CoreWindow::PointerMoved](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerMoved) 指標事件。 這些事件是在應用程式初始化期間和遊戲迴圈之前登錄。

這些事件是在叫用事件處理常式的個別執行緒中處理的。

如需如何初始化應用程式的詳細資訊，請參閱 [Marble Maze 應用程式結構](marble-maze-application-structure.md)。

```cpp
window->PointerPressed += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerPressed);

window->PointerReleased += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerReleased);

window->PointerMoved += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerMoved);
```

 **MarbleMazeMain** 類別也會建立 **std::map** 物件來保存觸控事件。 這個 map 物件的索引鍵是唯一識別輸入指標的值。 每個索引鍵對應至每個觸控點與螢幕中心之間的距離。 Marble Maze 之後會使用這些值來計算迷宮傾斜程度。

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

**MarbleMazeMain** 類別也會保留 [Accelerometer](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) 物件。

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

**Accelerometer** 物件在 **MarbleMazeMain** 建構函式中初始化，如下列範例所示。 [Windows::Devices::Sensors::Accelerometer::GetDefault](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer.GetDefault) 方法會傳回預設加速計的執行個體。 如果沒有預設加速計，**Accelerometer::GetDefault** 會傳回 **nullptr**。

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  <a name="navigating-the-menus"></a>巡覽選單

您可以使用滑鼠、觸控或 Xbox 控制器來巡覽選單，如下所示：

-   使用方向鍵來變更現用選單項目。
-   使用觸控、A 按鈕或選單鍵來挑選選單項目或關閉目前的選單，例如計分排行榜。
-   使用選單鍵來讓遊戲暫停或繼續。
-   以滑鼠按一下選單項目來選擇該動作。

###  <a name="tracking-xbox-controller-input"></a>追蹤 Xbox 控制器輸入

若要追蹤目前連接到裝置的遊戲台，**MarbleMazeMain** 定義成員變數 **m_myGamepads**，這是一系列 [Windows::Gaming::Input::Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad) 物件。 這會在建構函式中初始化，像這樣：

```cpp
m_myGamepads = ref new Vector<Gamepad^>();

for (auto gamepad : Gamepad::Gamepads)
{
    m_myGamepads->Append(gamepad);
}
```

此外，**MarbleMazeMain** 建構函式會在新增或移除遊戲台時登錄事件：

```cpp
Gamepad::GamepadAdded += 
    ref new EventHandler<Gamepad^>([=](Platform::Object^, Gamepad^ args)
{
    m_myGamepads->Append(args);
    m_currentGamepadNeedsRefresh = true;
});

Gamepad::GamepadRemoved += 
    ref new EventHandler<Gamepad ^>([=](Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        m_myGamepads->RemoveAt(indexRemoved);
        m_currentGamepadNeedsRefresh = true;
    }
});
```

當您新增遊戲台時，它是新增至 **m_myGamepads**；當移除遊戲台時，我們是在 **m_myGamepads** 中檢查遊戲台，如果是，我們將它移除。 上述兩種狀況中，我們將 **m_currentGamepadNeedsRefresh** 設定為 **true**，表示我們需要重新指派 **m_gamepad**。

最後，我們將遊戲台指派到 **m_gamepad**並將 **m_currentGamepadNeedsRefresh** 設定為 **false**：

```cpp
m_gamepad = GetLastGamepad();
m_currentGamepadNeedsRefresh = false;
```

在 **Update** 方法中，我們會檢查是否 **m_gamepad** 需要重新指派：

```cpp
if (m_currentGamepadNeedsRefresh)
{
    auto mostRecentGamepad = GetLastGamepad();

    if (m_gamepad != mostRecentGamepad)
    {
        m_gamepad = mostRecentGamepad;
    }

    m_currentGamepadNeedsRefresh = false;
}
```

如果 **m_gamepad** 需要重新指派，我們使用 **GetLastGamepad** 將指派最近新增的遊戲台，其定義如下：

```cpp
Gamepad^ MarbleMaze::MarbleMazeMain::GetLastGamepad()
{
    Gamepad^ gamepad = nullptr;

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(m_myGamepads->Size - 1);
    }

    return gamepad;
}
```

此方法只會傳回 **m_myGamepads** 中的最後一個遊戲台。

您最多可以將四個 Xbox 控制器連接至 Windows 10 裝置。 若要避免必須找出哪個控制器使用中，我們只要追蹤最近新增的遊戲台即可。 如果您的遊戲支援多位玩家，您必須個別追蹤每一位玩家的輸入。

**MarbleMazeMain::Update** 方法會輪詢輸入用的遊戲台：

```cpp
if (m_gamepad != nullptr)
{
    m_oldReading = m_newReading;
    m_newReading = m_gamepad->GetCurrentReading();
}
```

我們持續追蹤我們使用 **m_oldReading** 取得的輸入讀取，以及使用 **m_newReading** 取得的最新輸入讀取，這些是藉由呼叫 [Gamepad::GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading) 而取得。 這會傳回 [GamepadReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadreading) 物件，包含遊戲台目前狀態的相關資訊。

若要查看是否按鈕只是按下或釋放，我們會定義 **MarbleMazeMain::ButtonJustPressed** 和 **MarbleMazeMain::ButtonJustReleased**，這會比較這個框架和最後一個框架的按鈕讀取。 這樣我們可以只在按鈕最初按下或放開時才執行動作，而不是在按住按鈕時：

```cpp
bool MarbleMaze::MarbleMazeMain::ButtonJustPressed(GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (m_newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (m_oldReading.Buttons & selection));
    return newSelectionPressed && !oldSelectionPressed;
}

bool MarbleMaze::MarbleMazeMain::ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased = 
        (GamepadButtons::None == (m_newReading.Buttons & selection));

    bool oldSelectionReleased = 
        (GamepadButtons::None == (m_oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

[GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons) 讀取會使用位元運算進行比較&mdash;我們會使用 *bitwise and* (&) 檢查是否按下按鈕。 我們透過比較舊讀取和新讀取，來判斷按鈕是否剛按下或放開。

我們使用上述方法，檢查按鈕是否已按下，並執行任何必須發生的對應動作。 例如，按下 [選單] 按鈕 (**GamepadButtons::Menu**) 時，遊戲狀態會從作用中變成暫停，或從暫停變成作用中。

```cpp
if (ButtonJustPressed(GamepadButtons::Menu) || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;

    if (m_gameState == GameState::InGameActive)
    {
        SetGameState(GameState::InGamePaused);
    }  
    else if (m_gameState == GameState::InGamePaused)
    {
        SetGameState(GameState::InGameActive);
    }
}
```

我們也會檢查玩家是否按下 [檢視] 按鈕，此時我們會重新啟動遊戲，或清除計分排行榜：

```cpp
if (ButtonJustPressed(GamepadButtons::View) || m_homeKeyPressed)
{
    m_homeKeyPressed = false;

    if (m_gameState == GameState::InGameActive ||
        m_gameState == GameState::InGamePaused ||
        m_gameState == GameState::PreGameCountdown)
    {
        SetGameState(GameState::MainMenu);
        m_inGameStopwatchTimer.SetVisible(false);
        m_preGameCountdownTimer.SetVisible(false);
    }
    else if (m_gameState == GameState::HighScoreDisplay)
    {
        m_highScoreTable.Reset();
    }
}
```

如果主選單作用中，當方向鍵按上或下時，現用選單項目會變更。 如果使用者選擇目前的選取項目，適當的 UI 元素會標示為已選取。

```cpp
// Handle menu navigation.
bool chooseSelection = 
    (ButtonJustPressed(GamepadButtons::A) 
    || ButtonJustPressed(GamepadButtons::Menu));

bool moveUp = ButtonJustPressed(GamepadButtons::DPadUp);
bool moveDown = ButtonJustPressed(GamepadButtons::DPadDown);

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);
        if (m_startGameButton.GetSelected())
        {
            m_startGameButton.SetPressed(true);
        }
        if (m_highScoreButton.GetSelected())
        {
            m_highScoreButton.SetPressed(true);
        }
    }
    if (moveUp || moveDown)
    {
        m_startGameButton.SetSelected(!m_startGameButton.GetSelected());
        m_highScoreButton.SetSelected(!m_startGameButton.GetSelected());
        m_audio.PlaySoundEffect(MenuChangeEvent);
    }
    break;

case GameState::HighScoreDisplay:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::MainMenu);
    }
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::HighScoreDisplay);
    }
    break;

case GameState::InGamePaused:
    if (m_pausedText.IsPressed())
    {
        m_pausedText.SetPressed(false);
        SetGameState(GameState::InGameActive);
    }
    break;
}
```

### <a name="tracking-touch-and-mouse-input"></a>追蹤觸控和滑鼠輸入

對於觸控和滑鼠輸入，當使用者觸碰或按一下選單項目時，就會加以選擇。 下列範例顯示 **MarbleMazeMain::Update** 方法如何處理指標輸入來選取選單項目。 **m\_pointQueue** 成員變數會追蹤使用者在螢幕上觸碰或按一下的位置。 本文件稍後的[處理指標輸入](#processing-pointer-input)一節會進一步說明 Marble Maze 收集指標輸入的方式。

```cpp
// Check whether the user chose a button from the UI. 
bool anyPoints = !m_pointQueue.empty();
while (!m_pointQueue.empty())
{
    UserInterface::GetInstance().HitTest(m_pointQueue.front());
    m_pointQueue.pop();
}
```

**UserInterface::HitTest** 方法會判斷提供的點是否位於任何 UI 元素的邊界內。 任何通過此測試的 UI 元素會標示為已觸碰。 此方法是使用 **PointInRect** 函式來判斷提供的點是否位於每個 UI 元素的邊界內。

```cpp
void UserInterface::HitTest(D2D1_POINT_2F point)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if (!(*iter)->IsVisible())
            continue;

        TextButton* textButton = dynamic_cast<TextButton*>(*iter);
        if (textButton != nullptr)
        {
            D2D1_RECT_F bounds = (*iter)->GetBounds();
            textButton->SetPressed(PointInRect(point, bounds));
        }
    }
}
```

### <a name="updating-the-game-state"></a>更新遊戲狀態

**MarbleMazeMain::Update** 方法在處理控制器和觸控輸入之後，如果有任何按鈕按下，便會更新遊戲狀態。

```cpp
// Update the game state if the user chose a menu option. 
if (m_startGameButton.IsPressed())
{
    SetGameState(GameState::PreGameCountdown);
    m_startGameButton.SetPressed(false);
}
if (m_highScoreButton.IsPressed())
{
    SetGameState(GameState::HighScoreDisplay);
    m_highScoreButton.SetPressed(false);
}
```

##  <a name="controlling-game-play"></a>控制遊戲進行


遊戲迴圈和 **MarbleMazeMain::Update** 方法會共同合作來更新遊戲物件的狀態。 如果您的遊戲接受來自多個裝置的輸入，您可以將所有裝置的輸入累積在一組變數中，以便您撰寫更容易維護的程式碼。 **MarbleMazeMain::Update** 方法會定義一組變數來累積所有裝置的動作。

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

輸入機制可隨不同輸入裝置而有所不同。 例如，指標輸入是使用 Windows 執行階段事件處理模型來處理。 相反地，您在需要時會輪詢 Xbox 控制器的輸入資料。 我們建議您一律遵循特定裝置所規定的輸入機制。 本節說明 Marble Maze 如何讀取每個裝置的輸入、如何更新合併的輸入值，以及如何使用合併的輸入值來更新遊戲的狀態。

###  <a name="processing-pointer-input"></a>處理指標輸入

當您使用指標輸入時，請呼叫 [Windows::UI::Core::CoreDispatcher::ProcessEvents](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) 方法來處理視窗事件。 在更新或呈現場景之前，請在遊戲迴圈中呼叫此方法。 Marble Maze 會在  **App::Run** 方法中呼叫這個： 

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        m_main->Update();

        if (m_main->Render())
        {
            m_deviceResources->Present();
        }
    }
    else
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

如果看到視窗，我們將傳送 **CoreProcessEventsOption::ProcessAllIfPresent** 到 **ProcessEvents**，以處理所有佇列的活動，並立即傳回；否則，我們將傳送 **CoreProcessEventsOption::ProcessOneAndAllPending** 以處理所有佇列的事件並等待下一個新事件。 在處理事件之後，Marble Maze 會呈現並顯示下一個畫面。

Windows 執行階段會針對產生的每個事件呼叫已登錄的處理常式。 **App::SetWindow** 方法會登錄事件，並將指標資訊轉送至 **MarbleMazeMain** 類別。

```cpp
void App::OnPointerPressed(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void App::OnPointerReleased(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->RemoveTouch(args->CurrentPoint->PointerId);
}

void App::OnPointerMoved(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

**MarbleMazeMain** 類別會將保留觸控事件的 map 物件更新，以反應指標事件。 在第一次按下指標時，例如當使用者在啟用觸控的裝置上最初觸碰螢幕時，會呼叫 **MarbleMazeMain::AddTouch** 方法。 當指標位置移動時會呼叫 **MarbleMazeMain::UpdateTouch** 方法。 當釋放指標時 (例如，當使用者停止觸碰螢幕時)，會呼叫 **MarbleMazeMain::RemoveTouch** 方法。

```cpp
void MarbleMazeMain::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMazeMain::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());
}

void MarbleMazeMain::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

**PointToTouch** 函式會轉譯目前的指標位置，讓原點位於螢幕的中央，然後調整座標，讓它們的範圍大約介於 -1.0 到 +1.0 之間。 這樣較易於在不同輸入方法之間，以一致的方式來計算迷宮的傾斜度。

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Size bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

**MarbleMazeMain::Update** 方法會以固定縮放值來遞增傾斜係數，以更新合併的輸入值。 此縮放值是試驗過許多不同的值之後而決定。

```cpp
// Account for touch input.
for (TouchMap::const_iterator iter = m_touches.cbegin(); 
    iter != m_touches.cend(); 
    ++iter)
{
    combinedTiltX += iter->second.x * m_touchScaleFactor;
    combinedTiltY += iter->second.y * m_touchScaleFactor;
}
```

### <a name="processing-accelerometer-input"></a>處理加速計輸入

若要處理加速計輸入，**MarbleMazeMain::Update** 方法會呼叫 [Windows::Devices::Sensors::Accelerometer::GetCurrentReading](https://msdn.microsoft.com/library/windows/apps/br225699) 方法。 這個方法會傳回代表加速計讀數的 [Windows::Devices::Sensors::AccelerometerReading](https://msdn.microsoft.com/library/windows/apps/br225688) 物件。 **Windows::Devices::Sensors::AccelerometerReading::AccelerationX** 和 **Windows::Devices::Sensors::AccelerometerReading::AccelerationY** 屬性分別保有沿著 X 軸和 Y 軸的重力加速度。

下列範例顯示 **MarbleMazeMain::Update** 方法如何輪詢加速計及更新合併的輸入值。 當您傾斜裝置時，重力會讓彈珠移動得更快。

```cpp
// Account for sensors.
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += 
            static_cast<float>(reading->AccelerationX) * m_accelerometerScaleFactor;

        combinedTiltY += 
            static_cast<float>(reading->AccelerationY) * m_accelerometerScaleFactor;
    }
}
```

由於您無法確定使用者電腦上是否有加速計，因此在輪詢加速計之前，請一律要確定有一個有效的  [Accelerometer](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) 物件。

### <a name="processing-xbox-controller-input"></a>處理 Xbox 控制器輸入

在 **MarbleMazeMain::Update** 方法中，我們使用 **m_newReading** 來處理左邊類比搖桿的輸入：

```cpp
float leftStickX = static_cast<float>(m_newReading.LeftThumbstickX);
float leftStickY = static_cast<float>(m_newReading.LeftThumbstickY);

auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

if ((oppositeSquared + adjacentSquared) > m_deadzoneSquared)
{
    combinedTiltX += leftStickX * m_controllerScaleFactor;
    combinedTiltY += leftStickY * m_controllerScaleFactor;
}
```

我們會檢查左邊類比搖桿的輸入是否在靜止區域以外，若是，我們將其新增到 **combinedTiltX** 和 **combinedTiltY** (乘上縮放比例) 以傾斜階段。

> [!IMPORTANT]
> 當您使用 Xbox 控制器時，請一律將靜止區域列入考量。 靜止區域是指不同遊戲台對於初始移動的敏感度差異。 在某些控制器中，細微的移動可能不會產生讀數，但在其他控制器中，卻可能產生可測出的讀數。 為了讓遊戲納入此一考量，請為初始搖捍移動建立非移動區域。 如需靜止區域的詳細資訊，請參閱[讀取搖桿](gamepad-and-vibration.md#reading-the-thumbsticks)。

 

###  <a name="applying-input-to-the-game-state"></a>將輸入套用至遊戲狀態

裝置會以不同的方式報告輸入值。 例如，指標輸入可能以螢幕座標表示，而控制器輸入可能以完全不同的格式表示。 將多個裝置的輸入合併為一組輸入值的挑戰在於正規化，或將值轉換成一般格式。 Marble Maze 會將值縮放至範圍 \[-1.0, 1.0\] 來加以正規化。 本節稍早所述的 **PointToTouch** 函式會將螢幕座標轉換為介於大約 -1.0 和 +1.0 之間的範圍。

> [!TIP]
> 即使您的應用程式只使用一個輸入方法，仍建議您一律將輸入值正規化。 這樣做可簡化遊戲的其他元件解譯輸入的方式 (例如物理模擬)，也能更輕鬆地撰寫可在不同螢幕解析度下執行的遊戲。

 

**MarbleMazeMain::Update** 方法在處理輸入之後，會建立向量來代表迷宮傾斜對彈珠的效果。 下列範例示範 Marble Maze 如何使用 [XMVector3Normalize](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.geometric.xmvector3normalize) 函式來建立經過正規化的重力向量。 **maxTilt** 變數會限制迷宮傾斜的程度，避免迷宮翻覆。

```cpp
const float maxTilt = 1.0f / 8.0f;

XMVECTOR gravity = XMVectorSet(
    combinedTiltX * maxTilt, 
    combinedTiltY * maxTilt, 
    1.0f, 
    0.0f);

gravity = XMVector3Normalize(gravity);
```

為了完成場景物件更新，Marble Maze 會將更新的重力向量傳遞到物理模擬、更新前一個畫面以來所經過時間的物理模擬，以及更新彈珠的位置和方向。 如果彈珠掉落到迷宮底下，**MarbleMazeMain::Update** 方法會將彈珠放回彈珠上次碰觸的關卡，並重設物理模擬的狀態。

```cpp
XMFLOAT3A g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);

if (m_gameState == GameState::InGameActive)
{
    // Only update physics when gameplay is active.
    m_physics.UpdatePhysicsSimulation(static_cast<float>(m_timer.GetElapsedSeconds()));

    // ...Code omitted for simplicity...

}

// ...Code omitted for simplicity...

// Check whether the marble fell off of the maze. 
const float fadeOutDepth = 0.0f;
const float resetDepth = 80.0f;
if (marblePosition.z >= fadeOutDepth)
{
    m_targetLightStrength = 0.0f;
}
if (marblePosition.z >= resetDepth)
{
    // Reset marble.
    memcpy(&marblePosition, &m_checkpoints[m_currentCheckpoint], sizeof(XMFLOAT3));
    oldMarblePosition = marblePosition;
    m_physics.SetPosition((const XMFLOAT3&)marblePosition);
    m_physics.SetVelocity(XMFLOAT3(0, 0, 0));
    m_lightStrength = 0.0f;
    m_targetLightStrength = 1.0f;

    m_resetCamera = true;
    m_resetMarbleRotation = true;
    m_audio.PlaySoundEffect(FallingEvent);
}
```

本節不會說明物理模擬的運作方式。 如需詳細資訊，請參閱 Marble Maze 原始檔中的  **Physics.h** 和 **Physics.cpp**。

## <a name="next-steps"></a>後續步驟


如需使用音訊時應該牢記的一些重要做法的相關資訊，請參閱[在 Marble Maze 範例中加入音訊](adding-audio-to-the-marble-maze-sample.md)。 該文件會討論 Marble Maze 如何使用 Microsoft 媒體基礎和 XAudio2 來載入、混合和播放音訊資源。

## <a name="related-topics"></a>相關主題


* [在 Marble Maze 範例中加入音訊](adding-audio-to-the-marble-maze-sample.md)
* [在 Marble Maze 範例中加入視覺化內容](adding-visual-content-to-the-marble-maze-sample.md)
* [使用 C++ 和 DirectX 開發 Marble Maze (UWP 遊戲)](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




