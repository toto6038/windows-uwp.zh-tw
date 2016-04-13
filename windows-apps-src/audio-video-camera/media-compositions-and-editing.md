---
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: Windows.Media.Editing 命名空間中的 API 可讓您快速開發 app，讓使用者從音訊和視訊來源檔案建立媒體組合。
title: 媒體組合和編輯
---

# 媒體組合和編輯

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


[
            **Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) 命名空間中的 API 可讓您快速開發 app，讓使用者從音訊和視訊來源檔案建立媒體組合。 架構的功能包括能夠以程式設計的方式一起新增多個視訊剪輯、新增視訊與影像重疊、新增背景音訊，以及套用音訊與視訊效果。 建立之後，媒體組合可以轉譯為一般媒體檔案來播放或共用，但是組合也可以序列化至磁碟和從磁碟還原序列化，允許使用者載入和修改他們之前建立的組合。 這項功能是以方便使用的 Windows 執行階段介面提供，相較於低階 [Microsoft 媒體基礎](https://msdn.microsoft.com/library/windows/desktop/ms694197) API時，大幅減少執行這些工作所需之程式碼的數量和複雜度。

## 建立新的媒體組合

[
            **MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) 類別是適用於所有媒體剪輯的容器，這些媒體剪輯組成組合，負責轉譯最終組合、將組合載入和儲存到光碟，以及提供組合的預覽串流，讓使用者可以在 UI 中檢視。 若要在您的 app 中使用 **MediaComposition**，請包含 [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) 命名空間以及 [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) 命名空間，該命名空間提供您需要的相關 API。

[!code-cs[Namespace1](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace1)]

將會從您的程式碼的多個點存取 **MediaComposition** 物件，因此通常您會宣告其中的成員變數以儲存它。

[!code-cs[DeclareMediaComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

**MediaComposition** 的建構函式沒有引數。

[!code-cs[MediaCompositionConstructor](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetMediaCompositionConstructor)]

## 將媒體剪輯新增至組合

媒體組合通常包含一或多個視訊剪輯。 您可以使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/hh738369) 以允許使用者選取視訊檔案。 一旦選取檔案，建立新的 [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) 物件以包含視訊剪輯，方法是呼叫 [**MediaClip.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607)。 接著您將剪輯新增至 **MediaComposition** 物件的 [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648) 清單。

[!code-cs[PickFileAndAddClip](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetPickFileAndAddClip)]

-   媒體剪輯會以在 [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648) 清單中顯示的相同順序出現在 **MediaComposition** 中。

-   **MediaClip** 只能併入組合一次。 嘗試新增已由組合使用的 **MediaClip** 會導致錯誤。 若要在組合中重複使用視訊剪輯多次，請呼叫 [**Clone**](https://msdn.microsoft.com/library/windows/apps/dn652599) 以建立稍後可以新增至組合的新 **MediaClip** 物件。

-   通用 Windows app 沒有權限可以存取整個檔案系統。 [
            **StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) 類別的 [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) 屬性可以讓您的 app 儲存檔案的記錄，該檔案已由使用者選取，所以您可以保留存取檔案的權限。 **FutureAccessList** 具有最多的 1000 個項目，所以您的 app 需要管理清單以確定不會變滿。 如果您計劃支援載入和修改之前建立的組合，這特別重要。

-   **MediaComposition** 支援 MP4 格式的視訊剪輯。

-   如果視訊檔案包含多個內嵌的曲目，您可以藉由設定 [**SelectedEmbeddedAudioTrackIndex**](https://msdn.microsoft.com/library/windows/apps/dn652627) 屬性，選取要在組合中使用哪個曲目。

-   建立單一色彩填滿整個框架的 **MediaClip**，方法是呼叫 [**CreateFromColor**](https://msdn.microsoft.com/library/windows/apps/dn652605) 以及指定色彩和剪輯的持續時間。

-   從影像檔建立 **MediaClip**，方法是呼叫 [**CreateFromImageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652610) 以及指定影像檔和剪輯的持續時間。

-   從 [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) 建立 **MediaClip**，方法是呼叫 [**CreateFromSurface**](https://msdn.microsoft.com/library/dn764774) 以及指定表面和剪輯的持續時間。

## 預覽 MediaElement 中的組合

若要讓使用者檢視媒體組合，請將 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 新增至定義您 UI 的 XAML 檔案。

[!code-xml[MediaElement](./code/MediaEditing/cs/MainPage.xaml#SnippetMediaElement)]

宣告類型 [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn282716) 的成員變數。


[!code-cs[DeclareMediaStreamSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaStreamSource)]

呼叫 **MediaComposition** 物件的 [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) 方法來建立組合的 **MediaStreamSource**，然後呼叫 **MediaElement** 的 [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029) 方法。 現在可以在 UI 中檢視組合。


[!code-cs[UpdateMediaElementSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetUpdateMediaElementSource)]

-   **MediaComposition** 必須包含至少一個媒體剪輯，然後才能呼叫 [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674)，否則傳回的物件將會是 Null。

-   **MediaElement** 時間軸不會自動更新以反映組合中的變更。 當您每次對組合進行一組變更並且想要更新 UI 時，建議您同時呼叫 **GeneratePreviewMediaStreamSource** 和 **SetMediaStreamSource**。

當使用者離開頁面以釋放相關聯的資源時，建議您將 **MediaStreamSource** 物件和 **MediaElement** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 屬性設為 Null。

[!code-cs[OnNavigatedFrom](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

## 將組合轉譯為視訊檔案

若要將媒體組合轉譯為一般視訊檔案，以便共用及在其他裝置上檢視，您需要使用 [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) 命名空間的 API。 若要更新非同步作業進度的 UI，您也需要 [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383) 命名空間的 API。

[!code-cs[Namespace2](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace2)]

在允許使用者使用 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) 選取輸出檔案之後，藉由呼叫 **MediaComposition** 物件的 [**RenderToFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652690)，將組合轉譯為選取的檔案。 下列範例中的其他程式碼只是遵循處理 [**AsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/desktop/br205807) 的模式。

[!code-cs[RenderCompositionToFile](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetRenderCompositionToFile)]

-   [
            **MediaTrimmingPreference**](https://msdn.microsoft.com/library/windows/apps/dn640561) 可讓您設定轉碼作業速度與修剪相鄰媒體剪輯的精確度的優先順序。 **Fast** 會讓轉碼速度較快而修剪精確度較低，**Precise** 會讓轉碼較慢而修剪精確度較高。

## 修剪視訊剪輯

修剪組合中視訊剪輯的持續時間，方法是設定 [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) 物件的 [**TrimTimeFromStart**](https://msdn.microsoft.com/library/windows/apps/dn652637) 屬性、[**TrimTimeFromEnd**](https://msdn.microsoft.com/library/windows/apps/dn652634) 屬性，或兩者同時設定。

[!code-cs[TrimClipBeforeCurrentPosition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetTrimClipBeforeCurrentPosition)]

-   您可以使用任何您想要的 UI 讓使用者指定開始和結束修剪值。 上述範例使用 **MediaElement** 的 [**Position**](https://msdn.microsoft.com/library/windows/apps/br227407) 屬性，藉由檢查 [**StartTimeInComposition**](https://msdn.microsoft.com/library/windows/apps/dn652629) 和 [**EndTimeInComposition**](https://msdn.microsoft.com/library/windows/apps/dn652618)，先決定要在目前位置播放哪個 MediaClip。 然後再次使用 **Position** 和 **StartTimeInComposition** 屬性，計算從剪輯開頭修剪的時間量。 **FirstOrDefault** 方法是 **System.Linq** 命名空間的擴充方法，可簡化從清單中選取項目的程式碼。
-   **MediaClip** 物件的 [**OriginalDuration**](https://msdn.microsoft.com/library/windows/apps/dn652625) 屬性可讓您知道未套用任何剪輯的媒體剪輯的持續時間。
-   [
            **TrimmedDuration**](https://msdn.microsoft.com/library/windows/apps/dn652631) 屬性可讓您知道套用修剪之後的媒體剪輯的持續時間。
-   指定大於媒體剪輯原始持續時間的修剪值不會擲回錯誤。 不過，如果組合只包含單一剪輯，並且藉由指定大的修剪值而修剪為零長度，[**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) 的後續呼叫會傳回 Null，如同組合沒有任何剪輯。

## 將背景曲目新增至組合

若要將背景曲目新增至組合，載入音訊檔案然後建立 [**BackgroundAudioTrack**](https://msdn.microsoft.com/library/windows/apps/dn652544) 物件，方法是呼叫 Factory 方法 [**BackgroundAudioTrack.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652561)。 接著將 **BackgroundAudioTrack** 新增至組合的 [**BackgroundAudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn652647) 屬性。

[!code-cs[AddBackgroundAudioTrack](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddBackgroundAudioTrack)]

-   **MediaComposition** 支援下列格式的背景曲目：MP3、WAV、FLAC

-   背景曲目

-   如同使用視訊檔案，您應該使用 [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) 類別來保留組合中檔案的存取權。

-   如同使用 **MediaClip**，**BackgroundAudioTrack** 只能併入組合一次。 嘗試新增已由組合使用的 **BackgroundAudioTrack** 會導致錯誤。 若要在組合中重複使用曲目多次，請呼叫 [**Clone**](https://msdn.microsoft.com/library/windows/apps/dn652599) 以建立稍後可以新增至組合的新 **MediaClip** 物件。

-   根據預設，背景曲目會在組合的開頭開始播放。 如果有多個背景曲目存在，所有曲目都會在組合的開頭開始播放。 若要讓背景曲目在其他時間開始播放，請將 [**Delay**](https://msdn.microsoft.com/library/windows/apps/dn652563) 屬性設為想要的時間起始位移。

## 將重疊新增至組合

重疊可讓您在組合中每個視訊層上方堆疊多個視訊層。 組合可以包含多個重疊層，每一個可以包含多個重疊。 將 **MediaClip** 傳遞到其建構函式，以建立 [**MediaOverlay**](https://msdn.microsoft.com/library/windows/apps/dn764793) 物件。 設定重疊的位置和不透明度，然後建立新的 [**MediaOverlayLayer**](https://msdn.microsoft.com/library/windows/apps/dn764795)，並將 **MediaOverlay** 新增至其 [**Overlays**](https://msdn.microsoft.com/library/windows/desktop/dn280411) 清單。 最後，將 **MediaOverlayLayer** 新增至組合的 [**OverlayLayers**](https://msdn.microsoft.com/library/windows/apps/dn764791) 清單。

[!code-cs[AddOverlay](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddOverlay)]

-   視訊層內的重疊是根據其包含層之 **Overlays** 清單的順序進行 z 排序。 清單中具有較高索引的項目會轉譯在較低索引項目的上方。 組合中的重疊層也是如此。 在組合的 **OverlayLayers** 清單中具有較高索引的視訊層會轉譯在較低索引視訊層的上方。

-   因為重疊是堆疊在彼此上方，而不是順序播放，所以所有重疊預設會在組合的開頭開始播放。 若要讓重疊在其他時間開始播放，請將 [**Delay**](https://msdn.microsoft.com/library/windows/apps/dn764810) 屬性設為想要的時間起始位移。

## 將效果新增至媒體剪輯

組合中的每個 **MediaClip** 都有一份可以加入多個效果的音訊與視訊效果清單。 效果必須分別實作 [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044) 和 [**IVideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608047)。 下列範例會使用目前的媒體元件位置以選擇目前檢視的 **MediaClip**，然後建立 [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762) 的新執行個體，並將它附加到媒體剪輯的 [**VideoEffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/dn652643) 清單。

[!code-cs[AddVideoEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddVideoEffect)]

## 將組合儲存至檔案

媒體組合稍後可以序列化至檔案以進行修改。 挑選輸出檔案，然後呼叫 [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) 方法 [**SaveAsync**](https://msdn.microsoft.com/library/windows/apps/dn640554) 以儲存組合。

[!code-cs[SaveComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetSaveComposition)]

## 從檔案載入組合

可以從檔案還原序列化媒體組合，讓使用者檢視和修改組合。 挑選組合檔案，然後呼叫 [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) 方法 [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/dn652684) 以載入組合。

[!code-cs[OpenComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOpenComposition)]

-   如果組合中的媒體檔案不是在您 app 可存取的位置，且不是在 app 的 [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) 類別 [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) 屬性中，則在載入組合時會擲回錯誤。

 

 






<!--HONumber=Mar16_HO1-->


