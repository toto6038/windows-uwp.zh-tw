---
author: jwmsft
title: 關聯式動畫
description: 根據其他物件上的屬性建立動作。
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 動畫
ms.localizationpriority: medium
ms.openlocfilehash: cde3868d1a554396bfda7c13ea0c71bd037416bc
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7572410"
---
# <a name="relation-based-animations"></a>關聯式動畫

本文簡要概述如何使用 Composition ExpressionAnimations 製作關聯式動畫。

## <a name="dynamic-relation-based-experiences"></a>動態關聯式體驗

在應用程式中建置動作體驗時，有時候動作不是以時間為基礎，而是依賴其他物件上的屬性。 KeyFrameAnimations 就無法輕鬆表達這些動作體驗類型。 在這些具體情況下，動作不再需要分離和預先定義。 而是可以根據動作與其他物件屬性的關係，動態調整動作。 例如，您可以根據物件的水平位置，來為物件的不透明度設定動畫效果。 其他範例包括「固定式標頭」和「視差」等動作體驗。

這些動作體驗類型可讓您建立感覺更連貫的 UI，而不是個別無關聯的 UI。 對於使用者，這可提供動態 UI 體驗印象。

![軌道圈](images/animation/orbit.gif)

![使用視差的清單檢視](images/animation/parallax.gif)

## <a name="using-expressionanimations"></a>使用 ExpressionAnimations

若要建立關聯式動作體驗，您可以使用 ExpressionAnimation 類型。 ExpressionAnimations (或簡稱 Expressions) 是一種新動畫類型，可讓您表達數學關係 - 系統使用此關係來計算每個畫面中動畫屬性的值。 換句話說，Expressions 是單純的數學方程式，定義每個畫面中動畫屬性的所需值。 Expressions 是多功能元件，可用於多種不同的案例，其中包括：

- 相對大小，位移動畫。
- 固定式標頭，視差搭配 ScrollViewer。 (請參閱[美化現有的 ScrollViewer 體驗](scroll-input-animations.md))。
- 貼齊點搭配 InertiaModifiers 和 InteractionTracker。 (請參閱[使用慣性修飾詞建立貼齊點](inertia-modifiers.md))。

使用 ExpressionAnimations 時，有幾件事值得事先一提：

- 永不結束 – 與其 KeyFrameAnimation 同層級不同，Expressions 沒有有限期間。 因為 Expressions 是數學關係，它們是不斷「執行」的動畫。 您可以選擇停止這些動畫。
- 執行但不一定評估 – 對於不斷執行的動畫，永遠要顧慮到效能問題。 但您不用擔心，系統很聰明，只會在 Expression 的輸入或參數變更時才重新評估。
- 解析至正確的物件類型 – 因為 Expressions 是數學關係，請務必確定定義 Expression 的方程式解析為與目標動畫屬性相同的類型。 例如，如果將位移產生動畫效果，您的 Expression 應該解析為 Vector3 類型。

### <a name="components-of-an-expression"></a>Expression 的元件

建置 Expression 的數學關係時，有數個核心元件：

- 參數 - 代表常數值或參考其他 Composition 物件的值。
- 數學運算子 – 一般數學運算子：加號 (+)、減號 (-)、乘號 (*)、除號 (/)，將參數連接起來以形成方程式。 也包含條件式運算子，例如大於 (>)、等於 (==)、三元運算子 (condition ? ifTrue : ifFalse) 等。
- 數學函數 – 根據 System.Numerics 的數學函數/捷徑。 如需完整的支援函數清單，請參閱 [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation)。

Expressions 也支援一組關鍵字 - 只有在 ExpressionAnimation 系統中才具有不同意義的特殊片語。 這些會列在 (以及數學函數的完整清單) [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation) 文件中。

### <a name="creating-expressions-with-expressionbuilder"></a>使用 ExpressionBuilder 建立 Expressions

有兩個選項可在 UWP 應用程式中建立 Expressions：

1. 透過正式、公用 API 將方程式建置為字串。
1. 透過開放原始碼 ExpressionBuilder 工具，在型別安全物件模型中建置方程式。 請參閱 [Github 來源和文件](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/ExpressionBuilder)。

針對本文的目的，我們將使用 ExpressionBuilder 定義 Expressions。

### <a name="parameters"></a>參數

參數組成 Expression 的核心。 有兩種類型的參數：

- 常數：這些是代表類型 System.Numeric 變數的參數。 動畫開始時會指派值給這些參數。
- 參考：這些是代表 CompositionObjects 參考的參數，這些參數會在動畫開始後持續取得更新的值。

