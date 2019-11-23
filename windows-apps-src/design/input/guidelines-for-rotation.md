---
Description: 本主題說明輪替的新 Windows UI，並提供在 UWP 應用程式中使用這個新的互動機制時應考慮的使用者經驗指導方針。
title: 旋轉
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 71a09d304b0d68bf01012166173360ec6304fb2c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257931"
---
# <a name="rotation"></a>旋轉


本文描述旋轉的新 Windows UI，並提供在 Windows 市集應用程式中使用這項新的互動機制時，所應考慮的使用者經驗指導方針。

> **重要 API**：[**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)、[**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="dos-and-donts"></a>可行與禁止事項

-   使用旋轉以協助使用者直接旋轉 UI 元素。

## <a name="additional-usage-guidance"></a>其他用法指導方針


**旋轉的總覽**

旋轉是 UWP 應用程式所使用的一項觸控最佳化技術，可以讓使用者以圓形方向 (順時針或逆時針) 轉動物件。

根據輸入裝置，旋轉互動會透過以下方式執行：

-   滑鼠或主動式畫筆/手寫筆來移動所選物件的旋轉移駐夾。
-   使用旋轉手勢，利用觸控或被動式畫筆/手寫筆，將物件轉到想要的方向。

**使用旋轉的時機**

使用旋轉以協助使用者直接旋轉 UI 元素。 下圖顯示一些支援的旋轉互動手指位置。

![示範旋轉所支援的各種手指姿勢的圖。](images/ux-rotate-positions.png)

**請注意**   直覺地說，在大部分的情況下，旋轉點是兩個觸控點的其中之一，除非使用者可以指定與連絡人點無關的旋轉點（例如，在繪圖或配置應用程式中）。 下列影像示範未以這樣的方式限制旋轉點時，會如何對使用者經驗產生負面影響。

第一張圖片顯示初始 (拇指) 和次要 (食指) 觸碰點：食指觸碰一顆樹，而拇指觸碰一段木材。

![影像，顯示旋轉手勢的兩個初始觸控點。](images/ux-rotate-points1.png)
在第二張圖片中，是以繞著初始 (拇指) 觸碰點的方式執行旋轉。 在旋轉之後，食指仍然觸碰著樹幹，而拇指仍然觸碰著木材 (旋轉點)。

![影像，顯示旋轉點受限於兩個初始觸控點之一的旋轉圖片。](images/ux-rotate-points2.png)
在第三張圖片中，旋轉中心由應用程式定義 (或由使用者設定) 為圖片中心點。 在旋轉之後，因為圖片並未繞著其中一隻手指旋轉，直接操作的感覺就會消失 (除非使用者選擇此設定)。

![影像，其中會顯示旋轉的圖片，並將旋轉點限制為圖片中心，而不是兩個初始觸控點的其中一個。](images/ux-rotate-points3.png)
在最後一張圖片中，旋轉中心由應用程式定義 (或由使用者設定) 為圖片左邊緣的中間點。 同樣地，除非使用者選擇此設定，否則直接操作的感覺在此情況下將會消失。

![顯示旋轉後圖片的影像，其中旋轉點被限制為圖片最左邊的中心，而非兩個初始觸碰點的其中一點。](images/ux-rotate-points4.png)

 

Windows 10 支援三種類型的輪替： [免費]、[受限制] 和 [合併]。

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
<strong>請注意</strong>  使用者介面滑軌是一項功能，其中的目的地區域會限制某個特定值或位置的移動，以影響其選取範圍。
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>相關主題


**範例**
* [基本輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [低延遲輸入範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [使用者互動模式範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) \(英文\)

**封存範例**
* [輸入： XAML 使用者輸入事件範例](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [輸入：裝置功能範例](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [輸入：觸控點擊測試範例](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [XAML 捲軸、移動流覽和縮放範例](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [輸入：簡化的筆跡範例](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [輸入：使用 GestureRecognizer 的手勢和操作](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Input：操作和手勢（C++）範例](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [DirectX touch 輸入範例](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




