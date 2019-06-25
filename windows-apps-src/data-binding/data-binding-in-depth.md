---
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: 深入了解資料繫結
description: 資料繫結可讓您的 App UI 顯示資料，以及選擇性地與該資料保持同步。
ms.date: 10/05/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: f3cdb9cbb1aa3f62fb711be747c44a0df10fb1ee
ms.sourcegitcommit: f7e3782e24d46b2043023835c5b59d12d3b4ed4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/24/2019
ms.locfileid: "67345726"
---
# <a name="data-binding-in-depth"></a>深入了解資料繫結

**重要的 Api**

-   [ **{x： 繫結} 標記延伸**](../xaml-platform/x-bind-markup-extension.md)
-   [**繫結類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)
-   [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext)
-   [**INotifyPropertyChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)

> [!NOTE]
> 這個主題將提供資料繫結功能的詳細說明。 如需簡短且實用的簡介，請參閱[資料繫結概觀](data-binding-quickstart.md)。

本主題是關於在通用 Windows 平台 (UWP) 應用程式中的資料繫結。 此處所討論的 Api 位於[ **Windows.UI.Xaml.Data**命名空間](/uwp/api/windows.ui.xaml.data)。

資料繫結可讓您的 App UI 顯示資料，以及選擇性地與該資料保持同步。 資料繫結可讓您將資料與 UI 分開考量，為 app 建構更簡單的概念模型，以及更好的可讀性、測試性和維護性。

資料繫結可讓您在 UI 最初顯示時，單純地顯示資料來源的值，而不是要回應這些值的變更。 這是一種模式的繫結呼叫*單次*，和它非常適合在執行階段期間不會變更的值。 或者，您可以選擇 「 觀察 」 值，並更新 UI，其變更時。 這個模式稱為*單向*，而且它很適合唯讀資料。 最後，您可以選擇同時觀察和更新，將使用者對 UI 中的值所做的變更，自動推回到資料來源。 這個模式稱為*雙向*，和它非常適合讀寫資料。 以下是一些範例。

-   您可以使用一次模式繫結[**映像**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)目前使用者的相片。
-   您可以使用單向模式來繫結[ **ListView** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)報紙區段所分組的即時新聞文章的集合。
-   您可以使用雙向模式，來繫結[ **TextBox** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)表單中的客戶的名稱。

除了模式之外，有兩種類型的繫結，以及它們兩者通常宣告 UI 標記中。 您可以選擇使用 [{x:Bind} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)或 [{Binding} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)。 您甚至可以在相同的 app 中將兩者混用 (甚至在相同的 UI 元素上)。 {x:Bind} 是 Windows 10 新增的標記，效能更好。 本主題所述的所有詳細資料適用於這兩種繫結類型，除非明確指出不是如此。

**範例應用程式，示範 {x： 繫結}**

-   [{x:Bind} 範例](https://go.microsoft.com/fwlink/p/?linkid=619989)。
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)。
-   [XAML UI 基本知識範例](https://go.microsoft.com/fwlink/p/?linkid=619992)。

**範例應用程式，示範 {Binding}**

-   下載 [Bookstore1](https://go.microsoft.com/fwlink/?linkid=532950) app。
-   下載 [Bookstore2](https://go.microsoft.com/fwlink/?linkid=532952) app。

## <a name="every-binding-involves-these-pieces"></a>每個繫結包含這些項目

-   *繫結來源*。 這是繫結的資料來源，可以是任何類別的執行個體，且其成員有您想在 UI 中顯示的值。
-   *繫結目標*。 這是 UI 中用來顯示資料的 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) 的 [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement)。
-   *繫結物件*。 此項目將資料值從來源傳輸到目標，也可以從目標傳回到源。 繫結物件是在 XAML 載入時從 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 或 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 標記延伸建立。

在下列各節，我們將詳細說明繫結來源、繫結目標和繫結物件。 我們以一個範例來串連各節，此範例將一個按鈕的內容繫結到 **HostViewModel** 類別的 **NextButtonText** 字串屬性。

### <a name="binding-source"></a>繫結來源

以下是一個非常基本的類別實作，可做為繫結來源。

如果您使用[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，然後新增**Midl 檔 (.idl)** 項目加入專案中，如中所示C++/WinRT 下方的程式碼範例清單。 使用這些新檔案的內容取代[MIDL 3.0](/uwp/midl-3/intro)顯示在清單中，程式碼建置專案以產生`HostViewModel.h`和`.cpp`，然後將程式碼新增至產生的檔案，以符合清單中。 如需這些產生的檔案的詳細資訊，以及如何將它們複製到您的專案，請參閱 < [XAML 控制項，繫結至C++/WinRT 屬性](/windows/uwp/cpp-and-winrt-apis/binding-property)。

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

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Implement the constructor like this, and add this field:
...
HostViewModel() : m_nextButtonText{ L"Next" } {}
...
private:
    std::wstring m_nextButtonText;
...

// HostViewModel.cpp
// Implement like this:
...
hstring HostViewModel::NextButtonText()
{
    return hstring{ m_nextButtonText };
}

void HostViewModel::NextButtonText(hstring const& value)
{
    m_nextButtonText = value;
}
...
```

**HostViewModel** 的實作及其屬性 **NextButtonText** 僅適用於一次性繫結。 單向與雙向繫結很常見，在這些繫結類型中，UI 會隨著繫結來源的資料值變更而自動更新。 為了讓這些繫結類型正常運作，您需要讓繫結物件「可觀察」您的繫結來源。 因此，在範例中，如果我們要單向或雙向繫結到 **NextButtonText** 屬性，則該屬性的值在執行階段發生的任何變更，都必須讓繫結物件可觀察。

作法之一是從 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) 衍生代表繫結來源的類別，並透過 [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) 公開資料值。 這樣 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 就變成可觀察。 **FrameworkElements** 是很好用的現成繫結來源。

將類別變成可觀察還有更簡便的作法，也是已有基底類別的類別的必要作法，就是實作 [**System.ComponentModel.INotifyPropertyChanged**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?redirectedfrom=MSDN)。 這只需要實作一個名為 **PropertyChanged** 的事件。 以下是使用 **HostViewModel** 的範例。

```csharp
...
using System.ComponentModel;
using System.Runtime.CompilerServices;
...
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

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Add this field:
...
    winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler);
    void PropertyChanged(winrt::event_token const& token) noexcept;

private:
    winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
...

// HostViewModel.cpp
// Implement like this:
...
void HostViewModel::NextButtonText(hstring const& value)
{
    if (m_nextButtonText != value)
    {
        m_nextButtonText = value;
        m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"NextButtonText" });
    }
}

