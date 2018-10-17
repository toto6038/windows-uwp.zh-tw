---
author: Jwmsft
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: 使用筆刷
description: Brush 物件可用來繪製形狀、文字或部分控制項的內部或外框，這樣繪製的物件才會顯示在 UI 中。
ms.author: jimwalk
ms.date: 07/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0473ee984461bf46be4ebf866a564f0d51e0cfc5
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "4692505"
---
# <a name="using-brushes-to-paint-backgrounds-foregrounds-and-outlines"></a>使用筆刷繪製背景、前景和輪廓

您使用 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 物件來繪製 XAML 形狀、文字或部分控制項的內部或外框，這樣繪製的物件才會顯示在 UI 中。 讓我們看看可用的筆刷以及筆刷的用法。

> **重要 API**：[Brush 類別](/uwp/api/Windows.UI.Xaml.Media.Brush)

## <a name="introduction-to-brushes"></a>筆刷介紹

若要繪製顯示在 app 畫布上的物件 (像是 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 或部分 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390))，您需要使用 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)。 例如，您要將 **Shape** 或 [**Background**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx) 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) 屬性和 **Control** 的 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.foreground.aspx) 屬性設為 **Brush** 值，而 **Brush** 則決定 UI 元素如何繪製或在 UI 中轉譯的方式。 

筆刷的不同類型︰ 
-   [**AcrylicBrush**](/uwp/api/windows.ui.xaml.media.acrylicbrush)
-   [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)
-   [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) 
-   [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101)
-   [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703)
-   [**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)

## <a name="solid-color-brushes"></a>純色筆刷

[**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 會以單一 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) (如紅色或藍色) 繪製區域。 這是最基本的筆刷。 在 XAML 中，有三種方法可以定義 **SolidColorBrush** 及其指定的純色色彩：預先定義的色彩名稱、十六進位色彩值，或屬性 (Property) 元素語法。

### <a name="predefined-color-names"></a>預先定義的色彩名稱

您可以使用預先定義的色彩名稱，像是 [**Yellow**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.yellow.aspx) 或 [**Magenta**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.magenta.aspx)。 共有 256 個可用的命名色彩。 XAML 剖析器會將色彩名稱轉換成具有正確色板的 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) 結構。 這 256 個命名色彩是以階層式樣式表層級 3 (CSS3) 規格中的 *X11* 色彩名稱為基礎，因此如果您之前有 Web 開發或設計的經驗，您可能已經熟悉這個命名色彩的清單。

下列範例會將 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) 屬性設成預先定義的色彩 [**Red**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.red.aspx)。

```xml
<Rectangle Width="100" Height="100" Fill="Red" />
```

下列影像顯示套用到 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 的 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)。

![經過轉譯的 SolidColorBrush。](images/brushes-solidcolorbrush.jpg)

如果您使用程式碼定義 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 而不是使用 XAML，則每個命名色彩就是 [**Colors**](https://msdn.microsoft.com/library/windows/apps/windows.ui.colors) 類別的靜態屬性值。 例如，若要宣告 **SolidColorBrush** 的 [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.solidcolorbrush.color.aspx) 值以代表命名色彩 "Orchid"，請將 **Color** 值設為靜態值 [**Colors.Orchid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.orchid.aspx)。

### <a name="hexadecimal-color-values"></a>十六進位色彩值

您可以使用十六進位格式字串，為 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 宣告精確的 24 位元色彩值與 8 位元 Alpha 色板。 範圍 0 到 F 之間的兩個字元定義每個元件值，十六進位字串的元件值順序為：Alpha 色板 (不透明度)、紅色色板、綠色色板以及藍色色板 (**ARGB**)。 例如，十六進位值 "\#FFFF0000" 定義完全不透明的紅色 (Alpha="FF"、紅色="FF"、綠色="00"，以及藍色="00")。

下列 XAML 範例將 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) 屬性設為十六進位值 "\#FFFF0000"，可以得到與使用命名色彩 [**Colors.Red**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.red.aspx) 相同的結果。

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <a name="span-idpropertyelementsyntaxspanspan-idpropertyelementsyntaxspanspan-idpropertyelementsyntaxspanproperty-element-syntax"></a><span id="Property_element_syntax__"></span><span id="property_element_syntax__"></span><span id="PROPERTY_ELEMENT_SYNTAX__"></span>屬性 (Property) 元素語法

