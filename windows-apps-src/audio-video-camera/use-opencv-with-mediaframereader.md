---
ms.assetid: ''
description: 本文說明如何使用 Open Source Computer Vision Library (OpenCV) 搭配 MediaFrameReader 類別。
title: 使用 OpenCV 搭配 MediaFrameReader
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, openCV
ms.localizationpriority: medium
ms.openlocfilehash: f78197b6108a81f3335dc202585127105bd94339
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363720"
---
# <a name="use-the-open-source-computer-vision-library-opencv-with-mediaframereader"></a>使用 Open Source Computer Vision Library (OpenCV) 搭配 MediaFrameReader

本文說明如何使用 Open Source Computer Vision Library (OpenCV，這是一個原生程式碼程式庫，提供廣泛的影像處理演算法) 來搭配 [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) 類別，此類別可同時從多種來源讀取媒體畫面。 本文中的範例程式碼會引導您建立簡單的應用程式，從色彩感應器取得畫面、使用 OpenCV 程式庫模糊每個畫面，然後在 XAML **Image** 控制項中顯示處理過的影像。 

>[!NOTE]
>OpenCV 和 OpenCV 不會定期更新，且不會通過存放區合規性檢查，因此這些套件僅供實驗之用。

本文以其他兩篇文章的內容為基礎：

* [使用 MediaFrameReader 處理媒體畫面](process-media-frames-with-mediaframereader.md) - 這篇文章提供使用 **MediaFrameReader** 從一或多個取得媒體畫面來源取得畫面的詳細資訊，並詳細說明本文中的大部分的程式碼範例。 特別是 **〈使用 MediaFrameReader 處理媒體畫面〉** 提供 **FrameRenderer** 協助程式類別的程式碼清單，處理在 XAML **Image** 元素中的媒體畫面展示。 本文中的範例程式碼也使用這個協助程式類別。

* [使用 OpenCV 處理軟體點陣圖](process-software-bitmaps-with-opencv.md)-本文將逐步引導您建立原生程式碼 Windows 執行階段元件**OpenCVBridge**，這可協助您在**MediaFrameReader**所使用的**SoftwareBitmap**物件和 OpenCV 程式庫所使用的**材料**類型之間進行轉換。 這篇文章中的範例程式碼假設您已依照步驟新增 **OpenCVBridge** 元件到您的 UWP app 方案。

除了這些文章，若要檢視及下載本文所述案例的完整端對端工作範例，請參閱 Windows 通用範例 GitHub 存放庫中的[相機畫面 + OpenCV 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) (英文)。

