---
author: mithom
title: "遊戲的輸入練習"
description: "了解有效使用輸入裝置所需的模式與技巧。"
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 遊戲, 輸入"
ms.openlocfilehash: 15d56a27ad914b258bb19b80b3498510d01105cd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="input-practices-for-games"></a>遊戲的輸入練習

此頁面說明在通用 Windows 平台 (UWP) 遊戲中有效使用輸入裝置所需的模式與技巧。

閱讀此頁面，即可了解：
* 如何追蹤玩家，以及他們目前所使用的輸入與瀏覽裝置
* 如何偵測按鈕轉換 (按下轉換到放開，放開轉換到按下)
* 如何在單一測試中偵測複雜按鈕排列

## <a name="tracking-users-and-their-devices"></a>追蹤使用者及其裝置

所有的輸入裝置皆與[使用者][Windows.System.User]相關聯，如此他們的身分識別便可連結到其遊戲、成就、設定變更和其他活動。 使用者可以隨意登入或登出，而且通常不同的使用者可在前一名使用者登出後，從與系統連線的輸入裝置登入。 當使用者登入或登出，會引發 [IGameController.UserChanged][] 事件。 您可以登錄此事件的事件處理常式來追蹤玩家，以及其正在使用的裝置。

使用者身分識別是輸入裝置與其 UI 瀏覽控制器建立關聯的方式。

基於這些原因，應使用裝置類別的[使用者][igamecontroller.user]屬性 (繼承自 [IGameController][]介面) 來追蹤與關聯玩家輸入。

[UserGamepadPairingUWP _(github)_](
https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/UserGamepadPairingUWP) 範例示範如何持續追蹤使用者及其正在使用的裝置。

## <a name="detecting-button-transitions"></a>偵測按鈕轉換

您有時可能需要了解何時第一次按下或放開按鈕，更精確地說就是按鈕狀態轉換何時從放開轉換到按下，或從按下轉換到放開。 若要判斷此事，請記住之前的裝置讀數，並且和目前的讀數比較，以了解其間的變動狀況。

以下範例示範記住之前讀數的基本方法；這裡顯示的是遊戲台，但機台搖桿、賽車方向盤和 UI 瀏覽按鈕所適用的原則都相同

```cpp
GamepadReading newReading();
GamepadReading oldReading();

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.CurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased
}
```

進行其他動作之前，`Game::Loop` 將現有的 `newReading` 值 (之前迴圈反覆運算的遊戲台讀數) 移至 `oldReading`，然後使用目前反覆運算的遊戲台讀數填寫 `newReading`。 這提供您偵測按鈕轉換所需的資訊。

下列範例示範偵測按鈕轉換的基本方法。

```cpp
bool buttonJustPressed(const gamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool buttonJustReleased(gamepadButtons selection)
{
    bool newSelectionReleased = (gamepadButtons.None == (newReading.Buttons & selection));
    bool oldSelectionReleased = (gamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

這兩個功能會先從 `newReading` 及 `oldReading` 的按鈕選擇衍生布林值狀態，再執行布林邏輯以判斷是否已發生目標轉換。 如果新的讀數包含目標狀態 (分別為按下或放開) *而且*舊讀數也未包含目標狀態時，這些功能僅傳回 _true_，否則會傳回 _false_。


## <a name="detecting-complex-button-arrangements"></a>偵測複雜按鈕排列

輸入裝置的每個按鈕都會提供讀取數值，指明其按下 (向下) 或放開 (向上) 狀態。 基於效率考量，按鈕讀數並不是以個別布林值表示，而是封裝到以 [GamepadButtons][] 之類的裝置特定列舉表示的位元欄位中。 若要讀取特定按鈕，可使用位元遮罩來隔離您感興趣的值。 設定對應位元時，即按下 (下) 按鈕，否則為放開 (上)。

回想如何判斷按下或放開單一按鈕；這裡顯示的是遊戲台，但機台搖桿、賽車方向盤和 UI 瀏覽按鈕所適用的原則都相同。

```cpp
// determines whether gamepad button A is pressed
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}

// determines whether gamepad button A is released
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released (not pressed)
}
```

如您所見，判斷單一按鈕何的狀態是直截了當的，但是您有時可能會想要判斷是否按下或放開多個按鈕，或是否以特定方式排列一組按鈕 (部分為按下，部分則否)。 測試多個按鈕比測試單一按鈕更複雜，尤其是在可能的混合按鈕狀態，但是這些測試有一個簡單的公式，單一按鈕或多個按鈕測試皆適用。

下列範例會判斷是否同時按下遊戲台按鈕 A 和 B。

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // buttons A and B are pressed
}
```

下列範例會判斷是否同時放開遊戲台按鈕 A 和 B。

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // buttons A and B are released (not pressed)
}
```

下列範例會判斷是否在按下遊戲台按鈕 A 的同時放開按鈕 B。

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // button A is pressed and button B is released (button B is not pressed)
}
```

這五個範例共有的公式是，要進行測試的按鈕排列由等號比較運算子左側的運算式指定，而右側的遮罩運算式則選擇要考慮的按鈕。

下列範例藉由重寫之前的範例，更清楚地示範此公式。

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // button A is pressed and button B is released (button B is not pressed)
}
```

可套用此公式測試任何排列狀態中的任意按鈕數。



[Windows.System.User]: https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx
[igamecontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[igamecontroller.user]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.user.aspx
[igamecontroller.userchanged]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.userchanged.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx
