---
author: serenaz
Description: Learn how to use page transitions in your UWP apps.
title: 在 UWP app 中的頁面轉換
template: detail.hbs
ms.author: sezhen
ms.date: 04/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.openlocfilehash: dc42199eba00071f5dbabd83a4ae524298a619ee
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2018
ms.locfileid: "1818280"
---
# <a name="page-transitions"></a>頁面轉換

頁面轉換是動畫，當使用者在應用程式裡的頁面間瀏覽時才會播放，提供回饋做為頁面間的關係。 頁面轉換協助使用者了解它們是否在瀏覽階層的頂端、在同層級頁面間移動，或深入瀏覽至頁面階層。

針對應用程式裡頁面間的瀏覽，提供了兩個不同的動畫，*頁面重新整理*和*切入*，並由 [NavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo) 的子類別顯示。

## <a name="page-refresh"></a>重新整理頁面

頁面重新整理是向上滑動動畫和淡入/淡出動畫的組合，適用於傳入內容。 需要有使用者已重新開始的感覺。

當使用者被帶到瀏覽堆疊的頂層，例如在 [索引標籤](../controls-and-patterns/tabs-pivot.md) 或 [left-nav](../controls-and-patterns/navigationview.md) 項目之間瀏覽，請使用頁面重新整理。 預設情況下，[Frame.Navigate()](/uwp/api/windows.ui.xaml.controls.frame.navigate)使用頁面重新整理。

![頁面重新整理動畫。](images/page-refresh.gif)

頁面重新整理動畫會以 [EntranceNavigationTransitionInfoClass](/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo) 顯示。

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());
```

## <a name="drill"></a>切入

當使用者深入瀏覽應用程式，例如選取項目後顯示詳細資訊，請使用切入。

需要有使用者已深入應用程式裡的感覺。

![切入動畫](images/drill.gif)

切入動畫會以 [DrillInNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) 類別顯示

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="suppress"></a>隱藏

隱藏動畫很有用，如果您要使用[連接動畫](connected-animation.md)或隱含顯示/隱藏動畫建立自己的轉換。

若要在瀏覽期間避免播放任何動畫，請使用 [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo)以其他取代[NavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo) 子類型。

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

## <a name="backwards-navigation"></a>向後瀏覽

預設情況下，[Frame.GoBack()](/uwp/api/windows.ui.xaml.controls.frame.goback) 根據播放瀏覽至頁面的動畫。播放對應的「返回」動畫。 例如，使用者向後瀏覽時，使用切入在頁面中瀏覽的應用程式，會看到切出。

向後瀏覽時若要播放特定的轉換，請使用 `Frame.GoBack(NavigationTransitionInfo)`。 當您根據螢幕大小動態修改瀏覽行為，例如，回應式主要/詳細案例中，這非常實用。

## <a name="related-topics"></a>相關主題

- [在兩個頁面之間瀏覽](../basics/navigate-between-two-pages.md)
- [UWP app 裡的動作](index.md)
