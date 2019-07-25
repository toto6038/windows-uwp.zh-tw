---
description: 本主題討論使用 C++/WinRT 程式設計時處理錯誤的策略。
title: 使用 C++/WinRT 處理錯誤
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, error, handling, exception, 標準, 投影, 錯誤, 處理, 例外狀況
ms.localizationpriority: medium
ms.openlocfilehash: 37819d1626d3adc6f5647f447567a9273e72668d
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270134"
---
# <a name="error-handling-with-cwinrt"></a>使用 C++/WinRT 處理錯誤

本主題討論使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 進行程式設計時處理錯誤的策略。 如需更多一般資訊及背景，請參閱[錯誤和例外狀況處理 (新式 C++)](/cpp/cpp/errors-and-exception-handling-modern-cpp)。

## <a name="avoid-catching-and-throwing-exceptions"></a>避免攔截和擲回例外狀況
我們建議您繼續撰寫[異常安全程式碼](/cpp/cpp/how-to-design-for-exception-safety)，但您想要盡可能避免攔截和擲回例外狀況。 如果沒有例外狀況的處理常式，則 Windows 會自動產生錯誤報告 (包括損毀的小型傾印)，這有助於您追蹤問題出在哪裡。

不要擲回您希望攔截的例外狀況。 且不要對預期失敗使用例外狀況。 「只有在發生未預期執行階段錯誤時」  才擲回例外狀況，並使用錯誤/結果碼處理其他所有項目 &mdash; 直接且接近失敗的來源。 如此一來，當例外狀況被「擲回時」  ，您便知道原因是您程式碼中的錯誤，或者系統中例外的錯誤狀態。

請考量存取 Windows 登錄的案例。 如果您的應用程式無法從登錄讀取值，這是可以預期的，且您應該適當地處理它。 不要擲回例外狀況；而是傳回 `bool` 或 `enum` 值，指出值未讀取可能的原因。 無法將值「寫入」  登錄，換句話說，問題可能已超過可在您應用程式中合理地處理的範圍。 在這類案例中，您不想讓應用程式繼續，所以例外狀況產生的錯誤報告會是最快速的方式，防止您的應用程式造成任何損害。

舉另一個範例，請考慮從 [**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_) 的呼叫擷取一個縮圖影像，然後將該縮圖傳遞至 [**BitmapSource.SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_)。 如果該呼叫序列會讓您將 `nullptr` 傳遞到 **SetSourceAsync** (無法讀取影像檔案；附檔名或許讓它看起來像是包含影像的資料，但實際上並沒有)，則將會導致擲回不正確的指標例外狀況。 如果在您的程式碼中找出像這樣的案例，而不是作為例外狀況攔截並處理案例，請改為檢查從 **GetThumbnailAsync** 傳回的 `nullptr`。

擲回例外狀況傾向於比使用錯誤碼來的慢。 如果您只在發生嚴重錯誤時擲回例外狀況，則當一切順利時，您將不需要犧牲效能。

但更可能的是，效能受影響涉及執行階段額外負荷，這是為了確保在擲回例外狀況的罕見情況下，會呼叫適當的解構函式。 此保證措施的代價依是否實際擲回例外狀況而定。 因此，您應該確保編譯器知道哪些函式可能擲回例外狀況。 如果編譯器可以證明，特定函式 (`noexcept` 規格) 不會有任何例外狀況，便能最佳化它所產生的程式碼。

## <a name="catching-exceptions"></a>攔截例外狀況
在 [Windows 執行階段 ABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) 層出現的錯誤狀況，是以 HRESULT 值的格式傳回。 但是，您不需要在程式碼中處理 HRESULT。 在使用端上針對 API 所產生的 C++/WinRT 投影程式碼，在 ABI 層偵測到錯誤 HRESULT 程式碼且將程式碼轉換為 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 例外狀況，您可以攔截並處理。 如果您確定  要處理 HRESULTS，請使用 **winrt::hresult** 型別。

例如，如果使用者剛好從圖片媒體櫃刪除一個影像，而您的應用程式正在逐一查看該集合時，則投影會擲回例外狀況。 在此情況下，您必須攔截並處理該例外狀況。 以下程式碼範例顯示此案例。

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.UI.Xaml.Media.Imaging.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage;
using namespace Windows::UI::Xaml::Media::Imaging;

