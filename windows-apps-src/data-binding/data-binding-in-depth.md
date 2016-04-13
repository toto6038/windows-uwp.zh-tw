---
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
深入了解資料繫結
資料繫結可讓您的 App UI 顯示資料，以及選擇性地與該資料保持同步。
---
# 深入了解資料繫結

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** 重要 API **

-   [**Binding 類別**](https://msdn.microsoft.com/library/windows/apps/BR209820)
-   [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)
-   [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)

**注意** 這個主題將提供資料繫結功能的詳細說明。 如需簡短且實用的簡介，請參閱[資料繫結概觀](data-binding-quickstart.md)。

 

資料繫結可讓您的 App UI 顯示資料，以及選擇性地與該資料保持同步。 資料繫結可讓您將資料與 UI 分開考量，為 app 建構更簡單的概念模型，以及更好的可讀性、測試性和維護性。

資料繫結可讓您在 UI 最初顯示時，單純地顯示資料來源的值，而不是要回應這些值的變更。 這稱為一次性繫結，適用於執行階段不會變更值的資料。 此外，您可以選擇「觀察」這些值，並於值變更時更新 UI。 這稱為單向繫結，適用於唯讀資料。 最後，您可以選擇同時觀察和更新，將使用者對 UI 中的值所做的變更，自動推回到資料來源。 這稱為雙向繫結，適用於讀寫資料。 以下是一些範例。

-   您可以使用一次性繫結將 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 繫結到目前使用者的相片。
-   您可以使用單向繫結將 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 繫結到依新聞區段分組的即時新聞文章集合。
-   您可以使用雙向繫結將 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 繫結到表單中的客戶名稱。

繫結有兩種，通常都在 UI 標記中宣告。 您可以選擇使用 [{x:Bind} 標記延伸](https://msdn.microsoft.com/library/windows/apps/Mt204783)或 [{Binding} 標記延伸](https://msdn.microsoft.com/library/windows/apps/Mt204782)。 您甚至可以在相同的 app 中將兩者混用 (甚至在相同的 UI 元素上)。 {x:Bind} 是 Windows 10 新增的標記，效能更好。 {Binding} 有更多功能。 本主題所述的所有詳細資料適用於這兩種繫結類型，除非明確指出不是如此。

**示範 {x:Bind} 的範例 app**

-   [{x:Bind} 範例](http://go.microsoft.com/fwlink/p/?linkid=619989)。
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)。
-   [XAML UI 基本知識範例](http://go.microsoft.com/fwlink/p/?linkid=619992)。

**示範 {Binding} 的範例 app**

-   下載 [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950) app。
-   下載 [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) app。

每個繫結包含這些項目
------------------------------------

-   *繫結來源*。 這是繫結的資料來源，可以是任何類別的執行個體，且其成員有您想在 UI 中顯示的值。
-   *繫結目標*。 這是 UI 中用來顯示資料的 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) 的 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362)。
-   *繫結物件*。 此項目將資料值從來源傳輸到目標，也可以從目標傳回到源。 繫結物件是在 XAML 載入時從 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) 或 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 標記延伸建立。

在下列各節，我們將詳細說明繫結來源、繫結目標和繫結物件。 我們以一個範例來串連各節，此範例將一個按鈕的內容繫結到 **HostViewModel** 類別的 **NextButtonText** 字串屬性。

繫結來源
--------------

以下是一個非常基本的類別實作，可做為繫結來源。

**注意** 如果您將 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 與 Visual C++ 元件延伸 (C++/CX) 搭配使用，則需將 [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) 屬性新增到繫結來源類別。 如果使用 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)，則不需該屬性。 請參閱[新增詳細資料檢視](data-binding-quickstart.md#adding-a-details-view)，以取得程式碼片段。

```csharp
public class HostViewModel
{
    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText { get; set; }
}
```

**HostViewModel** 的實作及其屬性 **NextButtonText** 僅適用於一次性繫結。 單向與雙向繫結很常見，在這些繫結類型中，UI 會隨著繫結來源的資料值變更而自動更新。 為了讓這些繫結類型正常運作，您需要讓繫結物件「可觀察」您的繫結來源。 因此，在範例中，如果我們要單向或雙向繫結到 **NextButtonText** 屬性，則該屬性的值在執行階段發生的任何變更，都必須讓繫結物件可觀察。

作法之一是從 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356) 衍生代表繫結來源的類別，並透過 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362) 公開資料值。 這樣 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) 就變成可觀察。 **FrameworkElements** 是很好用的現成繫結來源。

將類別變成可觀察還有更簡便的作法，也是已有基底類別的類別的必要作法，就是實作 [**System.ComponentModel.INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged)。 這只需要實作一個名為 **PropertyChanged** 的事件。 以下是使用 **HostViewModel** 的範例。

**注意** 針對 C++/CX，您實作 [**Windows::UI::Xaml::Data::INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)，且繫結來源類別必須具備 [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) 或實作 [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878)。

``` csharp
public class HostViewModel : INotifyPropertyChanged
{
    private string nextButtonText;

    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set
        {
            this.nextButtonText = value;
            this.OnPropertyChanged();
        }
    }

    public void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        // Raise the PropertyChanged event, passing the name of the property whose value has changed.
        this.PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**NextButtonText** 屬性現在已變成可觀察的。 當您撰寫該屬性的單向或雙向繫結時 (稍後說明作法本)，產生的繫結物件會訂閱 **PropertyChanged** 事件。 該事件引發時，繫結物件的處理常式會收到一個引數，其中包含已變更的屬性的名稱。 就是這樣，繫結物件才知道要再次讀取哪個屬性的值。

因此，您不需要實作上述模式許多次，直接衍生自 [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) 範例 (在 "Common" 資料夾) 中的 **BindableBase** 基底類別即可。 以下是作法的範例。

``` csharp
public class HostViewModel : BindableBase
{
    private string nextButtonText;

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set { this.SetProperty(ref this.nextButtonText, value); }
    }
}
```

以引數 [**String.Empty**](T:System.String) 或 **null** 引發 **PropertyChanged** 事件時，表示應該重新讀取物件上的所有非索引子屬性。 您可以針對特定索引子使用引數 "Item\[*indexer*\]" (其中 *indexer* 是索引值)，或針對所有索引子使用值 "Item\[\]"，以引發事件來指出物件上的索引子屬性已變更。

繫結來源可以視為單一物件 (屬性包含資料) 或物件集合。 在 C# 和 Visual Basic 程式碼中，您可以一次性繫結到實作 [**List(Of T)**](T:System.Collections.Generic.List%601) 的物件，以顯示不會在執行階段變更的集合。 對於可觀察的集合 (觀察集合中新增和移除項目)，則改為單向繫結到 [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601)。 在 C++ 程式碼中，對於可觀察和不可觀察的集合，您都可以繫結到 [**Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/dn858385.aspx)。 如果要繫結到您自己的集合類別，請使用下表中的指導方針。

| 案例                                                        | C# 及 VB (CLR)                                                                                                                                                                                                                                                                                                                                                                                                                   | C++/CX                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 繫結到物件。                                              | 可為任何物件。                                                                                                                                                                                                                                                                                                                                                                                                                 | 物件必須具有 [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) 或實作 [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878)。                                                                                                                                                                                                                                                                                                             |
| 取得繫結物件的屬性變更更新。                | 物件必須實作 [**System.ComponentModel. INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged)。                                                                                                                                                                                                                                                                                                         | 物件必須實作 [**Windows.UI.Xaml.Data. INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)。                                                                                                                                                                                                                                                                                                                                                           |
| 繫結到集合。                                           | [**List(Of T)**](T:System.Collections.Generic.List%601)                                                                                                                                                                                                                                                                                                                                                                            | [**Platform::Collections::Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| 取得繫結集合的集合變更更新。          | [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601)                                                                                                                                                                                                                                                                                                                                        | [**Platform::Collections::Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| 實作支援繫結的集合。                   | 擴充 [**List(Of T)**](T:System.Collections.Generic.List%601) 或實作 [**IList**](T:System.Collections.IList)、[**IList**](T:System.Collections.Generic.IList%601)(Of [**Object**](T:System.Object))、[**IEnumerable**](T:System.Collections.IEnumerable) 或 [**IEnumerable**](T:System.Collections.Generic.IEnumerable%601)(Of **Object**)。 不支援繫結到泛型 **IList(Of T)** 與 **IEnumerable(Of T)**。 | 實作 [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979)、[**IBindableIterable**](https://msdn.microsoft.com/library/windows/apps/Hh701957)、[**IVector**](https://msdn.microsoft.com/library/windows/apps/BR206631)&lt;[**Object**](T:System.Object)^&gt;、[**IIterable**](https://msdn.microsoft.com/library/windows/apps/BR226024)&lt;**Object**^&gt;、**IVector**&lt;[**IInspectable**](https://msdn.microsoft.com/library/BR205821)\*&gt; 或 **IIterable**&lt;**IInspectable**\*&gt;。 不支援繫結到泛型 **IVector&lt;T&gt;** 與 **IIterable&lt;T&gt;**。 |
| 實作可支援集合變更更新的集合。 | 擴充 [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601) 或實作 (非泛型) [**IList**](T:System.Collections.IList) 與 [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged)。                                                                                                                                                               | 實作 [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979) 及 [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974)。                                                                                                                                                                                                                                                                                                                       |
| 實作支援增量載入的集合。       | 擴充 [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601) 或實作 (非泛型) [**IList**](T:System.Collections.IList) 與 [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged)。 此外，也實作 [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)。                                                          | 實作 [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979)、[**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974) 及 [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)。                                                                                                                                                                                                                                         |

 
您可以使用增量載入將清單控制項繫結到任意大的資料來源，而仍然保有高效能。 例如，您可以將清單控制項繫結到 Bing 影像查詢結果，而不需一次載入所有結果。 改為只立即載入一些結果，再視需要載入額外的結果。 若要支援增量載入，您必須在支援集合變更通知的資料來源上實作 [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)。 當資料繫結引擎要求更多資料時，您的資料來源必須提出適當的要求、整合結果，然後傳送適當的通知來更新 UI。

繫結目標
--------------

在下列兩個範例中，**Button.Content** 屬性是繫結目標，而它的值設定為宣告繫結物件的標記延伸。 首先顯示 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)，接著顯示 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)。 通常是在標記中宣告繫結 (方便、易讀、可加工)。 但如果需要的話，您可以避免使用標記，改為立即 (以程式設計方式) 建立 [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) 類別的執行個體。

<!-- XAML lang specifier not yet supported in OP. Using XML for now. -->
``` xml
<Button Content="{x:Bind ...}" ... /> 
```

``` xml
<Button Content="{Binding ...}" ... /> 
```

Binding object declared using {x:Bind}
--------------------------------------

There's one step we need to do before we author our [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) markup. We need to expose our binding source class from the class that represents our page of markup. We do that by adding a property (of type **HostViewModel** in this case) to our **HostView** page class.

``` csharp
namespace QuizGame.View
{
    public sealed partial class HostView : Page
    {
        public HostView()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
```完成了，我們現在可以仔細看看宣告繫結物件的標記。 下列範例使用稍早已在「繫結目標」一節中使用的 **Button.Content** 繫結目標，並顯示它已繫結到 **HostViewModel.NextButtonText** 屬性。

``` xml
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page itself, and in this case the path begins by referencing the **ViewModel** property that we just added to the **HostView** page. That property returns a **HostViewModel** instance, and so we can dot into that object to access the **HostViewModel.NextButtonText** property. And we specify **Mode**, to override the [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) default of one-time.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). For other settings, see [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783).

**Note**  Changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus, and not after every user keystroke.

**DataTemplate and x:DataType**

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) (whether used as an item template, a content template, or a header template), the value of **Path** is not interpreted in the context of the page, but in the context of the data object being templated. So that its bindings can be validated (and efficient code generated for them) at compile-time, a **DataTemplate** needs to declare the type of its data object using **x:DataType**. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of **SampleDataGroup** objects.

