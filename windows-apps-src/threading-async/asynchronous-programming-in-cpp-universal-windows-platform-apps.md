---
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: 此文章說明在 Visual C++ 元件延伸 (C++/CX) 中取用非同步方法的建議方式 (使用在 ppltasks.h 之 concurrency 命名空間中所定義的 task 類別)。
title: C++ 中的非同步程式設計
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, 執行緒, 非同步, C++
ms.localizationpriority: medium
ms.openlocfilehash: 0e3810b25ac35cbf5e16f49a86affb4792089d1e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161792"
---
# <a name="asynchronous-programming-in-ccx"></a>C++/CX 中的非同步程式設計
> [!NOTE]
> 本主題是為協助您維護您 C++/CX 應用程式。 但我們建議您將 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 用於新的應用程式。 C++/WinRT 是完全標準現代的 Windows 執行階段 (WinRT) API 的 C++17 語言投影，僅實作為標頭檔案式程式庫，以及設計用來提供您現代化 Windows API 的第一級存取。

此文章說明在 Visual C++ 元件延伸 (C++/CX) 中取用非同步方法的建議方式 (使用在 ppltasks.h 的 `concurrency` 命名空間中所定義的 `task` 類別)。

## <a name="universal-windows-platform-uwp-asynchronous-types"></a>通用 Windows 平台 (UWP) 非同步類型
通用 Windows 平台 (UWP) 功能是一種設計完善的模型，適用於呼叫非同步方法，並提供取用該方法所需的類型。 如果您不熟悉 UWP 非同步模型，請先閱讀[非同步程式設計][AsyncProgramming] ，然後閱讀本文的其他部分。

雖然您可以直接在 c + + 中使用非同步 Windows 執行階段 Api，但慣用的方法是使用工作 [**類別**][task-class] 及其相關的型別和函式，這些都包含在 [**並行**][concurrencyNamespace] 命名空間中，並在中定義 `<ppltasks.h>` 。 **concurrency::task** 是一種通用的類型，但是當使用 **/ZW** 編譯器參數 (通用 Windows 平台 (UWP) app 和元件的必要參數) 時，task 類別會封裝 UWP 非同步類型，以便於：

-   鏈結多個非同步和同步作業

-   處理工作鏈結中的例外狀況

-   在工作鏈結中執行取消動作

-   確定個別工作會在適當的執行緒內容或 Apartment 中執行

本文提供有關如何使用 **task** 類別來搭配 UWP 非同步 API 的基本指導方針。 如需工作及其相關方法（包括建立工作） [** \_ **][createTask]的完整**檔，請**參閱[ (並行執行階段) ][taskParallelism]的工作平行處理原則。 

## <a name="consuming-an-async-operation-by-using-a-task"></a>使用工作取用非同步作業
下列範例說明如何使用 task 類別取用傳回 [**IAsyncOperation**][IAsyncOperation] 介面並產生值的 **async** 方法。 基礎步驟如下：

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

