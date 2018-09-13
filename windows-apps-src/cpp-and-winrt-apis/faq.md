---
author: stevewhims
description: 有關於使用 C++/WinRT 撰寫及使用 Windows 執行階段 API 您可能會有的問題的解答。
title: 有關 C++/WinRT 的常見問題集
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, frequently, asked, questions, faq, 標準, 投影, 常見, 提問, 問題, 常見問題集
ms.localizationpriority: medium
ms.openlocfilehash: 9316a29a50970bdaa288a4744f3aab7d873cbe4e
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2018
ms.locfileid: "3962902"
---
# <a name="frequently-asked-questions-about-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>有關 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 的常見問題集
有關於使用 C++/WinRT 撰寫及使用 Windows 執行階段 API 您可能會有的問題的解答。

> [!NOTE]
> 如果您的問題是關於您已經看過的錯誤訊息，則也會看到[疑難排解 C++/WinRT](troubleshooting.md)主題。

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>為什麼我的新專案將無法編譯？ 我使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，和 SDK 版本 17134

如果您使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，並且針對 Windows SDK 版本 10.0.17134.0 (Windows 10，版本 1803年)，則新建立 C + + /winrt 專案可能會無法編譯錯誤 」*錯誤 C3861: 'from_abi': 識別碼不找到*」，並包含來自*base.h*其他錯誤。 解決方案是任一目標更新版本 （更多符合） 版本的 Windows SDK 中或將專案屬性**C/c + +** > **語言** > **一致性模式： 否**(此外，如果 **/ 寬鬆-** 會出現在專案屬性**C/C++** > **語言** > **命令列**在**其他選項**，然後刪除它)。

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>[C++/WinRT Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix) 的需求是什麼？
[VSIX](https://aka.ms/cppwinrt/vsix) 強制執行 10.0.17134.0 (Windows 10，版本 1803 ) 的最小 Windows SDK 目標版本。 您也需要 Visual Studio 2017 (至少版本 15.6；我們建議至少 15.7)。 您可以識別一個專案，其藉由 `.vcxproj`檔案中的 `<PropertyGroup Label="Globals">` 的 `<CppWinRTEnabled>true</CppWinRTEnabled>` 存在來使用 VSIX。 如需詳細資訊，請參閱 [適用於 C++/WinRT 的 Visual Studio 支援，以及 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="whats-a-runtime-class"></a>什麼是*執行階段類別*？
執行階段類別是可透過現代化 COM 介面啟動與使用的類型，一般經過可執行的界限。 不過，執行階段類別也可在實作它的編譯單位中使用。 您可以在介面定義語言 (IDL) 中宣告執行階段類別，並可以在使用 C++/WinRT 的標準 C++ 中實作它。

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>*投影類型*和*實作類型*的意思是什麼？
如果您只是*使用*一個 Windows 執行階段類別 (執行階段類別)，則您可以專用*投影類型*來處理。 C++/WinRT 是一個*語言投影*，因此投影類型是 Windows 執行階段介面一部分，其使用 C++/WinRT *投影*到 C++ 裡。 如需詳細資訊，請參閱 [使用 C++/WinRT 使用 API](consume-apis.md)。

*實作類型*包含執行階段類別的實作，因此它僅在實作執行階段類別的專案中可用。 當您在實作執行階段類別的專案中執行時 (Windows 執行階段元件專案，或使用 XAML UI 的專案)，請務必將熟悉適用於執行階段類別的實作類型間的區別，且代表執行階段類別的投影類型投影至 C++/WinRT。 如需詳細資訊，請參閱 [使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>我需要在執行階段類別的 IDL 中宣告一個建構函式嗎？
只有當設計此執行階段類別從其實作編譯單位之外來使用 (它是 Windows 執行階段元件，旨在供給 Windows 執行階段用戶端應用程式的一般使用)。 如需 IDL 中完整的目的與宣告建構函式結果的詳細資訊，請參閱 [執行階段類別建構函式](author-apis.md#runtime-class-constructors)。

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>為何連結器給我「LNK2019：無法解析的外部符號」錯誤？
如果無法解析的符號是來自 C++/WinRT 投影的 Windows 命名空間標頭的 API (在 **winrt** 命名空間中)，則API 在您已包含的標頭中是向前宣告，但其定義是在您尚未包含的標頭中。 包含為 API 命名空間命名的標頭，並且重新建置。 如需詳細資訊，請參閱 [C++/WinRT 投影標頭](consume-apis.md#cwinrt-projection-headers)。

如果無法解析的符號是 Windows 執行階段可用函式，例如 [RoInitialize](https://msdn.microsoft.com/library/br224650)，則您將需要在您的專案中明確包含 [WindowsApp.lib](/uwp/win32-and-com/win32-apis) 傘程式庫。 C++/WinRT 投影仰賴部分這些免費 (非成員) 函式和進入點。 如果您使用其中一個 [C++/WinRT Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix) 投影來為您的應用程式轉譯，則 `WindowsApp.lib` 會自動為您連結。 否則，您也可以使用專案連結設定來包括它，或在原始程式碼中執行它。

```cppwinrt
#pragma comment(lib, "windowsapp")
```

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>我是否應該實作 [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)，如果是，該如何進行？
如果在其解構程式中您有釋出資源的執行階段類別，且設計該執行階段類別從其實作編譯單位之外使用 (它是 Windows 執行階段元件，旨在供給 Windows 執行階段用戶端應用程式的一般使用)，我們建議您也實作 **IClosable** 以便支援不確定完成的語言使用您的執行階段類別。 請確定不論是否解構函式都會釋出您的資源，[**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close)，或兩者都呼叫。 可以任意呼叫 **IClosable::Close** 數次。

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>我需要在我使用的執行階段類別上呼叫 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_) 嗎？
**IClosable** 存在以支援不確定完成的語言。 因此，您就不應該呼叫 C++/WinRT 的 **IClosable::Close**，除非在非常少數案例中，牽涉到關機競爭或半致命採用。 如果您正使用 **Windows.UI.Composition** 類型，以此為例，您可能會遇到想在一組序列中處置物件的案例，允許 C++/WinRT 包裝函式的解構函式為您執行工作，做為替代方案。

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>我可以使用 LLVM/Clang 來編譯 C++/WinRT 嗎？
我們對於 C++/WinRT 不支援 LLVM 和 Clang toolchain，但我們在內部使用它來驗證 C++/WinRT 的標準一致性。 例如，如果您想要模擬我們在內部的操作，您可能會嘗試實驗，例如如下所述。

移至 [LLVM 下載頁面](https://releases.llvm.org/download.html)，尋找**下載 LLVM 6.0.0** > **預先建置的二進位檔**，然後下載 **Clang for Windows (64 位元)**。 在安裝期間，選擇新增 LLVM 到 PATH 系統變數，讓您可以從命令提示字元叫用它。 基於此實驗的目的，您可以略過 (如果您看到) 任何「找不到 MSBuild 工具組目錄」和/或「MSVC 整合安裝失敗」錯誤。 有各種不同的方式可叫用 LLVM/Clang;，以下範例顯示的只是一種方式。

```
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include "winrt/Windows.Foundation.h"
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

Visual Studio 是我們支援和為 C++/WinRT 建議的開發工具。 請參閱 [C++/WinRT 和 VSIX 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>為什麼沒有唯讀屬性的產生的實作函式`const`限定詞？

當您宣告中[MIDL 3.0](/uwp/midl-3/)的唯讀屬性時，您可能會預期`cppwinrt.exe`工具，來為您產生實作函式是`const`-完整 （const 函式會將視為 const*這個*指標）。

當然建議使用 const 如果可行，但`cppwinrt.exe`工具本身不會嘗試原因相關之實作函式都可能 const，而這可能無法。 您可以選擇讓任何您實作函式 const，如這個範例所示。

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

您可以移除這些`const`限定詞上**ToString**應該您決定您需要修改它的實作中一些物件狀態。 但讓每個您的成員函式 const 或非 const 不可兩者。 換句話說，不多載實作函式在`const`。

除了您實作的函式，另一個其他放置的位置 const 已能圖片是在 Windows 執行階段函式投射。 請考慮此程式碼。

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

**ToString**上述的呼叫，**請移至宣告**命令，在 Visual Studio 中的顯示的 Windows 執行階段**istringable:: Tostring**投影到 C + + /winrt 看起來像這樣。

```
winrt::hstring ToString() const;
```

投影的函式是 const 不論您選擇以符合您的實作它們的方式。 在幕後投影呼叫應用程式二進位介面 (ABI)，到透過 COM 介面指標呼叫的金額。 投影的**ToString**互動的唯一狀態是該 COM 介面指標。而且，當然具備不需要修改該指標，因此函式是 const。 這讓您確保它不會變更，透過呼叫**IStringable**參考的相關的任何項目，以及它可確保您可以呼叫**ToString**即使有 const 參考**IStringable**。

了解，這些範例中的`const`是實作詳細資料的 C + + /winrt 規劃和實作;它們會構成供您參考的程式碼健康。 沒有這類的`const`上，COM，也不 Windows 執行階段 ABI （適用於成員函式）。

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>您有任何建議縮小的程式碼的 C + + /winrt 的二進位檔嗎？

使用 Windows 執行階段物件時，您應該避免因為它可以在您的應用程式上引起不必要產生更多二進位檔案的程式碼會有負面影響，如下所示的程式碼撰寫模式。

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

在 Windows 執行階段世界中，編譯器就無法快取的值`c()`或透過間接取值稱為每個方法的介面 ('。 」)。 除非您介入，這樣會造成更多虛擬呼叫和參考計數額外負荷。 上述的模式可以輕鬆地產生倍地所需的程式碼。 相反地，想要的模式，您可以在任何地方，如下所示。 它會產生很多較少的程式碼，以及它可以也可大幅改善您的執行的階段效能。

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

上述的建議的模式適用於而不只是 C + + /winrt，但所有的 Windows 執行階段語言投影。

> [!NOTE]
> 如果本主題未能回答您的問題，也許您可透過使用 [Stack Overflow 上的 `c++-winrt` 標記](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt) 來尋求協助。
