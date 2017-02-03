---
author: Jwmsft
Description: "媒體播放器可用來檢視及聆聽視訊、音訊及影像。"
title: "媒體播放器"
ms.assetid: 9AABB5DE-1D81-4791-AB47-7F058F64C491
dev.assetid: AF2F2008-9B53-430C-BBC3-8888F631B0B0
label: Media player
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: 155f1712beb70d2a1fc93809ddc254e8f6e6f4a2

---
# <a name="media-player"></a>媒體播放器

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

媒體播放器可用來檢視及聆聽視訊和音訊。 媒體播放功能可以內嵌 (內嵌在頁面中或內嵌在一組其他的控制項中)，或以專用的全螢幕檢視方式顯示。 您可以修改播放器的按鈕集、變更控制列的背景，以及自行調整版面配置。 但請記住，使用者只想要一組基本的控制項 (播放/暫停、倒轉跳過、快轉跳過)。

![具有傳輸控制項的媒體播放器元素](images/controls/mtc_double_video_inprod.png)

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**MediaPlayerElement 類別**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)</li>
<li>[**MediaTransportControls 類別**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols)</li>
</ul>
</div>


> **注意**&nbsp;&nbsp; **MediaPlayerElement** 只能在 Windows 10 版本 1607 及以上的版本中取得。 如果您是針對舊版 Windows 10 開發 app，便必須改為使用 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)。 此頁面上的所有建議也適用於 MediaElement。

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

當您想要在您的應用程式中播放音訊或視訊時，請使用媒體播放器。 若要顯示影像的集合，請使用[翻轉檢視](flipview.md)。

## <a name="examples"></a>範例

Windows 10 入門 app 中的媒體播放器。

![Windows 10 入門 app 中的媒體元素。](images/control-examples/mtc_getstarted_example.png)

## <a name="create-a-media-player"></a>建立媒體播放器
在 XAML 中建立 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 物件，並將 [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 設定成指向音訊或視訊檔案的 [**MediaSource**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.core.mediasource.aspx)，來將媒體到新增您的 app。

此 XAML 會建立 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 並將其 [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 屬性設定成指向 app 的本機視訊檔案 URI。 **MediaPlayerElement** 會在頁面載入之後開始播放。 若要抑制媒體立即播放，您可以將 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.autoplay.aspx) 屬性設定成 **false**。

```xaml
<MediaPlayerElement x:Name="mediaSimple"
                    Source="Videos/video1.mp4"
                    Width="400" AutoPlay="True"/>
```

此 XAML 會建立已啟用內建傳輸控制項的 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)，並將 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.autoplay.aspx) 屬性設定為 **false**。


```xaml
<MediaPlayerElement x:Name="mediaPlayer"
                    Source="Videos/video1.mp4"
                    Width="400"
                    AutoPlay="False"
                    AreTransportControlsEnabled="True"/>
```

### <a name="media-transport-controls"></a>媒體傳輸控制項
[**MediaPlayerElement**]((https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 有內建的傳輸控制項，可處理播放、停止、暫停、音量、靜音、搜尋/進度、隱藏式輔助字幕及曲目選取。 若要啟用這些控制項，請將 [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.AreTransportControlsEnabled.aspx) 設定為 **true**。 若要停用，請將 **AreTransportControlsEnabled** 設定為 **false**。 傳輸控制項是由 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962) 類別來表示。 您可以依原樣使用傳輸控制項，或是以各種方式自訂它們。 如需詳細資訊，請參閱 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962) 類別參考和[建立自訂傳輸控制項](custom-transport-controls.md)。

傳輸控制項支援單列與雙列配置。 這裡的第一個範例是單列配置，播放/暫停按鈕位於媒體時間軸左邊。 此配置最適合保留給內嵌媒體播放與小型螢幕使用。

![單列 MTC 控制項範例](images/controls/mtc_single_inprod_02.png)

在大部分使用情況下 (特別是在較大的螢幕上 )，建議使用下面的雙列控制項配置。 此配置可提供更多空間供控制項使用，並且可讓使用者更容易操作時間軸。

![手機上雙列 MTC 控制項範例](images/controls/mtc_double_inprod.png)

