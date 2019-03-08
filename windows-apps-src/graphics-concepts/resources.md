---
title: 資源
description: 資源是 Direct3D 管線可以存取的記憶體中的區域。
ms.assetid: 2E68E5A8-83DA-4DC8-B7F3-B8988CF8090C
keywords:
- 資源
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c31dcbcc3019538d769118b018c693174b17b4c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631473"
---
# <a name="resources"></a>資源


資源是 Direct3D 管線可以存取的記憶體中的區域。 為了讓管線有效地存取記憶體，提供給管線的資料 (例如，輸入幾何、著色器資源及紋理) 必須儲存在資源中。 所有 Direct3D 資源衍生兩種類型的資源︰緩衝區或紋理。 每個管線階段可使用多達 128 種資源。

每個應用程式通常會建立許多資源。 資源的範例包括︰頂點緩衝區、索引緩衝區、常數緩衝區、紋理和著色器資源。 有幾個決定資源如何使用的選項。 您可以建立強類型或無類型的資源；您可以控制資源是否擁有讀取和寫入兩項存取權；您可以讓資源只供 CPU、GPU 或二者存取。 當然，會有速度和功能上的取捨，您若允許資源擁有更多的功能，您可預期的效能會更低。

因為應用程式通常會使用許多紋理，所以 Direct3D 也有紋理陣列的概念以簡化紋理管理。 紋理陣列包含一個或多個紋理 (所有類型與大小均相同)，可以從應用程式中或由著色器編制索引。 紋理陣列可讓您使用具有多個索引的單一介面來存取許多紋裡。 您可以依需求建立任意數量的紋理陣列來管理不同紋理類型。

當您建立好應用程式將使用的資源之後，您會連接或繫結每個資源到將使用它們的管線階段。 您可以呼叫繫結 API 來完成這項作業，也就是指向資源。 因為多個管線階段可能需要存取相同資源，所有 Direct3D 具有資源檢視的概念。 檢視會辨識可存取的部分資源。 您可以建立 *m* 檢視或資源並將它們繫結到 *n* 管線階段，假設是您遵循共用資源的繫結規則 (若不這麼做，執行階段會在編譯時產生錯誤)。

資源檢視提供一般模型來存取資源 (例如紋裡或緩衝區)。 因為您可以使用檢視來告訴執行階段進行存取以及如何存取，所以資源檢視可讓您建立無類型的資源。 也就是您可以在編譯時建立指定大小的資源，然後在資源繫結到管線時宣告資源內的資料類型。 檢視會公開許多使用資源的功能，例如讀回著色器中的深度/樣板表面，在單一行程中產生動態立方體地圖，以及同時轉譯到磁碟區的多個扇區。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本節內容


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="resource-types.md">資源類型</a></p></td>
<td align="left"><p>不同類型的資源會有不同的配置 (或記憶體使用量)。 Direct3D 管線使用的所有資源都衍生自兩個基本資源類型︰<a href="resource-types.md#buffer-resources">緩衝區</a>和<a href="resource-types.md#texture-resources">紋理</a>。 緩衝區是原始資料 (元素) 的集合，紋理是紋素 (紋理元素) 的集合。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="choosing-a-resource.md">選擇資源</a></p></td>
<td align="left"><p>資源是 3D 管線使用的一組資料。 建立資源及定義其行為，是應用程式的程式設計首項步驟。 本指引涵蓋基本主題，內容為選擇應用程式所需的資源。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="copying-and-accessing-resource-data.md">複製以及存取資源的資料</a></p></td>
<td align="left"><p>使用方式旗幟表示了應用程式將會如何使用資源資料，並在可能的情況下將資源放置於記憶體中效能最佳的區域。 資源資料在所有資源中都會被複製，以便 CPU 或 GPU 可以在不影響效能的情況下進行存取。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-views.md">紋理檢視</a></p></td>
<td align="left"><p>在 Direct3D 中，紋理資源是使用檢視進行存取，這是硬體解譯記憶體中資源的一項機制。 檢視允許特定的管線階段，在應用程式所要的表示中，只能存取其所需的<a href="resource-types.md">子資源</a>。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[座標系統](coordinate-systems.md)

[Direct3D 圖形學習指南](index.md)

[浮點數的規則](floating-point-rules.md)

[資料類型轉換](data-type-conversion.md)
