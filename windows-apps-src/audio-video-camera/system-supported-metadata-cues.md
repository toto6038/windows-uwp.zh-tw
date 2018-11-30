---
ms.assetid: F28162D4-AACC-4EE0-B243-5878F870F87F
description: 在媒體播放期間處理系統支援的中繼資料提示
title: 系統支援的定時中繼資料提示
ms.date: 04/18/2017
ms.topic: article
keywords: windows 10, uwp, metadata, cue, speech, chapter, 中繼資料, 提示, 語音, 章節
ms.localizationpriority: medium
ms.openlocfilehash: 2f461bb70c1319352c66b8d12775dc7fa1db0edf
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8329148"
---
# <a name="system-supported-timed-metadata-cues"></a>系統支援的定時中繼資料提示
本文描述如何充分利用可嵌入到媒體檔案或資料流的數種定時中繼資料格式。 UWP app 可以註冊只要發現這些中繼資料提示，媒體管線就會在播放期間引發的事件。 使用 [**DataCue**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.DataCue) 類別，應用程式可以實作其專屬自訂中繼資料提示，但本文著重在媒體管線自動偵測到的數種中繼資料標準，包括︰

* VobSub 格式的影像式字幕
* 語音提示，包括字詞界線、句子界線以及語音合成標記語言 (SSML) 書籤
* 章節提示
* 延伸 M3U 意見
* ID3 標記
* 分散的 mp4 emsg 方塊


本文是以[媒體項目、播放清單與曲目](media-playback-with-mediasource.md)文章中所討論過的概念為建置基礎，包括使用 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)、[**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) 和 [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) 類別的基本概念以及在應用程式中使用定時中繼資料的一般指導方針。

本文所述的所有不同類型的定時中繼資料，其基本實作步驟都會相同︰

