---
description: C++/WinRT 提供函式和基底類別，讓您想要實作及/或傳遞集合時省下許多的時間和精力。
title: 使用 C++/WinRT 的集合
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 投影、 集合
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a50ab5f70faa0c8f8b73eada38444bcafd444d8b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635433"
---
# <a name="collections-with-cwinrt"></a>使用 C++/WinRT 的集合

就內部而言，Windows 執行階段集合中有很多複雜的移動組件。 當您想要將集合物件傳遞至 Windows 執行階段函式，或實作您自己的集合屬性和集合型別，有一些函數和中的基底類別，但是[C + + /cli WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)以支援您。 這些功能需要您手裡，超出的複雜度，並省下您額外負荷的時間和精力。

[**IVector** ](/uwp/api/windows.foundation.collections.ivector_t_)是藉由將項目的任何隨機存取集合的 Windows 執行階段介面。 如果您要實作**IVector**自己，您也必須實作[ **IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_)， [ **IVectorView** ](/uwp/api/windows.foundation.collections.ivectorview_t_)，並[ **Iiterator<t**](/uwp/api/windows.foundation.collections.iiterator_t_)。 即使您*需要*很多工作的自訂集合類型。 但是，如果您將資料放**std:: vector** (或**std:: map**，或有**std:: unordered_map**) 和所有您想要將它傳遞給 Windows 執行階段 API，則您會想要避免的動作這種程度的工作，可能的話。 和避免使用它*是*可行的因為 C + + /cli WinRT 可協助您有效率地和輕輕鬆鬆建立集合。

另請參閱[控制項的 XAML 項目，繫結至 C + + /cli WinRT 集合](binding-collection.md)。

> [!NOTE]
> 如果您尚未安裝 Windows SDK 版本 10.0.17763.0 (Windows 10 版本 1809年)，或更新版本中，就不需要權限的函式和本主題中所述的基底類別。 相反地，請參閱 <<c0> [ 如果您有較舊版本的 Windows SDK](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk)如需您可以改為使用可觀察的向量範本的清單。

## <a name="helper-functions-for-collections"></a>集合的 helper 函式

### <a name="general-purpose-collection-empty"></a>一般用途的集合空白

本節涵蓋的案例，您要建立一個集合，最初是空的;然後將其填入*之後*建立。

