---
title: 遊戲台與震動
description: 使用 Windows.Gaming.Input 遊戲台 API 來偵測與讀取震動和脈衝命令，並將其傳送給遊戲台。
ms.assetid: BB03BB8E-255F-4AE8-AC43-1E519CA860FE
ms.date: 09/06/2018
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 遊戲台, 震動
ms.localizationpriority: medium
ms.openlocfilehash: e65b22039c381bd333516bd9f98c60bbddb9621c
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2018
ms.locfileid: "8877192"
---
# <a name="gamepad-and-vibration"></a>遊戲台與震動

此頁面說明使用 [Windows.Gaming.Input.Gamepad][gamepad] 的 Xbox One 遊戲台程式設計基本知識，以及通用 Windows 平台 (UWP) 的相關 API。

閱讀此頁面，即可了解：

* 如何收集所連接遊戲台和其使用者的清單
* 如何偵測已新增或移除遊戲台
* 如何讀取來自一個或多個遊戲台的輸入
* 如何傳送震動和脈衝命令
* 遊戲台如何當成 UI 瀏覽裝置

## <a name="gamepad-overview"></a>遊戲台概觀

Xbox 無線控制器和 Xbox 無線控制器 S 系列這類遊戲台是一般用途的遊戲輸入裝置。 它們是 Xbox One 的標準輸入裝置，而且是不愛用鍵盤和滑鼠的 Windows 遊戲玩家的共通選擇。 在 Windows 10 和 Xbox UWP 應用程式中，遊戲台受到 [Windows.Gaming.Input][] 命名空間所支援。

Xbox One 遊戲台配備方向板 （或方向鍵）;**A**、 **B**、 **X**、 **Y**、**檢視**及**功能表**按鈕;左和右搖桿、 lb，以及觸發程序;和，總共有四個震動馬達。 兩個搖桿都提供 X 和 Y 軸中的雙類比讀數，同時在往內按時當成按鈕。 每個觸發程序提供代表遠提取回類比讀數。

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **Paddle** buttons on its underside. These can be used to provide redundant access to game commands that are difficult to use together (such as the right thumbstick together with any of the **A**, **B**, **X**, or **Y** buttons) or to provide dedicated access to additional commands. -->

> [!NOTE]
> `Windows.Gaming.Input.Gamepad` 也支援 Xbox 360 遊戲台，這與標準 Xbox One 遊戲台相同的控制項配置。

### <a name="vibration-and-impulse-triggers"></a>震動和脈衝發射鍵

Xbox One 遊戲台提供兩個獨立馬達來進行強烈和輕微遊戲台震動，以及兩個專用馬達來提供每個發射鍵的劇烈震動 (這個特殊功能是 Xbox One 遊戲台發射鍵稱為「脈衝發射鍵」__ 的原因)。

> [!NOTE]
> Xbox 360 遊戲台未配備_脈衝發射鍵 」_。

