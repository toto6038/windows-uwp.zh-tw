---
author: Jwmsft
Description: The media player has customizable XAML transport controls to manage control of audio and video content.
title: 建立自訂媒體傳輸控制項
ms.assetid: 6643A108-A6EB-42BC-B800-22EABD7B731B
label: Create custom media transport controls
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0a8c909a64ec0f9a92eeea91761946742266dcfc
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
ms.locfileid: "1395757"
---
# <a name="create-custom-transport-controls"></a>建立自訂傳輸控制項



MediaPlayerElement 具有可自訂的 XAML 傳輸控制項，以管理通用 Windows 平台 (UWP) app 內的音訊和視訊內容的控制項。 在這裡，我們將示範如何自訂 MediaTransportControls 範本。 我們將說明如何使用溢位功能表、新增自訂按鈕，以及修改滑桿。

> **重要 API**：[MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)、[MediaPlayerElement.AreTransportControlsEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aretransportcontrolsenabled.aspx)、[MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677)

開始之前，您應該先熟悉 MediaPlayerElement 和 MediaTransportControls 類別。 如需詳細資訊，請參閱＜MediaPlayerElement 控制項指南＞。

> [!TIP]
> 本主題中的範例是以[媒體傳輸控制項範例](http://go.microsoft.com/fwlink/p/?LinkId=620023)為基礎。 您可以下載範例來檢視和執行完整的程式碼。

> [!NOTE]
> **MediaPlayerElement** 只能在 Windows 10 版本 1607 及以上的版本中取得。 如果您是針對舊版 Windows 10 開發 app，便必須改為使用 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)。 此頁面上的所有範例也適用於 **MediaElement**。

## <a name="when-should-you-customize-the-template"></a>您何時應該自訂範本？

**MediaPlayerElement** 具有內建的傳輸控制項，這些控制項已設計成在大多數視訊和音訊播放 app 中不需修改即可正常運作。 這些控制項是由 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) 類別所提供，並且包含可播放、停止及瀏覽媒體、調整音量、切換全螢幕、轉換到另一個裝置、啟用輔助字幕、切換曲目以及調整播放速率的按鈕。 MediaTransportControls 含有可讓您控制每個按鈕是否要顯示與啟用的屬性。 您也可以設定 [**IsCompact**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.iscompact.aspx) 屬性，來指定控制項是否顯示於一列或兩列中。

不過，可能有一些案例需要您進一步自訂控制項的外觀或變更其行為。 以下是一些範例：
- 變更圖示、滑桿行為及色彩。
- 將較不常用的命令按鈕移到溢位功能表。
- 在調整控制項大小時變更命令移出的順序。
- 提供不在預設集中的命令按鈕。

> [!NOTE]
> 如果畫面上沒有足夠的空間，螢幕上顯示的按鈕將會依照預先定義的順序刪除內建的傳輸控制項。 若要變更這個順序或將不符合的命令放入溢位功能表，您將需要自訂控制項。

您可以藉由修改預設範本來自訂控制項的外觀。 若要修改控制項的行為或增加新命令，您可以建立衍生自 MediaTransportControls 的自訂控制項。

> [!TIP]
> 可自訂的控制項範本是 XAML 平台的一個強大功能，但也有您應該納入考慮的後果。 當您自訂範本時，它會變成 App 的靜態部分，因此不會收到 Microsoft 對範本所做的任何平台更新。 如果是由 Microsoft 進行範本更新，您應該採用新範本並重新加以修改，以獲得更新範本的好處。

## <a name="template-structure"></a>範本結構

[**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.controltemplate.aspx) 是預設樣式的一部分。 傳輸控制項的預設樣式如 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) 類別參考頁中所示。 您可以將這個預設樣式複製到您的專案以進行修改。 ControlTemplate 可分割為類似其他 XAML 控制項範本的區段。
- 範本的第一個區段包含適用於 MediaTransportControls 的各種元件的 [**Style**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx) 定義。
- 第二個區段定義 MediaTransportControls 所使用的各種視覺狀態。
- 第三個區段包含 [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)，其可一併容納各種 MediaTransportControls 元素，並定義元件的版面配置方式。

