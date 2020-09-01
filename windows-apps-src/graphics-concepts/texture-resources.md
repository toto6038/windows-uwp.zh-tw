---
title: 紋理資源
description: 瞭解如何使用 Direct3D 材質資源進行轉譯，以及如何使用材質階段支援多個材質混色。
ms.assetid: 016F6CDA-D361-4E6B-BA99-49E915A19364
keywords:
- 紋理資源
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d82a5525601c98812d6aab97f5f5d4399ceddc91
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156182"
---
# <a name="texture-resources"></a>紋理資源


紋理是可作為轉譯用途的一種資源。

## <a name="span-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanrendering-with-texture-resources"></a><span id="Rendering_with_Texture_Resources"></span><span id="rendering_with_texture_resources"></span><span id="RENDERING_WITH_TEXTURE_RESOURCES"></span>利用紋理資源進行轉譯


Direct3D 透過紋理階段的概念支援多重紋理混合。 每個紋理階段都包含了一個紋理及可以執行於該紋理之上的運算。 紋理階段中的紋理構成了現有紋理的組合。 請參閱[紋理混合](texture-blending.md)。 每個紋理的狀態都封裝於其紋理階段中。

您的應用程式也可以設定紋理透視及紋理篩選狀態。 請參閱[紋理篩選](texture-filtering.md)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[紋理](textures.md)

 

 




