---
author: eliotcowley
title: 飛行桿
description: 使用 Windows.Gaming.Input 飛行桿API從飛行桿中讀取輸入。
ms.assetid: DC633F6B-FDC9-4D6E-8401-305861F31192
ms.author: wdg-dev-content
ms.date: 03/06/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 輸入, 飛行桿
ms.localizationpriority: medium
ms.openlocfilehash: ebe7695b3f16271f3adedae658c0d62d38d7c078
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995401"
---
# <a name="flight-stick"></a>飛行桿

此頁面說明使用 [Windows.Gaming.Input.ArcadeStickarcadestick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) 的 Xbox One 飛行桿程式認證基本知識，以及通用 Windows 平台 (UWP) 的相關 API。

閱讀此頁面，即可了解：

* 如何收集所連接飛行桿及其使用者的清單
* 如何偵測已新增或移除飛行桿
* 如何讀取來自一或多個飛行桿的輸入
* 飛行搖桿如何當成 UI 瀏覽裝置使用

## <a name="overview"></a>概觀

飛行桿是遊戲輸入裝置，價值在於重現飛機或太空船駕駛艙中的飛行桿體驗。 它們是快速且準確控制飛行的完美輸入裝置。 在 Windows 10 和 Xbox One UWP 應用程式中，飛行桿是由 [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) 命名空間所支援。

Xbox One 飛行桿配備下列控制項︰

* 可翻滾、俯仰及偏擺的可彎式類比搖桿
* 一個類比節流閥
* 兩個開火按鈕
* 一個數位控制帽切換裝置
* **檢視**和**功能表**按鈕

> [!NOTE]
> **檢視**和**功能表**按鈕用來支援 UI 瀏覽，而非遊戲命令，因此無法直接存取為搖桿按鈕。

### <a name="ui-navigation"></a>UI 瀏覽

為了減輕支援不同輸入裝置進行使用者介面瀏覽的負擔，以及鼓勵遊戲與裝置之間的一致性，大部分「實體」__ 輸入裝置同時會當成稱為 [UI 瀏覽控制器](ui-navigation-controller.md)的「邏輯」__ 輸入裝置使用。 UI 瀏覽控制器提供跨輸入裝置之 UI 瀏覽命令的通用詞彙。

