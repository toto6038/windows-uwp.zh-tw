---
author: knicholasa
description: '[Z 深度] 或 [相對深度] 和 [陰影] 是將深度併入應用程式的兩種方式，可讓使用者自然且有效率地進行焦點。'
title: UWP 應用程式的 Z 深度和陰影
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: Windows 10, UWP
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: 216974ba564a192f94473469f3a7a49191ef2192
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081391"
---
# <a name="z-depth-and-shadow"></a>Z 深度和陰影

![顯示四個灰色矩形的 gif，其中另一個是以對角方向排列。 Gif 會以動畫顯示，讓陰影出現並消失。](images/elevation-shadow/shadow.gif)

在您的 UI 中建立專案的視覺階層，可讓 UI 易於掃描並傳達著重于的重要事項。 提高許可權是將 UI 的 select 元素帶向前的動作，通常是用來達成軟體中的這類階層。 本文討論如何使用 z 深度和陰影在 UWP 應用程式中建立提高許可權。

Z 深度是在3D 應用程式建立者之間使用的詞彙，用來表示沿著 Z 軸的兩個表面之間的距離。 說明如何將物件與檢視器關閉。 將它想像成 x/y 座標的類似概念，但在 z 方向。

## <a name="why-use-z-depth"></a>為何要使用 z 深度？

在真實世界中，我們傾向于將焦點放在更接近我們的物件。 我們也可以將此空間直覺套用至數位 UI。 例如，如果您將專案帶到更接近使用者的專案，則使用者會將焦點 instinctively 在元素上。 藉由移動靠近 Z 軸的 UI 元素，您可以在物件之間建立視覺階層，協助使用者在您的應用程式中自然且有效率地完成工作。

## <a name="what-is-shadow"></a>什麼是陰影？

「陰影」是使用者感覺提高許可權的一種方式。 較高許可權的物件會在下方的表面上建立陰影。 物件越高，陰影會變大且更柔和。 在您的 UI 中，提高許可權的物件不需要有陰影，但有助於建立提高許可權的外觀。

在 UWP 應用程式中，應該在有目的中使用陰影，而不是美觀方式。 使用太多陰影會減少或消除陰影將使用者專注的能力。

如果您使用標準控制項，ThemeShadow shadows 會自動併入您的 UI 中。 不過，您可以使用 ThemeShadow 或 DropShadow Api，以手動方式將陰影加入您的 UI 中。 

## <a name="themeshadow"></a>ThemeShadow

[ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow)類型可以套用至任何 XAML 元素，以根據 x、y、z 座標適當地繪製陰影。 ThemeShadow 也會自動針對其他環境規格進行調整：

- 適應光源、使用者主題、應用程式環境和 shell 中的變更。
- 根據物件的 z 深度，自動套用陰影至元素。 
- 當專案移動和變更提高許可權時，讓元素保持同步。
- 讓整個應用程式的陰影保持一致。

以下是在 MenuFlyout 上執行 ThemeShadow 的方式。 MenuFlyout 有內建的經驗，主要介面會提升至32px，而每個額外的階層式功能表會在其開啟的功能表上方開啟 + 8px。

