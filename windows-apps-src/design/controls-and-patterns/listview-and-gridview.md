---
author: muhsinking
Description: Use ListView and GridView controls to display and manipulate sets of data, such as a gallery of images or a set of email messages.
title: 清單檢視和方格檢視
label: List view and grid view
template: detail.hbs
ms.author: jimwalk
ms.date: 05/20/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f8532ba0-5510-4686-9fcf-87fd7c643e7b
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1ee00a9af23be945ad27ab4b39eec127ec397894
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995870"
---
# <a name="list-view-and-grid-view"></a>清單檢視和方格檢視

大部分應用程式都會操縱和顯示資料組 (例如影像圖庫) 或一組電子郵件訊息。 XAML UI 架構提供 ListView 和 GridView 控制項，讓您輕鬆顯示和操縱應用程式中的資料。  

> **重要 API**：[ListView 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)、[GridView 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)、[ItemsSource 屬性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx)、[Items 屬性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx)

ListView 和 GridView 都是衍生自 ListViewBase 類別，因此具有相同功能，但會以不同方式顯示資料。 在本文中，當我們討論 ListView 時，除非另外指定，否則該資訊適用於 ListView 和 GridView 控制項。 我們可能會參考像是 ListView 或 ListViewItem 等類別，但對於對應的方格對等項目 (GridView 或 GridViewItem)，“List” 首碼可使用 “Grid” 來取代。 

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

ListView 會在單一欄中以垂直堆疊的方式顯示資料。 這常用於顯示已排序的項目清單，例如電子郵件清單或搜尋結果。 

![具有分組資料的清單檢視](images/simple-list-view-phone.png)

GridView 會在可垂直捲動的列和欄中顯示項目集合。 資料會以水平方向進行堆疊，直到它填滿該欄為止，然後繼續進行下一列。 這常用於需要以豐富視覺效果顯示每個項目時，例如影像中心這類每個項目需要更多空間的情況。 

![內容庫範例](images/controls_list_contentlibrary.png)

如需進一步比較該使用哪個控制項以及指導方針，請參閱[清單](lists.md)。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡開啟應用程式並查看 <a href="xamlcontrolsgallery:/item/ListView">ListView</a> 或 <a href="xamlcontrolsgallery:/item/GridView">GridView</a> 運作情形。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-list-view"></a>建立清單檢視

