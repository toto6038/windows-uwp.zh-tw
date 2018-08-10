---
author: mithom
title: 電動搖桿
description: 使用 Windows.Gaming.Input 機台搖桿 API，偵測和讀取機台搖桿。
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, 遊戲, 機台搖桿, 輸入
ms.openlocfilehash: b0411dcf1fd75ec7dc31d29a39e95f5c26073953
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.locfileid: "209217"
---
# <a name="arcade-stick"></a>機台搖桿

此頁面說明使用 [Windows.Gaming.Input.ArcadeStick][arcadestick] 的 Xbox One 電動搖桿程式設計基本知識，以及通用 Windows 平台 (UWP) 的相關 API。

閱讀此頁面，即可了解：
* 如何收集所連接電動搖桿和其使用者的清單
* 如何偵測已新增或移除電動搖桿
* 如何讀取來自一或多個電動搖桿的輸入
* 電動搖桿如何當成瀏覽裝置使用


## <a name="arcade-stick-overview"></a>電動搖桿概觀

電動搖桿為輸入裝置，價值在於重新產生直立式機台風格和其高精確度數位控制項。 電動搖桿是面對面格鬥和其他機台樣式遊戲的絕佳輸入裝置，並且適用於與全數位控制項搭配良好的任何遊戲。 在 Windows 10 和 Xbox One UWP 應用程式中，電動搖桿是由 [Windows.Gaming.Input][] 命名空間所支援。

Xbox One 電動搖桿配備 8 向數位搖桿、六個**動作**按鈕，以及兩個**特殊**按鈕；其為全數位輸入裝置，且不支援類比控制項或震動。 Xbox One 電動搖桿也配備用來支援 UI 瀏覽的 **\[檢視\]** 和 **\[功能表\]** 按鈕，但不支援遊戲命令，也無法以搖桿按鈕輕鬆地進行存取。

### <a name="ui-navigation"></a>UI 瀏覽

為了減輕支援不同輸入裝置進行使用者介面瀏覽的負擔，以及鼓勵遊戲與裝置之間的一致性，大部分「實體」__ 輸入裝置同時會當成稱為 [UI 瀏覽控制器](ui-navigation-controller.md)的不同「邏輯」__ 輸入裝置使用。 UI 瀏覽控制器提供跨輸入裝置之 UI 瀏覽命令的通用詞彙。

當成 UI 瀏覽控制器使用時，電動搖桿會將瀏覽命令的[必要集](ui-navigation-controller.md#required-set)對應至搖桿以及 **\[檢視\]**、**\[功能表\]**、**\[動作 1\]** 和 **\[動作 2\]** 按鈕。

| 瀏覽命令 | 電動搖桿輸入  |
| ------------------:| ------------------- |
|                 向上 | 搖桿向上            |
|               向下 | 搖桿向下          |
|               向左 | 搖桿向左          |
|              向右 | 搖桿向右         |
|               檢視 | 檢視按鈕         |
|               Menu | 功能表按鈕         |
|             接受 | 動作 1 按鈕     |
|             取消 | 動作 2 按鈕     |

電動搖桿不會對應瀏覽命令的任意[選用集](ui-navigation-controller.md#optional-set)。


## <a name="detect-and-track-arcade-sticks"></a>偵測和追蹤電動搖桿

電動搖桿由系統所管理，因此您不需加以建立或初始化。 系統提供已連接電動搖桿的清單，以及在新增或移除電動搖桿時通知您的事件。

### <a name="the-arcade-sticks-list"></a>電動搖桿清單

[ArcadeStick][] 類別提供靜態屬性 [ArcadeSticks][]，這個屬性是目前已連接電動搖桿的唯讀清單。 因為您可能只對一些已連接的電動搖桿感興趣，所以建議您維護您的專屬集合，而非透過 `ArcadeSticks` 屬性進行存取。

下列範例會將所有已連接的電動搖桿複製到新集合。
```cpp
auto myArcadeSticks = ref new Vector<ArcadeStick^>();

for (auto arcadestick : ArcadeStick::ArcadeSticks)
{
    // This code assumes that you're interested in all arcade sticks.
    myArcadeSticks->Append(arcadestick);
}
```

### <a name="adding-and-removing-arcade-sticks"></a>新增和移除電動搖桿

新增或移除電動搖桿時，會引發 [ArcadeStickAdded][] 和 [ArcadeStickRemoved][] 事件。 您可以註冊這些事件的處理常式來追蹤目前已連接的電動搖桿。

下列範例會開始追蹤已新增的電動搖桿。
```cpp
ArcadeStick::ArcadeStickAdded += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    // This code assumes that you're interested in all new arcade sticks.
    myArcadeSticks->Append(args);
}
```

下列範例會停止追蹤已移除的電動搖桿。
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

### <a name="users-and-headsets"></a>使用者和耳機

每個電動搖桿都可以與一個使用者帳戶建立關聯，以將其身分識別連結到其遊戲，並且可以連接耳機，以促進語音交談或遊戲中功能。 如需深入了解如何處理使用者和耳機，請參閱[追蹤使用者和其裝置](input-practices-for-games.md#tracking-users-and-their-devices)以及[耳機](headset.md)。


## <a name="reading-the-arcade-stick"></a>讀取電動搖桿

在您找出感興趣的電動搖桿之後，即可收集其輸入。 不過，電動搖桿不是透過引發事件來溝通狀態變更，這與您可能習慣使用的一些其他類型的輸入不同。 相反的，您可以進行「輪詢」__ 來定期讀取其目前狀態。

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

每個電動搖桿按鈕 (四向搖桿、六個**動作**按鈕和兩個**特殊**按鈕) 都會提供數位讀取來指出按下 (向下) 或放開 (向上)。 為了更有效率，按鈕讀取並非作為個別布林值呈現，而是壓縮成由 [ArcadeStickButtons][] 列舉所呈現的單一位元欄位。

> **注意**    電動搖桿配備用於 UI 瀏覽的其他按鈕，例如 **\[檢視\]** 和 **\[功能表\]** 按鈕。 這些按鈕不是 `ArcadeStickButtons` 列舉的一部分，而且只能將電動搖桿當成 UI 瀏覽裝置存取來進行讀取。 如需詳細資訊，請參閱 [UI 瀏覽裝置](ui-navigation-controller.md)。

按鈕值讀取自 [ArcadeStickReading][] 結構的 `Buttons` 屬性。 因為此屬性是位元欄位，所以使用位元遮罩來隔離感興趣按鈕的值。 設定對應位元時，即按下 (向下) 按鈕；否則為放開 (向上)。

下列範例判斷是否按下 [動作 1] 按鈕。
```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

下列範例判斷是否放開 [動作 1] 按鈕。
```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

您有時可能要判斷按鈕何時從按下轉換為放開或從放開轉換為按下、是否按下或放開多個按鈕，或是否以特定方式排列一組按鈕 (部分為按下，部分則否)。 如需如何偵測這些條件的相關資訊，請參閱[偵測按鈕轉換](input-practices-for-games.md#detecting-button-transitions)和[偵測複雜按鈕排列](input-practices-for-games.md#detecting-complex-button-arrangements)。

## <a name="run-the-inputinterfacing-sample"></a>執行 InputInterfacing 範例

[InputInterfacingUWP 範例 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) 示範如何同時使用電動搖桿和不同類型的輸入裝置，以及這些輸入裝置作為 UI 瀏覽控制器表現的方式。


## <a name="see-also"></a>另請參閱
[Windows.Gaming.Input.UINavigationController][]
[Windows.Gaming.Input.IGameController][]

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
