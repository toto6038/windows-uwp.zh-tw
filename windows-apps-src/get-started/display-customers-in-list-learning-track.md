---
title: 學習追蹤 - 在清單中顯示客戶
description: 了解您必須執行什麼作業來在清單中顯示 Customer 物件的集合。
ms.date: 05/07/2018
ms.topic: article
keywords: get started, uwp, windows 10, learning track, data binding, list, 開始使用, 學習追蹤, 資料繫結, 清單
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 097105d16d6d17807235ab61d36ab1fe185c8ca3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165052"
---
# <a name="display-customers-in-a-list"></a>在清單中顯示客戶

顯示和操作 UI 中的實際資料，對於許多應用程式的功能來說是很重要的。 本文將示範您必須執行什麼作業來在清單中顯示 Customer 物件的集合。

這不是教學課程。 如果您需要參考教學課程，請參閱我們的[資料繫結教學課程](../data-binding/xaml-basics-data-binding.md)，其能為您提供逐步引導體驗。

作為開始，我們將快速討論資料繫結，包含其內容和運作方式。 然後我們會將 **List View** 新增至 UI，新增資料繫結，然後使用額外功能自訂該資料繫結。

## <a name="what-do-you-need-to-know"></a>您需要知道哪些事項？

資料繫結是在應用程式的 UI 中顯示其資料的方式。 這可在您的應用程式中實現*不同考量的分離*，以讓您的 UI 與其他程式碼區分開來。 這會建立較為清楚的概念模型，以讓您可更輕鬆地讀取和維護它。

每個資料繫結都有兩個部分：

* 提供要繫結之資料的來源。
* 在 UI 中會顯示該資料的目標。

若要實作資料繫結，您需要將程式碼新增至為繫結提供資料的來源。 您也將需要將兩個標記延伸的其中一個新增至 XAML，以指定資料來源屬性。 以下是這兩個功能之間的主要差異：

* [**x:Bind**](../xaml-platform/x-bind-markup-extension.md) 是強型別，其會在編譯時間產生程式碼以提供更好的效能。 x:Bind 預設為一次性繫結，其適用於快速顯示不會變更的唯讀資料。
* [**Binding**](../xaml-platform/binding-markup-extension.md) 是弱型別，並會在執行階段組合。 這會導致比 x:Bind 較差的效能。 在大部分情況中，您應該使用 x:Bind 而不是 Binding。 不過，您很可能會在較舊的程式碼中遇到 Binding。 Binding 預設為單向資料傳輸，其適用於可在來源變更的唯讀資料。

