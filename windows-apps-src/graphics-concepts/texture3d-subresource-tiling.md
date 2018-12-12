---
title: Texture3D 子資源拼接
description: 下表顯示了 Texture3D 子資源的拼接方式。
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Texture3D 子資源拼接
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c9c232bc60bbbb3cccc16618d82ec23452c58ee8
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8933182"
---
# <a name="texture3d-subresource-tiling"></a>Texture3D 子資源拼接


下表顯示了 [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) 子資源的拼接方式。 此表格中的數值並未計入結尾 mip 包裝。

此表格利用了 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 拼接，將 x 及 y 維度各除以 4，並且增加了 16 層的深度。 所有第一個平面的磚 (2D 磚的平面定義了前 16 層深度) 都會在後續平面之前顯示。

**注意：**[**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562)支援串流資源不在初始串流資源實作中，公開，但所需要的磚圖形將此處列出作為未來版本中可能的支援。

 

| 位元數/像素 (1 個樣本/像素) | 磚尺寸 (像素，寬 x 高 x 深) |
|-----------------------------|---------------------------------|
| 8                           | 64 x 32 x 32                        |
| 16                          | 32 x 32 x 32                        |
| 32                          | 32 x 32 x 16                        |
| 64                          | 32 x 16 x 16                        |
| 128                         | 16 x 16 x 16                        |
| BC1,4                       | 128 x 64 x 16                       |
| BC2,3,5,6,7                 | 64 x 64 x 16                        |

 

不受串流資源支援的格式位元計算包括了 96 bpp 格式、視訊格式、DXGI\_FORMAT\_R1\_UNORM、DXGI\_FORMAT\_R8G8\_B8G8\_UNORM，以及 DXGI\_FORMAT\_R8R8\_G8B8\_UNORM。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[如何拼貼串流資源區域](how-a-streaming-resource-s-area-is-tiled.md)

 

 




