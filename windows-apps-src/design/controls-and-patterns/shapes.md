---
ms.assetid: 54CC0BD4-1961-44D7-AB40-6E8B58E42D65
title: 繪製圖形
description: 了解如何繪製圖形，例如橢圓形、矩形、多邊形以及路徑。 Path 類別是在 XAML UI 中將極複雜向量繪製語言視覺化的一種方法，例如，您可繪製貝茲曲線。
ms.date: 11/16/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a576add7a080874fb0f042748bef7472e04ac817
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8205762"
---
# <a name="draw-shapes"></a>繪製形狀

了解如何繪製圖形，例如橢圓形、矩形、多邊形以及路徑。 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 類別是在 XAML UI 中視覺化非常複雜之向量繪製語言的方法，例如，繪製貝茲曲線。

> **重要 API**：[Path 類別](/uwp/api/Windows.UI.Xaml.Shapes.Path)、[Windows.UI.Xaml.Shapes 命名空間](/uwp/api/Windows.UI.Xaml.Shapes)、[Windows.UI.Xaml.Media 命名空間](/uwp/api/Windows.UI.Xaml.Media)


有兩組類別可定義 XAML UI 的空間區域：[**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 類別與 [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) 類別。 這兩組類別的主要差異，在於 **Shape** 有一個相關聯的筆刷，而且可以轉譯到螢幕上；**Geometry** 只定義空間區域，而且除非有助於提供資訊給其他 UI 屬性，否則並不會進行轉譯。 您可以將 **Shape** 想成是一個 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)，且由 **Geometry** 定義其界限。 本主題主要涵蓋 **Shape** 類別。

[**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 類別有 [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line)、[**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)、[**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)、[**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)、[**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) 以及 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)。 **Path** 很有趣，因為它可以定義任意幾何圖形，而且這裡還涉及 [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) 類別，因為這是定義 **Path** 組件的其中一種方法。

## <a name="fill-and-stroke-for-shapes"></a>圖形的填滿與筆觸

若要將 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 轉譯到應用程式畫布，您必須使其與 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 建立關聯。 將 **Shape** 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 屬性設成所需的 **Brush**。 如需關於筆刷的資訊，請參閱[使用筆刷](../style/brushes.md)。

[**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 也可以包含 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke)，這是在圖形周邊繪製的線條。 **Stroke** 也需要 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 來定義外觀，且 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 應為非零的值。 **StrokeThickness** 是定義圖形邊緣之周邊厚度的屬性。 如果您沒有為 **Stroke** 指定 **Brush** 值，或是將 **StrokeThickness** 設為 0，則不會繪製圖形周圍的框線。

## <a name="ellipse"></a>橢圓形

[**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 是周邊為弧形的圖形。 若要建立基本的 **Ellipse**，請指定 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 的 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)、[**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 以及 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)。

以下範例會建立 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 200 與 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 200 的一個 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)，並使用 [**SteelBlue**](https://msdn.microsoft.com/library/windows/apps/Hh748056) 色的 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 作為其 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)。

```xaml
<Ellipse Fill="SteelBlue" Height="200" Width="200" />
```

```csharp
var ellipse1 = new Ellipse();
ellipse1.Fill = new SolidColorBrush(Windows.UI.Colors.SteelBlue);
ellipse1.Width = 200;
ellipse1.Height = 200;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(ellipse1);
```

以下是經過轉譯的 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)。

![經過轉譯的橢圓形。](images/shapes-ellipse.jpg)

大多數人會將這裡的 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 視為圓形，事實上您在 XAML 宣告圓形的方式正是：使用相等 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 與 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 的 **Ellipse**。

當 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 置於 UI 配置中時，會將其大小假設為與該 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 和 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 的矩形相同；周邊外圍的區域不會轉譯，但仍是其配置位置大小的一部分。

