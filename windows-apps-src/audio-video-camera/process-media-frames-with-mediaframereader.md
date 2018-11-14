---
author: drewbatgit
ms.assetid: a128edc8-8a80-4645-ac29-908ede2d1c72
description: 本文章示範如何使用 MediaFrameReader 搭配 MediaCapture，從一或多個可用來源取得媒體畫面。來源包括色彩、深度及紅外線相機、音訊裝置，甚至是自訂畫面來源 (例如，能產生骨骼追蹤畫面的來源)。
title: 使用 MediaFrameReader 處理媒體畫面
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c733fe0f4e8ee955c68ff4ec30bd9f9f2675899d
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6190730"
---
# <a name="process-media-frames-with-mediaframereader"></a>使用 MediaFrameReader 處理媒體畫面

本文章示範如何使用 [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) 搭配 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture)，從一或多個可用來源取得媒體畫面。來源包括色彩、深度及紅外線相機、音訊裝置，甚至是自訂畫面來源 (例如，能產生骨骼追蹤畫面的來源)。 此功能是針對要讓執行媒體畫面即時處理的 App 使用所設計，例如虛擬實境及深度感知相機 App。

如果您只對擷取視訊或相片感興趣，例如典型的相片應用程式，則您可能想要使用 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 支援的其他擷取技術的其中之一。 如需可用的媒體擷取技術和說明使用方式之文章的清單，請參閱[**相機**](camera.md)。

> [!NOTE] 
> 本文中所討論的功能只從 Windows 10 版本 1607 開始提供。

