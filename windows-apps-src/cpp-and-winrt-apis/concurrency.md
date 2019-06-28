---
description: 本主題示範的方式，您可以使用 C++/WinRT，同時建立及使用 Windows 執行階段非同步物件。
title: 透過 C++/WinRT 的並行和非同步作業
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, 並行, async, 非同步的, 非同步
ms.localizationpriority: medium
ms.openlocfilehash: 910d7a7ca2aaebac6dd462d7104b26a989cf8814
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "66721663"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>透過 C++/WinRT 的並行和非同步作業

本主題示範的方式，您可以使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，同時建立及使用 Windows 執行階段非同步物件。

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>非同步作業與 Windows 執行階段 "Async" 函式

實作有可能超過 50 毫秒完成的任何 Windows 執行階段 API 做為非同步函式 (名稱以 "Async" 結尾)。 非同步函式的實作在另一個執行續上起始工作，並立即傳回代表非同步作業的物件。 非同步作業完成時，傳回包含工作所產生任何值的物件。 **Windows::Foundation** Windows 執行階段命名空間包含四種非同步作業物件。

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)、
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)、
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_)，以及
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)。

這些非同步作業類型的每一個皆投影到 **winrt::Windows::Foundation** C++/WinRT 命名空間中的對應類型。 C++/WinRT 也包含內部等待配接器結構。 您不使用直接它，但多虧了有該結構，您可以撰寫 `co_await` 陳述式，合作等待傳回這些非同步作業類型之一的任一函式結果。 且您可以撰寫自己的協同程式，傳回這些類型。

非同步 Windows 函式的範例是 [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)，其傳回類型 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) 的非同步作業物件。 讓我們看一些使用 C++/WinRT 來呼叫這類 API 的方式 (先看封鎖，再看非封鎖)。

## <a name="block-the-calling-thread"></a>封鎖呼叫執行緒

下列的程式碼範例從 **RetrieveFeedAsync** 接收一個非同步作業物件，並在該物件上呼叫 **get**，封鎖呼叫執行緒，直到非同步作業的結果可用。

如果您想要將此範例直接複製並貼到 **Windows 主控台應用程式 (C++/WinRT)** 專案的主要原始碼檔案，則請先在專案屬性中設定 [不使用預先編譯的標頭]  。

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeed()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
    // use syndicationFeed.
}

int main()
{
    winrt::init_apartment();
    ProcessFeed();
}
```

呼叫 **get** 可方便您進行程式設計，它適用於您基於任何原因不想使用協同程式的主控台應用程式或背景執行緒。 但其非並行也不是非同步，因此不適合 UI 執行緒 (且如果您嘗試在其中一個使用，會在非最佳化的組建中觸發聲明)。 若要避免 OS 執行緒阻塞而無法執行其他有用的工作，我們需要不同的技術。

## <a name="write-a-coroutine"></a>撰寫協同程式

C++/WinRT 與 C++ 協同程式整合為程式設計模型，提供以自然的方式合作等待結果。 您可以藉由撰寫協同程式產生自己的 Windows 執行階段非同步作業。 下列程式碼範例中，**ProcessFeedAsync** 是協同程式。

> [!NOTE]
> **get** 函式存在於 C++/WinRT 投影類型 **winrt::Windows::Foundation::IAsyncAction** 上，因此您可以從任何 C++/WinRT 專案內呼叫該函式。 您不會看到該函式列為 [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) 介面的成員，因為 **get** 不屬於實際 Windows 執行階段類型 **IAsyncAction** 的應用程式二進位介面 (ABI) 介面。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    PrintFeed(syndicationFeed);
}

int main()
{
    winrt::init_apartment();

    auto processOp{ ProcessFeedAsync() };
    // do other work while the feed is being printed.
    processOp.get(); // no more work to do; call get() so that we see the printout before the application exits.
}
```

協同程式是一個函式，可暫停和繼續。 在上述的 **ProcessFeedAsync** 協同程式中，到達 `co_await` 陳述式時，協同程式便非同步起始 **RetrieveFeedAsync** 呼叫並立即自行暫停和將控制項傳回給呼叫者 (其為上述範例中的 **main**)。 **main** 可以繼續執行工作，同時擷取和列印摘要。 完成時 (當 **RetrieveFeedAsync** 呼叫完成時)，**ProcessFeedAsync** 協同程式在下一個陳述式繼續。

