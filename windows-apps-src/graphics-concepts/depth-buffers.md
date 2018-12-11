---
title: 深度緩衝區
description: 深度緩衝區 (或稱 z 緩衝區) 儲存深度資訊來控制所呈現而不是隱藏的多邊形區域。
ms.assetid: BE83A1D7-D43D-4013-8817-EFD2B24DC58E
keywords:
- 深度緩衝區
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 279e650532505467f3c0dbabf3814618b893aedb
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8884819"
---
# <a name="depth-buffers"></a>深度緩衝區


*深度緩衝區* (或稱 *z 緩衝區*) 儲存深度資訊來控制所呈現而不是隱藏的多邊形區域。

## <a name="span-idoverviewspanspan-idoverviewspanspan-idoverviewspanoverview"></a><span id="Overview"></span><span id="overview"></span><span id="OVERVIEW"></span>概觀


深度緩衝區 (通常稱為 z 緩衝區或 w 緩衝區)，是裝置屬性，用來儲存深度資訊供 Direct3D 使用。 當 Direct3D 將場景轉譯至目標表面時，它可以使用相關深度緩衝區表面中的記憶體做為工作區，來決定點陣化多邊形的像素如何彼此遮蓋。 Direct3D 使用螢幕外的 Direct3D 表面做為目標，將最終色彩值寫入其中。 與轉譯目標表面相關的深度緩衝區表面用來儲存深度資訊，告知 Direct3D 每一個可見像素在場景中的深度。

當場景點陣化並啟用深度緩衝處理時，轉譯表面上的每一個點都會經過測試。 深度緩衝區中的值可以是點的 z 座標，或其同質 w 座標，來自投射空間中點的 (x,y,z,w) 位置。 使用 z 值的深度緩衝區通常稱為 z 緩衝區，使用 w 值的則稱為 w 緩衝區。 每一種深度緩衝區都有優點和缺點，將於稍後討論。

在測試一開始，深度緩衝區中的深度值會設為場景的可能最大值。 轉譯表面上的色彩值會設為該點的背景色彩值或背景紋理的色彩值。 場景中的每個多邊形都會經過測試，查看是否與轉譯表面上目前的座標 (x,y) 交集。

如果確實交集，目前點的深度值 (會是 z 緩衝區中的 z 座標，在 w 緩衝區中則為 w 座標) 會進行測試，查看它是否小於儲存在深度緩衝區中的深度值。 如果多邊形值的深度較小，它會儲存在深度緩衝區中，來自多邊形的色彩值會寫入轉譯表面上目前的點。 如果該點上多邊形的深度值較大，則會測試清單中下一個多邊形。 此程序顯示在下圖。

![測試深度值的圖](images/zbuffer.png)

## <a name="span-idbufferingtechniquesspanspan-idbufferingtechniquesspanspan-idbufferingtechniquesspanbuffering-techniques"></a><span id="Buffering_techniques"></span><span id="buffering_techniques"></span><span id="BUFFERING_TECHNIQUES"></span>緩衝技術


雖然大部分應用程式不會使用此功能，但是您可以變更 Direct3D 用來判斷哪些值位於深度緩衝區中的比較，以及後續的轉譯目標表面。 在某些硬體上，變更比較功能可能會停用階層 z 測試。

市面上幾乎所有加速器都支援 z 緩衝，因此 z 緩衝區是目前最常見的深度緩衝區類型。 無論有多普遍，z 緩衝區仍有缺點。 由於涉及的數學計算，z 緩衝區中產生的 z 值較不平均分散在 z 緩衝區範圍中 (通常是 0.0 到 1.0 (含))。

具體而言，遠近裁剪平面之間的比例對 z 值分佈不平均影響很大。 使用遠平面距離到近平面距離比例為 100、深度緩衝區範圍的 90% 會用在場景深度範圍的前 10%。 一般用於娛樂或視覺化模擬的應用程式含有外部場景，經常需要遠平面/近平面比例介於 1,000 到 10,000 之間。 比例為 1,000 時，98% 的範圍會用在深度範圍的前 2%，分佈情形會比採用高比例時更糟。 這可能造成隱藏的表面成品在遠方物件中，尤其是使用 16 位元深度緩衝區時，這是最常見的支援位元深度。

另一方面來說，w 型深度緩衝區通常比 z 緩衝區分佈較平均，在遠近裁剪平面之間。 主要優點在於，遠近裁剪平面的距離比例不再是問題。 這可讓應用程式支援大型最大範圍，同時仍讓相對精確的深度緩衝靠近眼睛視覺點。 w 型深度緩衝區並不完美，有時可能會呈現近的物件的隱藏表面成品。 w 緩衝方法的另一個缺點與硬體支援相關：w 緩衝不如 z 緩衝一般在硬體之間廣受支援。

使用 z 緩衝區在轉譯時需要額外負荷。 各種不同的技術都可用來最佳化使用 z 緩衝區時的轉譯。 使用 z 緩衝和紋理處理時，應用程式可提高效能，藉由確保場景從前到後轉譯。 有紋理的 z 緩衝基本類型會針對 z 緩衝區預先測試，以掃描列為基礎。 如果掃描列被掀錢轉譯的多邊形隱藏，系統會快速且有效率地拒絕它。 Z 緩衝可以提升效能，但此技術在場景多次繪製相同像素時最實用。 這很難精確計算，但是您通常可以算出近似值。 如果相同像素繪製少於兩次，將 z 緩衝關閉並從後到前轉譯場景，將可達到最佳效能。

實際的深度值解譯為轉譯器特定。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[深度和樣板緩衝區](depth-and-stencil-buffers.md)

 

 




