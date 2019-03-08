---
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: 本文列出適用於 UWP app 的相機功能，以及示範如何使用它們的操作說明文章的連結。
title: 相機
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e190a6d5134cc1fba4ac8be970bb8d90847700e4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594053"
---
# <a name="camera"></a>相機

本節提供建立通用 Windows 平台 (UWP) app 的指導方針，這類 app 會使用相機或麥克風來擷取相片、視訊或音訊。

## <a name="use-the-windows-built-in-camera-ui"></a>使用 Windows 內建的相機 UI

| 主題 | 描述 |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [使用 Windows 內建相機 UI 擷取相片和視訊](capture-photos-and-video-with-cameracaptureui.md) | 示範如何使用 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI) 類別，透過 Windows 內建的相機 UI 來擷取相片或視訊。 如果您只想讓使用者能夠擷取相片或影片，並將結果傳回給您的 app，這會是最快速且最簡單的方法。  |

## <a name="basic-mediacapture-tasks"></a>基本的 MediaCapture 工作

| 主題 | 描述 |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [顯示相機預覽](simple-camera-preview-access.md) | 示範如何在通用 Windows 平台 (UWP) app 中的 XAML 頁面上快速顯示相機預覽資料流。 |
| [MediaCapture 擷取基本的相片、 視訊和音訊](basic-photo-video-and-audio-capture-with-MediaCapture.md) | 示範使用 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 類別來擷取相片和視訊的最簡單方式。 **MediaCapture** 類別會公開一組健全的 API，可提供擷取管線的低階控制權，並啟用進階擷取案例，但本文的目的是協助您快速且輕鬆地新增對 app 的基本媒體擷取。 |
| [行動裝置的相機 UI 功能](camera-ui-features-for-mobile-devices.md) | 示範如何利用只會出現在行動裝置上的特殊相機 UI 功能。  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>進階的 MediaCapture 工作   
                                                                                                               
| 主題                                                                                             | 描述                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [處理裝置和螢幕方向與 MediaCapture](handle-device-orientation-with-mediacapture.md) | 示範如何在使用協助程式類別擷取相片和視訊時處理裝置方向。 | 
| [探索並選取相機功能與觀景窗設定檔](camera-profiles.md) | 示範如何使用相機設定檔來探索和管理不同視訊擷取裝置的功能。 這其中包括如下的工作：選取支援特定解析度或畫面播放速率的設定檔、選取支援可同時存取多台相機的設定檔，以及選取支援 HDR 的設定檔。 |
| [設定的格式、 解析度和 MediaCapture 的畫面播放速率](set-media-encoding-properties.md) | 示範如何使用 [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) 介面，來設定相機預覽資料流及所擷取相片和視訊的解析度和畫面播放速率。 它也會說明如何確保預覽資料流的外觀比例符合所擷取的媒體。 |
| [HDR 和低亮度拍攝相片](high-dynamic-range-hdr-photo-capture.md) | 示範如何使用 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture) 類別，來擷取高動態範圍 (HDR) 相片和弱光相片。 |
| [相片和視訊擷取手動相機控制項](capture-device-controls-for-photo-and-video-capture.md) | 示範如何使用手動裝置控制項來啟用美化的相片和視訊擷取案例，包括光學防手震和平滑變焦。 |
| [手動相機控制項的視訊擷取](capture-device-controls-for-video-capture.md) | 示範如何使用手動裝置控制項來啟用美化的視訊擷取案例，包括 HDR 視訊和曝光優先順序。  |
| [視訊擷取視訊穩定功能效果](effects-for-video-capture.md) | 示範如何使用影像防震效果。  |
| [MediaCapture 的場景分析](scene-analysis-for-media-capture.md) | 示範如何使用 [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.SceneAnalysisEffect) 和 [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.FaceDetectionEffect) 來分析媒體擷取預覽資料流的內容。  |
| [擷取與 VariablePhotoSequence 相片順序](variable-photo-sequence.md) | 示範如何擷取可變相片序列，讓您以快速連續的方式擷取多個影像畫面，並針對每個畫面使用不同焦點、閃光燈、ISO、曝光度及曝光補償設定進行設定。  |
| [處理媒體與 MediaFrameReader 的畫面格](process-media-frames-with-mediaframereader.md) | 示範如何使用 [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) 搭配 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture)，從一或多個可用來源取得媒體畫面。來源包括色彩、深度及紅外線相機、音訊裝置，甚至是自訂畫面來源 (例如，能產生骨骼追蹤畫面的來源)。 此功能是針對要讓執行媒體畫面即時處理的 App 使用所設計，例如虛擬實境及深度感知相機 App。  |
| [取得預覽框架](get-a-preview-frame.md) | 示範如何從媒體擷取預覽資料流取得單一預覽畫面。  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>相機的 UWP app 範例

* [相機臉部偵測範例](https://go.microsoft.com/fwlink/p/?LinkID=619486&clcid=0x409)
* [相機預覽畫面範例](https://go.microsoft.com/fwlink/p/?LinkID=620516&clcid=0x409)
* [相機 HDR 範例](https://go.microsoft.com/fwlink/p/?LinkID=620517&clcid=0x409)
* [相機手動控制項範例](https://go.microsoft.com/fwlink/p/?LinkID=627611&clcid=0x409)
* [觀景窗設定檔範例](https://go.microsoft.com/fwlink/p/?LinkID=620518&clcid=0x409)
* [相機解析範例](https://go.microsoft.com/fwlink/p/?LinkID=624252&clcid=0x409)
* [相機工具之入門套件](https://go.microsoft.com/fwlink/p/?LinkID=619479&clcid=0x409)
* [網路攝影機視訊穩定功能範例](https://go.microsoft.com/fwlink/p/?LinkID=620519&clcid=0x409)

## <a name="related-topics"></a>相關主題

* [音訊、視訊和相機](index.md)
 

 




