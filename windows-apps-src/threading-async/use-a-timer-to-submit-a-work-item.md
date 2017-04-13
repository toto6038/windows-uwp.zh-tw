---
author: TylerMSFT
ms.assetid: AAE467F9-B3C7-4366-99A2-8A880E5692BE
title: "使用計時器提交工作項目"
description: "了解如何建立在計時器過後執行的工作項目。"
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 計時器, 執行緒"
ms.openlocfilehash: 7148e14734f54c24275c4127801b487e5862bba9
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="use-a-timer-to-submit-a-work-item"></a>使用計時器提交工作項目

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** 重要 API **

-   [**Windows.UI.Core 命名空間**](https://msdn.microsoft.com/library/windows/apps/BR208383)
-   [**Windows.System.Threading 命名空間**](https://msdn.microsoft.com/library/windows/apps/BR229642)

了解如何建立在計時器過後執行的工作項目。

## <a name="create-a-single-shot-timer"></a>建立單次計時器

使用 [**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921) 方法建立工作項目的計時器。 提供完成工作的 Lambda，以及使用 *delay* 參數指定執行緒集區需要等待多久的時間，才能將工作項目指派給可用的執行緒。 延遲使用 [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/BR225996) 結構指定。

> **注意**：您可以使用 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) 從工作項目存取 UI 及顯示進度。

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

如有需要，可使用 [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926) 處理取消及完成工作項目。 使用 [**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921) 超載提供其他 Lambda。 這會在取消計時器或工作項目完成時執行。

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

如果計時器仍在倒數計時，但已不再需要該工作項目，可呼叫 [**Cancel**](https://msdn.microsoft.com/library/windows/apps/BR230588)。 就會取消計時器，而且不會將工作項目提交至執行緒集區。

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> DelayTimer.Cancel();
> ```
> ``` cpp
> DelayTimer->Cancel();
> ```

## <a name="remarks"></a>備註

通用 Windows 平台 (UWP) app 無法使用 **Thread.Sleep**，因為它會封鎖 UI 執行緒。 您可以改為使用 [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587) 來建立工作項目，這將會延遲由該工作項目完成的工作，而且不會封鎖 UI 執行緒。

如需示範工作項目、計時器工作項目以及定期工作項目的完整程式碼範例，請參閱[執行緒集區範例](http://go.microsoft.com/fwlink/p/?linkid=255387)。 程式碼範例原本是針對 Windows 8.1 而撰寫的，但程式碼可以在 Windows 10 中重複使用。

如需重複計時器的詳細資訊，請參閱[建立定期工作項目](create-a-periodic-work-item.md)。

## <a name="related-topics"></a>相關主題

* [將工作項目提交至執行緒集區](submit-a-work-item-to-the-thread-pool.md)
* [使用執行緒集區的最佳做法](best-practices-for-using-the-thread-pool.md)
* [使用計時器提交工作項目](use-a-timer-to-submit-a-work-item.md)
 

 
