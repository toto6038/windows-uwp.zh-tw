---
ms.assetid: 66d0c3dc-81f6-4d9a-904b-281f8a334dd0
description: 本文示範使用 MediaCapture 類別來擷取相片和視訊的最簡單方式。
title: 使用 MediaCapture 進行基本相片、視訊和音訊的擷取
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 28974fea7861022c383efa5bf61565c4f18b5f8d
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254329"
---
# <a name="basic-photo-video-and-audio-capture-with-mediacapture"></a>使用 MediaCapture 進行基本相片、視訊和音訊的擷取


本文示範使用 [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) 類別來擷取相片和視訊的最簡單方式。 **MediaCapture** 類別會公開一組健全的 API，可提供擷取管線的低階控制權，並啟用進階擷取案例，但本文的目的是協助您快速且輕鬆地新增對 app 的基本媒體擷取。 如需深入了解 **MediaCapture** 提供的功能，請參閱[**相機**](camera.md)。

如果您只想擷取相片或視訊，而不想新增任何其他媒體擷取功能，或者不想建立自己的相機 UI，您可能會想使用 [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 類別，此類別讓您只需啟動 Windows 內建的相機 app，即可接收已擷取的相片或視訊檔案。 如需詳細資訊，請參閱[**使用 Windows 內建相機 UI 來擷取相片和視訊**](capture-photos-and-video-with-cameracaptureui.md)

此文章中的程式碼是從[**相機入門套件**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit)範例改編而來。 您可以下載範例以查看實際使用的程式碼，或以此範例做為自己的 App 起點。

## <a name="add-capability-declarations-to-the-app-manifest"></a>將功能宣告加入至應用程式資訊清單

為了讓您的 app 可存取裝置的相機，您必須宣告您的 app 使用 *webcam* 和 *microphone* 裝置功能。 如果您要將擷取的相片和視訊儲存到使用者的圖片媒體櫃或視訊媒體櫃，您也必須宣告 *picturesLibrary* 和 *videosLibrary* 功能。

**將功能新增至應用程式資訊清單**

1.  在 Microsoft Visual Studio 中，按兩下 **\[方案總管\]** 中的 **package.appxmanifest** 項目，開啟應用程式資訊清單的設計工具。
2.  選取 **\[功能\]** 索引標籤。
3.  核取 **\[網路攝影機\]** 方塊和 **\[麥克風\]** 方塊。
4.  如果要存取圖片媒體櫃和視訊媒體櫃，請選取 **\[圖片媒體櫃\]** 方塊和 **\[視訊媒體櫃\]** 方塊。


## <a name="initialize-the-mediacapture-object"></a>初始化 MediaCapture 物件
本文所述的所有擷取方法所需的第一個步驟是藉由呼叫建構函式，然後呼叫 [**InitializeAsync**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)，來初始化 [**MediaCapture**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync) 物件。 由於 **MediaCapture** 物件將可從您 app 內的多個位置中存取，因此，請宣告類別變數來保存此物件。  實作適用於 **MediaCapture** 物件的 [**Failed**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.failed) 事件的處理常式，以便在擷取作業失敗時收到通知。

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

[!code-cs[InitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-up-the-camera-preview"></a>設定相機預覽
您能夠使用 **MediaCapture** 來擷取相片、視訊及音訊，而不需顯示相機預覽，但通常您會想要顯示預覽資料流，讓使用者可以看到擷取到的內容。 此外，有一些 **MediaCapture** 功能需要預覽資料流處於執行狀態，才能夠啟用它們，包括自動對焦、自動曝光及自動白平衡。 若要了解如何設定相機預覽，請參閱[**顯示相機預覽**](simple-camera-preview-access.md)。

## <a name="capture-a-photo-to-a-softwarebitmap"></a>將相片擷取到 SoftwareBitmap
[  **SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 類別是在 Windows 10 中所引進，可提供多個功能中常見的影像表示法。 如果您想要擷取相片，然後立即在 app 中使用擷取的影像 (例如將它顯示於 XAML 中)，而不是擷取到檔案中，則您應該擷取到 **SoftwareBitmap**。 您稍後仍然可以選擇將影像儲存到磁碟。

初始化 **MediaCapture** 物件之後，您可以使用LowLagPhotoCapture[**類別，將相片擷取到**SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.LowLagPhotoCapture)。 藉由呼叫 [**PrepareLowLagPhotoCaptureAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.preparelowlagphotocaptureasync) 來取得此類別的執行個體，其會傳入 [**ImageEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) 物件，以指定您所需的影像格式。 [**CreateUncompressed**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.imageencodingproperties.createuncompressed)會使用指定的像素格式來建立未壓縮的編碼。 呼叫 [**CaptureAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagphotocapture.captureasync) 來擷取相片，這會傳回 [**CapturedPhoto**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CapturedPhoto) 物件。 透過存取Frame[**屬性，然後存取**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedphoto.frame)SoftwareBitmap[**屬性，來取得**SoftwareBitmap](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.softwarebitmap)。

您可以視需要重複呼叫 **CaptureAsync** 來擷取多張相片。 完成擷取時，呼叫 [**FinishAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.finishasync) 來關閉 **LowLagPhotoCapture** 工作階段，並釋放相關聯的資源。 呼叫 **FinishAsync** 之後，若要再次開始擷取相片，您將需要再次呼叫 [**PrepareLowLagPhotoCaptureAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.preparelowlagphotocaptureasync) 來將擷取工作階段重新初始化，然後再呼叫 [**CaptureAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagphotocapture.captureasync)。

[!code-cs[CaptureToSoftwareBitmap](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToSoftwareBitmap)]

從 Windows 版本 1803 開始，您可以存取從 [CaptureAsync**傳回之**CapturedFrame](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.bitmapproperties) 類別的BitmapProperties 屬性，以擷取被擷取相片的相關中繼資料。 您可以將此資料傳遞至 **BitmapEncoder** 以將中繼資料儲存到檔案。 以前沒有方法存取未壓縮影像格式的此項資料。 您也可以存取 [**ControlValues**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.controlvalues) 屬性來擷取 [**CapturedFrameControlValues**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframecontrolvalues) 物件，此物件描述所擷取畫面的控制項值，例如曝光及白平衡。

如需使用 **BitmapEncoder** 和操作 **SoftwareBitmap** 物件的相關資訊 (包含如何在 XAML 頁面中顯示該物件)，請參閱[**建立、編輯和儲存點陣圖影像**](imaging.md)。 

如需設定擷取裝置控制值的詳細資訊，請參閱[擷取相片和視訊的裝置控制項](capture-device-controls-for-photo-and-video-capture.md)。

從 Windows 10 版本 1803 開始，您可以透過存取 [MediaCapture**傳回之**CapturedFrame](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.bitmapproperties) 的BitmapProperties 屬性，取得以未壓縮格式擷取之相片的中繼資料，例如 EXIF 資訊。 在舊版中，只能在以壓縮檔案格式擷取之相片的標頭中存取此項資料。 手動寫入影像檔時，您可以將這項資料提供至 [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder)。 如需編碼點陣圖的詳細資訊，請參閱[建立、編輯和儲存點陣圖影像](imaging.md)。  您也可以透過存取 [**ControlValues**](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.capturedframe.controlvalues) 屬性，存取擷取影像時所用的畫面控制項值，例如曝光和閃光燈設定。 如需詳細資訊，請參閱[擷取相片和視訊擷取的裝置控制項](capture-device-controls-for-photo-and-video-capture.md)。

## <a name="capture-a-photo-to-a-file"></a>將相片擷取到檔案
典型的攝影 app 會將擷取的相片儲存到磁碟或雲端儲存空間，而且需要將中繼資料 (例如相片方向) 新增到檔案。 下列範例示範如何將相片擷取到檔案。 您稍後仍然可以選擇從影像檔建立 **SoftwareBitmap**。 

這個範例中示範的技術會將相片擷取到記憶體內部的資料流，然後將相片從資料流轉碼為磁碟上的檔案。 這個範例使用 [**GetLibraryAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.getlibraryasync) 來取得使用者的圖片媒體櫃，然後使用 [**SaveFolder**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.savefolder) 屬性來取得參考預設儲存資料夾。 請記得將**圖片媒體櫃**功能新增到您的應用程式資訊清單，以便存取這個資料夾。 [**CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync)會建立要儲存相片的新[**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 。

建立 [**InMemoryRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream)，然後呼叫 [**CapturePhotoToStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.capturephototostreamasync)，來將相片擷取到資料流，其會傳入資料流和 [**ImageEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) 物件，以指定應使用的影像格式。 您可以自行初始化該物件來建立自訂的編碼屬性，但類別會針對常見的編碼格式提供靜態方法，例如 [**ImageEncodingProperties.CreateJpeg**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.imageencodingproperties.createjpeg)。 接下來，呼叫 [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) 來建立輸出檔的檔案資料流。 建立 [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder)，將影像從記憶體內部的資料流解碼，接著建立 [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder)，藉由呼叫 [**CreateForTranscodingAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync) 來將影像編碼為檔案。

您可以選擇建立 [**BitmapPropertySet**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) 物件，然後在影像編碼器上呼叫 [**SetPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync)，以便在影像檔中包含相片相關的中繼資料。 如需編碼屬性的詳細資訊，請參閱[**影像中繼資料**](image-metadata.md)。 對於大部分的攝影 app 來說，適當地處理裝置方向是不可或缺的。 如需詳細資訊，請參閱[**使用 MediaCapture 處理裝置方向**](handle-device-orientation-with-mediacapture.md)。

最後，在編碼器物件上呼叫 [**FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync)，將相片從記憶體內部的資料流轉碼為檔案。

[!code-cs[CaptureToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToFile)]

如需使用檔案和資料夾的詳細資訊，請參閱[**檔案、資料夾和媒體櫃**](https://docs.microsoft.com/windows/uwp/files/index)。

## <a name="capture-a-video"></a>擷取視訊
使用 [**LowLagMediaRecording**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.LowLagMediaRecording) 類別，快速地將視訊擷取新增到您的 app。 首先，宣告物件的類別變數。

[!code-cs[LowLagMediaRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetLowLagMediaRecording)]

接下來，建立將儲存視訊的 **StorageFile** 物件。 請注意，若要儲存到使用者的視訊媒體櫃 (如此範例所示)，您必須將**視訊媒體櫃**功能新增到您的應用程式資訊清單。 呼叫 [**PrepareLowLagRecordToStorageFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.preparelowlagrecordtostoragefileasync) 來初始化媒體錄製，其會傳入存放檔案和 [**MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) 物件，以指定視訊的編碼方式。 此類別提供靜態方法 (例如 [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4))，來建立常見的視訊編碼設定檔。

最後，呼叫 [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.startasync) 開始擷取視訊。

[!code-cs[StartVideoCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartVideoCapture)]

若要停止錄製視訊，請呼叫 [**StopAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.stopasync)。

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

您可以繼續呼叫 **StartAsync** 和 **StopAsync** 來擷取其他視訊。 當您完成擷取視訊時，呼叫 [**FinishAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.finishasync) 來處置擷取工作階段，並清除相關聯的資源。 在這個呼叫之後，您必須再次呼叫 **PrepareLowLagRecordToStorageFileAsync** 以重新初始化拍攝工作階段，然後再呼叫 **StartAsync**。

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

擷取視訊時，您應該登錄適用於 [MediaCapture**物件的**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.recordlimitationexceeded)RecordLimitationExceeded 事件的處理常式，如果您超過單一錄製的限制 (目前為三小時)，則作業系統將會觸發此處理常式。 在事件的處理常式中，您應該藉由呼叫 [**StopAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.stopasync) 來完成錄製。

[!code-cs[RecordLimitationExceeded](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

[!code-cs[RecordLimitationExceededHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceededHandler)]

### <a name="play-and-edit-captured-video-files"></a>播放和編輯擷取的視訊檔案
您將視訊擷取至檔案後，可能會想要載入檔案並在應用程式的 UI 中播放。 您可以使用 **[MediaPlayerElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)** XAML 控制項和相關 **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** 進行這項作業。 如需在 XAML 頁面中播放媒體的相關資訊，請參閱[使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)。

您也可以透過呼叫 **[CreateFromFileAsync](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip)** ，從視訊檔案建立 **[MediaClip](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromfileasync)** 物件。  **[MediaComposition](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition)** 提供基本的視訊編輯功能，例如排列 **MediaClip** 物件的順序、修剪視訊長度、建立層級、增加背景音樂和套用視訊效果。 如需操作媒體組合的詳細資訊，請參閱[媒體組合和編輯](media-compositions-and-editing.md)。

## <a name="pause-and-resume-video-recording"></a>暫停和繼續視訊錄製
您可以藉由呼叫 [**PauseAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.pauseasync)，然後呼叫 [**ResumeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.resumeasync)，來暫停視訊錄製，然後再繼續錄製，而不需建立個別的輸出檔案。

[!code-cs[PauseRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseRecordingSimple)]

[!code-cs[ResumeRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeRecordingSimple)]

從 Windows 10 版本 1607 開始，您可以暫停視訊錄製，並且會接收到在暫停錄製之前所擷取到的最後一個畫面。 然後，您可以在相機預覽中重疊此畫面，讓使用者能夠在繼續錄製之前，使用暫停的畫面來校準相機。 呼叫 [**PauseWithResultAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.pausewithresultasync) 會傳回 [**MediaCapturePauseResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapturePauseResult) 物件。 [  **LastFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturepauseresult.lastframe) 屬性是代表最後一個畫面的 [**VideoFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.VideoFrame) 物件。 若要在 XAML 中顯示此畫面，請取得視訊畫面的 **SoftwareBitmap** 表示法。 目前，僅支援含有預乘或空的 Alpha 色板且格式為 BGRA8 的影像，因此，請視需要呼叫 [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) 來取得正確的格式。  建立新的 [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) 物件並呼叫 [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) 進行初始化。 最後，設定 XAMLImage[**控制項的**Source](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 屬性來顯示影像。 若要使用這個祕訣，您的影像必須與 **CaptureElement** 控制項對齊，而且透明度值應小於 1。 別忘了，您只能修改 UI 執行緒上的 UI，因此請在 [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) 內部進行此呼叫。

**PauseWithResultAsync** 也會傳回前一段錄製的視訊持續時間，以防您需要追蹤總錄製時間。

[!code-cs[PauseCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseCaptureWithResult)]

當您繼續錄製時，您可以將影像來源設為 null 加以隱藏。

[!code-cs[ResumeCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeCaptureWithResult)]

請注意，您也可以在停止視訊時，呼叫 [**StopWithResultAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.stopwithresultasync) 來取得結果畫面。


## <a name="capture-audio"></a>擷取音訊 
您可以使用上述用來擷取視訊的相同技術，快速地將音訊擷取新增到您的 app。 下列範例會在應用程式資料資料夾中建立 **StorageFile**。 呼叫 [**PrepareLowLagRecordToStorageFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.preparelowlagrecordtostoragefileasync) 來將擷取工作階段初始化，其會傳入檔案和 [**MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)，在這個範例中這是透過 [**CreateMp3**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3) 靜態方法所產生。 若要開始錄製，請呼叫 [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.startasync)。

[!code-cs[StartAudioCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartAudioCapture)]


呼叫 [**StopAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagphotosequencecapture.stopasync) 以停止音訊錄製。

## <a name="related-topics"></a>相關主題

* [相機](camera.md)  
[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

您可以多次呼叫 **StartAsync** 和 **StopAsync** 來錄製數個音訊檔。 當您完成擷取音訊時，呼叫 [**FinishAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.finishasync) 來處置擷取工作階段，並清除相關聯的資源。 在這個呼叫之後，您必須再次呼叫 **PrepareLowLagRecordToStorageFileAsync** 以重新初始化拍攝工作階段，然後再呼叫 **StartAsync**。

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]


## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>偵測及回應系統進行的音量變更
從 Windows 10 版本 1803 開始，您的應用程式可偵測系統何時將應用程式音訊擷取和音訊轉譯串流的音量降低或設為靜音。 例如，系統可能會在您的應用程式進入背景時，將其串流設為靜音。 [  **AudioStateMonitor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor) 類別可讓您註冊以在系統修改音訊資料流的音量時接收事件。 透過呼叫CreateForCaptureMonitoring[ **，取得** AudioStateMonitor](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.createforcapturemonitoring#Windows_Media_Audio_AudioStateMonitor_CreateForCaptureMonitoring) 的執行個體以監控音訊擷取串流。 透過呼叫 [**CreateForRenderMonitoring**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.createforrendermonitoring)，取得用於監控音訊轉譯串流的執行個體。 註冊每個監視器的 [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) 事件處理常式，以在系統變更對應串流類別的音訊時收到通知。

[!code-cs[AudioStateMonitorUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateMonitorUsing)]

[!code-cs[AudioStateVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[RegisterAudioStateMonitor](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

在擷取串流的 **SoundLevelChanged** 事件處理常式中，您可以檢查 [AudioStateMonitor**傳送者的**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel)SoundLevel 屬性來判斷新的音量大小。 請注意，擷取串流永遠不應由系統降低。 它應該只能設為靜音或切換回最大音量。 如果音訊被設為靜音，您可以停止進行中的擷取。 如果音訊還原為最大音量，您可以再次開始擷取。 下列範例使用一些布林值類別變數來追蹤應用程式目前是否正在擷取音訊，以及是否因為音訊狀態變更而停止擷取。 這些變數可用來判斷何時適合以程式設計方式停止或開始擷取音訊。

[!code-cs[CaptureSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureSoundLevelChanged)]

下列程式碼範例示範實作音訊轉譯的 **SoundLevelChanged**處理常式。 根據您的應用程式案例，以及您正在播放的內容類型，您可以在音量降低時暫停音訊播放。 如需處理媒體播放之音量變更的詳細資訊，請參閱[使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)。

[!code-cs[RenderSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRenderSoundLevelChanged)]


* [使用 Windows 內建攝影機 UI 來捕捉相片和影片](capture-photos-and-video-with-cameracaptureui.md)
* [使用 MediaCapture 處理裝置方向](handle-device-orientation-with-mediacapture.md)
* [建立、編輯和儲存點陣圖影像](imaging.md)
* [檔案、資料夾和媒體櫃](https://docs.microsoft.com/windows/uwp/files/index)

