---
Description: ItemsRepeater 是輕量級控制項產生，並呈現項目的集合。
title: ItemsRepeater
label: ItemsRepeater
template: detail.hbs
ms.date: 02/01/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 93a81501b524826484111419899675fbb99b86fa
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364761"
---
# <a name="itemsrepeater"></a>ItemsRepeater

使用[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)利用有彈性的版面配置系統、 自訂檢視和虛擬化建立自訂收集體驗。

不同於[ListView](/uwp/api/windows.ui.xaml.controls.listview)， [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)不提供完整的經驗 – 它沒有預設值的 UI，並不提供焦點，選取項目或使用者互動的任何原則。 相反地，它是您可用來建立您自己的唯一集合為基礎的體驗和自訂控制項的建置組塊。 雖然它有沒有內建的原則時，它可讓您附加原則以建置您所需要的體驗。 比方說，您可以定義要使用 keyboarding 原則選取原則等的配置。

您可以想像[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)在概念上為資料導向的面板，而不是像 ListView 的完整控制權。 您指定要顯示的資料項目的集合、 項目範本產生的每個資料項目，UI 項目和配置，決定如何調整大小及定位項目。 然後，ItemsRepeater 會產生資料來源為基礎的子元素，並顯示所指定的項目範本和版面配置。 顯示的項目不需要是同質性的因為 ItemsRepeater 可以載入內容來表示的資料項目，根據您在資料範本選取器中指定的準則。

| **取得 Windows 的 UI 程式庫** |
| - |
| 此控制項是包含 Windows UI 程式庫，包含新的控制項和 UWP 應用程式的 UI 功能的 NuGet 套件的過程。 如需詳細資訊，包括安裝指示，請參閱 < [Windows 的 UI 程式庫概觀](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **重要的 Api**:[ItemsRepeater 類別](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)， [ScrollViewer 類別](/uwp/api/windows.ui.xaml.controls.scrollviewer)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)以顯示資料的集合。 雖然它可以用來呈現一組基本的項目，您可能會經常使用它做為自訂控制項範本中顯示項目。

如果您需要的立即可用控制項清單或具有最低程度的自訂方格中顯示資料，請考慮使用[ListView](/uwp/api/windows.ui.xaml.controls.listview)或是[GridView](/uwp/api/windows.ui.xaml.controls.gridview)。

ItemsRepeater 並沒有內建的項目集合。 如果您需要直接提供的項目集合，而不是繫結到個別的資料來源，則您可能需要更高原則的體驗，也應該採用[ListView](/uwp/api/windows.ui.xaml.controls.listview)或是[GridView](/uwp/api/windows.ui.xaml.controls.gridview)。

[ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol)和 ItemsRepeater 這兩個提供可自訂的集合使用經驗，但 ItemsRepeater 支援此虛擬化的 UI 版面配置，而 ItemsControl 則否。 我們建議是否使用 ItemsRepeater 而不 itemscontrol 為例，其只是呈現資料的一些項目，或建置自訂集合控制項。

