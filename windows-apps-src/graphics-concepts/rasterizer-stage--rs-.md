---
title: 轉譯器 (RS) 階段
description: 轉譯器會裁剪不在檢視中的基本類型、為像素著色器 (PS) 階段準備基本類型，以及決定如何叫用像素著色器。
ms.assetid: 7E80724B-5696-4A99-91AF-49744B5CD3A9
keywords:
- 轉譯器 (RS) 階段
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 17d58a05a57fa833003b7c229db91473f6cde3d4
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5814914"
---
# <a name="rasterizer-rs-stage"></a>轉譯器 (RS) 階段


轉譯器會裁剪不在檢視中的基本類型、為[像素著色器 (PS) 階段](pixel-shader-stage--ps-.md)準備基本類型，以及決定如何叫用像素著色器。 點陣化階段會轉換向量資訊 (由圖形或基本類型組成) 為點陣影像 (由像素組成)，以顯示即時 3D 圖形。

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>用途和使用


在點陣化期間，每個基本類型會轉換成像素，同時針對每個基本類型插入每個頂點值。 點陣化包含裁剪頂點到檢視範圍、執行以 z 相除以提供透視圖、將基本類型對應到 2D 檢視區，以及決定如何叫用像素著色器。 當使用像素著色器為選擇性時，而轉譯器階段始終會執行裁剪，分割透視圖以將點轉換為同質空間，以及將頂點對應到檢視區。

您可以告訴管線沒有任何像素著色器 (設定[像素著色器 (PS) 階段](pixel-shader-stage--ps-.md)為 **NULL** 並停用深度和樣板測試) 來停用點陣化。 停用時，不會更新與點陣化相關的管線計數器。

在實作階層式 Z 緩衝區最佳化的硬體上，您可以在啟用深度和樣板測試的同時，透過將像素著色器 (PS) 階段設定為 **NULL** 來預先載入 z 緩衝區。

請參閱[點陣化規則](rasterization-rules.md)。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>輸入


即將進入轉譯器階段的頂點 (x、y、z、w) 假設為在同質的剪輯空間中。 在這個座標空間中，X 軸指向右，Y 指向上，Z 偏離相機。

固定函式轉譯器 (RS) 階段是由串流輸出 (SO) 階段和/或上一個管線階段，例如[幾何著色器 (GS) 階段](geometry-shader-stage--gs-.md)供給。 如果未使用 GS，RS 是由[網域著色器 (DS) 階段](domain-shader-stage--ds-.md)供給。 如果也未使用 DS，RS 是由[頂點著色器 (VS) 階段](vertex-shader-stage--vs-.md)供給。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>輸出


使用像素著色器 (PS) 階段是選擇性的；轉譯器階段可以改為直接輸出至[輸出合併 (OM) 階段](output-merger-stage--om-.md)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[點陣化規則](rasterization-rules.md)

[圖形管線](graphics-pipeline.md)

 

 




