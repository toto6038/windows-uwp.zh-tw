---
author: drewbatgit
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: "SystemMediaTransportControls 類別可讓您的 app 使用內建於 Windows 的系統媒體傳輸控制項，以及更新控制項顯示您的 app 目前正在播放的媒體相關的中繼資料。"
title: "系統媒體傳輸控制項"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 5a94ce4112f7662d3fe9bf3c8a7d3f60b1569931

---

# 系統媒體傳輸控制項

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


[**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 類別可讓您的 app 使用內建於 Windows 的系統媒體傳輸控制項，以及更新控制項顯示您的 app 目前正在播放的媒體相關的中繼資料。

系統傳輸控制項不同於 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 物件上的傳輸控制項。 系統傳輸控制項是在使用者按下硬體媒體鍵 (例如耳機上的音量控制或鍵盤上的媒體按鈕) 時以快顯方式顯示的控制項。 如果使用者按下鍵盤上的暫停鍵且您的應用程式支援 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)，您的應用程式會收到通知，而且您可以採取適當的動作。

您的 app 也可以更新媒體資訊，例如歌曲標題以及 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 顯示的縮圖影像。

**注意**  
[系統媒體傳輸控制項 UWP 範例](http://go.microsoft.com/fwlink/?LinkId=619488)會實作本概觀文章中所討論的程式碼。 您可以下載範例以查看內容中的程式碼，或做為您自己 app 的初期基礎。

## 設定傳輸控制項

在頁面的 XAML 檔案中定義 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)，它是由系統媒體傳輸控制項控制。 [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) 和 [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/br227394) 事件是用來更新系統媒體傳輸控制項，將會在本文稍後討論。