> [!NOTE]
> 如需修改範本的詳細資訊，請參閱[控制項範本]()。 您可以使用文字編輯器或 IDE 中的類似編輯器，來開啟 \(*Program Files*)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\\(*SDK version*)\Generic 中的 XAML 檔案。 每個控制項的預設樣式與範本都是在 **generic.xaml** 檔案中定義。 您可以在 generic.xaml 中搜尋 "MediaTransportControls"，以尋找 MediaTransportControls 範本。

在下列各節中，您將了解如何為傳輸控制項自訂數個主要元素：
- [**Slider**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx)：允許使用者拖曳他們的媒體，同時顯示進度
- [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx)：包含所有按鈕。
如需詳細資訊，請參閱 MediaTransportControls 參考主題的＜剖析＞一節。

## <a name="customize-the-transport-controls"></a>自訂傳輸控制項

如果您只想要修改 MediaTransportControls 的外觀，可以建立預設控制項樣式和範本的複本，並修改該複本。 不過，如果您也想要新增或修改控制項的功能，就需要建立衍生自 MediaTransportControls 的新類別。

### <a name="re-template-the-control"></a>重新設定控制項範本

**自訂 MediaTransportControls 預設樣式與範本**
1. 將預設樣式從 MediaTransportControls 樣式與範本複製到您專案中的 ResourceDictionary。
2. 為 Style 提供 x:Key 值來識別它，如下。

```xaml
<Style TargetType="MediaTransportControls" x:Key="myTransportControlsStyle">
    <!-- Style content ... -->
</Style>
```

3. 將含有 MediaTransportControls 的 MediaPlayerElement 新增到您的 UI。
4. 將 MediaTransportControls 元素的 Style 屬性設定為自訂的 Style 資源，如下所示。

```xaml
<MediaPlayerElement AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls Style="{StaticResource myTransportControlsStyle}"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

如需修改樣式與範本的詳細資訊，請參閱[設定控制項的樣式]()和[控制項範本]()。

### <a name="create-a-derived-control"></a>建立衍生的控制項

若要新增或修改傳輸控制項的功能，您必須建立衍生自 MediaTransportControls 的新類別。 名為 `CustomMediaTransportControls` 的衍生類別，會顯示於[媒體傳輸控制項範例](http://go.microsoft.com/fwlink/p/?LinkId=620023)和本頁的其餘範例中。

**建立衍生自 MediaTransportControls 的新類別**
1. 將新的類別檔案新增到您的專案。
    - 在 Visual Studio 中，選取 [專案] &gt; [加入類別]。 隨即會開啟 [加入新項目] 對話方塊。
    - 在 [加入新項目] 對話方塊中，輸入類別檔案的名稱，然後按一下 [加入]。 (在＜媒體傳輸控制項範例＞中，類別名為 `CustomMediaTransportControls`)。
2. 修改類別程式碼，以便衍生自 MediaTransportControls 類別。

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
}
```

3. 將 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) 的預設樣式複製到您專案中的 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.resourcedictionary.aspx)。 這是您修改的樣式與範本。
(在＜媒體傳輸控制項範本＞中，會建立名為 "Themes" 的新資料夾，並在其中加入名為 generic.xaml 的 ResourceDictionary 檔案)。
4. 將樣式的 [**TargetType**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.targettype.aspx) 變更為新的自訂控制項類型。 (在範例中，TargetType 會變更為 `local:CustomMediaTransportControls`)。

```xaml
xmlns:local="using:CustomMediaTransportControls">
...
<Style TargetType="local:CustomMediaTransportControls">
```