一組 6 個 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 元素屬於 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538) 控制項的控制項範本，而 2 個同心 **Ellipse** 元素則屬於 [**RadioButton**](https://msdn.microsoft.com/library/windows/apps/BR227544)。

## <a name="span-idrectanglespanspan-idrectanglespanspan-idrectanglespanrectangle"></a><span id="Rectangle"></span><span id="rectangle"></span><span id="RECTANGLE"></span>矩形

[**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 是對應邊相等的四邊形。 若要建立基本的 **Rectangle**，請指定 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)、[**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 以及 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)。

您可以使 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 的四角變成圓角。 若要建立圓角，請指定 [**RadiusX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) 與 [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy) 屬性的值。 這些屬性指定橢圓形的 x-軸與 y-軸，進而定義圓角的弧形。 **RadiusX** 允許的最大值是 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 除以二，**RadiusY** 允許的最大值是 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 除以二。

以下範例會建立 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 200 與 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 100 的 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)。 它的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 使用 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 的 [**Blue**](https://msdn.microsoft.com/library/windows/apps/Hh747837) 值，[**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 使用 **SolidColorBrush** 的 [**Black**](https://msdn.microsoft.com/library/windows/apps/Hh747833) 值。 我們將 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 設成 3。 同時並將 [**RadiusX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) 屬性設成 50，將 [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy) 屬性設成 10，就會使 **Rectangle** 變成圓角。

```xaml
<Rectangle Fill="Blue"
           Width="200"
           Height="100"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10" />
```

```csharp
var rectangle1 = new Rectangle();
rectangle1.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
rectangle1.Width = 200;
rectangle1.Height = 100;
rectangle1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
rectangle1.StrokeThickness = 3;
rectangle1.RadiusX = 50;
rectangle1.RadiusY = 10;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(rectangle1);
```

以下是經過轉譯的 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)。

![經過轉譯的矩形。](images/shapes-rectangle.jpg)

**提示：** 有某些情況下，UI 定義，而不是使用一個[**矩形**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)，[**框線**](https://msdn.microsoft.com/library/windows/apps/BR209250)可能更適合。 如果您要在其他內容的周圍建立矩形，使用 **Border** 較合適，因為可以包含子內容，並且會自動沿著內容調整大小，不像 **Rectangle** 使用固定的高度與寬度。 如果設定 [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.border.cornerradius) 屬性，則 **Border** 也有包含圓角的選項。

另一方面，[**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 或許是控制項組合較佳的選擇。 **Rectangle** 形狀在很多控制項範本都看得到，因為它可以做為能取得焦點之控制項的 "FocusVisual" 組件。 只要控制項處於「取得焦點」的視覺狀態，這個矩形就會顯示，在其他狀態則會隱藏。

## <a name="polygon"></a>多邊形

[**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) 是界限由任意數目的點所定義的形狀。 建立界限的方式是以直線連接一個點到下一個點，最後一個點再連回第一個點。 [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polygon.points.aspx) 屬性定義形成界限的點集合。 在 XAML 中，您使用逗號分隔清單定義各個點。 在程式碼後置中，則是使用 [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220) 定義點，並將每個獨立的點當成 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 值加入集合中。

起點與終點不需要明確宣告，就會指定為相同的 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 值。 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) 的轉譯邏輯會假設您正在定義封閉的形狀，並會自動連接終點與起點。

下一個範例建立 4 點分別設成 `(10,200)`、`(60,140)`、`(130,140)` 以及 `(180,200)` 的 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)。 它的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 使用 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 的 [**LightBlue**](https://msdn.microsoft.com/library/windows/apps/Hh747960) 值，[**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 則未包含值，因此並無周邊外框。

```xaml
<Polygon Fill="LightBlue"
         Points="10,200,60,140,130,140,180,200" />
```

```csharp
var polygon1 = new Polygon();
polygon1.Fill = new SolidColorBrush(Windows.UI.Colors.LightBlue);

var points = new PointCollection();
points.Add(new Windows.Foundation.Point(10, 200));
points.Add(new Windows.Foundation.Point(60, 140));
points.Add(new Windows.Foundation.Point(130, 140));
points.Add(new Windows.Foundation.Point(180, 200));
polygon1.Points = points;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(polygon1);
```

以下是經過轉譯的 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)。

![經過轉譯的多邊形。](images/shapes-polygon.jpg)

**提示：**[**點**](https://msdn.microsoft.com/library/windows/apps/BR225870)值通常會用來做為類型在 XAML 中宣告圖形頂點以外的案例。 例如，**Point** 屬於觸控事件的事件資料，因此您可以知道觸控動作在座標空間發生的確切位置。 如需有關 **Point** 以及如何將其使用於 XAML 或程式碼的詳細資訊，請參閱 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 的 API 參考主題。

## <a name="line"></a>線條

[**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line) 只是在座標空間中兩點之間繪製的線條。 **Line** 會忽略提供給 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 的任何值，因為它沒有內部空間。 對於 **Line**，請務必指定 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 的值與 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 屬性，否則 **Line** 無法轉譯。

您無法使用 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 值指定 [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line) 圖形，而是必須使用 [**X1**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.x1.aspx)、[**Y1**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.y1.aspx)、[**X2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.x2.aspx) 以及 [**Y2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.y2.aspx) 的離散 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 值。 這樣就可以使用最簡潔的標記語言來繪製水平或垂直線。 例如，`<Line Stroke="Red" X2="400"/>` 定義了 400 個像素長的水平線。 另一個 X、Y 屬性預設為 0，因此這個 XAML 的點會繪製從 `(0,0)` 到 `(400,0)` 的線條。 然後，如果您希望它從 (0,0) 以外的點開始，可以使用 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) 移動整個 **Line**。