> [!NOTE]
> 如果您有的情況下，您覺得 ItemsControl 是否符合您的需求，以及 ItemsRepeater 不，歡迎留下意見反應上[Windows UI 程式庫的 GitHub 專案](https://github.com/Microsoft/microsoft-ui-xaml/issues)，讓我們知道。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您有<strong style="font-weight: semi-bold">XAML 控制項陳列庫</strong>應用程式安裝，請按一下這裡可開啟應用程式，並查看<a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a>作用中。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="scrolling-with-itemsrepeater"></a>捲動與 ItemsRepeater

[**ItemsRepeater** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)不是衍生自[**控制**](/uwp/api/windows.ui.xaml.controls.control)，使其不含控制項範本。 因此，它不包含任何類似 ListView 捲動的內建或其他集合控制項。

當您使用**ItemsRepeater**，您應該提供捲動功能，藉由包裝在[ **ScrollViewer** ](/uwp/api/windows.ui.xaml.controls.scrollviewer)控制項。

> [!NOTE]
> 如果您的應用程式將執行舊版 Windows-這些都已釋出*之前*Windows 10，版本 1809-就也必須將裝載**ScrollViewer**內[ **ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost)。 
> ```xaml
> <muxc:ItemsRepeaterScrollHost>
>     <ScrollViewer>
>         <muxc:ItemsRepeater ... />
>     </ScrollViewer>
> </muxc:ItemsRepeaterScrollHost>
> ```
> 如果您的應用程式只會在最新版本的 Windows 10 版本 1809年及更新版本-上執行，則不需要使用[ **ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost)。
>
> 在 Windows 10 版本 1809 之前, **ScrollViewer**未實作[ **IScrollAnchorProvider** ](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)介面**ItemsRepeater**需要。  **ItemsRepeaterScrollHost**可讓**ItemsRepeater**協調**ScrollViewer**有關較早的版本，以正確保留顯示的項目位置使用者在檢視。  否則，項目可能會出現要移動或變更清單中的項目或調整大小的應用程式突然消失。

## <a name="create-an-itemsrepeater"></a>建立 ItemsRepeater

若要使用[ **ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)，您需要提供要藉由設定顯示的資料**ItemsSource**屬性。 然後，告訴它如何藉由設定顯示的項目[ **ItemTemplate** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate)屬性。

### <a name="itemssource"></a>ItemsSource

若要填入的檢視，將[ **ItemsSource** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource)資料項目集合的屬性。 在這裡， **ItemsSource**直接對集合的執行個體的程式碼中設定。

```csharp
ObservableCollection<string> Items = new ObservableCollection<string>();

ItemsRepeater itemsRepeater1 = new ItemsRepeater();
itemsRepeater1.ItemsSource = Items;
```

您也可以繫結**ItemsSource**屬性至 XAML 中的集合。 如需資料繫結的詳細資訊，請參閱[資料繫結概觀](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart)。


```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}"/>
```

### <a name="itemtemplate"></a>ItemTemplate
若要指定資料的項目視覺化的方式，設定[ **ItemTemplate** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate)屬性設[ **DataTemplate** ](/uwp/api/windows.ui.xaml.datatemplate)或[ **DataTemplateSelector** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector)已定義。 資料範本會定義將資料視覺化的方式。 根據預設，項目會顯示在檢視**TextBlock**使用資料物件的字串表示。

不過，您通常想要使用範本所定義的配置和要顯示的個別項目，您將使用的一或多個控制項的外觀顯示您的資料更豐富的呈現。 您在範本中使用的控制項可以繫結至資料物件的屬性，或具有靜態內容內嵌定義。

#### <a name="datatemplate"></a>DataTemplate
在此範例中，資料物件會是一個簡單的字串。 **DataTemplate**包含左邊的文字和樣式影像**TextBlock**藍綠色色彩顯示字串。

> [!NOTE]
> 當您使用[x： 繫結標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)中**DataTemplate**，您必須指定資料類型 (`x:DataType`) 上的 DataTemplate。

```xaml
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
```

以下是 顯示與這個項目會如何顯示**DataTemplate**。

![使用資料範本顯示的項目](images/listview-itemstemplate.png)

使用中項目數**DataTemplate**針對您的檢視會顯示大量項目項目對效能有重大影響。 如需詳細資訊和範例，示範如何使用**DataTemplate**來定義項目的外觀，在清單中，請參閱[項目容器和範本](item-containers-templates.md)。

> [!TIP]
> 為了方便起見，當您想要宣告內嵌的範本，而不是當做靜態資源參考，您可以指定**DataTemplate**或是**DataTemplateSelector**做的直接子系**ItemsRepeater**。  將值指派給**ItemTemplate**屬性。 比方說，這是有效的：
> ```xaml
> <ItemsRepeater ItemsSource="{x:Bind Items}">
>     <DataTemplate>
>         <!-- ... -->
>     </DataTemplate>
> </ItemsRepeater>
> ```

> [!TIP]
> 不同於**ListView**和其他集合的控制項， **ItemsRepeater**不換行中的項目**DataTemplate**與包含額外的項目容器預設原則，例如邊界、 邊框距離、 選取視覺效果或透過視覺狀態的指標。 相反地， **ItemsRepeater**只會呈現中所定義**DataTemplate**。 如果您想您的項目擁有相同的外觀，做為清單檢視項目時，您可以明確地包含容器，例如**ListViewItem**，在您的資料範本。 **ItemsRepeater**會顯示**ListViewItem**視覺效果，但不會自動進行的其他功能，例如選取項目，或顯示的多重選取的核取方塊使用。
>
> 同樣地，如果您的資料收集是實際的控制項集合，例如 **按鈕**(`List<Button>`)，您可以將放**ContentPresenter**中您**DataTemplate**至顯示控制項。

#### <a name="datatemplateselector"></a>DataTemplateSelector

您在檢視中顯示的項目不需要為相同的型別。 您可以提供[ **ItemTemplate** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate)屬性[ **DataTemplateSelector** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector)選取不同**DataTemplate**根據您指定的準則。

這個範例假設**DataTemplateSelector**已定義，決定之間兩個不同**DataTemplate**來代表大型和小型的項目。

```xaml
<ItemsRepeater ...>
    <ItemsRepeater.ItemTemplate>
        <local:VariableSizeTemplateSelector Large="{StaticResource LargeItemTemplate}" 
                                            Small="{StaticResource SmallItemTemplate}"/>
    </ItemsRepeater.ItemTemplate>
</ItemsRepeater>
```

定義時**DataTemplateSelector**搭配**ItemsRepeater**您只需要實作的覆寫[ **SelectTemplateCore(Object)** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore#Windows_UI_Xaml_Controls_DataTemplateSelector_SelectTemplateCore_System_Object_)方法。 如需詳細資訊和範例，請參閱 < [ **DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector)。

> [!NOTE]
> 替代**DataTemplate**來管理項目在更進階的案例中的建立方式的 s 是實作您自己[ **Windows.UI.Xaml.Controls.IElementFactory** ](/uwp/api/windows.ui.xaml.controls.ielementfactory)將用作**ItemTemplate**。  它會負責產生要求時的內容。

## <a name="configure-the-data-source"></a>設定資料來源

使用  [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource)屬性來指定要使用產生的項目內容的集合。 您可以將 ItemsSource 設可實作任何型別**IEnumerable**。 藉由將您的資料來源的其他集合介面，判斷哪些功能可供您的資料進行互動 ItemsRepeater。

此清單會顯示可用的介面，以及何時考慮使用每個。

- [IEnumerable](/dotnet/api/system.collections.generic.ienumerable-1)(.NET) / [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)

  - 可以用於小型的靜態資料集。

    最少的資料來源必須實作 IEnumerable / IIterable 介面。 如果這是所有受到然後控制項會逐一查看所有項目一次建立複本，它可用來存取透過索引值的項目。

- [IReadonlyList](/dotnet/api/system.collections.generic.ireadonlylist-1)(.NET) / [IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_)

  - 可用的靜態、 唯讀的資料集。

    啟用索引來存取項目控制項，並避免多餘的內部副本。

- [IList](/dotnet/api/system.collections.generic.ilist-1)(.NET) / [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

  - 可以用於靜態資料集。

    啟用索引來存取項目控制項，並避免多餘的內部副本。

    **警告**：變更而不需要實作清單/向量[INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)並不會反映在 UI 中。

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)(.NET)

  - 建議使用以支援變更通知。

    啟用的控制項，來觀察和回應資料來源中的變更以及在 UI 中反映這些變更。

- [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)

  - 支援變更通知

    像是**INotifyCollectionChanged**介面，這可讓觀察，並對資料來源中的變更做出回應的控制項。

    **警告**：Windows.Foundation.IObservableVector\<T > 不支援 [移動]' 動作。 這會造成的 UI 項目失去其視覺狀態。  例如，目前選取及 （或） 具有焦點，移動之後，即可後面接著 'Add' 'Remove' 的項目就會失去焦點，而不再處於選取狀態。

    Platform.Collections.Vector\<T > 使用 IObservableVector\<T > 且具有此相同的限制。 如果支援不需要將 [移動]' 動作然後使用**INotifyCollectionChanged**介面。  .NET ObservableCollection\<T > 類別會使用**INotifyCollectionChanged**。

- [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)

  - 當可以與每個項目相關聯的唯一識別碼。  建議使用 '重設' 做為集合變更動作時。

    可讓控制項非常有效率的方式復原之後接收硬碟 [重設] 動作，做為一部分的現有 UI **INotifyCollectionChanged**或是**IObservableVector**事件。 接收的重設後控制項將使用提供的唯一識別碼，它已建立的項目相關聯的目前資料。 沒有索引對應到索引鍵的控制者必須假設需要從頭建立 UI 的資料。

ListView 和 GridView 中所顯示的一樣，上面所列以外 IKeyIndexMapping，介面會提供 ItemsRepeater 中相同的行為。


啟用特殊功能，在 ListView 和 GridView 控制項中，ItemsSource 中的下列介面，但目前不影響 ItemsRepeater:

- [ISupportIncrementalLoading](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)
- [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)
- [ISelectionInfo](/uwp/api/windows.ui.xaml.data.iselectioninfo)

> [!TIP]
> 歡迎您隨時提供您的寶貴意見！ 讓我們知道您在上的想法[Windows UI 程式庫的 GitHub 專案](https://github.com/Microsoft/microsoft-ui-xaml/issues)。 請考慮將您的想法上現有的提案，這類[#374](https://github.com/Microsoft/microsoft-ui-xaml/issues/374):新增 ItemsRepeater 的累加式載入支援。

當使用者捲動時增加或相應減少，以累加方式載入資料的替代方法是觀察 ScrollViewer 的檢視區的位置，並載入更多的資料，當檢視區接近程度。

```xaml
<ScrollViewer ViewChanged="ScrollViewer_ViewChanged">
    <ItemsRepeater ItemsSource="{x:Bind MyItemsSource}" .../>
</ScrollViewer>
```

```csharp
private async void ScrollViewer_ViewChanged(object sender, ScrollViewerViewChangedEventArgs e)
{
    if (!e.IsIntermediate)
    {
        var scroller = (ScrollViewer)sender;
        var distanceToEnd = scroller.ExtentHeight - (scroller.VerticalOffset + scroller.ViewportHeight);

        // trigger if within 2 viewports of the end
        if (distanceToEnd <= 2.0 * scroller.ViewportHeight
                && MyItemsSource.HasMore && !itemsSource.Busy)
        {
            // show an indeterminate progress UI
            myLoadingIndicator.Visibility = Visibility.Visible;

            await MyItemsSource.LoadMoreItemsAsync(/*DataFetchSize*/);

            loadingIndicator.Visibility = Visibility.Collapsed;
        }
    }
}
```

## <a name="change-the-layout-of-items"></a>變更項目的配置

所顯示的項目[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)將會依照排列[版面配置](/uwp/api/microsoft.ui.xaml.controls.layout)管理的大小及位置其子元素的物件。 ItemsRepeater 搭配使用時，此配置物件啟用 UI 虛擬化。 提供的版面配置都會[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout)並[UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout)。 根據預設，ItemsRepeater 會使用垂直方向 StackLayout。

### <a name="stacklayout"></a>StackLayout

[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout)項目排列成單一行，您可以設定水平或垂直。

您可以設定[間距](/en-us/uwp/api/microsoft.ui.xaml.controls.stacklayout.spacing)調整項目之間的空間數量的屬性。 間距已套用的版面配置方向[方向](/uwp/api/microsoft.ui.xaml.controls.stacklayout.orientation)。

![堆疊配置間距](images/stack-layout.png)

此範例示範如何將 ItemsRepeater.Layout 屬性設定為使用水平方向和 8 個像素的間距 StackLayout。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}" ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:StackLayout Orientation="Horizontal" Spacing="8"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

### <a name="uniformgridlayout"></a>UniformGridLayout

[UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout)文繞圖版面配置中會以循序方式將元素。 項目是從左到右的順序配置時[方向](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.orientation)是**水平**，和配置上到下方向時**垂直**。 每個項目比例縮放。

![統一的格線版面配置間距](images/uniform-grid-layout.png)

水平版面配置的每個資料列中的項目數會受到最小的項目寬度。 在垂直版面配置的每個資料行中的項目數會受到最小的項目高度。

- 您可以明確提供要藉由設定使用的最小大小[MinItemHeight](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemheight)並[MinItemWidth](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemwidth)屬性。
- 如果您未指定最小的大小，第一個項目的測量的大小會被視為最小的大小，每個項目。

您也可以設定版面配置，以藉由設定包含資料列和資料行之間的最小間距[MinColumnSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.mincolumnspacing)並[MinRowSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minrowspacing)屬性。

![統一的格線大小和間距](images/uniform-grid-sizing-spacing.png)

如果尚未決定資料列或資料行中的項目數目會根據項目的最小大小和間距之後，可能會有未使用 （如先前的映像所示），資料列或資料行中的最後一個項目之後剩餘的空間。 您可以指定任何額外的空間會忽略，用來放大每個項目，或用來建立項目之間的額外空間。 這由控制[ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch)並[ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification)屬性。

您可以設定[ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch)屬性來指定如何將項目大小增加以填滿未使用的空間。

此清單會顯示可用的值。 定義假設使用預設**方向**的**水平**。

- **無**：額外的空間會保持未使用的資料列結尾。 這是預設值。
- **填滿**:項目會提供額外的寬度，以用完可用空間 （高度如果垂直）。
- **統一**:項目會提供額外的寬度，以用完可用空間，並提供額外的高度維持外觀比例 （高度和寬度切換如果垂直）。

下圖顯示的效果**ItemsStretch**水平版面配置中的值。

![統一的方格項目延伸](images/uniform-grid-item-stretch.png)

當**ItemsStretch**是**無**，您可以設定[ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification)屬性來指定如何額外空間用來對齊項目。

此清單會顯示可用的值。 定義假設使用預設**方向**的**水平**。

- **啟動**:項目會對齊資料列的開頭。 額外的空間會保持未使用的資料列結尾。 這是預設值。
- **Center**:項目是資料列的置中對齊。 開始和結束的資料列平均分配額外的空間。
- **結束**:與資料列結尾對齊項目。 額外的空間保留未使用的資料列的開頭。
- **SpaceAround**:項目會平均分散。 等量的空間會加入之前和之後每個項目。
- **SpaceBetween**:項目會平均分散。 每個項目之間會加上等量的空間。 開始和結束的資料列加入沒有空格。
- **SpaceEvenly**:使用等量的空間，每個項目間以及開始和結束的資料列平均分散的項目。

下圖顯示的效果**ItemsStretch** （套用至資料行，而非資料列） 的垂直版面配置中的值。

![統一的格線項目對齊](images/uniform-grid-item-justification.png)

> [!TIP]
> **ItemsStretch**屬性會影響_量值_的版面配置傳遞。 **ItemsJustification**屬性會影響_排列_的版面配置傳遞。

此範例示範如何設定**ItemsRepeater.Layout**屬性設**UniformGridLayout**。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                    ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:UniformGridLayout MinItemWidth="200"
                                MinColumnSpacing="28"
                                ItemsJustification="SpaceAround"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

## <a name="lifecycle-events"></a>生命週期事件

當您裝載中的項目[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)、 您可能需要採取某些動作，會顯示，或停止所顯示的項目時，例如當開始非同步下載一些內容，使項目與機制，以追蹤選取範圍，或停止某個背景工作。

在虛擬化的控制項中，您不能依賴載入/卸載事件因為它已回收時，項目可能會移除從即時視覺化樹狀結構。 相反地，若要管理的項目生命週期提供其他事件。 ItemsRepeater，並引發相關的事件時，此圖表會顯示項目生命的週期。

![生命週期事件的圖表](images/items-repeater-lifecycle.png)

- [**ElementPrepared** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementprepared)會在每次項目可供使用。 這是新建立的項目以及已經存在，而且正在回收佇列中重新使用的項目。
- [**ElementClearing** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementclearing)元素已傳送至資源回收佇列中，例如當它超出範圍的每次發現的項目立即發生。
- [**ElementIndexChanged** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementindexchanged
)針對每個發現它所代表的項目索引已變更的 UIElement，就會發生。 比方說，當新增或移除資料來源中另一個項目，項目之後的排序索引就會收到此事件。

這個範例會示範如何使用這些事件將自訂的選取項目服務，來追蹤中顯示的項目使用 ItemsRepeater 的自訂控制項的項目選取項目。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<UserControl ...>
    ...
    <ScrollViewer>
        <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                            ItemTemplate="{StaticResource MyTemplate}"
                            ElementPrepared="OnElementPrepared"
                            ElementIndexChanged="OnElementIndexChanged"
                            ElementClearing="OnElementClearing">
        </muxc:ItemsRepeater>
    </ScrollViewer>
    ...
</UserControl>
```

```csharp
interface ISelectable
{
    int SelectionIndex { get; set; }
    void UnregisterSelectionModel(SelectionModel selectionModel);
    void RegisterSelectionModel(SelectionModel selectionModel);
}

private void OnElementPrepared(ItemsRepeater sender, ElementPreparedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Wire up this item to recognize a 'select' and listen for programmatic
        // changes to the selection model to know when to update its visual state.
        selectable.SelectionIndex = args.Index;
        selectable.RegisterSelectionModel(this.SelectionModel);
    }
}

private void OnElementIndexChanged(ItemsRepeater sender, ElementIndexChangedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Sync the ID we use to notify the selection model when the item
        // we represent has changed location in the data source.
        selectable.SelectionIndex = args.NewIndex;
    }
}

