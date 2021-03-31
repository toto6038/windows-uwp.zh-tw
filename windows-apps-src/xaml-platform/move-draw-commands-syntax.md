---
description: 了解可用來指定路徑幾何做為 XAML 屬性值的移動與繪製命令 (一種迷你程式語言)。
title: 移動與繪製命令語法
ms.assetid: 7772BC3E-A631-46FF-9940-3DD5B9D0E0D9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e1a0350cfcc9d77b0341fe90a92749faac5c63f2
ms.sourcegitcommit: 249100d990cd5cf2854c59fa66803b7f83d5db96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2021
ms.locfileid: "105939013"
---
# <a name="move-and-draw-commands-syntax"></a>移動與繪製命令語法


了解可用來指定路徑幾何做為 XAML 屬性值的移動與繪製命令 (一種迷你程式語言)。 許多可以輸出向量圖形或形狀 (以序列化或交換格式) 的設計與圖形工具都會使用移動與繪製命令。

## <a name="properties-that-use-move-and-draw-command-strings"></a>使用移動與繪製命令字串的屬性

移動與繪製命令語法受到 XAML 的內部類型轉換器支援，這個轉換器會剖析命令並產生執行階段圖形表示法。 這個表示法基本上是一組可供呈現的已完成向量。 向量本身並不會完成呈現詳細資料；您仍然需要設定元素的其他值。 以 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 物件來說，您還需要 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill)、[**Stroke**](/uwp/api/windows.ui.xaml.shapes.shape.stroke) 及其他屬性的值，然後該 **Path** 必須以某種方式連接到視覺化樹狀結構。 針對 [**PathIcon**](/uwp/api/Windows.UI.Xaml.Controls.PathIcon) 物件，請設定 [**Foreground**](/uwp/api/windows.ui.xaml.controls.iconelement.foreground) 屬性。

Windows 執行階段有兩個屬性可以使用代表移動與繪製命令的字串：[**Path.Data**](/uwp/api/windows.ui.xaml.shapes.path.data) 與 [**PathIcon.Data**](/uwp/api/windows.ui.xaml.controls.pathicon.data)。 如果藉由指定移動與繪製命令來設定其中一個屬性，您通常會將它設為 XAML 屬性值以及該元素的其他必要屬性。 在不探討內容的情況下，它看起來就像以下這樣：

```xml
<Path x:Name="Arrow" Fill="White" Height="11" Width="9.67"
  Data="M4.12,0 L9.67,5.47 L4.12,10.94 L0,10.88 L5.56,5.47 L0,0.06" />
```

## <a name="using-move-and-draw-commands-versus-using-a-pathgeometry"></a>使用移動與複製命令與使用 **PathGeometry** 比較

對 Windows 執行階段 XAML 而言，移動與繪製命令會產生一個含有單一 [**PathFigure**](/uwp/api/Windows.UI.Xaml.Media.PathFigure) 物件搭配 [**Figures**](/uwp/api/windows.ui.xaml.media.pathgeometry.figures) 屬性值的 [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry)。 每個繪製命令都會在該單一 **PathFigure** 的 [**Segments**](/uwp/api/windows.ui.xaml.media.pathfigure.segments) 集合中產生一個 [**PathSegment**](/uwp/api/Windows.UI.Xaml.Media.PathSegment) 衍生類別，移動命令會變更 [**StartPoint**](/uwp/api/windows.ui.xaml.media.pathfigure.startpoint)，如果有關閉命令，則會將 [**IsClosed**](/uwp/api/windows.ui.xaml.media.pathfigure.isclosed) 設定為 **true**。 如果您在執行階段檢查 **Data** 值，可以將這個結構當做物件模型來瀏覽。

## <a name="the-basic-syntax"></a>基本語法

移動和繪製命令的語法摘要說明如下：

1.  從選用的填滿規則開始。 通常只有在不想要 **EvenOdd** 預設值時，才會指定這個選項。 (稍後將進一步說明 **EvenOdd**)。
2.  明確指定一個移動命令。
3.  指定一或多個繪製命令。
4.  指定一個關閉命令。 您可以省略關閉命令，但會讓圖形保持開放 (這並不常見)。

這個語法的一般規則是：

