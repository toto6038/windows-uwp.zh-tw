---
author: stevewhims
description: 本主題中顯示的方式，您可以使用 C++/WinRT，同時建立及使用 Windows 執行階段非同步物件
title: 透過 C++/WinRT 的並行和非同步作業。
ms.author: stwhi
ms.date: 04/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、標準、c++、cpp、winrt、投影、並行、async、非同步的、非同步
ms.localizationpriority: medium
ms.openlocfilehash: 3af9125abc3abf41327f5b49e6a05d81e214f89f
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
ms.locfileid: "1831832"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>透過 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 的並行和非同步作業。
本主題中顯示的方式，您可以使用 C++/WinRT，同時建立及使用 Windows 執行階段非同步物件

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>非同步作業與 Windows 執行階段 "Async" 函式
實作有可能超過 50 毫秒完成的任何 Windows 執行階段 API 做為非同步函式 (名稱以 "Async" 結尾)。 非同步函式的實作在另一個執行續上起始工作，並立即傳回代表非同步作業的物件。 非同步作業完成時，傳回包含工作所產生任何值的物件。 **Windows::Foundation** Windows 執行階段命名空間包含四種非同步作業物件，它們分別為

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)、
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)、
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_)，以及
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)。

這些非同步作業類型的每一個皆投影到 **winrt::Windows::Foundation** C++/WinRT 命名空間中的對應類型。 C++/WinRT 也包含內部等待介面卡結構。 您不使用直接它，但多虧了有該結構，您可以撰寫 **co_await** 陳述式，合作等待傳回這些非同步作業類型之一的任一函式結果。 且您可以撰寫自己的協同程式，傳回這些類型。

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

呼叫 **get** 讓程式設計便利，但不是您要搭配呼叫的項目。 非並行也沒有非同步。 若要避免 OS 執行緒阻塞而無法執行其它有用的工作，我們需要不同的技術。

## <a name="write-a-coroutine"></a>撰寫協同程式
C++/WinRT 與 C++ 協同程式整合為程式設計模型，提供以自然的方式合作等待結果。 您可以藉由撰寫協同程式產生自己的 Windows 執行階段非同步作業。 下列程式碼範例中，**ProcessFeedAsync** 是協同程式。

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed syndicationFeed)
{
    for (SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = co_await syndicationClient.RetrieveFeedAsync(rssFeedUri);
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

協同程式是一個函式，可暫停和繼續。 在上述的 **ProcessFeedAsync** 協同程式中，到達 **co_await** 陳述式時，協同程式便非同步起始 **RetrieveFeedAsync** 呼叫並立即自行暫停和將控制項傳回給呼叫者 (其為上述範例中的 **main**)。 **main** 可以繼續執行工作，同時擷取和列印摘要。 完成時 (當 **RetrieveFeedAsync** 呼叫完成時)，**ProcessFeedAsync** 協同程式在下一個陳述式繼續。

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

void PrintFeed(SyndicationFeed syndicationFeed)
{
    for (SyndicationItem syndicationItem : syndicationFeed.Items())
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

如果您正非同步傳回 Windows 執行階段類型 (不論是第一方或第三方類型)，則您應該要傳回 [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) 或 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)。

編譯器將會協助您解決 *必須為 WinRT 類型* 的錯誤，如果您嘗試使用非 Windows 執行階段類型的這些非同步作業類型之一的話。

## <a name="asychronously-return-a-non-windows-runtime-type"></a>非同步傳回非 Windows 執行階段類型
如果您正非同步傳回類型，其 *不是* Windows 執行階段類型，則您應該要傳回平行模式程式庫 (PPL) [**task**](https://msdn.microsoft.com/library/hh750113)。 我們建議 **task**，因為它提供您比 **std::future** 更好的效能 (和往後較佳的相容性)。

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
    // do other work.
    std::wcout << firstTitleOp.get() << std::endl;
}
```

## <a name="important-apis"></a>重要 API
* [concurrency::task](https://msdn.microsoft.com/library/hh750113)
* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)
