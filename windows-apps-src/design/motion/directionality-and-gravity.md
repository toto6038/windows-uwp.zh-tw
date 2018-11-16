---
author: jwmsft
Description: Learn how Fluent motion uses directionality and gravity.
title: 方向性和重力 - UWP app 中的動畫
label: Directionality and gravity
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 09bccec40c25e5eba06ba7a7c428b7b771f84271
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6987368"
---
# <a name="directionality-and-gravity"></a>方向性和重力

方向訊號有助於強化使用者體驗期間的心智模式。 任何動作的方向，必須能同時支援空間的持續性，以及空間中物件的完整性。

方向移動皆受像引力之類的力量牽引。 將力量運用到動作，可強化動作的自然感。

## <a name="direction-of-movement"></a>移動方向

:::row:::
    :::column:::
        Direction of movement corresponds to physical motion. Just like in nature, objects can move in any world axis - X,Y,Z. This is how we think of the movement of objects on the screen.

        When you move objects, avoid unnatural collisions. Keep in mind where objects come from and go to, and alway support higher level constructs that may be used in the scene, such as scroll direction or layout hierarchy.
    :::column-end:::
    :::column:::
        ![direction backward in](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>瀏覽方向

應用程式中場景之間的瀏覽方向是一種概念。 使用者前後瀏覽。 場景在視野中出入。 這些概念結合實體移動來指引使用者。

當瀏覽造成物件從先前的場景移動到新的場景時，物件可在螢幕上進行簡單的 A 到 B 的移動。 若要讓移動的感覺到更實際，會加入標準加/減速及重力感。

要往回瀏覽，就往反方向移動 (B 到 A)。 當使用者往回瀏覽時，它們會預期儘速返回到先前的狀態。 時間更快速、更直接，並開始減速。

在往返瀏覽期間，當選定項目停留在螢幕上時，套用這些原則。

![連續動作的 UI 範例](images/continuous3.gif)

當瀏覽造成螢幕上的項目遭到更換，很重要的一點是須顯示退出的場景退到哪裡，新的場景來自哪裡。

這有幾個優點：

- 它強化使用者的空間心理模式。
- 退出場景的持續時間提供更多時間準備內容讓傳入的場景做動畫效果。
- 它改善應用程式的感知效能。

有 4 個謹慎瀏覽方向考量。

:::row:::
    :::column:::
        **Forward-In**

        Celebrate content entering the scene in a manner that does not collide with outgoing content. Content decelerates into the scene.
    :::column-end:::
    :::column:::
        ![direction forward in](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Forward-Out**

        Content exits quickly. Objects accelerate off screen.
    :::column-end:::
    :::column:::
        ![direction forward out](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Backward-In**

        Same as Forward-In, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward in](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Backward-Out**

        Same as Forward-Out, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward out](images/backwardOUT.gif)
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