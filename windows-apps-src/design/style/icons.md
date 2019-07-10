---
Description: 理想的圖示達到印刷格式與其餘設計語言的平衡。 它們不會混合使用隱喻，而且只會盡可能快速並簡單地溝通所需的內容。
title: 圖示
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5e464251200812e79474d05d9d0a680b49167871
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "64564543"
---
# <a name="icons-for-uwp-apps"></a>適用於 UWP 應用程式的圖示

![圖示標頭影像](images/icons/header-icons.png)

圖示提供了視覺化的簡略表達方式，適用於動作、概念或產品。 圖示會將其代表的意義濃縮為一個符號性的圖像，而能跨越語言障礙，有助於節省螢幕空間這個無比珍貴的資源。 

圖示可在應用程式中顯示，甚至在應用程式以外顯示： 

:::row:::
    :::column:::
        **Icons inside the app**

        ![icons inside the app](images/icons/inside-icons.png)
在您的應用程式中，可以使用圖示來表示動作，例如複製文字，或瀏覽至設定頁面。
    :::column-end:::
    :::column:::
**應用程式外的圖示**

        ![icons outside the app](images/icons/outside-icons.jpg)
在應用程式之外，Windows 在 [開始] 功能表和工作列中，會以圖示來代表您的應用程式。 如果使用者選擇將應用程式釘選到 [開始] 功能表，應用程式的啟動磚可能出現應用程式的圖示。 應用程式的圖示會出現在標題列，您可以選擇建立含有應用程式標誌的啟動顯示畫面。
    :::column-end:::
:::row-end:::

本文章會介紹您應用程式中的圖示。 若要深入了解應用程式以外的圖示 (應用程式圖示)，請參閱[應用程式圖示及標誌](/windows/uwp/design/shell/tiles-and-notifications/app-assets)一文。

## <a name="when-to-use-icons"></a>使用圖示的時機

圖示可以節省空間，但該在什麼情況下使用？ 

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

像是剪下、複製、貼上和儲存之類的動作，或是導覽功能表的導覽項目，都可以使用圖示。
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

如果您想要表達的概念已經有圖示存在，請直接使用。 (若要查看圖示是否存在，請查看 Segoe 圖示清單)。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

如果有個圖示的意義很容易就能讓使用者了解，而且小尺寸也清晰易讀，就可使用該圖示。
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

如果圖示的意義不明確，或是需要複雜的圖形才能清晰顯示圖示，則請勿使用。
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>使用正確的圖示類型

有許多方式可建立圖示。 您可以使用像是 Segoe MDL2 Assets 的符號字型。 您可以建立自己的向量影像。 甚至可以使用點陣圖影像，雖然我們並不推薦。 以下會簡要說明幾個將圖示新增到應用程式內的方式。 

### <a name="use-a-predefined-icon"></a>使用預先定義的圖示。
:::row:::
    :::column:::
Microsoft 以 Segoe MDL2 Assets 字型的形式，提供了超過 1000 個圖示。 用字型取得圖示，可能並不是很直覺的方式。但透過我們的字型顯示技術，這些圖示在任何解析度、任何尺寸的顯示器上，都相當清晰銳利。 如需說明，請參閱 [Segoe MDL2 圖示](segoe-ui-symbol-font.md)。
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>使用字型。
:::row:::
    :::column:::
您不需要使用 Segoe MDL2 Assets 字型，您可以使用任何使用者系統上安裝的字型，例如 Wingdings 或 Webdings。
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>使用可縮放向量圖形 (SVG) 檔案。
:::row:::
    :::column:::
SVG 資源適用於圖示，因為任何大小或解析度上看起來都很銳利。 大部分的繪圖應用程式可以匯出為 SVG。 如需說明，請參閱 [SVGImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource)。
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>使用幾何物件。
:::row:::
    :::column:::
就像 SVG 檔案，幾何是向量資源，因此看起來總是很銳利。 不過，因為需要個別指定每個點和曲線，建立幾何物件並不容易。 只有當您需要修改圖示，而您的應用程式正在執行 (例如動畫) 時，才真的是個不錯的選擇。 如需說明，請參閱[移動與繪製命令語法](../../xaml-platform/move-draw-commands-syntax.md)。 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>您也可以使用點陣圖影像，例如 PNG、GIF 或 JPEG，雖然我們並不推薦。
:::row:::
    :::column:::
點陣圖影像是以特定大小建立而成，因此需視圖示需要的大小和螢幕的解析度，來縮放影像的大小。 影像縮小時會變得模糊，放大則會出現塊狀並像素化。 如果一定要使用點陣圖影像，建議使用 PNG 或 JPEG，而非 GIF。 
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![Bitmap image](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>讓圖示發揮功能

一旦您有一個圖示，下一個步驟便是讓它執行一些與命令或瀏覽動作相關的項目。 執行此操作的最佳方式是將圖示新增至按鈕或命令列。 

![命令列影像](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>建立圖示按鈕

您可以在標準按鈕中放置圖示。 因為可以使用按鈕的位置相當廣泛，所以動作圖示出現的位置更有彈性。 

有幾種方法可新增按鈕圖示：

:::row:::
    :::column span="2":::
        <b>Step 1</b><br>
將按鈕的字型家族設定為 `Segoe MDL2 Assets`，並其內容屬性設定為想使用的圖像的 unicode 值：
    :::column-end:::
    :::column:::
        ![Create an icon button step 1](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>Step 2</b><br>
您可以使用以下其中一個圖示元素物件：[BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon)、[FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon)、[PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) 或 [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon)。 這讓您有更多類型的圖示可選擇，並可讓您結合圖示和其他內容類型，例如文字，如果您想要：
    :::column-end:::
    :::column:::
        ![Create an icon button step 2](images/icons/icon-text-step-2.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>在命令列中建立一系列的圖示

:::row:::
    :::column span:::
當您有一系列結合的命令，例如剪下/複製/貼上或一組相片編輯程式的繪圖命令，請都放在同一個[命令列](../controls-and-patterns/app-bars.md)中。 命令列需要一個或多個應用程式列按鈕或是應用程式列切換按鈕，每個按鈕皆代表一個動作。 每個按鈕都有一個[圖示](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon)屬性，可用來控制要顯示哪個圖示。 指定圖示有很多種方式。 
    :::column-end:::
    :::column:::
        ![Example of a command bar with icons](images/icons/create-icon-command-bar.svg)
    :::column-end:::
:::row-end:::

最簡單的方式是使用我們提供的預先定義圖示清單，只需指定圖示名稱，例如 [返回] 或 [停止]，然後系統便會進行繪製： 

``` xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>
</CommandBar>

```
如需完整圖示名稱的清單，請參閱 [Symbol 列舉](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbol)。 

還有其他方式能在命令列中提供按鈕圖示：

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon)，圖示依據的是指定字型系列的字符。
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon)，圖示依據的是具有指定 **Uri** 的點陣圖影像。
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon)，圖示依據的是[路徑](/uwp/api/windows.ui.xaml.shapes.path)資料。

如需深入了解命令列，請參閱[命令列](../controls-and-patterns/app-bars.md)一文。 



## <a name="related-articles"></a>相關文章

* [磚和圖示資產的指導方針](../shell/tiles-and-notifications/app-assets.md)
