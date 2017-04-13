---
title: "裝置"
description: "Direct3D 裝置是 Direct3D 的轉譯元件。 裝置封裝並儲存呈現狀態、執行轉換照明作業，並將影像點陣化到表面。"
ms.assetid: BC903462-A32A-46BA-8411-FB294F5D2CD9
keywords: "裝置"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: e95c1e1cc9cf1b26553ec9e148438ae837dbdf0e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="devices"></a>裝置


Direct3D 裝置是 Direct3D 的轉譯元件。 裝置封裝並儲存呈現狀態、執行轉換照明作業，並將影像點陣化到表面。

架構上來說，Direct3D 裝置包含轉換模組、光源模組及點陣化模組，如下圖所示。

![Direct3D 裝置架構圖](images/d3ddev.png)

Direct3D 支援兩種主要的 Direct3D 裝置類型：

-   具有硬體加速點陣化的 HAL 裝置，以及同時具有硬體和軟體頂點處理的著色
-   參照裝置

這些裝置屬於兩種不同的驅動程式。 軟體與參照裝置是由軟體驅動程式表示，而 hal 裝置是由硬體驅動程式表示。 利用這些裝置的最常見方式是使用 hal 裝置傳送應用程式，以及參照裝置用於功能測試。 這些是由協力廠商提供，用來模擬特定裝置 - 例如尚未發行的開發中硬體。

應用程式建立的 Direct3D 裝置必須符合應用程式執行所在硬體的功能。 Direct3D 提供轉譯功能，可透過存取電腦上安裝的 3D 硬體，或藉由模擬軟體中的 3D 硬體功能。 因此，Direct3D 提供裝置同時用於硬體存取和軟體模擬。

硬體加速裝置提供的效能比軟體裝置好很多。 所有支援圖形卡的 Direct3D 上都提供 hal 裝置類型。 在大部分案例中，應用程式會以有硬體加速且依賴軟體模擬的電腦為目標，以配合較低階的電腦。

參照裝置則例外，軟體裝置不一定與硬體裝置支援相同的功能。 應用程式應該永遠查詢裝置功能，以判斷支援哪些功能。

由於 Direct3D 9 提供的軟體和參照裝置行為與 hal 裝置提供的相同，因此為搭配 hal 裝置所撰寫的應用程式程式碼將可搭配軟體或參照裝置運作，不需修改。 提供的軟體或參照裝置行為與 hal 裝置的行為一致，但裝置功能確實不一樣，而且特殊軟體裝置可能實作的功能更有限。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本節內容


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[裝置類型](device-types.md)</p></td>
<td align="left"><p>Direct3D 裝置類型包括硬體抽象層 (hal) 裝置和軟體模擬轉譯器。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[視窗與全螢幕模式](windowed-vs--full-screen-mode.md)</p></td>
<td align="left"><p>Direct3D 應用程式可以兩種模式執行：視窗或全螢幕模式。 在<em>視窗模式</em>，應用程式與所有執行中的應用程式共用桌面螢幕可用空間。 在<em>全螢幕模式</em>，應用程式執行所在的視窗涵蓋整個桌面，隱藏所有執行的應用程式（包含您的開發環境）。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[遺失裝置](lost-devices.md)</p></td>
<td align="left"><p>Direct3D 裝置可能在操作狀態或遺失狀態。 <em>操作</em>狀態是一般裝置狀態，裝置如預期般執行和呈現所有轉譯。 該裝置會轉換成<em>遺失</em>狀態，例如在全螢幕應用程式中遺失鍵盤焦點，會導致不可能轉譯。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[交換鏈結](swap-chains.md)</p></td>
<td align="left"><p>交換鏈結是用來向使用者顯示畫面的緩衝區集合。 每次應用程式提供新畫面供顯示時，交換鏈結中的第一個緩衝區會取代顯示之緩衝區的位置。 這個程序稱為<em>「交換」</em>或<em>「翻轉」</em>。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[點陣化規則簡介](introduction-to-rasterization-rules.md)</p></td>
<td align="left"><p>通常，為頂點指定的點不完全符合螢幕上的像素。 當發生此狀況，Direct3D 會套用點陣化規則來判斷哪些像素適用於指定三角形。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[Direct3D 圖形學習指南](index.md)

 

 