``` xml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**Weakly-typed objects in your Path**

Consider for example that you have a type named SampleDataGroup, which implements a string property named Title. And you have a property MainPage.SampleDataGroupAsObject, which is of type object but which actually returns an instance of SampleDataGroup. The binding `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` will result in a compile error because the Title property is not found on the type object. The remedy for this is to add a cast to your Path syntax like this: `<TextBlock Text="{x:Bind SampleDataGroupAsObject.(data:SampleDataGroup.Title)}"/>`. Here's another example where Element is declared as object but is actually a TextBlock: `<TextBlock Text="{x:Bind Element.Text}"/>`. And a cast remedies the issue: `<TextBlock Text="{x:Bind Element.(TextBlock.Text)}"/>`.

**如果您的資料會以非同步方式載入** 支援 **{x:Bind}** 的程式碼是在編譯頁面的部分類別時所產生。 您可以在 `obj` 資料夾中找到這些檔案，其名稱類似於 (適用於 C#) `<view name>.g.cs`. The generated code includes a handler for your page's [**Loading**](https://msdn.microsoft.com/library/windows/apps/BR208706-loading) event, and that handler calls the **Initialize** method on a generated class that represent's your page's bindings. **Initialize** in turn calls **Update** to begin moving data between the binding source and the target. **Loading** is raised just before the first measure pass of the page or user control. So if your data is loaded asynchronously it may not be ready by the time **Initialize** is called. So, after you've loaded data, you can force one-time bindings to be initialized by calling `this->Bindings->Update();`。 如果您只需要針對以非同步方式載入的資料使用一次性繫結，則將它們初始化的方法會比讓它擁有單向繫結並接聽變更來得經濟實惠。 如果您的資料不能進行細部變更，而且如果它很可能更新為特定動作的一部分，則您可以讓繫結變成一次性，並隨時呼叫 **Update** 來強制進行手動更新。

**限制** 

**{x:Bind}** 不適合晚期繫結案例 (例如，瀏覽 JSON 物件的字典結構)，也不適合 duck 類型，此為類型的弱式格式，其會以屬性名稱上的語彙相符項為依據 (「如果它走路、游泳的方式及呱呱聲像隻鴨子，那它就是隻鴨子」)。 透過 duck 類型，與 Age 屬性的繫結同樣可滿足 Person 或 Wine 物件。 針對這些案例，請使用 **{Binding}**。

使用 {Binding} 宣告的繫結物件 ---------------------------------------

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 預設會假設您繫結到標記頁面的 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)。 我們會將頁面的 **DataContext** 設定為繫結來源類別的執行個體 (在此案例中為 **HostViewModel** 類型)。 下列範例顯示宣告繫結物件的標記。 我們使用稍早已在「繫結目標」一節中使用的 **Button.Content** 繫結目標，並繫結到 **HostViewModel.NextButtonText** 屬性。

``` xml
<Page xmlns:viewmodel="using:QuizGame.ViewModel" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page's [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713), which in this example is set to an instance of **HostViewModel**. The path references the **HostViewModel.NextButtonText** property. We can omit **Mode**, because the [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) default of one-way works here.