一般而言，參考是如何動態變更 Expression 輸出的主要層面。 隨著這些參考變更，Expression 的輸出也會變更做為結果。 如果您使用字串建立 Expression 或在範本案例中使用 (使用您的 Expression 以多個 CompositionObjects 做為目標)，您將需要命名，並設定參數的值。 如需詳細資訊，請參閱「範例」區段。

### <a name="working-with-keyframeanimations"></a>使用 KeyFrameAnimations

Expressions 也可以搭配 KeyFrameAnimations 使用。 在這些情況中，您想使用 Expression 在某個時間點定義 KeyFrame，這類 KeyFrames 稱為 ExpressionKeyFrames。

```csharp
KeyFrameAnimation.InsertExpressionKeyFrame(Single, String)
KeyFrameAnimation.InsertExpressionKeyFrame(Single, ExpressionNode)
```

不過，與 ExpressionAnimations 不同，ExpressionKeyFrames 僅會在 KeyFrameAnimation 開始時評估一次。 請記住，您不會傳遞 ExpressionAnimation 做為 KeyFrame 的值，而是做為字串 (如果您使用 ExpressionBuilder 則為 ExpressionNode)。

## <a name="example"></a>範例

現在讓我們逐步解說使用 Expressions 的範例，尤其是 Windows UI 範例庫中的 PropertySet 範例。 我們接下來將看看 Expression 管理藍球軌道運動的行為。

![軌道圈](images/animation/orbit.gif)

有三個元件在播放總體體驗：

1. KeyFrameAnimation，讓紅球的 Y 位移產生動畫效果。
1. 使用 **Rotation** 屬性的 PropertySet，協助驅動軌道，由另一個 KeyFrameAnimation 產生動畫效果。
1. 驅動藍球位移的 ExpressionAnimation，參考紅球位移和 Rotation 屬性以保持完美軌道。

我們將重點放在 #3 定義的 ExpressionAnimation。 我們也會使用 ExpressionBuilder 類別來建構這個 Expression。 結尾會列出透過字串建置此體驗的程式碼。

在此方程式中，您必須從 PropertySet 參考兩個屬性，一個是中心點位移，另一個是旋轉。

```
var propSetCenterPoint =
_propertySet.GetReference().GetVector3Property("CenterPointOffset");

// This rotation value will animate via KFA from 0 -> 360 degrees
var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
```

接下來，您需要定義負責實際軌道旋轉的 Vector3 元件。

```
var orbitRotation = EF.Vector3(
    EF.Cos(EF.ToRadians(propSetRotation)) * 150,
    EF.Sin(EF.ToRadians(propSetRotation)) * 75, 0);
```

> [!NOTE]
> `EF` 是「使用」標記的簡寫，定義 ExpressionBuilder.ExpressionFunctions。

最後，將這些元件結合在一起，並參考紅球的位置來定義數學關係。

```
var orbitExpression = redSprite.GetReference().Offset + propSetCenterPoint + orbitRotation;
blueSprite.StartAnimation("Offset", orbitExpression);
```

假設您想要使用相同 Expression 但搭配另外兩個視覺效果，這表示 2 組軌道圈。 使用 CompositionAnimations，您可以重複使用動畫與並以多個 CompositionObjects 做為目標。 將此 Expression 用於其他軌道案例時，您只需要變更對視覺效果的參考。 我們將這稱為「範本化」。

在此案例中，您修改稍早建置的 Expression。 您建立具有名稱的參考然後指定不同的值，而不是「取得」CompositionObject 的參考：

```
var orbitExpression = ExpressionValues.Reference.CreateVisualReference("orbitRoundVisual");
orbitExpression.SetReferenceParameter("orbitRoundVisual", redSprite);
blueSprite.StartAnimation("Offset", orbitExpression);
// Later on … use same Expression to assign to another orbiting Visual
orbitExpression.SetReferenceParameter("orbitRoundVisual", yellowSprite);
greenSprite.StartAnimation("Offset", orbitExpression);
```

以下是透過公用 API 使用字串定義 Expression 的程式碼。

```
ExpressionAnimation expressionAnimation =
compositor.CreateExpressionAnimation("visual.Offset + " +
"propertySet.CenterPointOffset + " +
"Vector3(cos(ToRadians(propertySet.Rotation)) * 150," + "sin(ToRadians(propertySet.Rotation)) * 75, 0)");
 var propSetCenterPoint = _propertySet.GetReference().GetVector3Property("CenterPointOffset");
 var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
expressionAnimation.SetReferenceParameter("propertySet", _propertySet);
expressionAnimation.SetReferenceParameter("visual", redSprite);
```