winrt::event_token HostViewModel::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
{
    return m_propertyChanged.add(handler);
}

void HostViewModel::PropertyChanged(winrt::event_token const& token) noexcept
{
    m_propertyChanged.remove(token);
}
...
```

**NextButtonText** 屬性現在已變成可觀察的。 當您撰寫該屬性的單向或雙向繫結時 (稍後說明作法本)，產生的繫結物件會訂閱 **PropertyChanged** 事件。 該事件引發時，繫結物件的處理常式會收到一個引數，其中包含已變更的屬性的名稱。 就是這樣，繫結物件才知道要再次讀取哪個屬性的值。

因此，您不需要實作許多次，如上所示，如果您使用的模式C#則只是衍生自**BindableBase**中找到的基底類別[QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)範例 （在「 通用 」 的資料夾）。 以下是作法的範例。

```csharp
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

```cppwinrt
// Your BindableBase base class should itself derive from Windows::UI::Xaml::DependencyObject. Then, in HostViewModel.idl, derive from BindableBase instead of implementing INotifyPropertyChanged.
```

> [!NOTE]
> C + /cli WinRT，任何您宣告衍生自基底類別的應用程式中的執行階段類別就所謂*簡式*類別。 並且有條件約束可組合的類別。 將應用程式[Windows 應用程式認證套件](../debug-test-perf/windows-app-certification-kit.md)測試來驗證提交由 Visual Studio 和 Microsoft Store (因此成功擷取到 Microsoft Store 應用程式)、可組合的類別必須最終衍生自 Windows 的基底類別。 這表示繼承階層架構類別非常的根目錄必須源自 windows.* 命名空間中的類型。 如果您需要衍生自基底類別的執行階段類別&mdash;例如，若要實作**BindableBase**類別衍生自您檢視模型的所有&mdash;則可衍生自[ **Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)。

