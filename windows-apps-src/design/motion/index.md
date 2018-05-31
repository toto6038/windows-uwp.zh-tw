---
author: mijacobs
Description: Purposeful, well-designed motion brings your app to life and makes the experience feel crafted and polished. Help users understand context changes, and tie experiences together with visual transitions.
title: UWP app 中的動作和動畫
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1423aeff139758c780dcecb079141744931cdd7b
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2018
ms.locfileid: "1816533"
---
# <a name="motion-for-uwp-apps"></a>適用於 UWP app 的動作

有意義且精心設計的動作會讓應用程式更有生氣和活力。 動作可協助使用者了解內容的變更，以及他們在您應用程式瀏覽階層中的位置。 它讓使用體驗與視覺轉換結合在一起。 動作讓體驗增加了速度感與立體感。

## <a name="benefits-of-motion"></a>動作優點

動作不只是讓物品移動。 動作是一個用來為使用者建立實體生態系統的工具，以讓使用者沈浸其中，並透過滑鼠、鍵盤、觸控及手寫筆等各種輸入方式進行操縱。 體驗的品質高低依應用程式如何回應使用者及 UI 的通訊特質種類而定。

確保動作在您的應用程式中可以起作用。 最佳的通用 Windows 平台 (UWP) 應用程式會使用動作讓 UI 表現得活靈活現。 動作應︰

- 基於使用者的行為提供回饋。
- 教導使用者如何與 UI 互動。
- 指示如何瀏覽到之前或之後的檢視。

隨著使用者在應用程式內花費的時間越來越多，或者應用程式中的工作變得越來越複雜，高品質的動作變得越來越重要：因為它可以用來改變使用者對其認知負載和應用程式易用性的看法。 動作有許多其他直接益處：

- **動作支援互勳和尋找路徑方法。**

    動作是有方向性的：它會向前和向後移動，移出和移入內容，留下關於使用者如何到達目前檢視的心理「瀏覽軌跡」提示。 轉換可以透過類似使用者已經熟悉的工作來協助其了解如何操作新應用程式。

- **動作可讓人產生性能增強的印象。**

    當網路速度緩慢或系統暫停工作時，動畫可以讓使用者感覺等待的時間較短。 動畫可以用來讓使用者知道應用程式正在運作，而不是靜止不動，而且可以被動顯示使用者可能感興趣的新資訊。

- **動作可增添個性。**

    動作通常是常見的執行緒，可在使用者體驗時傳達您應用程式的個性。

- **動作可增添雅致。**

    流暢並有回應，動作能為體驗帶來自然的感覺，並建立情感的連結。

## <a name="examples-of-motion"></a>動作範例

以下為一些應用程式中的動作範例。

在此，應用程式使用連接動畫讓項目有動畫效果，因為它會「持續」成為下一頁標題的一部分。 此效果可幫助使用者在轉換之間維持脈絡。

![連接動畫](images/connected-animations/example.gif)

在此，視覺視差效果會在 UI 捲動或平移時以不同的速率移動不同的物件，以產生深度、透視和移動的感覺。

![一個帶有背景影像和清單的視差範例](images/_Parallax_v2.gif)


## <a name="types-of-motion"></a>動作類型

<table>
    <tr>
        <th align="left">動作類型</th>
        <th align="left">描述</th>
    </tr>
    <tr>
        <td><a href="motion-list.md">新增和刪除</a>
        </td>
        <td>清單動畫可讓您從集合 (如相簿或搜尋結果清單) 中插入或移除單個或多個項目。
        </td>
    </tr>
    <tr>
        <td><a href="connected-animation.md">連接動畫</a>
        </td>
        <td>連接動畫可讓兩個不同檢視之間元素的轉換有動畫效果，而產生動態且迷人的瀏覽體驗。 這可幫助使用者能夠在檢視之間保持其脈絡並連續性。 在連接動畫中，當 UI 內容變更期間元素在兩個檢視之間看起來是「連續的」，從其來源檢視中的位置飛過畫面到其新檢視中的目的地。 這可強調檢視之間的通用內容，並在轉換時創造美麗且動態的效果。 
        </td>
    </tr>
    <tr>
        <td><a href="content-transition-animations.md">內容轉換</a>
        </td>
        <td>內容轉換動畫可讓您變更畫面中區域的內容，同時保持容器或背景不變。 新的內容會淡入。 如果需要取代現有內容，該內容會淡出。 </td>
    </tr>
    <tr>
        <td><a href="motion-fade.md">淡化</a>
        </td>
        <td>使用淡化動畫將項目帶入檢視或帶出檢視。 兩個常見的淡化動畫會淡入和淡出。 </td>
    </tr>
    <tr>
        <td><a href="page-transitions.md">頁面轉換</a>
        </td>
        <td>使用者在應用程式裡的頁面間瀏覽，頁面轉換提供回饋做為頁面間的關係。
        </td>
    </tr>
    <tr>
        <td><a href="parallax.md">視差</a>
        </td>
        <td>視覺視差效果可幫助建立深度、透視和移動的感覺。 這是透過在 UI 捲動或平移時以不同的速率移動不同的物件來達成此效果。
        </td>
    </tr> 
    <tr>
        <td><a href="motion-pointer.md">按下回饋</a>
        </td>
        <td>指標按下動畫為使用者提供在項目上點選時的視覺化回饋。 第一次點選項目時，會播放稍微縮小並傾斜已按下項目的指標向下動畫。 當使用者放開指標時，則會播放將項目還原至其原始位置的指標向上動畫。
        </td>
    </tr>
</table>

## <a name="animations-in-xaml"></a>XAML 中的動畫

若要深入了解如何使用 XAML 的內建動畫或建立您自己的動畫，請參閱 [XAML 的動畫](xaml-animation.md)。 