---
description: Windows 執行階段是參考計數式系統；在這樣的系統中，請務必了解強式和弱式參考的重要性以及之間的區別。
title: C++/WinRT 中的弱式參考
ms.date: 05/16/2019
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, 強式, 弱式, 參考
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3ad6bb9a98b0fe2a699580001698740e44cea14f
ms.sourcegitcommit: cba3ba9b9a9f96037cfd0e07d05bd4502753c809
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/14/2019
ms.locfileid: "67870328"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>C++/WinRT 中的強式和弱式參考

Windows 執行階段是參考計數式系統；在這樣的系統中，請務必了解強式和弱式參考 (以及不是這兩者的參考，例如隱含 this  指標) 的重要性以及之間的區別。 您將在本主題中了解，針對順利執行的可靠系統和針對會意外當機的系統，正確管理這些參考的方式可能會有差異。 藉由提供可深入支援語言投影的協助程式函式，[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 可讓您輕鬆且正確地建置更複雜的系統。

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>安全地存取類別成員協同程式中的 this  指標

如需有關協同程式的詳細資訊和程式碼範例，請參閱[使用 C++/WinRT 的並行和非同步作業](/windows/uwp/cpp-and-winrt-apis/concurrency)。

下方列出的程式碼會示範典型的協同程式範例，其中一個類別有一個成員函式。 您可以複製此範例並貼到新 **Windows 主控台應用程式 (C++/WinRT)** 專案中的指定檔案。

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

struct MyClass : winrt::implements<MyClass, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    IAsyncOperation<winrt::hstring> RetrieveValueAsync()
    {
        co_await 5s;
        co_return m_value;
    }
};

int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };

    winrt::hstring result{ async.get() };
    std::wcout << result.c_str() << std::endl;
}
```

**MyClass::RetrieveValueAsync** 會花一些時間運作，並在最終傳回一份 `MyClass::m_value` 資料成員。 呼叫 **RetrieveValueAsync** 會建立非同步物件，而該物件具有隱含的 this  指標 (最終可透過此指標存取 `m_value`)。

請記住，在協同程式中，執行是同步的，直到在第一個暫停點上將控制項傳回呼叫端為止。 在 **RetrieveValueAsync** 中，第一個 `co_await` 是第一個暫停點。 協同程式恢復之前 (在此案例中，大約是五秒之後)，隱含的 this  指標 (可透過該指標存取 `m_value`) 上可能會發生任何事。

以下是完整的事件順序。

1. **MyClass** 的執行個體會在  之中建立 (`myclass_instance`)。
2. `async` 物件建立，並指向 (透過其中的 this  ) `myclass_instance`。
3. **winrt::Windows::Foundation::IAsyncAction::get** 函式會觸及其第一個暫停點、封鎖幾秒鐘的時間，然後傳回 **RetrieveValueAsync** 的結果。
4. **RetrieveValueAsync** 傳回 `this->m_value` 的值。

只要 this  仍有效，步驟 4 就是安全的。

但是，如果在非同步作業完成之前，類別執行個體就損毀了呢？ 有各種狀況會造成類別執行個體在非同步方法完成之前脫離範圍。 但是，我們可以藉由將類別執行個體設定為 `nullptr` 來模擬此狀況。

```cppwinrt
int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };
    myclass_instance = nullptr; // Simulate the class instance going out of scope.

    winrt::hstring result{ async.get() }; // Behavior is now undefined; crashing is likely.
    std::wcout << result.c_str() << std::endl;
}
```

在損毀類別執行個體的指標後方，這看起來像是我們沒有直接再次參考該執行個體。 但是非同步物件當然有指向它的 this  指標，而且嘗試使用該指標來複製類別執行個體中儲存的值。 協同程式是成員函式，而且其預期能夠順利使用 this  指標。

經由此程式碼變更，我們會在步驟 4 遇到問題，因為類別執行個體已損毀，而 this  已不再有效。 只要非同步物件嘗試存取類別執行個體內的變數，就會發生損毀 (或是執行某個未完全未定義的動作)。

解決方法是將非同步作業&mdash;協同程式&mdash;自己的強式參考提供給類別執行個體。 如同目前所撰寫的內容，協同程式可有效保留指向類別執行個體的原始 this  指標；但這並不足以讓類別執行個體存留下來。

若要讓類別執行個體存留下來，應變更 **RetrieveValueAsync** 的實作，如下列所示。

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

C++/WinRT 類別會直接或間接從 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 範本衍生。 因此，C++/WinRT 物件可以呼叫其受保護的 [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) 成員函式來對其 this  指標擷取強式參考。 請注意，您不需要在上述程式碼範例中確實使用 `strong_this` 變數，只要呼叫 **get_strong** 來累加 C++/WinRT 物件的參考計數，並讓其隱含的 this  指標保持有效即可。

> [!IMPORTANT]
> 由於 **get_strong** 是 **winrt::implements** 結構範本的成員函式，因此您只能從直接或間接衍生自 **winrt::implements** 的類別 (例如 C++/WinRT 類別) 中呼叫該函式。 如需有關衍生自 **winrt::implements** 的資訊和範例，請參閱[使用 C++/WinRT 撰寫 API](/windows/uwp/cpp-and-winrt-apis/author-apis)。

此方法解決了我們先前在步驟 4 時遇到的問題。 即使指向類別執行個體的所有其他參考都消失，協同程式已有保證其相依性穩定的預防措施。

如果強式參考並不適合，您可以改為呼叫 [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) 來對 this  擷取弱式參考。 請在確認您可以擷取強式參考後，再存取 this  。 同樣地，**get_weak** 是 **winrt::implements** 結構範本的成員函式。

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto weak_this{ get_weak() }; // Maybe keep *this* alive.

    co_await 5s;

    if (auto strong_this{ weak_this.get() })
    {
        co_return m_value;
    }
    else
    {
        co_return L"";
    }
}
```

