---
author: serenaz
description: Z 深度或相對深度及陰影有兩種深度納入您的應用程式自然且有效率地協助使用者專注。
title: Z 深度和陰影適用於 UWP app
template: detail.hbs
ms.author: sezhen
ms.date: 02/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: chigy
design-contact: balrayit
ms.localizationpriority: medium
ms.openlocfilehash: a1433b131b994ee2b1323909bc7c195e00f43cde
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2018
ms.locfileid: "4748510"
---
# <a name="z-depth-and-shadow"></a>Z 深度和陰影

![true 深度](images/elevation-shadow/depth.svg)

Fluent 深入了解系統會使用 3D 定位、 光線，例如實體的概念和陰影來重建如何數位 UI 可以認知更分層的實體環境中。 Z 深度或相對深度及陰影有兩種深度納入您的 UWP app。

## <a name="what-is-z-depth"></a>什麼是 z 深度？

Z 深度是與 z 軸，兩個表面之間的距離，它會示範如何關閉物件是檢視器。

![z 深度](images/elevation-shadow/elevation.svg)

### <a name="why-use-z-depth"></a>為什麼要使用 z 深度？

在實際世界中，我們通常會將焦點放在靠近我們的物件。 我們可以將此空間直覺套用至數位 UI。 例如，如果元素靠攏帶給使用者，使用者將 instinctively 重點放在項目。 移動 UI 項目接近 z 軸中，您可以建立物件，協助使用者完成工作自然且有效率地在您的應用程式之間的視覺階層。 

![在內容功能表中的 z 深度](images/elevation-shadow/whyelevation.svg)

除了提供有意義的視覺階層，也可 z 深度讓您建立橫向流動的經驗順暢地從 2D 到 3D 環境中，您的應用程式縮放跨所有裝置和外形規格。 

![以 2d 到 3d z 深度](images/elevation-shadow/elevation-2d3d.svg)

### <a name="how-is-z-depth-perceived"></a>如何認知 z 深度？

根據我們如何認知實際環境中的深度，以下是數種技術，可以用來在數位 UI 中顯示鄰近性。