private void OnElementClearing(ItemsRepeater sender, ElementClearingEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Disconnect handlers to recognize a 'select' and stop
        // listening for programmatic changes to the selection model.
        selectable.UnregisterSelectionModel(this.SelectionModel);
        selectable.SelectionIndex = -1;
    }
}
```

## <a name="sorting-filtering-and-resetting-the-data"></a>排序、 篩選及重設資料

當您執行動作，例如篩選或排序資料集時，您傳統上可能會有比先前的一組資料至新的資料，則發出透過細微的變更通知[INotifyCollectionChanged](/uwp/api/windows.ui.xaml.interop.inotifycollectionchanged)。 不過，通常會完全取代舊的資料，以新的資料，並觸發程序集合變更通知使用變得更加容易[重設](/uwp/api/windows.ui.xaml.interop.notifycollectionchangedaction)動作改。

一般而言，重設會導致發行現有的子項目並重頭開始，建置在捲軸位置 0 開始的 UI，因為它具有不知道到底如何資料有所變更時重設控制項。

不過，如果集合已指派為 ItemsSource 支援唯一的識別項藉由實作[IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)介面，則可以快速識別 ItemsRepeater:

- 針對存在於之前和之後重設資料的可重複使用 UIElements
- 先前顯示的項目已移除
- 加入新項目，將會顯示

這可讓 ItemsRepeater 避免從頭開始從捲軸位置為 0。 它也可讓它快速還原的 Uielement，重設，在未變更的資料產生更好的效能。

此範例示範如何顯示垂直堆疊中的項目清單何處_MyItemsSource_是包裝的項目基礎清單的自訂資料來源。 它會公開_資料_可用來重新指派新的清單，以做為項目來源，再觸發重設的屬性。

```xaml
<ScrollViewer x:Name="sv">
    <ItemsRepeater x:Name="repeater"
                ItemsSource="{x:Bind MyItemsSource}"
                ItemTemplate="{StaticResource MyTemplate}">
       <ItemsRepeater.Layout>
           <StackLayout ItemSpacing="8"/>
       </ItemsRepeater.Layout>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Similar to an ItemsControl, a developer sets the ItemsRepeater's ItemsSource.
    // Here we provide our custom source that supports unique IDs which enables
    // ItemsRepeater to be smart about handling resets from the data.
    // Unique IDs also make it easy to do things apply sorting/filtering
    // without impacting any state (i.e. selection).
    MyItemsSource myItemsSource = new MyItemsSource(data);

    repeater.ItemsSource = myItemsSource;

    // ...

    // We can sort/filter the data using whatever mechanism makes the
    // most sense (LINQ, database query, etc.) and then reassign
    // it, which in our implementation triggers a reset.
    myItemsSource.Data = someNewData;
}

