---
author: drewbatgit
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: "本文示範如何使用手動裝置控制項來啟用美化的視訊擷取案例，包括 HDR 視訊和曝光優先順序。"
title: "視訊擷取的手動相機控制項"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: cd2adcffa233b76563e47f93f298cf954154adef
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manual-camera-controls-for-video-capture"></a>視訊擷取的手動相機控制項

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文示範如何使用手動裝置控制項來啟用美化的視訊擷取案例，包括 HDR 視訊和曝光優先順序。

本文中討論的視訊裝置控制項全都會使用相同的模式新增到您的 app。 首先，檢查 app 目前執行所在的裝置是否支援此控制項。 如果支援此控制項，則為控制項設定所需的模式。 通常，如果目前的裝置不支援特定的控制項，您應該停用或隱藏可讓使用者啟用該功能的 UI 元素。

此文章中討論的所有裝置控制項 API 都是 [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902) 命名空間的成員。

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

> [!NOTE] 
> 本文是以[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)中討論的概念和程式碼為基礎，其中說明實作基本相片和視訊擷取的步驟。 我們建議您先熟悉該文章中的基本媒體擷取模式，然後再移到更多進階的擷取案例。 本文章中的程式碼假設您的 app 已有正確初始化的 MediaCapture 執行個體。

## <a name="hdr-video"></a>HDR 視訊

高動態範圍 (HDR) 視訊功能可將 HDR 處理套用至擷取裝置的視訊資料流。 選取 [**HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682) 屬性，以判斷是否支援 HDR 視訊。

HDR 視訊控制項支援開啟、關閉和自動三種模式，這表示裝置會動態判斷 HDR 視訊處理是否可改進媒體擷取，若是可以，則會啟用 HDR 視訊。 若要判斷目前的裝置是否支援特定的模式，請查看 [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683) 集合是否包含所需的模式。

將 [**HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681) 設定為所需的模式，以啟用或停用 HDR 視訊處理。

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## <a name="exposure-priority"></a>曝光優先順序

啟用 [**ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644) 後，會評估來自擷取裝置的視訊框架，以判斷視訊是否擷取光線不足的場景。 若是如此，此控制項會降低所擷取視訊的框架速度，以增加每個框架的曝光時間並改善所擷取視訊的視覺品質。

檢查 [**ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647) 屬性，以判斷目前的裝置是否支援曝光優先順序控制項。

將 [**ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646) 設定為所需的模式，以啟用或停用曝光優先順序控制項。

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