> [!NOTE] 
> 還有一個通用 Windows app 範例，示範使用 **MediaFrameReader** 顯示來自不同畫面來源 (包括色彩、深度與紅外線相機) 的畫面。 如需詳細資訊，請參閱[相機畫面範例](http://go.microsoft.com/fwlink/?LinkId=823230)。

> [!NOTE] 
> 使用 **MediaFrameReader** 搭配音訊資料的一組全新 API 在 Windows 10 版本 1803 中引進。 如需詳細資訊，請參閱[使用 MediaFrameReader 處理音訊框架](process-audio-frames-with-mediaframereader.md)。


## <a name="setting-up-your-project"></a>設定您的專案
就像任何使用 **MediaCapture** 的 App 一樣，您必須在嘗試存取任何相機裝置之前，宣告您的 App 是使用*網路攝影機*功能。 如果您的應用程式會從音訊裝置擷取，您也應該宣告*麥克風*裝置功能。 

**將功能新增到應用程式資訊清單**

1.  在 Microsoft Visual Studio 中，按兩下 **\[方案總管\]** 中的 **package.appxmanifest** 項目，開啟應用程式資訊清單的設計工具。
2.  選取 **\[功能\]** 索引標籤。
3.  核取 **\[網路攝影機\]** 方塊和 **\[麥克風\]** 方塊。
4.  如果要存取圖片媒體櫃和視訊媒體櫃，請選取 **\[圖片媒體櫃\]** 方塊和 **\[視訊媒體櫃\]** 方塊。

除了預設專案範本所包含的 API 以外，這篇文章中的範例程式碼還會使用下列命名空間的 API。

[!code-cs[FramesUsing](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFramesUsing)]

## <a name="select-frame-sources-and-frame-source-groups"></a>選取畫面來源和畫面來源群組
許多處理媒體畫面的 App 需要一次從多個來源取得畫面，例如裝置的彩色和景深相機。 [**MediaFrameSourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup)物件代表一組可同時使用的媒體畫面來源。 呼叫靜態方法 [**MediaFrameSourceGroup.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.FindAllAsync)，以取得目前裝置所支援之所有畫面來源群組的清單。

[!code-cs[FindAllAsync](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFindAllAsync)]

您也可以使用 [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceWatcher) 和 [**MediaFrameSourceGroup.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/br225427) 傳回的值建立 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.GetDeviceSelector)，以在裝置上可用的畫面來源群組變更時收到通知，例如當插入外部相機時。 如需詳細資訊，請參閱[**列舉裝置**](https://msdn.microsoft.com/windows/uwp/devices-sensors/enumerate-devices)。

[**MediaFrameSourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup) 有一個 [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo) 物件的集合，可描述群組中包含的畫面來源。 在擷取裝置上可用的畫面來源群組之後，您可以選取公開您感興趣的畫面來源群組。

下列範例示範選取畫面來源群組最簡單的方式。 這個程式碼僅重複查看所有可用的群組，然後重複查看 [**SourceInfos**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.SourceInfos) 集合中的每個項目。 每個 **MediaFrameSourceInfo** 都會被檢查，以查看是否支援我們要尋找的功能。 在這個案例中，會針對值 [**VideoPreview**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo.MediaStreamType) 檢查 [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaStreamType) 屬性，表示裝置提供一個視訊預覽資料流，以及針對值 [**Color**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo.SourceKind) 檢查 [**SourceKind**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceKind) 屬性，指示來源提供色彩畫面。

[!code-cs[SimpleSelect](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSimpleSelect)]

這個識別想要的畫面來源群組和畫面來源的方法，適用於簡單的案例，但是如果您想要根據更複雜的條件來選取畫面來源，它可能很快就會變得很麻煩。 另一種方法是使用 Linq 語法和匿名物件來選取項目。 下列範例使用 **Select** 延伸方法，來將 *frameSourceGroups* 清單中的 **MediaFrameSourceGroup** 物件轉換為包含兩個欄位的匿名物件：*sourceGroup*，代表群組本身，以及 *colorSourceInfo*，代表群組中的色彩畫面來源。 *colorSourceInfo* 欄位會設定為 **FirstOrDefault** 的結果，這會選取所提供述詞解析為 true 的第一個物件。 在這個案例中，如果資料流類型為 **VideoPreview**、來源種類為 **Color**，且相機位於裝置的前方面板上，述詞為 true。

從上述查詢傳回的匿名物件清單，**Where** 延伸方法是僅用來選取那些 *colorSourceInfo* 欄位不是 null 的物件。 最後，會呼叫 **FirstOrDefault** 來選取清單中的第一個項目。

現在您可以使用已選取物件的欄位來取得對已選取的 **MediaFrameSourceGroup** 和代表色彩相機之 **MediaFrameSourceInfo** 物件的參考。 這些稍後將會用來初始化 **MediaCapture** 物件，並針對已選取的來源建立 **MediaFrameReader**。 最後，您應該測試以查看來源群組是否為 null，這表示目前的裝置沒有您要求的擷取來源。

[!code-cs[SelectColor](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColor)]

下列範例使用上述類似技術，來選取包含彩色、景深與紅外線相機的來源群組。

[!code-cs[ColorInfraredDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetColorInfraredDepth)]

> [!NOTE]
> 從 Windows 10 版本 1803 開始，您可以使用 [**MediaCaptureVideoProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) 類別來選取具有一組所需功能的媒體畫面來源。 如需詳細資訊，請參閱本文稍後的**選取畫面來源使用影片的設定檔**章節。


## <a name="initialize-the-mediacapture-object-to-use-the-selected-frame-source-group"></a>初始化 MediaCapture 物件，以使用所選的畫面來源群組
下一步是初始化 **MediaCapture** 物件，以使用您在上一個步驟中選取的畫面來源群組。

**MediaCapture** 物件通常會在您 App 內部的多個位置使用，因此您應該宣告一個類別成員變數來保存它。

[!code-cs[DeclareMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

藉由呼叫建構函式，建立 **MediaCapture** 物件的執行個體。 接下來，建立將用來初始化 **MediaCapture** 物件的 [**MediaCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSettings) 物件。 在這個範例中，會使用下列設定︰

* [**SourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.SourceGroup) - 這會告訴系統您將會使用哪一個來源群組取得畫面。 請記住，來源群組會定義一組可同時使用的媒體畫面來源。
* [**SharingMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.SharingMode) - 這會告訴系統您是否需要擷取來源裝置的專屬控制項。 如果您將此設定為 [**ExclusiveControl**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSharingMode)，就表示您可以變更擷取裝置的設定 (像是裝置所產生的畫面格式)，但是這表示如果其他 App 已經有專屬控制項，當您的 App 嘗試初始化媒體擷取裝置時將會失敗。 如果您將此設定為 [**SharedReadOnly**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSharingMode)，即使畫面來源正由其他 App 使用，您也可以接收來自畫面來源的畫面，但您無法變更裝置的設定。
* [**MemoryPreference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.MemoryPreference) - 如果您指定 [**CPU**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureMemoryPreference)，系統將會使用 CPU 記憶體確保當畫面抵達時，畫面可以做為 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) 物件使用。 如果您指定 [**Auto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureMemoryPreference)，系統會以動態方式選擇最佳的記憶體位置來儲存畫面。 如果系統選擇使用 GPU 記憶體，媒體畫面會以 [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 物件的方式抵達，而不是 **SoftwareBitmap**。
* [**StreamingCaptureMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.StreamingCaptureMode) - 將它設定為 [**Video**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.StreamingCaptureMode) 以指示該音訊不需要串流處理。

呼叫 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)，以使用您想要的設定將 **MediaCapture** 初始化。 請務必在 *try* 區塊中呼叫，以防初始化失敗。

[!code-cs[InitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-the-preferred-format-for-the-frame-source"></a>針對畫面來源設定慣用的格式
若要設定畫面來源的慣用格式，您必須取得代表來源的 [**MediaFrameSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource) 物件。 您可以透過存取已初始化 **MediaCapture** 物件的 [**Frames**](https://msdn.microsoft.com/library/windows/apps/Windows.Phone.Media.Capture.CameraCaptureSequence.Frames) 字典，指定您想要使用的畫面來源的識別碼來取得此物件。 這就是為什麼我們在選取畫面來源群組時會儲存 [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo) 物件的原因。

[**MediaFrameSource.SupportedFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource.SupportedFormats) 屬性包含一份 [**MediaFrameFormat**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameFormat) 物件的清單，其中說明了畫面來源的支援格式。 使用 **Where** Linq 延伸方法，根據所需的屬性選取格式。 在這個範例中，會選取一個寬度為 1080 像素，並且可提供 32 位元 RGB 格式畫面的格式。 **FirstOrDefault** 延伸方法會選取清單中的第一個項目。 如果選取的格式為 null，那麼畫面來源就不支援要求的格式。 如果是支援的格式，您可透過呼叫 [**SetFormatAsync**](https://msdn.microsoft.com/library/windows/apps/)，要求來源使用此格式。

[!code-cs[GetPreferredFormat](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetPreferredFormat)]

## <a name="create-a-frame-reader-for-the-frame-source"></a>建立畫面來源的畫面讀取程式
若要接收媒體畫面來源的畫面，請使用 [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader)。

[!code-cs[DeclareMediaFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaFrameReader)]

藉由呼叫已初始化 **MediaCapture** 物件上的[**CreateFrameReaderAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CreateFrameReaderAsync) 將畫面讀取程式具現化。 這個方法的第一個引數是您想要接收畫面的畫面來源。 您可以針對每個想要使用的畫面來源建立個別的畫面讀取程式。 第二個引數會告訴系統您想要畫面到達時使用的輸出格式。 這可以幫助您避免在畫面到達時必須自行轉換。 請注意，如果您指定畫面來源不支援的格式，會擲回例外狀況，因此請確定此值存在於 [**SupportedFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource.SupportedFormats) 集合中。  

建立畫面讀取程式之後，請登錄 [**FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived) 事件的處理常式，只要來源有可用的新畫面時就會引發該事件。

透過呼叫 [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.StartAsync) 告訴系統開始從來源讀取畫面。

[!code-cs[CreateFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateFrameReader)]

## <a name="handle-the-frame-arrived-event"></a>處理畫面已到達事件
當有新畫面可用時，就會引發 [**MediaFrameReader.FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived) 事件。 您可以選擇處理每一個到達的畫面，或者僅在您需要時使用畫面。 因為畫面讀取程式會引發其本身執行緒上的事件，您可能需要實作部分同步邏輯來確定您沒有嘗試從多個執行緒存取相同的資料。 本節說明如何將繪圖色彩畫面同步到 XAML 頁面中的影像控制項。 本案例會解決要求在 UI 執行緒上執行所有 XAML 控制項更新的其他同步限制式。

在 XAML 中顯示畫面的第一步是建立影像控制項。 

[!code-xml[ImageElementXAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetImageElementXAML)]

在程式碼後置頁面中，宣告 **SoftwareBitmap** 類型的類別成員變數，這會做為後端緩衝區，以暫存所有複製的傳入影像。 請注意，影像資料本身不會複製，只會複製物件參考。 此外，請宣告布林值以追蹤我們的 UI 作業目前是否正在執行。

[!code-cs[DeclareBackBuffer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareBackBuffer)]

因為畫面會以 **SoftwareBitmap** 物件的方式到達，所以您必須建立 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) 物件，它可讓您使用 **SoftwareBitmap** 做為 XAML **控制項**的來源。 在您啟動畫面讀取程式之前，您應該在您程式碼中某處設定影像來源。

[!code-cs[ImageElementSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetImageElementSource)]

現在就可以實作 **FrameArrived** 事件處理常式。 呼叫這個處理常式時，*sender* 參數包含對引發事件的 **MediaFrameReader** 物件的參考。 在此物件上呼叫 [**TryAcquireLatestFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.TryAcquireLatestFrame) 以嘗試取得最新畫面。 如同名稱所示，**TryAcquireLatestFrame** 可能無法成功傳回畫面。 因此，當您存取 VideoMediaFrame 和 SoftwareBitmap 屬性時，請確定針對 null 進行測試。 這個範例是 null 條件式運算子嗎 ?  是用來存取 **SoftwareBitmap**，然後會檢查擷取的物件是否為 null。

**Image** 控制項可以僅顯示預乘或無 Alpha 的 BRGA8 格式影像。 如果送達的畫面不是該格式，會使用靜態方法 [**Convert**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap.Covert) 將軟體點陣圖轉換成正確的格式。

接下來，[**Interlocked.Exchange**](https://msdn.microsoft.com/library/bb337971) 方法會用來交換送達點陣圖的參考與後端緩衝區點陣圖。 這個方法會在安全執行緒的不可部分完成作業中交換這些參考。 交換之後，會處置舊的後端緩衝區影像 (現在在 *softwareBitmap* 變數中) 以清除其資源。

接下來，會使用與 **Image** 元素相關聯的 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher) 來建立將透過呼叫 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 在 UI 執行緒上執行的工作。 因為非同步工作將會在工作中執行，所以 lambda 運算式會傳遞到使用 *async* 關鍵字宣告的 **RunAsync**。

在工作中，會檢查 *_taskRunning* 變數以確定一次只執行工作的一個執行個體。 如果工作尚未執行，*_taskRunning* 會設為 true，以避免再次執行工作。 在 *while* 迴圈中，會呼叫 **Interlocked.Exchange** 來從後端緩衝區複製到暫存 **SoftwareBitmap** 中，直到後端緩衝區影像為 null。 每次填入暫存點陣圖，**Image** 的　**Source** 屬性會轉換成 **SoftwareBitmapSource**，然後呼叫 [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) 來設定影像來源。

最後，*_taskRunning* 變數會設定回 false，就可以在下一次呼叫處理常式時再次執行工作。

> [!NOTE] 
> 如果您要存取 [**MediaFrameReference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.VideoMediaFrame.SoftwareBitmap) 的 [**VideoMediaFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.VideoMediaFrame.Direct3DSurface) 屬性提供的 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference.VideoMediaFrame) 或 [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference) 物件，則系統會建立這些物件的強式參考，這表示當您在包含的 **MediaFrameReference** 上呼叫 [**Dispose**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference.Close) 時，他們不會被處置。 您必須針對要立即處置的物件明確地直接呼叫 **SoftwareBitmap** 或 **Direct3DSurface** 的 **Dispose** 方法。 否則，記憶體回收行程最終會釋放這些物件的記憶體，但您無法得知何時會釋放，而且如果配置的點陣圖或表面的數量超過系統允許的數量上限，新畫面的資料流就會停止。 您可以複製擷取的畫面 (例如使用 [**SoftwareBitmap.Copy**](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.imaging.softwarebitmap.copy) 方法)，然後釋出原始畫面來克服這項限制。 此外，如果您使用多載 [CreateFrameReaderAsync(Windows.Media.Capture.Frames.MediaFrameSource inputSource, System.String outputSubtype, Windows.Graphics.Imaging.BitmapSize outputSize)](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_Windows_Graphics_Imaging_BitmapSize_) 或 [CreateFrameReaderAsync(Windows.Media.Capture.Frames.MediaFrameSource inputSource, System.String outputSubtype)](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_) 建立 **MediaFrameReader**，傳回的畫面會是原始畫面資料的複本，因此在保留畫面時不會造成畫面擷取停止。 


[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFrameArrived)]

## <a name="cleanup-resources"></a>清理資源
當您完成後讀取畫面時，請確定有透過呼叫 [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.StopAsync)、取消登錄 **FrameArrived** 處理常式，並處置 **MediaCapture** 物件，來停止媒體畫面讀取程式。

[!code-cs[Cleanup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCleanup)]

如需有關在您的應用程式暫停時清理媒體擷取物件的詳細資訊，請參閱[**顯示相機預覽**](simple-camera-preview-access.md)。

## <a name="the-framerenderer-helper-class"></a>FrameRenderer 協助程式類別
通用 Windows [相機畫面範例](http://go.microsoft.com/fwlink/?LinkId=823230)提供了一個協助程式類別，可讓顯示來自您 App 中色彩、紅外線與深度來源的畫面變得更容易。 一般來說，您可能會想針對深度或紅外線資料多執行一些動作，而不僅是在螢幕上顯示，但是這個協助程式類別對於示範畫面讀取程式功能以及偵錯您自己的畫面讀取程式實作而言，是很有幫助的工具。

**FrameRenderer** 協助程式類別會實作下列方法。

* **FrameRenderer** 建構函式 - 建構函式會初始化協助程式類別，以使用您傳入的 XAML [**Image**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Image) 元素來顯示媒體畫面。
* **ProcessFrame** - 這個方法會顯示您傳入建構函式的 **Image** 元素中，以 [**MediaFrameReference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference) 表示的媒體畫面。 您通常應該從 [**FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived) 事件處理常式中呼叫此方法，傳入由 [**TryAcquireLatestFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.TryAcquireLatestFrame) 傳回的畫面。
* **ConvertToDisplayableImage** - 這個方法會檢查媒體畫面的格式，必要時會將媒體畫面的格式轉換成可顯示的格式。 針對色彩影像，這代表確定色彩格式為 BGRA8 且已預乘點陣圖 Alpha 模式。 針對深度或紅外線畫面，會使用也包含於範例中的 **PsuedoColorHelper** 類別 (如下所示) 來處理每條掃描線，以將深度或紅外線畫面轉換為偽色彩漸層。

> [!NOTE] 
> 為了在 **SoftwareBitmap** 影像上執行像素操作，您必須存取原生記憶體緩衝區。 若要這樣做，您必須使用包含於下面所列程式碼中的 IMemoryBufferByteAccess COM 介面，且您必須更新您的專案屬性，以允許編譯不安全的程式碼。 如需詳細資訊，請參閱[建立、編輯及儲存點陣圖影像](imaging.md)。

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetFrameRenderer)]

## <a name="use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources"></a>使用 MultiSourceMediaFrameReader 從多個來源取得與時間相互關聯的畫面
從 Windows 10 版本 1607 開始，您可以使用 [**MultiSourceMediaFrameReader**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader)，從多個來源接收與時間相互關聯的畫面。 此 API 可讓您更容易進行需要來自多個拍攝時間相近來源之畫面的處理，例如使用 [**DepthCorrelatedCoordinateMapper**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.depthcorrelatedcoordinatemapper) 類別。 使用這個新方法有一項限制，就是只能以最慢擷取來源的速率來引發畫面已到達事件。 將會捨棄較快速來源的額外畫面。 此外，由於系統預期畫面以不同的速率從不同的來源抵達，因此也無法自動辨識來源是否已完全停止產生畫面。 本節的範例程式碼示範如何使用事件建立您自己的逾時邏輯，只要相互關聯的畫面未在應用程式定義的時間限制內抵達，就會叫用此邏輯。

使用 [**MultiSourceMediaFrameReader**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) 的步驟和使用本文之前所述 [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) 的步驟相似。 此範例將會使用色彩來源和深度來源。 宣告一些字串變數來儲存媒體畫面來源識別碼，這些識別碼將用來選取每個來源的畫面。 接下來，宣告用於實作範例逾時邏輯的 [**ManualResetEventSlim**](https://docs.microsoft.com/dotnet/api/system.threading.manualreseteventslim?view=netframework-4.7)、[**CancellationTokenSource**](https://msdn.microsoft.com/library/system.threading.cancellationtokensource.aspx) 和 [**EventHandler**](https://msdn.microsoft.com/library/system.eventhandler.aspx)。 

[!code-cs[MultiFrameDeclarations](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameDeclarations)]

使用本文先前所述的技術，查詢包含此範例案例所需色彩及深度來源的 [**MediaFrameSourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup)。 選取所要的畫面來源群組之後，取得每個畫面來源的 [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo)。

[!code-cs[SelectColorAndDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColorAndDepth)]

將選取的畫面來源群組傳入初始化設定，建立和初始化 **MediaCapture** 物件。

[!code-cs[MultiFrameInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameInitMediaCapture)]

初始化 **MediaCapture** 物件後，擷取彩色攝影機和景深相機的 [**MediaFrameSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) 物件。 儲存每個來源的識別碼，這樣您就可以選取對應來源的到達畫面。

[!code-cs[GetColorAndDepthSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetColorAndDepthSource)]

呼叫 [**CreateMultiSourceFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createmultisourceframereaderasync) 並傳遞讀取器使用的畫面來源陣列，以建立和初始化 **MultiSourceMediaFrameReader**。 註冊 [**FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.FrameArrived) 事件的事件處理常式。 此範例會建立本文之前所述 **FrameRenderer** 協助程式類別的執行個體，將畫面呈現至 **Image** 控制項。 呼叫 [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.StartAsync) 以啟動畫面讀取器。

註冊範例先前宣告之 **CorellationFailed** 事件的事件處理常式。 我們會通知這個事件，正在使用的其中一個畫面來源是否停止產生畫面。 最後，呼叫 [**Task.Run**](https://msdn.microsoft.com/library/hh195051.aspx) 以呼叫不同執行緒上的逾時協助程式方法 **NotifyAboutCorrelationFailure**。 本文稍後會示範此方法的實作。

[!code-cs[InitMultiFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMultiFrameReader)]

每當所有由 **MultiSourceMediaFrameReader** 管理的媒體畫面來源有新的畫面時，都會引發 **FrameArrived** 事件。 這表示將會按照最慢媒體來源的頻率引發事件。 如果某個來源在較慢來源產生一個畫面的同時產生多個畫面，將會捨棄該快速來源的額外畫面。 

呼叫 [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.TryAcquireLatestFrame) 以取得與事件相關聯的 [**MultiSourceMediaFrameReference**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference)。 呼叫 [**TryGetFrameReferenceBySourceId**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference.trygetframereferencebysourceid) 並傳入初始化畫面讀取器時所儲存的識別碼字串，以取得與每個媒體畫面來源相關聯的 **MediaFrameReference**。

呼叫 **ManualResetEventSlim** 物件的 [**Set**](https://msdn.microsoft.com/library/system.threading.manualreseteventslim.set.aspx) 方法以發出畫面已到達的通知訊號。 我們將會在執行於不同執行緒的 **NotifyCorrelationFailure** 方法中檢查這個事件。 

最後，在與時間相互關聯的媒體畫面上執行任何處理。 此範例只是顯示來自深度來源的畫面。

[!code-cs[MultiFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameArrived)]

啟動畫面讀取器後，**NotifyCorrelationFailure** 協助程式方法已在不同的執行緒中執行。 在此方法中，檢查是否發出了畫面已收到事件的通知訊號。 請記住，在 **FrameArrived** 處理常式中，只要有一組相互關聯的畫面到達，我們就設定此事件。 如果事件有一段應用程式定義的時間 (5 秒為合理值) 沒有收到通知訊號，而且使用 **CancellationToken** 並未取消工作，那麼很可能其中一個媒體畫面來源已停止讀取畫面。 在這種情況下，您通常需要關閉畫面讀取器，以便引發應用程式定義的 **CorrelationFailed** 事件。 在此事件的處理常式中，您可以依照本文先前所述的方式，停止畫面讀取器並清理其相關聯的資源。

[!code-cs[NotifyCorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetNotifyCorrelationFailure)]

[!code-cs[CorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCorrelationFailure)]

## <a name="use-buffered-frame-acquisition-mode-to-preserve-the-sequence-of-acquired-frames"></a>使用緩衝的畫面擷取模式來保留取得框架的順序
從 Windows 10 版本 1709 開始，您可以設定 **MediaFrameReader** 或 **MultiSourceMediaFrameReader**  的 **[AcquisitionMode](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.AcquisitionMode)** 屬性為 **Buffered**，來保留從畫面來源傳遞至您應用程式的畫面順序。

[!code-cs[SetBufferedFrameAcquisitionMode](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSetBufferedFrameAcquisitionMode)]

在預設擷取模式 **Realtime** 中，如果您的應用程式仍在處理先前畫面的 **FrameArrived** 時從來源取得多個畫面，系統會將最近取得的畫面傳送給您的應用程式，並捨棄緩衝中等待的其他畫面。 這樣可隨時提供最新的可用畫面給您的應用程式。 這通常是適用於即時視覺電腦應用程式的最有用模式。 

在 **Buffered** 擷取模式中，系統會將所有畫面保留在緩衝中，並透過 **FrameArrived** 事件依收到的順序將這些畫面提供給您的應用程式。 請注意，在此模式下，系統的畫面緩衝區已滿時，系統將會停止取得新畫面，直到您的應用程式完成先前畫面的 **FrameArrived** 事件，釋放緩衝區的更多空間。

## <a name="use-mediasource-to-display-frames-in-a-mediaplayerelement"></a>使用 MediaSource 在 MediaPlayerElement 中顯示畫面
從 Windows 版本 1709 開始，您可以在 XAML 頁面的 **[MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)** 控制項中直接顯示從 **MediaFrameReader** 取得的畫面。 這是透過使用 **[MediaSource.CreateFromMediaFrameSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** 來建立 **[MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)** 物件 (可供 **MediaPlayerElement** 相關 **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** 直接使用) 來達成。 如需使用 **MediaPlayer** 與 **MediaPlayerElement** 的詳細資訊，請參閱[使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)。

下列程式碼範例顯示在 XAML 頁面上同時顯示前方和後方相機畫面的簡單實作。

首先，新增兩個 **MediaPlayerElement** 控制項到您的 XAML 頁面。

[!code-xml[MediaPlayerElement1XAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetMediaPlayerElement1XAML)]

[!code-xml[MediaPlayerElement2XAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetMediaPlayerElement2XAML)]

接下來，使用本文先前章節所述的技術，為前端面板和背面面板上的彩色相機選取包含 **MediaFrameSourceInfo** 物件的 **MediaFrameSourceGroup**。 請注意，**MediaPlayer** 不會自動將畫面的非色彩格式 (例如景深或紅外線資料) 轉換成色彩資料。 使用其他感應器類型可能會產生無法預期的結果。 

[!code-cs[MediaSourceSelectGroup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceSelectGroup)]

初始化 **MediaCapture** 物件，以使用所選的 **MediaFrameSourceGroup**。

[!code-cs[MediaSourceInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceInitMediaCapture)]

最後，使用相關 **MediaFrameSourceInfo** 物件的 **[Id](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.Id)** 屬性來選取 **MediaCapture** 物件之 **[FrameSources](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.FrameSources)** 集合中的其中一個畫面來源，以呼叫 **[MediaSource.CreateFromMediaFrameSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** 來為每個畫面來源建立 **MediaSource**。 透過呼叫 **[SetMediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.MediaPlayer)**，初始化新的 **MediaPlayer** 物件，並將它指派給 **MediaPlayerElement**。 然後設定 **[Source](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Source)** 屬性為新建立的 **MediaSource** 物件。

[!code-cs[MediaSourceMediaPlayer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceMediaPlayer)]

## <a name="use-video-profiles-to-select-a-frame-source"></a>使用視訊設定檔來選取畫面來源

以 [**MediaCaptureVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926694) 物件表示的相機設定檔，代表特定裝置提供的一組功能，例如畫面播放速率、解析度或 HDR 擷取等進階功能。 擷取裝置可能支援多個設定檔，讓您可以選取最適合您擷取案例的設定檔。 從 Windows 10 版本 1803 開始，在初始化 **MediaCapture** 物件前，您可以使用 **MediaCaptureVideoProfile** 來選取具有特殊功能的媒體畫面來源。 下列範例方法會尋找支援寬色域 (WCG) HDR 的視訊設定檔，並傳回 **MediaCaptureInitializationSettings** 物件，可用來初始化 **MediaCapture** 以使用所選裝置和設定檔。

首先，呼叫 [**MediaFrameSourceGroup.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.FindAllAsync) 以取得目前裝置上可用的所有媒體畫面來源群組清單。 循環顯示每個來源群組並呼叫 [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) 以取得支援所指定設定檔之目前來源群組的所有視訊設定檔清單，在此例中是 HDR 含 WCG 相片。 如果找到符合條件的設定檔，則建立新的 **MediaCaptureInitializationSettings** 物件，並將 **VideoProfile** 設定為所選設定檔以及將 **VideoDeviceId** 設定為目前媒體畫面來源群組的 **Id** 屬性。

[!code-cs[GetSettingsWithProfile](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetSettingsWithProfile)]

如需使用相機設定檔的詳細資訊，請參閱[相機設定檔](camera-profiles.md)。

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [相機畫面範例](http://go.microsoft.com/fwlink/?LinkId=823230)
 

 




