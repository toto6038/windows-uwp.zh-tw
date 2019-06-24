---
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: 本文說明使用視覺效果中的非同步方法的建議的方式C++元件擴充功能 (C++/CX) 使用 ppltasks.h 中的並行存取命名空間中定義的工作類別。
title: C++ 中的非同步程式設計
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, 執行緒, 非同步, C++
ms.localizationpriority: medium
ms.openlocfilehash: d0caf002a68ea1de1342381c9b1a7f9d745a7342
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322029"
---
# <a name="asynchronous-programming-in-ccx"></a>C++/CX 中的非同步程式設計
> [!NOTE]
> 本主題是為協助您維護您 C++/CX 應用程式。 但我們建議您將 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 用於新的應用程式。 C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。

本文說明使用視覺效果中的非同步方法的建議的方式C++元件擴充功能 (C++/CX) 使用`task`中所定義的類別`concurrency`ppltasks.h 中的命名空間。

## <a name="universal-windows-platform-uwp-asynchronous-types"></a>通用 Windows 平台 (UWP) 非同步類型
通用 Windows 平台 (UWP) 功能是一種設計完善的模型，適用於呼叫非同步方法，並提供取用該方法所需的類型。 如果您不熟悉 UWP 非同步模型，請先閱讀[非同步程式設計][AsyncProgramming]，然後閱讀本文的其他部分。

雖然您可以使用非同步 UWP Api 直接在C++，慣用的方法是使用[ **task 類別**][task-class] and its related types and functions, which are contained in the [**concurrency**][concurrencyNamespace]命名空間和定義於`<ppltasks.h>`。 **concurrency::task** 是一種通用的類型，但是當使用 **/ZW** 編譯器參數 (通用 Windows 平台 (UWP) app 和元件的必要參數) 時，task 類別會封裝 UWP 非同步類型，以便於：

-   鏈結多個非同步和同步作業

-   處理工作鏈結中的例外狀況

-   在工作鏈結中執行取消動作

-   確定個別工作會在適當的執行緒內容或 Apartment 中執行

本文提供有關如何使用 **task** 類別來搭配 UWP 非同步 API 的基本指導方針。 如需完成的文件的相關**任務**和其相關的方法，包括[**建立\_工作**][createTask], see [Task Parallelism (Concurrency Runtime)][taskParallelism]。 

## <a name="consuming-an-async-operation-by-using-a-task"></a>使用工作取用非同步作業
下列範例說明如何使用 task 類別取用傳回 [**IAsyncOperation**][IAsyncOperation] 介面並產生值的 **async** 方法。 這裡是基本步驟：

1.  呼叫 `create_task` 方法，並將 **IAsyncOperation^** 物件傳遞給它。

2.  呼叫此成員函式[ **task:: then** ][taskThen]上的工作，並提供 lambda，將會叫用非同步作業完成時。

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

