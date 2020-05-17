---
description: C++/WinRT 提供函式和基底類別，讓您想要實作及/或傳遞集合時省下許多的時間和精力。
title: 使用 C++/WinRT 的集合
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, 集合
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4f1b15ec377b030a467dded634abe3fdde717896
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "68270140"
---
# <a name="collections-with-cwinrt"></a>使用 C++/WinRT 的集合

就內部而言，Windows 執行階段集合有許多複雜的移動組件。 但是，當想要將集合物件傳遞至 Windows 執行階段函式，或是要實作您自己的集合屬性和集合型別的時候，[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 有一些函數和基底類別可提供支援。 這些功能可以降低複雜度，省下對時間和精力的額外負荷。

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) 是藉由項目的任何隨機存取集合實作的 Windows 執行階段介面。 如果您要自行實作 **IVector**，也需要實作 [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_)、[**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_) 和 [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_)。 即使您需要  自訂集合類型，也有許多作業需要進行。 但是，如果您在 **std::vector** (或 **std::map**，或 **std::unordered_map**) 之中有資料，而且要傳遞到 Windows 執行階段 API，則建議盡可能避免進行這類作業。 而且避免這些作業確實  可行，因為 C++/WinRT 可協助您有效地輕鬆建立集合。

另請參閱 [XAML 項目控制項；繫結至 C++/WinRT 集合](binding-collection.md)。

## <a name="helper-functions-for-collections"></a>集合的輔助函式

### <a name="general-purpose-collection-empty"></a>一般用途的集合，空白

本節所介紹的案例中，是想要建立最初為空的集合，然後在建立「之後」  將物件填入該集合。

若是想要擷取新物件的類型，所實作的是一般用途的集合，則可呼叫 [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector) 函式範本。 物件會做為 [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) 傳回，這是您呼叫傳回的物件函式和屬性時所用的介面。

如果您想要將下列程式碼範例直接複製並貼到 **Windows 主控台應用程式 (C++/WinRT)** 專案的主要原始程式碼檔，請先在專案屬性中設定 [不使用預先編譯的標頭]  。

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;

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

如上述程式碼範例所示，在建立集合之後，您可以附加項目、重複處理這些項目，而且通常可將這些項目視為可能會從 API 收到的任何 Windows 執行階段集合物件。 如果需要集合的固定畫面，您可以呼叫所示的 [**IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview)。 如上所示，&mdash;建立和使用集合&mdash;的模式適用於想要將資料傳入 API 或從 API 取得資料的簡單案例。 如果預期 [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_)，則可傳遞 **IVector** 或 **IVectorView**。

在以上的程式碼範例中，對 **winrt::init_apartment** 的呼叫會初始化 Windows 執行階段中的執行緒；預設是在多執行緒 Apartment 中。 此呼叫也會初始化 COM。

### <a name="general-purpose-collection-primed-from-data"></a>一般用途集合，從資料準備

本節說明的案例中，要建立集合並同時將物件填入該集合。

您可以避免先前的程式碼範例中呼叫 **Append** 的作業。 您可能已經有來源資料，也可能想要先填入來源資料，再建立 Windows 執行階段集合物件。 方法如下所示。

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

您可以將包含資料的暫存物件傳遞到 **winrt::single_threaded_vector**，如上述的 `coll1`。 或者，您也可以將 **std::vector** (假設您不會再存取) 移動到函式中。 在這兩種情況下，都可以將 *rvalue* 傳遞到函式。 這可提升編譯器的效率，並避免複製資料。 如果您想要深入了解 *rvalue*，請參閱[值類別和它們的參考](cpp-value-categories.md)一文。

如果想要將 XAML 項目控制項繫結至您的集合，可以如此做。 但請注意，若要正確設定 [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 屬性，您需要將它設定 **IInspectable** 的類型 **IVector** (或互通性類型，例如 [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)) 值。

以下的程式碼範例會產生適合繫結的類型集合，並且將元素附加到其中。 在 [XAML 項目控制項，繫結至 C++/WinRT 集合](binding-collection.md)中，您可以找到關於此程式碼範例的脈絡。

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

您可以從資料建立 Windows 執行階段集合，並且在準備傳遞至 API 時檢視，完全不需要複製任何內容。

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
Windows::Foundation::Collections::IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

在上述範例中，我們建立的集合確實可以  繫結至 XAML 項目控制項，但是無法觀察集合。

### <a name="observable-collection"></a>可觀察的集合

若要對於實作*可觀察*集合的類型擷取新物件，可以呼叫含有任何元素類型的 [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 函式範本。 但是，若要讓可觀察的集合適合繫結至 XAML 項目控制項，使用 **IInspectable** 做為元素類型。

物件會做為 [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) 傳回，這是您 (或繫結的控制項) 呼叫傳回的物件函式和屬性時所用的介面。

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

如需將使用者介面 (UI) 控制項繫結至可觀察的集合有關的詳細資訊和程式碼範例，請參閱 [XAML 項目控制項，繫結至 C++/WinRT 集合](binding-collection.md)。

### <a name="associative-collection-map"></a>關聯集合 (對應)

我們討論的兩個函式有關聯集合版本。

- [**winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) 函式範本會傳回非可觀察關聯集合成為 [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_)。
- [**winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) 函式範本會傳回可觀察關聯集合成為 [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_)。

您可以選擇傳遞至類型 **std::map** 或 **std::unordered_map** 的 *rvalue*，使用資料準備這些集合。

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

這些函式的名稱中出現的「單一執行緒」指出函式不提供任何並行&mdash;，也就是說，並非執行緒安全。 提及的執行緒與 Apartment 無關，因為從這些函式傳回的物件都是敏捷式 (請參閱 [C++/WinRT 中的敏捷式物件](agile-objects.md))。 物件只是單一執行緒。 如果您只想要在應用程式二進位介面 (ABI) 之間單向或反向傳遞資料，這種物件相當適合。

## <a name="base-classes-for-collections"></a>集合的基底類別

為了達到完全的彈性，您想要實作您自己的自訂集合，然後想要避免以後再次進行整個繁複的實作過程。 例如，*在不使用 C++/WinRT 的基底類別*時，實作自訂向量檢視就是如此繁複。

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

其實，可以很容易就從 [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) 結構範本衍生自訂向量檢視，並且直接實作 **get_container** 函式公開保留資料的容器。

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

**get_container** 傳回的容器必須提供 **winrt::vector_view_base** 預期的 **begin** 和 **end** 介面。 如以上範例所示，**std::vector** 提供這種介面。 但是，您可以傳回達到相同效果的任何容器，包括您自己的自訂容器。

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

這些是 C++/WinRT 提供的基底類別，可供您實作自訂集合。

### <a name="winrtvector_view_base"></a>[winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

請參閱以上的程式碼範例。

### <a name="winrtvector_base"></a>[winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

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

### <a name="winrtobservable_vector_base"></a>[winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

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

### <a name="winrtmap_view_base"></a>[winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

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

### <a name="winrtmap_base"></a>[winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

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

### <a name="winrtobservable_map_base"></a>[winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

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
* [winrt::single_threaded_observable_map 函式範本](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [winrt::single_threaded_map 函式範本](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [winrt::single_threaded_observable_vector 函式範本](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt::single_threaded_vector 函式範本](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [winrt::vector_base 結構範本](/uwp/cpp-ref-for-winrt/vector-base)
* [winrt::vector_view_base 結構範本](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>相關主題
* [值類別，以及其參考](cpp-value-categories.md)
* [XAML 項目控制項；繫結至一個 C++/WinRT 集合](binding-collection.md)
