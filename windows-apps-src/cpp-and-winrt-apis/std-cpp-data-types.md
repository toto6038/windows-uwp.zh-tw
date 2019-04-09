---
description: 使用 C++/WinRT，您可以使用標準 C++ 資料類型呼叫 Windows 執行階段 API。
title: 標準 C++ 資料類型與 C++/WinRT
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、資料、類型
ms.localizationpriority: medium
ms.openlocfilehash: 44de7b61264f8e0e04d1de6d2b1101844656f28b
ms.sourcegitcommit: 99271798fe53d9768fc52b21366de05268cadcb0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58221454"
---
# <a name="standard-c-data-types-and-cwinrt"></a>標準 C++ 資料類型與 C++/WinRT

具有[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，您可以呼叫使用標準的 Windows 執行階段 ApiC++資料類型，包括一些C++標準程式庫的資料類型。 您可以將標準字串傳遞至 Api (請參閱[字串處理 C + /cli WinRT](strings.md))，而且您可以將初始設定式清單和標準容器 api 所預期的語意相等的集合。

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
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to a winrt::array_view before being passed to WriteBytes.
}
```

有兩個項目參與進行這項工作。 第一個，**DataWriter::WriteBytes** 方法採用類型 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 的參數。

```cppwinrt
void WriteBytes(winrt::array_view<uint8_t const> value) const
```

**winrt::array_view**是自訂C++/WinRT 表示型別安全地一系列連續的值 (定義於C++/WinRT 基礎程式庫，也就是`%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`)。

第二個， **winrt::array_view**有初始設定式清單建構函式。

```cppwinrt
template <typename T> winrt::array_view(std::initializer_list<T> value) noexcept
```

在許多情況下，您可以選擇是否要留意**winrt::array_view**程式設計中。 如果您選擇 *不* 注意它，如果以及當 C++ 標準程式庫中出現對等類型時，則不會變更任何程式碼。

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

此處執行兩個因素。 首先，被呼叫端的建構**std:: vector**從初始設定式清單 （此被呼叫端是非同步的因此它無法擁有該物件，它必須)。 第二個、C++/WinRT 無障礙地 (且不使用複製) 繫結 **std::vector** 做為 Windows 執行階段集合參數。

## <a name="standard-arrays-and-vectors"></a>標準陣列和向量
[**winrt::array_view** ](/uwp/cpp-ref-for-winrt/array-view)還有轉換建構函式，從**std:: vector**並**std:: array**。

```cppwinrt
template <typename C, size_type N> winrt::array_view(std::array<C, N>& value) noexcept
template <typename C> winrt::array_view(std::vector<C>& vectorValue) noexcept
```

因此，您可以改為使用 **std::vector** 呼叫 **DataWriter::WriteBytes**。

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to a winrt::array_view before being passed to WriteBytes.
```

或使用 **std::array**。

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to a winrt::array_view before being passed to WriteBytes.
```

C++/WinRT 繫結 **std::vector** 做為 Windows 執行階段集合參數。 因此，您可以傳遞 **std::vector&lt;winrt::hstring&gt;**，並將它轉換為適當的 **winrt::hstring** Windows 執行階段集合。 沒有要謹記在心，如果是非同步的被呼叫端的額外詳細資料。 由於這種情況下的實作詳細資料，您必須提供右值，因此您必須提供複製還是移動的向量。 在下列程式碼範例中，移動向量的擁有權之物件的非同步被呼叫端所接受的參數類型 (就請小心，不要存取`vecH`在移動之後，一次)。 如果您想要深入了解右值，請參閱[值的類別，及其參考](cpp-value-categories.md)。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

但您無法傳遞 **std::vector&lt;std::wstring&gt;** 其中預期一個 Windows 執行階段集合。 這是因為發生轉換至適當的 **std::wstring** Windows 執行階段集合，C++ 語言不會強制該集合的類型參數。 因此，下列程式碼範例不會編譯 (和解決方法是傳遞**std:: vector&lt;winrt::hstring&gt;** 相反的如上所示)。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>原始陣列和指標範圍
記住，對等的型別可能存在於在未來需要注意C++標準程式庫，您也可以使用直接**winrt::array_view**如果您選擇，或需要。

**winrt::array_view**轉換建構函式從未經處理的陣列，並從各種**T&ast;**  （項目類型的指標）。

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the winrt::array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the winrt::array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>winrt::array_view 函式和運算子
建構函式、 運算子、 函數和迭代器的主控件會實作**winrt::array_view**。 A **winrt::array_view**是範圍，因此您可以使用它與範圍架構`for`，或使用**std:: for_each**。

如需詳細範例和資訊，請參閱 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) API 參考主題。

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt;** 和標準的反覆項目建構
[**SyndicationFeed.Items** ](/uwp/api/windows.web.syndication.syndicationfeed.items)是一個傳回的型別集合的 Windows 執行階段 API 的範例[ **IVector&lt;T&gt;**  ](/uwp/api/windows.foundation.collections.ivector_t_) （預計於 C ++ 做為 WinRT **winrt::Windows::Foundation::Collections::IVector&lt;T&gt;**)。 您可以使用此類型與標準的反覆項目建構，例如範圍架構的`for`。

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

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>C++使用非同步 Windows 執行階段 Api 的協同程式
您可以繼續使用[平行模式程式庫 (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl)呼叫非同步 Windows 執行階段 Api 時。 不過，在許多情況下，C++協同程式提供有效率且更輕鬆地-自動程式化的慣用語與非同步的物件互動。 如需詳細資訊，以及程式碼範例，請參閱[並行和非同步作業C++/WinRT](concurrency.md)。

## <a name="important-apis"></a>重要 API
* [IVector&lt;T&gt;介面](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::array_view 結構範本](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>相關主題
* [字串處理 C + /cli WinRT](strings.md)
