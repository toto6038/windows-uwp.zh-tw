---
author: stevewhims
description: C + + /winrt 提供函式和您節省很多時間和精力當您想要實作和/或傳遞集合的基底類別。
title: 集合使用 C + + /winrt
ms.author: stwhi
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 投影、 集合
ms.localizationpriority: medium
ms.openlocfilehash: 5495649a6b7fad633e24e244aa3f6efbcc05e441
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2018
ms.locfileid: "3238790"
---
# <a name="collections-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>使用集合[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

> [!NOTE]
> **正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。**

在內部，Windows 執行階段集合有許多的複雜的移動部分。 但當您想要將集合的物件傳遞到 Windows 執行階段函式中，或實作您自己的集合屬性和集合類型，有函式和基底類別，在 C + + /winrt 支援您。 這些功能需要從您的手各自的複雜性，並儲存您的額外負荷許多中的時間和精力。

> [!IMPORTANT]
> 本主題中所述的功能都適用於您已安裝[Windows 10 SDK 預覽版 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)，或更新版本。

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)是任何隨機存取的集合項目所實作的 Windows 執行階段介面。 如果您是要自行實作**IVector** ，您也必須實作[**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_)、 [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_)，以及[**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_)。 即使您*需要*自訂的集合類型，這會是工作的大量。 但如果您有**std:: vector** （或**std:: map**或**std::unordered_map**） 中的資料，而且您想要執行的是將它傳送至 Windows 執行階段 API，然後您會想要避免執行該層級的工作，如果可能的話。 並避免*是*越好，因為 C + + /winrt 可協助您有效地和輕鬆地建立集合。

## <a name="helper-functions-for-collections"></a>針對集合的協助程式函式

### <a name="general-purpose-collection-empty"></a>一般用途的集合，空的

若要擷取實作一般用途的集合的類型的新物件，您可以呼叫[**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector)函式範本。 物件會傳回為[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)，，而這就是您可透過呼叫傳回的物件的函式和屬性的介面。

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
int main()
{
    init_apartment();

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

如您所見上述的程式碼範例中，建立集合之後您可以新增項目、 逐一查看，和通常將該物件，就像任何 Windows 執行階段集合的物件，您可能會收到 API。 如果您需要的不可變的檢視集合時，您可以呼叫[**IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview)，如所示。 上述的模式&mdash;的建立和取用集合&mdash;適用於簡單案例中，您將資料傳遞至，或取得資料，API。

### <a name="general-purpose-collection-primed-from-data"></a>一般用途的集合，從資料 ！

您也可以避免呼叫您所見上述的程式碼範例中，**新增**的額外負荷。 您可能已經有資料來源，或您可能會想要填入它之前建立 Windows 執行階段集合物件。 以下是執行此作業的方式。

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

您可以將暫存物件包含您的資料**winrt::single_threaded_vector**，如同`coll1`上方。 或您可以將 （假設您將不會存取它一次） **std:: vector**移到函式。 在這兩個情況下，您傳送*右值*到此函式。 這可讓編譯器會失效，並以避免複製資料。 如果您想要深入了解*右*，請參閱[值類別，以及它們的參考](cpp-value-categories.md)。

如果您想要將 XAML 項目控制項繫結至您的集合，然後您可以。 但請注意，若要正確設定[**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)屬性，您需要將其設為的值類型**IVector** **IInspectable** （或的互通性類型，例如[**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)）。 以下是類型的會產生一組適用於繫結，並將項目附加至它的程式碼範例。

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

集合，「*可以*繫結至 XAML 項目控制項;但是集合不是可觀察。

### <a name="observable-collection"></a>可觀察的集合

若要擷取實作*可觀察*集合的類型的新物件，請呼叫[**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)函式範本與任何項目類型。 但若要讓適用於 XAML 項目控制項繫結可觀察的集合，使用**IInspectable**做為項目類型。

物件會傳回為[**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)，，而這就是透過其您 （或將它繫結的控制項） 呼叫傳回的物件的函式和屬性的介面。

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

如需詳細資訊和程式碼範例，關於繫結您的使用者介面 (UI) 來設定控制項可觀察的集合，請參閱[XAML 項目控制項; 繫結至 C + + /winrt 集合](binding-collection.md)。

### <a name="associative-collection-map"></a>關聯的集合 （對應）

有兩個函式，我們已討論關聯的集合版本。

- [**Winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map)函式範本會傳回非變成可觀察關聯集合做為[**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_)。
- [**Winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)函式範本會傳回為[**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_)關聯可觀察集合。

您可以選擇性地質數這些資料集合，藉由*右值*的類型**std:: map**或**std::unordered_map**傳遞給函式。

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

它們不提供任何並行 「 單一執行緒 」 中的這些函式名稱表示&mdash;亦即，但猶嫌安全執行緒。 提及的執行緒無關 apartment，因為這些函式傳回的物件是所有敏捷式物件 (請參閱[敏捷式物件，在 C + + /winrt](agile-objects.md))。 它只是物件是單一執行緒。 而這就是完全適當，如果您只想要跨應用程式二進位介面 (ABI) 傳遞或其他資料的一種方式。

## <a name="base-classes-for-collections"></a>針對集合的基底類別

如果提供完整的彈性，您會想要實作您自己自訂的集合，您會想要避免執行該動作的固定的方式。 例如，這是自訂向量檢視看起來像*而不需要協助，C + /winrt 的基底類別*。

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

相反地，就能輕鬆從其中[**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base)結構範本，您的自訂向量檢視，並且只是實作**get_container**函式來公開容器保留您的資料。

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

容器傳回**get_container**必須提供的**開始**和**結束**介面該**winrt::vector_view_base**預期。 如上述範例中所示， **std:: vector**提供的。 但您可以將傳回任何容器，fulfils 相同的合約，包括您自己自訂的容器。

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

這些是基底類別的 C + + /winrt 提供可協助您實作自訂集合。

### [<a name="winrtvectorviewbase"></a>winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

請參閱上述的程式碼範例。

### [<a name="winrtvectorbase"></a>winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

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

### [<a name="winrtobservablevectorbase"></a>winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

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

### [<a name="winrtmapviewbase"></a>winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

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

### [<a name="winrtmapbase"></a>winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

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

### [<a name="winrtobservablemapbase"></a>winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

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
* [ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)
* [winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)
* [winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)
* [winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [winrt::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [winrt::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)
* [winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>相關主題
* [值類別，以及它們的參考](cpp-value-categories.md)
* [XAML 項目控制項；繫結至一個 C++/WinRT 集合](binding-collection.md)
