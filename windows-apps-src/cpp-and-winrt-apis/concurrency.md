---
author: stevewhims
description: 本主題中顯示的方式，您可以使用 C++/WinRT，同時建立及使用 Windows 執行階段非同步物件
title: 透過 C++/WinRT 的並行和非同步作業。
ms.author: stwhi
ms.date: 10/27/2018
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、並行、async、非同步的、非同步
ms.localizationpriority: medium
ms.openlocfilehash: d943a43629860f666c9ec9eb7f0b3bb406b1b1b5
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6040365"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>透過 C++/WinRT 的並行和非同步作業

本主題示範您可以兩者的方式建立和取用與 Windows 執行階段非同步物件[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>非同步作業與 Windows 執行階段 "Async" 函式

實作有可能超過 50 毫秒完成的任何 Windows 執行階段 API 做為非同步函式 (名稱以 "Async" 結尾)。 非同步函式的實作在另一個執行續上起始工作，並立即傳回代表非同步作業的物件。 非同步作業完成時，傳回包含工作所產生任何值的物件。 **Windows::Foundation** Windows 執行階段命名空間包含四種非同步作業物件。

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)、
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)、
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_)，以及
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)。

這些非同步作業類型的每一個皆投影到 **winrt::Windows::Foundation** C++/WinRT 命名空間中的對應類型。 C++/WinRT 也包含內部等待介面卡結構。 直接但歸功於該結構，您不使用它，您可以撰寫`co_await`陳述式，合作等待傳回這些非同步作業類型之一的任一函式的結果。 且您可以撰寫自己的協同程式，傳回這些類型。

非同步 Windows 函式的範例是 [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)，其傳回類型 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) 的非同步作業物件。 讓我們看一下一些方式&mdash;第一次封鎖，並再非封鎖&mdash;使用 C + + WinRT 来呼叫的 API。

## <a name="block-the-calling-thread"></a>封鎖呼叫執行緒

下列的程式碼範例從 **RetrieveFeedAsync** 接收一個非同步作業物件，並在該物件上呼叫 **get**，封鎖呼叫執行緒，直到非同步作業的結果可用。

