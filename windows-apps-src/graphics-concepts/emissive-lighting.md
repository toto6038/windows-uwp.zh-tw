---
title: 放射光源
description: 放射光源是物件發出的光線，例如光芒。
ms.assetid: 262EB9E2-DF96-401F-93D6-51A7BB60074B
keywords:
- 放射光源
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ce2082479261cd96fb1c5bafd5f2df06bf6f239
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6047363"
---
# <a name="emissive-lighting"></a>放射光源


*放射光源*是物件發出的光線，例如光芒。 放射呈現物件看起來像自體發光。 放射會影響物件的色彩，例如讓深色材質更亮，並呈現部分的發出色彩。

放射光源是以單一字詞描述。

放射光源 = Cₑ

其中：

| 參數 | 預設值 | 類型                                                                 | 描述     |
|-----------|---------------|----------------------------------------------------------------------|-----------------|
| Cₑ        | (0,0,0,0)     | 紅色、綠色、藍色以及 alpha 透明度 (全都是浮點值) | 放射色彩。 |

 

Cₑ 的值為色彩 1 或色彩 2。 如果未提供頂點色彩，則會使用材質放射色彩。

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>範例


在此範例中，物件使用場景環境光線和材料環境色彩上色。

根據公式，產生的物件頂點色彩為材質色彩。

下圖顯示材質色彩，也就是綠色。 放射光線使用相同色彩照亮所有物件端點。 其不取決於頂點標準或光線方向。 如此一來，球體看起來像是 2D 圓形，因為物件表面周圍的陰影並無差別。

![綠色球體圖例](images/lighte.jpg)

下圖顯示放射光線如何與其他三種光線混合。 在球體右側，有綠色放射光線和紅色環境光線的混合。 在球體左側，綠色放射光線與紅色環境和擴散光線混合，產生紅色漸層。 反射強光中心為白色，在反射光線值驟然消失時會產生黃色光環，使得環境、擴散和放射光線值彼此混合而產生黃色。

![有放射光線的綠色球體圖](images/lightadse.jpg)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[光源的數學計算](mathematics-of-lighting.md)

 

 




