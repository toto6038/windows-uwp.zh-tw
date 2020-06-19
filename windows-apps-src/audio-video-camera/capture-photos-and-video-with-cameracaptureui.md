---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: 本文說明如何使用[**CameraCaptureUI**](/uwp/api/windows.media.capture.cameracaptureui)類別，透過 Windows 內建的相機 UI 來捕捉相片或影片。
title: 使用 Windows 內建攝影機 UI 來捕捉相片和影片
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: a512f72c01f2082dd067fc867f7434c92d2aa0c8
ms.sourcegitcommit: 79e4b3a9c53060b64513e2e240f0a4f073cc5dab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/18/2020
ms.locfileid: "84978933"
---
# <a name="capture-photos-and-video-with-the-windows-built-in-camera-ui"></a>使用 Windows 內建攝影機 UI 來捕捉相片和影片

本文說明如何使用[**CameraCaptureUI**](/uwp/api/windows.media.capture.cameracaptureui)類別，透過 Windows 內建的相機 UI 來捕捉相片或影片。 這項功能很容易使用。 它可讓您的應用程式只需幾行程式碼，就能取得使用者所捕獲的相片或影片。

如果您想要提供自己的相機 UI，或如果您的案例需要更健全、低層級的 capture 操作控制，則您應該使用[**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture)類別，並執行您自己的 capture 體驗。 如需詳細資訊，請參閱[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)。

> [!NOTE]
> 如果您的應用程式只使用**CameraCaptureUI**，您就不應該在應用程式資訊清單檔中指定**網路**攝影機或**麥克風**功能。 如果您這樣做，您的應用程式將會顯示在裝置的相機隱私權設定中，但即使使用者拒絕相機存取您的應用程式，也不會導致**CameraCaptureUI**無法捕獲媒體。 <p>原因是 Windows 內建相機應用程式是信任的第一方應用程式，需要使用者透過按下按鈕來起始相片、音訊和視訊擷取。 當您使用**CameraCaptureUI**作為唯一的相片捕捉機制時，如果指定網路攝影機或麥克風功能，您的應用程式可能會在提交到 Microsoft Store 時，將 Windows 應用程式認證套件認證失敗。<p>
如果您使用**MediaCapture**以程式設計方式來捕捉音訊、相片或影片，您必須在應用程式資訊清單檔中指定**網路**攝影機或**麥克風**功能。

## <a name="capture-a-photo-with-cameracaptureui"></a>使用 CameraCaptureUI 擷取相片

若要使用相機擷取 UI，您應該在專案中包含 [**Windows.Media.Capture**](/uwp/api/Windows.Media.Capture) 命名空間。 若要使用傳回的影像檔案進行檔案作業，請包含 [**Windows.Storage**](/uwp/api/Windows.Storage)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingCaptureUI":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingCaptureUI":::

若要擷取相片，請建立新的 [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) 物件。 藉由使用物件的[**PhotoSettings**](/uwp/api/windows.media.capture.cameracaptureui.photosettings)屬性，您可以指定所傳回相片的屬性，例如相片的影像格式。 根據預設，相機捕捉 UI 支援裁剪相片，然後再傳回。 您可以使用[**AllowCropping**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping)屬性來停用此功能。 此範例會設定 [**CroppedSizeInPixels**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) 以要求傳回的影像為 200 x 200 像素。

> [!NOTE]
> 行動裝置系列中的裝置不支援**CameraCaptureUI**中的影像裁剪。 當您的 app 在這些裝置上執行時會忽略 [**AllowCropping**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) 屬性的值。

呼叫 [**CaptureFileAsync**](/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) 並指定 [**CameraCaptureUIMode.Photo**](/uwp/api/Windows.Media.Capture.CameraCaptureUIMode)，來指定應該要擷取相片。 這個方法會傳回 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 包含影像的執行個體 (如果擷取成功的話)。 如果使用者取消擷取，則傳回的物件為 Null。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCapturePhoto":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCapturePhoto":::

包含擷取的相片的 **StorageFile** 會被指定一個動態產生的名稱，並儲存在您的應用程式本機資料夾中。 若要更有效地組織已捕捉的相片，您可以將檔案移到不同的資料夾。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCopyAndDeletePhoto":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCopyAndDeletePhoto":::

若要在您的 app 中使用相片，您可能想建立 [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 物件，該物件可搭配數個不同的通用 Windows app 功能使用。

首先，在您的專案中包含[**Windows. 影像處理**](/uwp/api/Windows.Graphics.Imaging)命名空間。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingSoftwareBitmap":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingSoftwareBitmap":::

呼叫 [**OpenAsync**](/uwp/api/windows.storage.istoragefile.openasync) 以從影像檔取得資料流。 呼叫 [**BitmapDecoder.CreateAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) 以取得資料流的點陣圖解碼器。 然後，呼叫[**GetSoftwareBitmap**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync)以取得影像的**SoftwareBitmap**標記法。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetSoftwareBitmap":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetSoftwareBitmap":::

若要在 UI 中顯示影像，請在 XAML 頁面中宣告 [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) 控制項。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetImageControl":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetImageControl":::

若要在 XAML 頁面中使用軟體點陣圖，請在專案中包含 using [**Windows.UI.Xaml.Media.Imaging**](/uwp/api/Windows.UI.Xaml.Media.Imaging) 命名空間。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingSoftwareBitmapSource":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingSoftwareBitmapSource":::

**影像**控制項要求影像來源必須是具有預乘 Alpha 或無 ALPHA 的 BGRA8 格式。 呼叫靜態方法[**SoftwareBitmap。轉換**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert)以使用所需的格式建立新的軟體點陣圖。 接下來，建立新的[**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource)物件，並呼叫它[**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) ，將軟體點陣圖指派給來源。 最後，設定**Image** 控制項的 [**Source**](/uwp/api/windows.ui.xaml.controls.image.source) 屬性，以在 UI 中顯示所擷取的相片。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetSetImageSource":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetSetImageSource":::

## <a name="capture-a-video-with-cameracaptureui"></a>使用 CameraCaptureUI 擷取視訊

若要擷取視訊，請建立新的 [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) 物件。 藉由使用物件的[**VideoSettings**](/uwp/api/windows.media.capture.cameracaptureui.videosettings)屬性，您可以指定所傳回影片的屬性，例如影片的格式。

呼叫[**CaptureFileAsync**](/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) ，並指定[**影片**](/uwp/api/windows.media.capture.cameracaptureui.videosettings)來捕捉影片。 這個方法會傳回 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 包含視訊的執行個體 (如果擷取成功的話)。 如果您取消捕捉，傳回的物件會是 null。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCaptureVideo":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCaptureVideo":::

您對擷取的視訊檔案所做的動作取決於您的 app 案例。 本文的其餘部分說明如何從一或多個擷取的視訊快速建立媒體組合並顯示在 UI 中。

首先，新增[**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)控制項，其中影片組合會顯示在您的 XAML 頁面上。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml" id="SnippetMediaElement":::

當影片檔案從相機捕捉 UI 傳回時，藉由呼叫**[CreateFromStorageFile](/uwp/api/windows.media.core.mediasource.createfromstoragefile)** 來建立新的[**MediaSource**](/uwp/api/windows.media.core.mediasource) 。 呼叫與 **MediaPlayerElement** 相關聯之預設 **[MediaPlayer](/uwp/api/windows.media.playback.mediaplayer)** 的 **[Play](/uwp/api/windows.media.playback.mediaplayer.Play)** 方法，來播放影片。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetPlayVideo":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetPlayVideo":::

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI)
