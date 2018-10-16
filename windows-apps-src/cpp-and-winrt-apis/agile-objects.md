---
author: stevewhims
description: 敏捷式物件是可以從任何執行緒中存取的一種物件。 C++/WinRT 預設為敏捷式，但您可以選擇退出。
title: 使用 C++/WinRT 的敏捷式物件
ms.author: stwhi
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、敏捷式、物件、敏捷性、IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 9af1fb0a9d23727924ae3c165bc8977fb9cc7774
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2018
ms.locfileid: "4623252"
---
# <a name="agile-objects-in-cwinrt"></a>在 C++/WinRT 的敏捷式物件
在大部分案例中，Windows 執行階段課類別的執行個體&mdash;像標準 C++ 物件一樣&mdash;可以從任何執行緒存取。 這樣的類別是*敏捷的*。 只有少數隨附於 Windows 的 Windows 執行階段類別是非敏捷的，但當您使用它們時，需要考量其執行緒模型與封送處理行為 (封送處理會通過執行緒或處理程序的界限傳遞資料)。 適用於敏捷式物件，每個 Windows 執行階段物件的最佳預設值是讓您自己[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)是敏捷的預設值。

但您可以選擇退出。您可能會有個有力的理由，讓您類型的物件駐留，例如，指定的單一執行緒 Apartment。 這通常與重新進入需求有關。 但即使使用者介面 (UI) API 提供越來越多敏捷式物件。 一般而言，敏捷性式最簡單且效能最佳的選項。 此外，當您實作啟動原廠，它必須是敏捷的，即使您對應執行階段類別不是。

> [!NOTE]
> Windows 執行階段是根據 COM。 在 COM 條款中，在 `ThreadingModel` = *Both* 中註冊一個敏捷式類別。 如需有關 COM 執行緒模式的詳細資訊，請參閱 [了解與使用 COM 執行緒模式](https://msdn.microsoft.com/library/ms809971)。

## <a name="code-examples"></a>程式碼範例
我們使用實作範例說明 C++/WinRT 如何支援敏捷性。

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

因為我們尚未退出，因為此實作是敏捷的。 [**Winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基礎結構實作 [**IAgileObject**](https://msdn.microsoft.com/library/windows/desktop/hh802476) 和 [**IMarshal**](https://docs.microsoft.com/previous-versions/windows/embedded/ms887993)。 **IMarshal** 實作使用 **CoCreateFreeThreadedMarshaler** 執行適用於不知道 **IAgileObject** 的舊版程式碼的正確做法。

此程式碼檢查物件的敏捷性。 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 的呼叫擲回例外狀況，如果 `myimpl` 不敏捷的話。

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
winrt::com_ptr<IAgileObject> iagileobject = myimpl.as<IAgileObject>();
```

您可以呼叫 [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)，而不是處理例外狀況。

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject = myimpl.try_as<IAgileObject>();
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** 沒有自己的方法，因此您無法使用它做很多事。 下一個變數比較一般。

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** 是一個 *標記介面*。 適用於 **IAgileObject** 的成功或失敗的查詢是您從其取得的資訊與公用程式的範圍。

## <a name="opting-out-of-agile-object-support"></a>選擇退出敏捷式物件支援
您可以藉由傳遞 [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non_agile) 標記結構做為您基底類別的範本引數，明確地選擇退出敏捷式物件支援。

如果您直接從 **winrt::implements** 衍生。

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, non_agile>
{
    ...
}
```

如果您撰寫一個執行階段類別。

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, non_agile>
{
    ...
}
```

不論標記結構在 variadic 參數套件中出現的位置。

不論您選是否擇退出敏捷性，您都可以自行實作 IMarshal。 例如，您可以使用 [**non_agile**] 標記避免預設敏捷性實作，並親自實作 IMarshal&mdash;或許可支援依照值封送處理的語意。

## <a name="agile-references-winrtagileref"></a>敏捷式參考資訊 (winrt::agile_ref)
如果您正使用不敏捷的物件，但您需要將其在某些潛在敏捷內容中傳遞，有一個方法是使用 [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) 結構範本，取得一個非敏捷式類型執行個體，或非敏捷式物件介面的敏捷式參考。

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```
或者，您可以使用 [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile) 協助程式函式。

```cppwinrt
NonAgileType nonagile_obj;
auto agile = winrt::make_agile(nonagile_obj);
```

任一情況下，現在可以自由在不同的 Apartment 將 `agile` 傳遞至執行緒，並且在那裡使用。

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again = agile.get();
winrt::hstring message = nonagile_obj_again.Message();
```

[**Agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agilerefget-function) 呼叫傳回一個在呼叫 **get** 的執行緒內容中可安全使用的 Proxy。

## <a name="important-apis"></a>重要 API
* [IAgileObject 介面](https://msdn.microsoft.com/library/windows/desktop/hh802476)
* [IMarshal 介面](https://docs.microsoft.com/previous-versions/windows/embedded/ms887993)
* [winrt::agile_ref 結構範本](/uwp/cpp-ref-for-winrt/agile-ref)
* [winrt::implements 結構範本](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make_agile 函式範本](/uwp/cpp-ref-for-winrt/make-agile)
* [winrt::non_agile 標記結構](/uwp/cpp-ref-for-winrt/non_agile)
* [winrt::Windows::Foundation::IUnknown::as 函式](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as 函式](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)

## <a name="related-topics"></a>相關主題
* [了解與使用 COM 執行緒模式](https://msdn.microsoft.com/library/ms809971)
