---
description: 本主題示範如何撰寫包含引發事件的執行階段類別的 Windows 執行階段元件。 也示範使用元件和處理事件的應用程式。
title: 以 C++/WinRT 撰寫事件
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 撰寫, 事件
ms.localizationpriority: medium
ms.openlocfilehash: 4499b2191734c6ae66131ce92aa2654188313d5e
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270181"
---
# <a name="author-events-in-cwinrt"></a>以 C++/WinRT 撰寫事件

本主題示範如何撰寫一個 Windows 執行階段元件，其包含一個執行階段類別代表銀行帳戶，當餘額進入借方時，引發一個事件。 也示範一個核心應用程式，其使用銀行帳戶執行階段類別，呼叫調整餘額的函式，並處理所造成的任何事件。

> [!NOTE]
> 如需安裝和使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援) 的資訊，請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

> [!IMPORTANT]
> 如需支援您了解如何使用 C++/WinRT 使用及撰寫執行階段類別的基本概念和詞彙，請參閱[使用 C++/WinRT 使用 API](consume-apis.md) 和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>建立 Windows 執行階段元件 (BankAccountWRC)

在 Microsoft Visual Studio 中，從建立新的專案開始。 建立 **Windows 執行階段元件 (C++/WinRT)** 專案，並將它命名為 *BankAccountWRC* (適用於「銀行帳戶 Windows 執行階段元件」)。 尚未建置專案。

新建立的專案中包含一個名為 `Class.idl` 的檔案。 將該檔案 `BankAccount.idl` 重新命名 (重新命名 `.idl` 檔案也會自動將相依的 `.h` 和 `.cpp` 檔案重新命名)。 以下面的清單取代 `BankAccount.idl` 的內容。

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        event Windows.Foundation.EventHandler<Single> AccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

儲存檔案。 此專案此刻不會建置完成，但立即建置是實用的做法，因為它會產生原始程式碼檔案，而您會在其中實作 **BankAccount** 執行階段類別。 因此請繼續並立即建置 (您可預期在這個階段看到的建置錯誤有找不到 `Class.h` 和 `Class.g.h`有關)。

