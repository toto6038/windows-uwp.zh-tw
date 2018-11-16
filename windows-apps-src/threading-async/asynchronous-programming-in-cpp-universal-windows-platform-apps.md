---
author: normesta
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: 本文說明的建議的方式取用非同步方法，在 VisualC + + 元件延伸 (C + + /CX) 使用在 ppltasks.h 中的並行命名空間中定義的 task 類別。
title: C++ 中的非同步程式設計
ms.author: normesta
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, 執行緒, 非同步, C++
ms.localizationpriority: medium
ms.openlocfilehash: 33b110e713608260cd5c19544292e9211904a730
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6984203"
---
# <a name="asynchronous-programming-in-ccx"></a>C++/CX 中的非同步程式設計
> [!NOTE]
> 本主題是為協助您維護您 C++/CX 應用程式。 但我們建議您將 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 用於新的應用程式。 C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。

本文章說明 VisualC + + 元件延伸中運用非同步方法的建議的方式 (C + + /CX) 使用`task`中定義的類別`concurrency`ppltasks.h 中的命名空間。

## <a name="universal-windows-platform-uwp-asynchronous-types"></a>通用 Windows 平台 (UWP) 非同步類型
通用 Windows 平台 (UWP) 功能是一種設計完善的模型，適用於呼叫非同步方法，並提供取用該方法所需的類型。 如果您不熟悉 UWP 非同步模型，請先閱讀[非同步程式設計][AsyncProgramming] ，然後閱讀本文的其他部分。

雖然您可以在 C++ 中直接取用非同步 UWP API，不過最好的方法是使用 [**task 類別**][task-class] 及其相關類型和函式，這些都包含在 [**concurrency**][concurrencyNamespace] 命名空間並定義於 `<ppltasks.h>` 中。 **concurrency::task** 是一種通用的類型，但是當使用 **/ZW** 編譯器參數 (通用 Windows 平台 (UWP) app 和元件的必要參數) 時，task 類別會封裝 UWP 非同步類型，以便於：

-   鏈結多個非同步和同步作業

-   處理工作鏈結中的例外狀況

-   在工作鏈結中執行取消動作

-   確定個別工作會在適當的執行緒內容或 Apartment 中執行

本文提供有關如何使用 **task** 類別來搭配 UWP 非同步 API 的基本指導方針。 如需 **task** 以及其相關方法 (包括 [**create\_task**][createTask] 的完整文件，請參閱[工作平行處理原則 (並行執行階段)][taskParallelism]。 

## <a name="consuming-an-async-operation-by-using-a-task"></a>使用工作取用非同步作業
下列範例說明如何使用 task 類別取用傳回 [**IAsyncOperation**][IAsyncOperation] 介面並產生值的 **async** 方法。 這裡是基本步驟：

1.  呼叫 `create_task` 方法，並將 **IAsyncOperation^** 物件傳遞給它。

2.  在工作上呼叫成員函式 [**task::then**][taskThen]，並提供要在非同步作業完成後叫用的 Lambda。

``` cpp
#include <ppltasks.h>
using namespace concurrency;
using namespace Windows::Devices::Enumeration;
...
void App::TestAsync()
{    
    //Call the *Async method that starts the operation.
    IAsyncOperation<DeviceInformationCollection^>^ deviceOp =
        DeviceInformation::FindAllAsync();

    // Explicit construction. (Not recommended)
    // Pass the IAsyncOperation to a task constructor.
    // task<DeviceInformationCollection^> deviceEnumTask(deviceOp);

    // Recommended:
    auto deviceEnumTask = create_task(deviceOp);

    // Call the task's .then member function, and provide
    // the lambda to be invoked when the async operation completes.
    deviceEnumTask.then( [this] (DeviceInformationCollection^ devices )
    {       
        for(int i = 0; i < devices->Size; i++)
        {
            DeviceInformation^ di = devices->GetAt(i);
            // Do something with di...          
        }       
    }); // end lambda
    // Continue doing work or return...
}
```

[**task::then**][taskThen] 函式建立並傳回的工作稱為「接續」**。 使用者提供的 Lambda 輸入引數 (在這個情況中) 會是工作完成後所產生的結果。 如果您直接使用 **IAsyncOperation** 介面，它是與呼叫 [**IAsyncOperation::GetResults**](https://msdn.microsoft.com/library/windows/apps/br206600) 所擷取的值相同的值。

[**task::then**][taskThen] 方法會立即傳回，而且除非非同步工作成功完成，否則不會執行它的委派。 在這個範例中，如果非同步作業擲回例外狀況，或者因取消要求而以取消狀態結束，則永遠不會執行接續。 我們稍後會描述如何編寫即使先前工作被取消或失敗，仍會執行的接續。

雖然您在本機堆疊中宣告這個工作變數，不過它會管理自己的週期，只會在所有作業完成且其所有相關參照超出範圍後，才刪除這個工作變數 (即使作業完成前已經傳回該方法)。

## <a name="creating-a-chain-of-tasks"></a>建立工作鏈結
非同步程式設計中通常會定義一連串的作業 (稱為「工作鏈結」**)，只有上一個接續完成後才會繼續執行下一個。 有時上一個 (或「前項」**) 工作會產生接續要接受的輸入值。 使用 [**task::then**][taskThen] 方法可讓您以直接又簡單的方法建立工作鏈結，這個方法會傳回 **task<T>**，其中 **T** 是 Lambda 函式的傳回類型。 您可以將多個接續編寫成一個工作鏈結： `myTask.then(…).then(…).then(…);`

當接續建立新的非同步作業時，工作鏈結尤為實用；這種工作稱為非同步工作。 下列範例說明有兩個接續的工作鏈結。 初始工作會取得現有檔案的控制代碼，而當該作業完成時，第一個接續會啟動新的非同步作業來刪除檔案。 當作業完成時，會執行第二個接續，然後輸出一個確認訊息。

``` cpp
#include <ppltasks.h>
using namespace concurrency;
...
void App::DeleteWithTasks(String^ fileName)
{    
    using namespace Windows::Storage;
    StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;
    auto getFileTask = create_task(localFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample) ->IAsyncAction^ {       
        return storageFileSample->DeleteAsync();
    }).then([](void) {
        OutputDebugString(L"File deleted.");
    });
}
```

上述範例中有 4 個重點：

-   第一個接續會將 [**IAsyncAction^**][IAsyncAction] 物件轉換成 **task<void>**，然後傳回 **task**。

-   第二個接續執行無錯處理，因此將 **void** (而不是 **task<void>**) 當作輸入。 這是一個數值型接續。

-   第二個接續在 [**DeleteAsync**][deleteAsync] 作業完成後才會執行。

-   因為第二個是數值型接續，所以如果呼叫 [**DeleteAsync**][deleteAsync] 所啟動的作業擲回例外狀況，則第二個接續不會執行。

**注意：** 建立工作鏈結只是其中之一的方式來使用**task**類別編寫非同步作業。 您也可以使用 join 和 choice 運算子 **&&** 以及 **||** 來編寫作業。 如需詳細資訊，請參閱[工作平行處理原則 (並行執行階段)][taskParallelism]。

## <a name="lambda-function-return-types-and-task-return-types"></a>Lambda 函式傳回類型和工作傳回類型
在工作接續中，Lambda 函式的傳回類型會包裝在 **task** 物件中。 如果 Lambda 傳回 **double**，則接續工作類型為 **task<double>**。 不過，工作物件的設計不會讓它產生不必要的巢狀傳回類型。 若 Lambda 傳回 **IAsyncOperation&lt;SyndicationFeed^&gt;^**，接續會傳回 **task&lt;SyndicationFeed^&gt;**，而非 **task&lt;task&lt;SyndicationFeed^&gt;&gt;** 或 **task&lt;IAsyncOperation&lt;SyndicationFeed^&gt;^&gt;^**。 這個處理程序稱為 *「非同步解除包裝」*，它也可以保證接續內的非同步作業會在完成時才呼叫下一個接續。

在上一個範例中，請注意工作傳回的是 **task<void>** 事件 (即使它的 Lambda 傳回 [**IAsyncInfo**][IAsyncInfo] 物件)。 下表摘要說明 Lambda 函式和封入工作之間的類型轉換：

| | |
|--------------------------------------------------------|---------------------|
| Lambda 傳回類型                                     | `.then` 傳回類型 |
| TResult                                                | 工作<TResult> |
| IAsyncOperation<TResult>^                        | 工作<TResult> |
| IAsyncOperationWithProgress&lt;TResult, TProgress&gt;^ | 工作<TResult> |
|IAsyncAction^                                           | 工作<void>    |
| IAsyncActionWithProgress<TProgress>^             |工作<void>     |
| 工作<TResult>                                    |工作<TResult>  |


## <a name="canceling-tasks"></a>取消工作
讓使用者有機會取消非同步作業通常是不錯的做法。 而且有時您也可能必須從工作鏈結外，透過程式設計的方式取消作業。 雖然每個 \***Async** 傳回類型都有一個繼承自 [**IAsyncInfo**][IAsyncInfo] 的 [**Cancel**][IAsyncInfoCancel] 方法，不過將它公開給外部方法並不合適。 在工作鏈結中取消動作的最好方法，是利用 [**cancellation\_token\_source**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749985.aspx) 建立一個 [**cancellation\_token**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749975.aspx)，然後將權杖傳送到初始工作的建構函式。 如果使用取消權杖建立非同步工作，然後呼叫 [**cancellation\_token\_source::cancel**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750076.aspx)，則這個工作會自動在 **IAsync\*** 作業上呼叫 **Cancel**，然後將取消要求傳送到它的接續鏈結中。 下列虛擬程式碼將示範一些基本的方法。

``` cpp
//Class member:
cancellation_token_source m_fileTaskTokenSource;

// Cancel button event handler:
m_fileTaskTokenSource.cancel();

// task chain
auto getFileTask2 = create_task(documentsFolder->GetFileAsync(fileName),
                                m_fileTaskTokenSource.get_token());
//getFileTask2.then ...
```

取消作業時，會將 [**task\_canceled**][taskCanceled] 例外狀況往下傳送到工作鏈結。 數值型接續將不會執行，但工作型接續會在呼叫 [**task::get**][taskGet] 之後擲回例外狀況。 如果您有一個錯誤處理接續，請確定它會清楚地攔截 **task\_canceled** 例外狀況。 (這個例外狀況不是從 [**Platform::Exception**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755825.aspx) 衍生的)。

取消作業需要搭配其他作業來完成。 如果您的接續不僅僅叫用 UWP 方法，還執行其他長時間執行的工作，則您必須定期檢查取消權杖的狀態，如果已經取消，請停止執行。 清除接續內配置的所有資源後，呼叫 [**cancel\_current\_task**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749945.aspx) 來取消這個工作，然後將取消往下傳送到任何後續的數值型接續。 這裡是另一個範例：您可以建立一個工作鏈結來表示 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/BR207871) 作業的結果。 如果使用者選擇 **\[取消\]** 按鈕，則不會呼叫 [**IAsyncInfo::Cancel**][IAsyncInfoCancel] 方法。 而作業將成功，但傳回 **nullptr**。 接續可以測試輸入參數，如果輸入為 **nullptr**，則會呼叫 **cancel\_current\_task**。

如需詳細資訊，請參閱 [PPL 中的取消](https://msdn.microsoft.com/library/windows/apps/xaml/dd984117.aspx)

## <a name="handling-errors-in-a-task-chain"></a>處理工作鏈結中的錯誤
如果您希望即使接續在前項已經取消或擲回例外狀況的情況下仍繼續執行，請將 Lambda 函式的輸入指定為 **task<TResult>** 來將接續變成工作型接續，或在前項工作的 Lambda 傳回 [**IAsyncAction^**][IAsyncAction] 時指定為 **task<void>**。

若要處理工作鏈結中的錯誤和取消作業，您不需要將每個接續變成工作型接續，也不必在 `try…catch` 區塊置入每個可能擲回的作業。 您可以在工作鏈結的尾端加上工作型接續，然後在該處處理所有的錯誤。 任何例外狀況 (包含 [**task\_canceled**][taskCanceled] 例外狀況) 都會往下傳送至工作鏈結，然後略過任何數值型接續，如此您就可以在錯誤處理的工作型接續中處理這些例外狀況。 我們可以重新改寫上一個範例，改用錯誤處理工作型接續：

``` cpp
#include <ppltasks.h>
void App::DeleteWithTasksHandleErrors(String^ fileName)
{    
    using namespace Windows::Storage;
    using namespace concurrency;

    StorageFolder^ documentsFolder = KnownFolders::DocumentsLibrary;
    auto getFileTask = create_task(documentsFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample)
    {       
        return storageFileSample->DeleteAsync();
    })

    .then([](task<void> t)
    {

        try
        {
            t.get();
            // .get() didn' t throw, so we succeeded.
            OutputDebugString(L"File deleted.");
        }
        catch (Platform::COMException^ e)
        {
            //Example output: The system cannot find the specified file.
            OutputDebugString(e->Message->Data());
        }

    });
}
```

在工作型接續中，我們會呼叫成員函式 [**task::get**][taskGet] 來取得工作的結果。 即使作業是不會產生任何結果的 [**IAsyncAction**][IAsyncAction]，我們仍然必須呼叫 **task::get**，這是因為 **task::get** 會取得任何已經往下傳送到工作的例外狀況。 如果輸入工作正在儲存例外狀況，就會在呼叫 **task::get** 時擲回例外狀況。 如果您不呼叫 **task::get**，或者未在鏈結的尾端使用工作型接續，或者未攔截擲回的例外類型，那麼當工作的所有參照全部被刪除時，就會擲回 **unobserved\_task\_exception**。

只攔截您可以處理的例外狀況。 如果應用程式遇到您無法修復的錯誤，寧可讓應用程式當機，也不要在不明的狀況下繼續執行。 另外，一般情況下，不要試著攔截 **unobserved\_task\_exception** 本身。 這個例外狀況主要用於診斷。 擲回 **unobserved\_task\_exception** 通常表示程式碼中有錯誤。 大多數的原因是發生您必須處理的例外狀況，或者是程式碼的部分錯誤導致了無法修復的例外狀況。

## <a name="managing-the-thread-context"></a>管理執行緒內容
UWP app 的 UI 會在單一執行緒 Apartment (STA) 中執行。 工作的 Lambda 如果傳回 [**IAsyncAction**][IAsyncAction] 或 [**IAsyncOperation**][IAsyncOperation] ，就是 Apartment 感知工作。 如果在 STA 建立工作，則它的所有接續也會在 STA 中執行 (預設值)，除非您指定別的地方。 換言之，整個工作鏈結會繼承上層作業的 Apartment 感知。 這種行為有助於簡化與 UI 控制項的互動 (從 STA 才辦得到)。

例如，在 UWP app 中，如果任何類別的成員函式代表 XAML 頁面，則您可以在 [**task::then**][taskThen] 方法中傳送 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) 控制項，不需使用 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211) 物件。

``` cpp
#include <ppltasks.h>
void App::SetFeedText()
{    
    using namespace Windows::Web::Syndication;
    using namespace concurrency;
    String^ url = "http://windowsteamblog.com/windows_phone/b/wmdev/atom.aspx";
    SyndicationClient^ client = ref new SyndicationClient();
    auto feedOp = client->RetrieveFeedAsync(ref new Uri(url));

    create_task(feedOp).then([this]  (SyndicationFeed^ feed)
    {
        m_TextBlock1->Text = feed->Title->Text;
    });
}
```

如果工作未傳回 [**IAsyncAction**][IAsyncAction] 或 [**IAsyncOperation**][IAsyncOperation]，則它便不是 Apartment 感知工作，而它的接續根據預設會在第一個可用的背景執行緒中執行。

您可以利用 [**task::then**][taskThen] 的多載 (使用 [**task\_continuation\_context**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749968.aspx))，覆寫任何類型工作的預設執行緒內容。 例如，在部分情況下，您可能想在背景執行緒中排程 Apartment 感知工作的接續。 在這個情況下，您可以在多執行緒 Apartment 中的下一個可用的執行緒，傳遞 [**task\_continuation\_context::use\_arbitrary**][useArbitrary] 以排程工作。 這可提高接續的效能，因為它的工作不必與 UI 執行緒中發生的其他工作同步。

