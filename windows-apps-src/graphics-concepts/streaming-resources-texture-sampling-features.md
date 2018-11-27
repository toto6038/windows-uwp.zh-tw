---
title: 串流資源紋理取樣功能
description: 串流資源紋理取樣功能包含取得有關對應區域的著色器狀態回饋、檢查存取的所有資料是否都在資源中對應、鉗制以協助著色器避免 mipmap 串流資源中已知為非對應的區域，以及探索整個紋理篩選足跡的完全對應最低 LOD。
ms.assetid: C2B2DD69-8354-417A-894D-6235A8B48B53
keywords:
- 串流資源紋理取樣功能
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8b6290fba9d4194df78c39902b8d96e952134682
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7718396"
---
# <a name="streaming-resources-texture-sampling-features"></a>串流資源紋理取樣功能


串流資源紋理取樣功能包含取得有關對應區域的著色器狀態回饋、檢查存取的所有資料是否都在資源中對應、鉗制以協助著色器避免 mipmap 串流資源中已知為非對應的區域，以及探索整個紋理篩選足跡的完全對應最低 LOD。

## <a name="span-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanrequirements-of-streaming-resources-texture-sampling-features"></a><span id="Requirements_of_streaming_resources_texture_sampling_features"></span><span id="requirements_of_streaming_resources_texture_sampling_features"></span><span id="REQUIREMENTS_OF_STREAMING_RESOURCES_TEXTURE_SAMPLING_FEATURES"></span>串流資源紋理取樣功能的需求


這裡描述的紋理取樣功能需要[第 2 層](tier-2.md)串流資源支援。

## <a name="span-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanshader-status-feedback-about-mapped-areas"></a><span id="Shader_status_feedback_about_mapped_areas"></span><span id="shader_status_feedback_about_mapped_areas"></span><span id="SHADER_STATUS_FEEDBACK_ABOUT_MAPPED_AREAS"></span>有關對應區域的著色器狀態回饋


讀取和/或寫入串流資源的任何著色器指令，會導致狀態被記錄。 這個狀態公開為進入 32 位元暫時暫存器之每個資源存取指令的選擇性額外傳回值。 傳回值的內容是不透明。 也就是，不允許著色器程式直接讀取。 但是，您可以使用[**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) 函式來擷取狀態資訊。

## <a name="span-idfullymappedcheckspanspan-idfullymappedcheckspanspan-idfullymappedcheckspanfully-mapped-check"></a><span id="Fully_mapped_check"></span><span id="fully_mapped_check"></span><span id="FULLY_MAPPED_CHECK"></span>完全對應檢查


[**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) 函式解譯從記憶體存取傳回的狀態，並指出存取的所有資料是否都在資源中對應。 如果資料已對應，**CheckAccessFullyMapped** 傳回 true (0xFFFFFFFF)，如果資料未對應則傳回 false (0x00000000)。

在篩選作業時，有時候特定紋素的加權會變成 0.0。 一個範例是紋理座標直接落在紋素中心的線性樣本：3 個其他紋素 (有哪些可能因硬體而不同) 貢獻到篩選，但具有 0 加權。 這些 0 加權紋素完全不會貢獻到篩選結果，如果它們恰巧落在 **NULL** 磚，它們不算是未對應的存取。 請注意，相同的保證適用於包括多個 mip 層級的紋理篩選；如果其中一個 mipmap 的紋素未對應，但在那些紋素的加權是 0，那些紋素不算是未對應的存取。

當您從少於 4 個元件 (例如 DXGI\_FORMAT\_R8\_UNORM) 的格式取樣，落在 **NULL** 磚的任何紋素都會導致 **NULL** 對應存取被回報，無論著色器實際上查看結果中的哪些元件。 例如，從 R8\_UNORM 讀取和以 .gba/.yzw 遮罩著色器讀取結果，似乎完全不需要讀取紋理。 但如果紋素位址是 **NULL** 對應磚，作業仍然算是 **NULL** 對應存取。

著色器可以檢查狀態，並在失敗時追求任何您想要的動作過程。 例如，動作過程可以是記錄「遺漏」(例如透過 UAV 寫入) 和/或發出另一個讀取 (鉗制到已知對應的更粗略 LOD)。 應用程式可能也要追蹤成功存取，以了解對應磚集的哪個部分被存取。

