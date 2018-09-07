---
author: stevewhims
description: 本主題中顯示的方式，您可以使用 C++/WinRT，同時建立及使用 Windows 執行階段非同步物件
title: 透過 C++/WinRT 的並行和非同步作業。
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、並行、async、非同步的、非同步
ms.localizationpriority: medium
ms.openlocfilehash: fe43eaa233d3384eecb5e8755190efc1a109bbb9
ms.sourcegitcommit: 53ba430930ecec8ea10c95b390fe6e654fe363e1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2018
ms.locfileid: "3421830"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>透過 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 的並行和非同步作業。
> [!NOTE]
> **正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。**

本主題示範的方式，您可以使用 C++/WinRT，同時建立及使用 Windows 執行階段非同步物件。

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>非同步作業與 Windows 執行階段 "Async" 函式
實作有可能超過 50 毫秒完成的任何 Windows 執行階段 API 做為非同步函式 (名稱以 "Async" 結尾)。 非同步函式的實作在另一個執行續上起始工作，並立即傳回代表非同步作業的物件。 非同步作業完成時，傳回包含工作所產生任何值的物件。 **Windows::Foundation** Windows 執行階段命名空間包含四種非同步作業物件。

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)、
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)、
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_)，以及
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)。

這些非同步作業類型的每一個皆投影到 **winrt::Windows::Foundation** C++/WinRT 命名空間中的對應類型。 C++/WinRT 也包含內部等待介面卡結構。 您不直接使用它，但歸功於該結構，您可以撰寫 ``co_await` 陳述式，合作等待傳回這些非同步作業類型之一的任一函式結果。 且您可以撰寫自己的協同程式，傳回這些類型。

非同步 Windows 函式的範例是 [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)，其傳回類型 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) 的非同步作業物件。 讓我們看一下一些方式&mdash;封鎖與非封鎖&mdash;使用 C++/WinRT API，呼叫這類的 API。

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
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
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
> **取得**函式存在於 C + + /winrt 投影類型**winrt::Windows::Foundation::IAsyncAction**，，因此您可以呼叫的函式內任何 C + + /winrt 專案。 您將找不到列為[**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)介面中，成員函式因為**取得**不是實際的 Windows 執行階段類型**IAsyncAction**的應用程式二進位介面 (ABI) 表面的一部分。

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

    auto processOp = ProcessFeedAsync();
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

    auto feedOp = RetrieveBlogFeedAsync();
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
        SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
        return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
    });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp = RetrieveFirstTitleAsync();
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
您在協同程式中進行計算繫結工作之前，您需要將執行傳回給呼叫者，這樣呼叫者便未遭到封鎖（亦即，導致暫停點）。 如果您尚未透過 `co-await` 一些其他作業來執行此作業，則您可以 `co-await` **winrt::resume_background** 函式。 其將控制項傳回給呼叫者，並立即恢復執行緒集區執行緒的執行。

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
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

上方的程式碼擲回一個 [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/hresult-wrong-thread) 例外，因為 **TextBlock** 必須從建立它的執行緒進行更新，也就是 UI 執行緒。 一種解決方案就是擷取我們最初呼叫的協同程式的執行緒內容。 將 **winrt::apartment_context** 物件具現化，然後 `co_await` 它。

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

> [!NOTE]
> **以下程式碼範例與正式發行前可能會進行大幅度修改之預先發行的產品相關。  Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。**

如需更多一般解決方案來更新 UI，其中包含您不確定有關呼叫執行緒的案例，您可以安裝 [Windows 10 SDK 預覽組建 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)，或更新版本。 然後，您就可以 `co-await` **winrt::resume_foreground** 函式，以切換至特定前景執行緒。 在下列程式碼範例中，我們透過傳遞與 **TextBlock** (透過存取其[**發送器**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher) 屬性) 相關聯的發送器物件，來指定前景執行緒。 **winrt::resume_foreground** 的實作在發送器物件上呼叫 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)，來執行協同程式中之後的工作。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="important-apis"></a>重要 API
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)

## <a name="related-topics"></a>相關主題
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)
* [標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)