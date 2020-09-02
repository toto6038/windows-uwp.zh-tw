---
ms.assetid: 0309c7a1-8e4c-4326-813a-cbd9f8b8300d
description: 本文示範如何建立、排程及管理媒體播放 app 的媒體中斷。
title: 建立、排程與管理媒體中斷
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4c3d9743b3c82b935029eeb0ee4c5b9347251fc2
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363971"
---
# <a name="create-schedule-and-manage-media-breaks"></a>建立、排程與管理媒體中斷

本文示範如何建立、排程及管理媒體播放 app 的媒體中斷。 媒體中斷通常是用來將音訊或視訊廣告插入媒體內容。 從 Windows 10 版本 1607 開始，您可以使用 [**MediaBreakManager**](/uwp/api/Windows.Media.Playback.MediaBreakManager) 類別，快速且輕鬆地將媒體中斷新增至任何您用來播放 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 的 [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem)。


當您排程一或多個媒體中斷之後，系統就會自動在播放期間的指定時間播放您的媒體內容。 **MediaBreakManager** 提供事件，讓您的 app 可以在媒體中斷開始、結束，或當使用者略過它們時加以回應。 您也可以針對媒體中斷存取 [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession)，以監視下載與緩衝處理進度更新等事件。

## <a name="schedule-media-breaks"></a>排程媒體中斷
每個 **MediaPlaybackItem** 物件都有自己的 [**MediaBreakSchedule**](/uwp/api/Windows.Media.Playback.MediaBreakSchedule)，您可以在播放項目時，用來設定將播放的媒體中斷。 在 app 中使用媒體中斷的第一個步驟是為您的主要播放內容建立 [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem)。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetMoviePlaybackItem":::

如需使用 **MediaPlaybackItem**、[**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) 及其他基本媒體播放 API 的詳細資訊，請參閱[媒體項目、播放清單與曲目](media-playback-with-mediasource.md)。

下一個範例示範如何將預載中斷新增到 **MediaPlaybackItem**，這表示系統將在播放媒體中斷所屬的項目之前，先播放該中斷。 首先，將新的 [**MediaBreak**](/uwp/api/Windows.Media.Playback.MediaBreak) 物件具現化。 在這個範例中，使用 [**MediaBreakInsertionMethod.Interrupt**](/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod) 來呼叫建構函式，這表示在播放中斷內容時，將暫停主要內容。 

接下來，針對要在中斷期間播放的內容 (例如廣告) 建立新的 **MediaPlaybackItem**。 此播放項目的 [**CanSkip**](/uwp/api/windows.media.playback.mediaplaybackitem.canskip) 屬性會設定為 false。 這表示使用者無法使用內建的媒體控制項來略過此項目。 您的 app 仍然可藉由呼叫 [**SkipCurrentBreak**](/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak)，以程式設計方式選擇略過新增。 

媒體中斷的 [**PlaybackList**](/uwp/api/windows.media.playback.mediabreak.playbacklist) 屬性是 [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList)，可讓您以播放清單形式播放多個媒體項目。 從清單的 **Items** 集合中新增一或多個 **MediaPlaybackItem** 物件，將它們包含於媒體中斷的播放清單中。

最後，使用主要內容播放項目的 [**BreakSchedule**](/uwp/api/windows.media.playback.mediaplaybackitem.breakschedule) 屬性來排程媒體中斷。 藉由將中斷指派給排程物件的 [**PrerollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.prerollbreak) 屬性，來指定要做為預載中斷的中斷。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetPreRollBreak":::

