---
title: WinRT API 錯誤處理
description: 了解如何進行 Xbox Live 服務呼叫的 WinRT api 時處理錯誤。
ms.assetid: b4f04798-df91-430e-90f0-83b5a48b9ab2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，錯誤處理
ms.localizationpriority: medium
ms.openlocfilehash: e72dfa0b6f98284c240cf6af2dde02439d694b48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598633"
---
# <a name="winrt-api-error-handling"></a>WinRT API 錯誤處理

在 WinRT API，能夠呼叫從 C + + /CX 中， C#，或 Javascript 中，使用例外狀況，因此必須使用 try/catch 區塊來處理錯誤報告錯誤。

請注意，兩個 try/catch 區塊所需的非同步呼叫，如下所示：

```cpp
    try
    {
        auto asyncOp = xboxLiveContext->TitleStorageService->UploadBlobAsync(
            blobMetadata,
            blobBuffer,
            TitleStorageETagMatchCondition::NotUsed,
            DEFAULT_UPLOAD_BLOCK_SIZE
            );

        create_task(asyncOp)
        .then([this, ui]( task<TitleStorageBlobMetadata^> t )
        {
            try
            {
                TitleStorageBlobMetadata^ blobMetadata = t.get();

                ui->Log(L"UploadBlobAsync succeeded");
                PrintBlobMetadata(ui, blobMetadata);

                SaveNewETag(blobMetadata->ETag);
            }
            catch (Platform::Exception^ ex)
            {
                // This could happen if there's a network error or the service returns an error
                ui->Log("The async task of UploadBlobAsync failed with", ex->ToString());
            }
        });
    }
    catch (Platform::Exception^ ex)
    {
        // This could happen if there's invalid args sent to the API
        ui->Log("The API call to UploadBlobAsync failed with", ex->ToString());
    }
```

XSAPI 非同步方法沒有時呼叫的方法以同步方式執行一些程式碼。  同步程式碼運作例如驗證輸入引數和啟動的非同步作業或動作。  因此，即使呼叫非同步方法可能會導致例外狀況 – 雖然這不應該發生 （也就是程式設計錯誤，不是網路錯誤） 的一般案例。  請在考慮此可能性和程式以謹慎的方式驗證輸入，並在開發期間，徹底測試您的程式碼。  不論您放置在 try/catch 呼叫本身，或將其中一個較高層級放在 呼叫堆疊，重點就是有一個。

## <a name="help-my-game-crashes-when-i-call-xyz-xbox-service-api"></a>幫助 ！ 當我呼叫 XYZ Xbox 服務 API 時，我的遊戲損毀

因此您的遊戲會當機，並呼叫堆疊顯示它是一個 Xbox 服務 API 呼叫，但它嗎？

```cpp
msvcr110.dll!Concurrency::details::_ReportUnobservedException() Line 2455    C++
Social110Release.exe!Concurrency::details::_ExceptionHolder::~_ExceptionHolder() Line 915    C++
Social110Release.exe!Concurrency::details::_Task_impl_base::~_Task_impl_base() Line 1488    C++
Social110Release.exe!Concurrency::details::_Task_impl<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::`scalar deleting destructor'(unsigned int)    C++
Social110Release.exe!Concurrency::task<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::_ContinuationTaskHandle<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64,void,<lambda_8571b6148830c0805feee6ba9e76a692>,std::integral_constant<bool,1>,Concurrency::details::_TypeSelectorNoAsync>::`scalar deleting destructor'(unsigned int)    C++
msvcr110.dll!Concurrency::details::_TaskCollection::_NotifyCompletedChoreAndFree(Concurrency::details::_UnrealizedChore * pChore) Line 1171    C++
msvcr110.dll!Concurrency::details::_UnrealizedChore::_UnstructuredChoreWrapper(Concurrency::details::_UnrealizedChore * pChore) Line 373    C++
msvcr110.dll!Concurrency::details::InternalContextBase::Dispatch(Concurrency::DispatchState * pDispatchState) Line 1716    C++
msvcr110.dll!Concurrency::details::FreeThreadProxy::Dispatch() Line 203    C++
msvcr110.dll!Concurrency::details::ThreadProxy::ThreadProxyMain(void * lpParameter) Line 174    C++
ntdll.dll!RtlUserThreadStart(long (void *) * StartAddress, void * Argument) Line 822    C++
```

這些呼叫堆疊會相當困擾。  並行存取，並行存取，...  喔 Microsoft::Xbox::Services::Social::XboxUserProfile 位於呼叫堆疊，因此，必須是罪魁禍首。  實際上，這是無效的 Xbox 使用者識別碼。 for GetUserProfileAsync 呼叫所產生的呼叫堆疊產生此呼叫堆疊的範例程式碼看起來像這樣：

```cpp
    auto pAsyncOp = requester->ProfileService->GetUserProfileAsync("abc123"); //passing invalid Xbox User Id;

    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)
    {
        // Oops, I forgot my exception handling code here.
        // If I don't call resultTask.get() and catch any potential exception it may throw,
        // then PPL will report an unobserved exception.  That unobserved exception will cause your
        // app to crash.
    });
```

您的呼叫堆疊是否包含 Concurrency::_ReportUnobservedException()？  探討另一部以上的呼叫堆疊。  這很重要。  如果您呼叫堆疊包含並行:: _ReportUnobservedException() 您已經在遊戲的程式碼中發現的 bug。

