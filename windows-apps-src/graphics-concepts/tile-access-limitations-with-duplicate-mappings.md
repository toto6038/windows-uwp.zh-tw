---
title: 重複對應的磚存取限制
description: 重複對應的磚有存取限制，例如複製來源和目的地重疊的串流資源時，或轉譯至在轉譯區域中共用的磚時。
ms.assetid: 6E40B1DC-BCF1-4B09-82A8-7B2D9B209A61
keywords:
- 重複對應的磚存取限制
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ce8f71f126aa536f4e235c58f9f5c5ddf13654df
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5931099"
---
# <a name="tile-access-limitations-with-duplicate-mappings"></a>重複對應的磚存取限制


重複對應的磚有存取限制，例如複製來源和目的地重疊的串流資源時，或轉譯至在轉譯區域中共用的磚時。

## <a name="span-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspancopying-streaming-resources-with-overlapping-source-and-destination"></a><span id="Copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="COPYING_STREAMING_RESOURCES_WITH_OVERLAPPING_SOURCE_AND_DESTINATION"></span>複製來源和目的地重疊的串流資源


如果 Copy\* 作業來源和目的地區域中的磚在複製區域有重複對應，而即使這兩個資源未串流資源仍會重疊且 Copy\* 作業支援重疊複製，則 Copy\* 作業會正常運作 (如同來源在前往目的地前會先複製到暫存位置)。 但如果重疊不明顯 (例如來源和目的地資源不同，但給定表面上的共用對應或對應重複)，則不會定義共用磚上複製作業的結果。

## <a name="span-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspancopying-to-streaming-resource-with-duplicated-tiles-in-destination-area"></a><span id="Copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="COPYING_TO_STREAMING_RESOURCE_WITH_DUPLICATED_TILES_IN_DESTINATION_AREA"></span>複製到在目的地區域中有重複磚的串流資源


若複製到在目的地區域中有重複磚的串流資源，則會在這些磚中產生未定義的結果，除非資料本身相同。不同的磚可能會以不同的順序寫入磚。

## <a name="span-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanuav-accesses-to-duplicate-tiles-mappings"></a><span id="UAV_accesses_to_duplicate_tiles_mappings"></span><span id="uav_accesses_to_duplicate_tiles_mappings"></span><span id="UAV_ACCESSES_TO_DUPLICATE_TILES_MAPPINGS"></span>UAV 對重複磚對應的存取


假設串流資源上未排序的存取檢視 (UAV) 在其區域有重複磚對應，或有與其他管線繫結的資源。 若由不同執行緒則執行，則不會定義這些重複磚的存取順序，就像任何 UAV 記憶體存取的順序通常都未排序。

## <a name="span-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanrendering-after-tile-mapping-changes-or-content-updates-from-outside-mappings"></a><span id="Rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="RENDERING_AFTER_TILE_MAPPING_CHANGES_OR_CONTENT_UPDATES_FROM_OUTSIDE_MAPPINGS"></span>磚對應變更或內容從對應外部更新後轉譯


如果串流資源的磚對應已變更，或在對應並排集區磚的內容透過另一個串流資源的對應進行變更，且串流資源即將透過轉譯目標檢視或深度樣板檢視進行轉譯，則應用程式必須清除 (使用固定的功能清除 API)，或使用 Copy\*/Update\* API 已在轉譯中區域 (對應或未對應) 變更的磚進行完整複製。

若應用程式無法在這些案例中進行清除或複製，則給定轉譯目標檢視或深度樣板檢視的硬體最佳化結構會過時，且會導致某些硬體上的廢棄項目轉譯結果，以及其他硬體間的不一致。 硬體所使用的這些隱藏的最佳化資料結構可能對個別對應而言為本機項目，而不會對其他記憶體相同的對應顯示。

當您清除資源檢視 (將資源檢視中的所有項目都設為一個值) 時，您可以清除具備矩形的轉譯目標檢視。 若為支援串流資源的硬體，清除資源檢視必須也支援為僅限深度的表面 (不含樣板) 清除具備矩形的深度樣板檢視。 這項作業可讓應用程式只清除表面的必要區域。

若應用程式需要保留串流資源中區域現有的記憶體內容，其中該串流資源的對應已變更，則該應用程式必須針對清楚的需求採取因應措施。 若應用程式要完成此因應措施，可以先儲存磚對應已變更的內容 (方法是將其複製到暫存表面)、發行需要的清除命令，然後再將內容複製回去。 雖然這可完成為遞增轉譯保存表面內容的工作，但缺點是表面的後續轉譯效能可能效能降低，因為轉譯最佳化可能遺失。

