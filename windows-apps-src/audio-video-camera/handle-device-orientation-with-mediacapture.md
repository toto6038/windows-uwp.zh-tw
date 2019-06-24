---
ms.assetid: af3941c0-3508-4ba2-a79e-fc71657c605f
description: 本文示範如何在使用協助程式類別擷取相片和視訊時處理裝置方向。
title: 使用 MediaCapture 處理裝置方向
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0e49bd31ff005799e1c3fc7f0bc44be8bd7c216c
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318420"
---
# <a name="handle-device-orientation-with-mediacapture"></a>使用 MediaCapture 處理裝置方向
當您的 app 擷取要在 app 以外檢視的相片或影片時 (例如，儲存到使用者裝置上的檔案或線上分享)，請務必使用正確的方向中繼資料來為影像編碼，如此一來，當其他 app 或裝置顯示該影像時，就能以正確方向顯示它。 判斷要在媒體檔案中包含的正確方向資料是一項複雜的工作，因為有數個變數需要考量，包括裝置底座的方向、顯示器的方向，以及相機在底座上的位置 (其為前方或後方相機)。 

為了簡化處理方向的程序，我們建議使用協助程式類別 **CameraRotationHelper**，本文結尾將提供此類別的完整定義。 您可以將這個類別新增到專案，然後依照本文的步驟來將方向支援新增到您的相機 app。 此協助程式類別也可讓您更容易在相機 UI 中旋轉控制項，因此，從使用者觀點來看時就能正確呈現它們。

> [!NOTE] 
> 本文是以[**使用 MediaCapture 進行基本相片、視訊和音訊的擷取**](basic-photo-video-and-audio-capture-with-mediacapture.md)一文中所討論的程式碼與概念為基礎。 我們建議您先熟悉使用 **MediaCapture** 的基本概念，然後再將方向支援新增到 app。

## <a name="namespaces-used-in-this-article"></a>本文中使用的命名空間
本文中的範例程式碼會使用來自下列命名空間的 API，您應該在程式碼中包含這些命名空間。 

[!code-cs[OrientationUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOrientationUsing)]

將方向支援新增到 app 的第一個步驟是鎖定顯示器，讓它不會在裝置旋轉時自動旋轉。 對於大多數類型的 app 而言，自動 UI 旋轉都能運作順暢，但當相機預覽旋轉時，對使用者來說，並不是直接易懂的。 藉由將 [**DisplayInformation.AutoRotationPreferences**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.autorotationpreferences) 屬性設為 [**DisplayOrientations.Landscape**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations) 來鎖定顯示方向。 

[!code-cs[AutoRotationPreference](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAutoRotationPreference)]

## <a name="tracking-the-camera-device-location"></a>追蹤相機裝置位置
若要計算所擷取媒體的正確方向，您的 app 必須判斷相機裝置在底座上的位置。 新增布林值成員變數，以追蹤相機是否為外接式裝置，例如 USB 網路攝影機。 新增另一個布林值變數，以追蹤是否應該對預覽進行鏡像處理，如果使用的是前方相機，即為此情況。 此外，新增用來儲存 **DeviceInformation** 物件的變數，此物件代表所選取的相機。

[!code-cs[CameraDeviceLocationBools](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCameraDeviceLocationBools)]
[!code-cs[DeclareCameraDevice](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareCameraDevice)]

## <a name="select-a-camera-device-and-initialize-the-mediacapture-object"></a>選取相機裝置並初始化 MediaCapture 物件
[  **使用 MediaCapture 進行基本相片、視訊和音訊的擷取**](basic-photo-video-and-audio-capture-with-mediacapture.md)一文示範如何只利用幾行程式碼來初始化 **MediaCapture** 物件。 為了支援相機方向，我們將在初始化程序中新增數個步驟。

首先，呼叫 [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)，其會傳入裝置選取器 [**DeviceClass.VideoCapture**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceClass)，以取得所有可用視訊擷取裝置的清單。 接下來，選取清單中的第一個裝置，其中已知相機的面板位置，而且它符合所提供的值，此範例中的相機為前方相機。 如果在所需的面板上找不到相機，即會使用第一台或預設可用的相機。

如果找到相機裝置，即會建立新的 [**MediaCaptureInitializationSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings) 物件，並將 [**VideoDeviceId**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videodeviceid) 屬性設為所選取的裝置。 接下來，建立 MediaCapture 物件並呼叫 [**InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync)，其會傳入設定物件以告知系統使用所選取的相機。

最後，檢查所選取的裝置面板是否為 null 或未知。 若是如此，則相機為外接式裝置，這表示它的旋轉與裝置旋轉無關。 如果面板是已知且位於裝置底座正面，我們就知道應該要對預覽進行鏡像處理，因此，會設定追蹤此情況的變數。

[!code-cs[InitMediaCaptureWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCaptureWithOrientation)]
## <a name="initialize-the-camerarotationhelper-class"></a>初始化 CameraRotationHelper 類別