**系統媒體傳輸控制項**

[**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 會自動與系統媒體傳輸控制項整合。 系統媒體傳輸控制項是指當使用者按下硬體媒體鍵 (例如鍵盤上的媒體按鈕) 時，會以快顯方式顯示的控制項。 如需詳細資訊，請參閱 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)。

> **注意**&nbsp;&nbsp; [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 不會自動與系統媒體傳輸控制項整合，因此您必須自行加以連接。 如需詳細資訊，請參閱[系統媒體傳輸控制項](https://msdn.microsoft.com/library/windows/apps/mt228338)。


### <a name="set-the-media-source"></a>設定媒體來源
若要播放位於網路上的檔案或內嵌於 app 的檔案，請將 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 的 [**Source**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.core.mediasource.aspx) 屬性設定為檔案的路徑。

**提示：**若要從網際網路開啟檔案，您需要在 app 資訊清單中宣告**網際網路 (用戶端)** 功能 (Package.appxmanifest)。 如需宣告功能的詳細資訊，請參閱 [App 功能宣告](https://msdn.microsoft.com/library/windows/apps/mt270968)。

 

此程式碼會嘗試將 XAML 中定義之 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 屬性設定為在 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 中輸入的檔案路徑。

```xaml
<TextBox x:Name="txtFilePath" Width="400"
         FontSize="20"
         KeyUp="TxtFilePath_KeyUp"
         Header="File path"
         PlaceholderText="Enter file path"/>
```

```csharp
private void TxtFilePath_KeyUp(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == Windows.System.VirtualKey.Enter)
    {
        TextBox tbPath = sender as TextBox;

        if (tbPath != null)
        {
            LoadMediaFromString(tbPath.Text);
        }
    }
}

private void LoadMediaFromString(string path)
{
    try
    {
        Uri pathUri = new Uri(path);
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

若要將媒體來源設定為內嵌於 app 中的媒體檔案，請初始化具有 **ms-appx:///** 為開頭之路徑的 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017)，以此 Uri 建立 [**MediaSource**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.core.mediasource.aspx)，然後將 [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 設定為該 Uri。 例如，如果檔案名為 **video1.mp4** 並且位於 **Videos** 子資料夾中，則路徑應該看起來如下：**ms-appx:///Videos/video1.mp4**

此程式碼會將先前在 XAML 中定義之 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 屬性設定為 **ms-appx:///Videos/video1.mp4**。

```csharp
private void LoadEmbeddedAppFile()
{
    try
    {
        Uri pathUri = new Uri("ms-appx:///Videos/video1.mp4");
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

### <a name="open-local-media-files"></a>開啟本機媒體檔案
若要開啟本機系統或 OneDrive 上的檔案，您可以使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 來取得檔案，並使用 [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 來設定媒體來源，或是以程式設計方式存取使用者媒體資料夾。

如果您的 app 需要不藉助使用者互動就能存取 [音樂]**** 或 [影片]**** 資料夾 (例如，如果您要列舉使用者收藏中的所有音樂或影片檔案並將它們顯示在 app 中)，您就必須宣告「音樂媒體櫃」****和「影片媒體櫃」****功能。 如需詳細資訊，請參閱[音樂、圖片及影片媒體櫃中的檔案和資料夾](https://msdn.microsoft.com/library/windows/apps/mt188703)。

[**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 不需要特殊功能來存取本機檔案系統 (例如，使用者的 [音樂]**** 或 [影片]**** 資料夾) 上的檔案，因為使用者可以完全控制存取的檔案。 從安全性與隱私權的立場看，最佳做法是將 app 使用的「功能」數降到最小。

**使用 FileOpenPicker 開啟本機媒體**

1.  呼叫 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)，讓使用者挑選媒體檔案。

    使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 類別選取媒體檔案。 設定 [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850)，來指定 **FileOpenPicker** 顯示的檔案類型。 呼叫 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275)，以啟動檔案選擇器並取得檔案。

2.  使用 [**MediaSource**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.core.mediasource.aspx)，將所選的媒體檔案設定為 [**MediaPlayerElement.Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx)。

    若要使用從 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br227171) 傳回的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br207847)，您需要呼叫 [**MediaSource**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.core.mediasource.createfromstoragefile.aspx) 上的 [**CreateFromStorageFile**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.core.mediasource.aspx) 方法，並將它設定為 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)。 接著，請呼叫 [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.play.aspx) 上的 [**Play**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) 來啟動媒體。


這個範例示範如何使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 來選擇檔案，並將該檔案設定為 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)。

```xaml
<MediaPlayerElement x:Name="mediaPlayer"/>
...
<Button Content="Choose file" Click="Button_Click"/>
```

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    await SetLocalMedia();
}

async private System.Threading.Tasks.Task SetLocalMedia()
{
    var openPicker = new Windows.Storage.Pickers.FileOpenPicker();

    openPicker.FileTypeFilter.Add(".wmv");
    openPicker.FileTypeFilter.Add(".mp4");
    openPicker.FileTypeFilter.Add(".wma");
    openPicker.FileTypeFilter.Add(".mp3");

    var file = await openPicker.PickSingleFileAsync();

    // mediaPlayer is a MediaPlayerElement defined in XAML
    if (file != null)
    {
        mediaPlayer.Source = MediaSource.CreateFromStorageFile(file);

        mediaPlayer.MediaPlayer.Play();
    }
}
```

### <a name="set-the-poster-source"></a>設定海報來源
您可以使用 [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.PosterSource.aspx) 屬性，來在載入媒體之前以視覺化呈現方式提供 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)。 **PosterSource** 是指螢幕擷取畫面或電影海報等，這類會取代媒體顯示的影像。 **PosterSource** 會在下列情況顯示：

-   未設定有效的來源。 例如，未設定 [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx)、已將 **Source** 設定為 **Null**，或來源無效 (如同發生 [**MediaFailed**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.mediafailed.aspx) 事件的情況)。
-   媒體正在載入時。 例如，已設定有效來源，但尚未發生 [**MediaOpened**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.mediaopened.aspx) 事件。
-   媒體正在串流處理到另一個裝置時。
-   媒體只包含音訊時。

以下的 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 已將其 [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 設定為專輯曲目，並將其 [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.PosterSource.aspx) 設定為專輯封面的影像。

```xaml
<MediaPlayerElement Source="Media/Track1.mp4" PosterSource="Media/AlbumCover.png"/>
```

### <a name="keep-the-devices-screen-active"></a>讓裝置的螢幕保持使用中
裝置通常會在使用者離開時讓顯示器變暗 (最後會將它關閉) 以延長電池壽命，但是視訊 app 需要讓螢幕一直開著，才能讓使用者觀賞視訊。 若要防止顯示器在未偵測到使用者動作時停用 (例如，app 正在播放影片時)，您可以呼叫 [**DisplayRequest.RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818)。 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 類別讓您告訴 Windows 保持開啟顯示器，讓使用者可以觀看影片。

若要省電並延長電池壽命，您應該呼叫 [**DisplayRequest.RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819)，以便在不再需要時釋放顯示器要求。 當您的 App 未顯示於螢幕上時，Windows 會自動停用它的啟用顯示要求，並在 App 回到前景時重新啟用顯示要求。

以下是一些您應該釋放顯示器要求的情況：

-   視訊播放暫停時，例如在因使用者動作、緩衝或因為頻寬有限而進行調整的情況下。
-   播放停止時。 例如，視訊播放完畢或簡報結束。
-   發生播放錯誤時。 例如，網路連線問題或檔案損毀。

> **注意**&nbsp;&nbsp; 如果 [**MediaPlayerElement.IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.IsFullWindow.aspx) 設為 true，且媒體正在播放，系統將會自動防止顯示器停用。

**讓螢幕保持使用中**

1.  建立全域 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 變數。 將變數初始化為 null。
```csharp
// Create this variable at a global scope. Set it to null.
private DisplayRequest appDisplayRequest = null;
```

2.  呼叫 [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818)，通知 Windows，App 需要讓顯示器保持開啟。

3.  每當影片播放停止、暫停或因播放錯誤而中斷時，呼叫 [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) 以釋放顯示要求。 當 App 不再有任何啟用的顯示要求時，Windows 會在沒有使用裝置時將顯示器畫面變暗 (最後將它關閉) 以延長電池壽命。

    每個 [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) 都有一個 [**MediaPlaybackSession**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.playbacksession.aspx) 類型 的 [**PlaybackSession**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacksession.aspx)，可控制媒體播放的各種方面，例如 [**PlaybackRate**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackrate.aspx)、[**PlaybackState**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackstate.aspx) 及 [**Position**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacksession.position.aspx)。 在這裡，您將使用 [**MediaPlayer.PlaybackSession**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.playbacksession.aspx) 上的 [**PlaybackStateChanged**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackstatechanged.aspx) 事件，來偵測何時應該釋放顯示器要求。 然後，使用 [**NaturalVideoHeight**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacksession.naturalvideoheight.aspx) 屬性來判斷是否正在播放音訊或視訊檔案，並只在播放視訊時才讓螢幕保持使用中狀態。
    ```xaml
<MediaPlayerElement x:Name="mpe" Source="Media/video1.mp4"/>
    ```

    ```csharp
    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        mpe.MediaPlayer.PlaybackSession.PlaybackStateChanged += MediaPlayerElement_CurrentStateChanged;
        base.OnNavigatedTo(e);
    }

    private void MediaPlayerElement_CurrentStateChanged(object sender, RoutedEventArgs e)
    {
        MediaPlaybackSession playbackSession = sender as MediaPlaybackSession;
        if (playbackSession != null && playbackSession.NaturalVideoHeight != 0)
        {
            if(playbackSession.PlaybackState == MediaPlaybackState.Playing)
            {
                if(appDisplayRequest == null)
                {
                    // This call creates an instance of the DisplayRequest object
                    appDisplayRequest = new DisplayRequest();
                    appDisplayRequest.RequestActive();
                }
            }
            else // PlaybackState is Buffering, None, Opening or Paused
            {
                if(appDisplayRequest != null)
                {
                      // Deactivate the displayr request and set the var to null
                      appDisplayRequest.RequestRelease();
                      appDisplayRequest = null;
                }
            }
        }

    }
    ```

### <a name="control-the-media-player-programmatically"></a>以程式設計方式控制媒體播放器
[**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 提供許多屬性、方法和事件，來透過 [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) 屬性控制音訊和視訊播放。 如需完整的屬性、方法及事件清單，請參閱 [**MediaPlayer**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.aspx) 參考頁面。

### <a name="advanced-media-playback-scenarios"></a>進階的媒體播放案例
針對如播放播放清單、在音訊語言之間切換，或是建立自訂的中繼資料曲目等較為複雜的媒體播放案例，請將 [**MediaPlayerElement.Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 設定為 [**MediaPlaybackItem**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybackitem.aspx) 或 [**MediaPlaybackList**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplaybacklist.aspx)。 如需如何啟用各種進階媒體功能的詳細資訊，請參閱開發人員中心的[**媒體播放**](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/media-playback-with-mediasource)頁面。

### <a name="enable-full-window-video-rendering"></a>啟用完整視窗視訊呈現

設定 [**IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.isfullwindow.aspx) 屬性來啟用和停用完整視窗呈現。 如果要在 app 中以程式設計方式設定完整視窗呈現，您應該一律使用 **IsFullWindow** 而不是手動進行。 **IsFullWindow** 可保證執行系統層級最佳化，以增進效能及電池壽命。 如果未正確設定完整視窗呈現，將不會啟用這些最佳化。

以下的部分程式碼會建立能切換完整視窗呈現的 [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244)。

```xaml
<AppBarButton Icon="FullScreen"
              Label="Full Window"
              Click="FullWindow_Click"/>>
