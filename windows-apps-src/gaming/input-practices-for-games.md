---
title: 遊戲的輸入練習
description: 了解有效使用輸入裝置所需的模式與技巧。
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.date: 11/20/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 輸入
ms.localizationpriority: medium
ms.openlocfilehash: 73e0ba3e563b57c2e392809097567b7e6739c90d
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/01/2018
ms.locfileid: "8351466"
---
# <a name="input-practices-for-games"></a>遊戲的輸入練習

此頁面說明在通用 Windows 平台 (UWP) 遊戲中有效使用輸入裝置所需的模式與技巧。

閱讀此頁面，即可了解：

* 如何追蹤玩家，以及他們目前所使用的輸入與瀏覽裝置
* 如何偵測按鈕轉換 (按下轉換到放開，放開轉換到按下)
* 如何在單一測試中偵測複雜按鈕排列

## <a name="choosing-an-input-device-class"></a>選擇輸入裝置類別

您有多種不同類型的輸入 API 可以使用，例如[ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick)、[FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick)和[Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad)。 如何決定要在遊戲使用哪個 API？

您應該選擇為遊戲提供最適當輸入的 API。 例如，如果您正在進行2D平台遊戲，或許您只要使用**Gamepad**類別即可，而不必因為其他類別提供的額外功能煩惱。 這會限制遊戲只支援遊戲台，並提供一致的介面，不需要額外的程式碼即可在多個不同遊戲台運作。

相反地，為進行複雜的飛行與賽車模擬，您必須列舉所有[RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller)物件作為基準，以確認其支援熱衷玩家可能會有的任何冷門裝置，包含仍有單一玩家使用的獨立踏板或節流閥等裝置。 

在該處，您可以使用輸入類別的**FromGameController**的方法，例如[Gamepad.FromGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.fromgamecontroller)，來查看各個裝置是否有統整得更好的檢視。 例如，如果您的裝置也是**Gamepad**，您就必須調整按鈕對應 UI加以反映，並提供對應一些合理預設按鈕對應以供選擇。 (這樣做的相反是要求玩家手動設定遊樂台輸入，如果您僅使用 **RawGameController**。) 

或者，您可以看看 **RawGameController** 的廠商識別碼 (VID) 和產品識別碼 (PID) (分別使用[HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId)和[HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId))，並為熱門裝置提供建議的按鈕對應，同時透過玩家的手動對應，仍能夠與未來推出的未知裝置相容。

## <a name="keeping-track-of-connected-controllers"></a>持續追蹤連接的控制器