以引數 [**String.Empty**](https://docs.microsoft.com/dotnet/api/system.string.empty?redirectedfrom=MSDN) 或 **null** 引發 **PropertyChanged** 事件時，表示應該重新讀取物件上的所有非索引子屬性。 您可以引發事件，以表示物件上的索引子屬性已變更所使用的引數 」 項目\[*索引子*\]「 針對特定的索引子 (其中*索引子*是索引值），或值為"項目\[\]"的所有索引子。

繫結來源可以視為單一物件 (屬性包含資料) 或物件集合。 在C#和 Visual Basic 程式碼，您可以一次的繫結到該物件會實作[ **List (Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1?redirectedfrom=MSDN)在執行階段顯示的集合，不會變更。 對於可觀察的集合 (觀察集合中新增和移除項目)，則改為單向繫結到 [**ObservableCollection(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1?redirectedfrom=MSDN)。 在 C++ 程式碼中，對於可觀察和不可觀察的集合，您都可以繫結到 [**Vector&lt;T&gt;** ](https://docs.microsoft.com/dotnet/api/system.numerics.vector-1?redirectedfrom=MSDN)。 如果要繫結到您自己的集合類別，請使用下表中的指導方針。

|狀況|C# 及 VB (CLR)|C++/WinRT|C++/CX|
|-|-|-|-|
|繫結到物件。|可為任何物件。|可為任何物件。|物件必須具有 [**BindableAttribute**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 或實作 [**ICustomPropertyProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider)。|
|從繫結的物件中取得屬性變更通知。|物件必須實作[ **INotifyPropertyChanged**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?redirectedfrom=MSDN)。| 物件必須實作[ **INotifyPropertyChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)。|物件必須實作[ **INotifyPropertyChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)。|
|繫結到集合。| [**List(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1?redirectedfrom=MSDN)|[**IVector** ](/uwp/api/windows.foundation.collections.ivector_t_)的[ **IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)，或[ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)。 請參閱[控制項的 XAML 項目，繫結至C++/WinRT 集合](../cpp-and-winrt-apis/binding-collection.md)並[集合C++/WinRT](../cpp-and-winrt-apis/collections.md)。| [**Vector&lt;T&gt;** ](https://docs.microsoft.com/cpp/cppcx/platform-collections-vector-class)|
|取得從繫結集合的集合變更通知。|[**ObservableCollection(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1?redirectedfrom=MSDN)|[**IObservableVector** ](/uwp/api/windows.foundation.collections.iobservablevector_t_)的[ **IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)。|[**IObservableVector&lt;T&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_)|
|實作支援繫結的集合。|擴充 [**List(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1?redirectedfrom=MSDN) 或實作 [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist?redirectedfrom=MSDN)、[**IList**](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1?redirectedfrom=MSDN)(Of [**Object**](https://docs.microsoft.com/dotnet/api/system.object?redirectedfrom=MSDN))、[**IEnumerable**](https://docs.microsoft.com/dotnet/api/system.collections.ienumerable?redirectedfrom=MSDN) 或 [**IEnumerable**](https://docs.microsoft.com/dotnet/api/system.collections.generic.ienumerable-1?redirectedfrom=MSDN)(Of **Object**)。 不支援繫結到泛型 **IList(Of T)** 與 **IEnumerable(Of T)** 。|實作[ **IVector** ](/uwp/api/windows.foundation.collections.ivector_t_)的[ **IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)。 請參閱[控制項的 XAML 項目，繫結至C++/WinRT 集合](../cpp-and-winrt-apis/binding-collection.md)並[集合C++/WinRT](../cpp-and-winrt-apis/collections.md)。|實作 [**IBindableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableVector)、[**IBindableIterable**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableIterable)、[**IVector**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IVector_T_)&lt;[**Object**](https://docs.microsoft.com/dotnet/api/system.object?redirectedfrom=MSDN)^&gt;、[**IIterable**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IIterable_T_)&lt;**Object**^&gt;、**IVector**&lt;[**IInspectable**](https://docs.microsoft.com/windows/desktop/api/inspectable/nn-inspectable-iinspectable)\*&gt; 或 **IIterable**&lt;**IInspectable**\*&gt;。 不支援繫結到泛型 **IVector&lt;T&gt;** 與 **IIterable&lt;T&gt;** 。|
| 實作支援變更通知的集合。 | 擴充 [**ObservableCollection(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1?redirectedfrom=MSDN) 或實作 (非泛型) [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist?redirectedfrom=MSDN) 與 [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged?redirectedfrom=MSDN)。|實作[ **IObservableVector** ](/uwp/api/windows.foundation.collections.iobservablevector_t_)的[ **IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)，或[ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).|實作 [**IBindableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableVector) 及 [**IBindableObservableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableObservableVector)。|
|實作支援增量載入的集合。|擴充 [**ObservableCollection(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1?redirectedfrom=MSDN) 或實作 (非泛型) [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist?redirectedfrom=MSDN) 與 [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged?redirectedfrom=MSDN)。 此外，也實作 [**ISupportIncrementalLoading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)。|實作[ **IObservableVector** ](/uwp/api/windows.foundation.collections.iobservablevector_t_)的[ **IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)，或[ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector). 此外，實作[ **ISupportIncrementalLoading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)|實作 [**IBindableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableVector)、[**IBindableObservableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableObservableVector) 及 [**ISupportIncrementalLoading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)。|

您可以使用增量載入將清單控制項繫結到任意大的資料來源，而仍然保有高效能。 例如，您可以將清單控制項繫結到 Bing 影像查詢結果，而不需一次載入所有結果。 改為只立即載入一些結果，再視需要載入額外的結果。 若要支援累加式載入，您必須實作[ **ISupportIncrementalLoading** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)支援集合的資料來源上變更通知。 當資料繫結引擎要求更多資料時，您的資料來源必須提出適當的要求、整合結果，然後傳送適當的通知來更新 UI。

### <a name="binding-target"></a>繫結目標

在兩個範例中， **Button.Content**屬性是繫結目標，且其值設定為標記延伸宣告繫結物件。 首先顯示 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)，接著顯示 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)。 通常是在標記中宣告繫結 (方便、易讀、可加工)。 但如果需要的話，您可以避免使用標記，改為立即 (以程式設計方式) 建立 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 類別的執行個體。

```xaml
<Button Content="{x:Bind ...}" ... />
```

```xaml
<Button Content="{Binding ...}" ... />
```

如果您使用C++/WinRT 或視覺效果C++元件擴充功能 (C++/CX)，則您必須新增[ **BindableAttribute** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute)屬性設定為您想要使用任何執行階段類別[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)標記延伸模組。

> [!IMPORTANT]
> 如果您使用[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，然後在[ **BindableAttribute** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute)屬性時才可以使用您已安裝 Windows SDK 版本 10.0.17763.0 (Windows 10版本 1809年），或更新版本。 如果沒有該屬性中，您必須實作[ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider)並[ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty)為了能夠使用的介面[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)標記延伸模組。

### <a name="binding-object-declared-using-xbind"></a>使用 {x:Bind} 宣告的繫結物件

在撰寫我們的 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 標記之前，還有一個步驟需要執行。 我們需要從代表標記頁面的類別中公開繫結來源類別。 我們的做法是將屬性 (型別的**HostViewModel**在此情況下) 到我們**MainPage**頁面類別。

```csharp
namespace DataBindingInDepth
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
```

```cppwinrt
// MainPage.idl
import "HostViewModel.idl";

namespace DataBindingInDepth
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        HostViewModel ViewModel{ get; };
    }
}

// MainPage.h
// Include a header, and add this field:
...
#include "HostViewModel.h"
...
    DataBindingInDepth::HostViewModel ViewModel();

private:
    DataBindingInDepth::HostViewModel m_viewModel{ nullptr };
...

// MainPage.cpp
// Implement like this:
...
MainPage::MainPage()
{
    InitializeComponent();

}

DataBindingInDepth::HostViewModel MainPage::ViewModel()
{
    return m_viewModel;
}
...
```

完成了，我們現在可以仔細看看宣告繫結物件的標記。 下列範例使用稍早已在「繫結目標」一節中使用的 **Button.Content** 繫結目標，並顯示它已繫結到 **HostViewModel.NextButtonText** 屬性。

```xaml
<!-- MainPage.xaml -->
<Page x:Class="DataBindingInDepth.Mainpage" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

請注意我們指定給 **Path** 的值。 這個值會解譯上頁面本身的內容中，並藉由參考路徑的開始在此情況下**ViewModel**我們剛才加入的屬性**MainPage**頁面。 該屬性會傳回 **HostViewModel** 執行個體，因此我們可以進入該物件來存取 **HostViewModel.NextButtonText** 屬性。 此外，我們會指定 **Mode** 來覆寫一次性的 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 預設值。

[  **Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) 屬性支援多種繫結語法選項，可讓您繫結到巢狀屬性、附加屬性以及整數和字串索引子。 如需詳細資訊，請參閱 [Property-path 語法](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax)。 繫結到字串索引子可以提供繫結至動態屬性的效果，卻不需實作 [**ICustomPropertyProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider)。 如需其他設定，請參閱 [{x:Bind} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)。

展示**HostViewModel.NextButtonText**時確實可觀察的屬性，加入**按一下**  按鈕，事件處理常式和更新的值**HostViewModel.NextButtonText**. 建置、 執行和按一下按鈕，以查看該按鈕的值**內容**更新。

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.ViewModel.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    ViewModel().NextButtonText(L"Updated Next button text");
}
```

> [!NOTE]
> 變更為[ **TextBox.Text** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.text)傳送至雙向繫結來源[**文字方塊**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)失去焦點，且不超過每個使用者按鍵動作。

**DataTemplate 和 x： 資料類型**

在 [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) 內 (不論是用來做為項目範本、內容範本或標頭範本)，**Path** 的值不是從頁面的內容來解譯，而是根據樣板化的資料物件來解譯。 因此在資料範本中使用 {x:Bind} 時，可以在編譯階段驗證其繫結 (並為它們產生有效率的程式碼)，而 **DataTemplate** 必須使用 **x:DataType** 宣告其資料物件的類型。 對於繫結到 **SampleDataGroup** 物件集合的項目控制項，以下提供的範例可做為其 **ItemTemplate**。

```xaml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**弱型別物件，在您的路徑**

假設有一個名為 SampleDataGroup 的類型，其會實作名為 Title 的字串屬性。 而且您具有的 MainPage.SampleDataGroupAsObject，這是物件類型，但這實際上會傳回 SampleDataGroup 的執行個體的屬性。 由於在此類型物件中找不到 Title 屬性，因此繫結 `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` 將產生編譯錯誤。 解決此問題的方式是在您的 Path 語法中新增一個轉型，如下所示：`<TextBlock Text="{x:Bind ((data:SampleDataGroup)SampleDataGroupAsObject).Title}"/>`。 以下是另一個將 Element 宣告為物件但實際為 TextBlock 的範例：`<TextBlock Text="{x:Bind Element.Text}"/>`。 而且轉型可以解決此問題：`<TextBlock Text="{x:Bind ((TextBlock)Element).Text}"/>`。

**如果您的資料會以非同步方式載入**

支援 **{x:Bind}** 的程式碼是在編譯頁面的部分類別時所產生。 您可以在 `obj` 資料夾中找到這些檔案，其名稱類似於 (適用於 C#) `<view name>.g.cs`。 產生的程式碼包含頁面的 [**Loading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 事件處理常式，而該處理常式會在產生的類別上呼叫 **Initialize** 方法，其代表頁面的繫結。 **Initialize** 接著會呼叫 **Update**，開始在繫結來源與目標之間移動資料。 **Loading** 只會在第一次衡量頁面或使用者控制項的階段之前引發。 如果您的資料是以非同步方式載入，可能就不會在呼叫 **Initialize** 之前備妥。 因此，載入資料之後，您可以呼叫 `this.Bindings.Update();`，強制將一次性繫結初始化。 如果您只需要以非同步方式載入資料的一次性繫結，則比它具有單向繫結和接聽的變更，這種方式初始化它們的成本更低。 如果您的資料不能進行細部變更，而且如果它很可能更新為特定動作的一部分，則您可以讓繫結變成一次性，並隨時呼叫 **Update** 來強制進行手動更新。

> [!NOTE]
> **{x： 繫結}** 不適合用於晚期繫結案例，例如巡覽的 JSON 物件，也就 duck typing 字典結構。 「 Duck typing 」 是弱式的形式輸入依據語彙的相符項目上屬性名稱 (在中，「 如果它將逐步引導，swims，叫起來也像鴨子，然後就 duck 」)。 Duck typing，繫結，以使用**年齡**屬性會使用同樣符合**人員**或**Wine**物件 (假設每一個這些類型必須**年齡**屬性)。 這些情況下，使用 **{Binding}** 標記延伸。

### <a name="binding-object-declared-using-binding"></a>使用 {Binding} 宣告的繫結物件

如果您使用C++/WinRT 或視覺效果C++元件擴充功能 (C++/CX) 然後，若要使用[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)標記延伸模組，您必須新增[ **BindableAttribute**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute)屬性設定為您想要繫結至任何執行階段類別。 若要使用[{x： 繫結}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)，您不需要該屬性。

```cppwinrt
// HostViewModel.idl
// Add this attribute:
[Windows.UI.Xaml.Data.Bindable]
runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
...
```

> [!IMPORTANT]
> 如果您使用[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，然後在[ **BindableAttribute** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute)屬性時才可以使用您已安裝 Windows SDK 版本 10.0.17763.0 (Windows 10版本 1809年），或更新版本。 如果沒有該屬性中，您必須實作[ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider)並[ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty)為了能夠使用的介面[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)標記延伸模組。

[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 預設會假設您繫結到標記頁面的 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext)。 我們會將頁面的 **DataContext** 設定為繫結來源類別的執行個體 (在此案例中為 **HostViewModel** 類型)。 下列範例顯示宣告繫結物件的標記。 我們使用稍早已在「繫結目標」一節中使用的 **Button.Content** 繫結目標，並繫結到 **HostViewModel.NextButtonText** 屬性。

```xaml
<Page xmlns:viewmodel="using:DataBindingInDepth" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel x:Name="viewModelInDataContext"/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.viewModelInDataContext.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    viewModelInDataContext().NextButtonText(L"Updated Next button text");
}
```

請注意我們指定給 **Path** 的值。 這個值是根據頁面的 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 來解譯，在這個範例中，這設定為 **HostViewModel** 的執行個體。 路徑參考 **HostViewModel.NextButtonText** 屬性。 我們可以省略 **Mode**，因為此處採用 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 預設的單向。

UI 元素的 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 的預設值繼承自父代。 當然，您可以明確設定 **DataContext** 來覆寫該預設值，依預設再由子系繼承。 當您想讓多個繫結都使用相同的來源時，在元素上設定 **DataContext** 是很有用的作法。

繫結物件有 **Source** 屬性，預設為宣告繫結的 UI 元素的 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext)。 您可以在繫結上明確設定 **Source**、**RelativeSource** 或 **ElementName**，以覆寫這個預設值 (如需詳細資訊，請參閱 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension))。

內部[ **DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)，則[ **DataContext** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext)會自動設為 正在樣板化的資料物件。 如果項目控制項繫結到任何類型的集合，且這些類型有 **Title** 和 **Description** 字串屬性，以下提供的範例可做為其 **ItemTemplate**。

```xaml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

> [!NOTE]
> 根據預設，變成[ **TextBox.Text** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.text)會傳送至雙向繫結來源[**文字方塊**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)失去焦點。 若要在使用者每次按下按鍵輸入傳送變更，請在標記中的繫結上將 **UpdateSourceTrigger** 設定為 **PropertyChanged**。 您可以也將 **UpdateSourceTrigger** 設定為 **Explicit**，以完全掌控何時將變更傳送到來源。 接著需要處理文字方塊上的事件 (通常是 [**TextBox.TextChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox))、在目標上呼叫 [**GetBindingExpression**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.getbindingexpression) 以取得 [**BindingExpression**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.bindingexpression) 物件，最後呼叫 [**BindingExpression.UpdateSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.bindingexpression.updatesource) 以程式設計方式更新資料來源。

[  **Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) 屬性支援多種繫結語法選項，可讓您繫結到巢狀屬性、附加屬性以及整數和字串索引子。 如需詳細資訊，請參閱 [Property-path 語法](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax)。 繫結到字串索引子可以提供繫結至動態屬性的效果，卻不需實作 [**ICustomPropertyProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider)。 [  **ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) 屬性在元素對元素繫結上很有用。 [  **RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource) 屬性有多種用途，其中之一是以更強大的方式替代 [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 內的範本繫結。 如需其他設定，請參閱 [{Binding} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)和 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 類別。

## <a name="what-if-the-source-and-the-target-are-not-the-same-type"></a>如果來源和目標不是相同類型，該怎麼辦？

如果您想要根據佈林值屬性的值控制 UI 元素的可見度、想要以數值的範圍或趨勢的函數呈現 UI 元素的色彩，或想要在應該為字串的 UI 元素屬性中顯示日期和/或時間值，則需要將值的類型轉換成另一種類型。 在某些情況下，正確的解決方案是從繫結來源類別公開正確類型的另一個屬性，但將轉換邏輯封裝在那裡並維持為可測試。 但是，當您有大量的或組合龐大的來源和目標屬性時，這就顯的缺乏彈性和擴充性。 在那樣的情況下，您有幾個選項：

* 如果使用 {x:Bind}，您可以直接繫結至函式來執行該轉換
* 或者您可以指定一個值轉換器，這是為執行轉換所設計的物件 

## <a name="value-converters"></a>值轉換器

以下是適用於一次性或單向繫結的值轉換器，可將 [**DateTime**](https://docs.microsoft.com/dotnet/api/system.datetime?redirectedfrom=MSDN) 值轉換成包含月份的字串值。 此類別實作 [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)。

```csharp
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

```cppwinrt
// See the "Formatting or converting data values for display" section in the "Data binding overview" topic.
```

以下說明如何在您的繫結物件標記中使用該值轉換器。

```xaml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>
...
<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>
<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

如果為繫結定義 [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter.convert) 參數，那麼繫結引擎就會呼叫 [**Convert**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter.convertback) 和 [**ConvertBack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter) 方法。 從來源傳遞資料時，繫結引擎會呼叫 **Convert** 並將傳回的資料傳遞到目標。 從目標傳遞資料時 (雙向繫結)，繫結引擎會呼叫 **ConvertBack**，並將傳回的資料傳遞到來源。

轉換子也有選擇性參數：[**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage)，這可讓您指定要在轉換中，使用的語言並[ **ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter)，以便將參數傳遞轉換邏輯。 如需使用轉換器參數的範例，請參閱 [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)。

> [!NOTE]
> 如果轉換卻發生錯誤，並不會擲回例外狀況。 而是傳回 [**DependencyProperty.UnsetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.unsetvalue)，以便停止資料傳送。

如果要在繫結來源無法解析時顯示預設值，請在標記中的繫結物件上設定 **FallbackValue** 屬性。 這對控制代碼轉換與格式錯誤很有用。 繫結到異質類型繫結集合中的所有物件上可能不存在的來源屬性也很有用。

如果您將文字控制項繫結到不是字串的值，資料繫結引擎會將該值轉換為字串。 如果該值是參照類型，資料繫結引擎會呼叫 [**ICustomPropertyProvider.GetStringRepresentation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.icustompropertyprovider.getstringrepresentation) 或 [**IStringable.ToString**](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nf-windows-foundation-istringable-tostring) (若有的話) 來擷取字串值，否則會呼叫 [**Object.ToString**](https://docs.microsoft.com/dotnet/api/system.object.tostring?redirectedfrom=MSDN#System_Object_ToString)。 但請注意，繫結引擎會忽略隱藏基底類別實作的任何 **ToString** 實作。 子類別實作應該會改而覆寫基底類別 **ToString** 方法。 同樣地，在原生語言中，所有 Managed 物件似乎都實作 [**ICustomPropertyProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider) 與 [**IStringable**](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nn-windows-foundation-istringable)。 不過，對 **GetStringRepresentation** 與 **IStringable.ToString** 的所有呼叫都會路由傳送到 **Object.ToString** 或該方法的覆寫，永遠不會路由傳送到隱藏基底類別實作的新 **ToString** 實作。

> [!NOTE]
> 從 Windows 10 版本 1607 開始，XAML 架構針對可見度轉換器提供了內建布林值。 轉換器會將 **true** 對應至 **Visible** 列舉值，並將 **false** 對應至 **Collapsed**，這樣您就可以將 Visibility 屬性繫結至布林值而不用建立轉換器。 若要使用內建轉換器，您 App 的最低目標 SDK 版本必須為 14393 或更新版本。 當您的 App 是以舊版 Windows 10 為目標時，您就無法使用它。 如需目標版本的相關詳細資訊，請參閱[版本調適型程式碼](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

## <a name="function-binding-in-xbind"></a>{x:Bind} 中的函式繫結

{x:Bind} 讓繫結路徑中的最後一個步驟可以是函式。 這可以用來執行轉換，以及執行和一個以上屬性相依的繫結。 請參閱[ **x： 繫結中的函式**](function-bindings.md)

<span id="resource-dictionaries-with-x-bind"/>

## <a name="element-to-element-binding"></a>項目繫結

您可以將一個 XAML 項目屬性繫結至另一個 XAML 元素的屬性。 以下是範例，在標記中的結果。

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

> [!IMPORTANT]
> 必要的工作流程項目繫結使用的C++/WinRT，請參閱[項目繫結](/windows/uwp/cpp-and-winrt-apis/binding-property#element-to-element-binding)。

## <a name="resource-dictionaries-with-xbind"></a>使用 {x:Bind} 的資源字典

[{X:Bind} 標記延伸](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)取決於程式碼產生，因此需要一個程式碼後置檔案，由其中的建構函式呼叫 **InitializeComponent** (初始化產生的程式碼)。 重複使用資源字典時需要具現化其類別 (以呼叫 **InitializeComponent**)，而不是參考其檔案名稱。 以下是在現有的資源字典中使用 {x:Bind} 的範例。

TemplatesResourceDictionary.xaml

```xaml
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

```csharp
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
```

MainPage.xaml

```xaml
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

## <a name="event-binding-and-icommand"></a>事件繫結和 ICommand

[{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 支援一個稱為「事件繫結」的功能。 除了在程式碼後置檔案中使用方法來處理事件，這項功能可讓您使用繫結來指定事件的處理常式。 假設您的 **MainPage** 類別有 **RootFrame** 屬性。

```csharp
public sealed partial class MainPage : Page
{
    ...
    public Frame RootFrame { get { return Window.Current.Content as Frame; } }
}
```

同樣地，您可以將按鈕的 **Click** 事件繫結到 **RootFrame** 屬性所傳回的 **Frame** 物件的方法。 請注意，我們也將按鈕的 **IsEnabled** 屬性繫結到相同 **Frame** 的另一個成員。

```xaml
<AppBarButton Icon="Forward" IsCompact="True"
IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
Click="{x:Bind RootFrame.GoForward}"/>
```

無法透過這項技巧使用多載方法來處理事件。 此外，如果處理事件的方法有參數，則必須全部都可以從事件的所有參數的類型分別指派。 在此案例中，[**Frame.GoForward**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goforward) 未多載，也沒有參數 (但即使接受兩個 **object** 參數，仍然有效)。 [**Frame.GoBack** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goback)超載的不過，因此我們不能利用這項技術會使用該方法。

事件繫結技巧類似於實作與使用命令 (命令是一個屬性，可傳回實作 [**ICommand**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) 介面的物件)。 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 與 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 都能搭配命令一起使用。 因此，您不需要實作命令模式許多次，您可以使用 [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper) 範例 (在 [Common] 資料夾) 中的**DelegateCommand** 協助程式類別。

## <a name="binding-to-a-collection-of-folders-or-files"></a>繫結到資料夾或檔案集合

您可以使用 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) 命名空間中的 API 來擷取資料夾和檔案資料。 不過，**GetFilesAsync**、**GetFoldersAsync** 及 **GetItemsAsync** 這些各式方法並不會傳回適合繫結到清單控制項的值。 您必須改為繫結到 [**FileInformationFactory**](https://docs.microsoft.com/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizedfilesvector) 類別之 [**GetVirtualizedFilesVector**](https://docs.microsoft.com/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizedfoldersvector)、[**GetVirtualizedFoldersVector**](https://docs.microsoft.com/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizeditemsvector) 及 [**GetVirtualizedItemsVector**](https://docs.microsoft.com/uwp/api/Windows.Storage.BulkAccess.FileInformationFactory) 方法的傳回值。 下列來自 [StorageDataSource 和 GetVirtualizedFilesVector 範例](https://go.microsoft.com/fwlink/p/?linkid=228621)的程式碼範例示範一般的使用模式。 請記得在您的應用程式套件資訊清單中宣告 **picturesLibrary** 功能，並確認您的 [圖片庫] 資料夾中有圖片。

```csharp
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
```

您通常會使用這個方法來建立檔案和資料夾資訊的唯讀檢視。 您可以建立檔案和資料夾屬性的雙向繫結，例如讓使用者在音樂檢視中給歌曲評分。 不過，所有變更都必須等到您呼叫適當的 **SavePropertiesAsync** 方法 (例如 [**MusicProperties.SavePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.musicproperties.savepropertiesasync)) 之後才能保留。 您應該在項目失去焦點時認可變更，因為這會觸發選取項目重設。

請注意，使用這項技術的雙向繫結只適用於已編製索引的位置 (例如 音樂)。 您可以呼叫 [**FolderInformation.GetIndexedStateAsync**](https://docs.microsoft.com/uwp/api/windows.storage.bulkaccess.folderinformation.getindexedstateasync) 方法來判斷位置是否已編製索引。

另請注意，虛擬化向量可能會在填入某些項目的值之前先傳回 **null**。 例如，使用與虛擬化向量繫結之清單控制項的 [**SelectedItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 值之前，您應該先檢查是否有 **null**，或者改用 [**SelectedIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)。

## <a name="binding-to-data-grouped-by-a-key"></a>繫結到依索引鍵分組的資料

如果需要項目的一般集合 (比方說，書籍，由**BookSku**類別) 和您群組使用通用的屬性做為索引鍵的項目 ( **BookSku.AuthorName**內容，例如) 然後其結果稱為群組的資料。 資料分組之後就不再是一般集合。 群組的資料是群組物件，其中每個群組物件都有的集合

- 索引鍵，和
- 其屬性符合該索引鍵的項目集合。

若要再次充分書籍的例子，結果的群組將書籍的作者名稱會導致其中每個群組都有的作者名稱群組的集合

- 索引鍵，也就是作者姓名，以及
- 集合**BookSku**s 其**AuthorName**屬性符合群組的索引鍵。

一般而言，若要顯示集合，您需要將項目控制項 (例如 [**ListView**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 或 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)) 的 [**ItemsSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 直接繫結到傳回集合的屬性。 如果是一般項目集合，則不需要採取任何特別的動作。 但如果是群組物件的集合 (例如繫結到分組資料時)，則需要一個稱為 [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 的中繼物件的服務，此物件位於項目控制項和繫結來源之間。 您需要將 **CollectionViewSource** 繫結到傳回分組資料的屬性，並將項目控制項繫結到 **CollectionViewSource**。 **CollectionViewSource** 額外的好處是它可以追蹤目前的項目，可讓您將多個項目控制項全部繫結到相同的 **CollectionViewSource**，以保持同步。 您可以透過 [**CollectionViewSource.View**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.icollectionview.currentitem) 屬性所傳回的物件的 [**ICollectionView.CurrentItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.collectionviewsource.view) 屬性，以程式設計方式存取目前的項目。

若要啟用 [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 的分組功能，請將 [**IsSourceGrouped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.collectionviewsource.issourcegrouped) 設定為 **true**。 不論您是否也需要設定 [**ItemsPath**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.collectionviewsource.itemspath) 屬性，這完全取決於您如何撰寫群組物件。 撰寫群組物件有兩種方式：「是一個群組」模式和「有一個群組」模式。 在「是一個群組」模式中，群組物件衍生自集合類型 (例如 **List&lt;T&gt;** )，因此群組物件本身實際上是項目群組。 使用這個模式時，您不需要設定 **ItemsPath**。 在「有一個群組」模式中，群組物件有集合類型 (例如 **List&lt;T&gt;** ) 的一或多個屬性，因此群組以屬性形式而「有一個」項目群組 (或以數個屬性的形式而有數個項目群組)。 使用這個模式時，您需要將 **ItemsPath** 設定為包含項目群組的屬性名稱。

下列範例說明「有一個群組」模式。 頁面類別有一個名為 [**ViewModel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 的屬性，可傳回檢視模型的執行個體。 [  **CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 繫結至檢視模型的 **Authors** 屬性 (**Authors** 為群組物件的集合)，也指定是 **Author.BookSkus** 屬性包含分組項目。 最後，[**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 繫結至 **CollectionViewSource**，並定義其群組樣式以呈現群組中的項目。

```csharp
<Page.Resources>
    <CollectionViewSource
    x:Name="AuthorHasACollectionOfBookSku"
    Source="{x:Bind ViewModel.Authors}"
    IsSourceGrouped="true"
    ItemsPath="BookSkus"/>
</Page.Resources>
...
<GridView
ItemsSource="{x:Bind AuthorHasACollectionOfBookSku}" ...>
    <GridView.GroupStyle>
        <GroupStyle
            HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
    </GridView.GroupStyle>
</GridView>
```

「是一個群組」模式有兩種實作方式。 其中一種方法是撰寫您自己的群組類別。 從 **List&lt;T&gt;** 衍生類別 (其中，*T* 是項目類型)。 例如， `public class Author : List<BookSku>` 。 第二種方法是使用 [LINQ](https://docs.microsoft.com/previous-versions/bb397926(v=vs.140)) 運算式，從 **BookSku** 項目的相似屬性值，動態建立群組物件 (和群組類別)。 這個方法—維護一般項目清單並即時將它們組合在一起—常見於從雲端服務存取資料的 app。 這可讓您依作者或內容類型 (舉例)，靈活地將書籍分組，而不需要 **Author** 和 **Genre** 之類的特殊群組類別。

下列範例使用 [LINQ](https://docs.microsoft.com/previous-versions/bb397926(v=vs.140)) 說明「是一個群組」模式。 這次我們依內容類型將書籍分組，並在群組標頭中顯示內容類型名稱。 這是由群組 [**Key**](https://docs.microsoft.com/dotnet/api/system.linq.igrouping-2.key?redirectedfrom=MSDN#System_Linq_IGrouping_2_Key) 值之參照中的 "Key" 屬性路徑所指示。

```csharp
using System.Linq;
...
private IOrderedEnumerable<IGrouping<string, BookSku>> genres;

public IOrderedEnumerable<IGrouping<string, BookSku>> Genres
{
    get
    {
        if (this.genres == null)
        {
            this.genres = from book in this.bookSkus
                          group book by book.genre into grp
                          orderby grp.Key
                          select grp;
        }
        return this.genres;
    }
}
```

請記住，當使用 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 和資料範本時，我們需要設定 **x:DataType** 值來表示繫結到的類型。 如果類型是泛型，則無法表達在標記中，我們需要在群組樣式標頭範本中改用 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)。

```xaml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{x:Bind Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{x:Bind GenreIsACollectionOfBookSku}">
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

[  **SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 控制項是讓使用者檢視及瀏覽分組資料的絕佳方式。 [Bookstore2](https://go.microsoft.com/fwlink/?linkid=532952) 範例 app 說明如何使用 **SemanticZoom**。 在該 app 中，您可以檢視依作者分組的書籍清單 (放大檢視)，也可以縮小來查看作者的捷徑清單 (縮小檢視)。 與捲動書籍清單相比，捷徑清單可提供更快速的瀏覽。 放大和縮小檢視實際上是繫結到相同 **CollectionViewSource** 的 **ListView** 或 **GridView** 控制項。

![SemanticZoom 的圖例](images/sezo.png)

當您繫結到階層式資料時—例如分類內的子分類—您可以選擇在 UI 中以一系列項目控制項來顯示階層式層級。 選取一個項目控制項會決定後續項目控制項的內容。 您可以將每個清單繫結到它自身的 [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 並將 **CollectionViewSource** 執行個體一起繫結在鏈結中，使清單保持同步。 這稱為主要/詳細資料 (或清單/詳細資料) 檢視。 如需詳細資訊，請參閱[如何繫結到階層式資料並建立主要/詳細資料檢視](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md)。

<span id="debugging"/>

## <a name="diagnosing-and-debugging-data-binding-problems"></a>診斷和偵錯資料繫結的問題

繫結標記包含屬性的名稱 (針對 C#，有時還有欄位和方法)。 因此，當您重新命名屬性時，也需要變更任何參考它的繫結。 忘記這樣做會造成常見的資料繫結錯誤，而且您的 app 將無法編譯或無法正確執行。

由 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 和 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 建立的繫結物件在功能上大致相同。 但是 {x:Bind} 有繫結來源的類型資訊，而且會在編譯階段產生原始程式碼。 使用 {x:Bind} 時，可以對其餘的程式碼進行同樣的問題偵測。 這包括編譯時驗證您的繫結運算式，以及在產生做為頁面部分類別的原始程式碼中設定中斷點來偵錯。 您可以在 `obj` 資料夾的檔案中找到這些類別，名稱類似 (適用於 C#) `<view name>.g.cs`)。 如果繫結發生問題，請在 Microsoft Visual Studio 偵錯工具中開啟 [發生未處理的例外狀況時中斷]。  偵錯工具會在那一刻中斷執行，讓您偵錯出了什麼問題。 對於繫結來源節點圖形的每個部分，{x:Bind} 產生的程式碼都採用相同的模式，您可以使用 **呼叫堆疊** 視窗中的資訊，協助您判斷導致問題的呼叫序列。

[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 沒有繫結來源的類型資訊。 但在附加偵錯工具的情況下執行您的 app 時，所有繫結錯誤都會顯示在 Visual Studio 的 [輸出] 視窗中。 

## <a name="creating-bindings-in-code"></a>在程式碼中建立繫結

**附註**  本節只適用於[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)，因為您無法建立[{x： 繫結}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)在程式碼中的繫結。 不過，部分與 {x:Bind} 相同的優點可以利用 [**DependencyObject.RegisterPropertyChangedCallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.registerpropertychangedcallback) 來達成，讓您能夠在任何相依性屬性上登錄變更通知。

您也可以使用程序性程式碼來取代 XAML，將 UI 元素連結到資料。 要這樣做，請建立新的 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 物件、設定適當的屬性，然後呼叫 [**FrameworkElement.SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding) 或 [**BindingOperations.SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.bindingoperations.setbinding)。 當您想在執行階段選擇繫結屬性值，或在多個控制項間共用單一繫結時，以程式設計方式建立繫結會很有用。 不過，需注意在呼叫 **SetBinding** 之後，您就無法變更繫結屬性值。

下列範例示範如何在程式碼中實作繫結。

```xaml
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

```vbnet
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
```

## <a name="xbind-and-binding-feature-comparison"></a>{x:Bind} 和 {Binding} 功能比較

| 功能 | {x:Bind} | {Binding} | 附註 |
|---------|----------|-----------|-------|
| Path 是預設屬性 | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Path 屬性 | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | 在 x:Bind 中，Path 預設以 Page 為根目錄，而不是 DataContext。 | 
| 索引子 | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | 繫結至集合中指定的項目。 支援僅整數索引。 | 
| 附加的屬性 | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | 使用括號指定附加屬性。 如果未在 XAML 命名空間中宣告屬性，則需要以 xml 命名空間為其加上首碼，而此命名空間應對應到文件最前面的程式碼命名空間。 | 
| 轉型 | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | 不需要。 | 使用括號指定轉型。 如果未在 XAML 命名空間中宣告屬性，則需要以 xml 命名空間為其加上首碼，而此命名空間應對應到文件最前面的程式碼命名空間。 | 
| Converter | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | Converter 必須在 Page/ResourceDictionary 的根目錄或 App.xaml 中宣告。 | 
| ConverterParameter、ConverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | Converter 必須在 Page/ResourceDictionary 的根目錄或 App.xaml 中宣告。 | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | 繫結運算式的分葉為 null 時使用。 使用單引號指定字串值。 | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | 繫結路徑的任何部分 (除了分葉) 為 null 時使用。 | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | 您使用 {x:Bind} 繫結到欄位；Path 預設以 Page 為根目錄，任何具名的項目都可以透過它的欄位來存取。 | 
| RelativeSource:Self | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | 使用 {X:Bind} 來命名元素，並在 Path 中使用它的名稱。 | 
| RelativeSource:TemplatedParent | 不需要 | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | 使用 {x： 繫結} TargetType ControlTemplate 上的會指出繫結至範本的父代。 {Binding} 的一般範本繫結可用在控制項範本中用於大部分用途。 但在需要使用轉換器或雙向繫結的情況下，請使用 TemplatedParent。&lt; | 
| `Source` | 不需要 | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | {X： 繫結} 您可以直接使用具名的項目使用屬性或靜態的路徑。 | 
| 模式 | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | Mode 可以是 OneTime、OneWay 或 TwoWay。 {x:Bind} 預設為 OneTime。{Binding} 預設為 OneWay。 | 
| UpdateSourceTrigger | `{x:Bind Name, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}` | `{Binding UpdateSourceTrigger=PropertyChanged}` | UpdateSourceTrigger 可以是 Default、LostFocus 或 PropertyChanged。 {x:Bind} 不支援 UpdateSourceTrigger=Explicit。 {x:Bind} 一律使用 PropertyChanged 行為，但 TextBox.Text 除外，它會使用 LostFocus 行為。 | 


