---
description: C++/WinRT 藉由在一般情況提供自動轉換，簡化將參數傳入 ABI 界線的過程。
title: 將參數傳入到 ABI 界限
ms.date: 07/10/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 傳遞, 參數, ABI
ms.localizationpriority: medium
ms.openlocfilehash: 51cde2332d3d9df9d1f488aa7f8246f9e1e2ed36
ms.sourcegitcommit: e1104689fc1db5afb85701205c2580663522ee6d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997975"
---
# <a name="passing-parameters-into-the-abi-boundary"></a>將參數傳入到 ABI 界限

透過 **winrt::param** 命名空間中的類型，C++/WinRT 可藉由在一般情況提供自動轉換，簡化將參數傳入 ABI 界線的過程。 您可以在[字串處理](/windows/uwp/cpp-and-winrt-apis/strings)和[標準 C++ 資料類型和 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types) 中看到更多詳細資料和程式碼範例。

> [!IMPORTANT]
> 您不會自己使用 **winrt::param** 命名空間中的類型。 這些類型是為了用於投影。

許多類型都有同步和非同步版本。 C++/WinRT 會在您將參數傳遞給同步方法時使用同步版本；在您將參數傳遞給非同步方法時使用非同步版本。 非同步版本會採用額外的步驟，使得呼叫者難以在作業完成之前改變集合。 不過，要注意的是，並沒有任何變體可防止從另一個執行緒改變集合。 避免此狀況是您的責任。

## <a name="string-parameters"></a>字串參數

**winrt::param::hstring** 可讓參數更容易地傳遞至採用 **HSTRING** 的 API。

|您可以傳遞的類型|附註|
|-|-|
|`{}`|傳遞空白字串。|
|**winrt::hstring**||
|**std::wstring_view**|針對常值，您可以撰寫 `L"Name"sv`。 檢視必須在結束之後有 null 結束字元。|
|**std::wstring**|-|
|**wchar_t const\***|null 終止的字串。|

不允許使用 `nullptr`。 請改為使用 `{}`。

編譯器會知道如何在編譯時評估字串常值上的 `wcslen`。 因此，常值的 `L"Name"sv` 和 `L"Name"` 會相等。

請注意，**std::wstring_view** 物件都不是以 null 終止，但 C++/WinRT 需要字串結束後的字元是 null。 如果您傳遞非 null 終止的 **std::wstring_view**，則會終止處理程序。

## <a name="iterable-parameters"></a>可反覆 (Iterable) 的參數

**winrt::param::iterable\<T\>** 和 **winrt::param::async_iterable\<T\>** 可讓參數更容易地傳遞至採用 **IIterable\<T\>** 的 API。

Windows 執行階段集合已是 **IIterable** 狀態。

|您可以傳遞的類型|同步|Async|附註|
|-|-|-|-|
| `nullptr` | 是 | 是 | 您必須確認基礎方法可支援 `nullptr`。|
| **IIterable\<T\>** | 是 | 是 | 或可以轉換成該類型的任何項目。|
| **std::vector\<T\> const&** | 是 | 否 ||
| **std::vector\<T\>&&** | 是 | 是 | 內容會移到迭代器中，以避免發生變更。|
| **std::initializer_list\<T\>** | 是 | 是 | 非同步版本會複製這些項目。|
| **std::initializer_list\<U\>** | 是 | 否 | **U** 必須可轉換成 **T**。|
| `{ ForwardIt begin, ForwardIt end }` | 是 | 否 | `*begin` 必須可轉換成 **T**。|

請注意，**IIterable\<U\>** 和 **std::vector\<U\>** 是不允許的項目，即使 **U** 可轉換成 **T** 也一樣。針對 **std::vector\<U\>** ，您可以使用雙重迭代器版本 (詳細資料如下)。

