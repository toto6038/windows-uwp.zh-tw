---
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: 本文說明如何使用 MediaPlayer 在您的通用 Windows App 中播放媒體。
title: 使用 MediaPlayer 播放音訊和視訊
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4ae87600c49b61e5ee426e8dd7ab33b3d3cf7ea3
ms.sourcegitcommit: c9bab19599c0eb2906725fd86d0696468bb919fa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2020
ms.locfileid: "78256151"
---
# <a name="play-audio-and-video-with-mediaplayer"></a>使用 MediaPlayer 播放音訊和視訊

本文說明如何使用 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) 類別在您的通用 Windows app 中播放媒體。 在 Windows 10 (版本 1607) 中，媒體播放 API 有了重大改進，包括已針對背景音效簡化單一處理程序設計，自動整合系統媒體傳輸控制項 (SMTC)、能夠同步處理多個媒體播放器、能夠使用 Windows.UI.Composition 表面、可在您的內容中建立和排程媒體中斷的簡單介面。 若要充分利用這些改進的功能，對於媒體播放的建議最佳做法是使用 **MediaPlayer** 類別來播放媒體，而不是 **MediaElement**。 已經引入精簡的 XAML 控制項 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)，讓您可以在 XAML 頁面中轉譯媒體內容。 許多 **MediaElement** 提供的播放控制項和狀態 API，都已經可以透過新的 [**MediaPlaybackSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackSession) 物件取得。 **MediaElement** 會繼續支援回朔相容性，但不會再為此類別新增功能。