The default value of [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) for a UI element is the inherited value of its parent. You can of course override that default by setting **DataContext** explicitly, which is in turn inherited by children by default. Setting **DataContext** explicitly on an element is useful when you want to have multiple bindings that use the same source.

A binding object has a **Source** property, which defaults to the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) of the UI element on which the binding is declared. You can override this default by setting **Source**, **RelativeSource**, or **ElementName** explicitly on the binding (see [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) for details).

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348), the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) is set to the data object being templated. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of any type that has string properties named **Title** and **Description**.

``` xml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

**Note**  By default, changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus. To cause changes to be sent after every user keystroke, set **UpdateSourceTrigger** to **PropertyChanged** on the binding in markup. You can also completely take control of when changes are sent to the source by setting **UpdateSourceTrigger** to **Explicit**. You then handle events on the text box (typically [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/BR209683-textchanged)), call [**GetBindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR208706-getbindingexpression) on the target to get a [**BindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR209820expression) object, and finally call [**BindingExpression.UpdateSource**](https://msdn.microsoft.com/library/windows/apps/BR209820expression-updatesource) to programmatically update the data source.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). The [**ElementName**](https://msdn.microsoft.com/library/windows/apps/BR209820-elementname) property is useful for element-to-element binding. The [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/BR209820-relativesource) property has several uses, one of which is as a more powerful alternative to template binding inside a [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209391). For other settings, see [{Binding} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204782) and the [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) class.

What if the source and the target are not the same type?
--------------------------------------------------------

If you want to control the visibility of a UI element based on the value of a boolean property, or if you want to render a UI element with a color that's a function of a numeric value's range or trend, or if you want to display a date and/or time value in a UI element property that expects a string, then you'll need to convert values from one type to another. There will be cases where the right solution is to expose another property of the right type from your binding source class, and keep the conversion logic encapsulated and testable there. But that isn't flexible nor scalable when you have large numbers, or large combinations, of source and target properties. In that case you'll want to use something known as a value converter. This section describes how to implement and consume a value converter.

Here's a value converter, suitable for a one-time or a one-way binding, that converts a [**DateTime**](T:System.DateTime) value to a string value containing the month. The class implements [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

``` csharp
public class DateToStringConverter : IValueConverter
{
    // Define the Convert method to convert a DateTime value to 
    // a month string.
    public object Convert(object value, Type targetType, 
        object parameter, string language)
    {
        // value is the data from the source object.
        DateTime thisdate = (DateTime)value;
        int monthnum = thisdate.Month;
        string month;
        switch (monthnum)
        {
            case 1:
                month = "January";
                break;
            case 2:
                month = "February";
                break;
            default:
                month = "Month not found";
                break;
        }
        // Return the value to pass to the target.
        return month;
    }

