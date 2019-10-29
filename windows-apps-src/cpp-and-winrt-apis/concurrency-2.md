---
description: C++/WinRT 中具有並行和非同步的更進階案例。
title: 採用 C++/WinRT 的更進階並行和非同步
ms.date: 07/23/2019
ms.topic: article
keywords: Windows 10, uwp, 標準, c++, cpp, winrt, 投影, 並行, async, 非同步的, 非同步
ms.localizationpriority: medium
ms.openlocfilehash: 4a671a319be49e07d3a8fcdacb569c4ae76e299b
ms.sourcegitcommit: 6fbf645466278c1f014c71f476408fd26c620e01
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2019
ms.locfileid: "72816668"
---
# <a name="more-advanced-concurrency-and-asynchrony-with-cwinrt"></a>採用 C++/WinRT 的更進階並行和非同步

本主題說明 C++/WinRT 中具有並行和非同步的更進階案例。

如需本主題的簡介請先閱讀[並行和非同步作業](concurrency.md)。

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
using namespace std::chrono_literals;
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

## <a name="a-deeper-dive-into-winrtresume_foreground"></a>深入探討 **winrt::resume_foreground**

從 [C++/WinRT 2.0](/windows/uwp/cpp-and-winrt-apis/newsnews#news-and-changes-in-cwinrt-20) 起，[**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 函式會暫停，即使從發送器執行緒呼叫亦然 (在先前的版本中，它可能在某些案例中引進鎖死，因為它只會在尚未位於發送器執行緒時暫停)。

目前的行為表示您可以依賴堆疊回溯並進行重新佇列處理；這對於系統穩定性非常重要 (尤其在低階系統程式碼中)。 前面[考量使用執行緒親和性程式設計](#programming-with-thread-affinity-in-mind)一節中所列的最後一段程式碼，說明如何在背景執行緒上執行一些複雜計算，然後切換到適當的 UI 執行緒，以便更新使用者介面 (UI)。

以下是 **winrt::resume_foreground** 在內部的外觀。

```cppwinrt
auto resume_foreground(...) noexcept
{
    struct awaitable
    {
        bool await_ready() const
        {
            return false; // Queue without waiting.
            // return m_dispatcher.HasThreadAccess(); // The C++/WinRT 1.0 implementation.
        }
        void await_resume() const {}
        void await_suspend(coroutine_handle<> handle) const { ... }
    };
    return awaitable{ ... };
};
```

此種目前 (相對於先前) 行為類似於 Win32 應用程式開發中 [**PostMessage**](/windows/win32/api/winuser/nf-winuser-postmessagew) 與 [**SendMessage**](/windows/win32/api/winuser/nf-winuser-sendmessage) 之間的差異。 **PostMessage** 會將工作排入佇列，然後回溯堆疊，而不需等候工作完成。 堆疊回溯不可或缺。

**winrt::resume_foreground** 函式一開始也只支援 [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) (繫結至a [**CoreWindow**](/uwp/api/windows.ui.core.corewindow))，這是 Windows 10 以前引進的。 我們引進了更具彈性且更有效的發送器：[**DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue)。 您可以基於自己的目的建立 **DispatcherQueue**。 請考慮此簡單主控台應用程式。

```cppwinrt
using namespace Windows::System;

winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    auto controller{ DispatcherQueueController::CreateOnDedicatedThread() };
    RunAsync(controller.DispatcherQueue());
    getchar();
}
```

上述範例會在私人執行緒上建立佇列 (包含在控制器內)，然後將控制器傳遞至協同程式。 協同程式可以使用佇列在私人執行緒上等候 (暫停和繼續)。 **DispatcherQueue** 的另一個常見用法是在目前的 UI 執行緒上，針對傳統型或 Win32 應用程式建立佇列。

```cppwinrt
DispatcherQueueController CreateDispatcherQueueController()
{
    DispatcherQueueOptions options
    {
        sizeof(DispatcherQueueOptions),
        DQTYPE_THREAD_CURRENT,
        DQTAT_COM_STA
    };
 
    ABI::Windows::System::IDispatcherQueueController* ptr{};
    winrt::check_hresult(CreateDispatcherQueueController(options, &ptr));
    return { ptr, take_ownership_from_abi };
}
```

這讓您了解如何呼叫 Win32 函式並將其併入 C++/WinRT 專案：只要呼叫 Win32 樣式的 [**CreateDispatcherQueueController**](/windows/win32/api/dispatcherqueue/nf-dispatcherqueue-createdispatcherqueuecontroller) 函式來建立控制器，然後將所產生佇列控制器的擁有權當作 WinRT 物件轉移給呼叫端即可。 這也明確地說明如何在現有的 Petzold 樣式 Win32 傳統型應用程式上支援有效且順暢的佇列處理。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    Window window;
    auto controller{ CreateDispatcherQueueController() };
    RunAsync(controller.DispatcherQueue());
    MSG message;
 
    while (GetMessage(&message, nullptr, 0, 0))
    {
        DispatchMessage(&message);
    }
}
```

在上面，簡單的 **main** 函式會從建立視窗開始。 您可以想像，這會註冊視窗類別，然後呼叫 [**CreateWindow**](/windows/win32/api/winuser/nf-winuser-createwindoww) 來建立最上層的傳統型視窗。 接著會呼叫 **CreateDispatcherQueueController** 函式來建立佇列控制器，然後使用此控制器所擁有的發送器佇列來呼叫某個協同程式。 接著會進入傳統訊息幫浦，其中協同程式繼續會自然發生於這個執行緒上。 這麼做之後，您就可以讓您應用程式內的非同步或訊息型工作流程，返回協同程式的優雅世界。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ... // Begin on the calling thread...
 
    co_await winrt::resume_foreground(queue);
 
    ... // ...resume on the dispatcher thread.
}
```

對 **winrt::resume_foreground** 的呼叫一律會「佇列處理」  ，然後回溯堆疊。 您也可以選擇性地設定繼續優先順序。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await winrt::resume_foreground(queue, DispatcherQueuePriority::High);
 
    ...
}
```

或者，使用預設佇列順序。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await queue;
 
    ...
}
```

