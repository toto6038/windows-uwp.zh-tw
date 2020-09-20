---
title: 使用 C++/WinRT 的 Windows 執行階段元件
description: 本主題說明如何使用 [c + +/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 來建立和取用 Windows 執行階段元件 &mdash; ，該元件可從使用任何 Windows 執行階段語言建立的通用 Windows 應用程式中呼叫。
ms.date: 07/06/2020
ms.topic: article
keywords: windows 10、uwp、windows、執行時間、元件、元件、Windows 執行階段元件、WRC、c + +/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: 25286260c4abd6686939393b3bf81df818879bf9
ms.sourcegitcommit: 21eb13a50402bf5442a5f0a4bf34800d1dc679c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2020
ms.locfileid: "90804748"
---
# <a name="windows-runtime-components-with-cwinrt"></a>使用 C++/WinRT 的 Windows 執行階段元件

本主題說明如何使用 [c + +/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 來建立和取用 Windows 執行階段元件 &mdash; ，該元件可從使用任何 Windows 執行階段語言建立的通用 Windows 應用程式中呼叫。

在 c + + 中建立 Windows 執行階段元件有幾個原因/Winrt
- 在複雜或需要大量運算的作業中享有 c + + 的效能優勢。
- 重複使用已撰寫並測試的標準 c + + 程式碼。
- 若要將 Win32 功能公開給以撰寫的通用 Windows 平臺 (UWP) 應用程式，例如 c #。

一般情況下，當您撰寫 c + +/WinRT 元件時，您可以使用 standard c + + 程式庫中的類型和內建類型，但在應用程式二進位介面 (ABI) 界限（您將資料傳入和寫入其他封裝中的程式碼） `.winmd` 。 在 ABI 上，使用 Windows 執行階段類型。 此外，在您的 c + +/WinRT 程式碼中，請使用委派和事件之類的型別，來執行可從元件引發並以其他語言處理的事件。 如需 c + +/Winrt 的詳細資訊，請參閱[c + +/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)

本主題的其餘部分將逐步引導您瞭解如何在 c + +/WinRT 中撰寫 Windows 執行階段元件，以及如何從應用程式使用它。

您將在本主題中建立的 Windows 執行階段元件包含代表銀行帳戶的執行時間類別。 本主題也會示範使用銀行帳戶執行時間類別的核心應用程式，並呼叫函式來調整餘額。

> [!NOTE]
> 如需安裝和使用 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援) 的資訊，請參閱 [C++/WinRT 的 Visual Studio 支援](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

> [!IMPORTANT]
> 如需一些基本概念和詞彙，以協助了解如何以 C++/WinRT 使用及撰寫執行階段類別，請參閱[使用 C++/WinRT 來使用 API](../cpp-and-winrt-apis/consume-apis.md) 和[使用 C++/WinRT 撰寫 API](../cpp-and-winrt-apis/author-apis.md)。

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>建立 Windows 執行階段元件 (BankAccountWRC)

請先在 Microsoft Visual Studio 中，建立新的專案。 建立 **Windows 執行階段元件 (C++/WinRT)** 專案，並將其命名為 *BankAccountWRC* (適用於「銀行帳戶 Windows 執行階段元件」)。 確定已取消核取 [將方案和專案放置於同一個目錄]。 以 Windows SDK 最新的正式推出版本 (即非預覽版本) 為目標。 為 *BankAccountWRC* 專案命名可讓您在本主題的其餘步驟中提供最簡單的使用體驗。 

尚未建置專案。

新建立的專案中包含一個名為 `Class.idl` 的檔案。 在方案總管中，將該檔案 `BankAccount.idl` 重新命名 (重新命名 `.idl` 檔案也會自動將相依的 `.h` 和 `.cpp` 檔案重新命名)。 以下面的清單取代 `BankAccount.idl` 的內容。

> [!NOTE]
> 不用說，您也不會以這種方式來實行生產財務軟體;我們 `Single` 只為了方便起見，在此範例中使用。

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        void AdjustBalance(Single value);
    };
}
```

儲存檔案。 此專案此刻不會建置完成，但立即建置是實用的做法，因為它會產生原始程式碼檔案，而您會在其中實作 **BankAccount** 執行階段類別。 因此請繼續並立即建置 (您可預期在這個階段看到的建置錯誤有找不到 `Class.h` 和 `Class.g.h`有關)。

在建置程序期間，執行 `midl.exe` 工具建立元件的 Windows 執行階段中繼資料檔案 (其為 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`)。 然後，執行 `cppwinrt.exe` 工具 (有 `-component` 選項) 產生原始碼檔案在撰寫您的元件中支援您。 這些檔案包含虛設常式，可協助您開始實作您在 IDL 中宣告的 **BankAccount** 執行階段類別。 這些虛設常式為 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` 與 `BankAccount.cpp`。

以滑鼠右鍵按一下專案節點，然後按一下 [在檔案總管中開啟資料夾]。 這會在檔案總管中開啟專案資料夾。 然後，從 `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` 資料夾將虛設常式檔案 `BankAccount.h` 和 `BankAccount.cpp` 複製到包含專案檔案的資料夾中，也就是 `\BankAccountWRC\BankAccountWRC\` 並取代目的地中的檔案。 現在要開啟 `BankAccount.h` 與 `BankAccount.cpp`，並實作我們的執行階段類別。 在中 `BankAccount.h` ，將新的私用成員新增 (至 BankAccount，*而不*是**BankAccount**的 factory 執行) 。

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        float m_balance{ 0.f };
    };
}
...
```

