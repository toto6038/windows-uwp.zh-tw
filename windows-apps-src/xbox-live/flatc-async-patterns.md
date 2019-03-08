---
title: 非同步的 C API 呼叫的模式
description: 了解非同步 C API XSAPI C API 的呼叫模式
ms.date: 06/10/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 開發人員計劃，
ms.localizationpriority: medium
ms.openlocfilehash: edc6248a363b844d94c8fa03ab7ce071cc941908
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608003"
---
# <a name="calling-pattern-for-xsapi-flat-c-layer-async-calls"></a>呼叫模式 XSAPI 一般 C 層非同步呼叫

**非同步 API**是一種 API，快速地傳回，但啟動**非同步工作**工作完成之後，傳回結果。

傳統遊戲都有控制哪一個執行緒上執行幾乎**非同步工作**，並使用時，哪一個執行緒會傳回結果**完成回呼**。  某些遊戲所設計，以便在堆積的一個區段只介紹由單一執行緒，以避免任何需要執行緒同步處理。 如果**完成回呼**遊戲從執行緒不呼叫的結果以更新共用的狀態的控制項**非同步工作**需要執行緒同步處理。

XSAPI C API 會公開新的非同步 C API 可讓開發人員直接的執行緒控制進行時**非同步 API**呼叫，例如**XblSocialGetSocialRelationshipsAsync()**， **XblProfileGetUserProfileAsync()** 並**XblAchievementsGetAchievementsForTitleIdAsync()**。

以下是基本範例呼叫**XblProfileGetUserProfileAsync** API。

```cpp
AsyncBlock* asyncBlock = new AsyncBlock {};
asyncBlock->queue = asyncQueue;
asyncBlock->context = customDataForCallback;
asyncBlock->callback = [](AsyncBlock* asyncBlock)
{
    XblUserProfile profile;
    if( SUCCEEDED( XblProfileGetUserProfileResult(asyncBlock, &profile) ) )
    {
        printf("Profile retrieved successfully\r\n");
    }
    delete asyncBlock;
};
XblProfileGetUserProfileAsync(asyncBlock, xboxLiveContext, xuid);
```

若要了解這種呼叫模式，您必須了解如何使用**AsyncBlock**並**AsyncQueue**。

* **AsyncBlock**攜帶的相關資訊的所有**非同步工作**並**完成回呼**。

* **AsyncQueue**可讓您判斷哪一個執行緒執行**非同步工作**哪一個執行緒呼叫 AsyncBlock**完成回呼**。

## <a name="the-asyncblock"></a>**AsyncBlock**

讓我們看看**AsyncBlock**在詳細資料。 它是結構，定義如下：

```cpp
typedef struct AsyncBlock
{
    AsyncCompletionRoutine* callback;
    void* context;
    async_queue_handle_t queue;
} AsyncBlock;
```

**AsyncBlock**包含：

* *回呼*-非同步工作完成之後就會呼叫的選擇性回呼函式。  如果您未指定的回呼，您可以等候**AsyncBlock**來完成，但**GetAsyncStatus** ，然後取得結果。
* *內容*-可讓您將資料傳遞至回呼函式。
* *佇列*-也就是控制代碼的 async_queue_handle_t 的指定**AsyncQueue**。 如果未設定這個項目，是將使用預設佇列。

如需每個非同步呼叫的 API 在堆積上，您應該建立新的 AsyncBlock。  之前稱為 AsyncBlock 完成回呼 」，然後才能刪除它，就必須即時 AsyncBlock。

> [!IMPORTANT]
> **AsyncBlock**必須留在記憶體中，直到**非同步工作**完成。 如果它以動態方式配置，才能刪除它內 AsyncBlock**完成回呼**。

### <a name="waiting-for-asynchronous-task"></a>等候**非同步工作**

您所見**非同步工作**會完成幾種不同的方式：

* AsyncBlock**完成回呼**稱為
* 呼叫**GetAsyncStatus**使用 true 來等候它完成。
* 在設定 waitEvent **AsyncBlock**並等待事件收到訊號

與**GetAsyncStatus** waitEvent，及**非同步工作**就會被視為之後 AsyncBlock**完成回呼**不過執行 AsyncBlock**完成回呼**是選擇性的。

一次**非同步工作**是完成時，您可以得到的結果。

