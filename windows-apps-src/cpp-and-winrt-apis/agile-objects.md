---
description: 敏捷式物件是可以從任何執行緒中存取的一個。 C++/WinRT 預設為敏捷式，但您可以選擇退出。
title: 使用 C++/WinRT 的敏捷式物件
ms.date: 10/20/2018
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、敏捷式、物件、敏捷性、IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 0b390161a4eb2c4f38fed9bce226c5a5e92c5ad8
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291776"
---
# <a name="agile-objects-in-cwinrt"></a>敏捷式軟體開發中的物件C++/WinRT

在大部分情況下，Windows 執行階段類別的執行個體可以從任何執行緒存取 (就像大部分的標準C++物件可以)。 Windows 執行階段類別是*敏捷式軟體開發*。 只有少數隨附於 Windows 的 Windows 執行階段類別的非 agile 的但當您使用它們時，您需要其執行緒模型和封送處理行為納入考量 （封送處理為傳遞資料跨 apartment 界限）。 它是不錯的預設值，每個 Windows 執行階段物件是敏捷式軟體開發，讓您自己[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)類型皆為敏捷式預設。

但是，您可以選擇退出。您可能會有充足的理由，需要存放，比方說，在指定的單一執行緒 apartment 中您型別的物件。 這通常與重新進入需求有關。 但即使使用者介面 (UI) API 提供越來越多敏捷式物件。 一般而言，敏捷性式最簡單且效能最佳的選項。 此外，當您實作啟動原廠，它必須是敏捷的，即使您對應執行階段類別不是。

> [!NOTE]
> Windows 執行階段是根據 COM。 在 COM 條款中，在 `ThreadingModel` = *Both* 中註冊一個敏捷式類別。 如需 COM 執行緒模型和 apartment 的詳細資訊，請參閱[了解與使用 COM 執行緒模型](https://msdn.microsoft.com/library/ms809971)。

## <a name="code-examples"></a>程式碼範例

讓我們使用執行階段類別的實作範例來說明如何C++/WinRT 支援靈活度。

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

因為我們尚未退出，因為此實作是敏捷的。 [  **Winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基礎結構實作 [**IAgileObject**](https://msdn.microsoft.com/library/windows/desktop/hh802476) 和 [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal)。 **IMarshal** 實作使用 **CoCreateFreeThreadedMarshaler** 執行適用於不知道 **IAgileObject** 的舊版程式碼的正確做法。

此程式碼檢查物件的敏捷性。 [  **IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 的呼叫擲回例外狀況，如果 `myimpl` 不敏捷的話。

```cppwinrt
winrt::com_ptr<MyType> myimpl{ winrt::make_self<MyType>() };
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.as<IAgileObject>() };
```

您可以呼叫 [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)，而不是處理例外狀況。

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.try_as<IAgileObject>() };
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** 沒有自己的方法，因此您無法使用它做很多事。 下一個變數比較一般。

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** 是一個 *標記介面*。 適用於 **IAgileObject** 的成功或失敗的查詢是您從其取得的資訊與公用程式的範圍。

## <a name="opting-out-of-agile-object-support"></a>選擇退出敏捷式物件支援

您可以藉由傳遞 [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile) 標記結構做為您基底類別的範本引數，明確地選擇退出敏捷式物件支援。

如果您直接從 **winrt::implements** 衍生。

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, winrt::non_agile>
{
    ...
}
```

如果您撰寫一個執行階段類別。

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, winrt::non_agile>
{
    ...
}
```

不論標記結構在 variadic 參數套件中出現的位置。

是否不論您選擇退出靈活度，您可以實作**IMarshal**自己。 例如，您可以使用**winrt::non_agile**標記，以避免預設的靈活度實作，並實作**IMarshal**自行&mdash;或許是為了支援傳值方式封送處理語意。

## <a name="agile-references-winrtagileref"></a>敏捷式參考資訊 (winrt::agile_ref)

如果您正使用不敏捷的物件，但您需要將其在某些潛在敏捷內容中傳遞，有一個方法是使用 [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) 結構範本，取得一個非敏捷式類型執行個體，或非敏捷式物件介面的敏捷式參考。

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```

或者，您可以使用 [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile) 協助程式函式。

```cppwinrt
NonAgileType nonagile_obj;
auto agile{ winrt::make_agile(nonagile_obj) };
```

任一情況下，現在可以自由在不同的 Apartment 將 `agile` 傳遞至執行緒，並且在那裡使用。

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again{ agile.get() };
winrt::hstring message{ nonagile_obj_again.Message() };
```

[  **Agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agile_refget-function) 呼叫傳回一個在呼叫 **get** 的執行緒內容中可安全使用的 Proxy。

## <a name="important-apis"></a>重要 API

* [IAgileObject 介面](https://msdn.microsoft.com/library/windows/desktop/hh802476)
* [IMarshal 介面](/windows/desktop/api/objidl/nn-objidl-imarshal)
* [winrt::agile_ref struct template](/uwp/cpp-ref-for-winrt/agile-ref)
* [winrt::implements 結構範本](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make_agile function template](/uwp/cpp-ref-for-winrt/make-agile)
* [winrt::non_agile marker struct](/uwp/cpp-ref-for-winrt/non-agile)
* [winrt::Windows::Foundation::IUnknown:: 函式](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as 函式](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)

## <a name="related-topics"></a>相關主題

* [了解與使用 COM 執行緒模型](https://msdn.microsoft.com/library/ms809971)