PPL 會建立可運用的其他工作的工作。  在上述範例中，create_task() 組建來呼叫 GetUserProfileAsync() 工作而.then() 會建立下列工作。  這些通常稱為前項工作 （第一次一個） 和接續 （秒）。  在範例中，接續遺漏錯誤處理。  如果工作擲回例外狀況，而且不會攔截例外狀況的工作或其接續的執行階段會結束應用程式。

談到接續工作的範例，請注意，有實際的兩種不同類型。  一種，工作為基礎的接續，會將先前的工作當作輸入引數。  這項工作中一律會執行，即使前項工作擲回例外狀況。  若要取得前項工作的結果，您必須呼叫.get() 引數。  第二個值為基礎，會收到上一個工作的輸出直接 – 首先是除了真的 syntactic sugar – 值為基礎的接續不完全執行，如果前項擲回例外狀況。

建議：若要避免損毀，請使用以工作為基礎的接續接續鏈結和範圍中的並行存取:: task::get() 或並行:: task::wait() 呼叫 try/catch 區塊可以復原的錯誤處理陳述式的結尾。

讓我們看看一些範例：

以工作為基礎的接續範例

```cpp
    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)   // Task-based continuation
    {
        try
        {
            XboxUserProfile^ result = resultTask.get();

            // success, do something
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
        }
    });
```

值為基礎的接續範例

```cpp
    create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
        // if the task didn't complete successfully, you'd better have a task-based
        // continuation at the end of the continuation chain or the app will crash.
    })
    .then( [this] (task<void> previousTask) // Task-based continuation
    {
        try
        {
            // DO NOT IGNORE THIS THINKING IT'S NOT IMPORTANT.

            // call concurrency::task::get and handle any unobserved exception
            // so the application doesn't crash.
            previousTask.get();

            // success, continue
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
            // By handling this exception, you ensure your application will not
            // crash when calling Xbox Service APIs
        }
    });
```

第三個解決方案 – 使用值為基礎的接續完全，但另一個執行緒上呼叫.get() 或.wait() 及攔截例外狀況那里。  以下是簡單的範例：

```cpp
    auto getProfileTask = create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
    });
    // Note the lack of a task-based continuation with error handling at the end

    // You may call .get() or .wait() on a value-based only chain, but
    // must ensure you surround the call in a try/catch block and handle errors
    try
    {
        getProfileTask.get();     // or getProfileTask.wait();
    }
    catch (Platform::Exception^ ex)
    {
        // concurrency::task::get threw an exception
        // safely handle the error here
        // By handling this exception, you ensure your application will not
        // crash when calling Xbox Service APIs
    }
```

**如果您未使用 PPL**如果您使用 AsyncOperationCompletionHandler 或 AsyncActionCompletionHandler 而不 PPL，則您也有一些錯誤處理，如果不正確地完成，將會導致應用程式當機。  以下是範例，示範如何處理錯誤

```cpp
    try
    {
        // Example is making a service call with an invalid XboxUserId which will result in an error.
        // The completion handler properly detects the error and does not crash the app.
        requester->ProfileService->GetUserProfileAsync("abc123")->Completed
            = ref new AsyncOperationCompletedHandler<XboxUserProfile^>([=] (IAsyncOperation<XboxUserProfile^>^ operation, Windows::Foundation::AsyncStatus status)
        {
            if( status == Windows::Foundation::AsyncStatus::Completed)
            {
                // Always check the AsyncStatus before calling GetResults().
                // If status is not AsyncStatus::Completed, calls to operation->GetResults()
                // may throw an exception.
                // You can also surround this call in a try/catch block for added safety.

                XboxUserProfile^ result = operation->GetResults();

                // success, do something with the result
            }
            else if( status == Windows::Foundation::AsyncStatus::Error )
            {
                // Failed
            }
        });
    }
    catch ( Platform::COMException^ ex )
    {
        // What is this try/catch block for?
        //
        // Xbox Service APIs do have some code that runs synchronously and errors need
        // to be safely handled.  In this example, if “” was passed instead of “abc123”,
        // then an invalid argument exception would be thrown when calling GetUserProfileAsync
    // See the next section for more a more detailed explanation.
        //
        // Note: this catch block will NOT catch exceptions thrown within the completion handler.
    }
```

**GetNextAsync() 和例外狀況**使用分頁 Api 嗎？  AchievementResult、 LeaderboardResult、 InventoryItemResult 和 TitleStorageBlobMetadataResult 物件都包含 GetNextAsync() 方法來要求下一頁結果。  沒有特殊的情況下，沒有更多資料可用時，呼叫 GetNextAsync() 時觸發例外狀況。  GetNextAsync() 同步執行期間擲回這個例外狀況。  在此情況下，GetNextAsync 方法會擲回 INET_E_DATA_NOT_AVAILABLE (0x800C0007)。

建議：請確定您包裝的 GetNextAsync() 方法在 try/catch 區塊中呼叫，並適當地處理 INET_E_DATA_NOT_AVAILABLE 案例。

```cpp
    try
    {
        // AchievementResult^ LastResult

        // Get next page of achievement results
        if(LastResult != nullptr)
        {
            auto getNextPage = LastResult->GetNextAsync(10);

            // create_task( getNextPage ) ...
        }
    }
    catch (Platform::Exception^ ex)
    {
        if (ex->HResult == INET_E_DATA_NOT_AVAILABLE)
        {
                // we hit the end of the achievements
        }
        else
        {
            // failed for unexpected reason
        }
    }
```
