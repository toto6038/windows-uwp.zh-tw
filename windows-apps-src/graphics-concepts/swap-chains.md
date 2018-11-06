---
title: 交換鏈結
description: 交換鏈結是用來向使用者顯示畫面的緩衝區集合。
ms.assetid: A38E8BB7-1E77-4D93-B321-D3572A80D5DD
keywords:
- 交換鏈結
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b38dc50f38276fb367402b230e6199fbabdcef80
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6043511"
---
# <a name="swap-chains"></a>交換鏈結


交換鏈結是用來向使用者顯示畫面的緩衝區集合。 每次應用程式提供新畫面供顯示時，交換鏈結中的第一個緩衝區會取代顯示之緩衝區的位置。 這個程序稱為 *「交換」* 或 *「翻轉」*。

圖形卡會保留表面的指標，表示監視器上顯示之影像，稱為前端緩衝區。 當監視器重新整理時，圖形卡會將前端緩衝區的內容傳送到監視器上顯示。 不過，這在轉譯即時圖形時會導致「撕裂」問題。 此問題的核心是，相較於電腦的其他部分，監視器重新整理頻率非常慢。 一般重新整理頻率範圍從 60 Hz（每秒 60 次）到 100 Hz。

如果在監視器重新整理進行時，您的應用程式更新前端緩衝區，顯示的影像會被切成一半，上半部顯示包含舊的影像，下半部包含新影像。 這個問題稱為 *「撕裂」*。

## <a name="span-idavoidingtearingspanspan-idavoidingtearingspanspan-idavoidingtearingspanavoiding-tearing"></a><span id="Avoiding_tearing"></span><span id="avoiding_tearing"></span><span id="AVOIDING_TEARING"></span>避免撕裂


Direct3D 實作兩個選項來避免撕裂：

-   一個選項只允許垂直折返（或垂直同步）作業的監視器更新。 監視器通常藉由水平移動光針來重新整理其影像，從監視器左上角開始以 Z 字形移至監視器右下方。 當光針到達底部時，監視器會將光針移回至左上角，重新校正光針，讓此程序可以重新開始。

    這個重新校正稱為垂直同步。在垂直同步時，監視器不繪製任何項目，所以將不會看到任何更新至前端緩衝區，直到監視器重新繪製為止。 垂直同步相當慢。不過，不會太慢而無法在等待時轉譯複雜的場景。 避免撕裂所需，並且可以呈現複雜場景的程序，稱為背景緩衝。

-   一個選項使用稱為背景緩衝的技術。 背景緩衝是將場景繪製到螢幕外表面（稱為背景緩衝區）的程序。 前端緩衝區以外的任何表面都稱為螢幕外表面，因為監視器永遠不會直接檢視它。

    使用背景緩衝區，應用程式有自由在系統閒置時（也就是沒有正在等待的 Windows 訊息）呈現場景，而不需要考慮監視器的重新整理頻率。 背景緩衝增加了如何及何時將背景緩衝區移至前端緩衝區的複雜度。

## <a name="span-idsurfaceflippingspanspan-idsurfaceflippingspanspan-idsurfaceflippingspansurface-flipping"></a><span id="Surface_flipping"></span><span id="surface_flipping"></span><span id="SURFACE_FLIPPING"></span>表面翻轉


將背景緩衝區移至前端緩衝區的程序，稱為表面翻轉。 因為圖形卡只是使用表面指標來代表前端緩衝區，所以只需要簡單指標變更，即可將背景緩衝區設至前端緩衝區。 當應用程式要求 Direct3D 將背景緩衝區呈現到前端緩衝區時，Direct3D 只是「翻轉」兩個表面指標。 結果是背景緩衝區現在是新的前端緩衝區，而舊的前端緩衝區現在是新的背景緩衝區。

當應用程式要求 Direct3D 裝置呈現背景緩衝區時，就會叫用表面翻轉；不過，Direct3D 可以設定為佇列要求，直到發生垂直同步為止。 這個選項稱為 Direct3D 裝置的呈現間隔。 根據應用程式指定 Direct3D 應如何處理表面翻轉，新的背景緩衝區中的資料可能無法重複使用。

表面翻轉是多媒體、動畫和遊戲軟體的關鍵；它相當於使用一疊紙產生動畫的方式。 在每頁上，畫家將人物稍微改變，所以當您快速翻轉紙張時繪圖看似動畫。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[裝置](devices.md)

 

 




