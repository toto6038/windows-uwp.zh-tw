---
author: scottmill
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: 組合動畫
description: 許多組合物件和效果屬性都可藉由使用主要畫面格和運算式動畫來製作動畫效果，這些動畫可允許隨時間或根據計算來變更 UI 元素的屬性。
---
# 組合動畫

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

許多組合物件和效果屬性都可藉由使用主要畫面格和運算式動畫來製作動畫效果，這些動畫可允許隨時間或根據計算來變更 UI 元素的屬性。 動畫的類型有兩種：主要畫面格動畫 (由 [**KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706830) 類別表示) 和運算式動畫 (由 [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817) 類別表示)。

-   [可展示動畫的屬性](./composition-animation.md#animatable_properties)
-   [KeyFrame 動畫](./composition-animation.md#keyframe-animations)
    -   [建立您的動畫與 KeyFrame](./composition-animation.md#creating-your-animation-and-Keyframes)
    -   [KeyFrame 屬性](./composition-animation.md#keyframe-properties)
    -   [Easing 函式](./composition-animation.md#easing-functions)
    -   [開始和停止 KeyFrame 動畫](./composition-animation.md#starting-and-stopping-keyframe-animations)
    -   [動畫完成事件](./composition-animation.md#animation-completion-events)
-   [Expression 動畫](./composition-animation.md#expression-animations)
    -   [建立您的運算式](./composition-animation.md#creating-your-expression)
    -   [屬性集](./composition-animation.md#property-sets)
    -   [運算式 KeyFrame](./composition-animation.md#expression_keyframes)

## 可展示動畫的屬性

下列屬性可透過與「KeyFrame 動畫」或「Expression 動畫」建立關聯來產生動畫效果：

-   CompositionColorBrush.Color
-   InsetClip.BottomInset
-   InsetClip.LeftInset
-   InsetClip.RightInset
-   InsetClip.TopInset
-   Visual.AnchorPoint
-   Visual.CenterPoint
-   Visual.Offset
-   Visual.Opacity
-   Visual.Orientation
-   Visual.RotationAngle
-   Visual.RotationAxis
-   Visual.Size
-   Visual.TransformMatrix
-   CompositionPropertySet

## KeyFrame 動畫

「KeyFrame 動畫」是時間型動畫，這種動畫使用一或多個主要畫面格來指定動畫值應如何隨時間變更。 畫面格代表標記，可讓您定義在特定時間應該有什麼動畫值。

### 建立您的動畫與 KeyFrame

若要建構「KeyFrame 動畫」，請使用與您想要產生動畫效果之屬性的結構類型相互關聯的 Compositor 類別的其中一個建構函式方法。

-   [**CreateColorKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt589424)
-   [**CreateQuaternionKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt517858)
-   [**CreateScalarKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706803)
-   [**CreateVector2KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706806)
-   [**CreateVector3KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706807)
-   [**CreateVector4KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706808)

例如，下列程式碼片段會建立一個 Vector3 主要畫面格動畫：

```cs
var animation = _compositor.CreateVector3KeyFrameAnimation();
```

建構每個 KeyFrame 時，都是透過呼叫 KeyFrameAnimation 的 InsertKeyFrame 方法並指定兩個元件和選擇性的第三個元件來建構：

-   時間：介於 0.0 – 1.0 之間的標準化 KeyFrame 進度狀態
-   值：動畫值在時間狀態的特定值
-   (選擇性) Easing 函式：描述上一個 KeyFrame 與目前 KeyFrame 格之間的內插補點的函式 (稍後討論)

例如，下列程式碼片段會在 Vector3KeyFrameAnimation 中插入一個將在動畫進行到一半時觸發的主要畫面格：

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```

### KeyFrame 屬性

在定義「KeyFrame 動畫」與個別的 KeyFrame 之後，您就可以定義動畫的多個屬性：

-   DelayTime – 在呼叫 StartAnimation() 之後、動畫開始之前的時間
-   Duration – 動畫的持續時間
-   IterationBehavior – 動畫的計數或無限重複行為
-   IterationCount –「KeyFrame 動畫」將重複的有限次數
-   KeyFrame 計數 – 特定 KeyFrameAnimation 中的 KeyFrame 數目
-   StopBehavior – 指定呼叫 StopAnimation 時，動畫屬性值的行為。

例如，下列程式碼片段會將動畫的持續時間設定為 5 秒：

```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

### Easing 函式

主要畫面格 Easing 函式 CompositionEasingFunction 會指出中繼值如何從上一個主要畫面格值進展到目前主要畫面格值。 如果您沒有為 KeyFrame 提供 Easing 函式，將會使用預設的曲線。

支援的 Easing 函式類型有兩種：

-   線性 ([**LinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706839))
-   三次方貝茲 ([**CubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706812))

若要建立 Easing 函式，請使用 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706761) 類別的 [**CreateLinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706801) 或 [**CreateCubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706791) 方法：

```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
```

注意：您可以在這裡找到產生「三次方貝茲」點的實用工具。

若要將 Easing 函式新增到您的 KeyFrame 中，只要在將 KeyFrame 插入「動畫」中時，將第三個參數新增到 KeyFrame 即可：

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

### 開始和停止 KeyFrame 動畫

在定義您的動畫、KeyFrame 及屬性之後，您便已準備好將動畫連接到您想要產生動畫效果的屬性。 您需呼叫屬性所屬的 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 物件上的 [**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840)，以將動畫連接到 您打算產生動畫效果的屬性。

一般的語法及範例如以下所示：

```cs
targetVisual.StartAnimation("Offset", animation);
```

在開始您的動畫之後，您也能夠將它停止和中斷。 做法是使用 [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841) 方法，並指定您想要停止產生動畫效果的屬性。

例如：

```cs
targetVisual.StopAnimation("Offset");
```

### 動畫完成事件

KeyFrame 動畫與條件式的 Expression 動畫不同，具有已定義的結束。 開發人員可以使用批次來決定群組或單一 KeyFrame 動畫何時完成。 建立或抓取批次時是根據批次物件類型，而批次可用來彙總動畫的結束狀態。 抓取到「認可」批次時會建立限定範圍的批次，稍後會說明這些不同的批次。

當批次內的所有動畫都完成時，就會引發批次完成事件。 引發批次完成事件所需的時間取決於批次中 最長或延遲最久的動畫。 當您需要知道所選取的幾組動畫何時完成以排定其他工作時，彙總結束狀態會相當有用。

### 限定範圍的批次

若要彙總特定的群組或單一動畫，您必須先呼叫 [**CreateScopedBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589425) 來建立「限定範圍的」批次。

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
```

建立「限定範圍的」批次之後，所有已開始的動畫都會進行彙總，直到使用 [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) 或 [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844) 將批次明確暫停或結束為止。

呼叫 [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) 會停止彙總動畫結束狀態， 直到呼叫 [**Resume**](https://msdn.microsoft.com/library/windows/apps/Mt590847) 為止。 這可讓您明確排除來自指定之批次的內容。

```cs
myScopedBatch.Suspend();
```

若要完成您的批次，您必須呼叫 [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844)。 如果沒有 End 呼叫，批次就會讓不斷進行收集的物件保持開啟。

```cs
myScopedBatch.End();
```

如果您想要知道單一動畫何時結束，您必須建立「限定範圍的」批次、開始動畫，然後結束批次。

例如：

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation("Opacity", myAnimation);
myScopedBatch.End();
```

### 認可批次

認可批次是在認可週期內以隱含方式建立的批次。 在認可週期內隨時呼叫 [**GetCommitBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589430)，即可抓取目前的「認可」批次。 認可週期的定義是從撰寫器進行更新之間的時間。 更新會先排入佇列，直到系統準備好處理變更並將位元繪製到螢幕為止。 標題並不會控制何時發生認可。 「認可」批次會將認可週期內的所有物件彙總，亦即在呼叫 GetCommitBatch 之前和之後的物件。 每一認可週期只有一個「認可」批次，因此您不需要明確地對認可批次呼叫 [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844)。

```cs
myCommitBatch = _compositor.GetCommitBatch(CompositionBatchTypes.Animation);
```

### 批次狀態

若要判斷批次是否接受進行動畫彙總，您可以使用 **IsActive** 屬性。 如果 **IsEnded** 屬性傳回 true，您就無法將動畫新增到該特定的批次。 「限定範圍的」批次會在透過呼叫 [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844) 方法明確結束這些批次時「結束」。 「認可」批次則會在目前的認可週期結束時結束。

## Expression 動畫

「Expression 動畫」是使用數學運算式來指定應如何計算每個畫面格之動畫值的動畫。 運算式語言本身可以參考來自其他組合物件的屬性。 不同於「KeyFrame 動畫」，Expression 不是以時間為基礎，而且會在每個畫面格 (如有必要) 都進行處理。 Expression 可用來描述「組合」引擎可以處理的較複雜使用者體驗，例如黏性標頭與視差。

### 建立您的運算式

若要建立您運算式，請對您的 Compositor 物件呼叫 [**CreateExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt187002)，並指定您想要使用的運算式：

```cs
var expression = _compositor.CreateExpressionAnimation("INSERT_EXPRESSION_STRING")
```

### 運算子、優先順序及關聯性

「運算式」語言支援下列運算子，並遵守「C# 語言規格」中定義的運算子優先順序與關聯性：

-   一元 (-)
-   乘法 (\* /)
-   加法 (- +)

「運算式」語言也支援每一元件數學運算的概念。 只有在「基本」類型 (向量和矩陣) 上才支援這些成員存取 (x.y) 運算子。 例如：

```cs
Offset.x + 5.0
```

### 屬性參數

「運算式」語言的其中一個強大要素就是能夠在運算式中使用參數做為變數。 運算式字串可以包含會以常數值取代的參數，或是計算時會參考另一個物件之屬性值的參數。 例如：

```cs
ChildVisual.Offset.X / ParentVisual.Offset.Y
```

在這個範例中，ChildOffset 和 ParentOffset 是代表兩個視覺效果的位移屬性的參數。 定義參數時，是使用 [**CompositionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706708) 類別的 **Set\*Parameter** 方法：

-   若要建立向量參數，請使用 [**SetVector2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector2parameter.aspx)、[**SetVector3Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector3parameter.aspx) 或 [**SetVector4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setvector4parameter)。
-   若要建立矩陣參數，請使用 [**SetMatrix3x2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix3x2parameter.aspx) 或 [**SetMatrix4x4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix4x4parameter)。
-   若要建立純量參數，請使用 [**SetScalarParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setscalarparameter)。
-   若要建立色彩參數，請使用 [**SetColorParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setcolorparameter)。
-   若要建立四元參數，請使用 [**SetQuaternionParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setquaternionparameter)。
-   若要建立參考參數，請使用 [**SetReferenceParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setreferenceparameter)。

在上述的「運算式字串」中，我們將需要建立兩個參數來定義兩個「視覺效果」：

```cs
Expression.SetReferenceParameter("ChildVisual", childVisual);
Expression.SetReferenceParameter("ParentVisual", parentVisual);
```

### 運算式協助程式函式

除了可以存取運算子和屬性參數之外，您也可以存取運算式語言的完整協助程式函式程式庫。 您可以在 [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817) 類別的＜備註＞一節中找到協助程式函式的完整清單，以下提供幾個協助程式函式：

-   Max (純量值1, 純量值2)
-   Scale (向量3 值, 向量係數)
-   Transform(向量2 值, 矩陣 3x2 矩陣)

以下是一個使用 Clamp 協助程式函式的較複雜運算式字串範例：

```cs
Clamp((scroller.Offset.y * -1.0) - container.Offset.y, 0, container.Size.y - header.Size.y)
```

### 開始和停止 Expression 動畫

若要開始和停止您的「Expression 動畫」，請使用您在「KeyFrame 動畫」使用的相同函式 ([**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840) 和 [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841))。 注意：「Expression 動畫」會持續執行，直到透過使用 **StopAnimation** 來停止它們為止，它們與「KeyFrame 動畫」不同，並沒有一個有限的持續時間。

### 屬性集

除了能夠參考其他「組合」物件的屬性之外， 您還能夠建立並儲存可跨多個動畫使用的您自己的屬性值。 屬性集是由 [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772) 類別表示。 屬性集可讓您建立包含值的屬性，並稍後在運算式中參考它，或甚至連結它做為「KeyFrame 動畫」的目標。

若要建立屬性集，請使用 Compositor 類別的 [**CreatePropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706802) 方法。 例如：

```cs
_sharedProperties = _compositor.CreatePropertySet();
```

建立您的屬性集之後，您可以使用 [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772) 的其中一個 **Insert\*** 方法，將屬性和值新增到該屬性集。 例如：

```cs
_sharedProperties.InsertVector3("NewOffset", offset);
```

建立運算式動畫之後，您可以在運算式字串中，使用參考參數來參考來自屬性集的屬性。 例如：

```cs
var expression = _compositor.CreateExpressionAnimation("sharedProperties.NewOffset");
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
```

### 運算式 KeyFrame

在本文稍早，我們已說明如何建立「KeyFrame 動畫」及其各自的 KeyFrame。 除了使用標準化的時間和值元件來建立傳統的 KeyFrame 之外，您也能夠定義運算式 KeyFrame。

我們可以不在 KeyFrame 中針對特定時間定義一個明確的值，而是使用運算式語法來描述在 KeyFrame 時間軸中的這個特定 時間點應該如何計算值。 您可以將運算式 KeyFrame 與您「KeyFrame 動畫」中的基本 KeyFrame 混搭。

### 建構運算式 KeyFrame

與傳統 KeyFrame 類似，運算式 KeyFrame 也是藉由指定 2 個元件來建構而成：

-   時間：介於 0.0 – 1.0 之間的標準化 KeyFrame 狀態
-   值：用來描述應該如何計算值的運算式字串

例如，下列程式碼片段會同時使用一般和運算式 KeyFrame 的組合：

```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, "VisualBOffset.X / VisualAOffset.Y");
animation.InsertKeyFrame(1.00f, 0.8f);
```

### 使用目前值與起始值

在運算式語言中，可以同時參考動畫的目前值與起始值。 這些值在運算式語言中前面都會加上 “this”：

-   this.CurrentValue
-   this.StartingValue

在「運算式 KeyFrame」中使用這些值的範例：

```cs
animation.InsertExpressionKeyFrame(0.0f, "this.CurrentValue + delta");
```

### 條件式運算式

除了支援基本數學運算式之外，您也可以使用三元運算子來定義條件式運算式：

```cs
(condition ? first_expression : second_expression)
```

在運算式本身內，在條件陳述式中支援下列條件式運算子：

-   等於 (==)
-   不等於 (!=)
-   小於 (<)
-   小於或等於 (<=)
-   大於 (>)
-   大於或等於 (>=)

最後，在條件陳述式中支援使用下列連接詞做為運算子或函式：

-   非：! / Not(bool1)
-   且：&& / And(bool1, bool2)
-   或：|| / Or(bool1, bool2)

下列程式碼片段示範一個在運算式中使用條件式的範例：

```cs
var expression = _compositor.CreateExpressionAnimation("target.Offset.x > 50 ? 0.0f +   (target.Offset.x / parent.Offset.x) : 1.0f");
```

 

 






<!--HONumber=May16_HO2-->


