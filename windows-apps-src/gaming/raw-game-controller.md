---
author: eliotcowley
title: 原始遊戲控制器
description: 使用 Windows.Gaming.Input 原始項遊戲控制器 API，從幾乎任何類型的遊戲控制器讀取輸入。
ms.assetid: 2A466C16-1F51-4D8D-AD13-704B6D3C7BEC
ms.author: wdg-dev-content
ms.date: 03/08/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 輸入,原始遊戲控制器
ms.localizationpriority: medium
ms.openlocfilehash: c57db3f9604e20d0dc83d6c3cf2ced87b1f5dcc1
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6651690"
---
# <a name="raw-game-controller"></a>原始遊戲控制器

此頁面說明使用 [Windows.Gaming.Input.RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller)及通用 Windows 平台 (UWP) 相關 API 為幾乎任何類型的遊戲控制器進行程式設計的基本知識。

閱讀此頁面，即可了解：

* 如何收集所連接原始遊戲控制器和其使用者的清單
* 如何偵測已新增或移除原始遊戲控制器
* 如何取得原始遊戲控制器的功能
* 如何從原始遊戲控制器讀取輸入

## <a name="overview"></a>概觀

原始遊戲控制器是遊戲控制器的一般呈現，具有在許多不同類型的常見遊戲控制器上找到的輸入。 這些輸入作為未命名的按鈕、切換裝置和軸的簡單陣列而公開。 您可以使用原始遊戲控制器，讓客戶建立自訂輸入的對應，不論使用哪種類型的控制器。

[RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller)類別非常適用於其他輸入類別 ([ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick)，[FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick)等等) 不符合您的需求的情況&mdash;如果您預期客戶會使用多種不同類型的遊戲控制器，而想要更一般的項目，就適合使用者個類別。

## <a name="detect-and-track-raw-game-controllers"></a>偵測及追蹤原始遊戲控制器

使用與遊戲台相同的方式偵測及追蹤原始遊戲控制器的運作，但 [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) 類別除外，而不是 [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) 類別。 如需詳細資訊，請參閱[遊戲台與震動](gamepad-and-vibration.md)。

<!-- Raw game controllers are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected raw game controllers and events to notify you when a raw game controller is added or removed.

### The raw game controller list

The [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) class provides a static property, [RawGameControllers](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllers), which is a read-only list of raw game controllers that are currently connected. Because you might only be interested in some of the connected raw game controllers, we recommend that you maintain your own collection instead of accessing them through the **RawGameControllers** property.

The following example copies all connected raw game controllers into a new collection:

```cpp
auto myRawGameControllers = ref new Vector<RawGameController^>();

for (auto rawGameController : RawGameController::RawGameControllers)
{
    // This code assumes that you're interested in all raw game controllers.
    myRawGameControllers->Append(rawGameController);
}
```

### Adding and removing raw game controllers

When a raw game controller is added or removed, the [RawGameControllerAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerAdded) and [RawGameControllerRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerRemoved) events are raised. You can register handlers for these events to keep track of the raw game controllers that are currently connected.

The following example starts tracking a raw game controller that's been added:

```cpp
RawGameController::RawGameControllerAdded += 
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    // This code assumes that you're interested in all new raw game controllers.
    myRawGameControllers->Append(args);
});
```

The following example stops tracking a raw game controller that's been removed:

```cpp
RawGameController::RawGameControllerRemoved +=
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    unsigned int indexRemoved;

    if (myRawGameControllers->IndexOf(args, &indexRemoved))
    {
        myRawGameControllers->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each raw game controller can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="get-the-capabilities-of-a-raw-game-controller"></a>取得原始遊戲控制器的功能

找出您感興趣的原始遊戲控制器後，您就可以收集控制器功能的相關資訊。 您可以用[RawGameController.ButtonCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.ButtonCount)取得原始遊戲控制器按鈕數目，用[RawGameController.AxisCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.AxisCount)取得類比軸數目，以及用[RawGameController.SwitchCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.SwitchCount)取得切換裝置數目。 此外，您可以使用[RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_)取得切換裝置類型。

下列範例會取得原始遊戲控制器的輸入次數︰

```cpp
auto rawGameController = myRawGameControllers->GetAt(0);
int buttonCount = rawGameController->ButtonCount;
int axisCount = rawGameController->AxisCount;
int switchCount = rawGameController->SwitchCount;
```

下列範例會判斷每個切換裝置的類型︰

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    GameControllerSwitchKind mySwitch = rawGameController->GetSwitchKind(i);
}
```

## <a name="reading-the-raw-game-controller"></a>讀取原始遊戲控制器

在您知道原始遊戲控制器上的輸入次數之後，就可以從中收集輸入。 不過，原始遊戲控制器不是透過引發事件來溝通狀態變更，這與您可能習慣使用的一些其他類型的輸入不同。 相反的，您可以進行_輪詢_來定期讀取其目前狀態。

### <a name="polling-the-raw-game-controller"></a>輪詢原始遊戲控制器

輪詢可在精確的時間點擷取原始遊戲控制器的快照。 這種輸入收集方式適用於大部分遊戲，因為其邏輯一般是以決定性迴圈執行，而不是透過事件驅動 從一次全部收集的輸入來解譯遊戲命令，一般也比從不同時間收集的許多單一輸入來解譯遊戲命令更為簡單。

輪詢原始遊戲控制器，可透過呼叫[RawGameController.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetCurrentReading_System_Boolean___Windows_Gaming_Input_GameControllerSwitchPosition___System_Double___)。 這項功能會填入包含原始遊戲控制器狀態的按鈕、切換裝置及軸的陣列。

下列範例會輪詢原始遊戲控制器的目前狀態。

```cpp
Platform::Array<bool>^ currentButtonReading =
    ref new Platform::Array<bool>(buttonCount);

Platform::Array<GameControllerSwitchPosition>^ currentSwitchReading =
    ref new Platform::Array<GameControllerSwitchPosition>(switchCount);

Platform::Array<double>^ currentAxisReading = ref new Platform::Array<double>(axisCount);

rawGameController->GetCurrentReading(
    currentButtonReading,
    currentSwitchReading,
    currentAxisReading);
```

因為無法保證在不同的控制器類型中，各個陣列的哪個位置會有哪個輸入值，所以您必須檢查哪個輸入使用[RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_)和[RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_)方法。

**GetButtonLabel**會告知您實體按鈕上印的文字或符號，而不是按鈕的功能&mdash;因此，在您想要提示玩家哪個按鈕會執行哪個功能的情況下，會最適合用作 UI 的協助。 **GetSwitchKind**會告知您切換裝置類型（也就是其具有幾個位置），但不是名稱。

取得軸或切換裝置標籤並沒有標準方法，因此您需要自己測試這些以判斷哪個輸入是哪個。

如果您有想支援的特定控制器，您可以取得[RawGameController.HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId)和[RawGameController.HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId)，並檢查是否符合該控制器。 對於每個具有相同**HardwareProductId**和**HardwareVendorId**的控制器而言，各陣列中各輸入的位置都是相同的，所以您不用擔心在相同類型的不同控制器之間可能存在不一致的邏輯。

除了原始遊戲控制器狀態之外，每次讀取都會傳回精確指出狀態擷取時間的時間戳記。 時間戳記適用於與先前讀取的計時或遊戲模擬的計時建立關聯。

### <a name="reading-the-buttons-and-switches"></a>讀取按鈕及切換裝置

原始遊戲控制器的兩個開火按鈕都各自提供數位讀取來指出按下(向下)或放開(向上)。 按鈕讀取會在單一陣列中以個別布林值呈現。 可以使用[RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_)搭配陣列中布林值的索引找到每個按鈕的標籤。 每個值都會以[GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel)呈現。