1. 建立 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)，然後針對要播放的內容建立 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)。
2. 註冊 [**MediaPlaybackItem.TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件，而在媒體管線解析媒體項目的子播放軌時會發生這個事件。
3. 針對您想要使用的定時中繼資料播放軌，註冊 [**TimedMetadataTrack.CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 和 [**TimedMetadataTrack.CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。
4. 在 **CueEntered** 事件處理常式中，會根據傳入 args 事件的中繼資料來更新 UI。 您可以再次於 **CueExited** 事件中更新 UI 一次，例如，移除目前字幕文字。

在本文中，處理每種類型的中繼資料都會顯示為不同的案例，但可能會使用大部分共用的程式碼來處理 (或略過) 不同類型的中繼資料。 您可以在程序的多個點檢查 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 物件的 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 屬性。 因此，例如，您可以選擇針對值為 **TimedMetadataKind.ImageSubtitle** 的中繼資料播放軌註冊 **CueEntered** 事件，而不是針對值為 **TimedMetadataKind.Speech** 的播放軌。 或者，相反地，您可以註冊所有中繼資料播放軌類型的處理常式，然後檢查 **CueEntered** 處理常式內的 **TimedMetadataKind** 值，以判斷要回應提示所採取的動作。

## <a name="image-based-subtitles"></a>影像式字幕
從 Windows 10 版本 1703 開始，UWP app 可以支援 VobSub 格式的外接式影像式字幕。 若要使用這項功能，請先建立將顯示影像字幕之媒體內容的 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 物件。 接下來，建立 [**TimedTextSource**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource) 物件，方法是呼叫 [**CreateFromUriWithIndex**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.CreateFromUriWithIndex) 或 [**CreateFromStreamWithIndex**](https://docs.microsoft.com/uwp/api/windows.media.core.timedtextsource.CreateFromStreamWithIndex)，並傳入包含字幕影像資料之 .sub 檔案的 Uri，以及包含字幕時間資訊的 .idx 檔案。 將 **TimedTextSource** 新增到 **MediaSource**，方法是將它新增到來源的 [**ExternalTimedTextSources**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.ExternalTimedTextSources) 集合。 從 **MediaSource** 建立 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)。

[!code-cs[ImageSubtitleLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleLoadContent)]

使用上一個步驟中所建立的 **MediaPlaybackItem** 物件，註冊影像字幕中繼資料事件。 這個範例使用 Helper 方法 **RegisterMetadataHandlerForImageSubtitles** 來註冊事件。 lambda 運算式用來實作 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件的處理常式，而這個事件是在系統偵測到與 **MediaPlaybackItem** 相關聯的中繼資料播放軌變更時發生。 在某些情況下，一開始解析播放項目時，可以使用中繼資料播放軌，因此在 **TimedMetadataTracksChanged** 處理常式外部，我們也會循環執行可用的中繼資料播放軌，並呼叫 **RegisterMetadataHandlerForImageSubtitles**。

[!code-cs[ImageSubtitleTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleTracksChanged)]

在註冊影像字幕中繼資料事件之後，會將 **MediaItem** 指派給 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)，以在 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 內播放。

[!code-cs[ImageSubtitlePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitlePlay)]

在 **RegisterMetadataHandlerForImageSubtitles** Helper 方法中，取得 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 類別的執行個體，方法是編製索引到 **MediaPlaybackItem** 的 **TimedMetadataTracks** 集合。 註冊 [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 事件和 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。 接著，您必須對 **TimedMetadataTracks** 集合的播放項目呼叫 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)，以指示系統該應用程式想要收到此播放項目的中繼資料提示事件。

[!code-cs[RegisterMetadataHandlerForImageSubtitles](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForImageSubtitles)]

在 **CueEntered** 事件的處理常式中，您可以檢查傳入處理常式之 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 物件的 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 屬性，以查看中繼資料是否適用於影像字幕。 這是必要的，如果您要將相同資料提示事件處理常式用於多種類型的中繼資料。 如果相關中繼資料播放軌的類型是 **TimedMetadataKind.ImageSubtitle**，請將 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 之 **Cue** 屬性中所含的資料提示轉型為 [**ImageCue**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue)。 **ImageCue** 的 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.SoftwareBitmap) 屬性包含字幕影像的 [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap) 呈現。 建立 [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)，並呼叫 [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.SetBitmapAsync)，以將影像指派給 XAML [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image)控制項。 **ImageCue** 的 [**Extent**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.Extent) 和 [**Position**](https://docs.microsoft.com/uwp/api/windows.media.core.imagecue.Position) 屬性提供字幕影像大小和位置的相關資訊。

[!code-cs[ImageSubtitleCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleCueEntered)]

## <a name="speech-cues"></a>語音提示
從 Windows 10 版本 1703 開始，UWP app 可以登錄以接收所播放媒體中回應字詞界線、句子界線和語音合成標記語言 (SSML) 書籤的事件。 這可讓您播放使用 [**SpeechSynthesizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer) 類別所產生的音訊資料流，並根據這些事件來更新 UI，例如顯示目前播放的字詞或句子的文字。

本節中顯示的範例使用類別成員變數來儲存將合成和播放的字串。

[!code-cs[SpeechInputText](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechInputText)]

建立新的 **SpeechSynthesizer** 類別執行個體。 將合成器的 [**IncludeWordBoundaryMetadata**](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeWordBoundaryMetadata) 和 [**IncludeSentenceBoundaryMetadata**](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeSentenceBoundaryMetadata) 選項設定為 **true**，以指定其應將中繼資料包含在產生的媒體資料流中。 呼叫 [**SynthesizeTextToStreamAsync**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer.SynthesizeTextToStreamAsync)，以產生包含合成語音和對應中繼資料的資料流。 從合成資料流建立 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 和 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)。

[!code-cs[SynthesizeSpeech](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSynthesizeSpeech)]

使用 **MediaPlaybackItem** 物件註冊語音中繼資料事件。 這個範例使用 Helper 方法 **RegisterMetadataHandlerForSpeech** 來註冊事件。 lambda 運算式用來實作 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件的處理常式，而這個事件是在系統偵測到與 **MediaPlaybackItem** 相關聯的中繼資料播放軌變更時發生。  在某些情況下，一開始解析播放項目時，可以使用中繼資料播放軌，因此在 **TimedMetadataTracksChanged** 處理常式外部，我們也會循環執行可用的中繼資料播放軌，並呼叫 **RegisterMetadataHandlerForSpeech**。

[!code-cs[SpeechTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechTracksChanged)]

在註冊語音中繼資料事件之後，會將 **MediaItem** 指派給 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)，以在 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 內播放。

[!code-cs[SpeechPlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechPlay)]

在 **RegisterMetadataHandlerForSpeech** Helper 方法中，取得 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 類別的執行個體，方法是編製索引到 **MediaPlaybackItem** 的 **TimedMetadataTracks** 集合。 註冊 [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 事件和 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。 接著，您必須對 **TimedMetadataTracks** 集合的播放項目呼叫 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)，以指示系統該應用程式想要收到此播放項目的中繼資料提示事件。

[!code-cs[RegisterMetadataHandlerForWords](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForWords)]

在 **CueEntered** 事件的處理常式中，您可以檢查傳入處理常式之 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 物件的 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 屬性，以查看中繼資料是否適用於語音。 這是必要的，如果您要將相同資料提示事件處理常式用於多種類型的中繼資料。 如果相關中繼資料播放軌的類型是 **TimedMetadataKind.Speech**，請將 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 之 **Cue** 屬性中所含的資料提示轉型為 [**SpeechCue**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue)。 針對語音提示，中繼資料播放軌中所含語音提示的類型是透過檢查 **Label** 屬性所決定。 這個屬性的值會是 "SpeechWord" (適用於字詞界限)、"SpeechSentence" (適用於句子界限) 或 "SpeechBookmark" (適用於 SSML 書籤)。 在這個範例中，檢查 "SpeechWord" 值，如果找到這個值，會使用 **SpeechCue** 的 [**StartPositionInInput**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue.StartPositionInInput) 和 [**EndPositionInInput**](https://docs.microsoft.com/uwp/api/windows.media.core.speechcue.EndPositionInInput) 屬性，來判斷目前正在播放之字組的輸入文字內的位置。 這個範例只會將每個字詞輸出到偵錯輸出。

[!code-cs[SpeechWordCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechWordCueEntered)]

## <a name="chapter-cues"></a>章節提示
從 Windows 10 版本 1703 開始，UWP app 可以註冊可回應媒體項目內章節的提示。 若要使用這項功能，請建立媒體內容的 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 物件，然後從 **MediaSource** 建立 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)。

[!code-cs[ChapterCueLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueLoadContent)]

使用上一個步驟中所建立的 **MediaPlaybackItem** 物件，註冊章節中繼資料事件。 這個範例使用 Helper 方法 **RegisterMetadataHandlerForChapterCues** 來註冊事件。 lambda 運算式用來實作 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件的處理常式，而這個事件是在系統偵測到與 **MediaPlaybackItem** 相關聯的中繼資料播放軌變更時發生。 在某些情況下，一開始解析播放項目時，可以使用中繼資料播放軌，因此在 **TimedMetadataTracksChanged** 處理常式外部，我們也會循環執行可用的中繼資料播放軌，並呼叫 **RegisterMetadataHandlerForChapterCues**。

[!code-cs[ChapterCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueTracksChanged)]

在註冊章節中繼資料事件之後，會將 **MediaItem** 指派給 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)，以在 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 內播放。

