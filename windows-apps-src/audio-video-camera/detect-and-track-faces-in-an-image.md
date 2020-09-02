---
ms.assetid: 84729E44-10E9-4D7D-8575-6A9D97467ECD
description: 本主題說明如何使用 FaceDetector 來偵測影像中的臉部。 FaceTracker 已進行最佳化，可在一連串視訊框架中用來追蹤隨著時間改變的臉部。
title: 偵測影像或影片中的臉部
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6232a8003140284956319728e16b3b953aff3177
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362731"
---
# <a name="detect-faces-in-images-or-videos"></a>偵測影像或影片中的臉部



\[某些資訊與預先發行的產品有關，在正式發行之前可能會經過大幅修改。 針對此處提供的資訊，Microsoft 不做任何明示或默許的擔保。\]

本主題說明如何使用 [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) 來偵測影像中的臉部。 [**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) 已進行最佳化，可在一連串視訊框架中用來追蹤隨著時間改變的臉部。

如需使用 [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) 追蹤臉部的替代方法，請參閱[媒體擷取的場景分析](scene-analysis-for-media-capture.md)。

本文中的程式碼是採用[基本臉部偵測](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceDetection)和[基本臉部追蹤](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceTracking)範例的程式碼。 您可以下載這些範例來查看內容中使用的程式碼，或是使用此範例做為自己 app 的起點。

## <a name="detect-faces-in-a-single-image"></a>偵測單一影像中的臉部

[**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) 類別可讓您偵測靜物影像中的一或多個臉部。

這個範例會使用來自下列命名空間的 API。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetFaceDetectionUsing":::

針對 [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) 物件以及將在影像中進行偵測的 [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) 物件清單來宣告類別成員變數。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetClassVariables1":::