您可以將一個協同程式彙總到其他協同程式。 或您可以呼叫 **get** 封鎖並等待完成 (取得結果，如果有的話)。 或者，您可以將它傳遞到另一個程式設計語言，其支援 Windows 執行階段。

也可以藉由使用委派，處理非同步動作與作業完成的和/或進行中事件。 如需詳細資訊和程式碼範例，請參閱[非同步動作與作業的委派類型](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)。

## <a name="asychronously-return-a-windows-runtime-type"></a>非同步傳回 Windows 執行階段類型

在下一個範例中，我們將呼叫包裝在 **RetrieveFeedAsync**，給特定的 URI，來提供我們 **RetrieveBlogFeedAsync** 函式，其非同步傳回一個 [**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed)。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> RetrieveBlogFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    return syndicationClient.RetrieveFeedAsync(rssFeedUri);
}

int main()
{
    winrt::init_apartment();

    auto feedOp{ RetrieveBlogFeedAsync() };
    // do other work.
    PrintFeed(feedOp.get());
}
```

上述範例中，**RetrieveBlogFeedAsync** 傳回 **IAsyncOperationWithProgress**，其同時具有處理程序和一個傳回值。 我們可以執行其他工作，同時 **RetrieveBlogFeedAsync** 執行其項目與擷取摘要。 接著，我們在非同步作業物件上呼叫 **get** 封鎖，等待其完成，再取得作業的結果。

如果您正非同步傳回 Windows 執行階段類型，則您應該要傳回 [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) 或 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)。 任何第一或第三方執行階段類別資格，或任何可從 Windows Runtime 函式或傳送至 Windows Runtime 函式的任何類型 (例如，`int`，或 **winrt::hstring**)。 編譯器將會協助您解決 *必須為 WinRT 類型* 的錯誤，如果您嘗試使用非 Windows 執行階段類型的這些非同步作業類型之一的話。

如果協同程式沒有至少一個 `co_await` 陳述式，為了符合協同程式，它必須至少有一個 `co_return` 或一個 `co_yield` 陳述式。 在這個情況下，您的協同程式不用引入任何非同步便可以傳回一個值，且因此不封鎖也不會切換內容。 以下是透過快取值這樣做（稱為第二次以上的次數）的範例。

```cppwinrt
winrt::hstring m_cache;

IAsyncOperation<winrt::hstring> ReadAsync()
{
    if (m_cache.empty())
    {
        // Asynchronously download and cache the string.
    }
    co_return m_cache;
}
``` 

## <a name="asychronously-return-a-non-windows-runtime-type"></a>非同步傳回非 Windows 執行階段類型

如果您正非同步傳回類型，其 *不是* Windows 執行階段類型，則您應該要傳回平行模式程式庫 (PPL) [**concurrency::task**](/cpp/parallel/concrt/reference/task-class)。 我們建議 **concurrency::task**，因為它提供您比 **std::future** 更好的效能 (和往後較佳的相容性)。

> [!TIP]
> 如果您包含 `<pplawait.h>`，然後您可以使用 **concurrency::task** 做為協同程式類型。

```cppwinrt
// main.cpp
#include <iostream>
#include <ppltasks.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

concurrency::task<std::wstring> RetrieveFirstTitleAsync()
{
    return concurrency::create_task([]
        {
            Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
            SyndicationClient syndicationClient;
            SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
            return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
        });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp{ RetrieveFirstTitleAsync() };
    // Do other work here.
    std::wcout << firstTitleOp.get() << std::endl;
}
```

## <a name="parameter-passing"></a>參數傳遞

針對同步功能，您應該使用預設的 `const&` 參數。 將可避免複本的額外負荷（這牽涉到參考計數，且這表示連鎖遞增與遞減）。

```cppwinrt
// Synchronous function.
void DoWork(Param const& value);
```

如果您將參考參數傳遞至協同程式，則您會遇到問題。

```cppwinrt
// NOT the recommended way to pass a value to a coroutine!
IASyncAction DoWorkAsync(Param const& value)
{
    // While it's ok to access value here...

    co_await DoOtherWorkAsync();

    // ...accessing value here carries no guarantees of safety.
}
```

在協同程式中，執行是同步的，直到第一個暫停點，其傳回控制項至呼叫端。 協同程式恢復時，參考參數參考的來源值可能發生任何事情。 從協同程式的觀點，參考參數有不受控制的存留期。 因此，上述範例中，我們安全存取 *值* 直到 `co_await`，但不是在其之後。 我們也不能安全的將 *值* 傳遞至 **DoOtherWorkAsync** 如果存在任何風險，該函式將會暫停，然後在其恢復後嘗試使用 *值*。 若要讓參數在暫停與恢復之後安全使用，您的協同程式必須使用預設的透過值傳遞，以確保他們透過值擷取並避免存留期問題。 如果您確定可以安全執行此操作，那麼您幾乎不會偏離該指導方針。

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value);
```

