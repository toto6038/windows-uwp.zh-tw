---
description: 無論您是剪下新的程式碼或移植現有的應用程式，本主題中的疑難排解問題和解決方式表格可能對您會很有幫助。
title: 針對 C++/WinRT 問題進行疑難排解
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, troubleshooting, HRESULT, error, 標準, 投影, 移難排解, 錯誤
ms.localizationpriority: medium
ms.openlocfilehash: 268792dfe8053feca8c1e6fcb486bede4b26ee6a
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297718"
---
# <a name="troubleshooting-cwinrt-issues"></a>針對 C++/WinRT 問題進行疑難排解

> [!NOTE]
> 如需安裝和使用 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) Visual Studio 延伸模組 (VSIX) (提供專案範本支援) 的資訊，請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

本主題是預先準備，這樣您就會立即知道；即使您還不需要。 無論您是剪下新的程式碼或移植現有的應用程式，下面的疑難排解問題和解決方式表格可能對您會很有幫助。 如果您正在移植，且您急著想要儘快進入建置及執行專案的階段，您可以暫時將任何非必要的程式碼標成註解或移除，稍後再回來解決。

如需常見問題清單，請參閱[常見問題集](faq.md)。

## <a name="tracking-down-xaml-issues"></a>追蹤 XAML 問題
XAML 剖析例外狀況可能難以診斷 &mdash; 特別是如果例外狀況中的錯誤訊息沒有意義。 請確定偵錯工具已設定為擷取第一個可能發生的例外狀況 (以嘗試並擷取早期剖析例外狀況)。 您可以在偵錯工具檢查例外狀況變數，以判斷 HRESULT 或訊息是否有任何有用的資訊。 同時檢查 Visual Studio 的輸出視窗當中是否有 XAML 剖析器輸出的錯誤訊息。

如果您的應用程式終止，而您知道的唯一事項是在 XAML 標記剖析期間擲回未處理的例外狀況，則有可能是所參考 (藉由索引鍵) 的資源遺失的結果。 或者，可能是在 UserControl、自訂控制項或自訂版面配置面板內擲回例外狀況。 真的別無他法時，才採用二元分割法。 從「XAML 頁面」移除一半的標記，然後重新執行應用程式。 這樣您就會知道錯誤是出在您所移除的那半部內 (無論如何，您現在都應該還原這個部分)，還是出在您「未」移除的那半部內。 不斷針對包含錯誤的那一半重複分割程序，直到您準確找出問題為止。

