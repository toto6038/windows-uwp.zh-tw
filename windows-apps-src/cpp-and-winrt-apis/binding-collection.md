---
author: stevewhims
description: 可有效地繫結至 XAML 項目控制項的集合稱為*可觀察的* 集合。 本主題示範實作和使用可觀察集合的方法，以及如何將 XAML 項目控制項繫結至它。
title: XAML 項目控制項；繫結至一個 C++/WinRT 集合
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、XAML、控制項、繫結、集合
ms.localizationpriority: medium
ms.openlocfilehash: 9ba935b1a5316c2d7af9c7681705595efea7ca08
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2018
ms.locfileid: "3661231"
---
# <a name="xaml-items-controls-bind-to-a-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-collection"></a>XAML 項目控制項；繫結至一個 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 集合
> [!NOTE]
> **正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。**

可有效地繫結至 XAML 項目控制項的集合稱為*可觀察的* 集合。 這個主意是以軟體設計模式為基礎稱為*觀察者模式*。 本主題示範在 C++/WinRT 中實作可觀察集合的方法，以及如何將 XAML 項目控制項繫結至它們。

在建立於 [XAML 控制項；繫結至 C++/WinRT 屬性](binding-property.md) 中的專案裡組建本逐步解說，並新增到該主題中所述的概念。

> [!IMPORTANT]
> 如需支援您了解如何使用 C++/WinRT 使用及撰寫執行階段類別的基本概念和詞彙，請參閱 [使用 C++/WinRT 使用API](consume-apis.md)和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="what-does-observable-mean-for-a-collection"></a>對一個集合來說，*可觀察*的意義是什麼？
如果表示集合的執行階段類別選擇引發 [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) 事件，每當將元素新增至它或從它移除，執行階段類別便是可觀察的集合。 XAML 項目控制項藉由擷取更新的集合並且更新其本身以顯示目前的元素，可繫結至以及處理這些事件。