在上述範例中，在未保留強式參考的情況下，弱式參考並不能避免類別執行個體發生損毀。 但可以讓您確認是否能在存取成員變數之前取得強式參考。

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>使用事件處理委派安全地存取 this  指標

### <a name="the-scenario"></a>案例

如需有關事件處理的一般資訊，請參閱[藉由在 C++/WinRT 中使用委派來處理事件](handle-events.md)。

在上一節中，我們已指出協同程式和並行存取方面的潛在存留期問題。 但是，如果您使用物件的成員函式，或是從物件成員函式裡的 lambda 函式中處理一個事件，您會需要考量事件 (處理事件的物件) 和事件來源 (引發事件的物件) 的相對存留時間。 讓我們來看看一些程式碼範例。

下方列出的程式碼會先定義簡單的 **EventSource** 類別，而其產生的一般事件會由任何已加入類別的委派來處理。 此範例事件會使用 [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) 委派類別，但此問題和補救方法適用於所有的委派類型。

然後，**EventRecipient** 類別會以 lambda 函式的形式為 **EventSource::Event** 事件提供處理常式。

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;

struct EventSource
{
    winrt::event<EventHandler<int>> m_event;

    void Event(EventHandler<int> const& handler)
    {
        m_event.add(handler);
    }

    void RaiseEvent()
    {
        m_event(nullptr, 0);
    }
};

struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event([&](auto&& ...)
        {
            std::wcout << m_value.c_str() << std::endl;
        });
    }
};

