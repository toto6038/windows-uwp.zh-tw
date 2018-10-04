---
author: stevewhims
description: Windows 執行階段是計算參考次數的系統。請務必讓您了解的重要性和區別，這類系統在強式與弱式參考。
title: C++/WinRT 中的弱式參考資料
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 投影、 強式弱式、 參考
ms.localizationpriority: medium
ms.openlocfilehash: 414a73c8df31e4547b8bd154945a8e9960529320
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "4354756"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>強式和弱式參考資料，在 C + + /winrt

Windows 執行階段是計算參考次數的系統。且這類系統中很重要的強式知道的重要性和區別，弱式參考 （及既不存在，例如隱含*此*指標的參考）。 您會看到本主題中，了解如何管理這些參考正確可能表示可靠的系統執行可順暢地，另一個則損毀置中行為之間的差異。 藉由提供深入的支援的語言投影，協助程式函式[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)符合您在您的工作，只要並正確地建置更複雜的系統的一半。

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>安全地存取*此*指標的類別成員協同程式

下列程式碼清單會顯示為類別的成員函式在協同程式的一般範例。

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

**MyClass::RetrieveValueAsync**運作一段時間，以及最後會接著它會傳回在一份`MyClass::m_value`資料成員。 呼叫**RetrieveValueAsync**造成非同步物件建立，且該物件具有隱含*此*指標 (透過哪個，最後，`m_value`存取)。

以下是完整的順序的事件。

1. 在**主要**，建立**MyClass**的執行個體 (`myclass_instance`)。
2. `async`建立物件時，（透過其*此*） 指向`myclass_instance`。
3. **Winrt::Windows::Foundation::IAsyncAction::get**函式幾秒鐘，封鎖，然後傳回**RetrieveValueAsync**的結果。
4. **RetrieveValueAsync**傳回的值`this->m_value`。

步驟 4 是安全，*此*為有效時。

但是，如果非同步作業完成之前，會損毀的類別執行個體嗎？ 有各種類別執行個體無法超出範圍之前先完成的非同步方法的方式。 但是，我們可以設定為類別執行個體來模擬`nullptr`。

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

之後我們損毀的類別執行個體的某個點，它看起來就像我們不直接參考它一次。 但當然非同步物件有*此*指標，並嘗試使用該資料來複製的類別執行個體內部儲存的值。 協同程式便是成員函式，以及它預期要能夠使用白白其*這個*指標。

透過這項變更的程式碼，我們遇到問題，以在步驟 4，因為已被摧毀類別執行個體，而且*此*不再有效。 儘速非同步物件會嘗試存取內部類別執行個體的變數，它將會當機 （或完全未定義的方式執行）。

在方案時，為非同步作業&mdash;協同程式&mdash;類別執行個體自己強式參考。 為目前撰寫、 協同程式有效地會保留的原始*此*指標的類別執行個體。但是，並不足以讓類別執行個體持續執行。

若要讓類別執行個體持續執行，變更**RetrieveValueAsync**的實作，如下所示。

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

因為 C + + /winrt 物件直接或間接衍生自[**winrt:: implements**](/uwp/cpp-ref-for-winrt/implements)範本的 C + + /winrt 物件可以呼叫其[**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)受保護的成員函式，來擷取*此*其指標的強式參考。 請注意，在無需實際上使用`strong_this`變數。只要呼叫**get_strong**遞增您參考計數，並在保留隱含*此*指標有效。

這個方法可以解決的問題，我們先前已有在我們之後到步驟 4。 即使類別執行個體的所有其他參考會消失，協同程式便已採取保證其相依性即會穩定的預防的措施。

如果強式參考不適當，您可以改為呼叫[**implements:: get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)來擷取*此*的弱式參考。 只是確認您可以存取*此*之前擷取的強式參考。

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

在上述範例中，弱式參考資料不會保留時維持強式參考不被摧毀類別執行個體。 但是，它可以讓您檢查是否可以存取成員變數之前中取得的強式參考。

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>安全地存取*此*指標事件處理委派

### <a name="the-scenario"></a>案例

如需一般有關的事件處理的詳細資訊，請參閱[處理事件，藉由使用委派在 C + + /winrt](handle-events.md)。

如上一節反白顯示的協同程式並行區域中的潛在存留期問題。 但是，如果您處理事件的物件的成員函式，或是從 lambda 函式內物件的成員函式，則您需要考量事件收件者 （處理事件的物件） 和事件來源 （的物件的相對存留時間引發事件）。 我們來看看一些程式碼範例。

以下程式碼清單會先定義引發任何已新增至它的委派來處理的一般事件的簡單**EventSource**類別。 這個範例事件會使用[**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler)委派類型，但將套用到所有的委派類型的問題和解決方式以下。

然後， **EventRecipient**類別提供的 lambda 函式的形式**EventSource::Event**事件的處理常式。

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

模式則是以事件收件者會在其*這個*指標上具有與相依性的 lambda 事件處理常式。 每當事件收件者會超越事件來源，它會超越這些相依性。 並在這些情況下，很常見，模式運作良好。 這些案例中有些很明顯，例如，UI 網頁藉由頁面上的控制項處理事件時。 在頁面超越按鈕&mdash;處理常式，也超越按鈕。 任何時候都是如此，收件者擁有來源 (例如，做為資料成員)，或收件者與來源屬同層級，且由其他物件擁有。 如果您確定有一個案例，其中處理常式不會超越其所依賴的*this*，您便可以正常擷取*this*，而不需要考量強式或弱式存留時間。

