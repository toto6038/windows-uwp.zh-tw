---
title: 三角形內插補點
description: 在轉譯期間，管線會在每個三角形上插入頂點資料。
ms.assetid: 1A76DD78-CED7-42BE-BA81-B9050CD3AF9B
keywords:
- 三角形內插補點
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e8017cd75ed3dfd4129d6c15d668648792cc8d0a
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8483234"
---
# <a name="triangle-interpolation"></a>三角形內插補點


在轉譯期間，管線會在每個矩形上插入頂點資料。 頂點資料可以是各式各樣的資料，可能包括 (但不限於)︰擴散色彩、反射色彩，擴散 Alpha (三角形不透明度)、反射 Alpha，以及一個霧因數。 對於可程式化的頂點管線，霧因數來自霧暫存器。 對於固定函式頂點管線，霧因數來自反射 alpha。

對於某些頂點資料，內插補點是根據目前的陰影模式而定，如下所示：

| 陰影模式 | 描述                                                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 平面         | 在平面陰影模式下，只內插霧因數。 對於所有其他內插補值，三角形中第一個頂點的色彩會套用到整個面。 |
| Gouraud      | 在所有三個頂點之間執行線性內插補點。                                                                                                               |

 

擴散色彩和反射色彩的處理方式不同，視色彩模型而定。 在 RGB 色彩模型，系統會在內插補點中使用紅色、綠色和藍色元件。

色彩的 Alpha 元件被視為不同的內插補值，因為裝置驅動程式可透過兩種不同方式實作透明度：使用紋理混合或使用點畫。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[座標系統與幾何](coordinate-systems-and-geometry.md)

 

 