透過 const 值來傳遞也是一個很好的做法 (除非您想要移動該值)。 它不會對您製作複本的來源值有任何影響，但它意圖明確，且有助於您無意中修改複本。

```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

也請參閱[標準陣列和向量](std-cpp-data-types.md#standard-arrays-and-vectors)，處理如何將標準向量傳遞至非同步被呼叫者。

## <a name="offloading-work-onto-the-windows-thread-pool"></a>卸載工作至 Windows 執行緒集區

協同程式和任何其他函式的類似之處在於，呼叫者會一直遭到封鎖，直到有函式對其傳回執行。 而且，第一個傳回協同程式的機會是第一個 `co_await`、`co_return` 或 `co_yield`。

因此，您在協同程式中進行計算繫結工作之前，必須先將執行傳回給呼叫者 (亦即，引入暫停點)，呼叫者才不會遭到封鎖。 如果您尚未透過對一些其他作業進行 `co_await` 來做到這一點，則可以對 [**winrt::resume_background**](/uwp/cpp-ref-for-winrt/resume-background) 函式進行 `co_await`。 其將控制項傳回給呼叫者，並立即恢復執行緒集區執行緒的執行。

實作中使用的執行緒集區是低層級的 [Windows 執行緒集區](https://docs.microsoft.com/windows/desktop/ProcThread/thread-pool-api)，所以是最有效的。

```cppwinrt
IAsyncOperation<uint32_t> DoWorkOnThreadPoolAsync()
{
    co_await winrt::resume_background(); // Return control; resume on thread pool.

    uint32_t result;
    for (uint32_t y = 0; y < height; ++y)
    for (uint32_t x = 0; x < width; ++x)
    {
        // Do compute-bound work here.
    }
    co_return result;
}
```

## <a name="programming-with-thread-affinity-in-mind"></a>考量使用執行緒親和性程式設計

從前一個案例展開此案例。 您將一些工作卸載至執行緒集區，但您想在使用者介面 (UI) 中顯示進度。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

上方的程式碼擲回一個 [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) 例外，因為 **TextBlock** 必須從建立它的執行緒進行更新，也就是 UI 執行緒。 一種解決方案就是擷取我們最初呼叫的協同程式的執行緒內容。 若要這樣做，請具現化 [**winrt::apartment_context**](/uwp/cpp-ref-for-winrt/apartment-context) 物件、執行背景工作，然後對 **apartment_context** 進行 `co_await` 來切換回呼叫內容。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    winrt::apartment_context ui_thread; // Capture calling context.

    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await ui_thread; // Switch back to calling context.

    textblock.Text(L"Done!"); // Ok if we really were called from the UI thread.
}
```

只要從建立 **TextBlock** 的 UI 執行緒呼叫上述的協同程式，然後這項技術便可運作。 在您確定的應用程式中將有許多案例。

如需更通用的 UI 更新解決方案 (可涵蓋您對呼叫執行緒不是很確定的案例)，您可以對 [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 進行 `co_await` 來切換至特定前景執行緒。 在下列程式碼範例中，我們透過傳遞與 **TextBlock** (透過存取其[**發送器**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher) 屬性) 相關聯的發送器物件，來指定前景執行緒。 **winrt::resume_foreground** 的實作在發送器物件上呼叫 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)，來執行協同程式中之後的工作。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>在協同程式中執行內容、繼續和切換

概括來說，在協同程式中的暫停點之後，執行作業的原始執行緒可能會消失，因此繼續作業可能會在任何執行緒上發生 (換句話說，任何執行緒都有可能會呼叫非同步作業的**已完成**方法)。

