---
title: "座標系統"
description: "3D 圖形應用程式通常會使用下列兩個笛卡兒座標系統中的其中一種︰左手系或右手系。 在這兩個座標系統中，正 x 軸指向右側，而正 y 軸指向上方。"
ms.assetid: 138D9B81-146F-4E9F-B742-1EDED8FBF2AE
keywords: "座標系統"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 64ee2dee2944af1d256b825fc08fb01c992d3218
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="coordinate-systems"></a>座標系統


3D 圖形應用程式通常會使用下列兩個笛卡兒座標系統中的其中一種︰左手系或右手系。 在這兩個座標系統中，正 x 軸指向右側，而正 y 軸指向上方。

## <a name="span-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanleft-and-right-handed-coordinates"></a><span id="Left_and_right_handed_coordinates"></span><span id="left_and_right_handed_coordinates"></span><span id="LEFT_AND_RIGHT_HANDED_COORDINATES"></span>左手系和右手系座標


您可以藉由將左手或右手指指向正 x 軸方向，並將它們彎曲至正 y 軸的方向，以記得正 z 軸指向的方向。 您拇指所指的方向，不論是指向您或指向您的反方向，即是該座標系統正 z 軸所指的方向。 下圖顯示這兩個座標系統。

![左手系和右手系笛卡兒座標的圖例](images/leftrght.png)

Direct3D 使用左手系座標系統。 雖然左手系和右手系座標是最常見的系統，但 3D 軟體中還使用各種不同的其他座標系統。 舉例來說，3D 模型應用程式也常使用一種座標系統，其中 y 軸指向檢視器或指向其反方向，而且 z 軸朝上。

## <a name="span-idverticesandvectorsspanspan-idverticesandvectorsspanspan-idverticesandvectorsspanvertices-and-vectors"></a><span id="Vertices_and_vectors"></span><span id="vertices_and_vectors"></span><span id="VERTICES_AND_VECTORS"></span>頂點和向量


根據座標系統的不同，x、y 和 z 座標可以定義空間中的一個點 (「頂點」)，或 3D 方向 (「向量」)。

可以用一系列頂點來定義線條和圖形。 可透過頂點定義的最簡單物件稱為[原始物件](primitives.md)，而由一組原始物件定義的更複雜物件稱為「網格」。

在 3D 座標系統中，網格上執行的幾項基本作業被定義為位移、旋轉和縮放。 您可以結合這些基本轉換來建立轉換矩陣。 如需詳細資訊，請參閱[轉換](transforms.md)。

當您結合這些作業時，結果不可相互交換。您以倍數增加矩陣的順序至關重要。

## <a name="span-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanporting-from-a-right-handed-coordinate-system"></a><span id="Porting_from_a_right-handed_coordinate_system"></span><span id="porting_from_a_right-handed_coordinate_system"></span><span id="PORTING_FROM_A_RIGHT-HANDED_COORDINATE_SYSTEM"></span>從右手座標系統移植


如果您移植以右手系座標系統為基礎的應用程式，您必須對傳送至 Direct3D 的資料進行兩項變更︰

-   翻轉三角形頂點的順序，讓系統從前面順時針轉動這些頂點。 意即如果頂點為 v0、v1、v2，請依 v0、v2、v1 的順序傳遞給 Direct3D。
-   使用檢視矩陣將世界空間在 z 方向縮放 -1。 若要完成這項動作，請翻轉 \_31、\_32、\_33 和 \_34 的符號，這些是您用於檢視矩陣的矩陣結構成員。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[座標系統與幾何](coordinate-systems-and-geometry.md)

 

 



