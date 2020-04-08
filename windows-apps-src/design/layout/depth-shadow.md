---
author: knicholasa
description: Z 深度 (即相對深度) 以及陰影，是兩種在應用程式中呈現深度的方式，有助於使用者自然且有效率地對焦。
title: UWP 應用程式的 Z 深度與陰影
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: 216974ba564a192f94473469f3a7a49191ef2192
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081391"
---
# <a name="z-depth-and-shadow"></a>Z 深度和陰影

![gif 顯示四個以對角線方式堆疊的灰色矩形，互相交疊。 gif 顯示動畫效果，呈現陰影明滅。](images/elevation-shadow/shadow.gif)

在 UI 中建立元素的視覺階層，可讓 UI 輕鬆掃描並傳達應該聚焦的重要事項。 系統往往會運用立體高度 (強調 UI 的重要元素) 在軟體中達到階層效果。 本文探討如何使用 z 深度和陰影在 UWP 應用程式中建立立體高度。

Z 深度是在 3D 應用程式製作者之間通用的詞彙，用來表示沿著 Z 軸的兩個表面之間的距離， 展現了物件與檢視者之間有多近。 可以將其視為類似於 x/y 座標的概念，但為 z 的方向。

## <a name="why-use-z-depth"></a>為何使用 z 深度？

在現實世界中，人類傾向於對焦於離自己較近的物件。 這種空間直覺也可以套用至數位 UI。 例如，如果將元素靠近使用者，則使用者會直覺地將焦點放在該元素上。 藉由在 z 軸上移近 UI 元素，您可以在物件之間建立視覺層級，讓使用者在應用程式中自然且有效率地完成任務。

## <a name="what-is-shadow"></a>什麼是陰影？

陰影是使用者感知立體高度圖的一種方式。 高架物件上方的光線會在下方的表面上產生陰影。 物件越高，陰影面積變得越大且越柔和。 UI 中的高架物件不需要陰影，但是陰影有助於展現立體高度的外觀。

在 UWP 應用程式中，應該基於明確的目的使用陰影，而非出於美觀。 使用太多陰影會降低或消除使用者透過陰影聚焦的能力。

如果使用標準控制項，則 ThemeShadow 陰影會自動合併到 UI 中。 不過，您可以使用 ThemeShadow 或 DropShadow API，在 UI 中手動加入陰影。 

## <a name="themeshadow"></a>ThemeShadow

[ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow) 類型可套用至任何 XAML 元素，以根據 x、y、z 座標適當繪製陰影。 ThemeShadow 也可以配合其他環境規格，自動進行調整：

- 適應光線、使用者主題、應用程式環境，以及命令介面的變化。
- 根據物件的 z 深度，將陰影自動套用至元素。 
- 在移動和變更立體高度時，讓元素保持同步。
- 讓整個應用程式的陰影保持一致。

以下會介紹在 MenuFlyout 上實作 ThemeShadow 的方式。 MenuFlyout 具有內建功能，會將主體表面提升到 32px，而每個額外的串聯功能表都在其開啟的功能表上方 +8px 處開啟。