但如果您對四種 Windows 執行階段非同步作業類型 (**IAsyncXxx**) 的任何一種進行 `co_await`，則 C++/WinRT 會在您進行 `co_await` 的時間點擷取呼叫內容。 而且，其可確保接續繼續執行時，您仍在該內容上。 C++/WinRT 能做到這一點的方法是，確認您是否已在該呼叫內容上，如果不在，則切換至該內容。 如果您在進行 `co_await` 之前位於單一執行緒 Apartment (STA) 執行緒上，則之後也會在同一個執行緒上；如果您在進行 `co_await` 之前位於多執行緒 Apartment (MTA) 執行緒上，則之後也會在該執行緒上。

```cppwinrt
IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    // The thread context at this point is captured...
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    // ...and is restored at this point.
}
```

之所以可以依賴此行為，原因是 C++/WinRT 會提供程式碼來將這些 Windows 執行階段非同步作業類型調整為 C++ 協同程式語言支援 (這些程式碼片段稱為等待配接器)。 C++/WinRT 中其餘的可等待類型就只是執行緒集區包裝函式和 (或) 協助程式；因此會在執行緒集區上完成。

```cppwinrt
using namespace std::chrono;
IAsyncOperation<int> return_123_after_5s()
{
    // No matter what the thread context is at this point...
    co_await 5s;
    // ...we're on the thread pool at this point.
    co_return 123;
}
```

如果您對某些其他類型進行 `co_await` (甚至在 C++/WinRT 協同程式實作內)，則另一個程式庫會提供配接器，且您必須了解這些配接器在繼續和內容方面的作用。

若要讓內容切換次數保持在最低狀態，您可以使用一些已在本主題中看過的技術。 我們來看看一些操作示範。 在接下來的這個虛擬程式碼範例中，我們會概述某個事件處理常式，其會呼叫 Windows 執行階段 API 來載入映像、放置到背景執行緒來處理該映像，然後傳回到 UI 執行緒以在 UI 中顯示該映像。

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    // Call StorageFile::OpenAsync to load an image file.

    // The call to OpenAsync occurred on a background thread, but C++/WinRT has restored us to the UI thread by this point.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

在此案例中，**StorageFile::OpenAsync** 的呼叫效率有點差。 因此，必須在繼續時 (C++/WinRT 會在此時間點之後還原 UI 執行緒內容) 將內容切換至背景執行緒 (以便讓處理常式能夠將執行傳回給呼叫者)。 但在此情況下，則不需要在 UI 執行緒上進行切換，直到我們即將更新 UI 時才有需要。 在呼叫 **winrt::resume_background** 之前  所呼叫的 Windows 執行階段 API 越多，便會產生更多不必要的來回內容切換。 其解決辦法是不要在這之前呼叫任何  Windows 執行階段 API。 將這些 API 全都移到 **winrt::resume_background** 之後。

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Call StorageFile::OpenAsync to load an image file.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

如果您想要更進一步地進行某些作業，則可以撰寫自己的等待配接器。 例如，如果您想要讓 `co_await` 在非同步動作完成時所在的相同執行緒上繼續進行 (因此，沒有任何內容切換)，則可以藉由撰寫與下面所示範例類似的等待配接器來開始進行。

> [!NOTE]
> 所提供的下列程式碼範例僅供用於教育用途；其目的是要讓您開始了解等待配接器的運作方式。 如果您想要在自己的程式碼基底中使用這項技術，建議您開發並測試您自己的等待配接器結構。 例如，您可以撰寫 **complete_on_any**、**complete_on_current** 和 **complete_on(dispatcher)** 。 也請考慮讓這些項目變成會以 **IAsyncXxx** 類型作為範本參數的範本。

```cppwinrt
struct no_switch
{
    no_switch(Windows::Foundation::IAsyncAction const& async) : m_async(async)
    {
    }

    bool await_ready() const
    {
        return m_async.Status() == Windows::Foundation::AsyncStatus::Completed;
    }

    void await_suspend(std::experimental::coroutine_handle<> handle) const
    {
        m_async.Completed([handle](Windows::Foundation::IAsyncAction const& /* asyncInfo */, Windows::Foundation::AsyncStatus const& /* asyncStatus */)
        {
            handle();
        });
    }

    auto await_resume() const
    {
        return m_async.GetResults();
    }

private:
    Windows::Foundation::IAsyncAction const& m_async;
};
```

若要了解如何使用 **no_switch** 等待配接器，您必須先知道當 C++ 編譯器遇到 `co_await` 運算式時，其會尋找稱為 **await_ready**、**await_suspend** 和 **await_resume** 的函式。 C++/WinRT 程式庫會提供這些函式，以便您會獲得合理的預設行為，如下所示。

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