## <a name="symptoms-and-remedies"></a>徵兆和解決方式
| 徵狀 | 補救方法 |
|---------|--------|
| 在執行階段期間擲回例外狀況，其 HRESULT 值為 REGDB_E_CLASSNOTREGISTERED。 | 請參閱[為什麼我會收到「類別未註冊」的例外狀況？](faq.md#why-am-i-getting-a-class-not-registered-exception)。 |
| C++ 編譯器產生錯誤「'implements_type': 不是 '&lt;投影類型&gt;' 任何直接或間接基底類別的成員**」。 | 您呼叫 **make** 實作類型的不完整命名空間名稱 (例如，**MyRuntimeClass**)，且您尚未包含該類型的標頭時，便會發生此情況。 編譯器解譯 **MyRuntimeClass** 作為投影型別。 解決方案會為您的實作類型包含標頭 (例如，`MyRuntimeClass.h`)。 |
| C++ 編譯器產生錯誤「嘗試參考被刪除的函式**」。 | 當您呼叫 **make** 且您作為範本參數傳遞的實作類型有 `= delete` 預設建構函式時，便會發生此情況。 編輯實作類型的標頭檔案，並將 `= delete` 變更為 `= default`。 您也可以將建構函式新增至適用於執行階段類別的 IDL。 |
| 您已實作 [**INotifyPropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged)，但您的 XAML 繫結未更新 (且 UI 未訂閱 [**PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged))。 | 請記得在 XAML 標記中的繫結運算式上設定 `Mode=OneWay` (或 TwoWay)。 請參閱 [XAML 控制項；繫結一個 C++/WinRT 屬性](binding-property.md)。 |
| 您把 XAML 項目控制項繫結到可觀察的集合，且在執行階段擲回例外狀況與訊息「參數不正確」。 | 在您的 IDL 和實作中，宣告任何可觀察的集合作為類型 **Windows.Foundation.Collections.IVector<IInspectable>**。 但傳回實作 **Windows.Foundation.Collections.IObservableVector<T>** 的物件，其中 T 為您的元素類型。 請參閱 [XAML 項目控制項；繫結至 C++/WinRT 集合](binding-collection.md)。  |
| C++ 編譯器產生一個錯誤的表單「'MyImplementationType_base&lt;MyImplementationType&gt;': 沒有適當的預設建構函式**」。|當您衍生自具有非一般建構函式的類型時，可能會發生這種情形。 您衍生的類型建構函式必須與需要基本類型建構函式的參數一起傳遞。 如需一個已執行的範例，請參閱[從非一般建構函式衍生](author-apis.md#deriving-from-a-type-that-has-a-non-default-constructor)。|
| C++ 編譯器產生錯誤「無法從 'const std::vector&lt;std::wstring,std::allocator&lt;_Ty&gt;&gt;' 轉換至 'const winrt::param::async_iterable&lt;winrt::hstring&gt; &'」。|您將 std::wstring 的 std::vector 傳遞到預期收到集合的 Windows 執行階段 API，便可能會發生此情況。 如需詳細資訊，請參閱[標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)。|
| C++ 編譯器產生錯誤「無法從 'const std::vector&lt;winrt::hstring,std::allocator&lt;_Ty&gt;&gt;' 轉換至 'const winrt::param::async_iterable&lt;winrt::hstring&gt; &'」。|當您將 winrt::hstring 的 std::vector 傳遞至預期收到集合的 Windows 執行階段 API 時，且您沒有將向量複製或移動到非同步被呼叫者，便會發生此情況。 如需詳細資訊，請參閱[標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)。|
| 開啟一個專案時，Visual Studio 產生此錯誤「未安裝專案的應用程式**」。|如果您尚未開始，您需要從 Visual Studio 的 [新專案]**** 對話方塊中安裝 **C++ 開發 Windows 通用工具**。 如果無法解決問題，則此專案可能相依於 C++/WinRT Visual Studio 擴充功能 (VSIX) (請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。|
| Windows 應用程式認證套件測試產生執行階段類別的錯誤之一「*不是從 Windows 的基底類別衍生。所有可組合類別必須最終從 Windows 命名空間中的類別衍生*」。|從基底類別衍生的任何執行階段類別 (您在您的應用程式中宣告)，都稱為「可組合」** 類別。 可組合類別的最終基底類別都必須是源自 Windows.* 命名空間的類別；例如，[**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)。 如需詳細資訊，請參閱 [XAML 控制項；繫結至 C++/WinRT 屬性](binding-property.md)。|
| C++ 編譯器產生 EventHandler 或 TypedEventHandler 委派特製化的「T 必須為 WinRT 類型」錯誤。|請考慮改用 **winrt::delegate&lt;...T&gt;**。 請參閱[在 C++/WinRT 中撰寫事件](author-events.md)。|
| C++ 編譯器產生 Windows 執行階段非同步作業特製化的「T 必須為 WinRT 類型」錯誤。|請考慮改為傳回平行模式程式庫 (PPL) [**task**](/cpp/parallel/concrt/reference/task-class)。 請參閱[並行和非同步作業](concurrency.md)。|
| 當您呼叫 [**winrt::xaml_typename**](/uwp/cpp-ref-for-winrt/xaml-typename)時，C++ 編譯器會產生「T 必須為 WinRT 類型」錯誤。|使用預估類型搭配 **winrt::xaml_typename** (例如，使用 **BgLabelControlApp::BgLabelControl**)，而不是實作類型 (例如，請勿使用 **BgLabelControlApp::implementation::BgLabelControl**)。 請參閱 [XAML 自訂 (範本化) 控制項](xaml-cust-ctrl.md)。|
| C++ 編譯器產生「錯誤 C2220: 將警告視為錯誤處理 - 沒有產生 'object' 檔案**」。|修正警告，或將 [C/C++] > [一般] > [將警告視為錯誤] 設定為 [否 (/WX-)]。|
| 您的應用程式當機，因為在終結物件後，呼叫了在 C++/WinRT 物件中處理的事件。|請參閱[使用事件處理委派安全地存取 *this* 指標](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)。|
| C++ 編譯器產生「*錯誤 C2338：這僅適用於弱式參考資料支援*」。|您為將 **winrt::no_weak_ref** 標記結構作為範本引數傳遞至其基底類別的類型，要求一個弱式參考。 請參閱[不使用弱式參考支援](weak-references.md#opting-out-of-weak-reference-support)。|
| C++ 連結器產生「錯誤 LNK2019:無法解析的外部符號」|請參閱[為何連結器給我「LNK2019:無法解析的外部符號」錯誤？](faq.md#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error)。|
| 當 LLVM 和 Clang 工具鏈搭配使用 C++/WinRT 時，會產生錯誤。|針對 C++/WinRT，我們不支援 LLVM 和 Clang 工具鏈，但如果您想要模擬我們如何在內部使用它，則您可以嘗試實驗，如[我可以使用 LLVM/Clang 來編譯 C++/WinRT 嗎？](faq.md#can-i-use-llvmclang-to-compile-with-cwinrt)中所述的。|
| C++ 編譯器產生投影型別的「沒有適當的預設建構函式**」。 | 如果您嘗試將執行階段類別物件的初始化延遲，或嘗試在相同專案中取用和實作執行階段類別，則您必須呼叫 **std::nullptr_t** 建構函式。 如需詳細資訊，請參閱[使用 C++/WinRT 取用 API](consume-apis.md)。 |
| C++ 編譯器產生「錯誤 C3861: 'from_abi': 找不到識別碼**」，及其他源自 *base.h* 的錯誤。 如果您是使用 Visual Studio 2017 (15.8.0 版或更新版本)，且目標為 Windows SDK 10.0.17134.0 版 (Windows 10 1803 版)，則您可能會看到此錯誤。 | 將 Windows SDK 的較新 (更一致) 版本設定為目標，或將專案屬性設定為 C/C++ > 語言 > 一致性模式:否 (此外，如果 **/permissive-** 顯示在專案屬性 C/C++ > 語言 > 命令列 的 其他選項 底下，請刪除它)。 |
| C++ 編譯器產生「錯誤 C2039:'IUnknown': 不是 '\`global namespace'' 的成員」。 | 請參閱[如何將您 C++/WinRT 專案的目標設定為 Windows SDK 的較新版本](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)。 |
| C++ 連接器產生「錯誤 LNK2019: 無法解析的外部符號 _WINRT_CanUnloadNow@0 在函式 _VSDesignerCanUnloadNow@0 中被參考**」 | 請參閱[如何將您 C++/WinRT 專案的目標設定為 Windows SDK 的較新版本](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)。 |
| 建置程序會產生錯誤訊息「C++/WinRT VSIX 不再提供專案的建置支援。請將專案參考加入 Microsoft.Windows.CppWinRT Nuget 套件」。 | 將 **Microsoft.Windows.CppWinRT** NuGet 套件安裝到您的專案中。 如需詳細資訊，請參閱[舊版 VSIX 擴充功能](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。 |
| C++ 連接器產生「錯誤 LNK2019: 無法解析的外部符號」**，包含提及 *winrt::impl::consume_Windows_Foundation_Collections_IVector*。 | 自 [C++/WinRT 2.0](news.md#news-and-changes-in-cwinrt-20)，如果您在 Windows 執行階段集合上使用範圍型 `for`，則您現在必須 `#include <winrt/Windows.Foundation.Collections.h>`。 |
| C++ 編譯器產生「*錯誤 C4002：函式類巨集引動過程 GetCurrentTime 有太多引數*」。 | 請參閱[如何解決 GetCurrentTime 和/或 TRY 意義不明的狀況？](faq.md#how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try)。 |
| C++編譯器會產生「錯誤 C2334：'{' 前面有非預期的權杖；略過顯示的函式主體**」。 | 請參閱[如何解決 GetCurrentTime 和/或 TRY 意義不明的狀況？](faq.md#how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try)。 |
| C++編譯器會產生「winrt::impl::produce&lt;D,I&gt; 無法具現化抽象類別，因為遺漏 GetBindingConnector**」。 | 您需要 `#include <winrt/Windows.UI.Xaml.Markup.h>`。 |
| C++ 編譯器會產生「錯誤 C2039：'promise_type': 不是 'std::experimental::coroutine_traits<void>' 的成員**」。 | 您的協同程式需要傳回非同步作業物件或 **winrt::fire_and_forget**。 請參閱[並行和非同步作業](concurrency.md)。 |
| 您的專案會產生「模稜兩可的 'PopulatePropertyInfoOverride' 存取**」。 | 當您在 IDL 中宣告一個基底類別，並在 XAML 標記中宣告不同基底類別時，就會發生此錯誤。 |
| 第一次載入 C++/WinRT 解決案會產生「*專案 'MyProject.vcxproj' 組態 'Debug\|x86' 的 Designtime 建置失敗。IntelliSense 可能無法使用。* 」 | 經過第一次建置之後，此 IntelliSense 問題就會解決。 |
| 註冊委派時嘗試指定 [**winrt::auto_revoke**](/uwp/cpp-ref-for-winrt/auto-revoke-t) 會產生 [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) 例外狀況。 | 請參閱[如果您的自動撤銷委派無法註冊](handle-events.md#if-your-auto-revoke-delegate-fails-to-register)。 |
|在 C++/WinRT 應用程式中，當取用一個使用 XAML 的 [C# Windows 執行階段元件](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md)時，編譯器會產生格式為 " *'MyNamespace_XamlTypeInfo': is not a member of 'winrt::MyNamespace'* "&mdash;其中 *MyNamespace* 是 Windows 執行階段元件命名空間的名稱。 | 在取用 C++/WinRT 應用程式的 `pch.h` 中，視適當情況新增 `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>`&mdash;取代 *MyNamespace*。 |

> [!NOTE]
> 如果本主題無法解答您的問題，則您可藉由造訪 [Visual Studio C++ 開發人員社群](https://developercommunity.visualstudio.com/spaces/62/index.html)，或[在 Stack Overflow 上使用 `c++-winrt` 標籤](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt)來尋找說明。