在某些情況下，您擁有的物件可能已實作您想要的 **IIterable**。 例如，[**FileOpenPicker.PickMultipleFilesAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync) 產生的 **IVectorView\<StorageFile\>** 會實作 **IIterable\<StorageFile\>** 。 但也會實作 **IIterable\<IStorageItem\>** ；您只需要明確要求即可。

```cppwinrt
IVectorView<StorageFile> pickedFiles{ co_await filePicker.PickMultipleFilesAsync() };
requestData.SetStorageItems(storageItems.as<IIterable<IStorageItem>>());
```

在其他情況下，您可以使用雙重迭代器版本。

```cppwinrt
std::vector<StorageFile> storageFiles;
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() });
```

雙重迭代器可更廣泛地用於不符合上述任何案例的集合，只要您可以對其反覆查看，並產生可以轉換成 **T** 的事項即可。我們先前已將其用於反覆查看衍生類型的向量。 在這裡，我們用其來反覆查看衍生類型的非向量部分。

```cppwinrt
std::array<StorageFile, 3> storageFiles;
requestData.SetStorageItems(storageFiles); // This doesn't work.
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() }); // But this works.
```

如果迭代器是 `RandomAcessIt`，則 [**IIterator\<T\>.GetMany(T\[\])** ](/uwp/api/windows.foundation.collections.iiterator-1.getmany) 的實作會更有效率。 否則，其會讓數個傳遞超出範圍。

|您可以傳遞的類型|同步|Async|附註|
|-|-|-|-|
| `nullptr` | 是 | 是 | 您必須確認基礎方法可支援 `nullptr`。|
| **IIterable\<IKeyValuePair\<K, V\>\>** | 是 | 是 | 或可以轉換成該類型的任何項目。|
| **std::map\<K, V\> const&** | 是 | 否 ||
| **std::map\<K, V\>&&** | 是 | 是 | 內容會移到迭代器中，以避免發生變更。|
| **std::unordered_map\<K, V\> const&** | 是 | 否 ||
| **std::unordered_map\<K, V\>&&** | 是 | 是 | 內容會移到迭代器中，以避免發生變更。|
| **std::initializer_list\<std::pair\<K, V\>\>** | 是 | 是 | **K** 和 **V** 類型必須完全相符。 索引鍵不得重複。 非同步版本會複製這些項目。|
| `{ ForwardIt begin, ForwardIt end }` | 是 | 否 | `begin->first` 和 `begin->second` 必須可分別轉換成 **K** 和 **V**。|

## <a name="vector-view-parameters"></a>向量檢視參數

**winrt::param::vector_view\<T\>** 和 **winrt::param::async_vector_view\<T\>** 可讓參數更容易地傳遞至採用 **IVectorView\<T\>** 的 API。

您可以使用 [**IVector\<T\>.GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview) 從 **IVector** 取得 **IVectorView**。

|您可以傳遞的類型|同步|Async|附註|
|-|-|-|-|
| `nullptr` | 是 | 是 | 您必須確認基礎方法可支援 `nullptr`。|
| **IVectorView\<T\>** | 是 | 是 | 或可以轉換成該類型的任何項目。|
| **std::vector\<T\>const&** | 是 | 否 ||
| **std::vector\<T\>&&** | 是 | 是 | 內容會移到檢視中，以避免發生變更。|
| **std::initializer_list\<T\>** | 是 | 是 | 類型必須完全相符。 非同步版本會複製這些項目。|
| `{ ForwardIt begin, ForwardIt end }` | 是 | 否 | `*begin` 必須可轉換成 **T**。|

雙重迭代器版本可用來另外建立不符合直接傳遞需求的向量檢視。 不過，請注意，由於向量支援隨機存取，因此我們建議您傳遞 `RandomAcessIter`。

## <a name="map-view-parameters"></a>對應檢視參數

**winrt::param::map_view\<T\>** 和 **winrt::param::async_map_view\<T\>** 可讓參數更容易地傳遞至採用 **IMapView\<T\>** 的 API。