### <a name="getting-the-result-of-the-asynchronous-task"></a>取得的結果**非同步工作**

若要得到的結果，大部分**非同步 API**函式具有對應\[函式名稱\]產生函式來接收非同步呼叫的結果。 在我們的範例程式碼**XblProfileGetUserProfileAsync**都有對應**XblProfileGetUserProfileResult**函式。 您可以使用此函式傳回的函式的結果，並據此採取行動。  請參閱每個文件**非同步 API**函式，如需擷取結果的完整詳細資料。

## <a name="the-asyncqueue"></a>**AsyncQueue**

**AsyncQueue**可讓您判斷哪一個執行緒執行**非同步工作**哪一個執行緒呼叫 AsyncBlock**完成回呼**。

您可以控制哪一個執行緒執行這些作業，藉由設定*分派模式*。 有可用的三種分派模式：

* *手動*-手動佇列不會自動發送。  它是由開發人員在任何他們想要的執行緒發送。 這可用來將指派的工作 」 或 「 回呼在特定執行緒的非同步呼叫端。  這會在下面詳細討論。

* *執行緒集區*-分派使用執行緒集區。  執行緒集區會叫用的呼叫，以平行方式，採用呼叫從佇列依序執行，執行緒集區執行緒變成可用。  這是要使用 easist，但可讓您最少的執行緒所用的控制項。

* *已修正執行緒*-建立非同步佇列的執行緒上使用 QueueUserAPC 分派。 當使用者模式 APC 排入佇列時，執行緒是不會導向至呼叫 APC 函式，除非它是在可警示的狀態。 執行緒進入可警示的狀態，藉由使用**SleepEx**， **SignalObjectAndWait**， **WaitForSingleObjectEx**， **WaitForMultipleObjectsEx**，或**MsgWaitForMultipleObjectsEx**執行警示的等待作業

若要建立新**AsyncQueue**您必須呼叫**CreateAsyncQueue**。

```cpp
STDAPI CreateAsyncQueue(
    _In_ AsyncQueueDispatchMode workDispatchMode,
    _In_ AsyncQueueDispatchMode completionDispatchMode,
    _Out_ async_queue_handle_t* queue);
```

其中 AsyncQueueDispatchMode 包含稍早的三個的分派模式：

```cpp
typedef enum AsyncQueueDispatchMode
{
    /// <summary>
    /// Async callbacks are invoked manually by DispatchAsyncQueue
    /// </summary>
    AsyncQueueDispatchMode_Manual,

    /// <summary>
    /// Async callbacks are queued to the thread that created the queue
    /// and will be processed when the thread is alertable.
    /// </summary>
    AsyncQueueDispatchMode_FixedThread,

    /// <summary>
    /// Async callbacks are queued to the system thread pool and will
    /// be processed in order by the threadpool.
    /// </summary>
    AsyncQueueDispatchMode_ThreadPool

} AsyncQueueDispatchMode;
```

**workDispatchMode**決定用來處理非同步工作中，執行緒的分派模式雖然**completionDispatchMode**決定用來處理非同步完成執行緒的分派模式作業。

您也可以呼叫**CreateSharedAsyncQueue**來建立**AsyncQueue**藉由指定佇列的識別碼相同的佇列類型。

```cpp
STDAPI CreateSharedAsyncQueue(
    _In_ uint32_t id,
    _In_ AsyncQueueDispatchMode workerMode,
    _In_ AsyncQueueDispatchMode completionMode,
    _Out_ async_queue_handle_t* aQueue);
```

> [!NOTE]
> 如果已經有具有此識別碼，並分派模式的佇列，它會參考它。  否則會建立新的佇列。

一旦您已建立您**AsyncQueue**只是將它加入**AsyncBlock**來控制執行緒在您的工作和完成函式。
完畢時使用**AsyncQueue**，通常當遊戲即將結束，您可以將它關閉與**CloseAsyncQueue**:

```cpp
STDAPI_(void) CloseAsyncQueue(
    _In_ async_queue_handle_t aQueue);
```

### <a name="manually-dispatching-an-asyncqueue"></a>以手動方式分派**AsyncQueue**

如果您使用的手動佇列分派模式**AsyncQueue**完成或工作佇列中，您必須以手動方式分派。
讓我們假設**AsyncQueue**所建立之設定為手動分派的工作佇列並完成佇列就像這樣：

