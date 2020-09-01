---
title: 第 1 層
description: 本節描述第 1 層支援。
ms.assetid: 153DA429-0C99-4691-AEB4-124FD9B17BE2
keywords:
- 第 1 層
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7fcd5debb367b73ad23492cf63d3acdd1797c23a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162702"
---
# <a name="tier-1"></a>第 1 層


本節描述第 1 層支援。

## <a name="span-idtier_1_general_limitationsspanspan-idtier_1_general_limitationsspanspan-idtier_1_general_limitationsspantier-1-general-limitations"></a><span id="Tier_1_general_limitations"></span><span id="tier_1_general_limitations"></span><span id="TIER_1_GENERAL_LIMITATIONS"></span>第 1 層一般限制


-   硬體至少為功能層級 11.0。
-   無退出支援。
-   無 Texture1D 或 Texture3D 支援。
-   無 2、8 或 16 倍多重採樣消除鋸齒 (MSAA) 支援。 僅需 4x，除非不具 128 bpp 格式。
-   無任何標準拌和模式 (64 KB 磚和結尾 MIP 封裝中的配置依硬體廠商而定)。
-   重複對應時存取磚的限制。 請參閱[重複對應的磚存取限制](tile-access-limitations-with-duplicate-mappings.md) (英文)。

## <a name="span-idspecific_limitations_affecting_tier_1_onlyspanspan-idspecific_limitations_affecting_tier_1_onlyspanspan-idspecific_limitations_affecting_tier_1_onlyspanspecific-limitations-affecting-tier-1-only"></a><span id="Specific_limitations_affecting_tier_1_only"></span><span id="specific_limitations_affecting_tier_1_only"></span><span id="SPECIFIC_LIMITATIONS_AFFECTING_TIER_1_ONLY"></span>特定限制只會影響到第 1 層


### <a name="span-idreading_writing_to_streaming_resources_that_have_null_mappingsspanspan-idreading_writing_to_streaming_resources_that_have_null_mappingsspanspan-idreading_writing_to_streaming_resources_that_have_null_mappingsspanreadingwriting-to-streaming-resources-that-have-null-mappings"></a><span id="Reading_writing_to_streaming_resources_that_have_NULL_mappings"></span><span id="reading_writing_to_streaming_resources_that_have_null_mappings"></span><span id="READING_WRITING_TO_STREAMING_RESOURCES_THAT_HAVE_NULL_MAPPINGS"></span>讀取/寫入具有 Null 對應的串流資源

串流處理資源可以具有 **NULL** 對應，但讀取或寫入它們會產生未定義的結果，其中包括裝置遭到移除。 應用程式能將虛設頁對應至所有空白的區域來避免發生此情況。 如果您寫入並轉譯對應至多個轉譯目標地點的頁面，請特別小心，因為寫入的順序為未定義。

### <a name="span-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanspan-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanspan-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanno-shader-instructions-for-clamping-lod-and-mapped-status-feedback"></a><span id="No_shader_instructions_for_clamping_LOD_and_mapped_status_feedback"></span><span id="no_shader_instructions_for_clamping_lod_and_mapped_status_feedback"></span><span id="NO_SHADER_INSTRUCTIONS_FOR_CLAMPING_LOD_AND_MAPPED_STATUS_FEEDBACK"></span>沒有固定」 LOD 和對應狀態回饋的著色器指示

無著色器指示可供鉗制 LOD 與對應狀態回應使用。 請參閱 [HLSL 串流資源曝光](hlsl-streaming-resources-exposure.md) (英文)。

### <a name="span-idalignment_constraints_for_standard_tile_shapesspanspan-idalignment_constraints_for_standard_tile_shapesspanspan-idalignment_constraints_for_standard_tile_shapesspanalignment-constraints-for-standard-tile-shapes"></a><span id="Alignment_constraints_for_standard_tile_shapes"></span><span id="alignment_constraints_for_standard_tile_shapes"></span><span id="ALIGNMENT_CONSTRAINTS_FOR_STANDARD_TILE_SHAPES"></span>標準磚圖形的對齊條件約束

它只保證維度為標準磚大小倍數的 Mip (從最小的開始) 支援標準磚圖形，而且可以任意對應或取消對應個別的磚。 在串流資源中，第一個有維度大小並非標準磚大小倍數的 Mipmap，以及所有粗 Mipmap，都可以是非標準並排圖形，並立刻配入此 Mip 集的 N 64KB 磚 (將 N 回報至應用程式)。 這些 N 磚被壓縮為一個單位，而且在任何時候都必須由應用程式完全對應或完全不對應，但是每個 N 磚都可以對應至磚集區中的任意斷續位置。

### <a name="span-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanspan-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanspan-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanarray-of-mipmaps-that-arent-a-multiple-of-standard-tile-size"></a><span id="Array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="ARRAY_OF_MIPMAPS_THAT_AREN_T_A_MULTIPLE_OF_STANDARD_TILE_SIZE"></span>非標準磚大小之倍數的 Mipmap 陣列

Mipmap 在所有維度中都並非標準磚大小之倍數的串流資源，其陣列大小不得超過 1。

### <a name="span-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanspan-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanspan-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanswitching-between-referencing-tiles-in-a-tile-pool-via-a-buffer-and-texture-resource"></a><span id="Switching_between_referencing_tiles_in_a_tile_pool_via_a_Buffer_and_Texture_resource"></span><span id="switching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resource"></span><span id="SWITCHING_BETWEEN_REFERENCING_TILES_IN_A_TILE_POOL_VIA_A_BUFFER_AND_TEXTURE_RESOURCE"></span>透過緩衝區和紋理資源，在磚集區中切換參考的磚

若要透過 [緩衝區](introduction-to-buffers.md) 資源在磚集區中的參考磚之間切換，以透過 [材質](introduction-to-textures.md) 資源參考相同的磚，或反之亦然，將對應加入至這些磚集區的磚對應或複製磚對應的最新更新，必須是相同資源維度 (緩衝區和材質 \*) 作為用來存取磚的資源維度。 否則，行為未定義，這包括裝置重設的機會。

舉例來說，更新磚對應來定義緩衝區的磚對應，並透過 [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) 資源將磚對應更新為和磚集區中的某些磚相同，接著透過緩衝區存取磚是無效的方式。 您可以在切換緩衝區和紋理 (反之亦然) 之間分享的磚時為資源重新定義磚，或是不在緩衝區資源和紋理資源之間分享磚集區中的磚，以避免這種情形。

### <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>最小/最大削減篩選

不支援最小/最大削減篩選。 請參閱[串流資源紋理取樣功能](streaming-resources-texture-sampling-features.md) (英文)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流資源功能層級](streaming-resources-features-tiers.md)

 

 