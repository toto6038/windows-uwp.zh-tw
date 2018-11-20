---
author: stevewhims
description: 使用 C++/WinRT，您可以使用標準 C++ 資料類型呼叫 Windows 執行階段 API。
title: 標準 C++ 資料類型與 C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、資料、類型
ms.localizationpriority: medium
ms.openlocfilehash: 5aa6e17fcd95813b6abe05e9e42ad7c86657159f
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7302230"
---
# <a name="standard-c-data-types-and-cwinrt"></a>標準 C++ 資料類型與 C++/WinRT

與[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，您可以呼叫 Windows 執行階段 Api 使用標準 c + + 資料類型，包括部分 c + + 標準程式庫資料類型。 您可以將標準字串傳遞至 Api (請參閱[字串處理在 C + + WinRT](strings.md))，以及您可以傳遞初始設定式清單和標準的容器來預期語意對等的集合的 Api。

## <a name="standard-initializer-lists"></a>標準初始設定式清單
初始設定式清單 (**std::initializer_list**) 是 C++ 標準程式庫建構。 您呼叫某些 Windows 執行階段建構函式與方法時，可以使用初始設定式清單。 例如，您可以使用一個來呼叫 [**DataWriter::WriteBytes**](/uwp/api/windows.storage.streams.datawriter.writebytes)。

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt::Windows::Storage::Streams;

int main()
{
    winrt::init_apartment();

    InMemoryRandomAccessStream stream;
    DataWriter dataWriter{stream};
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to an array_view before being passed to WriteBytes.
}
```

有兩個項目參與進行這項工作。 第一個，**DataWriter::WriteBytes** 方法採用類型 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 的參數。

```cppwinrt
void WriteBytes(array_view<uint8_t const> value) const
```

 **array_view** 是自訂 C++/WinRT 類型，其安全地表示一連串的值 (在 C++/WinRT 基礎程式庫中定義它，也就是 `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`)。

第二個，**array_view** 有初始設定式清單建構函式。

```cppwinrt
template <typename T> array_view(std::initializer_list<T> value) noexcept
```

在許多情況下，您可以選擇是否要在您的程式設計中注意 **array_view**。 如果您選擇 *不* 注意它，如果以及當 C++ 標準程式庫中出現對等類型時，則不會變更任何程式碼。

您可以將一個初始設定式清單傳遞至需要一個集合參數的 Windows 執行階段 API。 以 **StorageItemContentProperties::RetrievePropertiesAsync** 為例。

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

您可以像這樣使用初始設定式清單呼叫該 API。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" }) };
}
```

此處執行兩個因素。 第一個，被呼叫者從初始設定式清單建構一個 **std::vector** (此被呼叫者必須為非同步，如此才能有該物件)。 第二個、C++/WinRT 無障礙地 (且不使用複製) 繫結 **std::vector** 做為 Windows 執行階段集合參數。

## <a name="standard-arrays-and-vectors"></a>標準陣列和向量
**array_view**也有 **std::vector** 和 **std::array** 的轉換建構函式。

```cppwinrt
template <typename C, size_type N> array_view(std::array<C, N>& value) noexcept
template <typename C> array_view(std::vector<C>& vectorValue) noexcept
```

因此，您可以改為使用 **std::vector** 呼叫 **DataWriter::WriteBytes**。

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to an array_view before being passed to WriteBytes.
```

或使用 **std::array**。

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to an array_view before being passed to WriteBytes.
```

C++/WinRT 繫結 **std::vector** 做為 Windows 執行階段集合參數。 因此，您可以傳遞 **std::vector&lt;winrt::hstring&gt;**，並將它轉換為適當的 **winrt::hstring** Windows 執行階段集合。 還有額外的細節，如果是非同步的被呼叫者，請謹記。 因為該案例的實作詳細資料，而您將需要提供右值，因此您必須提供一份複本或移動向量。 在下列程式碼範例，移動向量的所有權接受非同步被呼叫者的參數類型的物件 (接通小心，不要存取`vecH`再次移動它之後)。 如果您想要深入了解右，請參閱[值類別，以及它們的參考](cpp-value-categories.md)。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

但您無法傳遞 **std::vector&lt;std::wstring&gt;** 其中預期一個 Windows 執行階段集合。 這是因為發生轉換至適當的 **std::wstring** Windows 執行階段集合，C++ 語言不會強制該集合的類型參數。 因此，不編譯下列程式碼範例 (和解決方案是通過**std:: vector&lt;winrt:: hstring&gt;** 相反地，如以上所示)。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>原始陣列和指標範圍
請記住，在之後 C++ 標準程式庫中可能會存在一個對等項目，您也可以直接使用 **array_view**，如果您選擇如此，或者需要這麼做的話。

**array_view**已從原始陣列，以及從一系列的轉換建構函式**T&ast; ** （指向元素類型）。

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>winrt::array_view 函式和運算子
建構函式的主機、運算子、函式以及為 **array_view** 實作 Iterator。 **array_view** 是一個範圍，讓您可以有範圍基礎 `for`，或有 **std::for_each** 來使用它。

如需詳細範例和資訊，請參閱 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) API 參考主題。

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt;** 和標準反覆項目建構
[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items)是 Windows 執行階段 API 傳回類型的集合的範例[**IVector&lt;T&gt; **](/uwp/api/windows.foundation.collections.ivector_t_) (投影到 C + + /winrt 做為**winrt::Windows::Foundation::Collections::IVector&lt;T&gt; **). 您可以使用標準的反覆項目建構，這種類型例如範圍為基礎`for`。

```cppwinrt
// main.cpp
#include "pch.h"
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}
```

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>使用非同步 Windows 執行階段 Api 的 c + + 協同程式
您可以繼續呼叫非同步 Windows 執行階段 Api 時使用[平行模式程式庫 (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) 。 不過，在許多情況下，c + + 協同程式提供有效率且更輕鬆地編碼的慣用語與非同步物件互動。 如需詳細資訊和程式碼範例，請參閱[並行和非同步作業，使用 C + + /winrt](concurrency.md)。

## <a name="important-apis"></a>重要 API
* [IVector&lt;T&gt;介面](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::array_view 結構範本](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>相關主題
* [C++/WinRT 中的字串處理](strings.md)