-   每個命令只由一個字母代表。
-   字母可以是大寫或小寫。 區分大小寫，我們將會另外說明。
-   除了關閉命令以外，每個命令後面通常會跟著一或多個數字。
-   如果命令有一個以上的數字，請使用逗號或空格來分隔。

**\[**_fillRule_ **\]**_moveCommand_ _drawCommand_ **\[** _drawCommand_ **\*\]** **\[** _closeCommand_**\]**

許多繪製命令會使用點，由您提供 _x,y_ 值。 每當您看到 \* _點_ 預留位置時，您可以假設您為點的 _x，y_ 值提供兩個十進位值。

當結果相當明確時，通常可以省略空白字元。 如果您使用逗號做為所有數字集 (點與大小) 的分隔符號，則實際上可以省略所有空白字元。 例如，下列是合法用法：`F1M0,58L2,56L6,60L13,51L15,53L6,64z`。 但是為了清楚表示，在命令之間加入空白字元是較常見的做法。

請勿使用逗號做為十進位數字的小數點；命令字串是由 XAML 解譯，而且不負責轉換與 **en-us** 地區設定中所用的不同文化特性數字格式。

## <a name="syntax-specifics"></a>語法內容

**填滿規則**

選用的填滿規則有兩個可能值：**F0** 或 **F1**。 (**F** 一律為大寫) **F0** 是預設值；它會產生 **EvenOdd** 填滿行為，因此您通常不會指定。 使用 **F1** 可以取得 **Nonzero** 填滿行為。 這些填滿值與 [**FillRule**](/uwp/api/Windows.UI.Xaml.Media.FillRule) 列舉的值一致。

**移動命令**

指定新圖表的起點。

| Syntax |
|--------|
| `M `_startPoint_ <br/>- 或 -<br/>`m`_startPoint_|

| 詞彙 | 描述 |
|------|-------------|
| _startPoint_ | [**Point**](/uwp/api/Windows.Foundation.Point) <br/>新圖表的起點。|

大寫 **M** 表示 *startPoint* 是絕對座標；小寫 **m** 表示 *startPoint* 是前一個點的位移，如果沒有前一個點，則為 (0,0)。

**注意：** 移動命令後可以指定多個點。 就像指定了線條命令一樣，會畫出連接到這些點的一條線。 不過，不建議採用這種樣式；請改用專用的線條命令。

**繪製命令**

繪製命令可由數個形狀命令組成：線條、水平線、垂直線、三次方貝茲曲線、二次方貝茲曲線、平滑的三次方貝茲曲線、平滑的二次方貝茲曲線以及橢圓形弧線。

所有繪製命令都區分大小寫。 大寫字母表示絕對座標，小寫字母表示與前一個命令相對的座標。

線段的控制點是相對於前面線段的終點。 依序輸入多個相同類型的命令時，可以省略重複的命令項目。 例如，`L 100,200 300,400` 相當於 `L 100,200 L 300,400`。

**Line 命令**

在目前的點和指定的點之間建立直線。 `l 20 30` 和 `L 20,30` 都是有效行命令的範例。 定義 [**LineGeometry**](/uwp/api/Windows.UI.Xaml.Media.LineGeometry) 物件的對等物件。

| Syntax |
|--------|
| `L`_端點_ <br/>- 或 -<br/>`l`_端點_ |

| 詞彙 | 描述 |
|------|-------------|
| endPoint | [**Point**](/uwp/api/Windows.Foundation.Point)<br/>行的結束點。|

**水平線命令**

在目前的點和指定的 X 座標之間建立水平線。 `H 90` 為有效水平線命令的範例。

| Syntax |
|--------|
| `H ` _x_ <br/> - 或 - <br/>`h ` _x_ |

| 詞彙 | 描述 |
|------|-------------|
| x | [**雙**](/dotnet/api/system.double) <br/> 線條結束點的 X 座標。 |

**垂直線命令**

在目前的點和指定的 Y 座標之間建立垂直線。 `v 90` 為有效垂直線命令的範例。

| Syntax |
|--------|
| `V ` _y_ <br/> - 或 - <br/> `v ` _y_ |

| 詞彙 | 描述 |
|------|-------------|
| *y* | [**雙**](/dotnet/api/system.double) <br/> 線條結束點的 Y 座標。 |

**三次方貝茲曲線命令**

