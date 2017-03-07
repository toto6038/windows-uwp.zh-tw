---
title: "Texture3D 子資源拼接"
description: "下表顯示了 Texture3D 子資源的拼接方式。"
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- "Texture3D 子資源拼接"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 6f6c2648ceefae5b13b481decc37be4047d938d4
ms.lasthandoff: 02/07/2017

---

# <a name="texture3d-subresource-tiling"></a>Texture3D 子資源拼接


下表顯示了 [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) 子資源的拼接方式。 此表格中的數值並未計入結尾 mip 包裝。

此表格利用了 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 拼接，將 x 及 y 維度各除以 4，並且增加了 16 層的深度。 所有第一個平面的磚 (2D 磚的平面定義了前 16 層深度) 都會在後續平面之前顯示。

**請注意：**串流資源中針對 [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) 的支援並未在初始串流資源實作中公開，但所需要磚的形狀仍在此列出，作為未來版本可能的支援使用。

 

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


[串流資源區域的拼接方式](how-a-streaming-resource-s-area-is-tiled.md)

 

 





