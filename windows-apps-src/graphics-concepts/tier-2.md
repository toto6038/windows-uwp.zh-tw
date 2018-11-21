---
title: 第 2 層
description: 串流資源的第 2 層支援新增第 1 層以外的功能，例如當大小至少是一個標準磚形狀時保證非壓縮紋理 Mipmap；鉗制詳細層級 (LOD) 以及取得著色器作業狀態的相關著色器指示；此外，讀取 NULL 對應磚，將取樣值視為零。
ms.assetid: 111A28EA-661A-4D29-921A-F2E376A46DC5
keywords:
- 第 2 層
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fac8780995231ce56d1264ea8a1a5cb52fd9a3d0
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2018
ms.locfileid: "7424554"
---
# <a name="tier-2"></a>第 2 層


串流資源的第 2 層支援新增第 1 層以外的功能，例如當大小至少是一個標準磚形狀時保證非壓縮紋理 Mipmap；鉗制詳細等級 (LOD) 以及取得著色器作業狀態的相關著色器指示；此外，讀取 NULL 對應磚，將取樣值視為零。

## <a name="span-idtier2generalsupportspanspan-idtier2generalsupportspanspan-idtier2generalsupportspantier-2-general-support"></a><span id="Tier_2_general_support"></span><span id="tier_2_general_support"></span><span id="TIER_2_GENERAL_SUPPORT"></span>第 2 層一般支援


第 2 層支援包括下列各項。

-   硬體至少為功能層級 11.1。
-   前一層的所有功能 (但沒有[第 1 層](tier-1.md)特定的限制) 以及新增的下列項目︰
-   有著色器指示可供鉗制 LOD 與對應狀態回應使用。 請參閱 [HLSL 串流資源曝光](hlsl-streaming-resources-exposure.md) (英文)。

除了這些項目外，還會出現一些特定的支援問題。

## <a name="span-idnon-mappedtilesspanspan-idnon-mappedtilesspanspan-idnon-mappedtilesspannon-mapped-tiles"></a><span id="Non-mapped_tiles"></span><span id="non-mapped_tiles"></span><span id="NON-MAPPED_TILES"></span>非對應的磚


讀取非對應的磚時，會在格式的所有非遺漏元件以及遺失元件的預設值內傳回 0。

非對應磚的寫入將停止移至記憶體，但最後可能會傳至快取，該快取後續會讀取同一個可能會被選取的位址。

## <a name="span-idtexturefilteringspanspan-idtexturefilteringspanspan-idtexturefilteringspantexture-filtering"></a><span id="Texture_filtering"></span><span id="texture_filtering"></span><span id="TEXTURE_FILTERING"></span>紋理篩選


記憶體使用量供 **NULL** 與非 **NULL** 磚共用的紋理篩選，會為 **NULL** 磚上的材質，而將 0 (含遺失格式元件預設值) 提供至整體篩選作業。 如果任何材質 (含非零權數) 落在 **NULL** 磚上，某些舊版的硬體會不符合此需求，並讓完整篩選結果傳回 0 (含遺失格式元件預設值)。 不允許任何其他硬體遺漏將所有 (非零權數) 材質包含在篩選作業中的需求。

存取 **NULL** 材質會造成狀態回應上 [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) (英文) 作業的紋理讀取傳回 False。 無論紋理存取結果如何在著色器內寫入遮罩，以及多少元件為紋理格式 (紋理格式的組合可能會造成不須存取其紋理的假象)，都會發生這項問題。

## <a name="span-idalignmentconstraintsspanspan-idalignmentconstraintsspanspan-idalignmentconstraintsspanalignment-constraints"></a><span id="Alignment_constraints"></span><span id="alignment_constraints"></span><span id="ALIGNMENT_CONSTRAINTS"></span>對齊限制


標準磚圖形對齊限制︰在所有維度填滿至少一個標準磚的 Mipmap 一定會使用標準並排，餘數會視為**單位**而封裝至 N 磚 (將 N 回報至應用程式)。 應用程式可將 N 磚對應至磚集區中的任意斷續位置，但必須將所有的壓縮磚都對應，否則全都不對應。 Mip 套件是每個陣列切割中的一組獨特壓縮磚。

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>最小/最大削減篩選


支援最小/最大削減篩選。 請參閱[串流資源紋理取樣功能](streaming-resources-texture-sampling-features.md) (英文)。

## <a name="span-idlimitationsspanspan-idlimitationsspanspan-idlimitationsspanlimitations"></a><span id="Limitations"></span><span id="limitations"></span><span id="LIMITATIONS"></span>限制


在任何維度中，若是串流資源使用的 Mipmap 小於標準磚大小，則任何陣列大小都不允許超過 1。

當重複對應繼續適用時，在存取磚上的限制。 請參閱[重複對應的磚存取限制](tile-access-limitations-with-duplicate-mappings.md) (英文)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流資源功能層級](streaming-resources-features-tiers.md)

 

 




