---
author: drewbatgit
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: "本文說明如何使用 MediaPlayer 在您的通用 Windows App 中播放媒體。"
title: "使用 MediaPlayer 播放音訊和視訊"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 30a84f9b36c8ce3ba339f6526ecb8f801f585492
ms.lasthandoff: 02/08/2017

---

# <a name="play-audio-and-video-with-mediaplayer"></a>使用 MediaPlayer 播放音訊和視訊

本文說明如何使用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 類別在您的通用 Windows app 中播放媒體。 在 Windows 10 (版本 1607) 中，媒體播放 API 有了重大改進，包括已針對背景音效簡化單一處理程序設計，自動整合系統媒體傳輸控制項 (SMTC)、能夠同步處理多個媒體播放器、能夠使用 Windows.UI.Composition 表面、可在您的內容中建立和排程媒體中斷的簡單介面。 若要充分利用這些改進的功能，對於媒體播放的建議最佳做法是使用 **MediaPlayer** 類別來播放媒體，而不是 **MediaElement**。 已經引入精簡的 XAML 控制項 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)，讓您可以在 XAML 頁面中轉譯媒體內容。 許多 **MediaElement** 提供的播放控制項和狀態 API，都已經可以透過新的 [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) 物件取得。 **MediaElement** 會繼續支援回朔相容性，但不會再為此類別新增功能。

本文會逐步說明一般媒體播放 App 中將使用的 **MediaPlayer** 功能。 請注意，**MediaPlayer** 對於所有媒體項目都是使用 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) 類別當作容器。 這個類別可讓您使用同一個介面，從許多不同的來源載入和播放媒體，這些來源包括本機檔案、記憶體資料流，以及網路來源。 也有可搭配 **MediaSource** 使用的高層級類別，像是 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) 和 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList)，它們提供更多進階功能，如播放清單，及管理包含多個音訊、視訊和中繼資料播放軌的媒體來源。 如需 **MediaSource** 和相關 API 的詳細資訊，請參閱[媒體項目、播放清單和曲目](media-playback-with-mediasource.md)。


##<a name="play-a-media-file-with-mediaplayer"></a>使用 MediaPlayer 播放媒體檔案  
使用 **MediaPlayer** 的基本媒體播放非常容易實作。 首先，建立新的 **MediaPlayer** 類別執行個體。 您可以同時有多個作用中的 **MediaPlayer** 執行個體。 接著，將播放器的 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 屬性設為實作 [**IMediaPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.IMediaPlaybackSource) 的物件，例如 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource)、[**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem)，或 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList)。 在此範例中，**MediaSource** 是由 App 本機存放區中的檔案建立，然後 **MediaPlaybackItem** 是由來源建立並指派到播放器的 **Source** 屬性。

不同於 **MediaElement**，**MediaPlayer** 預設不會自動開始播放。 您可以呼叫 [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play)、將 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) 屬性設為 true，或等候使用者以內建的媒體控制項初始化播放來開始播放。

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

當您的 App 不再使用 **MediaPlayer** 時，您應該呼叫 [**Close**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Close) 方法 (對應 C# 中的 **Dispose**) 來清除播放器使用的資源。

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

##<a name="use-mediaplayerelement-to-render-video-in-xaml"></a>在 XAML 中使用 MediaPlayerElement 轉譯視訊
您可以在 **MediaPlayer** 中播放媒體，而不在 XAML 中顯示，但許多媒體播放 app 將會想要在 XAML 頁面中轉譯媒體。 若要這麼做，請使用精簡的 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) 控制項。 就像 **MediaElement** 一樣，**MediaPlayerElement** 可讓您指定是否顯示內建的傳輸控制項。

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

您可以呼叫 [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764) 來設定該元素繫結的 **MediaPlayer** 執行個體。

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

您可以設定 **MediaPlayerElement** 上的播放來源，然後該元素就會自動使用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.MediaPlayer) 屬性建立您可以存取的新 **MediaPlayer** 執行個體。

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

