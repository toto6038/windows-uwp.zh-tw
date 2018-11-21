---
title: 矩形
description: 在整個 Direct3D 及 Windows 程式設計中，螢幕上的物件會以週框的形式參照。
ms.assetid: 3B78AE66-2C1A-4191-BDCA-D737E33460BA
keywords:
- 矩形
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cb870fa851e51773d95d23ebf9d31f76774924de
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2018
ms.locfileid: "7431528"
---
# <a name="rectangles"></a>矩形


在整個 Direct3D 及 Windows 程式設計中，螢幕上的物件會以週框的形式參照。 週框的側邊永遠和螢幕的側邊平行，讓矩形可由兩個點描述，這兩點為左上角和右下角。

## <a name="span-idboundingrectanglesspanspan-idboundingrectanglesspanspan-idboundingrectanglesspanbounding-rectangles"></a><span id="Bounding_rectangles"></span><span id="bounding_rectangles"></span><span id="BOUNDING_RECTANGLES"></span>週框


大部分的應用程式使用 [**RECT**](https://msdn.microsoft.com/library/windows/desktop/dd162897) 結構 (或 typedef 別名) 來具備在傳輸到畫面或執行命中偵測時要使用的週框相關資訊。 在 C++ 中，**RECT** 結構具有下列定義。

```
typedef struct tagRECT { 
    LONG    left;    // This is the upper-left corner x-coordinate.
    LONG    top;     // The upper-left corner y-coordinate.
    LONG    right;   // The lower-right corner x-coordinate.
    LONG    bottom;  // The lower-right corner y-coordinate.
} RECT, *PRECT, NEAR *NPRECT, FAR *LPRECT; 
```

在先前的範例中，左上成員是繫結矩形的左上角的 x 座標和 y 座標。 同樣地，右下成員組成右下角的座標。 下圖顯示您可以視覺化這些值的方式。

![左、上、右和下方值所繫結矩形的圖例](images/rect.png)

右緣的像素資料行和下緣的像素資料列不包含在 RECT 中。 例如，鎖定成員 {10, 10, 138, 138} 的 RECT 產生矩形寬度與高度為 128 像素的物件。

為了效率、一致性以及易用性起見，所有 Direct3D 展示函式都適用於矩形。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[座標系統與幾何](coordinate-systems-and-geometry.md)

 

 




