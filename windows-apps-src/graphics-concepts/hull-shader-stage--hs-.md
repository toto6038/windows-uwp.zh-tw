---
title: 輪廓著色器 (HS) 階段
description: 輪廓著色器 (HS) 階段是其中一個鑲嵌階段，有效率地將模型的單一表面分成多個三角形。
ms.assetid: C62F6F15-CAD7-4C72-9735-00762E346C4C
keywords:
- 輪廓著色器 (HS) 階段
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9137f7ef46da1b861976dbac680327febf315dac
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8788586"
---
# <a name="hull-shader-hs-stage"></a>輪廓著色器 (HS) 階段


輪廓著色器 (HS) 階段是其中一個鑲嵌階段，有效率地將模型的單一表面分成多個三角形。 輪廓著色器 (HS) 階段會產生幾何塊面 (和塊面常數)，對應每一個輸入塊面 (四方形、三角形或線條)。 輪廓著色器會針對每個塊面叫用一次，並且將定義低位表面的輸入控制點轉換成構成塊面的控制點。 它也會進行一些每個塊面計算，以提供資料給[曲面細分器 (TS) 階段](tessellator-stage--ts-.md)和[網域著色器 (DS) 階段](domain-shader-stage--ds-.md)。

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途和使用


![輪廓著色器階段圖](images/d3d11-hull-shader.png)

這三個鑲嵌階段共 同將更高位的表面 (保持模型簡單且有效率) 轉換為許多三角形，在圖形管線中詳細呈現。 鑲嵌階段包含輪廓著色器 (HS) 階段、[曲面細分器 (TS) 階段](tessellator-stage--ts-.md)及[網域著色器 (DS) 階段](domain-shader-stage--ds-.md)。

輪廓著色器 (HS) 階段是可程式化的著色器階段。 輪廓著色器是使用 HLSL 功能實作。

輪廓著色器分兩階段執行：控制點階段和塊面常數階段，由硬體同時執行。 HLSL 編譯器會擷取輪廓著色器中的平行處理原則，並將它編碼成驅動硬體的位元組程式碼。

-   控制點階段會針對每個控制點執行一次，讀取塊面的控制點，以及產生一個輸出控制點 (以 **ControlPointID** 識別)。
-   塊面常數階段會針對每個塊面執行一次，以產生邊緣鑲嵌係數，和其他每個塊面常數。 在內部，許多塊面常數階段可能同時執行。 塊面常數階段對於所有輸入和輸出控制點有唯讀存取權。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>輸入


介於 1 到 32 輸入控制點之間，共同定義低位表面。

-   輪廓著色器會宣告[曲面細分器 (TS) 階段](tessellator-stage--ts-.md)所需的狀態。 這些資訊包括控制點數目、塊面的類型，以及鑲嵌時分割使用的類型。 這項資訊會顯示為宣告，通常是在著色器程式碼的前方。
-   鑲嵌係數決定每個塊面要細分成多少數目。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>輸出


介於 1 到 32 輸出控制點之間，共同構成塊面。

-   著色器輸出介於 1 到 32 個控制點之間，無論鑲嵌係數的數目為何。 輪廓著色器的控制點輸出可以由網域著色器階段取用。 塊面常數資料可以由網域著色器取用。 鑲嵌係數可以由[曲面細分器 (TS) 階段](tessellator-stage--ts-.md)和[網域著色器 (DS) 階段](domain-shader-stage--ds-.md)取用。
-   如果輪廓著色器將任何邊緣鑲嵌係數設為 ≤ 0 或 NaN，塊面就會被消除 (省略)。 如此一來，曲面細分器階段就不一定會執行，網域著色器將不會執行，而該塊面也不會產生任何可見的輸出。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>範例


```
[patchsize(12)]
[patchconstantfunc(MyPatchConstantFunc)]
MyOutPoint main(uint Id : SV_ControlPointID,
     InputPatch<MyInPoint, 12> InPts)
{
     MyOutPoint result;
     
     ...
     
     result = TransformControlPoint( InPts[Id] );

     return result;
}
```

請參閱[使用方法︰建立輪廓著色器](https://msdn.microsoft.com/library/windows/desktop/ff476338)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[圖形管線](graphics-pipeline.md)

 

 




