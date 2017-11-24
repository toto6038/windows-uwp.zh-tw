---
author: jwmsft
title: "使用慣性修飾詞建立貼齊點"
description: 
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 動畫"
localizationpriority: medium
ms.openlocfilehash: c2d5bf4d11679c0f30969d541a9c985385538ec7
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2017
---
# <a name="create-snap-points-with-inertia-modifiers"></a>使用慣性修飾詞建立貼齊點

在本文中，我們將深入探討如何使用 InteractionTracker 的 InertiaModifier 功能來建立貼齊至指定點的動作體驗。

## <a name="prerequisites"></a>必要條件

我們此處假設您已熟悉這些文章中討論的概念：

- [輸入導向動畫](input-driven-animations.md)
- [使用 InteractionTracker 自訂操作體驗](interaction-tracker-manipulations.md)
- [關聯式動畫](relation-animations.md)

## <a name="what-are-snap-points-and-why-are-they-useful"></a>貼齊點是什麼？為什麼很實用？

建置自訂操作體驗時，在 InteractionTracker 一律會停靠的可捲動/可縮放畫布中建立專用_定位點_，有時候很有用。 它們通常稱為_「貼齊點」_。

請注意下列範例中，捲動如何將 UI 停留在不同影像之間的怪異位置：

![未使用貼齊點的捲動](images/animation/snap-points-none.gif)

如果新增貼齊點，當您在影像之間停止捲動，它們會「貼齊」指定的位置。 使用貼齊點，可讓影像間的捲動體驗更加俐落、更具回應性。

![使用單一貼齊點的捲動](images/animation/snap-points-single.gif)

## <a name="interactiontracker-and-inertiamodifiers"></a>InteractionTracker 和 InertiaModifiers

使用 InteractionTracker 建置自訂操作體驗時，您可以利用 InertiaModifiers 建立貼齊點動作體驗。 InertiaModifiers 基本上是定義 InteractionTracker 進入慣性狀態時，到達目的地的位置和方式。 您可以套用 InertiaModifiers 來影響 InteractionTracker 的 X 或 Y 位置或縮放屬性。

InertiaModifiers 有 3 種類型：

- InteractionTrackerInertiaRestingValue – 用來修改互動或程式設計速度之後的最終停留位置。 預先定義的動作會將 InteractionTracker 帶到該位置。
- InteractionTrackerInertiaMotion – 用來定義 InteractionTracker 在互動或程式設計速度之後將執行的特定動作。 最終位置衍生自這個動作。
- InteractionTrackerInertiaNaturalMotion – 定義互動或程式設計速度之後的最終停留位置，但使用基於物理的動畫 (NaturalMotionAnimation)。

進入慣性後，InteractionTracker 會評估指派給它的每個 InertiaModifiers，並判斷其中任一個是否適用。 這表示您可以建立並指派多個 InertiaModifiers 給 InteractionTracker，但是在定義每個項目時，您需要執行下列動作：

1. 定義條件 – 定義應執行此特定 InertiaModifier 之條件式陳述式的運算式。 這通常需要查看 InteractionTracker 的 NaturalRestingPosition (提供預設慣性下的目的地)。
1. 定義 RestingValue/Motion/NaturalMotion – 定義符合條件時執行的 Resting Value Expression、Motion Expression 或 NaturalMotionAnimation。

> [!NOTE]
> 當 InteractionTracker 進入慣性之後，才會評估 InertiaModifiers 的條件層面。 不過，僅限 InertiaMotion，修飾詞的條件為 true 的每個畫面都會評估動作運算式。

## <a name="example"></a>範例

現在讓我們來看看如何使用 InertiaModifiers 建立一些貼齊點體驗，以重新建立影像的捲動畫布。 在此範例中，每一項操作都是為了可能移動穿越單一影像 – 這通常稱為「單一強制貼齊點」。

一開始是設定 InteractionTracker、VisualInteractionSource 和 Expression 來運用 InteractionTracker 的位置。

```csharp
private void SetupInput()
{
    _tracker = InteractionTracker.Create(_compositor);
    _tracker.MinPosition = new Vector3(0f);
    _tracker.MaxPosition = new Vector3(3000f);

    _source = VisualInteractionSource.Create(_root);
    _source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    _source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
    _tracker.InteractionSources.Add(_source);

    var scrollExp = _compositor.CreateExpressionAnimation("-tracker.Position.Y");
    scrollExp.SetReferenceParameter("tracker", _tracker);
    ElementCompositionPreview.GetElementVisual(scrollPanel).StartAnimation("Offset.Y", scrollExp);
}
```

接下來，因為單一強制貼齊點行為會向上或向下移動內容，您將需要兩個不同的慣性修飾詞：一個將可捲動內容向上移動，一個將它向下移動。

```csharp
// Snap-Point to move the content up
var snapUpModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
// Snap-Point to move the content down
var snapDownModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
```

至於向上還是向下貼齊，取決於 InteractionTracker 相對於貼齊距離的自然著陸位置 – 也就是貼齊位置之間的距離。 如果超過中間點，就向下貼齊，否則向上貼齊。 (在此範例中，您將貼齊距離儲存在 PropertySet 中)

```csharp
// Is NaturalRestingPosition less than the halfway point between Snap Points?
snapUpModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y < (this.StartingValue – ” + “mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapUpModifier.Condition.SetReferenceParameter("prop", _propSet);
// Is NaturalRestingPosition greater than the halfway point between Snap Points?
snapDownModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y >= (this.StartingValue – ” + “mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapDownModifier.Condition.SetReferenceParameter("prop", _propSet);
```

這個圖表提供正在發生之邏輯的視覺描述：

![慣性修飾詞圖表](images/animation/inertia-modifier-diagram.png)

現在，您只需要為每個 InertiaModifier 定義 Resting Values：將 InteractionTracker 的位置移往上一個貼齊位置或是下一個。

```csharp
snapUpModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue - mod(this.StartingValue, prop.snapDistance)");
snapUpModifier.RestingValue.SetReferenceParameter("prop", _propSet);
snapForwardModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue + prop.snapDistance - mod(this.StartingValue, ” + “prop.snapDistance)");
snapForwardModifier.RestingValue.SetReferenceParameter("prop", _propSet);
```

最後，將 InertiaModifiers 加入 InteractionTracker。 現在，當 InteractionTracker 進入 InertiaState 時，就會檢查您 InertiaModifiers 的條件，查看是否應該修改它的位置。

```csharp
var modifiers = new InteractionTrackerInertiaRestingValue[] { 
snapUpModifier, snapDownModifier };
_tracker.ConfigurePositionYInertiaModifiers(modifiers);
```