![ThemeShadow 的螢幕擷取畫面，套用至具有三個開啟之嵌套功能表的 MenuFlyout。 第一個功能表會提高許可權32px，而從上一個功能表開啟的每個後續功能表會更高的8px，讓它在背景上留有相異的陰影。](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>通用控制項中的 ThemeShadow

除非另有指定，否則下列通用控制項會自動使用 ThemeShadow 從32px 深度轉換陰影：

- [內容功能表](../controls-and-patterns/menus.md)、[命令列](../controls-and-patterns/app-bars.md)、[命令列飛出](../controls-and-patterns/command-bar-flyout.md)視窗、[功能表](../controls-and-patterns/menus.md#create-a-menu-bar)欄
- 對話方塊[和 flyouts](../controls-and-patterns/dialogs.md) （位於64px 的對話）
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md)、 [DropDownButton、SplitButton、ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [行事曆/日期/時間選取器](../controls-and-patterns/date-and-time.md)
- [工具提示](../controls-and-patterns/tooltips.md)（16px）
- [媒體傳輸控制](../controls-and-patterns/media-playback.md#media-transport-controls)， [InkToolbar](../controls-and-patterns/inking-controls.md)
- [連接動畫](../motion/connected-animation.md)

注意： Flyouts 只會在針對 Windows 10 1903 版或較新版本的 SDK 編譯時套用 ThemeShadow。

### <a name="themeshadow-in-popups"></a>在快顯視窗中 ThemeShadow

通常，應用程式的 UI 會針對您需要使用者注意和快速動作的案例使用快顯視窗。 當您使用「陰影」來協助在應用程式的 UI 中建立階層時，這些都是很好的例子。

ThemeShadow 會在應用於[快顯視窗](/uwp/api/windows.ui.xaml.controls.primitives.popup)中的任何 XAML 專案時，自動轉換陰影。 它會在應用程式背景內容和其下的任何其他開啟的快顯視窗之間轉換陰影。

若要搭配使用 ThemeShadow 與快顯視窗，請使用 `Shadow` 屬性將 ThemeShadow 套用至 XAML 專案。 然後，將專案從其後方的其他元素提升，例如使用 `Translation` 屬性的 z 元件。
對於大部分的快顯 UI，相對於應用程式背景內容的建議預設提高許可權為32有效圖元。

這個範例會在快顯視窗中顯示一個矩形，將陰影轉換成應用程式背景內容和其後方的任何其他快顯視窗：

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

![具有陰影的單一矩形快顯視窗。](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>停用自訂飛出視窗控制項的預設 ThemeShadow

以[飛出](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout)視窗、 [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout)、 [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout)或[TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout)為依據的控制項會自動使用 ThemeShadow 來轉換陰影。

如果預設陰影在控制項的內容上看起來不正確，您可以將[IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled)屬性設定為相關聯 FlyoutPresenter 上的 `false`，將它停用：

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

在一般情況下，我們建議您仔細考慮使用陰影，並限制其使用方式來引進有意義的視覺階層。 不過，如果您有需要的先進案例，我們確實會提供從任何 UI 元素轉換陰影的方法。

若要從不在快顯視窗中的 XAML 專案轉換陰影，您必須明確指定可以在 `ThemeShadow.Receivers` 集合中接收陰影的其他元素。 接收者不可以是視覺化樹狀結構中 caster 的上階。

這個範例會顯示兩個矩形，將陰影轉換成其後方的格線：

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

![彼此連續的兩個青綠色矩形，兩者都有陰影。](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow 的效能最佳作法

1. 系統會將限制設定為 5 caster-接收者配對，如果超過此值，將會關閉陰影。 繼續進行系統強制的限制 5 caster-接收者配對。

2. 將自訂接收者元素的數目限制為所需的最小值。

3. 如果多個接收者元素的高度相同，請嘗試改為以單一父元素為目標來加以結合。

4. 如果多個元素會將相同類型的陰影轉換成相同的接收器專案，則將陰影新增為共用資源，並重複使用它。

## <a name="drop-shadow"></a>陰影

DropShadow 不會自動回應其環境，也不會使用光線來源。 如需範例，請參閱[DropShadow 類別](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow)。

## <a name="which-shadow-should-i-use"></a>應該使用哪一個陰影？

| 屬性 | ThemeShadow | DropShadow |
| - | - | - |
| **最小 SDK** | Windows 10 版本1903 | 14393 |
| **能力** | 是 | 否 |
| **定義** | 否 | 是 |
| **光線來源** | 自動（預設為全域，但可覆寫每個應用程式） | 無 |
| **在3D 環境中支援** | 是 | 否 |

- 請記住，shadow 的用途是提供有意義的階層，而不是簡單的視覺處理。
- 一般來說，我們建議使用 ThemeShadow，它會自動適應其環境。
- 如需有關效能的考慮，請限制陰影的數目、使用其他視覺處理，或使用 DropShadow。
- 如果您有更多更先進的案例可達成視覺階層，請考慮使用其他視覺處理（例如，色彩）。 如果需要陰影，請使用 DropShadow。
