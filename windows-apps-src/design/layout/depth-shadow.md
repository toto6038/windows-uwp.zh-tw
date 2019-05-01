---
author: knicholasa
description: Z 深度，或是相對深度，以及陰影是兩種方式併入您的應用程式，以協助使用者專注自然又有效率的深度。
title: Z 深度和 UWP 應用程式的陰影
template: detail.hbs
ms.author: nichola
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: Windows 10, UWP
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: ab49f13d3938e55750ce523f9e0d4ae241304763
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63817712"
---
# <a name="z-depth-and-shadow"></a>Z 深度和陰影

![顯示四個灰色矩形的 gif 對角線，堆疊彼此。 Gif 被動畫，讓陰影顯示和消失。](images/elevation-shadow/shadow.gif)

在 UI 中建立的項目以視覺階層可讓 UI 更容易閱讀，並傳達專注於重要。 提高權限，將選取的元素的正向，您的 UI 中的動作通常用來達成這類階層中的軟體。 這篇文章討論如何在 UWP 應用程式中建立權限提高，使用 z 深度和陰影。

Z 深度指的是 3D 應用程式建立者之間，用以代表沿著 z 軸的兩個介面之間的距離。 這個範例說明如何關閉物件是檢視器。 把它想成類似的概念，x / y 座標，但是 z 方向。

## <a name="why-use-z-depth"></a>為何要使用 z 深度？

在真實世界中，我們通常會將焦點放在接近我們的物件。 我們可以套用這個空間直覺數位的 ui。 例如，如果您讓使用者更接近的項目，然後使用者將憑著重在項目。 移動 UI 項目更接近 z 軸中，您可以建立視覺物件，協助您的應用程式中自然又有效率地完成工作的使用者之間的階層。

## <a name="what-is-shadow"></a>陰影是什麼？

陰影是使用者察覺到提高權限的一種方式。 於提升權限的物件上的指示燈上下方的介面建立一個陰影。 較高的物件、 大和柔和會變成陰影。 不需要提高權限的物件，在 UI 中會有陰影，但它們可協助建立外觀的提高權限。

在 UWP 應用程式，陰影應該使用有目的，而不是美觀的方式。 使用太多的陰影，將會減少或消除陰影專注使用者的能力。

如果您使用標準控制項，ThemeShadow shadows 會併入自動 UI。 不過，您可以手動包含陰影在 UI 中使用 ThemeShadow 或 DropShadow Api。 

## <a name="themeshadow"></a>ThemeShadow

類型可以套用至任何 XAML 項目，可以繪製 ThemeShadow 遮蔽適當地根據的 x、 y、 z 座標。 ThemeShadow 也會自動調整的其他環境的規格：

- 調整光源、 使用者佈景主題，應用程式的環境和 shell 中的變更。
- 適用於項目會自動根據其 z 深度陰影。 
- 它們移動和變更提高權限會讓項目保持同步。
- Shadows 讓保持一致，跨應用程式中使用。

以下是 ThemeShadow 如何實作 MenuFlyout 上。 MenuFlyout 體驗其中的主要介面提升為 32px，而每個額外的階層式功能表開啟 + 8px 上述從開啟的功能表中有內建的。