```cppwinrt
// main.cpp

#include "pch.h"
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

呼叫 **取得** 方便程式設計，它適用於您基於任何原因不想使用協同程式的主控台應用程式或背景執行緒。 但其非並行也不是非同步，因此不適合 UI 執行緒（且如果您嘗試在其中一個使用，會在非最佳化的組建中觸發聲明）。 若要避免 OS 執行緒阻塞而無法執行其它有用的工作，我們需要不同的技術。

## <a name="write-a-coroutine"></a>撰寫協同程式

C++/WinRT 與 C++ 協同程式整合為程式設計模型，提供以自然的方式合作等待結果。 您可以藉由撰寫協同程式產生自己的 Windows 執行階段非同步作業。 下列程式碼範例中，**ProcessFeedAsync** 是協同程式。

> [!NOTE]
> **取得**函式存在於 C + + /winrt 投影類型**winrt::Windows::Foundation::IAsyncAction**，，因此您可以呼叫的函式內任何 C + + /winrt 專案。 因為**取得**不是實際的 Windows 執行階段類型**IAsyncAction**的應用程式二進位介面 (ABI) 表面的一部分，您不會找到列為[**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)介面的成員函式。

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

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

您可以將一個協同程式彙總到其它協同程式。 或您可以呼叫 **get** 封鎖並等待完成 (取得結果，如果有的話)。 或者，您可以將它傳遞到另一個程式設計語言，其支援 Windows 執行階段。

也可以藉由使用委派，處理非同步動作與作業完成的和/或進行中事件。 如需詳細資訊和程式碼範例，請查看[非同步動作與作業的委派類型](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)。

## <a name="asychronously-return-a-windows-runtime-type"></a>非同步傳回 Windows 執行階段類型

在下一個範例中，我們將呼叫包裝在 **RetrieveFeedAsync**，給特定的 URI，來提供我們 **RetrieveBlogFeedAsync** 函式，其非同步傳回一個 [**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed)。

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

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

如果您正非同步傳回 Windows 執行階段類型，則您應該要傳回 [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) 或 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)。 任何第一或第三方執行階段類別資格，或任何可從 Windows Runtime 函式或傳送至 Windows Runtime 函式的任何類型 (例如，`int`，或 **winrt::hstring**)。 編譯器將會協助您解決 *必須為 WinRT 類型* 的錯誤，如果您嘗試使用非 Windows 執行階段類型的這些非同步作業類型之一的話。

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

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
#include <ppltasks.h>

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

協同程式很像任何其他函式，因為呼叫者會遭到封鎖，直到函式將執行傳回給它。 和協同程式，傳回的第一個機會是第一個`co_await`， `co_return`，或`co_yield`。

因此，您才開始計算繫結工作在協同程式中，您需要將執行傳回給呼叫者 （亦即，導致暫停點），以便呼叫者便未遭到封鎖。 如果您正在您尚未透過`co_await`-來執行此其他作業，則您可以`co_await` [**winrt:: resume_background**](/uwp/cpp-ref-for-winrt/resume-background)函式。 其將控制項傳回給呼叫者，並立即恢復執行緒集區執行緒的執行。

實作中使用的執行緒集區是低層級的 [Windows 執行緒集區](https://msdn.microsoft.com/library/windows/desktop/ms686766)，所以是最有效的。

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
IAsyncAction DoWorkAsync(TextBlock const& textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

上方的程式碼擲回一個 [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/hresult-wrong-thread) 例外，因為 **TextBlock** 必須從建立它的執行緒進行更新，也就是 UI 執行緒。 一種解決方案就是擷取我們最初呼叫的協同程式的執行緒內容。 若要這樣做， [**winrt:: apartment_context**](/uwp/cpp-ref-for-winrt/apartment-context)物件具現化、 執行背景工作，然後`co_await` **apartment_context**切換回呼叫內容。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock const& textblock)
{
    winrt::apartment_context ui_thread; // Capture calling context.

    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await ui_thread; // Switch back to calling context.

    textblock.Text(L"Done!"); // Ok if we really were called from the UI thread.
}
```

只要從建立 **TextBlock** 的 UI 執行緒呼叫上述的協同程式，然後這項技術便可運作。 在您確定的應用程式中將有許多案例。

如需更多一般解決方案來更新 UI，其中包含在您不確定有關呼叫執行緒的情況下，您可以`co_await` [**winrt:: resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)函式，以切換至特定前景執行緒。 在下列程式碼範例中，我們透過傳遞與 **TextBlock** (透過存取其[**發送器**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher) 屬性) 相關聯的發送器物件，來指定前景執行緒。 **winrt::resume_foreground** 的實作在發送器物件上呼叫 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)，來執行協同程式中之後的工作。

```cppwinrt
#include <winrt/Windows.UI.Core.h> // necessary in order to use winrt::resume_foreground.
IAsyncAction DoWorkAsync(TextBlock const& textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>執行內容，繼續執行，並在協同程式中切換

廣泛地說，在協同程式在暫停點之後, 在原始的執行緒可能會消失，並且回復也可能發生任何執行緒上 （亦即，任何執行緒可能會將**Completed**方法呼叫非同步作業）。

但如果您`co_await`任何四個 Windows 執行階段非同步作業類型 (**IAsyncXxx**)，然後 C + + /winrt 會擷取呼叫內容的某個點您`co_await`。 它可確保您是仍在該內容，則接續繼續執行時。 C + + /winrt 的運作方式檢查是否您已經在呼叫內容，而如果不是，改用它。 如果您已在前一個單一執行緒 apartment (STA) 執行緒上`co_await`，然後，您就會在同一個之後;如果您已在多執行緒的 apartment (MTA) 執行緒之前`co_await`，則您將會在同一個之後。

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

您可以依賴此行為的原因是因為 C + + /winrt 提供程式碼來調整 （這些程式碼片段稱為等待介面卡） 的 c + + 協同程式語言支援這些 Windows 執行階段非同步作業類型。 其餘的建立可等候型別在 C + + WinRT，執行緒集區包裝函式和/或協助程式; 只是因此，它們完成執行緒集區上。

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

如果您`co_await`一些其他類型&mdash;即使在 C + + /winrt 協同程式實作&mdash;則另一個文件庫提供介面卡，且您將需要了解這些介面卡執行思考回復與內容。

若要保留，最小內容切換，您可以使用的一些我們已經看過本主題中的技術。 我們來看看一些執行此作業的圖例。 在下一個虛擬程式碼範例，我們會示範事件處理常式，呼叫 Windows 執行階段 API 載入影像、 collect 到背景執行緒來處理該映像，然後傳回至 UI 執行緒來在 UI 中顯示影像的外框。

```cppwinrt
#include <winrt/Windows.UI.Core.h> // necessary in order to use winrt::resume_foreground.
IAsyncAction MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
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

