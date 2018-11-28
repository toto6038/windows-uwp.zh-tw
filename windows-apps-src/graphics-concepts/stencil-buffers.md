---
title: 樣板緩衝區
description: 樣板緩衝區用於影像中遮罩像素，製作特殊效果。
ms.assetid: 544B3B9E-31E3-41DA-8081-CC3477447E94
keywords:
- 樣板緩衝區
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 285e4a70062c57c957530aa1e548c22c4cf7711e
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7839856"
---
# <a name="stencil-buffers"></a>樣板緩衝區


*樣板緩衝區*用於影像中遮罩像素，製作特殊效果。 遮罩控制是否繪製像素。 這些特殊效果包含組合、印花、溶解、淡化、滑動、外框及剪影，以及雙面樣板。 一些比較常見的效果如下所示。

樣板緩衝區啟用或停用依據個別像素的轉譯目標表面繪圖。 在最基本層級，它可以讓應用程式遮罩轉譯影像的區段，讓它們不會顯示。 應用程式通常會將樣板緩衝區用於特殊效果，例如溶解、印花，以及外框。

樣板緩衝區資訊內嵌於 z 緩衝資料。

## <a name="span-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanhow-the-stencil-buffer-works"></a><span id="How_the_Stencil_Buffer_Works"></span><span id="how_the_stencil_buffer_works"></span><span id="HOW_THE_STENCIL_BUFFER_WORKS"></span>樣板緩衝區的運作方式


Direct3D 依據個別像素在樣板緩衝區的內容執行測試。 對於目標表面的每一個像素，它會使用樣板緩衝區中的對應值、樣板參考值，以及樣板遮罩值來執行測試。 如果通過測試，Direct3D 執行動作。 使用下列步驟執行測試。

1.  執行樣板參考值與樣板遮罩的位元 AND 運算。
2.  執行目前像素之樣板緩衝區值與樣板遮罩的位元 AND 運算。
3.  使用比較函式，比較步驟 1 的結果與步驟 2 的結果。

在下列一行的程式碼顯示上述步驟：

```
(StencilRef & StencilMask) CompFunc (StencilBufferValue & StencilMask)
```

-   StencilRef 代表樣板參考值。
-   StencilMask 代表樣板遮罩的值。
-   CompFunc 是比較函式。
-   StencilBufferValue 是目前像素的樣板緩衝區內容。
-   & 符號表示位元 AND 運算。

如果樣板測試通過，目前像素會寫入目標表面，否則會略過。 預設比較行為是寫入像素，不論如何每一位元運算。您可以藉由變更列舉來識別所需的比較函式類型的值來變更此行為。

您的應用程式可自訂樣板緩衝區的作業。 它可以設定比較函式、樣板遮罩和樣板參考值。 它也可以控制樣板測試通過或失敗時，Direct3D 採取的動作。

## <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>組合


您的應用程式可利用樣板緩衝區將 2D 或 3D 影像組合為 3D 場景。 樣板緩衝區中的遮罩可用於遮蔽轉譯目標的部分表面區域。 儲存的 2D 資訊 (例如文字或點陣圖) 接著便可寫入遮蔽區域。 或者，您的應用程式也可將其他 3D 原始物件轉譯至轉譯目標表面的樣板遮蔽區域。 甚至可以轉譯整個場景。

通常，遊戲會將多個 3D 場景合成在一起。 例如：競速遊戲通常都會顯示後照鏡。 後照鏡便包含了駕駛後方的 3D 場景。 該檢視基本上就是合成於駕駛前方檢視的第二個 3D 場景。

## <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>印花


Direct3D 應用程式使用印花來控制從特定原始影像繪製到轉譯目標表面上的像素。 應用程式將印花套用至原始物件的影像，以正確地轉譯共面多邊形。

例如：將輪胎標記及黃線畫上道路之後，標記應該要直接出現在道路上方。 但是，標記和道路的 z 值是相同的。 因此，深度緩衝區不一定會在這兩者之間正常分離。 某些後方原始物件的像素可能會轉譯至前方原始物件的上方，反之亦然。 產生的影像似乎在影格間不斷閃爍。 此效果稱為「*z 衝突*」或「*影格閃爍*」。

若要解決這個問題，可使用樣板將後方原始物件上印花出現的區域遮蔽。 關閉 z 緩衝並將前方基本類型的影像轉譯至轉譯目標表面上已被遮蔽的區域。

雖然您可使用多重紋理混合解決這個問題，如此一來會限制應用程式可產生的其他特殊效果數目。 使用樣板緩衝區套用印花會釋出紋理混合階段供其他效果使用。

## <a name="span-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspandissolves-fades-and-swipes"></a><span id="Dissolves__fades__and_swipes"></span><span id="dissolves__fades__and_swipes"></span><span id="DISSOLVES__FADES__AND_SWIPES"></span>溶解、淡化及滑動


