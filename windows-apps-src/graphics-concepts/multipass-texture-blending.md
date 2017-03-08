---
title: "多重紋理混合"
description: "Direct3D 應用程式可透過在多個轉譯階段套用各種紋理到基本類型，達到多項特效。"
ms.assetid: FB4D6E3F-4EF5-4D20-BF7E-1008E790E30C
keywords:
- "多重紋理混合"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 81e5683fd630d0c843ee59b26b5715090fdd0cea
ms.lasthandoff: 02/07/2017

---

# <a name="multipass-texture-blending"></a>多重紋理混合


Direct3D 應用程式可透過在多個轉譯階段套用各種紋理到基本類型，達到多項特效。 常用詞為*多重紋理混合*。 多重紋理混合的一般用途為藉由套用數種不同紋理的多重色彩，模擬複雜光源和著色模型的效果。 這類應用稱為*光影對應*。 請參閱[光線與紋理對應](light-mapping-with-textures.md)。

**注意**某些裝置可將多重紋理套用到單一階段基本類型。 請參閱[紋理混合](texture-blending.md)。

 

如果使用者的硬體不支援多重紋理混合，您的應用程式可以使用多重紋理混合來達到相同的視覺效果。 不過，當使用多重紋理混合時，應用程式無法維持可行的畫面播放速率。

若要在 C/C++ 應用程式中執行多重紋理混合︰

1.  請在紋理階段 0 設定紋理。
2.  選取想要的色彩和 alpha 混合引數與作業。 預設值適用於多重紋理混合。
3.  在場景中呈現適當物件。
4.  在紋理階段 0 設定下一個紋理。
5.  設定呈現狀態以視需要調整來源和目的地混合因素。 系統根據這些參數，在呈現目標表面中混合新紋理與現有像素。
6.  視需要盡量使用紋理重複步驟 3、4 及 5。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[紋理混合](texture-blending.md)

 

 





