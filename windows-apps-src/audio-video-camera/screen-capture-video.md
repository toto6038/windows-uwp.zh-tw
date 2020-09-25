---
title: 將螢幕擷取到影片
description: 本文說明如何使用 Windows. 圖形將 Api 編碼的畫面格編碼至影片檔案。
ms.date: 07/28/2020
ms.topic: article
dev_langs:
- csharp
keywords: windows 10、uwp、螢幕擷取畫面、影片
ms.localizationpriority: medium
ms.openlocfilehash: d8f70748d025d50d19dbf2cb184ae841cced7f8a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218631"
---
# <a name="screen-capture-to-video"></a>將螢幕擷取到影片

本文說明如何使用 Windows. 圖形將 Api 編碼的畫面格編碼至影片檔案。 如需螢幕捕捉（仍為影像）的相關資訊，請參閱 [Screeen capture](./screen-capture.md)。

## <a name="overview-of-the-video-capture-process"></a>影片捕獲流程總覽
本文提供範例應用程式的逐步解說，此應用程式會將視窗內容記錄至影片檔案。 雖然可能需要很多程式碼來執行此案例，但螢幕錄製器應用程式的高階結構相當簡單。 螢幕擷取畫面進程會使用三個主要 UWP 功能：

- [GraphicsCapture](/uwp/api/windows.graphics.capture) api 會執行實際從畫面抓取圖元的工作。 [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem)類別代表要捕捉的視窗或顯示。 [GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession) 可用來啟動和停止捕捉作業。 [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool)類別會維護螢幕內容複寫到其中的框架緩衝區。
- [>mediastreamsource](/uwp/api/windows.media.core.mediastreamsource)類別會接收已捕捉的框架，並產生影片串流。
- [MediaTranscoder](/uwp/api/windows.media.transcoding.mediatranscoder)類別會接收 **>mediastreamsource**所產生的資料流程，並將其編碼成影片檔案。

本文中顯示的範例程式碼可分類成幾個不同的工作：

- **初始化** -這包括設定上述的 UWP 類別、初始化圖形裝置介面、挑選要捕捉的視窗，以及設定編碼參數，例如解析度和畫面播放速率。
- **事件處理常式和執行緒**-主要 capture 迴圈的主要驅動程式是透過[SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested)事件定期要求框架的 **>mediastreamsource** 。 此範例會使用事件來協調範例不同元件之間新框架的要求。 同步處理對於允許同時捕捉及編碼框架而言很重要。
- **複製框架** -畫面格會從 capture 框架緩衝區複製到個別的 Direct3D 介面，此介面可傳遞至 **>mediastreamsource** ，以便在編碼時不會覆寫該資源。 Direct3D Api 是用來快速執行這種複製操作。 