若要擷取新的物件型別的實作一般用途的集合，您可以呼叫[ **winrt::single_threaded_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-vector)函式樣板。 物件會當成[ **IVector**](/uwp/api/windows.foundation.collections.ivector_t_)，是藉由呼叫傳回的物件函式和屬性的介面。

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
int main()
{
    winrt::init_apartment();

    Windows::Foundation::Collections::IVector<int> coll{ winrt::single_threaded_vector<int>() };
    coll.Append(1);
    coll.Append(2);
    coll.Append(3);

    for (auto const& el : coll)
    {
        std::cout << el << std::endl;
    }

    Windows::Foundation::Collections::IVectorView<int> view{ coll.GetView() };
}
```

如您在上述程式碼範例所見，在建立集合之後您可以新增項目、 重複處理它們，和通常視為物件，如同任何您可能會收到來自 API 的 Windows 執行階段集合物件。 如果您需要的不可變的檢視集合，則您可以呼叫[ **IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview)，如所示。 如上所示的模式&mdash;建立及使用集合&mdash;適用於您想要用來傳入資料，或取得的資料，API 的簡單案例。 您可以傳遞**IVector**，或有**IVectorView**隨時隨地[ **IIterable** ](/uwp/api/windows.foundation.collections.iiterable_t_)預期。

在上面呼叫程式碼範例**winrt::init_apartment**初始化 COM; 依預設，在多執行緒的 apartment。

### <a name="general-purpose-collection-primed-from-data"></a>一般用途的集合，初始化資料

本章節涵蓋的案例，您要建立集合，並填入一次。

您可以避免呼叫的額外負荷**Append**在先前的程式碼範例。 您可能已經有資料來源，或您可能會想要填入來源資料之前建立的 Windows 執行階段集合物件。 以下是如何執行該動作。

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

您可以將暫存的物件，其中包含您的資料，傳遞**winrt::single_threaded_vector**，如同`coll1`上面。 或您可以移動**std:: vector** （假設您將不會存取它一次） 執行函式。 在這兩種情況下，您只要傳遞*rvalue*執行函式。 可讓編譯器能夠有效率，並避免將資料複製。 如果您想要深入了解*rvalues*，請參閱[值分類及其參考](cpp-value-categories.md)。

如果您想要的 XAML 項目控制項繫結至您的集合，接著您可以。 但請注意，若要正確設定[ **ItemsControl.ItemsSource** ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)屬性，您必須將它設定為型別的值**IVector**的**IInspectable**(或的互通性類型，例如[ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector))。 以下是程式碼範例會產生適用於繫結型別的集合，並附加到其中的項目。

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

您可以從資料中，建立 Windows 執行階段集合，並準備將傳遞至 API，完全不複製任何項目在其上的檢視。

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

在上述範例中，集合我們會建立*可以*繫結至 XAML 項目控制項，但集合不是可預見值。

### <a name="observable-collection"></a>可觀察的集合

若要擷取新的物件，實作型別的*可預見值*集合中，呼叫[ **winrt::single_threaded_observable_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)任何函式樣板項目類型。 但若要讓可觀察的集合適合繫結至 XAML 項目控制項時，使用**IInspectable**做為項目類型。

物件會當成[ **IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)，是藉由您 （或它所繫結的控制項） 呼叫傳回的物件函式和屬性的介面。

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

如需詳細資訊和程式碼範例，有關繫結您的使用者介面 (UI) 控制項可觀察的集合，請參閱 <<c0> [ 控制項的 XAML 項目，繫結至 C + + /cli WinRT 集合](binding-collection.md)。

### <a name="associative-collection-map"></a>關聯的集合 (map)

有關聯的集合版本，我們已經討論過的兩個函式。

- [ **Winrt::single_threaded_map** ](/uwp/cpp-ref-for-winrt/single-threaded-map)函式範本會傳回非可預見值關聯的集合，做為[ **IMap**](/uwp/api/windows.foundation.collections.imap_k_v_)。
- [ **Winrt::single_threaded_observable_map** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)函式範本會傳回關聯的可觀察集合，做為[ **IObservableMap** ](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

您可以選擇性地質數傳遞至函式的這些集合的資料*rvalue*型別的**std:: map**或是**std:: unordered_map**。

```cppwinrt
auto coll1{
    winrt::single_threaded_map<winrt::hstring, int>(std::map<winrt::hstring, int>{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    })
};

