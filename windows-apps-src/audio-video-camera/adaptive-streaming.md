---
author: drewbatgit
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: 本文說明如何將彈性資料流多媒體內容播放新增到通用 Windows 平台 (UWP) app。 本功能目前支援 HTTP 即時資料流 (HLS) 與 HTTP 動態資料流 (DASH) 內容播放。
title: 彈性資料流
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ef8e3ab4abd9ee9159dc7d5aa757f55e00817a51
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/03/2018
ms.locfileid: "5996358"
---
# <a name="adaptive-streaming"></a>彈性資料流


本文說明如何將彈性串流多媒體內容播放新增到通用 Windows 平台 (UWP) 應用程式。 本功能支援 HTTP 即時串流 (HLS) 與 HTTP 動態串流 (DASH) 內容播放。 從 Windows 10 版本 1803 開始，**[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** 支援平滑串流處理。

如需支援的 HLS 通訊協定標記的清單，請參閱 [HLS 標記支援](hls-tag-support.md)。 

如需所支援 DASH 設定檔的清單，請參閱 [DASH 設定檔支援](dash-profile-support.md)。 

> [!NOTE] 
> 本文中的程式碼是採用 UWP [彈性資料流範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming)的程式碼。

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>使用 MediaPlayer 與 MediaPlayerElement 的簡易彈性資料流 

若要在 UWP app 中播放彈性資料流媒體，請建立一個指向 DASH 或 HLS 資訊清單檔案的 **Uri** 物件。 建立 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 類別的執行個體。 呼叫 [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) 以建立新的 **MediaSource** 物件，然後將它設定為 **MediaPlayer** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 屬性。 呼叫 [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play) 以開始播放媒體內容。

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

上述範例會播放媒體內容的音訊，但不會自動轉譯您 UI 中的內容。 播放視訊內容的多數應用程式都會想轉譯 XAML 頁面中的內容。  若要這樣做，請新增 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) 控制項到您的 XAML 頁面。

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

呼叫 [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) 以從 DASH 或 HLS 資訊清單檔案的 URI 建立 **MediaSource**。 接著設定 **MediaPlayerElement** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420) 屬性。 **MediaPlayerElement** 將會自動為內容建立一個新的 **MediaPlayer** 物件。 您可以在 **MediaPlayer** 上呼叫 **Play** 以開始播放內容。

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> 從 Windows 10 版本 1607 開始，建議您使用 **MediaPlayer** 類別來播放媒體項目。 **MediaPlayerElement** 是輕量型的 XAML 控制項，可用來轉譯 XAML 頁面中的 **MediaPlayer** 內容。 **MediaElement** 控制項仍持續受支援，以提供回溯相容性。 如需使用 **MediaPlayer** 與 **MediaPlayerElement** 播放媒體內容的詳細資訊，請參閱[使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)。 如需使用 **MediaSource** 和相關 API 來處理媒體內容的詳細資訊，請參閱[媒體項目、播放清單和曲目](media-playback-with-mediasource.md)。

## <a name="adaptive-streaming-with-adaptivemediasource"></a>使用 AdaptiveMediaSource 的彈性資料流

如果您的 app 需要更多進階彈性資料流功能 (例如提供自訂 HTTP 標頭、監視目前下載與播放位元速率，或調整判斷系統切換彈性資料流位元速率時機的比率)，請使用 **[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** 物件。

可在 [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279) 命名空間中找到彈性資料流 API。 本文中的範例使用下列命名空間中的 API。

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

## <a name="initialize-an-adaptivemediasource-from-a-uri"></a>從 URI 初始化 AdaptiveMediaSource。

透過呼叫 [**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261)，使用彈性資料流資訊清單檔的 URI 初始化 **AdaptiveMediaSource**。 此方法傳回的 [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) 值會讓您知道是否順利建立媒體來源。 如果是這樣，您可以將物件設定為 **MediaPlayer** 的串流來源，方式為呼叫 [**MediaSource.CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource.AdaptiveMediaSource)，再將其指派給媒體播放器的 [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Source) 屬性，以建立 **MediaSource** 物件。 在這個範例中，會查詢 [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) 屬性來判斷串流的位元速率支援上限，然後將該值設為初始位元速率。 這個範例也會暫存本文稍後討論之數個 **AdaptiveMediaSource** 事件的處理常式。

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

## <a name="initialize-an-adaptivemediasource-using-httpclient"></a>使用 HttpClient 初始化 AdaptiveMediaSource

如果您需要設定自訂的 HTTP 標頭以取得資訊清單檔，您可以建立 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 物件、設定想要的標題，然後將物件傳遞到 **CreateFromUriAsync** 的多載中。

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

當系統即將從伺服器擷取資源時，會引發 [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) 事件。 傳遞至事件處理常式的 [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935) 會公開提供要求之資源的相關資訊 (例如資源類型和 URI) 的內容。

## <a name="modify-resource-request-properties-using-the-downloadrequested-event"></a>使用 DownloadRequested 事件修改資源要求屬性

您可以使用 **DownloadRequested** 事件處理常式，透過更新事件引數所提供之 [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) 物件的內容，來修改資源要求。 在下列範例中，會透過更新結果物件的 [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) 內容來修改要從中擷取資源的 URI。 您也可以重新撰寫媒體區段的位元組範圍偏移和長度，或者，如下列範例所示，變更資源 URI 以下載完整的資源，並將位元組範圍偏移和長度設定為空值。

您可以透過設定結果物件的 [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) 或 [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) 屬性，來覆寫要求之資源的內容。 在下列範例中，會透過設定 **Buffer** 屬性來取代資訊清單資源的內容。 請注意，如果您是使用非同步取得的資料來更新資源要求 (例如從遠端伺服器或非同步使用者驗證擷取資料)，您必須呼叫 [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936) 取得延遲，然後在作業完成時呼叫 [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934)，通知系統可以繼續下載要求作業。

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

## <a name="use-bitrate-events-to-manage-and-respond-to-bitrate-changes"></a>使用 bitrate 事件管理和回應位元速率變更

**AdaptiveMediaSource** 物件提供了可讓您在下載或播放位元速率變更時用來反應的事件。 在此範例中，目前的位元速率僅在 UI 中更新。 請注意，您可以修改判斷系統切換彈性資料流位元速率時機的比率。 如需詳細資訊，請參閱 [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697) 屬性。

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="handle-download-completion-and-failure-events"></a>處理下載完成和失敗事件
所要求資源的下載失敗時，**AdaptiveMediaSource** 物件會引發 [**DownloadFailed**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadFailed) 事件。 您可以使用這個事件更新 UI，以回應失敗。 您也可以使用事件來記錄下載作業和失敗的統計資訊。 

傳入事件處理常式的 [**AdaptiveMediaSourceDownloadFailedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs) 物件包含失敗資源下載的中繼資料，例如資源類型、資源 URI，以及資料流內發生失敗的位置。 [**RequestId**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.RequestId) 取得要求的系統產生的唯一識別碼，這個要求可用來關聯多個事件中個人要求的狀態資訊。

[**Statistics**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.Statistics) 屬性傳回 [**AdaptiveMediaSourceDownloadStatistics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadstatistics) 物件，以提供發生事件時所收到的位元組數目，以及下載作業中各種里程碑的時間。 您可以記錄這項資訊，以找出彈性資料流實作的效能問題。

[!code-cs[AMSDownloadFailed](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadFailed)]


[**DownloadCompleted**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadCompleted) 事件是在資源下載完成時發生，並將類似的資料提供給 **DownloadFailed** 事件。 同樣地，提供 **RequestId** 來關聯單一要求的事件。 此外，提供 **AdaptiveMediaSourceDownloadStatistics** 物件來啟用下載統計資料的記錄。

[!code-cs[AMSDownloadCompleted](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadCompleted)]

## <a name="gather-adaptive-streaming-telemetry-data-with-adaptivemediasourcediagnostics"></a>使用 AdaptiveMediaSourceDiagnostics 收集彈性資料流遙測資料
**AdaptiveMediaSource** 公開 [**Diagnostics**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource?branch=master.Diagnostics) 屬性，以傳回 [**AdaptiveMediaSourceDiagnostics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics)物件。 使用這個物件註冊 [**DiagnosticAvailable**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics.DiagnosticAvailable) 事件。 這個事件適用於遙測收集，不應該用來修改執行階段的應用程式行為。 有許多不同的原因會引發這個診斷事件。 檢查傳入事件的 [**AdaptiveMediaSourceDiagnosticAvailableEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs) 物件的 [**DiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs.DiagnosticType) 屬性，以判斷引發事件的原因。 可能的原因包含存取所要求資源的錯誤以及剖析資料流資訊清單檔案的錯誤。 如需可觸發診斷事件的情況清單，請參閱 [**AdaptiveMediaSourceDiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostictype)。 與其他彈性資料流事件的引數一樣，**AdaptiveMediaSourceDiagnosticAvailableEventArgs** 提供 **RequestId** 屬性來關聯不同事件之間的要求資訊。

[!code-cs[AMSDiagnosticAvailable](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDiagnosticAvailable)]

## <a name="defer-binding-of-adaptive-streaming-content-for-items-in-a-playback-list-by-using-mediabinder"></a>使用 MediaBinder 延遲繫結播放清單中項目的彈性資料流內容
[**MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder) 類別可讓您延期繫結 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) 中的媒體內容。 從 Windows 10 版本 1703 開始，您可以提供 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 作為繫結的內容。 延遲繫結彈性媒體來源的程序大部分與繫結其他類型的媒體相同，如[媒體項目、播放清單與曲目](media-playback-with-mediasource.md)中所述。 

建立 **MediaBinder** 執行個體、設定應用程式所定義的 [**權杖**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token) 字串以找出要繫結的內容，以及註冊 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding) 事件。 呼叫 [**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder)，以從 **Binder** 建立 **MediaSource**。 然後，從 **MediaSource** 建立 **MediaPlaybackItem**，並將它新增到播放清單。

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

在 **Binding**事件處理常式、使用權杖字串找出要繫結的內容，然後呼叫**[CreateFromStreamAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromstreamasync)** 或**[CreateFromUriAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync)** 中的其中一個多載來建立彈性媒體來源。 因為這些是非同步方法，所以您應該先呼叫[**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral)方法，指示系統等候作業完成，再繼續進行。  呼叫**[SetAdaptiveMediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)**，將彈性媒體來源設定為繫結的內容。 最後，在作業完成之後呼叫[**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete)，指示系統繼續進行。

[!code-cs[BinderBindingAMS](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBindingAMS)]

如果您想要將事件處理常式註冊為繫結的彈性媒體來源，則可以在 **MediaPlaybackList** 之[**CurrentItemChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.CurrentItemChanged)事件的處理常式中執行這項作業。 [**CurrentMediaPlaybackItemChangedEventArgs.NewItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.NewItem)屬性會將目前播放的新**MediaPlaybackItem**包含在清單中。 取得代表新項目的**AdaptiveMediaSource**執行個體，方法是存取**MediaPlaybackItem**的[**Source**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.Source)屬性，然後存取媒體來源的[**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.AdaptiveMediaSource)屬性。 如果新的播放項目不是**AdaptiveMediaSource**，則這個屬性會是空值，因此您應該先測試是否為空值，再嘗試註冊任何物件事件的處理常式。

[!code-cs[AMSBindingCurrentItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAMSBindingCurrentItemChanged)]

## <a name="related-topics"></a>相關主題
* [媒體播放](media-playback.md)
* [HLS 標記支援](hls-tag-support.md) 
* [Dash 設定檔支援](dash-profile-support.md) 
* [使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)
* [在背景播放媒體](background-audio.md) 