IAsyncAction MakeThumbnailsAsync()
{
    auto imageFiles{ co_await KnownFolders::PicturesLibrary().GetFilesAsync() };

    for (StorageFile const& imageFile : imageFiles)
    {
        BitmapImage bitmapImage;
        try
        {
            auto thumbnail{ co_await imageFile.GetThumbnailAsync(FileProperties::ThumbnailMode::PicturesView) };
            if (thumbnail) bitmapImage.SetSource(thumbnail);
        }
        catch (winrt::hresult_error const& ex)
        {
            winrt::hresult hr = ex.to_abi(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

呼叫使用 `co_await` 的函式時，請在協同程式裡使用此相同模式。 此 HRESULT 例外狀況轉換的另一個範例是，在元件 API 傳回 E_OUTOFMEMORY (造成擲回 **std::bad_alloc**) 時。

## <a name="throwing-exceptions"></a>擲回例外狀況
在某些情況下，如果您決定指定函式的呼叫失敗，則您的應用程式將無法復原 (您將無法如預期般再依賴它來運作)。 下列程式碼範例使用 [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) 值作為從 [**CreateEvent**](https://docs.microsoft.com/windows/desktop/api/synchapi/nf-synchapi-createeventa) 傳回的 HANDLE 包裝函式。 然後將控制代碼 (從它建立 `bool` 值) 傳遞至 [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) 函式範本。 **winrt::check_bool** 處理 `bool`，或任何可轉換為 `false` (錯誤狀況)，或 `true` (成功狀況) 的值。

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

如果您傳遞至 [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) 的值是 false，則執行下列一系列動作。

- **winrt::check_bool** 呼叫 [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) 函式。
- **winrt::throw_last_error** 呼叫 [**GetLastError**](https://docs.microsoft.com/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror) 來擷取呼叫執行緒的最後一個錯誤碼值，並再呼叫 [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult) 函式。
- **winrt::throw_hresult** 使用代表該錯誤碼的 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 物件 (或標準物件) 擲回例外狀況。

因為 Windows API 報告執行階段錯誤使用各種傳回值型別，所以除了 **winrt::check_bool** 之外，還有許多其他實用的協助程式函式，適用於檢查值與擲回例外狀況。

- [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)。 檢查 HRESULT 碼是否代表錯誤，若是如此，則呼叫 **winrt::throw_hresult**。
- [**winrt::check_nt**](/uwp/cpp-ref-for-winrt/error-handling/check-nt)。 檢查程式碼是否代表錯誤，若是如此，則呼叫 **winrt::throw_hresult**。
- [**winrt::check_pointer**](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)。 檢查指標是否為 null，若是如此，則呼叫 **winrt::throw_last_error**。
- [**winrt::check_win32**](/uwp/cpp-ref-for-winrt/error-handling/check-win32)。 檢查程式碼是否代表錯誤，若是如此，則呼叫 **winrt::throw_hresult**。

您可以將這些協助程式函式用於一般傳回碼類型，或者您可以回應任何錯誤狀況並呼叫 [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) 或 [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)。 

## <a name="throwing-exceptions-when-authoring-an-api"></a>在撰寫 API 時擲回例外狀況
所有 [Windows 執行階段應用程式二進位介面](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types)界限 (或 ABI 界限) 都必須是 *noexcept*&mdash;這表示例外狀況絕對不能在該處逸出。 當您撰寫 API 時，您應該一律使用 C++ `noexcept` 關鍵字來標記 ABI 界限。 `noexcept` 在 C++ 中有特定行為。 如果 C++ 例外狀況命中 `noexcept` 界限，則此程序會因為 **std::terminate** 而立即失敗。 這種行為通常是理想的做法，因為未處理的例外狀況幾乎一律暗指程序中不明的狀態。

例外狀況不得跨 ABI 界限，因此在實作中出現的錯誤狀況，是以 HRESULT 錯誤碼格式跨 ABI 層傳回。 您使用 C++/WinRT 撰寫 API 時，系統會為您產生程式碼，將您在實作中「擲回」  的任何例外狀況轉換成 HRESULT。 所產生的程式碼中是以此模式使用 [**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) 函式。

```cppwinrt
HRESULT DoWork() noexcept
{
    try
    {
        // Shim through to your C++/WinRT implementation.
        return S_OK;
    }
    catch (...)
    {
        return winrt::to_hresult(); // Convert any exception to an HRESULT.
    }
}
```

[**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) 處理衍生自 **std::exception** 和 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 及其衍生型別的例外狀況。 在實作中，您應該慣用 **winrt::hresult_error** 或衍生型別，如此一來，您 API 的取用者便能接收豐富的錯誤資訊。 如果使用標準範本庫出現例外狀況，系統也支援 **std::exception** (對應至 E_FAIL)。

### <a name="debuggability-with-noexcept"></a>noexcept 的偵錯能力
如先前所述，命中 `noexcept` 界限的 C++ 例外狀況會因為 **std::terminate** 而立即失敗。 這不適合用於偵錯，因為 **std::terminate** 通常會遺失太多數或所有擲回的錯誤或例外狀況內容，特別是在涉及協同程式時。

因此，本節會處理您的 ABI 方法 (您已正確使用 `noexcept` 標註) 使用 `co_await` 來呼叫非同步C++/WinRT 投影程式碼的情況。 我們建議您將對 C++/WinRT 投影程式碼的呼叫包裝在 **winrt::fire_and_forget**內。 這麼做可提供適當的位置，讓未處理的例外狀況正確地記錄為 Stowed 例外狀況，進而大幅提高偵錯能力。

```cppwinrt
HRESULT MyWinRTObject::MyABI_Method() noexcept
{
    winrt::com_ptr<Foo> foo{ get_a_foo() };

    [/*no captures*/](winrt::com_ptr<Foo> foo) -> winrt::fire_and_forget
    {
        co_await winrt::resume_background();

        foo->ABICall();

        AnotherMethodWithLotsOfProjectionCalls();
    }(foo);

    return S_OK;
}
```

**winrt::fire_and_forget** 具有內建 `unhandled_exception` 方法協助程式，其會呼叫 **winrt::terminate**，而後會接著呼叫 **RoFailFastWithErrorContext**。 這保證會保留任何內容 (Stowed 例外狀況、錯誤碼、錯誤訊息、堆疊反向追蹤等等) 以便進行即時偵錯或事後剖析傾印。 為了方便起見，您可以將自主導引部分分解成個別的函式，該函式可傳回 **winrt::fire_and_forget**，然後呼叫該函式。

### <a name="synchronous-code"></a>同步程式碼
在某些情況下，您的 ABI 方法 (同樣地，您已使用 `noexcept` 正確標註) 只會呼叫同步程式碼。 換句話說，它絕不會使用 `co_await` 來呼叫非同步 Windows 執行階段方法，或是在前景與背景執行緒之間切換。 在這種情況下，fire_and_forget 技巧仍有作用，但沒有效果。 您可改為執行如下所似的動作。

```cppwinrt
HRESULT abi() noexcept try
{
    // ABI code goes here.
} catch (...) { winrt::terminate(); }
```

### <a name="fail-fast"></a>立即失敗
上一節中的程式碼仍會立即失敗。 如同所述，該程式碼不會處理任何例外狀況。 任何未處理的例外狀況都會導致程式終止。

但是該形式較佳，因為它可確保偵錯能力。 在少數情況下，您可能想要 `try/catch`，以及處理特定例外狀況。 但這應該很少見，如本主題所述，我們不鼓勵針對您預期的狀況，使用例外狀況作為流程控制機制。

請記住，讓未處理的例外狀況逸出暴露的 `noexcept` 內容並不是個好主意。 在這種情況下，C++ 執行階段會 **std::terminate** 程序，因而失去 C++/WinRT 小心記錄的任何 Stowed 例外狀況資訊。

## <a name="assertions"></a>判斷提示
針對您應用程式中的內部假設，可使用判斷提示。 盡可能優先選擇 **static_assert** 進行編譯階段驗證。 針對執行階段條件，請搭配使用 `WINRT_ASSERT` 和布林運算式。 `WINRT_ASSERT` 是巨集定義，而且會發展為 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

```cppwinrt
WINRT_ASSERT(pos < size());
```

在發行組建中，WINRT_ASSERT 會在編譯時移除；在偵錯組建中，它會將偵錯工具中的應用程式停止在判斷提示所在的程式碼行上。

您不應該在解構函式中使用例外狀況。 因此，至少在偵錯組建中，您可以使用 WINRT_VERIFY (搭配布林運算式) 和 WINRT_VERIFY_ (搭配預期結果與布林運算式)，從解構函式判斷呼叫函式的結果。

```cppwinrt
WINRT_VERIFY(::CloseHandle(value));
WINRT_VERIFY_(TRUE, ::CloseHandle(value));
```

## <a name="important-apis"></a>重要 API
* [winrt::check_bool 函式範本](/uwp/cpp-ref-for-winrt/error-handling/check-bool)
* [winrt::check_hresult 函式](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::check_nt 函式範本](/uwp/cpp-ref-for-winrt/error-handling/check-nt)
* [winrt::check_pointer 函式範本](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)
* [winrt::check_win32 函式範本](/uwp/cpp-ref-for-winrt/error-handling/check-win32)
* [winrt::handle 結構](/uwp/cpp-ref-for-winrt/handle)
* [winrt::hresult_error 結構](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::throw_hresult 函式](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)
* [winrt::throw_last_error 函式](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error)
* [winrt::to_hresult 函式](/uwp/cpp-ref-for-winrt/error-handling/to-hresult)

## <a name="related-topics"></a>相關主題
* [錯誤和例外狀況處理 (新式 C++)](/cpp/cpp/errors-and-exception-handling-modern-cpp)
* [如何：例外狀況安全的設計](/cpp/cpp/how-to-design-for-exception-safety)
