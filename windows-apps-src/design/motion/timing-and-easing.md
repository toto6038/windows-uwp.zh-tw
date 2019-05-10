---
Description: 了解如何 Fluent 動作使用的預存時間和 easing 函式。
title: 計時和加/減速 - UWP app 中的動畫
label: Timing and easing
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b736a10a7284e3cc9aa193e082dc654e908afe40
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444178"
---
# <a name="timing-and-easing"></a>計時和加/減速

動作是根據現實世界，而我們也有數位媒體，伴隨對速度和效能的期望。

## <a name="examples"></a>範例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果您有<strong style="font-weight: semi-bold">XAML 控制項陳列庫</strong>應用程式安裝，請按一下這裡可<a href="xamlcontrolsgallery:/item/EasingFunction">開啟 應用程式，並查看作用中的加/減速函式</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-fluent-motion-uses-time"></a>Fluent 動作如何使用時間

計時是在 UI 內當物件進入、離開或移動時使動態感覺自然的一項重要元素。

1. 物件或場景輸入檢視快速但著名。 這些動畫通常持續時間比離開較長以階層式建構場景。
1. 物件或場景進入檢視非常快速。 使用者應該能夠了解 UI 發生的位置。 不過，一旦 UI 關閉，它應該會讓開。
1. 物件跨場景轉譯應該有適合其所移動距離的持續時間量。

## <a name="timing-in-fluent-motion"></a>Fluent 動作中的計時

在 Fluent 中動作的計時使用 500ms (或二分之一秒) 做為基準，因為這是使用者感知為瞬間的最長時間量。

![主角圖像](images/time.gif)

### <a name="150ms-exit"></a>**150ms** (離開)

:::row:::
    :::column:::
用於物件或場景的結束或關閉頁面。
可讓非常快速的方向回饋離開 UI，而計時不會阻礙畫面播放速率以達到流暢的動畫。
    :::column-end:::
    :::column:::
        ![150ms motion](images/150msAlt.gif)
    :::column-end:::
:::row-end:::

### <a name="300ms-enter"></a>**300ms** (進入)

:::row:::
    :::column:::
物件或輸入場景或開啟的頁面中使用。
可在其進入場景時擁有合理的量的時間展示內容。
    :::column-end:::
    :::column:::
        ![300ms motion](images/300ms.gif)
    :::column-end:::
:::row-end:::

### <a name="500ms-move"></a>**≤500ms** (移動)

:::row:::
    :::column:::
使用的轉譯場景是單一或多個場景物件。 
    :::column-end:::
    :::column:::
        ![500ms motion](images/500ms.gif)
    :::column-end:::
:::row-end:::

## <a name="easing-in-fluent-motion"></a>Fluent 動畫中的加/減速

加/減速是操作物件移動時的速度的一種方式。 它可緊密結合所有的 Fluent 動畫體驗。 雖然極端，系統中所使用的加/減速可協助統一物件在整個系統移動的實際感覺。 這是唯一模擬真實世界的途徑，並可讓動作中的物件感覺它們屬於其環境。

![主角圖像](images/easing.gif)

## <a name="apply-easing-to-motion"></a>將加/減速套用到動作

這些加/減速有助於您達到更自然的操作，並且也是我們用於 Fluent 動作的基準。

程式碼範例顯示如何套用建議的加/減速值到 Storyboard 動畫 (XAML) 或 Composition 動畫 (C#)。

### <a name="accelerate-exit"></a>**加速** (離開)

:::row:::
    :::column:::
使用 UI 或即將結束場景的物件。

物件會提供，並取得趨勢電子報，直到達到逸出的速度。
產生的操作是物件會嘗試利用使用者的方式並騰出空間給新的內容傳入到其最困難。
    :::column-end:::
    :::column:::
        ![accelerate easing](images/accelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.7 , 0 , 1 , 0.5)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.15">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="4.5" EasingMode="EaseIn" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction accelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.7f, 0.0f), new Vector2(1.0f, 0.5f));
_exitAnimation = _compositor.CreateScalarKeyFrameAnimation();
_exitAnimation.InsertKeyFrame(0.0f, _startValue);
_exitAnimation.InsertKeyFrame(1.0f, _endValue, accelerate);
_exitAnimation.Duration = TimeSpan.FromMilliseconds(150);
```

### <a name="decelerate-enter"></a>**減速** (進入)

:::row:::
    :::column:::
物件或 UI 輸入場景，請瀏覽，或產生中使用。

一旦在場景，滿足物件，具有極大的阻力，會變慢的其餘部分的物件。
產生的操作是物件經過長時間距離離開和在極端的速度，輸入或已快速回到其餘狀態。

即使它前面的無回應，連入物件的速度就會有的感覺快速且回應迅速的效果。
    :::column-end:::
    :::column:::
        ![decelerate easing](images/decelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.1 , 0.9 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.3">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="7" EasingMode="EaseOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction decelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.1f, 0.9f), new Vector2(0.2f, 1.0f));
_enterAnimation = _compositor.CreateScalarKeyFrameAnimation();
_enterAnimation.InsertKeyFrame(0.0f, _startValue);
_enterAnimation.InsertKeyFrame(1.0f, _endValue, decelerate);
_enterAnimation.Duration = TimeSpan.FromMilliseconds(300);
```

### <a name="standard-easing-move"></a>**標準加/減速** (移動)

:::row:::
    :::column:::
這是在系統內任何動畫的參數變更加/減速的基準。
請對使用畫面上狀態會改變的物件使用標準加/減速，例如簡單的位置變更。 此外，對場景中的物件變形使用，像是會長大的物件。

物件變更狀態從 A，b 克服，，和透過所採取，自然會強制產生的操作。
    :::column-end:::
    :::column:::
        ![standard easing](images/standardEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.8 , 0 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.5">
        <DoubleAnimation.EasingFunction>
            <CircleEase EasingMode="EaseInOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction standard =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.8f, 0.0f), new Vector2(0.2f, 1.0f));
 _moveAnimation = _compositor.CreateScalarKeyFrameAnimation();
 _moveAnimation.InsertKeyFrame(0.0f, _startValue);
 _moveAnimation.InsertKeyFrame(1.0f, _endValue, standard);
 _moveAnimation.Duration = TimeSpan.FromMilliseconds(500);
```

## <a name="related-articles"></a>相關文章

- [影片概觀](index.md)
- [方向和重力](directionality-and-gravity.md)