每個控制器類型包括連接的控制器的清單時 (例如 [Gamepad.Gamepads](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.Gamepads))，最好維護您自己的一份控制器清單。 如需詳細資訊，請參閱[遊戲台清單](gamepad-and-vibration.md#the-gamepads-list) (每個控制器類型在它自己的主題上有命名類似的區段)。

不過，當玩家拔除他們控制器，或插入到一個新的時會發生什麼？ 您需要處理這些事件，並依此更新您的清單。 如需詳細資訊，請參閱[新增和移除遊戲台](gamepad-and-vibration.md#adding-and-removing-gamepads) (依樣，每個控制器類型在它自己的主題上有命名類似的區段)。

因為新增和移除事件是非同步引發，所以您在處理您的控制器清單時會取得不正確的結果。 因此，每當您存取您的控制器清單時，您應該將其鎖定，以便一次只有一個執行緒可以存取它。 這可以使用[並行執行階段](https://docs.microsoft.com/cpp/parallel/concrt/concurrency-runtime)做到，尤其是 **&lt;ppl.h&gt;** 中的[critical_section 類別](https://docs.microsoft.com/cpp/parallel/concrt/reference/critical-section-class)。

要思考的另一件事是一開始連接的控制器的清單將會是空白，需要一兩秒時間填入。 所以，如果您在 start 方法中只指派目前的遊戲台，其將會是 **null**！

若要修正這個問題，您應該具有「重新整理」主要遊戲台的方法 (在單一玩家遊戲中；多人遊戲會需要更複雜的解決方案)。 然後，您應該在新增地控制器與移除的控制器事件處理常式中，或在您的 update 方法中呼叫此方法。

下列方法只會傳回清單中的第一個遊戲台 (或者若清單空白，則為 **nullptr**)。 然後，您只需要在每次您使用控制器執行任何動作時記得檢查 **nullptr**。 您是否要在沒有連接控制器 (例如，暫停遊戲) 時封鎖遊戲台，或只在略過輸入時讓遊戲繼續進行，一切操之在您。

```cpp
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Gaming::Input;
using namespace concurrency;

Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();

Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}
```

總而言之，以下是如何處理遊樂台的輸入的範例：

```cpp
#include <algorithm>
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Foundation;
using namespace Windows::Gaming::Input;
using namespace concurrency;

static Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();
static Gamepad^          m_gamepad = nullptr;
static critical_section  m_lock{};

void Start()
{
    // Register for gamepad added and removed events.
    Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(&OnGamepadAdded);
    Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(&OnGamepadRemoved);

    // Add connected gamepads to m_myGamepads.
    for (auto gamepad : Gamepad::Gamepads)
    {
        OnGamepadAdded(nullptr, gamepad);
    }
}

void Update()
{
    // Update the current gamepad if necessary.
    if (m_gamepad == nullptr)
    {
        auto gamepad = GetFirstGamepad();

        if (m_gamepad != gamepad)
        {
            m_gamepad = gamepad;
        }
    }

    if (m_gamepad != nullptr)
    {
        // Gather gamepad reading.
    }
}

// Get the first gamepad in the list.
Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}

void OnGamepadAdded(Platform::Object^ sender, Gamepad^ args)
{
    // Check if the just-added gamepad is already in m_myGamepads; if it isn't, 
    // add it.
    critical_section::scoped_lock lock{ m_lock };
    auto it = std::find(begin(m_myGamepads), end(m_myGamepads), args);

    if (it == end(m_myGamepads))
    {
        m_myGamepads->Append(args);
    }
}

void OnGamepadRemoved(Platform::Object^ sender, Gamepad^ args)
{
    // Remove the gamepad that was just disconnected from m_myGamepads.
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ m_lock };

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == m_myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        m_myGamepads->RemoveAt(indexRemoved);
    }
}
```

## <a name="tracking-users-and-their-devices"></a>追蹤使用者及其裝置

所有的輸入裝置皆與[使用者](https://docs.microsoft.com/uwp/api/windows.system.user)相關聯，如此他們的身分識別便可連結到其遊戲、成就、設定變更和其他活動。 使用者可以隨意登入或登出，而且通常不同的使用者可在前一名使用者登出後，從與系統連線的輸入裝置登入。當使用者登入或登出時，會引發 [IGameController.UserChanged](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.UserChanged) 事件。 您可以登錄此事件的事件處理常式來追蹤玩家，以及其正在使用的裝置。

使用者識別也是輸入裝置與其對應 [UI 瀏覽控制器](ui-navigation-controller.md)建立關聯的方式。

基於這些原因，應使用裝置類別的 [User](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.User) 屬性 (繼承自 [IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller) 介面) 來追蹤與關聯玩家輸入。

[UserGamepadPairingUWP (GitHub)](
https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/UserGamepadPairingUWP)  範例示範如何持續追蹤使用者及其正在使用的裝置。

## <a name="detecting-button-transitions"></a>偵測按鈕轉換

您有時可能需要了解何時第一次按下或放開按鈕，更精確地說就是按鈕狀態轉換何時從放開轉換到按下，或從按下轉換到放開。 若要判斷此事，請記住之前的裝置讀數，並且和目前的讀數比較，以了解其間的變動狀況。

以下範例示範記住之前讀數的基本方法；這裡顯示的是遊戲台，但機台搖桿、賽車方向盤和其他輸入裝置類型所適用的原則都相同

```cpp
Gamepad gamepad;
GamepadReading newReading();
GamepadReading oldReading();

// Called at the start of the game.
void Game::Start()
{
    gamepad = Gamepad::Gamepads[0];
}

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.GetCurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased (see below)
}
```

進行其他動作之前，`Game::Loop` 將現有的 `newReading` 值 (之前迴圈反覆運算的遊戲台讀數) 移至 `oldReading`，然後使用目前反覆運算的遊戲台讀數填寫 `newReading`。 這提供您偵測按鈕轉換所需的資訊。

下列範例示範偵測按鈕轉換的基本方法。

```cpp
bool ButtonJustPressed(const GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased =
        (GamepadButtons.None == (newReading.Buttons & selection));

    bool oldSelectionReleased =
        (GamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

這兩個功能會先從 `newReading` 及 `oldReading` 的按鈕選擇衍生布林值狀態，再執行布林邏輯以判斷是否已發生目標轉換。 如果新的讀數包含目標狀態 (分別為按下或放開) *而且*舊讀數也未包含目標狀態時，這些功能僅傳回 **true**，否則會傳回 **false**。

## <a name="detecting-complex-button-arrangements"></a>偵測複雜按鈕排列

輸入裝置的每個按鈕都會提供讀取數值，指明其按下 (向下) 或放開 (向上) 狀態。 基於效率考量，按鈕讀數並不是以個別布林值表示，而是封裝到以 [GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons) 之類的裝置特定列舉表示的位元欄位中。 若要讀取特定按鈕，可使用位元遮罩來隔離您感興趣的值。 設定對應位元時，即按下 (向下) 按鈕；否則為放開 (向上)。

回想如何判斷按下或放開單一按鈕；這裡顯示的是遊戲台，但機台搖桿、賽車方向盤和其他輸入裝置類型所適用的原則都相同。

```cpp
GamepadReading reading = gamepad.GetCurrentReading();

// Determines whether gamepad button A is pressed.
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // The A button is pressed.
}

// Determines whether gamepad button A is released.
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // The A button is released (not pressed).
}
```

如您所見，判斷單一按鈕何的狀態是直截了當的，但是您有時可能會想要判斷是否按下或放開多個按鈕，或是否以特定方式排列一組按鈕 (部分為按下，部分則否)。 測試多個按鈕比測試單一按鈕更複雜 (尤其是在可能混合按鈕的狀態) 但是這些測試有一個簡單的公式，單一按鈕或多個按鈕測試皆適用。

下列範例會判斷是否同時按下遊戲台按鈕 A 和 B。

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both pressed.
}
```

下列範例會判斷是否同時放開遊戲台按鈕 A 和 B。

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both released (not pressed).
}
```

下列範例會判斷是否在按下遊戲台按鈕 A 的同時放開按鈕 B。

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

這五個範例共有的公式是，要進行測試的按鈕排列由等號比較運算子左側的運算式指定，而右側的遮罩運算式則選擇要考慮的按鈕。

下列範例藉由重寫之前的範例，更清楚地示範此公式。

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

可套用這個公式以其狀態的任何排列來測試任意數目的按鈕。

## <a name="get-the-state-of-the-battery"></a>取得電池的狀態

對於實作 [IGameControllerBatteryInfo](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo) 介面的任何遊戲控制器，您可以呼叫控制器執行個體上的 [TryGetBatteryReport](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo.TryGetBatteryReport)，取得提供有關控制器中電池的相關資訊的 [BatteryReport](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport) 物件。 您可以取得屬性，例如電池充電速率 ([ChargeRateInMilliwatts](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.ChargeRateInMilliwatts))、新電池的預估容量 ([DesignCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.DesignCapacityInMilliwattHours))，以及目前電池完全充電的容量 ([FullChargeCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.FullChargeCapacityInMilliwattHours))。

對於支援詳細的電池報告的遊戲控制器，您可以取得此報告以及詳細的電池相關資訊，如[取得電池資訊](../devices-sensors/get-battery-info.md)中所詳述。 不過，大部分遊戲控制器不支援該層級的電池報告] 下方，並且反而使用低成本的硬體。 對於這些控制器，請牢記下列事項：

* **ChargeRateInMilliwatts** 與 **DesignCapacityInMilliwattHours** 將一律為 **NULL**。

* 您可以藉由計算  [RemainingCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.RemainingCapacityInMilliwattHours) / **FullChargeCapacityInMilliwattHours** 來取得電池百分比。 您應該會忽略這些屬性的值，並只處理已計算的百分比。

* 前一個項目符號的百分比一定會是下列其中一項：

    * 100% (完全)
    * 70% (中等)
    * 40% (低)
    * 10% (嚴重)

如果您的程式碼根據電池剩餘容量的百分比執行某個動作 (例如繪製 UI)，請確認它符合上述值。 例如，如果您想要在電池電力不足時警告玩家，則在到達 10% 時發出警告。

## <a name="see-also"></a>另請參閱

* [Windows.System.User 類別](https://docs.microsoft.com/uwp/api/windows.system.user)
* [Windows.Gaming.Input.IGameController 介面](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Windows.Gaming.Input.GamepadButtons 列舉](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons)
