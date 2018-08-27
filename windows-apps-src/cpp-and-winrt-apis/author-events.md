---
author: stevewhims
description: 本主題示範如何撰寫包含引發事件的執行階段類別的 Windows 執行階段元件。 也示範使用元件和處理事件的應用程式。
title: '在 C++/WinRT 中撰寫事件 '
ms.author: stwhi
ms.date: 07/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, author, event, 標準, 投影, 撰寫, 事件
ms.localizationpriority: medium
ms.openlocfilehash: 3b52bf8e33bbf111dd02c695d8c3baf77e1338ac
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2018
ms.locfileid: "2862604"
---
# <a name="author-events-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>在 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中撰寫事件

本主題示範如何撰寫一個 Windows 執行階段元件，其包含一個執行階段類別代表銀行帳戶，當餘額進入借方時，引發一個事件。 也示範一個核心應用程式，其使用銀行帳戶執行階段類別，呼叫調整餘額的函式，並處理所造成的任何事件。

> [!NOTE]
> 如需有關安裝和使用 C++/WinRT Visual Studio 擴充功能 (VSIX) (提供專案範本的支援，以及 C++/WinRT MSBuild 屬性和目標) 的資訊，請參閱 [C++/WinRT 和 VSIX 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

> [!IMPORTANT]
> 如需支援您了解如何使用 C++/WinRT 使用及撰寫執行階段類別的基本概念和詞彙，請查閱[使用 C++/WinRT 使用API](consume-apis.md)和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>建立 Windows 執行階段元件 (BankAccountWRC)

在 Microsoft Visual Studio 中，藉由建立新的專案來開始。 建立 **Visual C++ Windows 執行階段元件 (C + + / WinRT)** 專案，並將它命名為 *BankAccountWRC*  (適用於「銀行帳戶 Windows 執行階段元件」)。

新建立的專案中包含一個名為 `Class.idl` 的檔案。 重新命名該檔案`BankAccount.idl`(重新命名`.idl`檔案會自動重新命名相依`.h`和`.cpp`太檔案)。 取代的內容`BankAccount.idl`以下清單。

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

儲存檔案。 專案不會建立完成可用，但建置現在是因為它會產生將實作**BankAccount** runtime 類別原始程式碼檔案有用事。 所以繼續並建置現在 (您可預期在此階段的組建錯誤必須執行的動作`Class.h`和`Class.g.h`找不到)。 在建立過程中，`midl.exe`執行工具，以建立您的元件 Windows 執行階段的中繼資料檔案 (這是`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`)。 然後，執行 `cppwinrt.exe` 工具 (有 `-component` 選項) 產生原始碼檔案在撰寫您的元件中支援您。 這些檔案包含讓您開始實作您在您 IDL 中宣告的**BankAccount** runtime 類別的 stub。 這些虛設常式為 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` 與 `BankAccount.cpp`。

在檔案總管] 中將 stub 檔案複製`BankAccount.h`和`BankAccount.cpp`從資料夾`\BankAccountWRC\BankAccountWRC\Generated Files\sources\`至包含您的專案檔案的資料夾，這是`\BankAccountWRC\BankAccountWRC\`，並將 servername/目的地的檔案。 現在，我們開啟 `BankAccount.h` 與 `BankAccount.cpp` 並實作我們的執行階段類別。 在 `BankAccount.h` 中，將兩個私用成員新增至 (*不*是原廠實作) BankAccount 的實作。

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

您可以看到上方，就[**winrt::event**](/uwp/cpp-ref-for-winrt/event)結構 template、 參數化特定委派類型所實作事件。

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

如果任何警告會防止您建置，然後加以解決或設定專案屬性**C/c + +** > **一般** > **警告視為錯誤**待**否 (/ WX-)**，並重新建置專案。

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>建立核心應用程式 (BankAccountCoreApp) 測試 Windows 執行階段元件

現在建立新的專案 (在您的 `BankAccountWRC` 解決方案中，或在新的一個裡)。 建立 **Visual C++ 核心應用程式 (C++/WinRT)** 專案，並將它命名為 *BankAccountCoreApp*。

新增參照，並瀏覽至`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`（或如果兩個專案位於相同的方案新增專案至 project 參考 （英文）、）。 按一下 \[新增\]****，然後 **\[確定\]**。 現在建置 BankAccountCoreApp。 您看到錯誤可能互不事件中的承載檔案`readme.txt`不存在於] 從 Windows 執行階段元件專案中排除該檔案、 重建，然後再重建 BankAccountCoreApp。

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

每一次您按一下視窗，從銀行帳戶餘額減 1。 若要示範預期會所引發的事件，放置會處理**AccountIsInDebit**事件 lambda 運算式內中斷點、 執行應用程式，然後按一下 [視窗內。

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>參數化的委派及 ABI 不同的簡易訊號

如果您事件必須可以存取不同的應用程式二進位介面 (ABI)&mdash;等元件和其使用的應用程式&mdash;然後您事件必須使用 Windows Runtime 委派類型。 上述範例會使用[**Windows::Foundation::EventHandler\ < T\ >**](/uwp/api/windows.foundation.eventhandler) Windows Runtime 委派類型。 [**TypedEventHandler\ < TSender、 TResult\ >**](/uwp/api/windows.foundation.eventhandler)是 Windows Runtime 委派類型的另一個範例。

這兩個委派類型類型參數必須跨 ABI，所以類型參數必須是 Windows Runtime 類型太。 包含第一個及第三方 runtime 類別，以及如號碼和字串的基本類型。 此編譯器能協助您完成 」*必須是格式為 WinRT 類型*」 的錯誤如果忘記該條件約束。

如果您不需要傳遞任何參數或引數與您的事件，您可以定義您自己的簡單 Windows Runtime 委派類型。 下列範例會顯示**BankAccount** runtime 類別的簡化版本。 它會宣告名為**SignalDelegate**委派類型，然後它會使用所引發，而不是以參數事件訊號類型事件。

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

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>參數化的代理人、 簡單的訊號和回呼在專案

如果您事件在僅限內部使用內在 C + + WinRT project （無法跨二進位檔案），則您仍使用[**winrt::event**](/uwp/cpp-ref-for-winrt/event)結構範本，但參數化與 C + + WinRT 的非 Windows Runtime [**winrt::delegate&lt;...]T&gt;**](/uwp/cpp-ref-for-winrt/delegate)結構範本，這是有效率、 參考 （英文） 計算的委派。 支援任意數量的參數，並不會限制在 Windows Runtime 類型。

下列範例會先顯示代理人不採用任何參數 （基本上簡單訊號） 的簽章，然後接受字串的其中一個。

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

請注意如何新增事件相同數目的訂閱代理人為您想。 但是，有一些與事件關聯的額外負荷。 如果您只需要為只有單一訂閱委派、 與簡易回呼然後您可以使用[**winrt::delegate&lt;...]T&gt;**](/uwp/cpp-ref-for-winrt/delegate)合用。

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

如果您正在移植從 C + + CX 程式碼基底其中事件和代理人內部內使用專案，然後**winrt::delegate**可協助您複製該模式在 C + + WinRT。

## <a name="design-guidelines"></a>設計指導方針

建議您為函數參數傳遞事件及未代理人。 因為您必須在該情況下通過代理人都有一個例外， [**winrt::event**](/uwp/cpp-ref-for-winrt/event) **新增**功能。 此準則的原因是因為代理人可以採取不同表單跨不同 Windows Runtime 語言 （方面它們是否支援一個用戶端註冊或多個）。 事件，其多個訂戶模型、 構成更加可預測又一致的選項。

事件處理常式委派的簽章須有兩個參數：*寄件者*(**IInspectable**) 及*args* （某些事件引數類型，例如[**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)）。

請注意以下指導方針不一定會套用是否設計的內部 API。 雖然內部 Api 通常會成為公用一段時間。

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 撰寫 API ](author-apis.md)
* [使用 C++/WinRT 使用 API ](consume-apis.md)
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)
