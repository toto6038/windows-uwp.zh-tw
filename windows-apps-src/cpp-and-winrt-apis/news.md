---
description: C++/WinRT 的新聞和變更。
title: 什麼是新的 C + /cli WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 投影、 新聞、 什麼的 new
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a84e118d988d8bf6a7d26eba7d5dd009c7ad44f3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360139"
---
# <a name="whats-new-in-cwinrt"></a>什麼是新的 C + /cli WinRT

## <a name="news-and-changes-in-cwinrt-20"></a>新聞和變更，在C++WinRT 2.0

如需詳細資訊[ C++WinRT Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix)，則[Microsoft.Windows.CppWinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)，而`cppwinrt.exe` 工具&mdash;包括如何取得並安裝它們&mdash;請參閱 < [Visual Studio 支援C++/WinRT、 XAML、 VSIX 擴充功能和 NuGet 套件](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>若要變更C++WinRT Visual Studio 擴充功能 (VSIX) 2.0 版

- 偵錯視覺化檢視現在支援 Visual Studio 2019;以及繼續支援 Visual Studio 2017。
- 已進行許多 bug 修正。

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>Microsoft.Windows.CppWinRT NuGet 封裝 2.0 版的變更

- `cppwinrt.exe`工具現在隨附於 Microsoft.Windows.CppWinRT NuGet 封裝中，與此工具會產生隨選的每個專案的平台 投影標頭。 因此，`cppwinrt.exe`工具不再相依於 Windows SDK （雖然此工具仍會隨附的 SDK 相容性的理由）。
- `cppwinrt.exe` 現在會產生每個平台/組態特有中繼資料夾下 ($IntDir) 啟用平行建置的投影標頭。
- C++/WinRT 建置支援 （屬性/目標） 現在已完整記載，萬一您想要以手動方式自訂您的專案檔。 請參閱[Microsoft.Windows.CppWinRT NuGet 套件](https://github.com/Microsoft/xlang/blob/master/src/package/cppwinrt/nuget/readme.md)。
- 已進行許多 bug 修正。

### <a name="changes-to-cwinrt-for-version-20"></a>若要變更C++2.0 版的 /WinRT

#### <a name="open-source"></a>開放原始碼

`cppwinrt.exe`工具會採用 Windows 執行階段中繼資料 (`.winmd`) 檔案，並從它產生的標頭檔案為基礎的標準C++程式庫的*專案*中繼資料中所述的 Api。 如此一來，您可以使用這些 Api，從您C++/WinRT 程式碼。

這項工具現在是完全開放原始碼專案，可在 GitHub 上。 請瀏覽[Microsoft\/xlang](https://github.com/Microsoft/xlang)，然後按一下 在以**src** > **工具** > **cppwinrt**。

#### <a name="xlang-libraries"></a>xlang 程式庫

（適用於剖析 Windows 執行階段所用的 ECMA-335 中繼資料格式） 的完全可攜式僅限標頭的程式庫會形成的所有 Windows 執行階段和工具從現在開始的 xlang 的基礎。 值得注意的是，我們也重寫`cppwinrt.exe`將其從基礎工具中使用 xlang 程式庫。 這會提供更精確的中繼資料查詢，解決一些長久以來的問題，使用C++/WinRT 語言投影。

#### <a name="fewer-dependencies"></a>較少的相依性

Xlang 的中繼資料讀取器，因為`cppwinrt.exe`工具本身有較少的相依性。 這使得更有彈性，以及正在使用的更多案例&mdash;尤其是在受條件約束建置環境。 值得注意的是，它不再依賴`RoMetadata.dll`。
 
這些是相依性`cppwinrt.exe`2.0。
 
- api-ms-win-core-processenvironment-l1-1-0.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- XmlLite.dll
- api-ms-win-core-memory-l1-1-0.dll
- api-ms-win-core-handle-l1-1-0.dll
- api-ms-win-core-file-l1-1-0.dll
- SHLWAPI.dll
- ADVAPI32.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-processthreads-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll

利用這些相依性，對照這`cppwinrt.exe`1.0 提供的。

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

#### <a name="the-windows-runtime-noexcept-attribute"></a>Windows 執行階段`noexcept`屬性

有新的 Windows 執行階段`[noexcept]`屬性，可用來裝飾方法和屬性[MIDL 3.0](/uwp/midl-3/predefined-attributes)。 屬性表示若要支援您的實作不會擲回例外狀況的工具 (也不會傳回失敗 HRESULT)。 這可讓語言投影來避免例外狀況處理額外負荷，擁有所需的支援可能失敗的應用程式二進位介面 (ABI) 呼叫，以最佳化程式碼產生。

C++/ WinRT 採用這所產生C++`noexcept`實作，同時使用，以及撰寫程式碼。 如果您有 API 方法或屬性都失敗，且您正在關注的程式碼大小，您可以調查此屬性。

#### <a name="optimized-code-generation"></a>最佳化的程式碼產生

C++/ WinRT 現在會產生更有效率C++原始檔程式碼 （幕後） 讓C++，編譯器可以產生的最小和最有效率二進位程式碼可能。 許多改善項目專為降低成本的例外狀況處理藉由避免不必要的回溯資訊。 使用大量的二進位檔C++/WinRT 程式碼將會大約 4%降低程式碼大小。 程式碼也是更有效率 （其執行速度較快） 由於精簡的指令計數。

這些增強功能依賴新的 interop 功能，也有提供給您。 所有的C++現在是資源擁有者的 WinRT 類型包含直接取得擁有權的建構函式避免先前兩個步驟的方法。

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>最佳化的例外狀況處理 (EH) 程式碼產生

這項變更可補充由 Microsoft 的工作C++最佳化工具小組，以降低成本的例外狀況處理。 如果您經常在您的程式碼中使用應用程式二進位介面 (Abi)，（例如 COM)，您將會發現許多遵循此模式的程式碼。

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

C++/ WinRT 本身會產生這種模式實作每一種 api。 具有上千個 API 函式，這裡任何最佳化很可觀。 在過去，最佳化工具不會偵測這些 catch 區塊都都有所不同，因此它重複大量程式碼的每個 ABI （這又造成信念系統程式碼中使用例外狀況會產生大型二進位檔案） 位置。 不過，從 Visual Studio 2019，C++編譯器摺疊所有項目攔截 funclets，並只會儲存唯一的。 結果是進一步和整體的 18%減少，在此模式是高度依賴的二進位檔的程式碼大小。 不只是 EH 程式碼現在比使用傳回碼，更有效率，但也較大的二進位檔的顧慮現在是過去。

#### <a name="incremental-build-improvements"></a>累加建置改善

`cppwinrt.exe`工具現在會比較針對在磁碟上，任何現有檔案的內容產生的標頭/原始程式檔的輸出，它只會寫入檔案時如果檔案實際上已變更。 這樣可以節省相當長的時間與磁碟 I/O，並確保檔案不會考慮"dirty"的C++編譯器。 結果是，避免或減少，在許多情況下，重新編譯。

#### <a name="generic-interfaces-are-now-all-generated"></a>泛型介面現在都是產生

Xlang 的中繼資料讀取器，因為C++/WinRT 現在所有的參數化，或泛型，介面會從產生的中繼資料。 這類介面[ivector&lt\<T\> ](/uwp/api/windows.foundation.collections.ivector_t_)會現在產生的中繼資料，而以手動撰寫`winrt/base.h`。 結果是大小`winrt/base.h`已被截斷成兩半，而且會產生最佳化滑鼠右鍵在程式碼 （這是難以手動復原的方法）。

> [!IMPORTANT]
> 介面，例如指定的範例現在會出現在其各自的命名空間的標頭，而非在`winrt/base.h`。 因此，如果您尚未這樣做，您必須包含適當的命名空間的標頭，若要使用的介面。

#### <a name="component-optimizations"></a>元件最佳化

這項更新加入支援數個其他選用功能最佳化的C++/WinRT，下列各節中所述。 由於這些最佳化重大變更 （這您可能需要進行微幅變更以支援），您必須將其開啟使用明確`cppwinrt.exe`工具的`-opt`旗標。

新的專案 （從專案範本） 會使用`-opt`預設。

##### <a name="uniform-construction-and-direct-implementation-access"></a>統一的建構，並直接實作的存取

這些兩種最佳化允許自己實作的類型，您的元件直接存取，即使它只使用投影的類型。 若要使用不需要[**請**](/uwp/cpp-ref-for-winrt/make)， [ **make_self**](/uwp/cpp-ref-for-winrt/make-self)，也不[ **get_self** ](/uwp/cpp-ref-for-winrt/get-self)如果您只想要使用公用 API 介面。 您的呼叫會編譯成直接呼叫實作中，和那些甚至會完全內嵌。

##### <a name="type-erased-factories"></a>已清除類型的處理站

此最佳化可避免 #include 中的相依性`module.g.cpp`，讓它需要不會重新編譯每次發生變更的任何單一實作類別。 結果會是更有效率的建置效能。

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>更聰明且更有效率`module.g.cpp`大型專案中使用多個程式庫

`module.g.cpp`檔案現在也包含兩個其他可撰寫協助程式，名為**winrt_can_unload_now**，以及**winrt_get_activation_factory**。 這些設計針對其中 DLL 由數個程式庫，每個都有它自己的執行階段類別組成的大型專案。 在此情況下，您需要將手動拼接在一起的 DLL **DllGetActivationFactory**並**DllCanUnloadNow**。 這些協助程式，請讓您執行此作業，藉由避免假性的起源錯誤更容易。 `cppwinrt.exe`工具的`-lib`旗標也可用來為每個個別的程式庫提供它自己的前序編碼 (而非`winrt_xxx`)，讓每個程式庫函式可能會個別名稱，並因此結合明確。

#### <a name="new-winrtcoroutineh-header"></a>新`winrt/coroutine.h`標頭

`winrt/coroutine.h`標頭是所有的新首頁C++/WinRT 的協同程式支援。 先前，這項支援是存放在一些地方，我們認為這是相當大的限制。 由於現在會產生 Windows 執行階段非同步介面，而不是手寫，它們現在位於`winrt/Windows.Foundation.h`。 除了要更容易維護且更具支援性，這表示該協同程式協助程式，例如[ **resume_foreground** ](/uwp/cpp-ref-for-winrt/resume-foreground)不再需要附加至特定的命名空間的標頭的結尾。 相反地，它們可以更自然方式包含它們的相依性。 這可以進一步讓**resume_foreground**來支援不只會繼續在給定[ **Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher)，但現在還可以支援繼續上指定[ **Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue)。 先前，只有一個可以支援;但非兩者，因為定義只可能位於一個命名空間。

以下是範例**DispatcherQueue**支援。

```cppwinrt
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

協同程式協助程式會現在也附有`[[nodiscard]]`，藉此改善其可用性。 如果您忘了 （或不知道您不必）`co_await`它們才能運作，則因為`[[nodiscard]]`，這類錯誤現在會產生編譯器警告。

#### <a name="help-with-diagnosing-stack-allocations"></a>協助診斷堆疊配置

規劃和實作的類別名稱 （根據預設值） 相同，並只因命名空間，因為它是可能產生錯誤的另一個，而不小心在堆疊上，建立實作，而不是使用[ **製作**](/uwp/cpp-ref-for-winrt/make)系列協助程式。 這可以是難在某些情況下，診斷，因為未完成的參考仍在飛行時，可能會終結物件。 判斷提示現在會挑選此，表示偵錯組建。 判斷提示偵測不到協同程式內的堆疊配置，而這一點可幫助攔截大部分這類錯誤。

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>改善的擷取協助程式，以及 variadic 委派

此更新修正擷取協助程式的限制，藉由支援投影的型別。 這帶來現在和未來的 Windows 執行階段的 interop Api，就會傳回投影的型別。

此更新也新增支援[ **get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)並[ **get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)時建立自己的 variadic （非 Windows 執行階段） 的委派。

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>支援延後的解構和安全 QI 解構期間

XAML 應用程式可以進入本身困難，因為它需要執行[ **QueryInterface** ](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) (QI) 解構函式，才能夠呼叫一些清除實作，增加或減少階層中。 但是，您也可以呼叫包括 QI 之後已有物件的參考計數, 達到零。 這項更新加入支援 debouncing 參考計數，如此可確保在達到零可以永遠不會和;同時，仍允許 QI 解構期間，所需要的任何暫存的。 此程序是在特定 XAML 應用程式/控制項中，無法避免和C++/WinRT 現在是從它復原。

可以藉由提供靜態延後解構**final_release**函式，並移動的擁有權**unique_ptr**至某些其他內容。

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

    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // Move 'ptr' as needed to delay destruction.
    }
};
```

在下列範例中，一次**MainPage** （適用於最後一次），會釋放**final_release**呼叫。 函式花費在等候 （執行緒集區），五秒，則它會繼續使用頁面**發送器**（這需要 QI/AddRef/發行工作）。 它接著會清除**unique_ptr**，會造成**MainPage**實際呼叫的解構函式。 即使在這裡， **DataContext**呼叫時，需要針對 QI **IFrameworkElement**。 很明顯地，您不需要實作您**final_release**為協同程式。 運作，但它可以讓您輕而易舉將解構移至不同的執行緒。

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

    static IAsyncAction final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;

        co_await resume_foreground(ptr->Dispatcher());

        ptr = nullptr;
    }
};
```

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>改進的支援的 COM 樣式單一介面繼承

以及對於 Windows 執行階段程式設計中， C++/WinRT 也會用來撰寫和使用僅限 COM Api。 此更新可讓您能夠實作 COM 伺服器所在的介面階層架構。 這並不適用 Windows 執行階段;但它對某些 COM 實作中是必要。

#### <a name="correct-handling-of-out-params"></a>正確處理`out`params

它可能很難使用`out`params; 特別是 Windows 執行階段陣列。 此更新中， C++/WinRT 是大幅更加穩定且從錯誤中復原時`out`params 和陣列; 這些參數會透過語言推演，，或從 COM 開發人員的使用者正在使用未經處理的 ABI，並者是否不一致的方式初始化變數的錯誤。 在任一情況下， C++/WinRT 現在會正確的事，就交由投影類型至 ABI （藉由記得釋放任何資源），並談到清空或清除掉抵達 abi 的參數。

#### <a name="events-now-handle-invalid-tokens-reliably"></a>事件現在處理無效的語彙基元可靠

[ **Winrt::event** ](/uwp/cpp-ref-for-winrt/event)現在實作依正常程序會處理的情況，其**移除**方法呼叫無效的權杖值 (不存在於值陣列）。

#### <a name="coroutine-locals-are-now-destroyed-before-the-coroutine-returns"></a>協同程式傳回之前，會立即終結協同程式的區域變數

實作協同程式類型的傳統方式可能會允許協同程式中的 [區域變數]，也將被銷毀*之後*協同程式傳回/完成 （而不是最終的暫止之前）。 接續的任何等候者現在會延遲到最終的暫止，若要避免這個問題，並產生其他權益。

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>新聞和變更，請在 Windows SDK 版本 10.0.17763.0 (Windows 10 版本 1809年)

下表包含新聞和變更C++Windows sdk 版本 10.0.17763.0 的 /WinRT (Windows 10 版本 1809年)。

| 新增或變更的功能 | 其他資訊 |
| - | - |
| **重大變更**。 它在編譯時， C++/WinRT 不會相依於 Windows SDK 標頭。 | 請參閱[從 Windows SDK 標頭檔的隔離](#isolation-from-windows-sdk-header-files)底下。 |
| Visual Studio 專案系統格式已變更。 | 請參閱[如何將目標重定您C++/WinRT 專案至較新版的 Windows sdk](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)底下。 |
| 有新的函式和基底類別可協助您將集合物件傳遞至 Windows 執行階段函式，或實作您自己的集合屬性和集合型別。 | 請參閱[集合C++/WinRT](collections.md)。 |
| 您可以使用[{Binding}](/windows/uwp/xaml-platform/binding-markup-extension)使用的標記延伸您C++/WinRT 執行階段類別。 | 如需詳細資訊，以及程式碼範例，請參閱[資料繫結概觀](/windows/uwp/data-binding/data-binding-quickstart)。 |
| 取消協同程式的支援可讓您註冊的取消回呼。 | 如需詳細資訊，以及程式碼範例，請參閱[取消非同步作業，並取消回呼](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks)。 |
| 在建立指向成員函式的委派時，您可以建立強式或目前物件的弱式參考 (而非原始*這*指標) 註冊處理常式的所在之處。 | 如需詳細資訊，以及程式碼範例，請參閱**如果您使用的成員函式為委派**一節中的子區段[安全地存取*這*與事件處理委派指標](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| 修正所發現的 Visual Studio 的改進的相容性的 bugC++標準。 LLVM 和 Clang 工具鏈也更好地用來驗證C++/WinRT 的標準一致性。 | 將不會再遇到問題中所述[為什麼無法我新增的專案編譯？我使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，和 SDK 版本 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

其他變更。

- **重大變更**。 [**winrt::get_abi(winrt::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi)現在會傳回`void*`而不是`HSTRING`。 您可以使用`static_cast<HSTRING>(get_abi(my_hstring));`取得 HSTRING。
- **重大變更**。 [**winrt::put_abi(winrt::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi)現在會傳回`void**`而不是`HSTRING*`。 您可以使用`reinterpret_cast<HSTRING*>(put_abi(my_hstring));`取得 HSTRING *。
- **重大變更**。 HRESULT 現在投射為**winrt::hresult**。 如果您需要 （或進行類型檢查，以支援類型特性） 的 HRESULT，則您可以`static_cast` **winrt::hresult**。 否則，請**winrt::hresult** ，只要您加入，將轉換成 HRESULT`unknwn.h`包含任何之前C++/WinRT 標頭。
- **重大變更**。 GUID 現在投射為**winrt::guid**。 對於您所實作的 Api，您必須使用**winrt::guid** GUID 參數。 否則，請**winrt::guid** ，只要您加入，將轉換成 GUID`unknwn.h`包含任何之前C++/WinRT 標頭。
- **重大變更**。 [ **Winrt::handle_type 建構函式**](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor)未經強化，以便明確 （現在很難撰寫不正確的程式碼與它）。 如果您要指派的未經處理的控制代碼值，請呼叫[ **handle_type::attach 函式**](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function)改。
- **重大變更**。 簽章**WINRT_CanUnloadNow**並**WINRT_GetActivationFactory**已變更。 您不得宣告這些函式。 相反地，包含`winrt/base.h`(如果您包含任何，這是自動包含C++WinRT Windows 命名空間的標頭檔) 包含這些函式的宣告。
- 針對[ **winrt::clock 結構**](/uwp/cpp-ref-for-winrt/clock)， **from_FILETIME/to_FILETIME**取代為**from_file_time/to_file_time**。
- 預期的 Api **IBuffer**已簡化參數。 足夠的 Api 集合或陣列，則偏好使用的大部分 Api，雖然已依賴**IBuffer** ，它需要更輕鬆地使用這類 Api，從C++。 此更新可直接存取的資料，背後**IBuffer**實作中，使用相同資料的命名慣例供C++標準程式庫容器。 這也可避免碰撞的慣例以大寫字母開頭的中繼資料名稱。
- 改善程式碼產生： 若要減少程式碼大小的各種改進改善內嵌 （inline），並處理站快取最佳化。
- 移除不必要遞迴。 當命令列參照至資料夾，而不是特定`.winmd`，則`cppwinrt.exe`工具不會再以遞迴方式搜尋`.winmd`檔案。 `cppwinrt.exe`工具現在也會處理重複的項目更智慧的方式，讓您更有彈性，為使用者錯誤，而且為不良形成`.winmd`檔案。
- 強化的智慧型指標。 之前，無法撤銷時事件 revokers 移動-指派新值。 這有助於找出的問題所在的智慧型指標類別不可靠地處理自我指派;在進行 root 破解[ **winrt::com_ptr 結構範本**](/uwp/cpp-ref-for-winrt/com-ptr)。 **winrt::com_ptr**已修正，並修正來處理事件 revokers 移動語意正確以便指派時就會撤銷。

> [!IMPORTANT]
> 已對重要的變更[ C++WinRT Visual Studio 擴充功能 (VSIX)](https://aka.ms/cppwinrt/vsix)，在版本 1.0.181002.2，版本 1.0.190128.4 然後再。 如需詳細資訊的這些變更，以及它們如何影響您現有的專案中， [Visual Studio 支援 C + /cli WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)和[VSIX 擴充功能的舊版](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

### <a name="isolation-from-windows-sdk-header-files"></a>從 Windows SDK 標頭檔的隔離

這可能是您的程式碼的重大變更。

它在編譯時， C++/WinRT 不再取決於 Windows SDK 中的標頭檔。 C 執行階段程式庫 (CRT) 中的標頭檔和C++標準範本庫 (STL) 也不包含任何 Windows SDK 標頭。 而且可以改善標準相容性、 可避免不小心的相依性，並可大幅減少您必須防範的巨集的數目。

此獨立性，表示C++/WinRT 現在更多的可攜式] 和 [標準的相容性，而且它以便促進它變成跨編譯器及跨平台程式庫的可能性。 這也表示C++/WinRT 標頭不受造成負面影響的巨集。

如果您先前所保留 C + /cli 要包含在您的專案中的任何 Windows 標頭，則您現在必須將它們包含您自己的 WinRT。 是，在任何情況下，永遠的最佳做法是明確包含的標頭，您需要，而且不將它留給將它們納入您的另一個程式庫。

目前，唯一的例外狀況為 Windows SDK 標頭檔案隔離是內建函式和數字。 沒有任何已知的問題，這些最後一個剩餘的相依性。

在專案中，您可以重新啟用的 Windows SDK 標頭的 interop 如果您需要。 您可能會比方說，要實作 COM 介面 (立[ **IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown))。 例如，包含`unknwn.h`包括任何之前C++/WinRT 標頭。 這麼做會導致C++/WinRT 啟用各種不同的攔截程序來支援傳統的 COM 介面的基底文件庫。 如需程式碼範例，請參閱 <<c0> [ 作者 COM 元件，與C++/WinRT](author-coclasses.md)。</c0> 同樣地，明確地包含宣告類型和/或您想要呼叫的函式的任何其他 Windows SDK 標頭。

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>如何將目標重定您C++/WinRT 專案至較新版的 Windows sdk

重定目標的專案可能會導致最少的編譯器和連結器問題的方法也是最費力的。 該方法牽涉到建立新的專案 （您所選擇的 Windows SDK 版本為目標），並接著透過從您的舊，將檔案複製到新的專案。 會有的舊版區段`.vcxproj`和`.vcxproj.filters`檔案，您可以直接複製超過儲存您在 Visual Studio 中新增檔案。

不過，有兩種方式可重定目標的 Visual Studio 中的專案。

- 移至 專案屬性**一般** \> **Windows SDK 版本**，然後選取**所有組態**並**所有平台**。 設定**Windows SDK 版本**，做為目標的版本。
- 在 **方案總管 中**，以滑鼠右鍵按一下專案節點，按一下**重定目標的專案**，選擇您想要的目標，然後按一下 版本**確定**。

如果您遇到任何編譯器或連結器錯誤之後使用下列兩種方法，則您可以嘗試清除方案 (**建置** > **清除方案**及/或以手動方式刪除所有暫存資料夾和檔案） 然後再嘗試再次建置。

如果C++編譯器會產生 「*錯誤 C2039:'IUnknown': 不是成員 '\`全域命名空間'* "，然後新增`#include <unknwn.h>`頂端您`pch.h`檔案 (包含任何之前C++/WinRT 標頭)。

您可能也需要新增`#include <hstring.h>`之後。

如果C++連結器會產生 」*錯誤 LNK2019： 無法解析的外部符號_WINRT_CanUnloadNow@0函式中參考_VSDesignerCanUnloadNow@0* "，則您可以解析，加上`#define _VSDESIGNER_DONT_LOAD_AS_DLL`至您`pch.h`檔案。
