---
Description: 本主題描述在 Windows 執行階段應用程式中如何使用接觸幾何來預測觸控目標，並提供觸控目標的最佳做法。
title: 目標預測
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6e8425232512650d5c80bf6fee9745b261aee8d9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646053"
---
# <a name="guidelines-for-targeting"></a>目標預測的指導方針


Windows 中的觸控目標預測，使用觸控數位板所偵測到每一根手指的完整接觸區域。 數位板所回報之較大、較複雜的輸入資料，可用於提高判斷使用者意向 (或最有可能的) 目標時的準確度。

> **重要的 Api**:[**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)， [ **Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)， [ **Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)

本主題描述在 Windows 執行階段應用程式中如何使用接觸幾何來預測觸控目標，並提供目標預測的最佳做法。

## <a name="measurements-and-scaling"></a>度量單位和縮放


為了在不同的畫面大小和像素密度之間保持一致，所有目標大小都是以實體單位 (公釐) 來表示。 實體單位可以透過下列等式轉換為像素：

像素 = 像素密度 × 度量單位

以下範例使用這個公式計算 9 公釐目標大小在每英吋 135 像素 (PPI) 的顯示器上且最大縮放倍數為 1 倍時的像素大小：

像素 = 135 PPI × 9 公釐

像素 = 135 PPI × (0.03937 英吋/公釐 × 9 公釐)

像素 = 135 PPI × 0.35433 英吋

像素 = 48 像素

這個結果必須根據系統定義的每一個縮放倍數調整。

## <a name="thresholds"></a>閾值


距離和時間閾值可以用來判斷互動的結果。

例如，偵測到觸碰時，如果物件從觸碰點拖曳的距離小於 2.7 公釐，而且在觸碰的 0.1 秒內提起，就會登錄點選。 手指移動距離超過這個 2.7 公釐的閾值時，便會拖曳物件以及選取或移動 (如需詳細資訊，請參閱[交叉滑動的指導方針](guidelines-for-cross-slide.md))。 視您的應用程式而定，手指按住不放 0.1 秒以上可能會造成系統執行自顯互動 (如需詳細資訊，請參閱[視覺化回饋的指導方針](guidelines-for-visualfeedback.md))。

## <a name="target-sizes"></a>目標大小


一般而言，將您的觸控目標大小設為 9 公釐或更大的方格 (在縮放倍數 1.0 倍的 135 PPI 螢幕上為 48x48 像素)。 避免使用小於 7 公釐方格的觸控目標大小。

下圖顯示目標大小一般是如何由視覺目標、實際目標大小以及實際目標與其他潛在目標的邊框間距所組成。

![顯示視覺目標、實際目標以及邊框間距之建議大小的圖。](images/targeting-size.png)

下表列出觸控目標的元件大小下限和建議大小。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">目標元件</th>
<th align="left">最小大小</th>
<th align="left">建議大小</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">邊框間距</td>
<td align="left">2 公釐</td>
<td align="left">不適用。</td>
</tr>
<tr class="even">
<td align="left">視覺目標大小</td>
<td align="left">&lt;實際大小的 60%</td>
<td align="left">實際大小的 90-100%
<p>如果視覺目標小於 4.2 公釐方格 (建議目標大小下限 7 公釐的 60%)，大部分使用者無法意識到這是可觸控的目標。</p></td>
</tr>
<tr class="odd">
<td align="left">實際目標大小</td>
<td align="left">7 公釐方格</td>
<td align="left">大於或等於 9 公釐方格 (48 x 48 像素 @ 1 倍)</td>
</tr>
<tr class="even">
<td align="left">總目標大小</td>
<td align="left">11 x 11 公釐 (約 60 像素：3 個 20 像素的格線單位 @ 1 倍)</td>
<td align="left">13.5 x 13.5 公釐 (72 x 72 px @ 1 倍)
<p>這表示實際目標與邊框間距結合後的大小，應該大於它們各自的最小值。</p></td>
</tr>
</tbody>
</table>

 

這些建議目標大小可以根據自己的特殊情況而加以調整。 這些建議考慮到下列事項：

-   修飾的頻率：請考慮將重複或經常按下大於最小大小的目標。
-   錯誤結果：有嚴重的後果，如果錯誤接觸到的目標應該有更大的填補，但更內容區域的邊緣。 如果目標經常被點選，更是如此。
-   在內容區中的位置
-   尺寸和螢幕大小
-   手指姿勢
-   觸控視覺效果
-   硬體和觸控數位板

## <a name="targeting-assistance"></a>目標預測協助


Windows 提供目標預測協助，以支援這裡所顯示的最小大小或邊框間距建議不適用的狀況；例如，網頁上的超連結、行事曆控制項、下拉式清單以及下拉式方塊，或是文字選取。

這些目標預測平台改進和使用者介面行為與視覺化回饋 (判別 UI) 搭配運作，共同改進使用者的準確度和自信心。 如需詳細資訊，請參閱[視覺化回饋的指導方針](guidelines-for-visualfeedback.md)。

如果可觸碰元素不得不小於建議的最小目標大小，可以使用下列技術將所造成的目標預測問題降到最小。

## <a name="tethering"></a>繫連


繫連是一種視覺提示 (從接觸點到物件週框矩形的連結線)，用來向使用者表示它們已經連結到物件並正與該物件互動，雖然輸入接觸並未直接與該物件接觸。 在以下情況，會發生這種現象：

-   在物件的某些鄰近性閾值以內，會先偵測到接觸點，然後這個物件會被當作是最有可能的接觸目標。
-   接觸點遠離物件，但是接觸點仍然在鄰近性閾值以內。

這項功能並未對使用 JavaScript 的 UWP app 的開發人員公開。

## <a name="scrubbing"></a>擦選


擦選是指觸控目標所在區域中的任何一個位置，然後滑動而不提起手指直到停在想要的目標上為止，以藉此選取想要的目標。 這也稱為「觸離啟動」，也就是被啟動的物件是手指從畫面提起時最後觸碰的物件。

設計擦選互動時，請遵循下列指導方針：

-   擦選要和判別 UI 搭配使用。 如需詳細資訊，請參閱[視覺化回饋的指導方針](guidelines-for-visualfeedback.md)。
-   擦選觸控目標的建議最小值為 20 px (3.75 公釐 @ 1 倍大小)。
-   在可移動瀏覽的表面 (例如網頁) 執行時，會優先進行擦選。
-   擦選目標應該彼此靠近。
-   當使用者將手指滑開擦選目標之後，動作會被取消。
-   如果目標執行的動作不具破壞性 (例如在行事曆上切換日期)，就會指定對擦選目標的繫連。
-   指定繫連時是採單一方向 (水平或垂直)。

## <a name="related-articles"></a>相關文章


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
* [輸入：Windows 8 筆勢範例](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [輸入：操作和手勢 （c + +） 範例](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 觸控的輸入的範例](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