現在，我們開始使用 **CameraRotationHelper** 類別。 宣告類別成員變數來儲存物件。 呼叫建構函式，其會傳入所選取相機的機殼位置。 協助程式類別會使用這項資訊來計算擷取媒體、預覽資料流和 UI 的正確方向。 為協助程式類別的 **OrientationChanged** 事件登錄處理常式，它將會在我們需要更新 UI 或預覽資料流的方向時引發。

[!code-cs[DeclareRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareRotationHelper)]

[!code-cs[InitRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitRotationHelper)]

## <a name="add-orientation-data-to-the-camera-preview-stream"></a>在相機預覽資料流中新增方向資料
將正確的方向新增到預覽資料流的中繼資料，不會影響為使用者顯示預覽的方式，也能協助系統為擷取自預覽資料流的畫面進行正確編碼。

您可以藉由呼叫 [**MediaCapture.StartPreviewAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.startpreviewasync) 來啟動相機預覽。 執行此動作之前，請檢查成員變數，以查看是否應該對預覽進行鏡像處理 (適用於前方相機)。 如果是，將 [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) 的 [**FlowDirection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.flowdirection) 屬性 (在這個範例中，名稱為 *PreviewControl*) 設為 [**FlowDirection.RightToLeft**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FlowDirection)。 啟動預覽之後，呼叫協助程式方法 **SetPreviewRotationAsync** 來設定預覽旋轉。 以下是此方法的實作。

[!code-cs[StartPreviewWithRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewWithRotationAsync)]

我們會在個別方法中設定預覽旋轉，讓它可以在手機旋轉時更新，而不需重新初始化預覽資料流。 如果相機是外接式裝置，就不會採取任何動作。 否則會呼叫 **CameraRotationHelper** 方法 **GetCameraPreviewOrientation**，並傳回預覽資料流的適當方向。 

