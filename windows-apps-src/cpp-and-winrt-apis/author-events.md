---
description: 本主題示範如何撰寫包含引發事件的執行階段類別的 Windows 執行階段元件。 也示範使用元件和處理事件的應用程式。
title: 以 C++/WinRT 撰寫事件
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 撰寫, 事件
ms.localizationpriority: medium
ms.openlocfilehash: 66691b1cd75a27e683261c12b7a3056c53160079
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297620"
---
# <a name="author-events-in-cwinrt"></a>以 C++/WinRT 撰寫事件

本主題係根據 Windows 執行階段元件和使用中應用程式建置的，而[使用 C++/WinRT 的 Windows 執行階段元件](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)主題會向您示範建置方式。

以下是本主題新增的功能。
- 更新溫度計執行階段類別，以在其溫度低於冰點時引發事件。
- 更新使用溫度計執行階段類別的核心應用程式，使其能夠處理該事件。

> [!NOTE]
> 如需安裝和使用 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援) 的資訊，請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

> [!IMPORTANT]
> 如需一些基本概念和詞彙，以協助了解如何以 C++/WinRT 使用及撰寫執行階段類別，請參閱[使用 C++/WinRT 來使用 API](consume-apis.md) 和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="create-thermometerwrc-and-thermometercoreapp"></a>建立 **ThermometerWRC** 和 **ThermometerCoreApp**

如果您想要遵循本主題所示的更新，讓您可以建置和執行程式碼，則第一個步驟是遵循[使用 C++/WinRT 的 Windows 執行階段元件](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)主題中的逐步解說。 如此一來，您將會有 **ThermometerWRC** Windows 執行階段元件，以及使用其的 **ThermometerCoreApp** 核心應用程式。

## <a name="update-thermometerwrc-to-raise-an-event"></a>更新 **ThermometerWRC** 以引發事件

更新 `Thermometer.idl`，使其看起來像下面的清單。 這是使用單精確度浮點數的引數宣告事件的方式，而此事件的委派類型為 [**EventHandler**](/uwp/api/windows.foundation.eventhandler-1)。

```idl
// Thermometer.idl
namespace ThermometerWRC
{
    runtimeclass Thermometer
    {
        Thermometer();
        void AdjustTemperature(Single deltaFahrenheit);
        event Windows.Foundation.EventHandler<Single> TemperatureIsBelowFreezing;
    };
}
```

儲存檔案。 專案不會以目前狀態完成建置，但在任何情況下都會立即執行建置，以產生 `\ThermometerWRC\ThermometerWRC\Generated Files\sources\Thermometer.h` 和 `Thermometer.cpp` Stub 檔案的更新版本。 在這些檔案中，您現在可以看到 **TemperatureIsBelowFreezing** 事件的虛設常式實作。 在 C++/WinRT 中，IDL 宣告的事件實作為一組的多載函式 (與將屬性實作為一對多載的 get 和 set 函式類似)。 一個多載會接受將註冊的委派，並傳回預付碼 ([**winrt::event_token**](/uwp/cpp-ref-for-winrt/event-token))。 其他則接收預付碼並撤銷相關的委派。

現在開啟 `Thermometer.h` 和 `Thermometer.cpp`，並更新 **Thermometer** 執行階段類別的實作。 在 `Thermometer.h` 中，新增兩個多載的 **TemperatureIsBelowFreezing** 函式，以及要在實作那些函式時使用的私人事件資料成員。

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...
        winrt::event_token TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<float> const& handler);
        void TemperatureIsBelowFreezing(winrt::event_token const& token) noexcept;

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_temperatureIsBelowFreezingEvent;
        ...
    };
}
...
```

如您所見，事件是由 [**winrt::event**](/uwp/cpp-ref-for-winrt/event) 結構範本表示，由特定委派類型參數化 (其本身可由 args 類型參數化)。

在 `Thermometer.cpp` 中，實作兩個多載的 **TemperatureIsBelowFreezing** 函式。

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    winrt::event_token Thermometer::TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_temperatureIsBelowFreezingEvent.add(handler);
    }

    void Thermometer::TemperatureIsBelowFreezing(winrt::event_token const& token) noexcept
    {
        m_temperatureIsBelowFreezingEvent.remove(token);
    }

    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
        if (m_temperatureFahrenheit < 32.f) m_temperatureIsBelowFreezingEvent(*this, m_temperatureFahrenheit);
    }
}
```