- **縮放比例**久的物件會較接近相同的大小的物件。 這是方法很難在 2D 空間中，有效地示範，因此通常不建議。 不過，您可以使用縮放比例和[陰影](#what-is-shadow)建立將靠攏給使用者的 2D 物件的有效模擬。

    ![鄰近性與縮放比例](images/elevation-shadow/elevation-scale.svg)

- **氛圍**物件可以顯示遠，以及退出與 「 smoky 」 覆疊或其他 atmospheric 效果的焦點。

    ![氛圍與鄰近性](images/elevation-shadow/elevation-atmosphere.svg)

- **動作**相對的速度可以用來示範鄰近性： 近的物件移動速度比背景遠方物件更快。 若要了解如何實作此效果，請參閱[視差](../motion/parallax.md)。

    ![鄰近性的動作](images/elevation-shadow/elevation-motion.svg)

### <a name="recommendations-for-z-depth"></a>適用於 z 深度的建議

減少提升權限以提供清楚的視覺焦點平面的數目。 在大部分情況下，兩個平面就足以： 一個用於前景項目 （高鄰近性），另一個用於背景項目 （低鄰近性）。 如果您有多個提升權限的項目不重疊，群組它們以降低平面的數目相同的平面 （亦即，前景）。

![應用程式中的 z 深度](images/elevation-shadow/app-depth.svg)

## <a name="what-is-shadow"></a>什麼是陰影？

![陰影](images/elevation-shadow/shadow.svg)

陰影是一種方式認知提高權限。 光線提升權限的物件上方時，就會有陰影下面的表面上。 較高的物件、 更大和柔和陰影會變得。 請注意，不需要提升權限的物件會有陰影，但陰影並表示提高權限。

在 UWP app 中，陰影應該有意義、 不美觀。 如果焦點和生產力陰影受影響，然後限制陰影的使用。

您可以使用陰影 ThemeShadow 或 DropShadow Api。

## <a name="themeshadow"></a>ThemeShadow

類型可以套用到任何 XAML 元素，以繪製 ThemeShadow 遮蔽適當地根據的 x，y，z 座標。 ThemeShadow 也會自動調整以其他環境規格：

- 會適應光源、 使用者的佈景主題、 應用程式的環境，以及殼層中的變更。
- 陰影項目會自動根據其提高權限。
- 移動並變更提高權限會保留項目同步。
- 讓陰影一致整個及跨應用程式。

以下是在不同的提升權限的淺色和深色佈景主題使用 ThemeShadow 的範例：

![使用淺色佈景主題的智慧陰影](images/elevation-shadow/smartshadow-light.svg)

![使用深色佈景主題的智慧陰影](images/elevation-shadow/smartshadow-dark.svg)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow 通用控制項

下列的通用控制項自動將會使用 ThemeShadow 轉型陰影：

- [對話方塊和飛出視窗](../controls-and-patterns/dialogs.md)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [媒體傳輸控制項](../controls-and-patterns/media-playback.md)
- [操作功能表](../controls-and-patterns/menus.md)
- [命令列](../controls-and-patterns/app-bars.md)
- [自動建議](../controls-and-patterns/auto-suggest-box.md)，[下拉式方塊](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)，[行事曆/日期/時間選擇器](../controls-and-patterns/date-and-time.md)，[工具提示](../controls-and-patterns/tooltips.md)
- [便捷鍵](../input/access-keys.md)

### <a name="themeshadow-in-popups"></a>在快顯視窗中 ThemeShadow

ThemeShadow 自動將轉換套用到快[顯視窗](/uwp/api/windows.ui.xaml.controls.primitives.popup)中的任何 XAML 元素時的陰影。 它會轉型陰影它和其底下的其他任何開啟快顯背後的應用程式背景內容。

若要使用快顯視窗 ThemeShadow，使用`Shadow`屬性，以將 ThemeShadow 套用到 XAML 元素。 然後，在元素從提高其他元素後面，例如使用的 z 元件`Translation`屬性。
對於大部分的快顯 UI，建議的預設權限提高相對於應用程式背景內容是 32 個有效像素。

此範例中顯示的矩形的快顯視窗中轉型陰影到應用程式背景內容及它後方任何其他快顯視窗：

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="White" Height="48" Width="96">
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

![陰影從程式碼範例](images/elevation-shadow/smartshadow-example.svg)

### <a name="themeshadow-in-other-elements"></a>在其他元素 ThemeShadow

轉型陰影從 XAML 項目不在快顯視窗中，您必須明確地指定可接收的陰影中的其他項目`ThemeShadow.Receivers`集合。

這個範例示範轉型陰影到背後的方格的兩個按鈕：

```xaml
<Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Button x:Name="Button1" Content="Button 1" Shadow="{StaticResource SharedShadow}" Margin="10" />

    <Button x:Name="Button2" Content="Button 2" Shadow="{StaticResource SharedShadow}" Margin="120" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Button1.Translation += new Vector3(0, 0, 16);
Button2.Translation += new Vector3(0, 0, 32);
```

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow 效能最佳做法

1. 限制自訂收件者的最小的必要項目數目。 

2. 如果多個收件者元素位於相同的權限提高請嘗試將它們結合改為針對單一父項目。

3. 如果多個項目將會轉型陰影到相同的收件者元素上的相同類型，將陰影新增為共用的資源，並重複使用它。

## <a name="drop-shadow"></a>陰影

DropShadow 不會自動回應它的環境，而且不會使用光線的來源。 如需範例實作，請參閱[DropShadow 類別](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow)。

## <a name="which-shadow-should-i-use"></a>我應該使用哪些陰影？

| 屬性 | ThemeShadow | DropShadow |
| - | - | - | - |
| **最小值 SDK** | RS5 | 14393 |
| **適應性** | 是 | 否 |
| **自訂項目** | 否 | 是 |
| **光源** | 自動 (全域根據預設，但每個應用程式可以覆寫) | 無 |
| **在 3D 環境中支援** | 是 | 否 |

- 一般而言，我們建議使用 ThemeShadow，自動會適應環境。
- 如果您有更多進階自訂陰影的案例，請使用 DropShadow，也能更大的自訂項目。
- 適用於回溯相容性，使用 DropShadow。
- 效能相關的疑慮，陰影的數目限制或使用 DropShadow。
- 在以，則為 true 的 3D HMDs，使用 ThemeShadow。 DropShadow 繪製在指定的位移自 visual，它從側邊，父項，因為它將會看起來像是懸空在空間中。 相反地，ThemeShadow 會呈現在上方定義為接收器的視覺效果。
