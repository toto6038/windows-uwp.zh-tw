---
title: 計算著色器 (CS) 階段
description: 計算著色器 (CS) 階段提供高速的一般用途運算，並利用圖形處理單元上的大量平行處理器 (GPU) 。
ms.assetid: 300D4C0C-5450-45F8-9F29-E1A101D38F73
keywords:
- 計算著色器 (CS) 階段
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2dfdebc0a5219d48853cab08845fe4cfb45ee94f
ms.sourcegitcommit: 21eb13a50402bf5442a5f0a4bf34800d1dc679c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2020
ms.locfileid: "90804738"
---
# <a name="compute-shader-cs-stage"></a>計算著色器 (CS) 階段

計算著色器 (CS) 階段提供高速的一般用途運算，並利用圖形處理單元上的大量平行處理器 (GPU) 。 計算著色器階段提供記憶體共用和執行緒同步處理功能，以允許更有效的平行程式設計方法。

計算著色器可在許多執行緒上平行執行。

計算著色器是 [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)可程式化的著色器階段，可將 Direct3D 擴充到圖形程式設計之外。 計算著色器技術也稱為 *DirectCompute* 技術。 如需詳細資訊，另請參閱 [計算著色器總覽](/windows/win32/direct3d11/direct3d-11-advanced-stages-compute-shader)。

## <a name="related-topics"></a>相關主題

[計算管線](compute-pipeline.md) 
[圖形管線](graphics-pipeline.md)
