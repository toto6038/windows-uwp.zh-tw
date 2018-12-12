---
title: 使用 InteractionTracker 自訂操作
description: 使用 InteractionTracker API 來建立自訂操作體驗。
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 動畫
ms.localizationpriority: medium
ms.openlocfilehash: 9d2c965bcfbf81efe73ce8aff93cdb8b31163fbd
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8941733"
---
# <a name="custom-manipulation-experiences-with-interactiontracker"></a>使用 InteractionTracker 自訂操作體驗

在本文中，我們將示範如何使用 InteractionTracker 來建立自訂操作體驗。

## <a name="prerequisites"></a>必要條件

我們此處假設您已熟悉這些文章中討論的概念：

- [輸入導向動畫](input-driven-animations.md)
- [關聯式動畫](relation-animations.md)

## <a name="why-create-custom-manipulation-experiences"></a>為何要建立自訂操作體驗？

在大部分案例中，利用預先建置的操作控制項已足夠建立 UI 體驗。 但是，如果您想與常用控制項有所區別呢？ 如果您想建立由輸入導向的特定體驗，或是傳統操作動作不足以運用的 UI 呢？ 這種時候就可以建立自訂體驗。 它們可讓應用程式開發人員和設計者更有創意 – 將可以更佳說明品牌的動作體驗和自訂設計語言呈現給使用者。 您可以從頭開始存取正確的建置組塊集，完全自訂操作體驗 – 從動作應該如何回應手指放上或放開螢幕，到貼齊點和輸入鏈結。

以下是建立自訂操作體驗的一些常見範例：

- 新增自訂撥動、刪除/關閉行為
- 輸入導向效果 (平移造成內容模糊)
- 使用量身打造的操作動作自訂控制項 (自訂 ListView、ScrollViewer 等)

![撥動捲動器範例](images/animation/swipe-scroller.gif)

![拖動動畫範例](images/animation/pull-to-animate.gif)

## <a name="why-use-interactiontracker"></a>為什麼要使用 InteractionTracker？

10586 SDK 版本在 Windows.UI.Composition.Interactions 命名空間引進 InteractionTracker。 InteractionTracker 可啟用：

- **完全彈性** – 我們希望您可以自訂及量身訂做操作體驗的每個層面；尤其是輸入期間或回應輸入的正確動作。 使用 InteractionTracker 建置自訂操作體驗時，您可以任意使用所需的全部旋鈕。
- **流暢的效能** – 操作體驗有一個挑戰是其效能取決於 UI 執行緒。 如果 UI 處於忙碌狀態，可能對操作體驗產生負面影響。 InteractionTracker 建置為使用在 60 FPS 的獨立執行緒上運作的新動畫引擎，因此可產生順暢的動作。

## <a name="overview-interactiontracker"></a>概觀：InteractionTracker

建立自訂操作體驗時，您會與兩個主要元件互動。 我們先討論這兩個元件：

- [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker) – 維護狀態機器的核心物件，其屬性由作用中使用者輸入或直接更新和動畫來驅動。 它隨後會用來繫結到 CompositionAnimation 以建立自訂操作動作。
- [VisualInteractionSource](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.visualinteractionsource) – 定義將輸入傳送到 InteractionTracker 之時機和條件的補充物件。 它會定義用於點擊測試的 CompositionVisual，以及其他輸入設定屬性。

做為狀態機器，InteractionTracker 的屬性可由下列任一項驅動：

- 直接使用者互動 – 使用者在 VisualInteractionSource 點擊測試區域中直接操作
- 慣性 – 從程式設計速度或使用者手勢，InteractionTracker 動畫屬性的慣性曲線
- CustomAnimation – 直接以 InteractionTracker 屬性為目標的自訂動畫

### <a name="interactiontracker-state-machine"></a>InteractionTracker 狀態機器

如前所述，InteractionTracker 是具有 4 種狀態 – 每一種皆可轉換到任何其他 fourstates 的狀態電腦。 (如需 InteractionTracker 如何在這些狀態之間轉換的詳細資訊，請參閱 [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker) 類別文件)。

| 狀態 | 描述 |
|-------|-------------|
| Idle | 未使用中，驅動輸入或動畫 |
| Interacting | 偵測到作用中使用者輸入 |
| Inertia | 作用中輸入或程式設計速度導致作用中動作 |
| CustomAnimation | 自訂動畫導致作用中動作 |

在 InteractionTracker 狀態變更的每種情況下，會產生您可以聆聽的事件 (或回呼)。 為了讓您可以聆聽這些事件，必須實作 [IInteractionTrackerOwner](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.iinteractiontrackerowner) 介面並使用 CreateWithOwner 方法來建立其 InteractionTracker 物件。 下圖也概述會觸發的不同事件。

![InteractionTracker 狀態機器](images/animation/interaction-tracker-diagram.png)

## <a name="using-the-visualinteractionsource"></a>使用 VisualInteractionSource

針對受到輸入驅動的 InteractionTracker，您需要連接 VisualInteractionSource (VIS)。 VIS 是建立做為補充物件，使用 CompositionVisual 來定義：

1. 會追蹤輸入的點擊測試區域，以及會偵測手勢的座標空間
1. 會偵測和傳送的輸入設定，包括：
    - 可偵測的手勢：位置 X 和 Y (水平和垂直平移)、縮放比例 (捏合)
    - Inertia
    - 柵欄與鏈結
    - 重新導向模式：會自動重新導向哪些輸入資料至 InteractionTracker

> [!NOTE]
> 因為 VisualInteractionSource 是根據視覺效果的點擊測試位置和座標空間來建立，建議不要使用運動中或變更位置的視覺效果。

> [!NOTE]
> 如果有多個點擊測試區域，您可以使用相同 InteractionTracker 的多個 VisualInteractionSource 執行個體。 不過，最常見的情況是只使用一個 VIS。

VisualInteractionSource 也負責管理何時將不同輸入形式 (觸控、PTP、手寫筆) 的資料傳送至 InteractionTracker。 此行為由 ManipulationRedirectionMode 屬性定義。 根據預設，所有指標輸入都會傳送至 UI 執行緒，而精確式觸控板輸入會前往 VisualInteractionSource 和 InteractionTracker。

因此，如果您想使用觸控和手寫筆 (Creators Update) 來透過 VisualInteractionSource 和 InteractionTracker 驅動操作，您必須呼叫 VisualInteractionSource.TryRedirectForManipulation 方法。 在下方來自 XAML 應用程式的簡短程式碼片段中，在最頂端 UIElement 方格中發生按下觸控事件時，會呼叫方法：

```csharp
private void root_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch)
    {
        _source.TryRedirectForManipulation(e.GetCurrentPoint(root));
    }
}
```

## <a name="tie-in-with-expressionanimations"></a>搭配 ExpressionAnimations

利用 InteractionTracker 來驅動操作體驗時，您主要與 Scale 及 Position 屬性互動。 與其他 CompositionObject 屬性相同，這些屬性可以在 CompositionAnimation 中做為目標和參考，最常見的是 ExpressionAnimations。

若要在 Expression 中使用 InteractionTracker，請參考追蹤器的 Position (或 Scale) 屬性，如以下範例所示。 當 InteractionTracker 的屬性隨著條件而修改 (如前所述)，Expression 的輸出也會隨之變更。

```csharp
// With Strings
var opacityExp = _compositor.CreateExpressionAnimation("-tracker.Position");
opacityExp.SetReferenceParameter("tracker", _tracker);

// With ExpressionBuilder
var opacityExp = -_tracker.GetReference().Position;
```

> [!NOTE]
> 在 Expression 中參考 InteractionTracker 的位置時，您必須為所產生 Expression 的值加上負號，才會移動到正確方向。 這是因為 InteractionTracker 從圖形的原點進展，您可以想成 InteractionTracker 是在「真實世界」座標中移動，例如與原點的距離。

## <a name="get-started"></a>入門

若要開始使用 InteractionTracker 建立自訂操作體驗：

1. 使用 InteractionTracker.Create 或 InteractionTracker.CreateWithOwner 建立 InteractionTracker 物件。
    - (如果您使用 CreateWithOwner，請務必實作 IInteractionTrackerOwner 介面)。
1. 設定新建立 InteractionTracker 的最小和最大位置。
1. 使用 CompositionVisual 建立 VisualInteractionSource。
    - 確定您傳入的視覺效果具有非零大小。 否則，無法正確取得點擊測試。
1. 設定 VisualInteractionSource 的屬性。
    - VisualInteractionSourceRedirectionMode
    - PositionXSourceMode, PositionYSourceMode, ScaleSourceMode
    - 柵欄與鏈結
1. 使用 InteractionTracker.InteractionSources.Add 新增 VisualInteractionSource 至 InteractionTracker。
1. 設定 TryRedirectForManipulation 用於偵測觸控和手寫筆輸入。
    - 對於 XAML，這通常在 UIElement 的 PointerPressed 事件上完成。
1. 建立 ExpressionAnimation，參考 InteractionTracker 的位置並以 CompositionObject 的屬性為目標。

以下是以動作顯示 #1–5 的簡短程式碼片段：

```csharp
private void InteractionTrackerSetup(Compositor compositor, Visual hitTestRoot)
{ 
    // #1 Create InteractionTracker object
    var tracker = InteractionTracker.Create(compositor);

    // #2 Set Min and Max positions
    tracker.MinPosition = new Vector3(-1000f);
    tracker.MaxPosition = new Vector3(1000f);

    // #3 Setup the VisualInteractionSourc
    var source = VisualInteractionSource.Create(hitTestRoot);

    // #4 Set the properties for the VisualInteractionSource
    source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    source.PositionXSourceMode = InteractionSourceMode.EnabledWithInertia;
    source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;

    // #5 Add the VisualInteractionSource to InteractionTracker
    tracker.InteractionSources.Add(source);
}
```

如需更多 InteractionTracker 進階使用方式，請參閱以下文章：

- [使用 InertiaModifiers 建立貼齊點](inertia-modifiers.md)
- [使用 SourceModifiers 拖動以重新整理](source-modifiers.md)
