---
author: jwmsft
title: 使用來源修飾詞執行拖動以重新整理
description: 使用 SourceModifiers 建立自訂的拖動以重新整理控制項
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 動畫
ms.localizationpriority: medium
ms.openlocfilehash: 997082d2ed7375d99a7be1543901d1dd854be1a0
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5666414"
---
# <a name="pull-to-refresh-with-source-modifiers"></a>使用來源修飾詞拖動以重新整理

在本文中，我們將深入探討如何使用 InteractionTracker 的 SourceModifier 功能，並示範使用它來建立自訂的拖動以重新整理控制項。

## <a name="prerequisites"></a>必要條件

我們此處假設您已熟悉這些文章中討論的概念：

- [輸入導向動畫](input-driven-animations.md)
- [使用 InteractionTracker 自訂操作體驗](interaction-tracker-manipulations.md)
- [關聯式動畫](relation-animations.md)

## <a name="what-is-a-sourcemodifier-and-why-are-they-useful"></a>SourceModifier 是什麼？為什麼很實用？

如同 [InertiaModifiers](inertia-modifiers.md)，SourceModifiers 可讓您對 InteractionTracker 的動作進行更細微的控制。 但 InertiaModifiers 是定義 InteractionTracker 進入慣性之後的動作，而 SourceModifiers 則是定義 InteractionTracker 仍在互動狀態中的動作。 在這些案例中，您想要與傳統「跟隨手指」不同的體驗。

經典範例是拖動以重新整理體驗 - 當使用者拖動清單以重新整理內容，清單以手指的相同速度移動並在固定距離後停止，動作會感覺突兀和機械化。 更自然的體驗是在使用者主動與清單互動時引進一種抗拒感。 這個細微差別可讓使用者與清單互動時的整體體驗更具活力和吸引力。 在「範例」區段中，我們會更詳細討論如何建置。

有 2 種來源修飾詞類型：

- DeltaPosition – 這是觸控移動期間，目前畫面位置與手指的先前畫面位置之間的差異。 這個來源修飾詞可讓您修改互動的差異位置，然後再將其傳送給其他處理。 這是 Vector3 類型參數，開發人員可選擇修改位置的任何 X、Y 或 Z 屬性，然後再傳遞至 InteractionTracker。
- DeltaScale - 這是觸控縮放互動期間，目前畫面縮放比例與先前套用之畫面縮放比例之間的差異。 這個來源修飾詞可讓您修改互動的縮放層級。 這是 float 類型屬性，開發人員可以加以修改，然後再傳遞至 InteractionTracker。

當 InteractionTracker 處於 Interacting 狀態，會評估指派給它的每個來源修飾詞，並判斷其中任一個是否適用。 這表示您可以建立並指派多個來源修飾詞給 InteractionTracker。 但是，在定義每個來源修飾詞時，您需要執行下列動作：

1. 定義條件 – 應套用此特定來源修飾詞時，定義條件式陳述式的運算式。
1. 定義 DeltaPosition/DeltaScale – 符合上述定義的條件時，改變 DeltaPosition 或 DeltaScale 的來源修飾詞運算式。

## <a name="example"></a>範例

現在讓我們來看看如何使用來源修飾詞，以現有的 XAML ListView 控制項建立自訂的拖動以重新整理體驗。 我們將使用畫布做為「重新整理面板」，這會堆疊在 XAML ListView 上方用以建置此經驗。

對於一般使用者體驗，我們想在使用者主動移動清單 (使用觸控) 時建立「抗拒」效果，並在位置超過特定點後停止移動。

![使用拖動以重新整理的清單](images/animation/city-list.gif)

