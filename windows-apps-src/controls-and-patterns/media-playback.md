---
author: Jwmsft
Description: 媒體播放器可用來檢視及聆聽視訊、音訊及影像。
title: 媒體播放器
ms.assetid: 9AABB5DE-1D81-4791-AB47-7F058F64C491
dev.assetid: AF2F2008-9B53-430C-BBC3-8888F631B0B0
label: Media player
template: detail.hbs
---
# 媒體播放器

媒體播放器可用來檢視及聆聽視訊、音訊及影像。 媒體播放功能可以內嵌 (內嵌在頁面中或內嵌在一組其他的控制項中)，或以專用的全螢幕檢視方式顯示。 您可以修改播放器的按鈕集、變更控制列的背景，以及自行調整版面配置。 但請記住，使用者只想要一組基本的控制項 (播放/暫停、倒轉跳過、快轉跳過)。

![具有傳輸控制項的媒體元素](images/controls/media-transport-controls.png)

<span class="sidebar_heading" style="font-weight: bold;">重要 API</span>

-   [**MediaElement 類別**](https://msdn.microsoft.com/library/windows/apps/br242926)
-   [**MediaTransportControls 類別**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols)

## 這是正確的控制項嗎？

當您想要在您的應用程式中播放音訊或視訊時，請使用媒體播放器。 若要顯示影像的集合，請使用[翻轉檢視](flipview.md)。

## 範例

Windows 10 入門 app 中的媒體元素。

![Windows 10 入門 app 中的媒體元素](images/control-examples/media-element-getstarted.png)

## 建立媒體播放器
使用 XAML 建立 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 物件來新增媒體到您的 app，並將 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 設定成指向音訊或視訊檔案的統一資源識別項 (URI)。

此 XAML 會建立 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 並將其 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 屬性設定成指向 app 本機的視訊檔案的 URI。 **MediaElement** 會在頁面載入之後開始播放。 若要抑制媒體立即播放，您可以將 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/br227360) 屬性設定成 **false**。

```xaml
<MediaElement x:Name="mediaSimple" 
              Source="Videos/video1.mp4" 
              Width="400" AutoPlay="False"/>
```

此 XAML 會建立已啟用內建傳輸控制項的 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)，並將 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/br227360) 屬性設定為 **false**。


```csharp
<MediaElement x:Name="mediaPlayer" 
              Source="Videos/video1.mp4" 
              Width="400" 
              AutoPlay="False"
              AreTransportControlsEnabled="True"/>
```

### 媒體傳輸控制項
MediaElement 具有內建的傳輸控制項，可處理播放、停止、暫停、音量、靜音、搜尋/進度，以及曲目選擇。 若要啟用這些控制項，請將 [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298977) 設定為 **true**。 若要停用，請將 **AreTransportControlsEnabled** 設定為 **false**。 傳輸控制項是由 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962) 類別來表示。 您可以依原樣使用傳輸控制項，或是以各種方式自訂它們。 如需詳細資訊，請參閱 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962) 類別參考和[建立自訂傳輸控制項](custom-transport-controls.md)。