> [!NOTE]
> 如需有關事件撤銷的詳細資訊，請參閱[撤銷已註冊的委派](handle-events.md#revoke-a-registered-delegate)。 您可以免費為您的事件取得自動事件撤銷實作。 換句話說，您不需要實作事件撤銷的多載&mdash;C++/WinRT 投影會為您提供。

其他多載 (註冊和手動撤銷多載) 則「不會」在投影中內建。 這是為了讓您能夠彈性地以最佳方式在案例中進行實作。 呼叫 [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) 和 [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function)(如同這些實作所示) 是有效率且並行/執行緒安全的預設值。 但若您有大量的事件，則您可能不想要每一個都有事件欄位，而寧願改用一些疏鬆的實作。

您也可以從上面看到，**AdjustTemperature** 函式的實作已更新為會在溫度低於冰點時引發 **TemperatureIsBelowFreezing** 事件。

## <a name="update-thermometercoreapp-to-handle-the-event"></a>更新 **ThermometerCoreApp** 以處理事件

在 **ThermometerCoreApp** 專案的 `App.cpp` 中，對程式碼進行下列變更以註冊事件處理常式，然後讓溫度低於冰點。

`WINRT_ASSERT` 是巨集定義，而且會發展為 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_thermometer.TemperatureIsBelowFreezing([](const auto &, float temperatureFahrenheit)
        {
            WINRT_ASSERT(temperatureFahrenheit < 32.f); // Put a breakpoint here.
        });
    }
    ...

    void Uninitialize()
    {
        m_thermometer.TemperatureIsBelowFreezing(m_eventToken);
    }
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(-1.f);
        ...
    }
    ...
};
```

請留意 **OnPointerPressed** 方法的變更。 現在，每當您按一下視窗時，就會讓溫度計的溫度*減去*華氏 1 度。 現在，應用程式正在處理溫度低於冰點時所引發的事件。 若要示範事件如預期般引發，請將中斷點放在正在處理 **TemperatureIsBelowFreezing** 事件的 lambda 運算式內、執行應用程式，然後在視窗中按一下。

## <a name="parameterized-delegates-across-an-abi"></a>透過 ABI 的參數化委派

如果您的事件必須可透過應用程式二進位介面 (ABI) 存取&mdash;例如界於元件與其取用的應用程式之間&mdash;則您的事件必須使用 Windows 執行階段委派類型。 上述範例使用 [**Windows::Foundation::EventHandler\<T\>** ](/uwp/api/windows.foundation.eventhandler) Windows 執行階段委派類型。 [**TypedEventHandler\<TSender, TResult\>** ](/uwp/api/windows.foundation.eventhandler) 是 Windows 執行階段委派類型的另一個範例。

這兩個委派類型的類型參數必須透過 ABI，因此類型參數也必須是 Windows 執行階段類型。 包含 Windows 執行階段類別、第三方執行階段類別，以及數字和字串等基本類型。 如果您忘記此限制的話，編譯器會協助您解決「T 必須為 WinRT 類型」錯誤。

以下是程式代碼清單形式的範例。 從您稍早在本主題中建立的 **ThermometerWRC** 和 **ThermometerCoreApp** 專案開始，編輯這些專案中的程式碼，使其看起來像這些清單中的程式碼。

第一個清單是 **ThermometerWRC** 專案的清單。 如下所示編輯 `ThermometerWRC.idl` 之後，請建立專案，然後將 `MyEventArgs.h` 和 `.cpp` 複製到專案中 (從 `Generated Files` 資料夾)，就如同您先前使用 `Thermometer.h` 和 `.cpp` 所做的一樣。

```cppwinrt
// ThermometerWRC.idl
namespace ThermometerWRC
{
    [default_interface]
    runtimeclass MyEventArgs
    {
        Single TemperatureFahrenheit{ get; };
    }

    [default_interface]
    runtimeclass Thermometer
    {
        ...
        event Windows.Foundation.EventHandler<ThermometerWRC.MyEventArgs> TemperatureIsBelowFreezing;
        ...
    };
}

// MyEventArgs.h
#pragma once
#include "MyEventArgs.g.h"

namespace winrt::ThermometerWRC::implementation
{
    struct MyEventArgs : MyEventArgsT<MyEventArgs>
    {
        MyEventArgs() = default;
        MyEventArgs(float temperatureFahrenheit);
        float TemperatureFahrenheit();

    private:
        float m_temperatureFahrenheit{ 0.f };
    };
}

// MyEventArgs.cpp
#include "pch.h"
#include "MyEventArgs.h"
#include "MyEventArgs.g.cpp"

namespace winrt::ThermometerWRC::implementation
{
    MyEventArgs::MyEventArgs(float temperatureFahrenheit) : m_temperatureFahrenheit(temperatureFahrenheit)
    {
    }

    float MyEventArgs::TemperatureFahrenheit()
    {
        return m_temperatureFahrenheit;
    }
}

// Thermometer.h
...
struct Thermometer : ThermometerT<Thermometer>
{
...
    winrt::event_token TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs> const& handler);
...
private:
    winrt::event<Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs>> m_temperatureIsBelowFreezingEvent;
...
}
...