    // ConvertBack is not implemented for a OneWay binding.
    public object ConvertBack(object value, Type targetType, 
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

``` vbnet
Public Class DateToStringConverter
    Implements IValueConverter

    ' Define the Convert method to change a DateTime object to
    ' a month string.
    Public Function Convert(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.Convert

        ' value is the data from the source object.
        Dim thisdate As DateTime = CType(value, DateTime)
        Dim monthnum As Integer = thisdate.Month
        Dim month As String
        Select Case (monthnum)
            Case 1
                month = "January"
            Case 2
                month = "February"
            Case Else
                month = "Month not found"
        End Select
        ' Return the value to pass to the target.
        Return month

    End Function

    ' ConvertBack is not implemented for a OneWay binding.
    Public Function ConvertBack(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.ConvertBack

        Throw New NotImplementedException

    End Function
End Class
```以下說明如何在您的繫結物件標記中使用該值轉換器。

``` xml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>

...

<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>

<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

The binding engine calls the [**Convert**](https://msdn.microsoft.com/library/windows/apps/BR209903-convert) and [**ConvertBack**](https://msdn.microsoft.com/library/windows/apps/BR209903-convertback) methods if the [**Converter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converter) parameter is defined for the binding. When data is passed from the source, the binding engine calls **Convert** and passes the returned data to the target. When data is passed from the target (for a two-way binding), the binding engine calls **ConvertBack** and passes the returned data to the source.

The converter also has optional parameters: [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterlanguage), which allows specifying the language to be used in the conversion, and [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterparameter), which allows passing a parameter for the conversion logic. For an example that uses a converter parameter, see [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

**Note**  If there is an error in the conversion, do not throw an exception. Instead, return [**DependencyProperty.UnsetValue**](https://msdn.microsoft.com/library/windows/apps/BR242362-unsetvalue), which will stop the data transfer.

To display a default value to use whenever the binding source cannot be resolved, set the **FallbackValue** property on the binding object in markup. This is useful to handle conversion and formatting errors. It is also useful to bind to source properties that might not exist on all objects in a bound collection of heterogeneous types.

If you bind a text control to a value that is not a string, the data binding engine will convert the value to a string. If the value is a reference type, the data binding engine will retrieve the string value by calling [**ICustomPropertyProvider.GetStringRepresentation**](https://msdn.microsoft.com/library/windows/apps/BR209878-getstringrepresentation) or [**IStringable.ToString**](https://msdn.microsoft.com/library/Dn302136) if available, and will otherwise call [**Object.ToString**](M:System.Object.ToString). Note, however, that the binding engine will ignore any **ToString** implementation that hides the base-class implementation. Subclass implementations should override the base class **ToString** method instead. Similarly, in native languages, all managed objects appear to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) and [**IStringable**](https://msdn.microsoft.com/library/Dn302135). However, all calls to **GetStringRepresentation** and **IStringable.ToString** are routed to **Object.ToString** or an override of that method, and never to a new **ToString** implementation that hides the base-class implementation.

Resource dictionaries with {x:Bind}
-----------------------------------

The [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783) depends on code generation, so it needs a code-behind file containing a constructor that calls **InitializeComponent** (to initialize the generated code). You re-use the resource dictionary by instantiating its type (so that **InitializeComponent** is called) instead of referencing its filename. Here's an example of what to do if you have an existing resource dictionary and you want to use {x:Bind} in it.

TemplatesResourceDictionary.xaml

``` xml
<ResourceDictionary
    x:Class="ExampleNamespace.TemplatesResourceDictionary"
    .....
    xmlns:examplenamespace="using:ExampleNamespace">
    
    <DataTemplate x:Key="EmployeeTemplate" x:DataType="examplenamespace:IEmployee">
        <Grid>
            <TextBlock Text="{x:Bind Name}"/>
        </Grid>
    </DataTemplate>
</ResourceDictionary>
```

TemplatesResourceDictionary.xaml.cs

``` csharp
using Windows.UI.Xaml.Data;
 
namespace ExampleNamespace
{
    public partial class TemplatesResourceDictionary
    {
        public TemplatesResourceDictionary()
        {
            InitializeComponent();
        }
    }
}
``` MainPage.xaml ``` xml
<Page x:Class="ExampleNamespace.MainPage"
    ....
    xmlns:examplenamespace="using:ExampleNamespace">

    <Page.Resources>
        <ResourceDictionary>
            .... 
            <ResourceDictionary.MergedDictionaries>
                <examplenamespace:TemplatesResourceDictionary/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>
</Page>
```

Event binding and ICommand
--------------------------

[{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) supports a feature called event binding. With this feature, you can specify the handler for an event using a binding, which is an additional option on top of handling events with a method on the code-behind file. Let's say you have a **RootFrame** property on your **MainPage** class.

``` csharp
    public sealed partial class MainPage : Page
    {
        ....    
        public Frame RootFrame { get { return Window.Current.Content as Frame; } }
    }
```同樣地，您可以將按鈕的 **Click** 事件繫結到 **RootFrame** 屬性所傳回的 **Frame** 物件的方法。 請注意，我們也將按鈕的 **IsEnabled** 屬性繫結到相同 **Frame** 的另一個成員。

``` xml
    <AppBarButton Icon="Forward" IsCompact="True"
    IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
    Click="{x:Bind RootFrame.GoForward}"/>
```

Overloaded methods cannot be used to handle an event with this technique. Also, if the method that handles the event has parameters then they must all be assignable from the types of all of the event's parameters, respectively. In this case, [**Frame.GoForward**](https://msdn.microsoft.com/library/windows/apps/BR242693) is not overloaded and it has no parameters (but it would still be valid even if it took two **object** parameters). [**Frame.GoBack**](https://msdn.microsoft.com/library/windows/apps/Dn996568) is overloaded, though, so we can't use that method with this technique.

The event binding technique is similar to implementing and consuming commands (a command is a property that returns an object that implements the [**ICommand**](T:System.Windows.Input.ICommand) interface). Both [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) work with commands. So that you don't have to implement the command pattern multiple times, you can use the **DelegateCommand** helper class that you'll find in the [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) sample (in the "Common" folder).

## Binding to a collection of folders or files

You can use the APIs in the [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) namespace to retrieve folder and file data. However, the various **GetFilesAsync**, **GetFoldersAsync**, and **GetItemsAsync** methods do not return values that are suitable for binding to list controls. Instead, you must bind to the return values of the [**GetVirtualizedFilesVector**](https://msdn.microsoft.com/library/windows/apps/Hh701422), [**GetVirtualizedFoldersVector**](https://msdn.microsoft.com/library/windows/apps/Hh701428), and [**GetVirtualizedItemsVector**](https://msdn.microsoft.com/library/windows/apps/Hh701430) methods of the [**FileInformationFactory**](https://msdn.microsoft.com/library/windows/apps/BR207501) class. The following code example from the [StorageDataSource and GetVirtualizedFilesVector sample](http://go.microsoft.com/fwlink/p/?linkid=228621) shows the typical usage pattern. Remember to declare the **picturesLibrary** capability in your app package manifest, and confirm that there are pictures in your Pictures library folder.

``` csharp
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var library = Windows.Storage.KnownFolders.PicturesLibrary;
            var queryOptions = new Windows.Storage.Search.QueryOptions();
            queryOptions.FolderDepth = Windows.Storage.Search.FolderDepth.Deep;
            queryOptions.IndexerOption = Windows.Storage.Search.IndexerOption.UseIndexerWhenAvailable;

            var fileQuery = library.CreateFileQueryWithOptions(queryOptions);

            var fif = new Windows.Storage.BulkAccess.FileInformationFactory(
                fileQuery,
                Windows.Storage.FileProperties.ThumbnailMode.PicturesView,
                190,
                Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale,
                false
                );

            var dataSource = fif.GetVirtualizedFilesVector();
            this.PicturesListView.ItemsSource = dataSource;
        }
```您一般會使用這個方法來建立檔案和資料夾資訊的唯讀檢視。 您可以建立檔案和資料夾屬性的雙向繫結，例如讓使用者在音樂檢視中給歌曲評分。 不過，所有變更都必須等到您呼叫適當的 **SavePropertiesAsync** 方法 (例如 [**MusicProperties.SavePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/BR207760)) 之後才能保留。 您應該在項目失去焦點時認可變更，因為這會觸發選取項目重設。

請注意，使用這項技術的雙向繫結只適用於已編製索引的位置 (例如 [音樂])。 您可以呼叫 [**FolderInformation.GetIndexedStateAsync**](https://msdn.microsoft.com/library/windows/apps/BR207627) 方法來判斷位置是否已編製索引。

另請注意，虛擬化向量可能會在填入某些項目的值之前先傳回 **null**。 例如，使用與虛擬化向量繫結之清單控制項的 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) 值之前，您應該先檢查是否有 **null**，或者改用 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/BR209768)。

繫結到依索引鍵分組的資料 -------------------------------- 如果您取得一般項目集合—例如書籍，以 **BookSku** 類別表示—且使用常見的屬性做為索引鍵將項目分組—例如 **BookSku.AuthorName** 屬性—則結果稱為分組資料。 資料分組之後就不再是一般集合。 分組資料是群組物件的集合，其中每個群組物件都有 a) 索引鍵，b) 屬性符合該索引鍵的項目集合。 同樣以書籍為例子，依作者名稱將書籍分組會產生作者名稱群組的集合，其中每個群組都有 a) 索引鍵，即作者命名，b) **AuthorName** 屬性符合群組索引鍵的 **BookSku** 集合。

一般而言，若要顯示集合，您需要將項目控制項 (例如 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 或 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)) 的 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) 直接繫結到傳回集合的屬性。 如果是一般項目集合，則不需要採取任何特別的動作。 但如果是群組物件的集合 (例如繫結到分組資料時)，則需要一個稱為 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 的中繼物件的服務，此物件位於項目控制項和繫結來源之間。 您需要將 **CollectionViewSource** 繫結到傳回分組資料的屬性，並將項目控制項繫結到 **CollectionViewSource**。 **CollectionViewSource** 額外的好處是它可以追蹤目前的項目，可讓您將多個項目控制項全部繫結到相同的 **CollectionViewSource**，以保持同步。 您可以透過 [**CollectionViewSource.View**](https://msdn.microsoft.com/library/windows/apps/BR209833-view) 屬性所傳回的物件的 [**ICollectionView.CurrentItem**](https://msdn.microsoft.com/library/windows/apps/BR209857) 屬性，以程式設計方式存取目前的項目。

若要啟用 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 的分組功能，請將 [**IsSourceGrouped**](https://msdn.microsoft.com/library/windows/apps/BR209833-issourcegrouped) 設定為 **true**。 不論您是否也需要設定 [**ItemsPath**](https://msdn.microsoft.com/library/windows/apps/BR209833-itemspath) 屬性，這完全取決於您如何撰寫群組物件。 撰寫群組物件有兩種方式：「是一個群組」模式和「有一個群組」模式。 在「是一個群組」模式中，群組物件衍生自集合類型 (例如 **List&lt;T&gt;**)，因此群組物件本身實際上是項目群組。 使用這個模式時，您不需要設定 **ItemsPath**。 在「有一個群組」模式中，群組物件有集合類型 (例如 **List&lt;T&gt;**) 的一或多個屬性，因此群組以屬性形式而「有一個」項目群組 (或以數個屬性的形式而有數個項目群組)。 使用這個模式時，您需要將 **ItemsPath** 設定為包含項目群組的屬性名稱。

下列範例說明「有一個群組」模式。 頁面類別有一個名為 [**ViewModel**](https://msdn.microsoft.com/library/windows/apps/BR208713) 的屬性，可傳回檢視模型的執行個體。 [
            **CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 繫結至檢視模型的 **Authors** 屬性 (**Authors** 為群組物件的集合)，也指定是 **Author.BookSkus** 屬性包含分組項目。 最後，[**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) 繫結至 **CollectionViewSource**，並定義其群組樣式以呈現群組中的項目。

``` csharp
    <Page.Resources>
        <CollectionViewSource
        x:Name="AuthorHasACollectionOfBookSku"
        Source="{x:Bind ViewModel.Authors}"
        IsSourceGrouped="true"
        ItemsPath="BookSkus"/>
    </Page.Resources>
    ...

