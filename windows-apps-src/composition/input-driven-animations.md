---
title: 輸入導向動畫
description: 了解如何在您的應用程式 UI 中使用輸入動畫。
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10、uwp、動畫
ms.localizationpriority: medium
ms.openlocfilehash: 94d15fc7f2443475020aa7e134c076b833db46a8
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8713081"
---
# <a name="input-driven-animations"></a>輸入導向動畫

本文簡介 InputAnimation API，並建議如何在您的 UI 中使用這些動畫類型。

## <a name="prerequisites"></a>必要條件

我們此處假設您已熟悉這些文章中討論的概念：

- [關聯式動畫](relation-animations.md)

## <a name="smooth-motion-driven-from-user-interactions"></a>從使用者互動方式導向的平滑動作

在 Fluent 設計語言中，使用者和應用程式之間的互動是第一要務。 應用程式不只需要看起來融為一體，也必須能自然和生動地回應與它們互動的使用者。 這表示把手指放在螢幕上時，UI 應該適當回應以變更輸入角度、捲動應該感覺順暢，並能與手指緊密結合的在畫面間移動。

建立能動態流暢地回應使用者輸入的 UI，可產生更高的使用者參與率 - 當使用者與不同的 UI 體驗互動時，動作不只要看起來很棒，更要感覺很棒且自然。 這可讓使用者更輕鬆與您的應用程式產生連結，讓體驗更令人印象深刻且更容易使用。

## <a name="expanding-past-just-touch"></a>超越觸控

雖然觸控是使用者用來操作 UI 內容的其中一種常見介面，但他們也會使用各種其他輸入形式，例如滑鼠和手寫筆。 在這些案例中，請務必謹記，使用者會察覺 UI 動態回應他們的輸入，無論他們選擇使用何種輸入樣式。 在設計輸入導向的動作體驗時，您應該認識不同的輸入形式。

## <a name="different-input-driven-motion-experiences"></a>不同的輸入導向動作體驗

InputAnimation 空間提供數個不同的體驗，以建立動態回應的動作。 就像其他 Windows UI 動畫系統，這些輸入導向動畫在獨立執行緒上操作，這有助動態的動作體驗。 不過，有時候體驗運用現有的 XAML 控制項和元件，這些體驗有部分仍與 UI 執行緒綁定。

在建置動態輸入導向動作動畫時，您會建立三個核心體驗：

1. 美化現有的 ScrollView 體驗 – 讓 XAML ScrollViewer 的位置驅動動態動畫體驗。
1. 指標位置導向體驗 – 使用點擊測試 UIElement 上的游標位置，驅動動態動畫體驗。
1. 使用 InteractionTracker 建立自訂操作體驗 – 使用 InteractionTracker，建立完全自訂、與執行緒無關的操作體驗 (例如捲動/縮放畫布)。

## <a name="enhancing-existing-scrollviewer-experiences"></a>美化現有的 ScrollViewer 體驗

建立更為動態體驗的其中一個常見方式是建置在現有的 XAML ScrollViewer 控制項上。 在這些情況下，您利用 ScrollViewer 的捲動位置來建立額外 UI 元件，讓簡單捲動體驗更為動態。 一些範例包括自黏/害羞標頭和視差。

![使用視差的清單檢視](images/animation/parallax.gif)

![害羞標頭](images/animation/shy-header.gif)

在建立這些體驗類型時，有一般公式可供遵循：

1. 存取您想要用來驅動動畫的 XAML ScrollViewer 的 ScrollManipulationPropertySet。
    - 透過 ElementCompositionPreview.GetScrollViewerManipulationPropertySet(UIElement element) API 完成
    - 傳回包含稱為 **Translation** 之屬性的 CompositionPropertySet
1. 使用參考 Translation 屬性的方程式來建立 Composition ExpressionAnimation。
1. 在 CompositionObject 的屬性上開始動畫。

如需建置這些體驗的詳細資訊，請參閱[美化現有的 ScrollViewer 體驗](scroll-input-animations.md)。

## <a name="pointer-position-driven-experiences"></a>指標位置導向體驗

另一個與輸入有關的常見動態體驗是根據指標位置 (例如滑鼠游標) 來驅動動畫。 在這些情況下，開發人員在 XAML UIElement 中測試點擊時運用游標位置，以便建立焦點顯示之類的體驗。

![指標焦點範例](images/animation/spotlight-reveal.gif)

![指標旋轉範例](images/animation/pointer-rotate.gif)

在建立這些體驗類型時，有一般公式可供遵循：

1. 存取測試點擊時您想要知道游標位置之 XAML UIElement 的 PointerPositionPropertySet。
    - 透過 ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element) API 完成
    - 傳回包含稱為 **Position** 之屬性的 CompositionPropertySet
1. 使用參考 Position 屬性的方程式來建立 CompositionExpressionAnimation。
1. 在 CompositionObject 的屬性上開始動畫。

## <a name="custom-manipulation-experiences-with-interactiontracker"></a>使用 InteractionTracker 自訂操作體驗

利用 XAML ScrollViewer 的其中一個挑戰是它繫結至 UI 執行緒。 因此，如果 UI 執行緒變得忙碌，捲動和縮放體驗常常會延遲並抖動，導致不吸引人的體驗。 此外，也無法自訂 ScrollViewer 體驗的多個層面。 建立 InteractionTracker 可以提供一組基本要素來建立在獨立執行緒中執行的自訂操作體驗，用以解決這兩個問題。

![3D 互動範例](images/animation/interactions-3d.gif)

![拖動動畫範例](images/animation/pull-to-animate.gif)

使用 InteractionTracker 建立體驗時，有一般公式可供遵循：

1. 建立您的 InteractionTracker 物件並定義屬性。
1. 為應該擷取 InteractionTracker 使用之輸入的任何 CompositionVisual 建立 VisualInteractionSources。
1. 使用參考 InteractionTracker 之 Position 屬性的方程式，建立 Composition ExpressionAnimation。
1. 在您希望由 InteractionTracker 驅動的 Composition Visual 屬性上開始動畫。
