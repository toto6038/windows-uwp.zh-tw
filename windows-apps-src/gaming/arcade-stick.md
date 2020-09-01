---
title: 機台搖桿
description: 使用 Windows.Gaming.Input 機台搖桿 API，偵測和讀取機台搖桿。
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 機台搖桿, 輸入
ms.localizationpriority: medium
ms.openlocfilehash: e9a9a2b3622169b99a1b3a0c3607329c85c5785f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159412"
---
# <a name="arcade-stick"></a>機台搖桿

此頁面說明使用 [Windows.Gaming.Input.ArcadeStick][arcadestick] 的 Xbox One 電動搖桿程式設計基本知識，以及通用 Windows 平台 (UWP) 的相關 API。

閱讀此頁面，即可了解：

* 如何收集所連接電動搖桿和其使用者的清單
* 如何偵測已新增或移除電動搖桿
* 如何讀取來自一或多個電動搖桿的輸入
* arcade 杆如何以 UI 導覽裝置的行為

## <a name="arcade-stick-overview"></a>電動搖桿概觀

電動搖桿為輸入裝置，價值在於重新產生直立式機台風格和其高精確度數位控制項。 電動搖桿是面對面格鬥和其他機台樣式遊戲的絕佳輸入裝置，並且適用於與全數位控制項搭配良好的任何遊戲。 在 Windows 10 和 Xbox One UWP 應用程式中，電動搖桿是由 [Windows.Gaming.Input][] 命名空間所支援。

Xbox One arcade 棒配備了8向數位操縱杆、六個 **動作** 按鈕 (以) 下圖中的 A1-A6 表示，以及兩個 **特殊** 按鈕 (以 S1 和 S2) 表示。它們都是不支援類比控制項或震動的數位輸入裝置。 Xbox One arcade 杆也配備了可用來支援 UI 導覽的 [ **View** ] 和 [ **Menu** ] 按鈕，但它們並不是為了支援遊戲命令，而且無法立即存取為搖桿按鈕。

![Arcade 與4方向的搖桿、6個動作按鈕 (A1-A6) ，以及2個特殊按鈕 (S1 和 S2) ](images/arcade-stick-1.png)

### <a name="ui-navigation"></a>UI 瀏覽

為了減輕支援不同輸入裝置進行使用者介面瀏覽的負擔，以及鼓勵遊戲與裝置之間的一致性，大部分「實體」__ 輸入裝置同時會當成稱為 [UI 瀏覽控制器](ui-navigation-controller.md)的不同「邏輯」__ 輸入裝置使用。 UI 瀏覽控制器提供跨輸入裝置之 UI 瀏覽命令的通用詞彙。