![套用至 MenuFlyout 的 ThemeShadow 的螢幕擷取畫面，具有三個開啟的巢狀功能表。 第一個功能表提高了 32px，而上一個功能表開啟的每個後續功能表也提高了 8px，因此在背景上留下明顯的陰影。](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>通用控制項中的 ThemeShadow

除非另有指定，否則以下通用控制項將自動使用 ThemeShadow 投射 32px 深度的陰影：

- [操作功能表](../controls-and-patterns/menus.md)、[命令列](../controls-and-patterns/app-bars.md)、[命令列飛出視窗](../controls-and-patterns/command-bar-flyout.md)、[功能表列](../controls-and-patterns/menus.md#create-a-menu-bar)
- [對話方塊與飛出視窗](../controls-and-patterns/dialogs.md) (64px 的對話方塊)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md)、[DropDownButton、SplitButton、ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [行事曆/日期/時間選擇器](../controls-and-patterns/date-and-time.md)
- [工具提示](../controls-and-patterns/tooltips.md) (16px)
- [媒體傳輸控制項](../controls-and-patterns/media-playback.md#media-transport-controls)、[InkToolbar](../controls-and-patterns/inking-controls.md)
- [連接動畫](../motion/connected-animation.md)

注意：飛出視窗只會在對 Windows 10 版本 1903 或較新版本的 SDK 編譯時，套用 ThemeShadow。

### <a name="themeshadow-in-popups"></a>快顯中的 ThemeShadow

通常，在需要使用者注意並快速採取動作的情況下，應用程式的 UI 會使用快顯。 這些是使用陰影以在應用程式 UI 中建立階層的優良範例。

當套用至[快顯](/uwp/api/windows.ui.xaml.controls.primitives.popup)中的任何 XAML 元素時，ThemeShadow 會自動投射陰影。 它會在後方的應用程式背景內容以及下方其他任何開啟的快顯上，投射陰影。

若要將 ThemeShadow 與快顯搭配使用，請使用 `Shadow` 屬性，以將 ThemeShadow 套用至 XAML 元素。 再從其背後的其他元素提高元素，例如使用 `Translation` 屬性的 z 元件。
對於大多數的快顯 UI，與應用程式背景內容相對的建議預設立體高度為 32 有效像素。

此範例的快顯中的矩形，會將陰影投射到應用程序背景內容及其後面的任何其他快顯上：

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="Lavender" Height="48" Width="96">
        <Rectangle.Shadow>
            <ThemeShadow />
        </Rectangle.Shadow>
    </Rectangle>
</Popup>
```

```csharp
// Elevate the rectangle by 32px
PopupRectangle.Translation += new Vector3(0, 0, 32);
```

![具有陰影的單個矩形快顯。](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>停用自訂飛出視窗控制項的預設 ThemeShadow

以 [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout)、[DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout)、[MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) 或 [TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) 為基礎的控制項，會自動使用 ThemeShadow 以投射陰影。

如果預設陰影在控制項的內容上看起來不正確，您可以在相關的 FlyoutPresenter 上，將 [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) 屬性設為 `false`，將其停用：

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>其他元素中的 ThemeShadow

在一般情況下，建議您仔細考慮陰影的使用方式，用途僅限於展現有意義的視覺階層。 不過如果您有進階案例需要此功能，確實有方式可從任何 UI 元素投射陰影。

如果要從並非位在快顯中的 XAML 元素投射陰影，必須明確指定可以在 `ThemeShadow.Receivers` 集合中接收陰影的其他元素。 接收者不可以是視覺化樹狀結構中投射者的上階。

此範例顯示兩個矩形，會將陰影投射到它們後面的網格：

```xaml
<Grid>
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" />

    <Rectangle x:Name="Rectangle1" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />

    <Rectangle x:Name="Rectangle2" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Rectangle1.Translation += new Vector3(0, 0, 16);
Rectangle2.Translation += new Vector3(120, 0, 32);
```

![兩個彼此相鄰的淺粉藍矩形，兩者都有陰影。](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow 的效能最佳做法

1. 系統設定 5 個投射者-接收者配對的限制，如果超過此限制，將關閉陰影。 請遵守系統強制規定的 5 個投射者-接收者配對限制。

2. 將自訂接收者元素的數量限制為所需的最小值。

3. 如果多個接收者元素位於相同的立體高度中，請嘗試將單一父元素設為目標來加以合併。

4. 如果多個元素將相同類型的陰影投射到相同的接收者元素上，則會將陰影新增為共用資源，並重複使用。

## <a name="drop-shadow"></a>陰影

DropShadow 不會自動回應其環境，而且不會使用光源。 如需實作範例，請參閱 [DropShadow 類別](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow)。

## <a name="which-shadow-should-i-use"></a>我應該使用哪一種陰影？

| 屬性 | ThemeShadow | DropShadow |
| - | - | - |
| **SDK 最低版本** | Windows 10 版本 1903 | 14393 |
| **適應性** | 是 | 否 |
| **自訂** | 否 | 是 |
| **光源** | 自動 (預設為全域，但可覆寫每個應用程式) | 無 |
| **在 3D 環境中支援** | 是 | 否 |

- 請記住，陰影的用途是展現有意義的階層，而非簡單的視覺處理。
- 通常建議使用自動適應其環境的 ThemeShadow。
- 如有效能方面的考量，請限制陰影數目，使用其他視覺處理，或使用 DropShadow。
- 如果您有更多進階案例要實作視覺階層，請考慮使用其他視覺處理 (例如顏色)。 如果需要陰影，請使用 DropShadow。
