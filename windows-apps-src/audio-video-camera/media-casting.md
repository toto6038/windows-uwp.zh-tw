---
ms.assetid: 40B97E0C-EB1B-40C2-A022-1AB95DFB085E
description: 本文示範如何從通用 Windows app 將媒體傳播到遠端裝置。
title: 媒體傳播
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9e4b794e560c213e5c3796b11dd1a5fd77a98506
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7968846"
---
# <a name="media-casting"></a>媒體傳播



本文示範如何從通用 Windows app 將媒體傳播到遠端裝置。

## <a name="built-in-media-casting-with-mediaplayerelement"></a>MediaPlayerElement 內建的媒體傳播

從通用 Windows app 傳播媒體最簡單的方式是使用 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) 控制項內建的傳播功能。

若要讓使用者開啟要在 **MediaPlayerElement** 控制項中播放的視訊檔案，請將下列命名空間加入到您的專案。

[!code-cs[BuiltInCastingUsing](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetBuiltInCastingUsing)]

在 app 的 XAML 檔案中，加入 **MediaPlayerElement** 並將 [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298977) 設定為 true。

[!code-xml[MediaElement](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetMediaElement)]

新增按鈕讓使用者開始挑選檔案。

[!code-xml[OpenButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetOpenButton)]

在按鈕的 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件處理常式中，建立 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 的新執行個體、將視訊檔案類型加入到 [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850) 集合，並將開始位置設定為使用者的視訊庫。

呼叫 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275)，以啟動檔案選擇器對話方塊。 這個方法傳回的結果是一個代表視訊檔案的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件。 請確認檔案不是 Null (使用者取消挑選作業時就會是 Null)。 呼叫檔案的 [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227221.aspx) 方法，取得檔案的 [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731)。 最後，藉由呼叫 [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909) 並將它指派給 **MediaPlayerElement** 物件的 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.Source) 屬性，以從選取的檔案建立一個新的 **MediaSource** 物件，讓該視訊檔案成為控制項的視訊來源。

[!code-cs[OpenButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetOpenButtonClick)]

當 **MediaPlayerElement** 中載入視訊之後，使用者可以在傳輸控制項直接按下傳播按鈕，以啟動內建的對話方塊，讓他們選擇已載入的媒體要傳播到哪個裝置。

![MediaElement 傳播按鈕](images/media-element-casting-button.png)

> [!NOTE] 
> 從 Windows 10 版本 1607 開始，建議您使用 **MediaPlayer** 類別來播放媒體項目。 **MediaPlayerElement** 是輕量型的 XAML 控制項，可用來轉譯 XAML 頁面中的 **MediaPlayer** 內容。 **MediaElement** 控制項仍持續受支援，以提供回溯相容性。 如需使用 **MediaPlayer** 與 **MediaPlayerElement** 播放媒體內容的詳細資訊，請參閱[使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)。 如需使用 **MediaSource** 和相關 API 的詳細資訊，請參閱[媒體項目、播放清單和曲目](media-playback-with-mediasource.md)。

## <a name="media-casting-with-the-castingdevicepicker"></a>使用 CastingDevicePicker 傳播媒體