    <GridView
    ItemsSource="{Binding Source={StaticResource AuthorHasACollectionOfBookSku}}" ...>
        <GridView.GroupStyle>
            <GroupStyle
                HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
        </GridView.GroupStyle>
    </GridView>
```

Note that the [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) must use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) (and not [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)) because it needs to set the **Source** property to a resource. To see the above example in the context of the complete app, download the [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app. Unlike the markup shown above, [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) uses {Binding} exclusively.

You can implement the "is-a-group" pattern in one of two ways. One way is to author your own group class. Derive the class from **List&lt;T&gt;** (where *T* is the type of the items). For example, `public class Author : List<BookSku>`. 第二種方法是使用 [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) 運算式，從 **BookSku** 項目的相似屬性值，動態建立群組物件 (和群組類別)。 這個方法—維護一般項目清單並即時將它們組合在一起—常見於從雲端服務存取資料的 app。 這可讓您依作者或內容類型 (舉例)，靈活地將書籍分組，而不需要 **Author** 和 **Genre** 之類的特殊群組類別。

下列範例使用 [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) 說明「是一個群組」模式。 這次我們依內容類型將書籍分組，並在群組標頭中顯示內容類型名稱。 這是由群組 [**Key**](P:System.Linq.IGrouping%602.Key) 值之參照中的 "Key" 屬性路徑所指示。

``` csharp
    using System.Linq;