使用兩個指定的控制點 (*controlPoint1* 與 *controlPoint2*) 在目前的點與指定的終點之間建立一條三次方貝茲曲線。 `C 100,200 200,400 300,200` 為有效曲線命令的範例。 以 [**BezierSegment**](/uwp/api/Windows.UI.Xaml.Media.BezierSegment) 物件定義 [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) 物件的對等物件。

| Syntax |
|--------|
| `C ` *controlPoint1* *controlPoint2* *endPoint* <br/> - 或 - <br/> `c ` *controlPoint1* *controlPoint2* *endPoint* |

| 詞彙 | 描述 |
|------|-------------|
| *controlPoint1* | [**Point**](/uwp/api/Windows.Foundation.Point) <br/> 曲線的第一個控制點，它能決定曲線的起始正切函數。 |
| *controlPoint2* | [**Point**](/uwp/api/Windows.Foundation.Point) <br/> 曲線的第二個控制點，它能決定曲線的結束正切函數。 |
| *端點* | [**Point**](/uwp/api/Windows.Foundation.Point) <br/> 曲線要繪製到的點。 |

**二次方貝茲曲線命令**

使用指定的控制點 (*controlPoint*) 在目前的點與指定的終點之間建立一條二次方貝茲曲線。 `q 100,200 300,200` 是有效二次方貝茲曲線命令的範例。 以 [**QuadraticBezierSegment**](/uwp/api/Windows.UI.Xaml.Media.QuadraticBezierSegment) 定義 [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) 的對等物件。

| Syntax |
|--------|
| `Q ` *controlPoint endPoint* <br/> - 或 - <br/> `q ` *controlPoint endPoint* |

| 詞彙 | 描述 |
|------|-------------|
| *controlPoint* | [**Point**](/uwp/api/Windows.Foundation.Point) <br/> 曲線的控制點，它能決定曲線的起始正切函數和結束正切函數。 |
| *端點* | [**Point**](/uwp/api/Windows.Foundation.Point)<br/> 曲線要繪製到的點。 |

**平滑的三次方貝茲曲線命令**

在目前的點與指定的終點之間建立一條三次方貝茲曲線。 第一個控制點會假設為 (相對於目前的點) 上一個命令之第二個控制點的反映。 如果沒有前一個命令，或者前一個命令不是三次方貝茲曲線命令或平滑的三次方貝茲曲線命令，則會假設第一個控制點就是目前的點。 第二個控制點 (曲線終點的控制點) 是由 *controlPoint2* 指定。 例如，`S 100,200 200,300` 是一個有效的平滑三次方貝茲曲線命令。 這個命令是以前面曲線線段的 [**BezierSegment**](/uwp/api/Windows.UI.Xaml.Media.BezierSegment) 定義 [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) 的對等物件。

| Syntax |
|--------|
| `S` *controlPoint2* *endPoint* <br/> - 或 - <br/>`s` *controlPoint2 endPoint* |

| 詞彙 | 描述 |
|------|-------------|
| *controlPoint2* | [**Point**](/uwp/api/Windows.Foundation.Point) <br/> 曲線的控制點，它能決定曲線的結束正切函數。 |
| *端點* | [**Point**](/uwp/api/Windows.Foundation.Point)<br/> 曲線要繪製到的點。 |

**平滑的二次方貝茲曲線命令**

在目前的點與指定的終點之間建立一條二次方貝茲曲線。 控制點會假設為 (相對於目前的點) 上一個命令之控制點的反映。 如果沒有前一個命令，或者前一個命令不是二次方貝茲曲線命令或平滑的二次方貝茲曲線命令，則控制點就是目前的點。 這個命令是以前面曲線線段的 [**QuadraticBezierSegment**](/uwp/api/Windows.UI.Xaml.Media.QuadraticBezierSegment) 定義 [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) 的對等物件。

| Syntax |
|--------|
| `T` *controlPoint* *endPoint* <br/> - 或 - <br/> `t` *controlPoint* *endPoint* |

| 詞彙 | 描述 |
|------|-------------|
| *controlPoint* | [**Point**](/uwp/api/Windows.Foundation.Point)<br/> 曲線的控制點，它能決定曲線的起始正切函數。 |
| *端點* | [**Point**](/uwp/api/Windows.Foundation.Point)<br/> 曲線要繪製到的點。 |