// Thermometer.cpp
#include "MyEventArgs.h"
...
winrt::event_token Thermometer::TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs> const& handler) { ... }
...
void Thermometer::AdjustTemperature(float deltaFahrenheit)
{
    m_temperatureFahrenheit += deltaFahrenheit;

    if (m_temperatureFahrenheit < 32.f)
    {
        auto args = winrt::make_self<winrt::ThermometerWRC::implementation::MyEventArgs>(m_temperatureFahrenheit);
        m_temperatureIsBelowFreezingEvent(*this, *args);
    }
}
...
```

這個清單是 **ThermometerCoreApp** 專案的清單。

```cppwinrt
// App.cpp
...
void Initialize(CoreApplicationView const&)
{
    m_eventToken = m_thermometer.TemperatureIsBelowFreezing([](const auto&, ThermometerWRC::MyEventArgs args)
    {
        float degrees = args.TemperatureFahrenheit();
        WINRT_ASSERT(degrees < 32.f); // Put a breakpoint here.
    });
}
...
```

## <a name="simple-signals-across-an-abi"></a>跨 ABI 的簡單訊號

如果您不需要透過您的事件傳遞任何參數或與引數，則可以定義自己的簡單 Windows 執行階段委派類型。 以下範例顯示 **Thermometer** 執行階段類別的簡易版本。 它會宣告名為 **SignalDelegate** 的委派類型，然後使用該委派類型來引發訊號類型事件，而不是具有參數的事件。

```idl
// ThermometerWRC.idl
namespace ThermometerWRC
{
    delegate void SignalDelegate();

    runtimeclass Thermometer
    {
        Thermometer();
        event ThermometerWRC.SignalDelegate SignalTemperatureIsBelowFreezing;
        void AdjustTemperature(Single value);
    };
}
```

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...

        winrt::event_token SignalTemperatureIsBelowFreezing(ThermometerWRC::SignalDelegate const& handler);
        void SignalTemperatureIsBelowFreezing(winrt::event_token const& token);
        void AdjustTemperature(float deltaFahrenheit);

    private:
        winrt::event<ThermometerWRC::SignalDelegate> m_signal;
        float m_temperatureFahrenheit{ 0.f };
    };
}
```

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    winrt::event_token Thermometer::SignalTemperatureIsBelowFreezing(ThermometerWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void Thermometer::SignalTemperatureIsBelowFreezing(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
        if (m_temperatureFahrenheit < 32.f)
        {
            m_signal();
        }
    }
}
```

```cppwinrt
// App.cpp
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    ThermometerWRC::Thermometer m_thermometer;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_thermometer.SignalTemperatureIsBelowFreezing([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_thermometer.SignalTemperatureIsBelowFreezing(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(-1.f);
        ...
    }
    ...
};
```

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>參數化委派、簡單訊號，以及專案中的回呼

如果您需要 Visual Studio 專案內部的事件 (而非跨二進位檔)，而這些事件不限於 Windows 執行階段類型，則您仍可使用 [**winrt::event**](/uwp/cpp-ref-for-winrt/event)\<Delegate\> 類別範本。 只要使用 [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate)，而不是實際的 Windows 執行階段委派類型，因為 **winrt::delegate** 也支援非 Windows 執行階段參數。

下列範例先顯示不採用任何參數的委派簽章 (基本上是簡單訊號)，而後顯示採用一個字串的委派簽章。

```cppwinrt
winrt::event<winrt::delegate<>> signal;
signal.add([] { std::wcout << L"Hello, "; });
signal.add([] { std::wcout << L"World!" << std::endl; });
signal();

winrt::event<winrt::delegate<std::wstring>> log;
log.add([](std::wstring const& message) { std::wcout << message.c_str() << std::endl; });
log.add([](std::wstring const& message) { Persist(message); });
log(L"Hello, World!");
```

請注意，如何才能將如您想要的多個訂閱委派新增至事件。 不過，事件還有一些相關聯的額外負荷。 如果您只需要僅含單一訂閱委派的簡單回呼，則可以使用 [**winrt::delegate&lt;...T&gt;** ](/uwp/cpp-ref-for-winrt/delegate)本身。

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

如果您正從在內部使用事件和委派的 C++/CX 程式碼基底進行移植，則 **winrt::delegate**會協助您在 C++/WinRT 中複寫該模式。

## <a name="design-guidelines"></a>設計指導方針

我們建議您將事件 (不是委派) 當作函式參數傳遞。 [**winrt::event**](/uwp/cpp-ref-for-winrt/event)的 **add** 函式是一個例外狀況，因為您必須在該情況下傳遞委派。 此指導方針的原因是因為委派可以在不同的 Windows 執行階段語言中採用不同的形式 (就它們支援一個或多個用戶端註冊而論)。 事件與其多個訂閱者模型，構成一個更容易預測且一致的選項。

事件處理常式委派的簽章應該包含兩個參數：*sender* (**IInspectable**) 和 *args* (某些事件引數類型，例如 [**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs))。

請注意，如果您要設計內部 API，不一定要套用這些指導方針。 雖然經過一段時間，內部 API 通常會變成公開的。

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 撰寫 API](author-apis.md)
* [使用 C++/WinRT 取用 API](consume-apis.md)
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)
* [使用 C++/WinRT 的 Windows 執行階段元件](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)