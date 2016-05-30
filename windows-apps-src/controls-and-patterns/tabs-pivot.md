---
author: Jwmsft
Description: 索引標籤和樞紐讓使用者可以在經常存取的內容之間瀏覽。
title: 索引標籤與樞紐
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: Tabs and pivots
template: detail.hbs
---
# 樞紐和索引標籤

樞紐和索引標籤模式適用於瀏覽經常存取、不同的內容類別。 樞紐和索引標籤是由兩個以上、具有對應類別標頭的內容窗格所組成。 標頭會保留在螢幕上，而且具有清楚顯示的選取狀態，使用者能夠清楚知道他們所在的類別。
![索引標籤範例](images/HIGSecOne_Tabs.png)

索引標籤是樞紐的視覺變體，且使用 [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) 控制項來建置。 在 GitHub 上提供的[**程式碼範例**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlPivot)示範如何自訂樞紐。

<span class="sidebar_heading" style="font-weight: bold;">重要 API</span>

-   [**Pivot 類別**](https://msdn.microsoft.com/library/windows/apps/dn608241)

## 索引標籤/樞紐模式

當建置包含索引標籤/樞紐模式的 app 時，有一些需要考量的主要設計變數。

- **標頭標籤。**  標頭可以具有包含文字的圖示或僅文字。
- **標頭對齊。**  標頭可以靠左對齊或置中。
- **最上層或子層瀏覽。**  這兩種層級的瀏覽都可以使用索引標籤/樞紐。 也可以選擇將[瀏覽窗格](nav-pane.md)做為主要層級，而索引標籤/樞紐為次要。
- **觸控手勢支援。**  對於支援觸控手勢的裝置，您可以使用兩個互動集合的其中一個在內容類別之間瀏覽：
    1. 點選索引標籤/樞紐標頭以瀏覽至該類別。
    2. 在內容區域上向左或向右撥動以瀏覽至相鄰的類別。

## 範例

Cortana Reminders 中預設的樞紐控制項。

![Cortana Reminders 中的樞紐範例](images/pivot_cortana-reminders.png)

「鬧鐘與時鐘」app 中的索引標籤模式。

![「鬧鐘與時鐘」中的索引標籤範例](images/tabs_alarms-and-clock.png)

## 建立 Pivot 控制項

[
            **Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) 控制項內建了這一節中所述的基本功能。

這段 XAML 會建立包含 3 個內容區段的基本 Pivot 控制項。

```xaml
<Pivot x:Name="rootPivot" Title="Pivot Title">
    <PivotItem Header="Pivot Item 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 1."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 2."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 3."/>
    </PivotItem>
</Pivot>
```

### Pivot 項目

Pivot 是一種 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx)，因此可以包含任何型別的項目集合。 所有新增到 Pivot 且不是明確為 [**PivotItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) 的項目，都會隱含地包裝在 PivotItem 中。 因為 Pivot 通常是用來在頁面內容之間瀏覽，所以通常會直接使用 XAML UI 元素填入 [**Items**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合。 或者，您可以將 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 屬性設為資料來源。 ItemsSource 中繫結的項目可以是任何型別，但如果它們不是明確為 PivotItems，則您必須定義 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 和 [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) 來指定如何顯示這些項目。

您可以使用 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) 屬性擷取或設定選取的項目。 使用 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) 屬性擷取或設定選取的項目。

### 樞紐標頭

您可以使用 [**LeftHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) 和 [**RightHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) 屬性來將其他控制項新增到樞紐標頭。

### 樞紐互動

以下是該控制項主要的觸控手勢互動：

-   點選樞紐項目標頭可瀏覽到該標頭的區段內容。
-   在樞紐項目標頭上向左或向右撥動可瀏覽到相鄰的區段。
-   在區段內容上向左或向右撥動可瀏覽到相鄰的區段。

該控制項有兩種模式：

**靜止**

-   當所有的樞紐標頭大小符合允許的空間時，樞紐會靜止。
-   雖然樞紐本身不會移動，但點選樞紐標籤會瀏覽到對應的頁面。 使用中的樞紐會反白顯示。

**浮動切換**

-   當所有的樞紐標頭大小不符合允許的空間時，樞紐會浮動切換。
-   點選樞紐標籤會瀏覽到對應的頁面，且使用中的樞紐標籤會浮動切換到第一個位置。
-   各樞紐項目會浮動循環切換，從最後一個接到第一個樞紐區段。

## 建議事項

-   根據螢幕大小對齊索引標籤/樞紐標頭。 對於寬度小於 720 epx 的螢幕，置中對齊通常比較好，而大多數情況是寬度大於 720 epx 的螢幕建議使用靠左對齊。
-   當使用浮動切換 (反覆) 模式時請避免使用超過 5 個標頭，因為循環超過 5 個標頭可能會混淆使用者。
-   當您的樞紐項目有個別的圖示時才使用索引標籤模式。
-   在樞紐項目標頭包含文字，以幫助使用者了解每個樞紐區段的意義。 圖示並非對於所有使用者都一目了然。



## 相關主題

[瀏覽設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958438)


<!--HONumber=May16_HO2-->


