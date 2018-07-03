---
author: jwmsft
Description: Learn how Fluent motion fundamentals come together in your app.
title: 執行中動作 - UWP app 中的動畫
label: Motion in practice
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 87a17d3f73887c9b1b5029e2096c5b41c9444c4e
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843761"
---
# <a name="bringing-it-together"></a>組合在一起

> [!IMPORTANT]
> 本文說明尚未發佈但可能在正式發行前大幅度修改的功能。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

時間、加減速、方向以及重力搭配，構成了 Fluent 動作的基礎。 每一個在其他應用程式中應予以考量，並在應用程式內容中適當地套用。

以下是 3 種在應用程式中套用 Fluent 動作基礎的方式。

::: 列:::::: 欄:::**隱含動畫**

        Automatic tween and timing between values in a parameter change to achieve very simple Fluent motion using the standardized values.
    :::column-end:::
    :::column:::
        **Built-in animation**

        System components, such as common controls and shared motion, are "Fluent by default". Fundamentals have been applied in a manner consistent with their implied usage.
    :::column-end:::
    :::column:::
        **Custom animation following guidance recommendations**

        There may be times when the system does not yet provide an exact motion solution for your scenario. In those cases, use the baseline fundamental recommendations as a starting point for your experiences.
    :::column-end:::
:::列結束:::

### **<a name="transition-example"></a>轉換範例**

![功能性動畫](images/pageRefresh.gif)

:::列::: :::欄::: <b>前出方向：</b><br>
        淡出：150 m;加減速：預設加速

        <b>Direction Forward In:</b><br>
        Slide up 150px: 300ms; Easing: Default Decelerate
    :::column-end:::
    :::column:::
         <b>Direction Backward Out:</b><br>
        Slide down 150px: 150ms; Easing: Default Accelerate
      
        <b>Direction Backward In:</b><br>
        Fade in: 300ms; Easing: Default Decelerate
    :::column-end:::
:::列結束:::

 ### **<a name="object-example"></a>物件範例**

 ![300ms 動作](images/control.gif)
 
::: 列:::::: 欄:::<b>方向展開：</b><br>
        增加：300ms; 加減速：標準::: 欄結束:::::: 欄:::<b>方向協定：</b><br>
        增加：150ms; 加減速：預設加速::: 欄結束:::::: 列結束:::

## <a name="related-articles"></a>相關文章

- [動作概觀](index.md)
- [計時和加/減速](timing-and-easing.md)
- [方向性和重力](directionality-and-gravity.md)