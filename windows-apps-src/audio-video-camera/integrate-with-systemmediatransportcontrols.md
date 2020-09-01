---
ms.assetid: eb690f2b-3bf8-4a65-99a4-2a3a8c7760b7
description: 本文說明如何與系統媒體傳輸控制項互動。
title: 與系統媒體傳輸控制項整合
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 449c8b445e70ffb68d0bc95f96e2b33c57e3b38f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163932"
---
# <a name="integrate-with-the-system-media-transport-controls"></a>與系統媒體傳輸控制項整合

本文說明如何與系統媒體傳輸控制項 (SMTC) 互動。 SMTC 是一組控制項，通用於所有 Windows 10 裝置，並提供一個一致的方式，讓使用者控制所有使用 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 進行播放之執行中應用程式的媒體播放。

系統媒體傳輸控制可讓媒體應用程式開發人員與內建的系統 UI 整合，以顯示媒體中繼資料，例如演出者、專輯職稱或章節標題。 系統傳輸控制也可讓使用者使用內建的系統 UI 來控制媒體應用程式的播放，例如暫停播放，以及向前和向後跳到播放清單。

<img alt="System Media Transtport Controls" src="images/smtc.png" />


如需示範與 SMTC 整合的完整範例，請參閱 [Github 上的系統媒體傳輸控制項範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)。
                    
## <a name="automatic-integration-with-smtc"></a>與 SMTC 自動整合
從 Windows 10 版本 1607 開始，使用 [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) 類別播放媒體的 UWP app 會根據預設，自動與 SMTC 整合。 只需執行個體化新的 **MediaPlayer** 執行個體，並將 [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource)、[**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) 或 [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) 指派給播放程式的 [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) 屬性，使用者就會在 SMTC 中看見您的應用程式名稱，並且可以透過使用 SMTC 控制項播放、暫停並移動您的播放清單。 

您的應用程式可以一次建立及使用多個 **MediaPlayer** 物件。 針對您應用程式中每個作用中的 **MediaPlayer** 執行個體，會在 SMTC 中建立個別的索引標籤，讓使用者能在您的作用中媒體播放程式以及其他執行中應用程式的媒體播放程式之間切換。 SMTC 中目前已選取的媒體播放程式，就是控制項會影響的播放程式。

如需在您的應用程式中使用 **MediaPlayer** 的詳細資訊 (包含將它繫結至 XAML 頁面中的 [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement))，請參閱[使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)。 

如需使用 **MediaSource**、**MediaPlaybackItem** 和 **MediaPlaybackList** 的詳細資訊，請參閱[媒體項目、播放清單以及曲目](media-playback-with-mediasource.md)。

## <a name="add-metadata-to-be-displayed-by-the-smtc"></a>新增由 SMTC 顯示的中繼資料
如果您想要針對您在 SMTC 中顯示的媒體項目新增或修改中繼資料 (例如影片或歌曲標題)，您必須更新代表您媒體項目的 **MediaPlaybackItem** 的顯示屬性。 首先，透過呼叫 [**GetDisplayProperties**](/uwp/api/windows.media.playback.mediaplaybackitem.getdisplayproperties) 取得 [**MediaItemDisplayProperties**](/uwp/api/Windows.Media.Playback.MediaItemDisplayProperties) 物件的參考。 接著，設定媒體、音樂或影片的類型 (針對具有 [**Type**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.type) 屬性的項目)。 然後您可以根據您所指定的媒體類型，填入 [**MusicProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.musicproperties) 或 [**VideoProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.videoproperties) 的欄位。 最後，透過呼叫 [**ApplyDisplayProperties**](/uwp/api/windows.media.playback.mediaplaybackitem.applydisplayproperties) 來更新媒體項目的中繼資料。

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]


> [!Note]
> 應用程式應該設定 [**Type**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.type) 屬性的值，即使它們並未提供系統媒體傳輸控制項所顯示的其他媒體中繼資料。 此值可協助系統正確處理您的媒體內容，包括防止螢幕保護裝置在播放期間啟用。


## <a name="use-commandmanager-to-modify-or-override-the-default-smtc-commands"></a>使用 CommandManager 修改或覆寫預設 SMTC 命令
您的應用程式可以使用 [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) 類別修改或完全覆寫 SMTC 控制項的行為。 透過存取 [**CommandManager**](/uwp/api/windows.media.playback.mediaplayer.commandmanager) 屬性，可以取得每個 **MediaPlayer** 類別執行個體的命令管理員執行個體。

針對每個命令，例如 *Next* 命令 (根據預設會跳到 **MediaPlaybackList** 中下一個項目)，命令管理員會公開收到的事件 (例如 [**NextReceived**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.nextreceived))，以及管理命令行為的物件 (例如 [**NextBehavior**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.nextbehavior))。 