**橢圓形弧線命令**

在目前的點和指定的結束點之間建立橢圓形弧線。 以 [**ArcSegment**](/uwp/api/Windows.UI.Xaml.Media.ArcSegment) 定義 [**PathGeometry**](/uwp/api/Windows.UI.Xaml.Media.PathGeometry) 的對等物件。

| Syntax |
|--------|
| `A ` *size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endPoint* <br/> - 或 - <br/>`a ` *size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endPoint* |

| 詞彙 | 描述 |
|------|-------------|
| *size* | [**大小**](/uwp/api/Windows.Foundation.Size)<br/>弧線的 x 軸半徑與 y 軸半徑。 |
| *rotationAngle* | [**雙**](/dotnet/api/system.double) <br/> 橢圓形的旋轉 (以角度為單位)。 |
| *isLargeArcFlag* | 如果弧線的角度應該為 180 度或大於 180 度，則設定為 1；否則，請設定為 0。 |
| *sweepDirectionFlag* | 如果弧線是以正值角度的方向繪製，則設定為 1；否則，請設定為 0。 |
| *端點* | [**Point**](/uwp/api/Windows.Foundation.Point) <br/> 弧線要繪製到的點。 |

**關閉命令**

結束目前的圖表，並建立連接目前的點和圖表起點的線條。 此命令會在圖表的最後一個區段和第一個區段之間建立線條聯結 (邊角)。

| Syntax |
|--------|
| `Z` <br/> - 或 - <br/> `z ` |

**點語法**

描述點的 x 座標與 y 座標。 另請參閱 [**Point**](/uwp/api/Windows.Foundation.Point)。

| Syntax |
|--------|
| *x*、*y*<br/> - 或 - <br/>*x* *y* |

| 詞彙 | 描述 |
|------|-------------|
| *x* | [**雙**](/dotnet/api/system.double) <br/> 點的 X 座標。 |
| *y* | [**雙**](/dotnet/api/system.double) <br/> 點的 Y 座標。 |

**其他注意事項**

除了標準數值，您也可以使用下列特殊值。 這些值區分大小寫。

-   **Infinity**：代表 **PositiveInfinity**。
-   **\- 無限大**：代表 **NegativeInfinity**。
-   **NaN**：代表 **NaN**。

您可以使用科學記號標記法代替使用小數或整數。 例如，`+1.e17` 是有效的值。

## <a name="design-tools-that-produce-move-and-draw-commands"></a>產生移動與繪製命令的設計工具

使用 Blend for Microsoft Visual Studio 2015 中的 **\[畫筆\]** 工具及其他繪圖工具通常會產生含有移動與繪製命令的 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 物件。

您會在 Windows 執行階段 XAML 控制項預設範本所定義的一些控制項組件中，看到現有的移動與繪製命令資料。 例如，有些控制項使用將資料定義為移動與繪製命令的 [**PathIcon**](/uwp/api/Windows.UI.Xaml.Controls.PathIcon)。

其他可將向量以 XAML 形式輸出的常用向量圖形設計工具有可用的匯出工具或外掛程式。 這些通常會在配置容器中建立含有 [**Path.Data**](/uwp/api/windows.ui.xaml.shapes.path.data) 之移動與繪製命令的 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 物件。 XAML 中可能會有多個 **Path** 元素，以便可以套用不同的筆刷。 許多這類匯出工具或外掛程式原先是為 Windows Presentation Foundation (WPF) XAML 或 Silverlight 撰寫的，但是該 XAML 路徑語法與 Windows 執行階段 XAML 相同。 通常，您可以使用匯出工具的 XAML 區塊，將它們貼到 Windows 執行階段 XAML 頁面中。 (不過，如果 **RadialGradientBrush** 是已轉換之 XAML 的一部分，您就無法加以使用，因為 Windows 執行階段 XAML 不支援該筆刷)。

## <a name="related-topics"></a>相關主題

* [繪製圖案](../design/controls-and-patterns/shapes.md)
* [使用筆刷](../design/style/brushes.md)
* [**Path.Data**](/uwp/api/windows.ui.xaml.shapes.path.data)
* [**PathIcon**](/uwp/api/Windows.UI.Xaml.Controls.PathIcon)