傳輸控制項讓使用者能夠控制 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的大部分層面，但是 **MediaElement** 也提供許多屬性和方法，讓您可用來控制音訊和視訊播放。 如需詳細資訊，請參閱本文稍後的[以程式設計方式控制 MediaElement](#control_mediaelement_programmatically) 一節。

傳輸控制項支援單列與雙列配置。 這裡的第一個範例是單列配置，播放/暫停按鈕位於媒體時間軸左邊。 此配置最適合保留給小型螢幕使用。 

![手機上單列 MTC 控制項範例](images/controls_mtc_singlerow_phone.png)

在大部分使用情況下 (特別是在較大的螢幕上 ) 建議使用下面的雙列控制項配置。 此配置可提供更多空間供控制項使用，並且可讓使用者更容易操作時間軸。

![手機上雙列 MTC 控制項範例](images/controls_mtc_doublerow_phone.png)

**系統媒體傳輸控制項**

您也可以將 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 與系統媒體傳輸控制項整合。 系統傳輸控制項是在使用者按下硬體媒體鍵 (例如鍵盤上的媒體按鈕) 時以快顯方式顯示的控制項。 如果使用者按下鍵盤上的暫停鍵且您的 app 支援 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)，您的 app 會收到通知，而且您可以採取適當的動作。 如需詳細資訊，請參閱[系統媒體傳輸控制項](https://msdn.microsoft.com/library/windows/apps/mt228338)。

### 設定媒體來源
若要播放位於網路上的檔案或內嵌於 app 的檔案，請將 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 屬性設定為檔案的路徑。

**提示：**若要從網際網路開啟檔案，您需要在 app 資訊清單中宣告**網際網路 (用戶端)** 功能 (Package.appxmanifest)。 如需宣告功能的詳細資訊，請參閱 [App 功能宣告](https://msdn.microsoft.com/library/windows/apps/mt270968)。

 

此程式碼會嘗試將以 XAML 定義的 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 屬性，設定為在 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 中輸入的檔案路徑。

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
        mediaPlayer.Source = pathUri;
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

若要將媒體來源設定為內嵌於 app 中的媒體檔案，請建立路徑以 **ms-appx:///** 開頭的 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017)，然後將 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 設定為該路徑。 例如，如果檔案名為 **video1.mp4** 並且位於 **Videos** 子資料夾中，則路徑應該看起來如下：**ms-appx:///Videos/video1.mp4**

此程式碼會將先前使用 XAML 定義的 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 屬性，設定為 **ms-appx:///Videos/video1.mp4**。

```csharp
private void LoadEmbeddedAppFile()
{
    try
    {
        Uri pathUri = new Uri("ms-appx:///Videos/video1.mp4");
        mediaPlayer.Source = pathUri;
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

### 開啟本機媒體檔案
若要開啟本機系統或 OneDrive 上的檔案，您可以使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 來取得檔案，以及使用 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) 來設定媒體來源，或是以程式設計方式存取使用者媒體資料夾。

如果您的 app 需要不藉助使用者互動就能存取 [音樂]**** 或 [影片]**** 資料夾 (例如，如果您要列舉使用者收藏中的所有音樂或影片檔案並將它們顯示在 app 中)，您就必須宣告「音樂媒體櫃」****和「影片媒體櫃」****功能。 如需詳細資訊，請參閱[音樂、圖片及影片媒體櫃中的檔案和資料夾](https://msdn.microsoft.com/library/windows/apps/mt188703)。

[
            **FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 不需要特殊功能來存取本機檔案系統 (例如，使用者的 [音樂]**** 或 [影片]**** 資料夾) 上的檔案，因為使用者可以完全控制存取的檔案。 從安全性與隱私權的立場看，最佳做法是將 app 使用的「功能」數降到最小。

**使用 FileOpenPicker 開啟本機媒體**

1.  呼叫 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)，讓使用者挑選媒體檔案。

    使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 類別選取媒體檔案。 設定 [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850)，來指定 **FileOpenPicker** 顯示的檔案類型。 呼叫 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275)，以啟動檔案選擇器並取得檔案。

2.  呼叫 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338)，將所選的媒體檔案設定為 [**MediaElement.Source**](https://msdn.microsoft.com/library/windows/apps/br227419)。

    若要將 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 設定為從 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 傳回的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)，您需要開啟串流。 呼叫 **StorageFile** 上的 [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851) 方法，其會傳回您可以傳入 [**MediaElement.SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) 方法的串流。 接著，呼叫 **MediaElement** 上的 [**Play**](https://msdn.microsoft.com/library/windows/apps/br227402) 來啟動媒體。

這個範例示範如何使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)來選擇檔案，並將該檔案設定為 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419)。

```xaml
<MediaElement x:Name="mediaPlayer"/>
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
    
    // mediaPlayer is a MediaElement defined in XAML
    if (file != null)
    {
        var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        mediaPlayer.SetSource(stream, file.ContentType);

        mediaPlayer.Play();
    }
}
```

### 設定海報來源
您可以使用 [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/br227409) 屬性，在載入媒體之前，為 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 提供視覺表示。 **PosterSource** 是影像，例如螢幕擷取畫面或電影海報，會取代媒體顯示。 **PosterSource** 會在下列情況顯示：

-   未設定有效的來源。 例如，未設定 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419)、已將 **Source** 設定為 **Null**，或來源無效 (如同發生 [**MediaFailed**](https://msdn.microsoft.com/library/windows/apps/br227393) 事件的情況)。
-   媒體正在載入時。 例如，已設定有效來源，但尚未發生 [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/br227394) 事件。
-   媒體正在串流處理到另一個裝置時。
-   媒體只具有音效時。

以下的 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 已將其 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 設定為專輯曲目，並將其 [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/br227409) 設定為專輯封面的影像。

```xaml
<MediaElement Source="Media/Track1.mp4" PosterSource="Media/AlbumCover.png"/> 
```

### 讓裝置的螢幕保持使用中
裝置通常會在使用者離開時讓顯示器變暗 (最後會將它關閉) 以延長電池壽命，但是視訊 app 需要讓螢幕一直開著，才能讓使用者觀賞視訊。 若要防止顯示器在未偵測到使用者動作時停用 (例如，app 正在播放全螢幕影片時)，您可以呼叫 [**DisplayRequest.RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818)。 [
            **DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 類別讓您告訴 Windows 保持開啟顯示器，讓使用者可以觀看影片。

若要省電並延長電池壽命，您應該呼叫 [**DisplayRequest.RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819)，以便在不再需要時釋放顯示器要求。 當您的 App 未顯示於螢幕上時，Windows 會自動停用它的啟用顯示要求，並在 App 回到前景時重新啟用顯示要求。

以下是一些您應該釋放顯示器要求的情況：

-   視訊播放暫停時，例如在因使用者動作、緩衝或因為頻寬有限而進行調整的情況下。
-   播放停止時。 例如，視訊播放完畢或簡報結束。
-   發生播放錯誤時。 例如，網路連線問題或檔案損毀。

**讓螢幕保持使用中**

1.  建立全域 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 變數。 將變數初始化為 null。
```csharp
// Create this variable at a global scope. Set it to null.
private DisplayRequest appDisplayRequest = null;
```

2.  呼叫 [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818)，通知 Windows，App 需要讓顯示器保持開啟。

3.  每當影片播放停止、暫停或因播放錯誤而中斷時，呼叫 [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) 以釋放顯示要求。 當 App 不再有任何啟用的顯示要求時，Windows 會在沒有使用裝置時將顯示器畫面變暗 (最後將它關閉) 以延長電池壽命。

    您會在此處使用 [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) 事件來偵測這些情況。 然後，使用 [**IsAudioOnly**](https://msdn.microsoft.com/library/windows/apps/hh965334) 屬性來判斷是否正在播放音訊或視訊檔案，唯有當視訊正在播放時，才會讓螢幕保持使用中狀態。
    ```xaml
<MediaElement Source="Media/video1.mp4"
              CurrentStateChanged="MediaElement_CurrentStateChanged"/>
    ```
 
    ```csharp
private void MediaElement_CurrentStateChanged(object sender, RoutedEventArgs e)
{
    MediaElement mediaElement = sender as MediaElement;
    if (mediaElement != null && mediaElement.IsAudioOnly == false)
    {
        if (mediaElement.CurrentState == Windows.UI.Xaml.Media.MediaElementState.Playing)
        {                
            if (appDisplayRequest == null)
            {
                // This call creates an instance of the DisplayRequest object. 
                appDisplayRequest = new DisplayRequest();
                appDisplayRequest.RequestActive();
            }
        }
        else // CurrentState is Buffering, Closed, Opening, Paused, or Stopped. 
        {
            if (appDisplayRequest != null)
            {
                // Deactivate the display request and set the var to null.
                appDisplayRequest.RequestRelease();
                appDisplayRequest = null;
            }
        }            
    }
} 
    ```

### 以程式設計方式控制媒體播放器
[
            **MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 提供許多屬性、方法及事件來控制音訊和視訊播放。 如需完整的屬性、方法及事件清單，請參閱 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 參考頁面。
    

### 選取不同語言的音軌

您可以使用 [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358) 屬性和 [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384) 方法，在視訊上將音訊變更為不同語言的音軌。 視訊也可以包含多個相同語言的音軌，例如，導演對於影片的評論。 這個範例特別示範如何在不同的語言之間切換，但您可以修改此程式碼，以便在任何音軌之間切換。

**選取不同語言的音軌**

1.  取得音軌。

    若要搜尋特定語言的音軌，請透過逐一查看視訊上的每個音軌來開始。 使用 [**AudioStreamCount**](https://msdn.microsoft.com/library/windows/apps/br227356) 作為 **for** 迴圈的最大值。

2.  取得音軌的語言。

    使用 [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384) 方法來取得音軌的語言。 音軌的語言是使用[語言代碼](http://msdn.microsoft.com/library/ms533052(vs.85).aspx)來識別，例如 **"en"** 代表英文，**"ja"** 代表日文。

3.  設定目前播放的音軌。

    當您尋找所需語言的音軌時，請將 [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358) 設定為曲目的索引。 將 **AudioStreamIndex** 設為 **null** 會選取內容所定義的預設音軌。

以下的部分程式碼會嘗試將音軌設定為指定的語言。 它會逐一查看 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 物件上的音軌，然後使用 [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384) 取得每個音軌的語言。 如果所需的語言音軌存在，就會將 [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358) 設定為該音軌的索引值。

```csharp
/// <summary>
/// Attemps to set the audio track of a video to a specific language
/// </summary>
/// <param name="lcid">The id of the language. For example, "en" or "ja"</param>
/// <returns>true if the track was set; otherwise, false.</returns>
private bool SetAudioLanguage(string lcid, MediaElement media)
{
    bool wasLanguageSet = false;

    for (int index = 0; index < media.AudioStreamCount; index++)
    {
        if (media.GetAudioStreamLanguage(index) == lcid)
        {
            media.AudioStreamIndex = index;
            wasLanguageSet = true;
        }
    }

    return wasLanguageSet;
}
```

### 啟用完整視窗視訊呈現

設定 [**IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/dn298980) 屬性來啟用和停用完整視窗呈現。 如果要在 app 中以程式設計方式設定完整視窗呈現，您應該一律使用 **IsFullWindow** 而不是手動進行。 **IsFullWindow** 可保證執行系統層級最佳化，以增進效能及電池壽命。 如果未正確設定完整視窗呈現，將不會啟用這些最佳化。

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

### 調整視訊大小和延展視訊

使用 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br227422) 屬性，來變更視訊內容填入所在容器的方式。 視 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968) 值而定，這樣做可能會延展視訊。 **Stretch** 狀態和許多電視機上的影像大小設定類似。 您可以將勾點設定在按鈕上，並允許使用者依偏好選擇所要的設定。

-   [
            **None**](https://msdn.microsoft.com/library/windows/apps/br242968) 會以原始大小顯示內容的原生解析度。
-   [
            **Uniform**](https://msdn.microsoft.com/library/windows/apps/br242968) 會盡可能地填滿空間，同時維持影像內容的外觀比例。 這會導致視訊邊緣出現水平或垂直的黑色長條。 這和寬螢幕模式類似。
-   [
            **UniformToFill**](https://msdn.microsoft.com/library/windows/apps/br242968) 會填滿個空間，同時維持外觀比例。 這會導致部分影像被裁切。 這和全螢幕模式類似。
-   [
            **Fill**](https://msdn.microsoft.com/library/windows/apps/br242968) 會填滿整個空間，但不會維持外觀比例。 影像不會被裁切，但可能發生延展現象。 這和延展模式類似。

![伸展列舉值](images/Image_Stretch.jpg) 此處的 [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) 可用來循環顯示 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968) 選項。 **switch** 陳述式會檢查 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br227422) 屬性目前的狀態，並將它設定成 **Stretch** 列舉中的下一個值。 這樣做可允許使用者循環使用不同的延展狀態。

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

### 啟用低延遲播放

在 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 上，將 [**RealTimePlayback**](https://msdn.microsoft.com/library/windows/apps/br227414) 屬性設定為 **true** 時，可讓媒體元素減少播放的初始延遲。 這對於雙向通訊的 app 而言非常重要，也很適用於部分遊戲案例。 請注意，這個模式需要更大量的資源，而且比較耗電。

這個範例會建立一個 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)，並將 [**RealTimePlayback**](https://msdn.microsoft.com/library/windows/apps/br227414) 設定成 **true**。

```xaml
<MediaElement x:Name="mediaPlayer" RealTimePlayback="True"/>
```

```csharp
MediaElement mediaPlayer = new MediaElement();
mediaPlayer.RealTimePlayback = true;
```
    
## 建議 

媒體播放器內建一個深色佈景主題與一個淺色佈景主題，但是在大部分情況下會選擇深色佈景主題。 深色背景可提供較高對比，特別是在低光源的情況下，而且可以避免控制列干擾觀賞體驗。

建議使用全螢幕模式取代內嵌模式，以獲得更好的觀賞體驗。 全螢幕檢視是最佳體驗模式，況且內嵌模式的選項有限。

如果您的螢幕空間夠大，建議使用雙列配置。 相較於精簡的單列配置，雙列配置可以提供更多空間供控制項使用。

請新增您需要的任何自訂選項到媒體播放器，來為您的 app 提供最佳體驗，但是請記住下列事項：

-   請勿大幅度自訂預設的控制項，因為這些預設控制項已經針對媒體播放器體驗最佳化。
-   在手機與其他行動裝置上，裝置色彩會維持黑色，但是在膝上型電腦與桌上型電腦上，裝置色彩會沿用使用者的佈景主題色彩。
-   盡量不要在控制列上放入太多選項。
-   請勿將媒體時間軸壓縮到比其預設大小下限更小，因為這將會嚴重限制其有效性。

## 相關文章

- [UWP app 的命令設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958433)
- [UWP app 的內容設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958434)


<!--HONumber=May16_HO2-->