記錄的一個複雜問題是沒有機制存在回報一組確切的磚被存取。 應用程式可做保守猜測，根據了解它用來存取的座標，以及使用 LOD 指令。例如，[**tex2Dlod**](https://msdn.microsoft.com/library/windows/desktop/bb509680) 傳回硬體 LOD 計算。

另一個問題是，許多存取會到同一個磚，所以會發生許多重複記錄和可能記憶體競爭。 如果硬體有選項，在之前其他地方有報告時，不需要為了報告磚存取而擔心，則可能很便利。 或許這類追蹤的狀態可能從 API 重設 (可能在畫面界限)。

## <a name="span-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanper-sample-minlod-clamp"></a><span id="Per-sample_MinLOD_clamp"></span><span id="per-sample_minlod_clamp"></span><span id="PER-SAMPLE_MINLOD_CLAMP"></span>每個樣本 MinLOD 鉗制


為了協助著色器避免 mipmap 串流資源中已知為非對應的區域，大部分包含使用取樣器 (篩選) 的著色器指令都有允許著色器傳遞其他 float32 MinLOD clamp 參數至紋理樣本的模式。 這個值位於檢視的 mipmap 數字空間，而不是基礎資源。

硬體在 LOD 計算的同一個位置，也就是發生每個資源 MinLOD 鉗制的位置，執行` max(fShaderMinLODClamp,fComputedLOD) `，這也是 [**max**](https://msdn.microsoft.com/library/windows/desktop/bb509624)()。

如果套用每個樣本 LOD 鉗制和定義於取樣器之任何其他 LOD 鉗制的結果是空集合，結果是和每個資源 minLOD 鉗制相同的超出範圍存取結果：0 表示表面格式中的元件，預設值表示遺失元件。

LOD 指令 (例如，[**tex2Dlod**](https://msdn.microsoft.com/library/windows/desktop/bb509680)) (其日期早於這裡所述的每個樣本 minLOD 鉗制) 會傳回鉗制和未鉗制的 LOD。 從這個 LOD 指令傳回的鉗制 LOD 反映所有鉗制，包括每個資源鉗制，但不是每個樣本鉗制。 每個樣本鉗制是由著色器控制和所知，因此著色器作者如有需要可以手動將該鉗制套用至 LOD 指令的傳回值。

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>最小/最大減少篩選


應用程式可以選擇管理自己的資料結構，通知資料結構串流資源的對應外觀。 一個範例是包含紋素以儲存串流資源每個磚資訊的表面。 有人可能會儲存在特定磚位置對應的第一個 LOD。 以類似於串流資源取樣的方式，仔細取樣此資料結構，我們可能會發現整個紋理篩選足跡的完全對應最低 LOD。 為了讓這個程序更容易，Direct3D 11.2 引進新的通用取樣器模式，最小/最大篩選。

LOD 追蹤的最小/最大篩選公用程式可能對其他用途有用，例如，也許是深度表面篩選。

最小/最大減少篩選是取樣器上的模式，會擷取一般紋理篩選擷取的一組相同的紋素。 但是，它會依據每個元素傳回所擷取紋素的 min() 或 max() (例如，所有 R 值的最小值，與所有 G 值的最小值分開，以此類推)，而不是混合值以產生答案。

最小/最大作業遵循 Direct3D 算術精確度規則。 比較的順序並不重要。

在不是最小/最大的篩選作業時，有時候特定紋素的加權會變成 0.0。 一個範例是紋理座標直接落在紋素中心的線性樣本：在此情況下，3 個其他紋素 (有哪些可能因硬體而不同) 貢獻到篩選，但具有 0 加權。 對於在非最小/最大篩選上 0 加權的任何紋素，如果篩選是最小/最大，這些紋素仍然不會貢獻到結果 (而且加權不會影響最小/最大篩選作業)。

此功能的支援取決於[第 2 層](tier-2.md)串流資源的支援。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[串流資源的存取管線](pipeline-access-to-streaming-resources.md)

 

 




