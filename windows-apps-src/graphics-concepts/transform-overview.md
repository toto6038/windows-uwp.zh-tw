---
title: 轉換概觀
description: 矩陣轉換處理很多 3D 圖形低階數學運算。
ms.assetid: B5220EE8-2533-4B55-BF58-A3F9F612B977
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 32f55b0a387221b792e37072f129edddf285195b
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7299336"
---
# <a name="transform-overview"></a>轉換概觀


矩陣轉換處理很多 3D 圖形低階數學運算。

幾何管線使用頂點做為輸入。 轉換引擎將世界、檢視和影轉換套用至頂點，剪輯結果，然後將所有項目傳遞至轉譯器。

| 轉換與空間                           | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 模型空間中的模型座標              | 在管線的開頭，模型頂點的宣告是相對於區域座標系統。 這是區域原點和方向。 此座標方向通常稱為*模型空間*。 個別座標稱為*模型座標*。                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 世界矩陣轉換成世界空間              | 幾何管線的第一階段會將模型的頂點從其區域座標系統轉換成場景中所有物件所使用的座標系統。 頂點重新定向的程序稱為[世界矩陣轉換](world-transform.md)，從模型空間轉換成新的方向，稱為*世界空間*。 在世界區域中的每個頂點都是使用*世界座標*宣告的。                                                                                                                                                                                                                                                                                                                           |
| 檢視轉換成檢視空間（相機空間） | 在下一個階段，描述您 3D 世界的頂點是相對於相機定向的。 也就是，您的應用程式選擇場景的觀點，而且世界空間座標圍繞相機的檢視重新定位和旋轉，將世界空間座標轉換成*檢視空間* (也稱為*相機空間*)。 這是[檢視轉換](view-transform.md)，從世界空間轉換成檢視空間。                                                                                                                                                                                                                                                                                                                        |
| 投影轉換成投影空間    | 下一個階段是[投影轉換](projection-transform.md)，從檢視空間轉換為投影空間。 在管線的這部分，物件通常相對於自身與檢視者之間的距離而縮放比例，以便提供深度幻覺給場景；將近物做得比遠方物件更大。 為了簡化，本文件將是投影轉換後頂點的存在空間稱為*投影空間*。 一些圖形書籍可能將投影空間稱為*透視後同質空間*。 並非所有投影轉換都會將場景中的物件大小縮放。 這類投影有時稱為*仿射*或*正交投影*。 |
| 螢幕空間中的裁剪                      | 在管線的最後一部分，將不會顯示在螢幕上的任何頂點移除，以便轉譯器不會花時間計算永遠不會看到之項目的色彩和陰影。 此程序稱為*裁剪*。 裁剪之後，剩餘頂點依據檢視區參數縮放，而且轉換成螢幕座標。 結果頂點 (當場景點陣化時會顯示在螢幕上) 存在於*螢幕空間*。                                                                                                                                                                                                                                                    |

 

轉換用於將物件幾何從某個座標轉換到另一個座標。 Direct3D 使用矩陣執行 3D 轉換。 矩陣建立 3D 轉換。 您可以結合矩陣，產生包含多個轉換的單一矩陣。

您可以轉換模型空間、世界空間，以及檢視空間之間的座標。

-   [世界矩陣轉換](world-transform.md) - 從模型空間轉換成世界空間。
-   [檢視轉換](view-transform.md) - 從世界空間轉換成檢視空間。
-   [投影轉換](projection-transform.md) - 從檢視空間轉換成投影空間。

## <a name="span-idmatrixtransformsspanspan-idmatrixtransformsspanspan-idmatrixtransformsspanmatrix-transforms"></a><span id="Matrix_Transforms"></span><span id="matrix_transforms"></span><span id="MATRIX_TRANSFORMS"></span>矩陣轉換


在與 3D 圖形搭配運作的應用程式，您可以使用幾何轉換，執行下列動作：

-   表示某物件相對於另一個物件的位置。
-   旋轉和調整物件大小。
-   變更檢視位置、方向及透視圖。

