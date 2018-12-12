---
title: 紋理檢視
description: 在 Direct3D 中，紋理資源可使用檢視進行存取。這是硬體解譯記憶體中資源的一項機制。
ms.assetid: 18DABFCE-8A36-4C4E-B08E-10428B05D701
keywords:
- 紋理檢視
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e9167db4648dd193acaff0a224f3378486d171ad
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8941062"
---
# <a name="texture-views"></a>紋理檢視


在 Direct3D 中，紋理資源是使用檢視進行存取，這是硬體解譯記憶體中資源的一項機制。 在應用程式所需要的表示中，檢視允許特定的管線階段僅能存取其所需的[子資源](resource-types.md)。

檢視支援無類型資源的概念。 無類型資源是具有特定的大小，但不具備特定資料類型的資源。 資料在與管線繫結之後會進行動態解譯。

下列圖例呈現了透過建立著色器資源檢視，以將 2D 紋理陣列與 6 個紋理繫結，作為著色器資源使用的範例。 資源接著便會作為紋理陣列進行定址。 (注意︰子資源無法同時作為輸入及輸出繫結。)

![六個紋理的紋理陣列圖例](images/d3d10-cube-texture-faces.png)

當使用 2D 紋理陣列作為轉譯目標時，資源可以作為具備 Mipmap 層級 (在此範例中為 3) 的 2D 紋理陣列 (在此範例中為 6 個紋理) 檢視。

透過呼叫 CreateRenderTargetView 以建立轉譯目標的檢視物件。 接著呼叫 OMSetRenderTargets 為管線設定轉譯目標檢視。 藉由呼叫 Draw 及使用 RenderTargetArrayIndex，索引至陣列中適當的紋理，並且轉譯進轉譯目標之中。 您可使用子資源 (一個 Mipmap 層級與陣列索引的組合) 繫結至任何子資源的陣列。 如此一來，您就可以繫結至第二個 Mipmap 層級，並且在您需要的時候只更新此一特定 Mipmap 層級，如下圖例所示。

![僅繫結至紋理陣列的第二個 Mipmap 層級之圖例](images/d3d10-cube-texture-faces-subresource.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[資源](resources.md)

 

 