您現在可以播放主要媒體項目，而您建立的媒體中斷將會在主要內容之前播放。 建立新的 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 物件，然後選擇性地將 [**AutoPlay**](/uwp/api/windows.media.playback.mediaplayer.autoplay) 屬性設為 true，以便自動開始播放。 將 **MediaPlayer** 的 [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) 屬性設為您的主要內容播放項目。 這不是必要動作，但您可以將 **MediaPlayer** 指派給 [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)，以轉譯 XAML 頁面中的媒體。 如需使用 **MediaPlayer** 的詳細資訊，請參閱[使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetPlay":::

使用與預載中斷相同的技術，新增要在包含您主要內容的 **MediaPlaybackItem** 完成播放之後播放的預載中斷，不同之處在於您會將 **MediaBreak** 物件指派給 [**PostrollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.postrollbreak) 屬性。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetPostRollBreak":::

您也可以排程一或多個插播中斷，其會在播放主要內容期間的特定時間播放。 在下列範例中，[**MediaBreak**](/uwp/api/Windows.Media.Playback.MediaBreak) 是使用接受 **TimeSpan** 物件的建構函式多載所建立，它會指定在播放中斷時，主要媒體項目播放期間內的時間。 同樣地，會指定 [**MediaBreakInsertionMethod.Interrupt**](/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod)，以指出中斷播放時，將暫停主要內容的播放。 您可以呼叫 [**InsertMidrollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.insertmidrollbreak) 來將插播中斷新增到排程。 您也可以存取 [**MidrollBreaks**](/uwp/api/windows.media.playback.mediabreakschedule.midrollbreaks) 屬性，來取得排程中目前插播中斷的唯讀清單。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetMidrollBreak":::

下一個示範的插播中斷範例會使用 [**MediaBreakInsertionMethod.Replace**](/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod) 插入方法，這表示系統將在中斷播放時繼續處理主要內容。 這個選項通常用於即時資料流處理媒體 app，您不想在播放廣告時暫停內容，而使得內容與即時資料流產生落差。 

這個範例也會使用 [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 建構函式的多載，此建構函式會接受兩個 [**TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan) 參數。 第一個參數指定媒體中斷項目內將開始播放的起點。 第二個參數指定將播放媒體中斷項目的持續時間。 因此，在下列範例中，**MediaBreak** 將會在主要內容播放 20 分鐘後開始播放。 當它播放時，媒體項目將會從中斷媒體項目開始啟動 30 秒，並且將在主要媒體內容繼續播放之前播放 15 秒。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetMidrollBreak2":::

## <a name="skip-media-breaks"></a>略過媒體中斷
如本文先前所述，您可以設定 **MediaPlaybackItem** 的 [**CanSkip**](/uwp/api/windows.media.playback.mediaplaybackitem.canskip) 屬性，來防止使用者略過內建控制項的內容。 不過，您隨時都可以從程式碼中呼叫 [**SkipCurrentBreak**](/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak) 來略過目前的中斷。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetSkipButtonClick":::

## <a name="handle-mediabreak-events"></a>處理 MediaBreak 事件

有數個與媒體中斷相關的事件，您可加以登錄，以便根據媒體中斷的變更狀態來採取動作。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterMediaBreakEvents":::

[**BreakStarted**](/uwp/api/windows.media.playback.mediabreakmanager.breakstarted) 會在媒體中斷啟動時引發。 您可能想要更新 UI，讓使用者知道正在播放媒體中斷內容。 這個範例使用傳入處理常式的 [**MediaBreakStartedEventArgs**](/uwp/api/Windows.Media.Playback.MediaBreakStartedEventArgs)，來取得啟動媒體中斷的參照。 接著，使用 [**CurrentItemIndex**](/uwp/api/windows.media.playback.mediaplaybacklist.currentitemindex) 屬性，來判斷正在播放媒體中斷播放清單中的哪一個媒體項目。 然後更新 UI，以便向使用者顯示目前的廣告索引，以及中斷內剩餘的廣告數目。 請記住，更新 UI 必須在 UI 執行緒上進行，因此，您應該在 [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 內進行此呼叫。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBreakStarted":::

當中斷內的所有媒體項目已完成播放或略過時，即會引發 [**BreakEnded**](/uwp/api/windows.media.playback.mediabreakmanager.breakended)。 您可以使用這個事件的處理常式來更新 UI，以指出不再播放媒體中斷內容。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBreakEnded":::

當使用者在 [**CanSkip**](/uwp/api/windows.media.playback.mediaplaybackitem.canskip) 為 true 的項目播放期間按下內建 UI 中的 *Next* 按鈕時，或者當您在程式碼中呼叫 [**SkipCurrentBreak**](/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak) 略過中斷時，即會引發 **BreakSkipped** 事件。

下列範例使用 **MediaPlayer** 的 [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) 屬性，針對主要內容取得媒體項目的參照。 略過的媒體中斷隸屬於此項目的中斷排程。 接下來，程式碼會檢查已略過的媒體中斷是否與設定為排程 [**PrerollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.prerollbreak) 屬性的媒體中斷相同。 如果是，這表示預載中斷就是已略過的中斷，在此情況下，會建立新的插播中斷，並排程在主要內容播放 10 分鐘時播放。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBreakSkipped":::

當主要媒體項目的播放位置通過了一或多個媒體中斷的排程時間時，即會引發 [**BreaksSeekedOver**](/uwp/api/windows.media.playback.mediabreakmanager.breaksseekedover)。 下列範例會檢查是否有多個媒體中斷被試圖結束、播放位置是否已向前移動，以及其向前移動的時間是否少於 10 分鐘。 如果是，被試圖結束的第一個中斷 (從事件引數公開的 [**SeekedOverBreaks**](/uwp/api/windows.media.playback.mediabreakseekedovereventargs.seekedoverbreaks) 集合所取得) 可藉由呼叫 **MediaPlayer.BreakManager** 的 [**PlayBreak**](/uwp/api/windows.media.playback.mediabreakmanager.playbreak) 方法立即播放。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBreakSeekedOver":::


## <a name="access-the-current-playback-session"></a>存取目前的播放工作階段
[**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) 物件會使用 **MediaPlayer** 類別，來提供與目前播放的媒體內容相關的資料和事件。 [**MediaBreakManager**](/uwp/api/Windows.Media.Playback.MediaBreakManager) 也會有 **MediaPlaybackSession**，您可以用來取得與正在播放的媒體中斷內容明確相關的資料和事件。 您可以從播放工作階段取得的資訊包括目前的播放狀態 (播放或暫停)，以及內容中目前的播放位置。 如果媒體中斷內容的外觀比例與您的主要內容不同，您可以使用 [**NaturalVideoWidth**](/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideowidth) 和 [**NaturalVideoHeight**](/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideoheight) 屬性及 [**NaturalVideoSizeChanged**](/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideosizechanged) 來調整視訊 UI。 您也可以接收像是 [**BufferingStarted**](/uwp/api/windows.media.playback.mediaplaybacksession.bufferingstarted)、[**BufferingEnded**](/uwp/api/windows.media.playback.mediaplaybacksession.bufferingended) 及 [**DownloadProgressChanged**](/uwp/api/windows.media.playback.mediaplaybacksession.downloadprogresschanged) 等事件，這些事件可以提供關於您 app 效能的重要遙測資料。

下列範例會登錄適用於 **BufferingProgressChanged 事件**的處理常式；在事件處理常式中，它會更新 UI 以顯示目前的緩衝處理進度。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterBufferingProgressChanged":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBufferingProgressChanged":::

## <a name="related-topics"></a>相關主題
* [媒體播放](media-playback.md)
* [使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)
* [系統媒體傳輸控制項的手動控制項](system-media-transport-controls.md)

 

 