您可以使用屬性 (Property) 元素語法來定義 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)。 這個語法比之前的方法更複雜，但是您可以在元素中指定其他屬性值，例如 [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.opacity.aspx)。 如需 XAML 語法的詳細資訊 (包括屬性 (Property) 元素語法)，請參閱 [XAML 概觀](https://msdn.microsoft.com/library/windows/apps/Mt185595)和 [XAML 語法指南](https://msdn.microsoft.com/library/windows/apps/Mt185596)。

在之前的範例中，您應該未曾在語法中看到 "SolidColorBrush" 字串。 建立的筆刷是經由隱含方式自動建立的，這是在多數常見案例中為協助保持簡單的 UI 定義而刻意使用的 XAML 語言簡略格式。 以下範例建立一個 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)，並明確地建立 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 做為 [**Rectangle.Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) 屬性的元素值。 **SolidColorBrush** 的 [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.solidcolorbrush.color.aspx) 設為 [**Blue**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.blue.aspx)，而 [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.opacity.aspx) 設為 0.5。

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <a name="span-idlineargradientbrushesspanspan-idlineargradientbrushesspanspan-idlineargradientbrushesspanlinear-gradient-brushes"></a><span id="Linear_gradient_brushes_"></span><span id="linear_gradient_brushes_"></span><span id="LINEAR_GRADIENT_BRUSHES_"></span>線性漸層筆刷

[**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) 繪製區域時所用的漸層是沿著一條線定義的。 這條線稱為 *「漸層軸」*。 您可以沿著使用 [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) 物件的漸層軸來指定色彩及其位置。 根據預設，漸層軸從筆刷繪製區域的左上角延伸至右下角，形成一個對角陰影。

[**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) 是漸層筆刷的基本建置組塊。 漸層停駐點會指定筆刷套用到繪製的區域時，漸層軸上的 [**Offset**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.offset.aspx) 使用什麼 [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx) 的筆刷。

漸層停駐點的 [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx) 屬性會指定漸層停駐點的色彩。 您可以使用預先定義的色彩或藉由指定十六進位 **ARGB** 值來設定色彩。

[**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) 的 [**Offset**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.offset.aspx) 屬性指定了每個 **GradientStop** 在漸層軸的位置。 **Offset** 是一個介於 0 到 1 的 **double**。 值為 0 的 **Offset** 會在漸層軸的起點放置 **GradientStop**，換句話說，就在 [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.startpoint.aspx) 的附近。 值為 1 的 **Offset** 會在 [**EndPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.endpoint.aspx) 放置 **GradientStop**。 有用的 [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) 至少應含有兩個 **GradientStop** 值，其中每個 **GradientStop** 應指定不同的 [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx)，並含有 0 到 1 之間的不同 **Offset**。

以下範例建立一個四色線性漸層，並用它來繪製 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)。