將媒體傳播到裝置的第二個方法是使用 [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525)。 若要使用這個類別，請在專案中加入 [**Windows.Media.Casting**](https://msdn.microsoft.com/library/windows/apps/dn972568) 命名空間。

[!code-cs[CastingNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingNamespace)]

宣告 **CastingDevicePicker** 物件的成員變數。

[!code-cs[DeclareCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareCastingPicker)]

當頁面初始化之後，建立傳播選擇器的新執行個體，並將 [**Filter**](https://msdn.microsoft.com/library/windows/apps/dn972540) 設定到 [**SupportsVideo**](https://msdn.microsoft.com/library/windows/apps/dn972526) 屬性，指出選擇器所列出的傳播裝置應該支援視訊。 針對使用者挑選傳播的裝置時所引發的 [**CastingDeviceSelected**](https://msdn.microsoft.com/library/windows/apps/dn972539) 事件，註冊處理常式。

[!code-cs[InitCastingPicker](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetInitCastingPicker)]

在 XAML 檔案中，新增按鈕讓使用者啟動選擇器。

[!code-xml[CastPickerButton](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCastPickerButton)]

在按鈕的 **Click** 事件處理常式中，呼叫 [**TransformToVisual**](https://msdn.microsoft.com/library/windows/apps/br208986)，以取得相對於另一個元素的 UI 元素轉換。 在這個範例中，轉換是指傳播選擇器按鈕的位置，相對於應用程式視窗的視覺化根目錄。 呼叫 [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525) 物件的 [**Show**](https://msdn.microsoft.com/library/windows/apps/dn972542) 方法，以啟動傳播選擇器對話方塊。 指定傳播選擇器按鈕的位置和尺寸，讓系統可以從使用者按下的按鈕帶出對話方塊。

[!code-cs[CastPickerButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastPickerButtonClick)]

在 **CastingDeviceSelected** 事件處理常式中，呼叫事件引數的 [**SelectedCastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972546) 屬性 (代表使用者選取的傳播裝置) 的 [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547) 方法。 註冊 [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519) 和 [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523) 事件的處理常式。 最後，呼叫 [**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520) 以開始傳播，並將結果傳入 **MediaPlayerElement** 控制項的 **MediaPlayer** 物件的 [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012) 方法，以指定要傳播的媒體是與 **MediaPlayerElement** 關聯的 **MediaPlayer** 的內容。

> [!NOTE] 
> 傳播連線必須在 UI 執行緒上起始。 因為UI 執行緒上不會呼叫 **CastingDeviceSelected**，您必須將這些呼叫放在 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 的呼叫內，才能在 UI 執行緒上呼叫它們。

[!code-cs[CastingDeviceSelected](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetCastingDeviceSelected)]

在 **ErrorOccurred** 和 **StateChanged** 事件處理常式中，您應該更新 UI 讓使用者知道目前的傳播狀態。 下節的建立自訂傳播裝置選擇器中，將詳細討論這些事件。

[!code-cs[EmptyStateHandlers](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEmptyStateHandlers)]

## <a name="media-casting-with-a-custom-device-picker"></a>使用自訂裝置選擇器來傳播媒體

下節說明如何從程式碼中列舉傳播裝置並起始連線，以建立您自己的傳播裝置選擇器 UI。

若要列舉可用的傳播裝置，請在專案中加入 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459) 命名空間。

[!code-cs[EnumerationNamespace](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

將下列控制項新增到 XAML 頁面，以實作這個範例的初步 UI：

-   用來啟動裝置監控程式的按鈕，以尋找可用的傳播裝置。
-   [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) 控制項會提供使用者回饋，讓他們知道傳播列舉正在進行。
-   [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868)，列出找到的傳播裝置。 定義控制項的 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830)，讓我們可以將傳播裝置物件直接指派給控制項，且仍然顯示 [**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549) 屬性。
-   可讓使用者中斷連接傳播裝置的按鈕。

[!code-xml[CustomPickerXAML](./code/MediaCasting_RS1/cs/MainPage.xaml#SnippetCustomPickerXAML)]

在程式碼後置中，宣告 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) 和 [**CastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972510) 的成員變數。

[!code-cs[DeclareDeviceWatcher](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatcher)]

在 *startWatcherButton* 的 **Click** 處理常式中，首先更新 UI，將按鈕停用，並在裝置列舉進行時讓進度環開始轉動。 清除傳播裝置的清單方塊。

接著，呼叫 [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427)，建立裝置監控程式。 這個方法可以用來監看許多不同類型的裝置。 使用 [**CastingDevice.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn972551) 傳回的裝置選取器字串，指定您要監看支援視訊傳播的裝置。

最後，註冊 [**Added**](https://msdn.microsoft.com/library/windows/apps/br225450)、[**Removed**](https://msdn.microsoft.com/library/windows/apps/br225453)、[**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) 和 [**Stopped**](https://msdn.microsoft.com/library/windows/apps/br225457) 事件的事件處理常式。

[!code-cs[StartWatcherButtonClick](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStartWatcherButtonClick)]

當監控程式發現新的裝置時會引發 **Added** 事件。 在這個事件的處理常式中，呼叫 [**CastingDevice.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn972550)，並傳入已發現的傳播裝置的識別碼 (包含在傳給處理常式的 **DeviceInformation** 物件中)，以建立新的 [**CastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972524) 物件。

將 **CastingDevice** 新增到傳播裝置 **ListBox**，供使用者選取。 根據 XAML 中定義的 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830)，[**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549) 屬性會當做清單方塊中的項目文字。 因為 UI 執行緒上不會呼叫這個事件處理常式，您必須從 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 的呼叫內更新 UI。

[!code-cs[WatcherAdded](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherAdded)]

當監控程式偵測到傳播裝置已不存在時會引發 **Removed** 事件。 比較傳入處理常式的 **Added** 物件的 ID 屬性，與清單方塊的 [**Items**](https://msdn.microsoft.com/library/windows/apps/br242823) 集合中每個 **Added** 的 ID。 如果識別碼相符，則從集合中移除該物件。 同樣地，因為是更新 UI，所以必須從 **RunAsync** 呼叫內執行這個呼叫。

[!code-cs[WatcherRemoved](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherRemoved)]

當監控程式完成偵測裝置時會引發 **EnumerationCompleted** 事件。 在這個事件的處理常式中，更新 UI 讓使用者知道裝置列舉已完成，並呼叫 [**Stop**](https://msdn.microsoft.com/library/windows/apps/br225456) 來停止裝置監控程式。

[!code-cs[WatcherEnumerationCompleted](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherEnumerationCompleted)]

當裝置監控程式完成停止時會引發 Stopper 事件。 在這個事件的處理常式中，停止 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) 控制項並重新啟用 *startWatcherButton*，讓使用者可以重新啟動裝置列舉程序。

[!code-cs[WatcherStopped](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetWatcherStopped)]

當使用者從清單方塊選取其中一個傳播裝置時，就會引發 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 事件。 在這個處理常式內，將會建立傳播連線並開始傳播。

首先，請先確認裝置監控程式已停止，以避免裝置列舉干擾媒體傳播。 在使用者所選取的 **CastingDevice** 物件上，呼叫 [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547) 來建立傳播連線。 新增 [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523) 和 [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519) 事件的事件處理常式。

呼叫 [**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520)，並傳入呼叫 **MediaPlayer** 方法 [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012) 所傳回的傳播來源，以開始傳播媒體。 最後，顯示中斷連線按鈕，讓使用者停止媒體傳播。

[!code-cs[SelectionChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetSelectionChanged)]

在狀態變更處理常式中，您所採取的動作取決於傳播連線的新狀態：

-   如果狀態為 **Connected** 或 **Rendering**，則確保 **ProgressRing** 控制項為非使用中，並顯示中斷連線按鈕。
-   如果狀態為 **Disconnected**，則取消選取清單方塊中目前的傳播裝置、將 **ProgressRing** 控制項變成非使用中，並隱藏中斷連線按鈕。
-   如果狀態為 **Connecting**，則將 **ProgressRing** 控制項變成使用中，並隱藏中斷連線按鈕。
-   如果狀態為 **Disconnecting**，則將 **ProgressRing** 控制項變成使用中，並隱藏中斷連線按鈕。

[!code-cs[StateChanged](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetStateChanged)]

在 **ErrorOccurred** 事件的處理常式中，更新 UI 讓使用者知道發生傳播錯誤，並取消選取清單方塊中目前的 **CastingDevice** 物件。

[!code-cs[ErrorOccurred](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetErrorOccurred)]

最後，實作中斷連線按鈕的處理常式。 呼叫 **CastingConnection** 物件的 [**DisconnectAsync**](https://msdn.microsoft.com/library/windows/apps/dn972518) 方法，停止媒體傳播並中斷連接傳播裝置。 必須呼叫 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317)，將這個呼叫分派至 UI 執行緒。

[!code-cs[DisconnectButton](./code/MediaCasting_RS1/cs/MainPage.xaml.cs#SnippetDisconnectButton)]

 

 




