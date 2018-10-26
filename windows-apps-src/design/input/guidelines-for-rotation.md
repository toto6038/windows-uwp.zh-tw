---
author: Karl-Bridge-Microsoft
Description: This topic describes the new Windows UI for rotation and provides user experience guidelines that should be considered when using this new interaction mechanism in your UWP app.
title: 旋轉
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9bffed44921df05a72025e86917901a65fe7ea82
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5565438"
---
# <a name="rotation"></a>旋轉


本文描述旋轉的新 Windows UI，並提供在 Windows 市集應用程式中使用這項新的互動機制時，所應考慮的使用者經驗指導方針。

> **重要 API**：[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)、[**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)

## <a name="dos-and-donts"></a>可行與禁止事項

-   使用旋轉以協助使用者直接旋轉 UI 元素。

## <a name="additional-usage-guidance"></a>其他用法指導方針


**旋轉概觀**

旋轉是 UWP 應用程式所使用的一項觸控最佳化技術，可以讓使用者以圓形方向 (順時針或逆時針) 轉動物件。

根據輸入裝置，旋轉互動會透過以下方式執行：

-   滑鼠或主動式畫筆/手寫筆來移動所選物件的旋轉移駐夾。
-   使用旋轉手勢，利用觸控或被動式畫筆/手寫筆，將物件轉到想要的方向。

**使用旋轉的時機**

使用旋轉以協助使用者直接旋轉 UI 元素。 下圖顯示一些支援的旋轉互動手指位置。

![示範旋轉所支援的各種手指姿勢的圖。](images/ux-rotate-positions.png)

**注意：** 直覺，以及在大部分情況下，旋轉點是兩個觸控點的其中一個除非使用者可以指定一個與接觸點 （例如，在繪圖或配置應用程式） 不相關的旋轉點。 下列影像示範未以這樣的方式限制旋轉點時，會如何對使用者經驗產生負面影響。

第一張圖片顯示初始 (拇指) 和次要 (食指) 觸碰點：食指觸碰一顆樹，而拇指觸碰一段木材。

![顯示旋轉手勢的兩個初始觸碰點的影像。](images/ux-rotate-points1.png)
在第二張圖片中，是以繞著初始 (拇指) 觸碰點的方式執行旋轉。 在旋轉之後，食指仍然觸碰著樹幹，而拇指仍然觸碰著木材 (旋轉點)。

![顯示旋轉後圖片的影像，其中旋轉點被限制為兩個初始觸碰點的其中一點。](images/ux-rotate-points2.png)
在第三張圖片中，旋轉中心由應用程式定義 (或由使用者設定) 為圖片中心點。 在旋轉之後，因為圖片並未繞著其中一隻手指旋轉，直接操作的感覺就會消失 (除非使用者選擇此設定)。

![顯示旋轉後圖片的影像，其中旋轉點被限制為圖片的中心，而非兩個初始觸碰點的其中一點。](images/ux-rotate-points3.png)
在最後一張圖片中，旋轉中心由應用程式定義 (或由使用者設定) 為圖片左邊緣的中間點。 同樣地，除非使用者選擇此設定，否則直接操作的感覺在此情況下將會消失。

![顯示旋轉後圖片的影像，其中旋轉點被限制為圖片最左邊的中心，而非兩個初始觸碰點的其中一點。](images/ux-rotate-points4.png)

 

Windows8 支援三種旋轉類型： 自由式、 限制式以及組合式。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">類型</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">自由式旋轉</td>
<td align="left"><p>自由旋轉可讓使用者在任何位置 360 度自由旋轉內容。當使用者釋放物件時，物件會停留在選擇的位置。 自由式旋轉適用於繪圖和配置應用程式，例如 Microsoft PowerPoint、Word、Visio 和 Paint；以及 Adobe Photoshop、Illustrator 和 Flash。</p></td>
</tr>
<tr class="even">
<td align="left">限制式旋轉</td>
<td align="left"><p>限制式旋轉支援在操作時進行自由式旋轉，但放開時強制使用以 90 度遞增的貼齊點 (0、90、180 及 270)。 當使用者放開物件時，物件會自動旋轉到最接近的貼齊點。</p>
<p>限制式旋轉是最常見的旋轉方法，而且它運作的方式與捲動內容類似。 貼齊點可以讓使用者不需要太精準就能達到他們的目標。 限制式旋轉適用於網頁瀏覽器和相簿之類的應用程式。</p></td>
</tr>
<tr class="odd">
<td align="left">組合式旋轉</td>
<td align="left"><p>組合式旋轉支援依區域 (類似於<a href="guidelines-for-panning.md">移動瀏覽的指導方針</a>中的柵欄) 進行自由式旋轉，每個區域皆使用限制式旋轉所強制的 90 度貼齊點。 如果使用者在其中一個 90 度區域之外放開物件，物件會留在該位置；否則，物件會自動旋轉到一個貼齊點。</p>
<div class="alert">
<strong>注意：</strong>使用者介面滑軌是項的功能，其中目標周遭的區域會限制對部分特定值或位置，以影響其選取範圍移動。
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>相關主題


**範例**
* [基本輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延遲輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [使用者互動模式範例](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦點視覺效果範例](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**封存範例**
* [輸入：XAML 使用者輸入事件範例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [輸入：裝置功能範例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [輸入：觸控點擊測試範例](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 捲動、移動瀏覽和縮放範例](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [輸入：簡化的筆跡範例](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [輸入：使用 GestureRecognizer 處理手勢與操作](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [輸入：操作和手勢 (C++) 範例](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 觸控輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




