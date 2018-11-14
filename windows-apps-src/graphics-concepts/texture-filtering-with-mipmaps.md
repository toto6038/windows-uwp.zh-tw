---
title: 使用 Mipmap 進行紋理篩選
description: Mipmap 是一組紋理序列，序列中的每個影像都是相同影像漸進式降低解析度的結果。 每個影像的高度、寬度，或是層級，都是前一層級以 2 的乘冪縮小的結果。
ms.assetid: 28E863A2-C776-40E4-8551-9851DF7EC93E
keywords:
- 使用 Mipmap 進行紋理篩選
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d32d5a77fe9bc840ea676c7156c1b59e498d07e1
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6200797"
---
# <a name="texture-filtering-with-mipmaps"></a>使用 Mipmap 進行紋理篩選


*Mipmap* 是一組紋理序列，序列中的每個影像都是相同影像漸進式降低解析度的結果。 每個影像的高度、寬度，或是層級，都是前一層級以 2 的乘冪縮小的結果。 Mipmap 不一定要是矩形。

高解析度的 Mipmap 影像用於接近使用者的物件。 低解析度的影像則是用於物件出現在較遠的位置時。 Mipmap 藉由消耗較多的記憶體，以達到改善轉譯紋理品質的效果。

Direct3D 以一串附加表面的鏈結來代表 Mipmap。 解析度最高的紋理位於鏈結的開頭，並且將下一層級的 Mipmap 作為附件。 接下來，該作為附件的層級又會將下一個層級的 Mipmap 作為附件，以此類推，直到最低解析度的 Mipmap。

下列圖例呈現了這些層級的範例。 點陣圖紋理代表 3D 第一人稱射擊遊戲中容器上的標誌。 作為 Mipmap 建立時，解析度最高的紋理就會是 Mipmap 組中的首位。 在 Mipmap 組中，接下來的每個紋理都會以 2 的乘冪縮小。 在此範例中，解析度最高的 Mipmap 為 256 x 256。 接下來的紋理為 128 x 128。 鏈結中最後一個紋理則為 64 x 64。

從使用者可看到該標誌開始，這個標誌擁有最遠的距離。 若使用者在距離標誌較遠的地方開始，遊戲會顯示 Mipmap 鏈結中最小的紋理。(在此一範例中即為 64 x 64。)

![64 x 64 危險標誌的圖例](images/mip1.jpg)

當使用者將視角往標誌移動時，隨著距離越來越近，遊戲就會使用解析度更高的紋理。 下列圖例的解析度是 128 x 128。

![128 x 128 危險標誌的圖例](images/mip2.jpg)

當使用者的視角位於標誌允許的最小觀看距離時，遊戲便會使用解析度最高的紋理，如下列圖例所示。

![256 x 256 危險標誌的圖例](images/mip3.jpg)

Mipmap 在模擬紋理的透視上更有效率。 相較於將單一紋理轉譯成多個解析度，直接利用多張不同解析度的紋理會更為快速。

Direct3D 可評估 Mipmap 組中哪一張紋理的解析度最接近所需要的輸出影像。 若最終影像的解析度介於 Mipmap 組中紋理的解析度之間，Direct3D 可檢查兩個 Mipmap 中的紋理並且將其色彩值混合。

若要使用 Mipmap，您的應用程式必須先建置一組 Mipmap。 應用程式透過將 Mipmap 組設定為現有紋理組中的第一個紋理來套用 Mipmap。 請參閱[紋理混色](texture-blending.md)。

接下來，您的應用程式必須設定 Direct3D 用來取樣材質的篩選方法。 Mipmap 篩選最快的方法就是讓 Direct3D 選擇最接近的材質。 使用 D3DTEXF\_POINT 列舉值加以選取。 若您的應用程式使用 D3DTEXF\_LINEAR 列舉值，Direct3D 可以產生更好的篩選結果。 這會選擇最接近的 Mipmap，並且計算圍繞著紋理中現有像素對應位置的材質之加權平均數。

Mipmap 紋理在 3D 場景中作為減少轉譯場景所需時間的用途使用。 其也能強化場景的擬真度。 然而，其通常需要大量的記憶體。

**注意：**  mipmap 鏈結中的每個表面有都有著鏈結中前一個表面的維度。 如果最上層的 Mipmap 有著 256 x 128 的維度大小，第二層 Mipmap 的維度大小即為 128 x 64，第三層則為 64 x 32，以此類推，直到 1 x 1 為止。 您無法請求可能會使鏈結中任何一個 Mipmap 的寬度或高度小於 1 的 Mipmap 級數。 簡單舉例來說，若最上層 Mipmap 表面為 4 x 2，最大容許級數即為 3。 最上層維度為 4 x 2，第二層維度為 2 x 1，第三層的維度即為 1 x 1。 由於超過第三層後即會產生第二層 Mipmap 高度的分數值，因此程式並不允許。

 

Direct3D 可以自動執行 Mipmap 紋理篩選。 應用程式也可以手動在一串 Mipmap 鏈結上周遊，並且將點陣圖資料載入每個鏈結上的表面。 這通常也是周遊鏈結的唯一理由。 在紋理建立時間中自動產生 Mipmap 可以利用硬體篩選，因為 Mipmap 位於視訊記憶體中。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[紋理篩選](texture-filtering.md)

 

 




