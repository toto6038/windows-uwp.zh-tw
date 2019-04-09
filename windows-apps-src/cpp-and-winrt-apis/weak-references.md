---
description: Windows 執行階段是參考計數式系統；在這樣的系統中，請務必了解強式和弱式參考的重要性以及之間的區別。
title: C++/WinRT 中的弱式參考
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 投影、 強式、 弱的參考
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0e2e40daaf777e36094b698d058f21840b1804c8
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291826"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>中的強式和弱式參考C++/WinRT

Windows 執行階段是參考計數系統;務必要了解的重要性，以及區別，這類系統在強式和弱式參考 (和參考，兩者皆否，例如隱含*這*指標)。 如您所見本主題中，了解如何正確地管理這些參考可能是指一套可靠的系統執行順暢和當機不正常的一個之間的差異。 藉由提供 helper 函式有深入的支援語言 projekci [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)符合您在您的工作，建置更複雜的系統，只需且正確的中間數。

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>安全地存取*這*類別成員協同程式中的指標

在下方列出的程式碼會顯示為類別成員函式的協同程式的典型範例。

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

**MyClass::RetrieveValueAsync**花一些時間運作，並傳回一份最終`MyClass::m_value`資料成員。 呼叫**RetrieveValueAsync**導致非同步物件建立，且該物件具有隱含*這*指標 (透過它，最後，`m_value`存取)。

以下是完整的事件順序。

1. 在 **主要**，執行個體**MyClass**建立 (`myclass_instance`)。
2. `async`建立物件時，指 (透過其*這*) 來`myclass_instance`。
3. **Winrt::Windows::Foundation::IAsyncAction::get**函式封鎖幾秒鐘的時間，並接著會傳回的結果**RetrieveValueAsync**。
4. **RetrieveValueAsync**傳回的值`this->m_value`。

步驟 4 是安全，只要*這*有效。

但是，在非同步作業完成之前，如果終結類別的執行個體嗎？ 有各種方法的非同步方法完成之前，將無法移出範圍的類別執行個體。 但是，我們可以將類別執行個體設定為來模擬`nullptr`。

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

之後我們損毀的類別執行個體，看起來我們沒有直接參考它一次。 當然有非同步的物件，但*這*指標給它，而且嘗試使用它來複製類別執行個體中儲存的值。 協同程式是成員函式，而且它預期能夠使用其*這*白白指標。

透過這項變更的程式碼，我們遇到問題在步驟 4，因為已損毀的類別執行個體，並*這*已不再有效。 非同步物件會嘗試存取類別執行個體內的變數，因為它會損毀 （或是執行某個動作未完全定義）。

解決方法是提供非同步作業&mdash;協同程式&mdash;它自己的類別執行個體的強式參考。 如同目前所撰寫，有效地協同程式保留原始*這*指標的類別執行個體，但這並不足夠，以維持的類別執行個體。

若要維持類別執行個體，將變更的實作**RetrieveValueAsync**下列所示。

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

因為C++/WinRT 物件直接或間接衍生自[ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) ] 範本， C++/WinRT 物件都可以呼叫其[ **implements.get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)受保護成員函式來擷取的強式參考其*這*指標。 請注意，則不需要實際使用`strong_this`變數，只呼叫**get_strong**您的參考計數會遞增，並讓您隱含*這*有效的指標。

這個方法可以解決的問題，先前我們進到步驟 4 時。 即使所有其他參考的類別執行個體消失，協同程式已保證其相依性是穩定的預防措施。

如果強式參考並不恰當，則您可以改為呼叫[ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)若要擷取的弱式參考*這*。 只要確認您可以存取之前擷取的強式參考*這*。

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

在上述範例中，弱式參考並不保留被終結時沒有強式參考保留的類別執行個體。 但可以讓您檢查是否可以存取的成員變數之前取得的強式參考。

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>安全地存取*這*與事件處理委派的指標

### <a name="the-scenario"></a>案例

一般事件處理的詳細資訊，請參閱[使用中的委派處理事件C++/WinRT](handle-events.md)。

上一節中，反白顯示潛在存留期協同程式和並行存取方面的問題。 但是，如果您處理的事件物件的成員函式，或是從 lambda 內的函式物件的成員函式，則您必須考慮事件收件者 （處理事件的物件） 和事件來源 （該物件的相對存留期引發事件）。 讓我們看看一些程式碼範例。

下方第一個列出的程式碼定義簡單**EventSource**類別，這會引發泛用的事件處理的任何已加入至它的委派。 此範例的事件會使用[ **Windows::Foundation::EventHandler** ](/uwp/api/windows.foundation.eventhandler)委派型別，但問題和這裡的補救方法適用於所有的委派型別。

然後， **EventRecipient**類別提供的處理常式**EventSource::Event** lambda 函式的形式的事件。

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

模式是事件收件者具有相依性的 lambda 事件處理常式，其*這*指標。 每當事件收件者還久的事件來源，它會超過這些相依性。 並在這些情況下，也就是常見的此模式非常適合。 這些案例中有些很明顯，例如，UI 網頁藉由頁面上的控制項處理事件時。 頁面還久按鈕&mdash;因此，處理常式也還久的按鈕。 任何時候都是如此，收件者擁有來源 (例如，做為資料成員)，或收件者與來源屬同層級，且由其他物件擁有。 如果您確定有一個案例，其中處理常式不會超越其所依賴的*this*，您便可以正常擷取*this*，而不需要考量強式或弱式存留時間。

但仍有的情況下所在*這*不必須有存在 （包括完成和進度的非同步動作與作業所引發的事件處理常式） 的處理常式，用於務必要知道如何處理它們。

