---
author: jwmsft
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: "組合動畫"
description: "許多組合物件和效果屬性都可藉由使用主要畫面格和運算式動畫來製作動畫效果，這些動畫可允許隨時間或根據計算來變更 UI 元素的屬性。"
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: e7d0f5c3fc0d1414dc1b4f714683494fcffd4f51
ms.sourcegitcommit: b42d14c775efbf449a544ddb881abd1c65c1ee86
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2017
---
# <a name="composition-animations"></a>組合動畫

Windows.UI.Composition API 可讓您在整合 API 層中針對撰寫器物件，執行建立、產生動畫效果、轉換以及操控等作業。 組合動畫提供了功能強大且具效率的方式，可讓您在應用程式 UI 中執行動畫。 其經過徹底從頭開始的全新設計，確保讓動畫無論 UI 執行緒為何皆能以 60 FPS 運作，此外並提供優異操作彈性以協助您打造令人驚艷的使用體驗，您不僅可運用時間，更可運用輸入和其他屬性來驅動動畫。

本文提供關於可用功能的概觀，協助您產生 Composition 物件屬性動畫效果。 我們假設您已熟悉視覺層結構的基本知識。 如需詳細資訊，請參閱[組合視覺效果](./composition-visual-tree.md)。

有兩種組合動畫類型：**KeyFrame 動畫**與 **Expression 動畫**。

![動畫類型](./images/composition-animation-types.png)

## <a name="types-of-composition-animations"></a>組合動畫類型

**KeyFrame 動畫**提供傳統的時間驅動型*逐格*動畫體驗。 您可明確定義*控制點*來描述動畫時間軸上特定點所需的動畫屬性值。 更重要的是，您可使用 Easing 函式 (亦稱 Interpolator) 描述這些控制點間的轉換方式。

**隱含動畫**是一種動畫類型，可讓您定義可重複使用的個別動畫或一系列動畫，並與核心應用程式邏輯分開處理。 隱含動畫可讓您建立動畫*範本*，並藉由觸發程序連結。 這些觸發程序是來自明確指派所造成的屬性變更。 您可將範本定義為單一動畫或動畫群組。 動畫群組是一組動畫範本集合，可藉由明確指派或觸發程序來一起啟動。 隱含動畫可讓您在每次要變更值的屬性並看見其產生動畫效果時，免去建立明確 KeyFrameAnimations 的需求。

**Expression 動畫**是 Windows 10 11 月更新 (組建 10586) 在視覺層中引入的動畫類型。
運算式動畫的背景概念，係指您可在 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 屬性與離散值之間建立數學關係，之後再於每個畫面進行評估與更新。 您可參考位於 Composition 物件上的屬性或屬性集、使用數學函式協助程式，甚至可參考輸入以衍生這些數學關係。 Expression 可在 Windows 平台上，提供諸如視差與固定式標頭的順暢使用體驗。

## <a name="why-composition-animations"></a>為何使用組合動畫？

**效能** 建置通用 Windows 應用程式時，會在 UI 執行緒上執行您大部分的程式碼。
為了確保讓動畫能夠在各個不同的裝置類別上順暢運作，系統會執行動畫計算並於獨立執行緒上運作，以維持 60 FPS 的執行效能。
這表示您可以仰賴系統提供順暢的動畫，同時您的應用程式可執行其他複雜的運算以提供進階的使用者體驗。

**可能性** 視覺層中組合動畫的目標是讓建立外型美觀的 UI 更為輕鬆。 我們想要提供給您不同類型的動畫，讓您更輕鬆地打造酷炫的想法。

**範本化** 視覺層中的所有組合動畫皆為範本 – 這表示您可在多個物件上使用動畫，而無須建立個別的動畫。
這可讓您使用相同的動畫並調整屬性或參數，以符合其他的需求，但是又無須擔憂妨礙先前的使用。

