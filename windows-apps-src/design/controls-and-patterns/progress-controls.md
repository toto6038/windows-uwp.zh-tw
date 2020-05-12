---
Description: 進度控制項為使用者提供回饋，告知正在進行長時間執行的操作。
title: 進度控制項的指導方針
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Progress controls
template: detail.hbs
ms.date: 11/29/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: jeffarn
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 34eca0c822b0da96cae39463777c5c3e9888240c
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970803"
---
# <a name="progress-controls"></a>進度控制項

進度控制項為使用者提供回饋，告知正在進行長時間執行的操作。 根據所使用的指示器，它可以表示在進度指示器可見的時候，使用者無法與 App 互動，也可以指示可能需要等待多久的時間。

**取得 Windows UI 程式庫**

|  |  |
| - | - |
| ![WinUI 標誌](images/winui-logo-64x64.png) | **ProgressBar** 控制項包含在 Windows UI 程式庫中，該程式庫是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **Windows UI 程式庫 API：** [ProgressBar 類別](https://docs.microsoft.com/uwp/api/Microsoft.UI.Xaml.Controls.ProgressBar)、[IsIndeterminate 屬性](https://docs.microsoft.com/uwp/api/Microsoft.ui.xaml.controls.progressbar.isindeterminate)、[ProgressRing 類別](https://docs.microsoft.com/uwp/api/Microsoft.UI.Xaml.Controls.ProgressRing)、[IsActive 屬性](https://docs.microsoft.com/uwp/api/Microsoft.ui.xaml.controls.progressring.isactive)
>
> **平台 API：** [ProgressBar 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar)、[IsIndeterminate 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressbar.isindeterminate)、[ProgressRing 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)、[IsActive 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring.isactive)

> [!NOTE]
> ProgressBar 和 ProgressRing 控制項有兩個版本：一個在平台中，由 Windows.UI.Xaml namespace 命名空間代表；另一個在 Windows UI 程式庫中，也就是 Microsoft.UI.Xaml 命名空間。 雖然 ProgressRing 和 ProgressBar 的 API 相同，但這兩個版本的控制面板外觀不同。 本文件將顯示較新 Windows UI 程式庫版本的映像。
在這整份文件中，我們將使用 XAML 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已將此新增至我們的[網頁](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page)元素：

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

在後方的程式碼中，我們也將使用 C# 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已在檔案頂端新增了此 **using** 陳述式：

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

```vb
Imports muxc = Microsoft.UI.Xaml.Controls
```

## <a name="types-of-progress"></a>進度的類型

有兩種控制項會向使用者顯示操作正在進行中：ProgressBar 或 ProgressRing。

-   ProgressBar 的「確定」  (determinate) 狀態會顯示工作已完成的百分比。 此控制項應用於已知持續時間的作業，但其進度不應封鎖使用者與應用程式的互動。
-   ProgressBar 的「不確定」  (indeterminate) 狀態會顯示操作正在進行中，它不會封鎖使用者與 App 的互動，且無法得知完成的時間。
-   ProgressRing 只有「不確定」  (indeterminate) 狀態，且應用於直到作業完成前，任何進一步使用者互動都會被封鎖的情況。

此外，進度控制項是唯讀的，無法互動。 表示使用者無法叫用或直接使用這些控制項。

![ProgressBar 狀態](images/progress-bar-two-states.png)

*由上至下 - 不確定的 ProgressBar 和確定的 ProgressBar*

![ProgressRing 狀態](images/ProgressRing_SingleState.png)

*不確定的 ProgressRing*

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡開啟應用程式並查看 <a href="xamlcontrolsgallery:/item/ProgressBar">ProgressBar</a> 或 <a href="xamlcontrolsgallery:/item/ProgressRing">ProgressRing</a> 運作情形。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="when-to-use-each-control"></a>各控制項的使用時機

發生中的事要使用何種控制項或狀態來顯示不一定總是明確。 有時候工作本身已足夠明顯，因此完全不需要進度控制項 – 而有時候即使已使用進度控制項，卻仍需要文字來向使用者說明進行中的作業。

### <a name="progressbar"></a>ProgressBar
-   **控制項是否有已定義的持續時間或可預測的結束時間？**

    如果有，則使用「確定」的 ProgressBar，並據此更新其百分比或值。

-   **使用者可以繼續，而不必監視操作的進度嗎？**

    當 ProgressBar 正在使用中時，互動為非強制回應的，通常表示使用者不會因該作業未完成而受到封鎖，而且可以繼續以其他方式使用該應用程式，直到該方面的作業完成為止。

-   **關鍵字**

    如果您的作業接近這些關鍵字的意思，或如果您要在作業進度旁邊顯示的文字符合這些關鍵字，請考慮使用 ProgressBar：

    - *正在載入...*
    - *正在抓取*
    - *處理中...*

### <a name="progressring"></a>ProgressRing

-   **該作業是否會導致使用者必須等候才能繼續？**

    如果作業完成之前會需要所有與 App 的互動先等候，則 ProgressRing 是較佳的選項。 ProgressRing 控制項是用於強制回應的互動，表示直到 ProgressRing 消失之前，使用者都會被封鎖。

-   **應用程式是否在等候使用者完成工作？**

    如果是，請使用 ProgressRing，因為它們專門用來為使用者指示未知的等候時間。

-   **關鍵字**

    如果您的作業接近這些關鍵字的意思，或如果您要在作業進度旁邊顯示的文字符合這些關鍵字，請考慮使用 ProgressRing：

    - *正在重新整理*
    - *正在登入...*
    - *正在連線...*

### <a name="no-progress-indication-necessary"></a>不需要進度指示
-   **使用者是否需要知道發生什麼事？**

    例如，如果應用程式正在背景進行下載，而使用者並未起始下載，則使用者不需要知道這項下載作業。

-   **作業是否屬於不會阻擋使用者活動，且使用者不感興趣 (但仍有一些興趣) 的背景活動？**

    當應用程式執行不用一直看到的工作但仍然需要顯示狀態時，則使用文字。

-   **使用者是否只在意作業已經完成？**

    有時，最好只在作業完成時顯示通知，或在作業完成時立即提供視覺提示，並在背景完成最終作業。

## <a name="progress-controls-best-practices"></a>進度控制項的最佳做法

有時看看一些不同進度控制項的使用時機與方式的視覺化範例會比較清楚：

**ProgressBar - 確定**

![「確定」的 ProgressBar 範例](images/progress-bar-determinate-example.png)

第一個範例是「確定」的 ProgressBar。 當作業的持續時間是已知時 (如正在安裝、下載、設定等時候)，ProgressBar 是最佳的選項。

**ProgressBar - 不確定**

![「不確定」的 ProgressBar 範例](images/progress-bar-indeterminate-example.png)

當作業所需的時間為未知時，則使用 ProgressBar。 當在填滿虛擬的清單時，以及在「不確定」和「確定」的 ProgressBar 之間建立流暢的視覺轉換時，也都很適合用「不確定」的 ProgressBar。

-   **該作業是否是在虛擬的集合中？**

    如果是，不要在每個清單項目出現的時候，都在它們上面放置進度指示器。 反之，請使用 ProgressBar 並將它至於載入之項目集合的頂端，已表示正在抓取項目。

**ProgressRing - 不確定**

![「不確定」的 ProgressRing 範例](images/PR_IndeterminateExample.png)

當使用者與應用程式之間任何進一步的互動停止時，或應用程式正在等候使用者輸入以繼續時，會使用不確定的 ProgressRing。 上方的「正在登入...」範例是絕佳的 ProgressRing 案例，使用者必須等到簽署完成後，才能繼續使用應用程式。

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
<muxc:ProgressBar Width="100" Foreground="Green"/>
```

變更 ProgressRing 的前景色彩會改變環形的填滿色彩。 ProgressBar 的 Foreground 屬性會變更填滿進度列的色彩 – 若要更改進度列為填滿的部分，只要覆寫 Background (背景) 屬性即可。

**顯示等待游標**

有時候，最好只在應用程式或作業需要時間思考時，以及您需要指示使用者，顯示等待游標的應用程式或區域要直到等待游標消失才能進行互動時，才短暫顯示等待游標。

```C#
Window.Current.CoreWindow.PointerCursor = new Windows.UI.Core.CoreCursor(Windows.UI.Core.CoreCursorType.Wait, 10);
```

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [ProgressBar 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar)
- [ProgressRing 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)

**適用於開發人員 (XAML)**
- [新增進度控制項](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10))
- [如何為 Windows Phone 建立自訂不確定的進度列](https://msdn.microsoft.com/library/windows/apps/gg442303.aspx)
