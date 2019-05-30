---
Description: 理想的圖示達到印刷格式與其餘設計語言的平衡。 它們不會混合使用隱喻，而且只會盡可能快速並簡單地溝通所需的內容。
title: 圖示
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5e464251200812e79474d05d9d0a680b49167871
ms.sourcegitcommit: 7da28cf4f4e8390bc9a21a9488b03af39271cbbe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2019
ms.locfileid: "64564543"
---
# <a name="icons-for-uwp-apps"></a>適用於 UWP 應用程式的圖示

![圖示標頭影像](images/icons/header-icons.png)

圖示提供視覺速記，適用於動作、概念或產品。 透過壓縮意義成為符號影像，圖示可以跨語言障礙並協助您節省非常重要的資源：螢幕空間。 

圖示可在應用程式中顯示—甚至在應用程式以外顯示： 

:::row:::
    :::column:::
        **Icons inside the app**

        ![icons inside the app](images/icons/inside-icons.png)
在您的應用程式中，您可以使用圖示來表示的動作，例如複製文字，或瀏覽至 [設定] 頁面。
    :::column-end:::
    :::column:::
**應用程式外的圖示**

        ![icons outside the app](images/icons/outside-icons.jpg)
外部應用程式，Windows 會使用圖示，來代表您在 [開始] 功能表和工作列中的應用程式。 如果使用者選擇要釘選到開始 功能表應用程式，您的應用程式啟動圖格可能適用於您的應用程式圖示。 您的應用程式圖示會出現在標題列，您可以選擇使用您的應用程式標誌建立啟動顯示畫面。
    :::column-end:::
:::row-end:::

本文章將描述您應用程式中的圖示。 若要深入了解應用程式以外的圖示（應用程式圖示），請參閱 [應用程式及磚圖示文章](/windows/uwp/design/shell/tiles-and-notifications/app-assets)。

## <a name="when-to-use-icons"></a>使用圖時的時機

圖示可以節省空間，但什麼情況該使用？ 

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

您可以使用 圖示動作，例如剪下、 複製、 貼上，並儲存，或導覽功能表中的導覽項目。
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

如果您想要代表概念已經存在，請使用的圖示。 （若要查看的圖示是否存在，請檢查 Segoe 圖示清單）。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

如果很容易讓使用者了解圖示的意義，而且很簡單，清除在小的大小，請使用的圖示。
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

請勿使用圖示，如果其意義不清楚，或讓它清除，需要複雜的圖形。
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>使用正確的圖示類型

有許多的方式可建立圖示。 您可以使用像是 Segoe MDL2 Assets 的符號字型。 您可以建立自己的向量為基礎的映像。 您甚至可以使用點陣圖影像，雖然我們不建議使用它。 以下是您可以將的圖示新增到您應用程式的不同方式摘要。 

### <a name="use-a-predefined-icon"></a>使用預先定義的圖示。
:::row:::
    :::column:::
Microsoft 提供超過 1000 個圖示的 Segoe MDL2 資產字型形式。 它可能不是直覺從字型取得圖示，但我們的字型顯示技術表示這些圖示在任何顯示器、任何解析度以及任何大小上上看起來都清楚銳利。 如需相關指示，請參閱 < [Segoe MDL2 圖示](segoe-ui-symbol-font.md)。
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>使用字型。
:::row:::
    :::column:::
您不需要使用 Segoe MDL2 資產的字型，您可以使用任何使用者在其系統上，例如 Wingdings 或 Webdings 安裝的字型。
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>使用可縮放向量圖形 (SVG) 檔案。
:::row:::
    :::column:::
SVG 資源適合用於圖示，因為它們一律會尋找任何大小或解析度清晰。 大部分的繪圖應用程式可以匯出到 SVG。 如需相關指示，請參閱 < [SVGImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource)。
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>使用幾何物件。
:::row:::
    :::column:::
SVG 檔案，例如幾何是向量為基礎的資源，讓它們一律會尋找清晰。 不過，建立幾何不容易，因為您需要個別指定每個點和曲線。 只有當您需要修改圖示，而您的應用程式正在執行（例如動畫）時，它真的是一個不錯的選擇。 如需指示，請參閱 [移動和繪製幾何命令](../../xaml-platform/move-draw-commands-syntax.md)。 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>您也可以使用點陣圖影像，例如 PNG, GIF, 或 JPEG，雖然我們不建議使用它。
:::row:::
    :::column:::
點陣圖影像是在特定的大小，建立的因此他們需要根據您想要圖示大小和螢幕解析度相應增加或相應減少。 當影像縮放縮小 (縮小) 時，顯示會變得模糊；當縮放放大時，顯示會成塊狀而變得不太美觀。 如果您必須使用點陣圖影像，我們建議使用 PNG 或 JPEG 上的 GIF。 
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![Bitmap image](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>讓圖示執行項目

圖示之後下, 一個步驟是對它執行動作的關聯，以使用命令或瀏覽動作。 若要執行這項操作，最好是將圖示新增至按鈕或命令列。 

![命令列影像](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>建立圖示按鈕

您可以在標準按鈕中放置圖示。 因為您可以使用按鈕的位置很廣泛，這讓您更具一點彈性來決定動作圖示出現的位置。 

有幾種方法新增按鈕的圖示：

:::row:::
    :::column span="2":::
        <b>Step 1</b><br>
若要設定按鈕的字型家族`Segoe MDL2 Assets`，其內容屬性設定為您想要使用圖像 （glyph） 的 unicode 值：
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
您可以使用其中一個圖示的項目物件：[BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon)， [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon)， [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon)，或[SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon)。 這可讓您更多類型的圖示，可供選擇，並可讓您合併圖示和其他類型的內容，例如文字，如果您想要：
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
當您有一系列的命令，放在一起，例如剪下/複製/貼上或一組繪製命令的相片編輯程式，將它們一起放[命令列](../controls-and-patterns/app-bars.md)。 命令列需要一個或多個應用程式列按鈕或應用程式列切換按鈕，每一個皆代表一個動作。 每個按鈕有一個[圖示](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) 屬性，您用來控制顯示哪些圖示。 指定圖示有很多種方式。 
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

還有其他方式在命令列中提供按鈕的圖示：

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon)，圖示會根據指定的字型系列的字符。
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon)，圖示會根據使用指定 **Uri** 的點陣圖影像。
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon)，圖示會根據[路徑](/uwp/api/windows.ui.xaml.shapes.path) 的資料。

如需深入了解命令列，請參閱[命令列文章](../controls-and-patterns/app-bars.md)。 



## <a name="related-articles"></a>相關文章

* [圖格和圖示的資產的指導方針](../shell/tiles-and-notifications/app-assets.md)
