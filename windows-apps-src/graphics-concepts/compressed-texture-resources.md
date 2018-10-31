---
title: 壓縮紋理資源
description: 紋理貼圖為繪製在三維圖形上的數位化影像，用來增加更多視覺上的細節。
ms.assetid: 2DD5FF94-A029-4694-B103-26946C8DFBC1
keywords:
- 壓縮紋理資源
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e808bf0fe1f521a60aa347efd148ede96be95964
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5835308"
---
# <a name="compressed-texture-resources"></a>壓縮紋理資源


紋理貼圖為繪製在三維圖形上的數位化影像，用來增加更多視覺上的細節。 他們在點陣化的過程中對應至這些圖形之上，並且整個過程可能會取用大量的系統匯流排及記憶體。 若要減少紋理所使用的記憶體，Direct3D 支援壓縮紋理表面。 某些 Direct3D 裝置原生支援壓縮紋理表面。 在這類裝置上，當您建立了一個壓縮表面並且將資料載入其中，該表面即可像任何其他的紋理表面一樣用於 Direct3D 之中。 Direct3D 將會在該紋理對應至 3D 物件之上時，對其進行解壓縮。

## <a name="span-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanstorage-efficiency-and-texture-compression"></a><span id="Storage-Efficiency-and-Texture-Compression"></span><span id="storage-efficiency-and-texture-compression"></span><span id="STORAGE-EFFICIENCY-AND-TEXTURE-COMPRESSION"></span>儲存效率與紋理壓縮


所有紋理壓縮格式都是二的乘冪。 雖然這並不表示紋理一定是矩形，但實際上紋理的 x 值和 y 值的確都是二的乘冪。 例如：一個原本是 512 x 128 位元組的紋理，在下一個 Mipmapping 將會縮小至 256 x 64，並且在每個層級以 2 的乘冪遞減。 當紋理在較低的層級被篩選至 16 x 2 或 8 x 1 時，將會有未充分利用的位元存在，因為壓縮區塊一律都是 4 x 4 的材質區塊。 未使用的區塊部分將會獲得填補。

雖然在最低的層級時會有未充分利用的位元，整體增益仍然相當可觀。 理論上，最差的情況便是 2K x 1 的紋理 (2⁰ 乘冪)。 在這種情況下，每一區塊只有一列像素獲得編碼，區塊的其餘部分都將不會被使用到。

## <a name="span-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanmixing-formats-within-a-single-texture"></a><span id="Mixing-Formats-Within-a-Single-Texture"></span><span id="mixing-formats-within-a-single-texture"></span><span id="MIXING-FORMATS-WITHIN-A-SINGLE-TEXTURE"></span>在單一紋理中混合格式


請務必注意任何單一紋理都必須指定由 16 個材質組成的每個群組，資料將會以 64 位元或是 128 位元的方式儲存。 如果 64 位元區塊 (即 BC1 區塊壓縮格式) 指定用於紋理，在同樣的紋理之中，便可以每個區塊作為基礎來混合不透明物與 1 位元的 Alpha 格式。 換句話說，針對每個 16 個材質組成的區塊，color\_0 與 color\_1 之間不帶正負號的整數大小比較，將會以唯一的方式進行。

使用 128 位元區塊時，整個紋理的 Alpha 色板必須指定為明確模式 (BC2 格式) 或插入模式 (BC3 格式)。 至於色彩，選取插入模式 (BC3 格式) 時，您可逐區塊地選擇使用八個 Alpha 值插入模式或六個 Alpha 值插入模式。 同樣地，alpha\_0 和 alpha\_1 之間大小的比較也會逐區塊地以唯一的方式進行。

Direct3D 針對用於 3D 模型貼圖的表面，提供了壓縮的服務。 本章節提供了建立和管理壓縮紋理表面中資料的相關資訊。

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
<td align="left"><p><a href="opaque-and-1-bit-alpha-textures.md">不透明和 1 位元 Alpha 紋理</a></p></td>
<td align="left"><p>紋理格式 BC1 主要針對不透明或只有單一透明色彩的紋理。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures-with-alpha-channels.md">使用 Alpha 色板的紋理</a></p></td>
<td align="left"><p>有兩種方式可以對呈現較複雜透明度的材質貼圖進行編碼。 在每個例子中，描述透明度的區塊都會優先於之前已描述完成的 64 位元區塊。 透明度通常不是透過每像素 4 個位元 (明確編碼) 的 4 x 4 點陣圖呈現，就是透過較少位元並且類比於色彩編碼的線性插補呈現。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="block-compression.md">區塊壓縮</a></p></td>
<td align="left"><p>區塊壓縮是一種失真紋理壓縮技術，主要用於減少紋理的大小和磁碟使用量並提升效能。 區塊壓縮紋理可以比每個色彩 32 位元的紋理更小。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="compressed-texture-formats.md">壓縮紋理格式</a></p></td>
<td align="left"><p>本章節包含了壓縮紋理格式內部組織的相關資訊。 您不需要這些詳細資料即可使用壓縮紋理，因為您可以使用 Direct3D 功能對壓縮格式進行轉換。 但是，若您想要對壓縮表面資料進行直接的操作，這項資訊對您將會非常有幫助。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[紋理](textures.md)

 

 