越來越多的應用程式使用電影和影片中常見的特殊效果，例如溶解、滑動和並淡化。

在溶解，某個影像會在一系列平滑的畫面中逐漸被另一個取代。 雖然 Direct3D 提供方法來使用多重紋理混合達到相同效果，但是使用樣板緩衝區執行溶解的應用程式在溶解時，可以將紋理混合功能用於其他效果。

當您的應用程式執行溶解時，必須呈現兩個不同的影像。 它會使用樣板緩衝區控制每個影像的哪些像素要繪製到轉譯目標表面。 您可以定義一系列的樣板遮罩，並將它們複製到連續畫面上的樣板緩衝區。 或者，您可以定義第一個畫面的基底樣板遮罩，並累加變更它。

在溶解開頭，應用程式設定樣板函式和樣板遮罩，使開始影像的大部分像素都通過樣板測試。 結束影像的大部分像素應該未通過樣板測試。 在後續畫面，樣板遮罩會更新，以便開始影像的更少像素通過測試。 隨著畫面進展，結束影像的更少像素未通過測試。 如此一來，您的應用程式可以使用任何任意溶解模式執行溶解。

淡入或淡出是溶解的特殊案例。 當淡入時，樣板緩衝區用來從黑或白的影像溶解到 3D 場景轉譯。 淡出則相反，應用程式從 3D 場景轉譯開始，然後溶解為黑或白。 您可以使用任何任意模式執行淡化。

Direct3D 應用程式對滑動使用類似技術。 例如，當應用程式執行由左向右滑動時，結束影像看起來在開始影像上逐漸從左向右滑動。 如同溶解，您必須定義一系列的樣板遮罩，並將它們載入到連續畫面上的樣板緩衝區，或者連續修改開始樣板遮罩。 樣板遮罩可用來停用開始影像的像素寫入，以及啟用結束影像的像素寫入。

滑動比溶解有點更複雜，因為應用程式必須以滑動反向順序從結束影像讀取像素。 也就是，如果滑動是從左向右移動，您的應用程式必須由右至左從結束影像讀取像素。

## <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanoutlines-and-silhouettes"></a><span id="Outlines_and_silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span>外框及剪影


您可以利用樣板緩衝區以獲得更抽象的效果，例如：外框及剪影。

如果應用程式將樣板遮罩套用至形狀相同但略小的基本類型影像，結果影像只包含基本類型影像的外框。 應用程式可以再以單色填滿影像中被樣板遮蔽的區域，給予基本類型一種浮凸的外觀。

如果樣板遮罩的大小及形狀與您正在轉譯的原始物件相同，產出的影像將會在原先原始物件所在的位置留下一個洞。 您的應用程式可以再以黑色填滿該空洞，產生基本類型的剪影效果。

## <a name="span-idtwo-sidedstencilspanspan-idtwo-sidedstencilspanspan-idtwo-sidedstencilspantwo-sided-stencil"></a><span id="Two-sided_stencil"></span><span id="two-sided_stencil"></span><span id="TWO-SIDED_STENCIL"></span>雙面樣板


利用樣板緩衝區繪製陰影時將會使用到陰影錐。 應用程式透過計算剪影的邊緣，將其往光源的反方向突出並形成一組 3D 體積的集合，進而計算遮蔽幾何物的陰影錐。 這些物體接著會在經過兩次的轉譯之後寫入樣板緩衝區。

第一次轉譯會繪製面向前端的多邊形，並遞增樣板緩衝區的值。 第二次轉譯會繪製陰影錐面向後端的多邊形，並遞減樣板緩衝區的值。 一般而言，所有遞增及遞減值有取消彼此。不過，導致某些像素，以測試失敗的 z 緩衝區陰影磁碟區會轉譯為一般幾何之前已經轉譯場景。 留在樣板緩衝區的值將會對應到陰影中的像素。 這些剩餘的樣板緩衝區內容會作為遮罩使用，以將一個包括所有內容的巨大黑色四元組 Alpha 混合至場景中。 藉由將樣板緩衝區作為遮罩，陰影中的像素將會進一步加深。

這表示針對每個光源，陰影幾何都會繪製兩次，因此對 GPU 的頂點輸送量形成了壓力。 雙面樣板便是為了避免這種情況而設計出來的功能。 在這種方法之下，會有兩組樣板狀態 (命名如下)：其中一組為針對每個面向前端的三角形設定的樣板狀態，另外一組則是針對面向後端的三角形。 如此一來，針對每個光源，每個陰影錐都只會進行一階段的繪製。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[深度和樣板緩衝區](depth-and-stencil-buffers.md)

 

 