您可以使用 **IMap::GetView** 從 **IMap** 取得 **IMapView**。

|您可以傳遞的類型|同步|Async|附註|
|-|-|-|-|
| `nullptr` | 是 | 是 | 您必須確認基礎方法可支援 `nullptr`。|
| **IMapView\<K, V\>** | 是 | 是 | 或可以轉換成該類型的任何項目。|
| **std::map\<K, V\> const&** | 是 | 否 ||
| **std::map\<K, V\>&&** | 是 | 是 | 內容會移到檢視中，以避免發生變更。|
| **std::unordered_map\<K, V\> const&**  | 是 | 否 ||
| **std::unordered_map\<K, V\>&&** | 是 | 是 | 內容會移到檢視中，以避免發生變更。|
| **std::initializer_list\<std::pair\<K, V\>\>** | 是 | 是 | 同步和非同步版本皆會複製這些項目。 您不得複製索引鍵。|

## <a name="vector-parameters"></a>向量參數

**winrt::param::vector\<T\>** 可讓參數更容易地傳遞至採用 **IVector\<T\>** 的 API。

|您可以傳遞的類型|附註|
|-|-|
| `nullptr` | 您必須確認基礎方法可支援 `nullptr`。|
| **IVector\<T\>** | 或可以轉換成該類型的任何項目。|
| **std::vector\<T\>&&** | 內容會移到參數中，以避免發生變更。 結果不會移回。|
| **std::initializer_list\<T\>** | 內容會複製到參數中，以避免發生變更。|

如果方法會改變向量，則觀察變化的唯一方式是直接傳遞 **IVector**。 如果您傳遞 **std::vector**，則方法會改變複製項目，而不是改變原始項目。

## <a name="map-parameters"></a>對應無參數

**winrt::param::map\<T\>** 可讓參數更容易地傳遞至採用 **IMap\<T\>** 的 API。

|您可以傳遞的類型|附註|
|-|-|
| `nullptr` | 您必須確認基礎方法可支援 `nullptr`。|
| **IMap\<T\>** | 或可以轉換成該類型的任何項目。|
| **std::map\<K, V\>&&** | 內容會移到參數中，以避免發生變更。 結果不會移回。|
| **std::unordered_map\<K, V\>&&** | 內容會移到參數中，以避免發生變更。 結果不會移回。|
| **std::initializer_list\<std::pair\<K, V\>\>** | 內容會複製到參數中，以避免發生變更。|

如果方法會改變對應，則觀察變化的唯一方式是直接傳遞 **IMap**。 如果您傳遞 **std::map** 或 **std::unordered_map**，則方法會改變複製項目，而不是改變原始項目。

## <a name="array-parameters"></a>陣列參數

**winrt::array_view\<T\>** 不在 **winrt::param** 命名空間中，但其會用於 C 樣式陣列&mdash;亦稱為*一致陣列*的參數。

|您可以傳遞的類型|附註|
|-|-|
| `{}` | 空的陣列。|
| **array** | C 的一致陣列 (亦即 `C array[N];`)，其中 **C** 可轉換成 **T**，而且 `sizeof(C) == sizeof(T)`。 |
| **std::array<C, N>** | **C** 的 C++ **std::array**，其中 **C** 可轉換成 **T**，而且 `sizeof(C) == sizeof(T)`。 |
| **std::vector<C>** | **C** 的 C++ **std::vector**，其中 **C** 可轉換成 **T**，而且 `sizeof(C) == sizeof(T)`。 |
| `{ T*, T* }` | 一組指標代表範圍 [開始、結束]。|
| **std::initializer_list\<T\>** ||

另請參閱部落格文章：[將 C 樣式陣列傳遞到 Windows 執行階段 ABI 界限的各種模式](https://devblogs.microsoft.com/oldnewthing/20200205-00/?p=103398)。