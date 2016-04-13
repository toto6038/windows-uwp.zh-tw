---
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: AdvancedPhotoCapture 類別可讓您擷取高動態範圍 (HDR) 相片。
title: 高動態範圍 (HDR) 相片擷取
---

# 高動態範圍 (HDR) 相片擷取

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


[
            **AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 類別可讓您擷取高動態範圍 (HDR) 相片。 這個 API 也可讓您在最終影像處理完成之前，從 HDR 擷取中取得參照框架。

關於 HDR 擷取的其他文件包括：

-   您可以使用 [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) 讓系統評估媒體擷取預覽資料流的內容，以判斷 HDR 處理是否會改善擷取結果。 如需詳細資訊，請參閱[媒體擷取的場景分析](scene-analysis-for-media-capture.md)。

-   使用 [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680)，以使用 Windows 內建 HDR 處理演算法來擷取影片。 如需詳細資訊，請參閱[視訊擷取的擷取裝置控制項](capture-device-controls-for-video-capture.md)。

-   您可以使用 [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564) 擷取一連串相片，每張相片各有不同的擷取設定，並實作您自己的 HDR 或其他處理演算法。 如需詳細資訊，請參閱[可變相片序列](variable-photo-sequence.md)。

**注意**
-   不支援使用 **AdvancedPhotoCapture** 同時進行錄製影片與相片擷取。

-   不支援在進階相片擷取期間使用相機閃光燈。

**注意** 本文是以[使用 MediaCapture 擷取相片和視訊](capture-photos-and-video-with-mediacapture.md)中討論的概念和程式碼為基礎，其中說明實作基本相片和視訊擷取的步驟。 建議您先熟悉該文中的基本媒體擷取模式，然後再移到更多進階的擷取案例。 本文章中的程式碼假設您的 app 已有正確初始化的 MediaCapture 執行個體。

## HDR 相片擷取命名空間

若要使用 HDR 相片擷取，除了基本媒體擷取所需的命名空間，您的 app 還必須包含下列命名空間。

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]


## 判斷目前的裝置是否支援 HDR 相片擷取

本文所述的 HDR 擷取技術是使用 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 物件來執行。 並非所有裝置都支援使用 **AdvancedPhotoCapture** 執行 HDR 擷取。 藉由取得 **MediaCapture** 物件的 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)，接著取得 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 屬性，以判斷目前正在執行您 app 的裝置是否支援這項技術。 檢查視訊裝置控制器的 [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) 集合是否包含 [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845)。 如果是的話，則支援使用 **AdvancedPhotoCapture** 執行 HDR 擷取。

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

## 設定和準備 AdvancedPhotoCapture 物件

因為您需要從程式碼中的多個位置存取 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 執行個體，所以您應該宣告成員變數來保存此物件。

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

在您的 app 中，於初始化 **MediaCapture** 物件之後，建立 [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) 物件，並將模式設定為 [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845)。 呼叫 [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) 物件的 [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) 方法，並傳入您建立的 **AdvancedPhotoCaptureSettings** 物件。

呼叫 **MediaCapture** 物件的 [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403)，並傳入 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 物件來指定擷取應該使用的編碼類型。 **ImageEncodingProperties** 類別提供靜態方法來建立 **MediaCapture** 支援的影像編碼。

**PrepareAdvancedPhotoCaptureAsync** 會傳回 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 物件，讓您用來起始相片擷取。 您可以使用此物件來註冊 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 和 [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) 的處理常式，本文稍後會討論。

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

## 擷取 HDR 相片

呼叫 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 物件的 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) 方法，以擷取 HDR 相片。 這個方法會傳回 [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378) 物件，而其 [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382) 屬性中會提供已擷取的相片。

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

**ConvertOrientationToPhotoOrientation** 和 **ReencodeAndSavePhotoAsync** 是[使用 MediaCapture 擷取相片和視訊](capture-photos-and-video-with-mediacapture.md)文章中的基本媒體擷取案例所論及的協助程式方法。

## 取得選用的參照框架

HDR 程序會擷取多個框架，然後在擷取所有框架之後，組合成單一影像。 在整個 HDR 程序完成之前，您可以透過處理 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 事件來存取已擷取的框架。 如果您只想要取得最終的 HDR 相片結果，則不需要這樣做。

**重要** 在支援硬體 HDR 的裝置上，由於不會引發 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392)，因此不會產生參照框架。 您的 app 應該處理不會引發這個事件的情況。

因為送達的參照框架與呼叫 **CaptureAsync** 無關，因此會提供一項機制以將內容資訊傳遞給 **OptionalReferencePhotoCaptured** 處理常式。 首先，您應該有一個包含內容資訊的物件。 這個物件的名稱和內容由您決定。 這個範例定義一個物件，其中有成員可追蹤擷取的檔案名稱和相機方向。

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

建立內容物件的新執行個體，並填入它的成員，然後傳遞給接受物件做為參數的 [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) 的多載。

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

在 [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) 事件處理常式中，將 [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404) 物件的 [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405) 屬性轉型為您的內容物件類別。 這個範例會修改檔案名稱來區分參照框架影像和最終 HDR 影像，然後呼叫 **ReencodeAndSavePhotoAsync** 協助程式方法以儲存影像。

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

## 在所有框架都被擷取之後收到通知

HDR 相片擷取有兩個步驟。 首先，擷取多個框架，然後框架經過處理成為最終 HDR 影像。 當仍在擷取來源 HDR 框架時，您無法起始另一個擷取，但在所有框架都已擷取之後到 HDR 後續處理完成之前，您可以起始擷取。 HDR 擷取完成時會引發 [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) 事件，讓您知道可以起始另一個擷取。 通常是在 HDR 擷取開始時停用 UI 的擷取按鈕，然後在引發 **AllPhotosCaptured** 時重新啟用該按鈕。

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

## 清除 AdvancedPhotoCapture 物件

當 app 完成擷取時，在處置 **MediaCapture** 物件之前，您應該呼叫 [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391) 並將成員變數設定為 Null，以關閉 [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) 物件。

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## 相關主題

* [使用 MediaCapture 擷取相片和視訊](capture-photos-and-video-with-mediacapture.md)

<!--HONumber=Mar16_HO1-->


