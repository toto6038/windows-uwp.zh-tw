---
author: Jwmsft
Description: "了解如何將影像整合到您的應用程式，包括如何使用兩個主要 XAML 類別的 API：Image 和 ImageBrush。"
title: "影像與影像筆刷"
ms.assetid: CEA8780C-71A3-4168-A6E8-6361CDFB2FAF
label: Images and image brushes
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: 6adb5c106ccbe7d5dccda405b301daf8ce4e69f4

---
# <a name="images-and-image-brushes"></a>影像與影像筆刷

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

若要顯示影像，您可以使用 **Image** 物件或 **ImageBrush** 物件。 Image 物件會轉譯影像，而 ImageBrush 物件會以影像繪製另一個物件。 

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**Image 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx)</li>
<li>[**Source 屬性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx)</li>
<li>[**ImageBrush 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx)</li>
<li>[**ImageSource 屬性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imagesource.aspx)</li>
</ul>
</div>


## <a name="are-these-the-right-elements"></a>這些是正確的元素嗎？
使用 **Image** 元素，以在您的應用程式中顯示獨立影像。

使用 **ImageBrush**，將影像套用到另一個物件。 ImageBrush 的使用包括修飾性文字效果，或控制項或版面配置容器的背景。


## <a name="create-an-image"></a>建立影像

### <a name="image"></a>Image
這個範例示範如何使用 [**Image**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx) 物件建立影像。


```XAML
<Image Width="200" Source="licorice.jpg" />
```

以下是轉譯的 Image 物件。

![影像元素範例](images/Image_Licorice.jpg)

在這個範例中，[**Source**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx) 屬性會指定影像的顯示位置。 您可以指定絕對 URL (例如 http://contoso.com/myPicture.jpg) 或者指定相對於應用程式封裝結構的 URL，以設定 Source。 例如，我們是將 "licorice.jpg" 影像檔放在專案的根資料夾中，然後宣告將影像檔納入做為內容的專案設定。

### <a name="imagebrush"></a>ImageBrush

利用 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx) 物件，您可以使用影像來繪製一個採用 [**Brush**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx) 物件的區域。 例如，您可以使用 ImageBrush 當作 [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.ellipse.aspx) 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) 屬性值，或是 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx) 的 [**Background**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx) 屬性值。

下一個範例示範如何使用 ImageBrush 繪製 Ellipse。

```XAML
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

以下是 ImageBrush 繪製的 Ellipse。

![ImageBrush 繪製的 Ellipse。](images/Image_ImageBrush_Ellipse.jpg)

### <a name="stretch-an-image"></a>伸展影像

如果未設定 **Image** 的 [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) 或 [**Height**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) 值，則會以 **Source** 指定的影像維度顯示。 設定 **Width** 和 **Height** 會建立一個包含矩形的區域，其中顯示影像。 您可以使用 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.stretch.aspx) 屬性，指定影像填滿這個包含區域的方式。 Stretch 屬性接受以下由 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx) 列舉所定義的值：

-   **None**：影像不會伸展以填滿整個輸出區域。 使用這個 Stretch 設定時請小心：如果來源影像大於要包含的區域，影像會被裁剪，而且這不像您可以小心處理 [**Clip**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.clip.aspx) 一樣，您完全無法控制檢視區，所以結果通常無法讓人滿意。
-   **Uniform**：影像會縮放以填滿整個輸出區域。 但是，內容的長寬比保持不變。 這是預設值。
-   **UniformToFill**：這個影像會調整以完全填滿整個輸出區域，但仍保留原始的外觀比例。
-   **Fill**：影像會縮放以填滿整個輸出區域。 因為內容的高度和寬度會單獨調整，所以可能不會保留影像的原始長寬比。 也就是說，必須讓影像失真才能完全放入輸出區域中。

![伸展設定範例。](images/Image_Stretch.jpg)

### <a name="crop-an-image"></a>裁剪影像

您可以使用 [**Clip**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.clip.aspx) 屬性，從影像輸出裁剪區域。 您需將 Clip 屬性設定成 [**Geometry**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.geometry.aspx)。 目前不支援非矩形裁剪。

下一個範例示範如何使用 [**RectangleGeometry**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rectanglegeometry.aspx) 當作影像的裁剪區域。 在這個範例中，我們定義一個高度為 200 的 **Image** 物件。 **RectangleGeometry** 會定義一個顯示影像區域的矩形。 [**Rect**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rectanglegeometry.rect.aspx) 屬性設定成 "25,25,100,150"，將矩形定義為開始位置 "25,25"，寬度 100，高度 150。 只會顯示矩形區域內的部分影像。

```xaml
<Image Source="licorice.jpg" Height="200">
    <Image.Clip>
        <RectangleGeometry Rect="25,25,100,150" />
    </Image.Clip>
