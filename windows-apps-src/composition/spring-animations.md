---
author: jwmsft
title: 彈簧動畫
description: 了解如何使用彈簧自然動作動畫。
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10、uwp、動畫
ms.localizationpriority: medium
ms.openlocfilehash: 2b28653fc7746075c57f862b0c885beac6d4934f
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7436634"
---
# <a name="spring-animations"></a>彈簧動畫

本文示範如何使用彈簧 NaturalMotionAnimations。

## <a name="prerequisites"></a>必要條件

我們此處假設您已熟悉這些文章中討論的概念：

- [自然運動動畫](natural-animations.md)

## <a name="why-springs"></a>為何使用彈簧？

我們在生活中經常可以遇到彈簧運動，從彈簧玩具到物理教室的彈簧掛鉤塊。 彈簧的擺動通常讓人產生俏皮和輕鬆的情緒反應。 因此，彈簧的動作可在應用程式 UI 中創造出比傳統三次方貝茲更輕快的「彈出」感。 在這些案例中，彈簧動作不僅建立更生動的運動體驗，也有助於吸引使用者注意新的或目前的動畫內容。 取決於應用程式品牌或動作語言，有時候擺動更明顯可見，有時候更隱晦點。

![使用彈簧動畫的動作](images/animation/offset-spring.gif)
![使用三次方貝茲動畫的動作](images/animation/offset-cubic-bezier.gif)

## <a name="using-springs-in-your-ui"></a>在您的 UI 中使用彈簧

如前所述，彈簧可以整合到您的應用程式中，來引進非常熟悉且充滿趣味的 UI 體驗。 UI 中的常見彈簧用途如下：

| 彈簧用途描述 | 視覺範例 |
| ------------------------ | -------------- |
| 讓動作體驗「彈出」且更生動。 (動畫縮放) | ![使用彈簧動畫的縮放動作](images/animation/scale-spring.gif) |
| 讓動作體驗稍微感到更有活力 (動畫位移) | ![使用彈簧動畫的位移動作](images/animation/offset-spring.gif) |

在這些案例中，彈簧動作可由「彈出」和圍繞新值擺動，或以一定的初始速度圍繞目前值擺動來觸發。

![彈簧動畫擺動](images/animation/spring-animation-diagram.png)

## <a name="defining-your-spring-motion"></a>定義您的彈簧動作

使用 NaturalMotionAnimation API 建立彈簧體驗。 具體而言，您可以使用 Compositor 的 Create* 方法來建立 SpringNaturalMotionAnimation。 然後，您就可以定義動作的下列屬性：

- DampingRatio – 表達動畫中使用的彈簧動作的阻尼程度。

| 阻尼比值 | 描述 |
| ------------------- | ----------- |
| DampingRatio = 0 | 無阻尼 - 彈簧會擺動很長時間 |
| 0 < DampingRatio < 1 | 阻尼不足 – 彈簧從一點點到擺動很多。 |
| DampingRatio = 1 | 臨界阻尼 - 彈簧不會擺動。 |
| DampingRation > 1 | 過阻尼 - 彈簧會突然減速以快速到達目的地，無擺動 |

- Period – 彈簧執行單一擺動所花費的時間。
- Final / Starting Value – 定義彈簧動作的開始和結束位置 (若未定義，開始值和/或最終值將是目前的值)。
- Initial Velocity – 動作的程式設計初始速度。

您也可以定義一組與 KeyFrameAnimations 相同的動作屬性：

- DelayTime / 延遲行為
- StopBehavior

在動畫 Offset 和 Scale/Size 的常見案例中，Windows 設計團隊建議對不同彈簧類型的 DampingRatio 和 Period 使用下列值：

| 屬性 | 一般彈簧 | 阻尼彈簧 | 低阻尼彈簧 |
| -------- | ------------- | --------------- | -------------------- |
| Offset | Damping Ratio = 0.8 <br/> Period = 50 ms | Damping Ratio = 0.85 <br/> Period = 50 ms | Damping Ratio = 0.65 <br/> Period = 60 ms |
| Scale/Size | Damping Ratio = 0.7 <br/> Period = 50 ms | Damping Ratio = 0.8 <br/> Period = 50 ms | Damping Ratio = 0.6 <br/> Period = 60 ms |

定義好屬性後，接著就可以傳遞彈簧 NaturalMotionAnimation 到 CompositionObject 的 StartAnimation 方法或 InteractionTracker InertiaModifier 的 Motion 屬性。

## <a name="example"></a>範例