您可以查看我們的 [Expression Animations (Expression 動畫)](https://channel9.msdn.com/events/Build/2016/P486)、[Interactive Experiences (互動式體驗)](https://channel9.msdn.com/Events/Build/2016/P405)、[Implicit Animations (隱含動畫)](https://channel9.msdn.com/events/Build/2016/P484) 和 [Connected Animations (連接動畫)](https://channel9.msdn.com/events/Build/2016/P485) //BUILD 討論，來了解一些可能的範例。

您亦可查看 [Composition GitHub](http://go.microsoft.com/fwlink/?LinkID=789439)，了解 API 使用方式以及逼真度更高的 API 運作相關資訊。

## <a name="what-can-you-animate-with-composition-animations"></a>您可使用組合動畫產生哪些動畫效果？

組合動畫可套用至大部分的組合物件屬性，例如 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 和 **InsetClip**。
您亦可將組合動畫套用至組合效果和屬性集。 **選擇要產生動畫效果的項目時請記下類型 – 使用此類型來判斷您所建構的 KeyFrame 動畫類型，或是運算式必須解析的類型。**

### <a name="visual"></a>Visual

|可產生動畫效果的 Visual 屬性|類型|
|------|------|
|AnchorPoint|Vector2|
|CenterPoint|Vector3|
|Offset|Vector3|
|Opacity|純量|
|Orientation|四元數|
|RotationAngle|純量|
|RotationAngleInDegrees|純量|
|RotationAxis|Vector3|
|Scale|Vector3|
|Size|Vector2|
|TransformMatrix*|Matrix4x4|
* 如果您想要將整個 TransformMatrix 屬性的動畫效果產生為 Matrix4x4，則必須使用 ExpressionAnimation 執行此動作。
或者，您亦可鎖定矩陣的個別儲存格並在其中使用 KeyFrame 或 ExpressionAnimation。

### <a name="insetclip"></a>InsetClip

|可產生動畫效果的 InsetClip 屬性|類型|
|-------------------------------|-------|
|BottomInset|純量|
|LeftInset|純量|
|RightInset|純量|
|TopInset|純量|

## <a name="visual-sub-channel-properties"></a>Visual 子通道屬性

除了能夠產生 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 屬性動畫效果外，您亦可鎖定這些屬性的*「子通道」*元件產生動畫效果。
例如，動畫效果只需要有 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 X Offset，而非整個 Offset。
動畫可鎖定 Vector3 Offset 屬性，或是 Offset 屬性的純量 X 元件。
除了能夠鎖定屬性的個別子通道元件外，您亦可鎖定多個元件。
例如，您可鎖定 Scale 的 X 和 Y 元件。

|可產生動畫效果的 Visual 子通道屬性|類型|
|----------------------------------------|------|
|AnchorPoint.x y|純量|
|AnchorPoint.xy|Vector2|
|CenterPoint.x, y, z|純量|
|CenterPoint.xy, xz, yz|Vector2|
|Offset.x, y, z|純量|
|Offset.xy, xz, yz|Vector2|
|RotationAxis.x, y, z|純量|
|RotationAxis.xy, xz, yz|Vector2|
|Scale.x, y, z|純量|
|Scale.xy, xz, yz|Vector2|
|Size.x, y|純量|
|Size.xy|Vector2|
|TransformMatrix._11 ... TransformMatrix._NN,|純量|
|TransformMatrix._11_12 ... TransformMatrix._NN_NN|Vector2|
|TransformMatrix._11_12_13 ... TransformMatrix._NN_NN_NN|Vector3|
|TransformMatrix._11_12_13_14|Vector4|
|Color*|Colors (Windows.UI)|

*產生 Brush 屬性 Color 子通道的動畫效果時會略有差異。 您將 StartAnimation() 連結至 Visual.Brush 並宣告屬性，以在參數中產生「色彩」動畫效果。
(稍候會討論關於產生色彩動畫效果的更多詳細資料)

## <a name="property-sets-and-effects"></a>屬性集與效果

除了產生 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 與 InsetClip 屬性動畫效果外，您亦可產生 PropertySet 或效果屬性動畫效果。
針對屬性集，您會定義屬性並將其儲存於組合屬性集。稍後可將該屬性做為動畫的目標 (此外亦會同時由其他動畫參考)。 在下列小節中將會詳細說明此資訊。

針對效果，您可使用組合效果 API 定義圖形效果 (如需[效果概觀](./composition-effects.md)，請參閱這裡)。
除了定義效果外，您亦可產生效果屬性值的動畫效果。
透過鎖定 Sprite Visual 上 Brush 屬性的屬性元件，即完成此動作。

## <a name="quick-formula-getting-started-with-composition-animations"></a>快速公式︰開始使用組合動畫

深入瞭解關於各個不同動畫類型建構與使用方式的詳細資訊前，您可參閱以下的快速高階公式，了解組合動畫的配置方式。

1. 取得組合器。 這可能是來自頁面或您建立動畫的 FrameworkElement。
1. 建立新的動畫物件 - 此物件可為 KeyFrame 或 Expression 動畫。
    * 針對 KeyFrame 動畫，確認您建立的 KeyFrame 動畫類型符合想要產生動畫效果的屬性類型。
    * Expression 動畫僅具有單一類型。
1. 定義動畫內容 – 插入 Keyframe 或定義 Expression 字串
    * 針對 KeyFrame 動畫，確認 KeyFrame 值類型與您想要產生動畫效果的屬性相同。
    * 針對 Expression 動畫，確認 Expression 字串將解析為與您想要產生動畫效果之屬性相同的類型。
1. 針對您想要產生動畫效果的 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 屬性開始動畫 – 其稱為 StartAnimation 且可包含作為參數：您想要產生動畫效果 (使用字串格式) 之屬性名稱與動畫物件。

```cs
// KeyFrame Animation Example to target Opacity property
// Step 1 - Get the compositor
_compositor = ElementCompositionPreview.GetElementVisual(this).Compositor;

// Step 2 - Create your animation object
var animation = _compositor.CreateScalarKeyFrameAnimation();

// Step 3 - Define Content
animation.Duration = TimeSpan.FromSeconds(1);
animation.InsertKeyFrame(1f, 0.2f);

// Step 4 - Attach animation to Visual property and start animation
_targetVisual.StartAnimation(nameof(Visual.Opacity), animation);

// Expression Animation Example to target Opacity property
// Step 2 - Create your animation object
var expression = _compositor.CreateExpressionAnimation();
// Step 3 - Define Content (you can also define the string as part of the expression object
// declaration)
expression.Expression = "targetVisual.Offset.X / windowWidth";
expression.SetReferenceParameter("targetVisual", _target);
expression.SetScalarParameter("windowWidth", _xSizeWindow);
// Step 4 - Attach animation to Visual property and start animation
_targetVisual.StartAnimation("Opacity", expression);

```

## <a name="using-keyframe-animations"></a>使用 KeyFrame 動畫

KeyFrame 動畫是時間型動畫，這種動畫使用一或多個主要畫面來指定動畫值應如何隨時間變更。
畫面代表標記或控制點，可讓您定義在特定時間應該有什麼動畫值。

### <a name="creating-your-animation-and-defining-keyframes"></a>建立您的動畫與 KeyFrame

若要建構 KeyFrame 動畫，請使用與您想要產生動畫效果之屬性類型相互關聯的 Compositor 物件建構函數方法。
KeyFrame 動畫具有以下各種不同的類型：

* ColorKeyFrameAnimation
* QuaternionKeyFrameAnimation
* ScalarKeyFrameAnimation
* Vector2KeyFrameAnimation
* Vector3KeyFrameAnimation
* Vector4KeyFrameAnimation

以下範例會建立 Vector3 KeyFrame 動畫：

```cs
var animation = _compositor.CreateVector3KeyFrameAnimation();
```

建構每個 KeyFrame 動畫時，會插入定義兩個元件 (可選擇插入第三個元件) 的個別 KeyFrame 區段

* 時間：介於 0.0 – 1.0 之間的標準化 KeyFrame 進度狀態。
* 值：動畫值在時間狀態的特定值。
* (選擇性) Easing 函數：描述上一個 KeyFrame 與目前 KeyFrame 之間內插補點的函數 (在本主題後面部分會加以討論)。

以下範例會在動畫的中間點插入 KeyFrame：

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```

**注意：**使用 KeyFrame 動畫產生色彩動畫效果時，請格外留意以下事項：

1. 將 StartAnimation 連結至 Visual.Brush 而非 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx)，且使用 **Color** 做為想要產生動畫效果的屬性值。
1. KeyFrame 的「值」元件，是由 Windows.UI 命名空間外部的色彩物件所定義。
1. 您可選擇藉由設定 InterpolationColorSpace 屬性，以定義內插補點的色彩空間。 可能的值包括：
    * CompositionColorSpace.Rgb
    * CompositionColorSpace.Hsl

## <a name="keyframe-animation-properties"></a>KeyFrame 動畫屬性

定義 KeyFrame 動畫與個別的 KeyFrame 後，您即可在動畫之外定義多個屬性：

* DelayTime – 在呼叫 StartAnimation() 之後、動畫開始之前的時間。
* Duration – 動畫的持續時間。
* IterationBehavior – 動畫的計數或無限重複行為。
* IterationCount –「KeyFrame 動畫」將重複的有限次數。
* KeyFrame Count – 特定 KeyFrame 動畫中的 KeyFrame 讀數。
* StopBehavior – 指定呼叫 StopAnimation 時，動畫屬性值的行為。
* Direction – 指定動畫播放的方向。

以下範例會將動畫的持續時間設為 5 秒：

```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

## <a name="easing-functions"></a>Easing 函數

Easing 函數 (CompositionEasingFunction) 會指出中繼值如何從上一個主要畫面值進展到目前主要畫面值。 如果您沒有為 KeyFrame 提供 Easing 函數，將會使用預設的曲線。

支援的 Easing 函數類型有三種：

* 線性
* 三次方貝茲
* 步驟

三次方貝茲是一種參數函數，經常用來描述可調整的平滑曲線。 搭配使用組合 KeyFrame 動畫時，您可定義兩個 Vector2 物件控制點。 這些控制點用來定義曲線的形狀。 建議您使用類似[此處](http://cubic-bezier.com/#0,-0.01,.48,.99)的網站，透過視覺化呈現方式了解這兩個控制點如何建構三次方貝茲的曲線。

若要建立 Easing 函式，請使用 Compositor 物件以外的建構函式方法。 以下兩個範例會建立線性 Easing 函式和基本三次方貝茲 Easing 函式。

```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
var step = _compositor.CreateStepEasingFunction();
```

若要將 Easing 函式新增至 KeyFrame，只要在將 KeyFrame 插入動畫時，將第三個參數新增至 KeyFrame 即可：

以下範例會使用 KeyFrame 新增至 easeIn Easing 函數：

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

## <a name="starting-and-stopping-keyframe-animations"></a>開始和停止 KeyFrame 動畫

定義動畫與 KeyFrame 後，您即可隨時連結動畫。 開始動畫時，您可指定要產生動畫效果的 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx)、要產生動畫效果的目標屬性以及動畫的參考。
您可呼叫 StartAnimation() 函式執行此動作。 請謹記，在屬性上呼叫 StartAnimation() 將會中斷連線，並移除先前執行的所有動畫。
**注意︰**您選擇要產生動畫效果的屬性參考係採用字串形式。

以下範例會在 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 Offset 屬性上設定和開始動畫：

```cs
targetVisual.StartAnimation("Offset", animation);
```

若您想要鎖定子通道屬性，可將 subchannel 新增至定義想要產生動畫效果屬性的字串。
在以上範例中，使用的語法會變更為 StartAnimation("Offset.X, animation2)，其中 animation2 為 ScalarKeyFrameAnimation。

開始動畫後，亦可在動畫結束前將其停止。 使用 StopAnimation() 函式可完成此動作。
以下範例會在 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 Offset 屬性上停止動畫：

```cs
targetVisual.StopAnimation("Offset");
```

您亦可定義動畫明確停止時的行為。 若要執行此動作，您可在動畫以外定義停止行為屬性。 提供三種選項：

* LeaveCurrentValue︰動畫會將已產生動畫效果的屬性值，標示為動畫的最後一個計算值。
* SetToFinalValue︰動畫會將已產生動畫效果的屬性值，標示為最後一個 keyframe 的值。
* SetToInitialValue︰動畫會將已產生動畫效果的屬性值，標示為第一個 keyframe 的值。

以下範例會設定 KeyFrame 動畫的 StopBehavior 屬性：

```cs
animation.StopBehavior = AnimationStopBehavior.LeaveCurrentValue;
```

## <a name="animation-completion-events"></a>動畫完成事件

KeyFrame 動畫可讓您在完成選定的動畫 (或動畫群組) 時，使用動畫批次執行彙總。
僅可批次處理 KeyFrame 動畫完成事件。 Expression 沒有明確的結束，因此不會觸發完成事件。
若在批次內部開始 Expression 動畫，則會如常執行動畫且不會影響批次觸發。

當批次內的所有動畫都完成時，就會觸發批次完成事件。
觸發批次事件所需的時間，取決於批次中最長或延遲最久的動畫。
當您需要知道所選取的幾組動畫何時完成以排定其他部分工作時，彙總結束狀態會相當有用。

一旦觸發完成事件，即會處置批次。 您亦可隨時呼叫 Dispose() 以提早釋出資源。
若批次處理動畫提早結束，且您不想挑選完成事件，則可能會想要手動處置批次物件。
若中斷或取消動畫，則會觸發完成事件並計入其中設定的批次。
在 [Windows/Composition GitHub](http://go.microsoft.com/fwlink/p/?LinkId=789439) 的 Animation_Batch SDK 範例中，會示範此程序。

## <a name="scoped-batches"></a>限定範圍的批次

若要彙總特定動畫群組或鎖定單一動畫完成事件，您可建立限定範圍的批次。

```cs
CompositionScopedBatch myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
```

建立「限定範圍的」批次之後，所有已開始的動畫都會進行彙總，直到使用 Suspend 或 End 將批次明確暫停或結束為止。

呼叫 Suspend 會停止彙總動畫結束狀態， 直到呼叫 Resume 為止。 這可讓您明確排除來自指定之批次的內容。

在以下範例中，鎖定 VisualA 位移屬性的動畫將不會納入批次中：

```cs
myScopedBatch.Suspend();
VisualA.StartAnimation("Offset", myAnimation);
myScopeBatch.Resume();
```

若要完成您的批次，您必須呼叫 End()。 如果沒有 End 呼叫，批次就會讓不斷進行收集的物件保持開啟。

下列程式碼片段與圖表，會顯示範例說明批次如何彙總動畫來追蹤結束狀態。
請注意，在此範例中，動畫 1、3 和 4 具有由此批次追蹤的結束狀態，但動畫 2 則否。

```cs
myScopedBatch.End();
CompositionScopedBatch myScopedBatch =  _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
// Start Animation1
[…]
myScopedBatch.Suspend();
// Start Animation2
[…]
myScopedBatch.Resume();
// Start Animation3
[…]
// Start Animation4
[…]
myScopedBatch.End();
```

![限定範圍的批次包含動畫一、動畫三及動畫四，而將動畫二排除在限定範圍的批次之外](./images/composition-scopedbatch.png)

## <a name="batching-a-single-animations-completion-event"></a>批次處理單一動畫的完成事件

若您想要知道單一動畫何時結束，則必須建立限定範圍的批次，其僅會包含您目前鎖定的動畫。 例如：

```cs
CompositionScopedBatch myScopedBatch =  _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation("Opacity", myAnimation);
myScopedBatch.End();
```

## <a name="retrieving-a-batchs-completion-event"></a>擷取批次完成事件

批次處理一或多個動畫時，將會依照相同方式擷取批次完成事件。
針對鎖定批次的已完成事件，註冊事件處理方法。

```cs
myScopedBatch.Completed += OnBatchCompleted;
```

## <a name="batch-states"></a>批次狀態

您可使用兩種屬性來判斷現有批次的狀態；IsActive 與 IsEnded。

若鎖定的批次接受進行動畫彙總，則 IsActive 屬性會傳回 true。 批次暫停或結束時，IsActive 會傳回 false。

若您無法將動畫新增至該特定批次，則 IsEnded 屬性會傳回 true。 若您針對特定批次明確呼叫 End()，則會結束批次。

## <a name="using-expression-animations"></a>使用 Expression 動畫

Expression 動畫是在 Windows 10 11 月更新中引入的新動畫類型 (10586)。
高階 Expression 動畫是以離散值間的數學方程式/關係，以及其他組合物件屬性的參考做為基礎。
相較於使用 interpolator 函數 (三次方貝茲、四次方、五次方等等) 描述值如何隨著時間變更的 KeyFrame 動畫，Expression 動畫係使用數學方程式來定義每個畫面的動畫值計算方式。
在此必須指出 Expression 動畫並無定義的持續期間 – 一旦開始 Expression 動畫，其即會執行並使用數學方程式判斷動畫屬性值，直到將其明確停止為止。

**Expression 動畫為何如此實用？**

Expression 動畫的真正優勢，在於其可建立數學關係來包含其他物件上之參數或屬性的參考。
這表示您可使用參考位於其他組合物件、區域變數的屬性值，甚或是位於組合屬性集之共用值的方程式。
由於採用此參考模式，且會針對每個畫面計算方程式，因此若值定義方程式變更，則方程式的輸出亦會隨之變更。
相較於值必須離散且預先定義的傳統 KeyFrame 動畫，此功能特色開啟了無限可能。
例如，您可使用 Expression 動畫輕鬆描述諸如固定式標頭和視差等體驗。

**注意：**我們使用「Expression」或「Expression 字串」一詞，稱呼 Expression 動畫物件之數學方程式。

## <a name="creating-and-attaching-your-expression-animation"></a>建立與連結 Expression 動畫

探討關於建立 Expression 動畫的語法資訊之前，必須提及幾項核心原則：

* Expression 動畫會使用定義的數學方程式，判斷每個畫面的動畫屬性值。
* 數學方程式會採用字串形式輸入至 Expression。
* 數學方程式的輸出，必須解析為與您想要產生動畫效果之屬性相同的類型。 若兩者不相符，則在計算 Expression 時會發生錯誤。 若方程式解析為 Nan (數字/0)，則系統會使用上次計算的值。
* Expression 動畫具有*無限的存留期* – 其會持續執行直到停止。

若要建立 Expression 動畫，只要在組合物件以外使用建構函數，並在其中定義數學運算式即可。

在以下的建構函數範例中，會定義加總兩個純量值的極基本運算式 (我們會在下一節中深入探討更複雜的運算式)：

```cs
var expression = _compositor.CreateExpressionAnimation("0.2 + 0.3");
```

與 KeyFrame 動畫類似，一旦您定義 Expression 動畫，即必須將其連結至 Visual 並宣告想要產生動畫效果的動畫屬性。
下面會接續上述範例，將 Expression 動畫連結至 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 Opacity 屬性 (純量類型)：

```cs
targetVisual.StartAnimation("Opacity", expression);
```

## <a name="components-of-your-expression-string"></a>Expression 字串的元件

在上一節的範例中，已說明如何加總兩個簡易純量值。
雖然此為有效的 Expression 範例，但其並未完全展現您可使用 Expression 執行的所有功能。
關於上述範例請注意一點，由於這些值為離散值，因此方程式會將每個畫面解析為 0.5，且在整個動畫存留期皆不會有所更動。
Expression 的真正功能優勢，在於其可定義數學關係來定期或隨時變更值。

讓我們逐步檢視構成這些 Expression 類型的各個不同部分。

### <a name="operators-precedence-and-associativity"></a>運算子、優先順序及關聯性

Expression 字串支援使用您想要描述各個不同方程式元件間數學關係的一般運算子：

|類別| 運算子|
|--------|-----------|
|一元| -|
|乘法|* /|
|加法|+ -|
|Mod| %|

同樣地，評估 Expression 時會遵守「C# 語言規格」中定義的運算子優先順序與關聯性。
換言之，其會遵守基本操作順序。

在以下範例中，執行評估時會先解析括號，然後再根據操作順序解析方程式的其餘部分：

```cs
"(5.0 * (72.4 – 36.0) + 5.0" // (5.0 * 36.4 + 5) -> (182 + 5) -> 187
```

### <a name="property-parameters"></a>屬性參數

屬性參數是 Expression 動畫的最強大元件之一。
在 Expression 字串中，您可參考來自諸如組合 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx)、組合屬性集等其他物件，或是其他 C# 物件的屬性值。

若要在 Expression 字串中使用這些值，僅須將參考定義為 Expression 動畫的參數即可。
將 Expression 使用的字串對應至實際物件，以完成此動作。 這可讓系統在評估方程式時，了解計算值所需檢查的項目。
提供各種不同類型的參數，其可與您想要在方程式中包含的物件類型相互關聯：

|類型|建立參數的函數|
|----|------------------------------|
|純量|SetScalarParameter(String ref, Scalar obj)|
|向量|SetVector2Parameter(String ref, Vector2 obj)<br/>SetVector3Parameter(String ref, Vector3 obj)<br/>SetVector4Parameter(String ref, Vector4 obj)|
|矩陣|SetMatrix3x2Parameter(String ref, Matrix3x2 obj)<br/>SetMatrix4x4Parameter(String ref, Matrix4x4 obj)|
|四元數|SetQuaternionParameter(String ref, Quaternion obj)|
|色彩|SetColorParameter(String ref, Color obj)|
|CompositionObject|SetReferenceParameter(String ref, Composition object obj)|
|布林值| SetBooleanParameter(String ref, Boolean obj)|

在以下範例中，我們會建立 Expression 動畫，它會參考兩個其他組合 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 與基礎 System.Numerics Vector3 物件的 Offset。

```cs
var commonOffset = new Vector3(25.0, 17.0, 10.0);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + additionalOffset");
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetVector3Parameter("additionalOffset", commonOffset);
```

此外，您可使用上述的相同模式，透過運算式參考屬性集中的值。
組合屬性集是一種實用的方式，可用來儲存動畫使用的資料，以及建立未繫結至其他所有組合物件存留期的可共用、可重複使用資料。
您可在運算式中參考屬性集值，與其他屬性參考類似。 (在稍後章節中將深入探討關於屬性集的詳細資訊)

我們可直接修改上述範例，這類屬性集用於定義 commonOffset 而非區域變數：

```cs
_sharedProperties = _compositor.CreatePropertySet();
_sharedProperties.InsertVector3("commonOffset", offset);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + sharedProperties.commonOffset");
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
```

最後在參考其他物件的屬性時，亦可參考位於 Expression 字串中或屬於參考參數一部份的子通道屬性。

在以下範例中，我們參考來自兩個 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 Offset 屬性 x 子通道 – 其中一個位於 Expression 字串本身，另一個則在建立參數參考時使用。
請注意，在參考 Offset 的 X 元件時，我們會將參數類型變更為純量參數，而非如先前範例般變更為 Vector3：

```cs
var expression = _compositor.CreateExpressionAnimation("xOffset/ ParentOffset.X");
expression.SetScalarParameter("xOffset", childVisual.Offset.X);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
```

### <a name="expression-helper-functions-and-constructors"></a>Expression 協助程式函數與建構函數

除了可存取運算式與屬性參數外，您可運用數學函數清單以在運算式中使用。
提供這些函數，以針對您想要比照 System.Numerics 物件採取類似作業的各個不同類型，執行計算和運算作業。

以下範例會建立針對純量的 Expression，其會運用 Clamp 協助程式函數︰

```cs
var expression = _compositor.CreateExpressionAnimation("Clamp((scroller.Offset.y * -1.0) – container.Offset.y, 0, container.Size.y – header.Size.y)"
```

除了協助程式函數清單外，您亦可使用 Expression 字串當中的內建建構函數方法，其會根據提供的參數產生該類型的執行個體。

以下範例會建立在 Expression 字串中定義新 Vector3 的 Expression：

```cs
var expression = _compositor.CreateExpressionAnimation("Offset / Vector3(targetX, targetY, targetZ");
```

您可在＜附錄＞一節找到完整大量的協助程式函數與建構函數清單，或是參閱以下清單中的每個類型：

* [純量](#scalar)
* [Vector2](#vector2)
* [Vector3](#vector3)
* [Matrix3x2](#matrix3x2)
* [Matrix4x4](#matrix4x4)
* [四元數](#quaternion)
* [色彩](#color)

### <a name="expression-keywords"></a>Expression 關鍵字

您可運用在評估 Expression 字串時經過不同處理的特殊「關鍵字」。
由於其會視為「關鍵字」，因此不會用來做為屬性參考的字串參數部分。

|關鍵字|描述|
|-------|--------------|
|This.StartingValue| 提供要產生動畫效果之原始開始屬性值的參考。|
|This.CurrentValue|提供目前「已知」屬性值的參考|
|Pi| 提供 PI 值的關鍵字參考|

以下範例說明如何使用 this.StartingValue 關鍵字：

```cs
var expression = _compositor.CreateExpressionAnimation("this.StartingValue + delta");
```

### <a name="expressions-with-conditionals"></a>條件式 Expression

除了支援使用運算紫、屬性參考以及函數與建構函數的數學關係外，您亦可建立包含三元運算子的運算式：

```cs
(condition ? ifTrue_expression : ifFalse_expression)
```

條件式陳述式可讓您撰寫運算式，根據特定條件，系統會使用不同的數學關係來計算產生動畫效果的屬性值。
三元運算子可為 true 或 false 陳述式的巢狀運算式。

條件式陳述式支援下列的條件式運算子：

* 等於 (==)
* 不等於 (!=)
* 小於 (&lt;)
* 小於或等於 (&lt;=)
* 大於 (&gt;)
* 大於或等於 (&gt;=)

在條件陳述式中支援使用下列連接詞做為運算子或函數：

* 非：! / Not(bool1)
* 且：&amp;&amp; / And(bool1, bool2)
* 或：|| / Or(bool1, bool2)

以下是使用條件式的 Expression 動畫範例。

```cs
var expression = _compositor.CreateExpressionAnimation("target.Offset.x > 50 ? 0.0f + (target.Offset.x / parent.Offset.x) : 1.0f");
```

## <a name="expression-keyframes"></a>Expression KeyFrame

在本文件前面部分，我們已說明建立 KeyFrame 動畫的方式，並向您介紹 Expression 動畫以及可用於組成 Expression 字串的所有不同項目。 若您想要取得 Expression 動畫的功能優勢，同時亦享有 KeyFrame 動畫提供的時間內差補點，則該如何處理？
Expression KeyFrame 即是您的完美選擇！

您可將值做為 Expression 字串，而無須針對 KeyFrame 動畫中的每個控制點定義離散值。
在此情況下，系統會使用運算式字串，計算時間軸上指定時點所應使用的產生動畫效果屬性值。
系統會如同一般 Keyframe 動畫般，逕行內差補點至此值。

您無須建立特殊動畫來使用 Expression KeyFrame – 只要將 ExpressionKeyFrame 插入至標準 KeyFrame 動畫，提供時間與 Expression 字串做為值即可。 下列範例會說明此動作，使用 Expression 字串做為其中一個 KeyFrame 的值：

```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, "VisualBOffset.X / VisualAOffset.Y");
animation.InsertKeyFrame(1.00f, 0.8f);
```

## <a name="expression-sample"></a>Expression 範例

下列程式碼顯示有關設定運算式動畫提供基本視差體驗，以從捲動檢視器提取值的範例。

```cs
// Get scrollviewer
ScrollViewer myScrollViewer = ThumbnailList.GetFirstDescendantOfType<ScrollViewer>();
_scrollProperties = ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);

// Setup the expression
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 *  ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5   * ItemHeight))";
```

## <a name="animating-with-property-sets"></a>使用屬性集產生動畫效果

組合屬性集能讓您儲存可橫跨多個動畫共用，且不會繫結至另一個組合物件存留器的值。
屬性集在儲存一般值時非常實用，讓您可在稍後於動畫中輕鬆參考這些值
您亦可使用屬性集，根據驅動運算式的應用程式邏輯來儲存資料。

若要建立屬性集，請使用 Compositor 物件以外的建構函數方法：

```cs
_sharedProperties = _compositor.CreatePropertySet();
```

建立您的屬性集之後，您可在其中新增屬性和值：

```cs
_sharedProperties.InsertVector3("NewOffset", offset);
```

與先前探討的內容類似，我們可在 Expression 動畫中參考此屬性集值：

```cs
var expression = _compositor.CreateExpressionAnimation("this.target.Offset + sharedProperties.NewOffset");
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
targetVisual.StartAnimation("Offset", expression);
```

此外亦可針對屬性集值產生動畫效果。 將動畫連結至 PropertySet 物件，然後再參考字串中的屬性名稱，以完成此動作。
下面我們會使用 KeyFrame 動畫，針對屬性集中的 NewOffset 屬性產生動畫效果。

```cs
var keyFrameAnimation = _compositor.CreateVector3KeyFrameAnimation()
keyFrameAnimation.InsertKeyFrame(0.5f, new Vector3(25.0f, 68.0f, 0.0f);
keyFrameAnimation.InsertKeyFrame(1.0f, new Vector3(89.0f, 125.0f, 0.0f);
_sharedProperties.StartAnimation("NewOffset", keyFrameAnimation);
```

您可能想知道若在 App 中執行此程式碼，則 Expression 動畫的連結目標產生動畫效果屬性值會有什麼變化。
在此情況下，運算式會先輸出值，不過一旦 KeyFrame 動畫開始針對屬性集中的屬性產生動畫效果，即會一併更新 Expression 值，這是因為每個畫面皆會計算方程式。 這是使用 Expression 與 KeyFrame 動畫的屬性集功能優勢所在！

## <a name="manipulationpropertyset"></a>ManipulationPropertySet

除了利用 Composition 屬性集之外，開發人員也可以存取 ManipulationPropertySet，便能從 XAML ScrollViewer 存取屬性。 這些屬性之後可在 Expression 動畫中使用及參考，以製作「視差」和「固定式標頭」等體驗。 注意：您可以抓取任何具有可捲動內容的 XAML 控制項 (ListView、GridView 等等) 的 ScrollViewer，並以它取得可捲動控制項的 ManipulationPropertySet。

在您的 Expression 中，您可以參考捲動檢視器的下列屬性：

|屬性| 類型|
|--------|-----|
|Translation| Vector3|
|Pan| Vector3|
|Scale| Vector3|
|CenterPoint| Vector3|
|Matrix| Matrix4x4|

若要取得 ManipulationPropertySet 的參考，利用 ElementCompositionPreview 的 GetScrollViewerManipulationPropertySet 方法。

```csharp
CompositionPropertySet manipPropSet = ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScroller);
```

有了這個屬性集參考之後，您可以參考在屬性集中找到的捲動檢視屬性。 首先要建立參考參數。

```csharp
ExpressionAnimation exp = compositor.CreateExpressionAnimation();
exp.SetReferenceParameter("ScrollManipulation", manipPropSet);
```

設定參考參數之後，您可以參考 Expression 中的 ManipulationPropertySet 屬性。

```csharp
exp.Expression = “ScrollManipulation.Translation.Y / ScrollBounds”;
_target.StartAnimation(“Opacity”, exp);
```

## <a name="using-implicit-animations"></a>使用隱含動畫

動畫是向使用者描述行為的絕佳方法。 有許多方法可讓您的內容產生動畫效果，但目前所有討論的方法都需要您明確地*「開始」*動畫。 雖然這樣可讓您擁有完整的控制權來定義何時要開始動畫，但每當屬性值將要變更就需要產生動畫效果時，就會變得難以管理。 這常發生在應用程式已經從應用程式「邏輯」(其中定義應用程式的核心元件及基礎結構) 分離定義動畫的應用程式「特質」時。 隱含動畫提供更簡潔的方式，從核心應用程式邏輯之外定義動畫。 您可以連結這些要執行的動畫來搭配特定屬性變更觸發程序執行。

### <a name="setting-up-your-implicitanimationcollection"></a>設定 ImplicitAnimationCollection

隱含動畫是由其他 **CompositionAnimation** 物件 (**KeyFrameAnimation** 或 **ExpressionAnimation**) 所定義。 **ImplicitAnimationCollection** 代表一組 **CompositionAnimation** 物件，當符合屬性變更*「觸發程序」*條件時將會開始該組物件。 請注意，在定義動畫時，務必設定 **Target** 屬性，這會定義動畫開始時將會鎖定的 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 屬性。 **Target** 的屬性僅能為可產生動畫的 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 屬性。
在以下的程式碼片段中，會建立單一的 **Vector3KeyFrameAnimation**，並且定義為 **ImplicitAnimationCollection** 的一部分。 **ImplicitAnimationCollection** 接著會附加到 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 **ImplicitAnimation** 屬性，如此當符合觸發程序時，將會開始動畫。

```csharp
Vector3KeyFrameAnimation animation = _compositor.CreateVector3KeyFrameAnimation();
animation.DelayTime =  TimeSpan.FromMilliseconds(index);
animation.InsertExpressionKeyFrame(1.0f, "this.FinalValue");
animation.Target = "Offset";
ImplicitAnimationCollection implicitAnimationCollection = compositor.CreateImplicitAnimationCollection();

visual.ImplicitAnimations = implicitAnimationCollection;
```

### <a name="triggering-when-the-implicitanimation-starts"></a>ImplicitAnimation 開始時觸發

觸發程序是用來描述動畫將以隱含方式開始時的詞彙。 目前的觸發程序定義，是根據 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 上可產生動畫屬性的任何變更而來 – 這些變更是透過對屬性的明確設定所發生。 例如，將 **Offset** 觸發程序放置於 **ImplicitAnimationCollection** 上，並將某個動畫與其產生關聯，則對於目標 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 **Offset** 更新將會使用集合中的動畫，對其新的值產生動畫效果。
透過上述範例，我們加入此額外的程式行，來設定目標 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 **Offset** 屬性的觸發程序。

```csharp
implicitAnimationCollection["Offset"] = animation;
```

請注意，**ImplicitAnimationCollection** 可以有多個觸發程序。 這代表隱含動畫或動畫群組可以因應不同的屬性變更來開始動畫。 在上述的範例中，開發人員可以為其他的屬性 (例如 Opacity) 加入觸發程序。

### <a name="thisfinalvalue"></a>this.FinalValue

在第一個隱含範例中，我們使用了 ExpressionKeyFrame 做為 “1.0” KeyFrame，並指派了 **this.FinalValue** 的運算式給它。 **this.FinalValue** 是運算式語言中的保留字，針對隱含動畫提供辨別行為。 **this.FinalValue** 會將 API 屬性上設定的值繫結到動畫。 這有助於建立真正的範本。 **this.FinalValue** 在明確動畫中並不好用，因為 API 屬性會立即設定，而在隱含動畫的情況下則是會延遲。

## <a name="using-animation-groups"></a>使用動畫群組

**CompositionAnimationGroup** 讓您能輕鬆的方式來將動畫清單分組，並搭配隱含動畫或明確動畫使用。

### <a name="creating-and-populating-animation-groups"></a>建立和填入動畫群組

Compositor 物件的 **CreateAnimationGroup** 方法可讓您建立動畫群組：

```sharp
CompositionAnimationGroup animationGroup = _compositor.CreateAnimationGroup();
animationGroup.Add(animationA);
animationGroup.Add(animationB);
```

動畫群組一旦建立之後，就可將個別的動畫加入到動畫群組。 請記住，您不需要明確地開始個別的動畫 – 當呼叫 **StartAnimationGroup** (明確的情況) 或符合觸發程序 (隱含的情況) 時，這些動畫將會全部開始。
請注意，務必確認加入到群組的動畫已經定義 **Target** 屬性。 這將會定義它們將產生動畫效果的目標 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 屬性。

### <a name="using-animation-groups-with-implicit-animations"></a>使用動畫群組搭配隱含動畫

您可以建立隱含動畫，如此當符合觸發程序時，一組動畫群組形式的動畫便會開始。 在此案例中，動畫群組會定義為符合觸發程序時開始的一組動畫。

```csharp
implicitAnimationCollection["Offset"] = animationGroup;
```

### <a name="using-animation-groups-with-explicit-animations"></a>使用動畫群組搭配明確動畫

您可以建立明確動畫，如此當呼叫 **StartAnimationGroup** 時，已加入群組的個別動畫將會開始。 請注意，在此 **StartAnimation** 呼叫中，群組沒有鎖定的屬性，因為個別的動畫可以鎖定不同的屬性。 請確定已設定每個動畫的目標屬性。

```csharp
visual.StartAnimationGroup(AnimationGroup);
```

### <a name="e2e-sample"></a>E2E 範例

此範例示範當設定新的值時，以隱含方式產生 Offset 屬性的動畫效果。

```csharp
class PropertyAnimation
{
    PropertyAnimation(Compositor compositor, SpriteVisual heroVisual, SpriteVisual listVisual)
    {
      // Define ImplicitAnimationCollection
        ImplicitAnimationCollection implicitAnimations =
        compositor.CreateImplicitAnimationCollection();

        // Trigger animation when the “Offset” property changes.
        implicitAnimations["Offset"] = CreateAnimation(compositor);

        // Assign ImplicitAnimations to a visual. Unlike Visual.Children,
        // ImplicitAnimations can be shared by multiple visuals so that they
        // share the same implicit animation behavior (same as Visual.Clip).
        heroVisual.ImplicitAnimations = implicitAnimations;

        // ImplicitAnimations can be shared among visuals
        listVisual.ImplicitAnimations = implicitAnimations;

        listVisual.Offset = new Vector3(20f, 20f, 20f);
    }

    Vector3KeyFrameAnimation CreateAnimation(Compositor compositor)
    {
        Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
        animation.InsertExpressionKeyFrame(0f, "this.StartingValue");
        animation.InsertExpressionKeyFrame(1f, "this.FinalValue");
        animation.Target = “Offset”;
        animation.Duration = TimeSpan.FromSeconds(0.25);
        return animation;
    }
}
```

## <a name="appendix"></a>附錄

### <a name="expression-functions-by-structure-type"></a>依結構類型的 Expression 函數

### <a name="scalar"></a>純量

|函數和建構函數操作| 描述|
|-----------------------------------|--------------|
|Abs(Float value)| 傳回代表浮點參數絕對值的浮點數|
|Clamp(Float value, Float min, Float max)| 若值小於最小值或最大值，或是值大於最大值，則會傳回大於最小值以及小於最大值或最小值的浮點值|
|Max (Float value1, Float value2)| 傳回介於 value1 與 value2 之間的更大浮點數。|
|Min (Float value1, Float value2)| 傳回介於 value1 與 value2 之間的更小浮點數。|
|Lerp(Float value1, Float value2, Float progress)| 傳回代表根據進度計算兩個純量值間線性內差補點的浮點數 (注意：進度值介於 0.0 與 1.0 之間)|
|Slerp(Float value1, Float value2, Float progress)| 傳回代表根據進度計算兩個浮點數值間球面內差補點的浮點數 (注意：進度值介於 0.0 與 1.0 之間)|
|Mod(Float value1, Float value2)| 傳回劃分 value1 與 value2 所產生的浮點數剩餘部分|
|Ceil(Float value)| 傳回四捨五入至更大整數的浮點數參數|
|Floor(Float value)| 傳回四捨五入至更小整數的浮點數參數|
|Sqrt(Float value)| 傳回浮點數參數的平方根|
|Square(Float value)| 傳回浮點數參數的平方|
|Sin(Float value1)| 傳回浮點數參數的 Sin|
|Asin(Float value2)| 傳回浮點數參數的 ArcSin|
|Cos(Float value1)| 傳回浮點數參數的 Cos|
|ACos(Float value2)| 傳回浮點數參數的 ArcCos|
|Tan(Float value1)| 傳回浮點數參數的 Tan|
|ATan(Float value2)| 傳回浮點數參數的 ArcTan|
|Round(Float value)| 傳回四捨五入至最近整數的浮點數參數|
|Log10(Float value)| 傳回浮點數參數的對數 (基數 10) 結果|
|Ln(Float value)| 傳回浮點數參數的自然對數結果|
|Pow(Float value, Float power)| 傳回增加至特殊次方的浮點數結果|
|ToDegrees(Float radians)| 傳回轉換為度的浮點數參數|
|ToRadians(Float degrees)| 傳回轉換為弧度的浮點數參數|

### <a name="vector2"></a>Vector2

|函數和建構函數操作|描述|
|-----------------------------------|--------------|
|Abs (Vector2 value)|傳回已將絕對值套用至每個元件的 Vector2|
|Clamp (Vector2 value1, Vector2 min, Vector2 max)|傳回針對每個個別元件包含限制值的 Vector2|
|Max (Vector2 value1, Vector2 value2)|傳回針對 value1 和 value2 的每個對應元件執行 Max 的 Vector2|
|Min (Vector2 value1, Vector2 value2)|傳回針對 value1 和 value2 的每個對應元件執行 Min 的 Vector2|
|Scale(Vector2 value, Float factor)|傳回將每個向量元件乘以縮放比例係數的 Vector2。|
|Transform(Vector2 value, Matrix3x2 matrix)|傳回在 Vector2 與 Matrix3x2 間執行線性轉換產生的 Vector2 (亦即向量乘以矩陣)。|
|Lerp(Vector2 value1, Vector2 value2, Float progress)|傳回代表根據進度計算兩個 Vector2 值間線性內差補點的 Vector2 (注意：進度值介於 0.0 與 1.0 之間)|
|Length(Vector2 value)|傳回代表 Vector2 長度/大小的浮點數值|
|LengthSquared(Vector2)|傳回代表 Vector2 長度/大小平方的浮點數值|
|Distance(Vector2 value1, Vector2 value2)|傳回代表兩個 Vector2 值間距離的浮點數值|
|DistanceSquared(Vector2 value1, Vector2 value2)|傳回代表兩個 Vector2 值間距離平方的浮點數值|
|Normalize(Vector2 value)|傳回代表已標準化所有元件之參數單位向量的 Vector2|
|Vector2(Float x, Float y)|使用兩個浮點參數建構 Vector2|

### <a name="vector3"></a>Vector3

|函數和建構函數操作|描述|
|-----------------------------------|--------------|
|Abs (Vector3 value)|傳回已將絕對值套用至每個元件的 Vector3|
|Clamp (Vector3 value1, Vector3 min, Vector3 max)|傳回針對每個個別元件包含限制值的 Vector3|
|Max (Vector3 value1, Vector3 value2)|傳回針對 value1 和 value2 的每個對應元件執行 Max 的 Vector3|
|Min (Vector3 value1, Vector3 value2)|傳回針對 value1 和 value2 的每個對應元件執行 Min 的 Vector3|
|Scale(Vector3 value, Float factor)|傳回將每個向量元件乘以縮放比例係數的 Vector3。|
|Lerp(Vector3 value1, Vector3 value2, Float progress)|傳回代表根據進度計算兩個 Vector3 值間線性內差補點的 Vector3 (注意：進度值介於 0.0 與 1.0 之間)|
|Length(Vector3 value)|傳回代表 Vector3 長度/大小的浮點數值|
|LengthSquared(Vector3)|傳回代表 Vector3 長度/大小平方的浮點數值|
|Distance(Vector3 value1, Vector3 value2)|傳回代表兩個 Vector3 值間距離的浮點數值|
|DistanceSquared(Vector3 value1, Vector3 value2)|傳回代表兩個 Vector3 值間距離平方的浮點數值|
|Normalize(Vector3 value)|傳回代表已標準化所有元件之參數單位向量的 Vector3|
|Vector3(Float x, Float y, Float z)|使用三個浮點參數建構 Vector3|

### <a name="vector4"></a>Vector4

|函數和建構函數操作|描述|
|-----------------------------------|--------------|
|Abs (Vector4 value)|傳回已將絕對值套用至每個元件的 Vector3|
|Clamp (Vector4 value1, Vector4 min, Vector4 max)|傳回針對每個個別元件包含限制值的 Vector4|
|Max (Vector4 value1 Vector4 value2)|傳回針對 value1 和 value2 的每個對應元件執行 Max 的 Vector4|
|Min (Vector4 value1 Vector4 value2)|傳回針對 value1 和 value2 的每個對應元件執行 Min 的 Vector4|
|Scale(Vector3 value, Float factor)|傳回將每個向量元件乘以縮放比例係數的 Vector3。|
|Transform(Vector4 value, Matrix4x4 matrix)|傳回在 Vector4 與 Matrix4x4 間執行線性轉換產生的 Vector4 (亦即向量乘以矩陣)。|
|Lerp(Vector4 value1, Vector4 value2, Float progress)|傳回代表根據進度計算兩個 Vector4 值間線性內差補點的 Vector4 (注意：進度值介於 0.0 與 1.0 之間)|
|Length(Vector4 value)|傳回代表 Vector4 長度/大小的浮點數值|
|LengthSquared(Vector4)|傳回代表 Vector4 長度/大小平方的浮點數值|
|Distance(Vector4 value1, Vector4 value2)|傳回代表兩個 Vector4 值間距離的浮點數值|
|DistanceSquared(Vector4 value1, Vector4 value2)|傳回代表兩個 Vector4 值間距離平方的浮點數值|
|Normalize(Vector4 value)|傳回代表已標準化所有元件之參數單位向量的 Vector4|
|Vector4(Float x, Float y, Float z, Float w)|使用四個浮點參數建構 Vector4|

### <a name="matrix3x2"></a>Matrix3x2

|函數和建構函數操作|   描述|
|-----------------------------------|--------------|
|Scale(Matrix3x2 value, Float factor)|  傳回將每個矩陣元件乘以縮放比例係數的 Matrix3x2。|
|Inverse(Matrix 3x2 value)| 傳回代表倒數矩陣的 Matrix3x2 物件|
|Matrix3x2(Float M11, Float M12, Float M21, Float M22, Float M31, Float M32)|   使用 6 個浮點數參數建構 Matrix3x2|
|Matrix3x2.CreateFromScale(Vector2 scale)|  透過代表縮放比例的 Vector2 建構 Matrix3x2<br/>\[scale.X, 0.0<br/> 0.0, scale.Y<br/> 0.0, 0.0 \]|
|Matrix3x2.CreateFromTranslation(Vector2 translation)|  透過代表平移的 Vector2 建構 Matrix3x2<br/>\[1.0, 0.0,<br/> 0.0, 1.0,<br/> translation.X, translation.Y\]|
|Matrix3x2.CreateSkew(Float x, Float y, Vector2 centerpoint)| 透過兩個浮點數和代表扭曲的 Vector2 建構 Matrix3x2<br/>\[1.0, Tan(y),<br/>Tan(x), 1.0,<br/>-centerpoint.Y * Tan(x), -centerpoint.X * Tan(y)\]|
|Matrix3x2.CreateRotation(Float radians)| 透過旋轉的弧度建構 Matrix3x2<br/>\[Cos(radians), Sin(radians),<br/>-Sin(radians), Cos(radians),<br/>0.0, 0.0 \]|
|Matrix3x2.CreateTranslation(Vector2 translation)| 與 CreateFromTranslation 相同|
|Matrix3x2.CreateScale(Vector2 scale)| 與 CreateFromScale 相同|


### <a name="matrix4x4"></a>Matrix4x4

|函數和建構函數操作|   描述|
|-----------------------------------|--------------|
|Scale(Matrix4x4 value, Float factor)|  傳回將每個矩陣元件乘以縮放比例係數的 Matrix4x4。|
|Inverse(Matrix4x4)|    傳回代表倒數矩陣的 Matrix4x4 物件|
|Matrix4x4(Float M11, Float M12, Float M13, Float M14,<br/>Float M21, Float M22, Float M23, Float M24,<br/>    Float M31, Float M32, Float M33, Float M34,<br/>    Float M41, Float M42, Float M43, Float M44)| 使用 16 個浮點數參數建構 Matrix4x4|
|Matrix4x4.CreateFromScale(Vector3 scale)|  透過代表縮放比例的 Vector3 建構 Matrix4x4<br/>\[scale.X, 0.0, 0.0, 0.0,<br/> 0.0, scale.Y, 0.0, 0.0,<br/> 0.0, 0.0, scale.Z, 0.0,<br/> 0.0, 0.0, 0.0, 1.0\]|
|Matrix4x4.CreateFromTranslation(Vector3 translation)|  透過代表平移的 Vector3 建構 Matrix4x4<br/>\[1.0, 0.0, 0.0, 0.0,<br/> 0.0, 1.0, 0.0, 0.0,<br/> 0.0, 0.0, 1.0, 0.0,<br/> translation.X, translation.Y, translation.Z, 1.0\]|
|Matrix4x4.CreateFromAxisAngle(Vector3 axis, Float angle)|  透過代表角度的 Vector3 軸和浮點數建構 Matrix4x4|
|Matrix4x4(Matrix3x2 matrix)| 使用 Matrix3x2 建構 Matrix4x4 <br/>\[matrix.11, matrix.12, 0, 0,<br/>matrix.21, matrix.22, 0, 0,<br/>0, 0, 1, 0,<br/>matrix.31, matrix.32, 0, 1\]|
|Matrix4x4.CreateTranslation(Vector3 translation)| 與 CreateFromTranslation 相同|
|Matrix4x4.CreateScale(Vector3 scale)| 與 CreateFromScale 相同|


### <a name="quaternion"></a>四元數

|函數和建構函數操作|   描述|
|-----------------------------------|--------------|
|Slerp(Quaternion value1, Quaternion value2, Float progress)|   傳回代表根據進度計算兩個四元數值間球面內差補點的四元數 (注意：進度值介於 0.0 與 1.0 之間)|
|Concatenate(Quaternion value1 Quaternion value2)|  傳回代表兩個四元數串連的四元數 (亦即代表合併兩個個別旋轉的四元數)|
|Length(Quaternion value)|  傳回代表四元數長度/大小的浮點數值|
|LengthSquared(Quaternion)| 傳回代表四元數長度/大小平方的浮點數值|
|Normalize(Quaternion value)|   傳回已標準化元件的四元數|
|Quaternion.CreateFromAxisAngle(Vector3 axis, Scalar angle)|    透過代表角度的 Vector3 軸和純量建構四元數|
|Quaternion(Float x, Float y, Float z, Float w)|    透過四個浮點數值建構四元數|

### <a name="color"></a>色彩

|函數和建構函數操作|   描述|
|-----------------------------------|--------------|
|ColorLerp(Color colorTo, Color colorFrom, Float progress)| 傳回代表根據指定進度計算兩個色彩物件間線性內差補點的色彩物件。 (注意：進度值介於 0.0 與 1.0 之間)|
|ColorLerpRGB(Color colorTo, Color colorFrom, Float progress)|  傳回代表根據 RGB 色彩空間中指定進度，計算兩個物件間線性內差補點的色彩物件。|
|ColorLerpHSL(Color colorTo, Color colorFrom, Float progress)|  傳回代表根據 HSL 色彩空間中指定進度，計算兩個物件間線性內差補點的色彩物件。|
|ColorArgb(Float a, Float r, Float g, Float b)| 建構代表由 ARGB 元件定義色彩的物件|
|ColorHsl(Float h, Float s, Float l)|   建構代表由 HSL 元件定義色彩的物件 (注意：色調值的定義範圍為 0 至 2pi 之間)|
