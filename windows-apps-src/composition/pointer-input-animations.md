---
author: jwmsft
title: 指標型動畫
description: 了解如何使用指標位置來建立動態的「與游標緊密結合」體驗。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10、uwp、動畫
ms.localizationpriority: medium
ms.openlocfilehash: b69899761e1c4a139fd2b15d6810440df5192487
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5598338"
---
# <a name="pointer-based-animations"></a>指標型動畫

本文示範如何使用指標位置來建立動態的「與游標緊密結合」體驗。

## <a name="prerequisites"></a>必要條件

我們此處假設您已熟悉這些文章中討論的概念：

- [輸入導向動畫](input-driven-animations.md)
- [關聯式動畫](relation-animations.md)

## <a name="why-create-pointer-position-driven-experiences"></a>為什麼要建立指標位置導向體驗？

在 Fluent 設計語言中，觸控不是與 UI 互動的唯一方式。 因為 UWP 跨越多個裝置的尺寸規格，因此使用者會使用其他輸入形式來與應用程式互動，例如滑鼠和手寫筆。 使用這些其他輸入形式的位置資料，可以讓使用者感覺與您的應用程式更有聯繫感。

指標位置導向體驗可讓您運用指標輸入形式的螢幕上位置來為您的應用程式建立其他動作和 UI 體驗。 這些體驗通常可以提供 UI 行為與結構的其他情境和意見反應給使用者。 體驗不再是單向串流，而是開始成為雙向串流，使用者運用輸入形式提供輸入，而應用程式 UI 可以回應。

一些範例包括：

- 讓焦點位置產生動畫效果以跟隨游標

    ![指標焦點範例](images/animation/spotlight-reveal.gif)

- 根據指標位置旋轉影像

    ![指標旋轉範例](images/animation/pointer-rotate.gif)

## <a name="using-pointerpositionpropertyset"></a>使用 PointerPositionPropertySet

您可以使用 PointerPositionPropertySet 建立這些體驗。 對 UIElement 進行正面點擊測試時，這個 PropertySet 會為 UIElement 建立以維持指標位置。 位置值是相對於 UIElement 的座標空間 (<0,0> 的位置是 UIElement 的左上角)。 您可以接著利用此屬性設定在動畫中驅動其他屬性的動作。

針對每個不同的指標輸入形式，輸入可以位於多種輸入狀態並變更位置：滑鼠指標暫留、已按下、按下與移動。 PointerPositionPropertySet 僅會在滑鼠和手寫筆的滑鼠指標暫留、已按下、按下與移動和已移動狀態中維持指標位置。

入門一般步驟：

1. 識別您想要讓指標追蹤其位置的 UIElement。
1. 存取 PointerPositionPropertySet via ElementCompositionPreview。
    - 將 UIElement 傳遞給 ElementCompositionPreview.GetPointerPositionPropertySet 方法。
1. 建立 ExpressionAnimation，參考 PropertySet 的 Position 屬性。
    - 不要忘記設定參考參數！
1. 使用 ExpressionAnimation 以 CompositionObject 的屬性做為目標。

> [!NOTE]
> 建議您將 GetPointerPositionPropertySet 方法傳回的 PropertySet 指派給類別變數。 如此可確保屬性設定不會被記憶體回收清理，因而對參考它的 ExpressionAnimation 沒有效果。 ExpressionAnimations 不會對方程式中使用的任何物件維持強式參考。

## <a name="example"></a>範例

讓我們來看一個範例，運用滑鼠和手寫筆輸入樣式的暫留位置來動態旋轉影像。

![指標旋轉範例](images/animation/pointer-rotate.gif)

影像是 UIElement，因此讓我們先取得 PointerPositionPropertySet 的參考

```csharp
_pointerPositionPropSet = ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element);
```

在此範例中，您有兩個運算式正在播放：

- 一個運算式中，影像根據指標遠離影像中心的位置來旋轉。 離得越遠，旋轉越多。
- 另一個運算式中，旋轉軸根據指標位置而變更。 您希望旋轉軸垂直於位置的向量。

讓我們定義兩個運算式，一個以 RotationAngle 屬性為目標，另一個以 RotationAxis 屬性為目標。 以參考任何其他 PropertySet 的方式來參考 PointerPositionPropertySet。

在此範例中，您使用 ExpressionBuilder 類別建置運算式。

```csharp
// || DEFINE THE EXPRESSION FOR THE ROTATION ANGLE ||
var hoverPosition = _pointerPositionPropSet.GetSpecializedReference
<PointerPositionPropertySetReferenceNode>().Position;
var angleExpressionNode =
EF.Conditional(
 hoverPosition == new Vector3(0, 0, 0),
 ExpressionValues.CurrentValue.CreateScalarCurrentValue(),
 35 * ((EF.Clamp(EF.Distance(center, hoverPosition), 0, distanceToCenter) % distanceToCenter) / distanceToCenter));
_tiltVisual.StartAnimation("RotationAngleInDegrees", angleExpressionNode);

// || DEFINE THE EXPRESSION FOR THE ROTATION AXIS ||
var axisAngleExpressionNode = EF.Vector3(
-(hoverPosition.Y - center.Y) * EF.Conditional(hoverPosition.Y == center.Y, 0, 1),
 (hoverPosition.X - center.X) * EF.Conditional(hoverPosition.X == center.X, 0, 1),
 0);
_tiltVisual.StartAnimation("RotationAxis", axisAngleExpressionNode);
```