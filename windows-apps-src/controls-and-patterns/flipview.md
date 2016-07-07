---
author: Jwmsft
Description: "顯示集合中的影像 (一次一個影像)，例如相簿中的相片或是產品詳細資料頁面中的項目。"
title: "翻轉檢視控制項的指導方針"
ms.assetid: A4E05D92-1A0E-4CDD-84B9-92199FF8A8A3
label: Flip view
template: detail.hbs
ms.sourcegitcommit: 7d438080e2e8533f1148c07e27143d4d1fcacf5d
ms.openlocfilehash: ecb46c0d42821d833e8232780b553754f8f097c5

---
# 翻轉檢視

針對瀏覽集合中的影像或其他項目 (一次一個項目) 使用翻轉檢視，例如相簿中的相片或是產品詳細資料頁面中的項目。 如果是觸控式裝置，可撥動項目以在集合中移動。 如果是滑鼠，在滑鼠暫留時會顯示瀏覽按鈕。 如果是鍵盤，可使用方向鍵在集合中移動。




-   [**FlipView 類別**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx)
-   [**ItemsSource 屬性**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx)
-   [**ItemTemplate 屬性**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)

## 這是正確的控制項嗎？

翻轉檢視最適合用來瀏覽小型到中型集合 (最高可達 25 個項目) 中的影像。 這類集合的範例包含產品詳細資料頁面中的項目，或是相簿中的相片。 雖然我們不建議針對大部分大型集合使用翻轉檢視，但該控制項一般會用來檢視相簿中的個別影像。

## 範例

水平瀏覽 (從最左邊的項目開始，在右邊翻轉) 是翻轉檢視典型的配置。 此種配置可在所有裝置的直向或橫向方向使用：

![水平翻轉檢視配置的範例](images/controls_flipview_horizonal.jpg)

翻轉檢視也可以垂直瀏覽：

![垂直翻轉檢視的範例](images/controls_flipview_vertical.jpg)

## 建立翻轉檢視

FlipView 是一種 [ItemsControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.aspx)，因此可以包含任何類型的項目集合。 若要填入檢視，請新增項目至 [**Items**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合，或將 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 屬性設定至資料來源。

根據預設，資料項目會在 FlipView 中以字串方式顯示所繫結的資料物件。 若要指定項目在翻轉檢視中的確切顯示方式，請建立 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.datatemplate.aspx) 以定義用來顯示個別項目的控制項配置。 配置中的控制項可以繫結至資料物件的屬性，或以內嵌方式定義內容。 將 DataTemplate 指派給 FlipView 的 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 屬性。

### 新增項目到 Items 集合

