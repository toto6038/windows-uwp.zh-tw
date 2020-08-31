---
title: Texture3D 子資源拼貼
description: 根據 Texture2D 並排顯示的每個圖元位數，查看顯示 Texture3D 子資源方式的表格。
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Texture3D 子資源拼貼
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ee4ff5c87022f9fd303b1331665a2551704cb93
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167942"
---
# <a name="texture3d-subresource-tiling"></a>Texture3D 子資源拼貼


下表說明 [**Texture3D**](/windows/desktop/direct3dhlsl/sm5-object-texture3d) 子資源如何並排顯示。 此表格中的數值並未計入結尾 mip 包裝。

此表格利用了 [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) 拼接，將 x 及 y 維度各除以 4，並且增加了 16 層的深度。 所有第一個平面的磚 (2D 磚的平面定義了前 16 層深度) 都會在後續平面之前顯示。

**請注意：** 串流資源中針對   [**Texture3D**](/windows/desktop/direct3dhlsl/sm5-object-texture3d) 的支援並未在初始串流資源實作中公開，但所需要磚的形狀仍在此列出，作為未來版本可能的支援使用。

 

| 位元數/像素 (1 個樣本/像素) | 磚尺寸 (像素，寬 x 高 x 深) |
|-----------------------------|---------------------------------|
| 8                           | 64 x 32 x 32                        |
| 16                          | 32 x 32 x 32                        |
| 32                          | 32 x 32 x 16                        |
| 64                          | 32 x 16 x 16                        |
| 128                         | 16 x 16 x 16                        |
| BC1,4                       | 128 x 64 x 16                       |
| BC2,3,5,6,7                 | 64 x 64 x 16                        |

 

串流資源不支援的格式位元數目為 96 bpp 格式、影片格式、DXGI \_ 格式 \_ R1 \_ UNORM、dxgi \_ 格式 \_ R8G8 \_ B8G8 \_ UNORM，以及 dxgi \_ 格式 \_ R8R8 \_ G8B8 UNORM \_ 。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[如何拼貼串流資源區域](how-a-streaming-resource-s-area-is-tiled.md)

 

 