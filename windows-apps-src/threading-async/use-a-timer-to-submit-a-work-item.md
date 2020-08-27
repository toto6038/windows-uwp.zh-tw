---
ms.assetid: AAE467F9-B3C7-4366-99A2-8A880E5692BE
title: 使用計時器提交工作項目
description: 瞭解如何使用 Windows.system.threading.threadpooltimer API 建立計時器，以在計時器于通用 Windows 平臺 (UWP) 應用程式中過期時提交工作專案。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, timer, threads, Windows 10, uwp, 計時器, 執行緒
ms.localizationpriority: medium
ms.openlocfilehash: 2c34f50d7b5abec28b11fc67a7e0515f07206060
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970126"
---
# <a name="use-a-timer-to-submit-a-work-item"></a>使用計時器提交工作項目


<b>重要 API</b>

-   [**Windows.UI.Core 命名空間**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)
-   [**Windows.System.Threading 命名空間**](https://docs.microsoft.com/uwp/api/Windows.System.Threading)

了解如何建立在計時器過後執行的工作項目。

## <a name="create-a-single-shot-timer"></a>建立單次計時器

使用 [**CreateTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer) 方法建立工作項目的計時器。 提供完成工作的 Lambda，以及使用 *delay* 參數指定執行緒集區需要等待多久的時間，才能將工作項目指派給可用的執行緒。 延遲使用 [**TimeSpan**](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan) 結構指定。

> **注意**   您可以使用[**CoreDispatcher RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync)來存取 UI，並顯示工作專案的進度。

下列範例會建立一個在三分鐘內執行的工作項目：

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, delay);
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             //
>             // TODO: Work
>             //
>             
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                     ExampleUIUpdateMethod("Timer completed.");
>
>                 }));
>
>         }), delay);
> ```

## <a name="provide-a-completion-handler"></a>提供完成處理常式

如有需要，可使用 [**TimerDestroyedHandler**](https://docs.microsoft.com/uwp/api/windows.system.threading.timerdestroyedhandler) 處理取消及完成工作項目。 使用 [**CreateTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer) 超載提供其他 Lambda。 這會在取消計時器或工作項目完成時執行。

下列範例會建立一個提交工作項目的計時器，並且在工作項目完成或取消計時器時呼叫方法：

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> bool completed = false;
>
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>                 CoreDispatcherPriority.High,
>                 () =>
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 });
>
>         completed = true;
>     },
>     delay,
>     (source) =>
>     {
>         //
>         // TODO: Handle work cancellation/completion.
>         //
>
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>                 if (completed)
>                 {
>                     // Timer completed.
>                 }
>                 else
>                 {
>                     // Timer cancelled.
>                 }
>
>             });
>     });
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> completed = false;
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Work
>             //
>
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 }));
>
>             completed = true;
>
>         }),
>         delay,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle work cancellation/completion.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // Update the UI thread by using the UI core dispatcher.
>                     //
>
>                     if (completed)
>                     {
>                         // Timer completed.
>                     }
>                     else
>                     {
>                         // Timer cancelled.
>                     }
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>取消計時器

如果計時器仍在倒數計時，但已不再需要該工作項目，可呼叫 [**Cancel**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.cancel)。 就會取消計時器，而且不會將工作項目提交至執行緒集區。

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> DelayTimer.Cancel();
> ```
> ``` cpp
> DelayTimer->Cancel();
> ```

## <a name="remarks"></a>備註

通用 Windows 平台 (UWP) app 無法使用 **Thread.Sleep**，因為它會封鎖 UI 執行緒。 您可以改為使用 [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) 來建立工作項目，這將會延遲由該工作項目完成的工作，而且不會封鎖 UI 執行緒。

如需示範工作項目、計時器工作項目以及定期工作項目的完整程式碼範例，請參閱[執行緒集區範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Thread%20pool%20sample)。 程式碼範例原本是針對 Windows 8.1 而撰寫的，但程式碼可以在 Windows 10 中重複使用。

如需重複計時器的詳細資訊，請參閱[建立定期工作項目](create-a-periodic-work-item.md)。

## <a name="related-topics"></a>相關主題

* [將工作項目提交至執行緒集區](submit-a-work-item-to-the-thread-pool.md)
* [使用執行緒集區的最佳做法](best-practices-for-using-the-thread-pool.md)
* [使用計時器提交工作項目](use-a-timer-to-submit-a-work-item.md)
 

 
