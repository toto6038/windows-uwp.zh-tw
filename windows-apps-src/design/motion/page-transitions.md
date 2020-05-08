---
Description: 瞭解如何在您的 Windows 應用程式中使用頁面轉換。
title: 頁面轉換
template: detail.hbs
ms.date: 04/08/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6239d8409767cab06d4d2c8c9c3abb9d743ca1c9
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970513"
---
# <a name="page-transitions"></a>頁面轉換

使用者在應用程式裡的頁面間瀏覽，頁面轉換提供回饋做為頁面間的關係。 頁面轉換協助使用者了解它們是否在瀏覽階層的頂端、在同層級頁面間移動，或深入瀏覽至頁面階層。

提供兩種不同的*動畫，供*您在應用程式中的頁面之間流覽、*頁面*重新整理和切入，並以[**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo)的子類別來表示。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果您已安裝<strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡以<a href="xamlcontrolsgallery:/item/PageTransition">開啟應用程式，並查看作用中的頁面轉換</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="page-refresh"></a>頁面重新整理

頁面重新整理是向上滑動動畫和淡入/淡出動畫的組合，適用於傳入內容。 當使用者被帶到瀏覽堆疊的頂層，例如在 索引標籤 或 left-nav 項目之間瀏覽，請使用頁面重新整理。

需要有使用者已重新開始的感覺。

![頁面重新整理動畫。](images/page-refresh.gif)

頁面重新整理動畫會以 [**EntranceNavigationTransitionInfoClass**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo) 顯示。

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**注意**：A [**畫面**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame)自動使用 [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) 以動畫顯示兩頁之間的瀏覽。 根據預設，動畫是頁面重新整理。

## <a name="drill"></a>深入探詢

當使用者深入瀏覽應用程式，例如選取項目後顯示詳細資訊，請使用切入。

需要有使用者已深入應用程式裡的感覺。

![切入動畫](images/drill.gif)

「演練」動畫是以[**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo)類別表示。

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>水準投影片

使用水準投影片顯示同輩頁面彼此旁顯示。 [NavigationView](../controls-and-patterns/navigationview.md)控制項會自動使用此動畫來進行熱門導覽，但如果您要建立自己的水準流覽體驗，則可以使用 SlideNavigationTransitionInfo 來執行水準投影片。

所要的感覺是使用者要在彼此之間的頁面之間流覽。 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromLeft } );
```

## <a name="suppress"></a>隱藏

若要避免在導覽期間播放任何動畫，請使用[**SuppressNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo)取代其他**NavigationTransitionInfo**子類型。

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
- [UWindowsWP 應用程式中的動作](index.md)
