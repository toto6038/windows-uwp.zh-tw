---
description: 查看最近的新增項目與改良功能，以及 C++/WinRT 2.0 和 Windows SDK 10.0.17763.0 版的新聞和變更。
title: C++/WinRT 的新功能
ms.date: 03/16/2020
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 新聞, 新功能
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f961fba5c4b5aca316da257ba66df17b504b3264
ms.sourcegitcommit: 539b428bcf3d72c6bda211893df51f2a27ac5206
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2021
ms.locfileid: "102629346"
---
# <a name="whats-new-in-cwinrt"></a>C++/WinRT 的新功能

隨著 C++/WinRT 後續版本的發行，本主題說明新功能和變更內容。

## <a name="rollup-of-recent-improvementsadditions-as-of-march-2020"></a>2020 年 3 月為止的最新改良/新增彙總套件

### <a name="up-to-23-shorter-build-times"></a>組建時間縮短達 23%

C++/WinRT 和 C++ 編譯器小組已共同合作，盡可能縮短組建時間。 我們仔細研究編譯器分析，以找出重組 C++/WinRT 內部結構的方法，來協助 C++ 編譯器消除額外的編譯時間，以及改進 C++ 編譯器本身以處理 C++/WinRT 程式庫的方法。 C++/WinRT 已針對編譯器最佳化；而編譯器也已針對 C++/WinRT 最佳化。

以最差的情況為例，組建預先編譯的標頭 (PCH)，其中包含每個單一 C++/WinRT 投影命名空間標頭。

| 版本 | PCH 大小 (位元組) | 次數 |
| - | - | - |
| 7 月的 C++/WinRT，含 Visual C++ 16.3 | 3,004,104,632 | 31 |
| C++/WinRT 2.0.200316.3 版，含 Visual C++ 16.5 | 2,393,515,336 | 24 |

大小縮減 20%，組建時間減少 23%。

### <a name="improved-msbuild-support"></a>已改善 MSBuild 支援

我們已投注大量心力來改善對各種不同方案的 [MSBuild](/visualstudio/msbuild/msbuild) 支援。

### <a name="even-faster-factory-caching"></a>更快速的處理站快取

我們改進了處理站快取的內嵌，以便更好地內嵌最忙碌路徑，從而加快執行速度。

