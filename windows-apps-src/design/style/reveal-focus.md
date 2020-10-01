---
description: 顯示焦點是一種光源效果，能在使用者將遊戲控制器或鍵盤的焦點移近可設定焦點的元素時，以動畫方式呈現這些元素的框線。
title: 顯示焦點
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 15ddbd46f2e4177b53701259feecd03d8306064b
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218111"
---
# <a name="reveal-focus"></a>顯示焦點

![主角圖像](images/header-reveal-focus.svg)

顯示焦點是適用於 [10 英呎體驗](../devices/designing-for-tv.md) (例如 Xbox One 和電視螢幕) 的光源效果。 它會在使用者將遊戲控制器或鍵盤的焦點移近可設定焦點的元素 (例如按鈕) 時，以動畫方式呈現這些元素的框線。 此效果預設會關閉，但也很容易啟用。 

(如需了解顯示顯目提示效果 (一種能醒目提示互動式元素的光源效果)，請參閱[顯示顯目提示文章](./reveal.md)。)


> **重要 API**：[Application.FocusVisualKind property](/uwp/api/windows.ui.xaml.application.FocusVisualKind) \(英文\)、[FocusVisualKind enum](/uwp/api/windows.ui.xaml.focusvisualkind) \(英文\)、[Control.UseSystemFocusVisuals property](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) \(英文\)

## <a name="how-it-works"></a>運作方式
顯示焦點會以動畫方式在元素框線周圍呈現光暈，讓使用者能注意到具有焦點的元素：

![顯示視覺效果](images/traveling-focus-fullscreen-light-rf.gif)

這在使用者可能無法注意到整個電視螢幕的 10 英呎案例中特別有用。 

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下此處以<a href="xamlcontrolsgallery:/item/RevealFocus">開啟應用程式並查看顯示焦點的運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>如何使用

系統預設會關閉顯示焦點。 若要啟用：
1. 在應用程式的建構函式中，呼叫 [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) 屬性，並檢查目前的裝置系列是否為 `Windows.Xbox`。
2. 如果裝置系列是 `Windows.Xbox`，請將 [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) 屬性設定為 `FocusVisualKind.Reveal`。 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

設定 **FocusVisualKind** 屬性後，系統會自動將顯示焦點效果套用至所有已將 [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) 屬性設定為 **True** (大多數控制項的預設值) 的控制項。 

## <a name="why-isnt-reveal-focus-on-by-default"></a>為什麼顯示焦點預設不會開啟？ 
如您所見，您可以很輕鬆地讓應用程式在 Xbox 上執行時自動開啟顯示焦點。 因此，為什麼系統不會直接為您開啟它？ 因為顯示焦點會增加焦點視覺效果的大小，這可能會導致 UI 版面配置上的問題。 在某些情況下，您會想要自訂顯色焦點效果，來針對您的應用程式進行最佳化。

## <a name="customizing-reveal-focus"></a>自訂顯示焦點

