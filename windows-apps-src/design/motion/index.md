---
Description: 有意義且精心設計的動作會讓應用程式更有生氣和活力。 協助使用者了解內容變更，並將視覺轉換和使用者經驗緊密結合。
title: UWP app 中的動作和動畫
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: 'windows 10, uwp'
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
---
# <a name="motion-for-uwp-apps"></a>適用於 UWP app 的動作

![主角圖像](images/header-motion2.svg)

Fluent 動作在您的應用程式中可以發揮作用。 它根據使用者的行為提供智慧的意見反應、保持 UI 充滿生氣，以及引導使用者瀏覽您的應用程式。 Fluent 動作在使用者及其數位體驗間引發感情的聯繫。 我們以使用者已經從實際的世界理解的自然移動為基礎來建置，並從那裡延伸我們的系統。

## <a name="fluent-motion-principles"></a>Fluent 動作原則

### <a name="physical"></a>實體

動作中的物件展示物體在現實世界中的行為。 流暢並有回應的動作能為體驗帶來自然的感覺，並建立情感的連結和增加個性。

![實體動作的 UI 範例](images/Physical.gif)
> 當您透過觸控與 UI 互動時，UI 的移動直接與互動的速度相關。 因為觸控是直接操縱，與您互動的物件會影響其周圍的物件。

### <a name="functional"></a>功能性

動作具有目的和信念。 它會引導使用者面對複雜性，並協助建立階層。 移動會給予效能提升的印象，並藉由隱藏認知的延遲來最佳化使用者體驗。

![功能性動作的 UI 範例](images/functional.gif)
> 頁面轉換是為特定用途而建置的。 它們可透露關於頁面相互關係的訊息。 它們移動的方式即使在效能不是最佳時也被視為快速。

### <a name="continuous"></a>連續

點到點的流暢移動自然會吸引目光，並引導使用者。 它會精細地將使用者的工作拼接在一起，讓它感覺更易使用和親和。

![連續動作的 UI 範例](images/continuous3.gif)
> 物件可以從場景到場景移動或在場景中轉化，以提供連續性並協助使用者呼應場景。

### <a name="contextual"></a>前後呼應

智慧型動作提供回饋給使用者，與其操作 UI 的方式一致。 互動以使用者為中心。 移動感覺上合乎其外形規格，並依情境而設計。 使用者看了應該都覺得舒適。

![前後呼應動作的 UI 範例](images/Contextual.gif)
> 動畫應繫結至使用者互動。 操作功能表會從使用者加以啟用之處部署。 

## <a name="motion-articles"></a>動作文章

:::row:::
    :::column:::
        ### [Timing and easing](timing-and-easing.md)
        Timing and easing are important elements that make motion feel natural for objects entering, exiting, or moving within the UI.
    :::column-end:::
    :::column:::
        ### [Directionality and gravity](directionality-and-gravity.md)
        Directional signals help provide a solid mental model of the journey a user takes across experiences. Directional movement is subject to forces like gravity, which reinforces the natural feel of the movement.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        ### [Page transitions](page-transitions.md)
        Page transitions navigate users between pages in an app, providing feedback about the relationship between pages. They help users understand where they are in the navigation hierarchy.
    :::column-end:::
    :::column:::
        ### [Connected animation](connected-animation.md)
        Connected animations let you create a dynamic and compelling navigation experience by animating the transition of an element between two different views.
    :::column-end:::
:::row-end:::
