---
author: drewbatgit
ms.assetid: 0309c7a1-8e4c-4326-813a-cbd9f8b8300d
description: 本文示範如何建立、排程及管理媒體播放 app 的媒體中斷。
title: 建立、排程與管理媒體中斷
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0feb7f6771254bf500e4b64fd0e632daad9817e4
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2018
ms.locfileid: "6048349"
---
# <a name="create-schedule-and-manage-media-breaks"></a>建立、排程與管理媒體中斷

本文示範如何建立、排程及管理媒體播放 app 的媒體中斷。 媒體中斷通常是用來將音訊或視訊廣告插入媒體內容。 從 Windows 10 版本 1607 開始，您可以使用 [**MediaBreakManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager) 類別，快速且輕鬆地將媒體中斷新增至任何您用來播放 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 的 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem)。


當您排程一或多個媒體中斷之後，系統就會自動在播放期間的指定時間播放您的媒體內容。 **MediaBreakManager** 提供事件，讓您的 app 可以在媒體中斷開始、結束，或當使用者略過它們時加以回應。 您也可以針對媒體中斷存取 [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession)，以監視下載與緩衝處理進度更新等事件。

## <a name="schedule-media-breaks"></a>排程媒體中斷
每個 **MediaPlaybackItem** 物件都有自己的 [**MediaBreakSchedule**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule)，您可以在播放項目時，用來設定將播放的媒體中斷。 在 app 中使用媒體中斷的第一個步驟是為您的主要播放內容建立 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem)。 

[!code-cs[MoviePlaybackItem](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMoviePlaybackItem)]

如需使用 **MediaPlaybackItem**、[**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList) 及其他基本媒體播放 API 的詳細資訊，請參閱[媒體項目、播放清單與曲目](media-playback-with-mediasource.md)。

下一個範例示範如何將預載中斷新增到 **MediaPlaybackItem**，這表示系統將在播放媒體中斷所屬的項目之前，先播放該中斷。 首先，將新的 [**MediaBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreak) 物件具現化。 在這個範例中，使用 [**MediaBreakInsertionMethod.Interrupt**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakInsertionMethod) 來呼叫建構函式，這表示在播放中斷內容時，將暫停主要內容。 

接下來，針對要在中斷期間播放的內容 (例如廣告) 建立新的 **MediaPlaybackItem**。 此播放項目的 [**CanSkip**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.CanSkip) 屬性會設定為 false。 這表示使用者無法使用內建的媒體控制項來略過此項目。 您的 app 仍然可藉由呼叫 [**SkipCurrentBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.SkipCurrentBreak)，以程式設計方式選擇略過新增。 

媒體中斷的 [**PlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreak.PlaybackList) 屬性是 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList)，可讓您以播放清單形式播放多個媒體項目。 從清單的 **Items** 集合中新增一或多個 **MediaPlaybackItem** 物件，將它們包含於媒體中斷的播放清單中。

最後，使用主要內容播放項目的 [**BreakSchedule**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.BreakSchedule) 屬性來排程媒體中斷。 藉由將中斷指派給排程物件的 [**PrerollBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule.PrerollBreak) 屬性，來指定要做為預載中斷的中斷。

[!code-cs[PreRollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPreRollBreak)]

您現在可以播放主要媒體項目，而您建立的媒體中斷將會在主要內容之前播放。 建立新的 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 物件，然後選擇性地將 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) 屬性設為 true，以便自動開始播放。 將 **MediaPlayer** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 屬性設為您的主要內容播放項目。 這不是必要動作，但您可以將 **MediaPlayer** 指派給 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)，以轉譯 XAML 頁面中的媒體。 如需使用 **MediaPlayer** 的詳細資訊，請參閱[使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)。