或者，在此情況下偵測佇列關閉，並正常地處理。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    if (co_await queue)
    {
        ... // Resume on dispatcher thread.
    }
    else
    {
        ... // Still on calling thread.
    }
}
```

`co_await` 運算式會傳回 `true`，表示將在發送器執行緒上繼續進行。 換句話說，該佇列已成功。 相反地，它傳回 `false`，表示執行作業仍在呼叫執行緒上，因為佇列的控制器正在關閉且不再為佇列要求提供服務。

因此，若結合 C++/WinRT 與協同程式，您就有許多隨時可供使用的功能；特別是在進行一些舊式 Petzold 樣式傳統型應用程式開發時。

## <a name="canceling-an-asynchronous-operation-and-cancellation-callbacks"></a>取消非同步作業，以及取消回呼

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

當您要在事件處理常式中執行非同步作業時，**winrt::fire_and_forget** 也適合用來作為事件處理常式的傳回類型。 以下是一個範例 (也請參閱 [C++/WinRT 中的強式和弱式參考](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine))。

```cppwinrt
winrt::fire_and_forget MyClass::MyMediaBinder_OnBinding(MediaBinder const&, MediaBindingEventArgs args)
{
    auto lifetime{ get_strong() }; // Prevent *this* from prematurely being destructed.
    auto ensure_completion{ unique_deferral(args.GetDeferral()) }; // Take a deferral, and ensure that we complete it.

    auto file{ co_await StorageFile::GetFileFromApplicationUriAsync(Uri(L"ms-appx:///video_file.mp4")) };
    args.SetStorageFile(file);

    // The destructor of unique_deferral completes the deferral here.
}
```

第一個引數 (sender  ) 保留未命名，因為我們永遠不會用到。 因此，我們可以安全地將其保留以作為參考。 但會看到 args  以值的形式傳遞。 請參閱上述的[參數傳遞](concurrency.md#parameter-passing)一節。

## <a name="awaiting-a-kernel-handle"></a>等候核心控制碼

C++/WinRT 提供 **resume_on_signal** 類別，您可以將其用於暫止，直到核心事件收到信號為止。 您需負責確保控制碼在 `co_await resume_on_signal(h)` 傳回前保持有效。 **resume_on_signal** 本身無法為您執行這項操作，因為即使在 **resume_on_signal** 開始前，您都可能遺失控制碼，如第一個範例所示。

```cppwinrt
IAsyncAction Async(HANDLE event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle is not valid here.
}
```

傳入的 **HANDLE** 只有在函式傳回前有效，且此函式 (也就是協同程式) 會在第一個暫停點傳回 (此案例中的第一個 `co_await`)。 在等候 **DoWorkAsync** 時，控制權已交回給呼叫端、呼叫框架已超出範圍，且您不再得知當協同程式繼續時，控制碼是否有效。

就技術上而言，我們的協同程式會以值的形式接收其參數 (請參閱上述的[參數傳遞](concurrency.md#parameter-passing))。 但在此情況，我們需要更進一步，才能遵守該指引的「精神」  (而不只是字面意義)。 我們需要傳遞強式參考 (也就是擁有權) 和控制碼。 方法如下。

```cppwinrt
IAsyncAction Async(winrt::handle event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle *is* not valid here.
}
```

以值的方式傳遞 [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) 可提供擁有權語意，以確保核心控制碼在協同程式的存留期內保持有效。

以下是您可呼叫該協同程式的方式。

```cppwinrt
namespace
{
    winrt::handle duplicate(winrt::handle const& other, DWORD access)
    {
        winrt::handle result;
        if (other)
        {
            winrt::check_bool(::DuplicateHandle(::GetCurrentProcess(),
                other.get(), ::GetCurrentProcess(), result.put(), access, FALSE, 0));
        }
        return result;
    }

    winrt::handle make_manual_reset_event(bool initialState = false)
    {
        winrt::handle event{ ::CreateEvent(nullptr, true, initialState, nullptr) };
        winrt::check_bool(static_cast<bool>(event));
        return event;
    }
}

IAsyncAction SampleCaller()
{
    handle event{ make_manual_reset_event() };
    auto async{ Async(duplicate(event)) };

    ::SetEvent(event.get());
    event.close(); // Our handle is closed, but Async still has a valid handle.

    co_await async; // Will wake up when *event* is signaled.
}
```

## <a name="asynchronous-timeouts-made-easy"></a>非同步逾時變簡單

C++/WinRT 已大量投入 C++ 協同程式中。 其對於撰寫平行程式碼的效果已徹底改觀。 本節討論非同步詳細資料並不重要的情況，而您只想要立即取得結果。 基於這個理由，C++/WinRT 的 [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) Windows 執行階段非同步作業介面具有 **get** 函式，其類似於 **std::function** 所提供的函式。

```cppwinrt
using namespace winrt::Windows::Foundation;
int main()
{
    IAsyncAction async = ...
    async.get();
    puts("Done!");
}
```

**get** 函式會無限期地封鎖，而非同步物件會完成。 非同步物件的存留時間可能很短，因此您通常只需要這麼做。

但在某些情況下，這並不足夠，而您必須在一段時間之後放棄等候。 幸虧有 Windows 執行階段所提供的建構元素，因此一律可撰寫該程式碼。 但現在 C++/WinRT 提供 **wait_for** 函式，可讓一切更加輕鬆。 它也會在 **IAsyncAction** 上實作，其同樣類似於 **std::function** 所提供的函式。

```cppwinrt
using namespace std::chrono_literals;
int main()
{
    IAsyncAction async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        puts("done");
    }
}
```

> [!NOTE]
> **wait_for** 在介面上使用 **std::chrono::duration**，但其限制為小於 **std::chrono::duration** 提供的範圍 (大約49.7 天)。

下一個範例中的 **wait_for** 大約會等候五秒鐘，然後檢查是否完成。 如果順利比較，那麼您就知道非同步物件已順利完成，且您已完成作業。 如果您正在等候某種結果，之後只要呼叫 **get** 函式來擷取結果即可。

```cppwinrt
int main()
{
    IAsyncOperation<int> async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        printf("result %d\n", async.get());
    }
}
```

因為非同步物件已由完成，所以 **get** 會立即傳回結果，而不需要任何進一步的等候。 如您所見，**wait_for** 會傳回非同步物件的狀態。 因此，您可以使用它來進行更精細的控制，就像這樣。

```cppwinrt
switch (async.wait_for(5s))
{
case AsyncStatus::Completed:
    printf("result %d\n", async.get());
    break;
case AsyncStatus::Canceled:
    puts("canceled");
    break;
case AsyncStatus::Error:
    puts("failed");
    break;
case AsyncStatus::Started:
    puts("still running");
    break;
}
```

- 請記住，**AsyncStatus::Completed** 表示非同步物件已順利完成，而您可呼叫 **get** 函式來擷取任何結果。
- **AsyncStatus::Canceled** 表示非同步物件已取消。 取消作業通常是由呼叫端提出要求，因此很少需要處理此狀態。 一般來說，已取消的非同步物件只是被捨棄。
- **AsyncStatus::Error** 表示非同步物件在某些方面失敗。 您可以視需要利用 **get** 重新擲回例外狀況。
- **AsyncStatus::Started**表示非同步物件仍在執行中。 Windows 執行階段非同步模式不允許多次等候，也不允許多位等候者。 這表示您無法在迴圈中呼叫 **wait_for**。 如果等候實際上逾時，您會有幾個選擇。 您可以放棄物件，也可以先輪詢其狀態，然後再呼叫 **get** 來擷取任何結果。 但在此時最好捨棄物件。

## <a name="returning-an-array-asynchronously"></a>以非同步方式傳回陣列

以下是 [MIDL 3.0](/uwp/midl-3/) 的範例，會產生*錯誤 MIDL2025：[msg]語法錯誤 [context]：預期應大於或接近 "["* 。

```idl
Windows.Foundation.IAsyncOperation<Int32[]> RetrieveArrayAsync();
```

其原因是，以陣列作為參數化介面的參數類型引數是無效的。 因此，我們需要以較隱含的方式，達到以非同步方式從執行階段類別方法傳回陣列的目的。 

您可以傳回已 Box 處理為 [PropertyValue](/uwp/api/windows.foundation.propertyvalue) 物件的陣列。 呼叫的程式碼隨後會將其 Unbox 處理。 以下是程式碼範例，您可以試著將 **SampleComponent** 執行階段類別新增至 **Windows 執行階段元件 (C++/WinRT)** 專案，然後從 **核心應用程式 (C++/WinRT)** 專案 (舉例而言) 加以取用。

```cppwinrt
// SampleComponent.idl
namespace MyComponentProject
{
    runtimeclass SampleComponent
    {
        Windows.Foundation.IAsyncOperation<IInspectable> RetrieveCollectionAsync();
    };
}

// SampleComponent.h
...
struct SampleComponent : SampleComponentT<SampleComponent>
{
    ...
    Windows::Foundation::IAsyncOperation<Windows::Foundation::IInspectable> RetrieveCollectionAsync()
    {
        co_return Windows::Foundation::PropertyValue::CreateInt32Array({ 99, 101 }); // Box an array into a PropertyValue.
    }
}
...

// SampleCoreApp.cpp
...
MyComponentProject::SampleComponent m_sample_component;
...
auto boxed_array{ co_await m_sample_component.RetrieveCollectionAsync() };
auto property_value{ boxed_array.as<winrt::Windows::Foundation::IPropertyValue>() };
winrt::com_array<int32_t> my_array;
property_value.GetInt32Array(my_array); // Unbox back into an array.
...
```

## <a name="important-apis"></a>重要 API
* [IAsyncAction 介面](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; 介面](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; 介面](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 介面](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync 方法](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::resume_foreground](/uwp/cpp-ref-for-winrt/resume-foreground)

## <a name="related-topics"></a>相關主題
* [並行和非同步作業](concurrency.md)
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)