```xaml
<Line Stroke="Red" X2="400"/>
```

```csharp
var line1 = new Line();
line1.Stroke = new SolidColorBrush(Windows.UI.Colors.Red);
line1.X2 = 400;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(line1);
```

## <a name="span-idpolylinespanspan-idpolylinespanspan-idpolylinespan-polyline"></a><span id="_Polyline"></span><span id="_polyline"></span><span id="_POLYLINE"></span>聚合線條

[**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) 與 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) 類似，都是由一組點定義圖形的界限，不過 **Polyline** 的最後一點並不會連接第一個點。

**注意：** 您明確可能會有相同的起點和終點，以[**點**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx)設定為[**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline)，但在該情況下您可能無法使用[**多邊形**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)改為。

如果指定 [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)，**Fill** 就會繪製圖形的內部空間，即使設定給 **Polyline** 之 [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) 的起點與終點未交叉也一樣。 如果未指定 **Fill**，**Polyline** 會與指定數個個別 [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line) 元素的轉譯結果一樣，即連續線條的起點與終點會交叉。

如同 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)，[**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) 屬性定義形成界限的點集合。 在 XAML 中，您使用逗號分隔清單定義各個點。 在程式碼後置中，則是使用 [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220) 定義點，並將每個獨立的點當成 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 結構加入集合中。

此範例會建立四點分別設為 `(10,200)`、`(60,140)`、`(130,140)` 和 `(180,200)` 的 [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline)。 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 已定義，但並非 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)。

```xaml
<Polyline Stroke="Black"
        StrokeThickness="4"
        Points="10,200,60,140,130,140,180,200" />
```

```csharp
var polyline1 = new Polyline();
polyline1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
polyline1.StrokeThickness = 4;

var points = new PointCollection();
points.Add(new Windows.Foundation.Point(10, 200));
points.Add(new Windows.Foundation.Point(60, 140));
points.Add(new Windows.Foundation.Point(130, 140));
points.Add(new Windows.Foundation.Point(180, 200));
polyline1.Points = points;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(polyline1);
```

以下是經過轉譯的 [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline)。 請注意，第一個點與最後一點並不會像在 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) 般以 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 外框相互連接。

![經過轉譯的聚合線條。](images/shapes-polyline.jpg)

## <a name="path"></a>路徑

[**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 是最多功能的 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)，因為它可以用來定義任意幾何圖形。 但是多功能性也意謂著複雜度較高。 我們來看一下如何在 XAML 中建立基本的 **Path**。

以 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 屬性定義路徑的幾何圖形。 設定 **Data** 的技術有兩種：

- 您可以在 XAML 中設定 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 的字串值。 使用此格式時，**Path.Data** 值會使用圖形的序列化格式。 這個值在最初建立後，您通常不會再以字串格式對此值做文字編輯。 而是使用可讓您在介面使用設計或以隱喻方法繪圖的設計工具。 然後儲存或匯出輸出，這樣您會取得含有 **Path.Data** 資訊的 XAML 檔案或 XAML 字串片段。
- 將 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 屬性設為單一 [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) 物件。 這可以在程式碼或在 XAML 中完成。 該單一 **Geometry** 通常為 [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.geometrygroup)，可以將多個幾何定義組合成單一物件的容器以作為物件模型之用。 最常這樣做的理由是因為您想要使用可以定義為 [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143) 之 [**Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164) 值的一或多個曲線與複合圖形，例如 [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/BR228068)。

這個範例顯示 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 的產生方式，我們在此使用 Blend for Visual Studio 製作少數向量圖形，接著將結果儲存為 XAML。 **Path** 共包含一段貝茲曲線與一段直線。 這個範例是為了讓您了解 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 序列化格式所含的元素，以及數字代表的意義。