下列範例示範什麼時候適合指定 [**task\_continuation\_context::use\_arbitrary**][useArbitrary] 選項，同時也說明預設的接續內容為什麼適用於同步非安全執行緒集合中的並行作業。 在這個程式碼中，我們為 RSS 摘要設定了 URL 清單迴圈，而我們會針對每個 URL 啟動一個非同步作業以抓取摘要資料。 我們無法控制摘要的抓取順序，但這並不重要。 當每個 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR210642) 作業完成時，第一個接續會接受 [**SyndicationFeed^**](https://msdn.microsoft.com/library/windows/apps/BR243485) 物件，然後使用它來初始化應用程式定義的 `FeedData^` 物件。 因為每個作業都是彼此獨立的，所以指定 **task\_continuation\_context::use\_arbitrary** 接續內容可以加快處理速度。 不過，初始化每個 `FeedData` 物件後，我們必須將它們新增至 [**Vector**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441570.aspx) (不是安全執行緒集合)。 因此，我們會建立一個接續並指定 [**task\_continuation\_context::use\_current**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750085.aspx)，以確保所有的 [**Append**](https://msdn.microsoft.com/library/windows/apps/BR206632) 呼叫會在同一個應用程式單一執行緒 Apartment (ASTA) 內容中進行。 因為 [**task\_continuation\_context::use\_default**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750085.aspx) 是預設的內容，所以我們不必明確指定它 (這裡是為了讓您可以更清楚了解才這樣做的)。

``` cpp
#include <ppltasks.h>
void App::InitDataSource(Vector<Object^>^ feedList, vector<wstring> urls)
{
                using namespace concurrency;
    SyndicationClient^ client = ref new SyndicationClient();

    std::for_each(std::begin(urls), std::end(urls), [=,this] (std::wstring url)
    {
        // Create the async operation. feedOp is an
        // IAsyncOperationWithProgress<SyndicationFeed^, RetrievalProgress>^
        // but we don't handle progress in this example.

        auto feedUri = ref new Uri(ref new String(url.c_str()));
        auto feedOp = client->RetrieveFeedAsync(feedUri);

        // Create the task object and pass it the async operation.
        // SyndicationFeed^ is the type of the return value
        // that the feedOp operation will eventually produce.

        // Then, initialize a FeedData object by using the feed info. Each
        // operation is independent and does not have to happen on the
        // UI thread. Therefore, we specify use_arbitrary.
        create_task(feedOp).then([this]  (SyndicationFeed^ feed) -> FeedData^
        {
            return GetFeedData(feed);
        }, task_continuation_context::use_arbitrary())

        // Append the initialized FeedData object to the list
        // that is the data source for the items collection.
        // This all has to happen on the same thread.
        // By using the use_default context, we can append
        // safely to the Vector without taking an explicit lock.
        .then([feedList] (FeedData^ fd)
        {
            feedList->Append(fd);
            OutputDebugString(fd->Title->Data());
        }, task_continuation_context::use_default())

        // The last continuation serves as an error handler. The
        // call to get() will surface any exceptions that were raised
        // at any point in the task chain.
        .then( [this] (task<void> t)
        {
            try
            {
                t.get();
            }
            catch(Platform::InvalidArgumentException^ e)
            {
                //TODO handle error.
                OutputDebugString(e->Message->Data());
            }
        }); //end task chain

    }); //end std::for_each
}
```

巢狀工作是建立在接續內的新工作，不會繼承初始工作的 Apartment 感知。

## <a name="handing-progress-updates"></a>處理進度更新
支援 [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx) 或 [**IAsyncActionWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206581.aspx) 的方法可以在作業進行時 (完成前)，定期提供進度更新。 進度報告與工作和接續的概念無關。 您只是為物件的 [**Progress**](https://msdn.microsoft.com/library/windows/apps/br206594) 屬性提供委派而已。 委派的典型用途是更新 UI 中的進度列。

## <a name="related-topics"></a>相關主題
* [使用 C++/CX 建立 UWP app 的非同步作業](https://msdn.microsoft.com/library/hh750082)
* [Visual C++ 語言參考](http://msdn.microsoft.com/library/windows/apps/hh699871.aspx)
* [非同步程式設計][AsyncProgramming]
* [工作平行處理原則 (並行執行階段)][taskParallelism]
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)

<!-- LINKS -->
[AsyncProgramming]: <https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps> "AsyncProgramming"
[concurrencyNamespace]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace> "並行命名空間"
[createTask]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task> "CreateTask"
[deleteAsync]: <https://msdn.microsoft.com/library/windows/apps/BR227199> "DeleteAsync"
[IAsyncAction]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx> "IAsyncAction"
[IAsyncOperation]: <https://msdn.microsoft.com/library/windows/apps/BR206598> "IAsyncOperation"
[IAsyncInfo]: <https://msdn.microsoft.com/library/windows/apps/BR206587> "IAsyncInfo"
[IAsyncInfoCancel]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncinfo.cancel> "IAsyncInfoCancel"
[taskCanceled]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-canceled-class> "TaskCancelled"
[task-class]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#get> "工作類別"
[taskGet]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750017.aspx> "TaskGet"
[taskParallelism]: <https://docs.microsoft.com/cpp/parallel/concrt/task-parallelism-concurrency-runtime> "工作平行"
[taskThen]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#then> "TaskThen"
[useArbitrary]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750036.aspx> "UseArbitrary"
