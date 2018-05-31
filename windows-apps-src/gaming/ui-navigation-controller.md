---
author: mithom
title: UI 瀏覽控制器
description: 使用 Windows.Gaming.Input UI 瀏覽控制器 API，為 UI 瀏覽偵測及讀取不同種類的輸入裝置。
ms.assetid: 5A14926D-8C2E-4DE8-AAFB-BEEB9BFE91A5
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 遊戲, ui, 瀏覽
ms.localizationpriority: medium
ms.openlocfilehash: cae9d515ba5925ce81c90dfe5eb3785491128010
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
ms.locfileid: "1656133"
---
# <a name="ui-navigation-controller"></a>UI 瀏覽控制器

此頁面說明使用 [Windows.Gaming.Input.UINavigationController][uinavigationcontroller] 進行 UI 瀏覽裝置程式設計的基本知識，以及通用 Windows 平台 (UWP) 的相關 API。

閱讀此頁面，即可了解：
* 如何收集所連接 UI 瀏覽裝置和其使用者的清單
* 如何偵測已新增或移除瀏覽裝置
* 如何自一或多個 UI 瀏覽裝置讀取輸入
* 遊戲台與機台搖桿如何作為瀏覽裝置使用

## <a name="ui-navigation-controller-overview"></a>UI 瀏覽控制器概觀

幾乎所有遊戲都具有至少一些與遊戲分離的使用者介面，即使只是遊戲前功能表或遊戲內的對話。 玩家需能夠透過使用其所選的任何輸入裝置瀏覽此 UI，但這讓開發人員得費心為各種輸入裝置新增特定支援，且可能在遊戲與輸入裝置間引入不一致而讓玩家感到困惑。 有鑑於這些原因，[UINavigationController][] API 應運而生。

UI 瀏覽控制器為「邏輯」__ 輸入裝置，存在目的為提供受多種「實體」__ 輸入裝置支援的常用 UI 瀏覽命令詞彙。 「UI 瀏覽控制器」__ 僅為另一種看待實體輸入裝置的方式，我們使用「瀏覽裝置」__ 指稱任何作為瀏覽控制器看待的實體輸入裝置。 透過對瀏覽裝置而非特定輸入裝置進行程式設計，開發人員可避免支援不同輸入裝置的負擔，並預設達成一致性。

由於各種輸入裝置所支援的控制項數目及種類大相逕庭，且因為特定輸入裝置可能會想支援一組更豐富的瀏覽命令組，因此瀏覽控制器介面將命令詞彙分割為「必要集」__(內含最常見且必要的命令)，以及「選用集」__(內含實用但可省略的命令)。 所有瀏覽裝置都支援「必要集」中的每個命令__，「選用集」__ 中的命令則予以全部、部分或完全不支援。

### <a name="required-set"></a>必要集

瀏覽裝置必須支援「必要集」__ 中的所有命令，其為定向 (上、下、左、右)、檢視、功能表、接受及取消命令。