5. 設定自訂類別的 [**DefaultStyleKey**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.defaultstylekey.aspx)。 這會通知您的自訂類別，將 Style 與 `local:CustomMediaTransportControls` 的 TargetType 搭配使用。

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }
}
```

6. 將 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 新增到 XAML 標記，並將自訂傳輸控制項新增到其中。 有一點需要注意的是，用於隱藏、顯示、停用和啟用預設按鈕的 API 仍會使用自訂的範本。

```xaml
<MediaPlayerElement Name="MediaPlayerElement1" AreTransportControlsEnabled="True" Source="video.mp4">
    <MediaPlayerElement.TransportControls>
        <local:CustomMediaTransportControls x:Name="customMTC"
                                            IsFastForwardButtonVisible="True"
                                            IsFastForwardEnabled="True"
                                            IsFastRewindButtonVisible="True"
                                            IsFastRewindEnabled="True"
                                            IsPlaybackRateButtonVisible="True"
                                            IsPlaybackRateEnabled="True"
                                            IsCompact="False">
        </local:CustomMediaTransportControls>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

您現在可以修改控制項樣式和範本來更新自訂控制項的外觀，以及修改控制項程式碼來更新它的行為。

### <a name="working-with-the-overflow-menu"></a>使用溢位功能表

您可以將 MediaTransportControls 命令按鈕移到溢位功能表，如此一來，即會隱藏較不常使用的命令，直到使用者需要它們為止。

在 MediaTransportControls 範本中，命令按鈕會包含於 [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx) 元素中。 命令列具有主要和次要命令的概念。 主要命令是預設會出現在控制項中且一定看得到 (除非您停用按鈕、隱藏該按鈕，或空間不足) 的按鈕。 次要命令會顯示於使用者按一下省略號 (...) 按鈕時出現的溢位功能表中。 如需詳細資訊，請參閱[應用程式列和命令列](app-bars.md)文章。

若要將某個元素從命令列主要命令移到溢位功能表，您需要編輯 XAML 控制項範本。

**將命令移至溢位功能表：**
1. 在控制項範本中，尋找名為 `MediaControlsCommandBar` 的 CommandBar 元素。
2. 將 [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) 區段新增到 CommandBar 的 XAML 中。 將其放置於 [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx) 的結尾標記後面。

```xaml
<CommandBar x:Name="MediaControlsCommandBar" ... >  
  <CommandBar.PrimaryCommands>
...
    <AppBarButton x:Name='PlaybackRateButton'
                    Style='{StaticResource AppBarButtonStyle}'
                    MediaTransportControlsHelper.DropoutOrder='4'
                    Visibility='Collapsed'>
      <AppBarButton.Icon>
        <FontIcon Glyph="&#xEC57;"/>
      </AppBarButton.Icon>
    </AppBarButton>
...
  </CommandBar.PrimaryCommands>
<!-- Add secondary commands (overflow menu) here -->
  <CommandBar.SecondaryCommands>
    ...
  </CommandBar.SecondaryCommands>
</CommandBar>
```

3. 若要將命令填入此功能表，請從 PrimaryCommands 剪下所需 [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx) 物件的 XAML，並將其貼到 SecondaryCommands 中即可。 在這個範例中，我們會將 `PlaybackRateButton` 移到溢位功能表。

4. 為按鈕新增標籤並移除樣式資訊，如下所示。
因為溢位功能表是由文字按鈕所組成，所以您必須為按鈕新增文字標籤，同時移除設定按鈕寬度與高度的樣式。 否則，它將無法正確顯示於溢位功能表中。

```xaml
<CommandBar.SecondaryCommands>
    <AppBarButton x:Name='PlaybackRateButton'
                  Label='Playback Rate'>
    </AppBarButton>
</CommandBar.SecondaryCommands>
```

> [!IMPORTANT]
> 您仍然必須讓按鈕顯示並加以啟用，才能在溢位功能表中予以使用。 在這個範例中，除非 IsPlaybackRateButtonVisible 屬性是 true，否則 PlaybackRateButton 元素不會顯示於溢位功能表中。 除非 IsPlaybackRateEnabled 屬性是 true，否則不會加以啟用。 設定這些屬性的方式，請見上一節。