下列範例會判斷是否按下**XboxA**按鈕。

```cpp
for (uint32_t i = 0; i < buttonCount; i++)
{
    if (currentButtonReading[i])
    {
        GameControllerButtonLabel buttonLabel = rawGameController->GetButtonLabel(i);

        if (buttonLabel == GameControllerButtonLabel::XboxA)
        {
            // XboxA is pressed.
        }
    }
}
```

您有時必須要判斷按鈕何時從按下轉換為放開或從放開轉換為按下、是否按下或放開多個按鈕，或是否以特定方式排列一組按鈕 (部分為按下，部分則否)。 如需如何偵測這些條件的相關資訊，請參閱[偵測按鈕轉換](input-practices-for-games.md#detecting-button-transitions)和[偵測複雜按鈕排列](input-practices-for-games.md#detecting-complex-button-arrangements)。

切換值會以[GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition)的陣列提供。 因為此屬性是位元欄位，所以使用位元遮罩來隔離切換裝置的方向。

下列範例會判斷各切換裝置是否在向上位置︰

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    if (GameControllerSwitchPosition::Up ==
        (currentSwitchReading[i] & GameControllerSwitchPosition::Up))
    {
        // The switch is in the up position.
    }
}
```

### <a name="reading-the-analog-inputs-sticks-triggers-throttles-and-so-on"></a>讀取類比輸入（搖桿, 觸發程序, 節流等等）

類比軸提供介於 0.0 與 1.0 之間的讀數。 這包含搖桿上的每個維度，例如標準搖桿的X 及Ｙ，或甚至飛行桿的 X、Y和 Z 軸（分別為翻滾、俯仰、偏擺）。

這些值可以代表類比觸發程序、節流閥或任何其他類型的類比輸入。 這些值未提供標籤，因此建議使用各種不同的輸入裝置來測試程式碼，以確保其與您的假設正確相符。

在所有軸中，當搖桿處於中心位置時，值大約會是 0.5，但其精確值通常會不同，即使在後續讀取時也是一樣；本節稍後會討論如何減少這項差異。

下列範例會顯示如何從Xbox控制器讀取類比值︰

```cpp
// Xbox controllers have 6 axes: 2 for each stick and one for each trigger.
float leftStickX = currentAxisReading[0];
float leftStickY = currentAxisReading[1];
float rightStickX = currentAxisReading[2];
float rightStickY = currentAxisReading[3];
float leftTrigger = currentAxisReading[4];
float rightTrigger = currentAxisReading[5];
```

讀取搖桿值時，如果搖桿靜止在中心位置，您會注意到搖桿值不會確實地產生中性讀數 0.5；相反地，每次移動搖桿並回到中心位置時，它們都會產生接近 0.5 的不同值。 若要減少這些變化，您可以實作小型「靜止區域」__，這是指接近理想中心位置的可忽略值範圍。

實作靜止區域的一種方法是判定搖桿與中心的距離，若讀數比您選擇的距離值更接近中心則予以忽略。 只要使用畢式定理，就可以約略計算出距離；因為搖桿讀數基本上是極性 (非平面) 值，所以距離不是確切值。 所產生的會是一個放射狀靜止區域。

下列範例會使用畢式定理示範基本放射狀靜止區域。

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = leftStickY * leftStickY;
float adjacentSquared = leftStickX * leftStickX;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

<!--## Run the RawGameControllerUWP sample

The [RawGameControllerUWP sample (GitHub)](TODO: Link) demonstrates how to use raw game controllers. TODO: More information-->

## <a name="see-also"></a>請參閱

* [遊戲的輸入](input-for-games.md)
* [遊戲的輸入練習](input-practices-for-games.md)
* [Windows.Gaming.Input 命名空間](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Windows.Gaming.Input.RawGameController 類別](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller)
* [Windows.Gaming.Input.IGameController 介面](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Windows.Gaming.Input.IGameControllerBatteryInfo 介面](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo)