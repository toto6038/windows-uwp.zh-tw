---
author: drewbatgit
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: "本文說明如何將彈性資料流多媒體內容播放新增到通用 Windows 平台 (UWP) app。 本功能目前支援 HTTP 即時串流 (HLS) 與 HTTP 動態資料流 (DASH) 內容播放。"
title: "彈性資料流"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 3afd0440d8e552ebc3459c5fe30dd766db3ae8b9
ms.lasthandoff: 02/07/2017

---

# <a name="adaptive-streaming"></a>彈性資料流

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本文說明如何將彈性資料流多媒體內容播放新增到通用 Windows 平台 (UWP) app。 本功能目前支援 HTTP 即時資料流 (HLS) 與 HTTP 動態資料流 (DASH) 內容播放。

如需支援的 HLS 通訊協定標記的清單，請參閱 [HLS 標記支援](hls-tag-support.md)。 

> [!NOTE] 
> 本文中的程式碼是採用 UWP [彈性資料流範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming)的程式碼。

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>使用 MediaPlayer 與 MediaPlayerElement 的簡易彈性資料流 

若要在 UWP app 中播放彈性資料流媒體，請建立一個指向 DASH 或 HLS 資訊清單檔案的 **Uri** 物件。 建立 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 類別的執行個體。 呼叫 [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) 以建立新的 **MediaSource** 物件，然後將它設定為 **MediaPlayer** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 屬性。 呼叫 [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play) 以開始播放媒體內容。

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

上述範例會播放媒體內容的音訊，但不會自動轉譯您 UI 中的內容。 播放視訊內容的多數應用程式都會想轉譯 XAML 頁面中的內容。  若要這樣做，請新增 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) 控制項到您的 XAML 頁面。

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

呼叫 [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) 以從 DASH 或 HLS 資訊清單檔案建立 **MediaSource**。 接著設定 **MediaPlayerElement** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420) 屬性。 **MediaPlayerElement** 將會自動為內容建立一個新的 **MediaPlayer** 物件。 您可以在 **MediaPlayer** 上呼叫 **Play** 以開始播放內容。

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> 從 Windows 10 版本 1607 開始，建議您使用 **MediaPlayer** 類別來播放媒體項目。 **MediaPlayerElement** 是輕量型的 XAML 控制項，可用來轉譯 XAML 頁面中的 **MediaPlayer** 內容。 **MediaElement** 控制項仍持續受支援，以提供回溯相容性。 如需使用 **MediaPlayer** 與 **MediaPlayerElement** 播放媒體內容的詳細資訊，請參閱[使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)。 如需使用 **MediaSource** 和相關 API 來處理媒體內容的詳細資訊，請參閱[媒體項目、播放清單和曲目](media-playback-with-mediasource.md)。

## <a name="adaptive-streaming-with-adaptivemediasource"></a>使用 AdaptiveMediaSource 的彈性資料流

如果您的 app 需要更多進階彈性資料流功能 (例如提供自訂 HTTP 標頭、監視目前下載與播放位元速率，或調整判斷系統切換彈性資料流位元速率時機的比率)，請使用 [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912) 物件。

可在 [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279) 命名空間中找到彈性資料流 API。

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

透過呼叫 [**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261)，使用彈性資料流資訊清單檔的 URI 初始化 **AdaptiveMediaSource**。 此方法傳回的 [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) 值會讓您知道是否順利建立媒體來源。 如果已建立，您可以呼叫 [**SetMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn652653)，將該物件設為 **MediaPlayer** 的串流來源。 在這個範例中，會查詢 [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) 屬性來判斷串流的位元速率支援上限，然後將該值設為初始位元速率。 此範例也會登錄 [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272)、[**DownloadBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931269) 和 [**PlaybackBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931278) 事件的處理常式，稍後會於本文章中討論。

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

如果您需要設定自訂的 HTTP 標頭以取得資訊清單檔，您可以建立 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 物件、設定想要的標題，然後將物件傳遞到 **CreateFromUriAsync** 的多載中。

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

當系統即將從伺服器擷取資源時，會引發 [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) 事件。 傳遞至事件處理常式的 [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935) 會公開提供要求之資源的相關資訊 (例如資源類型和 URI) 的內容。

您可以使用 **DownloadRequested** 事件處理常式，透過更新事件引數所提供之 [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) 物件的內容，來修改資源要求。 在下列範例中，會透過更新結果物件的 [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) 內容來修改要從中擷取資源的 URI。

您可以透過設定結果物件的 [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) 或 [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) 屬性，來覆寫要求之資源的內容。 在下列範例中，會透過設定 **Buffer** 屬性來取代資訊清單資源的內容。 請注意，如果您是使用非同步取得的資料來更新資源要求 (例如從遠端伺服器或非同步使用者驗證擷取資料)，您必須呼叫 [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936) 取得延遲，然後在作業完成時呼叫 [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934)，通知系統可以繼續下載要求作業。

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

**AdaptiveMediaSource** 物件提供了可讓您在下載或播放位元速率變更時用來反應的事件。 在此範例中，目前的位元速率僅在 UI 中更新。 請注意，您可以修改判斷系統切換彈性資料流位元速率時機的比率。 如需詳細資訊，請參閱 [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697) 屬性。

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="related-topics"></a>相關主題
* [媒體播放](media-playback.md)
* [HLS 標記支援](hls-tag-support.md) 
* [使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)
* [在背景播放媒體](background-audio.md) 