### <a name="adding-a-custom-button"></a>新增自訂按鈕

您可能想要自訂 MediaTransportControls 的其中一個理由是將自訂命令新增到控制項。 不論您將它新增為主要命令或次要命令，建立命令按鈕和修改其行為的程序都一樣。 在[媒體傳輸控制項範例](http://go.microsoft.com/fwlink/p/?LinkId=620023)中，會將 [評等] 按鈕新增到主要命令。

**新增自訂命令按鈕**
1. 建立 AppBarButton 物件，並將它新增到控制項範本中的 CommandBar。

```xaml
<AppBarButton x:Name="LikeButton"
              Icon="Like"
              Style="{StaticResource AppBarButtonStyle}"
              MediaTransportControlsHelper.DropoutOrder="3"
              VerticalAlignment="Center" />
```

    You must add it to the CommandBar in the appropriate location. (For more info, see the Working with the overflow menu section.) How it's positioned in the UI is determined by where the button is in the markup. For example, if you want this button to appear as the last element in the primary commands, add it at the very end of the primary commands list.

    You can also customize the icon for the button. For more info, see the [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx) reference.

2. 在 [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.onapplytemplate.aspx) 覆寫中，從範本中取得按鈕，並註冊適用於其 [**Click**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件的處理常式。 這個程式碼位於 `CustomMediaTransportControls` 類別中。

```csharp
public sealed class CustomMediaTransportControls :  MediaTransportControls
{
    // ...

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    //...
}
```

3. 將程式碼新增到 Click 事件處理常式，以執行按一下按鈕時所發生的動作。
這裡提供類別的完整程式碼。

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public event EventHandler< EventArgs> Liked;

    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    private void LikeButton_Click(object sender, RoutedEventArgs e)
    {
        // Raise an event on the custom control when 'like' is clicked.
        var handler = Liked;
        if (handler != null)
        {
            handler(this, EventArgs.Empty);
        }
    }
}
```

**新增「讚」按鈕的自訂媒體傳輸控制項**
![多了讚按鈕的自訂媒體傳輸控制項](images/controls/mtc_double_custom_inprod.png)

### <a name="modifying-the-slider"></a>修改滑桿

MediaTransportControls 的 "seek" 控制項是由 [**Slider**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx) 元素所提供。 您可以自訂它的其中一種方法是變更搜尋行為的細微性。

預設搜尋滑桿會分割成 100 個部分，因此，搜尋行為會以該區段個數為限。 您可以變更搜尋滑桿的細微性，方法是從 [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.mediaopened.aspx) 上 [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 事件處理常式的 XAML 視覺化樹狀結構中取得 Slider。 這個範例示範如何使用 [**VisualTreeHelper**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.visualtreehelper.aspx) 來取得 Slider 的參考，如果媒體超過 120 分鐘，則將滑桿的預設分段從 1% 變更為 0.1% (1000 段)。 MediaPlayerElement 的名稱為 `MediaPlayerElement1`。

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
  MediaPlayerElement1.MediaPlayer.MediaOpened += MediaPlayerElement_MediaPlayer_MediaOpened;
  base.OnNavigatedTo(e);
}

private void MediaPlayerElement_MediaPlayer_MediaOpened(object sender, RoutedEventArgs e)
{
  FrameworkElement transportControlsTemplateRoot = (FrameworkElement)VisualTreeHelper.GetChild(MediaPlayerElement1.TransportControls, 0);
  Slider sliderControl = (Slider)transportControlsTemplateRoot.FindName("ProgressSlider");
  if (sliderControl != null && MediaPlayerElement1.NaturalDuration.TimeSpan.TotalMinutes > 120)
  {
    // Default is 1%. Change to 0.1% for more granular seeking.
    sliderControl.StepFrequency = 0.1;
  }
}
```
## <a name="related-articles"></a>相關文章

- [媒體播放](media-playback.md)