> [!NOTE]
> 如需有關安裝和使用 C++/WinRT Visual Studio 擴充功能 (VSIX) (提供專案範本的支援，以及 C++/WinRT MSBuild 屬性和目標) 的資訊，請參閱 [C++/WinRT 和 VSIX 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="implement-singlethreadedobservablevectorlttgt"></a>實作 **single_threaded_observable_vector&lt;T&gt;**
有一個可觀察的向量範本會很有用，做為實用、一般用途的 [**IObservableVector&lt;T&gt;**](/uwp/api/windows.foundation.collections.iobservablevector_t_) 實作。 以下類別清單稱為 **single_threaded_observable_vector\<T\>**。

> [!NOTE]
> 如果您已安裝[Windows 10 SDK 預覽版 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)，或更新版本，然後您可以只是直接使用**winrt < 所 >** factory 函式而不是下面所列的程式碼 （我們將說明的確切的程式碼更新版本在此主題）。 如果您還不在 SDK 版本，則它便會容易能切換，當您使用**winrt**函式的程式碼清單版本。

```cppwinrt
// single_threaded_observable_vector.h
#pragma once

namespace winrt::Bookstore::implementation
{
    using namespace Windows::Foundation::Collections;

    template <typename T>
    struct single_threaded_observable_vector : implements<single_threaded_observable_vector<T>,
        IObservableVector<T>,
        IVector<T>,
        IVectorView<T>,
        IIterable<T>>
    {
        event_token VectorChanged(VectorChangedEventHandler<T> const& handler)
        {
            return m_changed.add(handler);
        }

        void VectorChanged(event_token const cookie)
        {
            m_changed.remove(cookie);
        }

        T GetAt(uint32_t const index) const
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            return m_values[index];
        }

        uint32_t Size() const noexcept
        {
            return static_cast<uint32_t>(m_values.size());
        }

        IVectorView<T> GetView()
        {
            return *this;
        }

        bool IndexOf(T const& value, uint32_t& index) const noexcept
        {
            index = static_cast<uint32_t>(std::find(m_values.begin(), m_values.end(), value) - m_values.begin());
            return index < m_values.size();
        }

        void SetAt(uint32_t const index, T const& value)
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values[index] = value;
            m_changed(*this, make<args>(CollectionChange::ItemChanged, index));
        }

        void InsertAt(uint32_t const index, T const& value)
        {
            if (index > m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.insert(m_values.begin() + index, value);
            m_changed(*this, make<args>(CollectionChange::ItemInserted, index));
        }

        void RemoveAt(uint32_t const index)
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.erase(m_values.begin() + index);
            m_changed(*this, make<args>(CollectionChange::ItemRemoved, index));
        }

        void Append(T const& value)
        {
            ++m_version;
            m_values.push_back(value);
            m_changed(*this, make<args>(CollectionChange::ItemInserted, Size() - 1));
        }

        void RemoveAtEnd()
        {
            if (m_values.empty())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.pop_back();
            m_changed(*this, make<args>(CollectionChange::ItemRemoved, Size()));
        }

        void Clear() noexcept
        {
            ++m_version;
            m_values.clear();
            m_changed(*this, make<args>(CollectionChange::Reset, 0));
        }

        uint32_t GetMany(uint32_t const startIndex, array_view<T> values) const
        {
            if (startIndex >= m_values.size())
            {
                return 0;
            }

            uint32_t actual = static_cast<uint32_t>(m_values.size() - startIndex);

            if (actual > values.size())
            {
                actual = values.size();
            }

            std::copy_n(m_values.begin() + startIndex, actual, values.begin());
            return actual;
        }

        void ReplaceAll(array_view<T const> value)
        {
            ++m_version;
            m_values.assign(value.begin(), value.end());
            m_changed(*this, make<args>(CollectionChange::Reset, 0));
        }

        IIterator<T> First()
        {
            return make<iterator>(this);
        }

    private:

        std::vector<T> m_values;
        event<VectorChangedEventHandler<T>> m_changed;
        uint32_t m_version{};

        struct args : implements<args, IVectorChangedEventArgs>
        {
            args(CollectionChange const change, uint32_t const index) :
                m_change(change),
                m_index(index)
            {
            }

            CollectionChange CollectionChange() const
            {
                return m_change;
            }

            uint32_t Index() const
            {
                return m_index;
            }

        private:

            Windows::Foundation::Collections::CollectionChange const m_change{};
            uint32_t const m_index{};
        };

        struct iterator : implements<iterator, IIterator<T>>
        {
            explicit iterator(single_threaded_observable_vector<T>* owner) noexcept :
            m_version(owner->m_version),
                m_current(owner->m_values.begin()),
                m_end(owner->m_values.end())
            {
                m_owner.copy_from(owner);
            }

            void abi_enter() const
            {
                if (m_version != m_owner->m_version)
                {
                    throw hresult_changed_state();
                }
            }

            T Current() const
            {
                if (m_current == m_end)
                {
                    throw hresult_out_of_bounds();
                }

                return*m_current;
            }

            bool HasCurrent() const noexcept
            {
                return m_current != m_end;
            }

            bool MoveNext() noexcept
            {
                if (m_current != m_end)
                {
                    ++m_current;
                }

                return HasCurrent();
            }

            uint32_t GetMany(array_view<T> values)
            {
                uint32_t actual = static_cast<uint32_t>(std::distance(m_current, m_end));

                if (actual > values.size())
                {
                    actual = values.size();
                }

                std::copy_n(m_current, actual, values.begin());
                std::advance(m_current, actual);
                return actual;
            }

        private:

            com_ptr<single_threaded_observable_vector<T>> m_owner;
            uint32_t const m_version;
            typename std::vector<T>::const_iterator m_current;
            typename std::vector<T>::const_iterator const m_end;
        };
    };
}
```

**Append** 函式示範如何引發 [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) 事件。

```cppwinrt
m_changed(*this, make<args>(CollectionChange::ItemInserted, Size() - 1));
```

事件引數同時指出插入的元素，以及它的索引是什麼 (此案例中，最後一個元素)。 這些引數啟用 XAML 項目控制項，回應事件以及以最佳方式重新整理自己。

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>將 **BookSkus** 集合新增至 **BookstoreViewModel**
在 [XAML 控制項；繫結至 C++/WinRT 屬性](binding-property.md) 中，我們已將類型 **BookSku** 的屬性新增至我們主要的檢視模型。 在此步驟，我們會使用 **single_threaded_observable_vector&lt;T&gt;**，協助我們在相同的檢視模型上實作 **BookSku** 的可觀察集合。

在 `BookstoreViewModel.idl` 中宣告一個新的屬性。

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IVector<IInspectable> BookSkus{ get; };
}
...
```

> [!IMPORTANT]
> 在上述的 MIDL 3.0 清單，請注意， **BookSkus**屬性的類型[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821)。 本主題的下一節中，我們將會繫結[**ListBox**](/uwp/api/windows.ui.xaml.controls.listbox)項目的來源到**BookSkus**。 清單方塊的項目控制項，且正確設定[**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)屬性，您需要將其設為類型**IVector**或的**IInspectable**，例如[**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)的互通性類型的值。

儲存並建置。 從 `Generated Files` 資料夾中的 `BookstoreViewModel.h` 與 `BookstoreViewModel.cpp` 複製存取子虛設常式，並加以執行。

```cppwinrt
// BookstoreViewModel.h
...
#include "single_threaded_observable_vector.h"
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::make<single_threaded_observable_vector<Windows::Foundation::IInspectable>>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="if-you-have-a-windows-10-sdk-preview-build"></a>如果您有 Windows 10 SDK 預覽版組建
如果您已安裝[Windows 10 SDK 預覽版 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)，或更新版本，取代這行程式碼

```cppwinrt
m_bookSkus = winrt::make<single_threaded_observable_vector<Windows::Foundation::IInspectable>>();
```

考慮到這。

```cppwinrt
m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
```

而不是呼叫[**winrt:: make**](https://docs.microsoft.com/en-us/uwp/cpp-ref-for-winrt/make)，您可以建立適當的集合物件呼叫**winrt < 所 >** factory 函式。

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
    MainViewModel().BookSkus().Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
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
