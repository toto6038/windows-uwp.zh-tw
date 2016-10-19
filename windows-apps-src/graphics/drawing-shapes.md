---
author: Jwmsft
ms.assetid: 54CC0BD4-1961-44D7-AB40-6E8B58E42D65
title: "繪製形狀"
description: "了解如何繪製圖形，例如橢圓形、矩形、多邊形以及路徑。 Path 類別是在 XAML UI 中將極複雜向量繪製語言視覺化的一種方法，例如，您可繪製貝茲曲線。"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 2fd20e07c9b7e54559baeeb8324f11065a25444c

---
# 繪製圖形

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** 重要 API **

-   [**路徑**](https://msdn.microsoft.com/library/windows/apps/BR243355)
-   [**Windows.UI.Xaml.Shapes 命名空間**](https://msdn.microsoft.com/library/windows/apps/BR243401)
-   [**Windows.UI.Xaml.Media 命名空間**](https://msdn.microsoft.com/library/windows/apps/BR243045)

了解如何繪製圖形，例如橢圓形、矩形、多邊形以及路徑。 [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355) 類別是在 XAML UI 中視覺化非常複雜之向量繪製語言的方法，例如，繪製貝茲曲線。

## 簡介

有兩組類別可定義 XAML UI 的空間區域：[**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377) 類別與 [**Geometry**](https://msdn.microsoft.com/library/windows/apps/BR210041) 類別。 這兩組類別的主要差異，在於 **Shape** 有一個相關聯的筆刷，而且可以轉譯到螢幕上；**Geometry** 只定義空間區域，而且除非有助於提供資訊給其他 UI 屬性，否則並不會進行轉譯。 您可以將 **Shape** 想成是一個 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)，且由 **Geometry** 定義其界限。 本主題主要涵蓋 **Shape** 類別。

[**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377) 類別有 [**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345)、[**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343)、[**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)、[**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359)、[**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) 以及 [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355)。 **Path** 很有趣，因為它可以定義任意幾何圖形，而且這裡還涉及 [**Geometry**](https://msdn.microsoft.com/library/windows/apps/BR210041) 類別，因為這是定義 **Path** 組件的其中一種方法。

## 圖形的填滿與筆觸

若要將 [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377) 轉譯到應用程式畫布，您必須使其與 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) 建立關聯。 將 **Shape** 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 屬性設成所需的 **Brush**。 如需關於筆刷的資訊，請參閱[使用筆刷](using-brushes.md)。

[**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377) 也可以包含 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke)，這是在圖形周邊繪製的線條。 **Stroke** 也需要 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) 來定義外觀，且 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 應為非零的值。 **StrokeThickness** 是定義圖形邊緣之周邊厚度的屬性。 如果您沒有為 **Stroke** 指定 **Brush** 值，或是將 **StrokeThickness** 設為 0，則不會繪製圖形周圍的框線。

## 橢圓形

[**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) 是周邊為弧形的圖形。 若要建立基本的 **Ellipse**，請指定 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 的 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751)、[**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 以及 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076)。

以下範例會建立 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) 200 與 [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 200 的一個 [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343)，並使用 [**SteelBlue**](https://msdn.microsoft.com/library/windows/apps/Hh748056) 色的 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 作為其 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)。

```xml
<Ellipse Fill="SteelBlue" Height="200" Width="200" />
```

以下是經過轉譯的 [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343)。

![經過轉譯的橢圓形。](images/shapes-ellipse.jpg)

大多數人會將這裡的 [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) 視為圓形，事實上您在 XAML 宣告圓形的方式正是：使用相等 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) 與 [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 的 **Ellipse**。

當 [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) 置於 UI 配置中時，會將其大小假設為與該 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) 和 [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 的矩形相同；周邊外圍的區域不會轉譯，但仍是其配置位置大小的一部分。

一組 6 個 [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) 元素屬於 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538) 控制項的控制項範本，而 2 個同心 **Ellipse** 元素則屬於 [**RadioButton**](https://msdn.microsoft.com/library/windows/apps/BR227544)。

## <span id="Rectangle"></span><span id="rectangle"></span><span id="RECTANGLE"></span>矩形

[**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 是對應邊相等的四邊形。 若要建立基本的 **Rectangle**，請指定 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751)、[**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 以及 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)。

您可以使 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 的四角變成圓角。 若要建立圓角，請指定 [**RadiusX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) 與 [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy) 屬性的值。 這些屬性指定橢圓形的 x-軸與 y-軸，進而定義圓角的弧形。 **RadiusX** 允許的最大值是 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) 除以二，**RadiusY** 允許的最大值是 [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 除以二。

以下範例會建立 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) 200 與 [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 100 的 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)。 它的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 使用 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 的 [**Blue**](https://msdn.microsoft.com/library/windows/apps/Hh747837) 值，[**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 使用 **SolidColorBrush** 的 [**Black**](https://msdn.microsoft.com/library/windows/apps/Hh747833) 值。 我們將 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 設成 3。 同時並將 [**RadiusX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) 屬性設成 50，將 [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy) 屬性設成 10，就會使 **Rectangle** 變成圓角。

```xml
<Rectangle Fill="Blue"
           Width="200"
           Height="100"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10" />
           ```

Here's the rendered [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371).

![A rendered Rectangle.](images/shapes-rectangle.jpg)

**Tip**  There are some scenarios for UI definitions where instead of using a [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371), a [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) might be more appropriate. If your intention is to create a rectangle shape around other content, it might be better to use **Border** because it can have child content and will automatically size around that content, rather than using the fixed dimensions for height and width like **Rectangle** does. A **Border** also has the option of having rounded corners if you set the [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.border.cornerradius) property.

 

On the other hand, a [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) is probably a better choice for control composition. A **Rectangle** shape is seen in many control templates because it's used as a "FocusVisual" part for focusable controls. Whenever the control is in a "Focused" visual state, this rectangle is made visible, in other states it's hidden.

## Polygon

A [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) is a shape with a boundary defined by an arbitrary number of points. The boundary is created by connecting a line from one point to the next, with the last point connected to the first point. The [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polygon.points.aspx) property defines the collection of points that make up the boundary. In XAML, you define the points with a comma-separated list. In code-behind you use a [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220) to define the points and you add each individual point as a [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) value to the collection.

You don't need to explicitly declare the points such that the start point and end point are both specified as the same [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) value. The rendering logic for a [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) assumes that you are defining a closed shape and will connect the end point to the start point implicitly.

The next example creates a [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) with 4 points set to `(10,200)`, `(60,140)`, `(130,140)`, and `(180,200)`. It uses a [**LightBlue**](https://msdn.microsoft.com/library/windows/apps/Hh747960) value of [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) for its [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill), and has no value for [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) so it has no perimeter outline.

```xml
<Polygon Fill="LightBlue"
         Points="10,200,60,140,130,140,180,200" />
```

以下是經過轉譯的 [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359)。

![經過轉譯的多邊形。](images/shapes-polygon.jpg)

**提示**：在宣告圖形頂點以外的 XAML 中，[**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 值通常會用來做為類型。 例如，**Point** 屬於觸控事件的事件資料，因此您可以知道觸控動作在座標空間發生的確切位置。 如需有關 **Point** 以及如何將其使用於 XAML 或程式碼的詳細資訊，請參閱 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 的 API 參考主題。

 

## 線條

[**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345) 只是在座標空間中兩點之間繪製的線條。 **Line** 會忽略提供給 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 的任何值，因為它沒有內部空間。 對於 **Line**，請務必指定 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 的值與 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 屬性，否則 **Line** 無法轉譯。

您無法使用 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 值指定 [**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345) 圖形，而是必須使用 [**X1**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.x1.aspx)、[**Y1**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.y1.aspx)、[**X2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.x2.aspx) 以及 [**Y2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.y2.aspx) 的離散 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 值。 這樣就可以使用最簡潔的標記語言來繪製水平或垂直線。 例如，`<Line Stroke="Red" X2="400"/>` 定義了 400 個像素長的水平線。 另一個 X、Y 屬性預設為 0，因此這個 XAML 的點會繪製從 `(0,0)` 到 `(400,0)` 的線條。 然後，如果您希望它從 (0,0) 以外的點開始，可以使用 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) 移動整個 **Line**。

## <span id="_Polyline"></span><span id="_polyline"></span><span id="_POLYLINE"></span> 聚合線條

[**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) 與 [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) 類似，都是由一組點定義圖形的界限，不過 **Polyline** 的最後一點並不會連接第一個點。

**注意**：您可以在設定給 [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) 的 [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) 中明確指定相同的起點與終點，但在該情況下，您大可改用 [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359)。

 

如果指定 [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)，**Fill** 就會繪製圖形的內部空間，即使設定給 **Polyline** 之 [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) 的起點與終點未交叉也一樣。 如果未指定 **Fill**，**Polyline** 會與指定數個個別 [**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345) 元素的轉譯結果一樣，即連續線條的起點與終點會交叉。

如同 [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359)，[**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) 屬性定義形成界限的點集合。 在 XAML 中，您使用逗號分隔清單定義各個點。 在程式碼後置中，則是使用 [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220) 定義點，並將每個獨立的點當成 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 結構加入集合中。

此範例會建立四點分別設為 `(10,200)`、`(60,140)`、`(130,140)` 和 `(180,200)` 的 [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365)。 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 已定義，但並非 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)。

```xml
<Polyline Stroke="Black"
        StrokeThickness="4"
        Points="10,200,60,140,130,140,180,200" />
```

以下是經過轉譯的 [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365)。 請注意，第一個點與最後一點並不會像在 [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) 般以 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 外框相互連接。

![經過轉譯的聚合線條。](images/shapes-polyline.jpg)

## 路徑

[**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355) 是最多功能的 [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377)，因為它可以用來定義任意幾何圖形。 但是多功能性也意謂著複雜度較高。 我們來看一下如何在 XAML 中建立基本的 **Path**。

以 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 屬性定義路徑的幾何圖形。 設定 **Data** 的技術有兩種：

-   您可以在 XAML 中設定 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 的字串值。 使用此格式時，**Path.Data** 值會使用圖形的序列化格式。 這個值在最初建立後，您通常不會再以字串格式對此值做文字編輯。 而是使用可讓您在介面使用設計或以隱喻方法繪圖的設計工具。 然後儲存或匯出輸出，這樣您會取得含有 **Path.Data** 資訊的 XAML 檔案或 XAML 字串片段。
-   將 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 屬性設為單一 [**Geometry**](https://msdn.microsoft.com/library/windows/apps/BR210041) 物件。 這可以在程式碼或在 XAML 中完成。 該單一 **Geometry** 通常為 [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.geometrygroup)，可以將多個幾何定義組合成單一物件的容器以作為物件模型之用。 最常這樣做的理由是因為您想要使用可以定義為 [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143) 之 [**Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164) 值的一或多個曲線與複合圖形，例如 [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/BR228068)。

這個範例顯示 [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355) 的產生方式，我們在此使用 Blend for Visual Studio 製作少數向量圖形，接著將結果儲存為 XAML。 **Path** 共包含一段貝茲曲線與一段直線。 這個範例是為了讓您了解 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 序列化格式所含的元素，以及數字代表的意義。

[**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 由移動命令 (以 "M" 表示) 開始進行，該命令會建立路徑的絕對起點。

第一段是起點為 `(100,200)` 而終點為 `(400,175)` 的貝茲立方曲線，使用兩個控制點 `(100,25)` 與 `(400,350)` 繪製。 這一段是由 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 屬性字串中的 "C" 命令指示。

第二段以絕對水平線命令 "H" 開始，該命令指定一條由前面子路徑端點 `(400,175)` 到新端點 `(280,175)` 所繪製出的直線。 因為它是一個水平線命令，所以指定的值是一個 x 座標。

```xml
<Path Stroke="DarkGoldenRod" 
      StrokeThickness="3"
      Data="M 100,200 C 100,25 400,350 400,175 H 280" />
      ```

Here's the rendered [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355).

![A rendered Path.](images/shapes-path.jpg)

The next example shows a usage of the other technique we discussed: a [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.geometrygroup) with a [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168). This example exercises some of the contributing geometry types that can be used as part of a **PathGeometry**: [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143) and the various elements that can be a segment in [**PathFigure.Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164).

```xml
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

使用 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168) 可能會較填入 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 字串更易閱讀。 相反地，[**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 使用與可縮放向量圖形 (SVG) 映像路徑定義相容的語法，在從 SVG 移植圖形或自 Blend 等工具輸出時非常實用。

 

 







<!--HONumber=Aug16_HO3-->