    ...

    private IOrderedEnumerable<IGrouping<string, BookSku>> genres; public IOrderedEnumerable<IGrouping<string, BookSku>> Genres
    {
        get
        {
            if (this.genres == null)
            {
                this.genres = from book in this.bookSkus
                group book by book.genre into grp
                orderby grp.Key select grp;
            }
            return this.genres;
        }
    } ```

Remember that when using [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) with data templates we need to indicate the type being bound to by setting an **x:DataType** value. If the type is generic then we can't express that in markup so we need to use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) instead in the group style header template.

``` xml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{Binding Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{Binding Source={StaticResource GenreIsACollectionOfBookSku}}">
        <GridView.ItemTemplate x:DataType="local:BookTemplate">
            <DataTemplate>
                <TextBlock Text="{x:Bind Title}"/>
            </DataTemplate>
        </GridView.ItemTemplate>
        <GridView.GroupStyle>
            <GroupStyle>
                <GroupStyle.HeaderTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Key}"/>
                    </DataTemplate>
                </GroupStyle.HeaderTemplate>
            </GroupStyle>
        </GridView.GroupStyle>
    </GridView>
```

A [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/Hh702601) control is a great way for your users to view and navigate grouped data. The [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app illustrates how to use the **SemanticZoom**. In that app, you can view a list of books grouped by author (the zoomed-in view) or you can zoom out to see a jump list of authors (the zoomed-out view). The jump list affords much quicker navigation than scrolling through the list of books. The zoomed-in and zoomed-out views are actually **ListView** or **GridView** controls bound to the same **CollectionViewSource**.

![An illustration of a SemanticZoom](images/sezo.png)

When you bind to hierarchical data—such as subcategories within categories—you can choose to display the hierarchical levels in your UI with a series of items controls. A selection in one items control determines the contents of subsequent items controls. You can keep the lists synchronized by binding each list to its own [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) and binding the **CollectionViewSource** instances together in a chain. This is called a master/details (or list/details) view. For more info, see [How to bind to hierarchical data and create a master/details view](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md).

Diagnosing and debugging data binding problems
-----------------------------------------------

Your binding markup contains the names of properties (and, for C#, sometimes fields and methods). So when you rename a property, you'll also need to change any binding that references it. Forgetting to do that leads to a typical example of a data binding bug, and your app either won't compile or won't run correctly.

The binding objects created by [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) are largely functionally equivalent. But {x:Bind} has type information for the binding source, and it generates source code at compile-time. With {x:Bind} you get the same kind of problem detection that you get with the rest of your code. That includes compile-time validation of your binding expressions, and debugging by setting breakpoints in the source code generated as the partial class for your page. These classes can be found in the files in your `obj` folder, with names like (for C#) `<view name>.g.cs`). 如果繫結發生問題，請在 Microsoft Visual Studio 偵錯工具中開啟 [發生未處理的例外狀況時中斷]****。 偵錯工具會在那一刻中斷執行，讓您偵錯出了什麼問題。 對於繫結來源節點圖形的每個部分，{x:Bind} 產生的程式碼都採用相同的模式，您可以使用 [呼叫堆疊]**** 視窗中的資訊，協助您判斷導致問題的呼叫序列。

[
            {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 沒有繫結來源的類型資訊。 但在附加偵錯工具的情況下執行您的 app 時，所有繫結錯誤都會顯示在 Visual Studio 的 [輸出]**** 視窗中。

在程式碼中建立繫結 -------------------------

**注意** 本節只適用於 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)，因為您不能在程式碼中建立 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) 繫結。 不過，部分與 {x:Bind} 相同的優點可以利用 [**DependencyProperty.RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/BR242356-registerpropertychangedcallback) 來達成，讓您能夠在任何相依性屬性上登錄變更通知。

您也可以使用程序性程式碼來取代 XAML，將 UI 元素連結到資料。 要這樣做，請建立新的 [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) 物件、設定適當的屬性，然後呼叫 [**FrameworkElement.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR208706-setbinding) 或 [**BindingOperations.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR209820operations-setbinding)。 當您想在執行階段選擇繫結屬性值，或在多個控制項間共用單一繫結時，以程式設計方式建立繫結會很有用。 不過，需注意在呼叫 **SetBinding** 之後，您就無法變更繫結屬性值。

下列範例示範如何在程式碼中實作繫結。

``` xml
<TextBox x:Name="MyTextBox" Text="Text"/>
```

```csharp
// Create an instance of the MyColors class 
// that implements INotifyPropertyChanged.
MyColors textcolor = new MyColors();

// Brush1 is set to be a SolidColorBrush with the value Red.
textcolor.Brush1 = new SolidColorBrush(Colors.Red);

// Set the DataContext of the TextBox MyTextBox.
MyTextBox.DataContext = textcolor;

// Create the binding and associate it with the text box.
Binding binding = new Binding() { Path = new PropertyPath("Brush1") };
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding);
```

``` vbnet
' Create an instance of the MyColors class 
' that implements INotifyPropertyChanged. 
Dim textcolor As New MyColors()

