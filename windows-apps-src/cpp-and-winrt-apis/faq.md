---
description: 有關於使用 C++/WinRT 撰寫及使用 Windows 執行階段 API 您可能會有的問題的解答。
title: 有關 C++/WinRT 的常見問題集
ms.date: 10/26/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, frequently, asked, questions, faq, 標準, 投影, 常見, 提問, 問題, 常見問題集
ms.localizationpriority: medium
ms.openlocfilehash: 9dd051ffe3af9e18370666f5c6c772b7f188e54a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635573"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>有關 C++/WinRT 的常見問題集
您很可能會有關於撰寫和使用 Windows 執行階段 Api 的問題的答案[C + + /cli WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。

> [!NOTE]
> 如果您的問題是關於您已經看過的錯誤訊息，則也會看到[疑難排解 C++/WinRT](troubleshooting.md)主題。

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>我該如何重定我的 C + + /cli 至較新版的 Windows sdk 的 WinRT 專案？
請參閱[如何將目標重定您 C + + /cli 至較新版的 Windows sdk 的 WinRT 專案](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)。

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>為什麼我的新專案將無法編譯？ 我使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，和 SDK 版本 17134
如果您使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，與目標 Windows SDK 版本 10.0.17134.0 (Windows 10 1803年版)，然後新建立 C + + /cli WinRT 專案可能無法編譯錯誤 」*錯誤 c3861:: 'from_abi':找不到識別項*"，且其他錯誤源自*base.h*。 解決方法是其中一個目標更新版本 （更一致） 版本的 Windows SDK 或將專案屬性**C/c + +** > **語言** > **一致性模式：否**(此外，如果 **/permissive--** 會顯示在專案屬性**C/c + +** > **命令列**下**其他選項**，再將它刪除)。

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>如何解決建置錯誤 「 C + + /cli WinRT VSIX 不再提供專案的建置支援。  請新增 Microsoft.Windows.CppWinRT Nuget 套件的專案參考 」？
安裝**Microsoft.Windows.CppWinRT** NuGet 套件納入您的專案。 如需詳細資訊，請參閱 < [VSIX 擴充功能的舊版](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>需要何種才能 C + + /cli WinRT Visual Studio 擴充功能 (VSIX)？
如需版本 1.0.190128.4 的 VSIX 擴充功能和更新版本，請參閱[Visual Studio 支援 C + /cli WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。 針對其他版本，請參閱[VSIX 擴充功能的舊版](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

## <a name="whats-a-runtime-class"></a>什麼是*執行階段類別*？
執行階段類別是可透過現代化 COM 介面啟動與使用的類型，一般經過可執行的界限。 不過，執行階段類別也可在實作它的編譯單位中使用。 您可以在介面定義語言 (IDL) 中宣告執行階段類別，並可以在使用 C++/WinRT 的標準 C++ 中實作它。

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>*投影類型*和*實作類型*的意思是什麼？
如果您只是*使用*一個 Windows 執行階段類別 (執行階段類別)，則您可以專用*投影類型*來處理。 C++/WinRT 是一個*語言投影*，因此投影類型是 Windows 執行階段介面一部分，其使用 C++/WinRT *投影*到 C++ 裡。 如需詳細資訊，請參閱 <<c0> [ 取用的 Api，使用 C + + /cli WinRT](consume-apis.md)。

*實作類型*包含執行階段類別的實作，因此它僅在實作執行階段類別的專案中可用。 當您在實作執行階段類別的專案中執行時 (Windows 執行階段元件專案，或使用 XAML UI 的專案)，請務必將熟悉適用於執行階段類別的實作類型間的區別，且代表執行階段類別的投影類型投影至 C++/WinRT。 如需詳細資訊，請參閱 [使用 C++/WinRT 撰寫 API](author-apis.md)。

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>我需要在執行階段類別的 IDL 中宣告一個建構函式嗎？
只有當設計此執行階段類別從其實作編譯單位之外來使用 (它是 Windows 執行階段元件，旨在供給 Windows 執行階段用戶端應用程式的一般使用)。 如需 IDL 中完整的目的與宣告建構函式結果的詳細資訊，請參閱 [執行階段類別建構函式](author-apis.md#runtime-class-constructors)。

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>為什麼連結器提供 「 LNK2019:無法解析的外部符號 」 錯誤？
如果無法解析的符號是來自 C++/WinRT 投影的 Windows 命名空間標頭的 API (在 **winrt** 命名空間中)，則API 在您已包含的標頭中是向前宣告，但其定義是在您尚未包含的標頭中。 包含為 API 命名空間命名的標頭，並且重新建置。 如需詳細資訊，請參閱 [C++/WinRT 投影標頭](consume-apis.md#cwinrt-projection-headers)。

如果無法解析的符號是 Windows 執行階段可用的函式，如[RoInitialize](https://msdn.microsoft.com/library/br224650)，則您必須明確地連結[WindowsApp.lib](/uwp/win32-and-com/win32-apis)傘程式庫專案中的。 C++/WinRT 投影仰賴部分這些免費 (非成員) 函式和進入點。 如果您使用其中一個 [C++/WinRT Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix) 投影來為您的應用程式轉譯，則 `WindowsApp.lib` 會自動為您連結。 否則，您也可以使用專案連結設定來包括它，或在原始程式碼中執行它。

```cppwinrt
#pragma comment(lib, "windowsapp")
```

很重要，您會解析您可以藉由連結的任何連結器錯誤**WindowsApp.lib**而不是替代的靜態連結程式庫，否則您的應用程式將不會傳遞[Windows 應用程式認證套件](../debug-test-perf/windows-app-certification-kit.md) Visual Studio 和 Microsoft Store 驗證提交 （亦即，將因此無法成功擷取到 Microsoft Store 應用程式） 使用的測試。

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>我是否應該實作 [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)，如果是，該如何進行？
如果在其解構程式中您有釋出資源的執行階段類別，且設計該執行階段類別從其實作編譯單位之外使用 (它是 Windows 執行階段元件，旨在供給 Windows 執行階段用戶端應用程式的一般使用)，我們建議您也實作 **IClosable** 以便支援不確定完成的語言使用您的執行階段類別。 請確定不論是否解構函式都會釋出您的資源，[**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close)，或兩者都呼叫。 可以任意呼叫 **IClosable::Close** 數次。

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>我需要在我使用的執行階段類別上呼叫 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) 嗎？
**IClosable** 存在以支援不確定完成的語言。 因此，您就不應該呼叫 C++/WinRT 的 **IClosable::Close**，除非在非常少數案例中，牽涉到關機競爭或半致命採用。 如果您正使用 **Windows.UI.Composition** 類型，以此為例，您可能會遇到想在一組序列中處置物件的案例，允許 C++/WinRT 包裝函式的解構函式為您執行工作，做為替代方案。

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>我可以使用 LLVM/Clang 來編譯 C++/WinRT 嗎？
我們對於 C++/WinRT 不支援 LLVM 和 Clang toolchain，但我們在內部使用它來驗證 C++/WinRT 的標準一致性。 例如，如果您想要模擬我們在內部的操作，您可能會嘗試實驗，例如如下所述。

移至 [LLVM 下載頁面](https://releases.llvm.org/download.html)，尋找**下載 LLVM 6.0.0** > **預先建置的二進位檔**，然後下載 **Clang for Windows (64 位元)**。 在安裝期間，選擇新增 LLVM 到 PATH 系統變數，讓您可以從命令提示字元叫用它。 基於此實驗的目的，您可以略過 (如果您看到) 任何「找不到 MSBuild 工具組目錄」和/或「MSVC 整合安裝失敗」錯誤。 有各種不同的方式可叫用 LLVM/Clang;，以下範例顯示的只是一種方式。

```
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

Visual Studio 是我們支援和為 C++/WinRT 建議的開發工具。 請參閱[Visual Studio 支援 C + /cli WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>為什麼沒有產生的實作函式，則為唯寫屬性`const`限定詞？
當您宣告中的唯讀屬性[MIDL 3.0](/uwp/midl-3/)，您所料`cppwinrt.exe`工具，來為您產生實作函式會`const`-完整 (const 函式視為*此*為 const 的指標)。

我們絕對建議使用 const，可能的話，但`cppwinrt.exe`工具本身不會嘗試實作的相關函式理論上可能 const，而且這不可能的原因。 您可以選擇讓您實作的函式的任何 const，如此範例所示。

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

您可以移除所`const`上的限定詞**ToString**如果您決定您要變更它的實作中一些物件狀態。 但讓每個成員函式 const 或非 const，不可同時為兩者。 換句話說，不多載實作函式上`const`。

除了您實作的函式，另一個其他放置其中 const 進入圖片最初是在 Windows 執行階段函式的投影。 請考慮這段程式碼。

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

呼叫**ToString**上述**移至宣告**Visual Studio 中的命令顯示 Windows 執行階段的投影**IStringable::ToString**於 C + + /cliWinRT 看起來像這樣。

```
winrt::hstring ToString() const;
```

上投影函式都是常數，無論您選擇如何符合您的實作，這些項目。 在幕後，投影會呼叫應用程式二進位介面 (ABI)，透過 COM 介面指標呼叫的數量。 唯一狀態預計**ToString**互動與在 COM 介面指標，而且當然有不需要修改該指標，因此函式是 const。 這可讓您它不會變更任何項目有關的保證**IStringable**參考，您會透過呼叫，並確保您可以呼叫**ToString**即使有的const參考**IStringable**。

了解，這些範例中的`const`是實作詳細資料，C + /cli WinRT 投影和實作，它們是由組成您的權益的程式碼防護。 沒有這類的`const`COM 或 Windows 執行階段的 ABI (成員函式）。

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>您是否有任何建議用於減少程式碼大小的 C + + /cli WinRT 二進位檔？
使用 Windows 執行階段物件，您應該避免因為它可以在您的應用程式上有負面影響，造成超過所產生的多個二進位程式碼，如下所示的程式碼撰寫模式。

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

在 Windows 執行階段環境中，編譯器就無法快取的值`c()`或間接透過會呼叫每個方法的介面 ('。 ')。 除非您介入，這會導致更多虛擬呼叫和參考計數的額外負荷。 上述模式可以輕鬆地產生兩倍，絕對必要的程式碼。 相反地，偏好的模式，您可以在任何地方，如下所示。 它會產生更少的程式碼，及其也可大幅改善您的執行的階段效能。

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

建議的模式，如上所示適用於不只是 C + + /cli WinRT 但所有的 Windows 執行階段語言投影。

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>如何為類型開啟字串&mdash;巡覽，例如？
在結尾[瀏覽檢視程式碼範例](/windows/uwp/design/controls-and-patterns/navigationview#code-example)(大部分是在C#)，沒有 C + + /cli WinRT 程式碼片段顯示如何執行這項操作。

> [!NOTE]
> 如果本主題不到解答您的問題，則您可能會發現說明，請造訪[Visual Studio c + + 開發人員社群](https://developercommunity.visualstudio.com/spaces/62/index.html)，或使用[ `c++-winrt` Stack Overflow 上標記](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt)。
