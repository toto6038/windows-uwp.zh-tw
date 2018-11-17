---
author: jwmsft
title: 美化現有的 ScrollViewer 體驗
description: 了解何使用 XAML ScrollViewer 和 ExpressionAnimations 來建立動態輸入導向的動作體驗。
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10、uwp、動畫
ms.localizationpriority: medium
ms.openlocfilehash: a078d096a9cffe26e9b342250726dd75cdf48817
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2018
ms.locfileid: "7151418"
---
# <a name="enhance-existing-scrollviewer-experiences"></a>美化現有的 ScrollViewer 體驗

本文說明如何使用 XAML ScrollViewer 和 ExpressionAnimations 來建立動態輸入導向的動作體驗。

## <a name="prerequisites"></a>必要條件

我們此處假設您已熟悉這些文章中討論的概念：

- [輸入導向動畫](input-driven-animations.md)
- [關聯式動畫](relation-animations.md)

## <a name="why-build-on-top-of-scrollviewer"></a>為何在 ScrollViewer 上建置？

一般而言，您可以使用現有的 XAML ScrollViewer 來為應用程式內容建立可捲動和可縮放表面。 Fluent 設計語言引入之後，您現在應該也會將焦點放在如何使用捲動或縮放表面的動作來驅動其他動作體驗。 例如，使用捲動可驅動背景的模糊動畫或驅動「固定式標頭」的位置。

在這些案例中，您都運用捲動和縮放等操作體驗的行為，讓您應用程式的其他部分更為生動。 這些會隨後轉讓應用程式感覺更為一致，讓使用者的體驗更令人印象深刻。 透過讓應用程式 UI 更令人印象深刻，使用者與應用程式的互動會更頻繁也持續更久。

## <a name="what-can-you-build-on-top-of-scrollviewer"></a>可以在 ScrollViewer 上建置什麼項目？

您可以利用 ScrollViewer 的位置來建置數種動態體驗：

- 視差 – 使用 ScrollViewer 的位置，以捲動位置的相對速率移動背景或前景內容。
- StickyHeaders – 使用 ScrollViewer 的位置製作標頭動畫並固定至某個位置
- 輸入導向效果 – 使用 Scrollviewer 的位置，製作組合效果，例如柔邊。

一般而言，透過使用 ExpressionAnimation 參考 ScrollViewer 的位置，您可以建立相對於捲動量而動態變化的動畫。

![使用視差的清單檢視](images/animation/parallax.gif)

![害羞標頭](images/animation/shy-header.gif)

## <a name="using-scrollmanipulationpropertyset"></a>使用 ScrollManipulationPropertySet

若要使用 XAML ScrollViewer 建立這些動態體驗，您必須能夠參考動畫中的捲動位置。 做法是存取稱為 ScrollManipulationPropertySet 的 XAML ScrollViewer 中的 CompositionPropertySet。
ScrollManipulationPropertySet 包含單一 Vector3 屬性，稱為 Translation，可提供 ScrollViewer 捲動位置的存取。 接著您可以像 ExpressionAnimation 中的任何其他 CompositionPropertySet，參考此項目。

入門一般步驟：

1. 透過 ElementCompositionPreview 存取 ScrollManipulationPropertySet。
    - `ElementCompositionPreview.GetScrollManipulationPropertySet(ScrollViewer scroller)`
1. 建立 ExpressionAnimation，參考 PropertySet 的 Translation 屬性。
    - 不要忘記設定參考參數！
1. 使用 ExpressionAnimation 以 CompositionObject 的屬性做為目標。

> [!NOTE]
> 建議您將 GetScrollManipulationPropertySet 方法傳回的 PropertySet 指派給類別變數。 如此可確保屬性設定不會被記憶體回收清理，因而對參考它的 ExpressionAnimation 沒有效果。 ExpressionAnimations 不會對方程式中使用的任何物件維持強式參考。

## <a name="example"></a>範例

讓我們來看看上述視差範例如何組合呈現。 如需參考資料，此應用程式的所有原始碼都能在[GitHub 上的 Window UI 開發人員實驗室存放庫](https://github.com/Microsoft/WindowsUIDevLabs)中找到。

首先，取得 ScrollManipulationPropertySet 的參考。

```csharp
_scrollProperties =
    ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);
```

下一個步驟是建立 ExpressionAnimation，用來定義利用 ScrollViewer 捲動位置的方程式。

```csharp
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight))";
```

> [!NOTE]
> 您也可以使用 ExpressionBuilder 協助程式類別來建構相同運算式，而不需用到字串：

> ```csharp
> var scrollPropSet = _scrollProperties.GetSpecializedReference<ManipulationPropertySetReferenceNode>();
> var parallaxValue = 0.5f;
> var parallax = (scrollPropSet.Translation.Y + startOffset);
> _parallaxExpression = parallax * parallaxValue - parallax;
> ```

最後，採取此 ExpressionAnimation，並以您想要執行視差的視覺效果做為目標。 在此例中，這是清單中每個項目的影像。

```csharp
Visual visual = ElementCompositionPreview.GetElementVisual(image);
visual.StartAnimation("Offset.Y", _parallaxExpression);
```