---
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: 本文示範如何使用 MediaSource 提供一個常見的方法來參考和播放來自不同來源 (例如本機或遠端檔案) 的媒體，並且公開一個常見的模型來存取媒體資料 (不論使用什麼基礎媒體格式)。
title: 媒體項目、播放清單與曲目
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3c3929c2b3765bd90dbe0be687834e94b4f222fc
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8348456"
---
# <a name="media-items-playlists-and-tracks"></a>媒體項目、播放清單與曲目


 本文示範如何使用 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) 類別提供一個常見的方法來參考和播放來自不同來源 (例如本機或遠端檔案) 的媒體，並且公開一個常見的模型來存取媒體資料 (不論使用什麼基礎媒體格式)。 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) 類別延伸了 **MediaSource** 的功能，可讓您針對包含在媒體項目中的多個音訊、視訊及中繼資料播放軌進行管理和選取。 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) 可讓您從一或多個媒體播放項目建立播放清單。


## <a name="create-and-play-a-mediasource"></a>建立和播放 MediaSource

呼叫類別所公開的其中一個 Factory 方法以建立新的 **MediaSource** 執行個體：

-   [**CreateFromAdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930906)
-   [**CreateFromIMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn965527)
-   [**CreateFromMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930907)
-   [**CreateFromMseStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930908)
-   [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)
-   [**CreateFromStream**](https://msdn.microsoft.com/library/windows/apps/dn930910)
-   [**CreateFromStreamReference**](https://msdn.microsoft.com/library/windows/apps/dn930911)
-   [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912)
-   [**CreateFromDownloadOperation**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromdownloadoperation)

建立 **MediaSource** 之後，透過設定 [**Source**](https://msdn.microsoft.com/library/windows/apps/dn987010) 屬性，即可使用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535) 進行播放。 從 Windows10 版本 1607 開始，您可以透過呼叫 [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764) 來指派 **MediaPlayer** 到 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)，以在 XAML 頁面中轉譯媒體播放程式內容。 這是優於使用 **MediaElement** 的慣用方法。 如需使用 **MediaPlayer** 的詳細資訊，請參閱[**使用 MediaPlayer 播放音訊和視訊**](play-audio-and-video-with-mediaplayer.md)。

下列範例示範如何使用 **MediaSource** 來播放 **MediaPlayer** 中使用者選取的媒體檔案。

您將需要包含 [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) 和 [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) 命名空間，才能完成這個案例。

[!code-cs[Using](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetUsing)]

宣告類型為 **MediaSource** 的變數。 在本文的範例中，是將媒體來源宣告為類別成員，以便能夠從多個位置存取它。

[!code-cs[DeclareMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaSource)]

宣告一個變數以儲存 **MediaPlayer** 物件，另外，如果您想以 XAML 轉譯媒體內容，請在您的頁面新增一個 **MediaPlayerElement** 控制項。

[!code-cs[DeclareMediaPlayer](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-xml[MediaPlayerElement](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMediaPlayerElement)]

若要允許使用者挑選要播放的媒體檔案，請使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)。 使用從選擇器的 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) 方法傳回的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件，藉由呼叫 [**MediaSource.CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)，初始化一個新的 MediaObject。 最後，藉由呼叫 [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085) 方法，將媒體來源設定為 **MediaElement** 的播放來源。

[!code-cs[PlayMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaSource)]

根據預設，設定媒體來源後，**MediaPlayer** 不會自動開始播放。 您可以透過呼叫 [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play)，以手動方式開始播放。

[!code-cs[Play](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

您也可以將 **MediaPlayer** 的 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) 屬性設為 true，以告知玩家媒體來源設定後即會開始播放。

[!code-cs[AutoPlay](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAutoPlay)]

### <a name="create-a-mediasource-from-a-downloadoperation"></a>從 DownloadOperation 建立 MediaSource
從 Windows 版本 1803 開始，您可以從 **DownloadOperation** 建立 **MediaSource** 物件。

[!code-cs[CreateMediaSourceFromDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetCreateMediaSourceFromDownload)]

請注意，雖然您可以從下載建立 **MediaSource** 而不啟動它或將其 **IsRandomAccessRequired** 屬性設定為 true，但您仍必須執行這兩項作業，才能嘗試將 **MediaSource** 連接到 **MediaPlayer** 或 **MediaPlayerElement** 以進行播放。

[!code-cs[StartDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetStartDownload)]


## <a name="handle-multiple-audio-video-and-metadata-tracks-with-mediaplaybackitem"></a>使用 MediaPlaybackItem 處理多個音訊、視訊及中繼資料曲目

使用 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905) 來進行播放相當便利，因為它提供一個可從不同種類的來源播放媒體的常見方法，但是從 **MediaSource** 建立 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) 則可執行更進階的動作。 這包括能夠存取和管理媒體項目的多個音訊、視訊及資料曲目。

宣告變數來儲存您的 **MediaPlaybackItem**。

[!code-cs[DeclareMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackItem)]

呼叫建構函式並傳入已初始化的 **MediaSource** 物件來建立 **MediaPlaybackItem**。

如果您的 app 支援一個媒體播放項目中有多個音訊、視訊或資料播放軌，請為 [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948)、[**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) 或 [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) 事件註冊事件處理常式。

最後，將 **MediaElement** 或 **MediaPlayer** 的播放來源設定為您的 **MediaPlaybackItem**。

[!code-cs[PlayMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackItem)]

> [!NOTE] 
> 一個 **MediaSource** 只能與一個 **MediaPlaybackItem** 關聯。 從來源建立 **MediaPlaybackItem** 之後，嘗試從相同的來源建立另一個播放項目將會導致錯誤。 此外，從媒體來源建立 **MediaPlaybackItem** 之後，您就不能將 **MediaSource** 物件直接設定為 **MediaPlayer** 的來源物件，而是應該改用 **MediaPlaybackItem**。

將包含多個視訊曲目的 **MediaPlaybackItem** 指派為播放來源之後，將會引發 [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) 事件，並且如果因項目變更而導致視訊曲目清單發生變更，就會再次引發該事件。 這個事件的處理常式可讓您更新 UI，以允許使用者在可用的播放軌之間切換。 這個範例使用 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) 來顯示可用的視訊播放軌。

