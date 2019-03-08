---
Description: 本主題說明新的 Windows UI，用於旋轉，並提供您的 UWP 應用程式中使用這個新的互動機制時，應考量的使用者經驗指導方針。
title: 旋轉
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f631f3178b4af4fe1c1d2d8b27e8ae6ac25c6ad1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617203"
---
# <a name="rotation"></a>旋轉


本文說明旋轉的新 Windows UI，並提供在 UWP 應用程式中使用這項新的互動機制時，所應考慮的使用者經驗指導方針。

> **重要的 Api**:[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)， [ **Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)

## <a name="dos-and-donts"></a>可行與禁止事項

-   使用旋轉以協助使用者直接旋轉 UI 元素。

## <a name="additional-usage-guidance"></a>其他用法指導方針


**旋轉的概觀**

旋轉是 UWP 應用程式所使用的一項觸控最佳化技術，可以讓使用者以圓形方向 (順時針或逆時針) 轉動物件。

根據輸入裝置，旋轉互動會透過以下方式執行：

-   滑鼠或主動式畫筆/手寫筆來移動所選物件的旋轉移駐夾。
-   使用旋轉手勢，利用觸控或被動式畫筆/手寫筆，將物件轉到想要的方向。

**使用循環的時機**

使用旋轉以協助使用者直接旋轉 UI 元素。 下圖顯示一些支援的旋轉互動手指位置。

![示範旋轉所支援的各種手指姿勢的圖。](images/ux-rotate-positions.png)

**附註**  直覺的方式，和在大部分情況下，旋轉點是兩個觸控點除非使用者可以指定與無關 （例如，在繪製或配置的應用程式） 的連絡點旋轉點。 下列影像示範未以這樣的方式限制旋轉點時，會如何對使用者經驗產生負面影響。

第一張圖片顯示初始 (拇指) 和次要 (食指) 觸碰點：食指觸碰一顆樹，而拇指觸碰一段木材。

![顯示兩個初始的觸控點旋轉筆勢的影像。](images/ux-rotate-points1.png)
在第二張圖片中，是以繞著初始 (拇指) 觸碰點的方式執行旋轉。 在旋轉之後，食指仍然觸碰著樹幹，而拇指仍然觸碰著木材 (旋轉點)。

![顯示與旋轉的點旋轉的圖片的影像限制為兩個初始的觸控點的其中一個。](images/ux-rotate-points2.png)
在第三張圖片中，旋轉中心由應用程式定義 (或由使用者設定) 為圖片中心點。 在旋轉之後，因為圖片並未繞著其中一隻手指旋轉，直接操作的感覺就會消失 (除非使用者選擇此設定)。

![顯示與旋轉的點旋轉的圖片的影像限制為中心的圖片而不是其中兩個初始的觸控點。](images/ux-rotate-points3.png)
在最後一張圖片中，旋轉中心由應用程式定義 (或由使用者設定) 為圖片左邊緣的中間點。 同樣地，除非使用者選擇此設定，否則直接操作的感覺在此情況下將會消失。

![顯示旋轉後圖片的影像，其中旋轉點被限制為圖片最左邊的中心，而非兩個初始觸碰點的其中一點。](images/ux-rotate-points4.png)

 

Windows 8 支援三種類型的旋轉： 免費、 受條件約束，且合併。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">類型</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">自由式旋轉</td>
<td align="left"><p>自由式旋轉可以讓使用者任意地 360 度自由旋轉內容。當使用者放開物件時，物件會留在選擇的位置。 自由式旋轉適用於繪圖和配置應用程式，例如 Microsoft PowerPoint、Word、Visio 和 Paint；以及 Adobe Photoshop、Illustrator 和 Flash。</p></td>
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
<strong>附註</strong>  使用者介面滑軌是目標周圍區域會限制一些特定值或來影響其選取項目位置的移動功能。
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>相關主題


**範例**
* [基本的輸入的範例](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延遲的輸入的範例](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [使用者互動模式範例](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦點視覺效果範例](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**封存範例**
* [輸入：XAML 使用者輸入的事件範例](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [輸入：裝置功能的範例](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [輸入：觸控的點擊測試範例](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [捲動、 移動和縮放範例的 XAML](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [輸入：簡化的手寫範例](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [輸入：筆勢和 GestureRecognizer 操作](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [輸入：操作和手勢 （c + +） 範例](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 觸控的輸入的範例](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