' Brush1 is set to be a SolidColorBrush with the value Red. 
textcolor.Brush1 = New SolidColorBrush(Colors.Red)

' Set the DataContext of the TextBox MyTextBox. 
MyTextBox.DataContext = textcolor

' Create the binding and associate it with the text box.
Dim binding As New Binding() With {.Path = New PropertyPath("Brush1")}
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding)
``` {x:Bind} 和 {Binding} 功能比較 ------------------------------------------ | 功能 | {x:Bind} | {Binding} | 附註 | |---------|----------|-----------|-------| | Path 是預設屬性 | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Path 屬性 | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | 在 x:Bind 中，Path 預設以 Page 為根目錄，而不是 DataContext。 | | 索引子 | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | 繫結至集合中指定的項目。 僅支援整數索引。 | | 附加屬性 | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | 使用括號指定附加屬性。 如果未在 XAML 命名空間中宣告屬性，則需要以 xml 命名空間為其加上首碼，而此命名空間應對應到文件最前面的程式碼命名空間。 | | 轉型 | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | 不需要< | 使用括號指定轉型。 如果未在 XAML 命名空間中宣告屬性，則需要以 xml 命名空間為其加上首碼，而此命名空間應對應到文件最前面的程式碼命名空間。 | 
| Converter | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | Converter 必須在 Page/ResourceDictionary 的根目錄或 App.xaml 中宣告。 | 
| ConverterParameter、ConverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | Converter 必須在 Page/ResourceDictionary 的根目錄或 App.xaml 中宣告。 | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | 繫結運算式的分葉為 null 時使用。 使用單引號指定字串值。 | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | 繫結路徑的任何部分 (除了分葉) 為 null 時使用。 | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | 您使用 {x:Bind} 繫結到欄位。Path 預設以 Page 為根目錄，任何具名的項目都可以透過它的欄位來存取。 | 
| RelativeSource: Self | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | With {x:Bind}, name the element and use its name in Path. | 
| RelativeSource: TemplatedParent | Not supported | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | 一般範本繫結可以用在控制項範本中滿足大部分的用途。 但在需要使用轉換器或雙向繫結的情況下，請使用 TemplatedParent。< | 
| Source | 不支援 | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | For {x:Bind} use a property or a static path instead. | 
| Mode | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | Mode can be OneTime, OneWay, or TwoWay. {x:Bind} defaults to OneTime; {Binding} defaults to OneWay. | 
| UpdateSourceTrigger | Not supported | `<Binding UpdateSourceTrigger="[Default | PropertyChanged | Explicit]"/>` | {x:Bind} 一律使用 PropertyChanged 行為，但 TextBox.Text 除外，在此情況下會等候失去焦點以更新來源。 | 




<!--HONumber=Mar16_HO1-->