> [!NOTE] 
> 如果您透過將 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) 設定為 false 來停用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 的 [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager)，它將會破壞 **MediaPlayer** 和由 **MediaPlayerElement** 所提供的 [**TransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.TransportControls) 之間的連結，使內建傳輸控制項無法繼續自動控制播放器的播放。 您必須改為實作自己的控制項以控制 **MediaPlayer**。

##<a name="common-mediaplayer-tasks"></a>常見的 MediaPlayer 工作
本節說明如何使用 **MediaPlayer** 的一些功能。

###<a name="set-the-audio-category"></a>設定音訊類別
將 **MediaPlayer** 的 [**AudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioCategory) 屬性設為[**MediaPlayerAudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerAudioCategory) 列舉的其中一個值，讓系統知道您播放的媒體是何種類型。 遊戲應將其音樂資料流的類別設為 **GameMedia**，這樣如果有其他應用程式於背景播放音樂，遊戲音樂就會自動靜音。 音樂或影片應用程式應將其資料流的類別設為 **Media** 或 **Movie**，使它們的優先順序高於 **GameMedia** 資料流。

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

###<a name="output-to-a-specific-audio-endpoint"></a>輸出到特定的音訊端點
根據預設，來自 **MediaPlayer** 的音訊輸出會路由到系統的預設音訊端點，但是您可以指定 **MediaPlayer** 應用於輸出的特定音訊端點。 在以下範例中，[**MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.MediaDevice.GetAudioRenderSelector) 傳回可唯一地識別裝置音訊轉譯器類別的字串。 接下來會呼叫 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation) 方法 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync)，以取得所選類型的所有可用裝置清單。 您可以透過程式設計方式來決定要使用的裝置，或者將傳回的裝置加入 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox) 讓使用者能夠選取裝置。

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

在裝置下拉式方塊的 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) 事件中，**MediaPlayer** 的 [**AudioDevice**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioDevice) 屬性是設為所選的裝置，其儲存在 **ComboBoxItem** 的 [**Tag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Tag) 屬性中。

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

###<a name="playback-session"></a>播放工作階段
如本文先前所述，**MediaElement** 類別公開的許多函式已經移動到 [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) 類別。 這包括播放器的播放狀態的相關資訊，例如，目前的播放位置、播放器已經暫停或正在播放，以及目前的播放速度。 **MediaPlaybackSession** 也提供數個事件，可在狀態變更時通知您，包括播放中內容目前的緩衝和下載狀態，以及目前播放中視訊內容的原始大小與外觀比例。

以下範例說明如何實作可以往前略過 10 秒的按一下按鈕處理常式。 首先，播放器的 **MediaPlaybackSession** 物件是和 [**PlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.PlaybackSession) 屬性一同抓取。 接著將 [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position) 屬性設為目前的播放位置加 10 秒。

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

下一個範例說明如何透過設定工作階段的 [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.PlaybackRate) 屬性，以在一般播放速度和 2 倍播放速度之間切換。

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

###<a name="pinch-and-zoom-video"></a>捏合和縮放視訊
**MediaPlayer** 可讓您指定視訊內容內應轉譯的來源矩形，以有效地允許您放大視訊。 您指定的矩形是相對於標準化的矩形 (0,0,1,1) 其中 0,0 是畫面的左上方位置，1,1 是指定畫面的完整寬度和高度。 舉例來說，若要縮放矩形，以轉譯視訊的右上方四分之一，您需要指定矩形 (.5,0,.5,.5)。  請務必檢查您的值，以確定來源矩形在 (0,0,1,1) 標準化矩形範圍內。 嘗試設定此範圍外的值會造成擲回例外狀況。

若要實作使用多點觸控手勢的捏合和縮放，您必須先指定要支援的手勢。 在此範例中，需要縮放和平移手勢。 當其中一個設定的手勢出現時，會引發 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.ManipulationDelta) 事件。 將會使用 [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) 事件來將縮放比例重設為完整畫面。 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

接下來，宣告將儲存目前縮放來源矩形的 **Rect** 物件。

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

