---
title: 賽車方向盤與動力回饋
description: 使用 Windows.Gaming.Input 賽車方向盤 API 可偵測和判斷功能，以及讀取和傳送動力回饋命令給賽車方向盤。
ms.assetid: 6287D87F-6F2E-4B67-9E82-3D6E51CBAFF9
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10, uwp, games, racing wheel, force feedback, 遊戲, 賽車方向盤, 動力回饋
ms.localizationpriority: medium
ms.openlocfilehash: 0d92fe45a008ee0c3864355faf8133ab8e1ebefb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163062"
---
# <a name="racing-wheel-and-force-feedback"></a>競速方向盤與力回饋

本頁面說明使用 [RacingWheel][racingwheel] 的程式設計的基本概念，以及適用于通用 WINDOWS 平臺 (UWP) Xbox One 的相關 api。

閱讀此頁面，即可了解：

* 如何收集所連接之賽車方向盤和其使用者的清單
* 如何偵測已新增或移除賽車方向盤
* 如何讀取來自一個或多個賽車方向盤的輸入
* 如何傳送動力回饋命令
* 賽車方向盤如何當成 UI 瀏覽裝置使用

## <a name="racing-wheel-overview"></a>賽車方向盤概觀

賽車方向盤是與真實賽車駕駛艙感覺類似的輸入裝置。 賽車方向盤對於以汽車或卡車為主的大型機台式與模擬式賽車遊戲而言，都是完美的輸入裝置。 在 Windows 10 和 Xbox One UWP 應用程式中，賽車方向盤受到 [Windows.Gaming.Input](/uwp/api/windows.gaming.input) 命名空間所支援。

Xbox One 賽車方向盤會有多種價位，通常隨著價位越高，輸入與動力回饋功能也會更多及更好。 所有的賽車方向盤都配備類比方向盤、類比油門和煞車控制以及方向盤上控制鈕。 部分賽車方向盤也會額外配備類比離合器和手煞車控制、換檔與動力回饋功能。 並非所有的賽車方向盤都有配備相同的功能組合，對於特定功能的支援也可能各不相同；例如，方向盤支援的旋轉範圍可能不同，而換檔功能支援檔數也不同。

### <a name="device-capabilities"></a>裝置功能

不同的 Xbox One 賽車輪子提供不同組的選用裝置功能，以及這些功能的不同支援層級;單一輸入裝置之間的這種差異層級，在 [Windows](/uwp/api/windows.gaming.input) 的裝置所支援的裝置之間是唯一的。 此外，您所接觸的大多數裝置至少會支援部分選用功能或其他變化。 因此，個別判斷每一個連接之賽車方向盤的功能以及支援您的遊戲所適合之各項功能，是非常重要的事。