[!code-cs[ChapterCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCuePlay)]

在 **RegisterMetadataHandlerForChapterCues** Helper 方法中，取得 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 類別的執行個體，方法是編製索引到 **MediaPlaybackItem** 的 **TimedMetadataTracks** 集合。 註冊 [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 事件和 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。 接著，您必須對 **TimedMetadataTracks** 集合的播放項目呼叫 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)，以指示系統該應用程式想要收到此播放項目的中繼資料提示事件。

[!code-cs[RegisterMetadataHandlerForChapterCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForChapterCues)]

在 **CueEntered** 事件的處理常式中，您可以檢查傳入處理常式之 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 物件的 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 屬性，以查看中繼資料是否適用於章節提示。這是必要的，如果您要將相同資料提示事件處理常式用於多種類型的中繼資料。 如果相關中繼資料播放軌的類型是 **TimedMetadataKind.Chapter**，請將 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 之 **Cue** 屬性中所含的資料提示轉型為 [**ChapterCue**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue)。 **ChapterCue** 的 [**Title**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue.Title) 屬性包含播放中剛剛到達的章節標題。

[!code-cs[ChapterCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueEntered)]

### <a name="seek-to-the-next-chapter-using-chapter-cues"></a>使用章節提示尋找下一章
除了在播放項目中的目前章節變更時接收通知之外，您也可以使用章節提示來尋找播放項目內的下一章。 下面顯示的範例方法接受 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 以及代表目前播放媒體項目的 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem) 作為引數。 會搜尋 [**TimedMetadataTracks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracks)集合，以查看是否有任何播放軌具有 **TimedMetadataKind.Chapter** 之 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 值的 [**TimedMetadataKind**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) 屬性。  如果找到章節播放軌，這個方法會循環執行播放軌之 [**Cues**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.Cues) 集合中的每個提示，以找到 [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.core.chaptercue.StartTime) 大於媒體播放程式之播放工作階段的目前 [**Position**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.Position) 的第一個提示。 找到正確提示之後，會更新播放工作階段的位置，並在 UI 中更新章節標題。

