---
author: QuinnRadich
Description: A progress control provides feedback to the user that a long-running operation is underway.
title: 進度控制項的指導方針
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Progress controls
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: jeffarn
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d92005dca87d1be0cf9fddd0a28402497ab56595
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "4352043"
---
# <a name="progress-controls"></a>進度控制項

 

進度控制項為使用者提供回饋，告知正在進行長時間執行的操作。 根據所使用的指示器，它可以表示在進度指示器可見的時候，使用者無法與 App 互動，也可以指示可能需要等待多久的時間。

> **重要 API**：[ProgressBar 類別](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.aspx)、[IsIndeterminate 屬性](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx)、[ProgressRing 類別](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.aspx)、[IsActive 屬性](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.isactive.aspx)

## <a name="types-of-progress"></a>進度的類型

有兩種控制項會向使用者顯示操作正在進行中：ProgressBar 或 ProgressRing。

-   ProgressBar 的「確定」** (determinate) 狀態會顯示工作已完成的百分比。 此控制項應用於已知持續時間的作業，但其進度不應封鎖使用者與 App 的互動。
-   ProgressBar 的「不確定」** (indeterminate) 狀態會顯示操作正在進行中，它不會封鎖使用者與 App 的互動，且無法得知完成的時間。
-   ProgressRing 只有「不確定」** (indeterminate) 狀態，且應用於直到作業完成前，任何進一步使用者互動都會被封鎖的情況。

此外，進度控制項是唯讀的，無法互動。 表示使用者無法叫用或直接使用這些控制項。

![ProgressBar 狀態](images/ProgressBar_TwoStates.png)

*由上至下 - 「不確定」的 ProgressBar 和「確定」的 ProgressBar*

![ProgressRing 狀態](images/ProgressRing_SingleState.png)

*不確定的 ProgressRing*

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡開啟應用程式並查看 <a href="xamlcontrolsgallery:/item/ProgressBar">ProgressBar</a> 或 <a href="xamlcontrolsgallery:/item/ProgressRing">ProgressRing</a> 運作情形。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="when-to-use-each-control"></a>各控制項的使用時機

發生中的事要使用何種控制項或狀態來顯示不一定總是明確。 有時候工作本身已足夠明顯，因此完全不需要進度控制項 – 而有時候即使用了進度控制項，卻仍需要文字來向使用者說明進行中的作業。

### <a name="progressbar"></a>ProgressBar
-   **控制項是否有已定義的持續時間或可預測的結束時間？**

    如果有，則使用「確定」的 ProgressBar，並據此更新其百分比或值。

-   **使用者可以繼續而不必監視操作的進度嗎？**

    當正在使用 ProgressBar 時，互動式非強制回應的，通常表示使用者不會因該作業未完成而被封鎖，且他們能夠以其他方式使用 App 直到該方面的作業完成。

-   **關鍵字**

    如果您的作業接近這些關鍵字的意思，或如果您要在作業進度旁邊顯示的文字符合這些關鍵字，請考慮使用 ProgressBar：

    - *正在載入...*
    - *正在抓取*
    - *處理中...*

### <a name="progressring"></a>ProgressRing

-   **該作業是否會導致使用者必須等候才能繼續？**

    如果作業完成之前會需要所有與 App 的互動先等候，則 ProgressRing 是較佳的選項。 ProgressRing 控制項是用於強制回應的互動，表示直到 ProgressRing 消失之前，使用者都會被封鎖。

-   **App 是否在等候使用者完成工作？**

    如果是，請使用 ProgressRing，因為它們專門用來為使用者指示未知的等候時間。

-   **關鍵字**

    如果您的作業接近這些關鍵字的意思，或如果您要在作業進度旁邊顯示的文字符合這些關鍵字，請考慮使用 ProgressRing：

    - *正在重新整理*
    - *正在登入...*
    - *連線中...*

### <a name="no-progress-indication-necessary"></a>不需要進度指示
-   **使用者是否需要知道發生什麼事？**

    例如，如果 App 正在背景進行下載，而使用者並未起始下載，則使用者不需要知道這項下載作業。

