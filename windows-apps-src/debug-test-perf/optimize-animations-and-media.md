---
author: jwmsft
ms.assetid: DE5B084C-DAC1-430B-A15B-5B3D5FB698F7
title: 最佳化動畫、媒體及影像
description: 建立具有流暢的動畫、高畫面播放速率和高效能媒體擷取和播放功能的通用 Windows 平台 (UWP) 應用程式。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2eebb967a7bf11163dc2e0ba502b40495901b39b
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5515654"
---
# <a name="optimize-animations-media-and-images"></a>最佳化動畫、媒體及影像


建立具有流暢的動畫、高畫面播放速率和高效能媒體擷取和播放功能的通用 Windows 平台 (UWP) app。

## <a name="make-animations-smooth"></a>建立流暢的動畫

流暢的互動是 UWP app 最主要的特性。 這包含與手指緊密結合的觸控操作、流暢的切換效果和動畫，以及提供輸入回饋的小動作。 XAML 架構有一個執行緒 (稱為合成執行緒) 專門在應用程式視覺元素中進行合成和製作動畫。 因為合成執行緒和 UI 執行緒 (執行架構和開發人員程式碼的執行緒) 是分開的，所以無論是複雜的版面配置階段或延伸的計算，app 都可以實現一致的畫面播放速率和流暢的動畫。 本節說明如何使用合成執行緒讓 app 的動畫更加流暢。 如需動畫的詳細資訊，請參閱[動畫概觀](https://msdn.microsoft.com/library/windows/apps/Mt187350)。 若要了解如何在執行大量計算時增加 app 的回應性，請參閱[讓 UI 執行緒保持回應](keep-the-ui-thread-responsive.md)。

### <a name="use-independent-instead-of-dependent-animations"></a>使用獨立式動畫，而非相依式動畫

獨立式動畫可以在建立時從開始到結束計算，因為要製作成動畫之屬性的變更不會影響場景中的其餘物件。 因此，獨立式動畫可在合成執行緒上執行，而不是在 UI 執行緒上執行。 這可以確保他們的流暢度，因為合成執行緒會持續執行更新。

下列動畫類型一定是獨立式動畫：

-   使用主要畫面的物件動畫
-   零持續時間動畫
-   [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/Hh759771) 和 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/Hh759772) 屬性的動畫
-   [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 屬性的動畫
-   以 [**SolidColorBrush.Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 子屬性為目標時，[**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 類型屬性的動畫
-   以這些傳回值類型的子屬性為目標時，下列 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) 屬性的動畫：

    -   [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.rendertransform)
    -   [**Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d)
    -   [**Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection)
    -   [**Clip**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.clip)

相依式動畫會影響配置，因此需要有來自 UI 執行緒的額外輸入才能進行計算。 相依式動畫包含對 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 和 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 這類屬性的修改。 相依式動畫預設不會執行，且需要由 app 開發人員選擇加入。 如果 UI 執行緒保持未封鎖狀態，則相依式動畫啟用後便可以很流暢地執行，但是如果架構或 app 正在 UI 執行緒上執行大量的其他工作，就會發生斷斷續續的現象。

XAML 架構中幾乎所有的動畫皆預設為獨立式，但是您可以執行一些動作來停用這個最佳化功能。 請注意這些情況，尤其是：

-   將 [**EnableDependentAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210356) 屬性設為允許在 UI 執行緒執行相依式動畫。 然後，將這些動畫轉換為獨立式版本。 例如，讓物件的 [**ScaleTransform.ScaleX**](https://msdn.microsoft.com/library/windows/apps/BR242946) 和 [**ScaleTransform.ScaleY**](https://msdn.microsoft.com/library/windows/apps/BR242948) 產生動畫，而不是 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 和 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)。 不要害怕縮放影像和文字這類物件。 架構只會在 [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940) 產生動畫時套用雙線性縮放。 為了確保清晰度，影像/文字會在最終大小進行點陣化處理。
-   進行每個畫面的更新，這是有效的相依式動畫。 這種情況的例子是在 [**CompositonTarget.Rendering**](https://msdn.microsoft.com/library/windows/apps/BR228127) 事件處理常式套用轉換。
-   執行元素 ([**CacheMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.cachemode) 屬性設為 **BitmapCache**) 中被視為獨立式動畫的任何動畫。 這可視為相依式動畫，因為必須針對每個畫面將快取重新點陣化。

### <a name="dont-animate-a-webview-or-mediaplayerelement"></a>不要讓 WebView 或 MediaPlayerElement 產生動畫

[**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702) 控制項內的網頁內容不是由 XAML 架構直接呈現，並且需要執行額外的工作才能與其他場景合成。 在畫面上產生控制項動畫會增加這項額外的工作，而這項工作可能會帶來同步化的問題 (例如，HTML 內容可能不會與頁面上的其餘 XAML 內容同步移動)。 當您需要讓 **WebView** 控制項產生動畫時，請在動畫期間將它交換為 [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webviewbrush.aspx)。

讓 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 產生動畫同樣也不是很好的做法。 除了效能變差，還會造成播放的視訊內容產生裂紋或其他殘影。

> **注意：**  **MediaPlayerElement**本篇文章中的建議也適用於[**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)。 **MediaPlayerElement** 只能在 Windows 10 版本 1607 取得，因此如果您是針對先前版本的 Windows 建立 App，則需要使用 **MediaElement**。

### <a name="use-infinite-animations-sparingly"></a>謹慎使用無限動畫

大部分動畫會有指定的執行時間，但是將 [**Timeline.Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 設定為永久會讓動畫無限期地執行下去。 建議您儘量不要使用無限動畫，因為這些動畫會不斷耗用 CPU 資源，並且可能會讓 CPU 無法進入低電源或閒置狀態，進而導致電力快速耗盡。

新增 [**CompositionTarget.Rendering**](https://msdn.microsoft.com/library/windows/apps/BR228127) 處理常式與執行無限動畫類似。 通常 UI 執行緒只會在有工作時啟用，但是新增這個事件處理常式會強制執行緒執行每個畫面。 沒有需要完成的工作時請移除處理常式，然後於需要時再重新登錄。

### <a name="use-the-animation-library"></a>使用動畫庫

[**Windows.UI.Xaml.Media.Animation**](https://msdn.microsoft.com/library/windows/apps/BR243232) 命名空間包含高效能且順暢的動畫庫，這些動畫的外觀及操作與其他 Windows 動畫相同。 相關類別的名稱中會包含「佈景主題」，其相關說明請見[動畫概觀](https://msdn.microsoft.com/library/windows/apps/Mt187350)。 這個動畫庫支援許多常見的動畫案例，例如將應用程式的第一個檢視以動畫顯示，並建立狀態與內容轉換。 建議您儘量使用這個動畫庫來提升效能以及與 UWP UI 的一致性。

> **注意：** 的動畫庫無法讓所有可能的屬性產生動畫。 如需無法套用動畫庫的 XAML 案例，請參閱[腳本動畫](https://msdn.microsoft.com/library/windows/apps/Mt187354)。


### <a name="animate-compositetransform3d-properties-independently"></a>個別使 CompositeTransform3D 屬性產生動畫效果

您可以分別為 [**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/Dn914714) 的每個屬性產生動畫效果，如此一來就只會套用您所需的動畫。 如需範例和詳細資訊，請參閱 [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d)。 如需產生動畫效果的轉換詳細資訊，請參閱 [腳本動畫](https://msdn.microsoft.com/library/windows/apps/Mt187354)和[主要畫面格和 Easing 函式動畫](https://msdn.microsoft.com/library/windows/apps/Mt187352)。

## <a name="optimize-media-resources"></a>最佳化媒體資源

音訊、視訊及影像是大部分 app 都會使用的精彩內容格式。 隨著媒體擷取速率增加以及內容從標準畫質進步到高畫質，儲存、解碼和播放這種內容所需的資源數量亦隨之增加。 UWP 媒體引擎中已加入使用最新功能建立的 XAML 架構，因此，app 可以免費使用這些改善的功能。 本文說明的一些額外技巧可以讓您在 UWP 中有效使用媒體。

### <a name="release-media-streams"></a>釋放媒體串流

媒體檔案是應用程式經常使用的一些最常見且成本較高的資源。 因為媒體檔案資源可能會大幅增加應用程式的磁碟使用量大小，所以在應用程式使用完媒體後，您必須記得立即釋放媒體的控制代碼。

例如，如果您的 app 使用 [**RandomAccessStream**](/uwp/api/Windows.Storage.Streams.RandomAccessStream) 或 [**IInputStream**](https://msdn.microsoft.com/library/windows/apps/BR241718) 物件，請確定您的 app 完成使用時呼叫物件的 close 方法，以釋放基礎物件。

### <a name="display-full-screen-video-playback-when-possible"></a>盡可能顯示全螢幕視訊播放

在 UWP App 中，一律在 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 上使用 [**IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.isfullwindow.aspx) 屬性，以啟用和停用完整視窗轉譯。 這可確保在媒體播放時會使用系統層級最佳化。

當視訊內容是唯一要轉譯的內容時，XAML 架構可以最佳化顯示的視訊內容，因此可以使用較少的電力，並產生較高的畫面播放速率。 最有效的媒體播放，是將 **MediaPlayerElement** 物件的大小設為畫面的寬度和長度，並且不顯示其他 XAML 元素。

基於某些正當理由，您可以在佔據了螢幕全長和全寬的 **MediaPlayerElement** 上重疊 XAML 元素，例如隱藏式輔助字幕或短暫的傳輸控制項。 不需要這些元素時，請務必隱藏它們 (設定 `Visibility="Collapsed"`)，讓媒體撥放返回最有效的狀態。

### <a name="display-deactivation-and-conserving-power"></a>顯示器停用與省電

若要防止顯示器在未偵測到使用者動作時停用 (例如 app 播放視訊時)，您可以呼叫 [**DisplayRequest.RequestActive**](https://msdn.microsoft.com/library/windows/apps/BR241818)。

若要省電並延長電池續航力，您應該在不再需要顯示器要求時呼叫 [**DisplayRequest.RequestRelease**](https://msdn.microsoft.com/library/windows/apps/BR241819) 以便立即釋放顯示器要求。

以下是您應該釋放顯示器要求的一些情況：

-   視訊播放暫停時，例如因使用者動作、緩衝或因為頻寬有限而進行調整。
-   播放停止時。 例如，視訊播放完畢或簡報結束。
-   發生播放錯誤時。 例如，網路連線問題或檔案損毀。

### <a name="put-other-elements-to-the-side-of-embedded-video"></a>將其他元素放到內嵌視訊的旁邊

App 通常會提供在頁面中播放視訊的內嵌檢視。 由於 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 不是頁面大小，再加上其他繪製的 XAML 物件，您很明顯地無法使用全螢幕播放。 請注意不要在 **MediaPlayerElement** 周圍繪製框線，以避免不慎進入這個模式。

當視訊是內嵌模式時，請勿在視訊上繪製 XAML 元素。 如果您這樣做，架構會強制執行一些額外工作來建構場景。 將傳輸控制項放在內嵌媒體元素下方 (而非視訊上)，是這個狀況最佳化的最佳範例。 在這個影像中，紅色列代表一組傳輸控制項 (播放、暫停及停止等)。

![含有重疊元素的 MediaPlayerElement](images/videowithoverlay.png)

請勿在非全螢幕的媒體上方放置這些控制項。 反之，將這些傳輸控制項放在媒體轉譯區域外部的位置。 在下一個影像中，控制項放在媒體下方。

![含有相鄰元素的 MediaPlayerElement](images/videowithneighbors.png)

### <a name="delay-setting-the-source-for-a-mediaplayerelement"></a>延遲設定 MediaPlayerElement 的來源

媒體引擎是耗費大量資源的物件，XAML 架構會盡可能延遲載入 dll 和建立大型物件。 在透過 [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 屬性設定來源後，[**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 會被強制執行這個工作。 在使用者真的準備好播放媒體時這樣設定，就能盡量延遲與 **MediaElement** 關聯的大多數資源消耗。

### <a name="set-mediaplayerelementpostersource"></a>設定 MediaPlayerElement.PosterSource

設定 [**MediaPlayerElement.PosterSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.postersource.aspx) 可以讓 XAML 釋放一些原先可能會被使用的 GPU 資源。 這個 API 可以讓 App 盡可能使用最少的記憶體。

### <a name="improve-media-scrubbing"></a>改善媒體清除

為了使媒體平台可以立即回應，清除永遠是一項困難的工作。 我們通常是透過變更滑桿的值來進行這項工作。 下列是一些如何讓這項工作更有效率的提示：

-   根據會查詢 [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) 上 [**Position**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.position.aspx) 的計時器來更新 [**Slider**](https://msdn.microsoft.com/library/windows/apps/BR209614) 的值。 請務必為您的計時器使用合理的更新頻率。 在播放期間，**Position** 只會每 250 毫秒更新一次。
-   滑桿上的步階頻率大小必須隨視訊長度調整。
-   訂閱滑桿上的 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx)、[**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointermoved.aspx) 以及 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerreleased.aspx) 事件，以在使用者拖曳滑桿的指標時將 [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackrate.aspx) 屬性設定為 0。
-   在 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerreleased.aspx) 事件處理常式中，將媒體位置手動設定為滑桿位置值，以在清除時獲得最佳貼齊效果。

### <a name="match-video-resolution-to-device-resolution"></a>採用與裝置解析度相符的視訊解析度

解碼視訊會耗用相當多記憶體和 GPU 週期，因此最好選擇與顯示時所採用的解析度相近的視訊格式。 如果 1080 視訊將縮減成很小的大小，那麼使用資源將它解碼就沒有任何意義。 許多應用程式不會根據不同的解析度來編碼相同的視訊，不過如果有這項功能，請使用與顯示時所採用的解析度相近的編碼方式。

### <a name="choose-recommended-formats"></a>選擇建議的格式

媒體格式選擇有可能是一個敏感的話題，而且通常是由企業決策所驅動。 從 UWP 效能觀點來看，我們建議使用 H.264 視訊做為主要的視訊格式，以及使用 AAC 與 MP3 做為慣用的音訊格式。 就本機檔案播放而言，MP4 是視訊內容的慣用檔案容器。 H.264 解碼會透過最新的圖形硬體來加速。 此外，雖然 VC-1 解碼的硬體加速非常普及，但對於市場上許多圖形硬體而言，這個加速在許多情況下都被限制成部分加速等級 (或是 IDCT 等級)，而不是全速等級的硬體卸載 (也就是 VLD 模式)。

如果您對於視訊內容產生程序有完整的控制權，您應該思考如何在壓縮效率與 GOP 結構之間保持良好的平衡。 含 B 圖片且相對較小的 GOP 大小可在搜尋或策略模式中提升效能。

包含簡短且低延遲的音訊效果時 (例如在遊戲中)，請使用含未壓縮 PCM 資料的 WAV 檔案，以減輕壓縮音訊格式時常會發生的額外處理負荷。


## <a name="optimize-image-resources"></a>最佳化影像資源

### <a name="scale-images-to-the-appropriate-size"></a>將影像按比例調整為適當大小

如果影像以非常高的解析度擷取，可能會導致 app 解碼影像資料時使用更多 CPU，從磁碟載入影像之後使用更多記憶體。 但是如果只為了顯示比原生大小還小的影像，而將高解析度影像解碼並儲存在記憶體中，實在不太合理。 因此會改為使用 [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 屬性，以繪製在螢幕上的實際大小建立一個影像版本。

不要這樣做：

```xaml
<Image Source="ms-appx:///Assets/highresCar.jpg"
       Width="300" Height="200"/>    <!-- BAD CODE DO NOT USE.-->
```

改為這樣做：

```xaml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg"
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

[**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 的單位預設是實體像素。 [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) 屬性可以用來變更這個行為：將 **DecodePixelType** 設定為 **Logical** 會導致解碼大小自動採用系統目前的縮放比例，類似於其他的 XAML 內容。 因此，如果您希望 **DecodePixelWidth** 和 **DecodePixelHeight** 符合影像顯示所在之 Image 控制項的 Height 和 Width 屬性，將 **DecodePixelType** 設定為 **Logical** 會比較適當。 使用實體像素的預設行為，您必須自行考量系統目前的縮放比例；而且您必須接聽縮放比例變更通知，以免使用者變更其顯示喜好設定。

如果 DecodePixelWidth/Height 明確設定超過要在螢幕上顯示的影像大小，則 app 會不必要地使用額外的記憶體—每像素最多 4 個位元組—大型影像就會變得非常耗費資源。 影像也會使用雙線性縮放比例縮小，而當縮放比例變大時可能會導致影像模糊。

如果 DecodePixelWidth/DecodePixelHeight 明確設定低於要在螢幕上顯示的影像大小，則影像會被放大而變得不太美觀。

在無法預先決定適當解碼大小的某些情況下，您應該遵從 XAML 的自動以正確大小解碼，它會在未指定明確的 DecodePixelWidth/DecodePixelHeight 時，盡可能嘗試以適當大小將影像解碼。

如果您預先知道影像內容的大小，就要設定明確的解碼大小。 如果提供的解碼大小與其他 XAML 元素的大小成比例，您也應該同時將 [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) 設定為 **Logical**。 例如，如果您明確使用 Image.Width 和 Image.Height 設定內容的大小，可以將 DecodePixelType 設定為 DecodePixelType.Logical 以使用與 Image 控制項相同的邏輯像素維度，然後明確使用 BitmapImage.DecodePixelWidth 和/或 BitmapImage.DecodePixelHeight 控制影像的大小來節省大量記憶體。

請注意，決定解碼內容的大小時，應考量 Image.Stretch。

### <a name="right-sized-decoding"></a>以正確大小解碼

如果您沒有明確設定解碼大小，XAML 會根據內含頁面的初始配置，將影像解碼成要在螢幕上顯示的確切大小，盡可能嘗試節省記憶體。 建議您盡可能以這種方式撰寫您的應用程式以便使用這項功能。 如果符合下列任何一個條件，則會停用此功能。

-   使用 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) 或 [**UriSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.urisource.aspx) 設定內容之後，[**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) 連接到動態的 XAML 樹狀結構。
-   影像使用同步解碼 (例如 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255)) 來解碼。
-   影像在主機影像元素或筆刷或任何父元素上透過將 [[**不透明度**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)] 設定為 0，或將 [[**可見度**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility)] 設定為 [**收合**] 而隱藏。
-   影像控制項或筆刷使用設定為 **None** 的 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/BR242968)。
-   影像用來做為 [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756)。
-   `CacheMode="BitmapCache"` 已在影像元素或任何父元素上設定。
-   影像筆刷是非矩形 (例如套用至圖形或文字時)。

在上述案例中，設定明確的解碼大小是節省記憶體的唯一方法。

您在設定來源之前，應該一律將 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) 附加到動態樹狀結構。 每當在標記中指定影像元素或筆刷時，就自動會是這種情況。 「動態樹狀結構範例」標題之下會提供範例。 您在設定串流來源時應該一律避免使用 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255)，改為使用 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522)。 而且最好避免在等待 [**ImageOpened**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.imageopened.aspx) 事件引發時隱藏影像內容 (無論是不透明度設定為零，或是摺疊的可見性)。 這麼做只是為了判斷：如果完成，您也無法從自動以正確大小解碼獲得好處。 如果您的 app 必須一開始就隱藏影像內容，則可能的話也應該明確設定解碼大小。

**動態樹狀結構範例**

範例 1 (好)—在標記中指定的統一資源識別元 (URI)。

```xaml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

範例 2 標記—在程式碼後置中指定的 URI。

```xaml
<Image x:Name="myImage"/>
```

範例 2 程式碼後置 (好)—將 BitmapImage 連接到樹狀結構之後才設定其 UriSource。

```csharp
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

範例 2 程式碼後置 （壞） — 將 BitmapImage UriSource 設定連接到樹狀結構之前先。

```csharp
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

### <a name="caching-optimizations"></a>快取最佳化

快取最佳化適用於使用 [**UriSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.urisource.aspx) 從應用程式套件或從網路載入內容的影像。 URI 是用來唯一識別相關的內容，因此就不需要內部多次下載 XAML 架構或解碼內容。 相反地會使用快取的軟體或硬體資源來多次顯示內容。

這個最佳化的例外是如果影像以不同解析度顯示多次 (這可以明確指定或透過自動以正確大小解碼)。 每個快取項目也會儲存影像的解析度，如果 XAML 找不到有來源 URI 的影像符合需要的解析度，則會以該大小解碼新的版本， 但是不會再下載編碼的影像資料。

因此，您必須在從應用程式套件載入影像時包含使用 [**UriSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.urisource.aspx)，以及避免使用檔案串流和 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) (如果不必要)。

### <a name="images-in-virtualized-panels-listview-for-instance"></a>虛擬化面板中的影像 (例如 ListView)

如果影像從樹狀結構移除—因為 app 明確將影像移除，或者因為影像位於新式虛擬化面板中而在捲動到檢視之外時以隱含方式移除—則 XAML 會為影像釋放已經不再需要的硬體資源來最佳化記憶體使用量。 記憶體不會立即釋放，而是在影像元素從樹狀結構移除之後發生畫面更新期間時釋放。

因此，您應該盡量使用新式虛擬化面板來裝載影像內容的清單。

### <a name="software-rasterized-images"></a>軟體點陣化的影像

當影像用於非矩形的筆刷或用於 [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756) 時，影像會使用軟體點陣化路徑，完全不會縮放影像。 此外，必須在軟體和硬體記憶體中儲存一份影像。 例如，如果有一張影像用來做為橢圓形的筆刷，則內部會儲存兩次可能很大的完整影像。 使用 **NineGrid** 或非矩形的筆刷時，您的 app 應該會將影像預先縮放到接近要呈現的大小。

### <a name="background-thread-image-loading"></a>背景執行緒影像載入

XAML 具備內部最佳化的功能，允許以非同步方式將影像的內容解碼到硬體記憶體中的表層，而不需要軟體記憶體中的中繼層。 這樣可以減少記憶體的尖峰使用量與轉譯延遲。 如果符合下列任何一個條件，則會停用此功能。

-   影像用來做為 [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756)。
-   `CacheMode="BitmapCache"` 已在影像元素或任何父元素上設定。
-   影像筆刷是非矩形 (例如套用至圖形或文字時)。

### <a name="softwarebitmapsource"></a>SoftwareBitmapSource

[**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Dn997854) 類別交換不同 WinRT 命名空間 (例如 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/BR226176)、相機 API 以及 XAML) 之間的互通性未壓縮映像。 這個類別會排除通常是使用 [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/BR243259) 才需要的額外複本，以及協助減少尖峰記憶體和來源到螢幕的延遲。

提供來源資訊的 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Dn887358) 也可以設定為使用自訂 [**IWICBitmap**](https://msdn.microsoft.com/library/windows/desktop/Ee719675)，提供可重新載入的備份存放區，讓 app 需要時就可以重新對應記憶體。 這是進階的 C++ 使用案例。

您的 app 應該使用 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Dn887358) 和 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Dn997854) 與其他製作和使用影像的 WinRT API 相互溝通。 您的 app 也應該在載入未壓縮的影像資料時使用 **SoftwareBitmapSource** 而不是使用 [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/BR243259)。

### <a name="use-getthumbnailasync-for-thumbnails"></a>使用 GetThumbnailAsync 以取得縮圖

按比例調整影像的一個使用案例是建立縮圖。 雖然您可以使用 [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 與 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 來提供較小的影像版本，但 UWP 提供更有效的 API 供您擷取縮圖。 [**GetThumbnailAsync**](https://msdn.microsoft.com/library/windows/apps/BR227210) 會為已快取檔案系統的影像提供縮圖。 這樣做的效能比使用 XAML API 更好，因為不需要開啟或解碼影像。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> FileOpenPicker picker = new FileOpenPicker();
> picker.FileTypeFilter.Add(".bmp");
> picker.FileTypeFilter.Add(".jpg");
> picker.FileTypeFilter.Add(".jpeg");
> picker.FileTypeFilter.Add(".png");
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary;
>
> StorageFile file = await picker.PickSingleFileAsync();
>
> StorageItemThumbnail fileThumbnail = await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64);
>
> BitmapImage bmp = new BitmapImage();
> bmp.SetSource(fileThumbnail);
>
> Image img = new Image();
> img.Source = bmp;
> ```
> ```vb
> Dim picker As New FileOpenPicker()
> picker.FileTypeFilter.Add(".bmp")
> picker.FileTypeFilter.Add(".jpg")
> picker.FileTypeFilter.Add(".jpeg")
> picker.FileTypeFilter.Add(".png")
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary
>
> Dim file As StorageFile = Await picker.PickSingleFileAsync()
>
> Dim fileThumbnail As StorageItemThumbnail = Await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64)
>
> Dim bmp As New BitmapImage()
> bmp.SetSource(fileThumbnail)
>
> Dim img As New Image()
> img.Source = bmp
> ```

### <a name="decode-images-once"></a>解碼影像一次

為避免影像解碼超過一次，請從 URI 指派 [**Image.Source**](https://msdn.microsoft.com/library/windows/apps/BR242760) 屬性，而非使用記憶體串流。 XAML 架構可以將多個位置中的同一個 URI 與一個解碼的影像建立關聯，但無法為包含相同資料的多個記憶體串流執行相同動作，而是會為每個記憶體串流建立一個不同的解碼影像。
