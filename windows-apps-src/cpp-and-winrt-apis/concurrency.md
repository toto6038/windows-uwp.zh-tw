---
description: 本主題示範的方式，您可以使用 C++/WinRT，同時建立及使用 Windows 執行階段非同步物件。
title: 透過 C++/WinRT 的並行和非同步作業
ms.date: 07/08/2019
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, 並行, async, 非同步的, 非同步
ms.localizationpriority: medium
ms.openlocfilehash: 048d6fe455f7c3e77922ef8b937a9cb1d6cbb21c
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "81266896"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>透過 C++/WinRT 的並行和非同步作業

> [!IMPORTANT]
> 本主題介紹*協同程式* 和 `co_await` 的概念，建議您在 UI *和*非 UI 應用程式中均應加以使用。 為了簡單起見，本簡介主題中大部分的程式碼範例均說明 **Windows 主控台應用程式 (C++/WinRT)** 專案。 本主題中後續的程式碼範例會使用協同程式，但為了方便起見，主控台應用程式範例仍會繼續在結束之前使用封鎖 **get** 函式呼叫，以免應用程式在完成其輸出的列印之前結束。 您不應從 UI 執行緒執行此動作 (呼叫封鎖 **get** 函式)。 您應使用 `co_await` 陳述式。 [更進階的並行和非同步](concurrency-2.md)主題會說明您將在 UI 應用程式中使用的技術。

本簡介主題說明您如何使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 同時建立及使用 Windows 執行階段非同步物件。 閱讀本主題之後 (特別是針對您將在 UI 應用程式中使用的技術)，另請參閱[更進階的並行和非同步](concurrency-2.md)。

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>非同步作業與 Windows 執行階段 "Async" 函式

實作有可能超過 50 毫秒完成的任何 Windows 執行階段 API 做為非同步函式 (名稱以 "Async" 結尾)。 非同步函式的實作在另一個執行續上起始工作，並立即傳回代表非同步作業的物件。 非同步作業完成時，傳回包含工作所產生任何值的物件。 **Windows::Foundation** Windows 執行階段命名空間包含四種非同步作業物件。

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)、
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress-1)、
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation-1)，以及
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)。

這些非同步作業類型的每一個皆投影到 **winrt::Windows::Foundation** C++/WinRT 命名空間中的對應類型。 C++/WinRT 也包含內部等待配接器結構。 您不會直接使用它，但多虧了該結構，您可以撰寫 `co_await` 陳述式，合作等待傳回這些非同步作業類型之一的任何函式結果。 且您可以撰寫自己的協同程式，傳回這些類型。

非同步 Windows 函式的範例是 [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)，其傳回類型 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress-2) 的非同步作業物件。

讓我們看一些使用 C++/WinRT 來呼叫這類 API 的方式 (先看封鎖，再看非封鎖)。 我們將在以下幾個程式碼範例中使用 **Windows 主控台應用程式 (C++/WinRT)** 專案，以便說明基本概念。 [更進階的並行和非同步](concurrency-2.md)會討論更適合 UI 應用程式使用的技術。

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

在上述程式碼範例中您可以看到，我們在結束 **main** 之前繼續使用封鎖 **get** 函式呼叫。 但這只是為了讓應用程式不會在完成其輸出的列印之前即結束。

## <a name="asynchronously-return-a-windows-runtime-type"></a>非同步傳回 Windows 執行階段類型

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

如果您正非同步傳回 Windows 執行階段類型，則您應該要傳回 [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation-1) 或 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)。 任何第一或第三方執行階段類別資格，或任何可從 Windows Runtime 函式或傳送至 Windows Runtime 函式的任何類型 (例如，`int`，或 **winrt::hstring**)。 如果您嘗試將其中一個非同步作業類型與非 Windows 執行階段類型一起使用，編譯器會協助您解決*必須為 WinRT 類型*的錯誤。

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

## <a name="asynchronously-return-a-non-windows-runtime-type"></a>非同步傳回非 Windows 執行階段類型

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

    co_await DoOtherWorkAsync(); // (this is the first suspension point)...

    // ...accessing value here carries no guarantees of safety.
}
```

在協同程式中，執行是同步的，直到在第一個暫停點上將控制項傳回呼叫端且呼叫框架超出範圍為止。 協同程式恢復時，參考參數參考的來源值可能發生任何事情。 從協同程式的觀點，參考參數有不受控制的存留期。 因此，上述範例中，我們安全存取 *值* 直到 `co_await`，但不是在其之後。 如果呼叫端將「值」  解構，應在導致記憶體損毀之後，嘗試在協同程式內部存取該值。 我們也不能安全的將 *值* 傳遞至 **DoOtherWorkAsync** 如果存在任何風險，該函式將會暫停，然後在其恢復後嘗試使用 *值*。

若要讓參數在暫停與恢復之後安全使用，您的協同程式必須使用預設的透過值傳遞，以確保他們透過值擷取並避免存留期問題。 如果您確定可以安全執行此操作，那麼您幾乎不會偏離該指導方針。

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value); // not const&
```

若要以值的形式傳遞，引數必須以不耗費資源的方式移動或複製，而這通常是智慧指標的情況。

透過 const 值來傳遞也是一個很好的做法 (除非您想要移動該值)。 它不會對您製作複本的來源值有任何影響，但它意圖明確，且有助於您無意中修改複本。

```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

也請參閱[標準陣列和向量](std-cpp-data-types.md#standard-arrays-and-vectors)，處理如何將標準向量傳遞至非同步被呼叫者。

如果您不能變更協同程式的簽章，但您可以變更實作，則您可以在第一個 `co_await` 之前建立本機複本。

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_value = value;
    // It's ok to access both safe_value and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_value here (not value).
}
```

如果 `Param` 需要耗費很多資源才能複製，則只擷取您在第一個 `co_await` 之前需要的部分。

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_data = value.data;
    // It's ok to access safe_data, value.data, and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_data here (not value.data, nor value).
}
```

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>安全地存取類別成員協同程式中的 this  指標

請參閱 [C++/WinRT 中的強式和弱式參考](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine)。

## <a name="important-apis"></a>重要 API
* [concurrency::task 類別](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction 介面](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; 介面](/uwp/api/windows.foundation.iasyncactionwithprogress-1)
* [IAsyncOperation&lt;TResult&gt; 介面](/uwp/api/windows.foundation.iasyncoperation-1)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 介面](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)
* [SyndicationClient::RetrieveFeedAsync 方法](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed 類別](/uwp/api/windows.web.syndication.syndicationfeed)

## <a name="related-topics"></a>相關主題
* [更進階的並行和非同步](concurrency-2.md)
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)
* [標準 C++ 資料類型與 C++/WinRT](std-cpp-data-types.md)