您可以透過修改每個控制項的焦點視覺效果屬性來自定顯示焦點效果：[FocusVisualPrimaryThickness](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness) \(英文\)、[FocusVisualSecondaryThickness](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness) \(英文\)、[FocusVisualPrimaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) \(英文\)，以及 [FocusVisualSecondaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) \(英文\)。 這些屬性可讓您自訂焦點矩形的色彩和粗細。 (這些屬性和您用來建立[高可見度焦點視覺效果](../input/guidelines-for-visualfeedback.md#high-visibility-focus-visuals) \(部分機器翻譯\) 的屬性是相同的)。 

但是在您開始進行自訂之前，先多了解一點構成顯示焦點的元件是件很有幫助的事。

預設的顯示焦點視覺效果有三個部分︰主要框線、次要框線和顯示光暈。 主要框線的粗細為 **2px**，圍繞在次要框線「外」  。 次要框線的粗細為 **1px**，圍繞在主要框線「內」  。 顯示焦點光暈的粗細會與主要框線的粗細成比例，並圍繞在主要框線「之外」  。

除了靜態元素之外，顯示焦點視覺效果還具有能在靜止時規律波動，並在移動焦點時依焦點方向移動的動畫光源。

![顯示焦點層](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>自訂框線粗細

若要變更控制項框線類型的粗細，請使用這些屬性：

| 框線類型 | 屬性 |
| --- | --- |
| 主要、光暈   | [FocusVisualPrimaryThickness](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness) \(英文\)<br/> (變更主要框線會依比例變更光暈的粗細。)   |
| 次要   | [FocusVisualSecondaryThickness](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness) \(英文\)   |


此範例會變更按鈕焦點視覺效果的框線粗細：

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>自訂邊界

邊界是控制項的視覺界限與焦點視覺效果次要框線的起始位置之間的空間。 預設邊界是位於控制項界限 1px 的距離之外。 您可以針對個別控制項編輯這個邊界，方法是變更 [FocusVisualMargin](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin) \(英文\) 屬性︰

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

邊界為負值時，會使框線遠離控制項的中心；邊界為正值時，則會使框線向控制項的中心靠攏。

## <a name="customize-the-color"></a>自訂色彩

若要變更的顯示焦點視覺效果的色彩，請使用 [FocusVisualPrimaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) \(英文\) 和 [FocusVisualSecondaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) \(英文\) 屬性。

| 屬性 | 預設資源 | 預設資源值 |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) \(英文\) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) \(英文\)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

(FocusPrimaryBrush 屬性只會在 **FocusVisualKind** 被設定為 **Reveal** 時，才會預設至 **SystemControlRevealFocusVisualBrush** 資源。 否則，它會使用 **SystemControlFocusVisualPrimaryBrush**。)


若要變更個別控制項的焦點視覺效果色彩，請直接設定控制項上的屬性。 此範例會覆寫某個按鈕的焦點視覺效果色彩。

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

若要變更整個應用程式中所有焦點視覺效果的色彩，請使用您自己的定義來覆寫 **SystemControlRevealFocusVisualBrush** 和 **SystemControlFocusVisualSecondaryBrush** 資源：

```xaml
<!-- App.xaml -->
<Application.Resources>

    <!-- Override Reveal Focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

如需有關修改焦點視覺效果色彩的詳細資訊，請參閱[色彩商標和自訂](../input/guidelines-for-visualfeedback.md#color-branding--customizing) \(部分機器翻譯\)。


## <a name="show-just-the-glow"></a>只顯示光暈

如果您只想要使用光暈，而不使用主要或次要焦點視覺效果，只需將控制項的 [FocusVisualPrimaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) \(英文\) 屬性設定為 `Transparent`，並將 [FocusVisualSecondaryThickness](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness) \(英文\) 設定為 `0`。 在這種情況下，光暈將會採用控制項背景的色彩來提供無框線風格。 您可以使用 [FocusVisualPrimaryThickness](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness) \(英文\) 來修改光暈粗細。

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>使用您自己的焦點視覺效果

另一個自訂顯示焦點的方法，是使用視覺效果狀態自行繪製焦點視覺效果，而不使用系統提供的焦點視覺效果。 若要深入了解，請參閱[焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) \(英文\)。


## <a name="reveal-focus-and-the-fluent-design-system"></a>顯示焦點和 Fluent Design 系統

顯示焦點是能將光源加入應用程式中的 Fluent Design 系統元件。 若要深入了解 Fluent Design 系統及該系統的其他元件，請參閱 [Fluent Design 概觀](/windows/apps/fluent-design-system)。

## <a name="related-articles"></a>相關文章

- [顯示顯目提示](./reveal.md) \(部分機器翻譯\)
- [針對 Xbox 和電視進行設計](../devices/designing-for-tv.md)
- [遊戲控制器與遙控器的互動](../input/gamepad-and-remote-interactions.md) \(部分機器翻譯\)
- [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) \(英文\)
- [組合效果](../../composition/composition-effects.md) \(部分機器翻譯\)
- [系統中的科學：Fluent Design 和深度](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f) \(英文\)
- [系統中的科學：Fluent Design 和光線](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f) \(英文\)
