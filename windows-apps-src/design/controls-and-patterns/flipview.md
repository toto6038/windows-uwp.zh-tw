---
Description: 顯示集合中的影像 (一次一個影像)，例如相簿中的相片或是產品詳細資料頁面中的項目。
title: 翻轉檢視控制項的指導方針
ms.assetid: A4E05D92-1A0E-4CDD-84B9-92199FF8A8A3
label: Flip view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ef17fdc69f7a9f25831c5c17419768ff32874f80
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2020
ms.locfileid: "80080941"
---
# <a name="flip-view"></a>翻轉檢視

針對瀏覽集合中的影像或其他項目 (一次一個項目) 使用翻轉檢視，例如相簿中的相片或是產品詳細資料頁面中的項目。 如果是觸控式裝置，可撥動項目以在集合中移動。 如果是滑鼠，在滑鼠暫留時會顯示瀏覽按鈕。 如果是鍵盤，可使用方向鍵在集合中移動。

**取得 Windows UI 程式庫**

|  |  |
| - | - |
| ![WinUI 標誌](images/winui-logo-64x64.png) | Windows UI 程式庫 2.2 或更新版本中有這個控制項使用圓角的新範本。 如需詳細資訊，請參閱[圓角半徑](/windows/uwp/design/style/rounded-corner)。 WinUI 是 NuGet 套件，包含適用於 UWP 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **平台 API：** [FlipView 類別](/uwp/api/windows.ui.xaml.controls.flipview)[ItemsSource 屬性](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)[ItemTemplate 屬性](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

翻轉檢視最適合用來瀏覽小型到中型集合 (最高可達 25 個項目) 中的影像。 這類集合的範例包含產品詳細資料頁面中的項目，或是相簿中的相片。 雖然我們不建議針對大部分大型集合使用翻轉檢視，但該控制項一般會用來檢視相簿中的個別影像。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/FlipView">開啟應用程式並查看 FlipView 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

水平瀏覽 (從最左邊的項目開始，在右邊翻轉) 是翻轉檢視典型的配置。 此種配置可在所有裝置的直向或橫向方向使用：

![水平翻轉檢視配置的範例](images/controls_flipview_horizonal.jpg)

翻轉檢視也可以垂直瀏覽：

![垂直翻轉檢視的範例](images/controls_flipview_vertical.jpg)

## <a name="create-a-flip-view"></a>建立翻轉檢視

FlipView 是一種 [ItemsControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol)，因此可以包含任何類型的項目集合。 若要填入檢視，請新增項目至 [**Items**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 集合，或將 [**ItemsSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 屬性設定至資料來源。

根據預設，資料項目會在 FlipView 中以字串方式顯示所繫結的資料物件。 若要指定項目在翻轉檢視中的確切顯示方式，請建立 [**DataTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) 以定義用來顯示個別項目的控制項配置。 配置中的控制項可以繫結至資料物件的屬性，或以內嵌方式定義內容。 將 DataTemplate 指派給 FlipView 的 [**ItemTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 屬性。

### <a name="add-items-to-the-items-collection"></a>新增項目到 Items 集合

您可以使用 XAML 或程式碼，將項目新增到 [**Items**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 集合。 如果您只有少量不會變更且很容易在 XAML 中定義的項目，或是如果您在執行階段於程式碼中產生項目，便通常會用這種方式新增項目。 以下的翻轉檢視含有以內嵌方式定義的項目。

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

將項目新增到翻轉檢視時，它們會自動放置到 [**FlipViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipViewItem) 容器中。 若要變更項目的顯示方式，您可以設定 [**ItemContainerStyle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle) 屬性，將樣式套用到項目容器。 

在 XAML 中定義項目時，項目會自動新增到 Items 集合。

### <a name="set-the-items-source"></a>設定項目來源

您通常會使用翻轉檢視以顯示來自資料庫或網際網路等來源的資料。 若要從資料來源填入翻轉檢視，您要將它的 [**ItemsSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 屬性設定為資料項目的集合。

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

您也可以將 ItemsSource 屬性繫結到 XAML 中的集合。 如需詳細資訊，請參閱[與 XAML 的資料繫結](../../data-binding/data-binding-quickstart.md)。

此處的 ItemsSource 是繫結至名為 `itemsViewSource` 的 [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)。 

```xaml
<Page.Resources>
    <!-- Collection of items displayed by this page -->
    <CollectionViewSource x:Name="itemsViewSource" Source="{Binding Items}"/>
</Page.Resources>

...

<FlipView x:Name="itemFlipView" 
          ItemsSource="{Binding Source={StaticResource itemsViewSource}}"/>
```

>**注意：** &nbsp;&nbsp;填入翻轉檢視有兩種方法，您可以將項目新增到它的 Items 集合，或是設定它的 ItemsSource 屬性，但是不可以同時使用這兩種方式。 如果您設定 ItemsSource 屬性並在 XAML 中新增項目，新增的項目將會被略過。 如果您設定 ItemsSource 屬性並將項目新增到程式碼的 Items 集合，則會擲出例外狀況。

### <a name="specify-the-look-of-the-items"></a>指定項目的外觀

根據預設，資料項目會在 FlipView 中以字串方式顯示所繫結的資料物件。 您通常會想要以更多樣化的表示方式顯示資料。 為了明確指定項目在翻轉檢視內的顯示方式，您需要建立一個 [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)。 在 DataTemplate 中的 XAML 會定義用來顯示個別項目之控制項的配置和外觀。 配置中的控制項可以繫結至資料物件的屬性，或以內嵌方式定義內容。 DataTemplate 是指派給 FlipView 控制項的 [**ItemTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 屬性。

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

### <a name="set-the-orientation-of-the-flip-view"></a>設定翻轉檢視的方向

根據預設，翻轉檢視是以水平翻轉。 若要讓它垂直翻轉，請以具有垂直方向的堆疊面板做為翻轉檢視的 [**ItemsPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel)。

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

## <a name="adding-a-context-indicator"></a>新增內容指示器

翻轉檢視中的內容指示器提供有用的參考點。 標準內容指示器中的點並非互動式。 如此範例所示，最佳擺放位置通常為圖庫下方的中心位置：

![頁面指示器的範例](images/controls_pageindicator.png)

如果是較大型的集合 (10-25 個項目)，請考慮使用可提供更多內容的指示器 (例如縮圖影片)。 有別於使用簡易點的內容指示器，影片區域中的每個縮圖會顯示對應影像的小型版本，且應為可選：

![內容指示器的範例](images/controls_contextindicator.jpg)

如需示範如何將內容指示器新增至 FlipView 的範例程式碼，請參閱 [XAML FlipView 範例](https://code.msdn.microsoft.com/windowsapps/XAML-FlipView-control-0ae45312)。

## <a name="dos-and-donts"></a>可行與禁止事項

-   翻轉檢視最適合用於最多 25 個項目的集合。
-   避免針對較大型集合使用翻轉檢視控制項，因為翻閱各個項目的重複動作可能會很煩人。 相簿則是一個例外，因為它通常有數百或數千個影像。 在格線檢視配置中選取相片之後，相簿幾乎一律會切換為翻轉檢視。 對於其他大型集合，請考量[清單檢視或格線檢視](lists.md)。
-   對於內容指示器：
    -   點 (或您選擇的任一種視覺標記) 的順序在置中且位於水平移動瀏覽圖庫下方時的效果最好。
    -   如果您想在垂直移動瀏覽的圖庫中使用內容指示器，則它在置中且位於影像右側時的效果最好。
    -   反白顯示的點表示目前的項目。 通常反白顯示的點是白色，其他點則是灰色。
    -   點數目可能會改變，但不會多到讓使用者可能必須努力尋找他或她的位置 - 10 個點通常是顯示的數目上限。

## <a name="globalization-and-localization-checklist"></a>全球化和當地語系化檢查清單

<table>
<tr>
<th>雙向考量</th><td>使用 RTL 語言的標準鏡像。 上一頁和下一頁控制項應該根據語言的方向，因此針對 RTL 語言，右邊的按鈕應該往後瀏覽，左邊的按鈕則應該往前瀏覽。</td>
</tr>

</table>

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [清單的指導方針](lists.md)
- [**FlipView 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView)