建議您盡量使用 **x:Bind**，而我們會在本文中的程式碼片段中示範它。 如需有關其差異的詳細資訊，請參閱 [{x:Bind} 和 {Binding} 功能比較](../data-binding/data-binding-in-depth.md#xbind-and-binding-feature-comparison)。

## <a name="create-a-data-source"></a>建立資料來源

首先，您需要類別來代表您的客戶資料。 為了提供您參考點，我們將會在這個基礎範例中示範此程序：

```csharp
public class Customer
{
    public string Name { get; set; }
}
```

## <a name="create-a-list"></a>建立清單

在可以顯示任何客戶之前，您需要先建立清單來保存他們。 [List View](../design/controls-and-patterns/listview-and-gridview.md) 是基本的 XAML 控制項，其很適用於這個工作。 您的 ListView 目前在頁面上需要一個位置，且很快就需要適用於其 **ItemSource** 屬性的值。

```xaml
<ListView ItemsSource=""
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
```

當您將資料繫結至您的 ListView 之後，建議您返回文件並嘗試自訂其外觀和版面配置以符合您的需求。

## <a name="bind-data-to-your-list"></a>將資料繫結至您的清單

您已經製作出基本的 UI 來保存您的繫結，現在您需要設定來源來提供它們。 以下範例會示範如何達成此目標：

```csharp
public sealed partial class MainPage : Page
{
    public ObservableCollection<Customer> Customers { get; }
        = new ObservableCollection<Customer>();

    public MainPage()
    {
        this.InitializeComponent();
          // Add some customers
        this.Customers.Add(new Customer() { Name = "NAME1"});
        this.Customers.Add(new Customer() { Name = "NAME2"});
        this.Customers.Add(new Customer() { Name = "NAME3"});
    }
}
```
```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBlock Text="{x:Bind Name}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

[資料繫結概觀](../data-binding/data-binding-quickstart.md#binding-to-a-collection-of-items)在其有關繫結至項目集合的小節中，會引導您處理類似的問題。 我們的範例會示範下列重要步驟：

* 在 UI 的程式碼後置中，建立 **ObservableCollection<T>** 類型的屬性來保存您的 Customer 物件。
* 將 ListView 的 **ItemSource** 繫結至該屬性。
* 為 ListView 提供基本的 **ItemTemplate**，其將會設定每個項目在清單中的顯示方式。

如果您想要自訂版面配置、新增項目選取，或是調整您剛建立的 **DataTemplate**，您可以回頭查看 [ListView](../design/controls-and-patterns/listview-and-gridview.md) 文件。 如果您想要編輯客戶，該怎麼辦？

## <a name="edit-your-customers-through-the-ui"></a>透過 UI 編輯客戶

您已在清單中顯示客戶，但資料繫結可讓您達成更多工作。 您甚至可以直接從 UI 編輯您的資料。 若要這樣做，我們先來討論資料繫結的三個模式：

* *單次*：此資料繫結只會啟用一次，且不會對變更做出反應。
* *單向*：此資料繫結會以對資料來源所做的任何變更來更新 UI。
* *雙向*：此資料繫結會以對資料來源所做的任何變更來更新 UI，也會以在 UI 內所做的任何變更來更新資料。

如果您已按照先前的程式碼片段執行，您所做的繫結會使用 x:Bind 且不指定模式，這使它成為「單次」繫結。 如果您想要直接從 UI 編輯您的客戶，您需要將它變更為「雙向」繫結，來使資料的變更會傳送回 Customer 物件。 [深入了解資料繫結](../data-binding/data-binding-in-depth.md)中會提供詳細資訊。

若資料來源已變更，雙向繫結也會更新 UI。 若樣讓此能夠運作，您必須在來源上實作 [**INotifyPropertyChanged**](/dotnet/api/system.componentmodel.inotifypropertychanged) \(部分機器翻譯\)，並確定其屬性 setter 會引發 **PropertyChanged** 事件。 常見的作法是讓它們呼叫 **OnPropertyChanged** 方法之類的協助程式方法，如下所示：

```csharp
public class Customer : INotifyPropertyChanged
{
    private string _name;
    public string Name
    {
        get => _name;
        set
        {
            if (_name != value)
                {
                    _name = value;
                    this.OnPropertyChanged();
                }
        }
    }
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        this.PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```
然後，使用 **TextBox** 而不是 **TextBlock**，來使 ListView 中的文字可編輯，並確定已將資料繫結上的 **Mode** 設定為 **TwoWay**。

```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBox Text="{x:Bind Name, Mode=TwoWay}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

快速確認是否能正確運作的方法，是新增含有 TextBox 控制項及 OneWay 繫結的第二個 ListView。 當您編輯第一個清單時，系統將會自動變更第二個清單中的值。

> [!NOTE]
> 直接在 ListView 內編輯是確認雙向繫結能成功運作的簡單方式，但這可能會導致可用性的複雜化。 如果您想進一步擴充應用程式的功能，請考慮使用[其他 XAML 控制項](../design/controls-and-patterns/controls-and-events-intro.md)來編輯資料，並將 ListView 保留為僅顯示。

## <a name="going-further"></a>更進一步

您已經建立具有雙向繫結的客戶清單，歡迎返回我們先前為您提供的文件連結，並進一步實驗。 如果您想要基本和進階繫結的逐步解說，也可以查看我們的[資料繫結教學課程](../data-binding/xaml-basics-data-binding.md)；您也可以研究[主要/詳細資訊模式](../design/controls-and-patterns/master-details.md)之類的控制項，以開發出更為強大的 UI。

## <a name="useful-apis-and-docs"></a>實用的 API 和文件

以下是 API 的快速摘要及其他實用的文件，以協助您開始使用資料繫結。

### <a name="useful-apis"></a>實用的 API

| API | 說明 |
|------|---------------|
| [DataTemplate](/uwp/api/Windows.UI.Xaml.DataTemplate) \(英文\) | 描述資料物件的視覺化結構，以允許在 UI 中顯示特定元素。 |
| [x:Bind](../xaml-platform/x-bind-markup-extension.md) | 關於建議的 x:Bind 標記延伸的文件。 |
| [繫結](../xaml-platform/binding-markup-extension.md) | 關於較舊的 Binding 標記延伸的文件。 |
| [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView) \(英文\) | 以垂直堆疊顯示資料項目的 UI 控制項。 |
| [TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox) \(英文\) | 用來在 UI 中顯示可編輯文字資料的基本文字控制項。 |
| [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged) \(英文\) | 使資料變成可觀察，並將其提供至資料繫結的介面。 |
| [ItemsControl](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) \(英文\) | 此類別的 **ItemsSource** 屬性能允許 ListView 繫結至資料來源。 |

### <a name="useful-docs"></a>實用的文件

| 主題 | 說明 |
|-------|----------------|
| [深入了解資料繫結](../data-binding/data-binding-in-depth.md) | 資料繫結原則的基本概觀 |
| [資料繫結概觀](../data-binding/data-binding-quickstart.md) | 有關資料繫結的詳細概念性資訊。 |
| [清單檢視](../design/controls-and-patterns/listview-and-gridview.md) | 建立和設定 ListView 的相關資訊，包括 **DataTemplate** 的實作 |

## <a name="useful-code-samples"></a>實用的程式碼範例

| 程式碼範例 | 說明 |
|-----------------|---------------|
| [資料繫結教學課程](../data-binding/xaml-basics-data-binding.md) | 資料繫結基本概念的逐步引導體驗。 |
| [ListView 和 GridView](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) \(英文\) | 透過資料繫結探索更詳細的 ListView。 |
| [QuizGame](https://github.com/Microsoft/Windows-appsample-networkhelper) \(英文\) | 查看資料繫結的運作方式，包括適用於 **INotifyPropertyChanged** 之標準實作的 **BindableBase** 類別 (位於 Common 資料夾中)。 |