若要使用 **no_switch** 等待配接器，只要將該 `co_await` 運算式的類型從 **IAsyncXxx** 變更為 **no_switch** 即可，如下所示。

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

然後，C++ 編譯器不會尋找那三個符合 **IAsyncXxx** 的 **await_xxx** 函式，而是會尋找符合 **no_switch** 的函式。

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>取消非同步作業，以及取消回呼

Windows 執行階段的非同步程式設計功能可讓您取消進行中的非同步動作或作業。 以下範例會呼叫 [**StorageFolder::GetFilesAsync**](/uwp/api/windows.storage.storagefolder.getfilesasync) 來擷取可能很大的檔案集合，並將產生的非同步作業物件儲存在資料成員中。 使用者可選擇取消該作業。

```cppwinrt
// MainPage.xaml
...
<Button x:Name="workButton" Click="OnWork">Work</Button>
<Button x:Name="cancelButton" Click="OnCancel">Cancel</Button>
...

// MainPage.h
...
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.Search.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::Storage;
using namespace Windows::Storage::Search;
using namespace Windows::UI::Xaml;
...
struct MainPage : MainPageT<MainPage>
{
    MainPage()
    {
        InitializeComponent();
    }

    IAsyncAction OnWork(IInspectable /* sender */, RoutedEventArgs /* args */)
    {
        workButton().Content(winrt::box_value(L"Working..."));

        // Enable the Pictures Library capability in the app manifest file.
        StorageFolder picturesLibrary{ KnownFolders::PicturesLibrary() };

        m_async = picturesLibrary.GetFilesAsync(CommonFileQuery::OrderByDate, 0, 1000);

        IVectorView<StorageFile> filesInFolder{ co_await m_async };

        workButton().Content(box_value(L"Done!"));

        // Process the files in some way.
    }

    void OnCancel(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        if (m_async.Status() != AsyncStatus::Completed)
        {
            m_async.Cancel();
            workButton().Content(winrt::box_value(L"Canceled"));
        }
    }

private:
    IAsyncOperation<::IVectorView<StorageFile>> m_async;
};
...
```

