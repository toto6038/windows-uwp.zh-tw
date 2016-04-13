---
ms.assetid: 84729E44-10E9-4D7D-8575-6A9D97467ECD
description: 本主題說明如何使用 FaceDetector 來偵測影像中的臉部。 FaceTracker 已進行最佳化，可在一連串視訊框架中用來追蹤隨著時間改變的臉部。
title: 偵測影像或影片中的臉部
---

# 偵測影像或影片中的臉部

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[正式發行前可能會進行大幅度修改之預先發行的產品的一些相關資訊。 Microsoft 對此處提供的資訊，不提供任何明確或隱含的瑕疵擔保。\]

本主題說明如何使用 [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) 來偵測影像中的臉部。 [
            **FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) 已進行最佳化，可在一連串視訊框架中用來追蹤隨著時間改變的臉部。

如需使用 [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/dn948776) 追蹤臉部的替代方法，請參閱[媒體擷取的場景分析](scene-analysis-for-media-capture.md)。

本文中的程式碼是採用[基本臉部偵測](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409)和[基本臉部追蹤](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409)範例的程式碼。 您可以下載這些範例來查看內容中使用的程式碼，或是使用此範例做為自己 app 的起點。

## 偵測單一影像中的臉部

[
            **FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) 類別可讓您偵測靜物影像中的一或多個臉部。

這個範例會使用來自下列命名空間的 API。