```xml
<!-- This rectangle is painted with a diagonal linear gradient. -->
<Rectangle Width="200" Height="100">
    <Rectangle.Fill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Yellow" Offset="0.0" x:Name="GradientStop1"/>
            <GradientStop Color="Red" Offset="0.25" x:Name="GradientStop2"/>
            <GradientStop Color="Blue" Offset="0.75" x:Name="GradientStop3"/>
            <GradientStop Color="LimeGreen" Offset="1.0" x:Name="GradientStop4"/>
        </LinearGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

在漸層停駐點之間每個點的色彩，都是以線性插補成由兩個連結漸層停駐點所指定的色彩結合。 下列圖例強調顯示前述範例中的漸層停駐點。 圓圈標示漸層停駐點的位置，虛線則是漸層軸。

![漸層停駐點](images/linear-gradients-stops.png) 您可以將 [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.startpoint.aspx) 與 [**EndPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.endpoint.aspx) 屬性設為與起始預設值 `(0,0)` 與 `(1,1)` 不同的其他值，藉此變更漸層停駐點所在的線條位置。 變更 **StartPoint** 與 **EndPoint** 座標值，就能建立水平或垂直漸層、反轉漸層方向，或是壓縮漸層範圍以套用到比完整繪製區域小的範圍。 若要壓縮漸層，請將 **StartPoint** 和/或 **EndPoint** 的值設在 0 到 1 之間。 例如，如果想要水平漸層在筆刷的左半部漸層但在右半部使用上個 [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) 使用的純色，請將 **StartPoint** 指定為 `(0,0)`，**EndPoint** 指定為 `(0.5,0)`。

### <a name="span-idusetoolstomakegradientsspanspan-idusetoolstomakegradientsspanspan-idusetoolstomakegradientsspanuse-tools-to-make-gradients"></a><span id="Use_tools_to_make_gradients"></span><span id="use_tools_to_make_gradients"></span><span id="USE_TOOLS_TO_MAKE_GRADIENTS"></span>使用工具製作漸層

在了解線性漸層的運作方式之後，現在您可以利用 Visual Studio 或 Blend，簡化這些漸層的建立作業。 若要建立漸層，請在設計表面或 XAML 檢視中選取要套用漸層的物件。 展開 **\[筆刷\]**，然後選取 **\[線性漸層\]** 索引標籤 (請參閱下一個螢幕擷取畫面)。

![使用 Visual Studio 建立線性漸層。](images/tool-gradient-brush-1.png)

現在，您可以變更漸層停駐點的色彩，並使用底部的列移動其位置。 您也可以按一下此列以新增漸層停駐，以及將停駐點拖出此列來移除停駐點 (請參閱下一個螢幕擷取畫面)。

![位於屬性視窗底部可控制漸層停駐點的列。](images/tool-gradient-brush-2.png)

## <a name="span-idimagebrushesspanspan-idimagebrushesspanspan-idimagebrushesspanimage-brushes"></a><span id="Image_brushes"></span><span id="image_brushes"></span><span id="IMAGE_BRUSHES"></span>影像筆刷

[**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 使用影像繪製區域，而要用來繪製的影像則來自影像檔案來源。 [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/BR210107) 屬性應設定為要載入之影像的路徑。 影像來源通常來自 app 資源中的 **Content** 項目。

根據預設值，[**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 會伸展影像，使其完全填滿繪圖區，如果繪圖區與影像的長寬比不同，影像可能會失真。 只要變更 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/BR242975) 屬性的預設值 **Fill**，將它設定為 **None**、**Uniform** 或 **UniformToFill**，就可以變更此行為。

以下範例建立一個 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101)，並將 [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/BR210107) 設成名為 licorice.jpg 的影像，該影像必須位在 app 的資源中。 **ImageBrush** 接著繪製 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 形狀定義的區域。

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

以下是 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 轉譯後的樣子。

![經過轉譯的 ImageBrush。](images/brushes-imagebrush.jpg)

[**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 與 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 會以統一資源識別元 (URI) 參照影像來源檔案，而該影像來源檔案使用數個可能的影像格式。 這些影像來源檔案是以 URI 來指定。 如需指定影像來源、可使用的影像格式，以及將影像來源封裝在應用程式中的相關資訊，請參閱[Image 和 ImageBrush](https://msdn.microsoft.com/library/windows/apps/Mt280382)。

## <a name="brushes-and-text"></a>筆刷與文字

您也可以使用筆刷將轉譯特性套用至文字元素。 例如，[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 的 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665) 屬性可以接受 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)。 您可以將這裡描述的任何筆刷套用到文字。 將筆刷套用到文字時請務必小心，如果您使用的筆刷會與文字的背景混淆，或者會讓使用者無法分辨文字的外框，可能導致使用者無法閱讀文字。 在大多數的情況下使用 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 來維持可讀性，除非您的文字元素僅供裝飾使用。

即使是使用純色，也請注意您選擇的任何文字色彩，都必須與文字配置容器的背景色彩有足夠的對比。 文字前景與文字容器背景之間的對比度是必須考量的協助工具設定。

## <a name="webviewbrush"></a>WebViewBrush

[**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) 是特殊的筆刷類型，可以存取一般在 [**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702) 控制項中檢視的內容。 **WebViewBrush** 並非在矩形的 **WebView** 控制項區域中轉譯內容，而是將該內容繪製到具有轉譯介面 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 類型屬性的另一個元素上。 **WebViewBrush** 並不適用於每種筆刷案例，但對轉換 **WebView** 很有用。 如需詳細資訊，請參閱 [**WebViewBrush**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)。

## <a name="xamlcompositionbrushbase"></a>XamlCompositionBrushBase

[**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)是用來建立使用[**CompositionBrush**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.CompositionBrush)繪製 XAML UI 元素之自訂筆刷的基底類別。

這可促成 Windows.UI.Xaml 與 Windows.UI.Composition 圖層次之間的交互操作，如[**視覺層概觀**](/windows/uwp/composition/visual-layer)中所述。 

若要建立自訂筆刷，請建立從 XamlCompositionBrushBase 繼承並實作所需方法的新類別。

例如，使用此項以套用[**效果**](/windows/uwp/composition/composition-effects)至 XAML UIElement，方法是使用 [**CompositionEffectBrush**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.CompositionEffectBrush)(如 **GaussianBlurEffect**或[**SceneLightingEffect**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect))，其功用是當 [**XamlLight**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamllight) 照亮時控制 XAML UIElement 的反射屬性。

如需程式碼範例，請參閱 [**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) 的參考頁面。

## <a name="brushes-as-xaml-resources"></a>XAML 資源形式的筆刷

您可以在 XAML 資源字典將任一筆刷宣告為索引 XAML 資源。 這樣就可以輕鬆將相同筆刷值複寫要套用 UI 的多項元素中。 然後就能在將筆刷資源參照為 [{StaticResource}](https://msdn.microsoft.com/library/windows/apps/Mt185588) 用法的 XAML 中共用和套用筆刷值。 這情況好比您擁有一個參照共用筆刷的 XAML 控制項範本，該控制項範本本身就是索引 XAML 資源。

## <a name="brushes-in-code"></a>筆刷程式碼

使用 XAML 指定筆刷比使用程式碼定義筆刷更常見。 這是因為筆刷通常定義為 XAML 資源，且因為筆刷值通常是設計工具的輸出，不然就是 XAML UI 定義的一部分。 儘管如此，如果您偶爾想要使用程式碼定義筆刷，所有 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 類型都可以用在程式碼具現化。

若要以程式碼建立 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)，請使用採用 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) 參數的建構函式。 傳送 [**Colors**](https://msdn.microsoft.com/library/windows/apps/windows.ui.colors) 類別的靜態屬性值，像這樣：

```cs
SolidColorBrush blueBrush = new SolidColorBrush(Windows.UI.Colors.Blue);
```

```vb
Dim blueBrush as SolidColorBrush = New SolidColorBrush(Windows.UI.Colors.Blue)
```

```cppwinrt
Windows::UI::Xaml::Media::SolidColorBrush blueBrush{ Windows::UI::Colors::Blue() };
```

```cpp
blueBrush = ref new SolidColorBrush(Windows::UI::Colors::Blue);
```

如果是 [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) 和 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101)，使用預設建構函式，然後呼叫其他 API 後再嘗試將該筆刷用在 UI 屬性。

-   當您使用程式碼定義 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 時，[**ImageSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imagebrush.imagesourceproperty.aspx) 需要一個 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) (不是 URI)。 如果您的來源是資料流，使用 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) 方法初始化該值。 如果您的來源是 URI，包含 app 中使用 **ms-appx** 或 **ms-resource** 配置的內容，則使用採用 URI 的 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/br243238.aspx) 建構函式。 如果有任何與影像來源的抓取或解碼相關的時機問題，您也可以考慮處理 [**ImageOpened**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imageopened.aspx) 事件，在這種情況下，您可能需要在影像來源可供使用前先顯示替代內容。
-   對於 [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703)，如果您最近已重設 [**SourceName**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.sourcename.aspx) 屬性，或者如果程式碼也同時變更 [**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702) 的內容，則可能需要呼叫 [**Redraw**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.redraw.aspx)。

如需程式碼範例，請參閱 [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703)、 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 和 [**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) 的參考頁面。
 

 




