---
title: 轉譯器排序檢視 (ROV)
description: 轉譯器排序檢視可解決讓某些深度緩衝區限制，尤其是有多個紋理的透明度全套用至同一個像素時。
ms.assetid: BCB1EE0D-4C1D-4E17-BDB7-173F448E0A7B
keywords:
- 轉譯器排序檢視 (ROV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7327304e2b42ff5ff71be136220b58e99c6228d2
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6846331"
---
# <a name="rasterizer-ordered-view-rov"></a>轉譯器排序檢視 (ROV)


轉譯器排序檢視可解決讓某些深度緩衝區限制，尤其是有多個紋理的透明度全套用至同一個像素時。

轉譯器排序檢視可讓「與順序無關的透明度」(OIT) 演算法套用到像素的呈現。 深度緩衝區只會讓像素繪製或封閉，並沒有部分封閉透明度的概念。 OIT 演算法會以正確的順序套用透明紋理，因此如果，例如透明玻璃物件應該出現在透明視窗後方，也就是在使用透明紋理的部分植被後方，然後才如所預測地正確繪製最終結果。 沒有 ROV 和 OIT 演算法時，繪製這些透明物件的順序是無法預測的，而且呈現的場景可能只是混亂和錯誤。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[檢視](views.md)

 

 