本文會逐步說明一般媒體播放 App 中將使用的 **MediaPlayer** 功能。 請注意，**MediaPlayer** 對於所有媒體項目都是使用 [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource) 類別當作容器。 這個類別可讓您使用同一個介面，從許多不同的來源載入和播放媒體，這些來源包括本機檔案、記憶體資料流，以及網路來源。 也有可搭配 **MediaSource** 使用的高層級類別，像是 [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 和 [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList)，它們提供更多進階功能，如播放清單，及管理包含多個音訊、視訊和中繼資料播放軌的媒體來源。 如需 **MediaSource** 和相關 API 的詳細資訊，請參閱[媒體項目、播放清單和曲目](media-playback-with-mediasource.md)。

> [!NOTE] 
> Windows 10 N 和 Windows 10 KN 版不含使用 **MediaPlayer** 播放所需的媒體功能。 這些功能可以手動安裝。 如需詳細資訊，請參閱[適用於 Windows 10 N 和 Windows 10 KN 版本的 Media Feature Pack](https://support.microsoft.com/help/3010081/media-feature-pack-for-windows-10-n-and-windows-10-kn-editions)。

## <a name="play-a-media-file-with-mediaplayer"></a>使用 MediaPlayer 播放媒體檔案  
使用 **MediaPlayer** 的基本媒體播放非常容易實作。 首先，建立新的 **MediaPlayer** 類別執行個體。 您可以同時有多個作用中的 **MediaPlayer** 執行個體。 接著，將播放器的 [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source) 屬性設為實作 [**IMediaPlaybackSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.IMediaPlaybackSource) 的物件，例如 [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource)、[**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem)，或 [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList)。 在此範例中，**MediaSource** 是由 App 本機存放區中的檔案建立，然後 **MediaPlaybackItem** 是由來源建立並指派到播放器的 **Source** 屬性。

不同於 **MediaElement**，**MediaPlayer** 預設不會自動開始播放。 您可以呼叫 [**Play**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.play)、將 [**AutoPlay**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.autoplay) 屬性設為 true，或等候使用者以內建的媒體控制項初始化播放來開始播放。

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

當您的 App 不再使用 **MediaPlayer** 時，您應該呼叫 [**Close**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.close) 方法 (對應 C# 中的 **Dispose**) 來清除播放器使用的資源。

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

## <a name="use-mediaplayerelement-to-render-video-in-xaml"></a>在 XAML 中使用 MediaPlayerElement 轉譯視訊
您可以在 **MediaPlayer** 中播放媒體，而不在 XAML 中顯示，但許多媒體播放 app 將會想要在 XAML 頁面中轉譯媒體。 若要這麼做，請使用精簡的 [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) 控制項。 就像 **MediaElement** 一樣，**MediaPlayerElement** 可讓您指定是否顯示內建的傳輸控制項。

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

您可以呼叫SetMediaPlayer[**來設定該元素繫結的**MediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.setmediaplayer) 執行個體。

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

您可以設定 **MediaPlayerElement** 上的播放來源，然後該元素就會自動使用MediaPlayer[**屬性建立您可以存取的新**MediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer) 執行個體。

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

> [!NOTE] 
> 如果您透過將 [**IsEnabled**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) 設定為 false 來停用 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) 的 [**MediaPlaybackCommandManager**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled)，它將會破壞 **MediaPlayer** 和由 [MediaPlayerElement**所提供的**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols)TransportControls 之間的連結，使內建傳輸控制項無法繼續自動控制播放器的播放。 您必須改為實作自己的控制項以控制 **MediaPlayer**。

## <a name="common-mediaplayer-tasks"></a>常見的 MediaPlayer 工作
本節說明如何使用 **MediaPlayer** 的一些功能。

### <a name="set-the-audio-category"></a>設定音訊類別
將 [MediaPlayer**的**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.audiocategory)AudioCategory 屬性設為[**MediaPlayerAudioCategory**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayerAudioCategory) 列舉的其中一個值，讓系統知道您播放的媒體是何種類型。 遊戲應將其音樂資料流的類別設為 **GameMedia**，這樣如果有其他應用程式於背景播放音樂，遊戲音樂就會自動靜音。 音樂或影片應用程式應將其資料流的類別設為 **Media** 或 **Movie**，使它們的優先順序高於 **GameMedia** 資料流。

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

### <a name="output-to-a-specific-audio-endpoint"></a>輸出到特定的音訊端點
根據預設，來自 **MediaPlayer** 的音訊輸出會路由到系統的預設音訊端點，但是您可以指定 **MediaPlayer** 應用於輸出的特定音訊端點。 在以下範例中，[**MediaDevice.GetAudioRenderSelector**](https://docs.microsoft.com/uwp/api/windows.media.devices.mediadevice.getaudiorenderselector) 傳回可唯一地識別裝置音訊轉譯器類別的字串。 接下來會呼叫 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 方法 [**FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)，以取得所選類型的所有可用裝置清單。 您可以透過程式設計方式來決定要使用的裝置，或者將傳回的裝置加入 [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 讓使用者能夠選取裝置。

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

在裝置下拉式方塊的 [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 事件中，[MediaPlayer**的**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.audiodevice)AudioDevice 屬性是設為所選的裝置，其儲存在 [ComboBoxItem**的**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.tag)Tag 屬性中。

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

### <a name="playback-session"></a>播放工作階段
如本文先前所述，**MediaElement** 類別公開的許多函式已經移動到 [**MediaPlaybackSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackSession) 類別。 這包括播放器的播放狀態的相關資訊，例如，目前的播放位置、播放器已經暫停或正在播放，以及目前的播放速度。 **MediaPlaybackSession** 也提供數個事件，可在狀態變更時通知您，包括播放中內容目前的緩衝和下載狀態，以及目前播放中視訊內容的原始大小與外觀比例。

以下範例說明如何實作可以往前略過 10 秒的按一下按鈕處理常式。 首先，播放器的 **MediaPlaybackSession** 物件是和 [**PlaybackSession**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.playbacksession) 屬性一同抓取。 接著將 [**Position**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.position) 屬性設為目前的播放位置加 10 秒。

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

下一個範例說明如何透過設定工作階段的 [**PlaybackRate**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.playbackrate) 屬性，以在一般播放速度和 2 倍播放速度之間切換。

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

開始使用 Windows 10 版本 1803 時，您可以設定視訊在 **MediaPlayer** 以 90 度為增量旋轉的功能。

[!code-cs[SetRotation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetRotation)]

### <a name="detect-expected-and-unexpected-buffering"></a>偵測預期和非預期的緩衝處理
上一節描述的 **MediaPlaybackSession** 物件提供兩個事件來偵測目前播放媒體檔案何時開始與結束緩衝： **[BufferingStarted](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.BufferingStarted)** 和 **[BufferingEnded](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.BufferingEnded)** 。 這可讓您更新您的 UI 以告知使用者正在緩衝。 第一次開啟媒體檔案或當使用者切換到播放清單中的新項目時，會發生預期中的初始緩衝。 未預期緩衝則發生在網路速度降低或提供內容的內容管理系統發生技術問題時。 從 RS3 開始，您可以使用 **BufferingStarted** 事件來判斷緩衝事件是否為預期中的，或者是非預期的並會中斷播放。 您可以使用此資訊做為您的應用程式或媒體傳遞服務的遙測資料。 

登錄 **BufferingStarted** 和 **BufferingEnded** 的處理常式以接收緩衝狀態通知。

[!code-cs[RegisterBufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingHandlers)]

在 **BufferingStarted** 事件處理常式中，將傳遞至事件的事件引數轉換為 **[MediaPlaybackSessionBufferingStartedEventArgs](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs)** 物件，並檢查 **[IsPlaybackInterruption](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs.IsPlaybackInterruption)** 屬性。 若此值為 true，則觸發事件的緩衝是非預期的並會中斷播放。 否則，是預期中的初始緩衝。 

[!code-cs[BufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetBufferingHandlers)]


### <a name="pinch-and-zoom-video"></a>捏合和縮放視訊
**MediaPlayer** 可讓您指定視訊內容內應轉譯的來源矩形，以有效地允許您放大視訊。 您指定的矩形是相對於標準化的矩形 (0,0,1,1) 其中 0,0 是畫面的左上方位置，1,1 是指定畫面的完整寬度和高度。 舉例來說，若要縮放矩形，以轉譯視訊的右上方四分之一，您需要指定矩形 (.5,0,.5,.5)。  請務必檢查您的值，以確定來源矩形在 (0,0,1,1) 標準化矩形範圍內。 嘗試設定此範圍外的值會造成擲回例外狀況。

若要實作使用多點觸控手勢的捏合和縮放，您必須先指定要支援的手勢。 在此範例中，需要縮放和平移手勢。 當其中一個設定的手勢出現時，會引發 [**ManipulationDelta**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta) 事件。 將會使用 [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped) 事件來將縮放比例重設為完整畫面。 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

接下來，宣告將儲存目前縮放來源矩形的 **Rect** 物件。

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

**ManipulationDelta** 處理常式會調整縮放矩形的縮放或平移。 如果差異縮放值不是 1，表示使用者是執行捏合手勢。 如果值大於 1，來源矩形應變小以放大內容。 如果值小於 1，則來源矩形應變大以縮小內容。設定新的縮放值之前，系統會檢查產生的矩形，以確定它完全在 (0,0,1,1) 的限制範圍內。

如果縮放值是 1，則會處理平移手勢。 矩形的平移是根據手勢移動的像素數目除以控制項的寬度和高度來計算。 同樣地，系統會檢查產生的矩形，以確定它在 (0,0,1,1) 的限制範圍內。

最後，[MediaPlaybackSession**的**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.normalizedsourcerect)NormalizedSourceRect 會設為剛調整好的新矩形，以指定視訊畫面中應轉譯的區域。

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

在 [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped) 事件處理常式中，來源矩形是設回 (0,0,1,1)，使整個視訊畫面都會轉譯。

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]

**注意**本節說明觸控輸入。 觸控板會傳送指標事件，而不會傳送操作事件。

### <a name="handling-policy-based-playback-degradation"></a>處理原則型播放降低

在某些情況下，系統可能會根據原則 (而非根據效能問題) 降低媒體項目的播放，例如降低解析度 (限制)。 例如，如果使用未簽署的視訊驅動程式進行播放，系統可能會降低視訊。 您可以呼叫 [**MediaPlaybackSession.GetOutputDegradationPolicyState**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.getoutputdegradationpolicystate#Windows_Media_Playback_MediaPlaybackSession_GetOutputDegradationPolicyState) 來判斷是否正在發生原則型降低及其原因，並警示使用者或記錄原因以用於遙測用途。

下列範例顯示當播放機開啟新的媒體項目時，實作 **MediaPlayer.MediaOpened** 事件的處理程式。 在傳遞到處理常式的 **MediaPlayer** 上呼叫 **GetOutputDegradationPolicyState**。 [  **VideoConstrictionReason**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionoutputdegradationpolicystate.videoconstrictionreason#Windows_Media_Playback_MediaPlaybackSessionOutputDegradationPolicyState_VideoConstrictionReason) 的值指出影片受限制的原則原因。 如果值不是 **None**，此範例會記錄降低原因以供遙測之用。 此範例也示範將目前正在播放的 **AdaptiveMediaSource** 位元速率設定為最低頻寬以節省數據使用量 (既然影片受限制且不會以高解析度顯示)。 如需使用 **AdaptiveMediaSource** 的詳細資訊，請參閱[彈性資料流](adaptive-streaming.md)。

[!code-cs[PolicyDegradation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPolicyDegradation)]
        
## <a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>使用 MediaPlayerSurface 將視訊轉譯到 Windows.UI.Composition 表面
從 Windows 10 (版本 1607) 開始，您可以使用 **MediaPlayer** 來將視訊轉譯到 [**ICompositionSurface**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ICompositionSurface)，可讓播放器與 [**Windows.UI.Composition**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition) 命名空間中的 API 相互溝通。 組合架構可讓您在 XAML 和低層級 DirectX 圖形 API 之間處理圖形。 這能夠用於將視訊轉譯到任何 XAML 控制項等案例。 如需使用組合 API 的詳細資訊，請參閱[視覺層](https://docs.microsoft.com/windows/uwp/composition/visual-layer)。

以下範例說明如何將影片播放器內容轉譯到 [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) 控制項上。 此範例中媒體播放器專屬的呼叫是 [**SetSurfaceSize**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.setsurfacesize) 和 [**GetSurface**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.getsurface)。 **SetSurfaceSize** 會告訴系統應配置的緩衝區大小以用於轉譯內容。 **GetSurface** 會接受 [**Compositor**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Compositor) 作為引數，並抓取 [**MediaPlayerSurface**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayerSurface) 類別的執行個體。 這個類別會提供 **MediaPlayer** 和 **Compositor** 的存取權，以用來建立表面，並透過 [**CompositionSurface**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayersurface.compositionsurface) 屬性來公開表面本身。

範例中其餘的程式碼會建立 [**SpriteVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual)，它會轉譯視訊並將大小設為將顯示視覺的畫布元素大小。 接著從 [**MediaPlayerSurface**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) 建立 [**CompositionBrush**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayerSurface)，並指派給視覺的 [**Brush**](https://docs.microsoft.com/uwp/api/windows.ui.composition.spritevisual.brush) 屬性。 然後建立 [**ContainerVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual)，並在其視覺化樹狀結構頂端插入 **SpriteVisual**。 最後，呼叫 [**SetElementChildVisual**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual) 以將容器視覺指派到 **Canvas**。

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
## <a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>使用 MediaTimelineController 來跨多個播放器同步內容。
如本文中先前所討論，您的 App 可以同時有數個作用中的 **MediaPlayer** 物件。 根據預設，您建立的每個 **MediaPlayer** 都是獨立運作。 在某些情況下 (例如將講評的播放軌與視訊同步)，您可能會想要同步播放器狀態、播放位置，以及多個播放器的播放速度。 從 Windows 10 (版本 1607) 開始，您可以使用 [**MediaTimelineController**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaTimelineController) 類別來實作這個行為。

### <a name="implement-playback-controls"></a>實作播放控制項
下列範例示範如何使用 **MediaTimelineController** 控制 **MediaPlayer** 的兩個執行個體。 首先，初始化 **MediaPlayer** 的每個執行個體，並將 **Source** 設為媒體檔案。 接著，建立新的 **MediaTimelineController**。 對於每個 **MediaPlayer**，透過將 [**IsEnabled**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) 屬性設為 false，來停用與每個播放器相關聯的 [**MediaPlaybackCommandManager**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled)。 然後，將 [**TimelineController**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.timelinecontroller) 屬性設定至時間軸控制器物件。

[!code-cs[DeclareMediaTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaTimelineController)]

[!code-cs[SetTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetTimelineController)]

**注意**[**MediaPlaybackCommandManager**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) 提供 **MediaPlayer** 和系統媒體傳輸控制項 (SMTC) 之間的自動整合，但此自動整合無法和以 **MediaTimelineController** 控制的媒體播放器搭配使用。 因此您必須在設定播放器的時間軸控制器之前，先停用媒體播放器的命令管理員。 若沒有這麼做，會導致擲回包含以下訊息的例外狀況：「因為物件目前的狀態，已封鎖連接「媒體時間軸控制器」。」 如需媒體播放器與 SMTC 整合的詳細資訊，請參閱[與系統媒體傳輸控制項整合](integrate-with-systemmediatransportcontrols.md)。 如果您是使用 **MediaTimelineController**，您仍然可以手動控制 SMTC。 如需詳細資訊，請參閱[系統媒體傳輸控制項的手動控制](system-media-transport-controls.md)。

一旦您已經將 **MediaTimelineController** 連接到一或多個媒體播放器，就可以使用控制器所公開的方法來控制播放狀態。 以下範例呼叫 [**Start**](https://docs.microsoft.com/uwp/api/windows.media.mediatimelinecontroller.start)，讓所有相關聯的媒體播放器從媒體開頭位置開始播放。

[!code-cs[PlayButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPlayButtonClick)]

這個範例說明暫停及繼續所有已連接的媒體播放器。

[!code-cs[PauseButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPauseButtonClick)]

若要向前快轉所有已連接的媒體播放器，請將播放速度設為大於 1 的值。

[!code-cs[FastForwardButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFastForwardButtonClick)]

下一個範例示範如何使用 **Slider** 控制項來顯示時間軸控制器目前的播放位置 (相對於其中一個已連接媒體播放器內容的長度)。 首先，會建立新的 **MediaSource**，並登錄媒體來源之 [**OpenOperationCompleted**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.openoperationcompleted) 的處理常式。 

[!code-cs[CreateSourceWithOpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCreateSourceWithOpenCompleted)]

**OpenOperationCompleted** 處理常式是用來當成探索媒體來源內容長度的機會。 一旦決定長度，就會將 **Slider** 控制項的最大值設定為媒體項目的總秒數。 該值是設定在對 [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) 的呼叫中，以確保它是在 UI 執行緒上執行。

[!code-cs[DeclareDuration](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareDuration)]

[!code-cs[OpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenCompleted)]

接下來，會登錄時間軸控制器之 [**PositionChanged**](https://docs.microsoft.com/uwp/api/windows.media.mediatimelinecontroller.positionchanged) 事件的處理常式。 這會由系統定期呼叫 (約每秒 4 次)。

[!code-cs[RegisterPositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPositionChanged)]

在 **PositionChanged** 的處理常式中，滑桿的值會更新，以反映時間軸控制項的目前位置。

[!code-cs[PositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPositionChanged)]

### <a name="offset-the-playback-position-from-the-timeline-position"></a>讓播放位置對時間軸位置產生位移
某些情況下您可能會想讓與時間軸控制器相關聯的一或多個媒體播放器的播放位置，和其他播放器之間產生位移。 若要這麼做，您可以設定要位移之 [MediaPlayer**物件的**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.timelinecontrollerpositionoffset)TimelineControllerPositionOffset 屬性。 以下範例使用兩個媒體播放器之內容的長度，來設定兩個滑桿控制項的最小和最大值，來和項目的長度相加及相減。  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

在每個滑桿的 [**ValueChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) 事件中，每個播放器的 **TimelineControllerPositionOffset** 都設為相對應的值。

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

請注意，如果播放器的位移值是對應負的播放位置，該剪輯會保持暫停直到位移達到零，然後開始播放。 同樣地，如果位移值對應到的播放位置大於媒體項目的長度，則會顯示最後一個畫面，如同單一媒體播放器到達其內容結尾時一樣。

## <a name="play-spherical-video-with-mediaplayer"></a>使用 MediaPlayer 播放球面視訊
從 Windows 10 版本 1703 開始，**MediaPlayer**支援進行球面視訊播放的等距長方投影。 球面視訊內容與一般視訊不同，差異在於**MediaPlayer**只要支援視訊編碼，就會轉譯視訊。 如果球面視訊包含指定視訊使用等距長方投影的中繼資料標記，則**MediaPlayer**可以使用指定的視野範圍和檢視方向來轉譯視訊。 這會啟用具有頭戴式顯示器的虛擬實境視訊播放這類案例，或是只讓使用者透過滑鼠或鍵盤輸入移動瀏覽球面視訊內容。

若要播放球面視訊，請使用用於播放本文先前所述視訊內容的步驟。 另一個步驟是註冊[**MediaPlayer MediaOpened**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer#Windows_Media_Playback_MediaPlayer_MediaOpened)事件的處理常式。 這個事件可讓您啟用和控制球面視訊播放參數。

[!code-cs[OpenSphericalVideo](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenSphericalVideo)]

在 **MediaOpened**處理常式中，請先檢查新開啟之媒體項目的畫面格式，方法是檢查[**PlaybackSession.SphericalVideoProjection.FrameFormat**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.FrameFormat)屬性。 如果此值是[**SphericaVideoFrameFormat.Equirectangular**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat)，則系統可以自動投影視訊內容。 首先，請將[**PlaybackSession.SphericalVideoProjection.IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.IsEnabled)屬性設定為**true**。 您也可以調整屬性，例如媒體播放程式將用來投影視訊內容的檢視方向和視野範圍。 在此範例中，視野範圍設定為 120 度的寬幅值，方法是設定[**HorizontalFieldOfViewInDegrees**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.HorizontalFieldOfViewInDegrees)屬性。

如果視訊內容是球面，但為等距長方以外的格式，則您可以使用媒體播放程式的畫面伺服器模式實作自己的投影演算法，來接收和處理個別畫面。

[!code-cs[SphericalMediaOpened](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalMediaOpened)]

下列範例程式碼說明如何使用向左鍵和向右鍵調整球面視訊檢視方向。

[!code-cs[SphericalOnKeyDown](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalOnKeyDown)]

如果您的應用程式支援視訊播放清單，則您可能會想要找出 UI 中包含球面視訊的播放項目。 [媒體項目、播放清單與曲目](media-playback-with-mediasource.md)文章會詳細討論媒體播放清單。 下列範例示範如何建立新的播放清單、新增項目，以及註冊[**MediaPlaybackItem.VideoTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.VideoTracksChanged)事件的處理常式，而這個事件是在解析媒體項目的視訊播放軌時發生。

[!code-cs[SphericalList](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalList)]

在 **VideoTracksChanged**事件處理常式中，取得任何新增之視訊播放軌的編碼屬性，方法是呼叫[**VideoTrack.GetEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.core.videotrack.GetEncodingProperties)。 如果編碼屬性的[**SphericalVideoFrameFormat**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties.SphericalVideoFrameFormat)屬性是[**SphericaVideoFrameFormat.None**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat)以外的值，則視訊播放軌包含球面視訊，而且您可以自行選擇適當地更新 UI。

[!code-cs[SphericalTracksChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalTracksChanged)]

## <a name="use-mediaplayer-in-frame-server-mode"></a>以畫面伺服器模式使用 MediaPlayer
從 Windows 10 版本 1703 開始，您可以透過畫面伺服器模式使用**MediaPlayer**。 在此模式下，**MediaPlayer**不會自動向相關**MediaPlayerElement**轉譯畫面。 相反地，您的應用程式會將目前畫面從**MediaPlayer**複製到實作[**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface)的物件。 這項功能所啟用的主要案例將會使用像素著色器來處理**MediaPlayer**所提供的視訊畫面。 您的應用程式負責在處理後顯示每個畫面，例如透過在 XAML [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) 控制項中顯示畫面。

在下列的範例中，會初始化新**MediaPlayer**，並載入視訊內容。 接下來，會登錄[**VideoFrameAvailable**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.VideoFrameAvailable)的處理常式。 畫面伺服器模式的啟用方式是將**MediaPlayer**物件的[**IsVideoFrameServerEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.IsVideoFrameServerEnabled)屬性設定為**true**。 最後，會使用[**Play**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)呼叫來啟動媒體播放。

[!code-cs[FrameServerInit](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFrameServerInit)]

下一個範例示範**VideoFrameAvailable**的處理常式，其使用[Win2D](https://github.com/Microsoft/Win2D)新增視訊之每個畫面的簡單模糊效果，然後透過 XAML [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image)控制項顯示處理過的畫面。

只要呼叫**VideoFrameAvailable**處理常式，就會使用[**CopyFrameToVideoSurface**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.copyframetovideosurface)方法，將畫面內容複製到[**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface)。 您也可以使用[**CopyFrameToStereoscopicVideoSurfaces**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.copyframetostereoscopicvideosurfaces)將 3D 內容複製到兩個表面，以個別處理左眼和右眼內容。 若要取得實作**IDirect3DSurface**的物件，這個範例會建立[**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap)，然後使用該物件建立 Win2D **CanvasBitmap**，以實作所需的介面。 **CanvasImageSource**是可作為**Image**控制項來源的 Win2D 物件，因此會建立新項目，並將其設定為**Image**來源，而在其中顯示內容。 接下來，會建立 **CanvasDrawingSession**。 這是供 Win2D 用來轉譯模糊效果。

具現化所有必要物件之後，會呼叫**CopyFrameToVideoSurface**，以將目前畫面從**MediaPlayer**複製到**CanvasBitmap**。 接下來，會建立 Win2D **GaussianBlurEffect**，而且**CanvasBitmap**設定為運算來源。 最後，呼叫**CanvasDrawingSession.DrawImage**，將套用模糊效果的來源影像繪製到與**Image**控制項相關的**CanvasImageSource**，以在 UI 中進行繪製。

[!code-cs[VideoFrameAvailable](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetVideoFrameAvailable)]

如需 Win2D 的詳細資訊，請參閱[Win2D GitHub 存放庫](https://github.com/Microsoft/Win2D)。 若要試用上述的範例程式碼，您需要使用下列指示，將 Win2D NuGet 套件新增至專案。

**將 Win2D NuGet 套件新增至您的效果專案**

1.  在**方案總管**中，以滑鼠右鍵按一下專案，然後選取 **[管理 NuGet 套件]** 。
2.  在視窗頂端，選取 **\[瀏覽\]** 索引標籤。
3.  在搜尋方塊中輸入 **Win2D**。
4.  選取 **\[Win2D.uwp\]** ，然後選取右窗格中的 **\[安裝\]** 。
5.  **\[檢閱變更\]** 對話方塊會顯示要安裝的套件。 按一下 [確定]。
6.  接受套件授權。

## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>偵測及回應系統進行的音量變更
從 Windows 10 版本 1803 開始，您的應用程式可偵測系統將目前播放 **MediaPlayer** 的音量降低或設為靜音。 例如，系統可能會在鬧鈴響起時，降低 (或者「迴避」) 音訊播放音量。 如果您的應用程式未在應用程式資訊清單中宣告 *backgroundMediaPlayback* 功能，當您的應用程式進入背景時，系統會將其設為靜音。 [  **AudioStateMonitor**](https://docs.microsoft.comuwp/api/windows.media.audio.audiostatemonitor) 類別可讓您註冊以在系統修改音訊資料流的音量時接收事件。 存取 **MediaPlayer** 的 **AudioStateMonitor** 屬性並註冊 [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) 事件的處理常式，以在系統變更 **MediaPlayer** 的音量時收到通知。

[!code-cs[RegisterAudioStateMonitor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

處理 **SoundLevelChanged** 事件時，您可能需要根據播放的內容類型而採取不同的動作。 如果目前正在播放音樂，您可能會想讓音樂在音量降低的情況下繼續播放。 但是，如果正在播放播客，您可能會想在音量降低的情況下暫停播放，以免使用者錯過任何內容。

此範例宣告變數來追蹤目前播放的內容是否為播客，它假設您在選取 **MediaPlayer** 的內容時將此設定為適當的值。 我們也建立類別變數來追蹤當音量變更時以程式設計方式暫停播放。

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

在 **SoundLevelChanged** 事件處理常式中，檢查 [AudioStateMonitor**傳送者的**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel)SoundLevel 屬性來判斷新的音量大小。 此範例會查看是否新的音量是否為最大音量，這表示系統已停止靜音或降低音量，或音量是否已降低但仍在播放非播客內容。 如果其中一項為 true 且先前已以程式設計方式暫停內容，將會繼續播放。 如果新音量設為靜音或目前內容為播客且音量很小，則會暫停播放，並將變數設定為追蹤暫停是以程式設計方式初始化。

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

即使系統已降低音量，使用者仍可決定是否要暫停或繼續播放。 此範例顯示播放和暫停按鈕的事件處理常式。 如果已以程式設計方式暫停播放，則在暫停按鈕點選處理常式中是暫停，接著我們會更新變數來指出使用者已暫停內容。 在播放按鈕點選處理常式中，我們繼續播放並清除追蹤變數。

[!code-cs[ButtonUserClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetButtonUserClick)]

## <a name="related-topics"></a>相關主題
* [媒體播放](media-playback.md)
* [媒體專案、播放清單和追蹤](media-playback-with-mediasource.md)
* [與系統媒體傳輸控制項整合](integrate-with-systemmediatransportcontrols.md)
* [建立、排程及管理媒體中斷](create-schedule-and-manage-media-breaks.md)
* [在背景播放媒體](background-audio.md)





 

 