但仍有案例，其中*這*不會超越處理常識其使用 （包括完成和由非同步動作與作業引發進度事件的處理常式） 的處理常式中，而它務必了解如何處理它們。

- 如果您正撰寫協同程式，實作一種非同步方法，則是有可能。
- 少數案例中特定 XAML UI 架構物件 (例如，[**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel))，則是有可能的，如果收件者不需要從事件來源取消註冊，便能完成的話。

### <a name="the-issue"></a>問題

這個下一個版本的**主要**功能的模擬終結事件收件者時，會發生什麼事 （或許可其超出範圍） 時的事件來源仍會引發事件。

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

事件收件者會損壞，但在它的 lambda 事件處理常式仍然訂閱**事件**的事件。 當引發該事件時，lambda 會嘗試將取值*此*指標，也就是該點無效。 因此，從程式碼在處理常式 （或在協同程式的接續中） 會產生存取違規嘗試使用它。

> [!IMPORTANT]
> 如果您遇到像這樣的情況下，則您將需要思考的存留期*這個*物件;並擷取到*此*物件超越此擷取。 如果沒有，然後使用擷取它的強式或弱式參考資料，因為我們將示範下方。
>
> 或者&mdash;如果合理的案例中，且如果執行緒考量讓它甚至可行&mdash;則另一個選項是之後收件者完成事件，或在收件者解構函式中撤銷處理常式。 請參閱[撤銷已註冊的委派](handle-events.md#revoke-a-registered-delegate)。

這是我們要登錄處理常式的方式。

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Lambda 自動擷取任何區域變數的參考。 因此，如這個範例中，我們無法對等程式碼撰寫這。

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

在這兩種情況下，我們只要擷取原始*此*指標。 且具有不會影響參考計數，因此不會防止終結目前的物件。

### <a name="the-solution"></a>在方案

解決方案是將擷取的強式參考。 強式參考，*並*遞增參考計數，而且*會*保留持續執行目前的物件。 您只是宣告擷取變數 (稱為`strong_this`在此範例中)，並將它初始化[**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)，會擷取我們*這個*指標的強式參考呼叫。

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

您甚至可以省略自動擷取目前的物件，並透過擷取變數而不是透過隱含*這個*存取的資料成員。

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

如果強式參考不適當，您可以改為呼叫[**implements:: get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)來擷取*此*的弱式參考。 只是確認，所以您仍然可以擷取從它的強式參考之前存取成員。

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

### <a name="if-you-use-a-member-function-as-a-delegate"></a>如果您使用成員函式做為委派

以及 lambda 函式，這些原則也適用於使用成員函式做為委派。 語法不同，因此讓我們看看一些程式碼。 首先，以下是可能不安全的成員函式事件處理常式中，使用原始*此*指標。

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

這是標準、 傳統的方式來參考的物件，並其成員函式。 若要讓這安全，您可以&mdash;自 Windows SDK 版本 10.0.17763.0 (Windows 10，版本 1809年)&mdash;建立強式或弱式參考某個點的處理常式註冊的位置。 在這個時候，事件收件者物件已知為仍在執行。

針對強式參考資料，只需呼叫[**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)取代原始*此*指標。 C + + /winrt 可確保產生的委派保存目前的物件的強式參考。

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

適用於弱式參考資料中，呼叫[**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)。 C + + /winrt 可確保產生的委派會保留弱式參考資料。 最後，並在幕後，此委派會嘗試解析一個強式將弱式參考，而且只呼叫成員函式，如果成功。

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>使用**SwapChainPanel::CompositionScaleChanged**的弱式參考資料範例

在此程式碼範例中，我們會使用[**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged)事件透過另一個弱式參考資料的圖例。 程式碼會登錄事件處理常式使用擷取收件者弱式參考資料的 lambda。

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

上方，我們可以看到正在使用的弱式參考資料。 一般情況下，很適合中斷循環參考。 例如，對於 XAML 型 UI 架構的原生實作&mdash;因為架構的歷史設計&mdash;的弱式參考機制，在 C + + /winrt 是才能處理循環參考。 XAML 以外，不過，您可能不需要使用弱式參考資料 (不是，沒有任何項目原本就特定 XAML 相關)。 而是您應該不用，能夠設計您的 C + + 中這類的方式來避免循環參考和弱式參考的 WinRT Api。 

針對您宣告的任何指定類型，當弱式參考是必要的，是否無法馬上看得出來是 C++/WinRT。 因此，C++/WinRT 自動在結構範本[**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 上提供弱式參考支援，您的 C++/WinRT 類型直接或間接從其中衍生。 它是使用付費，其中您不需要支付任何項目除非確實針對 [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) 查詢您的物件。 且您可以明確選擇[退出該支援](#opting-out-of-weak-reference-support)。

### <a name="code-examples"></a>程式碼範例
[**Winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref)結構範本是取得類別執行個體的弱式參考選項之一。

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

提供的一些其他強式參考仍然存在，[**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weakrefget-function) 呼叫遞增參考計數並將強式參考傳回給該呼叫者。

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
* [implements::get_weak function](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [winrt::make_weak 函式範本](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref 標記結構](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref 結構範本](/uwp/cpp-ref-for-winrt/weak-ref)