如果磚同時對應至多個串流資源，且透過其中一個串流資源以任何方式 (轉譯、複製等等) 操作磚內容，則如果同一個磚即將透過任何其他串流資源進行轉譯，該磚就必須如上文所述進行清除。

## <a name="span-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanrendering-to-tiles-shared-outside-render-area"></a><span id="Rendering_to_tiles_shared_outside_render_area"></span><span id="rendering_to_tiles_shared_outside_render_area"></span><span id="RENDERING_TO_TILES_SHARED_OUTSIDE_RENDER_AREA"></span>轉譯至在轉譯區域以外共用的磚


假設串流資源的區域為轉譯目標，且轉譯區域所參考的磚集區磚也在轉譯區域外部為對應目標 (包括透過其他串流資源，同時或不同時)。 透過其他對應檢視時，轉譯至這些磚的資料並不保證會正確顯示，即使基礎記憶體配置相容也一樣。 這是因為某些硬體所使用的最佳化資料結構對可轉譯表面的個別對應而言可能為本機，但不會對記憶體位置相同的其他對應顯示。

您可以從轉譯對應複製到所有其他可能存取之記憶體相同的對應 (或者若不再需要舊的內容，即可清除該記憶體或將其他資料複製到其中) 來因應這項限制。 雖然這項因應措施看起來很冗餘，但可讓所有其他記憶體相同的對應正確了解如何存取其內容，且至少因為只有單一實體記憶體備份保持不變而節省記憶體。

同時，當您使用共用對應的不同串流資源在之間切換時 (除非僅限讀取)，您必須在切換之間於多個並列資源之間指定資料存取順序限制 (障礙)。

## <a name="span-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanrendering-to-tiles-shared-within-render-area"></a><span id="Rendering_to_tiles_shared_within_render_area"></span><span id="rendering_to_tiles_shared_within_render_area"></span><span id="RENDERING_TO_TILES_SHARED_WITHIN_RENDER_AREA"></span>轉譯至在轉譯區域以內共用的磚


如果串流資源的區域為轉譯目標，且在轉譯區域中多個磚對應至相同的磚集區位置，則轉譯結果在該類磚上不會定義。

## <a name="span-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspandata-compatibility-across-streaming-resources-sharing-tiles"></a><span id="Data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="DATA_COMPATIBILITY_ACROSS_STREAMING_RESOURCES_SHARING_TILES"></span>串流資源共用磚之間的資料相容性


假設多個串流資源對應至相同的磚集區位置，且各個資源皆用來存取相同的資料。 本案例只在以下情況才有效：避免其他關於避免硬體最佳化結構問題的規則、已指定適當障礙 (在多個並行資源之間指定資料存取順序限制)，且串流資源彼此相容。

後者於此說明，內容關於串流資源共用磚不相容的意義。 存取重複磚對應之間相同資料的不相容情況是使用不同的表面維度或格式，或是資源上轉譯目標或深度樣板繫結旗標呈現的差異。 若您後續透過來自不相容資源的對應進行讀取或轉譯，則使用一種對應寫入記憶體會產生未定義的結果。

若另一個資源共用對應首先使用新資料初始化 (回收記憶體作為其他用途)，則後續讀取或轉譯作業會運作正常，因為資料不會在不相容的解譯間洩出。 不過，當您像這樣在存取不相容的對應間切換時，您必須指定障礙 (在多個並排資源間指定資料存取順序限制)。

如果在任何資源共用對應上彼此都未設定轉譯目標或深度樣板繫結旗標，則限制大為降低。 只要格式及表面類型 (例如 Texture2D) 相同，即可共用磚。 不同的格式彼此相容的案例，例如 BC\* 表面和大小相當的解壓縮 32 位元或每個元件格式 16 位元，像是 BC6H 及 R32G32B32A32。 許多每個項目 32 位元的格式也可將 R32\_\* 作為別名 (R10G10B10A2\_\*、R8G8B8A8\_\*、B8G8R8A8\_\*、B8G8R8X8\_\*、R16G16\_\*)；非串流資源一律允許這項作業。

如果格式相容且磚會填入純色，則壓縮與非壓縮磚之間的共用會運作良好。

最後，如果資源共用磚對應之間唯一的共同點是其皆不具轉譯目標或深度樣板繫結旗標，則僅可安全地共用填入 0 的記憶體；對應會顯示為任何 0 針對給定資源格式定義的解碼目標 (通常僅為 0)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流資源的存取管線](pipeline-access-to-streaming-resources.md)

 

 