[!code-cs[GoToNextChapter](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetGoToNextChapter)]

## <a name="extended-m3u-comments"></a>延伸 M3U 意見
從 Windows 10 版本 1703 開始，UWP app 可以註冊可回應延伸 M3U 資訊清單檔案內意見的提示。 這個範例使用 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 播放媒體內容。 如需詳細資訊，請參閱 [彈性資料流](adaptive-streaming.md)。 建立內容的**AdaptiveMediaSource**，方法是呼叫 [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync)或[**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync)。 呼叫 [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) 來建立 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 物件，然後從 **MediaSource** 建立 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)。

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

使用上一個步驟中所建立的 **MediaPlaybackItem** 物件，註冊 M3U 中繼資料事件。 這個範例使用 Helper 方法 **RegisterMetadataHandlerForEXTM3UCues** 來註冊事件。 lambda 運算式用來實作 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件的處理常式，而這個事件是在系統偵測到與 **MediaPlaybackItem** 相關聯的中繼資料播放軌變更時發生。 在某些情況下，一開始解析播放項目時，可以使用中繼資料播放軌，因此在 **TimedMetadataTracksChanged** 處理常式外部，我們也會循環執行可用的中繼資料播放軌，並呼叫 **RegisterMetadataHandlerForEXTM3UCues**。

[!code-cs[EXTM3UCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueTracksChanged)]

在註冊 M3U 中繼資料事件之後，會將 **MediaItem** 指派給 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)，以在 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 內播放。

[!code-cs[EXTM3UCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCuePlay)]

在 **RegisterMetadataHandlerForEXTM3UCues** Helper 方法中，取得 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 類別的執行個體，方法是編製索引到 **MediaPlaybackItem** 的 **TimedMetadataTracks** 集合。 檢查中繼資料播放軌的 DispatchType 屬性，如果播放軌代表 M3U 意見，則這會有 "EXTM3U" 值。 註冊 [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 事件和 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。 接著，您必須對 **TimedMetadataTracks** 集合的播放項目呼叫 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)，以指示系統該應用程式想要收到此播放項目的中繼資料提示事件。

[!code-cs[RegisterMetadataHandlerForEXTM3UCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEXTM3UCues)]