如需詳細資訊，請參閱[震動和脈衝發射鍵概觀](#vibration-and-impulse-triggers-overview)。

### <a name="thumbstick-deadzones"></a>搖桿靜止區域

理論上，中心位置的靜止搖桿每次產生的 X 和 Y 軸中性讀數應相同。 不過，基於搖桿的機械力和敏感度，中心位置的實際讀數僅大約是理想中性值，而且後續讀數可能會不同。 基於這個原因，您必須一律使用小型的_靜止區域_&mdash;接近理想中心位置的可忽略值範圍&mdash;以補償製造差異、 機械損耗或其他遊戲台問題。

較大的靜止區域可以輕鬆地區隔想要的輸入與不想要的輸入。

如需詳細資訊，請參閱[讀取搖桿](#reading-the-thumbsticks)。

### <a name="ui-navigation"></a>UI 瀏覽

為了減輕支援不同輸入裝置進行使用者介面瀏覽的負擔，以及鼓勵遊戲與裝置之間的一致性，大部分「實體」__ 輸入裝置同時會當成稱為 [UI 瀏覽控制器](ui-navigation-controller.md)的「邏輯」__ 輸入裝置使用。 UI 瀏覽控制器提供跨輸入裝置之 UI 瀏覽命令的通用詞彙。

當成 UI 瀏覽控制器使用時，遊戲台會將瀏覽命令 「[必要的集](ui-navigation-controller.md#required-set)對應至左搖桿、 方向鍵、**檢視**、**功能表**、 **A**和**B**按鈕。

| 瀏覽命令 | 遊戲台輸入                       |
| ------------------:| ----------------------------------- |
|                 向上 | 左搖桿向上/方向鍵向上       |
|               向下 | 左搖桿向下/方向鍵向下   |
|               向左 | 左搖桿向左/方向鍵向左   |
|              向右 | 左搖桿向右/方向鍵向右 |
|               檢視 | 檢視按鈕                         |
|               Menu | 功能表按鈕                         |
|             Accept | A 按鈕                            |
|             取消 | B 按鍵                            |

此外，遊戲台會將所有[選擇性集](ui-navigation-controller.md#optional-set)的瀏覽命令對應到其餘輸入。

| 瀏覽命令 | 遊戲台輸入          |
| ------------------:| ---------------------- |
|            上一頁 | LT鍵           |
|          下一頁 | RT 鍵          |
|          向左翻頁 | LB 鍵            |
|         向右翻頁 | RB 鍵           |
|          向上捲動 | 右搖桿向上    |
|        向下捲動 | 右搖桿向下  |
|        向左捲動 | 右搖桿向左  |
|       向右捲動 | 右搖桿向右 |
|          內容 1 | X 按鈕               |
|          內容 2 | Y 按鈕               |
|          內容 3 | 左搖桿按下  |
|          內容 4 | 右搖桿按下 |

## <a name="detect-and-track-gamepads"></a>偵測和追蹤遊戲台

遊戲台是由系統所管理，因此您不需要對其進行建立或初始化動作。 系統提供已連接遊戲台的清單，以及在新增或移除遊戲台時通知您的事件。

### <a name="the-gamepads-list"></a>遊戲台清單

[Gamepad][] 類別提供靜態屬性 [Gamepad][]，這個屬性是目前已連接遊戲台的唯讀清單。 因為您可能只感興趣一些已連接遊戲台，建議您維護您的專屬集合，而不是進行透過存取`Gamepads`屬性。

下列範例會將所有已連接的遊戲台複製到新集合。 請注意，因為在背景中的其他執行緒將會存取這個集合 （在[GamepadAdded][]和[GamepadRemoved][]事件），需要放置鎖定繞著或更新集合的任何程式碼。

```cpp
auto myGamepads = ref new Vector<Gamepad^>();
critical_section myLock{};

for (auto gamepad : Gamepad::Gamepads)
{
    // Check if the gamepad is already in myGamepads; if it isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), gamepad);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all gamepads.
        myGamepads->Append(gamepad);
    }
}
```

```cs
private readonly object myLock = new object();
private List<Gamepad> myGamepads = new List<Gamepad>();
private Gamepad mainGamepad;

private void GetGamepads()
{
    lock (myLock)
    {
        foreach (var gamepad in Gamepad.Gamepads)
        {
            // Check if the gamepad is already in myGamepads; if it isn't, add it.
            bool gamepadInList = myGamepads.Contains(gamepad);

            if (!gamepadInList)
            {
                // This code assumes that you're interested in all gamepads.
                myGamepads.Add(gamepad);
            }
        }
    }   
}
```

### <a name="adding-and-removing-gamepads"></a>新增和移除遊戲台

當新增或移除遊戲台時， [GamepadAdded][]和[GamepadRemoved][]會引發事件。 您可以註冊這些事件的處理常式來追蹤目前已連接的遊戲台。

下列範例會開始追蹤已新增的遊戲台。

```cpp
Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all new gamepads.
        myGamepads->Append(args);
    }
}
```

```cs
Gamepad.GamepadAdded += (object sender, Gamepad e) =>
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    lock (myLock)
    {
        bool gamepadInList = myGamepads.Contains(e);

        if (!gamepadInList)
        {
            myGamepads.Add(e);
        }
    }
};
```

下列範例會停止追蹤已移除遊戲台。 您也需要在處理它們正在移除; 時，您正在追蹤遊戲台會發生什麼事例如，只會追蹤輸入從一個遊戲台，這個程式碼，並只需將它設定成`nullptr`移除時。 您將需要檢查每個框架，如果您的遊戲台是作用中，並更新哪一個遊戲台的控制器連接及中斷連線時，正在收集的輸入。

```cpp
Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ myLock };

    if(myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        myGamepads->RemoveAt(indexRemoved);
    }
}
```

```cs
Gamepad.GamepadRemoved += (object sender, Gamepad e) =>
{
    lock (myLock)
    {
        int indexRemoved = myGamepads.IndexOf(e);

        if (indexRemoved > -1)
        {
            if (mainGamepad == myGamepads[indexRemoved])
            {
                mainGamepad = null;
            }

            myGamepads.RemoveAt(indexRemoved);
        }
    }
};
```

如需詳細資訊，請參閱[適用於遊戲的輸入練習](input-practices-for-games.md)。

### <a name="users-and-headsets"></a>使用者和耳機

每個遊戲台都可以與一個使用者帳戶建立關聯，以將其身分識別連結到其遊戲，並且可以連接耳機，以促進語音交談或遊戲中功能。 如需深入了解如何處理使用者和耳機，請參閱[追蹤使用者和其裝置](input-practices-for-games.md#tracking-users-and-their-devices)以及[耳機](headset.md)。

## <a name="reading-the-gamepad"></a>讀取遊戲台

在您找出感興趣的遊戲台之後，即可收集其輸入。 不過，遊戲台不是透過引發事件來溝通狀態變更，這與您可能習慣使用的一些其他類型的輸入不同。 相反的，您可以進行「輪詢」__ 來定期讀取其目前狀態。

### <a name="polling-the-gamepad"></a>輪詢遊戲台

輪詢可在精確的時間點擷取瀏覽裝置的快照。 這種輸入收集方式適用於大部分遊戲，因為其邏輯一般是以決定性迴圈執行，而不是透過事件驅動；透過一次所收集的輸入來解譯遊戲命令，一般也比透過不同時間所收集的許多單一輸入來解譯遊戲命令更為簡單。

呼叫 [GetCurrentReading][]，即可以輪詢遊戲台；此函式會傳回包含遊戲台狀態的 [GamepadReading][]。

下列範例會輪詢遊戲台的目前狀態。

```cpp
auto gamepad = myGamepads[0];

GamepadReading reading = gamepad->GetCurrentReading();
```

```cs
Gamepad gamepad = myGamepads[0];

GamepadReading reading = gamepad.GetCurrentReading();
```

除了遊戲台狀態之外，每次讀取都會包含精確指出狀態擷取時間的時間戳記。 時間戳記適用於與先前讀取的計時或遊戲模擬的計時建立關聯。

### <a name="reading-the-thumbsticks"></a>讀取搖桿

每個搖桿都提供 X 和 Y 軸中介於 -1.0 與 +1.0 之間的類比讀數。 在 X 軸中，-1.0 的值對應到最左邊的搖桿位置；+1.0 的值對應到最右邊的搖桿位置。 在 Y 軸中，-1.0 的值對應到最底端的搖桿位置；+1.0 的值對應到最頂端的搖桿位置。 在這兩個軸，值大約會是 0.0，當搖桿處於中心位置，但其精確值會不同，即使在後續讀取;在本節稍後會討論如何減少這項差異的策略。

左搖桿之 X 軸的值讀取自 [GamepadReading][] 結構的 `LeftThumbstickX` 屬性；Y 軸的值則讀取自 `LeftThumbstickY` 屬性。 右搖桿之 X 軸的值讀取自 `RightThumbstickX` 屬性；Y 軸的值則讀取自 `RightThumbstickY` 屬性。

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
float rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
float rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
double rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
double rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

讀取搖桿值時，如果搖桿靜止在中心位置，您會注意到搖桿值不會確實地產生中性讀數 0.0；相反地，每次移動搖桿並回到中心位置時，它們都會產生接近 0.0 的不同值。 若要減少這些變化，您可以實作小型「靜止區域」__，這是指接近理想中心位置的可忽略值範圍。 實作靜止區域的一種方法是判定搖桿與中心的距離，若讀數比您選擇的距離值更接近中心則加以忽略。 您可以約略計算出距離&mdash;它不是確切因為搖桿讀數基本上是極性、 不平面，值&mdash;只要使用畢式定理產生。 所產生的會是一個放射狀靜止區域。

下列範例示範使用畢式定理產生基本放射狀靜止區域。

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const float deadzoneRadius = 0.1;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const double deadzoneRadius = 0.1;
const double deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
double oppositeSquared = leftStickY * leftStickY;
double adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

每個搖桿也可以在往內按時當成按鈕使用；如需讀取此輸入的詳細資訊，請參閱[讀取按鈕](#reading-the-buttons)。

### <a name="reading-the-triggers"></a>讀取發射鍵

發射鍵是以 0.0 (完全放開) 與 1.0 (完全按下) 之間的浮點值表示。 LT 鍵的值讀取自 [GamepadReading][] 結構的 `LeftTrigger` 屬性；RT 鍵的值則讀取自 `RightTrigger` 屬性。

```cpp
float leftTrigger  = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
float rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

```cs
double leftTrigger = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
double rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

### <a name="reading-the-buttons"></a>讀取按鈕

每個遊戲台按鈕&mdash;四個方向的方向鍵、 向左和右 lb、 向左和右搖桿按下、 **A**、 **B**、 **X**、 **Y**、**檢視**及**功能表**&mdash;提供數位讀取，指出指明其按下 （向下） 或放開 （向上）。 基於效率考量，按鈕讀取並非作為個別布林值; 呈現而被壓縮成[GamepadButtons][]列舉所代表的單一位元欄位。

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **paddle** buttons on its underside. These buttons are also represented in the `GamepadButtons` enumeration and their values are read in the same way as the standard gamepad buttons. -->

按鈕值讀取自 [GamepadReading][] 結構的 `Buttons` 屬性。 因為此屬性是位元欄位，所以使用位元遮罩來隔離感興趣按鈕的值。 設定對應位元時，即按下 (向下) 按鈕；否則為放開 (向上)。

下列範例判斷是否按下 A 按鈕。

```cpp
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}
```

```cs
if (GamepadButtons.A == (reading.Buttons & GamepadButtons.A))
{
    // button A is pressed
}
```

下列範例判斷是否放開 A 按鈕。

```cpp
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released
}
```

```cs
if (GamepadButtons.None == (reading.Buttons & GamepadButtons.A))
{
    // button A is released
}
```

您有時可能會想要判斷按鈕何時從轉換為放開的按下或放開為按下、 是否按下或放開，多個按鈕，或是否以特定方式排列一組按鈕是&mdash;部分為按，某些否。 如需如何偵測所有這些條件的相關資訊，請參閱[偵測按鈕轉換](input-practices-for-games.md#detecting-button-transitions)和[偵測複雜按鈕排列](input-practices-for-games.md#detecting-complex-button-arrangements)。

## <a name="run-the-gamepad-input-sample"></a>執行遊戲台輸入範例

[GamepadUWP 範例 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadUWP) 示範如何連接到遊戲台，以及如何讀取其狀態。

## <a name="vibration-and-impulse-triggers-overview"></a>震動和脈衝發射鍵概觀

遊戲台內的震動馬達用來將觸感反應提供給使用者。 遊戲會使用這項功能來創造更強大的身歷其境感受、協助溝通狀態資訊 (例如發生損害)、提示重要物件的鄰近性，或其他創意用途。

Xbox One 遊戲台共配備四個獨立震動馬達。 其中兩個是大型位在遊戲台主體馬達;左的馬達提供粗略激烈、 大幅度的震動，右馬達則提供較溫和、 較輕微的震動。 另外兩個是小型馬達，每個發射鍵內各有一個，其提供使用者發射手指的直接劇烈大量的震動；Xbox One 遊戲台的這項獨特功能是其發射鍵稱為「脈衝發射鍵」__ 的原因。 將這些馬達組合在一起，可以產生多種不同的觸覺。

## <a name="using-vibration-and-impulse"></a>使用震動與脈衝

遊戲台震動是透過 [Gamepad][] 類別的 [Vibration][] 屬性所控制。 `Vibration` 是由四個浮點值所構成之 [GamepadVibration][] 結構的執行個體；每個值都代表一個馬達的強度。

雖然的成員`Gamepad.Vibration`屬性可以直接修改，建議您初始化個別`GamepadVibration`執行個體的值，然後複製到`Gamepad.Vibration`屬性，以同時變更實際馬達強度。

下列範例示範如何同時變更馬達強度。

```cpp
// get the first gamepad
Gamepad^ gamepad = Gamepad::Gamepads->GetAt(0);

// create an instance of GamepadVibration
GamepadVibration vibration;

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

```cs
// get the first gamepad
Gamepad gamepad = Gamepad.Gamepads[0];

// create an instance of GamepadVibration
GamepadVibration vibration = new GamepadVibration();

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

### <a name="using-the-vibration-motors"></a>使用震動馬達

左右震動馬達會採用 0.0 (無震動) 與 1.0 (最大強度的震動) 之間的浮點值。 左馬達的強度是透過 [GamepadVibration][] 結構的 `LeftMotor` 屬性所設定；右馬達的強度則是透過 `RightMotor` 屬性所設定。

下列範例設定兩個震動馬達的強度，以及啟用遊戲台震動。

```cpp
GamepadVibration vibration;
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
mainGamepad.Vibration = vibration;
```

請記住，這兩個馬達不會完全相同，因此，將這些屬性設定成相同值並不會讓兩個馬達產生相同的震動。 針對任何值，左的馬達產生更強的震動以較低的頻率比右馬達的&mdash;針對相同的值&mdash;會產生較溫和的震動，還要高的頻率。 即使是最大值，左馬達還是無法產生右馬達的高頻率，右馬達也無法產生左馬達的高力道。 然而，因為馬達與遊戲台主體緊密連接，即使馬達具有不同特性並反應不同強度的震動，玩家不會完整感覺到單一馬達的震動。 這種方式所產生的觸覺會比馬達完全相同時更多樣、更複雜。

### <a name="using-the-impulse-triggers"></a>使用脈衝發射鍵

每個脈衝發射鍵馬達都採用 0.0 (無震動) 與 1.0 (最大強度的震動) 之間的浮點值。 LT 鍵的強度是透過 [GamepadVibration][] 結構的 `LeftTrigger` 屬性所設定；RT 鍵的強度則是透過 `RightTrigger` 屬性所設定。

下列範例設定並啟用兩個脈衝發射鍵的強度。

```cpp
GamepadVibration vibration;
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
mainGamepad.Vibration = vibration;
```

與其他震動馬達不同，發射鍵內的兩個震動馬達完全相同，因此，針對相同的值，它們會在任一馬達中產生相同的震動。 不過，因為這些馬達並未完全緊密連接，所以玩家會個別地感覺到震動。 這種方式可將個別的完整觸感同時導向兩個馬達，並協助傳達更為具體的資訊，這是遊戲台主體馬達無法做到的。

## <a name="run-the-gamepad-vibration-sample"></a>執行遊戲台震動範例

[GamepadVibrationUWP 範例 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadVibrationUWP) 示範如何使用遊戲台震動馬達和脈衝發射鍵來產生各種效果。

## <a name="see-also"></a>另請參閱

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [遊戲的輸入練習](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[gamepad]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[gamepads]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepads.aspx
[gamepadadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadadded.aspx
[gamepadremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.getcurrentreading.aspx
[vibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.vibration.aspx
[gamepadreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadreading.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx
[gamepadvibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadvibration.aspx
