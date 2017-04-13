---
title: "串流資源的需求"
description: "串流資源能讓 GPU 記憶體不浪費在儲存未存取的表面區域，並告訴硬體如何跨相鄰磚篩選。"
ms.assetid: A88BE65B-104F-4176-9809-C12580A3684C
keywords: "串流資源的需求"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 5060d0076d93f8bca7e1547c4d9fb05ad4b1a3f5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="the-need-for-streaming-resources"></a>串流資源的需求


串流資源能讓 GPU 記憶體不浪費在儲存未存取的表面區域，並告訴硬體如何跨相鄰磚篩選。

## <a name="span-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanstreaming-resources-or-sparse-textures"></a><span id="Streaming_resources_or_sparse_textures"></span><span id="streaming_resources_or_sparse_textures"></span><span id="STREAMING_RESOURCES_OR_SPARSE_TEXTURES"></span>串流資源或疏鬆紋理


*「串流資源」* (在 Direct3D 11 中稱為*「並排資源」*)，為使用少量實體記憶體的大型邏輯資源。

串流資源的另一個名稱為*「疏鬆紋理」*。 「疏鬆」表達了並排資源的本質，以及並排它們的主要原因，也就是說這些資源並非會一次對應完畢。 事實上，可想而知應用程式可以刻意撰寫一種串流資源，其中沒有針對資源之所有區域和 Mip 撰寫的資料。 因此，內容本身應該為疏鬆的，而特定時間內圖形處理器 (GPU) 記憶體中的內容對應將會是該資源的子集 (更疏鬆)。

## <a name="span-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanwithout-tiling-memory-allocations-are-managed-at-subresource-granularity"></a><span id="Without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="WITHOUT_TILING__MEMORY_ALLOCATIONS_ARE_MANAGED_AT_SUBRESOURCE_GRANULARITY"></span>在沒有並排的情況下，應該在子資源資料粒度上管理記憶體配置


圖形系統 (也就是作業系統、顯示驅動程式及圖形硬體) 中若不支援串流資源，圖形系統即會在子資源資料粒度上管理所有 Direct3D 記憶體配置。

對於[緩衝區](introduction-to-buffers.md)而言，整個緩衝區都是子資源。

對於[紋理](textures.md)而言，(例如 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) (英文))，每個 MIP 層級都是子資源。對於紋理陣列而言，(例如 [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) (英文)) 每個特定陣列切割上的 MIP 層級都是子資源。 圖形系統只會公開在此子資源資料粒度上管理配置之對應的功能。 在串流資源的內容中，「對應」是指讓 GPU 能夠看到資料。

## <a name="span-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanwithout-tiling-cant-access-only-a-small-portion-of-mipmap-chain"></a><span id="Without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="WITHOUT_TILING__CAN_T_ACCESS_ONLY_A_SMALL_PORTION_OF_MIPMAP_CHAIN"></span>如果沒有並排，就不能只存取 Mipmap 鏈的一小部分


假設應用程式了解特定轉譯作業只需要存取圖片 Mipmap 鏈的一小部分 (甚至可能不到特定 Mipmap 的完整區域)。 理想上來說，該應用程式可能會向圖形系統通知這項需求。 則圖形系統之後便只會確保所需的記憶體在 GPU 上對應，而不會為太多記憶體進行分頁。

實際上，若是沒有串流資源的支援，圖形系統就只會知道於子資源資料粒度上對應 GPU 的記憶體需求 (例如，可存取的一系列完整 MIP 層級)。 圖形系統中不會發生隨選失敗，所以可能導致必須使用許多額外的 GPU 記憶體，讓完整子資源對應搶先在參照記憶體中任何部分的轉譯命令執行之前完成。 以上僅舉出一例在 Direct3D 內沒有串流資源的支援下，就難以使用大量記憶體配置的問題。

## <a name="span-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspansoftware-paging-to-break-the-surface-into-smaller-tiles"></a><span id="Software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="SOFTWARE_PAGING_TO_BREAK_THE_SURFACE_INTO_SMALLER_TILES"></span>軟體會使用分頁將表面分解為較小的磚


軟體分頁可將表面分解為硬體能夠處理的較小磚。

