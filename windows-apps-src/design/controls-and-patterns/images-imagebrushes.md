---
Description: 了解如何將影像整合到您的應用程式，包括如何使用兩個主要 XAML 類別的 API：Image 和 ImageBrush。
title: 影像與影像筆刷
ms.assetid: CEA8780C-71A3-4168-A6E8-6361CDFB2FAF
label: Images and image brushes
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 59678dc5eca7dec0857cadd9249dd19e25b3430b
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319050"
---
# <a name="images-and-image-brushes"></a>影像與影像筆刷

若要顯示影像，您可以使用 **Image** 物件或 **ImageBrush** 物件。 Image 物件會轉譯影像，而 ImageBrush 物件會以影像繪製另一個物件。 

> **重要 API**：[Image 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)[Source 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source)[ImageBrush 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)[ImageSource 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource)

## <a name="are-these-the-right-elements"></a>這些是正確的元素嗎？
使用 **Image** 元素，以在您的應用程式中顯示獨立影像。

使用 **ImageBrush**，將影像套用到另一個物件。 ImageBrush 的使用包括修飾性文字效果，或控制項或版面配置容器的背景。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/Image">開啟應用程式，並查看 Image 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-an-image"></a>建立影像

### <a name="image"></a>Image
這個範例示範如何使用 [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 物件建立影像。


```XAML
<Image Width="200" Source="sunset.jpg" />
```

以下是轉譯的 Image 物件。

![影像元素範例](images/Image_Licorice.jpg)

在這個範例中，[Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) 屬性會指定影像的顯示位置。 您可以指定絕對 URL (例如 http://contoso.com/myPicture.jpg) 或指定相對於應用程式封裝結構的 URL，以設定 Source。 例如，我們是將 "licorice.jpg" 影像檔放在專案的根資料夾中，然後宣告將影像檔納入做為內容的專案設定。

### <a name="imagebrush"></a>ImageBrush

利用 [ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) 物件，您可以使用影像來繪製一個採用 [Brush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Brush) 物件的區域。 例如，您可以使用 ImageBrush 當作 [Ellipse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 的 [Fill](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.fill) 屬性值，或是 [Canvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) 的 [Background](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.background) 屬性值。

下一個範例示範如何使用 ImageBrush 繪製 Ellipse。

```XAML
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="sunset.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

以下是 ImageBrush 繪製的 Ellipse。

![ImageBrush 繪製的 Ellipse。](images/Image_ImageBrush_Ellipse.jpg)

### <a name="stretch-an-image"></a>伸展影像

如果未設定 **Image** 的 [Width](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) 或 [Height](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) 值，則會以 **Source** 指定的影像維度顯示。 設定 **Width** 和 **Height** 會建立一個包含矩形的區域，其中顯示影像。 您可以使用 [Stretch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.stretch) 屬性，指定影像填滿這個包含區域的方式。 Stretch 屬性接受以下由 [Stretch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) 列舉所定義的值：

-   **無**：影像不會伸展以填滿整個輸出尺寸。 使用這個 Stretch 設定時請小心：如果來源影像大於要包含的區域，影像會被裁剪，而且這不像您可以小心處理 [Clip](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip) 一樣，您完全無法控制檢視區，所以結果通常無法讓人滿意。
-   **Uniform**：影像縮放到適合輸出尺寸。 但是，內容的長寬比保持不變。 這是預設值。
-   **UniformToFill**：縮放影像以完全填滿整個輸出區域，但仍保留原始的外觀比例。
-   **Fill**：影像縮放到適合輸出尺寸。 因為內容的高度和寬度會單獨調整，所以可能不會保留影像的原始長寬比。 也就是說，必須讓影像失真才能完全放入輸出區域中。

![伸展設定範例。](images/Image_Stretch.jpg)

### <a name="crop-an-image"></a>裁剪影像

您可以使用 [Clip](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip) 屬性，從影像輸出裁剪區域。 您需將 Clip 屬性設定成 [Geometry](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Geometry)。 目前不支援非矩形裁剪。

下一個範例示範如何使用 [RectangleGeometry](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RectangleGeometry) 當作影像的裁剪區域。 在這個範例中，我們定義一個高度為 200 的 **Image** 物件。 **RectangleGeometry** 會定義一個顯示影像區域的矩形。 [Rect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rectanglegeometry.rect) 屬性設定成 "25,25,100,150"，將矩形定義為開始位置 "25,25"，寬度 100，高度 150。 只會顯示矩形區域內的部分影像。

```xaml
<Image Source="sunset.jpg" Height="200">
    <Image.Clip>
        <RectangleGeometry Rect="25,25,100,150" />
    </Image.Clip>
</Image>
```

以下是背景黑色的裁剪的影像。

![由 RectangleGeometry 裁剪的 Image 物件。](images/Image_Cropped.jpg)

### <a name="apply-an-opacity"></a>套用不透明度

您可以將 [Opacity](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.opacity) 套用至影像，讓影像呈現半透明狀。 不透明值從 0.0 到 1.0，其中 1.0 是完全不透明，而 0.0 是完全透明。 這個範例示範如何將 0.5 的不透明值套用至 Image。

```xaml
<Image Height="200" Source="sunset.jpg" Opacity="0.5" />
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

[Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)、[BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) 及 [BitmapSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapSource) 的 API 不包括任何用來編碼和解碼媒體格式的專用方法。 所有編碼及解碼作業都是內建作業，最多只會將編碼或解碼的各個層面呈現為載入事件的部分事件資料。 如果要利用影像編碼或解碼來執行任何特殊工作 (如果 app 正在執行影像轉換或操作，您就有可能這樣做)，則應該使用 [Windows.Graphics.Imaging](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging) 命名空間提供的 API。 Windows 的 Windows 影像處理元件 (WIC) 也支援這些 API。

從 Windows 10 版本 1607 開始，**Image** 元素支援動畫 GIF 影像。 當您使用 **BitmapImage** 做為影像的 **Source** 時，您可以存取 BitmapImage API 來控制動畫 GIF 影像的播放。 如需詳細資訊，請參閱 [BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) 類別頁面的＜備註＞。

> **注意**&nbsp;&nbsp;當您的應用程式是針對 Windows 10 版本 1607 進行編譯，並在版本 1607 (或更新版本) 上執行時，即可支援動畫 GIF。 當您的應用程式是針對較舊版本進行編譯並在其上執行時，系統會顯示 GIF 的第一個畫面，但不會產生動畫效果。

如需應用程式資源以及如何封裝應用程式影像來源的詳細資訊，請參閱[定義應用程式資源](https://docs.microsoft.com/previous-versions/windows/apps/hh965321(v=win.10))。

### <a name="writeablebitmap"></a>WriteableBitmap

[WriteableBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap) 會提供可修改且不會使用來自 WIC 的基本檔案型解碼的 [BitmapSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapSource)。 您可以動態更改影像以及重新轉譯更新後的影像。 若要定義 **WriteableBitmap** 的緩衝內容，可使用 [PixelBuffer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.writeablebitmap.pixelbuffer) 屬性存取緩衝，然後使用資料流或特定語言緩衝類型填滿。 如需範例程式碼，請參閱 [WriteableBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap)。

### <a name="rendertargetbitmap"></a>RenderTargetBitmap

[RenderTargetBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.RenderTargetBitmap) 類別可以從執行中的 app 擷取 XAML UI 樹狀目錄，然後呈現點陣圖影像來源。 擷取後，該影像來源可以套用到應用程式的其他部分、由使用者儲存為資源或應用程式資料，或用於其他案例。 其中一個特別有用的案例就是建立瀏覽配置的 XAML 頁面執行階段縮圖，例如，從 [Hub](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) 控制項提供影像連結。 **RenderTargetBitmap** 對於顯示在擷取影像的內容有一些限制。 如需詳細資訊，請參閱 [RenderTargetBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.RenderTargetBitmap) 的 API 參考資料主題。

### <a name="image-sources-and-scaling"></a>影像來源和縮放

您應該以建議的數種大小來建立影像來源，以確保當 Windows 縮放您的應用程式時，仍能保持很棒的外觀。 為 **Image** 指定 **Source** 時，您可以使用命名慣例來根據目前的縮放情形，自動參考正確的資源。 如需命名慣例的細節及其他資訊，請參閱[快速入門：使用檔案或影像資源](https://docs.microsoft.com/previous-versions/windows/apps/hh965325(v=win.10))。

如需如何針對縮放進行設計的詳細資訊，請參閱[版面配置和縮放的 UX 指導方針](https://developer.microsoft.com/windows/apps/design)。

### <a name="image-and-imagebrush-in-code"></a>程式碼中的 Image 和 ImageBrush

通常都會使用 XAML 指定 Image 和 ImageBrush 元素，而不是程式碼。 這是因為這些元素通常是設計工具的輸出，而且是 XAML UI 定義的一部分。

如果使用程式碼定義 Image 或 ImageBrush，請使用預設建構函式，然後設定相關來源屬性 ([Image.Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) 或 [ImageBrush.ImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource))。 當您使用程式碼設定來源屬性時，來源屬性需要一個 [BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) (不是 URI)。 如果您的來源是資料流，請使用 [SetSourceAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) 方法來初始化該值。 如果您的來源是 URI，包含應用程式中使用 **ms-appx** 或 **ms-resource** 配置的內容，配置的內容，則使用採用 URI 的 [BitmapImage](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage) 建構函式。 如果有任何與影像來源的抓取或解碼相關的時機問題，您也可以考慮處理 [ImageOpened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.imageopened) 事件，在這種情況下，您可能需要在影像來源可供使用前先顯示替代內容。 如需範例程式碼，請參閱 [XAML 影像範例](https://go.microsoft.com/fwlink/p/?linkid=238575)。

> [!NOTE]
> 如果您使用程式碼建立影像，可以使用自動處理，以目前的比例和文化限定詞存取不完整的資源，或使用 [ResourceManager](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) 和 [ResourceMap](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap) 搭配文化和比例的限定詞，以直接取得資源。 如需詳細資訊，請參閱[資源管理系統](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

-   [音訊、視訊和相機](https://docs.microsoft.com/windows/uwp/audio-video-camera/index)
-   [Image 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)
-   [ImageBrush 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)