[!code-xml[MediaElementSystemMediaTransportControls](./code/SMTCWin10/cs/MainPage.xaml#SnippetMediaElementSystemMediaTransportControls)]

將按鈕新增至 XAML 檔案，讓使用者選取要播放的檔案。

[!code-xml[OpenButton](./code/SMTCWin10/cs/MainPage.xaml#SnippetOpenButton)]

在程式碼後置頁面中，針對下列命名空間新增 using 指示詞。

[!code-cs[Namespace](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetNamespace)]

新增按鈕 click 處理常式，它使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 以允許使用者選取檔案，然後呼叫 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) 讓它成為 **MediaElement** 的使用中檔案。

[!code-cs[OpenMediaFile](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetOpenMediaFile)]

呼叫 [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708) 以取得 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 的執行個體。

啟用您的 app 將會使用的按鈕，方法是設定 **SystemMediaTransportControls** 物件的「已啟用」屬性，例如 [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714)、[**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713)、[**IsNextEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278712) 和 [**IsPreviousEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278715)。 請參閱 **SystemMediaTransportControls** 參考文件，以取得可用控制項的完整清單。

註冊 [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) 事件的處理常式，當使用者按下按鈕時接收通知。

[!code-cs[SystemMediaTransportControlsSetup](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsSetup)]

## 處理系統媒體傳輸控制項按鈕按下

當某個啟用的按鈕被按下時，系統傳輸控制項會引發 [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) 事件。 傳入事件處理常式的 [**SystemMediaTransportControlsButtonPressedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn278683) 的 [**Button**](https://msdn.microsoft.com/library/windows/apps/dn278685) 屬性，是 [**SystemMediaTransportControlsButton**](https://msdn.microsoft.com/library/windows/apps/dn278681) 列舉的成員，該列舉表示已按下哪個已啟用的按鈕。

若要從 [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) 事件處理常式更新 UI 執行緒上的物件 (如 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 物件)，您必須透過 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 為呼叫進行封送處理。 這是因為 **ButtonPressed** 事件處理常式不是從 UI 執行緒呼叫，因此如果您嘗試直接修改 UI 將會擲回例外狀況。

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## 以目前的媒體狀態更新系統媒體傳輸控制項

當媒體狀態已變更時，您應該通知 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)，以便讓系統可以更新控制項以反映目前狀態。 若要這樣做，請從 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的 [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) 事件 (當媒體狀態變更時引發)，將 [**PlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278719) 屬性設為適當的 [**MediaPlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278665) 值。

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## 以媒體資訊和縮圖更新系統媒體傳輸控制項

您可以使用 [**SystemMediaTransportControlsDisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278686) 類別來更新傳輸控制項顯示的媒體資訊，例如，目前播放媒體項目的歌曲標題或專輯封面。 使用 [**SystemMediaTransportControls.DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707) 屬性取得這個類別的執行個體。 對於典型的案例，傳遞中繼資料的建議方式是呼叫 [**CopyFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn278694)，傳入目前播放媒體檔案。 顯示更新程式會自動從檔案擷取中繼資料和縮圖影像。

呼叫 [**Update**](https://msdn.microsoft.com/library/windows/apps/dn278701) 會造成系統媒體傳輸控制項使用新的中繼資料與縮圖更新它的 UI。

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

如果您的案例需要，您可以手動更新系統媒體傳輸控制項顯示的中繼資料，方法是設定 [**DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707) 類別公開之 [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/dn278696)、[**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/dn278695) 或 [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/dn278702) 物件的值。

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

## 更新系統媒體傳輸控制項時間軸屬性

系統傳輸控制項會顯示目前播放媒體項目之時間軸的相關資訊，包括媒體項目的目前播放位置、開始時間及結束時間。 若要更新系統傳輸控制項時間軸屬性，請建立新的 [**SystemMediaTransportControlsTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218746) 物件。 設定物件的屬性以反映正在播放的媒體項目的目前狀態。 呼叫 [**SystemMediaTransportControls.UpdateTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218760) 讓控制項更新時間軸。

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   您必須提供 [**StartTime**](https://msdn.microsoft.com/library/windows/apps/mt218751)、[**EndTime**](https://msdn.microsoft.com/library/windows/apps/mt218747) 和 [**Position**](https://msdn.microsoft.com/library/windows/apps/mt218755) 的值，系統控制項才能為您正在播放的項目顯示時間軸。

-   [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) 和 [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) 可讓您指定使用者可以搜尋的時間軸的範圍。 典型的案例是讓內容提供者在他們的媒體中包含廣告中斷。

    您必須設定 [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) 和 [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748)，才會引發 [**PositionChangeRequest**](https://msdn.microsoft.com/library/windows/apps/mt218755)。

-   建議您讓系統控制項與您的媒體播放保持同步，方法是在播放期間大約每 5 秒更新這些屬性，以及每當播放的狀態變更時更新，例如暫停或搜尋新的位置。

## 回應播放程式屬性變更

有一組系統傳輸控制項屬性與媒體播放程式本身的目前狀態相關，而不是與正在播放的媒體項目狀態相關。 每個屬性與使用者調整相關聯控制項時所引發的事件相符。 這些屬性與事件包括：

| 屬性                                                                  | 事件                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](https://msdn.microsoft.com/library/windows/apps/mt218753) | [**AutoRepeatModeChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218754) |
| [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756)     | [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757)     |
| [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt218758) | [**ShuffleEnabledChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218759) |

 
若要處理使用者與其中一個控制項的互動，請先註冊相關聯事件的處理常式。

[!code-cs[RegisterPlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterPlaybackChangedHandler)]

在事件處理常式中，請先確定要求的值是在有效且預期的範圍內。 如果是的話，設定 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的對應屬性，然後設定 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 物件的對應屬性。

[!code-cs[PlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetPlaybackChangedHandler)]

-   為了引發其中一個播放程式屬性事件，您必須設定屬性的初始值。 例如，[**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757) 不會引發，直到您設定 [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756) 屬性的值至少一次之後。

## 針對背景音訊使用系統媒體傳輸控制項

若要針對背景音訊使用系統媒體傳輸控制項，您必須將 [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714) 和 [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713) 設為 true，以啟用播放和暫停按鈕。 您的 app 還必須處理 [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) 事件。

若要從您的 app 的背景工作取得 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 的執行個體，您必須使用 [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635)，而不是 [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708)，這僅適用從前景 app 內使用。

如需有關在背景播放音訊的詳細資訊，請參閱[背景音訊](background-audio.md)。

 

 







<!--HONumber=Jun16_HO4-->