清單檢視是一個 [ItemsControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.aspx)，因此可包含任何類型的項目集合。 在其 [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合中必須有項目，才能在畫面上顯示任何內容。 若要填入檢視，您可以直接將項目新增至 [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合，或將 [ItemsSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 屬性設為資料來源。 

**重要**&nbsp;&nbsp;您可以使用 Items 或 ItemsSource 來填入清單，但無法同時使用這兩者。 如果您設定 ItemsSource 屬性並在 XAML 中新增項目，新增的項目將會被略過。 如果您設定 ItemsSource 屬性並將項目新增到程式碼的 Items 集合，則會擲出例外狀況。

> **注意**&nbsp;&nbsp;為了簡化起見，本文的許多範例會直接填入 **Items** 集合。 不過，清單中較常見的項目是來自動態來源，例如，來自線上資料庫的書籍清單。 您可以使用 **ItemsSource** 屬性來達到此目的。 

### <a name="add-items-to-the-items-collection"></a>將項目新增到 Items 集合

您可以使用 XAML 或程式碼，將項目新增到 [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合。 如果您只有少量不會變更且很容易在 XAML 中定義的項目，或者會在執行階段於程式碼中產生項目，通常就會用這種方式新增項目。 

以下的清單檢視含有 XAML 中以內嵌方式定義的項目。 在 XAML 中定義項目時，項目會自動新增到 Items 集合。

**XAML**
```xaml
<ListView x:Name="listView1"> 
   <x:String>Item 1</x:String> 
   <x:String>Item 2</x:String> 
   <x:String>Item 3</x:String> 
   <x:String>Item 4</x:String> 
   <x:String>Item 5</x:String> 
</ListView>  
```

以下是在程式碼中建立的清單檢視。 產生的清單與先前在 XAML 中建立的一樣。

**C#**
```csharp
// Create a new ListView and add content. 
ListView listView1 = new ListView(); 
listView1.Items.Add("Item 1"); 
listView1.Items.Add("Item 2"); 
listView1.Items.Add("Item 3"); 
listView1.Items.Add("Item 4"); 
listView1.Items.Add("Item 5");
 
// Add the ListView to a parent container in the visual tree. 
stackPanel1.Children.Add(listView1); 
```

ListView 看起來如下。

![簡單的清單檢視](images/listview-simple.png)

### <a name="set-the-items-source"></a>設定項目來源

您通常會使用清單檢視，以顯示來自資料庫或網際網路等來源的資料。 若要從資料來源填入清單檢視，您要將它的 [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 屬性設定為資料項目的集合。

在這裡，清單檢視的 ItemsSource 會在程式碼中直接設定為集合的執行個體。

**C#**
```csharp 
// Instead of hard coded items, the data could be pulled 
// asynchronously from a database or the internet.
ObservableCollection<string> listItems = new ObservableCollection<string>();
listItems.Add("Item 1");
listItems.Add("Item 2");
listItems.Add("Item 3");
listItems.Add("Item 4");
listItems.Add("Item 5");

// Create a new list view, add content, 
ListView itemListView = new ListView();
itemListView.ItemsSource = listItems;

// Add the list view to a parent container in the visual tree.
stackPanel1.Children.Add(itemListView);
```

您也可以將 ItemsSource 屬性繫結到 XAML 中的集合。 如需資料繫結的詳細資訊，請參閱[資料繫結概觀](https://msdn.microsoft.com/windows/uwp/data-binding/data-binding-quickstart)。

在這裡，ItemsSource 會繫結至名為 `Items` 的公用屬性，此屬性可公開頁面的私用資料集合。

**XAML**
```xaml
<ListView x:Name="itemListView" ItemsSource="{x:Bind Items}"/>
```

**C#**
```csharp
private ObservableCollection<string> _items = new ObservableCollection<string>();

public ObservableCollection<string> Items
{
    get { return this._items; }
}

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    // Instead of hard coded items, the data could be pulled 
    // asynchronously from a database or the internet.
    Items.Add("Item 1");
    Items.Add("Item 2");
    Items.Add("Item 3");
    Items.Add("Item 4");
    Items.Add("Item 5");
}
```

如果您需要在清單檢視中顯示分組資料，就必須繫結到 [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx)。 CollectionViewSource 會做為 XAML 中集合類別的 Proxy，並啟用貨幣和群組支援。 如需詳細資訊，請參閱 [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx)。

## <a name="data-template"></a>資料範本

項目的資料範本會定義資料視覺化的方式。 根據預設，資料項目會在清單檢視中，以字串形式顯示所繫結的資料物件。 您可以將 [DisplayMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.displaymemberpath.aspx) 設定為該屬性，以顯示資料項目之特定屬性的字串表示法。

但是，您通常會想要以更多樣化的表示方式來顯示資料。 為了明確指定項目在清單檢視中的顯示方式，您需要建立一個 [DataTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx)。 DataTemplate 中的 XAML 會定義用來顯示個別項目之控制項的配置和外觀。 配置中的控制項可以繫結至資料物件的屬性，或以內嵌方式定義靜態內容。 將 DataTemplate 指派給清單控制項的 [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 屬性。

在此範例中，資料項目是一個簡單字串。 您使用 DataTemplate 來將影像新增到字串左邊，並以藍綠色顯示字串。

> **注意**&nbsp;&nbsp;當您在 DataTemplate 中使用 [x:Bind 標記延伸](https://msdn.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)時，必須在 DataTemplate 上指定 DataType (`x:DataType`)。

**XAML**
```XAML
<ListView x:Name="listView1">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="47"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Image Source="Assets/placeholder.png" Width="32" Height="32" 
                       HorizontalAlignment="Left"/>
                <TextBlock Text="{x:Bind}" Foreground="Teal" 
                           FontSize="15" Grid.Column="1"/>
            </Grid> 
        </DataTemplate>
    </ListView.ItemTemplate>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

以下是使用此資料範本顯示資料項目時的樣子。

![使用資料範本的清單檢視項目](images/listview-itemstemplate.png)

資料範本是您定義清單檢視外觀的主要方式。 如果您的清單會顯示大量項目，它們也會對效能產生顯著的影響。 本文中的大多數範例，我們都會使用簡單的字串資料，而且不會指定資料範本。 如需如何使用資料範本和項目容器，在清單或方格中定義項目外觀的詳細資訊和範例，請參閱[項目容器與範本](item-containers-templates.md)。 

## <a name="change-the-layout-of-items"></a>變更項目的配置

當您將項目新增到清單檢視或方格檢視時，控制項會在項目容器中將每個項目自動換行，接著配置所有項目容器。 這些項目容器的配置方式取決於控制項的 [ItemsPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx)。  
- 根據預設，**ListView** 會使用 [ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx)，這會產生垂直清單，如下所示。

![簡單的清單檢視](images/listview-simple.png)

- **GridView** 會使用 [ItemsWrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemswrapgrid.aspx)，以水平方向新增項目，然後換行並以垂直方向捲動，如下所示。

![簡單的方格檢視](images/gridview-simple.png)

您可以藉由調整項目面板上的屬性來修改項目配置，或者可以使用另一個面板來取代預設面板。

> 注意&nbsp;&nbsp;如果您變更 ItemsPanel，請小心不要停用虛擬化。 **ItemsStackPanel** 和 **ItemsWrapGrid** 都支援虛擬化，因此可放心使用這兩者。 如果您使用任何其他面板，您可能會停用虛擬化，並拖慢清單檢視的效能。 如需詳細資訊，請參閱[效能](https://msdn.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui)下方的清單檢視文章。 

這個範例示範如何藉由變更 **ItemsStackPanel** 的 [Orientation](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.orientation.aspx) 屬性，讓 **ListView** 能夠在水平清單中配置項目容器。
因為清單檢視預設是垂直捲動，所以您也需要在清單檢視的內部 [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) 上調整一些屬性，讓它能夠水平捲動。
- 將 [ScrollViewer.HorizontalScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode.aspx) 設為 **Enabled** 或 **Auto**
- 將 [ScrollViewer.HorizontalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility.aspx) 設為 **Auto** 
- 將 [ScrollViewer.VerticalScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticalscrollmode.aspx) 設為 **Disabled** 
- 將 [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility.aspx) 設為 **Hidden** 

> **注意**&nbsp;&nbsp;這些範例所顯示的清單檢視寬度不受限制，因此不會顯示水平捲軸。 如果您執行這個程式碼，可在 ListView 上設定 `Width="180"` 來顯示捲軸。

**XAML**
```xaml
<ListView Height="60" 
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

產生的清單外觀如下。

![水平清單檢視](images/listview-horizontal.png)

 在下一個範例中，**ListView** 會使用 **ItemsWrapGrid** 而不是 **ItemsStackPanel**，在垂直換行清單中配置項目。 
 
> **注意**&nbsp;&nbsp;清單檢視的高度必須受到限制，才能強制控制項在容器中換行。

**XAML**
```xaml
<ListView Height="100"
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

產生的清單外觀如下。

![使用方格配置的清單檢視](images/listview-itemswrapgrid.png)

如果您在清單檢視中顯示分組資料，ItemsPanel 會決定配置項目群組的方式，而不是配置個別項目的方式。例如，如果使用先前所示的水平 ItemsStackPanel 來顯示分組資料，群組會呈現水平排列，但是每個群組中的項目仍然會以垂直方向進行堆疊，如下所示。

![分組的水平清單檢視](images/listview-horizontal-groups.png)

## <a name="item-selection-and-interaction"></a>項目選取與互動

您可以從各種方式進行選擇，讓使用者可以與清單檢視互動。 根據預設，使用者可以選取單一項目。 您可以變更 [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) 屬性，以啟用多重選取或停用選取。 您可以設定 [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) 屬性，讓使用者可按一下項目來叫用動作 (例如按鈕)，而不需選取項目。

> **注意**&nbsp;&nbsp;ListView 和 GridView 都會針對其 SelectionMode 屬性使用 [ListViewSelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewselectionmode.aspx) 列舉。 根據預設，IsItemClickEnabled 是 **False**，因此您只能設定它來啟用按一下模式。

下表顯示使用者可以與清單檢視互動的方式，以及您可以回應互動的方式。

若要啟用此互動︰ | 使用這些設定： | 處理這個事件︰ | 使用此屬性來取得選取的項目：
----------------------------|---------------------|--------------------|--------------------------------------------
沒有互動 | [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) = **None**，[IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) = **False** | 不適用 | 無 
單一選取 | SelectionMode = **Single**，IsItemClickEnabled = **False** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selecteditem.aspx)，[SelectedIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectedindex.aspx)  
多重選取 | SelectionMode = **Multiple**，IsItemClickEnabled = **False** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx)  
延伸選取 | SelectionMode = **Extended**，IsItemClickEnabled = **False** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx)  
按一下 | SelectionMode = **None**，IsItemClickEnabled = **True** | [ItemClick](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.itemclick.aspx) | 不適用 

> **注意**&nbsp;&nbsp;從 Windows 10 開始，您可以啟用 IsItemClickEnabled 來引發 ItemClick 事件，同時也將 SelectionMode 設定為 Single、Multiple 或 Extended。 如果您執行此動作，就會先引發 ItemClick 事件，接著引發 SelectionChanged 事件。 在某些情況下，假設您瀏覽到 ItemClick 事件處理常式的另一個頁面，就不會引發 SelectionChanged 事件且不會選取該項目。

您可以在 XAML 或程式碼中設定這些屬性，如下所示。

**XAML**
```xaml
<ListView x:Name="myListView" SelectionMode="Multiple"/>

<GridView x:Name="myGridView" SelectionMode="None" IsItemClickEnabled="True"/> 
```

**C#**
```csharp
myListView.SelectionMode = ListViewSelectionMode.Multiple; 

myGridView.SelectionMode = ListViewSelectionMode.None;
myGridView.IsItemClickEnabled = true;
```

### <a name="read-only"></a>唯讀

您可以將 SelectionMode 屬性設為 **ListViewSelectionMode.None** 來停用項目選取。 這會將控制項放置於唯讀模式中，以用來顯示資料，但不會與它互動。 控制項本身並未停用，只會停用選取的項目。

### <a name="single-selection"></a>單一選取

下表說明當 SelectionMode 為 **Single** 時的鍵盤、滑鼠及觸控互動。

輔助按鍵 | 互動
-------------|------------
無 | <li>使用者可以使用空格鍵、按一下滑鼠或觸控點選來選取單一項目。</li>
Ctrl | <li>使用者可以使用空格鍵、按一下滑鼠或觸控點選來取消選取單一項目。</li><li>使用者可以使用方向鍵來移動各自獨立的選取焦點。</li>

當 SelectionMode 是 **Single** 時，您可以從 [SelectedItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selecteditem.aspx) 屬性取得選取的資料項目。 您可以使用 [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectedindex.aspx) 屬性來取得選取項目集合中的索引。 如果未選取任何項目，SelectedItem 即為 **null** 且 SelectedIndex 是 -1。 
 
如果您嘗試將不在 **Items** 集合中的項目設定為 **SelectedItem**，則此作業會遭到忽略，而且 SelectedItem 為**null**。 不過，如果您嘗試將 **SelectedIndex** 設為不在清單中 **Items** 範圍內的索引，即會發生 **System.ArgumentException** 例外狀況。 

### <a name="multiple-selection"></a>多重選取

下表說明當 SelectionMode 為 **Multiple** 時的鍵盤、滑鼠及觸控互動。

輔助按鍵 | 互動
-------------|------------
無 | <li>使用者可以使用空格鍵、按一下滑鼠或觸控點選來選取多個項目，以便在焦點項目上切換選取項目。</li><li>使用者可以使用方向鍵來移動各自獨立的選取焦點。</li>
Shift | <li>使用者可以選取多個連續項目，方法是按一下或點選選取範圍中的第一個項目，然後按一下或點選選取範圍中的最後一個項目。</li><li>使用者可以使用方向鍵來建立連續的選取範圍，選取範圍的第一個項目是按下 Shift 鍵時所選取的項目。</li>

### <a name="extended-selection"></a>延伸選取

下表說明當 SelectionMode 為 **Extended** 時的鍵盤、滑鼠及觸控互動。

輔助按鍵 | 互動
-------------|------------
無 | <li>此行為與**單一**選取相同。</li>
Ctrl | <li>使用者可以使用空格鍵、按一下滑鼠或觸控點選來選取多個項目，以便在焦點項目上切換選取項目。</li><li>使用者可以使用方向鍵來移動各自獨立的選取焦點。</li>
Shift | <li>使用者可以選取多個連續項目，方法是按一下或點選選取範圍中的第一個項目，然後按一下或點選選取範圍中的最後一個項目。</li><li>使用者可以使用方向鍵來建立連續的選取範圍，選取範圍的第一個項目是按下 Shift 鍵時所選取的項目。</li>

當 SelectionMode 是 **Multiple** 或 **Extended** 時，您可以從 [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx) 屬性取得選取的資料項目。 

**SelectedIndex**、**SelectedItem** 及 **SelectedItems** 屬性會同步處理。 例如，如果您將 SelectedIndex 設為 -1，SelectedItem 就會設為 **null** 且 SelectedItems 是空的；如果您將 SelectedItem 設為 **null**，SelectedIndex 即會設為 -1 且 SelectedItems 是空的。

在多重選取模式中，**SelectedItem** 會包含第一個選取的項目，而 **Selectedindex** 會包含第一個選取的項目索引。 

### <a name="respond-to-selection-changes"></a>回應選取項目變更

若要回應清單檢視中的選取變更，請處理 [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) 事件。 在事件處理常式程式碼中，您可以從 [SelectionChangedEventArgs.AddedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.selectionchangedeventargs.addeditems.aspx) 屬性取得所選項目的清單。 您可以取得已從 [SelectionChangedEventArgs.RemovedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.selectionchangedeventargs.removeditems.aspx) 屬性中取消選取的任何項目。 除非使用者長按 Shift 鍵來選取某個範圍的項目，否則 AddedItems 和 RemovedItems 集合最多只會包含 1 個項目。

這個範例示範如何處理 **SelectionChanged** 事件，以及存取各種不同的項目集合。

**XAML**
```xaml
<StackPanel HorizontalAlignment="Right">
    <ListView x:Name="listView1" SelectionMode="Multiple" 
              SelectionChanged="ListView1_SelectionChanged">
        <x:String>Item 1</x:String>
        <x:String>Item 2</x:String>
        <x:String>Item 3</x:String>
        <x:String>Item 4</x:String>
        <x:String>Item 5</x:String>
    </ListView>
    <TextBlock x:Name="selectedItem"/>
    <TextBlock x:Name="selectedIndex"/>
    <TextBlock x:Name="selectedItemCount"/>
    <TextBlock x:Name="addedItems"/>
    <TextBlock x:Name="removedItems"/>
</StackPanel> 
```

**C#**
```csharp
private void ListView1_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (listView1.SelectedItem != null)
    {
        selectedItem.Text = 
            "Selected item: " + listView1.SelectedItem.ToString();
    }
    else
    {
        selectedItem.Text = 
            "Selected item: null";
    }
    selectedIndex.Text = 
        "Selected index: " + listView1.SelectedIndex.ToString();
    selectedItemCount.Text = 
        "Items selected: " + listView1.SelectedItems.Count.ToString();
    addedItems.Text = 
        "Added: " + e.AddedItems.Count.ToString();
    removedItems.Text = 
        "Removed: " + e.RemovedItems.Count.ToString();
}
```

### <a name="click-mode"></a>按一下模式

您可以變更清單檢視，讓使用者按一下項目像是在按一下按鈕，而不用選取項目。 例如，當您的使用者按一下清單或方格中的項目來使應用程式瀏覽到新頁面時，這會非常實用。 若要啟用此行為︰
- 將 **SelectionMode** 設為 **None**。
- 將 **IsItemClickEnabled** 設為 **true**。
- 處理 **ItemClick** 事件，在使用者按一下項目時執行一些動作。

以下是具有可點選項目的清單檢視。 ItemClick 事件處理常式中的程式碼會瀏覽到新頁面。

**XAML**
```xaml
<ListView SelectionMode="None"
          IsItemClickEnabled="True" 
          ItemClick="ListView1_ItemClick">
    <x:String>Page 1</x:String>
    <x:String>Page 2</x:String>
    <x:String>Page 3</x:String>
    <x:String>Page 4</x:String>
    <x:String>Page 5</x:String>
</ListView>
```

**C#**
```csharp
private void ListView1_ItemClick(object sender, ItemClickEventArgs e)
{
    switch (e.ClickedItem.ToString())
    {
        case "Page 1":
            this.Frame.Navigate(typeof(Page1));
            break;

        case "Page 2":
            this.Frame.Navigate(typeof(Page2));
            break;

        case "Page 3":
            this.Frame.Navigate(typeof(Page3));
            break;

        case "Page 4":
            this.Frame.Navigate(typeof(Page4));
            break;

        case "Page 5":
            this.Frame.Navigate(typeof(Page5));
            break;

        default:
            break;
    }
}
```

### <a name="select-a-range-of-items-programmatically"></a>以程式設計的方式選取某個範圍的項目

您有時需要以程式設計方式操縱清單檢視的項目選取。 例如，您可能會提供 **\[全選\]** 按鈕，讓使用者選取清單中的所有項目。 在此情況下，從 SelectedItems 集合逐一新增和移除項目通常是不太有效率的。 每個項目變更都會導致 SelectionChanged 事件的發生，當您直接使用項目而不是使用索引值時，即會取消項目的虛擬化。

[SelectAll](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectall.aspx)、[SelectRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectrange.aspx) 與 [DeselectRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.deselectrange.aspx) 方法提供比使用 SelectedItems 屬性更有效率的方式來修改選取項目。 這些方法會使用項目索引的範圍來選取或取消選取。 由於只使用索引，因此，已虛擬化的項目仍會維持虛擬化狀態。 指定範圍中的所有項目都會選取 (或取消選取)，而無論其原始選取狀態為何。 針對這些方法的每一個呼叫，SelectionChanged 事件只會發生一次。

> **重要**&nbsp;&nbsp;只有當 SelectionMode 屬性設為 Multiple 或 Extended 時，才能呼叫這些方法。 如果您在 SelectionMode 為 Single 或 None 時呼叫 SelectRange，即會擲回例外狀況。

當您使用索引範圍選取項目時，請使用 [SelectedRanges](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectedranges.aspx) 屬性來取得清單中的所有選取範圍。

如果 ItemsSource 實作 [IItemsRangeInfo](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.iitemsrangeinfo.aspx)，而您使用這些方法來修改選取，則不會在 SelectionChangedEventArgs 中設定 **AddedItems** 和 **RemovedItems** 屬性。 設定這些屬性需要將項目物件取消虛擬化。 請改用 **SelectedRanges** 屬性來取得項目。

您可以呼叫 SelectAll 方法來選取集合中的所有項目。 不過，沒有對應的方法來取消選取所有項目。 您可以呼叫 DeselectRange 並傳遞 FirstIndex 值為 0 和 [Length](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.length.aspx) 值等於集合中項目數目的 [[ItemIndexRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.firstindex.aspx)](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.aspx) 來取消所有項目。 

**XAML**
```xaml
<StackPanel Width="160">
    <Button Content="Select all" Click="SelectAllButton_Click"/>
    <Button Content="Deselect all" Click="DeselectAllButton_Click"/>
    <ListView x:Name="listView1" SelectionMode="Multiple">
        <x:String>Item 1</x:String>
        <x:String>Item 2</x:String>
        <x:String>Item 3</x:String>
        <x:String>Item 4</x:String>
        <x:String>Item 5</x:String>
    </ListView>
</StackPanel>
```

**C#**
```csharp
private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.SelectAll();
    }
}

private void DeselectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.DeselectRange(new ItemIndexRange(0, (uint)listView1.Items.Count));
    }
}
```

如需如何變更選取項目外觀的詳細資訊，請參閱[項目容器與範本](item-containers-templates.md)。

### <a name="drag-and-drop"></a>拖放

ListView 和 GridView 控制項支援在項目本身內部，以及在本身和其他 ListView 與 GridView 控制項之間進行拖放。 如需有關實作拖放模式的詳細資訊，請參閱[拖放](https://msdn.microsoft.com/windows/uwp/design/input/drag-and-drop)。 

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML ListView 和 GridView 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)：示範 ListView 和 GridView 控制項。
- [XAML 拖放範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop)：使用 ListView 控制項示範拖放。
- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)：以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [清單](lists.md)
- [項目容器與範本](item-containers-templates.md)
- [拖放](https://msdn.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
