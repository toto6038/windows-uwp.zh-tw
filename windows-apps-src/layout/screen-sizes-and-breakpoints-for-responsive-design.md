---
author: mijacobs
title: "回應式設計的螢幕大小與中斷點"
description: .
ms.assetid: BF42E810-CDC8-47D2-9C30-BAA19DCBE2DA
label: Screen sizes and break points
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: b56cdeeb9a3c3d3ca89e19d8057e3d93241e6c3c
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/22/2017
---
#  <a name="screen-sizes-and-break-points-for-responsive-design"></a>回應式設計的螢幕大小與中斷點

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

整個 Windows 10 生態系統中，裝置目標和螢幕大小有太多種，以致於無法針對每一個最佳化您的 UI。 我們建議使用幾個重要的寬度 (也稱做「中斷點」) 的設計：360、640、1024 以及 1366 有效像素，做為替代方案。

> [!TIP]
> 當針對特定中斷點進行設計時，請針對您應用程式可以使用的螢幕空間量 (應用程式的視窗) 進行設計。 當以全螢幕執行應用程式時，應用程式視窗與螢幕大小相同，但是在其他情況下則較小。
 

下表描述不同的大小類別，並提供針對這些大小類別量身訂做的一般建議。

![回應式設計中斷點](images/rsp-design/rspd-breakpoints.png)

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">大小類別</th>
<th align="left">小型</th>
<th align="left">中型</th>
<th align="left">大型</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;">一般螢幕大小 (對角線)</td>
<td style="vertical-align:top;">4&quot; 到 6&quot;</td>
<td style="vertical-align:top;">7&quot; 到 12&quot;，或電視</td>
<td style="vertical-align:top;">13&quot; 及更大</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">一般裝置</td>
<td style="vertical-align:top;">手機</td>
<td style="vertical-align:top;">平板手機、平板電腦、電視</td>
<td style="vertical-align:top;">電腦、膝上型電腦、Surface Hub</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">通用視窗大小 (以有效像素為單位)</td>
<td style="vertical-align:top;">320x569、360x640、480x854</td>
<td style="vertical-align:top;">960x540、1024x640</td>
<td style="vertical-align:top;">1366x768、1920x1080</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">視窗寬度中斷點 (以有效像素為單位)</td>
<td style="vertical-align:top;">640px 或更少</td>
<td style="vertical-align:top;">641px 到 1007px</td>
<td style="vertical-align:top;">1008px 或更大像素</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">一般建議</td>
<td style="vertical-align:top;"><ul>
<li>中央索引標籤元素。</li>
<li>設定左右視窗邊界為 12px，以在應用程式視窗左右邊緣建立視覺區隔。</li>
<li>將[應用程式列](../controls-and-patterns/app-bars.md)固定在視窗底部以改善存取性</li>
<li>一次使用 1 個欄位/區域</li>
<li>使用圖示來表示搜尋 (不顯示搜尋方塊)。</li>
<li>將[瀏覽窗格](../controls-and-patterns/navigationview.md)以重疊模式放置，以節省螢幕空間。</li>
<li>如果您使用[主要詳細資料模式](../controls-and-patterns/master-details.md)，請使用堆疊展示模式，以節省螢幕空間。</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>將索引標籤元素靠左對齊。</li>
<li>設定左右視窗邊界為 24px，以在應用程式視窗左右邊緣建立視覺區隔。</li>
<li>將[應用程式列](../controls-and-patterns/app-bars.md)之類的命令元素放置在應用程式視窗的頂端。</li>
<li>最多 2 個欄位/區域</li>
<li>顯示搜尋方塊。</li>
<li>將[瀏覽窗格](../controls-and-patterns/navigationview.md)以窄條模式放置，以便讓圖示帶狀線永遠顯示。</li>
<li>請考慮進一步自訂[電視體驗](http://go.microsoft.com/fwlink/?LinkId=760736)。</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>將索引標籤元素靠左對齊。</li>
<li>設定左右視窗邊界為 24px，以在應用程式視窗左右邊緣建立視覺區隔。</li>
<li>將[應用程式列](../controls-and-patterns/app-bars.md)之類的命令元素放置在應用程式視窗的頂端。</li>
<li>最多 3 個欄位/區域</li>
<li>顯示搜尋方塊。</li>
<li>將[瀏覽窗格](../controls-and-patterns/navigationview.md)以停駐模式放置，以便讓它永遠顯示。</li>
</ul></td>
</tr>
</tbody>
</table>

透過 [**Continuum 手機版**](http://go.microsoft.com/fwlink/p/?LinkID=699431) (此為適用於可相容之 Windows 10 行動裝置的新體驗)，使用者可將其手機連接到監視器、滑鼠和鍵盤，讓手機可以像膝上型電腦一樣運作。 針對特定中斷點進行設計時，請記住這項新功能- 行動電話不會永遠留在小型類別中。
 
