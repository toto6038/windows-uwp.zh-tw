---
description: 顯色焦點是一種光源效果會動畫顯示可設定焦點元素的框線，當使用者將遊戲台或鍵盤焦點移至它們。
title: 顯色焦點
template: detail.hbs
ms.date: 03/1/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 311e5714c5428fac6509564fd00784299a02f630
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8799335"
---
# <a name="reveal-focus"></a>顯色焦點

![主角圖像](images/header-reveal-focus.svg)

顯色焦點是一種光源效果，針對[10 英呎體驗](/windows/uwp/design/devices/designing-for-tv)，例如 Xbox One 和電視螢幕。 這會在使用者將遊戲台或鍵盤焦點移近可設定焦點元素 (例如按鈕) 時，以動畫方式呈現這些元素的框線。 預設會關閉此效果，但要啟用，也很簡單 

（顯色醒目提示效果，光源造成的影響，反白顯示互動式元素，請參閱[顯色醒目提示文章](/windows/uwp/design/style/reveal)）。


> **重要 API**：[Application.FocusVisualKind property](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind)、[FocusVisualKind enum](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind)、[Control.UseSystemFocusVisuals property](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>運作方式
藉由在元素框線周圍以動畫方式呈現光暈，顯色焦點引起對取得焦點的元素：

![顯色視覺效果](images/traveling-focus-fullscreen-light-rf.gif)

這是使用者可能不會全神貫注於整個電視螢幕的 10 英呎案例中特別有用。 

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝的<strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，按一下這裡<a href="xamlcontrolsgallery:/item/RevealFocus">開啟應用程式</a>並查看顯色焦點情形。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>如何使用

顯色焦點預設為關閉。 若要啟用：
1. 在 App 的建構函式中，呼叫 [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) 屬性，並檢查目前的裝置系列是否為 `Windows.Xbox`。
2. 如果裝置系列是 `Windows.Xbox`，請將 [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) 屬性設定為 `FocusVisualKind.Reveal`。 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

設定**Reveal**屬性之後，系統會自動套用顯色焦點效果至其[UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)屬性設為**True** （大多數控制項的預設值） 的所有控制項。 

## <a name="why-isnt-reveal-focus-on-by-default"></a>為什麼不開啟顯色焦點預設？ 
如您所見，它是很容易就能開啟顯色焦點，當應用程式偵測到它在 Xbox 上執行。 因此，為什麼系統不直接為您開啟它？ 因為顯色焦點會增加焦點視覺效果的大小，可能導致 UI 配置發生問題。 在某些情況下，您會想要自訂顯色焦點效果，將它最佳化您的應用程式。

## <a name="customizing-reveal-focus"></a>自訂顯色焦點

您可以自訂顯色焦點效果，方法是修改每個控制項的焦點視覺效果屬性： [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)、 [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)、 [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush)，以及[FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)。 這些屬性可讓您自訂焦點矩形的色彩和粗細 (這些屬性是您用於建立[高可見度焦點視覺效果](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals)的相同屬性)。 

但在您開始構成會很有幫助多了一點構成顯色焦點的元件的相關。

有三個預設顯色焦點視覺效果部分︰ 主要框線、 次要框線和顯色光暈。 主要框線的粗細為 **2px**，圍繞在次要框線 *「外」*。 次要框線的粗細為 **1px**，圍繞在主要框線 *「內」*。 顯色焦點光暈粗細為主要框線粗細成比例，圍繞*以外*的主要框線。

除了靜態元素，顯色焦點視覺效果功能規律波動、 時依焦點方向移動焦點移動的動畫的光源。

![顯色焦點圖層](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>自訂框線粗細

若要變更控制項框線類型的粗細，請使用下列屬性：

| 框線類型 | 屬性 |
| --- | --- |
| 主要、光暈   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> (變更主要框線會依比例變更光暈的粗細)。   |
| 次要   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


此範例變更按鈕焦點視覺效果的框線粗細：

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>自訂邊界

邊界是控制項視覺界限與焦點視覺效果次要框線起始位置之間的空間。 預設邊界是距離控制項界限 1px。 您可以編輯個別控制項的這個邊界，方法是變更 [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin) 屬性︰

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

邊界為負值時，會將框線推離控制項的中心，邊界為正值時，則會將框線向控制項的中心靠攏。

## <a name="customize-the-color"></a>自訂色彩

若要變更的顯色焦點視覺效果色彩，使用[FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush)和[FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)屬性。

| 屬性 | 預設資源 | 預設資源值 |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

(當 **FocusVisualKind** 設定為 **Reveal** 時，FocusPrimaryBrush 屬性只會預設為 **SystemControlRevealFocusVisualBrush** 資源。 否則會使用 **SystemControlFocusVisualPrimaryBrush**)。


若要變更個別控制項的焦點視覺效果色彩，請直接設定控制項上的屬性。 此範例會覆寫按鈕的焦點視覺效果色彩。

```xaml

<!-- Specifying a color directly -->
<Button
    FocusVisualPrimaryBrush="DarkRed"
    FocusVisualSecondaryBrush="Pink"/>

<!-- Using theme resources -->
<Button
    FocusVisualPrimaryBrush="{ThemeResource SystemBaseHighColor}"
    FocusVisualSecondaryBrush="{ThemeResource SystemAccentColor}"/>    
```

若要變更整個 App 中所有焦點視覺效果的色彩，請使用您自己的定義來覆寫 **SystemControlRevealFocusVisualBrush** 和 **SystemControlFocusVisualSecondaryBrush** 資源：

```xaml
<!-- App.xaml -->
<Application.Resources>

    <!-- Override Reveal Focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

如需有關修改焦點視覺效果色彩的詳細資訊，請參閱[色彩商標和自訂](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#color-branding--customizing)。


## <a name="show-just-the-glow"></a>只顯示光暈

如果您只要使用不含主要或次要焦點視覺效果的光暈，只需將控制項的 [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) 屬性設定為 `Transparent`，並將 [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness) 設定為 `0`。 在這種情況下，光暈將會採用控制項背景的色彩來提供無框線風格。 您可以使用 [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness) 來修改光暈粗細。

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>使用您自己的焦點視覺效果

若要自訂顯色焦點的另一種方式是使用視覺狀態自行繪製選擇不使用系統提供的焦點視覺效果。 若要深入了解，請參閱[焦點視覺效果範例](http://go.microsoft.com/fwlink/p/?LinkID=619895)。


## <a name="reveal-focus-and-the-fluent-design-system"></a>顯色焦點和 Fluent 設計系統

顯色焦點是將光源加入您的應用程式中的 Fluent 設計系統元件。 若要深入了解 Fluent Design 系統及其他元件，請參閱[適用於 UWP 的 Fluent Design 概觀](../fluent-design-system/index.md)。

## <a name="related-articles"></a>相關文章

- [顯色醒目提示](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [針對 Xbox 和電視進行設計](/windows/uwp/design/devices/designing-for-tv)
- [遊戲台與遙控器的互動](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [焦點視覺效果範例](http://go.microsoft.com/fwlink/p/?LinkID=619895)
- [組合效果](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [系統中的科學：Fluent Design 和深度](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [系統中的科學：Fluent Design 和光源](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
