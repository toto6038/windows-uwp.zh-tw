---
author: jwmsft
title: 自然運動動畫
description: 深入了解自然動作動畫以及如何在您的應用程式 UI 中使用之。
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 動畫
ms.localizationpriority: medium
ms.openlocfilehash: 537e722917f00d590428dd2b5ee2d24e023e52b6
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "5753453"
---
# <a name="natural-motion-animations"></a>自然運動動畫

本文簡要概述 NaturalMotionAnimation 空間，以及如何概念性思考在 UI 中使用這些動畫類型。

## <a name="making-motion-feel-familiar-and-natural"></a>讓動作感覺熟悉且自然

出色的應用程式會建立吸引使用者注意的體驗，並協助引導使用者完成工作。 動作是區別使用者介面和使用者體驗的關鍵差異化因素 – 在使用者與其互動的應用程式之間引發連結。 連結得越好，使用者的參與程度和滿意度都越高。

動作可協助建置這種連結的其中一種方式是建立使用者熟悉的外觀和感受。 使用者在無意識中會預期收到以實際生活體驗為基礎的動作。 我們觀察物體如何滑過地板、從桌子上掉下來、互相反彈以及使用彈簧擺盪。 運用真實世界物理原理的動作，我們看起來和感覺起來都會更自然。 動作會變得更加自然且互動性更高，但更重要的是，整個體驗會變得更令人印象深刻且容易使用。

![不使用動畫的縮放動作](images/animation/scale-no-animation.gif)
![使用三次方貝茲的縮放動作](images/animation/scale-cubic-bezier.gif)
![使用彈簧動畫的縮放動作](images/animation/scale-spring.gif)

獲得的成果就是使用者參與率以及應用程式保留率提高。

## <a name="balancing-control-and-dynamism"></a>在控制程度和動態之間取得平衡

在傳統 UI 中，[KeyFrameAnimation](https://docs.microsoft.com/uwp/api/windows.ui.composition.keyframeanimation)s 是描述動作的主流方式。 KeyFrames 可提供最高的控制程度給設計人員和開發人員來定義開始、結尾和插補。 雖然這在許多情況下很有幫助，但 KeyFrame 動畫不夠動態；動作不會自動調整，在任何條件下看起來都相同。

而在光譜的另一端，常可在遊戲與物理引擎中看到模擬。 這些通常是使用者可與之互動的最栩栩如生體驗 – 創造出使用者每天可以看到的氛圍和隨意感覺。 雖然這能讓動作感覺更生動活潑，但會讓設計人員和開發人員感覺控制程度變低，因而更難以整合到傳統 UI。

![控制範圍圖表](images/animation/natural-motion-diagram.png)

[NaturalMotionAnimation](https://docs.microsoft.com/uwp/api/windows.ui.composition.naturalmotionanimation)s 有助於彌合這個鴻溝 – 可以提供開始/完成之類動畫重要元素的控制權平衡，同時維持動畫看起來自然生動。

> [!NOTE]
> NaturalMotionAnimations 並非用來取代 KeyFrame 動畫 – Fluent 設計語言中仍有推薦使用 KeyFrames 的地方。 NaturalMotionAnimations 是為了用在需要使用動作，但 KeyFrame 動畫不夠動態的地方。

## <a name="using-naturalmotionanimations"></a>使用 NaturalMotionAnimations

從 Fall Creators Update 開始，您可以存取新的動作體驗：**彈簧動畫**。 請參閱[彈簧動畫](spring-animations.md)以取得更詳細的彈簧逐步解說。

這個動作類型使用新的 NaturalMotionAnimation – 這個新動畫類型的重心放在讓開發人員在其 UI 中建置更熟悉且感覺自然的動作，並在控制程度和動態之間取得平衡。 它們會公開下列功能：

- 定義開始和結束值。
- 使用 InteractionTracker 定義 InitialVelocity 並繫結至輸入。
- 定義動作的特定屬性 (例如彈簧的 DampingRatio)。

入門一般公式：

1. 使用其中一個 **Create** 方法，從 Compositor 建立 NaturalMotionAnimation。
1. 定義動畫的屬性。
1. 將 NaturalMotionAnimation 做為參數傳遞至 CompositionObject 的 StartAnimation 呼叫。
    - 或者設定 InteractionTracker InertiaModifier 的 Motion 屬性。

使用彈簧 NaturalMotionAnimation 在新的 X 位移位置製作視覺「彈簧」的基本範例：

```csharp
_springAnimation = _compositor.CreateSpringScalarAnimation();
_springAnimation.Period = TimeSpan.FromSeconds(0.07);
_springAnimation.DelayTime = TimeSpan.FromSeconds(1);
_springAnimation.EndPoint = 500f;
sectionNav.StartAnimation("Offset.X", _springAnimation);
```