此體驗的有效程式碼可在 [GitHub 上的 Window UI 開發人員實驗室存放庫](https://github.com/Microsoft/WindowsUIDevLabs)中找到。 以下逐步解說建置體驗。
在 XAML 標記程式碼中，有下列項目：

```xaml
<StackPanel Height="500" MaxHeight="500" x:Name="ContentPanel" HorizontalAlignment="Left" VerticalAlignment="Top" >
 <Canvas Width="400" Height="100" x:Name="RefreshPanel" >
<Image x:Name="FirstGear" Source="ms-appx:///Assets/Loading.png" Width="20" Height="20" Canvas.Left="200" Canvas.Top="70"/>
 </Canvas>
 <ListView x:Name="ThumbnailList"
 MaxWidth="400"
 Height="500"
ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.IsScrollInertiaEnabled="False" ScrollViewer.IsVerticalScrollChainingEnabled="True" >
 <ListView.ItemTemplate>
 ……
 </ListView.ItemTemplate>
 </ListView>
</StackPanel>
```

因為 ListView (`ThumbnailList`) 是已捲動的 XAML 控制項，當其到達最上方項目且不能再捲動時，您需讓捲動鏈結至其父系 (`ContentPanel`)。 (ContentPanel 是套用來源修飾詞的地方)。為了做到這一點，您需要在 ListView 標記中將 ScrollViewer.IsVerticalScrollChainingEnabled 設定為 **true**。 您也需要將 VisualInteractionSource 上的鏈結模式設定為 **Always**。

您需要設定 PointerPressedEvent 處理常式搭配將 _handledEventsToo_ 參數設為 **true**。 若沒有此選項，PointerPressedEvent 將不會鏈結至 ContentPanel，ListView 控制項會將將這些事件標示為已處理，且不會傳送視覺鏈結。

```csharp
//The PointerPressed handler needs to be added using AddHandler method with the //handledEventsToo boolean set to "true"
//instead of the XAML element's "PointerPressed=Window_PointerPressed",
//because the list view needs to chain PointerPressed handled events as well.
ContentPanel.AddHandler(PointerPressedEvent, new PointerEventHandler( Window_PointerPressed), true);
```

現在，您已可以將此繫結至 InteractionTracker。 一開始是設定 InteractionTracker、VisualInteractionSource 和 Expression 來運用 InteractionTracker 的位置。

```csharp
// InteractionTracker and VisualInteractionSource setup.
_root = ElementCompositionPreview.GetElementVisual(Root);
_compositor = _root.Compositor;
_tracker = InteractionTracker.Create(_compositor);
_interactionSource = VisualInteractionSource.Create(_root);
_interactionSource.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
_interactionSource.PositionYChainingMode = InteractionChainingMode.Always;
_tracker.InteractionSources.Add(_interactionSource);
float refreshPanelHeight = (float)RefreshPanel.ActualHeight;
_tracker.MaxPosition = new Vector3((float)Root.ActualWidth, 0, 0);
_tracker.MinPosition = new Vector3(-(float)Root.ActualWidth, -refreshPanelHeight, 0);

// Use the Tacker's Position (negated) to apply to the Offset of the Image.
// The -{refreshPanelHeight} is to hide the refresh panel
m_positionExpression = _compositor.CreateExpressionAnimation($"-tracker.Position.Y - {refreshPanelHeight} ");
m_positionExpression.SetReferenceParameter("tracker", _tracker);
_contentPanelVisual.StartAnimation("Offset.Y", m_positionExpression);
```

設定好後，重新整理面板一開始在檢視區外面，所有使用者看到的是 listView，當移動到達 ContentPanel 時將引發PointerPressed 事件，您在此要求系統使用 InteractionTracker 來驅動操作體驗。

```csharp
private void Window_PointerPressed(object sender, PointerRoutedEventArgs e)
{
if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch) {
 // Tell the system to use the gestures from this pointer point (if it can).
 _interactionSource.TryRedirectForManipulation(e.GetCurrentPoint(null));
 }
}
```

> [!NOTE]
> 如果您不需要鏈結已處理事件，新增 PointerPressedEvent 處理常式可以透過使用屬性 (`PointerPressed="Window_PointerPressed"`) 的 XAML 標記來直接完成。

下一步是設定來源修飾詞。 您將使用 2 個來源修飾詞取得此行為：_Resistance_ 和 _Stop_。

- Resistance– 將 DeltaPosition.Y 以一半的速度移動，直到到達 RefreshPanel 的高度為止。

```csharp
CompositionConditionalValue resistanceModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation resistanceCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y < {pullToRefreshDistance}");
resistanceCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation resistanceAlternateValue = _compositor.CreateExpressionAnimation(
 "source.DeltaPosition.Y / 3");
resistanceAlternateValue.SetReferenceParameter("source", _interactionSource);
resistanceModifier.Condition = resistanceCondition;
resistanceModifier.Value = resistanceAlternateValue;
```

- Stop – 當整個 RefreshPanel 都在螢幕上時停止移動。

```csharp
CompositionConditionalValue stoppingModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation stoppingCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y >= {pullToRefreshDistance}");
stoppingCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation stoppingAlternateValue = _compositor.CreateExpressionAnimation("0");
stoppingModifier.Condition = stoppingCondition;
stoppingModifier.Value = stoppingAlternateValue;
Now add the 2 source modifiers to the InteractionTracker.
List<CompositionConditionalValue> modifierList = new List<CompositionConditionalValue>()
{ resistanceModifier, stoppingModifier };
_interactionSource.ConfigureDeltaPositionYModifiers(modifierList);
```

這個圖表提供 SourceModifiers 設定的視覺效果。

![移動圖表](images/animation/source-modifiers-diagram.png)

現在使用 SourceModifiers，您會注意到往下移動 ListView 並到達最頂端項目時，重新整理面板會以移動的一半速度向下拖動，直到它到達 RefreshPanel 高度，接著停止移動。

在完整範例中，在互動期間使用 Keyframe 動畫旋轉 RefreshPanel 畫布中的圖示。 任何內容可用於它的位置，或利用 InteractionTracker 的位置來分別驅動動畫。