```

```csharp
private void FullWindow_Click(object sender, object e)
{
    mediaPlayer.IsFullWindow = !media.IsFullWindow;
}
```

### <a name="resize-and-stretch-video"></a>調整視訊大小和延展視訊

使用 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.stretch.aspx) 屬性，來變更視訊內容，和/或 [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.postersource.aspx) 填入所在容器的方式。 視 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968) 值而定，這樣做可能會延展視訊。 **Stretch** 狀態和許多電視機上的影像大小設定類似。 您可以將勾點設定在按鈕上，並允許使用者依偏好選擇所要的設定。

-   [**None**](https://msdn.microsoft.com/library/windows/apps/br242968) 會以原始大小顯示內容的原生解析度。
-   [**Uniform**](https://msdn.microsoft.com/library/windows/apps/br242968) 會盡可能地填滿空間，同時維持影像內容的外觀比例。 這會導致視訊邊緣出現水平或垂直的黑色長條。 這和寬螢幕模式類似。
-   [**UniformToFill**](https://msdn.microsoft.com/library/windows/apps/br242968) 會填滿個空間，同時維持外觀比例。 這會導致部分影像被裁切。 這和全螢幕模式類似。
-   [**Fill**](https://msdn.microsoft.com/library/windows/apps/br242968) 會填滿整個空間，但不會維持外觀比例。 影像不會被裁切，但可能發生延展現象。 這和延展模式類似。

![伸展列舉值](images/Image_Stretch.jpg)

此處的 [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) 可用來循環顯示 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968) 選項。 **switch** 陳述式會檢查 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br227422) 屬性目前的狀態，並將它設定成 **Stretch** 列舉中的下一個值。 這樣做可允許使用者循環使用不同的延展狀態。

```xaml
<AppBarButton Icon="Switch"
              Label="Resize Video"
              Click="PictureSize_Click" />
