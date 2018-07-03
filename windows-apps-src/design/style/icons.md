---
author: mijacobs
Description: Good icons harmonize with typography and with the rest of the design language. They don’t mix metaphors, and they communicate only what’s needed, as speedily and simply as possible.
title: 圖示
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.author: mijacobs
ms.date: 05/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 077967c37f76c8f1d0942f365344de65db13b041
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983571"
---
# <a name="icons-for-uwp-apps"></a>適用於 UWP 應用程式的圖示

![圖示標頭影像](images/icons/header-icons.png)

圖示提供視覺速記，適用於動作、概念或產品。 透過壓縮意義成為符號影像，圖示可以跨語言障礙並協助您節省非常重要的資源：螢幕空間。 

圖示可在應用程式中顯示—甚至在應用程式以外顯示： 

:::列::: :::欄::: **應用程式裡的圖示**

        ![icons inside the app](images/icons/inside-icons.png)
        Inside your app, you use icons to represent an action, such as copying text or navigating to the settings page.
    :::column-end:::
    :::column:::
        **Icons outside the app**

        ![icons outside the app](images/icons/outside-icons.jpg)
         Outside your app, Windows uses an icon to represent your app in the start menu and in the taskbar. If the user chooses to pin your app to the start menu, your app's start tile can feature your app's icon. Your app's icon appears in the title bar and you can choose to create a splash screen with your app's logo.
    :::column-end:::
:::列結束:::

本文章將描述您應用程式中的圖示。 若要深入了解應用程式以外的圖示（應用程式圖示），請參閱 [應用程式及磚圖示文章](/windows/uwp/design/shell/tiles-and-notifications/app-assets)。

## <a name="when-to-use-icons"></a>使用圖時的時機

圖示可以節省空間，但什麼情況該使用？ 

:::列::: :::欄::: ![執行](images/do.svg) ![圖示標準影像](images/icons/icons-standard.svg)<br>

        Use an icon for actions, like cut, copy, paste, and save, or for navigation items in a navigation menu.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

        Use an icon if one already exists for the concept you want to represent. (To see whether an icon exists, check the Segoe icon list.)
    :::column-end:::
:::列結束:::

:::列::: :::欄::: ![執行](images/do.svg)![圖示購物車](images/icons/icon-shopping-cart.svg)<br>

        Use an icon if it's easy for the user to understand what the icon means and it's simple enough to be clear at small sizes.
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

        Don't use an icon if its meaning isn't clear, or if making it clear requires a complex shape.
    :::column-end:::
:::列結束:::



## <a name="using-the-right-type-of-icon"></a>使用正確的圖示類型

有許多的方式可建立圖示。 您可以使用像是 Segoe MDL2 Assets 的符號字型。 您可以建立自己的向量影像。 您甚至可以使用點陣圖影像，雖然我們不建議使用它。 以下是您可以將的圖示新增到您應用程式的不同方式摘要。 

### <a name="use-a-predefined-icon"></a>使用預先定義的圖示。
:::列::: :::欄::: Microsoft 提供超過 1000 個圖示的 Segoe MDL2 Assets 字型表單。 它可能不是直覺從字型取得圖示，但我們的字型顯示技術表示這些圖示在任何顯示器、任何解析度以及任何大小上上看起來都清楚銳利。 :::欄結束::: :::欄::: ![預先定義的圖示影像](images/icons/predefined-icon.png) :::欄結束::: :::列結束:::

### <a name="use-a-font"></a>使用字型。
:::列::: :::欄::: 您不需要使用 Segoe MDL2 Assets 字型--要您可以使用任何字型，例如 Wingdings 或 Webdings 使用者已安裝在他們系統上的。
:::欄結束::: :::欄::: ![wingdings 影像](images/icons/wingdings.png) :::欄結束::: :::列結束:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>使用可縮放向量圖形 (SVG) 檔案。
:::列::: :::欄::: SVG 資源適用於圖示，因為它們在任何大小或解析度上看起來都很銳利。 大部分的繪圖應用程式可以匯出到 SVG。 :::欄結束::: :::欄::: ![SVG 影像](images/icons/icon-scale.gif) :::欄結束::: :::列結束:::

### <a name="use-geometry-objects"></a>使用幾何物件。
:::列::: :::欄::: 就像 SVG 檔案，幾何是向量資源，因此它們看起來總是很銳利。 不過，建立幾何不容易，因為您需要個別指定每個點和曲線。 只有當您需要修改圖示，而您的應用程式正在執行（例如動畫）時，它真的是一個不錯的選擇。 如需指示，請參閱 [移動和繪製幾何命令](../../xaml-platform/move-draw-commands-syntax.md)。 :::欄結束::: :::欄::: ![幾何物件影像](images/icons/geometry-objects.png) :::欄結束::: :::列結束:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>您也可以使用點陣圖影像，例如 PNG, GIF, 或 JPEG，雖然我們不建議使用它。
:::列::: :::欄::: 在特定大小上建立的點陣圖影像，讓他們必須根據您所需的圖示大小與螢幕的解析度將縮放放大或縮小。 當影像縮放縮小 (縮小) 時，顯示會變得模糊；當縮放放大時，顯示會成塊狀而變得不太美觀。 如果您必須使用點陣圖影像，我們建議使用 PNG 或 JPEG 上的 GIF。 :::欄結束::: :::欄::: ![don't](images/dont.svg) ![點陣圖影像](images/icons/bitmap-image.png)  :::欄結束::: :::列結束:::

## <a name="make-the-icon-do-something"></a>讓圖示執行項目

一旦您有一個圖示，下一個步驟便是讓它執行一些與命令或瀏覽動作相關的項目。 執行此操作的最佳方式是將圖示新增至按鈕或命令列。 

![命令列影像](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>建立圖示按鈕

您可以在標準按鈕中放置圖示。 因為您可以使用按鈕的位置很廣泛，這讓您更具一點彈性來決定動作圖示出現的位置。 

有幾種方法新增按鈕的圖示：

:::列::: :::範圍欄 ="2"::: <b>步驟 1</b><br>
        設定按鈕的字型系列為 `Segoe MDL2 Assets` 以及您要使用的字符 unicode 值的內容屬性 : :::欄結束::: :::欄::: ![建立圖示按鈕步驟 1](images/icons/create-icon-step-1.svg) ::: 欄結束::: :::列結束:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::列::: :::範圍欄 ="2"::: <b>步驟 2</b><br>
        您可以使用其中一個圖示元素物件：[BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon)，[FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon)、[PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon)，或[SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon)。 這讓您有更多類型的圖示可選擇，並可讓您結合圖示和其他內容類型，例如文字，如果您想要 : :::欄結束::: :::欄::: ![建立圖示按鈕步驟 2](images/icons/icon-text-step-2.svg) :::欄結束::: :::列結束:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>在命令列中建立一系列的圖示

:::列::: :::範圍欄::: 當您有一系列結合的命令，例如剪下/複製/貼上或一組相片編輯程式的繪圖命令，請將它們放在同一個 [命令列](../controls-and-patterns/app-bars.md) 中。 命令列需要一個或多個應用程式列按鈕或應用程式列切換按鈕，每一個皆代表一個動作。 每個按鈕有一個[圖示](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) 屬性，您用來控制顯示哪些圖示。 指定圖示有很多種方式。 :::欄結束::: :::欄::: ![使用圖示的命令列範例](images/icons/create-icon-command-bar.svg) :::欄結束::: :::列結束:::

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

* [磚和圖示資產的指導方針](../shell/tiles-and-notifications/app-assets.md)
