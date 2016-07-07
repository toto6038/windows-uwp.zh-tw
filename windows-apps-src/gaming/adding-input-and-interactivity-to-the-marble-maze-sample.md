---
author: mtoepke
title: "在 Marble Maze 範例中加入輸入和互動"
description: "通用 Windows 平台 (UWP) app 遊戲可在各種裝置上執行，例如桌上型電腦、膝上型電腦和平板電腦。"
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 563ee292ec7189b0c365ae5ee0d1c41fd6fd1a09

---

# 在 Marble Maze 範例中加入輸入和互動


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


通用 Windows 平台 (UWP) app 遊戲可在各種裝置上執行，例如桌上型電腦、膝上型電腦和平板電腦。 裝置可以具備各種輸入和控制機制。 支援多種輸入裝置可讓您的遊戲兼顧客戶更廣泛的各種偏好和能力。 本文件說明使用輸入裝置時需要牢記的重要做法，並示範 Marble Maze 如何運用這些做法。

> 
            **注意** 與本文件對應的範例程式碼可以在 [DirectX Marble Maze 遊戲範例](http://go.microsoft.com/fwlink/?LinkId=624011)中找到。

 
以下是本文件所討論在遊戲中使用輸入時的一些重點：

-   盡可能支援多種輸入裝置，讓您的遊戲兼顧客戶更廣泛的各種偏好和能力。 雖然遊戲控制器和感應器的使用並非必要，但強烈建議使用它來增強玩家體驗。 我們已設計遊戲控制器和感應器 API 來協助您更輕鬆地整合這些輸入裝置。
-   若要初始化觸控，您必須登錄視窗事件，例如在指標啟動、釋放和移動時。 若要初始化加速計，請在初始化應用程式時建立 [**Windows::Devices::Sensors::Accelerometer**](https://msdn.microsoft.com/library/windows/apps/br225687) 物件。 Xbox 360 控制器不需要初始化。
-   對於單人遊戲，請考慮是否要合併來自所有可能的 Xbox 360 控制器的輸入。 如此一來，您就不需要追蹤哪項輸入來自哪個控制器。
-   在處理輸入裝置之前先處理 Windows 事件。
-   Xbox 360 控制器和加速計支援輪詢。 也就是說，您可以在需要時輪詢資料。 對於觸控，請將觸控事件記錄在可供輸入處理程式碼使用的資料結構中。
-   考慮是否將輸入值正規化為一般格式。 這樣做可以簡化遊戲的其他元件解譯輸入的方式 (例如物理模擬)，也能更輕鬆地撰寫可在不同螢幕解析度下執行的遊戲。

## Marble Maze 支援的輸入裝置


Marble Maze 支援以 Xbox 360 一般控制器裝置、滑鼠及觸控來選取功能表項目，也支援以 Xbox 360 控制器、滑鼠、觸控和加速計來控制遊戲進行。 Marble Maze 使用 XInput API 來輪詢控制器的輸入。 觸控可讓應用程式追蹤並回應指尖輸入。 加速計是測量沿著 X 軸、Y 軸和 Z 軸所施加力量的感應器。 您可以使用 Windows 執行階段來輪詢加速計裝置的目前狀態，以及透過 Windows 執行階段事件處理機制來接收觸控事件。

> 
            **注意** 本文件使用「觸控」來表示觸控輸入和滑鼠輸入兩者，而使用「指標」來表示任何使用指標事件的裝置。 由於觸控和滑鼠會使用標準指標事件，因此，您可以使用任一裝置來選取功能表項目和控制遊戲進行。

 

> 
            **注意** 套件資訊清單將 Landscape 設為遊戲支援的旋轉方式，以防止當您旋轉裝置讓彈珠滾動時改變方向。

 

## 初始化輸入裝置


Xbox 360 控制器不需要初始化。 若要初始化觸控，您必須登錄視窗事件，像是啟動 (例如使用者按下滑鼠按鈕或觸碰螢幕)、釋放和移動指標等事件。 若要初始化加速計，您必須在初始化應用程式時建立 [**Windows::Devices::Sensors::Accelerometer**](https://msdn.microsoft.com/library/windows/apps/br225687) 物件。

下列範例顯示 **DirectXPage** 建構函式如何登錄 [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) 的 [**Windows::UI::Core::CoreIndependentInputSource::PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn298471)、[**Windows::UI::Core::CoreIndependentInputSource::PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn298472) 和 [**Windows::UI::Core::CoreIndependentInputSource::PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn298469) 指標事件。 這些事件是在應用程式初始化期間和遊戲迴圈之前登錄。

這些事件是在叫用事件處理常式的個別執行緒中處理的。

如需如何初始化應用程式的詳細資訊，請參閱 [Marble Maze 應用程式結構](marble-maze-application-structure.md)。

```cpp
coreInput->PointerPressed += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerPressed);
coreInput->PointerMoved += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerMoved);
coreInput->PointerReleased += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerReleased);
```

MarbleMaze 類別也會建立 std::map 物件來保存觸控事件。 這個 map 物件的索引鍵是唯一識別輸入指標的值。 每個索引鍵對應至每個觸控點與螢幕中心之間的距離。 Marble Maze 之後會使用這些值來計算迷宮傾斜程度。

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

MarbleMaze 類別會保留 Accelerometer 物件。

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

Accelerometer 物件在 MarbleMaze::Initialize 方法中初始化，如下列範例所示。 Windows::Devices::Sensors::Accelerometer::GetDefault 方法會傳回預設加速計的執行個體。 如果沒有預設加速計，Accelerometer::GetDefault 之 m\_accelerometer 的值會保持為 nullptr。

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  瀏覽功能表


###  追蹤 Xbox 360 控制器輸入

您可以使用滑鼠、觸控或 Xbox 360 控制器來瀏覽功能表，如下所示：

-   使用方向鍵來變更現用功能表項目。
-   使用觸控、A 按鈕或 \[開始\] 按鈕來挑選功能表項目或關閉目前的功能表，例如計分排行榜。
-   使用 \[開始\] 按鈕來讓遊戲暫停或繼續。
-   以滑鼠按一下功能表項目來選擇該動作。

###  追蹤觸控和滑鼠輸入

為了追蹤 Xbox 360 控制器輸入，**MarbleMaze::Update** 方法會定義一組按鈕來定義輸入行為。 XInput 只提供控制器的目前狀態。 因此，**MarbleMaze::Update** 還會定義兩組陣列，以追蹤每個可能的 Xbox 360 控制器是否在前一個畫面期間按下按鈕，以及目前是否按下按鈕。

```cpp
// This array contains the constants from XINPUT that map to the 
// particular buttons that are used by the game. 
// The index of the array is used to associate the state of that button in 
// the wasButtonDown, isButtonDown, and combinedButtonPressed variables. 

static const WORD buttons[] = {
    XINPUT_GAMEPAD_A,
    XINPUT_GAMEPAD_START,
    XINPUT_GAMEPAD_DPAD_UP,
    XINPUT_GAMEPAD_DPAD_DOWN,
    XINPUT_GAMEPAD_DPAD_LEFT,
    XINPUT_GAMEPAD_DPAD_RIGHT,
    XINPUT_GAMEPAD_BACK,
};

static const int buttonCount = ARRAYSIZE(buttons);
static bool wasButtonDown[XUSER_MAX_COUNT][buttonCount] = { false, };
bool isButtonDown[XUSER_MAX_COUNT][buttonCount] = { false, };
```

您最多可以將四個 Xbox 360 控制器連接至 Windows 裝置。 為了避免需要找出哪個控制器才是作用中的控制器，**MarbleMaze::Update** 方法會合併所有控制器的輸入。

```cpp
bool combinedButtonPressed[buttonCount] = { false, };
```

如果您的遊戲支援多位玩家，您必須個別追蹤每一位玩家的輸入。

在迴圈中，**MarbleMaze::Update** 方法會輪詢每個控制器以取得輸入，並讀取每個按鈕的狀態。

```cpp
// Account for input on any connected controller.
XINPUT_STATE inputState = {0};
for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
{
    DWORD result = XInputGetState(userIndex, &inputState);
    if(result != ERROR_SUCCESS) 
        continue;

    SHORT thumbLeftX = inputState.Gamepad.sThumbLX;
    if (abs(thumbLeftX) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftX = 0;

    SHORT thumbLeftY = inputState.Gamepad.sThumbLY;
    if (abs(thumbLeftY) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftY = 0;

    combinedTiltX += static_cast<float>(thumbLeftX) / 32768.0f;
    combinedTiltY += static_cast<float>(thumbLeftY) / 32768.0f;

    for (int i = 0; i < buttonCount; ++i)
        isButtonDown[userIndex][i] = (inputState.Gamepad.wButtons & buttons[i]) == buttons[i];
}
```

在 **MarbleMaze::Update** 方法輪詢以取得輸入之後，它會更新合併的輸入陣列。 合併的輸入陣列只會追蹤哪些按鈕先前未按下但現在已按下。 這樣可讓遊戲只在按鈕最初按下時才執行動作，而不是在按住按鈕時。

```cpp
bool combinedButtonPressed[buttonCount] = { false, };
for (int i = 0; i < buttonCount; ++i)
{
    for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
    {
        bool pressed = !wasButtonDown[userIndex][i] && isButtonDown[userIndex][i];
        combinedButtonPressed[i] = combinedButtonPressed[i] || pressed;
    }
}
```

在 **MarbleMaze::Update** 方法收集按鈕輸入之後，它會執行必須發生的任何動作。 例如，按下 \[開始\] 按鈕 (XINPUT\_GAMEPAD\_START) 時，遊戲狀態會從作用中變成暫停，或從暫停變成作用中。

```cpp
// Check whether the user paused or resumed the game. 
// XINPUT_GAMEPAD_START  
if (combinedButtonPressed[1] || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;
    if (m_gameState == GameState::InGameActive)
        SetGameState(GameState::InGamePaused);
    else if (m_gameState == GameState::InGamePaused)
        SetGameState(GameState::InGameActive);
}
```

如果主功能表在作用中，當方向鍵按上或下時，現用功能表項目會變更。 如果使用者選擇目前的選取項目，適當的 UI 元素會標示為已選取。

```cpp
// Handle menu navigation. 

// XINPUT_GAMEPAD_A or XINPUT_GAMEPAD_START 
bool chooseSelection = (combinedButtonPressed[0] || combinedButtonPressed[1]);

// XINPUT_GAMEPAD_DPAD_UP 
bool moveUp = combinedButtonPressed[2];

// XINPUT_GAMEPAD_DPAD_DOWN 
bool moveDown = combinedButtonPressed[3];                                           

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);

        if (m_startGameButton.GetSelected())
            m_startGameButton.SetPressed(true);

        if (m_highScoreButton.GetSelected())
            m_highScoreButton.SetPressed(true);
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
        SetGameState(GameState::MainMenu);
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
        SetGameState(GameState::HighScoreDisplay);
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

在 **MarbleMaze::Update** 方法處理控制器輸入之後，它會儲存目前的輸入狀態供下一個畫面使用。

```cpp
// Update the button state for the next frame.
memcpy(wasButtonDown, isButtonDown, sizeof(wasButtonDown));
```

### 追蹤觸控和滑鼠輸入

對於觸控和滑鼠輸入，當使用者觸碰或按一下功能表項目時，就會加以選擇。 下列範例顯示 **MarbleMaze::Update** 方法如何處理指標輸入來選取功能表項目。 
            **m\_pointQueue** 成員變數會追蹤使用者在螢幕上觸碰或按一下的位置。 本文件稍後的＜處理指標輸入＞一節會進一步說明 Marble Maze 收集指標輸入的方式。

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

### 更新遊戲狀態


            **MarbleMaze::Update** 方法在處理控制器和觸控輸入之後，如果有任何按鈕按下，便會更新遊戲狀態。

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

##  控制遊戲進行


遊戲迴圈和 **MarbleMaze::Update** 方法會共同合作來更新遊戲物件的狀態。 如果您的遊戲接受來自多個裝置的輸入，您可以將所有裝置的輸入累積在一組變數中，以便您撰寫更容易維護的程式碼。 
            **MarbleMaze::Update** 方法會定義一組變數來累積所有裝置的動作。

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

輸入機制可隨不同輸入裝置而有所不同。 例如，指標輸入是使用 Windows 執行階段事件處理模型來處理。 相反地，您在需要時會輪詢 Xbox 360 控制器的輸入資料。 我們建議您一律遵循特定裝置所規定的輸入機制。 本節說明 Marble Maze 如何讀取每個裝置的輸入、如何更新合併的輸入值，以及如何使用合併的輸入值來更新遊戲的狀態。

###  處理指標輸入

當您使用指標輸入時，請呼叫 [**Windows::UI::Core::CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208217) 方法來處理視窗事件。 在更新或呈現場景之前，請在遊戲迴圈中呼叫此方法。 Marble Maze 會將 **CoreProcessEventsOption::ProcessAllIfPresent** 傳遞給這個方法，以處理所有已佇列的事件，然後立即返回。 在處理事件之後，Marble Maze 會呈現並顯示下一個畫面。

```cpp
// Process windowing events.
CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
```

Windows 執行階段會針對產生的每個事件呼叫已登錄的處理常式。 
            **DirectXApp** 類別會登錄事件，並將指標資訊轉送至 **MarbleMaze** 類別。

```cpp
void DirectXApp::OnPointerPressed(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void DirectXApp::OnPointerReleased(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->RemoveTouch(args->CurrentPoint->PointerId);
}

void DirectXApp::OnPointerMoved(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```


            **MarbleMaze** 類別會將保留觸控事件的 map 物件更新，以反應指標事件。 在第一次按下指標時，例如當使用者在觸控裝置上最初觸碰螢幕時，會呼叫 **MarbleMaze::AddTouch** 方法。 當指標位置移動時會呼叫 **MarbleMaze::AddTouch** 方法。 當釋放指標時 (例如，當使用者停止觸碰螢幕時)，會呼叫 **MarbleMaze::RemoveTouch** 方法。

```cpp
void MarbleMaze::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_windowBounds);

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMaze::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_windowBounds);
}

void MarbleMaze::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

PointToTouch 函式會轉譯目前的指標位置，讓原點位於螢幕的中央，然後調整座標，讓它們的範圍大約介於 -1.0 到 +1.0 之間。 這樣較易於在不同輸入方法之間，以一致的方式來計算迷宮的傾斜度。

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Rect bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```


            **MarbleMaze::Update** 方法會以固定縮放值來遞增傾斜係數，以更新合併的輸入值。 此縮放值是試驗過許多不同的值之後而決定。

```cpp
// Account for touch input. 
const float touchScalingFactor = 2.0f;
for (TouchMap::const_iterator iter = m_touches.cbegin(); iter != m_touches.cend(); ++iter)
{
    combinedTiltX += iter->second.x * touchScalingFactor;
    combinedTiltY += iter->second.y * touchScalingFactor;
}
```

### 處理加速計輸入

若要處理加速計輸入，**MarbleMaze::Update** 方法會呼叫 [**Windows::Devices::Sensors::Accelerometer::GetCurrentReading**](https://msdn.microsoft.com/library/windows/apps/br225699) 方法。 這個方法會傳回代表加速計讀數的 [**Windows::Devices::Sensors::AccelerometerReading**](https://msdn.microsoft.com/library/windows/apps/br225688) 物件。 
            **Windows::Devices::Sensors::AccelerometerReading::AccelerationX** 和 **Windows::Devices::Sensors::AccelerometerReading::AccelerationY** 屬性分別保有沿著 X 軸和 Y 軸的重力加速度。

下列範例顯示 **MarbleMaze::Update** 方法如何輪詢加速計及更新合併的輸入值。 當您傾斜裝置時，重力會讓彈珠移動得更快。

```cpp
// Account for sensors. 
const float acceleromterScalingFactor = 3.5f;
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += static_cast<float>(reading->AccelerationX) * acceleromterScalingFactor;
        combinedTiltY += static_cast<float>(reading->AccelerationY) * acceleromterScalingFactor;
    }
}
```

由於您無法確定使用者電腦上是否有加速計，因此在輪詢加速計之前，請一律要確定有一個有效的 Accelerometer 物件。

### 處理 Xbox 360 控制器輸入

下列範例顯示 **MarbleMaze::Update** 方法如何從 Xbox 360 控制器讀取並更新合併的輸入值。 
            **MarbleMaze::Update** 方法使用 for 迴圈，以接收來自任何已連接控制器的輸入。 
            **XInputGetState** 方法會將控制器的目前狀態填入 XINPUT\_STATE 物件。 
            **combinedTiltX** 和 **combinedTiltY** 值會根據左搖捍的 X 值和 Y 值而更新。

```cpp
// Account for input on any connected controller.
XINPUT_STATE inputState = {0};
for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
{
    DWORD result = XInputGetState(userIndex, &inputState);
    if(result != ERROR_SUCCESS) 
        continue;

    SHORT thumbLeftX = inputState.Gamepad.sThumbLX;
    if (abs(thumbLeftX) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftX = 0;

    SHORT thumbLeftY = inputState.Gamepad.sThumbLY;
    if (abs(thumbLeftY) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftY = 0;

    combinedTiltX += static_cast<float>(thumbLeftX) / 32768.0f;
    combinedTiltY += static_cast<float>(thumbLeftY) / 32768.0f;

    for (int i = 0; i < buttonCount; ++i)
        isButtonDown[userIndex][i] = (inputState.Gamepad.wButtons & buttons[i]) == buttons[i];
}
```

XInput 定義左搖桿的 **XINPUT\_GAMEPAD\_LEFT\_THUMB\_DEADZONE** 常數。 這是適合大部分遊戲的靜止區域臨界值。

> 
            **重要** 當您使用 Xbox 360 控制器時，請總是考量靜止區域。 靜止區域是指不同遊戲台對於初始移動的敏感度差異。 在某些控制器中，細微的移動可能不會產生讀數，但在其他控制器中，卻可能產生可測出的讀數。 為了讓遊戲納入此一考量，請為初始搖捍移動建立非移動區域。 如需靜止區域的詳細資訊，請參閱 [XInput 入門](https://msdn.microsoft.com/library/windows/desktop/ee417001)。

 

###  將輸入套用至遊戲狀態

裝置會以不同的方式報告輸入值。 例如，指標輸入可能以螢幕座標表示，而控制器輸入可能以完全不同的格式表示。 將多個裝置的輸入合併為一組輸入值的挑戰在於正規化，或將值轉換成一般格式。 Marble Maze 會將值縮放至範圍 \[-1.0, 1.0\] 來加以正規化。 為了將 Xbox 360 控制器輸入正規化，Marble Maze 會將輸入值除以 32768，因為搖桿輸入值一律介於 -32768 和 32767 之間。 本節稍早所述的 **PointToTouch** 函式會將螢幕座標轉換為介於大約 -1.0 和 +1.0 之間的正規化值，以得到類似的結果。

> 
            **祕訣** 即使您的應用程式只使用一個輸入方法，仍建議您一律將輸入值正規化。 這樣做可簡化遊戲的其他元件解譯輸入的方式 (例如物理模擬)，也能更輕鬆地撰寫可在不同螢幕解析度下執行的遊戲。

 


            **MarbleMaze::Update** 方法在處理輸入之後，會建立向量來代表迷宮傾斜對彈珠的效果。 下列範例示範 Marble Maze 如何使用 **XMVector3Normalize** 函式來建立經過正規化的重力向量。 
            *MaxTilt* 變數會限制迷宮傾斜的程度，避免迷宮翻覆。

```cpp
const float maxTilt = 1.0f / 8.0f;
XMVECTOR gravity = XMVectorSet(combinedTiltX * maxTilt, combinedTiltY * maxTilt, 1.0f, 0.0f);
gravity = XMVector3Normalize(gravity);
```

為了完成場景物件更新，Marble Maze 會將更新的重力向量傳遞到物理模擬、更新前一個畫面以來所經過時間的物理模擬，以及更新彈珠的位置和方向。 如果彈珠掉落到迷宮底下，**MarbleMaze::Update** 方法會將彈珠放回彈珠上次碰觸的關卡，並重設物理模擬的狀態。

```cpp
XMFLOAT3 g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);
```

```cpp
// Only update physics when gameplay is active.
m_physics.UpdatePhysicsSimulation(timeDelta);
```

```cpp
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

本節不會說明物理模擬的運作方式。 如需詳細資訊，請參閱 Marble Maze 原始檔中的 Physics.h 和 Physics.cpp。

## 後續步驟


如需使用音訊時應該牢記的一些重要做法的相關資訊，請參閱[在 Marble Maze 範例中加入音訊](adding-audio-to-the-marble-maze-sample.md)。 該文件會討論 Marble Maze 如何使用 Microsoft 媒體基礎和 XAudio2 來載入、混合和播放音訊資源。

## 相關主題


* [在 Marble Maze 範例中加入音訊](adding-audio-to-the-marble-maze-sample.md)
* [在 Marble Maze 範例中加入視覺化內容](adding-visual-content-to-the-marble-maze-sample.md)
* [使用 C++ 和 DirectX 開發 Marble Maze (UWP 遊戲)](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Jun16_HO4-->