飛行桿是UI 瀏覽控制器，會將瀏覽命令的[必要設定](ui-navigation-controller.md#required-set)對應到搖桿及**View**，**Menu**，**FirePrimary**，和**FireSecondary**按鈕。

| 瀏覽命令 | 飛行桿輸入                  |
| ------------------:| ----------------------------------- |
|                 Up | 搖桿向上                         |
|               Down | 搖桿向下                       |
|               Left | 搖桿向左                       |
|              Right | 搖桿向右                      |
|               View | **View**按鈕                     |
|               Menu | **Menu**按鈕                     |
|             Accept | **FirePrimary** 按鈕              |
|             Cancel | **FireSecondary** 按鈕            |

飛行桿不會對應瀏覽命令的任何[選用設定](ui-navigation-controller.md#optional-set)。

## <a name="detect-and-track-flight-sticks"></a>偵測和追蹤飛行桿

使用與遊戲台相同的方式偵測及追蹤飛行桿的運作，但 [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) 類別除外，而不是 [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) 類別。 如需詳細資訊，請參閱[遊戲台與震動](gamepad-and-vibration.md)。

<!-- Flight sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected flight sticks and events to notify you when a flight stick is added or removed.

### The flight stick list

The [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) class provides a static property, [FlightSticks](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightSticks), which is a read-only list of flight sticks that are currently connected. Because you might only be interested in some of the connected flight sticks, we recommend that you maintain your own collection instead of accessing them through the `FlightSticks` property.

The following example copies all connected flight sticks into a new collection:

```cpp
auto myFlightSticks = ref new Vector<FlightStick^>();

for (auto flightStick : FlightStick::FlightSticks)
{
    // This code assumes that you're interested in all flight sticks.
    myFlightSticks->Append(flightStick);
}
```

### Adding and removing flight sticks

When a flight stick is added or removed, the [FlightStickAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickAdded) and [FlightStickRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickRemoved) events are raised. You can register handlers for these events to keep track of the flight sticks that are currently connected.

The following example starts tracking a flight stick that's been added:

```cpp
FlightStick::FlightStickAdded += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    // This code assumes that you're interested in all new flight sticks.
    myFlightSticks->Append(args);
});
```

The following example stops tracking a flight stick that's been removed:

```cpp
FlightStick::FlightStickRemoved += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    unsigned int indexRemoved;

    if (myFlightSticks->IndexOf(args, &indexRemoved))
    {
        myFlightSticks->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each flight stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-flight-stick"></a>讀取飛行桿

在您找出感興趣的飛行桿之後，即可收集其輸入。 不過，飛行桿不是透過引發事件來溝通狀態變更，這與您可能習慣使用的一些其他類型的輸入不同。 相反的，您可以進行_輪詢_來定期讀取其目前狀態。

### <a name="polling-the-flight-stick"></a>輪詢飛行桿

輪詢可在精確的時間點擷取飛行桿的快照。 這種輸入收集方式適用於大部分遊戲，因為其邏輯一般是以決定性迴圈執行，而不是透過事件驅動 從一次全部收集的輸入來解譯遊戲命令，一般也比從不同時間收集的許多單一輸入來解譯遊戲命令更為簡單。

您可輪詢飛行桿，透過呼叫[FlightStick.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick.GetCurrentReading)。 這項功能會傳回[FlightStickReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading)包含飛行桿的狀態。

下列範例會對飛行桿的目前狀態進行輪詢。

```cpp
auto flightStick = myFlightSticks->GetAt(0);
FlightStickReading reading = flightStick->GetCurrentReading();
```

除了飛行桿狀態之外，每次讀取都會包含精確指出狀態擷取時間的時間戳記。 時間戳記適用於與先前讀取的計時或遊戲模擬的計時建立關聯。

### <a name="reading-the-joystick-and-throttle-input"></a>讀取搖桿和節流閥輸入

搖桿提供 X 和 Y 軸中介於 -1.0 與 1.0 之間的類比讀數（分別是翻滾、俯仰、偏擺）。 若是翻滾，-1.0 的值對應到最左邊的搖桿位置；1.0 的值對應到最右邊的搖桿位置。 若是俯仰，-1.0 的值對應到最底端的搖桿位置；1.0 的值對應到最頂端的搖桿位置。 若是偏擺，-1.0 值對應最逆時鐘、彎曲的位置，而 1.0 對應最順時針方向的位置。

在所有軸中，搖桿處於中心位置時，值大約會是 0.0，但其精確值通常會不同，即使在後續讀取時也是一樣 本節稍後會討論如何減少這項差異。

搖桿的值讀取自[FlightStickReading.Roll](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Roll)屬性、繞 X 軸旋轉的值讀取自[FlightStickReading.Pitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Pitch)屬性和繞 Y 軸旋轉的值讀取自[FlightStickReading.Yaw](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Yaw)屬性︰

```cpp
// Each variable will contain a value between -1.0 and 1.0.
float roll = reading.Roll;
float pitch = reading.Pitch;
float yaw = reading.Yaw;
```

讀取搖桿值時，如果搖桿靜止在中心位置，您會注意到搖桿值不會確實地產生中性讀數 0.0；相反地，每次移動搖桿並回到中心位置時，它們都會產生接近 0.0 的不同值。 若要減少這些變化，您可以實作小型「靜止區域」__，這是指接近理想中心位置的可忽略值範圍。

實作靜止區域的一種方法是判定搖桿與中心的距離，若讀數比您選擇的距離值更接近中心則予以忽略。 只要使用畢式定理，就可以約略計算出距離；因為搖桿讀數基本上是極性 (非平面) 值，所以距離不是確切值。 所產生的會是一個放射狀靜止區域。

下列範例示範使用畢式定理產生基本放射狀靜止區域：

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = pitch * pitch;
float adjacentSquared = roll * roll;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

### <a name="reading-the-buttons-and-hat-switch"></a>讀取按鈕與控制帽切換裝置

飛行桿的兩個開火按鈕都各自提供數位讀取來指出按下(向下)或放開(向上)。 為了更有效率，按鈕讀取並非作為個別布林值呈現，而是壓縮成由 [FlightStickButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickbuttons) 列舉所呈現的單一位元欄位。 此外，控制帽切換裝置提供壓縮成由[GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition)列舉所呈現的單一位元欄位。

> [!NOTE]
> 飛行桿配備用於 UI 瀏覽的其他按鈕，例如 **View**和 **Menu**按鈕。 這些按鈕不是 `FlightStickButtons` 列舉的一部分，而且只能將飛行桿當成 UI 瀏覽裝置存取來進行讀取。 如需詳細資訊，請參閱 [UI 瀏覽控制器](ui-navigation-controller.md)。

按鈕值讀取自[FlightStickReading.Buttons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Buttons)屬性。 因為此屬性是位元欄位，所以使用位元遮罩來隔離感興趣按鈕的值。 設定對應位元時，即按下 (向下) 按鈕；否則為放開 (向上)。

下列範例會判斷是否按下**FirePrimary**按鈕。

```cpp
if (FlightStickButtons::FirePrimary == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is pressed.
}
```

下列範例會判斷是否放開**FirePrimary**按鈕。

```cpp
if (FlightStickButtons::None == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is released (not pressed).
}
```

您有時可能要判斷按鈕何時從按下轉換為放開或從放開轉換為按下、是否按下或放開多個按鈕，或是否以特定方式排列一組按鈕 (部分為按下，部分則否)。 如需如何偵測所有這些條件的相關資訊，請參閱[偵測按鈕轉換](input-practices-for-games.md#detecting-button-transitions)和[偵測複雜按鈕排列](input-practices-for-games.md#detecting-complex-button-arrangements)。

控制帽切換裝置的值讀取自[FlightStickReading.HatSwitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.HatSwitch)屬性。 因為此屬性也是位元欄位，所以再一次使用位元遮罩來找出控制帽切換裝置的位置。

下列範例判斷控制帽切換裝置是否維持在上方位置︰

```cpp
if (GameControllerSwitchPosition::Up == (reading.HatSwitch & GameControllerSwitchPosition::Up))
{
    // The hat switch is in the up position.
}
```

下列範例判斷控制帽切換裝置是否維持在中心位置︰

```cpp
if (GameControllerSwitchPosition::Center == (reading.HatSwitch & GameControllerSwitchPosition::Center))
{
    // The hat switch is in the center position.
}
```

<!--## Run the InputInterfacingUWP sample

The [InputInterfacingUWP sample _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstrates how to use flight sticks and different kinds of input devices in tandem, as well as how these input devices behave as UI navigation controllers.-->

## <a name="see-also"></a>另請參閱

* [Windows.Gaming.Input.UINavigationController 類別](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Windows.Gaming.Input.IGameController 介面](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [遊戲的輸入練習](input-practices-for-games.md)