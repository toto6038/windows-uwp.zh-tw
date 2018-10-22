---
author: Jwmsft
Description: Learn how to use page transitions in your UWP apps.
title: 在 UWP app 中的頁面轉換
template: detail.hbs
ms.author: jimwalk
ms.date: 04/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.openlocfilehash: 2f4fc4cd9701778b3919896cf90929272e6b0923
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "5398977"
---
# <a name="page-transitions"></a>頁面轉換

使用者在應用程式裡的頁面間瀏覽，頁面轉換提供回饋做為頁面間的關係。 頁面轉換協助使用者了解它們是否在瀏覽階層的頂端、在同層級頁面間移動，或深入瀏覽至頁面階層。

針對應用程式裡頁面間的瀏覽，提供了兩個不同的動畫，*頁面重新整理*和*切入*，並以 [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo) 的子類別顯示。

## <a name="page-refresh"></a>重新整理頁面

頁面重新整理是向上滑動動畫和淡入/淡出動畫的組合，適用於傳入內容。 當使用者被帶到瀏覽堆疊的頂層，例如在 索引標籤 或 left-nav 項目之間瀏覽，請使用頁面重新整理。

需要有使用者已重新開始的感覺。

![頁面重新整理動畫。](images/page-refresh.gif)

頁面重新整理動畫會以 [**EntranceNavigationTransitionInfoClass**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo) 顯示。

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**注意**：A [**畫面**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame)自動使用 [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) 以動畫顯示兩頁之間的瀏覽。 根據預設，動畫是頁面重新整理。

## <a name="drill"></a>切入

當使用者深入瀏覽應用程式，例如選取項目後顯示詳細資訊，請使用切入。

需要有使用者已深入應用程式裡的感覺。

![切入動畫](images/drill.gif)

切入動畫會以 [**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) 類別顯示。

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>水平的投影片

使用水平的投影片顯示同層級頁面顯示旁邊彼此。 [NavigationView](../controls-and-patterns/navigationview.md)控制項會自動使用這個動畫，如上方瀏覽，但如果您正在建置您自己的水平瀏覽體驗，然後您可以實作具有 SlideNavigationTransitionInfo 水平投影。

所需的感覺是使用者，彼此旁邊的頁面之間瀏覽。 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { SlideNavigationTransitionEffect.FromLeft } );
```

## <a name="suppress"></a>隱藏

若要在瀏覽期間避免播放任何動畫，請使用 [**SuppressNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) 以其他取代**NavigationTransitionInfo** 子類型。

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

隱藏動畫很有用，如果您要使用[連接動畫](connected-animation.md)或隱含顯示/隱藏動畫建立自己的轉換。

## <a name="backwards-navigation"></a>向後瀏覽

當向後瀏覽時，您可以使用 `Frame.GoBack(NavigationTransitionInfo)` 播放特定的轉換。

當您根據螢幕大小動態修改瀏覽行為，例如在回應式主要/詳細案例中，這個做法是非常實用的。

## <a name="related-topics"></a>相關主題

- [在兩個頁面之間瀏覽](../basics/navigate-between-two-pages.md)
- [UWP app 裡的動作](index.md)