若要快速開始開發，您可以使用 NuGet 套件將 OpenCV 程式庫包含在 UWP 應用程式專案中，但當您將應用程式提交至商店時，這些套件可能無法通過應用程式 certficication 流程，因此建議您先下載 OpenCV 程式庫原始程式碼並自行建立二進位檔，然後再提交您的應用程式。 使用 OpenCV 進行開發的資訊可于 [https://opencv.org](https://opencv.org)


## <a name="implement-the-opencvhelper-native-windows-runtime-component"></a>執行 OpenCVHelper 原生 Windows 執行階段元件
遵循以 [OpenCV 處理軟體點陣圖](process-software-bitmaps-with-opencv.md) 中的步驟，建立 OpenCV helper Windows 執行階段元件，並將元件專案的參考新增至 UWP 應用程式解決方案。

## <a name="find-available-frame-source-groups"></a>尋找可用的畫面來源群組
首先，您需要尋找可從哪個畫面來源群組取得媒體畫面。 藉由呼叫 **[MediaFrameSourceGroup.FindAllAsync](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)**，取得目前裝置上可用的來源群組清單。 然後選取可提供您 app 案例所需感應器類型的來源群組。 針對此範例，我們只需要能從 RGB 相機提供畫面的來源群組。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVFrameSourceGroups":::

## <a name="initialize-the-mediacapture-object"></a>初始化 MediaCapture 物件
接下來，您需要透過設定 **MediaCaptureInitializationSettings** 的 **[SourceGroup](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** 屬性來初始化 **MediaCapture** 物件，以使用上一個步驟中選取的畫面來源群組。

> [!NOTE] 
> OpenCVHelper 元件所用的技術 (於[使用 OpenCV 處理點陣圖](process-software-bitmaps-with-opencv.md)中詳細說明) 需要影像資料位於 CPU 記憶體中，而不是 GPU 記憶體中。 因此，您應該對 **MediaCaptureInitializationSettings** 的 **[MemoryPreference](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.MemoryPreference)** 欄位指定 **MemoryPreference.CPU**。

初始化 **MediaCapture** 物件後，存取 **[MediaCapture.FrameSources](/uwp/api/windows.media.capture.mediacapture.FrameSources)** 屬性以取得 RGB 框架來源的參考。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVInitMediaCapture":::

## <a name="initialize-the-mediaframereader"></a>初始化 MediaFrameReader
接下來，為上一個步驟中擷取的 RGB 畫面來源建立 [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader)。 為了維持良好的畫面播放速率，您可能會想讓所處理畫面的解析度低於感應器的解析度。 此範例提供 **[MediaCapture.CreateFrameReaderAsync](/uwp/api/windows.media.capture.mediacapture.createframereaderasync)** 方法的選用 **[BitmapSize](/uwp/api/windows.graphics.imaging.bitmapsize)** 引數，要求將畫面讀取程式提供的畫面大小調整為 640 x 480 像素。

建立畫面讀取程式之後，登錄 **[FrameArrived](/uwp/api/windows.media.capture.frames.mediaframereader.FrameArrived)** 事件的處理常式。 接著建立新的 **[SoftwareBitmapSource](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)** 物件，讓 **FrameRenderer** 協助程式類別用來呈現處理後的影像。 然後呼叫 **FrameRenderer** 的建構函式。 初始化 OpenCVBridge Windows 執行階段元件中定義之 **OpenCVHelper** 類別的實例。 此協助程式類別用於 **FrameArrived** 處理常式中，用來處理每個畫面。 最後，呼叫 **[StartAsync](/uwp/api/windows.media.capture.frames.mediaframereader.StartAsync)** 以啟動畫面讀取程式。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVFrameReader":::


## <a name="handle-the-framearrived-event"></a>處理 FrameArrived 事件
當有來自畫面讀取程式的可用新畫面時，就會引發 **FrameArrived** 事件。 呼叫 **[TryAcquireLatestFrame](/uwp/api/windows.media.capture.frames.mediaframereader.TryAcquireLatestFrame)** 以取得畫面，如果有的話。 從 **[MediaFrameReference](/uwp/api/windows.media.capture.frames.mediaframereference)** 取得 **SoftwareBitmap**。 請注意，此範例中使用的 **CVHelper** 類別，需要影像使用搭配預乘 Alpha 的 BRGA8 像素格式。 如果傳遞至事件的畫面具有不同的格式，請將 **SoftwareBitmap** 轉換為正確的格式。 接下來，建立 **SoftwareBitmap** 以做為模糊作業的目標。 將使用來源影像屬性做為建構函式的引數，來建立具備相符格式的點陣圖。 呼叫協助程式類別 **Blur** 方法來處理畫面。 最後，將模糊作業的輸出影像傳遞至 **FrameRenderer** 協助程式類別的 **PresentSoftwareBitmap** 方法，以在其初始化的 XAML **Image** 控制項中顯示影像。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVFrameArrived":::

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [使用 MediaFrameReader 處理媒體畫面](process-media-frames-with-mediaframereader.md)
* [使用 OpenCV 處理軟體點陣圖](process-software-bitmaps-with-opencv.md)
* [相機畫面範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [相機畫面 + OpenCV 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV)
 

 