在此範例中，您將建立瀏覽和畫布 UI 體驗，當使用者按下展開按鈕，瀏覽窗格會以動畫方式出現，搭配輕快的擺動動作。

![點擊的彈簧動畫](images/animation/spring-animation-on-click.gif)

一開始，先為瀏覽窗格出現時機，定義按下事件中的彈簧動畫。 接著定義動畫屬性，使用 InitialValueExpression 功能來使用 Expression 定義 FinalValue。 您也可以追蹤窗格是否開啟，以及在準備好時開始動畫。

```csharp
private void Button_Clicked(object sender, RoutedEventArgs e)
{
 _springAnimation = _compositor.CreateSpringScalarAnimation();
 _springAnimation.DampingRatio = 0.75f;
 _springAnimation.Period = TimeSpan.FromSeconds(0.5);

 if (!_expanded)
 {
 _expanded = true;
 _propSet.InsertBoolean("expanded", true);
 _springAnimation.InitialValueExpression[“FinalValue”] = “this.StartingValue + 250”;
 } else
 {
 _expanded = false;
 _propSet.InsertBoolean("expanded", false);
_springAnimation.InitialValueExpression[“FinalValue”] = “this.StartingValue - 250”;
 }
 _naviPane.StartAnimation("Offset.X", _springAnimation);
}
```

現在，如果您想將此動作繫結至輸入呢？ 如果使用者向外撥動，窗格就會以彈簧動作出現？ 更重要的是，如果使用者更用力或更快撥動，動作會根據使用者的速度而調整。

![撥動的彈簧動畫](images/animation/spring-animation-on-swipe.gif)

若要這樣做，您可以採用我們的相同彈簧動畫並使用 InteractionTracker 將它傳遞至 InertiaModifier。 如需 InputAnimations InteractionTracker 的詳細資訊，請參閱[使用 InteractionTracker 自訂操作體驗](interaction-tracker-manipulations.md)。 對於此程式碼範例，我們會假設您已設定好 InteractionTracker 和 VisualInteractionSource。 我們會著重在建立將用於 NaturalMotionAnimation 的 InertiaModifiers，在本例中為彈簧。

```csharp
// InteractionTracker and the VisualInteractionSource previously setup
// The open and close ScalarSpringAnimations defined earlier
private void SetupInput()
{
 // Define the InertiaModifier to manage the open motion
 var openMotionModifer = InteractionTrackerInertiaNaturalMotion.Create(compositor);

 // Condition defines to use open animation if panes in non-expanded view
 // Property set value to track if open or closed is managed in other part of code
 openMotionModifer.Condition = _compositor.CreateExpressionAnimation(
"propset.expanded == false");
 openMotionModifer.Condition.SetReferenceParameter("propSet", _propSet);
 openMotionModifer.NaturalMotion = _openSpringAnimation;

 // Define the InertiaModifer to manage the close motion
 var closeMotionModifier = InteractionTrackerInertiaNaturalMotion.Create(_compositor);

 // Condition defines to use close animation if panes in expanded view
 // Property set value to track if open or closed is managed in other part of code
 closeMotionModifier.Condition = 
_compositor.CreateExpressionAnimation("propSet.expanded == true");
 closeMotionModifier.Condition.SetReferenceParameter("propSet", _propSet);
 closeMotionModifier.NaturalMotion = _closeSpringAnimation;

 _tracker.ConfigurePositionXInertiaModifiers(new 
InteractionTrackerInertiaNaturalMotion[] { openMotionModifer, closeMotionModifier});

 // Take output of InteractionTracker and assign to the pane
 var exp = _compositor.CreateExpressionAnimation("-tracker.Position.X");
 exp.SetReferenceParameter("tracker", _tracker);
 ElementCompositionPreview.GetElementVisual(pageNavigation).
StartAnimation("Translation.X", exp);
}
```

現在您的 UI 中同時具備程式設計和輸入導向的彈簧動畫！

總結來說，在您的應用程式中使用彈簧動畫的步驟如下：

1. 從 Compositor 建立 SpringAnimation。
1. 如果您想使用非預設值，請定義 SpringAnimation 的屬性︰
    - DampingRatio
    - Period
    - Final Value
    - Initial Value
    - Initial Velocity
1. 指派給目標。
    - 如果您要讓 CompositionObject 屬性產生動畫效果，請以參數傳遞 SpringAnimation 給 StartAnimation。
    - 如果您想要用於輸入，請將 InertiaModifier 的 NaturalMotion 屬性設為 SpringAnimation。