在 **CueEntered** 事件的處理常式中，將 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 的 **Cue** 屬性中所含的資料提示轉型為 [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue)。  檢查以確定提示的 **DataCue** 和 [**Data**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Data) 屬性不是空值。 是以 UTF-16、little endian、空值終止字串的形式提供延伸 EMU 意見。 建立新的 **DataReader** 來讀取提示資料，方法是呼叫 [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer)。 將讀取器的 [**UnicodeEncoding**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.UnicodeEncoding) 屬性設定為 [**Utf16LE**](https://docs.microsoft.com/uwp/api/windows.storage.streams.unicodeencoding)，以正確的格式讀取資料。 呼叫 [**ReadString**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.ReadString) 以讀取資料，並指定 **Data** 欄位的一半長度，因為每個字元的大小都是兩個位元組，並減一以移除尾端空值字元。 在這個範例中，只會將 M3U 意見寫入偵錯輸出中。

[!code-cs[EXTM3UCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueEntered)]

## <a name="id3-tags"></a>ID3 標記
從 Windows 10 版本 1703 開始，UWP app 可以註冊對應至 Http 即時串流 (HLS) 內容內 ID3 標記的提示。 這個範例使用 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 播放媒體內容。 如需詳細資訊，請參閱 [彈性資料流](adaptive-streaming.md)。 建立內容的**AdaptiveMediaSource**，方法是呼叫 [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync)或[**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync)。 呼叫 [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) 來建立 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 物件，然後從 **MediaSource** 建立 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)。

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

使用上一個步驟中所建立的 **MediaPlaybackItem** 物件，註冊 ID3 標記事件。 這個範例使用 Helper 方法 **RegisterMetadataHandlerForID3Cues** 來註冊事件。 lambda 運算式用來實作 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件的處理常式，而這個事件是在系統偵測到與 **MediaPlaybackItem** 相關聯的中繼資料播放軌變更時發生。 在某些情況下，一開始解析播放項目時，可以使用中繼資料播放軌，因此，在**TimedMetadataTracksChanged**處理常式外部，我們也會循環執行可用的中繼資料播放軌，並呼叫**RegisterMetadataHandlerForID3Cues**。

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

在註冊 ID3 中繼資料事件之後，會將 **MediaItem** 指派給 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)，以在 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 內播放。

[!code-cs[ID3CuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CuePlay)]


在 **RegisterMetadataHandlerForID3Cues** Helper 方法中，取得 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 類別的執行個體，方法是編製索引到 **MediaPlaybackItem** 的 **TimedMetadataTracks** 集合。 檢查中繼資料播放軌的 DispatchType 屬性，如果播放軌代表 ID3 標記，則這會有包含 GUID 字串 "15260DFFFF49443320FF49443320000F" 的值。 註冊 [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 事件和 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。 接著，您必須對 **TimedMetadataTracks** 集合的播放項目呼叫 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)，以指示系統該應用程式想要收到此播放項目的中繼資料提示事件。

[!code-cs[RegisterMetadataHandlerForID3Cues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForID3Cues)]

