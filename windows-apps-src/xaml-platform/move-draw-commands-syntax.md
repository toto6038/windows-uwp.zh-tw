---
description: 了解可用來指定路徑幾何做為 XAML 屬性值的移動與繪製命令 (一種迷你程式語言)。
title: 移動與繪製命令語法
ms.assetid: 7772BC3E-A631-46FF-9940-3DD5B9D0E0D9
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 604ad25bb65486b3b388a9a03d7503b0c1ce9c03
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8214905"
---
# <a name="move-and-draw-commands-syntax"></a>移動與繪製命令語法


了解可用來指定路徑幾何做為 XAML 屬性值的移動與繪製命令 (一種迷你程式語言)。 許多可以輸出向量圖形或形狀 (以序列化或交換格式) 的設計與圖形工具都會使用移動與繪製命令。

## <a name="properties-that-use-move-and-draw-command-strings"></a>使用移動與繪製命令字串的屬性

移動與繪製命令語法受到 XAML 的內部類型轉換器支援，這個轉換器會剖析命令並產生執行階段圖形表示法。 這個表示法基本上是一組可供呈現的已完成向量。 向量本身並不會完成呈現詳細資料；您仍然需要設定元素的其他值。 以 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 物件來說，您還需要 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill)、[**Stroke**](https://msdn.microsoft.com/library/windows/apps/br243383) 及其他屬性的值，然後該 **Path** 必須以某種方式連接到視覺化樹狀結構。 針對 [**PathIcon**](https://msdn.microsoft.com/library/windows/apps/dn252722) 物件，請設定 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/dn251974) 屬性。

Windows 執行階段有兩個屬性可以使用代表移動與繪製命令的字串：[**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356) 與 [**PathIcon.Data**](https://msdn.microsoft.com/library/windows/apps/dn252723)。 如果藉由指定移動與繪製命令來設定其中一個屬性，您通常會將它設為 XAML 屬性值以及該元素的其他必要屬性。 在不探討內容的情況下，它看起來就像以下這樣：

```xml
<Path x:Name="Arrow" Fill="White" Height="11" Width="9.67"
  Data="M4.12,0 L9.67,5.47 L4.12,10.94 L0,10.88 L5.56,5.47 L0,0.06" />
```

[**PathGeometry.Figures**](https://msdn.microsoft.com/library/windows/apps/br210169) 也可以使用移動與繪製命令。 您可以將使用移動與繪製命令的 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) 物件與 [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/br210057) 物件中的其他 [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) 類型結合，然後用來做為 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356) 的值。 但是相較於將移動與繪製命令用於屬性定義的資料，這並不常見。

## <a name="using-move-and-draw-commands-versus-using-a-pathgeometry"></a>使用移動與複製命令與使用 **PathGeometry** 比較

對 Windows 執行階段 XAML 而言，移動與繪製命令會產生一個含有單一 [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/br210143) 物件搭配 [**Figures**](https://msdn.microsoft.com/library/windows/apps/br210169) 屬性值的 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168)。 每個繪製命令都會在該單一 **PathFigure** 的 [**Segments**](https://msdn.microsoft.com/library/windows/apps/br210164) 集合中產生一個 [**PathSegment**](https://msdn.microsoft.com/library/windows/apps/br210174) 衍生類別，移動命令會變更 [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/br210166)，如果有關閉命令，則會將 [**IsClosed**](https://msdn.microsoft.com/library/windows/apps/br210159) 設定為 **true**。 如果您在執行階段檢查 **Data** 值，可以將這個結構當做物件模型來瀏覽。

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

**\[**_fillRule_**\]** _moveCommand_ _drawCommand_ **\[**_drawCommand_**\*\]** **\[**_closeCommand_**\]**

許多繪製命令會使用點，由您提供 _x,y_ 值。 每當您看到 \*_points_ 預留位置時，都可以假設是要為一個點的 _x,y_ 值提供兩個十進位值。

當結果相當明確時，通常可以省略空白字元。 如果您使用逗號做為所有數字集 (點與大小) 的分隔符號，則實際上可以省略所有空白字元。 例如，下列是合法用法：`F1M0,58L2,56L6,60L13,51L15,53L6,64z`。 但是為了清楚表示，在命令之間加入空白字元是較常見的做法。

請勿使用逗號做為十進位數字的小數點；命令字串是由 XAML 解譯，而且不負責轉換與 **en-us** 地區設定中所用的不同文化特性數字格式。

## <a name="syntax-specifics"></a>語法內容

**填滿規則**

選用的填滿規則有兩個可能值：**F0** 或 **F1**。 (**F** 一律為大寫) **F0** 是預設值；它會產生 **EvenOdd** 填滿行為，因此您通常不會指定。 使用 **F1** 可以取得 **Nonzero** 填滿行為。 這些填滿值與 [**FillRule**](https://msdn.microsoft.com/library/windows/apps/br210030) 列舉的值一致。

**移動命令**

指定新圖形的起點。

| 語法 |
|--------|
| `M ` _startPoint_ <br/>- 或 -<br/>`m` _startPoint_|

| 詞彙 | 說明 |
|------|-------------|
| _startPoint_ | [**點**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/>新圖形的起點。|

大寫 **M** 表示 *startPoint* 是絕對座標；小寫 **m** 表示 *startPoint* 是前一個點的位移，如果沒有前一個點，則為 (0,0)。

**注意：** 是合法移動命令後可以指定多個點。 就像指定了線條命令一樣，會畫出連接到這些點的一條線。 不過，不建議採用這種樣式；請改用專用的線條命令。

**繪製命令**

繪製命令可由數個形狀命令組成：線條、水平線、垂直線、三次方貝茲曲線、二次方貝茲曲線、平滑的三次方貝茲曲線、平滑的二次方貝茲曲線以及橢圓形弧線。

所有繪製命令都區分大小寫。 大寫字母表示絕對座標，小寫字母表示與前一個命令相對的座標。

線段的控制點是相對於前面線段的終點。 依序輸入多個相同類型的命令時，可以省略重複的命令項目。 例如，`L 100,200 300,400` 等同於 `L 100,200 L 300,400`。

**線條命令**

在目前的點與指定的終點之間建立一條直線。 `l 20 30` 與 `L 20,30` 都是有效的線條命令範例。 定義 [**LineGeometry**](https://msdn.microsoft.com/library/windows/apps/br210117) 物件的對等物件。

| 語法 |
|--------|
| `L` _endPoint_ <br/>- 或 -<br/>`l` _endPoint_ |

| 詞彙 | 說明 |
|------|-------------|
| endPoint | [**點**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/>線條的終點。|

**水平線命令**

在目前的點與指定的 x 座標之間建立一條水平線。 `H 90` 是有效的水平線命令範例。

| 語法 |
|--------|
| `H ` _x_ <br/> - 或 - <br/>`h ` _x_ |

| 詞彙 | 說明 |
|------|-------------|
| x | [**雙線**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> 線條終點的 x 座標。 |

**垂直線命令**

在目前的點與指定的 y 座標之間建立一條垂直線。 `v 90` 是有效的垂直線命令範例。

| 語法 |
|--------|
| `V ` _y_ <br/> - 或 - <br/> `v ` _y_ |

| 詞彙 | 說明 |
|------|-------------|
| *y* | [**雙線**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> 線條終點的 y 座標。 |

**三次方貝茲曲線命令**

使用兩個指定的控制點 (*controlPoint1* 與 *controlPoint2*) 在目前的點與指定的終點之間建立一條三次方貝茲曲線。 `C 100,200 200,400 300,200` 是有效的曲線命令範例。 以 [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/br228068) 物件定義 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) 物件的對等物件。

| 語法 |
|--------|
| `C ` *controlPoint1* *controlPoint2* *endPoint* <br/> - 或 - <br/> `c ` *controlPoint1* *controlPoint2* *endPoint* |

| 詞彙 | 說明 |
|------|-------------|
| *controlPoint1* | [**點**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> 曲線的第一個控制點，決定曲線的起點切線。 |
| *controlPoint2* | [**點**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> 曲線的第二個控制點，決定曲線的終點切線。 |
| *endPoint* | [**點**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> 曲線將繪製到的點。 | 

**二次方貝茲曲線命令**

使用指定的控制點 (*controlPoint*) 在目前的點與指定的終點之間建立一條二次方貝茲曲線。 `q 100,200 300,200` 是有效的二次方貝茲曲線命令範例。 以 [**QuadraticBezierSegment**](https://msdn.microsoft.com/library/windows/apps/br210249) 定義 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) 的對等物件。

| 語法 |
|--------|
| `Q ` *controlPoint endPoint* <br/> - 或 - <br/> `q ` *controlPoint endPoint* |

| 詞彙 | 說明 |
|------|-------------|
| *controlPoint* | [**點**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> 曲線的控制點，決定曲線的起點與終點切線。 |
| *endPoint* | [**點**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> 曲線將繪製到的點。 |

**平滑的三次方貝茲曲線命令**

在目前的點與指定的終點之間建立一條三次方貝茲曲線。 第一個控制點假設為前一個命令之第二個控制點相對於目前點的反射。 如果沒有前一個命令，或者前一個命令不是三次方貝茲曲線命令或平滑的三次方貝茲曲線命令，則會假設第一個控制點就是目前的點。 第二個控制點 (曲線終點的控制點) 是由 *controlPoint2* 指定。 例如，`S 100,200 200,300` 是一個有效的平滑三次方貝茲曲線命令。 這個命令是以前面曲線線段的 [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/br228068) 定義 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) 的對等物件。

| 語法 |
|--------|
| `S` *controlPoint2* *endPoint* <br/> - 或 - <br/>`s` *controlPoint2 endPoint* |

| 詞彙 | 說明 |
|------|-------------|
| *controlPoint2* | [**點**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> 曲線的控制點，決定曲線的終點切線。 |
| *endPoint* | [**點**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> 曲線將繪製到的點。 |

**平滑的二次方貝茲曲線命令**

在目前的點與指定的終點之間建立一條二次方貝茲曲線。 控制點假設為前一個命令之控制點相對於目前點的反射。 如果沒有前一個命令，或者前一個命令不是二次方貝茲曲線命令或平滑的二次方貝茲曲線命令，則控制點就是目前的點。 這個命令是以前面曲線線段的 [**QuadraticBezierSegment**](https://msdn.microsoft.com/library/windows/apps/br210249) 定義 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) 的對等物件。

| 語法 |
|--------|
| `T` *controlPoint* *endPoint* <br/> - 或 - <br/> `t` *controlPoint* *endPoint* |

| 詞彙 | 說明 |
|------|-------------|
| *controlPoint* | [**點**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> 曲線的控制點，決定曲線的起點切線。 |
| *endPoint* | [**點**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> 曲線將繪製到的點。 |

**橢圓形弧線命令**

在目前的點與指定的終點之間建立一條橢圓形弧線。 以 [**ArcSegment**](https://msdn.microsoft.com/library/windows/apps/br228054) 定義 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) 的對等物件。

| 語法 |
|--------|
| `A ` *size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endPoint* <br/> - 或 - <br/>`a ` *sizerotationAngleisLargeArcFlagsweepDirectionFlagendPoint* |

| 詞彙 | 說明 |
|------|-------------|
| *size* | [**大小**](https://msdn.microsoft.com/library/windows/apps/br225995)<br/>弧線的 x 軸半徑與 y 軸半徑。 |
| *rotationAngle* | [**雙線**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> 橢圓形的旋轉度數。 |
| *isLargeArcFlag* | 如果弧線的角度應該等於或大於 180 度，則設定為 1；否則設定為 0。 |
| *sweepDirectionFlag* | 如果是以正角方向繪製弧線，則設定為 1；否則設定為 0。 |
| *endPoint* | [**點**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> 弧線將繪製到的點。|
 
**關閉命令**

結束目前的圖形，並建立一條將目前點連接至圖形起點的線。 這個命令會在圖形的最後一個線段與第一個線段之間建立一個線條聯結 (邊角)。

| 語法 |
|--------|
| `Z` <br/> - 或 - <br/> `z ` |

**點語法**

描述點的 x 座標與 y 座標。 另請參閱 [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)。

| 語法 |
|--------|
| *x*,*y*<br/> - 或 - <br/>*x**y* |

| 詞彙 | 說明 |
|------|-------------|
| *x* | [**雙線**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> 點的 x 座標。 |
| *y* | [**雙線**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> 點的 y 座標。 |

**其他附註**

您也可以使用下列特殊值來代替標準數值。 這些值區分大小寫。

-   **Infinity**：代表 **PositiveInfinity**。
-   **\-Infinity**：代表 **NegativeInfinity**。
-   **NaN**：代表 **NaN**。

您可以使用科學記號標記法代替使用小數或整數。 例如，`+1.e17` 是有效的值。

## <a name="design-tools-that-produce-move-and-draw-commands"></a>產生移動與繪製命令的設計工具

Blend for Microsoft Visual Studio2015 中使用**手寫筆**工具及其他繪圖工具將通常會產生[**路徑**](/uwp/api/Windows.UI.Xaml.Shapes.Path)物件，與移動，與繪製命令。

您會在 Windows 執行階段 XAML 控制項預設範本所定義的一些控制項組件中，看到現有的移動與繪製命令資料。 例如，有些控制項使用將資料定義為移動與繪製命令的 [**PathIcon**](https://msdn.microsoft.com/library/windows/apps/dn252722)。

其他可將向量以 XAML 形式輸出的常用向量圖形設計工具有可用的匯出工具或外掛程式。 這些通常會在配置容器中建立含有 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356) 之移動與繪製命令的 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 物件。 XAML 中可能會有多個 **Path** 元素，以便可以套用不同的筆刷。 許多這類匯出工具或外掛程式原先是為 Windows Presentation Foundation (WPF) XAML 或 Silverlight 撰寫的，但是該 XAML 路徑語法與 Windows 執行階段 XAML 相同。 通常，您可以使用匯出工具的 XAML 區塊，將它們貼到 Windows 執行階段 XAML 頁面中。 (不過，如果 **RadialGradientBrush** 是已轉換之 XAML 的一部分，您就無法加以使用，因為 Windows 執行階段 XAML 不支援該筆刷)。

## <a name="related-topics"></a>相關主題

* [繪製形狀](https://msdn.microsoft.com/library/windows/apps/mt280380)
* [使用筆刷](https://msdn.microsoft.com/library/windows/apps/mt280383)
* [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356)
* [**PathIcon**](https://msdn.microsoft.com/library/windows/apps/dn252722)

