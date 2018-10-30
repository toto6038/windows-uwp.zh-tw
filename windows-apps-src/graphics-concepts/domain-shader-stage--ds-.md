---
title: 網域著色器 (DS) 階段
description: 網域著色器 (DS) 階段會計算輸出修補檔案中細分點的頂點位置；它會計算對應每一個網域取樣的頂點位置。
ms.assetid: 673CC04A-A74F-495F-AFB7-49157538749C
keywords:
- 網域著色器 (DS) 階段
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3bcad4a5e22249d4d7faed08fe9cc9af4c3fb338
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "5753281"
---
# <a name="domain-shader-ds-stage"></a>網域著色器 (DS) 階段


網域著色器 (DS) 階段會計算輸出修補檔案中細分點的頂點位置；它會計算對應每一個網域取樣的頂點位置。 網域著色器會在每個曲面細分器階段輸出點執行一次，並且對輪廓著色器輸出修補檔案和輸出輸出修補檔案常數，以及曲面細分器階段輸出 UV 座標具有唯讀存取權。

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途和使用


網域著色器 (DS) 階段會在輸出修補檔案中輸出細分點的頂點位置，根據來自[輪廓著色器 (HS) 階段](hull-shader-stage--hs-.md)和[曲面細分器 (TS) 階段](tessellator-stage--ts-.md)的輸入。

![網域著色器階段圖](images/d3d11-domain-shader.png)

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>輸入


-   網域著色器會耗用來自[輪廓著色器 (HS) 階段](hull-shader-stage--hs-.md)的輸出控制點。 輪廓著色器輸出包括：
    -   控制點。
    -   修補檔案常數資料。
    -   鑲嵌係數。 鑲嵌係數可能包括固定函式曲面細分器使用的值，以及原始值 (例如在整數鑲嵌進位之前)，藉此簡化地理變形。
-   網域著色器會對來自[曲面細分器 (TS) 階段](tessellator-stage--ts-.md)的每個輸出座標叫用一次。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>輸出


-   網域著色器 (DS) 階段會將輸出修補檔案中細分點的頂點位置輸出。

網域著色器完成後，鑲嵌就會完成且管線資料會繼續進到下一個管線階段，例如[幾何著色器 (GS) 階段](geometry-shader-stage--gs-.md)和[像素著色器 (PS) 階段](pixel-shader-stage--ps-.md)。 預期相鄰基本類型 (例如每個三角形 6 個頂點) 在鑲嵌為使用中時無效 (這樣會導致未定義的行為，偵錯層將會抱怨此行為)。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>範例


```
void main( out    MyDSOutput result, 
           float2 myInputUV : SV_DomainPoint, 
           MyDSInput DSInputs,
           OutputPatch<MyOutPoint, 12> ControlPts, 
           MyTessFactors tessFactors)
{
     ...

     result.Position = EvaluateSurfaceUV(ControlPoints, myInputUV);
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[圖形管線](graphics-pipeline.md)

 

 