若要設定中繼資料，藉由呼叫 [**VideoDeviceController.GetMediaStreamProperties**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties) 來擷取預覽資料流屬性。 接著，建立代表視訊資料流旋轉的媒體基礎轉換 (MFT) 屬性的 GUID。 在 C++ 中，您可以使用常數 [**MF_MT_VIDEO_ROTATION**](https://docs.microsoft.com/windows/desktop/medfound/mf-mt-video-rotation)，但在 C# 中，您必須手動指定 GUID 值。 

將屬性值新增到資料流屬性物件，指定 GUID 做為機碼並指定預覽旋轉做為值。 這個屬性預期值是以逆時針旋轉度數的單位來表示，因此可使用 **CameraRotationHelper** 方法 **ConvertSimpleOrientationToClockwiseDegrees** 來轉換簡單的方向值。 最後，呼叫 [**SetEncodingPropertiesAsync**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture#Windows_Media_Capture_MediaCapture_SetEncodingPropertiesAsync_Windows_Media_Capture_MediaStreamType_Windows_Media_MediaProperties_IMediaEncodingProperties_Windows_Media_MediaProperties_MediaPropertySet_)，將新的旋轉屬性套用到資料流。

[!code-cs[SetPreviewRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSetPreviewRotationAsync)]

接下來，新增適用於 **CameraRotationHelper.OrientationChanged** 事件的處理常式。 此事件會傳入引數，可讓您知道是否需要旋轉預覽資料流。 如果裝置的方向已變更為朝上或朝下，則這個值將會是 false。 如果需要預覽旋轉，請呼叫 **SetPreviewRotationAsync** (已於先前定義)。

接著，在 **OrientationChanged** 事件處理常式中，視需要更新您的 UI。 藉由呼叫 **GetUIOrientation**，來取得協助程式類別目前建議的 UI 方向，並將值轉換為順時針旋轉度數，這適用於 XAML 轉換。 從方向值建立 [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform)，並設定 XAML 控制項的 [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) 屬性。 根據您的 UI 配置，除了簡單旋轉控制項之外，您可能還需要在此處進行額外調整。 此外請記住，UI 的所有更新都必須在 UI 執行緒上進行，因此，您應該將這個程式碼放置於 [**RunAsync**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher#Windows_UI_Core_CoreDispatcher_RunAsync_Windows_UI_Core_CoreDispatcherPriority_Windows_UI_Core_DispatchedHandler_) 的呼叫內。  

[!code-cs[HelperOrientationChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetHelperOrientationChanged)]

## <a name="capture-a-photo-with-orientation-data"></a>使用方向資料擷取相片
[  **使用 MediaCapture 進行基本相片、視訊和音訊的擷取**](basic-photo-video-and-audio-capture-with-mediacapture.md)一文示範如何將相片擷取到檔案，方法是先擷取到記憶體內部的資料流，然後使用解碼器讀取來自資料流的影像資料，並使用編碼器來將影像資料轉碼為檔案。 取自 **CameraRotationHelper** 類別的方向資料，可以在轉碼作業期間新增到影像檔案。

在下列範例中，會呼叫 [**CapturePhotoToStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.capturephototostreamasync) 來將相片擷取到 [**InMemoryRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream)，並從資料流中建立 [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder)。 接下來，建立並開啟 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 以擷取 [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream) 來寫入檔案。 

進行檔案轉碼之前，會從協助程式類別方法 **GetCameraCaptureOrientation** 擷取相片的方向。 這個方法會傳回 [**SimpleOrientation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.SimpleOrientation) 物件，其會利用協助程式方法 **ConvertSimpleOrientationToPhotoOrientation** 轉換為 [**PhotoOrientation**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.PhotoOrientation) 物件。 接著，建立新的 **BitmapPropertySet** 物件，以及新增機碼為 "System.Photo.Orientation" 且值為相片方向的屬性 (以 **BitmapTypedValue** 表示)。 "System.Photo.Orientation" 可以中繼資料形式新增到影像檔案的數個 Windows 屬性之一。 如需所有相片相關的屬性清單，請參閱 [**Windows 屬性 - 相片**](https://docs.microsoft.com/previous-versions/ff516600(v=vs.85))。 如需在影像中使用中繼資料的詳細資訊，請參閱[**影像中繼資料**](image-metadata.md)。

最後，藉由呼叫 [**SetPropertiesAsync**](https://docs.microsoft.com/windows/uwp/develop/) 來為編碼器設定包含方向資料的屬性集，並呼叫 [**FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) 來為影像轉碼。

[!code-cs[CapturePhotoWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCapturePhotoWithOrientation)]

## <a name="capture-a-video-with-orientation-data"></a>使用方向資料擷取視訊
基本的視訊擷取說明請參閱[**使用 MediaCapture 進行基本相片、視訊和音訊的擷取**](basic-photo-video-and-audio-capture-with-mediacapture.md)。 將方向資料新增到所擷取視訊的編碼，是使用先前將方向資料新增到預覽資料流的相關章節中所述的相同方式來完成。

在下列範例中，會建立一個檔案來寫入所擷取的視訊。 使用靜態方法 [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) 來建立 MP4 編碼設定檔。 藉由呼叫 **GetCameraCaptureOrientation**，從 **CameraRotationHelper** 類別取得視訊的正確方向。由於旋轉屬性需要以逆時針旋轉度數表示方向，因此，會呼叫 **ConvertSimpleOrientationToClockwiseDegrees** 協助程式方法來轉換方向值。 接著，建立代表視訊資料流旋轉的媒體基礎轉換 (MFT) 屬性的 GUID。 在 C++ 中，您可以使用常數 [**MF_MT_VIDEO_ROTATION**](https://docs.microsoft.com/windows/desktop/medfound/mf-mt-video-rotation)，但在 C# 中，您必須手動指定 GUID 值。 將屬性值新增到資料流屬性物件，指定 GUID 做為機碼並指定旋轉做為值。 最後會呼叫 [**StartRecordToStorageFileAsync**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture#Windows_Media_Capture_MediaCapture_StartRecordToStorageFileAsync_Windows_Media_MediaProperties_MediaEncodingProfile_Windows_Storage_IStorageFile_)，開始利用方向資料來為錄製的視訊編碼。

[!code-cs[StartRecordingWithOrientationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartRecordingWithOrientationAsync)]

## <a name="camerarotationhelper-full-code-listing"></a>CameraRotationHelper 完整的程式碼清單
下列程式碼片段會列出 **CameraRotationHelper** 類別的完整程式碼，此類別會管理硬體方向感應器、為相片和視訊計算適當的方向值，並提供協助程式方法在不同 Windows 功能所使用的不同方向表示法之間進行切換。 如果您遵循上述文章中所示的指導方針，就可以將這個類別依原樣新增到您的專案，而不需進行任何變更。 當然，您可以隨意地自訂下列程式碼，以符合您的特定案例需求。

這個協助程式類別會使用裝置的 [**SimpleOrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.SimpleOrientationSensor) 來判斷裝置底座目前的方向，以及使用 [**DisplayInformation**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayInformation) 類別來判斷顯示器目前的方向。 這其中每一個類別都提供會在目前方向變更時引發的事件。 掛接擷取裝置的面板 (前方、後方或外接式) 可用來判斷是否應該對預覽資料流進行鏡像處理。 此外，部分裝置支援的 [**EnclosureLocation.RotationAngleInDegreesClockwise**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.enclosurelocation.rotationangleindegreesclockwise) 屬性，可用來判斷在底座上掛接相機的方向。

下列方法可用來取得針對指定相機 app 工作所建議的方向值︰

* **GetUIOrientation** - 傳回相機 UI 元素的建議方向。
* **GetCameraCaptureOrientation** - 傳回用來編碼至影像中繼資料的建議方向。
* **GetCameraPreviewOrientation** - 傳回預覽資料流的建議方向，以提供更自然的使用者體驗。

[!code-cs[CameraRotationHelperFull](./code/SimpleCameraPreview_Win10/cs/CameraRotationHelper.cs#SnippetCameraRotationHelperFull)]



## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [MediaCapture 擷取基本的相片、 視訊和音訊](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