已建立並傳回的工作[ **task:: then** ][taskThen]函式稱為*接續*。 使用者提供的 Lambda 輸入引數 (在這個情況中) 會是工作完成後所產生的結果。 如果您直接使用 **IAsyncOperation** 介面，它是與呼叫 [**IAsyncOperation::GetResults**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperation_TResult_#Windows_Foundation_IAsyncOperation_1_GetResults) 所擷取的值相同的值。

[ **Task:: then** ][taskThen]方法會立即傳回，並在非同步工作作業成功完成之前就不會執行它的委派。 在這個範例中，如果非同步作業擲回例外狀況，或者因取消要求而以取消狀態結束，則永遠不會執行接續。 我們稍後會描述如何編寫即使先前工作被取消或失敗，仍會執行的接續。

雖然您在本機堆疊中宣告這個工作變數，不過它會管理自己的週期，只會在所有作業完成且其所有相關參照超出範圍後，才刪除這個工作變數 (即使作業完成前已經傳回該方法)。

## <a name="creating-a-chain-of-tasks"></a>建立工作鏈結
非同步程式設計中通常會定義一連串的作業 (稱為「工作鏈結」  )，只有上一個接續完成後才會繼續執行下一個。 有時上一個 (或「前項」  ) 工作會產生接續要接受的輸入值。 藉由使用[ **task:: then** ][taskThen]方法，您可以建立工作鏈結以直覺式且簡單的方式; 方法會傳回**工作<T>** 其中**T**是 lambda 函式的傳回型別。 您可以撰寫多個接續工作鏈結： `myTask.then(…).then(…).then(…);`

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

-   第一個接續會將 [**IAsyncAction^** ][IAsyncAction] 物件轉換成 **task<void>** ，然後傳回 **task**。

-   第二個接續執行無錯處理，因此將 **void** (而不是 **task<void>** ) 當作輸入。 這是一個數值型接續。

-   第二個接續不會執行直到[ **DeleteAsync** ][deleteAsync]作業完成。

-   因為第二個接續是以值為基礎，如果作業是在呼叫開始著手[ **DeleteAsync** ][deleteAsync]擲回例外狀況，第二個接續不會執行完全。

**附註**  建立工作鏈結只是其中一個使用方式**工作**撰寫非同步作業的類別。 您也可以使用 join 和 choice 運算子 **&&** 以及 **||** 來編寫作業。 如需詳細資訊，請參閱 <<c0> [ 工作平行處理原則 （並行執行階段）][taskParallelism]。

## <a name="lambda-function-return-types-and-task-return-types"></a>Lambda 函式傳回類型和工作傳回類型
在工作接續中，Lambda 函式的傳回類型會包裝在 **task** 物件中。 如果 Lambda 傳回 **double**，則接續工作類型為 **task<double>** 。 不過，工作物件的設計不會讓它產生不必要的巢狀傳回類型。 若 Lambda 傳回 **IAsyncOperation&lt;SyndicationFeed^&gt;^** ，接續會傳回 **task&lt;SyndicationFeed^&gt;** ，而非 **task&lt;task&lt;SyndicationFeed^&gt;&gt;** 或 **task&lt;IAsyncOperation&lt;SyndicationFeed^&gt;^&gt;^** 。 這個處理程序稱為「非同步解除包裝」，它也可以保證接續內的非同步作業會在完成時才呼叫下一個接續。 

在上一個範例中，請注意工作傳回的是 **task<void>** 事件 (即使它的 Lambda 傳回 [**IAsyncInfo**][IAsyncInfo] 物件)。 下表摘要說明 Lambda 函式和封入工作之間的類型轉換：

| | |
|--------------------------------------------------------|---------------------|
| Lambda 傳回類型                                     | `.then` 傳回型別 |
| TResult                                                | 工作<TResult> |
| IAsyncOperation<TResult>^                        | 工作<TResult> |
| IAsyncOperationWithProgress&lt;TResult, TProgress&gt;^ | 工作<TResult> |
|IAsyncAction^                                           | 工作<void>    |
| IAsyncActionWithProgress<TProgress>^             |工作<void>     |
| 工作<TResult>                                    |工作<TResult>  |


## <a name="canceling-tasks"></a>取消工作
讓使用者有機會取消非同步作業通常是不錯的做法。 而且有時您也可能必須從工作鏈結外，透過程式設計的方式取消作業。 雖然每個\***非同步**傳回型別具有[**取消**][IAsyncInfoCancel] method that it inherits from [**IAsyncInfo**][IAsyncInfo]，很難請將它公開給外部方法。 為支援取消作業工作鏈結中的慣用的方法是使用[**取消\_語彙基元\_來源**](https://docs.microsoft.com/cpp/parallel/concrt/reference/cancellation-token-source-class)建立[**取消\_語彙基元**](https://docs.microsoft.com/cpp/parallel/concrt/reference/cancellation-token-class)，然後將權杖傳遞給建構函式的初始工作。 如果非同步工作會透過取消語彙基元，並[**取消\_語彙基元\_source::cancel** ](https://docs.microsoft.com/cpp/parallel/concrt/reference/cancellation-token-source-class?view=vs-2017)呼叫時，工作會自動呼叫**取消**上**IAsync\*** 取消作業，並傳遞要求其接續鏈結中向下。 下列虛擬程式碼將示範一些基本的方法。

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

當取消工作時， [**任務\_取消**][taskCanceled] exception is propagated down the task chain. Value-based continuations will simply not execute, but task-based continuations will cause the exception to be thrown when [**task::get**][taskGet]呼叫。 如果您有錯誤處理接續工作時，請確定它會攔截**任務\_取消**例外狀況明確。 (這個例外狀況不是從 [**Platform::Exception**](https://docs.microsoft.com/cpp/cppcx/platform-exception-class) 衍生的)。

取消作業需要搭配其他作業來完成。 如果您的接續不僅僅叫用 UWP 方法，還執行其他長時間執行的工作，則您必須定期檢查取消權杖的狀態，如果已經取消，請停止執行。 您可以清除接續已配置的所有資源之後，請呼叫[**取消\_目前\_工作**](https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017)取消該工作，並傳播到任何取消值為基礎的接續在它後面。 這裡是另一個範例：您可以建立一個工作鏈結來表示 [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) 作業的結果。 如果使用者選擇**取消** 按鈕， [ **IAsyncInfo::Cancel**][IAsyncInfoCancel]不會呼叫方法。 而作業將成功，但傳回 **nullptr**。 接續可測試輸入的參數並呼叫**取消\_目前\_工作**如果輸入是**nullptr**。

如需詳細資訊，請參閱 [PPL 中的取消](https://docs.microsoft.com/cpp/parallel/concrt/cancellation-in-the-ppl)

## <a name="handling-errors-in-a-task-chain"></a>處理工作鏈結中的錯誤
如果您希望即使接續在前項已經取消或擲回例外狀況的情況下仍繼續執行，請將 Lambda 函式的輸入指定為 **task<TResult>** 來將接續變成工作型接續，或在前項工作的 Lambda 傳回 [**IAsyncAction^** ][IAsyncAction] 時指定為 **task<void>** 。

若要處理工作鏈結中的錯誤和取消作業，您不需要將每個接續變成工作型接續，也不必在 `try…catch` 區塊置入每個可能擲回的作業。 您可以在工作鏈結的尾端加上工作型接續，然後在該處處理所有的錯誤。 任何例外狀況，這包括[**工作\_取消**][taskCanceled]例外狀況，將工作鏈結中向下傳播並略過任何值為基礎的接續，以便您可以將它處理中的錯誤處理以工作為基礎的接續。 我們可以重新改寫上一個範例，改用錯誤處理工作型接續：

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

以工作為基礎的接續，在中，我們呼叫此成員函式[ **task:: get** ][taskGet]取得工作的結果。 即使作業是不會產生任何結果的 [**IAsyncAction**][IAsyncAction]，我們仍然必須呼叫 **task::get**，這是因為 **task::get** 也會取得任何已經往下傳送到工作的例外狀況。 如果輸入工作正在儲存例外狀況，就會在呼叫 **task::get** 時擲回例外狀況。 如果您不要呼叫**task:: get**，或請勿使用以工作為基礎的接續鏈結中，結尾，或不會攔截例外狀況類型擲回，則**未觀察到\_工作\_的例外狀況**已刪除工作的所有參考時擲回。

只攔截您可以處理的例外狀況。 如果應用程式遇到您無法修復的錯誤，寧可讓應用程式當機，也不要在不明的狀況下繼續執行。 此外，在一般情況下，請勿嘗試攔截**未觀察到\_任務\_例外狀況**本身。 這個例外狀況主要用於診斷。 當**未觀察到\_任務\_例外狀況**會擲回，通常表示程式碼中的 bug。 大多數的原因是發生您必須處理的例外狀況，或者是程式碼的部分錯誤導致了無法修復的例外狀況。

## <a name="managing-the-thread-context"></a>管理執行緒內容
UWP app 的 UI 會在單一執行緒 Apartment (STA) 中執行。 工作的 lambda 會傳回[ **IAsyncAction**][IAsyncAction] or [**IAsyncOperation**][IAsyncOperation]是 apartment 感知。 如果在 STA 建立工作，則它的所有接續也會在 STA 中執行 (預設值)，除非您指定別的地方。 換言之，整個工作鏈結會繼承上層作業的 Apartment 感知。 這種行為有助於簡化與 UI 控制項的互動 (從 STA 才辦得到)。

比方說，在表示 XAML 頁面上，任何類別成員函式中的 UWP 應用程式，您可以填入[ **ListBox** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)內的控制項[ **then**][taskThen]方法，而不需要使用[**發送器**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)物件。

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

如果工作不會傳回[ **IAsyncAction**][IAsyncAction] or [**IAsyncOperation**][IAsyncOperation]，然後不是 apartment 感知並，根據預設，會執行其接續第一天可用的背景執行緒。

您可以使用來覆寫任何一種工作的預設執行緒內容的多載[ **task:: then** ][taskThen]採用[**工作\_接續\_內容**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-continuation-context-class)。 例如，在部分情況下，您可能想在背景執行緒中排程 Apartment 感知工作的接續。 在此情況下，您可以傳遞[**任務\_接續\_context::use\_任意**][useArbitrary]來排定在下一個可用的執行緒執行工作的工作多執行緒的 apartment。 這可提高接續的效能，因為它的工作不必與 UI 執行緒中發生的其他工作同步。

下列範例示範當很適合用來指定[**任務\_接續\_context::use\_任意**][useArbitrary]選項，但它也會示範如何預設接續內容可用於同步處理並行作業上的非安全執行緒集合。 在這個程式碼中，我們為 RSS 摘要設定了 URL 清單迴圈，而我們會針對每個 URL 啟動一個非同步作業以抓取摘要資料。 我們無法控制摘要的抓取順序，但這並不重要。 當每個 [**RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.isyndicationclient.retrievefeedasync) 作業完成時，第一個接續會接受 [**SyndicationFeed^** ](https://docs.microsoft.com/uwp/api/Windows.Web.Syndication.SyndicationFeed) 物件，然後使用它來初始化應用程式定義的 `FeedData^` 物件。 因為每個作業是從其他獨立的我們可能會加快向上藉由指定**任務\_接續\_context::use\_任意**接續內容. 不過，初始化每個 `FeedData` 物件後，我們必須將它們新增至 [**Vector**](https://docs.microsoft.com/cpp/cppcx/platform-collections-vector-class) (不是安全執行緒集合)。 因此，我們建立接續，並指定[**任務\_接續\_context::use\_目前**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017)以確保所有呼叫[**Append** ](https://docs.microsoft.com/uwp/api/windows.foundation.collections.ivector_t_.append)相同 Application Single-Threaded Apartment (ASTA) 內容中進行。 因為[**任務\_接續\_context::use\_預設**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017)是預設的內容，我們不需要明確地指定它，但我們在這裡的 sake 的清楚起見。

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
支援 [**IAsyncOperationWithProgress**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) 或 [**IAsyncActionWithProgress**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncActionWithProgress_TProgress_) 的方法可以在作業進行時 (完成前)，定期提供進度更新。 進度報告與工作和接續的概念無關。 您只是為物件的 [**Progress**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) 屬性提供委派而已。 委派的典型用途是更新 UI 中的進度列。

## <a name="related-topics"></a>相關主題
* [建立非同步作業，在C++/CX for UWP 應用程式](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)
* [視覺化C++語言參考](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx)
* [非同步程式設計][AsyncProgramming]
* [工作平行處理原則 （並行執行階段）][taskParallelism]
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)

<!-- LINKS -->
[AsyncProgramming]: <https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps> "AsyncProgramming"
[concurrencyNamespace]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace> "concurrency 命名空間"
[createTask]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task> "CreateTask"
[deleteAsync]: <https://msdn.microsoft.com/library/windows/apps/BR227199> "DeleteAsync"
[IAsyncAction]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx> "IAsyncAction"
[IAsyncOperation]: <https://msdn.microsoft.com/library/windows/apps/BR206598> "IAsyncOperation"
[IAsyncInfo]: <https://msdn.microsoft.com/library/windows/apps/BR206587> "IAsyncInfo"
[IAsyncInfoCancel]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncinfo.cancel> "IAsyncInfoCancel"
[taskCanceled]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-canceled-class> "TaskCancelled"
[task-class]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#get> "Task 類別"
[taskGet]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750017.aspx> "TaskGet"
[taskParallelism]: <https://docs.microsoft.com/cpp/parallel/concrt/task-parallelism-concurrency-runtime> "工作平行處理原則"
[taskThen]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#then> "TaskThen"
[useArbitrary]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750036.aspx> "UseArbitrary"
