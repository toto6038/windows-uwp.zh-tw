---
description: 瞭解在 UI 中進入、結束或移動物件時，讓運動感到自然的重要性和簡化。
title: 計時和加/減速
label: Timing and easing
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 46ec373ffb7ad141bcff36c33fa35900c888daf8
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054428"
---
# <a name="timing-and-easing"></a>計時和加/減速

動作是根據現實世界，而我們也有數位媒體，伴隨對速度和效能的期望。

## <a name="examples"></a>範例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項資源庫</strong> 應用程式，請按一下這裡 <a href="xamlcontrolsgallery:/item/EasingFunction">開啟應用程式，並查看緩和功能的</a>作用。</p>
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
用於離開場景或關閉的物件或頁面。
可讓非常快速的方向回饋離開 UI，而計時不會阻礙畫面播放速率以達到流暢的動畫。
    :::column-end:::
    :::column:::
        ![150ms 動作](images/150msAlt.gif)
    :::column-end:::
:::row-end:::

### <a name="300ms-enter"></a>**300ms** (進入)

:::row:::
    :::column:::
用於進入場景或開啟的物件或頁面。
可在其進入場景時擁有合理的量的時間展示內容。
    :::column-end:::
    :::column:::
        ![300ms 動作](images/300ms.gif)
    :::column-end:::
:::row-end:::

### <a name="500ms-move"></a>**≤500ms** (移動)

:::row:::
    :::column:::
針對在單一場景或多個場景之間轉譯的物件使用。 
    :::column-end:::
    :::column:::
        ![500毫秒動作](images/500ms.gif)
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
用於即將結束場景的 UI 或物件。

物件會變成有動力並獲得動力，直到達到 escape 速度為止。
產生的結果是，物件正在嘗試使其無法使用使用者的方式，並騰出空間給新的內容。
    :::column-end:::
    :::column:::
        ![加速簡化](images/accelEase.gif)
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
針對輸入場景的物件或 UI 使用，不論是流覽或產生。

一旦進入場景之後，就會達到物件的極端干擾，使物件變慢。
產生的結果是，物件從較遠的距離移離並進入極遠的速度，或是很快地回到 rest 狀態。

即使它之前有點無回應，連入物件的速度也具有感覺迅速和回應的效果。
    :::column-end:::
    :::column:::
        ![減速的簡化](images/decelEase.gif)
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
這是系統內任何動畫參數變更的基準簡化。
請對使用畫面上狀態會改變的物件使用標準加/減速，例如簡單的位置變更。 此外，對場景中的物件變形使用，像是會長大的物件。

產生的結果是，將狀態從 A 變更為 B 的物件會克服，並透過自然的強制執行。
    :::column-end:::
    :::column:::
        ![標準簡化](images/standardEase.gif)
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

- [動作概觀](index.md)
- [方向性和重力](directionality-and-gravity.md)
