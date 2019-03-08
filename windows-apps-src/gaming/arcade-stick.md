---
title: 機台搖桿
description: 使用 Windows.Gaming.Input 電動搖桿 API，偵測和讀取電動搖桿。
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 機台搖桿, 輸入
ms.localizationpriority: medium
ms.openlocfilehash: 6f9e3ff29dfb17b6e2a07df52153013b5266206e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593143"
---
# <a name="arcade-stick"></a>機台搖桿

此頁面說明使用 [Windows.Gaming.Input.ArcadeStick][arcadestick] 的 Xbox One 電動搖桿程式設計基本知識，以及通用 Windows 平台 (UWP) 的相關 API。

閱讀此頁面，即可了解：

* 如何收集所連接電動搖桿和其使用者的清單
* 如何偵測已新增或移除電動搖桿
* 如何讀取來自一或多個電動搖桿的輸入
* arcade 隨身碟為 UI 瀏覽裝置的行為

## <a name="arcade-stick-overview"></a>電動搖桿概觀

電動搖桿為輸入裝置，價值在於重新產生直立式機台風格和其高精確度數位控制項。 電動搖桿是面對面格鬥和其他機台樣式遊戲的絕佳輸入裝置，並且適用於與全數位控制項搭配良好的任何遊戲。 在 Windows 10 和 Xbox One UWP 應用程式中，電動搖桿是由 [Windows.Gaming.Input][] 命名空間所支援。

Xbox One arcade 隨身碟配有 8 向處理數位搖桿六**動作**按鈕 （A1 A6 以在下圖表示） 和兩個**特殊**按鈕 （表示為 S1 和 S2）; 它們所有數位輸入的裝置不支援類比控制項或震動。 Xbox One arcade 隨身碟也配備**檢視**並**功能表**按鈕用來支援 UI 導覽，但它們不為了支援遊戲命令，而且無法輕易地存取為搖桿按鈕.

![Arcade 隨身碟 4 方向性搖桿，具有 6 個動作按鈕 (A1-A6) 和 2 個特殊按鈕 （S1 和 S2）](images/arcade-stick-1.png)

### <a name="ui-navigation"></a>UI 瀏覽

為了減輕支援不同輸入裝置進行使用者介面瀏覽的負擔，以及鼓勵遊戲與裝置之間的一致性，大部分「實體」輸入裝置同時會當成稱為 [UI 瀏覽控制器](ui-navigation-controller.md)的不同「邏輯」輸入裝置使用。 UI 瀏覽控制器提供跨輸入裝置之 UI 瀏覽命令的通用詞彙。

為 UI 導覽控制站，arcade 隨身碟地圖[必要組](ui-navigation-controller.md#required-set)瀏覽命令，以搖桿並**檢視**，**功能表**，**動作 1**，並**動作 2**按鈕。

| 瀏覽命令 | 機台搖桿輸入  |
| ------------------:| ------------------- |
|                 Up | 搖桿向上            |
|               Down | 搖桿向下          |
|               Left | 搖桿向左          |
|              Right | 搖桿向右         |
|               View | 檢視按鈕         |
|               Menu | 功能表按鈕         |
|             Accept | 動作 1 按鈕     |
|             取消 | 動作 2 按鈕     |

電動搖桿不會對應瀏覽命令的任意[選用集](ui-navigation-controller.md#optional-set)。

## <a name="detect-and-track-arcade-sticks"></a>偵測和追蹤電動搖桿

偵測和追蹤 arcade 隨身碟的運作方式完全相同的方式如同對遊戲台，除了具有[ArcadeStick][]而不是類別[遊戲台](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad)類別。 如需詳細資訊，請參閱[遊戲台與震動](gamepad-and-vibration.md)。

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

在您找出感興趣的電動搖桿之後，即可收集其輸入。 不過，電動搖桿不是透過引發事件來溝通狀態變更，這與您可能習慣使用的一些其他類型的輸入不同。 相反地，您可以進行「輪詢」來定期讀取其目前狀態。

### <a name="polling-the-arcade-stick"></a>對電動搖桿進行輪詢

輪詢可在精確的時間點擷取電動搖桿的快照。 這種輸入收集方式適用於大部分遊戲，因為其邏輯一般是以決定性迴圈執行，而不是透過事件驅動；透過一次所收集的輸入來解譯遊戲命令，一般也比透過不同時間所收集的許多單一輸入來解譯遊戲命令更為簡單。

呼叫 [GetCurrentReading][]，即可輪詢電動搖桿；此函式會傳回包含電動搖桿狀態的 [ArcadeStickReading][]。

下列範例會對電動搖桿的目前狀態進行輪詢。

```cpp
auto arcadestick = myArcadeSticks[0];

ArcadeStickReading reading = arcadestick->GetCurrentReading();
```

除了電動搖桿狀態之外，每次讀取都會包含精確指出狀態擷取時間的時間戳記。 此時間戳記適用於關聯到先前讀取的計時或遊戲模擬的計時。

### <a name="reading-the-buttons"></a>讀取按鈕

每個 arcade 隨身碟按鈕&mdash;痼蹗搖桿，六個**動作**按鈕和兩個**特殊**按鈕&mdash;提供數位閱讀的指出是否已按下 （向下） 或發行 （上）。 為了提高效率，按鈕的讀數不表示為個別的布林值;相反地，它們所有封裝到單一的位元欄位，由[ArcadeStickButtons][]列舉型別。

> [!NOTE]
> Arcade 隨身碟配備其他按鈕，例如用於 UI 導覽**檢視**並**功能表**按鈕。 這些按鈕不是 `ArcadeStickButtons` 列舉的一部分，而且只能將電動搖桿當成 UI 瀏覽裝置存取來進行讀取。 如需詳細資訊，請參閱 [UI 瀏覽裝置](ui-navigation-controller.md)。

按鈕值讀取自 [ArcadeStickReading][] 結構的 `Buttons` 屬性。 由於此屬性是位元欄位，所以使用位元遮罩來隔離您感興趣的按鈕值。 設定對應位元時，即按下 (向下) 按鈕；否則為放開 (向上)。

下列範例會判斷是否**動作 1**按下按鈕時。

```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

下列範例會判斷是否**動作 1**放開按鍵時。

```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

您有時必須要判斷按鈕何時從按下轉換為放開或從放開轉換為按下、是否按下或放開多個按鈕，或是否以特定方式排列一組按鈕 (部分為按下，部分則否)。 如需如何偵測這些條件的相關資訊，請參閱[偵測按鈕轉換](input-practices-for-games.md#detecting-button-transitions)和[偵測複雜按鈕排列](input-practices-for-games.md#detecting-complex-button-arrangements)。

## <a name="run-the-inputinterfacing-sample"></a>執行 InputInterfacing 範例

[InputInterfacingUWP 範例 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) 示範如何同時使用電動搖桿和不同類型的輸入裝置，以及這些輸入裝置作為 UI 瀏覽控制器表現的方式。

## <a name="see-also"></a>請參閱

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [輸入適用於遊戲的作法](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[arcadestick]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.aspx
[arcadesticks]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadesticks.aspx
[arcadestickadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickadded.aspx
[arcadestickremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.getcurrentreading.aspx
[arcadestickreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickreading.aspx
[arcadestickbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickbuttons.aspx