Direct3D 支援在特定邊上使用最多 16384 像素的 [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) 表面。 長寬為 16384 x 16384 且每像素 4 個位元組的影像會消耗 1 GB 的視訊記憶體 (新增 Mipmap 則會消耗加倍的量)。 在實際應用上，幾乎不需要在單一轉譯作業中參照完整 1GB 的大小。

有些遊戲開發人員會用最大至 128k x 128K 的大小塑造地型表面。 讓這種作法能在現有 GPU 上運作的方式，是將表面分解為硬體能夠處理的小型磚。 應用程式必須了解需要將哪個磚載入 GPU 上紋理的快取內，這也就是軟體分頁系統的作用。

此方法的一項重大缺點，在於硬體完全不了解任何進行中的分頁作業︰當一部分影像需要顯示在跨立於磚的螢幕上時，硬體會不知道如何跨磚執行固定功能 (也就是有效率地執行) 篩選。 這表示必須在著色器程式碼中使用手動紋理篩選，以管理應用程式本身的軟體並排 (如果想要使用高品質非等向性篩選，成本將會變得很高)，以及 (或) 必須浪費記憶體在磚上製作包含鄰近磚資料的裝訂邊，以便固定功能硬體篩選得以繼續提供協助。

## <a name="span-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanmaking-tiled-representation-of-surface-allocations-a-first-class-feature"></a><span id="Making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="MAKING_TILED_REPRESENTATION_OF_SURFACE_ALLOCATIONS_A_FIRST-CLASS_FEATURE"></span>讓表面配置的並排表示成為第一級功能


如果表面配置的並排表示是圖形系統中的第一級功能，應用程式將能通知硬體，讓硬體提供正確的磚。 如此一來，就能浪費較少 GPU 記憶體在儲存應用程式未存取的表面區域上，且硬體會了解如何進行跨相鄰磚篩選，減輕開發人員在執行軟體並排時所遇到的一些麻煩。

但若要提供完整的解決方案，有些項目必須先處理完成，像是不論是否支援表面上的並排，目前最大的表面維度是 16384，這和應用程式想要的 128K+ 差距很大。 僅讓硬體支援較大的紋理大小是一種方法，但採取此方法需要花費大量費用和 (或) 其他方面的取捨。

若要讓 16K 紋理支援其他需求，則會超過 Direct3D 之紋理篩選路徑和轉譯路徑的精確度極限，造成支援的檢視區在轉譯時會偏離表面，或支援的紋理在篩選時沒有對齊表面邊緣等情況。 可能需要進行取捨，像是紋理大小增加超過 16K 時，功能或精確度會在某方面失效。 即使在功能上做出這項讓步，還是可能需要額外的硬體效能，才能提升整體硬體系統的處理性能，以轉換至更大的紋理大小。

## <a name="span-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanissue-with-large-textures-precision-for-locations-on-surface"></a><span id="Issue_with_large_textures__precision_for_locations_on_surface"></span><span id="issue_with_large_textures__precision_for_locations_on_surface"></span><span id="ISSUE_WITH_LARGE_TEXTURES__PRECISION_FOR_LOCATIONS_ON_SURFACE"></span>大型紋理會出現的問題︰表面位置的精確度


當紋理變得非常大時會浮現一個問題，那就是單精確度浮點數紋理座標 (以及已與座標建立關聯的內插運算，用以支援點陣化) 將用完精確度，因此無法準確指定表面上的位置。 在這之後會發生抖動紋理篩選。 這時其中一個較為昂貴的選項是需要雙精確度內插運算支援，但在有其他選擇時，此選項可能會有點大材小用。

## <a name="span-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanenabling-multiple-resources-of-different-dimensions-to-share-memory"></a><span id="Enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="ENABLING_MULTIPLE_RESOURCES_OF_DIFFERENT_DIMENSIONS_TO_SHARE_MEMORY"></span>讓不同維度中的多個資源分享記憶體


串流資源上的另一個案例是讓不同維度或格式的多個資源共用相同的記憶體。 有時候應用程式有無法同時使用的專屬資源集，或建立來僅供短暫期間使用並在使用後就刪除的資源，而在資源刪除後接著再建立其他資源。 一種常被忽略的「串流資源」形式是可以讓使用者指向相同 (重疊) 記憶體中的多個不同的資源。 意即，可以從應用程式觀點將「資源」的建立和刪除 (定義維度或格式等項目) 和具備基礎資源之記憶體的管理分離。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流資源](streaming-resources.md)

 

 




