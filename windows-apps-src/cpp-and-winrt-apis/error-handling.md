---
description: 本主題討論使用 C++/WinRT 程式設計時處理錯誤的策略。
title: 使用 C++/WinRT 的錯誤處理
ms.date: 05/21/2018
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, 錯誤, 處理, 例外狀況
ms.localizationpriority: medium
ms.openlocfilehash: c6f7135e85ab63ddfe92bd0de8c656b58fb1a020
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8193017"
---
# <a name="error-handling-with-cwinrt"></a>使用 C++/WinRT 的錯誤處理

本主題討論使用進行程式設計時處理錯誤的策略[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。 如需更多一般資訊及背景，請參閱 [錯誤以及例外狀況處理（現代化 C++）](/cpp/cpp/errors-and-exception-handling-modern-cpp)。

## <a name="avoid-catching-and-throwing-exceptions"></a>避免攔截並擲回例外狀況
我們建議您繼續寫入 [異常安全程式碼](/cpp/cpp/how-to-design-for-exception-safety)，但您想要盡可能避免攔截並擲例外狀況。 如果沒有例外狀況的處理常式，則 Windows 會自動產生錯誤報告（包括損毀的小型傾印），這有助於您追蹤問題出在哪裡。

不要擲回您希望攔截的例外狀況。 且不要對預期失敗使用例外狀況。 *只有在發生未預期執行階段錯誤時* 才擲回例外狀況，並使用錯誤/結果碼&mdash; 直接處理其他所有項目，且接近失敗的來源。 如此一來，當例外 *被* 擲回時，您便知道原因不是您程式碼中的錯誤，便是系統中例外的錯誤狀態。

請考量存取 Windows 登錄的案例。 如果您的應用程式無法從登錄讀取值，這是可以預期的，且您應該適當地處理它。 不要擲回例外狀況；而是傳回 `bool` 或 `enum` 值，指出值未讀取可能的原因。 無法將值 *寫入* 登錄，換句話說，可能表示在您的應用程式中有超越您可合理處理的更大問題。 在這類案例中，您不想讓應用程式繼續，所以例外狀況產生的錯誤報告會是最快速的方式，防止您的應用程式造成任何損害。

舉另一個範例，請考從 [**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_) 的呼叫擷取一個縮圖影像，然後將該縮圖傳遞至 [**BitmapSource.SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_)。 如果該呼叫序列會讓您將 `nullptr` 傳遞到 **SetSourceAsync**（無法讀取影像檔案；附檔名或許讓它看起來像是包含影像的資料，但實際上並沒有），則將會導致擲回不正確的指標例外狀況。 如果在您的程式碼中找出像這樣的案例，而不是攔截並處理案例做為例外狀況，請改為檢查從 **GetThumbnailAsync** 傳回的 `nullptr`。

擲回例外狀況傾向於比使用錯誤碼來的慢。 發生嚴重錯誤時，如果您只擲回例外狀況，然後如果一切順利，則您不需要支付效能價格。

但更可能效能受影響涉及執行階段額外負荷，以確保萬一擲回例外狀況的情況下呼叫適當的解構函式。 是否實際值回例外狀況會產生此保證的費用。 因此，您應該確定編譯器是否了解哪些函式可能擲回例外狀況。 如果編譯器可以證明，特定函式 (`noexcept` 規格) 不會有任何例外狀況，便能最佳化它所產生的程式碼。

## <a name="catching-exceptions"></a>攔截例外狀況
在 HRESULT 值的表單中傳回在 [Windows 執行階段 ABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) 層出現的錯誤狀況。 但是，您不需要在程式碼中處理 HRESULT。 為 API 在使用端所產生的 C++/WinRT 投影程式碼，在 ABI 層偵測錯誤 HRESULT 程式碼且將程式碼轉換為 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 例外狀況，您可以攔截並處理。

例如，如果使用者剛好從圖片媒體櫃刪除一個影像，而您的應用程式逐一查看該集合時，則投影擲回一個例外狀況。 且這是您必須攔截並處理該例外狀況的案例。 以下程式碼範例示範此案例。

```cppwinrt
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
            HRESULT hr = ex.to_abi(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

呼叫 `co_await`-ed 函式時，在協同程式裡使用這個相同的模式。 此 HRESULT 例外狀況轉換的另一個範例是，元件 API 傳回 E_OUTOFMEMORY 時，會造成擲回一個 **std::bad_alloc**。

## <a name="throwing-exceptions"></a>擲回例外狀況
在某些情況下，如果您決定指定函式的呼叫失敗，則您的應用程式將無法復原（您將無法如預期般再依賴它來運作）。 下列程式碼範例使用 [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) 值做為從 [**CreateEvent**](https://msdn.microsoft.com/library/windows/desktop/ms682396) 傳回的 HANDLE 包裝函式。 然後將控制代碼 (從它建立 `bool` 值) 傳遞至 [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) 函式範本。 **winrt::check_bool** 使用 `bool`，或使用任何可轉換為 `false` （錯誤狀況），或 `true` （成功狀況）的值。

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

如果您傳遞至 [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) 的值是 false，則執行下列一系列動作。

- **winrt::check_bool** 呼叫 [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) 函式。
- **winrt:: throw_last_error**呼叫[**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360)來擷取呼叫執行緒的最後一個錯誤碼值，並接著會呼叫[**winrt:: throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)函式。
- **winrt::throw_hresult** 使用 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 物件（或標準物件）擲回例外狀況，代表該錯誤碼。

Windows Api 報告執行階段錯誤使用各種傳回值類型，因此除了 **winrt::check_bool** 之外，還有少數其他實用的協助程式函式，適用於檢查值與擲回例外狀況。

- [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)。 檢查 HRESULT 碼是否代表錯誤，若是如此，則呼叫 **winrt::throw_hresult**。
- [**winrt::check_nt**](/uwp/cpp-ref-for-winrt/error-handling/check-nt)。 檢查程式碼是否代表錯誤，若是如此，則呼叫 **winrt::throw_hresult**。
- [**winrt::check_pointer**](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)。 檢查指標是否為 null，若是如此，則呼叫 **winrt::throw_last_error**。
- [**winrt::check_win32**](/uwp/cpp-ref-for-winrt/error-handling/check-win32)。 檢查程式碼是否代表錯誤，若是如此，則呼叫 **winrt::throw_hresult**。

您可以將這些協助程式函式用於一般傳回碼類型，或者您可以回應任何錯誤狀況並呼叫 [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) 或 [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)。 

## <a name="throwing-exceptions-when-authoring-an-api"></a>撰寫 API 時，擲回例外狀況
跨 [Windows 執行階段 ABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) 界限的例外狀況無效，因此在 HRESULT 錯誤碼的表單中跨 ABI 層傳回實作中出現的錯誤狀況。 您使用 C++/WinRT 撰寫 API 時，產生程式碼讓您可轉換任何例外狀況，在實作中您將其 *執行* 擲回 HRESULT。 [**Winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) 函式用於像這樣的模式中所產生的程式碼。

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

[**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) 處理衍生自 **std::exception** 的例外狀況，以及 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)，和其所衍生的類型。 實作中，您應該偏好 **winrt::hresult_error**，或衍生的類型，如此一來，您 API 的消費者便能接收豐富的錯誤資訊。 如果使用標準範本庫出現例外狀況，則支援 **std::exception**（對應至 E_FAIL）。

## <a name="assertions"></a>判斷提示
針對您應用程式中的內部假設有判斷提示。 盡可能優先選擇 **static_assert** 進行編譯階段驗證。 針對執行階段狀況，請用布林運算是來使用 WINRT_ASSERT。

```cppwinrt
WINRT_ASSERT(pos < size());
```

在發行組建中編譯 WINRT_ASSERT；在偵錯組建中，它停止偵錯工具中判斷提示所在的這行程式碼上的應用程式。

您不應該在解構函式中使用例外狀況。 因此，至少在偵錯組建中，您可以使用 WINRT_VERIFY（使用布林運算式）和 WINRT_VERIFY_ （使用預期結果與布林運算式）從解構函式判斷呼叫函式的結果。

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
* [錯誤和例外狀況處理（現代化 C++）](/cpp/cpp/errors-and-exception-handling-modern-cpp)
* [作法：例外狀況安全的設計](/cpp/cpp/how-to-design-for-exception-safety)