// ...


public class MyItemsSource : IReadOnlyList<ItemBase>, IKeyIndexMapping, INotifyCollectionChanged
{
    private IList<ItemBase> _data;

    public MyItemsSource(IEnumerable<ItemBase> data)
    {
        if (data == null) throw new ArgumentNullException();

        this._data = data.ToList();
    }

    public IList<ItemBase> Data
    {
        get { return _data; }
        set
        {
            _data = value;

            // Instead of tossing out existing elements and re-creating them,
            // ItemsRepeater will reuse the existing elements and match them up
            // with the data again.
            this.CollectionChanged?.Invoke(
                this,
                new NotifyCollectionChangedEventArgs(NotifyCollectionChangedAction.Reset));
        }
    }

    #region IReadOnlyList<T>

    public ItemBase this[int index] => this.Data != null
        ? this.Data[index]
        : throw new IndexOutOfRangeException();

    public int Count => this.Data != null ? this.Data.Count : 0;
    public IEnumerator<ItemBase> GetEnumerator() => this.Data.GetEnumerator();
    IEnumerator IEnumerable.GetEnumerator() => this.GetEnumerator();

    #endregion

    #region INotifyCollectionChanged

    public event NotifyCollectionChangedEventHandler CollectionChanged;

    #endregion

    #region IKeyIndexMapping

