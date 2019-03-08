---
title: Texture3D 子資源拼貼
description: 下表顯示了 Texture3D 子資源的拼接方式。
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Texture3D 子資源拼貼
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c9c232bc60bbbb3cccc16618d82ec23452c58ee8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645953"
---
# <a name="texture3d-subresource-tiling"></a>Texture3D 子資源拼貼


下表顯示了 [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) 子資源的拼接方式。 此表格中的數值並未計入結尾 mip 包裝。

此表格利用了 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 拼接，將 x 及 y 維度各除以 4，並且增加了 16 層的深度。 所有第一個平面的磚 (2D 磚的平面定義了前 16 層深度) 都會在後續平面之前顯示。

**請注意：** 串流資源中針對   [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) 的支援並未在初始串流資源實作中公開，但所需要磚的形狀仍在此列出，作為未來版本可能的支援使用。

 

| 位元數/像素 (1 個樣本/像素) | 磚尺寸 (像素，寬 x 高 x 深) |
|-----------------------------|---------------------------------|
| 8                           | 64 x 32 x 32                        |
| 16                          | 32 x 32 x 32                        |
| 32                          | 32 x 32 x 16                        |
| 64                          | 32 x 16 x 16                        |
| 128                         | 16 x 16 x 16                        |
| BC1,4                       | 128 x 64 x 16                       |
| BC2,3,5,6,7                 | 64 x 64 x 16                        |

 

不支援使用資料流資源的格式元計數是 96 bpp 格式的視訊格式，DXGI\_格式\_R1\_UNORM、 DXGI\_格式\_R8G8\_B8G8\_UNORM，和 DXGI\_格式\_R8R8\_G8B8\_UNORM。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[資料流資源的區域會並排顯示方式](how-a-streaming-resource-s-area-is-tiled.md)

 

 