[!code-xml[VideoComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetVideoComboBox)]

請在 **VideoTracksChanged** 處理常式中，循環處理播放項目之 [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) 清單中的所有播放軌。 針對每個播放軌，將會建立一個新的 [**ComboBoxItem**](https://msdn.microsoft.com/library/windows/apps/br209349)。 如果播放軌尚未擁有標籤，將會從播放軌索引產生標籤。 下拉式方塊項目的 [**Tag**](https://msdn.microsoft.com/library/windows/apps/br208745) 屬性是設定為播放軌索引，以便稍後可供識別。 最後，該項目會新增到下拉式方塊中。 請注意，這些操作是在 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 呼叫內執行，因為所有 UI 變更都必須在 UI 執行緒上進行，而這個事件是在不同的執行緒上引發。

[!code-cs[VideoTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksChanged)]

在下拉式方塊的 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 處理常式中，會從所選項目的 **Tag** 屬性抓取播放軌索引。 設定媒體播放項目之 [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) 清單的 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn956634) 屬性會使 **MediaElement** 或 **MediaPlayer** 將使用中的視訊播放軌切換到指定的索引。

[!code-cs[VideoTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksSelectionChanged)]

管理含有多個音軌的媒體項目與管理含有視訊播放軌的媒體項目方式完全相同。 請處理 [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948)，以使用在播放項目的 [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn930947) 清單中找到的音軌更新您的 UI。 當使用者選取音軌時，設定 **AudioTracks** 清單的 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn930937) 屬性以使 **MediaElement** 或 **MediaPlayer** 將使用中的音軌切換到指定的索引。

[!code-xml[AudioComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetAudioComboBox)]

[!code-cs[AudioTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged)]

[!code-cs[AudioTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksSelectionChanged)]