在這個案例中，沒有一點 ineffiency 周圍**StorageFile::OpenAsync**呼叫。 沒有必要內容切換為背景執行緒 （因此，在處理常式可以將執行傳回給呼叫者） 上回復之後的 C +, + /winrt 會還原的 UI 執行緒內容。 但是，在此情況下，沒有必要將會在 UI 執行緒上，直到我們即將更新 UI。 我們會呼叫*之前*我們呼叫**winrt:: resume_background**，我們會帶來更不必要背面往來內容切換多個 Windows 執行階段 Api。 解決方案不是*任何*Windows 執行階段 Api 之前，先呼叫然後。 **Winrt:: resume_background**之後的所有移動。

```cppwinrt
#include <winrt/Windows.UI.Core.h> // necessary in order to use winrt::resume_foreground.
IAsyncAction MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
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

如果您想要執行某些更進階的則您可以撰寫您自己，await 介面卡。 例如，如果您想要`co_await`非同步動作完成的相同執行緒上繼續 （因此，沒有任何內容切換），則您可以藉由撰寫開始 await 介面卡類似如下所示。

> [!NOTE]
> 下列程式碼範例是教育僅用以提供;它是可協助您開始了解如何 await 介面卡的工作。 如果您想要使用這項技術，在您自己的程式碼基底，我們建議您開發和測試您自己 await struct(s) 介面卡。 例如，您可以撰寫**complete_on_any**、 **complete_on_current**，以及**complete_on(dispatcher)**。 也請考慮讓它們變成採取**IAsyncXxx**類型做為範本參數的範本。

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

若要了解如何使用**no_switch** await 介面卡，首先您必須知道當 c + + 編譯器遇到`co_await`運算式，它會尋找函式呼叫**await_ready**、 **await_suspend**，以及**await_resume**。 C + + /winrt 文件庫提供這些函式，以便您取得合理的行為，根據預設，就像這樣。

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

若要使用**no_switch** await 介面卡，只是變更該類型`co_await` **IAsyncXxx** **no_switch**，就像這樣的運算式。

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

然後，而不是尋找符合**IAsyncXxx**的三個**await_xxx**函式，c + + 編譯器會尋找符合**no_switch**的函式。

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>取消非同步作業，並取消回呼

在 Windows 執行階段功能進行非同步程式設計可讓您取消 「 執行中 」 的非同步動作或作業。 以下是範例會呼叫[**StorageFolder::GetFilesAsync**](/uwp/api/windows.storage.storagefolder.getfilesasync)来擷取的檔案，可能很大集合，它會產生非同步作業物件儲存在資料成員。 使用者也可選擇取消作業。

```cppwinrt
// MainPage.xaml
...
<Button x:Name="workButton" Click="OnWork">Work</Button>
<Button x:Name="cancelButton" Click="OnCancel">Cancel</Button>
...

// MainPage.h
...
#include <winrt/Windows.Storage.Search.h>
...
struct MainPage : MainPageT<MainPage>
{
    MainPage()
    {
        InitializeComponent();
    }