```cpp
CreateAsyncQueue(
    AsyncQueueDispatchMode_Manual,
    AsyncQueueDispatchMode_Manual,
    &queue);
```

分派已指派的工作才能**AsyncQueueDispatchMode_Manual**您必須將它與分派**DispatchAsyncQueue**函式。

```cpp
STDAPI_(bool) DispatchAsyncQueue(
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type,
    _In_ uint32_t timeoutInMs);
```

* *佇列*-來分派工作上的佇列
* *型別*-此執行個體**AsyncQueueCallbackType**列舉
* *timeoutInMs* -uint32_t 以毫秒為單位的逾時。

有兩個所定義的回呼類型**AsyncQueueCallbackType**列舉：

```cpp
typedef enum AsyncQueueCallbackType
{
    /// <summary>
    /// Async work callbacks
    /// </summary>
    AsyncQueueCallbackType_Work,

    /// <summary>
    /// Completion callbacks after work is done
    /// </summary>
    AsyncQueueCallbackType_Completion
} AsyncQueueCallbackType;
```

### <a name="when-to-call-dispatchasyncqueue"></a>何時要呼叫**DispatchAsyncQueue**

若要檢查當佇列已收到新項目，您可以呼叫**AddAsyncQueueCallbackSubmitted**設定事件處理常式，讓您知道工作或完成準備好要分派的程式碼。

```cpp
STDAPI AddAsyncQueueCallbackSubmitted(
    _In_ async_queue_handle_t queue,
    _In_opt_ void* context,
    _In_ AsyncQueueCallbackSubmitted* callback,
    _Out_ uint32_t* token);
```

**AddAsyncQueueCallbackSubmitted**採用下列參數：

* *佇列*-非同步佇列提交的回呼。
* *內容*-應該傳遞至送出回呼的資料指標。
* *回呼*-新的回撥提交到佇列時就會叫用的函式。
* *語彙基元*-將在稍後的呼叫，若要使用的語彙基元**RemoveAsynceCallbackSubmitted**來移除回呼。

例如，以下是呼叫**AddAsyncQueueCallbackSubmitted**:

`AddAsyncQueueCallbackSubmitted(queue, nullptr, HandleAsyncQueueCallback, &m_callbackToken);`

對應**AsyncQueueCallbackSubmitted**回呼可能會實作，如下所示：

```cpp
void CALLBACK HandleAsyncQueueCallback(
    _In_ void* context,
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type)
{
    switch (type)
    {
    case AsyncQueueCallbackType::AsyncQueueCallbackType_Work:
        ReleaseSemaphore(g_workReadyHandle, 1, nullptr);
        break;
    }
}
```

然後在背景執行緒中，您可以接聽此喚醒並呼叫的號誌**DispatchAsyncQueue**。

```cpp
DWORD WINAPI BackgroundWorkThreadProc(LPVOID lpParam)
{
    HANDLE hEvents[2] =
    {
        g_workReadyHandle.get(),
        g_stopRequestedHandle.get()
    };

    async_queue_handle_t queue = static_cast<async_queue_handle_t>(lpParam);

    bool stop = false;
    while (!stop)
    {
        DWORD dwResult = WaitForMultipleObjectsEx(2, hEvents, false, INFINITE, false);
        switch (dwResult)
        {
        case WAIT_OBJECT_0: 
            // Background work is ready to be dispatched
            DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);
            break;

        case WAIT_OBJECT_0 + 1:
        default:
            stop = true;
            break;
        }
    }

    CloseAsyncQueue(queue);
    return 0;
}
```

建議您最好的做法是使用實作與 Win32 號誌物件。  如果改為您實作使用 Win32 事件物件，則您必須確保千萬別錯過任何事件的程式碼類似：

```cpp
    case WAIT_OBJECT_0: 
        // Background work is ready to be dispatched
        DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);        
        
        if (!IsAsyncQueueEmpty(queue, AsyncQueueCallbackType_Work))
        {
            // If there's more pending work, then set the event to process them
            SetEvent(g_workReadyHandle.get());
        }
        break;
```


您可以檢視在非同步整合的最佳作法的範例[社交 C 範例 AsyncIntegration.cpp](https://github.com/Microsoft/xbox-live-api/blob/master/InProgressSamples/Social/Xbox/C/AsyncIntegration.cpp)