    private int lastRequestedIndex = IndexNotFound;
    private const int IndexNotFound = -1;

    // When UniqueIDs are supported, the ItemsRepeater caches the unique ID for each item
    // with the matching UIElement that represents the item.  When a reset occurs the
    // ItemsRepeater pairs up the already generated UIElements with items in the data
    // source.
    // ItemsRepeater uses IndexForUniqueId after a reset to probe the data and identify
    // the new index of an item to use as the anchor.  If that item no
    // longer exists in the data source it may try using another cached unique ID until
    // either a match is found or it determines that all the previously visible items
    // no longer exist.
    public int IndexForUniqueId(string uniqueId)
    {
        // We'll try to increase our odds of finding a match sooner by starting from the
        // position that we know was last requested and search forward.
        var start = lastRequestedIndex;
        for (int i = start; i < this.Count; i++)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        // Then try searching backward.
        start = Math.Min(this.Count - 1, lastRequestedIndex);
        for (int i = start; i >= 0; i--)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        return IndexNotFound;
    }

    public string UniqueIdForIndex(int index)
    {
        var key = this[index].PrimaryKey;
        lastRequestedIndex = index;
        return key;
    }

    #endregion
}

```

## <a name="create-a-custom-collection-control"></a>建立自訂集合的控制項

您可以使用[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)建立完成，但它自己的控制項來呈現每個項目類型的自訂集合控制項。

> [!NOTE]
> 這是類似於使用**ItemsControl**，但而不是衍生自**ItemsControl**放**ItemsPresenter**在控制項範本中，您會衍生自**控制項**，並插入**ItemsRepeater**控制項範本中。 自訂集合控制項 」 具有" **ItemsRepeater**與 「 是 」 **ItemsControl**。 這表示您必須明確地選擇哪些屬性，以公開，而不是其繼承屬性，以不支援。

此範例示範如何將放[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)名為自訂控制項範本中_MediaCollectionView_並公開其屬性。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Style TargetType="local:MediaCollectionView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="local:MediaCollectionView">
                <Border
                    Background="{TemplateBinding Background}"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer">
                        <muxc:ItemsRepeater x:Name="ItemsRepeater"
                                            ItemsSource="{TemplateBinding ItemsSource}"
                                            ItemTemplate="{TemplateBinding ItemTemplate}"
                                            Layout="{TemplateBinding Layout}"
                                            TabFocusNavigation="{TemplateBinding TabFocusNavigation}"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

```csharp
public sealed class MediaCollectionView : Control
{
    public object ItemsSource
    {
        get { return (object)GetValue(ItemsSourceProperty); }
        set { SetValue(ItemsSourceProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemsSource.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemsSourceProperty =
        DependencyProperty.Register(nameof(ItemsSource), typeof(object), typeof(MediaCollectionView), new PropertyMetadata(0));

    public DataTemplate ItemTemplate
    {
        get { return (DataTemplate)GetValue(ItemTemplateProperty); }
        set { SetValue(ItemTemplateProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemTemplate.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemTemplateProperty =
        DependencyProperty.Register(nameof(ItemTemplate), typeof(DataTemplate), typeof(MediaCollectionView), new PropertyMetadata(0));

    public Layout Layout
    {
        get { return (Layout)GetValue(LayoutProperty); }
        set { SetValue(LayoutProperty, value); }
    }

    // Using a DependencyProperty as the backing store for Layout.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty LayoutProperty =
        DependencyProperty.Register(nameof(Layout), typeof(Layout), typeof(MediaCollectionView), new PropertyMetadata(0));

    public MediaCollectionView()
    {
        this.DefaultStyleKey = typeof(MediaCollectionView);
    }
}
```

## <a name="display-grouped-items"></a>顯示群組的項目

您可以巢狀[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)中[ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate)的另一個 ItemsRepeater 建立巢狀虛擬化版面配置。 架構會藉由減少不必要的實現，看不到的項目，或接近目前檢視區讓有效率地使用資源。

這個範例會示範如何顯示的群組項目清單，以及在垂直堆疊中。 外部 ItemsRepeater 會產生每個群組。 在範本中的每個群組，另一個 ItemsRepeater 會產生項目。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind AppNotifications}"
                      Layout="{StaticResource MyGroupLayout}">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="ExampleApp:AppNotifications">
        <!-- Group -->
        <StackPanel>
          <!-- Header -->
          TextBlock Text="{x:Bind AppTitle}"/>
          <!-- Items -->
          <muxc:ItemsRepeater ItemsSource="{x:Bind Notifications}"
                              Layout="{StaticResource MyItemLayout}"
                              ItemTemplate="{StaticResource MyTemplate}"/>
          <!-- Footer -->
          <Button Content="{x:Bind FooterText}"/>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```

這個範例會示範已隨著使用者喜好設定，並會顯示為水平捲動清單，如下所示的各種類別的應用程式的配置。

![巢狀的版面配置與項目重複項](images/items-repeater-nested-layout.png)

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind Categories}"
                      Background="LightGreen">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="local:Category">
        <StackPanel Margin="12,0">
          <TextBlock Text="{x:Bind Name}" Style="{ThemeResource TitleTextBlockStyle}"/>
          <!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
          <ScrollViewer HorizontalScrollMode="Enabled"
                                          VerticalScrollMode="Disabled"
                                          HorizontalScrollBarVisibility="Auto" >
            <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                                Background="Orange">
              <muxc:ItemsRepeater.ItemTemplate>
                <DataTemplate x:DataType="local:CategoryItem">
                  <Grid Margin="10"
                        Height="60" Width="120"
                        Background="LightBlue">
                    <TextBlock Text="{x:Bind Name}"
                               Style="{StaticResource SubtitleTextBlockStyle}"
                               Margin="4"/>
                  </Grid>
                </DataTemplate>
              </muxc:ItemsRepeater.ItemTemplate>
              <muxc:ItemsRepeater.Layout>
                <muxc:StackLayout Orientation="Horizontal"/>
              </muxc:ItemsRepeater.Layout>
            </muxc:ItemsRepeater>
          </ScrollViewer>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```

## <a name="bringing-an-element-into-view"></a>項目帶入檢視

XAML 架構已處理 1） 接收到鍵盤焦點或 2） 取得焦點 [朗讀程式] 時 FrameworkElement 帶入檢視。 可能有其他情況下，您需要明確地將項目帶入檢視。 比方說，以回應使用者動作，或在網頁巡覽後還原 UI 狀態。

虛擬化的項目帶入檢視涉及下列工作：
1. 了解的 UIElement 項目
2. 執行版面配置，以確保項目具有有效的位置
3. 起始要求來具現化的項目帶入檢視

下列範例示範在網頁巡覽後還原的一般、 垂直清單中的項目捲軸位置的這些步驟。 在使用巢狀的 ItemsRepeaters 的階層式資料的情況下，方法基本上相同，但是必須在每個階層層級執行。

```xaml
<ScrollViewer x:Name="scrollviewer">
  <ItemsRepeater x:Name="repeater" .../>
</ScrollViewer>
```

```csharp
public class MyPage : Page
{
    // ...

    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        base.OnNavigatedTo(e);

            // retrieve saved offset + index(es) of the tracked element and then bring it into view.
            // ... 

            var element = repeater.GetOrCreateElement(index);

            // ensure the item is given a valid position
            element.UpdateLayout();

            element.StartBringIntoView(new BringIntoViewOptions()
            {
                VerticalOffset = relativeVerticalOffset
            });
    }

    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        base.OnNavigatingFrom(e);

        // retrieve and save the relative offset and index(es) of the scrollviewer's current anchor element ...
        var anchor = this.scrollviewer.CurrentAnchor;
        var index = this.repeater.GetElementIndex(anchor);
        var anchorBounds = anchor.TransformToVisual(this.scrollviewer).TransformBounds(new Rect(0, 0, anchor.ActualWidth, anchor.ActualHeight));
        relativeVerticalOffset = this.sv.VerticalOffset – anchorBounds.Top;
    }
}

```

## <a name="enable-accessibility"></a>啟用協助工具

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)不會提供預設的協助工具體驗。 上的文件[UWP 應用程式的使用性](/windows/uwp/design/usability)提供豐富的資訊可協助您確保您的應用程式提供內含使用者體驗。 如果您用來建立自訂控制項 ItemsRepeater 請務必查看文件上[自訂自動化對等](/windows/uwp/design/accessibility/custom-automation-peers)。

### <a name="keyboarding"></a>鍵盤輸入
ItemsRepeater 提供的焦點移動的最小 keyboarding 支援根據 XAML 的[Keyboarding 的 2D 方向式巡覽](/windows/uwp/design/input/focus-navigation#2d-directional-navigation-for-keyboard)。

![方向導覽](/windows/uwp/design/input/images/keyboard/directional-navigation.png)

ItemsRepeater [XYFocusKeyboardNavigation 模式](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)是_已啟用_預設。 根據預期的體驗，請考慮將常見[鍵盤互動](/windows/uwp/design/input/keyboard-interactions)Home、 End、 PageUp，等。

ItemsRepeater 自動確保，預設的定位順序，其項目 （不論是虛擬化與否） 如下所示的相同順序來提供資料的項目。 依預設具有 ItemsRepeater 及其[TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement.tabfocusnavigation)屬性設定為[一次](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode)而不是常見的預設值是_本機_。

> [!NOTE]
> ItemsRepeater 自動不記得最後一個焦點的項目。  這表示當使用者正在使用 Shift + Tab 便可能到最後一個進入實現項目。

### <a name="announcing-item-x-of-y-in-screen-readers"></a>宣布 「 項目_X_的_Y_」 在螢幕助讀程式

您需要管理設定適當的自動化屬性，例如值**PositionInSet**並**SizeOfSet**，並確保它們保持最新狀態的項目新增時，移動、 移除、 等等。

某些自訂的版面配置中可能不會有明顯的順序，以視覺化的順序。  使用者以最低限度應該由螢幕助讀員 PositionInSet 和 SizeOfSet 屬性的值會符合的項目 （1，以符合自然計數與 0 為基礎的位移） 的資料中出現的順序。

若要達到此目的，最好是讓項目控制項實作自動化對等個體[GetPositionInSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpositioninsetcore)並[GetSizeOfSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getsizeofsetcore)方法和報表資料集中項目的位置表示控制項。 在執行階段時存取的輔助技術只計算的值，並讓它保持在最新狀態會變成問題。 值必須符合的資料順序。

此範例示範如何可以這麼做時顯示自訂控制項稱為_CardControl_。

```xaml
<ScrollViewer >
    <ItemsRepeater x:Name="repeater" ItemsSource="{x:Bind MyItemsSource}">
       <ItemsRepeater.ItemTemplate>
           <DataTemplate x:DataType="local:CardViewModel">
               <local:CardControl Item="{x:Bind}"/>
           </DataTemplate>
       </ItemsRepeater.ItemTemplate>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
internal sealed class CardControl : CardControlBase
{
    protected override AutomationPeer OnCreateAutomationPeer() => new CardControlAutomationPeer(this);

    private sealed class CardControlAutomationPeer : FrameworkElementAutomationPeer
    {
        private readonly CardControl owner;

        public CardControlAutomationPeer(CardControl owner) : base(owner) => this.owner = owner;

        protected override int GetPositionInSetCore()
          => ((ItemsRepeater)owner.Parent)?.GetElementIndex(this.owner) + 1 ?? base.GetPositionInSetCore();

        protected override int GetSizeOfSetCore()
          => ((ItemsRepeater)owner.Parent)?.ItemsSourceView?.Count ?? base.GetSizeOfSetCore();
    }
}
```

## <a name="related-articles"></a>相關文章

- [清單](lists.md)
- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