-   **作業是否屬於不會阻擋使用者活動，且使用者不感興趣 (但仍有一些興趣) 的背景活動？**

    當應用程式執行不用一直看到的工作但仍然需要顯示狀態時，則使用文字。

-   **使用者是否只在意作業已經完成？**

    有時最好只在作業完成時顯示通知，或在作業完成時立即提供視覺提示，並在背景完成最終作業。

## <a name="progress-controls-best-practices"></a>進度控制項的最佳做法

有時看一些不同進度控制項的使用時機與方式的視覺化範例會比較清楚：

**ProgressBar - 確定**

![「確定」的 ProgressBar 範例](images/PB_DeterminateExample.png)

第一個範例是「確定」的 ProgressBar。 當作業的持續時間是已知時 (如正在安裝、下載、設定等時候)，ProgressBar 是最佳的選項。

**ProgressBar - 不確定**

![「不確定」的 ProgressBar 範例](images/PB_IndeterminateExample.png)

當作業所需的時間為未知時，則使用 ProgressBar。 當在填滿虛擬的清單時，以及在「不確定」和「確定」的 ProgressBar 之間建立流暢的視覺轉換時，也都很適合用「不確定」的 ProgressBar。

-   **該作業是否是在虛擬的集合中？**

    如果是，不要在每個清單項目出現的時候，都在它們上面放置進度指示器。 反之，請使用 ProgressBar 並將它至於載入之項目集合的頂端，已表示正在抓取項目。

**ProgressRing - 不確定**

![「不確定」的 ProgressRing 範例](images/PR_IndeterminateExample.png)

當使用者與 App 間任何進一步的互動會被阻擋時，或 App 正在等候使用者輸入以繼續時，使用「不確定」的 ProgressRing。 「正在登入...」 上方的範例是絕佳的 ProgressRing 案例，使用者必須登到登入完成才能繼續使用 App。

## <a name="customizing-a-progress-control"></a>自訂進度控制項

這兩種進度控制項都較為簡易，但要自訂某些視覺化功能的方法則較不明顯。

**調整 ProgressRing 的大小**

您可以隨意放大 ProgressRing 的大小，但最小只能到 20x20epx。 若要重新調整 ProgressRing 的大小，您必須設定它的高度和寬度。 如果只設定高度或寬度其中之一，控制項會假設大小是最小 (20x20epx) – 反之，如果高度和寬度設為不同大小，控制項會使用較小的一邊。
為了確保 ProgressRing 符合您的需要，請將高度和寬度設為相同的值：

```XAML
<ProgressRing Height="100" Width="100"/>
```

若要讓 ProgressRing 可見且以動畫顯示，您必須將 IsActive 屬性設為 true：

```XAML
<ProgressRing IsActive="True" Height="100" Width="100"/>
```

```C#
progressRing.IsActive = true;
```

**調整進度控制項的色彩**

根據預設，進度控制項的主色彩是設為系統的輔色。 變更任一個控制項的 Foreground (前景) 屬性即可覆寫這個筆刷。

```XAML
<ProgressRing IsActive="True" Height="100" Width="100" Foreground="Blue"/>
<ProgressBar Width="100" Foreground="Green"/>
```

變更 ProgressRing 的前景色彩會變更點的色彩。 ProgressBar 的 Foreground 屬性會變更填滿進度列的色彩 – 若要更改進度列為填滿的部分，只要覆寫 Background (背景) 屬性即可。

**顯示等待游標**

當 App 或作業需要時間思考時，以及您需要指示使用者，顯示等待游標的 App 或區域要直到等待游標消失才能進行互動時，最好只短暫顯示等待游標。

```C#
Window.Current.CoreWindow.PointerCursor = new Windows.UI.Core.CoreCursor(Windows.UI.Core.CoreCursorType.Wait, 10);
```

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [ProgressBar 類別](https://msdn.microsoft.com/library/windows/apps/br227529)
- [ProgressRing 類別](https://msdn.microsoft.com/library/windows/apps/br227538)

**適用於開發人員 (XAML)**
- [新增進度控制項](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651)
- [如何為 Windows Phone 建立自訂不確定的進度列](http://go.microsoft.com/fwlink/p/?LinkID=392426)