在建置程序期間，執行 `midl.exe` 工具建立元件的 Windows 執行階段中繼資料檔案 (其為 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`)。 然後，執行 `cppwinrt.exe` 工具 (有 `-component` 選項) 產生原始碼檔案在撰寫您的元件中支援您。 這些檔案包含虛設常式，可協助您開始實作您在 IDL 中宣告的 **BankAccount** 執行階段類別。 這些虛設常式為 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` 與 `BankAccount.cpp`。

以滑鼠右鍵按一下專案節點，然後按一下 [在檔案總管中開啟資料夾]  。 這會在檔案總管中開啟專案資料夾。 然後，從 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` 資料夾將虛設常式檔案 `BankAccount.h` 和 `BankAccount.cpp` 複製到包含專案檔案的資料夾中，也就是 `\BankAccountWRC\BankAccountWRC\` 並取代目的地中的檔案。 現在，我們開啟 `BankAccount.h` 與 `BankAccount.cpp` 並實作我們的執行階段類別。 在 `BankAccount.h` 中，將兩個私用成員新增至 (「不是」  原廠實作) BankAccount 的實作。

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_accountIsInDebitEvent;
        float m_balance{ 0.f };
    };
}
...
```

如上文所見，事件會依據 [**winrt::event**](/uwp/cpp-ref-for-winrt/event) 結構範本實作，並由特定的委派類型參數化。

在 `BankAccount.cpp` 中，如下列程式碼範例所示實作函式。 在 C++/WinRT 中，IDL 宣告的事件實作為一組的多載函式 (與將屬性實作為一對多載的 get 和 set 函式類似)。 一個多載會接受將註冊的委派，並傳回預付碼。 其他則接收預付碼並撤銷相關的委派。

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(winrt::event_token const& token) noexcept
    {
        m_accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f) m_accountIsInDebitEvent(*this, m_balance);
    }
}
```

> [!NOTE]
> 如需有關事件撤銷的詳細資訊，請參閱[撤銷已註冊的委派](handle-events.md#revoke-a-registered-delegate)。 您可以免費為您的事件取得自動事件撤銷實作。 換句話說，您不需要實作事件撤銷的多載&mdash;C++/WinRT 投影會為您提供。

其他多載 (註冊和手動撤銷多載) 則「不會」  在投影中內建。 這是為了讓您能夠彈性地以最佳方式在案例中進行實作。 呼叫 [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) 和 [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function)(如同這些實作所示) 是有效率且並行/執行緒安全的預設值。 但若您有大量的事件，則您可能不想要每一個都有事件欄位，而寧願改用一些疏鬆的實作。

如果餘額為負數的話，您也可能看到以上 **AdjustBalance** 函式的實作引發 **AccountIsInDebit** 事件。

如果有任何警告妨礙您建置，則加以解決或將專案屬性 **C/C++**  > **一般** > **視警告為錯誤** 設定為 **No (/WX-)** ，並再試一次建置專案。

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>建立核心應用程式 (BankAccountCoreApp) 測試 Windows 執行階段元件

現在建立新的專案 (在您的 `BankAccountWRC` 解決方案中，或在新的一個裡)。 建立**核心應用程式 (C++/WinRT)** 專案，並將它命名為 *BankAccountCoreApp*。

新增參考，並瀏覽至 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (或者如果有兩個專案在相同的方案中，新增專案對專案參考)。 按一下 [新增]  ，然後按一下 [確定]  。 現在建置 BankAccountCoreApp。 如果您看到承載檔案 `readme.txt` 不存在的錯誤，在此不可能發生的事件中，請從 Windows 執行階段元件專案排除該檔案，重建它，然後重建 BankAccountCoreApp。

在建置程序期間，執行 `cppwinrt.exe` 工具將被參考的 `.winmd` 檔案處理到包含投影類型的原始碼檔案中，在使用元件裡支援您。 適用於您元件執行階段類別的投影類型標頭 &mdash;名為`BankAccountWRC.h`&mdash;會在資料夾 `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\` 中產生。

在 `App.cpp` 中包含該標頭。

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

也在 `App.cpp` 中，新增下列程式碼起始一個 BankAccount (使用投影類型的預設建構函式)，註冊事件處理常式，然後導致帳戶進入借方。

`WINRT_ASSERT` 是巨集定義，而且會發展為 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            WINRT_ASSERT(balance < 0.f); // Put a breakpoint here.
        });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.AccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

每一次您按一下視窗，從銀行帳戶餘額減 1。 若要示範事件如預期般引發，請將中斷點放在正在處理 **AccountIsInDebit** 事件的 lambda 運算式內、執行應用程式，然後在視窗中按一下。

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>透過 ABI 的參數化委派，以及簡單訊號

如果您的事件必須可透過應用程式二進位介面 (ABI) 存取&mdash;例如界於元件與其取用的應用程式之間&mdash;則您的事件必須使用 Windows 執行階段委派類型。 上述範例使用 [**Windows::Foundation::EventHandler\<T\>** ](/uwp/api/windows.foundation.eventhandler) Windows 執行階段委派類型。 [**TypedEventHandler\<TSender, TResult\>** ](/uwp/api/windows.foundation.eventhandler) 是 Windows 執行階段委派類型的另一個範例。

這兩個委派類型的類型參數必須透過 ABI，因此類型參數也必須是 Windows 執行階段類型。 包含第一和第三方執行階段類別，以及數字和字串等基本類型。 如果您忘記此限制的話，編譯器會協助您解決「必須是 WinRT 類型」  的錯誤。

如果您不需要透過您的事件傳遞任何參數或與引數，則可以定義自己的簡單 Windows 執行階段委派類型。 以下範例顯示 **BankAccount** 執行階段類別的簡易版本。 它會宣告名為 **SignalDelegate** 的委派類型，然後使用該委派類型來引發訊號類型事件，而不是具有參數的事件。

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    delegate void SignalDelegate();

    runtimeclass BankAccount
    {
        BankAccount();
        event BankAccountWRC.SignalDelegate SignalAccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

        winrt::event_token SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler);
        void SignalAccountIsInDebit(winrt::event_token const& token);
        void AdjustBalance(float value);

    private:
        winrt::event<BankAccountWRC::SignalDelegate> m_signal;
        float m_balance{ 0.f };
    };
}
```

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void BankAccount::SignalAccountIsInDebit(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f)
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
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.SignalAccountIsInDebit([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.SignalAccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>參數化委派、簡單訊號，以及專案中的回呼

如果您的事件只在 C++/WinRT 專案內部使用 (不會非跨越二進位檔)，您仍可使用 [**winrt::event**](/uwp/cpp-ref-for-winrt/event) 結構範本，但您會透過 C++/WinRT 的非 Windows 執行階段 [**winrt::delegate&lt;...T&gt;** ](/uwp/cpp-ref-for-winrt/delegate) 結構範本 (也就是有效、計算參考次數的委派) 將它參數化。 其支援任意多個參數，而且不限於 Windows 執行階段類型。

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

事件處理常式委派的簽章應該包含兩個參數：*sender* (**IInspectable**) 和 *args* (某些事件引數類型，例如 [ **RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs))。

請注意，如果您要設計內部 API，不一定要套用這些指導方針。 雖然經過一段時間，內部 API 通常會變成公開的。

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 撰寫 API](author-apis.md)
* [使用 C++/WinRT 取用 API](consume-apis.md)
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)