您可以使用 XAML 或程式碼，將項目新增到 [**Items**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合。 如果您只有少量不會變更且很容易在 XAML 中定義的項目，或是如果您在執行階段於程式碼中產生項目，便通常會用這種方式新增項目。 以下的翻轉檢視含有以內嵌方式定義的項目。

```xaml
<FlipView x:Name="flipView1">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

```csharp
// Create a new flip view, add content, 
// and add a SelectionChanged event handler.
FlipView flipView1 = new FlipView();
flipView1.Items.Add("Item 1");
flipView1.Items.Add("Item 2");

// Add the flip view to a parent container in the visual tree.
stackPanel1.Children.Add(flipView1);
```

將項目新增到翻轉檢視時，它們會自動放置到 [**FlipViewItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipviewitem.aspx) 容器中。 若要變更項目的顯示方式，您可以設定 [**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle.aspx) 屬性，將樣式套用到項目容器。 

在 XAML 中定義項目時，項目會自動新增到 Items 集合。

### 設定項目來源

您通常會使用翻轉檢視以顯示來自資料庫或網際網路等來源的資料。 若要從資料來源填入翻轉檢視，您要將它的 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 屬性設定為資料項目的集合。

在這裡，翻轉檢視的 ItemsSource 會在程式碼中直接設定至集合的執行個體。

```csharp
// Data source.
List<String> itemsList = new List<string>();
itemsList.Add("Item 1");
itemsList.Add("Item 2");

// Create a new flip view, add content, 
// and add a SelectionChanged event handler.
FlipView flipView1 = new FlipView();
flipView1.ItemsSource = itemsList;
flipView1.SelectionChanged += FlipView_SelectionChanged;

// Add the flip view to a parent container in the visual tree.
stackPanel1.Children.Add(flipView1);
```

您也可以將 ItemsSource 屬性繫結到 XAML 中的集合。 如需詳細資訊，請參閱[與 XAML 的資料繫結](../data-binding/data-binding-quickstart.md)。

此處的 ItemsSource 是繫結至名為 `itemsViewSource` 的 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx)。 

```xaml
<Page.Resources>
    <!-- Collection of items displayed by this page -->
    <CollectionViewSource x:Name="itemsViewSource" Source="{Binding Items}"/>
</Page.Resources>

...

<FlipView x:Name="itemFlipView" 
          ItemsSource="{Binding Source={StaticResource itemsViewSource}}"/>
```

>**注意** &nbsp;&nbsp;填入 FlipView 有兩種方法，您可以新增項目到它的 Items 集合，或是設定它的 ItemsSource 屬性，但是不可以同時使用這兩種方式。 如果您設定 ItemsSource 屬性並在 XAML 中新增項目，新增的項目將會被略過。 如果您設定 ItemsSource 屬性並將項目新增到程式碼的 Items 集合，則會擲出例外狀況。

### 指定項目的外觀

根據預設，資料項目會在 FlipView 中以字串方式顯示所繫結的資料物件。 您通常會想要以更多樣化的表示方式顯示資料。 為了明確指定項目在翻轉檢視內的顯示方式，您需要建立一個 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx)。 在 DataTemplate 中的 XAML 會定義用來顯示個別項目之控制項的配置和外觀。 配置中的控制項可以繫結至資料物件的屬性，或以內嵌方式定義內容。 DataTemplate 是指派給 FlipView 控制項的 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 屬性。

在這個範例中，FlipView 的 ItemTemplate 是以內嵌方式定義。 重疊會新增至影像以顯示影像名稱。 

```XAML
<FlipView x:Name="flipView1" Width="480" Height="270" 
          BorderBrush="Black" BorderThickness="1">
    <FlipView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Image Width="480" Height="270" Stretch="UniformToFill"
                       Source="{Binding Image}"/>
                <Border Background="#A5000000" Height="80" VerticalAlignment="Bottom">
                    <TextBlock Text="{Binding Name}" 
                               FontFamily="Segoe UI" FontSize="26.667" 
                               Foreground="#CCFFFFFF" Padding="15,20"/>
                </Border>
            </Grid>
        </DataTemplate>
    </FlipView.ItemTemplate>
</FlipView>
```

這裡是資料範本定義的配置看起來的樣子。

翻轉檢視資料範本。

### 設定翻轉檢視的方向

根據預設，翻轉檢視是以水平翻轉。 若要讓它垂直翻轉，請以具有垂直方向的堆疊面板做為翻轉檢視的 [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx)。

這個範例顯示如何以具有垂直方向的堆疊面板做為 FlipView 的 ItemsPanel。

```XAML
<FlipView x:Name="flipViewVertical" Width="480" Height="270" 
          BorderBrush="Black" BorderThickness="1">
    
    <!-- Use a vertical stack panel for vertical flipping. -->
    <FlipView.ItemsPanel>
        <ItemsPanelTemplate>
            <VirtualizingStackPanel Orientation="Vertical"/>
        </ItemsPanelTemplate>
    </FlipView.ItemsPanel>
    
    <FlipView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Image Width="480" Height="270" Stretch="UniformToFill"
                       Source="{Binding Image}"/>
                <Border Background="#A5000000" Height="80" VerticalAlignment="Bottom">
                    <TextBlock Text="{Binding Name}" 
                               FontFamily="Segoe UI" FontSize="26.667" 
                               Foreground="#CCFFFFFF" Padding="15,20"/>
                </Border>
            </Grid>
        </DataTemplate>
    </FlipView.ItemTemplate>
</FlipView>
```

以下為以垂直方向顯示之翻轉檢視的外觀。

![垂直翻轉檢視的範例](images/controls_flipview_vertical.jpg)

## 新增內容指示器

翻轉檢視中的內容指示器提供有用的參考點。 標準內容指示器中的點並非互動式。 如此範例所示，最佳擺放位置通常為圖庫下方的中心位置：

![頁面指示器的範例](images/controls_pageindicator.png)

如果是較大型的集合 (10-25 個項目)，請考慮使用可提供更多內容的指示器 (例如縮圖影片)。 有別於使用簡易點的內容指示器，影片區域中的每個縮圖會顯示對應影像的小型版本，並且應該是可選的：

![內容指示器的範例](images/controls_contextindicator.jpg)

## 可行與禁止事項

-   翻轉檢視最適合用於最多 25 個項目的集合。
-   避免針對較大型集合使用翻轉檢視控制項，因為翻閱各個項目的重複動作可能會很煩人。 相簿則是一個例外，因為它通常有數百或數千個影像。 在格線檢視配置中選取相片之後，相簿幾乎一律會切換為翻轉檢視。 對於其他大型集合，請考量[清單檢視或格線檢視](lists.md)。
-   對於內容指示器：
    -   點 (或您選擇的任一種視覺標記) 的順序在置中且位於水平移動瀏覽圖庫下方時的效果最好。
    -   如果您想在垂直移動瀏覽的圖庫中使用內容指示器，則它在置中且位於影像右側時的效果最好。
    -   反白顯示的點表示目前的項目。 通常反白顯示的點是白色，其他點則是灰色。
    -   點數目可能會改變，但不會多到讓使用者可能必須努力尋找他或她的位置 - 10 個點通常是顯示的數目上限。

## 全球化和當地語系化檢查清單

<table>
<tr>
<th>雙向考量</th><td>使用 RTL 語言的標準鏡像。 上一頁和下一頁控制項應該根據語言的方向，因此針對 RTL 語言，右邊的按鈕應該往後瀏覽，左邊的按鈕則應該往前瀏覽。</td>
</tr>

</table>


## 相關文章

- [清單的指導方針](lists.md)
- [**FlipView 類別**](https://msdn.microsoft.com/library/windows/apps/br242678)



<!--HONumber=Jun16_HO4-->


