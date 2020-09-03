---
Description: 透過使用者輸入篩選集合中的項目。
title: 篩選集合
label: Filtering collections
template: detail.hbs
ms.date: 12/3/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: b1ffa6374753343321f34d388eb994a62614cb15
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172601"
---
# <a name="filtering-collections-and-lists-through-user-input"></a>透過使用者輸入篩選集合和清單
如果集合顯示許多項目，或高度繫結至使用者互動，則篩選是一項可實作的實用功能。 使用本文所述的方法進行篩選可對大部分的集合控制項實作，包括 [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView)、[GridView](/uwp/api/windows.ui.xaml.controls.gridview) 和 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)。 許多類型的使用者輸入都可以用來篩選集合 (例如核取方塊、選項按鈕和滑杆)，但本文將著重於採用以文字為基礎的使用者輸入，並根據使用者的搜尋，將其用於即時更新 ListView。 

> [!NOTE]
> 本文將著重於使用 ListView 進行篩選。 請注意，此篩選方法也可套用至其他集合控制項，例如 GridView、ItemsRepeater 或 TreeView。

## <a name="setting-up-the-ui-and-xaml-for-filtering"></a>設定 UI 和 XAML 以進行篩選
若要實作篩選，您的應用程式應該要有一個 ListView 出現在允許使用者輸入的 TextBox 或其他控制項旁邊。 使用者在 TextBox 中輸入的文字將會作為篩選準則，也就是只會顯示包含其文字輸入/搜尋查詢的結果。 當使用者在 TextBox 中輸入時，ListView 會持續以篩選後的結果進行更新，特別是在每次 TextBox 中的文字變更時，即使是一個字母，ListView 也會瀏覽其項目並使用該字詞進行篩選。

下列程式碼顯示具有簡單 ListView 和其 DataTemplate 的 UI，以及隨附的 TextBox。 在此範例中，ListView 會顯示 Person 物件的集合。 Person 是程式碼後置中定義的類別 (未顯示在下面的程式碼範例中)，而每個 Person 物件都具有下列屬性：FirstName、LastName 和 Company。

使用者可以使用 TextBox 來輸入搜尋/篩選字詞，以便依姓氏篩選 Person 物件的清單。 請注意，TextBox 會繫結至特定名稱 (`FilterByLName`)，並有自己的 TextChanged 事件 (`FilteredLV_LNameChanged`)。 繫結的名稱可讓我們存取程式碼後置中 TextBox 的內容/文字，而每當使用者在 TextBox 中輸入時，就會引發 TextChanged 事件，讓我們能在接收使用者輸入時執行篩選作業。 

若要讓篩選運作，ListView 必須具有可在程式碼後置中操作的資料來源，例如 `ObservableCollection<>`。 在此情況下，ListView 的 ItemsSource 屬性會指派給程式碼後置中的 `ObservableCollection<Person>`。 

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="1*"></ColumnDefinition>
        <ColumnDefinition Width="1*"></ColumnDefinition>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
            <RowDefinition Height="400"></RowDefinition>
            <RowDefinition Height="400"></RowDefinition>
    </Grid.RowDefinitions>

    <ListView x:Name="FilteredListView"
                Grid.Column="0"
                Margin="0,0,20,0">

        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Person">
                <StackPanel>
                    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" Margin="0,5,0,5">
                        <Run Text="{x:Bind FirstName}"></Run>
                        <Run Text="{x:Bind LastName}"></Run>
                    </TextBlock>
                    <TextBlock Style="{ThemeResource BodyTextBlockStyle}" Margin="0,5,0,5" Text="{x:Bind Company}"/>
                </StackPanel>
            </DataTemplate>
        </ListView.ItemTemplate>

    </ListView>

    <TextBox x:Name="FilterByLName" Grid.Column="1" Header="Last Name" Width="200"
             HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0,0,0,20"
             TextChanged="FilteredLV_LNameChanged"/>