在 **CueEntered** 事件的處理常式中，將 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 的 **Cue** 屬性中所含的資料提示轉型為 [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue)。  檢查以確定提示的 **DataCue** 和 [**Data**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Data) 屬性不是空值。 在傳輸資料流中，是以表單原始位元組提供延伸 EMU 意見 (請參閱 [http://id3.org/id3v2.4.0-structure](http://id3.org/id3v2.4.0-structure))。 建立新的 **DataReader** 來讀取提示資料，方法是呼叫 [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer)。  在此範例中，ID3 標記中的標頭值是從提示資料中進行讀取，並寫入偵錯輸出中。

[!code-cs[ID3CueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CueEntered)]

## <a name="fragmented-mp4-emsg-boxes"></a>分散的 mp4 emsg 方塊
從 Windows 10 版本 1703 開始，UWP app 可以註冊可回應分散 mp4 資料流內 emsg 方塊的提示。 這類型的中繼資料的使用範例適用於內容提供者發出用戶端應用程式訊號，以在即時串流內容期間播放廣告。 這個範例使用 [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) 播放媒體內容。 如需詳細資訊，請參閱 [彈性資料流](adaptive-streaming.md)。 建立內容的**AdaptiveMediaSource**，方法是呼叫 [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync)或[**CreateFromStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync)。 呼叫 [**CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) 來建立 [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) 物件，然後從 **MediaSource** 建立 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem)。

[!code-cs[EmsgLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgLoadContent)]

使用上一個步驟中所建立的 **MediaPlaybackItem** 物件，註冊 emsg 方塊事件。 這個範例使用 Helper 方法 **RegisterMetadataHandlerForEmsgCues** 來註冊事件。 lambda 運算式用來實作 [**TimedMetadataTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) 事件的處理常式，而這個事件是在系統偵測到與 **MediaPlaybackItem** 相關聯的中繼資料播放軌變更時發生。 在某些情況下，一開始解析播放項目時，可以使用中繼資料播放軌，因此在 **TimedMetadataTracksChanged** 處理常式外部，我們也會循環執行可用的中繼資料播放軌，並呼叫 **RegisterMetadataHandlerForEmsgCues**。

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

在註冊 emsg 方塊中繼資料事件之後，會將 **MediaItem** 指派給 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)，以在 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) 內播放。

[!code-cs[EmsgCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCuePlay)]


在 **RegisterMetadataHandlerForEmsgCues** Helper 方法中，取得 [**TimedMetadataTrack**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack) 類別的執行個體，方法是編製索引到 **MediaPlaybackItem** 的 **TimedMetadataTracks** 集合。 檢查中繼資料播放軌的 DispatchType 屬性，如果播放軌代表 emsg 方塊，則這會有 "emsg:mp4" 值。 註冊 [**CueEntered**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) 事件和 [**CueExited**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatatrack.CueExited) 事件。 接著，您必須對 **TimedMetadataTracks** 集合的播放項目呼叫 [**SetPresentationMode**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode)，以指示系統該應用程式想要收到此播放項目的中繼資料提示事件。


[!code-cs[RegisterMetadataHandlerForEmsgCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEmsgCues)]


在 **CueEntered** 事件的處理常式中，將 [**MediaCueEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediacueeventargs) 的 **Cue** 屬性中所含的資料提示轉型為 [**DataCue**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue)。  檢查以確定 **DataCue** 物件不是空值。 媒體管線會將 emsg 方塊的屬性提供為 DataCue 物件之 [**Properties**](https://docs.microsoft.com/uwp/api/windows.media.core.datacue.Properties) 集合中的自訂屬性。 本範例嘗試使用 **[TryGetValue](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset.trygetvalue)** 方法擷取數個不同的屬性值。 如果此方法傳回空值，表示 emsg 方塊中沒有所要求的屬性，因此會改為設定預設值。

這個範例的下一個部分說明觸發廣告播放的案例，這種情況是上一個步驟中取得的 *scheme_id_uri* 屬性具有 "urn:scte:scte35:2013:xml" 值 (請參閱 [http://dashif.org/identifiers/event-schemes/](http://dashif.org/identifiers/event-schemes/))。 請注意，標準會建議您傳送此 emsg 數次以進行備援，因此，此範例會維護已處理的 emsg 識別碼清單，並且只處理新訊息。 呼叫 [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.FromBuffer) 來建立新的 **DataReader** 以讀取提示資料，並設定 [**UnicodeEncoding**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.UnicodeEncoding) 屬性將編碼設定為 UTF-8，然後讀取資料。 在這個範例中，會將訊息承載寫入偵錯輸出中。 實際應用程式會使用承載資料來排程廣告的播放。

[!code-cs[EmsgCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCueEntered)]


## <a name="related-topics"></a>相關主題

* [媒體播放](media-playback.md)
* [媒體項目、播放清單與曲目](media-playback-with-mediasource.md)


 




