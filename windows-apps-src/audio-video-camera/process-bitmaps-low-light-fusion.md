---
description: 本文說明如何使用 LowLightFusion 類別來處理點陣圖。
title: 使用 Low Light Fusion API 處理點陣圖
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 弱光融合, 點陣圖, 影像處理
ms.localizationpriority: medium
ms.openlocfilehash: 6c1ae98b12d9ddb83f5109212d91ae2aa804e32a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163642"
---
# <a name="process-bitmaps-with-the-lowlightfusion-api"></a>使用 LowLightFusion API 處理點陣圖

弱光影像很難以良好的影像品質擷取，尤其是使用固定光圈和感應器大小的行動裝置。 為了補償弱光，裝置可能會增加曝光時間或感應器增益，但這又可能導致動作模糊並增加影像中的雜訊。 

[LowLightFusion 類別](/uwp/api/windows.media.core.lowlightfusion)可從多個拍攝時間相近的畫面 (亦即短連拍影像) 取樣像素資訊來減少雜訊和動作模糊，以改善弱光影像的品質。 這個好用的功能通常會加入相片編輯應用程式之類的項目中。

此功能也可透過 [AdvancedPhotoCapture 類別](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture)取得，這會在擷取映像後，視需要直接將弱光融合演算法套用至一系列影像。 請參閱[弱光相片擷取](./high-dynamic-range-hdr-photo-capture.md#low-light-photo-capture)以了解如何執行這項功能。

## <a name="prepare-the-images-for-processing"></a>準備影像以進行處理

在此範例中，我們將示範如何使用 [LowLightFusion 類別](/uwp/api/windows.media.core.lowlightfusion)以及 [FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 來允許使用者選取多個影像以便對其執行弱光融合。

首先，我們需要判斷演算法可接受多少影像 (也稱為畫面)，並建立保有這些畫面的清單。

[!code-cs[SnippetGetMaxLLFFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetMaxLLFFrames)]

確定弱光融合演算法可接受多少畫面後，我們可以使用 [FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 來允許使用者選擇演算法中應該使用哪些影像。

[!code-cs[SnippetGetFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetFrames)]

現在我們已經選取正確的畫面數，我們需要將畫面解碼至 [SoftwareBitmaps](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 並確保 SoftwareBitmaps 針對 LowLightFusion 使用正確格式。

[!code-cs[SnippetDecodeFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetDecodeFrames)]


## <a name="fuse-the-bitmaps-into-a-single-bitmap"></a>將點陣圖合成為單一點陣圖

現在，我們擁有可接受格式的正確畫面數，可以使用 **[FuseAsync](/uwp/api/windows.media.core.lowlightfusion.fuseasync)** 方法來套用弱光融合演算法。 我們的結果會是 SoftwareBitmap 形式的處理過影像，並已改善清晰度。 

[!code-cs[SnippetFuseFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetFuseFrames)]

最後，我們要清理產生的 SoftwareBitmap，將其編碼並儲存為方便使用的「一般」影像，類似於我們一開始使用的輸入影像。

[!code-cs[SnippetEncodeFrame](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetEncodeFrame)]


## <a name="before-and-after"></a>處理前和處理後

以下範例是一個輸入影像以及套用弱光融合演算法之後的產生輸出影像。

> [!div class="mx-tableFixed"] 
| 輸入畫面 | 弱光融合輸出 | 
|-------------|-------------------------|
| ![輸入到弱光融合演算法的畫面](./images/LLF-Input.png) | ![弱光融合演算法的結果畫面](./images/LLF-Output.png) |

您可以從輸畫面和輸出畫面看出，橫幅周圍陰影的光源和清晰度已改善。

## <a name="related-topics"></a>相關主題 
[LowLightFusion 類別](/uwp/api/windows.media.core.lowlightfusion)  
[LowLightFusionResult 類別](/uwp/api/windows.media.core.lowlightfusionresult)