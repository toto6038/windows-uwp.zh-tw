---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: 本文說明如何使用 CameraCaptureUI 類別，透過 Windows 內建的相機 UI 來擷取相片或視訊。
title: 使用 Windows 內建相機 UI 來擷取相片和視訊
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 35eed8310b406a960334c90d6c359c0313b2660c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358891"
---
# <a name="capture-photos-and-video-with-windows-built-in-camera-ui"></a>使用 Windows 內建相機 UI 來擷取相片和視訊



本文說明如何使用 CameraCaptureUI 類別，透過 Windows 內建的相機 UI 來擷取相片或視訊。 這個功能很容易使用，可讓 app 只需幾行程式碼即可取得使用者所擷取的相片或視訊。

如果您想提供自己的相機 UI 或您的案例需要更健全的擷取作業低階控制項，您應該使用 [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) 物件並實作您自己的擷取體驗。 如需詳細資訊，請參閱[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)。

> [!NOTE]
> 您不應指定**網路攝影機**或是**麥克風**應用程式中的功能資訊清單檔案，如果您的應用程式只會使用 CameraCaptureUI。 因為如果您這麼做，將會以裝置的相機隱私性設定您的應用程式；但即使使用者拒絕相機對您應用程式的存取，還是不會防止 CameraCaptureUI 擷取媒體。 原因是 Windows 內建相機應用程式是信任的第一方應用程式，需要使用者透過按下按鈕來起始相片、音訊和視訊擷取。 您的應用程式可能會失敗時，如果您指定的網路攝影機或麥克風功能使用 CameraCaptureUI 作為唯一相片擷取機制提交至市集時，Windows 應用程式認證套件認證。
> 如果您是使用 MediaCapture 透過程式設計方式來擷取音訊、相片或視訊，則必須在應用程式資訊清單檔案中指定網路攝影機或麥克風功能。

## <a name="capture-a-photo-with-cameracaptureui"></a>使用 CameraCaptureUI 擷取相片

若要使用相機擷取 UI，您應該在專案中包含 [**Windows.Media.Capture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture) 命名空間。 若要使用傳回的影像檔案進行檔案作業，請包含 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage)。

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

若要擷取相片，請建立新的 [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 物件。 使用物件的 [**PhotoSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.photosettings) 屬性，可以指定所傳回相片的屬性，例如相片的影像格式。 根據預設，相機擷取 UI 可讓使用者在傳回相片前進行裁剪，但可使用 [**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) 屬性予以停用。 此範例會設定 [**CroppedSizeInPixels**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) 以要求傳回的影像為 200 x 200 像素。

> [!NOTE]
> 行動裝置系列中的裝置不支援在 **CameraCaptureUI** 中進行影像裁剪。 當您的 app 在這些裝置上執行時會忽略 [**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) 屬性的值。

呼叫 [**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.) 並指定 [**CameraCaptureUIMode.Photo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUIMode)，來指定應該要擷取相片。 這個方法會傳回 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 包含影像的執行個體 (如果擷取成功的話)。 如果使用者取消擷取，則傳回的物件為 Null。

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

包含擷取的相片的 **StorageFile** 會被指定一個動態產生的名稱，並儲存在您的應用程式本機資料夾中。 為了更清楚地組織您擷取的相片，您可能想要將檔案移動到不同的資料夾。

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

若要在您的 app 中使用相片，您可能想建立 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 物件，該物件可搭配數個不同的通用 Windows app 功能使用。

首先，您應該在專案中包含 [**Windows.Graphics.Imaging**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging) 命名空間。

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

呼叫 [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) 以從影像檔取得資料流。 呼叫 [**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) 以取得資料流的點陣圖解碼器。 然後呼叫 [**GetSoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) 以取得影像的 **SoftwareBitmap** 表示法。

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

若要在 UI 中顯示影像，請在 XAML 頁面中宣告 [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 控制項。

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

若要在 XAML 頁面中使用軟體點陣圖，請在專案中包含 using [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) 命名空間。

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

**Image** 控制項要求影像來源為 BGRA8 格式 (具有預乘的 Alpha 或沒有 Alpha)，所以呼叫靜態方法 [**SoftwareBitmap.Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.windows) 以使用所需的格式建立新的軟體點陣圖。 接下來，建立新的 [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) 物件並呼叫 [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) 以將軟體點陣圖指派給來源。 最後，設定**Image** 控制項的 [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) 屬性，以在 UI 中顯示所擷取的相片。

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>使用 CameraCaptureUI 擷取視訊

若要擷取視訊，請建立新的 [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 物件。 使用物件的 [**VideoSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings) 屬性，可以指定所傳回視訊的屬性，例如視訊的影像格式。

呼叫 [**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.) 並指定 [**Video**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings)，以指定應該擷取視訊。 這個方法會傳回 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 包含視訊的執行個體 (如果擷取成功的話)。 如果使用者取消擷取，則傳回的物件為 Null。

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

您對擷取的視訊檔案所做的動作取決於您的 app 案例。 本文的其餘部分說明如何從一或多個擷取的視訊快速建立媒體組合並顯示在 UI 中。

首先，新增 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 控制項，以便讓視訊組合顯示在 XAML 頁面中。

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]


利用從相機擷取 UI 傳回的視訊檔案，藉由呼叫 [**CreateFromStorageFile**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 來建立新的 **[MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)** 。 呼叫與 **MediaPlayerElement** 相關聯之預設 **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** 的 **[Play](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)** 方法，來播放影片。

[!code-cs[PlayVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetPlayVideo)]
 

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [MediaCapture 擷取基本的相片、 視訊和音訊](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 
 

 