臉部偵測可在使用各種不同方式建立的 [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 物件上運作。 這個範例使用 [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)，讓使用者能夠挑選將在其中偵測臉部的影像檔。 如需使用軟體點陣圖的詳細資訊，請參閱[影像處理](imaging.md)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetPicker":::

使用 [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) 類別，將影像檔解碼到 **SoftwareBitmap**。 利用較小的影像可讓臉部偵測程序更快速，因此，您可能會想縮小來源影像的大小。 您可以在解碼期間執行此動作，方法是建立 [**BitmapTransform**](/uwp/api/Windows.Graphics.Imaging.BitmapTransform) 物件、設定 [**ScaledWidth**](/uwp/api/windows.graphics.imaging.bitmaptransform.scaledwidth) 和 [**ScaledHeight**](/uwp/api/windows.graphics.imaging.bitmaptransform.scaledheight) 屬性並將它傳送到對 [**GetSoftwareBitmapAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) 的呼叫，這會傳回已解碼且已調整大小的 **SoftwareBitmap**。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetDecode":::

在目前版本中，**FaceDetector** 類別僅支援 Gray8 或 Nv12 的影像。 **SoftwareBitmap** 類別提供 [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) 方法，可將點陣圖從某一種格式轉換成其他格式。 這個範例會將來源影像轉換為 Gray8 像素格式 (如果還不是這種格式)。 如有需要，您可以使用 [**GetSupportedBitmapPixelFormats**](/uwp/api/windows.media.faceanalysis.facedetector.getsupportedbitmappixelformats) 和 [**IsBitmapPixelFormatSupported**](/uwp/api/windows.media.faceanalysis.facedetector.isbitmappixelformatsupported) 方法，在執行階段判斷是否支援某種像素格式 (假設將在未來版本中擴充支援的格式組合)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetFormat":::

將 **FaceDetector** 物件具現化，方法是呼叫 [**CreateAsync**](/uwp/api/windows.media.faceanalysis.facedetector.createasync)，然後呼叫 [**DetectFacesAsync**](/uwp/api/windows.media.faceanalysis.facedetector.detectfacesasync)，在已將大小調整為合理大小且轉換成支援像素格式的點陣圖中進行傳遞。 這個方法會傳回 [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) 物件的清單。 **ShowDetectedFaces** 是一個協助程式方法 (如下所示)，會在影像中的臉部四周繪製方框。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetDetect":::

確定會處置在臉部偵測程序期間建立的物件。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetDispose":::

若要顯示影像並在偵測到的臉部四周繪製方框，可在您的 XAML 頁面中新增 [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) 元素。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml" id="SnippetCanvas":::

定義一些成員變數來設定將繪製的方框樣式。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetClassVariables2":::

在 **ShowDetectedFaces** 協助程式方法中會建立新的 [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)，並將來源設為從代表來源影像的 **SoftwareBitmap** 中建立的 [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource)。 XAML **Canvas** 控制項的背景已設定為影像筆刷。

如果傳遞到協助程式方法的臉部清單不是空的，請重複執行清單中的每一個臉部，並使用 [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) 類別的 [**FaceBox**](/uwp/api/windows.media.faceanalysis.detectedface.facebox) 屬性來判斷影像中包含臉部的方框大小與位置。 因為 **Canvas** 控制項的大小很可能與來源影像的大小不同，所以，您應該將 X 和 Y 座標以及 **FaceBox** 的寬度和高度乘以縮放比例值，此值是來源影像大小與 **Canvas** 控制項實際大小的比例。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetShowDetectedFaces":::

## <a name="track-faces-in-a-sequence-of-frames"></a>追蹤一系列畫面中的臉部

如果您想要偵測影片中的臉部，比起 [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) 類別，更有效率的方式是使用 [**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) 類別 (儘管這兩者的實作步驟非常類似)。 **FaceTracker** 會使用先前處理過的畫面相關資訊來將偵測程序最佳化。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetFaceTrackingUsing":::

宣告 **FaceTracker** 物件的類別變數。 這個範例會使用 [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer)，以定義的時間間隔來初始臉部追蹤。 [SemaphoreSlim](/dotnet/api/system.threading.semaphoreslim) 可用來確定一次只會執行一個臉部追蹤作業。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetClassVariables3":::

若要初始臉部追蹤作業，請呼叫 [**CreateAsync**](/uwp/api/windows.media.faceanalysis.facetracker.createasync) 來建立新的 **FaceTracker** 物件。 初始所需的時間間隔，然後建立計時器。 每次超過指定的間隔時間，就會呼叫 **ProcessCurrentVideoFrame** 協助程式方法。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetTrackingInit":::

計時器會以非同步方式來呼叫 **ProcessCurrentVideoFrame** 協助程式，因此，此方法會先呼叫旗號的 **Wait** 方法，來查看追蹤作業是否正在進行中，如果正在進行，方法即會回傳而不需嘗試偵測臉部。 在這個方法結束時，會呼叫旗號的 **Release** 方法，這樣就能對 **ProcessCurrentVideoFrame** 進行後續呼叫來繼續。

[**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) 類別會在 [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame) 物件上運作。 有多種方式您可以用來取得 **VideoFrame**，包括從執行中的 [MediaCapture](./index.md) 物件擷取預覽框架，或者透過實作 [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) 的 [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicaudioeffect.processframe) 方法 。 這個範例使用未定義的協助程式方法，此方法會傳回視訊畫面 **GetLatestFrame**，以做為這個作業的預留位置。 如需從執行中的媒體擷取裝置的預覽資料流中取得視訊畫面的相關資訊，請參閱[取得預覽畫面](get-a-preview-frame.md)。

如同 **FaceDetector**，**FaceTracker** 支援一組有限的像素格式。 這個範例會在提供的畫面格式不是 Nv12 格式時放棄臉部偵測。

呼叫 [**ProcessNextFrameAsync**](/uwp/api/windows.media.faceanalysis.facetracker.processnextframeasync) 來抓取 [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) 物件清單，這類物件代表畫面中的臉部。 具備臉部清單之後，就可以使用上述針對臉部偵測所說明的相同方式來顯示它們。 請注意，因為不會在 UI 執行緒上呼叫臉部追蹤 helper 方法，所以您必須在呼叫 [**CoreDispatcher. RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)中進行任何 UI 更新。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/FaceDetection_Win10/cs/MainPage.xaml.cs" id="SnippetProcessCurrentVideoFrame":::

## <a name="related-topics"></a>相關主題

* [媒體擷取的場景分析](scene-analysis-for-media-capture.md)
* [基本臉部偵測範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceDetection)
* [基本臉部追蹤範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceTracking)
* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [媒體播放](media-playback.md)