這項改進並不會影響程式碼大小 (如下方[最佳化 EH 程式碼產生](#optimized-exception-handling-eh-code-generation)中所述)，如果您的應用程式大量使用 C++ 例外狀況處理程式，則可以使用 `/d2FH4` 選項來壓縮二進位檔，依預設，使用 Visual Studio 2019 16.3 和更高版本建立新專案中已啟用該功能。

### <a name="more-efficient-boxing"></a>更有效率的 Boxing

在 XAML 應用程式中使用時，[**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 現在更有效率 (請參閱 [Boxing 和 Unboxing](./boxing.md))。 執行大量 Boxing 的應用程式也會注意到程式碼大小的縮減。

### <a name="support-for-implementing-com-interfaces-that-implement-iinspectable"></a>支援執行 IInspectable 的實作 COM 介面

如果您需要執行一個 (非 Windows 執行時間) COM 介面，而該介面剛好實作了 [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)，則現在可以使用 C++/WinRT 來執行此動作。 請參閱[實作 IInspectable 的 COM 介面](https://github.com/microsoft/xlang/pull/603)。

### <a name="module-locking-improvements"></a>模組鎖定改進

對模組鎖定的控制現在既可以自訂託管方案，也可以完全消除模組層級的鎖定。 請參閱[模組鎖定改進](https://github.com/microsoft/xlang/pull/583)。

### <a name="support-for-non-windows-runtime-error-information"></a>支援非 Windows 執行階段錯誤資訊

某些 API (即使是某些 Windows 執行階段 API) 會報告錯誤，而不會使用 Windows 執行階段錯誤來源 API。 在這種情況下，C++/WinRT 現在回到使用 COM 錯誤資訊。 請參閱[非 WinRT 錯誤資訊的 C++/WinRT 支援](https://github.com/microsoft/xlang/pull/582)。

### <a name="enable-c-module-support"></a>啟用 C++ 模組支援 

C++ 模組支援已恢復，但僅適用於實驗性表單。 這項功能在 C++ 編譯器中尚未完成。

### <a name="more-efficient-coroutine-resumption"></a>協同程式恢復更有效率

C++/WinRT 協同程式已經順利執行，但我們會繼續尋找改善的方式。 請參閱[提高協同程式恢復的可擴縮性](https://github.com/microsoft/xlang/pull/546)。

### <a name="new-when_all-and-when_any-async-helpers"></a>新增 **when_all** 和 **when_any** 非同步協助程式

**when_all** 協助程式函式建立了 [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) 物件，該物件在所有提供的可等候都完成時就會完成。 **when_any** 協助程式建立了 **IAsyncAction**，其在所有提供的可等候都完成時就會完成。 

請參閱[新增 when_any 非同步協助程式](https://github.com/microsoft/xlang/pull/520)和[新增 when_all 非同步協助程式](https://github.com/microsoft/xlang/pull/516)。

### <a name="other-optimizations-and-additions"></a>其他最佳化和新增功能

此外，還引進了許多錯誤修正和一些最佳化和新增功能，包括各種改善措施，以簡化偵錯功能並將內部和預設實作最佳化。 如需完整清單，請進入此連結：[https://github.com/microsoft/xlang/pulls?q=is%3Apr+is%3Aclosed](https://github.com/microsoft/xlang/pulls?q=is%3Apr+is%3Aclosed)。

## <a name="news-and-changes-in-cwinrt-20"></a>C++/WinRT 2.0 的新聞和變更

如需關於 [C++/WinRT Visual Studio Extension (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)、[Microsoft.Windows.CppWinRT NuGet package](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) 和 `cppwinrt.exe` 工具的詳細資訊 &mdash; 包括如何取得並加以安裝 &mdash; 請參閱[適用於 C++/WinRT、XAML、VSIX 擴充功能，以及 NuGet 封裝的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>C++/WinRT Visual Studio 擴充功能 (VSIX) 2.0 版的變更

- 偵錯視覺特效播放器目前支援 Visual Studio 2019；並繼續支援 Visual Studio 2017。
- 已進行許多錯誤修正。

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>Microsoft.Windows.CppWinRT NuGet 封裝 2.0 版的變更

- `cppwinrt.exe` 工具目前包含在 Microsoft.Windows.CppWinRT NuGet 封裝中，此工具可視需要為每個專案產生平台投影標頭。 因此，`cppwinrt.exe` 工具不再依賴 Windows SDK (儘管基於相容性原因，此工具仍隨附 SDK)。
- `cppwinrt.exe` 現在會在每個平台/特定設定的中繼資料夾 ($IntDir) 之下產生投影標頭，以啟用平行組建。
- 如果您想要手動自訂專案檔案，C++/WinRT 組件支援 (屬性/目標) 現在已完整記載。 請參閱 Microsoft.Windows.CppWinRT NuGet 套件[讀我檔案](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing)。
- 已進行許多錯誤修正。

### <a name="changes-to-cwinrt-for-version-20"></a>C++/WinRT 2.0 版的變更

#### <a name="open-source"></a>開放原始碼

`cppwinrt.exe` 工具採用 Windows 執行階段中繼資料 (`.winmd`) 檔案，並從中產生以標頭檔案為基礎的標準 C++ 程式庫，其 *投影* 中繼資料中所述的 API。 如此一來，便可從 C++/WinRT 程式碼中使用取用這些 API。

此工具目前是完全開放的原始碼專案，可在 GitHub 上找到。 瀏覽 [Microsoft\/cppwinrt](https://github.com/microsoft/cppwinrt)。

#### <a name="xlang-libraries"></a>xlang 程式庫

可完全移植的僅標頭程式庫 (用於剖析 Windows 執行階段所用的 ECMA-335 中繼資料格式)，構成了所有 Windows 執行階段和 xlang 工具進步的基礎。 值得注意的是，我們使用了 xlang 程式庫從頭開始重新撰寫 `cppwinrt.exe` 工具。 如使提供了更準確的中繼資料查詢，解決了 C++/WinRT 語言投影的一些長期問題。

#### <a name="fewer-dependencies"></a>較低的相依性

由於 xlang 中繼資料讀取器，`cppwinrt.exe` 工具本身具有較低的相依性。 這使其更有彈性，並且可以在更多情況下使用 &mdash; 尤其是在受限制的建置環境中。 值得注意的是，它不再依賴 `RoMetadata.dll`。
 
以下為 `cppwinrt.exe` 2.0 的相依性。
 
- ADVAPI32.dll
- KERNEL32.dll
- SHLWAPI.dll
- XmlLite.dll

這些 DLL 不僅適用於 Windows 10，還能適用於 Windows 7，甚至適用於 Windows Vista。 如果您想要，執行 Windows 7 的舊組建伺服器現在可以執行 `cppwinrt.exe`，為您的專案產生 C++ 標頭。 透過少許工作，您甚至可以[在 Windows 7 上執行 C++/WinRT](https://github.com/kennykerr/win7) (如果您有興趣的話)。

對照上述清單與 `cppwinrt.exe` 1.0 具備的下列相依性。

- ADVAPI32.dll
- SHELL32.dll
- api-ms-win-core-file-l1-1-0.dll
- XmlLite.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- api-ms-win-core-processenvironment-l1-1-0.dll
- RoMetadata.dll
- SHLWAPI.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-timezone-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll
- OLEAUT32.dll
- api-ms-win-core-winrt-error-l1-1-0.dll
- api-ms-win-core-winrt-error-l1-1-1.dll
- api-ms-win-core-winrt-l1-1-0.dll
- api-ms-win-core-winrt-string-l1-1-0.dll
- api-ms-win-core-synch-l1-1-0.dll
- api-ms-win-core-threadpool-l1-2-0.dll
- api-ms-win-core-com-l1-1-0.dll
- api-ms-win-core-com-l1-1-1.dll
- api-ms-win-core-synch-l1-2-0.dll 

#### <a name="the-windows-runtime-noexcept-attribute"></a>Windows 執行階段 `noexcept` 屬性

Windows 執行階段有新的 `[noexcept]` 屬性，您可用於裝飾 [MIDL 3.0](/uwp/midl-3/predefined-attributes) 中的方法與屬性。 此屬性表示支援不會讓您的實作擲回例外狀況的工具 (也不會傳回失敗的 HRESULT)。 如此語言投影就能免除掉用於處理意外狀況的額外負荷，通常這是為了支援可能失敗之應用程式二進位介面 (ABI) 呼叫所需，以此方式可讓程式碼的產生作業達到最佳化。

C++/WinRT 會產生取用和撰寫程式碼的 C++ `noexcept` 實作，充分運用此功能。 如果您有零失誤的 API 方法或屬性，而且需要顧慮程式碼大小，就建議您深入研究此屬性。

#### <a name="optimized-code-generation"></a>最佳化的程式碼產生

C++/WinRT 現在會產生更有效率的 C++ 原始程式碼 (幕後)，以便 C++ 編譯器產生最小且最有效的二進位程式碼。 許多改良功能的設計，目標都是避免不必要的回溯資訊，以期降低例外狀況處理的成本。 使用了大量 C++/WinRT 程式碼的二進位檔，大約可減少 4% 的程式碼大小。 由於指令數量減少，程式碼也會更有效率 (執行速度更快)。

這些改良功能依賴您可用的全新互通性功能。 作為資源擁有者的所有 C++/WinRT 類型，目前皆包含可直接獲取擁有權的建構函式，以避開先前的雙步驟方法。

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>最佳化的例外狀況處理 (EH) 程式碼產生

此變更補強了 Microsoft C++ 最佳化工具小組為了降低例外狀況處理成本所做的工作。 如果您在程式碼中大量使用應用程式二進位介面 (ABI) (例如 COM)，就會發現許多遵循此模式的程式碼。

```cpp
int32_t Function() noexcept
{
    try
    {
        // code here constitutes unique value.
    }
    catch (...)
    {
        // code here is always duplicated.
    }
}
```

C++/WinRT 本身為每個實作的 API 產生此模式。 有了數千個 API 函式，這裡的任何最佳化可能會產生重大影響。 在過去，最佳化工具不會偵測到那些 Catch 區塊皆完全相同，因此會圍繞每個 ABI 複製許多程式碼 (這又導致人們相信在系統程式碼中使用例外狀況會產生大型的二進位檔)。 但是，從 Visual Studio 2019 開始，C++ 編譯器會摺疊所有 catch funclet，並且只儲存唯一的 catch funclet。 結果是，重度依賴此模式的二進位檔程式碼大小進而減少了 18%。 不僅是現在的 EH 程式碼比起使用傳回程式碼更有效率，而且對於較大型二進位檔的顧慮已成為過去。

#### <a name="incremental-build-improvements"></a>累加建置改善

`cppwinrt.exe` 工具現在會將產生的標頭/來源檔案的輸出結果，與磁碟上任何現有檔案的內容進行比較，而且只有在檔案實際發生變更時，才會寫出檔案。 如此可大量節省磁碟 I/O 時間，並確保 C++ 編譯器不會將檔案視為「已變更」。 結果，在許多情況下避免或減少了重新編譯。

#### <a name="generic-interfaces-are-now-all-generated"></a>現在都是產生泛型介面

由於 xlang 中繼資料讀取器，C++/WinRT 現在可以從中繼資料產生所有參數化或泛型介面。 [Windows::Foundation::Collections::IVector\<T\>](/uwp/api/windows.foundation.collections.ivector_t_) 之類的介面現在會從中繼資料產生，而不是使用 `winrt/base.h` 手動撰寫。 結果是 `winrt/base.h` 的大小減少一半，而且在程式碼中產生最佳化 (這是透過手動方法難以達成的目標)。

> [!IMPORTANT]
> 目前範例所提供的這類介面會出現在各自的命名空間標頭中，而不是在 `winrt/base.h` 中。 因此，如果尚未這麼做，則必須納入適當的命名空間標頭，才能使用該介面。

#### <a name="component-optimizations"></a>元件最佳化

此更新新增對 C++/WinRT 幾個其他選擇加入最佳化的支援，如下列章節所述。 由於這些最佳化是重大變更 (您可能需要對支援進行微幅變更)，因此，您需要明確啟用這些最佳化。 在 Visual Studio 中，將 [通用屬性]   > [C++/WinRT]   > [最佳化]  設為 [是]  。 此動作會將 `<CppWinRTOptimized>true</CppWinRTOptimized>` 新增至您的專案檔。 而且這與從命令列叫用 `cppwinrt.exe` 時，新增 `-opt[imize]` 參數的效果一樣。

依預設，新專案 (來自專案範本) 將使用 `-opt`。

##### <a name="uniform-construction-and-direct-implementation-access"></a>統一建構，以及直接實作存取

這些兩種最佳化可讓您的元件直接存取其實作類型，即使它只使用投影的類型。 如果您只是想要使用公用 API 表面，則不需要使用 [**make**](/uwp/cpp-ref-for-winrt/make)[**make_self**](/uwp/cpp-ref-for-winrt/make-self)，也不需要 [**get_self**](/uwp/cpp-ref-for-winrt/get-self)。 您的呼叫將編譯成直接呼叫實作，甚至可能完全內嵌。

如需詳細資訊和程式碼範例，請參閱[加入統一建構和直接實作存取](./author-apis.md#opt-in-to-uniform-construction-and-direct-implementation-access)。

##### <a name="type-erased-factories"></a>已清除類型的處理站

此最佳化可避免 `module.g.cpp` 中的 #include 相依性，因此不需要在每次變更任何單一實作類別時重新編譯。 結果會提高建置效能。

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>更聰明且更有效率的 `module.g.cpp` 適用於具有多個程式庫的大型專案

`module.g.cpp` 檔案現在也包含兩個額外的可組合協助程式，名稱為 **winrt_can_unload_now** 和 **winrt_get_activation_factory**。 這些設計用於較大的專案，其中 DLL 由許多程式庫組成，每個程式庫都有自己的執行階段類別。 在此情況下，您需要將 DLL 的 **DllGetActivationFactory** 和 **DllCanUnloadNow** 手動拼接在一起。 這些協助程式可避免假性來源錯誤，讓您更容易進行此操作。 `cppwinrt.exe` 工具的 `-lib` 旗標也可用於為每個單獨的程式庫提供自己的前序 (而不 `winrt_xxx`)，以便每個程式庫函數可以單獨命名，進而明確地組合。

#### <a name="coroutine-support"></a>協同程式支援

自動包含協同程式支援。 先前，支援位於多個地方，而感覺受到過多限制。 然後對於 v2.0 來說，曾暫時需要 `winrt/coroutine.h` 標頭檔案，但如今已不再需要。 由於現在已出現 Windows 執行階段非同步介面，而不是手寫，因此現在是位於 `winrt/Windows.Foundation.h` 中。 除了更容易維護和支援以外，這表示不再需要將協同程式 (例如 [**resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)) 附加至特定命名空間標頭的結尾。 如此反而可以更自然地納入其相依性。 這可以進一步讓 **resume_foreground** 不僅支援在特定的 [**Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) 上繼續進行，現在也可以支援在特定的 [**Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue) 上繼續進行。 以前只能支援一個，而無法同時支援兩者，因為定義只能位於一個命名空間中。

以下是 **DispatcherQueue** 支援的範例。

```cppwinrt
...
#include <winrt/Windows.System.h>
using namespace Windows::System;
...
fire_and_forget Async(DispatcherQueueController controller)
{
    bool queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(queued);

    // This is just to simulate queue failure...
    co_await controller.ShutdownQueueAsync();

    queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(!queued);
}
```

協同程式協助程式現在也用 `[[nodiscard]]` 裝置，藉此提高其可用性。 如果您忘記 `co_await` (或者不知道必須如此做)，以讓其得以運作，則由於 `[[nodiscard]]`，此類錯誤現在會產生編譯器警告。

#### <a name="help-with-diagnosing-direct-stack-allocations"></a>協助診斷直接 (堆疊) 配置

由於投影和實作類別名稱 (依預設) 相同，而且只有命名空間不同，因此可能會誤用，並且可能會不小心在堆疊上建立實作，而不是使用協助程式的 [**make**](/uwp/cpp-ref-for-winrt/make) 系列。 在某些情況下，這可能很難診斷，因為物件可能會遭受破壞，而未完成的參照仍在執行中。 對於偵錯組件，判斷提示現在選擇此項目。 雖然判斷提示沒有偵測到協同程式中的堆疊配置，但它仍然有助於擷取大部分此類錯誤。

如需詳細資訊，請參閱[診斷直接配置](./diag-direct-alloc.md)。

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>改善擷取協助程式，以及 variadic 委派

此更新藉由支援投影類型來修正擷取協助程式的限制。 當它們傳回投影類型時，這會隨著 Windows 執行階段互通性 API 而出現。

使更新也在建立 variadic (非 Windows 執行階段) 委派時，新增了 [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) 和 [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) 的支援。

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>在解構期間支援遞延解構和安全 QI

在執行階段類別物件的解構函式中，呼叫可暫時壓縮參考計數的方法並不稀奇。 當參考計數會回到零時，物件會第二次解構。 在 XAML 應用程式中，您可能需要在解構函式中執行 [**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) (QI)，以便在階層中向上或向下呼叫某些清除實作。 但是，物件的參考計數已經達到零，因此 QI 也會構成參考計數反彈。

此更新新增對消除彈跳引用計數的支援，確保一旦達到零之後，就永遠無法重新啟動；同時仍然允許將 QI 用於解構期間所需的任何暫存。 在某些 XAML 應用程式/控制項中，無法避免此過程，而 C++/WinRT 現在具有彈性。

您可以藉由在您的實作類型上提供靜態 **final_release** 函式來延遲解構。 物件的最後一個剩餘指標 (**std::unique_ptr** 形式) 會傳遞至您的 **final_release**。 然後，您可以選擇將該指標的擁有權移至其他某些內容。 您可以安全地對指標使用 QI，而不觸發雙重解構。 但參考計數的淨變更在解構物件之處必須是零。

**final_release** 的傳回值可以是 `void`，也就是 [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) 或 **winrt::fire_and_forget** 等非同步作業物件。

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    hstring ToString()
    {
        return L"Sample";
    }

    ~Sample()
    {
        // Called when the unique_ptr below is reset.
    }

    static void final_release(std::unique_ptr<Sample> self) noexcept
    {
        // Move 'self' as needed to delay destruction.
    }
};
```

在以下範例中，(最後一次) 釋出 **MainPage** 之後，會呼叫 **final_release**。 該函式花費五秒鐘等待 (在執行緒集區上)，然後使用頁面的 **Dispatcher** (需要 QI/AddRef/Release 進行運作) 以繼續進行。 接著，函式會清除該 UI 執行緒上的資源。 最後，清除 **unique_ptr**，這會導致實際呼叫 **MainPage** 解構函式。 即使在該解構函式中，也會呼叫 **DataContext**，其需要適用於 **IFrameworkElement** 的 QI。

您不需要實作 **final_release** 做為協同程式。 但這確實有效，而且可輕鬆地將解構移動到不同的執行緒，也就是此範例中的狀況。

```cppwinrt
struct MainPage : PageT<MainPage>
{
    MainPage()
    {
    }

    ~MainPage()
    {
        DataContext(nullptr);
    }

    static IAsyncAction final_release(std::unique_ptr<MainPage> self)
    {
        co_await 5s;

        co_await resume_foreground(self->Dispatcher());
        co_await self->resource.CloseAsync();

        // The object is destructed normally at the end of final_release,
        // when the std::unique_ptr<MyClass> destructs. If you want to destruct
        // the object earlier than that, then you can set *self* to `nullptr`.
        self = nullptr;
    }
};
```

如需詳細資訊，請參閱[延遲解構](./details-about-destructors.md#deferred-destruction)。

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>已改善對 COM 樣式單一介面繼承的支援

與 Windows 執行階段程式設計一樣， C++/WinRT 也用於撰寫和取用僅限 COM 的 API。 此更新可讓您實作存在介面階層的 COM 伺服器。 Windows 執行階段不需要這麼做；但它對於某些 COM 實作是必要的。

#### <a name="correct-handling-of-out-params"></a>正確處理 `out` 參數

使用 `out` 參數可能很棘手；尤其是 Windows 執行階段陣列。 透過此更新，涉及 `out` 參數和陣列方面時，C++/WinRT 可更加強大且彈性地抵禦錯誤；無論這些參數是透過語言投影到達，還是來自使用原始 ABI 的 COM 開發人員，以及犯下未利用一致的方式對變數進行初始化的錯誤。 在任一情況下，涉及將投影類型移交給 ABI 時 (藉由記住釋出任何資源)，以及涉及歸零或清除通過 ABI 到達的參數時，C++/WinRT 現在皆會做出正確的操作。

#### <a name="events-now-handle-invalid-tokens-reliably"></a>事件現在可靠地處理無效權杖

[**winrt::event**](/uwp/cpp-ref-for-winrt/event) 實作現在可以依正常程序處理使用無效權杖值 (陣列中不存在的值) 來呼叫其 **remove** 方法的情況。

#### <a name="coroutine-local-variables-are-now-destroyed-before-the-coroutine-returns"></a>協同程式區域變數現在會在協同程式傳回之前終結

實作協同程式類型的傳統方式可能會讓協同程式內的區域變數在協同程式傳回/完成 *之後* (而不是在最終暫停之前) 終結。 現在必須延遲到最後暫停，才能重新恢復任何等候程序，以避免這個問題並產生其他好處。

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Windows SDK 版本 10.0.17763.0 (Windows 10 版本 1809) 中的新聞和變更

下表包含 Windows SDK 版本 10.0.17763.0 (Windows 10 版本 1809) 中 C++/WinRT 的新聞和變更。

| 新增或變更的功能 | 其他資訊 |
| - | - |
| **重大變更**。 若要進行編譯，C++/WinRT 不會根據 Windows SDK 的標頭。 | 請參閱下方的[從 Windows SDK 標頭檔案隔離](#isolation-from-windows-sdk-header-files)。 |
| Visual Studio 專案系統格式已變更。 | 請參閱下方[如何將 C++/WinRT 專案的目標重定為 Windows SDK 的較新版本](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)。 |
| 有一些新函式和基底類別有助於將集合物件傳遞給 Windows 執行階段函式，或者實作您自己的集合屬性和集合類別。 | 請參閱[使用 C++/WinRT 的集合](collections.md)。 |
| 您可以將 [{Binding}](../xaml-platform/binding-markup-extension.md) 標記延伸與 C++/WinRT 執行階段類別搭配使用。 | 如需更多資訊與程式碼範例，請參閱[資料繫結概觀](../data-binding/data-binding-quickstart.md)。 |
| 支持取消協同程式可讓您註冊取消回呼。 | 如需更多資訊和程式碼範例，請參閱[取消非同步作業，以及取消回呼](concurrency-2.md#canceling-an-asynchronous-operation-and-cancellation-callbacks)。 |
| 建立指向成員函式的委派時，可以在註冊處理常式的位置建立對目前物件 (而不是原始的「this」  指標) 的強式參考或弱式參考。 | 如需更多資訊和程式碼範例，請參閱 [使用事件處理委派安全地存取「this」  指標](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)小節的 **如果您使用成員函式作為委派** 子小節。 |
| 修正 Visual Studio 對於 C++ 標準改善一致性未涵蓋的錯誤。 LLVM 和 Clang 工具鏈也可以更好地用於驗證 C++/WinRT 的標準一致性。 | 您將不再遇到[為什麼無法編譯我的新專案？我使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，以及 SDK 版本 17134](faq.yml#why-won-t-my-new-project-compile--i-m-using-visual-studio-2017--version-15-8-0-or-higher---and-sdk-version-17134) 中描述的問題 |

其他變更。

- **重大變更**。 [**winrt::get_abi(winrt::hstring const&)**](/uwp/cpp-ref-for-winrt/get-abi) 現在會傳回 `void*`，而不是 `HSTRING`。 您可以使用 `static_cast<HSTRING>(get_abi(my_hstring));` 以取得 HSTRING。 請參閱[交互操作 ABI 的 HSTRING](interop-winrt-abi.md#interoperating-with-the-abis-hstring)。
- **重大變更**。 [**winrt::put_abi(winrt::hstring&)**](/uwp/cpp-ref-for-winrt/put-abi) 現在會傳回 `void**`，而不是 `HSTRING*`。 您可以使用 `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` 以取得 HSTRING*。 請參閱[交互操作 ABI 的 HSTRING](interop-winrt-abi.md#interoperating-with-the-abis-hstring)。
- **重大變更**。 HRESULT 現在投影為 **winrt::hresult**。 如果您需要 HRESULT (以進行類別檢查，或支援類型特性)，您可以 `static_cast`**winrt::hresult**。 否則，只要您在加入任何 C++/WinRT 標頭之前，先加入 `unknwn.h`，**winrt::hresult** 會轉換成 HRESULT。
- **重大變更**。 GUID 現在投影為 **winrt::guid**。 對於您實作的 API，必須為 GUID 參數使用 **winrt::guid**。 否則，只要您在加入任何 C++/WinRT 標頭之前，先加入 `unknwn.h`，**winrt::guid** 會轉換成 GUID。 請參閱[交互操作 ABI 的 GUID 結構](interop-winrt-abi.md#interoperating-with-the-abis-guid-struct)。
- **重大變更**。 [**winrt::handle_type 建構函式**](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor)已透過明確宣告而強化 (現在很難使用它撰寫錯誤的程式碼)。 如果需要指派原始的控制碼值，請改由呼叫 [**handle_type::attach 函式**](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function)。
- **重大變更**。 **WINRT_CanUnloadNow** 和 **WINRT_GetActivationFactory** 的簽章已變更。 您不得宣告這些函式。 相反，加入 `winrt/base.h` (如果包含任何 C++/WinRT Windows 命名空間標頭檔案，則會自動包含)，以包含這些函式的宣告。
- 對於 [**winrt::clock struct**](/uwp/cpp-ref-for-winrt/clock)，**from_FILETIME/to_FILETIME** 已過時，建議使用 **from_file_time/to_file_time**。
- 預期使用 **IBuffer** 參數的簡化 API。 大部分的 API 偏好集合或陣列。 但我們認為應該讓您更輕鬆地呼叫依賴 **IBuffer** 的 API。 此更新可讓您直接存取 **IBuffer** 實作背後的資料。 其使用與 C++ 標準程式庫容器所用相同的資料命名慣例。 該慣例也避免了通常以大寫字母開頭的中繼資料名稱產生衝突。
- 改善的程式碼產生：各種改善項目可縮減程式碼大小、改善內嵌，並最佳化處理站快取。
- 已移除不必要的遞迴。 當命令列指向資料夾而非特定的 `.winmd` 時，`cppwinrt.exe` 工具不會再以遞迴方式搜尋 `.winmd` 檔案。 `cppwinrt.exe` 工具現在也可以更有智慧的方式處理重複項目，使其更容易復原使用者錯誤，以及格式錯誤的 `.winmd` 檔案。
- 強化的智慧型指標。 之前，當移動指派新值時，事件撤銷無法撤銷。 這有助於揭露智慧型指標類別無法可靠地處理自我指派的問題；植根於 [**winrt::com_ptr 結構範本**](/uwp/cpp-ref-for-winrt/com-ptr)。 **winrt::com_ptr** 已修正，而且事件撤銷已修正為正確處理移動語意，以便在指派時撤銷。

> [!IMPORTANT]
> 對於版本 1.0.181002.2 中的 [C++/WinRT Visual Studio 延伸模組 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) 已進行重要變更，之後在版本 1.0.190128.4 中也進行了變更。 如需這些變更的詳細資料，以及如何影響您現有的專案，請參閱[適用於 C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)和[舊版 VSIX 延伸模組](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

### <a name="isolation-from-windows-sdk-header-files"></a>從 Windows SDK 標頭檔案隔離

這可能是您程式碼的重大變更。

若要進行編譯，C++/WinRT 不會再根據 Windows SDK 的標頭檔案。 C 執行階段程式庫 (CRT) 和 C++ 標準範本程式庫 (STL) 中的標頭檔案也不包含任何 Windows SDK 標頭。 這樣可以提高標準相容性，避免不小心產生的相依性，並可大幅減少必須防範的巨集數。

這種獨立性表示 C++/WinRT 現在更具可移植性和標準相容性，而且進一步推動成為跨編譯器和跨平台程式庫的可能性。 這也表示 C++/WinRT 標頭不會對巨集造成負面影響。

如果您之前將其留在 C++/WinRT 中，以在專案中包含任何 Windows 標頭，那麼您現在需要自行包含標頭。 在任何情況下，最佳做法永遠是明確包含您所需的標頭，而不是將其留給另一個程式庫以包含標頭。

目前，唯一的例外狀況為 Windows SDK 標頭檔案隔離是內建函式和數值。 這些最後剩餘的相依性沒有已知問題。

在專案中，您可以視需要使用 Windows SDK 標頭以重新啟用互通性。 例如，您可以想要實作 COM 介面 (根植於 [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown))。 對於該範例，在包含任何 C++/WinRT 標頭之前包含 `unknwn.h`。 這樣做會造成 C++/WinRT 基礎程式庫啟用各種勾點以支援傳統的 COM 介面。 如需程式碼範例，請參閱[使用 C++/WinRT 撰寫 COM 元件](author-coclasses.md)。 同樣地，請明確納入任何其他 Windows SDK 標頭，這些標頭會宣告您要呼叫的類型及/或函式。

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>如何將 C++/WinRT 專案的目標重定為 Windows SDK 的較新版本

重定專案目標的方法，所產生的編譯器和連接器問題可能最少，卻也最耗費人力。 該方法涉及建立新專案 (針對您選擇的 Windows SDK 版本)，然後將檔案從舊專案複製到新專案。 您可以複製舊 `.vcxproj` 和 `.vcxproj.filters` 檔案的部分，以省下在 Visual Studio 中新增檔案的過程。

但是，有另外兩種方法可以在 Visual Studio 中重定專案目標。

- 移至專案屬性 [一般]  \>[Windows SDK 版本]  ，然後選取 [所有組態]  和 [所有平台]  。 將 **Windows SDK 版本** 設為您要設定目標的版本。
- 在 [方案總管]  中，在專案節點上按一下滑鼠右鍵，按一下 [重定專案目標]  ，選擇您要重定目標的版本，然後按一下 [確定]  。

如果使用這兩種方法之一後，遇到任何編譯器或連接器錯誤，則可以先嘗試清除解決方案 ([建置]   > [清除解決方案]  及/或手動刪除所有暫存資料夾和檔案)，然後再嘗試建置。

如果 C++ 編譯器產生「*錯誤 C2039：'IUnknown': 不是 '\`global namespace''* 的成員」，則將 `#include <unknwn.h>` 新增至 `pch.h` 檔案的頂端 (在您加入任何 C++/WinRT 標頭之前)。

您可能需要在此之後加入 `#include <hstring.h>`。

如果 C++ 連接器產生「*錯誤 LNK2019：函式 _VSDesignerCanUnloadNow@0* 中參照無法解析的外部符號 _WINRT_CanUnloadNow@0」，您可以藉由將 `#define _VSDESIGNER_DONT_LOAD_AS_DLL` 新增至 `pch.h` 檔案，以解決問題。
