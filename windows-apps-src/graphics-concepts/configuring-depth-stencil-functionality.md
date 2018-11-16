---
title: 設定深度樣板功能
description: 本章節涵蓋的步驟，內容為設定深度樣板緩衝區及輸出合併階段的深度樣板狀態。
ms.assetid: B3F6CDAA-93ED-4DC1-8E69-972C557C7920
keywords:
- 設定深度樣板功能
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3c15ff6aa43540b61b410525e6bb20a0de3da821
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6984612"
---
# <a name="span-iddirect3dconceptsconfiguringdepth-stencilfunctionalityspanconfiguring-depth-stencil-functionality"></a><span id="direct3dconcepts.configuring_depth-stencil_functionality"></span>設定深度樣板功能


本章節涵蓋的步驟，內容為設定深度樣板緩衝區及輸出合併階段的深度樣板狀態。

待您了解如何使用深度樣板緩衝區及其對應的深度樣板狀態之後，請參閱[進階樣板技術](#advanced-stencil-techniques)。

## <a name="span-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespancreate-depth-stencil-state"></a><span id="Create_Depth_Stencil_State"></span><span id="create_depth_stencil_state"></span><span id="CREATE_DEPTH_STENCIL_STATE"></span>建立深度樣板狀態


深度樣板狀態會告訴輸出合併階段如何執行[深度樣板測試](https://msdn.microsoft.com/library/windows/desktop/bb205120)。 深度樣板測試決定是否要繪製給定的像素。

## <a name="span-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanbind-depth-stencil-data-to-the-om-stage"></a><span id="Bind_Depth_Stencil_to_the_OM_Stage"></span><span id="bind_depth_stencil_to_the_om_stage"></span><span id="BIND_DEPTH_STENCIL_TO_THE_OM_STAGE"></span>將深度樣板資料繫結至輸出合併階段


繫結深度樣板狀態。

使用檢視繫結深度樣板資源。

轉譯目標必須全部都是相同的資源類型。 若要使用多重採樣消除鋸齒，所有已繫結的轉譯目標及深度緩衝區都必須要有相同的樣本數量。

當緩衝區作為轉譯目標使用時，則不支援深度樣板測試和多重轉譯目標。

-   最多可以同時繫結 8 個轉譯目標。
-   所有轉譯目標在所有的維度上都必須要有相同的大小。(3D 的寬度、高度及深度，或是 \*Array 類型的陣列大小。)
-   各個轉譯目標可能會有不同的資料格式。
-   寫入遮罩可用來控制將寫入轉譯目標的資料。 輸出寫入遮罩可根據每個轉譯目標及每個元件，控制將寫入轉譯目標的資料。

## <a name="span-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvanced-stencil-techniquesspanadvanced-stencil-techniques"></a><span id="Advanced_Stencil_Techniques"></span><span id="advanced_stencil_techniques"></span><span id="ADVANCED_STENCIL_TECHNIQUES"></span><span id="advanced-stencil-techniques"></span>進階樣板技術


深度樣板緩衝區的樣板部分可用於建立轉譯效果，例如：合成、印花，以及外框。

-   [合成](#compositing)
-   [印花](#decaling)
-   [外框及剪影](#outlines-and-silhouettes)
-   [雙面樣板](#two-sided-stencil)
-   [讀取深度樣板緩衝區作為紋理](#reading-the-depth-stencil-buffer-as-a-texture)

### <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>合成

您的應用程式可利用樣板緩衝區將 2D 或 3D 影像組合為 3D 場景。 樣板緩衝區中的遮罩可用於遮蔽轉譯目標的部分表面區域。 儲存的 2D 資訊 (例如文字或點陣圖) 接著便可寫入遮蔽區域。 或者，您的應用程式也可將其他 3D 原始物件轉譯至轉譯目標表面的樣板遮蔽區域。 甚至可以轉譯整個場景。

通常，遊戲會將多個 3D 場景合成在一起。 例如：競速遊戲通常都會顯示後照鏡。 後照鏡便包含了駕駛後方的 3D 場景。 該檢視基本上就是合成於駕駛前方檢視的第二個 3D 場景。

### <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>印花

Direct3D 應用程式使用印花來控制從特定原始影像繪製到轉譯目標表面上的像素。 應用程式將印花套用至原始物件的影像，以正確地轉譯共面多邊形。

例如：將輪胎標記及黃線畫上道路之後，標記應該要直接出現在道路上方。 但是，標記和道路的 z 值是相同的。 因此，深度緩衝區不一定會在這兩者之間正常分離。 某些後方原始物件的像素可能會轉譯至前方原始物件的上方，反之亦然。 產生的影像似乎在影格間不斷閃爍。 此效果稱為「z 衝突」或「影格閃爍」。

若要解決這個問題，可使用樣板將後方原始物件上印花出現的區域遮蔽。 關閉 z 緩衝，並將前方原始物件的影像轉譯至轉譯目標表面上已被遮蔽的區域。

多重紋理混合可用於解決此問題。

### <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlines-and-silhouettesoutlines-and-silhouettes"></a><span id="Outlines_and_Silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span><span id="outlines-and-silhouettes">外框及剪影

您可以利用樣板緩衝區以獲得更抽象的效果，例如：外框及剪影。

如果您的應用程式進行了二階段的轉譯 (第一階段產生樣板遮罩，並於第二階段在原始物件稍微變小之後，將樣板遮罩套用至影像)，產生的影像可能只會包含原始物件的外框。 應用程式可以再以單色填滿影像中被樣板遮蔽的區域，給予原始物件浮凸的外觀。

如果樣板遮罩的大小及形狀與您正在轉譯的原始物件相同，產出的影像將會在原先原始物件所在的位置留下一個洞。 您的應用程式可以再以黑色填滿該空洞，產生基本類型的剪影效果。

### <a name="span-idtwosidedstencilspanspan-idtwosidedstencilspanspan-idtwosidedstencilspantwo-sided-stencil"></a><span id="Two_Sided_Stencil"></span><span id="two_sided_stencil"></span><span id="TWO_SIDED_STENCIL"></span>雙面樣板

利用樣板緩衝區繪製陰影時將會使用到陰影錐。 應用程式透過計算剪影的邊緣，將其往光源的反方向突出並形成一組 3D 體積的集合，進而計算遮蔽幾何物的陰影錐。 這些物體接著會在經過兩次的轉譯之後寫入樣板緩衝區。

第一次轉譯會繪製面向前端的多邊形，並遞增樣板緩衝區的值。 第二次轉譯會繪製陰影錐面向後端的多邊形，並遞減樣板緩衝區的值。

一般而言，所有遞增及遞減值有取消彼此。不過，導致某些像素，以測試失敗的 z 緩衝區陰影磁碟區會轉譯為一般幾何之前已經轉譯場景。 留在樣板緩衝區的值將會對應到陰影中的像素。 這些剩餘的樣板緩衝區內容會作為遮罩使用，以將一個包括所有內容的巨大黑色四元組 Alpha 混合至場景中。 藉由將樣板緩衝區作為遮罩，陰影中的像素將會進一步加深。

這表示針對每個光源，陰影幾何都會繪製兩次，因此對 GPU 的頂點輸送量形成了壓力。 雙面樣板便是為了避免這種情況而設計出來的功能。 在這種方法之下，會有兩組樣板狀態 (命名如下)：其中一組為針對每個面向前端的三角形設定的樣板狀態，另外一組則是針對面向後端的三角形。 如此一來，針對每個光源及每個陰影錐都只會進行一階段的繪製。

### <a name="span-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreading-the-depth-stencil-buffer-as-a-texturespanreading-the-depth-stencil-buffer-as-a-texture"></a><span id="Reading_the_Depth-Stencil_Buffer_as_a_Texture"></span><span id="reading_the_depth-stencil_buffer_as_a_texture"></span><span id="READING_THE_DEPTH-STENCIL_BUFFER_AS_A_TEXTURE"></span><span id="reading-the-depth-stencil-buffer-as-a-texture"></span>讀取深度樣板緩衝區作為紋理

非使用中的深度樣板緩衝區可由著色器作為紋理讀取。 讀取深度樣板緩衝區作為紋理的應用程式，將會進行二階段的轉譯：在第一階段寫入深度樣板緩衝區，並於第二階段從緩衝區中讀取。 這可以讓著色器將先前寫入緩衝區的深度值或樣板值，與目前轉譯中像素的值互相比較。 比較的結果可用來建立效果，例如陰影貼圖或粒子系統中的軟粒子。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[輸出合併 (OM) 階段](output-merger-stage--om-.md)

[圖形管線](graphics-pipeline.md)

[輸出合併階段](https://msdn.microsoft.com/library/windows/desktop/bb205120)
