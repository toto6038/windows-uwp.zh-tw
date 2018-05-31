---
author: stevewhims
description: 本主題示範如何撰寫包含引發事件的執行階段類別的 Windows 執行階段元件。 也示範使用元件和處理事件的應用程式。
title: '在 C++/WinRT 中撰寫事件 '
ms.author: stwhi
ms.date: 04/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、撰寫、事件
ms.localizationpriority: medium
ms.openlocfilehash: b7574f1a3406dae665ced80294f7bc1cf91aeb8c
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832022"
---
# <a name="author-events-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>在 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中撰寫事件
> [!NOTE]
> **正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。**

本主題示範如何撰寫一個 Windows 執行階段元件，其包含一個執行階段類別代表銀行帳戶，當餘額進入借方時，引發一個事件。 也示範一個核心應用程式，其使用銀行帳戶執行階段類別，呼叫調整餘額的函式，並處理所造成的任何事件。

> [!NOTE]
> 如需有關目前可用的 C++/WinRT Visual Studio 擴充功能 (VSIX) ( 其提供專案範本的支援，以及 C++/WinRT MSBuild 屬性和目標) 的資訊，請查閱 [適用於 C++/WinRT 的 Visual Studio 支援，以及 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

> [!IMPORTANT]
> 如需支援您了解如何使用 C++/WinRT 使用及撰寫執行階段類別的基本概念和詞彙，請查閱[使用 C++/WinRT 使用API](consume-apis.md)和[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="windowsfoundationeventhandlerlttgt-and-typedeventhandlerlttgt"></a>Windows::Foundation::EventHandler&lt;T&gt;和 [TypedEventHandler&lt;T&gt;
如果您想要從 Windows 執行階段元件中實作的執行階段類別引發一個事件，您應該使用適用於您事件委派類型的[**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) 或 [**TypedEventHandler**](/uwp/api/windows.foundation.eventhandler)。 此類型參數必須是 Windows 執行階段類型，以便允許基本類型以及第三方執行階段類別。

編譯器會協助您解決 *必須是 WinRT 類型* 的錯誤，如果您忘記此限制的話。

## <a name="winrtdelegatelttgt"></a>winrt::delegate&lt;...T&gt;
如果您想要從 C++ 類型 (在同一個專案中撰寫與使用) 引發一個事件，您可以使用適用於您事件委派類型的 C++/WinRT 的 **winrt::delegate&lt;...T&gt;**。 在此情況下，類型參數便不需要為 Windows 執行階段類型。

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>建立 Windows 執行階段元件 (BankAccountWRC)
在 Microsoft Visual Studio 中，藉由建立新的專案來開始。 建立 **Visual C++ Windows 執行階段元件 (C + + / WinRT)** 專案，並將它命名為 *BankAccountWRC*  (適用於「銀行帳戶 Windows 執行階段元件」)。

新建立的專案中包含一個名為 `Class.idl` 的檔案。 重新命名檔案 `BankAccountWRC.idl`，讓您建置時，元件的 Windows 執行階段中繼資料會以元件本身來命名。 在 `BankAccountWRC.idl` 中，定義您的介面，如下列清單所示。

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

儲存檔案並建置專案。 建置尚未成功。 但，在建置程序期間，執行 `midl.exe` 工具建立元件的 Windows 執行階段中繼資料檔案 (其為 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`)。 然後，執行 `cppwinrt.exe` 工具 (有 `-component` 選項) 產生原始碼檔案在撰寫您的元件中支援您。 這些檔案包含虛設常式，可協助您開始實作您在 IDL 中宣告的 `BankAccount` 執行階段類別。 這些虛設常式為 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` 與 `BankAccount.cpp`。

從 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` 將虛設常式檔案 `BankAccount.h` 和 `BankAccount.cpp` 複製到專案資料夾中，也就是 `\BankAccountWRC\BankAccountWRC\`。 在 **\[方案總管\]** 中，確定 **\[顯示所有檔案\]** 已切換成開啟。 按一下滑鼠右鍵您複製的虛設常式檔案，然後按一下 **\[加入至專案\]**。 此外，按一下滑鼠右鍵 `Class.h` 和 `Class.cpp`，且按一下 **\[從專案中移除\]**。

現在，我們開啟 `BankAccount.h` 與 `BankAccount.cpp` 並實作我們的執行階段類別。 在 `BankAccount.h` 中，將兩個私用成員新增至 (*不*是原廠實作) BankAccount 的實作。

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        event<Windows::Foundation::EventHandler<float>> accountIsInDebitEvent;
        float balance{ 0.f };
    };
}
...
```

在 `BankAccount.cpp` 中，如下實作函式。

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(event_token const& token)
    {
        accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        balance += value;
        if (balance < 0.f) accountIsInDebitEvent(*this, balance);
    }
}
```

**AdjustBalance**函式的實作引發 **AccountIsInDebit** 事件，如果餘額為負數的話。

如果有任何警告妨礙您建置，則將專案屬性 **C/C++** > **一般** > **視警告為錯誤** 設定為 **No (/WX-)**，並再試一次建置專案。

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>建立核心應用程式 (BankAccountCoreApp) 測試 Windows 執行階段元件
現在建立新的專案 (在您的 `BankAccountWRC` 解決方案中，或在新的一個裡)。 建立 **Visual C++ 核心應用程式 (C++/WinRT)** 專案，並將它命名為 *BankAccountCoreApp*。

新增參考資料，並瀏覽至`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`。 按一下 [新增]****，然後 **[確定]**。 現在建置 BankAccountCoreApp。 如果您看到裝載檔案 `readme.txt`不存在的錯誤，從 Windows 執行階段元件專案排除該檔案，重建它，然後重建 BankAccountCoreApp。

在建置程序期間，執行 `cppwinrt.exe` 工具將被參考的 `.winmd` 檔案處理到包含投影類型的原始碼檔案中，在使用元件裡支援您。 適用於您元件執行階段類別的投影類型標頭&mdash;命名為`BankAccountWRC.h`&mdash;在資料夾`\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\`中產生。

在 `App.cpp` 中包含該標頭。

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

也在 `App.cpp` 中，新增下列程式碼起始一個 BankAccount  (使用投影類型的預設建構函式)，註冊事件處理常式，然後導致帳戶進入借方。

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    event_token eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        eventToken = bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            assert(balance < 0.f);
        });
    }
    ...

    void Uninitialize()
    {
        bankAccount.AccountIsInDebit(eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

每一次您按一下視窗，從銀行帳戶餘額減 1。 若要示範事件如預期般引發，請將中斷點放至於 lambda 運算式中、執行應用程式，然後在視窗中按一下。

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 撰寫 API ](author-apis.md)
* [使用 C++/WinRT 使用 API ](consume-apis.md)
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)
