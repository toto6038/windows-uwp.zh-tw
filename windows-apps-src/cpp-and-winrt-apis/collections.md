---
author: stevewhims
description: C + + WinRT 提供的功能和儲存許多的時間和工作當您想要實作及 （或） 傳遞集合的基底類別。
title: 集合 C + + WinRT
ms.author: stwhi
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp、 標準、 c + +、 cpp、 winrt、 預測集合
ms.localizationpriority: medium
ms.openlocfilehash: 54f949c41af885ec379eaa9e5b12764710532b50
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2018
ms.locfileid: "2867939"
---
# <a name="collections-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>使用集合[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

> [!NOTE]
> **正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。**

內部 Windows Runtime 集合有許多的複雜移動組件。 但在當您想要傳遞至 Windows Runtime 函數的集合物件或實作您自己的集合的屬性及集合類型，有函數和基底類別的 C + + WinRT 以支援您。 這些功能需要不在您送複雜性和儲存時間和工作中的許多的額外負荷。

> [!IMPORTANT]
> 本主題所述的功能都已安裝[Windows 10 SDK 預覽建置的 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)，才可使用或更新版本。

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)是由元素的任何隨機存取集合中實作 Windows Runtime 介面。 如果您將自行實作**IVector** ，也會需要實作[**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_)、 [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_)及[**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_)。 即使您*需要*自訂集合類型，這是許多工作。 但如果您有**std::vector** （或**std::map**或**std::unordered_map**） 中的資料與您想要做的只有將會傳遞到 Windows Runtime API，再將想要避免盡可能執行的工作，該層級。 避免*為*可能的因為與 C + + WinRT 可協助您建立有效率且以一點的集合。

## <a name="helper-functions-for-collections"></a>Helper 函數的集合

### <a name="general-purpose-collection-empty"></a>一般用途集合空白

若要擷取新的物件類型的實作的一般用途集合，您可以呼叫**winrt::single_threaded_vector**函數範本。 為[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)中，傳回的物件而透過其呼叫傳回的物件的功能和屬性的介面。

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

您可以看到上述程式碼範例中，建立集合後您可以附加元素、 逐一，和通常將物件一樣您可能會收到來自 API 任何 Windows Runtime 集合物件。 如果您需要在檢視 over 集合，您可以呼叫[IVector::GetView](/uwp/api/windows.foundation.collections.ivector-1.getview)，如下所示。 上面所示的圖樣&mdash;建立和取用集合的&mdash;適合您要傳遞至、 資料或取得資料移出，API 的簡單案例。

### <a name="general-purpose-collection-primed-from-data"></a>一般用途集合中，從資料 ！

您也可以避免為**Append** ，您可以看見上述的程式碼範例中呼叫的額外負荷。 您可能已經具備資料來源，或可能會想要填入之前建立的 Windows Runtime 集合物件。 以下是如何執行的動作。

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

您可以傳遞含有資料的**winrt::single_threaded_vector**、 暫時物件在`coll1`、 上面。 您可以移動 （假設您將不會存取其再次） **std::vector**或給函數。 在這兩種情況下，您正在將將*右值*傳遞給函數。 這可讓編譯器有效率並避免複製資料。 如果您想要更了解*右*，請參閱[值類別和它們的參照](cpp-value-categories.md)。

如果您想要將 XAML 的項目控制項繫結至您的集合，然後您可以。 不過請注意正確地設定[**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)屬性，必須將它設為類型**IVector** **IInspectable** （或[**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)等的互通性類型） 的值。 以下是會產生一群類型適用於繫結，並將項目附加至其的程式碼範例。

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

高於*可以*集合繫結至 XAML 的項目控制項;但集合不機制。

### <a name="observable-collection"></a>觀察到的集合

若要擷取新的物件類型的實作*機制*集合，請呼叫**winrt::single_threaded_observable_vector**函數範本與任何項目類型。 但是若要讓適用於 XAML 的項目控制項繫結觀察到的集合，請使用**IInspectable**做為項目類型。

為[**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)中，傳回的物件而透過其您 （或控制項所繫結） 呼叫傳回的物件的功能和屬性的介面。

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

如需詳細資訊和程式碼範例、 繫結您的使用者介面 (UI) 控制項的觀察到的集合，請參閱[XAML 項目] 控制項，將繫結至 C + + WinRT 集合](binding-collection.md)。

### <a name="associative-container-map"></a>關聯的容器 (map)

有關聯的容器版本的兩個我們已經討論過的功能。

- **Single_threaded_map**函數範本為[**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_)傳回關聯的容器。 無法顯著對應。
- **Single_threaded_observable_map**函數範本會以[**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_)傳回觀察到的關聯容器。

您選擇性地可以傳遞給函數的類型**std::map**或**std::unordered_map**將*右值*質數這些容器的資料。

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

「 單一執行緒"中的這些函數名稱表示它們不提供任何並行&mdash;換句話說，它們不是安全執行緒。 其中的執行緒是無關公寓，因為這些函數所傳回的物件包括所有靈活 (請參閱[靈活物件在 C + + WinRT](agile-objects.md))。 它只是物件是單一執行緒。 也就是如果您只想要傳遞資料的其中一個方法或其他任何不同的應用程式二進位介面 (ABI) 完全適當。

## <a name="base-classes-for-collections"></a>基底類別的集合

如果完整的彈性，針對您要實作您自己的自訂集合，您將想要避免執行之動作的硬碟的方式。 例如，這是自訂向量檢視的起來*不需要協助的 C + + WinRT 的基底類別*。

```cppwinrt
...
using namespace Windows::Foundation::Collections;

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

而是很容易衍生自**winrt::vector_view_base**結構範本的自訂向量檢視並只實作**get_container**函數公開含有您資料的容器。

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

**Get_container**所傳回的容器必須提供的**開始**和**結束**介面該**winrt::vector_view_base**預期會出現。 上述範例所示， **std::vector**提供的。 但是您可以傳回 fulfils 相同的合約，包括您自己的自訂容器任何容器。

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

這些是在基底類別的 C + + WinRT 提供可協助您實作自訂集合。

- **winrt::vector_view_base**
- **winrt::vector_base**
- **winrt::observable_vector_base**
- **winrt::map_view_base**
- **winrt::map_base**
- **winrt::observable_map_base**

## <a name="important-apis"></a>重要 API
* [ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

## <a name="related-topics"></a>相關主題
* [值類別及給他們參考](cpp-value-categories.md)
* [XAML 項目控制項；繫結至一個 C++/WinRT 集合](binding-collection.md)
