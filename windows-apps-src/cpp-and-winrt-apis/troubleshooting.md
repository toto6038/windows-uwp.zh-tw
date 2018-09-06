---
author: stevewhims
description: 無論您是剪下新的程式碼或移植現有的應用程式，本主題中的疑難排解問題和解決方式表格可能對您會很有幫助。
title: 疑難排解 C++/WinRT 問題
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、移難排解、HRESULT、錯誤
ms.localizationpriority: medium
ms.openlocfilehash: 4129c50a2273c8ac425f6ea972898aa09fe0fcf3
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2018
ms.locfileid: "3390654"
---
# <a name="troubleshooting-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-issues"></a>疑難排解 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 問題
> [!NOTE]
> 如需有關安裝和使用 C++/WinRT Visual Studio 擴充功能 (VSIX) (提供專案範本的支援，以及 C++/WinRT MSBuild 屬性和目標) 的資訊，請參閱 [C++/WinRT 和 VSIX 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

本主題是預先準備，這樣您就會立即知道；即使您還不需要。 無論您是剪下新的程式碼或移植現有的應用程式，下述疑難排解問題和解決方式表格可能對您會很有幫助。 如果您正在移植，且您急著想要儘快進入建置及執行專案的階段，您可以暫時將任何非必要的程式碼標成註解或移除，稍後再回來清償該負債。

如需常見問題集的清單，請參閱[常見問題集](faq.md)。

## <a name="tracking-down-xaml-issues"></a>追蹤 XAML 問題
XAML 剖析例外狀況可能難以診斷&mdash;特別是如果例外狀況中的錯誤訊息沒有意義。 請確定偵錯工具已設定為擷取第一個可能發生的例外狀況 (以嘗試並擷取早期剖析例外狀況)。 您可以在偵錯工具檢查例外狀況變數，以判斷 HRESULT 或訊息是否有任何有用的資訊。 同時檢查 Visual Studio 的輸出視窗當中是否有 XAML 剖析器輸出的錯誤訊息。

如果您的應用程式終止，而您知道的唯一事項是在 XAML 標記剖析期間擲回未處理的例外狀況，則那有可能是所參考 (藉由索引鍵) 的資源遺失的結果 或者，可能是在 UserControl、自訂控制項或自訂版面配置面板內擲回例外狀況。 真的別無他法時，才採用二元分割法。 從「XAML 頁面」移除一半的標記，然後重新執行應用程式。 這樣您就會知道錯誤是出在您所移除的那半部內 (無論如何，您現在都應該還原這個部分)，還是出在您「未」移除的那半部內。 不斷針對包含錯誤的那一半重複分割程序，直到您準確找出問題為止。

## <a name="symptoms-and-remedies"></a>問題和解決方式
| 問題 | 解決方式 |
|---------|--------|
| 有個例外，在執行階段擲回 REGDB_E_CLASSNOTREGISTERED 的 HRESULT 值。 | 這個錯誤的其中一個原因是無法載入您的 Windows 執行階段元件。 請確定元件的 Windows 執行階段中繼資料檔案 (`.winmd`) 具有與元件二進位相同的名稱 ( `.dll`)，這也是專案名稱以及根命名空間的名稱。 也會確保，Windows 執行階段中繼資料和二進位已由建置程序正確複製到使用中應用程式`Appx`資料夾。 並且確認使用中的應用程式 `AppxManifest.xml`(也在`Appx`資料夾) 包含一個正確宣告啟動類別與二進位檔案名稱的 **&lt;InProcessServer&gt;** 元素。 如果您透過投影類型的預設建構函式犯了具現化一個本機實作執行階段類別的錯誤，也可能會發生這個錯誤。 請參閱 [XAML 控制項；繫結至 C++/WinRT 屬性](binding-property.md) 了解如何正確此時使用該案例中的投影類型的詳細資訊。 |
| C++ 編譯器產生此錯誤 「*'implements_type'：不是 '&lt;投影類型&gt;' 任一直接或間接基底類別的成員之一*」。 | 您呼叫 **make** 實作類型的不完整命名空間名稱 (例如，**MyRuntimeClass**)，且您尚未包含該類型的標頭時，便會發生此情況。 編譯器解譯 **MyRuntimeClass** 做為投影類型。 解決方案會包含適用於您實作類型的標頭 (例如，`MyRuntimeClass.h`)。 |
| C++ 編譯器產生錯誤「*嘗試參考已刪除的函式*」。 | 您呼叫 **make** 且您做為範本參數傳遞的實作類型有一個 `= delete` 預設建構函式時，便會發生此情況。 編輯實作類型的標頭檔案，並變更 `= delete` 為 `= default`。 您也可以將建構函式新增至適用於執行階段類別的 IDL。 |
| 您已實作 [**INotifyPropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged)，但無法更新您的 XAML 繫結 (且 UI 不訂閱 [**PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged))。 | 請記得在 XAML 標記中的繫結運算式上設定 `Mode=OneWay` (或 TwoWay)。 請參閱 [XAML 控制項；繫結一個 C++/WinRT 屬性](binding-property.md)。 |
| 您把 XAML 項目控制項繫結到可觀察的集合，且在執行階段擲回一個例外與「參數不正確」的訊息。 | 在您的 IDL 和實作中，宣告任何可觀察的集合做為類型 **Windows.Foundation.Collections.IVector<IInspectable>**。 但傳回實作 **Windows.Foundation.Collections.IObservableVector<T>** 的物件，其中 T 為您的元素類型。 請參閱 [XAML 項目控制項；繫結一個 C++/WinRT 集合](binding-collection.md)。  |
| C++ 編譯器產生一個錯誤的表單「*' MyImplementationType_base&lt;MyImplementationType&gt;'；沒有適當的預設建構函式可使用*」。|您已從有一個非小型建構函式衍生時，這可能會發生。 您衍生的類型建構函式需要與需要基本類型建構函式的參數一起傳遞。 如需一個已執行的範例，請參閱 [從一個不小的建構函式衍生](author-apis.md#deriving-from-a-type-that-has-a-non-trivial-constructor)。|
| C++ 編譯器產生錯誤「*無法從 'const std::vector&lt;std::wstring,std::allocator&lt;_Ty&gt;&gt;' 轉換至 'const winrt::param::async_iterable&lt;winrt::hstring&gt; &'*」。|您將 std::wstring 的 std::vector 傳遞到除了一個集合之外的 Windows 執行階段 API，便可能會發生此情況。 如需詳細資訊，請參閱 [標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)。|
| C++ 編譯器產生錯誤「*無法從 'const std::vector&lt;winrt::hstring,std::allocator&lt;_Ty&gt;&gt;' 轉換至 'const winrt::param::async_iterable&lt;winrt::hstring&gt; &'*」。|當您將 winrt::hstring 的 std::vector 傳遞至除了一個集合之外的 Windows 執行階段 API，且您已沒有將向量複製或移動到非同步被呼叫者，便會發生此情況。 如需詳細資訊，請參閱 [標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)。|
| 打開一個專案時，Visual Studio 產生此錯誤「*未安裝此專案的應用程式*」。|如果您尚未開始，您需要從 Visual Studio 的**新專案**對話方塊中安裝 **C++ 開發 Windows 通用工具**。 如果無法解決問題，則此專案可能取決於 C++/WinRT Visual Studio 擴充功能 (VSIX) (請查閱[適用於 C++/WinRT 的 Visual Studio 支援，以及 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix))。|
| Windows 應用程式認證套件測試產生執行階段類別的錯誤之一「*不是從 Windows 的基底類別衍生。所有可組合類別必須最終從 Windows 命名空間中的類別衍生*」。|任何執行階段類別 （在您的應用程式中宣告） 衍生自基底類別，稱為 「*可組合*類別。 可組合類別的最終基底類別必須是來自 windows.* 命名空間; 的類型例如， [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)。 請參閱[XAML 控制項; 繫結至 C + + /winrt 屬性](binding-property.md)如需詳細資訊。|
| C++ 編譯器產生一個 EventHandler 或 TypedEventHandler 特定委派的「*必須為 WinRT 類型*」錯誤。|請考慮改用 **winrt::delegate&lt;...T&gt;**。 請參閱 [在 C++/WinRT 中撰寫事件](author-events.md)。|
| C++ 編譯器產生一個特定 Windows 執行階段非同步作業的「*必須為 WinRT 類型*」錯誤。|請考慮改傳回平行模式程式庫 (PPL) [**工作**](https://msdn.microsoft.com/library/hh750113)。 請查閱[並行和非同步作業](concurrency.md)。|
| C++ 編譯器產生「*錯誤 C2220：警告視為錯誤，不產生 '物件' 檔案*」。|修正警告，或設定 **C/C++** > **一般** > **視警告為錯誤**為**No (/WX-)**。|
| 您的應用程式當機，因為在終結物件後，呼叫了在 C++/WinRT 物件中處理的事件。|請參閱 [在事件處裡常式中使用 *this* 物件](handle-events.md#using-the-this-object-in-an-event-handler)。|
| C++ 編譯器產生「*錯誤 C2338：這僅適用於弱式參考資料支援*」。|您為將 **winrt::no_weak_ref** 標記結構做為範本引數傳遞至其基底類別的類型，要求一個弱式參考資料。 請參閱[不使用弱式參考資料支援](weak-references.md#opting-out-of-weak-reference-support)|
| C + + 連結器會產生 「*錯誤 LNK2019： 無法解析的外部符號*」|請參閱[為何連結器給我 「 LNK2019： 無法解析的外部符號 」 錯誤？](faq.md#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error)|
| LLVM 和 Clang toolchain 會產生錯誤時搭配 C + + /winrt。|我們不支援 LLVM 和 Clang toolchain 適用於 C + + /winrt，但如果您想要模擬我們如何使用它在內部，則您可能會嘗試實驗，例如一個中所述[可以使用 LLVM/Clang 來編譯 C + + /winrt？](faq.md#can-i-use-llvmclang-to-compile-with-cwinrt)。|
| C + + 編譯器產生的投影類型的 「*沒有適當的預設建構函式*」。 | 如果您正嘗試延遲的執行階段類別物件中，初始化或使用，並在同一個專案中，實作執行階段類別，則您必須呼叫`nullptr_t`建構函式。 如需詳細資訊，請參閱 [使用 C++/WinRT 取用 API](consume-apis.md)。 |
| C + + 編譯器產生 「*錯誤 C3861: 'from_abi': 識別碼找不到*」，以及其他來自*base.h*的錯誤。 您可能會看到這個錯誤，如果您使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，以及 Windows SDK 版本 10.0.17134.0 (Windows 10，版本 1803年) 為目標。 | 無論是以更新版本 （更多符合） 為目標版本的 Windows SDK 中或將專案屬性**C/c + +** > **語言** > **一致性模式： 否**(此外，如果 **/ 已-** 會出現在專案屬性**C/c + +**  > **語言** > **命令列****的其他選項**，然後刪除它)。 |

> [!NOTE]
> 如果本主題未能回答您的問題，也許您可透過使用 [Stack Overflow 上的 `c++-winrt` 標記](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt) 來尋求協助。