int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_source.RaiseEvent();
}
```

模式是事件收件者會有相依於其 this  指標的 lambda 事件處理常式。 只要事件收件者的存留期比事件來源長，處理常式的存留期就會超過這些相依性的存留期。 而且在這些情況下，該模式通常會順利運作。 這些案例中有些很明顯，例如，UI 網頁藉由頁面上的控制項處理事件時。 頁面存留期會超過按鈕存留期&mdash;因此，處理常式的存留期也會超過按鈕。 任何時候都是如此，收件者擁有來源 (例如，做為資料成員)，或收件者與來源屬同層級，且由其他物件擁有。

如果您確定有一個案例，其中處理常式不會超越其所依賴的 this  ，您便可以正常擷取 this  ，而不需要考量強式或弱式存留時間。

但仍有案例，其中 this  的存留期不會超過其在處理常式中的使用時間 (包括由非同步動作與作業引發的完成和進行中事件的處理常式)，而您必須知道如何處理這些狀況。

- 當事件來源「同步」  引發其事件時，您可以撤銷您的處理常式並確信您不會再收到任何事件。 但針對非同步事件，即使在撤銷 (尤其是在建構函式內撤銷) 之後，執行中的事件可能會在開始解構之後到達您的物件。 在解構前找到可取消訂閱的地方可能會減輕問題，但要繼續閱讀以取得健全的解決方案。
- 如果您正撰寫協同程式來實作非同步方法，就有可能發生此狀況。
- 在少數使用特定 XAML UI 架構物件 (例如，[**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel)) 的案例中有可能發生 (如果收件者不需要從事件來源取消註冊，便能完成的話)。

### <a name="the-issue"></a>問題

下一版的 **main** 函式在此會模擬當事件來源仍在產生事件時，事件收件者卻發生損毀 (或許是脫離範圍) 時的狀況。

```cppwinrt
int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_recipient = nullptr; // Simulate the event recipient going out of scope.
    event_source.RaiseEvent(); // Behavior is now undefined within the lambda event handler; crashing is likely.
}
```

事件收件者雖發生損毀，但其中的 lambda 事件處理常式仍訂閱 **Event** 事件。 事件產生時，lambda 會嘗試對該階段中無效的 this  指標取值 (dereference)。 因此，處理常式 (或協同程式的接續) 中嘗試使用該指標的程式碼會產生存取違規。

> [!IMPORTANT]
> 如果您遇到這類情形，不論擷取的this  物件是否能存留得此擷取作業久，您都需要考量 this  物件的存留時間。 如果物件沒有存留得比擷取作業久，則使用強式或弱式參考來加以擷取，我們將在下方示範此操作。
>
> 或&mdash;如果您的案例適合的話，且如果執行緒考量甚至可行&mdash;則另一個方法是，在收件者完成事件後，或在收件者解構函式中，撤銷處理常式。 請參閱[撤銷已註冊的委派](handle-events.md#revoke-a-registered-delegate)。

這是我們將註冊處理常式的方式。

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

lambda 會自動依據參考來擷取任何區域變數。 因此，針對此範例，我們可以同樣地撰寫下列內容。

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

在這兩種情況下，我們都只擷取原始 this  指標。 這並不會影響參考計數，因此無法避免目前物件發生損毀。

### <a name="the-solution"></a>解決方案

解決方案是擷取強式參考 (或者，如我們所見，若弱式參考更為適當，則擷取之)。 強式參考「會」  累加參考計數，並且「會」  讓目前物件存留下來。 您只需宣告擷取變數 (在此範例中稱為 `strong_this`)，並藉由對 [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) 發出呼叫來將其初始化，此呼叫會對我們的 this  指標擷取強式參考。

> [!IMPORTANT]
> 由於 **get_strong** 是 **winrt::implements** 結構範本的成員函式，因此您只能從直接或間接衍生自 **winrt::implements** 的類別 (例如 C++/WinRT 類別) 中呼叫該函式。 如需有關衍生自 **winrt::implements** 的資訊和範例，請參閱[使用 C++/WinRT 撰寫 API](/windows/uwp/cpp-and-winrt-apis/author-apis)。

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

您甚至可以略過目前物件的自動擷取，並透過擷取變數存取資料成員 (而非透過隱含的 this  )。

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

如果強式參考並不適合，您可以改為呼叫 [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) 來對 this  擷取弱式參考。 弱式參考「不會」  讓目前的物件保持運作。 所以，在確認您仍可以從弱式參考擷取強式參考後，再存取成員。

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

如果您擷取原始指標，則必須確保讓指向的物件保持運作。

### <a name="if-you-use-a-member-function-as-a-delegate"></a>如果您使用成員函式作為委派

如同 lambda 函式，這些原則也適用於使用成員函式作為您的委派。 由於語法不同，因此我們來看看一些程式碼。 首先，以下是可能不安全的成員函式事件處理常式，並且正在使用原始 this  指標。

```cppwinrt
struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event({ this, &EventRecipient::OnEvent });
    }

    void OnEvent(IInspectable const& /* sender */, int /* args */)
    {
        std::wcout << m_value.c_str() << std::endl;
    }
};
```

這是參考物件和其成員函式的慣用標準方式。 若要讓此方式變得安全，您可以在註冊處理常式時建立強式或弱式參考&mdash;從 Windows SDK 的 10.0.17763.0 (Windows 10 1809 版) 開始&mdash;。 此時，我們已知事件的收件者物件仍在執行。

若要建立強式參考，只需要呼叫 [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) 來取代原始 this  指標。 C++/ WinRT 可確保產生的委派會保留目前物件的強式參考。

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

擷取強式參考表示只有在取消註冊處理常式且所有未完成的回呼都已傳回後，您的物件才有資格解構。 不過，該保證只有在引發事件時才有效。 如果您的事件處理常式為非同步，您必須在第一個暫停點之前，為您的協同程式提供類別執行個體的強式參考 (如需詳細資訊以及程式碼，請參閱本主題稍早的[安全地存取類別成員協同程式中的 this  指標](#safely-accessing-the-this-pointer-in-a-class-member-coroutine)一節)。 但是，這會在事件來源與您的物件之間建立循環參考，因此您必須藉由撤銷事件來明確中斷。

若要建立弱式參考，請呼叫 [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)。 C++/ WinRT 可確保產生的委派會保留弱式參考。 在最後一刻，委派會在幕後嘗試讓弱式參考變成強式參考，並且只會在成功時呼叫成員函式。

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

如果委派「的確」  呼叫您的成員函式，則 C++/WinRT 會讓您的物件保持運作，直到您的處理常式傳回為止。 不過，如果您的處理常式為非同步，則它會在暫停點傳回，因此您必須在第一個暫停點之前，為您的協同程式提供類別執行個體的強式參考。 同樣地，如需詳細資訊，請參閱本主題稍早的[安全地存取類別成員協同程式中的 this  指標](#safely-accessing-the-this-pointer-in-a-class-member-coroutine)一節。

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>弱式參考範例會使用 **SwapChainPanel::CompositionScaleChanged**

在此程式碼範例中，我們會以另一種弱式參考形式來使用 [**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) 事件。 程式碼會使用擷取收件者弱式參考資料的 lambda 註冊事件處理常式。

```cppwinrt
winrt::Windows::UI::Xaml::Controls::SwapChainPanel m_swapChainPanel;
winrt::event_token m_compositionScaleChangedEventToken;