</Image>
```

以下是背景黑色的裁剪的影像。

![由 RectangleGeometry 裁剪的 Image 物件。](images/Image_Cropped.jpg)

### <a name="apply-an-opacity"></a>套用不透明度

您可以將 [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.opacity.aspx) 套用至影像，讓影像呈現半透明狀。 不透明值從 0.0 到 1.0，其中 1.0 是完全不透明，而 0.0 是完全透明。 這個範例示範如何將 0.5 的不透明值套用至 Image。

```xaml
<Image Height="200" Source="licorice.jpg" Opacity="0.5" />
```

以下是轉譯後的影像，它的不透明度為 0.5，而且部分不透明的地方顯示黑色背景。

![不透明度為 .5 的 Image 物件。](images/Image_Opacity.jpg)

### <a name="image-file-formats"></a>影像檔案格式

**Image** 與 **ImageBrush** 可以顯示這些影像檔案格式：

-   JPEG 格式 (JPEG)
-   可攜式網路圖形 (PNG)
-   點陣圖 (BMP)
-   圖形交換格式 (GIF)
-   標記的影像檔案格式 (TIFF)
-   JPEG XR
-   圖示 (ICO)

[**Image**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx)、[**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx) 及 [**BitmapSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.aspx) 的 API 不包括任何用來編碼和解碼媒體格式的專用方法。 所有編碼及解碼作業都是內建作業，最多只會將編碼或解碼的各個層面呈現為載入事件的部分事件資料。 如果要利用影像編碼或解碼來執行任何特殊工作 (如果應用程式正在執行影像轉換或操作，您就有可能這樣做)，則應該使用 [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.graphics.imaging.aspx) 命名空間提供的 API。 Windows 的 Windows 影像處理元件 (WIC) 也支援這些 API。

從 Windows 10 版本 1607 開始，**Image** 元素支援動畫 GIF 影像。 當您使用 **BitmapImage** 做為影像的 **Source** 時，您可以存取 BitmapImage API 來控制動畫 GIF 影像的播放。 如需詳細資訊，請參閱 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx) 類別頁面的＜備註＞。

> **注意**&nbsp;&nbsp;當您的應用程式是針對 Windows 10 版本 1607 進行編譯，並在版本 1607 (或更新版本) 上執行時，便能獲得動畫 GIF 支援。 當您的應用程式是針對較舊版本進行編譯並在其上執行時，系統會顯示 GIF 的第一個畫面，但不會產生動畫效果。

如需應用程式資源以及如何封裝應用程式影像來源的詳細資訊，請參閱[定義應用程式資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321)。

### <a name="writeablebitmap"></a>WriteableBitmap

[**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.aspx) 會提供可修改且不會使用來自 WIC 的基本檔案型解碼的 [**BitmapSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.aspx)。 您可以動態更改影像以及重新轉譯更新後的影像。 若要定義 **WriteableBitmap** 的緩衝內容，可使用 [**PixelBuffer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.pixelbuffer.aspx) 屬性存取緩衝，然後使用資料流或語言特定緩衝類型填滿。 如需範例程式碼，請參閱 [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.aspx)。

### <a name="rendertargetbitmap"></a>RenderTargetBitmap

[**RenderTargetBitmap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.rendertargetbitmap.aspx) 類別可以從執行中的應用程式擷取 XAML UI 樹狀目錄，然後呈現點陣圖影像來源。 擷取後，該影像來源可以套用到應用程式的其他部分、由使用者儲存為資源或應用程式資料，或用於其他案例。 其中一個特別有用的案例就是建立瀏覽配置的 XAML 頁面執行階段縮圖，例如，從 [**Hub**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.hub.aspx) 控制項提供影像連結。 **RenderTargetBitmap** 對於顯示在擷取影像的內容有一些限制。 如需詳細資訊，請參閱 [**RenderTargetBitmap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.rendertargetbitmap.aspx) 的 API 參考資料主題。

### <a name="image-sources-and-scaling"></a>影像來源和縮放

您應該以建議的數種大小來建立影像來源，以確保當 Windows 縮放您的應用程式時，仍能保持很棒的外觀。 為 **Image** 指定 **Source** 時，您可以使用命名慣例來根據目前的縮放情形，自動參考正確的資源。 如需命名慣例的細節及其他資訊，請參閱[快速入門：使用檔案或影像資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965325)。

如需如何針對縮放進行設計的詳細資訊，請參閱[版面配置和縮放的 UX 指導方針](https://msdn.microsoft.com/library/windows/apps/dn611863)。

### <a name="image-and-imagebrush-in-code"></a>程式碼中的 Image 和 ImageBrush

通常都會使用 XAML 指定 Image 和 ImageBrush 元素，而不是程式碼。 這是因為這些元素通常是設計工具的輸出，而且是 XAML UI 定義的一部分。

如果使用程式碼定義 Image 或 ImageBrush，請使用預設建構函式，然後設定相關來源屬性 ([**Image.Source**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx) 或 [**ImageBrush.ImageSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imagesource.aspx))。 當您使用程式碼設定來源屬性時，來源屬性需要一個 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx) (不是 URI)。 如果您的來源是資料流，請使用 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync.aspx) 方法來初始化該值。 如果您的來源是 URI，包含應用程式中使用 **ms-appx** 或 **ms-resource** 配置的內容，則使用採用 URI 的 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/xaml/br243238.aspx) 建構函式。 如果有任何與影像來源的擷取或解碼相關的時機問題，您也可以考慮處理 [**ImageOpened**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.imageopened.aspx) 事件，在這種情況下，您可能需要在影像來源可供使用前先顯示替代內容。 如需範例程式碼，請參閱 [XAML 影像範例](http://go.microsoft.com/fwlink/p/?linkid=238575) (英文)。

> [!NOTE]
> 如果您使用程式碼建立影像，可以使用自動處理，以目前的比例和文化限定詞存取不合格的資源，或是使用 [**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcemanager.aspx) 和 [**ResourceMap**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcemap.aspx) 搭配文化和比例限定詞來直接取得資源。 如需詳細資訊，請參閱[資源管理系統](https://msdn.microsoft.com/library/windows/apps/xaml/jj552947.aspx)。

## <a name="related-articles"></a>相關文章

-   [音訊、視訊和相機](https://msdn.microsoft.com/windows/uwp/audio-video-camera/index)
-   [**Image 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx)
-   [**ImageBrush 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx)


<!--HONumber=Dec16_HO1-->


