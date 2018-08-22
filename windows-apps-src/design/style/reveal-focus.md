---
author: cphilippona
description: 公開會議狀態 」 是當使用者會控制板或鍵盤焦點移至其動畫顯示可設定焦點元素的框線的光源效果。
title: 顯示焦點
template: detail.hbs
ms.author: mijacobs
ms.date: 03/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 7b5fa84efbe20368be55a50ce20c8e6e5d1fe439
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "2787277"
---
# <a name="reveal-focus"></a>顯示焦點

![主角圖像](images/header-reveal-focus.svg)

公開會議狀態 」 是[10 英呎經驗](/windows/uwp/design/devices/designing-for-tv)，例如 Xbox 一個和電視畫面的光源效果。 這會在使用者將遊戲台或鍵盤焦點移近可設定焦點元素 (例如按鈕) 時，以動畫方式呈現這些元素的框線。 預設會關閉此效果，但要啟用，也很簡單 

（如顯示醒目提示的效果，醒目提示互動式元素光源影響請參閱[顯示反白顯示本文](/windows/uwp/design/style/reveal)）。


> **重要 API**：[Application.FocusVisualKind property](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind)、[FocusVisualKind enum](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind)、[Control.UseSystemFocusVisuals property](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>運作方式
所新增之元素的框線周圍動畫的光暈顯示通話注意焦點移至具有焦點的項目：

![顯色視覺效果](images/traveling-focus-fullscreen-light-rf.gif)

這是在其中使用者可能不注意完整整個 TV 螢幕 10 英呎案例中尤其實用。 

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您有安裝的<strong style="font-weight: semi-bold">XAML 控制項圖庫</strong>應用程式，請按一下這裡以<a href="xamlcontrolsgallery:/item/RevealFocus">開啟該應用程式並查看巨集指令中的 [顯示焦點</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>如何使用

顯示焦點預設為關閉。 若要啟用：
1. 在 App 的建構函式中，呼叫 [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) 屬性，並檢查目前的裝置系列是否為 `Windows.Xbox`。
2. 如果裝置系列是 `Windows.Xbox`，請將 [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) 屬性設定為 `FocusVisualKind.Reveal`。 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

您可以設定**FocusVisualKind**屬性後，系統會自動顯示焦點之效果套其[UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)屬性設為**True** （最多控制項的預設值） 的所有控制項。 

## <a name="why-isnt-reveal-focus-on-by-default"></a>為什麼要選擇不顯示焦點上是預設？ 
如您所見，它會開啟顯示焦點時應用程式偵測到執行於 Xbox 簡單。 因此，為什麼系統不直接為您開啟它？ 顯示焦點會增加視覺焦點的大小，因為這可能會造成問題具有 UI 版面配置。 在某些情況下，您將要自訂最佳化您的應用程式的顯示焦點效果。

## <a name="customizing-reveal-focus"></a>自訂 Reveal 焦點

您可以修改每個控制項的焦點視覺屬性的自訂顯示焦點效果： [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)、 [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)、 [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush)及[FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)。 這些屬性可讓您自訂焦點矩形的色彩和粗細 (這些屬性是您用於建立[高可見度焦點視覺效果](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals)的相同屬性)。 

開始自訂之前，但它，相當有用更多有關了解組成顯示焦點的元件。

有三個組件至預設顯示焦點視覺效果： 主要框線、 次要框線及光暈 Reveal。 主要框線的粗細為 **2px**，圍繞在次要框線 *「外」*。 次要框線的粗細為 **1px**，圍繞在主要框線 *「內」*。 顯示焦點光暈粗細為主要框線的粗細調和間距且執行*外*的主要的框線周圍。

除了靜態元素中，顯示焦點視覺效果功能時在放 pulsates 且時移動焦點的焦點方向的移動動畫效果的光源。

![顯示焦點層級](images/reveal-breakdown.svg)

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

若要變更顯示焦點視覺化的色彩，使用[FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush)和[FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)屬性。

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

若要選擇退出系統提供焦點視覺效果的繪圖您自己使用視覺狀態是另一種方式來自訂顯示焦點。 若要深入了解，請參閱[焦點視覺效果範例](http://go.microsoft.com/fwlink/p/?LinkID=619895)。


## <a name="reveal-focus-and-the-fluent-design-system"></a>顯示焦點和 Fluent 設計系統

公開會議狀態 」 是新增至您的應用程式的淺色 Fluent 設計系統元件。 若要深入了解 Fluent Design 系統及其他元件，請參閱[適用於 UWP 的 Fluent Design 概觀](../fluent-design-system/index.md)。

## <a name="related-articles"></a>相關文章

- [顯示醒目提示](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [針對 Xbox 和電視進行設計](/windows/uwp/design/devices/designing-for-tv)
- [遊戲台與遙控器的互動](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [焦點視覺效果範例](http://go.microsoft.com/fwlink/p/?LinkID=619895)
- [組合效果](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [系統中的科學：Fluent Design 和深度](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [系統中的科學：Fluent Design 和光源](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
