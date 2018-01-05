---
title: "單色光線對應"
description: "當較舊的 3D 加速板不支援使用目的地像素 alpha 值的紋理混合時，單色光線對應可讓較舊的介面卡執行物件多重紋理混合。"
ms.assetid: 60F8F8F6-9DB7-452B-8DC0-407FFAA4BFE1
keywords: "單色光線對應"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67029443f59d87c7f3e38560595dca1b7b399632
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/22/2017
---
# <a name="monochrome-light-maps"></a>單色光線對應


當較舊的 3D 加速板不支援使用目的地像素 alpha 值的紋理混合時，單色光線對應可讓較舊的介面卡執行物件多重紋理混合。

某些舊版 3D 加速板不支援使用 alpha 目的地像素值的紋理混合。 通常這些介面卡也不支援多紋理混合。 如果您的應用程式正在這類的介面卡上執行，就可以使用多重紋理混合來執行單色光線對應。

若要執行單色光線對應，應用程式會將光源資訊儲存在光線對應紋理的 alpha 資料中。 此應用程式使用 Direct3D 的紋理篩選功能，從基本影像中的每個像素對應到光線圖中的對應紋素中。 它將來源混合因素設定為對應紋素的 alpha 值。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[光線與紋理對應](light-mapping-with-textures.md)

 

 




