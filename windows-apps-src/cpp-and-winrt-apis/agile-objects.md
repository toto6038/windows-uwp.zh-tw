---
description: 敏捷式物件是可以從任何執行緒中存取的一個。 C++/WinRT 預設為敏捷式，但您可以選擇退出。
title: 使用 C++/WinRT 的敏捷式物件
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, 敏捷式, 物件, 敏捷性, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 82dff619e6fa3934f69b93090bee90de6359ca07
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "66360326"
---
# <a name="agile-objects-in-cwinrt"></a>使用 C++/WinRT 的敏捷式物件

在大部分案例中，Windows 執行階段類別的執行個體可以從任何執行緒存取 (就像大部分的標準 C++ 物件一樣)。 這樣的 Windows 執行階段類別是「敏捷式」  。 只有少數隨附於 Windows 的 Windows 執行階段類別是非敏捷的，但當您使用它們時，需要考量其執行緒模式與封送處理行為 (封送處理會跨 Apartment 界限傳遞資料)。 它是可讓每個 Windows 執行階段物件成為敏捷式的最佳預設值，因此您自己的 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 類型在預設情況下是敏捷的。

但是，您可以選擇退出。您可能會有個有力的理由，讓您類型的物件駐留，例如，在指定的單一執行緒 Apartment 中。 這通常與重新進入需求有關。 但是，甚至使用者介面 (UI) API 也提供越來越多敏捷式物件。 一般而言，敏捷性是最簡單且效能最佳的選項。 此外，當您實作啟用處理站，它必須是敏捷式，即使您的對應執行階段類別不是。

> [!NOTE]
> Windows 執行階段是以 COM 為基礎。 在 COM 條款中，會向 `ThreadingModel` = *Both* 註冊敏捷式類別。 如需有關 COM 執行緒模式和 Apartment 的詳細資訊，請參閱[了解與使用 COM 執行緒模式](/previous-versions/ms809971(v=msdn.10))。

## <a name="code-examples"></a>程式碼範例

讓我們使用執行階段類別的實作範例，說明 C++/WinRT 如何支援敏捷性。

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

因為我們尚未退出，因為此實作是敏捷的。 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基底結構實作 [**IAgileObject**](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject) 與 [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal)。 **IMarshal** 實作使用 **CoCreateFreeThreadedMarshaler** 執行適用於不知道 **IAgileObject** 的舊版程式碼的正確做法。

此程式碼檢查物件的敏捷性。 如果 `myimpl` 不是敏捷式，[**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 的呼叫會擲回例外狀況。

```cppwinrt
winrt::com_ptr<MyType> myimpl{ winrt::make_self<MyType>() };
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.as<IAgileObject>() };
```

您可以呼叫 [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)，而不是處理例外狀況。

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.try_as<IAgileObject>() };
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** 沒有自己的方法，因此您無法使用它做很多事。 下一個變數則比較一般。

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** 是「base struct」  。 **IAgileObject** 的成功或失敗查詢是您從其取得的資訊與公用程式的範圍。

## <a name="opting-out-of-agile-object-support"></a>選擇退出敏捷式物件支援

您可以透過傳遞 [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile) 標記結構做為您基底類別的範本引數，明確地選擇退出敏捷式物件支援。

如果您直接從 **winrt::implements** 衍生。

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, winrt::non_agile>
{
    ...
}
```

如果您正在撰寫執行階段類別。

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, winrt::non_agile>
{
    ...
}
```

標記結構在 variadic 參數套件中出現的位置並不重要。

不論您是否選擇退出敏捷性，您都可以自行實作 **IMarshal**。 例如，您可以使用 **winrt::non_agile** 標記避免預設敏捷性實作，並自行實作 **IMarshal**&mdash;或許可支援「依值封送處理」語意。

## <a name="agile-references-winrtagile_ref"></a>敏捷式參考資訊 (winrt::agile_ref)

如果您正在使用非敏捷式物件，但您需要在某些潛在敏捷內容中傳遞它，有一個方法是使用 [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) 結構範本來取得非敏捷式類型執行個體或非敏捷式物件介面的敏捷式參考。

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```

或者，您可以使用 [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile) 協助程式函式。

```cppwinrt
NonAgileType nonagile_obj;
auto agile{ winrt::make_agile(nonagile_obj) };
```

在任一情況下，`agile` 現在可以自由地傳遞到不同 Apartment 中的執行緒，並在那裡使用。

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again{ agile.get() };
winrt::hstring message{ nonagile_obj_again.Message() };
```

[**agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agile_refget-function) 呼叫會傳回一個可在所呼叫 **get** 的執行緒內容中安全使用的 Proxy。

## <a name="important-apis"></a>重要 API

* [IAgileObject 介面](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject)
* [IMarshal 介面](/windows/desktop/api/objidl/nn-objidl-imarshal)
* [winrt::agile_ref 結構範本](/uwp/cpp-ref-for-winrt/agile-ref)
* [winrt::implements 結構範本](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make_agile 函式範本](/uwp/cpp-ref-for-winrt/make-agile)
* [winrt::non_agile 標記結構](/uwp/cpp-ref-for-winrt/non-agile)
* [winrt::Windows::Foundation::IUnknown::as 函式](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as 函式](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)

## <a name="related-topics"></a>相關主題

* [了解與使用 COM 執行緒模式](/previous-versions/ms809971(v=msdn.10))
