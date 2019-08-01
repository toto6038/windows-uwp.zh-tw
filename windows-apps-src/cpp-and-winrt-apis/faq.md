---
description: 有關於使用 C++/WinRT 撰寫及使用 Windows 執行階段 API 您可能會有的問題的解答。
title: 有關 C++/WinRT 的常見問題集
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, frequently, asked, questions, faq, 標準, 投影, 常見, 提問, 問題, 常見問題集
ms.localizationpriority: medium
ms.openlocfilehash: 6bac3fec34467f29d9cf2cc3f1ce4e3754187745
ms.sourcegitcommit: 7ece8a9a9fa75e2e92aac4ac31602237e8b7fde5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/25/2019
ms.locfileid: "68485153"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>有關 C++/WinRT 的常見問題集
有關於使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 撰寫及使用 Windows 執行階段 API 您可能會有的問題的解答。

> [!NOTE]
> 如果您的問題是關於您已經看過的錯誤訊息，則也會看到[疑難排解 C++/WinRT](troubleshooting.md)主題。

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>如何將 C++/WinRT 專案的目標重新設定為 Windows SDK 的較新版本？
請參閱[如何將您 C++/WinRT 專案的目標設定為 Windows SDK 的較新版本](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)。

## <a name="why-wont-my-new-project-compile-now-that-ive-moved-to-cwinrt-20"></a>我已經移至 C++/WinRT 2.0，但為什麼我的新專案不會編譯？
如需完整的變更集合 (包括重大變更)，請參閱 [C++/WinRT 2.0 中的新聞和變更](news.md#news-and-changes-in-cwinrt-20)。 例如，如果您在 Windows 執行階段集合上使用範圍型 `for`，則您現在必須 `#include <winrt/Windows.Foundation.Collections.h>`。

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>我的新專案為什麼不會編譯？ 我使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，以及 SDK 版本 17134
如果您使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，並將 Windows SDK 版本 10.0.17134.0 (Windows 10 版本 1803) 設定為目標，則新建立的 C++/WinRT 專案可能無法完成，並出現錯誤「error C3861: 'from_abi': identifier not found」  ，以及其他源自 base.h  的錯誤。 解決辦法是將 Windows SDK 的較新 (更一致) 版本設定為目標，或將專案屬性設定為 [C/C++]   > [語言]   > [一致性模式:  否] (此外，如果 **/permissive-** 顯示在 [其他選項]  底下的專案屬性 [C/C++]   > [命令列]  中，請加以刪除)。

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>如何解決建置錯誤「C++/WinRT VSIX 不再提供專案建置支援。  請將專案參考新增至 Microsoft.Windows.CppWinRT Nuget 套件」？
將 **Microsoft.Windows.CppWinRT** NuGet 套件安裝到您的專案中。 如需詳細資訊，請參閱[舊版 VSIX 擴充功能](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>C++/WinRT Visual Studio 擴充功能 (VSIX) 有何需求？
若為 VSIX 擴充功能的版本 1.0.190128.4 和更新版本，請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。 若為其他版本，則請參閱[舊版 VSIX 擴充功能](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

## <a name="whats-a-runtime-class"></a>什麼是「執行階段類別」  ？
執行階段類別是可透過現代化 COM 介面啟動與使用的類型 (通常是跨可執行檔邊界的)。 不過，執行階段類別也可在實作它的編譯單位中使用。 您可以在介面定義語言 (IDL) 中宣告執行階段類別，並可以在使用 C++/WinRT 的標準 C++ 中實作它。

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>「投影類型」  和「實作類型」  是什麼意思？
如果您只「取用」  Windows 執行階段類別 (執行階段類別)，則您可以專用「投影類型」  來處理。 C++/WinRT 是「語言投影」  ，因此投影類型是 Windows 執行階段介面一部分，其使用 C++/WinRT「投影」  到 C++ 裡。 如需詳細資訊，請參閱[使用 C++/WinRT 使用 API](consume-apis.md)。

「實作類型」  包含執行階段類別的實作，因此它僅在實作執行階段類別的專案中可用。 當您在實作執行階段類別的專案中執行時 (Windows 執行階段元件專案，或使用 XAML UI 的專案)，請務必將熟悉適用於執行階段類別的實作類型間的區別，且代表執行階段類別的投影類型投影至 C++/WinRT。 如需詳細資訊，請參閱[使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>我需要在執行階段類別的 IDL 中宣告一個建構函式嗎？
只有當設計此執行階段類別從其實作編譯單位之外來使用 (它是 Windows 執行階段元件，旨在供給 Windows 執行階段用戶端應用程式的一般使用)。 如需 IDL 中完整的目的與宣告建構函式結果的詳細資訊，請參閱[執行階段類別建構函式](author-apis.md#runtime-class-constructors)。

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>為何連結器給我「LNK2019:無法解析的外部符號」錯誤？
如果無法解析的符號是來自 C++/WinRT 投影的 Windows 命名空間標頭的 API (在 **winrt** 命名空間中)，則 API 在您已包含的標頭中是向前宣告，但其定義是在您尚未包含的標頭中。 包含為 API 命名空間命名的標頭，並且重新建置。 如需詳細資訊，請參閱 [C++/WinRT 投影標頭](consume-apis.md#cwinrt-projection-headers)。

如果無法解析的符號是 Windows 執行階段可用函式，例如 [RoInitialize](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roinitialize)，您便需要在專案中明確連結 [WindowsApp.lib](/uwp/win32-and-com/win32-apis) 傘程式庫。 C++/WinRT 投影仰賴部分這些免費 (非成員) 函式和進入點。 如果您使用其中一個 [C++/WinRT Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix) 專案範本來供應用程式使用，則系統會自動為您連結 `WindowsApp.lib`。 否則，您也可以使用專案連結設定來包括它，或在原始程式碼中執行它。

```cppwinrt
#pragma comment(lib, "windowsapp")
```

請務必藉由連結 **WindowsApp.lib** (而非連結替代的靜態連結程式庫) 來解決可以解決的連結器錯誤，否則應用程式將不會傳遞 Visual Studio 和 Microsoft Store 用來驗證提交的 [Windows 應用程式認證套件](../debug-test-perf/windows-app-certification-kit.md)測試 (亦即，應用程式將因此無法成功內嵌到 Microsoft Store)。

## <a name="why-am-i-getting-a-class-not-registered-exception"></a>為什麼我會收到「類別未註冊」的例外狀況？

此情況的徵兆是&mdash;建構執行階段類別或存取靜態成員時&mdash;您看到執行階段上擲回例外狀況，且其中包含 REGDB_E_CLASSNOTREGISTERED 的 HRESULT 值。

其中一個原因是無法載入您的 Windows 執行階段元件。 請確定元件的 Windows 執行階段中繼資料檔 (`.winmd`) 與元件二進位檔 (`.dll`) 具有相同名稱，這也是專案名稱以及根命名空間的名稱。 也請確定，Windows 執行階段中繼資料和二進位檔已由建置程序正確複製到使用中應用程式的 `Appx` 資料夾。 並且確認使用中應用程式的 `AppxManifest.xml` (也在 `Appx` 資料夾中) 包含正確宣告可啟動類別與二進位檔案名稱的 **&lt;InProcessServer&gt;** 元素。

### <a name="uniform-construction"></a>統一建構

如果您嘗試透過任何投影類型的建構函式(而非其 **std::nullptr_t** 建構函式) 具現化本機上實作的執行階段類別，也可能會發生此錯誤。 若要執行此操作，您將需要通常稱為統一建構的 C++/WinRT 2.0 功能。 如果您想要加入這項功能，請參閱[加入統一建構和直接實作存取。](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access)，以取得詳細資訊和程式碼範例。

如您想以「不」  需要統一建構的方式具現化本機上實作的執行階段類別，請參閱 [XAML 控制項；繫結至 C++/WinRT 屬性](binding-property.md)。

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>我是否應該實作 [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)，如果是，該如何進行？
如果在其解構程式中您有釋出資源的執行階段類別，且設計該執行階段類別從其實作編譯單位之外使用 (它是 Windows 執行階段元件，旨在供給 Windows 執行階段用戶端應用程式的一般使用)，我們建議您也實作 **IClosable** 以便支援不確定完成的語言使用您的執行階段類別。 請確定不論是否解構函式都會釋出您的資源，[**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close)，或兩者都呼叫。 可以任意呼叫 **IClosable::Close** 數次。

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>我需要在我使用的執行階段類別上呼叫 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) 嗎？
**IClosable** 存在以支援不確定完成的語言。 因此，您就不應該呼叫 C++/WinRT 的 **IClosable::Close**，除非在非常少數案例中，牽涉到關機競爭或半致命採用。 如果您正使用 **Windows.UI.Composition** 類型，以此為例，您可能會遇到想在一組序列中處置物件的案例，允許 C++/WinRT 包裝函式的解構函式為您執行工作，做為替代方案。

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>我可以使用 LLVM/Clang 來編譯 C++/WinRT 嗎？
我們對於 C++/WinRT 不支援 LLVM 和 Clang toolchain，但我們在內部使用它來驗證 C++/WinRT 的標準一致性。 例如，如果您想要模擬我們在內部的操作，您可能會嘗試實驗，例如如下所述。

移至 [LLVM 下載頁面](https://releases.llvm.org/download.html)，尋找 [下載 LLVM 6.0.0]   > [預先建置的二進位檔]  ，然後下載 **Clang for Windows (64 位元)** 。 在安裝期間，選擇新增 LLVM 到 PATH 系統變數，讓您可以從命令提示字元叫用它。 基於此實驗的目的，您可以略過 (如果您看到) 任何「找不到 MSBuild 工具組目錄」和/或「MSVC 整合安裝失敗」錯誤。 有各種不同的方式可叫用 LLVM/Clang;，以下範例顯示的只是一種方式。

```cmd
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include <winrt/Windows.Foundation.h>
#include <stdio.h>
#include <iostream>

using namespace winrt;

int main()
{
    winrt::init_apartment();
    Windows::Foundation::Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    std::wcout << rssFeedUri.Domain().c_str() << std::endl;
}

C:\ExperimentWithLLVMClang>clang-cl main.cpp /EHsc /I ..\.. -Xclang -std=c++17 -Xclang -Wno-delete-non-virtual-dtor -o app.exe

C:\ExperimentWithLLVMClang>app
windows.com
```

因為 C++/WinRT 使用 C++17 標準，您將需要使用任何必要的編譯器旗標來取得支援，這類旗標在編譯器間各不相同。

Visual Studio 是我們支援和為 C++/WinRT 建議的開發工具。 請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>為什麼針對唯讀屬性產生的實作函式沒有 `const` 限定詞？
當您在 [MIDL 3.0](/uwp/midl-3/) 中宣告唯讀屬性時，您預期 `cppwinrt.exe` 工具會為您產生限定 `const` (const 函式會將「此」  指標視為 const) 的實作函式。

我們無疑會建議您盡可能使用 const，但 `cppwinrt.exe` 工具本身不會嘗試推斷哪些實作函式理論上可能是 const，哪些可能不是。 您可以和本範例一樣選擇讓任何實作函式成為 const。

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

如果您決定您需要變更實作中的某些物件狀態，則可以在 **ToString** 上移除該 `const` 限定詞。 但請讓每個成員函式同時成為 const 或非 const，但不可兩者兼具。 換句話說，請勿在 `const` 上多載實作函式。

除了實作函式外，會讓 const 引人注意的另一個位置是在 Windows 執行階段函式投影中。 請考慮下列程式碼。

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

針對上述 **ToString** 的呼叫，Visual Studio 中的 **Go To Declaration** 命令顯示 Windows 執行階段 **IStringable::ToString** 對 C++/WinRT 的投影樣貌如下。

```cppwinrt
winrt::hstring ToString() const;
```

投影上的函式都是 const，無論您如何選擇來符合函式的實作都是如此。 在幕後，投影會呼叫應用程式二進位介面 (ABI)，這相當於透過 COM 介面指標所進行的呼叫。 所投影的 **ToString** 唯一會互動的狀態便是該 COM 介面指標，而且當然不需要修改該指標，因此該函式是 const。 這可讓您確信其不會變更呼叫時所用 **IStringable** 參考的任何元素，並確保您即便使用 **IStringable** 的 const 參考，也可以呼叫 **ToString**。

請了解 `const` 的這些範例是 C++/WinRT 投影和實作的實作詳細資料；其可為您構成程式碼檢查。 不論是 COM 還是 Windows 執行階段 ABI (針對成員函式)，上面都沒有 `const` 這類函式。

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>對於該如何減少 C++/WinRT 二進位檔的程式碼大小，您是否有任何建議？
在使用 Windows 執行階段物件時，請避免使用如下所示的編碼模式，因為這麼做會產生超過所需的二進位程式碼，而對應用程式造成負面影響。

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

在 Windows 執行階段環境中，編譯器既無法快取 `c()` 的值，也無法快取透過間接方式 ('.') 所呼叫的每個方法所使用的介面。 除非您介入，否則這會導致更多虛擬呼叫和參考計數額外負荷。 上述模式輕易就能產生真正所需數量兩倍的程式碼。 相反地，如果可以，最好使用如下所示的模式。 其產生的程式碼會少很多，而且還可以大幅改善執行階段的效能。

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

如上所示的建議模式不只適用於 C++/WinRT，還適用於所有 Windows 執行階段語言投影。

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>如何讓字串變為類型 &mdash; 舉例來說，為了瀏覽？
[瀏覽檢視程式碼範例](/windows/uwp/design/controls-and-patterns/navigationview#code-example) (大部分使用 C#) 結尾有 C++/WinRT 程式碼片段會示範如何執行這項操作。

## <a name="how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try"></a>如何解決 GetCurrentTime 和/或 TRY 意義不明的狀況？

標頭檔 `winrt/Windows.UI.Xaml.Media.Animation.h` 會宣告名為 **GetCurrentTime** 的方法，而 `windows.h` (透過 `winbase.h`) 會定義名為 **GetCurrentTime** 的巨集。 當兩者發生衝突時，C++ 編譯器會產生「*錯誤 C4002：函式類巨集引動過程 GetCurrentTime 有太多引數*」。

同樣地，`winrt/Windows.Globalization.h` 會宣告名為 **TRY** 的方法，而 `afx.h` 會定義名為 **GetCurrentTime** 的巨集。 當這些有所衝突時，C++編譯器會產生「錯誤 C2334：'{' 前面有非預期的權杖；略過顯示的函式主體  」。

若要解決其中一個問題，或同時解決這兩個問題，您可以執行此操作。

```cppwinrt
#pragma push_macro("GetCurrentTime")
#pragma push_macro("TRY")
#undef GetCurrentTime
#undef TRY
#include <winrt/include_your_cppwinrt_headers_here.h>
#include <winrt/include_your_cppwinrt_headers_here.h>
#pragma pop_macro("TRY")
#pragma pop_macro("GetCurrentTime")
```

> [!NOTE]
> 如果本主題無法解答您的問題，則您可藉由造訪 [Visual Studio C++ 開發人員社群](https://developercommunity.visualstudio.com/spaces/62/index.html)，或[在 Stack Overflow 上使用 `c++-winrt` 標籤](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt)來尋找說明。
