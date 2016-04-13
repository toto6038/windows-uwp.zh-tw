---
索引標籤和樞紐讓使用者可以在經常存取的內容之間瀏覽。
索引標籤與樞紐
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
索引標籤與樞紐
template: detail.hbs
---
# 索引標籤與樞紐

索引標籤和樞紐適用於瀏覽經常存取、不同的內容類別。 索引標籤/樞紐模式是由兩個以上、具有對應類別標頭的內容窗格所組成。 標頭會保留在螢幕上，而且具有清楚顯示的選取狀態，使用者能夠清楚知道他們所在的類別。
![索引標籤範例](images/HIGSecOne_Tabs.png)

索引標籤和樞紐為相同有效的模式，兩者都是使用 [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) 控制項所建置。 稍後於本文中會描述 Pivot 控制項的基本功能。

<span class="sidebar_heading" style="font-weight: bold;">重要 API</span>

-   [**Pivot 類別**](https://msdn.microsoft.com/library/windows/apps/dn608241)

## 索引標籤/樞紐模式

當建置包含索引標籤/樞紐模式的 app 時，根據模式的可設定功能集，有一些需要考量的主要設計變數。

- **放置標頭。**   標頭可以放置於畫面頂端或底部。
    
    **注意**&nbsp;&nbsp;將標頭放置在畫面底部需要重新調整 Pivot 控制向。
- **標頭標籤。**  標頭可以具有包含文字的圖示、僅文字，或僅圖示。
- **標頭對齊。**  標頭可以靠左對齊或置中。
- **最上層或子層瀏覽。**  索引標籤/樞紐適用於其中一種層級瀏覽，且可在最上層/子層模式中堆疊。 當有兩種層級的索引標籤/樞紐時，最上層和子層標頭應該要有足夠的視覺差異，讓使用者能夠清楚區別。
- **觸控手勢支援。**  對於支援觸控手勢的裝置，您可以使用兩個互動集合的其中一個在內容類別之間瀏覽：
    1. 點選索引標籤/樞紐標頭瀏覽至該類別，或在內容區域上撥動以瀏覽至相鄰的類別。
    2. 點選索引標籤/樞紐標頭以瀏覽至該類別。 (無撥動)。

### 模式設定

索引標籤/樞紐模式的最佳排列方式取決於互動案例與將會顯示您應用程式的裝置。 下表概述一些熱門的案例和模式設定。

互動案例|建議的設定
--------------------|-------------------------
在 2 到 5 個最上層清單之間，或是手機或平板手機上的格線檢視內容類別之間橫向移動。|索引標籤/樞紐：放在螢幕頂端，置中對齊
|標頭標籤：圖示 + 文字
|在內容區域上撥動：啟用
在手機或平板手機 (其中於內容區域上撥動不適合瀏覽) 的內容類別範圍之間移動|索引標籤/樞紐：放在螢幕底部，置中對齊
|標頭標籤：圖示 + 文字
|在內容區域上撥動：停用
使用滑鼠和鍵盤的最上層瀏覽|索引標籤/樞紐：放在螢幕頂端，靠左對齊
 *或*|標頭標籤：僅文字
 觸控裝置上的頁面層級瀏覽|在內容區域上撥動：停用

## 範例

這個快餐車應用程式的設計顯示將索引標籤/樞紐放置在頂端或底部時看起來的樣子。 在行動裝置上，將索引標籤放在底部有助於方便存取。

![行動裝置上索引標籤的範例](images/uap_foodtruck_phone_320_tabsboth.png)

膝上型電腦/桌上型電腦上的快餐車應用程式設計強調僅文字標頭。 針對標頭使用包含文字的圖示可協助觸控目標預測，但是對滑鼠與鍵盤來說，僅文字標頭的運作較好。

![桌上型電腦上索引標籤的範例](images/uap_foodtruck_desktop_home_700.png)

## 建立 Pivot 控制項

索引標籤/樞紐瀏覽模式是使用 [**Pivot**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) 控制項所建置。 該控制項內建了這一節中所述的基本功能。

這段 XAML 會建立包含 3 個內容區段的基本 Pivot 控制項。

```xaml
<Pivot x:Name="rootPivot" Title="PIVOT TITLE">
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

**Pivot 項目**

Pivot 是一種 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx)，因此可以包含任何類型的項目集合。 所有新增到 Pivot 且不是明確為 [**PivotItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) 的項目，都會隱含地包裝在 PivotItem 中。 因為 Pivot 通常是用來在頁面內容之間瀏覽，所以通常會直接使用 XAML UI 元素填入 [**Items**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合。 或者，您可以將 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 屬性設為資料來源。 ItemsSource 中繫結的項目可以是任何型別，但如果它們不是明確為 PivotItems，則您必須定義 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 和 [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) 來指定如何顯示這些項目。

您可以使用 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) 屬性擷取或設定選取的項目。 使用 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) 屬性擷取或設定選取的項目。 

**樞紐標頭**

您可以使用 [**LeftHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) 和 [**RightHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) 屬性來將其他控制項新增到樞紐標頭。 

### 樞紐互動

以下是該控制項主要的觸控手勢互動：

-   點選標頭可瀏覽到該標頭的區段內容。
-   在標頭上向左或向右撥動可瀏覽到相鄰的標頭/區段。
-   在區段內容上向左或向右撥動可瀏覽到相鄰的標頭/區段。

該控制項有兩種模式：

**靜止**

-   當所有的樞紐標頭大小符合允許的空間時，樞紐會靜止。
-   雖然樞紐本身不會移動，但點選樞紐標籤會瀏覽到對應的頁面。 使用中的樞紐會反白顯示。

**浮動切換**

-   當所有的樞紐標頭大小不符合允許的空間時，樞紐會浮動切換。
-   點選樞紐標籤會瀏覽到對應的頁面，且使用中的樞紐標籤會浮動切換到第一個位置。

該控制項具有內建的中斷點功能，此功能是以標頭數目以及標籤的字串長度為基礎。

## 建議

-   根據螢幕大小對齊索引標籤/樞紐標頭。 對於寬度小於 720 epx 的螢幕，置中對齊通常比較好，而大多數情況是寬度大於 720 epx 的螢幕建議使用靠左對齊。
-   當調整視窗大小時，一旦索引標籤/樞紐標頭數量超過可用的空間，就會開始將標頭推入溢位區域。
-   索引標籤/樞紐適用於任意螢幕方向，但請務必確認在橫向與直向方向維持相同總數的標頭 (可見與隱藏)。
-   當使用浮動切換 (反覆) 模式時請避免使用超過 5 個標頭，因為超過 5 個可能造成使用者失去方向。
-   在行動裝置上，將索引標籤/樞紐放置於底部有助於方便存取 (如果已在 UI 的其他部分使用撥動)，並避免 UI 頭重腳輕的感覺。
-   當部署螢幕小鍵盤之後，就可以將標頭移出螢幕外以保留空間。

\[本文包含通用 Windows 平台 (UWP) app 與 Windows 10 專屬的資訊。 如需 Windows 8.1 指導方針，請下載 [Windows 8.1 指導方針 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)。\]

## 相關主題

[瀏覽設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958438)


<!--HONumber=Mar16_HO1-->


