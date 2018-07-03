---
author: jwmsft
Description: Learn how Fluent motion uses timing and easing functions.
title: 計時和加/減速 - UWP app 中的動畫
label: Timing and easing
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 412ba7e36c2bb36562ceee13bb1e204ff402a882
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843765"
---
# <a name="timing-and-easing"></a>計時和加/減速

動作是根據現實世界，而我們也有數位媒體，伴隨對速度和效能的期望。 

## <a name="how-fluent-motion-uses-time"></a>Fluent 動作如何使用時間

計時是在 UI 內當物件進入、離開或移動時使動態感覺自然的一項重要元素。

1. 物件或場景輸入檢視快速但著名。 這些動畫通常持續時間比離開較長以階層式建構場景。
1. 物件或場景進入檢視非常快速。 使用者應該能夠了解 UI 發生的位置。 不過，一旦 UI 關閉，它應該會讓開。
1. 物件跨場景轉譯應該有適合其所移動距離的持續時間量。

## <a name="timing-in-fluent-motion"></a>Fluent 動作中的計時

在 Fluent 中動作的計時使用 500ms (或二分之一秒) 做為基準，因為這是使用者感知為瞬間的最長時間量。

![主角圖像](images/time.gif)

### <a name="150ms-exit"></a>**150ms** (離開)

::: 列:::::: 欄::: 用於離開場景或關閉的物件或頁面。
可讓非常快速的方向回饋離開 UI，而計時不會阻礙畫面播放速率以達到流暢的動畫。
:::欄結束::: :::欄::: ![150ms 動畫](images/150msAlt.gif) :::欄結束::: :::列結束:::

### <a name="300ms-enter"></a>**300ms** (進入)

::: 列:::::: 欄::: 用於進入場景或開啟的物件或頁面。
可在其進入場景時擁有合理的量的時間展示內容。
:::欄結束::: :::欄::: ![300ms 動畫](images/300ms.gif) :::欄結束::: :::列結束:::

### <a name="500ms-move"></a>**≤500ms** (移動)

::: 列:::::: 欄::: 用於跨單一場景或多個場景轉譯的物件。 :::欄結束::: :::欄::: ![500ms 動畫](images/500ms.gif) :::欄結束::: :::列結束:::

## <a name="easing-in-fluent-motion"></a>Fluent 動畫中的加/減速

加/減速是操作物件移動時的速度的一種方式。 它可緊密結合所有的 Fluent 動畫體驗。 雖然極端，系統中所使用的加/減速可協助統一物件在整個系統移動的實際感覺。 這是唯一模擬真實世界的途徑，並可讓動作中的物件感覺它們屬於其環境。

![主角圖像](images/easing.gif)

## <a name="apply-easing-to-motion"></a>將加/減速套用到動作

這些加/減速有助於您達到更自然的操作，並且也是我們用於 Fluent 動作的基準。

程式碼範例顯示如何套用建議的加/減速值到 Storyboard 動畫 (XAML) 或 Composition 動畫 (C#)。

### <a name="accelerate-exit"></a>**加速** (離開)

::: 列:::::: 欄::: 用於離開場景的 UI 或物件。

        Objects become powered and gain momentum until they reach escape velocity.
        The resulting feel is that the object is trying its hardest to get out of the user's way and make room for new content to come in.
    :::column-end:::
    :::column:::
        ![accelerate easing](images/accelEase.gif)
    :::column-end:::
:::列結束:::

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

::: 列:::::: 欄::: 用於進入場景的物件或 UI，或是瀏覽或繁衍。

        Once on-scene, the object is met with extreme friction, which slows the object to rest.
        The resulting feel is that the object traveled from a long distance away and entered at an extreme velocity, or is quickly returning to a rest state.

        Even if it's preceded by a moment of unresponsiveness, the velocity of the incoming object has the effect of feeling fast and responsive.
    :::column-end:::
    :::column:::
        ![decelerate easing](images/decelEase.gif)
    :::column-end:::
:::列結束:::

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

::: 列:::::: 欄::: 這是系統內任何動畫參數變更的基準加/減速。
請對使用畫面上狀態會改變的物件使用標準加/減速，例如簡單的位置變更。 此外，對場景中的物件變形使用，像是會長大的物件。

        The resulting feel is that objects changing state from A to B are overcoming, and taken over by, natural forces.
    :::column-end:::
    :::column:::
        ![standard easing](images/standardEase.gif)
    :::column-end:::
:::列結束:::

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

- [動作概觀](index.md)
- [方向性和重力](directionality-and-gravity.md)