![套用至三個開放式的巢狀功能表 MenuFlyout ThemeShadow 的螢幕擷取畫面。 第一個功能表是提高權限的 32px，每個後續的功能表，從上一個 功能表開啟提升權限的 8px 詳細，會使不同的陰影的背景。](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow 通用控制項

下列的一般控制項轉換 32px 深度從陰影，除非另有指定，會自動使用 ThemeShadow:

- [操作功能表](../controls-and-patterns/menus.md)，[命令列](../controls-and-patterns/app-bars.md)，[命令列飛出視窗](../controls-and-patterns/command-bar-flyout.md)，[功能表列](../controls-and-patterns/menus.md#create-a-menu-bar)
- [對話方塊和延伸顯示](../controls-and-patterns/dialogs.md)（在對話方塊中的從 64px）
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md)， [DropDownButton，SplitButton，ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [行事曆/日期/時間選擇器](../controls-and-patterns/date-and-time.md)
- [工具提示](../controls-and-patterns/tooltips.md)(16px)
- [媒體傳輸控制項](../controls-and-patterns/media-playback.md#media-transport-controls)， [InkToolbar](../controls-and-patterns/inking-controls.md)
- [已連線的動畫](../motion/connected-animation.md)

注意：延伸顯示 ThemeShadow 針對 Windows 10 版本 1903年或較新的 SDK 編譯時才適用。

### <a name="themeshadow-in-popups"></a>在快顯功能表中的 ThemeShadow

它通常是，您的應用程式 UI 快顯視窗會針對使用案例，您需要使用者的注意及快速動作的情況。 當陰影應該用來協助您的應用程式 UI 中建立階層時，這些是絕佳的範例。

ThemeShadow 自動將轉換套用在任何 XAML 項目時的陰影[快顯](/uwp/api/windows.ui.xaml.controls.primitives.popup)。 它會轉換應用程式背景內容，它和其下的其他任何開啟快顯背後的陰影。

若要使用快顯 ThemeShadow，使用`Shadow`屬性，將 ThemeShadow 套用至 XAML 項目。 然後，提升權限項目從其他項目後面，例如使用的 z 元件`Translation`屬性。
大部分的快顯 ui，相對於應用程式背景內容建議的預設權限提高為 32 有效的像素。

此範例顯示矩形快顯視窗中拖曳至應用程式背景內容和其後的任何其他快顯投射的陰影：

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

![單一矩形快顯視窗以陰影。](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>停用預設 ThemeShadow 上自訂的彈出式視窗控制項

控制項根據[飛出視窗](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout)， [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout)， [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout)或是[TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout)自動使用 ThemeShadow 轉換陰影部分。

如果預設陰影看起來不正確上控制項的內容，則您可以藉由設定停用[IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled)屬性設`false`上相關聯的 FlyoutPresenter:

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>其他項目中的 ThemeShadow

一般情況下我們鼓勵您仔細思考陰影的使用，並限制其用途，它引入了有意義的視覺階層的案例。 不過，我們提供用來在投射陰影從任何 UI 項目，如果您有進階的案例需要它。

若要投射陰影從 XAML 項目不在快顯視窗中，您必須明確指定其他可接收在陰影的項目`ThemeShadow.Receivers`集合。 接收者不能施法視覺化樹狀結構中的上階。

此範例顯示兩個轉換拖曳至方格中，其背後的陰影的矩形：

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

![兩個彼此相鄰，都有陰影的淺粉藍色矩形。](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow 的效能最佳做法

1. 系統會設定為 5 施法者與接收者組，且如果超出此配額，將會關閉陰影。 堅持 5 施法者與接收者組的系統強制執行限制。

2. 限制的最小必要的自訂接收器元素數目。

3. 如果多個接收者項目都位於相同的提高權限，請嘗試將它們結合為改為目標的單一父項目。

4. 如果多個項目會投射陰影到相同的 「 接收器 」 項目上的相同類型，加入陰影做為共用的資源，並重複使用它。

## <a name="drop-shadow"></a>陰影

DropShadow 不自動回應其環境，而且不會使用光源。 比方說的實作，請參閱 < [DropShadow 類別](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow)。

## <a name="which-shadow-should-i-use"></a>我應該使用哪一個陰影？

| 屬性 | ThemeShadow | DropShadow |
| - | - | - | - |
| **Min SDK** | Windows 10 版本 1903， | 14393 |
| **可調適性** | 是 | 否 |
| **自訂** | 否 | 是 |
| **光源** | 自動 (根據預設，全域，但可以覆寫每個應用程式) | None |
| **3D 的環境中支援** | 是 | 否 |

- 請記住，陰影的目的是要提供有意義的階層中，不是簡單的視覺化方式處理。
- 一般而言，我們建議使用 ThemeShadow，自動配合其環境。
- 基於效能的相關考量，恒薴菾 shadows、 使用其他視覺化的方式處理，或使用 DropShadow。
- 如果您有更進階案例，以達到視覺階層，請考慮使用其他 visual 的處理方式 （例如色彩）。 如果需要陰影，則使用 DropShadow。
