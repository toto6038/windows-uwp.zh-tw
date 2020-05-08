---
Description: 瞭解流暢動作如何使用方向和引力。
title: 方向和引力-Windows 應用程式中的動畫
label: Directionality and gravity
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ddcfac5e36500a8fc6dc41c7c86037f5a1483203
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970643"
---
# <a name="directionality-and-gravity"></a>方向性和重力

方向訊號有助於強化使用者體驗期間的心智模式。 任何動作的方向，必須能同時支援空間的持續性，以及空間中物件的完整性。

方向移動皆受像引力之類的力量牽引。 將力量運用到動作，可強化動作的自然感。

## <a name="examples"></a>範例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下此處以<a href="xamlcontrolsgallery:/category/Motion">開啟應用程式並查看動作運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="direction-of-movement"></a>移動方向

:::row:::
    :::column:::
移動方向對應于實體動作。 就像大自然中，物件可在任何世界軸 (X、Y、Z) 移動。 這就是我們想像螢幕上物件如何移動的情況。
當您移動物件時，請避免發生非自然衝突。 請記住，物件來自並移至的位置，而且一律支援可能用於場景中的較高層級結構，例如 [捲軸] 或 [版面配置階層]。
    :::column-end:::
    :::column:::
        ![後進方向](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>瀏覽方向

應用程式中場景之間的瀏覽方向是一種概念。 使用者前後瀏覽。 場景在視野中出入。 這些概念結合實體移動來指引使用者。

當瀏覽造成物件從先前的場景移動到新的場景時，物件可在螢幕上進行簡單的 A 到 B 的移動。 若要讓移動的感覺到更實際，會加入標準加/減速及重力感。

要往回瀏覽，就往反方向移動 (B 到 A)。 當使用者往回瀏覽時，它們會預期儘速返回到先前的狀態。 時間更快速、更直接，並開始減速。

在往返瀏覽期間，當選定項目停留在螢幕上時，套用這些原則。

![連續動作的 UI 範例](images/continuous3.gif)

當瀏覽造成螢幕上的項目遭到更換，很重要的一點是須顯示退出的場景退到哪裡，新的場景來自哪裡。

這有幾項優點：

- 它強化使用者的空間心理模式。
- 退出場景的持續時間提供更多時間準備內容讓傳入的場景做動畫效果。
- 它改善應用程式的感知效能。

有 4 個謹慎瀏覽方向考量。

:::row:::
    :::column:::
**向前**以不會與外寄內容衝突的方式來輸入場景的慶祝內容。 內容減速進入場景。
    :::column-end:::
    :::column:::
        ![方向向前](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**向前快**轉內容很快就會結束。 物件的加速畫面。
    :::column-end:::
    :::column:::
        ![向外方向](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**後**置與正向，但相反。
    :::column-end:::
    :::column:::
        ![後進方向](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**向外**與順向外，但相反。
    :::column-end:::
    :::column:::
        ![方向向外](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>重力

重力可讓您體驗更自然的感覺。 在 z 軸上移動的物件，其不是透過螢幕上的布景固定在場景上，便可能受到重力影響。 當物件擺脫場景，在其達到逸出速度之前，重力會將物件拉住，讓物件移動的軌跡曲線更加自然。

當物件必須從一個場景跳到另一個場景時，重力通常會顯現出來。 因此，連接的動畫便使用重力的概念。

在此，方格頂端列中的元素受到重力影響，讓它在離開其位置時緩慢地往下掉，並移動到前方。

![後進方向](images/continuity-photos.gif)

## <a name="related-articles"></a>相關文章

- [動作概觀](index.md)
- [計時和加/減速](timing-and-easing.md)
