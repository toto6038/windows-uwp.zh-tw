---
title: 指標型動畫
description: 了解如何使用指標位置來建立動態的「與游標緊密結合」體驗。
ms.date: 03/23/2021
ms.topic: article
keywords: windows 10, uwp, 動畫
ms.localizationpriority: medium
ms.openlocfilehash: 80fd46af6f84d4a23329ad34fc77a14faf97b913
ms.sourcegitcommit: 34f532fd023af2849c3e975baf7aa6771d7e53b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/23/2021
ms.locfileid: "104893858"
---
# <a name="pointer-based-animations"></a>指標型動畫

本文示範如何使用指標位置來建立動態的「與游標緊密結合」體驗。

## <a name="prerequisites"></a>Prerequisites

我們此處假設您已熟悉這些文章中討論的概念：

- [輸入導向動畫](input-driven-animations.md)
- [關聯式動畫](relation-animations.md)

## <a name="why-create-pointer-position-driven-experiences"></a>為什麼要建立指標位置導向體驗？

在 [流暢的設計語言](/windows/apps/fluent-design-system)中，觸控不是與 UI 互動的唯一方法。 因為 UWP 跨越多個裝置的尺寸規格，因此使用者會使用其他輸入形式來與應用程式互動，例如滑鼠和手寫筆。 使用這些其他輸入形式的位置資料，可以讓使用者感覺與您的應用程式更有聯繫感。

指標位置導向體驗可讓您運用指標輸入形式的螢幕上位置來為您的應用程式建立其他動作和 UI 體驗。 這些體驗通常可以提供 UI 行為與結構的其他情境和意見反應給使用者。 體驗不再是單向串流，而是開始成為雙向串流，使用者運用輸入形式提供輸入，而應用程式 UI 可以回應。

部分範例包括：

- 以動畫顯示 [焦點](/uwp/api/windows.ui.composition.spotlight) 位置以跟隨游標

    ![指標焦點範例](images/animation/spotlight-reveal.gif)

- 根據指標位置旋轉影像

    ![指標旋轉範例](images/animation/pointer-rotate.gif)

## <a name="using-pointerpositionpropertyset"></a>使用 PointerPositionPropertySet

您可以使用 PointerPositionPropertySet 建立這些體驗。 對 UIElement 進行正面點擊測試時，這個 PropertySet 會為 UIElement 建立以維持指標位置。 位置值是相對於 UIElement 的座標空間 (<0,0> 的位置是 UIElement 的左上角)。 您可以接著利用此屬性設定在動畫中驅動其他屬性的動作。

針對每個不同的指標輸入形式，輸入可以位於多種輸入狀態並變更位置：滑鼠指標暫留、已按下、按下與移動。 PointerPositionPropertySet 只會在滑鼠和畫筆的停留、按下和已移動狀態下，維護指標的位置。

入門一般步驟：

1. 識別您想要讓指標追蹤其位置的 UIElement。
1. 存取 PointerPositionPropertySet via ElementCompositionPreview。
    - 將 UIElement 傳遞至 [ElementCompositionPreview. GetPointerPositionPropertySet](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getpointerpositionpropertyset) 方法。
1. 建立 ExpressionAnimation，參考 PropertySet 的 Position 屬性。
    - 別忘了設定參考參數！
1. 以 ExpressionAnimation 為目標的 CompositionObject 屬性。

> [!NOTE]
> 建議您將 GetPointerPositionPropertySet 方法傳回的 PropertySet 指派給類別變數。 如此可確保屬性設定不會被記憶體回收清理，因而對參考它的 ExpressionAnimation 沒有效果。 ExpressionAnimations 不會對方程式中使用的任何物件維持強式參考。

## <a name="example"></a>範例

我們來看看一個範例，其中會利用滑鼠和畫筆輸入樣式的停留位置，動態旋轉影像。

![指標旋轉範例](images/animation/pointer-rotate.gif)

影像是 UIElement，因此我們先取得 PointerPositionPropertySet 的參考

```csharp
_pointerPositionPropSet = ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element);
```

在此範例中，您有兩個運算式正在播放：

- 一個運算式中，影像根據指標遠離影像中心的位置來旋轉。 離得越遠，旋轉越多。
- 另一個運算式中，旋轉軸根據指標位置而變更。 您希望旋轉軸垂直於位置的向量。

在這裡，您會定義兩個運算式，一個是以 RotationAngle 屬性為目標，另一個則是 RotationAxis 屬性。 以參考任何其他 PropertySet 的方式來參考 PointerPositionPropertySet。

在此範例中，您會使用 [r](/windows/communitytoolkit/animations/expressions) 類別來建立運算式。

```csharp
// using EF = ExpressionBuilder.ExpressionFunctions;
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

## <a name="see-also"></a>另請參閱

- [指標旋轉範例](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK%2015063/PointerRotate)
- [PointerPositionPropertySetReferenceNode 類別](/dotnet/api/microsoft.toolkit.uwp.ui.animations.expressions.pointerpositionpropertysetreferencenode?view=win-comm-toolkit-dotnet-7.0&preserve-view=true)