在中 `BankAccount.cpp` ，執行 **AdjustBalance** 方法，如下列清單所示。

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
    }
}
```

您也必須從這兩個檔案中刪除 `static_assert`。

如果有任何警告妨礙您建置，則加以解決或將專案屬性 **C/C++**  > **一般** > **視警告為錯誤** 設定為 **No (/WX-)** ，並再試一次建置專案。

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>建立核心應用程式 (BankAccountCoreApp) 以測試 Windows 執行階段元件

現在建立新的專案 (在您的 *BankAccountWRC* 解決方案中，或在新的解決方案中)。 建立**核心應用程式 (C++/WinRT)** 專案，並將它命名為 *BankAccountCoreApp*。 如果兩個專案位於相同的方案中，請將 *BankAccountCoreApp* 設定為啟始專案。

> [!NOTE]
> 如先前所述，Windows 執行階段元件的 Windows 執行階段中繼資料檔案 (命名為 *BankAccountWRC*的專案) 是建立於 `\BankAccountWRC\Debug\BankAccountWRC\` 資料夾中。 該路徑的第一個區段是包含解決方案檔案的資料夾名稱；下一個區段是名為 `Debug` 的子目錄；最後一個區段是為 Windows 執行階段元件所命名的子目錄。 如果您未將專案命名為 *BankAccountWRC*，則您的中繼資料檔案將會在 `\<YourProjectName>\Debug\<YourProjectName>\` 資料夾中。

現在，在核心應用程式專案 (*BankAccountCoreApp*) 新增參考，並瀏覽至 `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (或者如果有兩個專案在相同的解決方案中，新增專案對專案參考)。 按一下 [新增]，然後按一下 [確定]。 現在建置 *BankAccountCoreApp*。 萬一您看到承載檔案 `readme.txt` 不存在的錯誤，請將該檔案從 Windows 執行階段元件專案中排除，進行重建，然後重建 *BankAccountCoreApp*。

在建置程序期間，執行 `cppwinrt.exe` 工具將被參考的 `.winmd` 檔案處理到包含投影類型的原始碼檔案中，在使用元件裡支援您。 適用於您元件執行階段類別的投影類型標頭 &mdash;名為`BankAccountWRC.h`&mdash;會在資料夾 `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\` 中產生。

在 `App.cpp` 中包含該標頭。

```cppwinrt
// App.cpp
...
#include <winrt/BankAccountWRC.h>
...
```

此外，在中 `App.cpp` ，加入下列程式碼以具現化 **BankAccount** 物件 (使用投影的型別預設的函式) ，然後在銀行帳戶物件上呼叫方法。

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(1.f);
        ...
    }
    ...
};
```

每次您按一下視窗時，都會遞增銀行帳戶物件的餘額。 如果您想要逐步執行程式碼，以確認應用程式確實呼叫 Windows 執行階段元件，您可以設定中斷點。

## <a name="next-steps"></a>後續步驟

若要將更多功能或新的 Windows 執行階段類型新增至 c + +/WinRT Windows 執行階段元件，您可以遵循如上所示的相同模式。 首先，使用 IDL 來定義您想要公開的功能。 然後，在 Visual Studio 中建立專案，以產生存根的執行。 然後視需要完成執行。 使用您的 Windows 執行階段元件的應用程式，可以看到您在 IDL 中定義的任何方法、屬性和事件。 如需 IDL 的詳細資訊，請參閱 [Microsoft 介面定義語言3.0 簡介](/uwp/midl-3/intro)。

如需如何將事件加入 Windows 執行階段元件的範例，請參閱 [在 c + + 中撰寫事件/WinRT](../cpp-and-winrt-apis/author-events.md)。

## <a name="troubleshooting"></a>疑難排解

| 徵狀 | 補救方法 |
|---------|--------|
|在 c + +/WinRT 應用程式中，使用使用 XAML 的 [c # Windows 執行階段元件](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic) 時，編譯器會產生格式為 "*' MyNamespace_XamlTypeInfo ' 的錯誤：不是 ' WinRT：： MyNamespace ' 的成員*， &mdash; 其中 *MyNamespace* 是 Windows 執行階段元件命名空間的名稱。 | 在 `pch.h` 使用 c + +/WinRT 應用程式中，新增 `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>` &mdash; 適當的取代*MyNamespace* 。 |
