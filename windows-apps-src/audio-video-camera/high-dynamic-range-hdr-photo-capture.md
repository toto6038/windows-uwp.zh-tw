---
author: drewbatgit
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: 本文示範如何使用 AdvancedPhotoCapture 類別，來擷取高動態範圍 (HDR) 相片和弱光相片。
title: 高動態範圍 (HDR) 和弱光相片擷取
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ffdba499d8e38bb9248071daeddd850f921e931e
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843358"
---
# <a name="high-dynamic-range-hdr-and-low-light-photo-capture"></a>高動態範圍 (HDR) 和弱光相片擷取



本文示範如何使用 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 類別，來擷取高動態範圍 (HDR) 相片。 這個 API 也可讓您在最終影像處理完成之前，從 HDR 擷取中取得參照畫面。

關於 HDR 擷取的其他文件包括：

-   您可以使用 [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902)，讓系統評估媒體擷取預覽資料流的內容，以判斷 HDR 處理是否會改善擷取結果。 如需詳細資訊，請參閱[媒體擷取的場景分析](scene-analysis-for-media-capture.md)。

-   使用 [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680)，以使用 Windows 內建 HDR 處理演算法來擷取視訊。 如需詳細資訊，請參閱[視訊擷取的擷取裝置控制項](capture-device-controls-for-video-capture.md)。

-   您可以使用 [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564) 擷取一連串相片，每張相片各有不同的擷取設定，並實作您自己的 HDR 或其他處理演算法。 如需詳細資訊，請參閱[可變相片序列](variable-photo-sequence.md)。



> [!NOTE] 
> 從 Windows 10 版本 1709 開始，支援同時錄製視訊和使用 **AdvancedPhotoCapture**。  先前版本不支援此功能。 這項變更表示您可以同時準備好 **[LowLagMediaRecording](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording)** 和 **[AdvancedPhotoCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture)**。 您可以在呼叫 **[MediaCapture.PrepareAdvancedPhotoCaptureAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)** 和 **[AdvancedPhotoCapture.FinishAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.FinishAsync)** 之間開始或停止錄影。 您也可以在錄製視訊時呼叫 **[AdvancedPhotoCapture.CaptureAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.CaptureAsync)**。 不過，有些 **AdvancedPhotoCapture** 案例 (例如在錄製視訊時擷取 HDR 相片) 會導致某些視訊畫面遭到 HDR 擷取修改，這會導致負面使用者經驗。 基於這個原因，錄製視訊時 **[AdvancedPhotoControl.SupportedModes](https://docs.microsoft.com/uwp/api/windows.media.devices.advancedphotocontrol.SupportedModes)** 傳回的模式清單將會不同。 您應該在開始或停止視訊錄製後立即檢查此值，以確定目前的視訊錄製狀態支援您想要的模式。


> [!NOTE] 
> 從 Windows 10 版本 1709 開始，當 **AdvancedPhotoCapture** 設定為 HDR 模式時，會忽略 [**FlashControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Enabled) 屬性的設定且永不引發閃光燈。 對於其他擷取模式，如果是 **FlashControl.Enabled**，則將會覆寫 **AdvancedPhotoCapture** 設定，並導致系統使用閃光燈拍攝一般相片。 如果 [**Auto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Auto) 設定為 true，**AdvancedPhotoCapture** 可能會也可能不會使用閃光燈，取決於相機驅動程式對目前場景中條件的預設行為。 在先前版本中，**AdvancedPhotoCapture** 閃光燈設定一律覆寫 **FlashControl.Enabled** 設定。

> [!NOTE] 
> 本文是以[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)中討論的概念和程式碼為基礎，其中說明實作基本相片和視訊擷取的步驟。 我們建議您先熟悉該文章中的基本媒體擷取模式，然後再移到更多進階的擷取案例。 本文中的程式碼假設您的 app 已有正確初始化的 MediaCapture 執行個體。

有一個通用 Windows 範例可示範如何使用 **AdvancedPhotoCapture** 類別，您可以使用此類別來查看內容中使用的 API，或做為您 app 的起點。 如需詳細資訊，請參閱[相機進階擷取範例](http://go.microsoft.com/fwlink/?LinkID=620517)。

## <a name="advanced-photo-capture-namespaces"></a>進階相片擷取命名空間

本文中的程式碼範例除了基本媒體擷取所需的命名空間之外，還會在下列命名空間中使用 API。

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]

## <a name="hdr-photo-capture"></a>HDR 相片擷取

### <a name="determine-if-hdr-photo-capture-is-supported-on-the-current-device"></a>判斷目前的裝置是否支援 HDR 相片擷取

本文所述的 HDR 擷取技術是使用 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 物件來執行。 並非所有裝置都支援使用 **AdvancedPhotoCapture** 執行 HDR 擷取。 藉由取得 **MediaCapture** 物件的 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)，接著取得 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 屬性，以判斷目前正在執行您 app 的裝置是否支援這項技術。 檢查視訊裝置控制器的 [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) 集合以查看是否包含 [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845)。若是，則支援使用 **AdvancedPhotoCapture** 的 HDR 擷取。

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