### <a name="about-the-direct3d-apis"></a>關於 Direct3D Api
如上所述，每個已捕獲框架的複製可能是本文中所示之實作為最複雜的部分。 在低層級中，這項作業是使用 Direct3D 完成的。 在此範例中，我們使用 [SharpDX](http://sharpdx.org/) 程式庫從 c # 執行 Direct3D 作業。 此程式庫已不再正式支援，但已選擇，因為低層級複製作業的效能適用于此案例。 我們試著盡可能將 Direct3D 作業保持為離散，讓您更輕鬆地以自己的程式碼或其他程式庫取代這些工作。

## <a name="setting-up-your-project"></a>設定您的專案
本逐步解說中的範例程式碼是使用 Visual Studio 2019 中 ** (通用 Windows) ** c # 專案範本的空白應用程式所建立。 若要在您的應用程式中使用 **Windows.** . a api，您必須在專案的 package.appxmanifest 檔案中包含 **圖形捕捉** 功能。 此範例會將產生的影片檔案儲存至裝置上的影片庫。 若要存取這個資料夾，您必須包含影片 **庫** 功能。

若要安裝 SharpDX Nuget 套件，請在 Visual Studio 選取 [ **管理 Nuget 套件**]。 在 [流覽] 索引標籤中，搜尋 "SharpDX. Direct3D11" 套件，然後按一下 [ **安裝**]。

請注意，為了縮減本文中的程式代碼清單大小，下列逐步解說中的程式碼會省略明確的命名空間參考，以及以前置底線 "_" 命名的 MainPage 類別成員變數宣告。

## <a name="setup-for-encoding"></a>編碼的設定

本節中所述的 **SetupEncoding** 方法會初始化一些主要物件，這些物件將用來捕捉及編碼影片畫面，並設定所捕獲影片的編碼參數。 您可以透過程式設計方式呼叫這個方法，或回應使用者的互動，例如按一下按鈕。 **SetupEncoding**的程式代碼清單顯示在初始化步驟的說明之後。

- **檢查是否有取得支援。** 開始進行捕捉程式之前，您必須先呼叫 [GraphicsCaptureSession IsSupported](/uwp/api/windows.graphics.capture.graphicscapturesession.issupported) ，以確定目前的裝置支援螢幕擷取畫面功能。

- **初始化 Direct3D 介面。** 此範例會使用 Direct3D 將從螢幕捕獲的圖元複製到編碼為影片框架的材質。 本文稍後會顯示用來初始化 Direct3D 介面（ **CreateD3DDevice** 和 **CreateSharpDXDevice**）的 helper 方法。

- **將 GraphicsCaptureItem 初始化。** [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem)代表螢幕上要被捕獲的專案，可能是視窗或整個畫面。 允許使用者藉由建立 [GraphicsCapturePicker](/uwp/api/windows.graphics.capture.graphicscapturepicker) 並呼叫 [PickSingleItemAsync](/uwp/api/windows.graphics.capture.graphicscapturepicker.picksingleitemasync)來挑選要捕捉的專案。

- **建立組合材質。** 建立材質資源和將用來複製每個影片畫面格的相關聯轉譯目標視圖。 您必須在建立 **GraphicsCaptureItem** 並知道其維度之後，才能建立此材質。 請參閱 **WaitForNewFrame** 的描述，以瞭解如何使用這個組合材質。 本文稍後也會顯示建立此材質的 helper 方法。

- **建立 MediaEncodingProfile 和 VideoStreamDescriptor。** [>mediastreamsource](/uwp/api/windows.media.core.mediastreamsource)類別的實例會取得從畫面中取出的影像，並將其編碼成影片串流。 然後，影片串流將會由 [MediaTranscoder](/uwp/api/windows.media.transcoding.mediatranscoder) 類別轉碼至影片檔案。 [VideoStreamDecriptor](/uwp/api/windows.media.core.videostreamdescriptor)為 **>mediastreamsource**提供編碼參數，例如解析度和畫面播放速率。 **MediaTranscoder**的影片檔案編碼參數會以[MediaEncodingProfile](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)來指定。 請注意，用於影片編碼的大小不一定要與所要捕捉的視窗大小相同，但為了讓這個範例保持簡單，編碼設定是硬式編碼，以使用 capture 專案的實際維度。

- **建立 >mediastreamsource 和 MediaTranscoder 物件。** 如先前所述， **>mediastreamsource** 物件會將個別的框架編碼成影片串流。 呼叫這個類別的函式，並傳入在上一個步驟中建立的 **MediaEncodingProfile** 。 將緩衝區時間設定為零，並註冊 [啟動](/uwp/api/windows.media.core.mediastreamsource.starting) 和 [SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested) 事件的處理常式，這將在本文稍後顯示。 接下來，請建立 **MediaTranscoder** 類別的新實例，並啟用硬體加速。

- **建立輸出** 檔此方法中的最後一個步驟，是建立將轉碼影片的目標檔案。 在此範例中，我們只會在裝置上的影片庫資料夾中建立唯一的命名檔案。 請注意，為了存取此資料夾，您的應用程式必須在應用程式資訊清單中指定「影片庫」功能。 建立檔案之後，請開啟檔案進行讀取和寫入，然後將產生的資料流程傳遞至 **EncodeAsync** 方法，以顯示下一步。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SetupEncoding":::

## <a name="start-encoding"></a>開始編碼
現在主要物件已經初始化， **EncodeAsync** 方法會實作為開始進行捕捉作業。 這個方法會先檢查以確定我們尚未錄製，如果不是，則會呼叫 helper 方法 **StartCapture** 來開始從畫面中捕獲畫面格。 本文稍後會顯示這個方法。 接下來，使用我們在上一節中建立的編碼設定檔，呼叫 [PrepareMediaStreamSourceTranscodeAsync](/uwp/api/windows.media.transcoding.mediatranscoder.preparemediastreamsourcetranscodeasync) 來取得 **MediaTranscoder** 準備好將 **>mediastreamsource** 物件所產生的影片串流轉碼至輸出檔案資料流程。 備妥轉碼程式之後，請呼叫 [TranscodeAsync](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) 以開始轉碼。 如需有關使用 **MediaTranscoder**的詳細資訊，請參閱 [轉碼媒體](./transcode-media-files.md)檔案。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_EncodeAsync":::

## <a name="handle-mediastreamsource-events"></a>處理 >mediastreamsource 事件
**>mediastreamsource**物件會採用我們從畫面捕捉的畫面格，並將其轉換成可使用**MediaTranscoder**儲存至檔案的影片串流。 我們會透過物件事件的處理常式，將畫面格傳遞給 **>mediastreamsource** 。

當 **>mediastreamsource**準備好新的影片框架時，就會引發[SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested)事件。 確定目前正在錄製之後，就會呼叫 helper 方法 **WaitForNewFrame** 來取得從畫面中捕捉到的新框架。 本文章稍後所示的這個方法會傳回包含已捕捉框架的 [ID3D11Surface](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 物件。 在此範例中，我們會將 **IDirect3DSurface** 介面包裝在協助程式類別中，此類別也會儲存捕獲框架的系統時間。 畫面格和系統時間都會傳遞至[MediaStreamSample. CreateFromDirect3D11Surface](/uwp/api/windows.media.core.mediastreamsample.createfromdirect3d11surface) factory 方法，而產生的[MediaStreamSample](/uwp/api/windows.media.core.mediastreamsample)會設定為[MediaStreamSourceSampleRequestedEventArgs](/uwp/api/windows.media.core.mediastreamsourcesamplerequestedeventargs)的[MediaStreamSourceSampleRequest 範例](/uwp/api/windows.media.core.mediastreamsourcesamplerequest.sample)屬性。 這是提供給 **>mediastreamsource**的捕獲框架的方式。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceSampleRequested":::

在 [開始](/uwp/api/windows.media.core.mediastreamsource.starting) 事件的處理常式中，我們會呼叫 **WaitForNewFrame**，但是只會將畫面格的系統時間傳遞給 [MediaStreamSourceStartingRequest. SetActualStartPosition](/uwp/api/windows.media.core.mediastreamsourcestartingrequest.setactualstartposition) 方法，而 **>mediastreamsource** 會使用該方法來適當地編碼後續框架的時間。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceStarting":::

## <a name="start-capturing"></a>開始捕獲
此步驟中所顯示的 **StartCapture** 方法是從上一個步驟中所示的 **EncodeAsync** helper 方法來呼叫。 首先，這個方法會初始化一組用來控制捕捉作業流程的事件物件。

- **_multithread** 是包裝 SharpDX 程式庫之多 **執行緒** 物件的 helper 類別，用來確保在複製時，不會有其他執行緒存取 SharpDX 材質。
- **_frameEvent** 用來表示已捕捉到新的框架，並可傳遞至 **>mediastreamsource**
- **_closedEvent** 表示記錄已停止，而且不應該等待任何新的框架。

Frame 事件和 closed 事件會新增至陣列，因此我們可以在 capture 迴圈中等候其中一個。

**StartCapture**方法的其餘部分會設定將會進行實際螢幕捕捉的 Windows. 圖形。 首先，會註冊 **CaptureItem** 的事件。 接下來會建立 [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool) ，這可讓您一次緩衝處理多個已捕獲的框架。 [CreateFreeThreaded](/uwp/api/windows.graphics.capture.direct3d11captureframepool.createfreethreaded)方法是用來建立框架組區，以便在集區自己的背景工作執行緒上呼叫[FrameArrived](/uwp/api/windows.graphics.capture.direct3d11captureframepool.framearrived)事件，而不是在應用程式的主執行緒上呼叫。 接著，會為 **FrameArrived** 事件註冊處理常式。 最後，會為選取的**CaptureItem**建立[GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession) ，並藉由呼叫[StartCapture](/uwp/api/windows.graphics.capture.graphicscapturesession)起始框架的捕獲。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_StartCapture":::

## <a name="handle-graphics-capture-events"></a>處理圖形捕獲事件
在上一個步驟中，我們為圖形捕獲事件註冊了兩個處理常式，並設定一些事件來協助管理捕捉迴圈的流程。

當**Direct3D11CaptureFramePool**有新的已捕捉畫面格可用時，就會引發**FrameArrived**事件。 在這個事件的處理常式中，呼叫寄件者上的 [TryGetNextFrame](/uwp/api/windows.graphics.capture.direct3d11captureframepool.trygetnextframe) 以取得下一個捕捉的框架。 在抓取畫面格之後，我們會設定 **_frameEvent** ，讓我們的 capture 迴圈知道有新的畫面格可用。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnFrameArrived":::

在 **關閉** 的事件處理常式中，我們會通知 **_closedEvent** ，讓 capture 迴圈知道何時停止。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnClosed":::

## <a name="wait-for-new-frames"></a>等候新的框架
本節中所述的 **WaitForNewFrame** helper 方法，是在何處進行捕捉迴圈的繁重工作。 請記住，每當 **>mediastreamsource**準備好要將新的框架新增至影片串流時，就會從**OnMediaStreamSourceSampleRequested**事件處理常式呼叫這個方法。 在高階中，此函式只會將每個螢幕上的影片畫面從一個 Direct3D 介面複製到另一個，以便在新的框架被捕獲時，將其傳遞至 **>mediastreamsource** 進行編碼。 此範例會使用 SharpDX 程式庫來執行實際的複製作業。

在等候新的框架之前，方法會處置儲存于類別變數中的任何先前的框架， **_currentFrame**，然後重設 **_frameEvent**。 然後，方法會等候 **_frameEvent** 或 **_closedEvent** 發出信號。 如果已設定關閉的事件，則應用程式會呼叫 helper 方法來清除捕獲資源。 本文稍後會顯示這個方法。

如果已設定 frame 事件，則我們知道先前步驟中所定義的 **FrameArrived** 事件處理常式已被呼叫，而我們開始將已捕捉的框架資料複製到將會傳遞至 **>mediastreamsource**的 Direct3D 11 介面中。

此範例會使用協助程式類別 **SurfaceWithInfo**，這只是讓我們將畫面格所需的影片畫面和系統時間傳遞給 **>mediastreamsource** ，作為單一物件。 畫面格複製程式的第一個步驟是將這個類別具現化，並設定系統時間。

接下來的步驟是此範例中特別依賴 SharpDX 程式庫的部分。 這裡使用的 helper 函式是在本文結尾定義。 首先，我們會使用 **MultiThreadLock** 來確保在進行複製時，不會有其他執行緒存取影片框架緩衝區。 接下來，我們會呼叫 helper 方法 **CreateSharpDXTexture2D** ，從影片框架建立 SharpDX **Texture2D** 物件。 這會是複製作業的來源紋理。 

接下來，我們會從上一個步驟中建立的 **Texture2D** 物件複製到我們稍早在流程中建立的組合材質。 此組合材質可作為交換緩衝區，讓編碼程式可以在下一個畫面格被捕捉時，以圖元為單位來運作。 為了執行複製，我們會清除與組合材質相關聯的轉譯目標視圖，然後在要複製的材質中定義區域（在此案例中為整個材質），然後呼叫 **CopySubresourceRegion** ，以實際將圖元複製到組合材質。

我們會建立一份材質描述的複本，以便在我們建立目標材質時使用，但會修改描述，將 **BindFlags** 設定為 **RenderTarget** ，讓新的材質具有寫入存取權。 將 **CpuAccessFlags** 設定為「 **無** 」可讓系統優化複製操作。 材質描述是用來建立新的材質資源，而複合材質資源會透過呼叫 **CopyResource**複製到這個新的資源。 最後，呼叫 **CreateDirect3DSurfaceFromSharpDXTexture** 來建立從這個方法傳回的 **IDirect3DSurface** 物件。


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_WaitForNewFrame":::

## <a name="stop-capture-and-clean-up-resources"></a>停止捕獲和清除資源
**Stop**方法提供停止捕捉作業的方法。 您的應用程式可能會以程式設計方式呼叫此程式，或回應使用者的互動，例如按一下按鈕。 這個方法只會設定 **_closedEvent**。 在先前的步驟中定義的 **WaitForNewFrame** 方法會尋找這個事件，如果有設定，則會關閉捕捉作業。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Stop":::

**清除**方法可用來適當處置複製作業期間所建立的資源。 這包括：

- Capture 會話所使用的 **Direct3D11CaptureFramePool** 物件
- **GraphicsCaptureSession**和**GraphicsCaptureItem**
- Direct3D 和 SharpDX 裝置
- 複製作業中所使用的 SharpDX 材質和轉譯目標視圖。
- 用於儲存目前框架的 **Direct3D11CaptureFrame** 。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Cleanup":::

## <a name="helper-wrapper-classes"></a>Helper 包裝函式類別
下列 helper 類別的定義，是為了協助進行本文中的範例程式碼。

**MultithreadLock** helper 類別會包裝 SharpDX 多**執行緒**類別，以確保在複製時，其他執行緒不會存取材質資源。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_MultithreadLock":::

**SurfaceWithInfo** 是用來將 **IDirect3DSurface** 與代表已捕捉框架的 **SystemRelativeTime** 以及個別捕獲的時間產生關聯。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SurfaceWithInfo":::

## <a name="direct3d-and-sharpdx-helper-apis"></a>Direct3D 和 SharpDX helper Api
下列協助程式 Api 的定義是為了抽象出 Direct3D 和 SharpDX 資源的建立。 這些技術的詳細說明不在本文的討論範圍內，但此處提供的程式碼可讓您執行逐步解說中所示的範例程式碼。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/Direct3D11Helpers.cs" id="snippet_Direct3D11Helpers":::

## <a name="see-also"></a>另請參閱

* [Windows.Graphics.Capture 命名空間](/uwp/api/windows.graphics.capture)
* [螢幕擷取](screen-capture.md)