**ManipulationDelta** 處理常式會調整縮放矩形的縮放或平移。 如果差異縮放值不是 1，表示使用者是執行捏合手勢。 如果值大於 1，來源矩形應變小以放大內容。 如果值小於 1，則來源矩形應變大以縮小內容。 設定新的縮放值之前，系統會檢查產生的矩形，以確定它完全在 (0,0,1,1) 的限制範圍內。

如果縮放值是 1，則會處理平移手勢。 矩形的平移是根據手勢移動的像素數目除以控制項的寬度和高度來計算。 同樣地，系統會檢查產生的矩形，以確定它在 (0,0,1,1) 的限制範圍內。

最後，**MediaPlaybackSession** 的 [**NormalizedSourceRect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NormalizedSourceRect) 會設為剛調整好的新矩形，以指定視訊畫面中應轉譯的區域。

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

在 [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) 事件處理常式中，來源矩形是設回 (0,0,1,1)，使整個視訊畫面都會轉譯。

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]
        
##<a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>使用 MediaPlayerSurface 將視訊轉譯到 Windows.UI.Composition 表面
從 Windows 10 (版本 1607) 開始，您可以使用 **MediaPlayer** 來將視訊轉譯到 [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ICompositionSurface)，可讓播放器與 [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition) 命名空間中的 API 相互溝通。 組合架構可讓您在 XAML 和低層級 DirectX 圖形 API 之間處理圖形。 這能夠用於將視訊轉譯到任何 XAML 控制項等案例。 如需使用組合 API 的詳細資訊，請參閱[視覺層](https://msdn.microsoft.com/windows/uwp/graphics/visual-layer)。

以下範例說明如何將影片播放器內容轉譯到 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Canvas) 控制項上。 此範例中媒體播放器專屬的呼叫是 [**SetSurfaceSize**](https://msdn.microsoft.com/library/windows/apps/mt489968) 和 [**GetSurface**](https://msdn.microsoft.com/library/windows/apps/mt489963)。 **SetSurfaceSize** 會告訴系統應配置的緩衝區大小以用於轉譯內容。 **GetSurface** 會接受 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.Compositor) 作為引數，並抓取 [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) 類別的執行個體。 這個類別會提供 **MediaPlayer** 和 **Compositor** 的存取權，以用來建立表面，並透過 [**CompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface.CompositionSurface) 屬性來公開表面本身。

範例中其餘的程式碼會建立 [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual)，它會轉譯視訊並將大小設為將顯示視覺的畫布元素大小。 接著從 [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) 建立 [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.CompositionBrush)，並指派給視覺的 [**Brush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual.Brush) 屬性。 然後建立 [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ContainerVisual)，並在其視覺化樹狀結構頂端插入 **SpriteVisual**。 最後，呼叫 [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/mt608981) 以將容器視覺指派到 **Canvas**。

[!code-cs[撰寫器](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
##<a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>使用 MediaTimelineController 來跨多個播放器同步內容。
如本文中先前所討論，您的 App 可以同時有數個作用中的 **MediaPlayer** 物件。 根據預設，您建立的每個 **MediaPlayer** 都是獨立運作。 在某些情況下 (例如將講評的播放軌與視訊同步)，您可能會想要同步播放器狀態、播放位置，以及多個播放器的播放速度。 從 Windows 10 (版本 1607) 開始，您可以使用 [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) 類別來實作這個行為。

###<a name="implement-playback-controls"></a>實作播放控制項
下列範例示範如何使用 **MediaTimelineController** 控制 **MediaPlayer** 的兩個執行個體。 首先，初始化 **MediaPlayer** 的每個執行個體，並將 **Source** 設為媒體檔案。 接著，建立新的 **MediaTimelineController**。 對於每個 **MediaPlayer**，透過將 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) 屬性設為 false，來停用與每個播放器相關聯的 [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager)。 然後將 [**TimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineController) 屬性設定至時間軸控制器物件。

[!code-cs[DeclareMediaTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaTimelineController)]

[!code-cs[SetTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetTimelineController)]

**注意** [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) 提供 **MediaPlayer** 和系統媒體傳輸控制項 (SMTC) 之間的自動整合，但此自動整合無法和以 **MediaTimelineController** 控制的媒體播放器搭配使用。 因此您必須在設定播放器的時間軸控制器之前，先停用媒體播放器的命令管理員。 若沒有這麼做，會導致擲回包含以下訊息的例外狀況：「因為物件目前的狀態，已封鎖連接「媒體時間軸控制器」。」 如需媒體播放器與 SMTC 整合的詳細資訊，請參閱[與系統媒體傳輸控制項整合](integrate-with-systemmediatransportcontrols.md)。 如果您是使用 **MediaTimelineController**，您仍然可以手動控制 SMTC。 如需詳細資訊，請參閱[系統媒體傳輸控制項的手動控制](system-media-transport-controls.md)。

一旦您已經將 **MediaTimelineController** 連接到一或多個媒體播放器，就可以使用控制器所公開的方法來控制播放狀態。 以下範例呼叫 [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.Start)，讓所有相關聯的媒體播放器從媒體開頭位置開始播放。

[!code-cs[PlayButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPlayButtonClick)]

這個範例說明暫停及繼續所有已連接的媒體播放器。

[!code-cs[PauseButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPauseButtonClick)]

若要向前快轉所有已連接的媒體播放器，請將播放速度設為大於 1 的值。

[!code-cs[FastForwardButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFastForwardButtonClick)]

下一個範例示範如何使用 **Slider** 控制項來顯示時間軸控制器目前的播放位置 (相對於其中一個已連接媒體播放器內容的長度)。 首先，會建立新的 **MediaSource**，並登錄媒體來源之 [**OpenOperationCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource.OpenOperationCompleted) 的處理常式。 

[!code-cs[CreateSourceWithOpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCreateSourceWithOpenCompleted)]

**OpenOperationCompleted** 處理常式是用來當成探索媒體來源內容長度的機會。 一旦決定長度，就會將 **Slider** 控制項的最大值設定為媒體項目的總秒數。 該值是設定在對 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 的呼叫中，以確保它是在 UI 執行緒上執行。

[!code-cs[DeclareDuration](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareDuration)]

[!code-cs[OpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenCompleted)]

接下來，會登錄時間軸控制器之 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.PositionChanged) 事件的處理常式。 這會由系統定期呼叫 (約每秒 4 次)。

[!code-cs[RegisterPositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPositionChanged)]

在 **PositionChanged** 的處理常式中，滑桿的值會更新，以反映時間軸控制項的目前位置。

[!code-cs[PositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPositionChanged)]

###<a name="offset-the-playback-position-from-the-timeline-position"></a>讓播放位置對時間軸位置產生位移
某些情況下您可能會想讓與時間軸控制器相關聯的一或多個媒體播放器的播放位置，和其他播放器之間產生位移。 若要這麼做，您可以設定要位移之 **MediaPlayer** 物件的 [**TimelineControllerPositionOffset**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineControllerPositionOffset) 屬性。 以下範例使用兩個媒體播放器之內容的長度，來設定兩個滑桿控制項的最小和最大值，來和項目的長度相加及相減。  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

在每個滑桿的 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.RangeBase.ValueChanged) 事件中，每個播放器的 **TimelineControllerPositionOffset** 都設為相對應的值。

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

請注意，如果播放器的位移值是對應負的播放位置，該剪輯會保持暫停直到位移達到零，然後開始播放。 同樣地，如果位移值對應到的播放位置大於媒體項目的長度，則會顯示最後一個畫面，如同單一媒體播放器到達其內容結尾時一樣。

## <a name="related-topics"></a>相關主題
* [媒體播放](media-playback.md)
* [媒體項目、播放清單與曲目](media-playback-with-mediasource.md)
* [與系統媒體傳輸控制項整合](integrate-with-systemmediatransportcontrols.md)
* [建立、排程與管理媒體中斷](create-schedule-and-manage-media-breaks.md)
* [在背景播放媒體](background-audio.md)





 

 





