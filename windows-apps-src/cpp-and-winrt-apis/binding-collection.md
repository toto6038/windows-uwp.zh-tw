---
author: stevewhims
description: 可有效地繫結至 XAML 項目控制項的集合稱為*可觀察的* 集合。 本主題示範實作和使用可觀察集合的方法，以及如何將 XAML 項目控制項繫結至它。
title: XAML 項目控制項；繫結至一個 C++/WinRT 集合
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、XAML、控制項、繫結、集合
ms.localizationpriority: medium
ms.openlocfilehash: 22594c1cfc503b28163d9fca1f46a6861a4f59ad
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2018
ms.locfileid: "5132142"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>XAML 項目控制項；繫結至一個 C++/WinRT 集合

可有效地繫結至 XAML 項目控制項的集合稱為*可觀察的* 集合。 這個主意是以軟體設計模式為基礎稱為*觀察者模式*。 本主題示範如何實作可觀察的集合中[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，以及如何將 XAML 繫結項目控制項給他們。

在建立於 [XAML 控制項；繫結至 C++/WinRT 屬性](binding-property.md) 中的專案裡組建本逐步解說，並新增到該主題中所述的概念。

> [!IMPORTANT]
> 如需支援您了解如何使用 C++/WinRT 使用及撰寫執行階段類別的基本概念和詞彙，請參閱 [使用 C++/WinRT 使用API](consume-apis.md)和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="what-does-observable-mean-for-a-collection"></a>對一個集合來說，*可觀察*的意義是什麼？
如果表示集合的執行階段類別選擇引發 [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) 事件，每當將元素新增至它或從它移除，執行階段類別便是可觀察的集合。 XAML 項目控制項藉由擷取更新的集合並且更新其本身以顯示目前的元素，可繫結至以及處理這些事件。

> [!NOTE]
> 如需有關安裝和使用 C++/WinRT Visual Studio 擴充功能 (VSIX) (提供專案範本的支援，以及 C++/WinRT MSBuild 屬性和目標) 的資訊，請參閱 [C++/WinRT 和 VSIX 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>將 **BookSkus** 集合新增至 **BookstoreViewModel**

在 [XAML 控制項；繫結至 C++/WinRT 屬性](binding-property.md) 中，我們已將類型 **BookSku** 的屬性新增至我們主要的檢視模型。 在此步驟中，我們會使用[**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) factory 函式範本，以協助我們在相同的檢視模型上實作可觀察的集合，一個**BookSku** 。

> [!NOTE]
> 如果您還沒有安裝 Windows SDK 版本 10.0.17763.0 (Windows 10，版本 1809年)，或更新版本，然後查看[如果有較舊版本的 Windows SDK](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) ，您可以使用而不是**winrt::single_ 可觀察的向量範本的清單threaded_observable_vector**。

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
> 在上述 MIDL 3.0 清單，請注意， **BookSkus**屬性類型的[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) 。 本主題的下一節中，我們將繫結[**ListBox**](/uwp/api/windows.ui.xaml.controls.listbox)項目的來源至**BookSkus**。 清單方塊是一個項目控制項，以及正確設定[**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)屬性，您需要設定它的值類型**IObservableVector** （或**IVector**） 的**IInspectable**，或的互通性類型，例如[**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)。

儲存並建置。 從 `Generated Files` 資料夾中的 `BookstoreViewModel.h` 與 `BookstoreViewModel.cpp` 複製存取子虛設常式，並加以執行。

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
* [winrt::make 函式範本](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 使用 API ](consume-apis.md)
* [使用 C++/WinRT 撰寫 API ](author-apis.md)