```

```csharp
private void PictureSize_Click(object sender, RoutedEventArgs e)
{
    switch (mediaPlayer.Stretch)
    {
        case Stretch.Fill:
            mediaPlayer.Stretch = Stretch.None;
            break;
        case Stretch.None:
            mediaPlayer.Stretch = Stretch.Uniform;
            break;
        case Stretch.Uniform:
            mediaPlayer.Stretch = Stretch.UniformToFill;
            break;
        case Stretch.UniformToFill:
            mediaPlayer.Stretch = Stretch.Fill;
            break;
        default:
            break;
    }
}
```

### <a name="enable-low-latency-playback"></a>啟用低延遲播放

在 [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.realtimeplayback.aspx) 上將 [**RealTimePlayback**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) 屬性設定為 **true**，以讓媒體元素減少播放的初始延遲。 這對於雙向通訊的 app 而言非常重要，也很適用於部分遊戲案例。 請注意，這個模式需要更大量的資源，而且比較耗電。

這個範例會建立一個 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)，並將 [**RealTimePlayback**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.playback.mediaplayer.realtimeplayback.aspx) 設定為 **true**。


```csharp
MediaPlayerElement mp = new MediaPlayerElement();
mp.MediaPlayer.RealTimePlayback = true;
```

## <a name="recommendations"></a>建議事項

媒體播放器支援淺色和深色佈景主題，但是對於大多數的娛樂案例，深色佈景主題可提供較佳的體驗。 深色背景可提供較高對比，特別是在低光源的情況下，而且可以避免控制列干擾觀賞體驗。

播放視訊內容時，建議以全螢幕模式取代內嵌模式，以獲得更好的觀賞體驗。 全螢幕檢視是最佳體驗模式，況且內嵌模式的選項有限。

如果您有螢幕實際可用空間，或是正在設計 10 呎體驗，請使用雙列配置。 它能比精簡的單列配置提供更多的控制項空間，而且能夠針對 10 呎體驗提供更佳的控制器瀏覽支援。

> **注意**&nbsp;&nbsp; 如需針對 10 呎體驗最佳化應用程式的詳細資訊，請造訪[針對 Xbox 和電視進行設計](../input-and-devices/designing-for-tv.md)文章。

預設控制項已針對媒體播放最佳化，不過，您可以將所需的自訂選項新增至媒體播放器，以為您的 app 提供最佳體驗。 若要深入了解新增自訂控制項，請造訪[建立自訂傳輸控制項](custom-transport-controls.md)。

## <a name="related-articles"></a>相關文章

- [UWP app 的命令設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958433)
- [UWP app 的內容設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958434)



<!--HONumber=Dec16_HO2-->


