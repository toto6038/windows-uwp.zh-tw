---
title: 添加紋理基本概念
description: 早期電腦產生的 3D 影像，雖然普遍對當代而言算先進，但通常會有具光澤的塑膠感。
ms.assetid: 3CA3905D-E837-48EB-A81F-319AA1C6537E
keywords:
- 添加紋理基本概念
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c48b7b007c1af1eaf6013d5f6e99468713d50745
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "5749976"
---
# <a name="basic-texturing-concepts"></a>添加紋理基本概念


早期電腦產生的 3D 影像，雖然普遍對當代而言算先進，但通常會有具光澤的塑膠感。 其缺乏可讓 3D 物件看起來逼真且複雜的標記類型，例如磨損、裂縫，指紋及指尖。 紋理在加強電腦產生之 3D 影像的真實性方面，變得愈來愈受歡迎。

在日常的使用中，文字紋理是指物件的平滑或粗糙度。 不過，在電腦圖形中，紋理是像素色彩的點陣圖，讓物件具有紋理的外觀。

因為 Direct3D 紋理為點陣圖，所以任何點陣圖都可套用至 Direct3D 原始物件。 例如，應用程式可以建立並操控似乎具備木紋圖樣的物件。 草地、泥土和岩石可套用至形成山丘的一組 3D 原始物件。 結果為逼真的山坡。 您也可以添加紋理來建立效果，例如路旁的路標、懸崖的岩石層，或地板大理石的外觀。

此外，Direct3D 支援更進階的添加紋理技術，例如紋理混合 (選擇性搭配透明度) 及光線對應。 參閱[紋理混合](texture-blending.md)和[使用紋理進行光線對應](light-mapping-with-textures.md)。

如果您的應用程式建立硬體抽象層 (HAL) 裝置或軟體裝置，其可以使用 8、16、24、32、64 或 128 位元紋理。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[紋理](textures.md)

 

 