使用 4x4 矩陣，您可以將任何點 (x,y,z) 轉換成另一個點 (x', y', z')，如下面方程式中顯示。

![將任何點轉換成另一個點的方程式](images/matmult.png)

在 (x, y, z) 和矩陣執行下列方程式以產生點 (x', y', z')。

![新點的方程式](images/matexpnd.png)

最常見的轉換是轉移、旋轉和縮放比例。 您可以將產生這些效果的矩陣結合成單一矩陣，一次計算幾個轉換。 例如，您可以建立單一矩陣以轉移並旋轉一系列的點。

矩陣依列-欄順序寫入。 沿著每個軸稱平均縮放頂點 (稱為統一縮放) 的矩陣，由下列使用數學符號的矩陣表示。

![統一縮放矩陣的方程式](images/matrix.png)

在 C++，Direct3D 使用矩陣結構，宣告矩陣為二維陣列。 下列範例顯示如何初始化 [**D3DMATRIX**](https://msdn.microsoft.com/library/windows/desktop/bb172573) 結構做為統一縮放矩陣（縮放比例 "s"）。

```
D3DMATRIX scale = {
    5.0f,            0.0f,            0.0f,            0.0f,
    0.0f,            5.0f,            0.0f,            0.0f,
    0.0f,            0.0f,            5.0f,            0.0f,
    0.0f,            0.0f,            0.0f,            1.0f
};
```

## <a name="span-idtranslatespanspan-idtranslatespanspan-idtranslatespantranslate"></a><span id="Translate"></span><span id="translate"></span><span id="TRANSLATE"></span>轉移


下面方程式將點 (x, y, z) 轉移到新點 (x', y', z')。

![新點轉移矩陣的方程式](images/transl8.png)

您可以在 C++ 中手動建立轉移矩陣。 下列範例程式碼示範如何建立矩陣以轉移頂點的功能。

```
D3DXMATRIX Translate(const float dx, const float dy, const float dz) {
    D3DXMATRIX ret;

    D3DXMatrixIdentity(&ret);
    ret(3, 0) = dx;
    ret(3, 1) = dy;
    ret(3, 2) = dz;
    return ret;
}    // End of Translate
```

## <a name="span-idscalespanspan-idscalespanspan-idscalespanscale"></a><span id="Scale"></span><span id="scale"></span><span id="SCALE"></span>縮放比例


下面方程式以 x、y 與 z 方向的任意值，將點 (x, y, z) 縮放成新點 (x', y', z')。

![新點縮放矩陣的方程式](images/matscale.png)

## <a name="span-idrotatespanspan-idrotatespanspan-idrotatespanrotate"></a><span id="Rotate"></span><span id="rotate"></span><span id="ROTATE"></span>旋轉


這裡描述的轉換適用於左手座標系統，因此可能不同於您在其他地方看到的轉移矩陣。

下面方程式圍繞 x 軸旋轉點 (x, y, z)，產生新點 (x', y', z')。

![新點 x 旋轉矩陣的方程式](images/matxrot.png)

下面方程式圍繞 y 軸旋轉點。

![新點 y 旋轉矩陣的方程式](images/matyrot.png)

下面方程式圍繞 z 軸旋轉點。

![新點 z 旋轉矩陣的方程式](images/matzrot.png)

在這些範例矩陣，希臘字母 theta 表示旋轉角度 (以弧度為單位)。 沿著旋轉軸向原點方向看時，角度是順時針方向測量。

下列程式碼顯示圍繞 X 軸處理旋轉的功能。

```
    // Inputs are a pointer to a matrix (pOut) and an angle in radians.
    float sin, cos;
    sincosf(angle, &sin, &cos);  // Determine sin and cos of angle

    pOut->_11 = 1.0f; pOut->_12 =  0.0f;   pOut->_13 = 0.0f; pOut->_14 = 0.0f;
    pOut->_21 = 0.0f; pOut->_22 =  cos;    pOut->_23 = sin;  pOut->_24 = 0.0f;
    pOut->_31 = 0.0f; pOut->_32 = -sin;    pOut->_33 = cos;  pOut->_34 = 0.0f;
    pOut->_41 = 0.0f; pOut->_42 =  0.0f;   pOut->_43 = 0.0f; pOut->_44 = 1.0f;

    return pOut;
}
```

## <a name="span-idconcatenatingmatricesspanspan-idconcatenatingmatricesspanspan-idconcatenatingmatricesspanconcatenating-matrices"></a><span id="Concatenating_Matrices"></span><span id="concatenating_matrices"></span><span id="CONCATENATING_MATRICES"></span>串連矩陣


使用矩陣的一個優點是，您可以透過將矩陣相乘，組合兩個或更多矩陣的效果。 這表示，若要旋轉模型，然後將它轉移到特定位置，您不需要套用兩個矩陣。 而是，您將旋轉矩陣和轉移矩陣相乘，產生一個包含所有效果的複合矩陣。 這個程序，稱為矩陣串連，可以使用下面方程式撰寫。

![矩陣串連的方程式](images/matrxcat.png)

在這個方程式中，C 是所建立的複合矩陣，而 M₁ 到 Mₙ 是個別矩陣。 在大部分案例中，只會串連兩個或三個矩陣，但沒有限制。

執行矩陣乘法的順序非常重要。 上述公式反映矩陣串連的由左至右規則。 也就是，您用來建立複合矩陣之矩陣的可見效果是依由左至右的順序發生。 以下範例顯示一般世界矩陣。 想像您要為定型飛行器建立世界矩陣。 您可能會想要圍繞飛行器的中心 (模型空間的 y 軸) 來旋轉飛行器，並將它轉移到場景中的其他位置。 若要完成此效果，您首先建立旋轉矩陣，再將它乘以轉移矩陣，如下面方程式中顯示。

![根據旋轉矩陣和轉移矩陣之旋轉的方程式](images/wrldexpl.png)

在這個公式中，R<sub>y</sub> 是圍繞 y 軸之旋轉的矩陣，而 T<sub>w</sub> 則是轉移到世界座標中的特定位置。

矩陣相乘的順序很重要，因為不像兩個純量數值相乘，矩陣乘法不具交換性。 依相反順序將矩陣相乘，有轉移飛行器到世界空間位置，然後圍繞世界原點旋轉的視覺效果。

無論您建立何種矩陣類型，請記住由左至右規則，以確定您達到預期的效果。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[轉換](transforms.md)

 

 