如需詳細資訊，請參閱[判斷賽車方向盤功能](#determining-racing-wheel-capabilities)。

### <a name="force-feedback"></a>動力回饋

部分 Xbox One 賽車方向盤可提供真正的動力回饋，也就是說，它們會在控制軸 (如方向盤)上施加實際力量，而不只是簡單的震動。 遊戲會使用這項功能創造更具身歷其境的感受 (_模擬衝撞損害_、「道路駕駛的感覺」)，同時也可增加出色駕駛的難度。

如需詳細資訊，請參閱[動力回饋概觀](#force-feedback-overview)。

### <a name="ui-navigation"></a>UI 瀏覽

為了減輕支援不同輸入裝置進行使用者介面瀏覽的負擔，以及鼓勵遊戲與裝置之間的一致性，大部分「實體」__ 輸入裝置同時會當成稱為 [UI 瀏覽控制器](ui-navigation-controller.md)的「邏輯」__ 輸入裝置使用。 UI 瀏覽控制器提供跨輸入裝置之 UI 瀏覽命令的通用詞彙。

由於它們特別著重於不同賽車方向盤之間的類比控制與變化程度，所以它們通常會配備數位方向鍵、**檢視**、**選單**、**A**、**B**、**X** 和 **Y** 按鈕，與[遊戲台](gamepad-and-vibration.md)類似；這些按鈕並非用於支援遊戲命令，所以無法直接當做賽車方向盤按鈕進行存取。

以 UI 流覽控制器而言，賽車會將 [所需的一組](ui-navigation-controller.md#required-set) 流覽命令對應到左側操縱杆、D-Pad **、View**、 **Menu**、 **a**和 **B** 按鈕。

| 導覽命令 | 賽車方向盤輸入 |
| ------------------:| ------------------ |
|                 Up | 方向鍵向上           |
|               Down | 方向鍵向下         |
|               Left | 方向鍵向左         |
|              Right | 方向鍵向右        |
|               檢視 | 檢視按鈕        |
|               功能表 | 功能表按鈕        |
|             接受 | A 按鍵           |
|             取消 | B 按鍵           |

此外，部分賽車方向盤可能會將瀏覽命令的一些[選用集](ui-navigation-controller.md#optional-set)對應到它們所支援的其他輸入，但是命令對應可能會因裝置而異。 您也可考慮支援這些命令，但是請確定這些不是瀏覽您遊戲介面的必要命令。

| 導覽命令 | 賽車方向盤輸入    |
| ------------------:| --------------------- |
|            Page Up | _不同_              |
|          Page Down | _不同_              |
|          向左翻頁 | _不同_              |
|         向右翻頁 | _不同_              |
|          向上捲動 | _不同_              |
|        向下捲動 | _不同_              |
|        向左捲動 | _不同_              |
|       向右捲動 | _不同_              |
|          內容 1 | X 按鈕 (_通常_)  |
|          內容 2 | Y 按鈕 (__ 常用) |
|          內容 3 | _不同_              |
|          內容 4 | _不同_              |

## <a name="detect-and-track-racing-wheels"></a>偵測和追蹤賽車方向盤

使用與遊戲台相同的方式偵測及追蹤賽車方向盤的運作，但 [RacingWheel](/uwp/api/windows.gaming.input.racingwheel) 類別除外，而不是 [Gamepad](/uwp/api/Windows.Gaming.Input.Gamepad) 類別。 如需詳細資訊，請參閱[遊戲台與震動](gamepad-and-vibration.md)。

<!-- Racing wheels are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected racing wheels and events to notify you when a racing wheel is added or removed.

### The racing wheels list

The [RacingWheel](/uwp/api/windows.gaming.input.racingwheel) class provides a static property, [RacingWheels](/uwp/api/windows.gaming.input.racingwheel.racingwheels#Windows_Gaming_Input_RacingWheel_RacingWheels), which is a read-only list of racing wheels that are currently connected. Because you might only be interested in some of the connected racing wheels, it's recommended that you maintain your own collection instead of accessing them through the `RacingWheels` property.

The following example copies all connected racing wheels into a new collection.
```cpp
auto myRacingWheels = ref new Vector<RacingWheel^>();

for (auto racingwheel : RacingWheel::RacingWheels)
{
    // This code assumes that you're interested in all racing wheels.
    myRacingWheels->Append(racingwheel);
}
```

### Adding and removing racing wheels

When a racing wheel is added or removed the [RacingWheelAdded](/uwp/api/windows.gaming.input.racingwheel.racingwheeladded) and [RacingWheelRemoved](/uwp/api/windows.gaming.input.racingwheel.racingwheelremoved) events are raised. You can register handlers for these events to keep track of the racing wheels that are currently connected.

The following example starts tracking an racing wheels that's been added.
```cpp
RacingWheel::RacingWheelAdded += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    // This code assumes that you're interested in all new racing wheels.
    myRacingWheels->Append(args);
}
```

The following example stops tracking a racing wheel that's been removed.
```cpp
RacingWheel::RacingWheelRemoved += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    unsigned int indexRemoved;

    if(myRacingWheels->IndexOf(args, &indexRemoved))
    {
        myRacingWheels->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each racing wheel can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-racing-wheel"></a>讀取賽車方向盤

在您找出感興趣的賽車方向盤之後，即可收集其輸入。 不過，賽車方向盤不是透過引發事件來溝通狀態變更，這與您可能習慣使用的一些其他類型的輸入不同。 相反的，您可以進行_輪詢_來定期讀取其目前狀態。

### <a name="polling-the-racing-wheel"></a>輪詢賽車方向盤

輪詢可在精確的時間點擷取賽車方向盤的快照。 這種輸入收集方式適用於大部分遊戲，因為其邏輯一般是以決定性迴圈執行，而不是透過事件驅動；透過一次所收集的輸入來解譯遊戲命令，一般也比透過不同時間所收集的許多單一輸入來解譯遊戲命令更為簡單。

呼叫 [GetCurrentReading](/uwp/api/windows.gaming.input.racingwheel.getcurrentreading#Windows_Gaming_Input_RacingWheel_GetCurrentReading) 即可輪詢賽車方向盤；此函式會傳回包含賽車方向盤狀態的 [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading)。

下列範例會輪詢賽車方向盤的目前狀態。

```cpp
auto racingwheel = myRacingWheels[0];

RacingWheelReading reading = racingwheel->GetCurrentReading();
```

除了賽車方向盤狀態之外，每次讀取都會包含精確指出狀態擷取時間的時間戳記。 時間戳記適用於與先前讀取的計時或遊戲模擬的計時建立關聯。

### <a name="determining-racing-wheel-capabilities"></a>判斷賽車方向盤功能

許多賽車方向盤控制項為選用，或甚至在必要的控制項中也會支援不同的變化，所以您必須個別判斷每個賽車方向盤的功能，然後才可以處理在每個賽車方向盤讀取中所收集的輸入。

選用的控制項為手煞車、離合器和換檔功能；您可以藉由分別讀取賽車方向盤的 [HasHandbrake](/uwp/api/windows.gaming.input.racingwheel.hashandbrake#Windows_Gaming_Input_RacingWheel_HasHandbrake)、[HasClutch](/uwp/api/windows.gaming.input.racingwheel.hasclutch#Windows_Gaming_Input_RacingWheel_HasClutch) 和 [HasPatternShifter](/uwp/api/windows.gaming.input.racingwheel.haspatternshifter#Windows_Gaming_Input_RacingWheel_HasPatternShifter) 屬性來判斷連接的賽車方向盤是否支援這些控制項。 如果屬性的值為 **true**，就表示支援該控制項，否則就是不支援。

```cpp
if (racingwheel->HasHandbrake)
{
    // the handbrake is supported
}

if (racingwheel->HasClutch)
{
    // the clutch is supported
}

if (racingwheel->HasPatternShifter)
{
    // the pattern shifter is supported
}
```

此外，可能會有變動的控制項為方向盤和換檔功能。 方向盤會因實際賽車方向盤可支援的實體旋轉程度而異，換檔功能則會因它所支援的前進檔數不同而異。 您可以讀取賽車方向盤的 `MaxWheelAngle` 屬性來判斷它實際支援的最大旋轉角度；其值代表順時鐘方向所支援的實體角度上限 (正值)，逆時鐘方向也同樣支援 (負值)。 您可以讀取賽車方向盤的 `MaxPatternShifterGear` 屬性來判斷換檔功能支援的最大前進檔；其值代表所支援的最大前進檔；如果它的值為 4，則表示換檔功能支援倒退檔、空檔、一檔、二檔、三檔和四檔。

```cpp
auto maxWheelDegrees = racingwheel->MaxWheelAngle;
auto maxShifterGears = racingwheel->MaxPatternShifterGear;
```

最後，部分賽車方向盤會透過方向盤來支援動力回饋。 您可以讀取賽車方向盤的 [WheelMotor](/uwp/api/windows.gaming.input.racingwheel.wheelmotor#Windows_Gaming_Input_RacingWheel_WheelMotor) 屬性來判斷連接的賽車方向盤是否支援動力回饋。 如果不是 null，則支援強制意見反應， `WheelMotor` 否則不支援。 **null**

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    // force feedback is supported
}
```

如需有關如何使用賽車方向盤所支援之動力回饋功能的詳細資訊，請參閱[動力回饋概觀](#force-feedback-overview)。

### <a name="reading-the-buttons"></a>讀取按鈕

每一個賽車滾輪都 &mdash; 有四個方向： D 板、 **先前的齒輪** 和 **下齒輪** 按鈕，還有另外16個按鈕則 &mdash; 會提供數位閱讀，指出是否按下 () ，或 (向上) 釋放。 基於效率考量，按鈕讀取並不是以個別布林值表示，而是封裝到以 [RacingWheelButtons](/uwp/api/windows.gaming.input.racingwheelbuttons) 列舉表示的單一位元欄位中。

> [!NOTE]
> 賽車方向盤有配備用於 UI 瀏覽的其他按鈕，例如**檢視**和**選單**按鈕。 這些按鈕不是 `RacingWheelButtons` 列舉的一部分，而且只能將賽車方向盤當成 UI 瀏覽裝置存取來進行讀取。 如需詳細資訊，請參閱 [UI 瀏覽裝置](ui-navigation-controller.md)。

按鈕值讀取自 [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) 結構的 `Buttons` 屬性。 因為此屬性是位元欄位，所以使用位元遮罩來隔離感興趣按鈕的值。 設定對應位元時，即按下 (向下) 按鈕；否則為放開 (向上)。

下列範例會判斷是否按下**下一檔**按鈕。

```cpp
if (RacingWheelButtons::NextGear == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is pressed
}
```

下列範例會判斷是否放開「下一檔」按鈕。

```cpp
if (RacingWheelButtons::None == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is released (not pressed)
}
```

您有時必須要判斷按鈕何時從按下轉換為放開或從放開轉換為按下、是否按下或放開多個按鈕，或是否以特定方式排列一組按鈕 (部分為按下，部分則否)。 如需如何偵測這些條件的相關資訊，請參閱[偵測按鈕轉換](input-practices-for-games.md#detecting-button-transitions)和[偵測複雜按鈕排列](input-practices-for-games.md#detecting-complex-button-arrangements)。

### <a name="reading-the-wheel"></a>讀取方向盤

方向盤是必要的控制項，它會提供介於 -1.0 與 +1.0 之間的類比讀數。 -1.0 的值對應到最左邊的方向盤位置；+1.0 的值對應到最右邊的方向盤位置。 方向盤的值讀取自 [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) 結構的 `Wheel` 屬性。

```cpp
float wheel = reading.Wheel;  // returns a value between -1.0 and +1.0.
```

雖然方向盤讀數會對應到實際方向盤中的不同實體旋轉角度 (取決於實體賽車方向盤所支援的旋轉範圍)，但是您通常不會想要調整方向盤讀數；支援更大旋轉角度的方向盤就會提供更高的精準度。

### <a name="reading-the-throttle-and-brake"></a>讀取油門和煞車

油門和煞車是必要的控制項，兩者皆可提供介於 0.0 (完全放開) 和 1.0 (完全按下) 之間的類比讀數 (以浮點值表示)。 油門控制項的值讀取自 [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) 結構的 `Throttle` 屬性；煞車控制項的值讀取自 `Brake` 屬性。

```cpp
float throttle = reading.Throttle;  // returns a value between 0.0 and 1.0
float brake    = reading.Brake;     // returns a value between 0.0 and 1.0
```

### <a name="reading-the-handbrake-and-clutch"></a>讀取手煞車和離合器

手煞車和離合器是選用的控制項，兩者皆可提供介於 0.0 (完全放開) 和 1.0 (完全使用) 之間的類比讀數 (以浮點值表示)。 手煞車控制項的值讀取自 [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) 結構的 `Handbrake` 屬性；離合器控制項的值讀取自 `Clutch` 屬性。

```cpp
float handbrake = 0.0;
float clutch = 0.0;

if(racingwheel->HasHandbrake)
{
    handbrake = reading.Handbrake;  // returns a value between 0.0 and 1.0
}

if(racingwheel->HasClutch)
{
    clutch = reading.Clutch;        // returns a value between 0.0 and 1.0
}
```

### <a name="reading-the-pattern-shifter"></a>讀取換檔功能

換檔功能是選用的控制項，可提供介於 -1 與 [MaxPatternShifterGear](/uwp/api/windows.gaming.input.racingwheel.maxpatternshiftergear) 之間的數位讀數 (以帶正負號的整數值表示)。 -1 或 0 的值會分別對應到 _「倒退檔」_ 和 _「空檔」_；越大的正數值會對應到越大的前進檔，最大為 **MaxPatternShifterGear**。 模式移位的值是從[RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading)結構的[PatternShifterGear](/uwp/api/windows.gaming.input.racingwheelreading.patternshiftergear)屬性讀取的。

```cpp
if (racingwheel->HasPatternShifter)
{
    gear = reading.PatternShifterGear;
}
```

> [!NOTE]
> 當支援換檔功能時，也會有必要的**上一檔**和**下一檔**按鈕，這些也會影響玩家的車子目前所使用的排檔。 當兩者都存在，可利用一個簡單策略整合這些輸入，就是在玩家為車子選擇自動變速時忽略換檔功能 (和離合器)，以及在玩家為車子選擇手動變速時 (若賽車方向盤配有換檔控制) 忽略**上一檔**和**下一檔**按鈕。 如果這不適用於您的遊戲，您可以實作其他整合策略。

## <a name="run-the-inputinterfacing-sample"></a>執行 InputInterfacing 範例

[InputInterfacingUWP 範例 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) 示範如何同時使用賽車方向盤和不同類型的輸入裝置，以及這些輸入裝置如何當成 UI 瀏覽控制器操作。

## <a name="force-feedback-overview"></a>動力回饋概觀

許多賽車方向盤都有動力回饋功能，以便提供更身歷其境及挑戰性更高的駕駛體驗。 支援動力回饋的賽車方向盤通常都會配備單一馬達，以便沿著單一軸 (方向盤旋轉的軸) 對方向盤施力。 [ForceFeedback](/uwp/api/windows.gaming.input.forcefeedback)命名空間會在 Windows 10 和 Xbox One UWP 應用程式中支援強制意見反應。

> [!NOTE]
> 動力回饋 API 能夠支援多個施力軸，但是目前沒有任何 Xbox One 賽車方向盤可支援方向盤旋轉軸以外的回饋軸。

## <a name="using-force-feedback"></a>使用動力回饋

這些章節說明針對 Xbox One 賽車方向盤編寫動力回饋程式效果的基本知識。 回饋的套用方式是透過效果，效果會先載入動力回饋裝置上，然後像音效一樣可加以啟動、暫停、恢復執行及停止；但是，您必須先判斷賽車方向盤是否支持回饋功能。

### <a name="determining-force-feedback-capabilities"></a>判斷動力回饋功能

您可以讀取賽車方向盤的 [WheelMotor](/uwp/api/windows.gaming.input.racingwheel.wheelmotor#Windows_Gaming_Input_RacingWheel_WheelMotor) 屬性來判斷連接的賽車方向盤是否支援動力回饋。 如果 `WheelMotor` 值為 **null** 就表示不支援動力回饋，否則就是支援動力回饋；接著您可以繼續判斷馬達的特定回饋功能，像是可影響的軸。

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    auto axes = racingwheel->WheelMotor->SupportedAxes;

    if(ForceFeedbackEffectAxes::X == (axes & ForceFeedbackEffectAxes::X))
    {
        // Force can be applied through the X axis
    }

    if(ForceFeedbackEffectAxes::Y == (axes & ForceFeedbackEffectAxes::Y))
    {
        // Force can be applied through the Y axis
    }

    if(ForceFeedbackEffectAxes::Z == (axes & ForceFeedbackEffectAxes::Z))
    {
        // Force can be applied through the Z axis
    }
}
```

### <a name="loading-force-feedback-effects"></a>載入動力回饋效果

動力回饋效果會載入到回饋裝置上，然後在裝置上由遊戲控制自行「播放」。 提供一些基本效果;您可以透過實 [IForceFeedbackEffect](/uwp/api/windows.gaming.input.forcefeedback.iforcefeedbackeffect) 介面的類別來建立自訂效果。

| 效果類別         | 效果描述                                                                     |
| -------------------- | -------------------------------------------------------------------------------------- |
| ConditionForceEffect | 為了回應裝置中目前的感應器來施加變動力量的效果。 |
| ConstantForceEffect  | 沿著向量施加固定力量的效果。                                  |
| PeriodicForceEffect  | 沿著向量施加由波形所定義之變動力量的效果。           |
| RampForceEffect      | 沿著向量施加線性遞增/遞減力量的效果。          |

```cpp
using FFLoadEffectResult = ForceFeedback::ForceFeedbackLoadEffectResult;

auto effect = ref new Windows.Gaming::Input::ForceFeedback::ConstantForceEffect();
auto time = TimeSpan(10000);

effect->SetParameters(Windows::Foundation::Numerics::float3(1.0f, 0.0f, 0.0f), time);

// Here, we assume 'racingwheel' is valid and supports force feedback

IAsyncOperation<FFLoadEffectResult>^ request
    = racingwheel->WheelMotor->LoadEffectAsync(effect);

auto loadEffectTask = Concurrency::create_task(request);

loadEffectTask.then([this](FFLoadEffectResult result)
{
    if (FFLoadEffectResult::Succeeded == result)
    {
        // effect successfully loaded
    }
    else
    {
        // effect failed to load
    }
}).wait();
```

### <a name="using-force-feedback-effects"></a>使用動力回饋效果

一旦載入後，所有效果都能以同步方式啟動、暫停、恢復執行和停止，只要呼叫賽車方向盤之 `WheelMotor` 屬性上的函式，或是個別針對回饋效果本身呼叫函式即可。 一般來說，您應該在遊戲開始之前將您想使用的所有效果載入到回饋裝置上，然後隨著遊戲的進度使用其各自的 `SetParameters` 函式來更新效果。

```cpp
if (ForceFeedbackEffectState::Running == effect->State)
{
    effect->Stop();
}
else
{
    effect->Start();
}
```

最後，您可以隨時在特定的賽車方向盤上以非同步方式啟用、停用或重設整個動力回饋系統。

## <a name="see-also"></a>另請參閱

* [Windows.Gaming.Input.UINavigationController](/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Windows.Gaming.Input.IGameController](/uwp/api/windows.gaming.input.igamecontroller)
* [遊戲的輸入練習](input-practices-for-games.md)

[Windows.Gaming.Input]: /uwp/api/Windows.Gaming.Input
[Windows.Gaming.Input.UINavigationController]: /uwp/api/Windows.Gaming.Input.UINavigationController
[Windows.Gaming.Input.IGameController]: /uwp/api/Windows.Gaming.Input.IGameController
[racingwheel]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheels]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheeladded]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheelremoved]: /uwp/api/Windows.Gaming.Input.RacingWheel
[haspatternshifter]: /uwp/api/Windows.Gaming.Input.RacingWheel
[hashandbrake]: /uwp/api/Windows.Gaming.Input.RacingWheel
[hasclutch]: /uwp/api/Windows.Gaming.Input.RacingWheel
[maxpatternshiftergear]: /uwp/api/Windows.Gaming.Input.RacingWheel
[maxwheelangle]: /uwp/api/Windows.Gaming.Input.RacingWheel
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.RacingWheel
[wheelmotor]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheelreading]: /uwp/api/Windows.Gaming.Input.RacingWheelReading
[racingwheelbuttons]: /uwp/api/Windows.Gaming.Input.RacingWheelButtons