---
description: 本主題示範如何撰寫包含引發事件的執行階段類別的 Windows 執行階段元件。 也示範使用元件和處理事件的應用程式。
title: 在 C++/WinRT 中撰寫事件
ms.date: 07/18/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, author, event, 標準, 投影, 撰寫, 事件
ms.localizationpriority: medium
ms.openlocfilehash: 5c410d209972a0221928548901f79bd599c67eae
ms.sourcegitcommit: c315ec3e17489aeee19f5095ec4af613ad2837e1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/04/2019
ms.locfileid: "58921694"
---
# <a name="author-events-in-cwinrt"></a>在 C++/WinRT 中撰寫事件

本主題示範如何撰寫一個 Windows 執行階段元件，其包含一個執行階段類別代表銀行帳戶，當餘額進入借方時，引發一個事件。 也示範一個核心應用程式，其使用銀行帳戶執行階段類別，呼叫調整餘額的函式，並處理所造成的任何事件。

> [!NOTE]
> 如需安裝和使用的資訊[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Visual Studio 擴充功能 (VSIX) 和 NuGet 套件 （其同時提供專案範本，並建置支援），請參閱[Visual Studio 支援C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

> [!IMPORTANT]
> 如需支援您了解如何使用 C++/WinRT 使用及撰寫執行階段類別的基本概念和詞彙，請查閱[使用 C++/WinRT 使用API](consume-apis.md)和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>建立 Windows 執行階段元件 (BankAccountWRC)

在 Microsoft Visual Studio 中，從建立新的專案開始。 建立**Visual C++**   >  **Windows Universal** > **Windows 執行階段元件 (C++/WinRT)** 專案，並將它命名*BankAccountWRC* （適用於 「 銀行帳戶為 Windows 執行階段元件 」）。

新建立的專案中包含一個名為 `Class.idl` 的檔案。 重新命名該檔案`BankAccount.idl`(重新命名`.idl`檔案會自動重新命名相依`.h`和`.cpp`太檔案)。 內容取代`BankAccount.idl`與下列清單。

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

儲存檔案。 專案不會建置目前的完成，但現在建置是有用的事情，因為它會產生您將實作的原始程式碼檔**BankAccount**執行階段類別。 因此請繼續並立即建置 (組建錯誤，您可以預期看到在這個階段有如何處理`Class.h`和`Class.g.h`找不到)。 在建置過程`midl.exe`工具會執行以建立元件的 Windows 執行階段中繼資料檔案 (也就是`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`)。 然後，執行 `cppwinrt.exe` 工具 (有 `-component` 選項) 產生原始碼檔案在撰寫您的元件中支援您。 這些檔案包含可協助您開始實作 stub **BankAccount**您在您的 IDL 中宣告的執行階段類別。 這些虛設常式為 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` 與 `BankAccount.cpp`。

以滑鼠右鍵按一下專案節點，然後按一下 [**在檔案總管] 中開啟資料夾**。 這會開啟檔案總管 中的專案資料夾。 Stub 檔案，複製`BankAccount.h`並`BankAccount.cpp`從資料夾`\BankAccountWRC\BankAccountWRC\Generated Files\sources\`並包含您的專案檔案的資料夾，也就是`\BankAccountWRC\BankAccountWRC\`，目的地中的檔案取代。 現在，我們開啟 `BankAccount.h` 與 `BankAccount.cpp` 並實作我們的執行階段類別。 在 `BankAccount.h` 中，將兩個私用成員新增至 (*不*是原廠實作) BankAccount 的實作。

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

如上文所見，此事件實作[ **winrt::event** ](/uwp/cpp-ref-for-winrt/event)結構的範本，由特定的委派型別參數化。

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

如果任何警告會使您無法建置，然後解決這些問題，或設定專案屬性**C /C++** > **一般** > **警告視為錯誤**要**否 (/ WX-)**，並再次建置專案。

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>建立核心應用程式 (BankAccountCoreApp) 測試 Windows 執行階段元件

現在建立新的專案 (在您的 `BankAccountWRC` 解決方案中，或在新的一個裡)。 建立**Visual C++**   >  **Windows Universal** > **Core 應用程式 (C++/WinRT)** 專案，並將它命名*BankAccountCoreApp*。

加入參考，並瀏覽至`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`（或加入專案對專案參考，如果位於相同方案中的兩個專案）。 按一下 \[新增\]，然後 **\[確定\]**。 現在建置 BankAccountCoreApp。 您會看到錯誤的罕見事件中，將內容檔案`readme.txt`不存在，從 Windows 執行階段元件專案中排除該檔案，請重建它然後重建 BankAccountCoreApp。

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

每一次您按一下視窗，從銀行帳戶餘額減 1。 若要示範如預期般運作，所引發的事件，將會處理在 lambda 運算式內中斷點放**AccountIsInDebit**事件，執行應用程式，並在視窗內按一下。

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>參數化的委派，以及簡單的訊號，跨 ABI

如果您的事件必須可存取應用程式二進位介面 (ABI) 之間&mdash;這類元件和其取用的應用程式之間&mdash;則您的事件必須使用 Windows 執行階段委派型別。 上述範例使用[ **Windows::Foundation::EventHandler\<T\>**  ](/uwp/api/windows.foundation.eventhandler) Windows 執行階段委派型別。 [**TypedEventHandler\<，TResult\>**  ](/uwp/api/windows.foundation.eventhandler)是 Windows 執行階段委派類型的另一個範例。

跨 ABI，因此型別參數必須是 Windows 執行階段類型，也不必針對兩個委派類型的型別參數。 包含第一個和第三方執行階段類別，以及數字和字串等基本類型。 編譯器可協助您使用 「*必須是 WinRT 型別*」 錯誤，如果您忘記該條件約束。

如果您不需要任何參數或與您的事件引數傳遞，您可以定義您自己的簡單 Windows 執行階段委派類型。 下列範例示範的簡易版本**BankAccount**執行階段類別。 它會宣告名為委派類型**SignalDelegate**然後使用所引發的訊號類型事件，而不是事件的參數。

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

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>參數化的委派、 簡單的訊號和在專案內的回撥

如果您的事件只有在內部會在您C++/WinRT 專案 （不是在二進位檔），則您仍然可以使用[ **winrt::event** ](/uwp/cpp-ref-for-winrt/event)結構的範本，但您將它參數化與C++/WinRT 的非 Windows Runtime [ **winrt::delegate&lt;...T&gt;**  ](/uwp/cpp-ref-for-winrt/delegate)結構的範本，也就是有效的、 參考計數的委派。 它支援任意數目的參數，而且它們不一定要 Windows 執行階段類型。

下列範例會先顯示委派簽章不採用任何參數 （基本上是簡單信號），以及採用字串一。

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

請注意如何加入事件多個訂閱委派，您希望的位置。 不過，還有一些與事件相關聯的額外負荷。 如果您只需要是簡單的回呼，只有單一訂閱委派，則您可以使用[ **winrt::delegate&lt;...T&gt;**  ](/uwp/cpp-ref-for-winrt/delegate)本身。

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

如果您要移植從C++/CX 程式碼基底位置事件與委派會在內部使用在專案中，然後**winrt::delegate**可協助您在複寫中的該模式C++/WinRT。

## <a name="design-guidelines"></a>設計指導方針

我們建議您傳遞的事件，並不是委派，做為函式參數。 **新增**函式[ **winrt::event** ](/uwp/cpp-ref-for-winrt/event)是一個例外狀況，因為您必須在此情況下傳遞委派。 這項指導方針的原因是因為委派可以有不同的形式在不同的 Windows 執行階段語言 （根據它們是否支援一部用戶端註冊或多個） 之間。 事件，但其多個 「 訂閱者 」 模型，構成更容易預測且一致的選項。

事件處理常式委派的簽章應該包含兩個參數：*寄件者*(**IInspectable**)，並*args* (某些事件引數類型，例如[ **RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs))。

請注意，是否您在設計內部 API，不一定會套用這些指導方針。 雖然內部 Api 通常會變成公用經過一段時間。

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 撰寫 API](author-apis.md)
* [使用 C++/WinRT 來使用 API](consume-apis.md)
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)