定向命令目的為供單一 UI 元素間的主要 [XY 焦點瀏覽](../design/devices/designing-for-tv.md#xy-focus-navigation-and-interaction)所使用。 檢視及功能表選單目的分別為顯示遊戲資訊 (通常為短暫，有時為強制)，以及在遊戲及功能表內容間切換。 接受及取消命令的目的分別為肯定 (是) 及否定 (否) 回應。

下表為這些命令及其預定用途的摘要，內含範例。
| 命令 | 預定用途
| -------:| ---------------
|      Up | XY 焦點向上瀏覽
|    Down | XY 焦點向下瀏覽
|    Left | XY 焦點向左瀏覽
|   Right | XY 焦點向右瀏覽
|    View | 顯示遊戲資訊 (計分板、遊戲統計資料、目標、世界或區域地圖)__
|    Menu | 主要功能表 / 暫停 (設定、狀態、設備、存貨、暫停)__
|  Accept | 肯定回應 (接受、進階、確認、開始、是)__
|  Cancel | 否定回應 (拒絕、反向、婉拒、停止、否)__


### <a name="optional-set"></a>選用集

瀏覽裝置可能同時支援全部、部分或不支援「選用集」__ 內的瀏覽命令，其為分頁 (上、下、左、右)、捲動 (上、下、左、右)，及內容相關的 (內容 1-4) 命令。

內容相關的命令目的明確地為應用程式特定命令及瀏覽捷徑。 分頁及捲動命令目的分別為快速在頁面及 UI 元素群組間瀏覽，以及在 UI 元素內更精細地瀏覽。

下表為這些命令及其預定用途的摘要。
|     命令 | 預定用途
| -----------:| ------------
|      PageUp | 向上跳 (至上層/前一個垂直頁或群組)
|    PageDown | 向下跳 (至下層/下一個垂直頁或群組)
|    PageLeft | 向左跳 (至左方/前一個水平頁或群組)
|   PageRight | 向右跳 (至右方/下一個水平頁或群組)
|    ScrollUp | 向上捲動 (在焦點 UI 元素或可捲動的群組內)
|  ScrollDown | 向下捲動 (在焦點 UI 元素或可捲動的群組內)
|  ScrollLeft | 向左捲動 (在焦點 UI 元素或可捲動的群組內)
| ScrollRight | 向右捲動 (在焦點 UI 元素或可捲動的群組內)
|    Context1 | 主要內容動作
|    Context2 | 次要內容動作
|    Context3 | 第三內容動作
|    Context4 | 第四內容動作

> **注意**    雖然遊戲可任意回應任何命令 (其中實際功能與預期用途不同)，但仍應避免意料外的行為。 尤其，如需命令的預期用途，請勿變更其實際功能；請嘗試將新穎功能指派給最適當的命令，並將對應功能指派給對應命令，例如 PageUp/PageDown。 最後，考量各種輸入裝置支援哪些命令，以及其對應至哪些控制項，進而確保每部裝置都可存取關鍵的命令。

## <a name="gamepad-arcade-stick-and-racing-wheel-navigation"></a>遊戲台、機台搖桿以及競速方向盤瀏覽

Windows.Gaming.Input 命名空間支援的所有輸入裝置皆為 UI 瀏覽裝置。

下表為瀏覽命令「必要集」__ 對應至各種輸入裝置的方式摘要。

| 瀏覽命令 | 遊戲台輸入                       | 機台搖桿輸入 | 競速方向盤輸入 |
| ------------------:| ----------------------------------- | ------------------ | ------------------ |
|                 Up | 左搖桿向上 / 方向鍵向上       | 搖桿向上           | 方向鍵向上           |
|               向下 | 左搖桿向下 / 方向鍵向下   | 搖桿向下         | 方向鍵向下         |
|               向左 | 左搖桿向左 / 方向鍵向左   | 搖桿向左         | 方向鍵向左         |
|              向右 | 左搖桿向右 / 方向鍵向右 | 搖桿向右        | 方向鍵向右        |
|               View | 檢視按鈕                         | 檢視按鈕        | 檢視按鈕        |
|               Menu | 功能表按鈕                         | 功能表按鈕        | 功能表按鈕        |
|             Accept | A 按鈕                            | 動作 1 按鈕    | A 按鈕           |
|             取消 | B 按鈕                            | 動作 2 按鈕    | B 按鈕           |

下表為瀏覽命令「選用集」__ 對應至各種輸入裝置的方式摘要。

| 瀏覽命令 | 遊戲台輸入          | 機台搖桿輸入 | 競速方向盤輸入    |
| ------------------:| ---------------------- | ------------------ | --------------------- |
|             PageUp | LT 鍵           | _不受支援_    | _變動_              |
|           PageDown | RT 鍵          | _不受支援_    | _變動_              |
|           PageLeft | LB 鍵            | _不受支援_    | _變動_              |
|          PageRight | RB 鍵           | _不受支援_    | _變動_              |
|           ScrollUp | 右搖桿向上    | _不受支援_    | _變動_              |
|         ScrollDown | 右搖桿向下  | _不受支援_    | _變動_              |
|         ScrollLeft | 右搖桿向左  | _不受支援_    | _變動_              |
|        ScrollRight | 右搖桿向右 | _不受支援_    | _變動_              |
|           Context1 | X 按鈕               | _不受支援_    | X 按鈕 (常用__) |
|           Context2 | Y 按鈕               | _不受支援_    | Y 按鈕 (常用__) |
|           Context3 | 左搖桿按下  | _不受支援_    | _變動_              |
|           Context4 | 右搖桿按下 | _不受支援_    | _變動_              |


## <a name="detect-and-track-ui-navigation-controllers"></a>偵測與追蹤 UI 瀏覽控制器

雖然 UI 瀏覽控制器為邏輯輸入裝置，其仍代表實體裝置且以相同方式由系統所管理。 您無須加以建立或初始化，系統會提供已連接 UI 瀏覽控制器及事件的清單，於新增或移除 UI 瀏覽控制器時通知您。

### <a name="the-ui-navigation-controllers-list"></a>UI 瀏覽控制器清單

[UINavigationController][] 類別提通靜態屬性，[UINavigationControllers][] 為目前已連接之 UI 瀏覽裝置的唯讀清單。 因為您可能只對一些已連接的瀏覽裝置感興趣，所以建議您維護您的專屬集合，而非透過 `UINavigationControllers` 屬性進行存取。

下列範例將所有 UI 瀏覽控制器複製到新的集合中。
```cpp
auto myNavigationControllers = ref new Vector<UINavigationController^>();

for (auto device : UINavigationController::UINavigationControllers)
{
    // This code assumes that you're interested in all navigation controllers.
    myNavigationControllers->Append(device);
}
```

### <a name="adding-and-removing-ui-navigation-controllers"></a>新增及移除 UI 瀏覽控制器

新增或移除 UI 瀏覽控制器時，即會引發 [UINavigationControllerAdded][] 及 [UINavigationControllerRemoved][] 事件。 您可以登錄這些事件的事件處理常式來追蹤目前已連接的瀏覽裝置。

下列範例開始追蹤新增的 UI 瀏覽裝置。
```cpp
UINavigationController::UINavigationControllerAdded += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    // This code assumes that you're interested in all new navigation controllers.
    myNavigationControllers->Append(args);
}
```

下列範例會停止追蹤已移除的機台搖桿。
```cpp
UINavigationController::UINavigationControllerRemoved += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    unsigned int indexRemoved;

    if(myNavigationControllers->IndexOf(args, &indexRemoved))
    {
        myNavigationControllers->RemoveAt(indexRemoved);
    }
}
```

### <a name="users-and-headsets"></a>使用者和耳機

每個瀏覽裝置都可以與一個使用者帳戶建立關聯，以便將使用者的身分識別連結到他們的輸入，並且可以連接耳機來協助語音交談或瀏覽功能。 如需深入了解如何處理使用者和耳機，請參閱[追蹤使用者和其裝置](input-practices-for-games.md#tracking-users-and-their-devices)及[耳機](headset.md)。

## <a name="reading-the-ui-navigation-controller"></a>閱讀 UI 瀏覽控制器

在您識別到感興趣的 UI 瀏覽裝置之後，即可收集其輸入。 不過，瀏覽裝置不是透過引發事件來溝通狀態變更，這與您可能習慣使用的一些其他類型的輸入不同。 相反地，您可以進行「輪詢」__ 來定期讀取其目前狀態。

### <a name="polling-the-ui-navigation-controller"></a>對 UI 瀏覽控制器進行輪詢

輪詢可在精確的時間點擷取瀏覽裝置的快照。 這種輸入收集方式適用於大部分遊戲，因為其邏輯一般是以決定性迴圈執行，而不是透過事件驅動；透過一次所收集的輸入來解譯遊戲命令，一般也比透過一段時間所收集的許多單一輸入來解譯遊戲命令更為簡單。

您透過呼叫 [UINavigationController.GetCurrentReading][getcurrentreading] 對瀏覽裝置進行輪詢，此函式會傳回內含瀏覽裝置狀態的 [UINavigationReading][]。

```cpp
auto navigationController = myNavigationControllers[0];

UINavigationReading reading = navigationController->GetCurrentReading();
```

### <a name="reading-the-buttons"></a>讀取按鈕

每個 UI 瀏覽按鈕都會提供布林值讀取，其對應至其按下 (向下) 或放開 (向上)。 為了更有效率，按鈕讀取並非作為個別布林值呈現，而是壓縮成由 [RequiredUINavigationButtons][] 及 [OptionalUINavigationButtons][] 列舉所呈現之位元欄位的其中一項。

屬於「必要集」__ 的按鈕會從 `RequiredButtons` 屬性讀取 (該屬性屬於 [UINavigationReading][] 結構)，而屬於「選用集」__ 的按鈕會從 `OptionalButtons` 屬性讀取。 因為這些屬性為位元欄位，所以使用位元遮罩來隔離感興趣按鈕的值。 設定對應位元時，即按下 (下) 按鈕；否則為放開 (上)。

下列範例判斷「必要集」__ 的 [接受] 按鈕是否按下。
```cpp
if (RequiredUINavigationButtons::Accept == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is pressed
}
```

下列範例判斷「必要集」__ 的 [接受] 按鈕是否放開。
```cpp
if (RequiredUINavigationButtons::None == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is released (not pressed)
}
```

讀取「選用集」__ 中的按鈕時，請務必使用 `OptionalButtons` 屬性及 `OptionalUINavigationButtons` 列舉。

下列範例判斷「選用集」__ 的 [Context 1] 按鈕是否按下。
```cpp
if (OptionalUINavigationButtons::Context1 == (reading.OptionalButtons & OptionalUINavigationButtons::Context1))
{
    // Context 1 is pressed
}
```

您有時可能要判斷按鈕何時從按下轉換為放開或從放開轉換為按下、是否按下或放開多個按鈕，或是否以特定方式排列一組按鈕 (部分為按下，部分則否)。 如需如何偵測這些條件的相關資訊，請參閱[偵測按鈕轉換](input-practices-for-games.md#detecting-button-transitions)和[偵測複雜按鈕排列](input-practices-for-games.md#detecting-complex-button-arrangements)。


## <a name="run-the-ui-navigation-controller-sample"></a>執行 UI 瀏覽控制器範例

[InputInterfacingUWP 範例 _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) 示範不同輸入裝置作為 UI 瀏覽控制器表現的方式。

## <a name="see-also"></a>另請參閱
[Windows.Gaming.Input.Gamepad][]
[Windows.Gaming.Input.ArcadeStick][]
[Windows.Gaming.Input.RacingWheel][]
[Windows.Gaming.Input.IGameController][]


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.Gamepad]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[Windows.Gaming.Input.Arcadestick]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.aspx
[Windows.Gaming.Input.Racingwheel]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[uinavigationcontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[uinavigationcontrollers]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrollers.aspx
[uinavigationcontrolleradded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrolleradded.aspx
[uinavigationcontrollerremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrollerremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.getcurrentreading.aspx
[uinavigationreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationreading.aspx
[requireduinavigationbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.requireduinavigationbuttons.aspx
[optionaluinavigationbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.optionaluinavigationbuttons.aspx
