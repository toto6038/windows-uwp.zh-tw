---
author: jwmsft
title: 時間動畫
description: 使用 KeyFrameAnimation 類別隨時間變更您的 UI。
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 動畫
ms.localizationpriority: medium
ms.openlocfilehash: bf6d3f16c7b240ca370c01a787fef09862f35863
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6982523"
---
# <a name="time-based-animations"></a>以時間為基礎的動畫

當元件進入或整體使用者體驗變更時，使用者通常會以兩種方式進行觀察︰隨時間或立即。 在 Windows 平台上，前者優於後者-立即經常變更的使用者體驗混淆並嚇到使用者，因為他們無法理解發生什麼事情。 接著使用者會以突兀和不自然的方式感知體驗。

因此，您可以隨時間慢慢變更 UI，引導使用者或通知他們關於體驗的變更。 在 Windows 平台上，做法是使用時間為基礎的動畫，也就是 KeyFrameAnimations。 KeyFrameAnimations 可讓您隨著時間變更 UI，以及控制動畫的每個層面，包括如何及何時開始，以及如何到達其結束狀態。 例如，用 300 毫秒時間以動畫效果將物件移到新的位置，會比立即「傳送」過去更令人容易接受。 使用動畫而不是瞬間變更，獲得的成果會是更令人容易接受以及吸引人的體驗。

## <a name="types-of-time-based-animations"></a>以時間為基礎的動畫類型

有兩種以時間為基礎的動畫，可用於在 Windows 上建置美觀的使用者體驗：

**明確動畫** – 正如其名，您明確地開始動畫以進行更新。
**隱含動畫** – 符合條件時，系統會代表您開始這些動畫。

對於本文，我們將討論如何搭配 KeyFrameAnimations 建立並使用_明確的_以時間為基礎的動畫。

對於明確和隱含的以時間為基礎動畫，有不同的類型，對應至可以動畫顯示的不同 CompositionObjects 屬性類型。

- ColorKeyFrameAnimation
- QuaternionKeyFrameAnimation
- ScalarKeyFrameAnimation
- Vector2KeyFrameAnimation
- Vector3KeyFrameAnimation
- Vector4KeyFrameAnimation

## <a name="create-time-based-animations-with-keyframeanimations"></a>使用 KeyFrameAnimations 建立以時間為基礎的動畫

在描述如何搭配 KeyFrameAnimations 建立明確的以時間為基礎的動畫之前，先讓我們說明幾個概念。

- KeyFrames – 這是動畫將以動畫顯示的個別「快照」。
  - 定義為索引鍵值組。 索引鍵代表介於 0 和 1 之間的進度，亦即這個「快照」發生在動畫存留期的位置。 另一個參數代表這個時候的屬性值。
- KeyFrameAnimation 屬性 – 可套用以符合 UI 需求的自訂選項。
  - DelayTime – 在呼叫 StartAnimation 之後、動畫開始之前的時間。
  - Duration – 動畫的持續時間。
  - IterationBehavior – 動畫的計數或無限重複行為。
  - IterationCount –「KeyFrame 動畫」將重複的有限次數。
  - KeyFrame Count – 特定 KeyFrame 動畫中的 KeyFrame 讀數。
  - StopBehavior – 指定呼叫 StopAnimation 時，動畫屬性值的行為。
  - Direction – 指定動畫播放的方向。
- 動畫群組 – 同時開始多個動畫。
  - 通常用於想同時以動畫顯示多個屬性時。

如需詳細資訊，請參閱 [CompositionAnimationGroup](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionanimationgroup)。

記住這些概念後，現在來談一談建構 KeyFrameAnimation 的一般公式：

1. 識別您需產生動畫效果的 CompositionObject 及其各別屬性。
1. 建立組合器的 KeyFrameAnimation 類型範本，以符合您想要產生動畫效果的屬性類型。
1. 使用動畫範本，開始新增 KeyFrames 並定義動畫的屬性。
    - 至少需要一個 KeyFrame (100% 或 1f KeyFrame)。
    - 建議也定義持續時間。
1. 準備好執行此動畫後，在 CompositionObject 上呼叫 StartAnimation(...)，以您想要產生動畫效果的屬性為目標。 具體而言：
    - `Visual.StartAnimation("targetProperty", CompositionAnimation animation);`
    - `Visual.StartAnimationGroup(AnimationGroup animationGroup);`
1. 如果您有執行中的動畫，而且您想要停止動畫或動畫群組，您可以使用這些 API：
    - `Visual.StopAnimation("targetProperty");`
    - `Visual.StopAnimationGroup(AnimationGroup AnimationGroup);`

讓我們看看一個範例，了解這個公式如何以動作呈現。

## <a name="example"></a>範例

在此範例中，您想要建立在 1 秒內從 <0,0,0> 位移至 <20,20,20> 的視覺效果動畫。 此外，您想以 10 倍查看這些位置之間的視覺動畫。

![主要畫面格動畫](images/animation/animated-rectangle.gif)

首先，識別您要產生動畫效果的 CompositionObject 及屬性。 在此例中，紅色正方形由名為 `redSquare` 的組合視覺效果代表。 從此物件開始您的動畫。

接下來，因為您想要為 Offset 屬性產生動畫效果，您必須建立 Vector3KeyFrameAnimation (Offset 是 Vector3 類型)。 您也可以定義 KeyFrameAnimation 的對應 KeyFrames。

```csharp
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
```

接著，我們將會定義 KeyFrameAnimation 的屬性來描述它的持續時間以及以 10 倍描述兩個位置 (目前和 <200,0,0>) 之間的動畫行為。

```csharp
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
```

最後，為了執行動畫，您需要在 CompositionObject 的屬性上開始。

```csharp
redVisual.StartAnimation("Offset.X", animation);
```

以下是完整程式碼。

```csharp
private void AnimateSquare(Compositor compositor, SpriteVisual redSquare)
{ 
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
    visual.StartAnimation("Offset.X", animation);
} 
```
