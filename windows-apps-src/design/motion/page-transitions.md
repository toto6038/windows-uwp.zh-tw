---
Description: 了解如何使用您的 UWP 應用程式中的頁面轉換。
title: 在 UWP app 中的頁面轉換
template: detail.hbs
ms.date: 04/08/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9b3244c24ff4fa8e3c85ee9970536b1b35d8efd5
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444200"
---
# <a name="page-transitions"></a>頁面轉換

使用者在應用程式裡的頁面間瀏覽，頁面轉換提供回饋做為頁面間的關係。 頁面轉換協助使用者了解它們是否在瀏覽階層的頂端、在同層級頁面間移動，或深入瀏覽至頁面階層。

針對應用程式裡頁面間的瀏覽，提供了兩個不同的動畫，*頁面重新整理*和*切入*，並以 [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo) 的子類別顯示。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果您有<strong style="font-weight: semi-bold">XAML 控制項陳列庫</strong>應用程式安裝，請按一下這裡可<a href="xamlcontrolsgallery:/item/PageTransition">開啟 應用程式，並查看作用中的頁面轉換</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="page-refresh"></a>重新整理頁面

頁面重新整理是向上滑動動畫和淡入/淡出動畫的組合，適用於傳入內容。 當使用者被帶到瀏覽堆疊的頂層，例如在 索引標籤 或 left-nav 項目之間瀏覽，請使用頁面重新整理。

需要有使用者已重新開始的感覺。

![頁面重新整理動畫。](images/page-refresh.gif)

頁面重新整理動畫會以 [**EntranceNavigationTransitionInfoClass**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo) 顯示。

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**注意**：A [**框架**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame)會自動使用[ **NavigationThemeTransition** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition)以動畫顯示兩個頁面之間瀏覽。 根據預設，動畫是頁面重新整理。

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

您可以使用水平的滑桿顯示同層級頁面會相互並排顯示。 [NavigationView](../controls-and-patterns/navigationview.md)控制會自動使用最上層導覽中，這個動畫，但如果您要建置您自己的水平的瀏覽體驗，然後您可以實作具有 SlideNavigationTransitionInfo 水平的投影。

所需的看法卻是使用者屬於彼此相鄰的頁面之間巡覽。 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromLeft } );
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

- [兩個頁面之間巡覽](../basics/navigate-between-two-pages.md)
- [在 UWP 應用程式中的動作](index.md)