[!code-cs[FaceDetectionUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

針對 [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) 物件以及將在影像中進行偵測的 [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) 物件清單來宣告類別成員變數。

[!code-cs[ClassVariables1](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables1)]

臉部偵測可在使用各種不同方式建立的 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 物件上運作。 這個範例使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)，讓使用者能夠挑選將在其中偵測臉部的影像檔。 如需使用軟體點陣圖的詳細資訊，請參閱[影像處理](imaging.md)。

[!code-cs[Picker](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetPicker)]

使用 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 類別，將影像檔解碼到 **SoftwareBitmap**。 利用較小的影像可讓臉部偵測程序更快速，因此，您可能會想縮小來源影像的大小。 您可以在解碼期間執行此動作，方法是建立 [**BitmapTransform**](https://msdn.microsoft.com/library/windows/apps/br226254) 物件、設定 [**ScaledWidth**](https://msdn.microsoft.com/library/windows/apps/br226261) 和 [**ScaledHeight**](https://msdn.microsoft.com/library/windows/apps/br226260) 屬性並將它傳送到對 [**GetSoftwareBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn887332) 的呼叫，這會傳回已解碼且已調整大小的 **SoftwareBitmap**。

[!code-cs[Decode](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDecode)]

在目前版本中，**FaceDetector** 類別僅支援 Gray8 或 Nv12 的影像。 **SoftwareBitmap** 類別提供 [**Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) 方法，可將點陣圖從某一種格式轉換成其他格式。 這個範例會將來源影像轉換為 Gray8 像素格式 (如果還不是這種格式)。 如有需要，您可以使用 [**GetSupportedBitmapPixelFormats**](https://msdn.microsoft.com/library/windows/apps/dn974140) 和 [**IsBitmapPixelFormatSupported**](https://msdn.microsoft.com/library/windows/apps/dn974142) 方法，在執行階段判斷是否支援某種像素格式 (假設將在未來版本中擴充支援的格式組合)。

[!code-cs[Format](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFormat)]

將 **FaceDetector** 物件具現化，方法是呼叫 [**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974132)，然後呼叫 [**DetectFacesAsync**](https://msdn.microsoft.com/library/windows/apps/dn974134)，利用已將大小調整為合理大小且轉換成支援像素格式的點陣圖中進行傳遞。 這個方法會傳回 [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) 物件的清單。 **ShowDetectedFaces** 是一個協助程式方法 (如下所示)，會在影像中的臉部四周繪製方框。

[!code-cs[Detect](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDetect)]

確定會處置在臉部偵測程序期間建立的物件。

[!code-cs[Dispose](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDispose)]

若要顯示影像並在偵測到的臉部四周繪製方框，可在您的 XAML 頁面中新增 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 元素。

[!code-xml[Canvas](./code/FaceDetection_Win10/cs/MainPage.xaml#SnippetCanvas)]

定義一些成員變數來設定將繪製的方框樣式。

[!code-cs[ClassVariables2](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables2)]

在 **ShowDetectedFaces** 協助程式方法中會建立新的 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101)，並將來源設為從代表來源影像的 **SoftwareBitmap** 中建立的 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854)。 XAML **Canvas** 控制項的背景已設定為影像筆刷。

如果傳遞到協助程式方法的臉部清單不是空的，請重複執行清單中的每一個臉部，並使用 [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) 類別的 [**FaceBox**](https://msdn.microsoft.com/library/windows/apps/dn974126) 屬性來判斷影像中包含臉部的方框大小與位置。 因為 **Canvas** 控制項的大小很可能與來源影像的大小不同，所以，您應該將 X 和 Y 座標以及 **FaceBox** 的寬度和高度乘以縮放比例值，此值是來源影像大小與 **Canvas** 控制項實際大小的比例。

[!code-cs[ShowDetectedFaces](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetShowDetectedFaces)]

## 追蹤一系列畫面中的臉部

如果您想要偵測影片中的臉部，比起 [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) 類別，更有效率的方式是使用 [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) 類別 (儘管這兩者的實作步驟非常類似)。 **FaceTracker** 會使用先前處理過的畫面相關資訊來將偵測程序最佳化。

[!code-cs[FaceTrackingUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceTrackingUsing)]

宣告 **FaceTracker** 物件的類別變數。 這個範例會使用 [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587)，以定義的時間間隔來初始臉部追蹤。 [SemaphoreSlim](https://msdn.microsoft.com/library/system.threading.semaphoreslim.aspx) 可用來確定一次只會執行一個臉部追蹤作業。

[!code-cs[ClassVariables3](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables3)]

若要初始臉部追蹤作業，請呼叫 [**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974151) 來建立新的 **FaceTracker** 物件。 初始所需的時間間隔，然後建立計時器。 每次超過指定的間隔時間，就會呼叫 **ProcessCurrentVideoFrame** 協助程式方法。

[!code-cs[TrackingInit](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetTrackingInit)]

計時器會以非同步方式來呼叫 **ProcessCurrentVideoFrame** 協助程式，因此，此方法會先呼叫旗號的 **Wait** 方法，來查看追蹤作業是否正在進行中，如果正在進行，方法即會回傳而不需嘗試偵測臉部。 在這個方法結束時，會呼叫旗號的 **Release** 方法，這樣就能對 **ProcessCurrentVideoFrame** 進行後續呼叫來繼續。

[
            **FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) 類別會在 [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) 物件上運作。 有多種方式您可以用來取得 **VideoFrame**，包括從執行中的 [MediaCapture](capture-photos-and-video-with-mediacapture.md) 物件擷取預覽框架，或者透過實作 [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788) 的 [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) 方法 。 這個範例使用未定義的協助程式方法，此方法會傳回視訊畫面 **GetLatestFrame**，以做為這個作業的預留位置。 如需從執行中的媒體擷取裝置的預覽資料流中取得視訊畫面的相關資訊，請參閱[取得預覽畫面](get-a-preview-frame.md)。

如同 **FaceDetector**，**FaceTracker** 支援一組有限的像素格式。 這個範例會在提供的畫面格式不是 Nv12 格式時放棄臉部偵測。

呼叫 [**ProcessNextFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn974157) 來抓取 [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) 物件清單，這類物件代表框架中的臉部。 一旦具備臉部清單之後，就可以使用上述針對臉部偵測所說明的相同方式來顯示它們。 請注意，由於臉部追蹤協助程式方法不會在 UI 執行緒上呼叫，因此，您必須在呼叫 [**CoredDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 中進行任何 UI 更新。

[!code-cs[ProcessCurrentVideoFrame](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetProcessCurrentVideoFrame)]

## 相關主題

* [媒體擷取的場景分析](scene-analysis-for-media-capture.md)
* [基本臉部偵測範例](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409)
* [基本臉部追蹤範例](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409)
* [使用 MediaCapture 擷取相片和視訊](capture-photos-and-video-with-mediacapture.md)


<!--HONumber=Mar16_HO1-->


