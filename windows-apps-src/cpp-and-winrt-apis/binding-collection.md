---
description: 可有效地繫結至 XAML 項目控制項的集合稱為*可觀察的* 集合。 本主題示範實作和使用可觀察集合的方法，以及如何將 XAML 項目控制項繫結至它。
title: XAML 項目控制項；繫結至一個 C++/WinRT 集合
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、XAML、控制項、繫結、集合
ms.localizationpriority: medium
ms.openlocfilehash: 7669c6536f28d5f979567f5b433dbf614800bec3
ms.sourcegitcommit: d23dab1533893b7fe0f01ca6eb273edfac4705e6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2019
ms.locfileid: "65627681"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>XAML 項目控制項；繫結至一個 C++/WinRT 集合

可有效地繫結至 XAML 項目控制項的集合稱為*可觀察的* 集合。 這個主意是以軟體設計模式為基礎稱為*觀察者模式*。 本主題說明如何實作可觀察的集合中[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，和如何繫結 XAML 項目給他們的控制項。

如果您想要遵循本主題中，則我們建議您先建立專案中所述[XAML 控制項，繫結至C++/WinRT 屬性](binding-property.md)。 本主題將更多程式碼加入至該專案，並將會加入該主題所述的概念。

> [!IMPORTANT]
> 如需支援您了解如何使用 C++/WinRT 使用及撰寫執行階段類別的基本概念和詞彙，請查閱[使用 C++/WinRT 使用API](consume-apis.md)和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="what-does-observable-mean-for-a-collection"></a>對一個集合來說，*可觀察*的意義是什麼？
如果表示集合的執行階段類別選擇引發 [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) 事件，每當將元素新增至它或從它移除，執行階段類別便是可觀察的集合。 XAML 項目控制項藉由擷取更新的集合並且更新其本身以顯示目前的元素，可繫結至以及處理這些事件。

> [!NOTE]
> 如需安裝和使用的資訊C++WinRT Visual Studio 擴充功能 (VSIX) 和 NuGet 套件 （其同時提供專案範本，並建置支援），請參閱[Visual Studio 支援C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>將 **BookSkus** 集合新增至 **BookstoreViewModel**

在 [XAML 控制項；繫結至 C++/WinRT 屬性](binding-property.md) 中，我們已將類型 **BookSku** 的屬性新增至我們主要的檢視模型。 在此步驟中，我們將使用[ **winrt::single_threaded_observable_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) factory 函式樣板，以協助我們實作的可觀察集合**BookSku**上相同的檢視模型。

在 `BookstoreViewModel.idl` 中宣告一個新的屬性。

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<IInspectable> BookSkus{ get; };
}
...
```

> [!IMPORTANT]
> 在上述的 MIDL 3.0 清單中，請注意，該類**BookSkus**屬性是[ **IObservableVector** ](/uwp/api/windows.foundation.collections.ivector_t_)的[ **IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). 在本主題的下一步 區段中，我們將會繫結的項目來源[ **ListBox** ](/uwp/api/windows.ui.xaml.controls.listbox)來**BookSkus**。 清單方塊是以項目控制項，並正確設定[ **ItemsControl.ItemsSource** ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)屬性，您必須將它設定為型別的值**IObservableVector** (或是**IVector**) 的**IInspectable**，或是屬於這類的互通性型別[ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)。

儲存並建置。 複製從存取子虛設常式`BookstoreViewModel.h`並`BookstoreViewModel.cpp`中`\Bookstore\Bookstore\Generated Files\sources`資料夾 (如需詳細資訊，請參閱先前的主題[XAML 控制項，繫結至C++/WinRT 屬性](binding-property.md))。 實作這些存取子虛設常式，就像這樣。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookstoreViewModel::BookSkus()
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

在 `MainPage.cpp` 中，將一行程式碼新增至 **按一下** 事件處理常式，將一本書附加至集合。

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

現在建置並執行專案。 按一下按鈕執行 **按一下** 事件處理常式。 我們所見 **Append** 的實作引發一個事件，讓 UI 知道集合已變更；且 **ListBox** 重新查詢集合，更新其自己的 **Items** 值。 就像以前一樣，變更書籍其中之一的標題；且同時在按鈕與清單方塊上反映該標題的變更。

## <a name="important-apis"></a>重要 API
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [winrt::make 函式樣板](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 取用 API](consume-apis.md)
* [使用 C++/WinRT 撰寫 API](author-apis.md)