    IAsyncAction OnWork(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
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
```

取消的實作端，讓我們開始使用簡單的範例。

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
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

如果您執行上述範例，則您會看到**ImplicitCancellationAsync**列印一則訊息每秒後的三秒的時間就會自動終止帶來被取消。 因為在遇到`co_await`運算式，協同程式會檢查是否已取消。 如果有，則它 short-circuits 出;如果未指定，則它就會暫停為一般。

協同程式暫停時，取消可以當然，會發生。 只有當協同程式恢復時，碰到另一部或`co_await`，它會檢查的取消。 問題是其中一個可能太--粗糙的延遲時間以回應取消作業。

因此，另一個選項是明確地輪詢取消從您的協同程式中。 更新，上述範例使用下列清單中的程式碼。 在這個新的範例中， **ExplicitCancellationAsync**擷取[**winrt::get_cancellation_token**](/uwp/cpp-ref-for-winrt/get-cancellation-token)函式所傳回的物件，並使用它來定期檢查是否已取消協同程式。 協同程式，只要不取消，迴圈無限期;一旦取消，迴圈，並將函式正常的結束。 因為先前範例中，但此處離開會發生明確，並控制下，結果會相同。

```cppwinrt
...
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

等候**winrt::get_cancellation_token**擷取代表您的協同程式便會產生**IAsyncAction**知識與取消語彙基元。 您可以在該權杖使用函式呼叫運算子來查詢取消狀態&mdash;基本上輪詢取消。 如果您正在執行某些計算繫結的作業，或逐一查看大型集合，這是合理的技術。

### <a name="register-a-cancellation-callback"></a>登錄取消回呼

在 Windows 執行階段取消不會自動流向其他非同步物件。 但是&mdash;版本 10.0.17763.0 (Windows 10 版本 1809年) 的 Windows sdk 中導入&mdash;您可以登錄取消回呼。 這是預先勾點取消可以被傳播，並讓您使用現有的並行處理程式庫整合。

在下一個程式碼範例， **NestedCoroutineAsync**負責運作，但它有任何特殊的取消邏輯中它。 **CancellationPropagatorAsync**是基本上是巢狀的協同程式; 上的包裝函式包裝函式會聯合國轉送取消。

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
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

    cancellation_token.callback([&]
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

**CancellationPropagatorAsync**登錄它自己的取消回呼，lambda 函式，並在它的等待然後 （它會暫停） 除非巢狀的工作完成。 當或取消**CancellationPropagatorAsync** ，它傳播到巢狀的協同程式取消。 不需要為輪詢取消;也不會取消封鎖無限期。 這種機制是有足夠的彈性，您可以使用它來與其互通的協同程式或並行處理的媒體櫃知道執行任何動作的 C + + /winrt。

## <a name="reporting-progress"></a>報告進度

如果您的協同程式傳回[**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)或[**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)，然後您可以擷取[**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token)函式所傳回的物件並使用它來回到進度報告進度處理常式。 以下是程式碼範例。

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
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
> 不正確實作一個以上的非同步動作或作業*完成處理常式*。 您可以讓任一單一委派的已完成的事件，或者您可以`co_await`它。 如果您有兩者，則第二個將會失敗。 任一種下列兩種完成處理常式的其中一個是適當;不同時針對相同的非同步物件。

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

如需完成處理常式的詳細資訊，請參閱[適用於非同步動作和作業的委派類型](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)。

## <a name="fire-and-forget"></a>引發及忘記

有時候，您有一個和其他工作，同時可以完成的工作，而且您不需要等待完成該工作 （任何其他工作相依性，） 也不需要它傳回一個值。 在此情況下，您可以關閉工作觸發，且忘記它。 您可以藉由撰寫協同程式，其傳回類型是[**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) （而不是其中一個 Windows 執行階段非同步作業類型，或是**concurrency:: task**）。

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
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
* [concurrency:: task 類別](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction 介面](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;介面](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;介面](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult，TProgress&gt;介面](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [Syndicationclient:: Retrievefeedasync 方法](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed 類別](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>相關主題
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)
* [標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)