### <a name="configure-and-prepare-the-advancedphotocapture-object"></a>設定和準備 AdvancedPhotoCapture 物件

因為您需要從程式碼中的多個位置存取 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 執行個體，所以您應該宣告成員變數來保存此物件。

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

在您的應用程式中，在您初始化 **MediaCapture** 物件後，建立 [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) 物件並將模式設定為 [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845)。呼叫 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 物件的 [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) 方法，傳遞至您所建立的 **AdvancedPhotoCaptureSettings** 物件。

呼叫 **MediaCapture** 物件的 [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403)，並傳入 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 物件來指定擷取應該使用的編碼類型。 **ImageEncodingProperties** 類別提供靜態方法來建立 **MediaCapture** 支援的影像編碼。

**PrepareAdvancedPhotoCaptureAsync** 會傳回 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 物件，讓您用來起始相片擷取。 您可以使用此物件來註冊 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 和 [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) 的處理常式，本文稍後會討論。

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

### <a name="capture-an-hdr-photo"></a>擷取 HDR 相片

呼叫 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 物件的 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) 方法，以擷取 HDR 相片。 這個方法會傳回 [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378) 物件，而其 [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382) 屬性中會提供已擷取的相片。

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

大部分的攝影 app 會想要將所擷取相片的旋轉編碼為影像檔，讓其他 app 和裝置可正確地顯示該影像檔。 這個範例示範如何使用協助程式類別 **CameraRotationHelper** 來計算檔案的正確方向。 [**使用 MediaCapture 處理裝置方向**](handle-device-orientation-with-mediacapture.md)一文中會完整說明並列出此類別。

本文稍後會討論可將影像儲存到磁碟的 **SaveCapturedFrameAsync** 協助程式方法。

### <a name="get-optional-reference-frame"></a>取得選用的參照畫面

HDR 程序會擷取多個框架，然後在擷取所有框架之後，組合成單一影像。 在整個 HDR 程序完成之前，您可以透過處理 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 事件來存取已擷取的框架。 如果您只想要取得最終的 HDR 相片結果，則不需要這樣做。

> [!IMPORTANT]
> 在支援硬體 HDR 的裝置上，不會引發 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392)，因此不會產生參照框架。 您的 app 應該處理不會引發這個事件的情況。

因為送達的參照畫面與呼叫 **CaptureAsync** 無關，因此會提供一項機制以將內容資訊傳遞給 **OptionalReferencePhotoCaptured** 處理常式。 首先，您應該呼叫將包含內容資訊的物件。 這個物件的名稱和內容由您決定。 這個範例定義一個物件，其中有成員可追蹤擷取的檔案名稱和相機方向。

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

建立內容物件的新執行個體，並填入它的成員，然後傳遞給接受物件做為參數的 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) 的多載。

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

在 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 事件處理常式中，將 [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404) 物件的 [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405) 屬性轉型為您的內容物件類別。 這個範例會修改檔案名稱來區分參照畫面影像和最終 HDR 影像，然後呼叫 **SaveCapturedFrameAsync** 協助程式方法來儲存影像。

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

### <a name="receive-a-notification-when-all-frames-have-been-captured"></a>在所有框架都被擷取之後收到通知

HDR 相片擷取有兩個步驟。 首先，擷取多個框架，然後框架經過處理成為最終 HDR 影像。 當仍在擷取來源 HDR 框架時，您無法起始另一個擷取，但在所有框架都已擷取之後到 HDR 後續處理完成之前，您可以起始擷取。 HDR 擷取完成時會引發 [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) 事件，讓您知道可以起始另一個擷取。 通常是在 HDR 擷取開始時停用 UI 的擷取按鈕，然後在引發 **AllPhotosCaptured** 時重新啟用該按鈕。

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

### <a name="clean-up-the-advancedphotocapture-object"></a>清除 AdvancedPhotoCapture 物件

當 app 完成擷取時，在處置 **MediaCapture** 物件之前，您應該呼叫 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391) 並將成員變數設定為 Null，以關閉 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 物件。

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]


## <a name="low-light-photo-capture"></a>弱光相片擷取
從 Windows 10 版本 1607 開始，可以使用 **AdvancedPhotoCapture**，利用內建演算法來擷取相片，以增強弱光設定中所擷取的相片品質。 當您使用 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture) 類別的弱光功能時，系統將會評估目前的場景，並視需要套用演算法來補償弱光的情況。 如果系統判斷不需演算法，即會改為執行一般擷取。

