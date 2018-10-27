---
author: QuinnRadich
title: 學習追蹤 - 顯示清單中的客戶
description: 了解您需要如何做來顯示清單中 Customer 物件的收藏。
ms.author: quradic
ms.date: 05/07/2018
ms.topic: article
keywords: 開始使用, uwp, windows 10, 學習追蹤, 資料繫結, 清單
ms.localizationpriority: medium
ms.openlocfilehash: 4e58231d644616a088eb06f24a2465c240e10c16
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5694581"
---
# <a name="display-customers-in-a-list"></a>顯示清單中的客戶

顯示和操作實際 UI 中的資料，這對於許多應用程式的功能是很重要的。 此文章顯示您需要如何做來顯示清單中 Customer 物件的收藏。

這不是教學課程。 如果您需要，請參閱我們的[資料繫結教學課程](../data-binding/xaml-basics-data-binding.md)，其將為您提供逐步導覽體驗。

我們將開始快速討論資料繫結 - 它的內容和運作方式。 然後我們將新增 **List View** 至 UI、新增資料繫結，以及使用額外功能自訂資料繫結。 

## <a name="what-do-you-need-to-know"></a>您需要知道哪些事項？

資料繫結是在其 UI 中顯示應用程式資料的方式。 在您的應用程式中*區分不同的考量*，讓您的 UI 其他程式碼區分開來。 這會建立可更輕鬆讀取和維護的更清楚概念模型。

每個資料繫結有兩個項目：

* 提供要繫結之資料的來源。
* UI 中會顯示資料的目標。

若要實作資料繫結，您將需要新增程式碼至提供資料至繫結的來源。 您也將需要將兩個標記擴充功能的其中一個新增至 XAML，以指定資料來源屬性。 以下是這兩個功能之間的主要差異：

* [**x:Bind**](../xaml-platform/x-bind-markup-extension.md) 是強型別，會在編譯時間產生程式碼以獲得更好的效能。 x:Bind 預設至一次性繫結，其中針對快速顯示未變更的唯讀資料進行最佳化。
* [**繫結**](../xaml-platform/binding-markup-extension.md)是弱型別和執行階段組合。 這所產生的效能比 x:Bind 的更差。 在大部分情況中，您應該使用 x:Bind 而不是 Binding。 不過，您很可能會在較舊的程式碼遇到它。 繫結預設為單向資料傳輸，其針對可在來源變更的唯讀資料進行最佳化。

建議您盡量使用**x:Bind**，而我們會這篇文章中將它中顯示在程式碼片段中。 如需有關差異性的的更多詳細資訊，請參閱 [{x:Bind} 和 {Binding} 功能比較}](../data-binding/data-binding-in-depth.md#xbind-and-binding-feature-comparison)。

## <a name="create-a-data-source"></a>建立資料來源

首先，您需要一個課來代表您的客戶資料。 為了提供您參考點，我們將會在此基礎範例中顯示處理程序：

```csharp
public class Customer
{
    public string Name { get; set; }
}
```

## <a name="create-a-list"></a>建立清單

在向任何客戶展示之前，需要先建立清單予以保存。 [List View]](../design/controls-and-patterns/listview-and-gridview.md) 是基本的 XAML 控制項，相當適用於這項工作。 您的 List View 目前在頁面上需要一個位置，其 **ItemSource** 屬性很快就會需要一個值。

```xaml
<ListView ItemsSource=""
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
```

一旦將資料繫結至您的 List View，建議您回到文件，並嘗試自訂它的外觀和版面配置，以完全符合您的需求。

## <a name="bind-data-to-your-list"></a>資料繫結至您的清單

既然您已經製作出一個基本 UI 來保留您的繫結，您需要設定一個來源提供給它們。 以下示範如何完成上述作法：

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

[資料繫結概觀](../data-binding/data-binding-quickstart.md#binding-to-a-collection-of-items)會在有關繫結至收藏的項目區段中引導您如何依序處理類似的問題。 以下範例顯示重要的執行步驟：

* 在使用者介面的後置程式碼中，建立類型 **ObservableCollection<T>** 的屬性保存您的 Customer 物件。
* 繫結 List View 的 **ItemSource** 至該屬性。
* 提供 List View 的基本 **ItemTemplate**，其將會設定每個項目在清單中的顯示方式。

如果您想要自訂版面配置，新增選取項目，或調整您剛建立的 **DataTemplate**，歡迎回來查看 [List View]](../design/controls-and-patterns/listview-and-gridview.md) 文件。 但是，萬一您要編輯您的客戶時怎麼辦？