下列範例會針對 **NextReceived** 事件以及 **NextBehavior** 的 [**IsEnabledChanged**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabledchanged) 事件登錄一個處理常式。

[!code-cs[AddNextHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddNextHandler)]

下列範例說明應用程式想要在使用者點按播放清單中五個項目之後，停用 *Next* 命令的情況，也許在繼續播放內容之前需要一些使用者互動。 引發每個 ## **NextReceived** 事件，計數器都會增加。 一旦計數器達到目標數，*Next* 命令的[**EnablingRule**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.enablingrule) 會設定為 [**Never**](/uwp/api/Windows.Media.Playback.MediaCommandEnablingRule)，這會停用該命令。 

[!code-cs[NextReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetNextReceived)]

您也可以將命令設定為 **Always**，這表示命令一律會啟用，即使播放清單中沒有項目時也是如此 (適用於 *Next* 命令範例)。 或者您可以將命令設定為 **Auto**，其中系統會根據目前播放的內容決定是否啟用命令。

針對上述案例，在某些時候應用程式會想要透過將 **EnablingRule** 設定為 **Auto**，重新啟用 *Next* 命令。

[!code-cs[EnableNextButton](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetEnableNextButton)]

因為您的應用程式在前景中時，可能有自己用來控制播放的 UI，您可以使用 [**IsEnabledChanged**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabledchanged) 事件更新您自己的 UI 以符合 SMTC，因為命令已透過存取已傳入到處理常式之[**MediaPlaybackCommandManagerCommandBehavior**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior) 的 [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabled) 來啟用或停用。

[!code-cs[IsEnabledChanged](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetIsEnabledChanged)]

在某些情況下，您可能想要完全覆寫 SMTC 命令的行為。 下列範例說明應用程式使用 *Next* 和 *Previous* 命令在網際網路廣播電台之間切換，而不是在目前播放清單中的曲目之間切換的情況。 如同先前的範例中，在收到命令時會登錄處理常式，此範例中則是 [**PreviousReceived**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.previousreceived) 事件。

[!code-cs[AddPreviousHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddPreviousHandler)]

在 **PreviousReceived** 處理常式中，透過呼叫已傳入到處理常式之 [**MediaPlaybackCommandManagerPreviousReceivedEventArgs**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs) 的 [**GetDeferral**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagerpreviousreceivedeventargs.getdeferral)，會先取得 [**Deferral**](/uwp/api/Windows.Foundation.Deferral)。 這會告訴系統等待延期完成才執行命令。 如果您要在處理常式中進行非同步呼叫，這非常重要。 到目前為止，範例呼叫的自訂方法會傳回代表先前的廣播電台的 **MediaPlaybackItem**。

接下來，會檢查 [**Handled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagerpreviousreceivedeventargs.handled) 屬性，以確定事件尚未由另一個處理常式進行處理。 如果已進行處理，**Handled** 屬性會設為 true. 這可讓 SMTC 以及任何其他訂閱的處理常式知道，它們應該不要進行任何動作來執行此命令，因為命令已經處理。 接著程式碼會針對媒體播放程式設定新的來源，並開始播放程式。

最後，會在延期物件上呼叫 [**Complete**](/uwp/api/windows.foundation.deferral.complete)，讓系統知道您已經完成處理命令。

[!code-cs[PreviousReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetPreviousReceived)]
                 
## <a name="manual-control-of-the-smtc"></a>手動控制 SMTC
如本文先前所提及，SMTC 會自動偵測與顯示您應用程式所建立之每個 **MediaPlayer** 執行個體的資訊。 如果您想要使用多個 **MediaPlayer** 執行個體但希望 SMTC 針對您的應用程式提供單一項目，則您必須手動控制 SMTC 的行為，而非仰賴自動整合。 此外，如果您使用 [**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController) 來控制一或多個媒體播放程式，您必須使用手動 SMTC 整合。 此外，如果您的應用程式使用 API 而不是 **MediaPlayer** (例如 [**AudioGraph**](/uwp/api/Windows.Media.Audio.AudioGraph) 類別) 來播放媒體，您必須實作手動 SMTC 整合，讓使用者使用 SMTC 來控制您的應用程式。 如需如何手動控制 SMTC 的詳細資訊，請參閱[系統媒體傳輸控制項的手動控制項](system-media-transport-controls.md)。



## <a name="related-topics"></a>相關主題
* [媒體播放](media-playback.md)
* [使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)
* [系統媒體傳輸控制項的手動控制項](system-media-transport-controls.md)
* [Github 上的系統媒體傳輸控制項範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)
 

 