使用弱光相片擷取之前，藉由取得 **MediaCapture** 物件的 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)，接著取得 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 屬性，以判斷目前正在執行您 app 的裝置是否支援這項技術。 檢查視訊裝置控制器的 [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) 集合，以查看其中是否包含 [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845)。 如果是，則支援使用 **AdvancedPhotoCapture** 執行弱光擷取。 
[!code-cs[LowLightSupported1](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported1)]

[!code-cs[LowLightSupported2](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported2)]

接下來，宣告成員變數來儲存 **AdvancedPhotoCapture** 物件。 

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

在您的 app 中，於初始化 **MediaCapture** 物件之後，建立 [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) 物件，並將模式設定為 [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845)。 呼叫 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 物件的 [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) 方法，並傳入您建立的 **AdvancedPhotoCaptureSettings** 物件。

呼叫 **MediaCapture** 物件的 [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403)，並傳入 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 物件來指定擷取應該使用的編碼類型。 

[!code-cs[CreateAdvancedCaptureLowLightAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureLowLightAsync)]

若要擷取相片，請呼叫 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync)。

[!code-cs[CaptureLowLight](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureLowLight)]

如上述的 HDR 範例，這個範例使用稱為 **CameraRotationHelper** 的協助程式類別，來判斷應該編碼到影像中的旋轉值，讓其他 app 和裝置可正常顯示。 [**使用 MediaCapture 處理裝置方向**](handle-device-orientation-with-mediacapture.md)一文中會完整說明並列出此類別。

本文稍後會討論可將影像儲存到磁碟的 **SaveCapturedFrameAsync** 協助程式方法。

您可以擷取多張弱光相片，而不需重新設定 **AdvancedPhotoCapture** 物件，但當您完成擷取時，您應該呼叫 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync) 來清理物件及相關聯的資源。

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## <a name="working-with-advancedcapturedphoto-objects"></a>使用 AdvancedCapturedPhoto 物件
[**AdvancedPhotoCapture.CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync) 會傳回 [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto) 物件，代表擷取的相片。 這個物件會公開 [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto.Frame) 屬性，其傳回代表影像的 [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame) 物件。 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.OptionalReferencePhotoCaptured) 事件也會在其事件引數中提供 **CapturedFrame** 物件。 取得這個類型的物件之後，有許多您可以使用該物件執行的動作，包括建立 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) 或將影像儲存至檔案。 

## <a name="get-a-softwarebitmap-from-a-capturedframe"></a>從 CapturedFrame 取得 SoftwareBitmap
從 **CapturedFrame** 物件取得 **SoftwareBitmap** 非常簡單，只需存取物件的 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap) 屬性即可。 不過，大部分的編碼格式不支援 **SoftwareBitmap** 搭配 **AdvancedPhotoCapture**，因此您應該先檢查並確定該屬性不是 null，才能使用它。

[!code-cs[SoftwareBitmapFromCapturedFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapFromCapturedFrame)]

在目前版本中，唯一支援 **AdvancedPhotoCapture** 的 **SoftwareBitmap** 的編碼格式是未壓縮的 NV12。 因此，如果您想要使用這項功能，您必須在呼叫 [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403) 時指定該編碼。 

[!code-cs[UncompressedNv12](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUncompressedNv12)]

當然，您可以一律將影像儲存到檔案，然後在個別步驟中將檔案載入 **SoftwareBitmap**。 如需使用 **SoftwareBitmap** 的詳細資訊，請參閱[**建立、編輯和儲存點陣圖影像**](imaging.md)。

## <a name="save-a-capturedframe-to-a-file"></a>將 CapturedFrame 儲存到檔案
[**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame) 類別會實作 IInputStream 介面，讓它可用來做為 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder) 的輸入，然後可使用 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder) 來將影像資料寫入磁碟。

下列範例會在使用者的圖片媒體櫃中建立新資料夾，並在此資料夾中建立檔案。 請注意，您的 app 需要在應用程式資訊清單檔案中包含**圖片媒體櫃**功能，才能存取此目錄。 隨即會將檔案資料流開啟至指定的檔案。 接下來，從 **CapturedFrame** 呼叫 [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder.CreateAsync) 以建立解碼器。 然後 [**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/br226214) 會從檔案資料流和解碼器建立編碼器。

後續步驟會使用編碼器的 [**BitmapProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.BitmapProperties)，將相片的方向編碼到影像檔案。 如需在擷取影像時處理方向的詳細資訊，請參閱[**使用 MediaCapture 處理裝置方向**](handle-device-orientation-with-mediacapture.md)。

最後，呼叫 [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync) 來將影像寫入檔案。

[!code-cs[SaveCapturedFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSaveCapturedFrameAsync)]

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