[**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 由移動命令 (以 "M" 表示) 開始進行，該命令會建立路徑的絕對起點。

第一段是起點為 `(100,200)` 而終點為 `(400,175)` 的貝茲立方曲線，使用兩個控制點 `(100,25)` 與 `(400,350)` 繪製。 這一段是由 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 屬性字串中的 "C" 命令指示。

第二段以絕對水平線命令 "H" 開始，該命令指定一條由前面子路徑端點 `(400,175)` 到新端點 `(280,175)` 所繪製出的直線。 因為它是一個水平線命令，所以指定的值是一個 x 座標。

```xaml
<Path Stroke="DarkGoldenRod" 
      StrokeThickness="3"
      Data="M 100,200 C 100,25 400,350 400,175 H 280" />
```

以下是經過轉譯的 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)。

![經過轉譯的路徑。](images/shapes-path.jpg)

下個範例示範我們已討論過的另一個技術用法：[**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.geometrygroup) 搭配 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168)。 這個範例練習可用在 **PathGeometry** 的一些參與幾何類型：[**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143) 以及可以是 [**PathFigure.Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164) 區段的各種元素。

```xaml
<Path Stroke="Black" StrokeThickness="1" Fill="#CCCCFF">
    <Path.Data>
        <GeometryGroup>
            <RectangleGeometry Rect="50,5 100,10" />
            <RectangleGeometry Rect="5,5 95,180" />
            <EllipseGeometry Center="100, 100" RadiusX="20" RadiusY="30"/>
            <RectangleGeometry Rect="50,175 100,10" />
            <PathGeometry>
                <PathGeometry.Figures>
                    <PathFigureCollection>
                        <PathFigure IsClosed="true" StartPoint="50,50">
                            <PathFigure.Segments>
                                <PathSegmentCollection>
                                    <BezierSegment Point1="75,300" Point2="125,100" Point3="150,50"/>
                                    <BezierSegment Point1="125,300" Point2="75,100"  Point3="50,50"/>
                                </PathSegmentCollection>
                            </PathFigure.Segments>
                        </PathFigure>
                    </PathFigureCollection>
                </PathGeometry.Figures>
            </PathGeometry>
        </GeometryGroup>
    </Path.Data>
</Path>
```

```csharp
var path1 = new Windows.UI.Xaml.Shapes.Path();
path1.Fill = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 204, 204, 255));
path1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
path1.StrokeThickness = 1;

var geometryGroup1 = new GeometryGroup();
var rectangleGeometry1 = new RectangleGeometry();
rectangleGeometry1.Rect = new Rect(50, 5, 100, 10);
var rectangleGeometry2 = new RectangleGeometry();
rectangleGeometry2.Rect = new Rect(5, 5, 95, 180);
geometryGroup1.Children.Add(rectangleGeometry1);
geometryGroup1.Children.Add(rectangleGeometry2);

var ellipseGeometry1 = new EllipseGeometry();
ellipseGeometry1.Center = new Point(100, 100);
ellipseGeometry1.RadiusX = 20;
ellipseGeometry1.RadiusY = 30;
geometryGroup1.Children.Add(ellipseGeometry1);

var pathGeometry1 = new PathGeometry();
var pathFigureCollection1 = new PathFigureCollection();
var pathFigure1 = new PathFigure();
pathFigure1.IsClosed = true;
pathFigure1.StartPoint = new Windows.Foundation.Point(50, 50);
pathFigureCollection1.Add(pathFigure1);
pathGeometry1.Figures = pathFigureCollection1;

var pathSegmentCollection1 = new PathSegmentCollection();
var pathSegment1 = new BezierSegment();
pathSegment1.Point1 = new Point(75, 300);
pathSegment1.Point2 = new Point(125, 100);
pathSegment1.Point3 = new Point(150, 50);
pathSegmentCollection1.Add(pathSegment1);

var pathSegment2 = new BezierSegment();
pathSegment2.Point1 = new Point(125, 300);
pathSegment2.Point2 = new Point(75, 100);
pathSegment2.Point3 = new Point(50, 50);
pathSegmentCollection1.Add(pathSegment2);
pathFigure1.Segments = pathSegmentCollection1;

geometryGroup1.Children.Add(pathGeometry1);
path1.Data = geometryGroup1;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(path1);
```

以下是經過轉譯的 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)。

![經過轉譯的路徑。](images/shapes-path-2.png)

使用 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168) 可能會較填入 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 字串更易閱讀。 相反地，[**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 使用與可縮放向量圖形 (SVG) 映像路徑定義相容的語法，在從 SVG 移植圖形或自 Blend 等工具輸出時非常實用。
