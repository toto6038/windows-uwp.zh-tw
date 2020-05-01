---
description: 可有效地繫結至 XAML 項目控制項的集合稱為「可觀察的」  集合。 本主題示範實作和使用可觀察集合的方法，以及如何將 XAML 項目控制項繫結至它。
title: XAML 項目控制項；繫結至一個 C++/WinRT 集合
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, XAML, 控制項, 繫結, 集合
ms.localizationpriority: medium
ms.openlocfilehash: a98056190d035910a8ed83d2f37799a98b685ce6
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "70304522"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>XAML 項目控制項；繫結至一個 C++/WinRT 集合

可有效地繫結至 XAML 項目控制項的集合稱為「可觀察的」  集合。 這個主意是以軟體設計模式為基礎稱為*觀察者模式*。 本主題顯示在 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中實作可觀察集合，以及將 XAML 項目繫結至這些集合的方法 (如需背景資訊，請參閱[資料繫結](/windows/uwp/data-binding))。

如果您想要按照本主題的步驟進行，建議先建立 [XAML 控制項；繫結一個 C++/WinRT 屬性](binding-property.md)一文所述的專案。 本主題會將更多程式碼加入至該專案，且能進一步輔助本主題所介紹的概念。

> [!IMPORTANT]
> 如需支援您了解如何使用 C++/WinRT 使用及撰寫執行階段類別的基本概念和詞彙，請參閱[使用 C++/WinRT 使用 API](consume-apis.md) 和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="what-does-observable-mean-for-a-collection"></a>對一個集合來說，「可觀察」  有何意義？
如果代表集合的執行階段類別，每當將元素新增至該類別或從中移除時，會選擇引發 [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) 事件，則該執行階段類別便是可觀察的集合。 XAML 項目控制項藉由擷取更新的集合並且更新其本身以顯示目前的元素，可繫結至以及處理這些事件。

> [!NOTE]
> 如需安裝和使用 C++/WinRT Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援) 的資訊，請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>將 **BookSkus** 集合新增至 **BookstoreViewModel**

在 [XAML 控制項；繫結至 C++/WinRT 屬性](binding-property.md) 中，我們已將類型 **BookSku** 的屬性新增至我們主要的檢視模型。 在此步驟，我們會使用 [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 原廠函式範本，協助我們在相同的檢視模型上實作 **BookSku** 的可觀察集合。

在 `BookstoreViewModel.idl` 中宣告一個新的屬性。

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
}
...
```

> [!NOTE]
> 在上述的 MIDL 3.0 清單中，請注意，**BookSkus** 屬性的類別是 [BookSku**的**](/uwp/api/windows.foundation.collections.ivector_t_)IObservableVector  。 在本主題的下一節中，我們會將 [**ListBox**](/uwp/api/windows.ui.xaml.controls.listbox) 的項目來源繫結至 **BookSkus**。 清單方塊是項目控制項，若要正確設定 [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 屬性，您需要將它設定為 **IObservableVector** 或 **IVector** 類型的值，或互通性類型值，例如 [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)。

> [!WARNING]
> 本主題中所顯示的程式碼適用於 C++/WinRT 版本2.0.190530.8 和更新版本。 如果您使用較早版本，則需要對顯示的程式碼進行一些微幅調整。 在上述的 MIDL 3.0 清單中，將 **BookSkus** 屬性變更為 [**IInspectable**](/uwp/api/windows.foundation.collections.ivector_t_) 的 [**IObservableVector**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)。 然後在您的實作中也使用 **IInspectable** (而非 **BookSku**)。

儲存並建置。 複製 `BookstoreViewModel.h` 資料夾之中 `BookstoreViewModel.cpp` 和 `\Bookstore\Bookstore\Generated Files\sources` 的存取子虛設常式 (如需詳細資訊，請參閱先前的主題 [XAML 控制項，繫結至 C++/WinRT 屬性](binding-property.md))。 實作像這樣的這些存取子虛設常式。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="bind-a-listbox-to-the-bookskus-property"></a>將一個 ListBox 繫結至 **BookSkus** 屬性
開啟 `MainPage.xaml`，其中包含我們主要 UI 頁面的 XAML 標記。 在相同的 **StackPanel** 中新增下列標記做為**按鈕**。

```xaml
<ListBox ItemsSource="{x:Bind MainViewModel.BookSkus}">
    <ItemsControl.ItemTemplate>
        <DataTemplate x:DataType="local:BookSku">
            <TextBlock Text="{x:Bind Title, Mode=OneWay}"/>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ListBox>
```

在 `MainPage.cpp` 中，將一行程式碼新增至 **Click** 事件處理常式，將一本書附加至集合。

```cppwinrt
// MainPage.cpp
...
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    MainViewModel().BookSkus().Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
}
...
```

現在請建置並執行專案。 按一下按鈕執行 [按一下]  事件處理常式。 我們所見 **Append** 的實作引發一個事件，讓 UI 知道集合已變更；且 **ListBox** 重新查詢集合，更新其自己的 **Items** 值。 就像以前一樣，變更書籍其中之一的標題；且同時在按鈕與清單方塊上反映該標題的變更。

## <a name="important-apis"></a>重要 API
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [winrt::make 函式範本](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 取用 API](consume-apis.md)
* [使用 C++/WinRT 撰寫 API](author-apis.md)