## <a name="edit-your-customers-through-the-ui"></a>透過 UI 編輯客戶

您已將客戶顯示在清單中，但資料 B=binding 可讓您完成更多工作。 如果可以直接從 UI 編輯您的資料呢？ 若要這樣做，我們先來討論資料繫結的三個模式：

* *一次性*：此資料繫結只啟用一次，無法回應變更。
* *單向*：此資料繫結將用對資料來源所做的任何變更來更新 UI。
* *雙向*：此資料繫結將用對資料來源所做的任何變更來更新 UI，並且也會用在 UI 內所做的任何變更來更新資料。

如果您已按照從先前的程式碼片段，您所做的繫結會使用 x:Bind 且不指定模式，使其變成一次繫結。 如果您想要直接從 UI 編輯您的客戶，您需要將它變更為雙向繫結，使資料的變更傳送回 Customer 物件。 [深入了解資料繫結](../data-binding/data-binding-in-depth.md)中提供更多資訊。

若資料來源已變更，雙向繫結也會更新 UI。 為了能夠運作，您必須執行來源的 [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged(d=robot).aspx)，並確定其屬性 setter 提出 **PropertyChanged** 事件。 常見的作法是讓它們呼叫像 **OnPropertyChanged** 方法一樣的協助程式方法，如下所示：

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
然後，使用 **TextBox** 而不是 **TextBlock**，使 List View 中的文字變成可編輯，並確定將您的資料繫結上的**模式**設定為 **TwoWay**。

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

快速確認此工作是新增含 TextBox 控制項及與 OneWay 繫結的第二個 ListView。 當您編輯第一個清單時，將會自動變更第二個清單中的值。

> [!NOTE]
> 直接編輯 ListView 內部，是顯示動作中的雙向繫結的簡單方式，但可能會導致可用性的複雜化。 如果您想要讓應用程式有更進一步發揮，請考慮使用[其他 XAML 控制項](../design/controls-and-patterns/controls-and-events-intro.md)編輯資料，並保留 ListView 為僅顯示。

## <a name="going-further"></a>更進一步

既然您已經建立具雙向繫結的客戶清單，歡迎回到我們已經連結您並做實驗的文件。 您也可以查看我們的[資料繫結教學課程](../data-binding/xaml-basics-data-binding.md)如果您想要逐步解說的基本和進階繫結，或調查像[主要/詳細資訊模式](../design/controls-and-patterns/master-details.md)的控制項，以使 UI 更穩定。

## <a name="useful-apis-and-docs"></a>實用的 API 和文件

以下是 API 的快速摘要，以及其他實用的文件，有助於您開始使用資料繫結。

### <a name="useful-apis"></a>實用的 API

| API | 說明 |
|------|---------------|
| [資料範本](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) | 描述資料物件的視覺化結構，允許顯示 UI 中的特定元素。 |
| [x:Bind](../xaml-platform/x-bind-markup-extension.md) | 關於建議的 x:Bind 標記延伸的文件。 |
| [Binding](../xaml-platform/binding-markup-extension.md) | 關於較舊的 Binding 標記延伸的文件。 |
| [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) | 以垂直堆疊顯示資料項目的 UI 控制項。 |
| [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | 用於在 UI 中顯示可編輯文字資料的控制項的基本文字。 |
| [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged(d=robot).aspx) | 使資料變成可觀察的介面，提供至資料繫結。 |
| [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) | 此類別的 **ItemsSource** 屬性允許 ListView 繫結至資料來源。 |

### <a name="useful-docs"></a>實用的文件

| 主題 | 說明 |
|-------|----------------|
| [深入了解資料繫結](../data-binding/data-binding-in-depth.md) | 資料繫結原則的基本概觀 |
| [資料繫結概觀](../data-binding/data-binding-quickstart.md) | 有關資料繫結的詳細概念資訊。 |
| [清單檢視](../design/controls-and-patterns/listview-and-gridview.md) | 有關建立和設定 ListView 的資訊，包括實作 **DataTemplate** |

## <a name="useful-code-samples"></a>實用的程式碼範例

| 程式碼範例 | 說明 |
|-----------------|---------------|
| [資料繫結教學課程](../data-binding/xaml-basics-data-binding.md) | 透過基本資料繫結的逐步引導體驗。 |
| [ListView 和 GridView](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) | 透過資料繫結探索更詳細的 ListView。 |
| [QuizGame](https://github.com/Microsoft/Windows-appsample-networkhelper) | 查看動作中的資料繫結，包括標準實作 **INotifyPropertyChanged** 的 **BindableBase** 類別 (在 [Common] 資料夾中)。 |