Arcade 會以 UI 流覽控制器的方式，將 [所需的一組](ui-navigation-controller.md#required-set) 流覽命令對應至操縱杆和 **View**、 **Menu**、 **action 1**和 **action 2** 按鈕。

| 導覽命令 | 電動搖桿輸入  |
| ------------------:| ------------------- |
|                 Up | 搖桿向上            |
|               Down | 搖桿向下          |
|               Left | 搖桿向左          |
|              Right | 搖桿向右         |
|               檢視 | 檢視按鈕         |
|               功能表 | 功能表按鈕         |
|             接受 | 動作 1 按鈕     |
|             取消 | 動作 2 按鈕     |

電動搖桿不會對應瀏覽命令的任意[選用集](ui-navigation-controller.md#optional-set)。

## <a name="detect-and-track-arcade-sticks"></a>偵測和追蹤電動搖桿

除了 [ArcadeStick][] 類別（而非 [遊戲台](/uwp/api/Windows.Gaming.Input.Gamepad) 類別）之外，偵測和追蹤 arcade 杆的運作方式與 gamepads 完全相同。 如需詳細資訊，請參閱[遊戲台與震動](gamepad-and-vibration.md)。

<!-- Arcade sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected arcades sticks and events to notify you when an arcade stick is added or removed.

### The arcade sticks list

The [ArcadeStick][] class provides a static property, [ArcadeSticks][], which is a read-only list of arcade sticks that are currently connected. Because you might only be interested in some of the connected arcade sticks, it's recommended that you maintain your own collection instead of accessing them through the `ArcadeSticks` property.

The following example copies all connected arcade sticks into a new collection. Note that because other threads in the background will be accessing this collection (in the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events), you need to place a lock around any code that reads or updates the collection.

```cpp
auto myArcadeSticks = ref new Vector<ArcadeStick^>();
critical_section myLock{};

for (auto arcadeStick : ArcadeStick::ArcadeSticks)
{
    // Check if the arcade stick is already in myArcadeSticks; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myArcadeSticks), end(myArcadeSticks), arcadeStick);

    if (it == end(myArcadeSticks))
    {
        // This code assumes that you're interested in all arcade sticks.
        myArcadeSticks->Append(arcadeStick);
    }
}
```

### Adding and removing arcade sticks

When an arcade stick is added or removed the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events are raised. You can register handlers for these events to keep track of the arcade sticks that are currently connected.

The following example starts tracking an arcade stick that's been added.

```cpp
ArcadeStick::ArcadeStickAdded += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    // Check if the just-added arcade stick is already in myArcadeSticks; if it
    // isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    // This code assumes that you're interested in all new arcade sticks.
    myArcadeSticks->Append(args);
}
```

The following example stops tracking an arcade stick that's been removed.

```cpp
ArcadeStick::ArcadeStickRemoved += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    unsigned int indexRemoved;

    if(myArcadeSticks->IndexOf(args, &indexRemoved))
    {
        myArcadeSticks->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each arcade stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-arcade-stick"></a>讀取電動搖桿

在您找出感興趣的電動搖桿之後，即可收集其輸入。 不過，電動搖桿不是透過引發事件來溝通狀態變更，這與您可能習慣使用的一些其他類型的輸入不同。 相反的，您可以進行_輪詢_來定期讀取其目前狀態。

### <a name="polling-the-arcade-stick"></a>對電動搖桿進行輪詢

輪詢可在精確的時間點擷取電動搖桿的快照。 這種輸入收集方式適用於大部分遊戲，因為其邏輯一般是以決定性迴圈執行，而不是透過事件驅動；透過一次所收集的輸入來解譯遊戲命令，一般也比透過不同時間所收集的許多單一輸入來解譯遊戲命令更為簡單。

呼叫 [GetCurrentReading][]，即可輪詢電動搖桿；此函式會傳回包含電動搖桿狀態的 [ArcadeStickReading][]。

下列範例會對電動搖桿的目前狀態進行輪詢。

```cpp
auto arcadestick = myArcadeSticks[0];

ArcadeStickReading reading = arcadestick->GetCurrentReading();
```

除了電動搖桿狀態之外，每次讀取都會包含精確指出狀態擷取時間的時間戳記。 時間戳記適用於與先前讀取的計時或遊戲模擬的計時建立關聯。

### <a name="reading-the-buttons"></a>讀取按鈕

每個 [arcade] 按鈕 &mdash; 是搖桿的四個方向、六個 [ **動作** ] 按鈕，以及兩個 **特殊** 按鈕，會 &mdash; 提供數位閱讀，指出是否已按下 () ，或 (向上) 放開。 為了提高效率，按鈕讀數不會以個別布林值表示;相反地，它們會封裝成 [ArcadeStickButtons][] 列舉所表示的單一位位。

> [!NOTE]
> Arcade 杆配備了其他按鈕，用於 UI 導覽，例如 [ **視圖** ] 和 [ **功能表** ] 按鈕。 這些按鈕不是 `ArcadeStickButtons` 列舉的一部分，而且只能將電動搖桿當成 UI 瀏覽裝置存取來進行讀取。 如需詳細資訊，請參閱 [UI 瀏覽裝置](ui-navigation-controller.md)。

按鈕值讀取自 [ArcadeStickReading][] 結構的 `Buttons` 屬性。 因為此屬性是位元欄位，所以使用位元遮罩來隔離感興趣按鈕的值。 設定對應位元時，即按下 (向下) 按鈕；否則為放開 (向上)。

下列範例會決定是否按下 [ **動作 1** ] 按鈕。

```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

下列範例會決定是否要釋放 [ **動作 1** ] 按鈕。

```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

您有時必須要判斷按鈕何時從按下轉換為放開或從放開轉換為按下、是否按下或放開多個按鈕，或是否以特定方式排列一組按鈕 (部分為按下，部分則否)。 如需如何偵測這些條件的相關資訊，請參閱[偵測按鈕轉換](input-practices-for-games.md#detecting-button-transitions)和[偵測複雜按鈕排列](input-practices-for-games.md#detecting-complex-button-arrangements)。

## <a name="run-the-inputinterfacing-sample"></a>執行 InputInterfacing 範例

[InputInterfacingUWP 範例 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) 示範如何同時使用電動搖桿和不同類型的輸入裝置，以及這些輸入裝置作為 UI 瀏覽控制器表現的方式。

## <a name="see-also"></a>另請參閱

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [遊戲的輸入練習](input-practices-for-games.md)

[Windows. 輸入]: /uwp/api/Windows.Gaming.Input
[Windows.Gaming.Input.IGameController]: /uwp/api/Windows.Gaming.Input.IGameController
[Windows.Gaming.Input.UINavigationController]: /uwp/api/Windows.Gaming.Input.UINavigationController
[arcadestick]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadesticks]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadestickadded]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadestickremoved]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadestickreading]: /uwp/api/Windows.Gaming.Input.ArcadeStickReading
[arcadestickbuttons]: /uwp/api/Windows.Gaming.Input.ArcadeStickButtons