std::map<winrt::hstring, int> values{
    { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
};
auto coll2{ winrt::single_threaded_map<winrt::hstring, int>(std::move(values)) };
```

### <a name="single-threaded"></a>單一執行緒

「 單一執行緒 」 的這些函式名稱中指出它們不提供任何並行&mdash;也就是說，他們不具備執行緒安全。 值得一提的執行緒無關 apartment，因為從這些函式傳回的物件是所有敏捷式軟體開發 (請參閱[敏捷式軟體開發的物件，在 C + + /cli WinRT](agile-objects.md))。 它只是，物件都是單一執行緒。 而這是很適當，如果您只想要在應用程式二進位介面 (ABI) 之間傳遞資料之一或其他執行個體。

## <a name="base-classes-for-collections"></a>集合的基底類別

如果完整的彈性，您會想要實作您自己自訂的集合，您會想要避免這樣的教訓。 比方說，這是自訂的向量檢視看起來*而不需要協助的 C + + /cli WinRT 的基底類別*。

```cppwinrt
...
using namespace winrt;
using namespace Windows::Foundation::Collections;
...
struct MyVectorView :
    implements<MyVectorView, IVectorView<float>, IIterable<float>>
{
    // IVectorView
    float GetAt(uint32_t const) { ... };
    uint32_t GetMany(uint32_t, winrt::array_view<float>) const { ... };
    bool IndexOf(float, uint32_t&) { ... };
    uint32_t Size() { ... };

    // IIterable
    IIterator<float> First() const { ... };
};
...
IVectorView<float> view{ winrt::make<MyVectorView>() };
```

相反地，很容易地衍生您的自訂向量檢視，從[ **winrt::vector_view_base** ](/uwp/cpp-ref-for-winrt/vector-view-base)結構的範本，並只實作**get_container**函式公開 （expose) 保留資料的容器。

```cppwinrt
struct MyVectorView2 :
    implements<MyVectorView2, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView2, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

所傳回的容器**get_container**必須提供**開始**並**end**介面**winrt::vector_view_base**預期。 在上述範例所示**std:: vector**提供的。 但是，您可以傳回任何可藉相同的合約，包括您自己的自訂容器的容器。

```cppwinrt
struct MyVectorView3 :
    implements<MyVectorView3, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView3, float>
{
    auto get_container() const noexcept
    {
        struct container
        {
            float const* const first;
            float const* const last;

            auto begin() const noexcept
            {
                return first;
            }

            auto end() const noexcept
            {
                return last;
            }
        };

        return container{ m_values.data(), m_values.data() + m_values.size() };
    }

private:
    std::array<float, 3> m_values{ 0.2f, 0.3f, 0.4f };
};
```

這些是基底類別的 C + + /cli WinRT 提供可協助您實作自訂的集合。

### <a name="winrtvectorviewbaseuwpcpp-ref-for-winrtvector-view-base"></a>[winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

請參閱上述的程式碼範例。

### <a name="winrtvectorbaseuwpcpp-ref-for-winrtvector-base"></a>[winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

```cppwinrt
struct MyVector :
    implements<MyVector, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::vector_base<MyVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtobservablevectorbaseuwpcpp-ref-for-winrtobservable-vector-base"></a>[winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

```cppwinrt
struct MyObservableVector :
    implements<MyObservableVector, IObservableVector<float>, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::observable_vector_base<MyObservableVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtmapviewbaseuwpcpp-ref-for-winrtmap-view-base"></a>[winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

```cppwinrt
struct MyMapView :
    implements<MyMapView, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_view_base<MyMapView, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtmapbaseuwpcpp-ref-for-winrtmap-base"></a>[winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

```cppwinrt
struct MyMap :
    implements<MyMap, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_base<MyMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtobservablemapbaseuwpcpp-ref-for-winrtobservable-map-base"></a>[winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

```cppwinrt
struct MyObservableMap :
    implements<MyObservableMap, IObservableMap<winrt::hstring, int>, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::observable_map_base<MyObservableMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

## <a name="important-apis"></a>重要 API
* [ItemsControl.ItemsSource 屬性](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [IObservableVector 介面](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [IVector 介面](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::map_base 結構範本](/uwp/cpp-ref-for-winrt/map-base)
* [winrt::map_view_base 結構範本](/uwp/cpp-ref-for-winrt/map-view-base)
* [winrt::observable_map_base 結構範本](/uwp/cpp-ref-for-winrt/observable-map-base)
* [winrt::observable_vector_base 結構範本](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [winrt::single_threaded_observable_map 函式樣板](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [winrt::single_threaded_map 函式樣板](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [winrt::single_threaded_observable_vector 函式樣板](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt::single_threaded_vector 函式樣板](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [winrt::vector_base 結構範本](/uwp/cpp-ref-for-winrt/vector-base)
* [winrt::vector_view_base 結構範本](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>相關主題
* [值類別，以及其參考](cpp-value-categories.md)
* [XAML 項目控制項；繫結至一個 C++/WinRT 集合](binding-collection.md)