</Grid>
```
## <a name="filtering-the-data"></a>篩選資料
[Linq](/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries) 查詢可讓您對集合中的特定項目進行分組、排序和選取。 為了篩選清單，我們會建立一個 Linq 查詢，其只會選取符合使用者輸入搜尋查詢/篩選字詞 (在 `FilterByLName` TextBox 中輸入) 的字詞。 您可將查詢結果指派給 [IEnumerable<T>](/dotnet/api/system.collections.generic.ienumerable-1) 集合物件。 一旦擁有這個集合，我們就可以使用它來與原始清單進行比較，移除不相符的項目並加回相符的項目 (如果是退格鍵)。

> [!NOTE]
> 為了讓 ListView 在增減項目時以最直覺的方式動畫呈現，請務必移除項目並將其新增至 ListView 的 ItemsSource 集合本身，而不是建立新的篩選後物件集合，並將其指派給ListView 的 ItemsSource 屬性。

首先，我們必須在不同的集合中初始化原始資料來源，例如 `List<T>` 或 `ObservableCollection<T>`。 在此範例中，我們有一個稱為 `People` 的 `List<Person>`，其會保存 ListView 中顯示的所有 Person 物件 (此清單的擴展/初始化不會顯示在下面的程式碼片段中)。 我們也需要一個清單來保存篩選後的資料，其會在每次套用篩選條件時不斷變更。 這會是稱為 `PeopleFiltered` 的 `ObservableCollection<Person>`，而在初始化時會具有與 `People` 相同的內容。
 
以下程式碼會透過下列步驟來執行篩選作業，如以下程式碼所示：
 - 將 ListView 的 ItemsSource 屬性設定為 `PeopledFiltered`。 
 - 定義 `FilterByLName` TextBox 的 TextChanged 事件 `FilteredLV_LNameChanged()`。 在此函式中，篩選資料。
 - 若要篩選資料，請透過 `FilterByLName.Text` 存取使用者輸入的搜尋查詢/篩選字詞。 使用 Linq 查詢來選取 `People` 中其姓氏包含 `FilterByLName.Text` 字詞的項目，並將這些相符項目新增至名為 `TempFiltered` 的集合中。
 - 比較目前的 `PeopleFiltered` 集合與 `TempFiltered` 中新篩選的項目，並在必要時從 `PeopleFiltered` 移除和新增項目。
 - 在 `PeopleFiltered` 中移除和新增項目時，ListView 會據以更新並以動畫呈現。

 ```csharp
using System.Linq;

public MainPage()
{
    // Define People collection to hold all Person objects. 
    // Populate collection - i.e. add Person objects (not shown)
    IList<Person> People = new List<Person>();

    // Create PeopleFiltered collection and copy data from original People collection
    ObservableCollection<Person> PeopleFiltered = new ObservableCollection<Person>(People);

    // Set the ListView's ItemsSource property to the PeopleFiltered collection
    FilteredListView.ItemsSource = PeopleFiltered;

    // ... 
}

private void FilteredLV_LNameChanged(object sender, TextChangedEventArgs e)
{
    /* Perform a Linq query to find all Person objects (from the original People collection)
    that fit the criteria of the filter, save them in a new List called TempFiltered. */
    List<Person> TempFiltered;
    
    /* Make sure all text is case-insensitive when comparing, and make sure 
    the filtered items are in a List object */
    TempFiltered = people.Where(contact => contact.LastName.Contains(FilterByLName.Text, StringComparison.InvariantCultureIgnoreCase)).ToList();
    
    /* Go through TempFiltered and compare it with the current PeopleFiltered collection,
    adding and subtracting items as necessary: */

    // First, remove any Person objects in PeopleFiltered that are not in TempFiltered
    for (int i = PeopleFiltered.Count - 1; i >= 0; i--)
    {
        var item = PeopleFiltered[i];
        if (!TempFiltered.Contains(item))
        {
            PeopleFiltered.Remove(item);
        }
    }

    /* Next, add back any Person objects that are included in TempFiltered and may 
    not currently be in PeopleFiltered (in case of a backspace) */

    foreach (var item in TempFiltered)
    {
        if (!PeopleFiltered.Contains(item))
        {
            PeopleFiltered.Add(item);
        }
    }
}
 ```

現在，當使用者在 `FilterByLName` TextBox 中輸入篩選字詞時，ListView 會立即更新，只顯示姓氏包含篩選字詞的人員。

## <a name="next-steps"></a>接下來的步驟

### <a name="get-the-sample-code"></a>取得範例程式碼
- 如果您已安裝 XAML 控制項庫</strong>應用程式，請按一下[這裡](xamlcontrolsgallery:/item/ListView)以開啟應用程式並查看 ListView 頁面上更強固、深入的清單篩選範例。
- [取得 XAML 控制項庫應用程式 (Microsoft Store)](https://www.microsoft.com/store/productId/9MSVH128X2ZT)。

### <a name="related-articles"></a>相關文章
- [清單](lists.md)
- [清單檢視和方格檢視](listview-and-gridview.md)
- [集合命令](collection-commanding.md)