至於要如何實作取消，請先看一下這個簡單的範例。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction ImplicitCancellationAsync()
{
    while (true)
    {
        std::cout << "ImplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto implicit_cancellation{ ImplicitCancellationAsync() };
    co_await 3s;
    implicit_cancellation.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

如果您執行了上面的範例，便會看到 **ImplicitCancellationAsync** 會每秒列印一則訊息，並持續三秒，然後便會因為遭到取消而自動終止。 其運作原理是，在遇到 `co_await` 運算式時，協同程式會確認其是否已遭到取消。 如果是，其便會短路；如果否，則會正常暫止。

當然，協同程式暫止時也會發生取消。 只有當協同程式繼續或叫用另一個 `co_await` 時，才會確認是否已取消。 其問題是在回應取消時會有一個可能太粗略的延遲。

因此，另一個選項是從協同程式內明確地輪詢取消。 使用下列程式碼來更新上面的範例。 在這個新的範例中，**ExplicitCancellationAsync** 會擷取 [**winrt::get_cancellation_token**](/uwp/cpp-ref-for-winrt/get-cancellation-token) 函式所傳回的物件，並使用該物件來定期確認協同程式是否已遭到取消。 協同程式只要未遭到取消就會無限地迴圈；一旦遭到取消，迴圈和函式就會正常結束。 其結果和上述範例相同，但在這裡，結束會明確地發生，並受到控制。

```cppwinrt
IAsyncAction ExplicitCancellationAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };

    while (!cancellation_token())
    {
        std::cout << "ExplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto explicit_cancellation{ ExplicitCancellationAsync() };
    co_await 3s;
    explicit_cancellation.Cancel();
}
...
```

等候 **winrt::get_cancellation_token** 會擷取取消權杖，此權杖會知道協同程式代替您產生的 **IAsyncAction**。 您可以在該權杖上使用函式呼叫運算子，來查詢取消狀態 (本質上會輪詢取消)。 如果您要執行某些與計算繫結的作業，或逐一查看大型集合，便適合使用此技巧。

### <a name="register-a-cancellation-callback"></a>註冊取消回呼

Windows 執行階段的取消不會自動流向其他非同步物件。 但是，&mdash;在 Windows SDK 的 10.0.17763.0 (Windows 10 版本 1809) 版本中導入&mdash;您可以註冊取消回呼。 這是可用來傳播取消的先佔式勾點，並可讓您與現有的並行處理程式庫整合。

在接下來的這個程式碼範例中，**NestedCoroutineAsync** 會進行此工作，但其中沒有任何特殊的取消邏輯。 **CancellationPropagatorAsync** 基本上是巢狀協同程式上的包裝函式；該包裝函式會事先轉送取消。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction NestedCoroutineAsync()
{
    while (true)
    {
        std::cout << "NestedCoroutineAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction CancellationPropagatorAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };
    auto nested_coroutine{ NestedCoroutineAsync() };

    cancellation_token.callback([=]
    {
        nested_coroutine.Cancel();
    });

    co_await nested_coroutine;
}

IAsyncAction MainCoroutineAsync()
{
    auto cancellation_propagator{ CancellationPropagatorAsync() };
    co_await 3s;
    cancellation_propagator.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

**CancellationPropagatorAsync** 會為其本身的取消回呼註冊 Lambda 函式，然後等候 (暫止) 直到巢狀的工作完成為止。 當 **CancellationPropagatorAsync** 取消時 (或如果取消)，其便會將取消傳播到巢狀的協同程式。 不必輪詢取消；取消也不會無限期地遭到封鎖。 這項機制有足夠的彈性，可讓您與不知道 C++/WinRT 的協同程式或並行處理程式庫互通。

## <a name="reporting-progress"></a>報告進度

如果協同程式傳回 [**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_) 或 [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)，您便可以擷取 [**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token) 函式所傳回的物件，並使用該物件來向進度處理常式回報進度。 以下是程式碼範例。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncOperationWithProgress<double, double> CalcPiTo5DPs()
{
    auto progress{ co_await winrt::get_progress_token() };

    co_await 1s;
    double pi_so_far{ 3.1 };
    progress(0.2);

    co_await 1s;
    pi_so_far += 4.e-2;
    progress(0.4);

    co_await 1s;
    pi_so_far += 1.e-3;
    progress(0.6);

    co_await 1s;
    pi_so_far += 5.e-4;
    progress(0.8);

    co_await 1s;
    pi_so_far += 9.e-5;
    progress(1.0);
    co_return pi_so_far;
}

IAsyncAction DoMath()
{
    auto async_op_with_progress{ CalcPiTo5DPs() };
    async_op_with_progress.Progress([](auto const& /* sender */, double progress)
    {
        std::wcout << L"CalcPiTo5DPs() reports progress: " << progress << std::endl;
    });
    double pi{ co_await async_op_with_progress };
    std::wcout << L"CalcPiTo5DPs() is complete !" << std::endl;
    std::wcout << L"Pi is approx.: " << pi << std::endl;
}

int main()
{
    winrt::init_apartment();
    DoMath().get();
}
```

> [!NOTE]
> 對非同步動作或作業實作多個「完成處理常式」  不是正確的操作。 要為其已完成的事件準備單一委派還是對其進行 `co_await`，您只能擇一來做。 如果兩者都做，第二個便會失敗。 下列兩種完成處理常式的任何一個都可以；但不能讓同一個非同步物件同時使用兩種。

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
async_op_with_progress.Completed([](auto const& sender, AsyncStatus /* status */)
{
    double pi{ sender.GetResults() };
});
```

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
double pi{ co_await async_op_with_progress };
```

如需完成處理常式的詳細資訊，請參閱[非同步動作與作業的委派類型](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)。

## <a name="fire-and-forget"></a>啟動後便不予理會

有時，您的工作可以與其他工作並行完成，您既不需要等待該工作完成 (其他工作皆未與其相依)，也不需要用該工作來傳回值。 在此情況下，您可以啟動該工作，然後就不予理會。 若要這麼做，請撰寫傳回類型是 [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) (而不是 Windows 執行階段非同步作業類型或 **concurrency::task** 其中之一) 的協同程式。

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace std::chrono_literals;

winrt::fire_and_forget CompleteInFiveSeconds()
{
    co_await 5s;
}

int main()
{
    winrt::init_apartment();
    CompleteInFiveSeconds();
    // Do other work here.
}
```

## <a name="important-apis"></a>重要 API
* [concurrency::task 類別](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction 介面](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; 介面](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; 介面](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 介面](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync 方法](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed 類別](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>相關主題
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)
* [標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)
