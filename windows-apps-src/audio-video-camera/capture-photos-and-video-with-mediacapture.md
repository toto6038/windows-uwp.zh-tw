---
author: drewbatgit
ms.assetid: 1361E82A-202F-40F7-9239-56F00DFCA54B
description: "本文描述使用 MediaCapture API 擷取相片和視訊的步驟，包括初始化和關閉 MediaCapture 以及處理裝置方向的變更。"
title: "使用 MediaCapture 擷取相片和視訊"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c20c735d38e6baabe2f8bc0c7c682706d3946ed9

---

# 使用 MediaCapture 擷取相片和視訊

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


此文章說明使用 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) API 擷取相片和視訊的步驟，包括初始化和關閉 **MediaCapture** 以及處理裝置方向的變更。

提供的 **MediaCapture** 支援需要低度控制媒體擷取處理程序的 app，以及可實作需要進階擷取功能之案例的 app。 使用 **MediaCapture** 也會要求您提供自己的擷取 UI。 如果您的 app 只需要擷取相片或視訊，而未涉及進階的擷取技術，[**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 可讓您輕鬆地只用幾行程式碼來擷取相片或視訊。 如需詳細資訊，請參閱[使用 CameraCaptureUI 擷取相片和視訊](capture-photos-and-video-with-cameracaptureui.md)。

此文章中的程式碼是從 [CameraStarterKit 範例](http://go.microsoft.com/fwlink/?LinkId=619479)改編而來。 您可以下載範例以查看內容中使用的程式碼，或以此範圍做為自己的 app 起點。

## 設定您的專案

### 將功能宣告加入至應用程式資訊清單

為了讓您的 app 可存取裝置的相機，您必須宣告您的 app 使用 *webcam* 和 *microphone* 裝置功能。 如果您要將拍攝的相片和影片儲存到使用者的圖片媒體櫃或視訊媒體櫃，您也必須宣告 *picturesLibrary* 和 *videosLibrary* 功能。

**將功能新增到 app 資訊清單**

1.  在 Microsoft Visual Studio 中，按兩下 [方案總管]**** 中的 **package.appxmanifest** 項目，開啟 app 資訊清單的設計工具。
2.  選取 [功能]**** 索引標籤。
3.  核取 [網路攝影機]**** 方塊和 [麥克風]**** 方塊。
4.  如果要存取圖片媒體櫃和視訊媒體櫃，請核取 [圖片媒體櫃]**** 方塊和 [視訊媒體櫃]**** 方塊。

### 新增媒體擷取相關 API 的 using 指示詞

下列程式碼清單顯示本文中的範例程式碼所參照的命名空間，以及描述每個命名空間可提供的功能。

[!code-cs[Using](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## 初始化 MediaCapture 物件

[**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738)命名空間中的 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 類別是所有媒體都擷取操作的基本介面。 App 通常會宣告範圍限於單一頁面的此類型變數。 您的 app 需要追蹤 **MediaCapture** 的目前狀態，所以您應該宣告可供初始化、預覽及記錄物件狀態的布林值變數。

[!code-cs[MediaCaptureVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaCaptureVariables)]

若要協助正確導向預覽視訊，請建立成員變數，以追蹤相機是否為外接式以及 app 目前是否在鏡像處理預覽資料流。 當您認為視訊輸入正在擷取使用者時，您的 app 應該鏡像處理預覽資料流。因為這是更為自然的使用者經驗。

[!code-cs[PreviewVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewVariables)]

下列範例方法會初始化媒體擷取物件。 首先，程式碼會搜尋可用於媒體擷取的視訊擷取裝置。 找到後，則會初始化 **MediaCapture** 物件並登錄其事件的處理常式。 接下來，會使用視訊擷取裝置的識別碼建立 [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/desktop/hh802710) 物件。 然後，呼叫[**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) 以初始化 **MediaCapture**。

[!code-cs[InitializeCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitializeCameraAsync)]

-   [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) 方法可用來尋找所有指定類型的裝置。 此範例會傳入 **DeviceClass.VideoCapture** 列舉值，表示只應傳回視訊擷取裝置。 請注意，視訊擷取裝置用於擷取相片和視訊。

-   **FindAllAsync** 會傳回 [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) 物件，其中包含每個找到之要求類型裝置的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) 物件。 **System.Linq** 命名空間中的 **FirstOrDefault** 延伸方法提供了簡單的語法，以便根據指定的條件選取清單中的項目。 第一次呼叫會嘗試選取清單中 [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) 值為 **Panel.Back** 的第一個 **DeviceInformation**，以表示相機位於裝置機殼的背面面板上。 如果裝置的背面面板上沒有相機，則會使用第一個可用的相機。

-   如果在初始化 [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573) 時並未指定裝置識別碼，系統將會選擇裝置的內部清單中的第一個裝置。

-   呼叫 [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) 可讓物件開始使用指定的擷取裝置。 此呼叫是在 **try** 區塊內部進行，因為如果使用者拒絕了對相機的呼叫 app 存取，它將會擲回 **UnauthorizedAccessException**。 如果呼叫成功，**\_isInitialized** 變數會設定為 true，以便後續方法呼叫判斷是否已初始化擷取裝置。

- **重要事項** 在某些裝置系列，在授與您的 app 存取裝置相機的權限之前，會先向使用者顯示使用者同意提示。 基於此因素，您必須只能從主要的 UI 執行緒呼叫 [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)。 嘗試從另一個執行緒初始化相機，可能導致初始化失敗。

-   如果擷取裝置的初始化成功，則會設定變數以反映擷取裝置是否為外接式，或是否位於裝置的前端面板上。 這些值將用來正確地為使用者導向擷取預覽。 最後會更新 UI，以反映擷取可用且來自擷取裝置的預覽資料流已啟動。 本文稍後會說明在協助程式方法中執行的工作。

## 啟動擷取預覽

為了讓使用者能夠查看他們所擷取的內容，您必須提供視訊擷取裝置目前在您 UI 中所見內容的預覽。

**重要：**您必須啟動擷取預覽，擷取裝置才能啟用自動對焦、自動曝光和自動白平衡。

提供的 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 控制項可啟用擷取預覽。 以下顯示的範例 XAML 程式碼會定義擷取元素。

[!code-xml[CaptureElement](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCaptureElement)]

使用者期待在預覽視訊擷取畫面時，畫面會保持開啟狀態，不會因為閒置而關閉。 若要達到此目的，您必須建立 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 物件。 使用頁面範圍宣告此變數，使其在擷取工作階段內持續存在。

[!code-cs[DisplayRequest](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayRequest)]

下列方法可啟動媒體擷取預覽。 首先，它會在 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 上呼叫 [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818)，要求顯示器維持使用中狀態。 接下來，呼叫 [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) 以啟動預覽。

[!code-cs[StartPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

-   在 **DisplayRequest** 物件上呼叫 [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) 方法，以要求系統讓畫面保留開啟狀態。

-   **CaptureElement** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 屬性會設定為 app 的 **MediaCapture** 物件，以定義預覽的來源。

-   [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) 屬性是由 XAML 架構提供，可支援雙向使用者介面。 將 **CaptureElement** 的流向設定為 [**FlowDirection.RightToLeft**](https://msdn.microsoft.com/library/windows/apps/br242397)，可使得預覽視訊水平翻轉。 這使用於擷取裝置位於裝置的前面板時，如此才能從使用者的角度觀看正確方向的預覽。

-   [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) 方法可在 **CaptureElement** 中開始顯示預覽資料流。 如果預覽啟動成功，**\_isPreviewing** 變數會設為允許 app 的其他部分得知 app 目前正在預覽，並會呼叫用於設定預覽旋轉的協助程式方法。 下一節中會定義這個方法。

## 偵測畫面和裝置方向

在行動裝置 (如手機或平板電腦) 上執行媒體擷取 app 時，有數個領域會受到裝置目前的方向所影響。 這些領域包括從相機正確旋轉預覽資料流以及正確編碼擷取的影像和視訊，這樣使用者檢視時的方向才會正確。

「顯示方向」一詞是指系統在裝置上旋轉 XAML 頁面的方式，以便使用者直立檢視。 「裝置方向」是指全局空間中的裝置方向，因此是全局空間中實體相機裝置的方向。 這兩種方向都與媒體擷取 app 相關。 若要處理顯示方向，請宣告並初始化 [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/dn264258) 類別的頁面範圍變數。 宣告另一個 [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142) 類型的變數，以追蹤目前的顯示器方向。 宣告 [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400) 變數和 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) 變數，以追蹤裝置方向。

[!code-cs[DisplayInformationAndOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayInformationAndOrientation)]

下列協助程式方法可登錄和取消登錄 [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) 和 [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407) 事件的事件處理常式，並以目前的方向初始化追蹤變數。 請注意，並非所有裝置都有 [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400)，因此您應該在登錄處理常式或嘗試取得目前方向之前進行檢查。

[!code-cs[RegisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterOrientationEventHandlers)]

[!code-cs[UnregisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterOrientationEventHandlers)]

在 **SimpleOrientationSensor.OrientationChanged** 事件的事件處理常式中，以目前的方向更新裝置方向變數。 若裝置是面向上或面向下，您不應該更新方向。

[!code-cs[SimpleOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSimpleOrientationChanged)]

在 **DisplayInformation.OrientationChanged** 事件的事件處理常式中，以目前的方向更新顯示器方向變數。 如果目前顯示擷取裝置的視訊預覽，請更新預覽視訊資料流的旋轉。 下一節會說明 **SetPreviewRotationAsync** 協助程式方法。

[!code-cs[DisplayOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayOrientationChanged)]

## 設定媒體擷取預覽旋轉

使用者預期 UI 控制項會在其行動裝置的方向變更時旋轉，以便 UI 中的文字垂直對齊並可閱讀。 不過，對於 **CaptureElement** 控制項，使用者通常不希望視訊預覽的方向跟著裝置旋轉。 為了提供預期的使用者體驗，您應該旋轉預覽資料流以符合裝置的方向。

預覽資料流方向必須以度數表示。 下列協助程式方法可將 [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142) 列舉值轉換為度數。

[!code-cs[ConvertDisplayOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDisplayOrientationToDegrees)]

此外，此協助程式方法可轉換 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) 列舉值，並由 [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400) 用來表示裝置的旋轉 (以度數表示)。

[!code-cs[ConvertDeviceOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDeviceOrientationToDegrees)]

低階的資料流旋轉實際上是由「Microsoft 媒體基礎」架構執行。 使用 [MF\_MT\_VIDEO\_ROTATION](https://msdn.microsoft.com/library/windows/desktop/hh162880) 屬性可指定旋轉。 因為這是 Windows 執行階段 app，所以會使用屬性的 GUID 指定旋轉，而非使用屬性名稱。 定義下列 GUID 來識別視訊旋轉屬性。

[!code-cs[RotationKey](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRotationKey)]

下列方法可設定預覽資料流的旋轉。 媒體擷取之 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) 的 [**GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) 方法會傳回組成機碼/值組的屬性集。 指定 [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640)，表示我們想要視訊預覽資料流的屬性，而不是視訊錄製資料流或音訊資料流。 此屬性集是用於設定資料流屬性的一般用途介面，但對此工作而言，以上定義的視訊旋轉 GUID 會新增為屬性機碼，而所需的視訊資料流方向 (以度數表示) 則會指定為此值。 [ **SetEncodingPropertiesAsync** ](https://msdn.microsoft.com/library/windows/apps/dn297781) 可使用新的值更新編碼屬性。 再次指定 **MediaStreamType.VideoPreview**，表示所要設定的屬性適用於視訊預覽資料流。

[!code-cs[SetPreviewRotation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetPreviewRotation)]

-   對於具有外接式相機的裝置，使用者不會預期相機資料流會跟著裝置旋轉。

-   如果針對前面板上的相機鏡像處理預覽，則必須反轉旋轉方向以符合裝置的旋轉。

-   有些裝置 (通常是電話) 支援將 [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259) 設定為方向值 (例如 [**DisplayOrientations.Landscape**](https://msdn.microsoft.com/library/windows/apps/br226142))，以強制顯示跟著裝置旋轉。 您應該設定此值，因為它可在支援的裝置上提供良好的經驗，但您仍應在您的 app 中實作上述模式，以支援不支援自動旋轉喜好設定的裝置。

-   在舊版中，[**SetPreviewRotation**](https://msdn.microsoft.com/library/windows/apps/br226611) 方法是旋轉預覽資料流的唯一方式。 此方法目前仍存在於 API 介面中，以支援現有的 app，但這個方法沒有效率且不得使用於新的 app。

## 擷取相片

下列方法會使用 [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/hh700840) 方法擷取相片，並傳入要求的編碼屬性以及將會包含擷取操作輸出的 [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241720) 物件。 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 類別會提供協助程式方法 (如 [**CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/hh700994))，以產生媒體擷取所支援檔案類型的編碼屬性。

[!code-cs[TakePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetTakePhotoAsync)]

將相片儲存到檔案之前，您必須判斷相片的正確方向。 **MediaCapture** 物件不知道裝置的方向，所以它會以擷取裝置採用其預設方向的方式來編碼所擷取的相片資料。 這會在使用者檢視所擷取的相片時造成負面的使用者體驗，因為相片的方向不正確。 下列協助程式方法可判斷正確的相片方向，然後以正確的方向儲存檔案。

**GetCameraOrientation** 協助程式方法會以目前的裝置方向啟動，然後根據裝置的原生方向和裝置上的相機位置旋轉該值。 如果相機固定在裝置前面板 (如這個範例中的 **\_mirroringPreview** 變數所示)，則相機方向應該會反轉。

[!code-cs[GetCameraOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetCameraOrientation)]

下列協助程式方法只是將方向感應器所使用的 [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) 列舉值轉換為將用來儲存檔案之點陣圖編碼器所使用的同等 [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476) 值。

[!code-cs[ConvertOrientationToPhotoOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertOrientationToPhotoOrientation)]

最後，可以編碼和儲存所擷取的相片。 從包含所擷取相片資料的輸入資料流建立 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 物件。 建立新的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 並將它開啟以便讀取和寫入。 建立 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) 物件，傳入輸出檔案及包含影像資料的解碼器。 建立新的 [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338) 並加入新的屬性。 "System.Photo.Orientation" 屬性的機碼指定屬性代表相片的方向。 這個值是先前計算的 [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476) 值。 呼叫 [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252) 以在編碼器上更新屬性，然後呼叫 [**FlushAsync**](https://msdn.microsoft.com/library/dn237883) 將相片寫入存放檔案。

[!code-cs[ReencodeAndSavePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetReencodeAndSavePhotoAsync)]

-   設定 "System.Photo.Orientation" 點陣圖屬性可將相片的方向編碼成檔案的中繼資料。 這不會導致以不同的方式編碼實際影像資料。 如需有關將中繼資料內嵌到影像檔案的詳細資訊，請參閱[影像中繼資料](image-metadata.md)。

-   如需有關使用影像的詳細資訊，包括影像編碼和解碼，請參閱[影像處理](imaging.md)。

## 擷取視訊

若要開始擷取視訊，請先建立將要記錄視訊的存放檔案。 接著建立 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026)，供 **MediaCapture** 將視訊編碼成檔案。 **MediaEncodingProfile** 類別提供一些方法 (例如 [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078))，以建立所支援視訊格式的編碼設定檔。 使用先前討論的協助程式方法，取得視訊的正確旋轉度數。 與相片案例不同，視訊旋轉資訊會由 **MediaCapture** 編碼成資料流。 將旋轉資訊加入至 [**VideoEncodingProperties.Properties**](https://msdn.microsoft.com/library/windows/apps/hh701254) 集合，即可將它新增到編碼設定檔。 先前為視訊旋轉定義的 GUID 會做為機碼，而其值是旋轉度數。 最後，呼叫 [**MediaCapture.StartRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh700863)，指定編碼屬性與輸出檔案即可開始錄製。

[!code-cs[StartRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartRecordingAsync)]

若要停止錄製，只需呼叫 [**MediaCapture.StopRecordAsync**](https://msdn.microsoft.com/library/windows/apps/br226623)。

[!code-cs[StopRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopRecordingAsync)]

已在初始化 **MediaCapture** 時登錄 [**MediaCapture.RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/hh973312) 事件的處理常式。 在處理常式中，呼叫 **StopRecordingAsync** 方法來停止錄製。

[!code-cs[RecordLimitationExceeded](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

## 暫停和繼續視訊擷取

在某些情況下，您可能想要暫停並繼續進行中的視訊擷取，而不是停止擷取並開始進行新的擷取。 若要暫停錄製，請呼叫 [**PauseRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858102)。 如果您指定 [**MediaCapturePauseBehavior.RetainHardwareResources**](https://msdn.microsoft.com/library/windows/apps/dn926686)，則在不支援同時視訊與相片擷取的裝置上，app 將無法在視訊暫停時擷取相片。 如需判斷裝置是否支援同時相片和視訊擷取的資訊，請參閱[相機設定檔](camera-profiles.md)。

[!code-cs[PauseRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPauseRecordingAsync)]

若要繼續先前暫停的視訊擷取，請呼叫 [**ResumeRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858103)。

[!code-cs[ResumeRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResumeRecordingAsync)]

## 清理擷取資源

請務必正確地關閉並釋出媒體擷取資源。 若無法這麼做，在 app 關閉之後，將會造成未預期的相機行為，進而導致 App 使用者發生負面的使用經驗。 下列各節逐步引導您完成關閉相機所需的步驟。

### 關閉擷取預覽

若要關閉擷取預覽，請先呼叫 [**MediaCapture.StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622)。 將 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 屬性設定為 Null。 接著，對 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 變數呼叫 [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819)，允許系統在必要時關閉顯示器。

[!code-cs[StopPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopPreviewAsync)]

-   您無法從非 UI 執行緒存取 UI，所以必須使用 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 方法設定 **MediaElement.Source** 屬性及呼叫 **RequestRelease**，才能在 UI 執行緒上執行呼叫。

### 關閉和處置 MediaCapture 物件

處置 **MediaCapture** 物件之前，請停止進行中的錄製作業，並呼叫先前定義的協助程式方法來停止預覽資料流。 完成此動作之後，移除向 **MediaCapture** 登錄的所有事件處理常式，然後呼叫 [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) 以釋出與此物件相關聯的資源並將 **MediaCapture** 變數設定為 Null

[!code-cs[CleanupCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

您應該呼叫這個方法，以從 [**MediaCapture.Failed**](https://msdn.microsoft.com/library/windows/apps/br226593) 事件的處理常式內部關閉並處置 **MediaCapture** 物件。

[!code-cs[CaptureFailed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureFailed)]

### 處理 app 存留期和頁面瀏覽事件

App 存留期事件讓 app 有機會初始化並釋出資源。 這對於 **Application.Suspending** 事件特別重要，您的 app 必須正確地處置媒體擷取資源。

您可以在頁面的建構函式中登錄 app 存留期事件的處理常式。

[!code-cs[RegisterAppLifetimeEvents](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterAppLifetimeEvents)]

在 **Application.Suspending** 事件的處理常式中，您應該取消登錄顯示器和裝置方向事件的處理常式並關閉 **MediaCapture** 物件。 本文稍後描述的另一個應用程式週期相關工作會需要此處取消註冊的 [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720) 事件。

**警告**：您必須在擱置中事件處理常式的開頭呼叫 [**SuspendingOperation.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690)，以要求延遲擱置。 這會要求系統等到您發出作業已完成的訊號，再清除您的 app。 這是必要的，因為 **MediaCapture** 關閉作業是非同步的，所以 **Application.Suspending** 事件處理常式可以在相機正常關機前完成。 等到非同步呼叫完成之後，您應該藉由呼叫 [**SuspendingDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 來釋放延遲。

[!code-cs[Suspending](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSuspending)]

在 **Application.Resuming** 事件的處理常式中，您應該登錄顯示器和裝置方向事件的處理常式、登錄 **SystemMediaTransportControls.PropertyChanged** 物件，以及初始化 **MediaCapture** 物件。

[!code-cs[Resuming](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResuming)]

[**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) 事件讓您一開始有機會登錄顯示器和裝置方向事件的處理常式並初始化 **MediaCapture** 物件。

[!code-cs[OnNavigatedTo](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatedTo)]

如果 app 有多個頁面，您應該在 [**OnNavigatingFrom**](https://msdn.microsoft.com/library/windows/apps/br227509) 事件處理常式中清理您的媒體擷取物件。

[!code-cs[OnNavigatingFrom](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatingFrom)]

為了讓 app 能在支援多個同步視窗的裝置上正常運作，您必須在 app 已最小化或還原時進行回應。 若要這樣做，您必須處理 [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720) 事件。 初始化成員變數，以針對您的 app 儲存對 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 物件的參考。

[!code-cs[DeclareSystemMediaTransportControls](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareSystemMediaTransportControls)]

如上述範例所示，用以登錄和取消登錄 **PropertyChanged** 事件的程式碼應該新增至 app 週期事件。 在事件的處理常式中，查看觸發此事件的屬性變更是否為 [**SystemMediaTransportControlsProperty.SoundLevel**](https://msdn.microsoft.com/library/windows/apps/dn278721) 屬性。 如果這是變更的屬性，請檢查屬性的值。 如果此值為 [**SoundLevel.Muted**](https://msdn.microsoft.com/library/windows/apps/hh700852)，則 app 會最小化，而您應該適當地清理媒體擷取資源。 否則，事件會發出 app 視窗已還原的訊號，而您應重新初始化您的媒體擷取資源。 必須在 UI 執行緒上存取 **SoundLevel** 屬性，以便在對 [**Dispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 的呼叫中包裝此方法中的程式碼。

[!code-cs[SystemMediaControlsPropertyChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSystemMediaControlsPropertyChanged)]

## 媒體擷取的其他 UI 考量

### 設定自動旋轉喜好設定

如上一節的旋轉預覽資料流所述，有些裝置支援設定 [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259)以避免頁面 (包括顯示預覽的 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)) 在裝置旋轉時跟著旋轉。 這可在支援的裝置上提供良好的使用者經驗。 當 app 啟動或當您開始顯示預覽時，設定這個值。 請注意，您仍應該對不支援自動旋轉喜好設定的裝置，實作預覽旋轉處理。

[!code-cs[SetAutoRotationPreferences](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetAutoRotationPreferences)]

### 處理 UI 元素方向

使用者通常希望相機 app 中的 UI 元素 (例如用來起始相片或視訊擷取的按鈕) 隨著視訊預覽一起旋轉。 下列方法說明如何使用先前定義的方向協助程式方法，正確地在相機 UI 中設定按鈕的方向。 您應該在 app 初次啟動時，從 [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) 和 [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407) 事件處理常式呼叫這個方法。 您的實作會因 app 的 UI 而有所不同。

[!code-cs[UpdateButtonOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateButtonOrientation)]

當 app 關閉時，或如果您瀏覽至與媒體擷取無關的頁面，請將自動旋轉喜好設定設為 [**None**](https://msdn.microsoft.com/library/windows/apps/br226142) 以允許 UI 正常旋轉。

[!code-cs[RevertAutoRotationPreferences](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRevertAutoRotationPreferences)]

### 支援同時的相片和影片擷取

[**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) API 可讓您在支援的裝置上同時擷取相片和視訊。 為求簡潔，這個範例使用 [**ConcurrentRecordAndPhotoSupported**](https://msdn.microsoft.com/library/windows/apps/dn278843) 屬性來判斷是否支援同時擷取視訊與相片，但更強大且建議的方法是使用相機設定檔。 如需詳細資訊，請參閱[相機設定檔](camera-profiles.md)。

下列協助程式方法會更新 app 的控制項，以符合 app 目前的擷取狀態。 每當 app 的擷取狀態變更 (例如影片擷取啟動或停止) 時，請呼叫此方法。

[!code-cs[UpdateCaptureControls](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateCaptureControls)]

### 支援行動裝置特定的 UI 功能

本文中顯示的所有程式碼將會在通用 Windows app 中運作。 透過額外幾行程式碼，您可以利用只會出現在行動裝置上的特殊 UI 功能。 若要使用這些功能，您必須將 Microsoft Mobile Extension SDK for Universal App Platform 的參照新增到您的專案。

**若要新增硬體相機按鈕支援的行動擴充功能 SDK 的參照**

1.  在 [方案總管]**** 中的 [參照]**** 上按一下滑鼠右鍵，然後選取 [加入參照...]****。

2.  展開 [**Windows 通用**] 節點，然後選取 [**擴充功能**]。

3.  按一下 [**Microsoft Mobile Extension SDK for Universal App Platform**] 旁邊的核取方塊。

行動裝置的 [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) 控制項可為使用者提供有關裝置的狀態資訊。 此控制項會佔掉螢幕上可能會干擾媒體擷取 UI 的空間。 您可以呼叫 [**HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339) 以隱藏狀態列，但是您必須在條件性區塊中進行此呼叫，而此種區塊中您會使用 [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/dn949016) 方法來判斷 API 是否可用。 這個方法只會在支援狀態列的行動裝置上傳回 true。 您應該在 app 啟動時或在您開始從相機進行預覽時隱藏狀態列。

[!code-cs[HideStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

當 app 關閉或使用者離開 app 的媒體擷取頁面時，您可讓此控制項再次顯現。

[!code-cs[ShowStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

相較於螢幕上的控制項，有些行動裝置有某些使用者偏好的專用硬體相機按鈕。 若要在按下硬體相機按鈕時收到通知，請登錄 [**HardwareButtons.CameraPressed**](https://msdn.microsoft.com/library/windows/apps/dn653805) 事件的處理常式。 因為此 API 只能用於行動裝置，所以您必須再次使用 **IsTypePresent** 來確定目前的裝置支援此 API，再嘗試進行存取。

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[RegisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

在 **CameraPressed** 事件的處理常式中，您可以起始相片擷取。

[!code-cs[CameraPressed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

當 app 關閉或使用者離開 app 的媒體擷取頁面時，請取消登錄硬體按鈕處理常式。

[!code-cs[UnregisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

**注意：**本文章適用於撰寫通用 Windows 平台 (UWP) App 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

## 相關主題

* [相機設定檔](camera-profiles.md)
* [高動態範圍 (HDR) 相片擷取](high-dynamic-range-hdr-photo-capture.md)
* [相片和視訊擷取的擷取裝置控制項](capture-device-controls-for-photo-and-video-capture.md)
* [視訊擷取的擷取裝置控制項](capture-device-controls-for-video-capture.md)
* [視訊擷取的效果](effects-for-video-capture.md)
* [媒體擷取的場景分析](scene-analysis-for-media-capture.md)
* [可變相片序列](variable-photo-sequence.md)
* [取得預覽框架](get-a-preview-frame.md)
* [CameraStarterKit 範例](http://go.microsoft.com/fwlink/?LinkId=619479)



<!--HONumber=Jun16_HO4-->