[!code-cs[Play](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

使用與預載中斷相同的技術，新增要在包含您主要內容的 **MediaPlaybackItem** 完成播放之後播放的預載中斷，不同之處在於您會將 **MediaBreak** 物件指派給 [**PostrollBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule.PostrollBreak) 屬性。

[!code-cs[PostRollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPostRollBreak)]

您也可以排程一或多個插播中斷，其會在播放主要內容期間的特定時間播放。 在下列範例中，[**MediaBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreak) 是使用接受 **TimeSpan** 物件的建構函式多載所建立，它會指定在播放中斷時，主要媒體項目播放期間內的時間。 同樣地，會指定 [**MediaBreakInsertionMethod.Interrupt**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakInsertionMethod)，以指出中斷播放時，將暫停主要內容的播放。 您可以呼叫 [**InsertMidrollBreak**](https://msdn.microsoft.com/library/windows/apps/mt670692) 來將插播中斷新增到排程。 您也可以存取 [**MidrollBreaks**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule.MidrollBreaks) 屬性，來取得排程中目前插播中斷的唯讀清單。

[!code-cs[MidrollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMidrollBreak)]

下一個示範的插播中斷範例會使用 [**MediaBreakInsertionMethod.Replace**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakInsertionMethod) 插入方法，這表示系統將在中斷播放時繼續處理主要內容。 這個選項通常用於即時資料流處理媒體 app，您不想在播放廣告時暫停內容，而使得內容與即時資料流產生落差。 

這個範例也會使用 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) 建構函式的多載，此建構函式會接受兩個 [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.TimeSpan) 參數。 第一個參數指定媒體中斷項目內將開始播放的起點。 第二個參數指定將播放媒體中斷項目的持續時間。 因此，在下列範例中，**MediaBreak** 將會在主要內容播放 20 分鐘後開始播放。 當它播放時，媒體項目將會從中斷媒體項目開始啟動 30 秒，並且將在主要媒體內容繼續播放之前播放 15 秒。

[!code-cs[MidrollBreak2](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMidrollBreak2)]

## <a name="skip-media-breaks"></a>略過媒體中斷
如本文先前所述，您可以設定 **MediaPlaybackItem** 的 [**CanSkip**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.CanSkip) 屬性，來防止使用者略過內建控制項的內容。 不過，您隨時都可以從程式碼中呼叫 [**SkipCurrentBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.SkipCurrentBreak) 來略過目前的中斷。

[!code-cs[SkipButtonClick](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetSkipButtonClick)]

## <a name="handle-mediabreak-events"></a>處理 MediaBreak 事件

有數個與媒體中斷相關的事件，您可加以登錄，以便根據媒體中斷的變更狀態來採取動作。

[!code-cs[RegisterMediaBreakEvents](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetRegisterMediaBreakEvents)]

[**BreakStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.BreakStarted) 會在媒體中斷啟動時引發。 您可能想要更新 UI，讓使用者知道正在播放媒體中斷內容。 這個範例使用傳入處理常式的 [**MediaBreakStartedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakStartedEventArgs)，來取得啟動媒體中斷的參照。 接著，使用 [**CurrentItemIndex**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList.CurrentItemIndex) 屬性，來判斷正在播放媒體中斷播放清單中的哪一個媒體項目。 然後更新 UI，以便向使用者顯示目前的廣告索引，以及中斷內剩餘的廣告數目。 請記住，更新 UI 必須在 UI 執行緒上進行，因此，您應該在 [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 內進行此呼叫。 

[!code-cs[BreakStarted](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakStarted)]

當中斷內的所有媒體項目已完成播放或略過時，即會引發 [**BreakEnded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.BreakEnded)。 您可以使用這個事件的處理常式來更新 UI，以指出不再播放媒體中斷內容。

[!code-cs[BreakEnded](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakEnded)]

當使用者在 [**CanSkip**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.CanSkip) 為 true 的項目播放期間按下內建 UI 中的 *Next* 按鈕時，或者當您在程式碼中呼叫 [**SkipCurrentBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.SkipCurrentBreak) 略過中斷時，即會引發 **BreakSkipped** 事件。

下列範例使用 **MediaPlayer** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 屬性，針對主要內容取得媒體項目的參照。 略過的媒體中斷隸屬於此項目的中斷排程。 接下來，程式碼會檢查已略過的媒體中斷是否與設定為排程 [**PrerollBreak**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSchedule.PrerollBreak) 屬性的媒體中斷相同。 如果是，這表示預載中斷就是已略過的中斷，在此情況下，會建立新的插播中斷，並排程在主要內容播放 10 分鐘時播放。

[!code-cs[BreakSkipped](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakSkipped)]

當主要媒體項目的播放位置通過了一或多個媒體中斷的排程時間時，即會引發 [**BreaksSeekedOver**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager.BreaksSeekedOver)。 下列範例會檢查是否有多個媒體中斷被試圖結束、播放位置是否已向前移動，以及其向前移動的時間是否少於 10 分鐘。 如果是，被試圖結束的第一個中斷 (從事件引數公開的 [**SeekedOverBreaks**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakSeekedOverEventArgs.SeekedOverBreaks) 集合所取得) 可藉由呼叫 **MediaPlayer.BreakManager** 的 [**PlayBreak**](https://msdn.microsoft.com/library/windows/apps/mt670689) 方法立即播放。

[!code-cs[BreakSeekedOver](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakSeekedOver)]


## <a name="access-the-current-playback-session"></a>存取目前的播放工作階段
[**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) 物件會使用 **MediaPlayer** 類別，來提供與目前播放的媒體內容相關的資料和事件。 [**MediaBreakManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager) 也會有 **MediaPlaybackSession**，您可以用來取得與正在播放的媒體中斷內容明確相關的資料和事件。 您可以從播放工作階段取得的資訊包括目前的播放狀態 (播放或暫停)，以及內容中目前的播放位置。 如果媒體中斷內容的外觀比例與您的主要內容不同，您可以使用 [**NaturalVideoWidth**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NaturalVideoWidth) 和 [**NaturalVideoHeight**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NaturalVideoHeight) 屬性及 [**NaturalVideoSizeChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NaturalVideoSizeChanged) 來調整視訊 UI。 您也可以接收像是 [**BufferingStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.BufferingStarted)、[**BufferingEnded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.BufferingEnded) 及 [**DownloadProgressChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.DownloadProgressChanged) 等事件，這些事件可以提供關於您 app 效能的重要遙測資料。

下列範例會登錄適用於 **BufferingProgressChanged 事件**的處理常式；在事件處理常式中，它會更新 UI 以顯示目前的緩衝處理進度。

[!code-cs[RegisterBufferingProgressChanged](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingProgressChanged)]

[!code-cs[BufferingProgressChanged](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBufferingProgressChanged)]

## <a name="related-topics"></a>相關主題
* [媒體播放](media-playback.md)
* [使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)
* [系統媒體傳輸控制項的手動控制項](system-media-transport-controls.md)

 

 