void RegisterEventHandler()
{
    m_compositionScaleChangedEventToken = m_swapChainPanel.CompositionScaleChanged([weak_this{ get_weak() }]
        (Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
        Windows::Foundation::IInspectable const& object)
    {
        if (auto strong_this{ weak_this.get() })
        {
            strong_this->OnCompositionScaleChanged(sender, object);
        }
    });
}

void OnCompositionScaleChanged(Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
    Windows::Foundation::IInspectable const& object)
{
    // Here, we know that the "this" object is valid.
}
```

Lamba 擷取子句中，已建立暫存變數，代表 this  的弱式參考。 在 lambda 本文中，如果可以取得 this  的強式參考，則可呼叫 **OnCompositionScaleChanged** 函式。 如此一來，在 **OnCompositionScaleChanged** 中，可安全使用 this  。

## <a name="weak-references-in-cwinrt"></a>C++/WinRT 中的弱式參考

在上述內容中，我們已看到正在使用的弱式參考。 一般情況下，此方式適合用來中斷循環參考。 例如，XAML 型 UI 架構的原生實作需要 C++/WinRT 中的弱式參考機制才能處理循環參考&mdash;因為架構的歷史設計&mdash;。 不過，除了 XAML，您可能不需要使用弱式參考 (請注意任何在本質上是 XAML 特有內容的項目)。 您應該多以這種方法來設計自己的 C++/WinRT API，以避免循環參考和弱式參考的需求。 

針對您宣告的任何指定類型，當弱式參考是必要的，是否無法馬上看得出來是 C++/WinRT。 因此，C++/WinRT 自動在結構範本[**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 上提供弱式參考支援，您的 C++/WinRT 類型直接或間接從其中衍生。 它是使用付費，其中您不需要支付任何項目除非確實針對 [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) 查詢您的物件。 且您可以明確選擇[退出該支援](#opting-out-of-weak-reference-support)。

### <a name="code-examples"></a>程式碼範例
[**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref)結構範本是取得類別執行個體的弱式參考選項之一。

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```

或者，您可以使用 [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak) 協助程式函式。

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

建立弱式參考並不會影響物件本身的參考計數。只會配置控制區。 該控制區負責實作弱式參考語意。 然後您可以嘗試將弱式參考升級為強式參考，如果成功了，請使用它。

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

提供的一些其他強式參考仍然存在，[**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weak_refget-function) 呼叫遞增參考計數並將強式參考傳回給該呼叫者。

### <a name="opting-out-of-weak-reference-support"></a>不使用弱式參考資料支援
弱式參考資料支援會自動執行。 但您可以藉由傳遞 [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) 標記結構做為基底類別的範本引數，明確地選擇退出物件支援。

如果您直接從 **winrt::implements** 衍生。

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

如果您正在撰寫執行階段類別。

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

標記結構在 variadic 參數套件中出現的位置並不重要。 如果您要求弱式參考一個退出的類型，然後編譯器會協助您使用「*這僅適用於弱式參考支援*」。

## <a name="important-apis"></a>重要 API
* [implements::get_weak 函式](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::make_weak 函式範本](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref 標記結構](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref 結構範本](/uwp/cpp-ref-for-winrt/weak-ref)