除了音訊和視訊之外，**MediaPlaybackItem** 物件可能會包含零個或多個 [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) 物件。 定時中繼資料播放軌可以包含字幕或輔助字幕文字，也可以包含您 app 專屬的自訂資料。 定時中繼資料播放軌包含由繼承 [**IMediaCue**](https://msdn.microsoft.com/library/windows/apps/dn930899) 的物件 (例如 [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) 或 [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655)) 所代表的提示清單。 每個提示都有一個開始時間和持續時間，用來決定何時啟用提示及要持續多久。

與音軌和視訊播放軌類似，透過處理 **MediaPlaybackItem** 的 [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) 事件，即可探索媒體項目的定時中繼資料播放軌。 不過，使用定時中繼資料播放軌時，使用者可能會想要一次啟用多個中繼資料播放軌。 此外，視您的 app 情況而定，您可能會想要自動啟用或停用中繼資料播放軌，而不需要使用者介入。 為了方便說明，這個範例會為媒體項目中的每個中繼資料播放軌新增 [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/br209795)，以允許使用者啟用和停用播放軌。每個按鈕的 **Tag** 屬性都設定為相關中繼資料播放軌的索引，以便在切換按鈕時能夠加以識別。

[!code-xml[MetaStackPanel](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMetaStackPanel)]

[!code-cs[TimedMetadataTrackschanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedMetadataTrackschanged)]

由於一次可以有多個中繼資料播放軌處於使用中，因此您不是直接設定中繼資料播放軌清單的使用中索引。 而是改為呼叫 **MediaPlaybackItem** 物件的 [**SetPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn986977) 方法，藉此傳入您想要切換之播放軌的索引，然後從 [**TimedMetadataTrackPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn987016) 列舉提供值。 您選擇的呈現模式取決於您 app 的實作。 在這個範例中，中繼資料播放軌在啟用時會設定為 **PlatformPresented**。 就文字型播放軌而言，這表示系統會自動顯示播放軌中的文字提示。當切換按鈕被關閉時，呈現模式會設定成 **Disabled**，這表示不會顯示任何文字，也不會引發任何提示事件。 本文稍後會討論提示事件。

[!code-cs[ToggleChecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleChecked)]

[!code-cs[ToggleUnchecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleUnchecked)]

當您處理中繼資料曲目時，您可以透過存取 [**Cues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.Cues) 或 [**ActiveCues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.ActiveCues) 屬性來存取曲目內的提示集。 這樣做可以更新 UI 以顯示媒體項目的提示位置。

## <a name="handle-unsupported-codecs-and-unknown-errors-when-opening-media-items"></a>處理開啟媒體項目時不支援的轉碼器和不明的錯誤
從 Windows10 版本 1607 開始，您可以檢查執行您的應用程式所在的裝置是否支援或部分支援播放媒體項目需要的轉碼器。 在 **MediaPlaybackItem** 曲目變更事件 (例如 [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracksChanged)) 的事件處理常式中，先檢查曲目變更是否為插入新曲目。如果是，您可以透過使用在 **IVectorChangedEventArgs.Index** 參數中傳入的索引與適當的 **MediaPlaybackItem** 參數曲目集合 (例如 [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracks) 集合)，以取得插入中曲目的參照。

取得已插入曲目的參照後，請檢查該曲目的 [**SupportInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.SupportInfo) 屬性的 [**DecoderStatus**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrackSupportInfo.DecoderStatus)。 如果值為 [**FullySupported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus)，則播放曲目需要的適當轉碼器會顯示在裝置上。 如果值為 [**Degraded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus)，系統雖然可以播放曲目，但播放品質會降低。 例如，5.1 的音訊曲目可能會改播放為雙通道立體聲。 如果是這種情況，您可能想更新您的 UI 以提醒使用者播放品質降低。 如果值為 [**UnsupportedSubtype**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus) 或 [**UnsupportedEncoderProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus)，則無法使用裝置上目前的轉碼器來播放曲目。 您可能想要對使用者發出警示並略過播放的項目，或是實作 UI 以供使用者下載正確的轉碼器。 曲目的 [**GetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.GetEncodingProperties) 方法可用來判斷播放所需的轉碼器。

最後，您可以註冊曲目的 [**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed) 事件，如果裝置支援播放該曲目，但因為管線中不明的錯誤而無法開啟，則會引發此事件。

[!code-cs[AudioTracksChanged_CodecCheck](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged_CodecCheck)]

在 [**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed) 事件處理常式中，您可以檢查 **MediaSource** 狀態是否為不明，如果是，您可以程式設計方式選取其他曲目進行播放、讓使用者能選擇其他曲目或中止播放。

[!code-cs[OpenFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetOpenFailed)]

## <a name="set-display-properties-used-by-the-system-media-transport-controls"></a>設定系統媒體傳輸控制項使用的顯示屬性
從 Windows10 版本 1607 開始，[**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 中播放的媒體預設會自動整合到系統媒體傳輸控制項 (SMTC)。 您可以更新 **MediaPlaybackItem** 的顯示屬性，以指定 SMTC 將顯示的中繼資料。 透過呼叫 [**GetDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.GetDisplayProperties)，以取得代表項目顯示屬性的物件。 透過設定 [**Type**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.Type) 屬性，以設定播放項目是音樂或視訊。 接著設定物件的 [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.VideoProperties) 或 [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.MusicProperties) 屬性。 呼叫 [**ApplyDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/mt489923) 以將項目的屬性更新為您提供的值。 一般而言，App 會以動態方式擷取 Web 服務中的值，但下列範例會示範此程序搭配硬式編碼值。

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]

## <a name="add-external-timed-text-with-timedtextsource"></a>使用 TimedTextSource 新增外部定時文字

在某些情況下，您可能會有包含與媒體項目關聯之定時文字的外部檔案，例如包含不同地區設定之字幕的個別檔案。 使用 [**TimedTextSource**](https://msdn.microsoft.com/library/windows/apps/dn956679) 類別以從串流或 URI 載入外部定時文字檔案。

這個範例使用 **Dictionary** 集合來儲存媒體項目的定時文字來源清單，其中會使用來源 URI 和 **TimedTextSource** 物件做為「機碼/值」組，以在解析播放軌之後識別播放軌。

[!code-cs[TimedTextSourceMap](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceMap)]

呼叫 [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn708190) 來為每個外部定時文字檔案建立一個新的 **TimedTextSource**。 在 **Dictionary** 中為定時文字來源新增一個項目。 為 [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) 事件新增一個處理常式，以處理在順利載入項目之後，項目無法載入或設定其他屬性的情況。

將您所有的 **TimedTextSource** 物件新增到 [**ExternalTimedTextSources**](https://msdn.microsoft.com/library/windows/apps/dn930916) 集合，以向 **MediaSource** 註冊這些物件。 請注意，外部定時文字來源會直接新增到 **MediaSource**，而不是新增到從來源建立的 **MediaPlaybackItem**。 若要更新您的 UI 以反映外部文字播放軌，請依照本文先前所述的方式註冊並處理 **TimedMetadataTracksChanged** 事件。

[!code-cs[TimedTextSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSource)]

在 [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) 事件的處理常式中，檢查傳遞給處理常式之 [**TimedTextSourceResolveResultEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn965537) 的 **Error** 屬性，以判斷嘗試載入定時文字資料時是否發生錯誤。 如果已順利解析項目，您可以使用這個處理常式來更新已解析之播放軌的其他屬性。這個範例會根據先前儲存在 **Dictionary** 中的 URI，為每個播放軌新增一個標籤。

[!code-cs[TimedTextSourceResolved](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceResolved)]

## <a name="add-additional-metadata-tracks"></a>新增其他中繼資料播放軌

您可以在程式碼中動態建立自訂的中繼資料播放軌，然後將它們與媒體來源建立關聯。 您建立的播放軌可以包含字幕或輔助字幕文字，也可包含您的專屬 App 資料。

呼叫建構函式並指定識別碼、語言識別碼及來自 [**TimedMetadataKind**](https://msdn.microsoft.com/library/windows/apps/dn956578) 列舉的值，以建立新的 [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580)。 註冊 [**CueEntered**](https://msdn.microsoft.com/library/windows/apps/dn956583) 和 [**CueExited**](https://msdn.microsoft.com/library/windows/apps/dn956584) 事件的處理常式。 當已達到提示的開始時間，以及當提示的持續時間已過期時，將會分別引發這些事件。

為您建立的中繼資料播放軌建立適用其類型的新提示物件，並設定播放軌的識別碼、開始時間及持續時間。這個範例會建立一個資料播放軌，因此會產生一組 [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) 物件，並為每個提示提供一個包含 app 特定資料的緩衝區。 若要註冊新播放軌，請將它新增到 **MediaSource** 物件的 [**ExternalTimedMetadataTracks**](https://msdn.microsoft.com/library/windows/apps/dn930915) 集合。

從 Windows 10 版本 1703 開始，**DataCue.Properties**屬性公開可用來以索引鍵/資料配對儲存自訂屬性的[**PropertySet**](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset)，而這些配對可以在**CueEntered**和**CueExited**事件中進行擷取。  

[!code-cs[AddDataTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddDataTrack)]

只要相關播放軌的呈現模式為 **ApplicationPresented**、**Hidden** 或 **PlatformPresented**，當達到提示的開始時間時，便會引發 **CueEntered** 事件。 如果播放軌的呈現模式為 **Disabled** 時，則不會為中繼資料播放軌引發提示事件。 這個範例只會將與提示關聯的自訂資料輸出到偵錯視窗。

[!code-cs[DataCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDataCueEntered)]

這個範例會藉由在建立播放軌時指定 **TimedMetadataKind.Caption**，並使用 [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) 物件將提示新增到播放軌，來新增自訂文字播放軌。

[!code-cs[AddTextTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddTextTrack)]

[!code-cs[TextCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTextCueEntered)]

## <a name="play-a-list-of-media-items-with-mediaplaybacklist"></a>使用 MediaPlaybackList 播放媒體項目清單

[**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) 可讓您建立媒體項目 (由 **MediaPlaybackItem** 物件代表) 的播放清單。

**注意：** [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955)中的項目會使用無間斷播放呈現。 系統會使用 MP3 或 AAC 編碼檔案中所提供的中繼資料，以判斷無間斷播放所需的延遲或間隔補償。 如果 MP3 或 AAC 編碼檔案未提供此中繼資料，則系統會啟發式地判斷延遲或間隔。 針對不失真的格式 (例如 PCM、FLAC 或 ALAC)，系統不會採取任何動作，因為這些編碼器不會導致延遲或間隔。

若要開始，請宣告變數來儲存您的 **MediaPlaybackList**。

[!code-cs[DeclareMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackList)]

使用本文先前所述的相同程序，為您想要新增到清單中的每個媒體項目建立 **MediaPlaybackItem**。 將您的 **MediaPlaybackList** 物件初始化，然後將媒體播放項目新增到此物件中。 註冊 [**CurrentItemChanged**](https://msdn.microsoft.com/library/windows/apps/dn930957) 事件的處理常式。 這個事件可讓您更新 UI 以反映目前播放的媒體項目。 您也可以註冊[ItemOpened](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemOpened)事件以在成功開啟清單中的項目時引發，以及註冊[ItemFailed](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemFailed)事件以在無法開放清單中的項目時引發。

從 Windows 10 版本 1703 開始，您可以指定**MediaPlaybackList**中系統在播放後將持續開啟的**MediaPlaybackItem**物件數目上限，方法是設定[MaxPlayedItemsToKeepOpen](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.MaxPlayedItemsToKeepOpen)屬性。 如果持續開啟**MediaPlaybackItem**，則會在使用者因不需要重新載入項目而切換至該項目時立即開始播放項目。 但持續開啟項目也會增加應用程式的記憶體耗用，因此您應該在設定此值時考慮回應性與記憶體使用之間的平衡。 

若要啟用清單的播放，請將**MediaPlayer**的播放來源設定為**MediaPlaybackList**。

[!code-cs[PlayMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackList)]

在 **CurrentItemChanged** 事件處理常式中，更新您的 UI 以反映目前正在播放的項目 (藉由使用傳遞給事件之 [**CurrentMediaPlaybackItemChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn930929) 物件的 [**NewItem**](https://msdn.microsoft.com/library/windows/apps/dn930930) 屬性即可抓取此項目)。 請記住，如果您是從這個事件更新 UI，您應該在對 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 的呼叫中執行此操作，以便在 UI 執行緒上進行更新。

從 Windows 10 版本 1703 開始，您可以檢查[CurrentMediaPlaybackItemChangedEventArgs.Reason](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.Reason)屬性來取得值，指出項目變更原因，例如以程式設計方式設計應用程式切換項目、先前播放的項目達到其結尾，或發生錯誤。

[!code-cs[MediaPlaybackListItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetMediaPlaybackListItemChanged)]


呼叫 [**MovePrevious**](https://msdn.microsoft.com/library/windows/apps/mt146455) 或 [**MoveNext**](https://msdn.microsoft.com/library/windows/apps/mt146454)，以使媒體播放器播放您 **MediaPlaybackList** 中的上一個或下一個項目。

[!code-cs[PrevButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPrevButton)]

[!code-cs[NextButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNextButton)]

設定 [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146457) 屬性，以指定媒體播放器是否應該以隨機順序播放您清單中的項目。

[!code-cs[ShuffleButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetShuffleButton)]

設定 [**AutoRepeatEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146452) 屬性，以指定媒體播放器是否應該循環播放您清單中的項目。

[!code-cs[RepeatButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRepeatButton)]


### <a name="handle-the-failure-of-media-items-in-a-playback-list"></a>處理播放清單中失敗的媒體項目
當清單中的項目無法開啟時，會引發 [**ItemFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList.ItemFailed) 事件。 可以的話，傳入處理常式的 [**MediaPlaybackItemError**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError) 物件的 [**ErrorCode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError.ErrorCode) 屬性會列舉失敗的特定原因，包括網路錯誤、解碼錯誤或加密錯誤。

[!code-cs[ItemFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetItemFailed)]

### <a name="disable-playback-of-items-in-a-playback-list"></a>停用播放清單中項目的播放
從 Windows 10 版本 1703 開始，您可以停用**MediaPlaybackItemList**中一或多個項目的播放，方法是將[MediaPlaybackItem](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem)的[IsDisabledInPlaybackList](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.IsDisabledInPlaybackList)屬性設定為 false。 

這項功能的一般案例是針對播放從網際網路串流處理之音樂的應用程式。 應用程式可以接聽裝置的網路連線狀態變更，然後停用未完全下載之項目的播放。 在下列範例中，會註冊處理常式的[NetworkInformation.NetworkStatusChanged](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged)事件。

[!code-cs[RegisterNetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRegisterNetworkStatusChanged)]

在**NetworkStatusChanged**的處理常式中，確認[GetInternetConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.GetInternetConnectionProfile)是否傳回空值，指出未連接網路。 如果是這種情況，請循環執行播放清單中的所有項目，而且如果項目的[TotalDownloadProgress](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TotalDownloadProgress)小於 1，表示尚未完全下載項目，請停用項目。 如果啟用網路連線，請循環執行播放清單中的所有項目，並啟用每個項目。

[!code-cs[NetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNetworkStatusChanged)]

### <a name="defer-binding-of-media-content-for-items-in-a-playback-list-by-using-mediabinder"></a>使用 MediaBinder 延遲繫結播放清單中項目的媒體內容
在先前的範例中，會從檔案、URL 或資料流建立**MediaSource**，之後，則會建立**MediaPlaybackItem**，並將其新增至**MediaPlaybackList**。 在某些情況下，例如，如果使用者正在付費檢視內容，您可能會想要延遲擷取**MediaSource**的內容，直到準備好實際播放播放清單中的項目。 若要實作此案例，請建立[**MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder)類別的執行個體。 將[**Token**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token)屬性設定為應用程式定義的字串，以識別您想要延遲擷取的內容，然後註冊[**Binding**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding)事件的處理常式。 接著，呼叫[**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder)，以從**Binder**建立**MediaSource**。 然後，從**MediaSource**建立**MediaPlaybackItem**，並如常將它新增至播放清單。

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

當系統判斷需要擷取與**MediaBinder**相關的內容時，會引發**Binding**事件。 在這個事件的處理常式中，您可以從傳入事件的[**MediaBindingEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs)中擷取**MediaBinder**執行個體。 擷取您為**Token**屬性指定的字串，並使用它來判斷應該擷取的內容。 **MediaBindingEventArgs**提供方法，以透過數種不同的呈現來設定繫結的內容，包括[**SetStorageFile**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstoragefile)、[**SetStream**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstream)、[**SetStreamReference**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstreamreference)和[**SetUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.seturi)。 

[!code-cs[BinderBinding](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBinding)]

請注意，如果您要在**Binding**事件處理常式中執行非同步作業，例如 Web 要求，則應該呼叫[**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral)方法，指示系統等候作業完成，再繼續進行。 在作業完成之後呼叫[**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete)，指示系統繼續進行。

從 Windows 10 版本 1703 開始，您可以將[**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource)提供為繫結的內容，方法是呼叫 [**SetAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)。 如需在應用程式中使用彈性資料流的詳細資訊，請參閱[彈性資料流](adaptive-streaming.md)。



## <a name="related-topics"></a>相關主題
* [媒體播放](media-playback.md)
* [使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)
* [與系統媒體傳輸控制項整合](integrate-with-systemmediatransportcontrols.md)
* [在背景播放媒體](background-audio.md)

