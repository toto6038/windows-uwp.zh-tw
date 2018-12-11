---
description: 本主題示範如何撰寫包含引發事件的執行階段類別的 Windows 執行階段元件。 也示範使用元件和處理事件的應用程式。
title: '在 C++/WinRT 中撰寫事件 '
ms.date: 07/18/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, author, event, 標準, 投影, 撰寫, 事件
ms.localizationpriority: medium
ms.openlocfilehash: 31f076ca259d10cc5bd49daea66741ead6e117c2
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2018
ms.locfileid: "8874185"
---
# <a name="author-events-in-cwinrt"></a>在 C++/WinRT 中撰寫事件 

本主題示範如何撰寫一個 Windows 執行階段元件，其包含一個執行階段類別代表銀行帳戶，當餘額進入借方時，引發一個事件。 也示範一個核心應用程式，其使用銀行帳戶執行階段類別，呼叫調整餘額的函式，並處理所造成的任何事件。

> [!NOTE]
> 如需有關安裝和使用資訊[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Visual Studio 擴充功能 (VSIX) (提供專案範本的支援，以及 C + + /winrt MSBuild 屬性和目標) 看到[Visual Studio 支援 C + + /winrt，以及 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

> [!IMPORTANT]
> 如需支援您了解如何使用 C++/WinRT 使用及撰寫執行階段類別的基本概念和詞彙，請查閱[使用 C++/WinRT 使用API](consume-apis.md)和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>建立 Windows 執行階段元件 (BankAccountWRC)

在 Microsoft Visual Studio 中，藉由建立新的專案來開始。 建立**Visual c + +** > **Windows 通用** > **Windows 執行階段元件 (C + + /winrt)** 專案，並將它命名為*BankAccountWRC* （適用於 「 銀行帳戶 Windows 執行階段元件 」）。

新建立的專案中包含一個名為 `Class.idl` 的檔案。 重新命名檔案`BankAccount.idl`(重新命名`.idl`檔案，自動重新命名相依於`.h`和`.cpp`檔案、 太)。 內容取代成`BankAccount.idl`具有下列清單。

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

儲存檔案。 將不會完成時刻，來建置專案，但現在建置很有用，是因為它會產生原始碼檔案，您將會在其中實作**BankAccount**執行階段類別。 因此根源和現在建置 (若要查看在這個階段，您可以預期的建置錯誤都與`Class.h`和`Class.g.h`找不到)。 在建置過程中，`midl.exe`工具建立元件的 Windows 執行階段中繼資料檔案執行時 (也就是`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`)。 然後，執行 `cppwinrt.exe` 工具 (有 `-component` 選項) 產生原始碼檔案在撰寫您的元件中支援您。 這些檔案包含虛設常式，可協助您開始實作您在 IDL 中宣告的**BankAccount**執行階段類別。 這些虛設常式為 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` 與 `BankAccount.cpp`。

在檔案總管] 中，複製的虛設常式檔案`BankAccount.h`和`BankAccount.cpp`資料夾從`\BankAccountWRC\BankAccountWRC\Generated Files\sources\`到包含您的專案檔案的資料夾，也就是`\BankAccountWRC\BankAccountWRC\`，並取代目的地中的檔案。 現在，我們開啟 `BankAccount.h` 與 `BankAccount.cpp` 並實作我們的執行階段類別。 在 `BankAccount.h` 中，將兩個私用成員新增至 (*不*是原廠實作) BankAccount 的實作。

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

您可以看到上方，事件會實作而言[**winrt::event**](/uwp/cpp-ref-for-winrt/event)結構範本，依特定委派類型參數化。

在`BankAccount.cpp` 中，如下列程式碼範例所示實作函式。 在 C++/WinRT 中，IDL 宣告的事件實作為一組的多載函式 (與將屬性實作為一對多載的 get 和 set 函式類似)。 一個多載會接受將註冊的委派，並傳回預付碼。 其他則接收預付碼並撤銷相關的委派。

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(winrt::event_token const& token)
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

您不需要實作事件撤銷的多載 (如需詳細資訊，請參閱[撤銷已註冊的委派](handle-events.md#revoke-a-registered-delegate))&mdash;透過 C++/WinRT 投影為您處理。 為了提供您彈性以針對您的案例進行最佳實作，其他多載不會納入投影。 呼叫 [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) 和 [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function)，如同這是有效率且並行/執行緒安全的預設值。 但若您有大量的事件，則您可能不想要每一個都有事件欄位，而寧願改用一些疏鬆的實作。

如果餘額為負數的話，您也可能看到以上 **AdjustBalance** 函式的實作引發 **AccountIsInDebit** 事件。

如果任何警告妨礙您建置，然後先解決這些問題或將專案屬性**C/c + +** > **一般** > **警告為錯誤**以**否 (/ /wx-)**，並再次建置專案。

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>建立核心應用程式 (BankAccountCoreApp) 測試 Windows 執行階段元件

現在建立新的專案 (在您的 `BankAccountWRC` 解決方案中，或在新的一個裡)。 建立**Visual c + +** > **Windows 通用** > **核心應用程式 (C + + /winrt)** 專案，並將它命名為*BankAccountCoreApp*。

新增參考資料，並瀏覽至`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`（或新增專案對專案參照，如果兩個專案在同一個方案中）。 按一下 \[新增\]****，然後 **\[確定\]**。 現在建置 BankAccountCoreApp。 萬一您看到錯誤的承載檔案`readme.txt`不存在，從 Windows 執行階段元件專案排除該檔案，重建它，然後重建 BankAccountCoreApp。

在建置程序期間，執行 `cppwinrt.exe` 工具將被參考的 `.winmd` 檔案處理到包含投影類型的原始碼檔案中，在使用元件裡支援您。 適用於您元件執行階段類別的投影類型標頭&mdash;命名為`BankAccountWRC.h`&mdash;在資料夾`\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\`中產生。

在 `App.cpp` 中包含該標頭。

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

也在 `App.cpp` 中，新增下列程式碼起始一個 BankAccount  (使用投影類型的預設建構函式)，註冊事件處理常式，然後導致帳戶進入借方。

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
            WINRT_ASSERT(balance < 0.f);
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

每一次您按一下視窗，從銀行帳戶餘額減 1。 若要示範如預期般運作，會被引發的事件，放置中斷點至於 lambda 運算式正在處理**AccountIsInDebit**事件、 執行應用程式，並在視窗中按一下。

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>參數化的委派和簡單的訊號，跨 ABI

如果您的事件必須可以透過應用程式二進位介面 (ABI)&mdash;類元件和其使用的應用程式&mdash;，然後您的事件必須使用 Windows 執行階段委派類型。 上述範例使用[**Windows::Foundation::EventHandler\<T\ >**](/uwp/api/windows.foundation.eventhandler) Windows 執行階段委派類型。 [**TypedEventHandler\<TSender、 TResult\ >**](/uwp/api/windows.foundation.eventhandler)是 Windows 執行階段委派類型的另一個範例。

這些兩個委派類型的類型參數必須跨 ABI 類型參數必須是 Windows 執行階段類型，也因此。 這包括第一方和第三方執行階段類別，以及基本類型，例如數字和字串。 編譯器可協助您與 「*必須為 WinRT 類型*」 錯誤如果您忘記密碼該限制。

如果您不需要通過任何參數或引數與您的事件，您可以定義自己簡單的 Windows 執行階段委派類型。 下列範例顯示簡單**BankAccount**執行階段類別的版本。 它宣告名為**SignalDelegate**的委派類型，並接著它會使用，來引發訊號類型事件，而不是含有參數的事件。

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

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>參數化的委派、 簡單的訊號，以及在專案中的回呼

如果您的事件只在內部使用內您 C + + WinRT 專案 （並非所有二進位檔案），則您仍然使用[**winrt::event**](/uwp/cpp-ref-for-winrt/event)結構範本，但參數化使用 C + + /winrt 的非 Windows Runtime [**winrt:: delegate&lt;...T&gt;**](/uwp/cpp-ref-for-winrt/delegate)結構範本，也就是有效率、 計算參考次數的委派。 它支援任何數量的參數，而且它們不受限於 Windows 執行階段類型。

下列範例會先顯示委派不接受 （基本上是簡單的訊號），任何參數的簽章，然後的另一個採用字串。

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

請注意如何您可以新增到事件為您想要的多個訂閱委派。 不過，還有一些與事件相關聯的額外負荷。 如果您只需要簡單回呼使用只有單一訂閱委派，則您可以使用[**winrt:: delegate&lt;...T&gt;**](/uwp/cpp-ref-for-winrt/delegate)本身。

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

如果您正在移植從 C + + /CX 程式碼基底其中事件和委派在內部使用在專案中，則**winrt:: delegate**可協助您中複寫該模式在 C + + /winrt。

## <a name="design-guidelines"></a>設計指導方針

我們建議您做為函式參數傳遞事件，以及不委派。 **新增**的功能[**winrt::event**](/uwp/cpp-ref-for-winrt/event)是一個例外，因為您必須在此情況下傳遞委派。 此指導方針的原因是因為委派可以採取不同形式跨不同的 Windows 執行階段語言 （而言，它們是否支援一部用戶端註冊或多個）。 事件，使用其多個訂閱者模型，構成更可預測且一致的選項。

事件處理常式委派的簽章應該包含兩個參數：*寄件者*(**IInspectable**) 和*引數*（某些事件引數類型，例如[**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)）。

請注意，是否您正在設計內部的 API，不一定會套用這些指導方針。 雖然內部 Api 通常成為公用隨著時間。

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 撰寫 API ](author-apis.md)
* [使用 C++/WinRT 使用 API ](consume-apis.md)
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)