[**task::then**][taskThen] 函式建立並傳回的工作稱為「接續」**。 使用者提供的 Lambda 輸入引數 (在這個情況中) 會是工作完成後所產生的結果。 如果您直接使用 **IAsyncOperation** 介面，它是與呼叫 [**IAsyncOperation::GetResults**](/uwp/api/Windows.Foundation.IAsyncOperation_TResult_#Windows_Foundation_IAsyncOperation_1_GetResults) 所擷取的值相同的值。

[**task::then**][taskThen] 方法會立即傳回，而且除非非同步工作成功完成，否則不會執行它的委派。 在這個範例中，如果非同步作業擲回例外狀況，或者因取消要求而以取消狀態結束，則永遠不會執行接續。 我們稍後會描述如何編寫即使先前工作被取消或失敗，仍會執行的接續。

雖然您在本機堆疊中宣告這個工作變數，不過它會管理自己的週期，只會在所有作業完成且其所有相關參照超出範圍後，才刪除這個工作變數 (即使作業完成前已經傳回該方法)。

## <a name="creating-a-chain-of-tasks"></a>建立工作鏈結
非同步程式設計中通常會定義一連串的作業 (稱為「工作鏈結」**)，只有上一個接續完成後才會繼續執行下一個。 有時上一個 (或「前項」**) 工作會產生接續要接受的輸入值。 使用 [**task::then**][taskThen] 方法可讓您以直接又簡單的方法建立工作鏈結，這個方法會傳回 **task<T>**，其中 **T** 是 Lambda 函式的傳回類型。 您可以在工作鏈中撰寫多個接續： `myTask.then(…).then(…).then(…);`

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

**注意**   建立工作鏈只是使用**工作類別來**撰寫非同步作業的其中一種方式。 您也可以使用 join 和 choice 運算子和來撰寫 **&&** 作業 **||** 。 如需詳細資訊，請參閱[工作平行處理原則 (並行執行階段)][taskParallelism]。

## <a name="lambda-function-return-types-and-task-return-types"></a>Lambda 函式傳回類型和工作傳回類型
在工作接續中，Lambda 函式的傳回類型會包裝在 **task** 物件中。 如果 Lambda 傳回 **double**，則接續工作類型為 **task<double>**。 不過，工作物件的設計不會讓它產生不必要的巢狀傳回類型。 若 Lambda 傳回 **IAsyncOperation&lt;SyndicationFeed^&gt;^**，接續會傳回 **task&lt;SyndicationFeed^&gt;**，而非 **task&lt;task&lt;SyndicationFeed^&gt;&gt;** 或 **task&lt;IAsyncOperation&lt;SyndicationFeed^&gt;^&gt;^**。 此程式稱為非同步解除 *包裝* ，也可確保在叫用下一個接續之前，會先完成接續內的非同步作業。

在上一個範例中，請注意工作傳回的是 **task<void>** 事件 (即使它的 Lambda 傳回 [**IAsyncInfo**][IAsyncInfo] 物件)。 下表摘要說明 Lambda 函式和封入工作之間的類型轉換：

| | |
|--------------------------------------------------------|---------------------|
| Lambda 傳回類型                                     | `.then` 傳回類型 |
| TResult                                                | 任務<TResult> |
| IAsyncOperation<TResult>^                        | 任務<TResult> |
| IAsyncOperationWithProgress&lt;TResult, TProgress&gt;^ | 任務<TResult> |
|IAsyncAction^                                           | 任務<void>    |
| IAsyncActionWithProgress<TProgress>^             |任務<void>     |
| 任務<TResult>                                    |任務<TResult>  |


## <a name="canceling-tasks"></a>取消工作
讓使用者有機會取消非同步作業通常是不錯的做法。 而且有時您也可能必須從工作鏈結外，透過程式設計的方式取消作業。 雖然每個 \* **非同步**傳回型別都有一個從[**IAsyncInfo**][IAsyncInfo]繼承的[**Cancel**][IAsyncInfoCancel]方法，但是將它公開給外部方法並不難。 在工作鏈中支援取消的慣用方式是使用 [**取消 \_ 標記 \_ 來源**](/cpp/parallel/concrt/reference/cancellation-token-source-class) 來建立 [**取消 \_ 權杖**](/cpp/parallel/concrt/reference/cancellation-token-class)，然後將權杖傳遞給初始工作的函式。 如果使用解除標記來建立異步工作，並呼叫[**取消 \_ 標記 \_ 來源：： cancel**](/cpp/parallel/concrt/reference/cancellation-token-source-class?view=vs-2017) ，則工作會自動對**IAsync \* **作業呼叫**cancel** ，並將取消要求傳遞至其接續鏈。 下列虛擬程式碼將示範一些基本的方法。

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

當工作取消時，[工作 [**已 \_ 取消**][taskCanceled] ] 例外狀況會傳播至工作鏈。 數值型接續將不會執行，但工作型接續會在呼叫 [**task::get**][taskGet] 之後擲回例外狀況。 如果您有錯誤處理接續，請確定它會明確攔截工作 ** \_ 已取消** 的例外狀況。 (這個例外狀況不是從 [**Platform::Exception**](/cpp/cppcx/platform-exception-class) 衍生的)。

取消作業需要搭配其他作業來完成。 如果您的接續不僅僅叫用 UWP 方法，還執行其他長時間執行的工作，則您必須定期檢查取消權杖的狀態，如果已經取消，請停止執行。 清除在接續配置的所有資源之後，請呼叫 [**取消 \_ 目前 \_ **](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) 的工作來取消該工作，並將取消傳播至其後的任何以值為基礎的接續。 這裡是另一個範例：您可以建立一個工作鏈結來表示 [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) 作業的結果。 如果使用者選擇 **\[取消\]** 按鈕，則不會呼叫 [**IAsyncInfo::Cancel**][IAsyncInfoCancel] 方法。 而作業將成功，但傳回 **nullptr**。 接續可以測試輸入參數，並在輸入為**nullptr**時呼叫**取消 \_ 目前 \_ **的工作。

如需詳細資訊，請參閱 [PPL 中的取消](/cpp/parallel/concrt/cancellation-in-the-ppl)

## <a name="handling-errors-in-a-task-chain"></a>處理工作鏈結中的錯誤
如果您希望即使接續在前項已經取消或擲回例外狀況的情況下仍繼續執行，請將 Lambda 函式的輸入指定為 **task<TResult>** 來將接續變成工作型接續，或在前項工作的 Lambda 傳回 [**IAsyncAction^**][IAsyncAction] 時指定為 **task<void>**。

若要處理工作鏈結中的錯誤和取消作業，您不需要將每個接續變成工作型接續，也不必在 `try…catch` 區塊置入每個可能擲回的作業。 您可以在工作鏈結的尾端加上工作型接續，然後在該處處理所有的錯誤。 任何例外狀況（包括工作 [** \_ 取消**][taskCanceled] 的例外狀況）都會向下傳播工作鏈，並略過任何以值為基礎的接續，讓您可以在錯誤處理工作的接續中處理。 我們可以重新改寫上一個範例，改用錯誤處理工作型接續：

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

在工作型接續中，我們會呼叫成員函式 [**task::get**][taskGet] 來取得工作的結果。 即使作業是不會產生任何結果的 [**IAsyncAction**][IAsyncAction]，我們仍然必須呼叫 **task::get**，這是因為 **task::get** 會取得任何已經往下傳送到工作的例外狀況。 如果輸入工作正在儲存例外狀況，就會在呼叫 **task::get** 時擲回例外狀況。 如果您未呼叫 **task：： get**，或未在鏈結尾使用以工作為基礎的接續，或未攔截所擲回的例外狀況類型，則在刪除所有對工作的參考時，就會擲回 **未觀察到工作 \_ \_ 例外** 狀況。

只攔截您可以處理的例外狀況。 如果應用程式遇到您無法修復的錯誤，寧可讓應用程式當機，也不要在不明的狀況下繼續執行。 此外，一般而言，請勿嘗試攔截 **未觀察到工作 \_ \_ 例外** 狀況本身。 這個例外狀況主要用於診斷。 擲回 **未觀察到工作 \_ \_ 例外** 狀況時，通常會指出程式碼中的 bug。 大多數的原因是發生您必須處理的例外狀況，或者是程式碼的部分錯誤導致了無法修復的例外狀況。

## <a name="managing-the-thread-context"></a>管理執行緒內容
UWP app 的 UI 會在單一執行緒 Apartment (STA) 中執行。 工作的 Lambda 如果傳回 [**IAsyncAction**][IAsyncAction] 或 [**IAsyncOperation**][IAsyncOperation] ，就是 Apartment 感知工作。 如果在 STA 建立工作，則它的所有接續也會在 STA 中執行 (預設值)，除非您指定別的地方。 換言之，整個工作鏈結會繼承上層作業的 Apartment 感知。 這種行為有助於簡化與 UI 控制項的互動 (從 STA 才辦得到)。

例如，在 UWP app 中，如果任何類別的成員函式代表 XAML 頁面，則您可以在 [**task::then**][taskThen] 方法中傳送 [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) 控制項，不需使用 [**Dispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 物件。

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

您可以使用工作 [**：： then**][taskThen] 的多載，來覆寫這兩種工作的預設執行緒內容，以取得工作 [** \_ 接續 \_ 內容**](/cpp/parallel/concrt/reference/task-continuation-context-class)。 例如，在部分情況下，您可能想在背景執行緒中排程 Apartment 感知工作的接續。 在這種情況下，您可以傳遞工作 [** \_ 接續內容 \_ ：：使用 \_ 任意**][useArbitrary] ，在多執行緒單元中的下一個可用執行緒上排程工作的工作。 這可提高接續的效能，因為它的工作不必與 UI 執行緒中發生的其他工作同步。

下列範例將示範何時可以指定工作 [** \_ 接續內容 \_ ：： use \_ 任意**][useArbitrary] 選項，同時也會顯示在非安全線程的集合上同步處理並行作業時，預設接續內容是否有用。 在這個程式碼中，我們為 RSS 摘要設定了 URL 清單迴圈，而我們會針對每個 URL 啟動一個非同步作業以抓取摘要資料。 我們無法控制摘要的抓取順序，但這並不重要。 當每個 [**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.isyndicationclient.retrievefeedasync) 作業完成時，第一個接續會接受 [**SyndicationFeed^**](/uwp/api/Windows.Web.Syndication.SyndicationFeed) 物件，然後使用它來初始化應用程式定義的 `FeedData^` 物件。 由於每個作業都與其他作業無關，因此我們可以藉由指定工作接續內容來加速專案 ** \_ \_ ：：使用 \_ 任意** 接續內容。 不過，初始化每個 `FeedData` 物件後，我們必須將它們新增至 [**Vector**](/cpp/cppcx/platform-collections-vector-class) (不是安全執行緒集合)。 因此，我們會建立接續並指定 [**工作 \_ 接續內容 \_ ：：使用 \_ Current**](/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017) 來確保所有 [**附加**](/uwp/api/windows.foundation.collections.ivector_t_.append) 的呼叫都會出現在同一個應用程式單一執行緒的單元 (ASTA) 內容中。 由於 [**工作 \_ 接續內容 \_ ：： use \_ default**](/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017) 是預設的內容，因此我們不需要明確地指定，但為了清楚起見，我們會在這裡這麼做。

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
支援 [**IAsyncOperationWithProgress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) 或 [**IAsyncActionWithProgress**](/uwp/api/Windows.Foundation.IAsyncActionWithProgress_TProgress_) 的方法可以在作業進行時 (完成前)，定期提供進度更新。 進度報告與工作和接續的概念無關。 您只是為物件的 [**Progress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) 屬性提供委派而已。 委派的典型用途是更新 UI 中的進度列。

## <a name="related-topics"></a>相關主題
* [使用 C++/CX 建立 UWP app 的非同步作業](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)
* [Visual C++ 語言參考](/cpp/cppcx/visual-c-language-reference-c-cx)
* [非同步程式設計][AsyncProgramming]
* [工作平行處理 (並行執行階段) ][taskParallelism]
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)

<!-- LINKS -->
[AsyncProgramming]: ./asynchronous-programming-universal-windows-platform-apps.md "AsyncProgramming"
[concurrencyNamespace]: /cpp/parallel/concrt/reference/concurrency-namespace "Concurrency 命名空間"
[createTask]: /cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task "CreateTask"
[deleteAsync]: /uwp/api/Windows.Storage.StorageFile "DeleteAsync"
[IAsyncAction]: /uwp/api/Windows.Foundation.IAsyncAction "IAsyncAction"
[IAsyncOperation]: /uwp/api/windows.foundation.iasyncoperation-1 "Iasyncoperation<tresult>HTTP"
[IAsyncInfo]: /uwp/api/Windows.Foundation.IAsyncInfo "IAsyncInfo"
[IAsyncInfoCancel]: /uwp/api/Windows.Foundation.IAsyncInfo "IAsyncInfoCancel"
[taskCanceled]: /cpp/parallel/concrt/reference/task-canceled-class "TaskCancelled"
[task-class]: /cpp/parallel/concrt/reference/task-class#get "Task 類別"
[taskGet]: /cpp/parallel/concrt/reference/task-class "TaskGet"
[taskParallelism]: /cpp/parallel/concrt/task-parallelism-concurrency-runtime "工作平行"
[taskThen]: /cpp/parallel/concrt/reference/task-class#then "TaskThen"
[useArbitrary]: /cpp/parallel/concrt/reference/task-continuation-context-class "UseArbitrary"