- 如果您正撰寫協同程式，實作一種非同步方法，則是有可能。
- 少數案例中特定 XAML UI 架構物件 (例如，[**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel))，則是有可能的，如果收件者不需要從事件來源取消註冊，便能完成的話。

### <a name="the-issue"></a>此問題

下的一版**主要**函式會模擬在終結事件收件者時，會發生什麼事 （或許是超出範圍） 時的事件來源仍會引發事件。

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

損毀事件收件者，但仍訂閱 lambda 事件處理常式，其內**事件**事件。 當引發該事件時，lambda 會嘗試取值 （dereference）*這*指標，這是在該點無效。 因此，發生存取違規所得的處理常式中 （或協同程式的接續） 程式碼嘗試使用它。

> [!IMPORTANT]
> 如果您遇到的情況下，像這樣，則您必須思考的存留期*這*物件和是否擷取*這*物件還久的擷取。 如果沒有，然後擷取它以強式或弱式參考，我們將示範以下。
>
> 或是&mdash;如果它適合您案例中，且執行緒考量進行它甚至可以&mdash;則會撤銷此處理常式與事件，或在 收件者的解構函式中完成收件者的另一個選項。 請參閱[撤銷已註冊的委派](handle-events.md#revoke-a-registered-delegate)。

這是我們正在註冊處理常式的方式。

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Lambda 會自動依參考來擷取任何區域變數。 因此，針對此範例中，我們可以對等程式碼撰寫如下。

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

在這兩種情況下，只擷取原始*這*指標。 而且，沒有任何作用在參考計數，以便執行任何動作，導致目前的物件無法終結。

### <a name="the-solution"></a>解決方案

解決方法是擷取的強式參考。 強式參考*並未*遞增參考計數，以及它*沒有*維持目前的物件。 您只是宣告擷取變數 (稱為`strong_this`在此範例中)，並將它初始化藉由呼叫[ **implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)，表示擷取的強式參考我們*這*指標。

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

您甚至可以略過目前的物件，自動擷取，並透過而不是透過隱含擷取變數存取的資料成員*這*。

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

如果強式參考並不恰當，則您可以改為呼叫[ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)若要擷取的弱式參考*這*。 只確認，所以您仍可以擷取從它的強式參考之前存取成員。

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

### <a name="if-you-use-a-member-function-as-a-delegate"></a>如果您使用的成員函式與委派

與使用 lambda 函式，這些原則也適用於使用成員函式，為您的委派。 語法不同，因此讓我們看看一些程式碼。 首先，以下是可能不安全的成員函式事件處理常式，使用原始*這*指標。

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

這是標準的傳統方式來參考物件和其成員函式。 若要使這個安全，您可以&mdash;10.0.17763.0 (Windows 10 版本 1809年) 版的 Windows SDK&mdash;建立強式或弱式參考註冊處理常式的所在之處。 此時，事件的收件者物件已知為仍在執行。

如需強式參考，只需要呼叫[ **get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)來取代原始*這*指標。 C++/ WinRT 可確保產生的委派包含目前物件的強式參考。

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

如需弱式參考，呼叫[ **get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)。 C++/ WinRT 可確保產生的委派保留的弱式參考。 在最後時刻，並在幕後，委派會嘗試解決一個強式的弱式參考，並只會呼叫此成員函式如果成功。

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>弱式參考的範例使用**SwapChainPanel::CompositionScaleChanged**

在此範例中，我們會使用[ **SwapChainPanel::CompositionScaleChanged** ](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged)透過另一個圖解，弱式參考的事件。 程式碼會註冊事件處理常式使用 lambda 擷取 收件者的弱式參考。

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

Lamba 擷取子句中，已建立暫存變數，代表*this*的弱式參考資料。 lambda 本文中，如果可以取得*this*的強式參考資料，則可呼叫**OnCompositionScaleChanged**函式。 如此一來，在 **OnCompositionScaleChanged** 中，可安全使用 *this*。

## <a name="weak-references-in-cwinrt"></a>C++/WinRT 中的弱式參考

以上版本，我們會看到正在使用的弱式參考。 一般情況下，它們適合用來中斷循環參考。 例如，對於以 XAML 為基礎的 UI 架構的原生實作&mdash;歷史架構設計&mdash;弱式參考機制，在C++/WinRT 是為了處理循環參考。 外部 XAML，不過，您可能不需要使用弱式參考 (不會有任何項目本質上特定 XAML 相關)。 而是您應該往往，能夠設計您自己C++一種避免需要循環參考和弱式參考的 WinRT Api。 

針對您宣告的任何指定類型，當弱式參考是必要的，是否無法馬上看得出來是 C++/WinRT。 因此，C++/WinRT 自動在結構範本[**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 上提供弱式參考支援，您的 C++/WinRT 類型直接或間接從其中衍生。 它是使用付費，其中您不需要支付任何項目除非確實針對 [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) 查詢您的物件。 且您可以明確選擇[退出該支援](#opting-out-of-weak-reference-support)。

### <a name="code-examples"></a>程式碼範例
[  **Winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref)結構範本是取得類別執行個體的弱式參考選項之一。

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```

或者，您可以使用 [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak) 協助程式函式。

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

建立弱式參考並不會影響物件本身的參考計數。只會導致配置控制區。 該控制區負責實作弱式參考語意。 然後您可以嘗試將弱式參考升級為強式參考，如果成功了，請使用它。

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

如果您撰寫一個執行階段類別。

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

不論標記結構在 variadic 參數套件中出現的位置。 如果您要求弱式參考一個退出的類型，然後編譯器會協助您使用「*這僅適用於弱式參考支援*」。

## <a name="important-apis"></a>重要 API
* [implements::get_weak 函式](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::make_weak 函式樣板](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref marker struct](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref 結構範本](/uwp/cpp-ref-for-winrt/weak-ref)
