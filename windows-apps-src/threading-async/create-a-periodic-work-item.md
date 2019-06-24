---
ms.assetid: 1B077801-0A58-4A34-887C-F1E85E9A37B0
title: 建立定期工作項目
description: 了解如何建立定期重複執行的工作項目。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 定期工作項目, 執行緒, 計時器
ms.localizationpriority: medium
ms.openlocfilehash: 8e045c12f96cc9404abb4ba9be395eb49b1ab850
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322003"
---
# <a name="create-a-periodic-work-item"></a>建立定期工作項目


<b>重要的 Api</b>

-   [**CreatePeriodicTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer)
-   [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer)

了解如何建立定期重複執行的工作項目。

## <a name="create-the-periodic-work-item"></a>建立定期工作項目

使用 [**CreatePeriodicTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer) 方法來建立定期工作項目。 提供可完成工作的 Lambda，並且使用 *period* 參數指定提交間隔。 期間使用 [**TimeSpan**](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan) 結構指定。 每次期間過後工作項目就會重新提交，因此請確定期間長度足以完成工作。

[**CreateTimer** ](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer)會傳回[ **ThreadPoolTimer** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer)物件。 請將這個物件儲存起來，以防需要取消計時器。

> **附註**  避免指定的值為零 （或任何值小於一毫秒） 的間隔。 這會讓定期計時器變得像是單次計時器。

> **附註**  您可以使用[ **CoreDispatcher.RunAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync)存取 UI，並顯示工作項目中的進度。

下列範例會建立一個每隔 60 秒執行一次的工作項目：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> TimeSpan period = TimeSpan.FromSeconds(60);
>
> ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, period);
> ```
> ``` cpp
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
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
>                 }));
>
>         }), period);
> ```

## <a name="handle-cancellation-of-the-periodic-work-item-optional"></a>處理定期工作項目的取消 (選用)

如有必要，您可以使用 [**TimerDestroyedHandler**](https://docs.microsoft.com/uwp/api/windows.system.threading.timerdestroyedhandler) 處理定期計時器的取消。 使用 [**CreatePeriodicTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer) 超載提供其他可處理定期工作項目取消的 Lambda。

下列範例會建立一個每 60 秒重複一次的定期工作項目，同時提供取消處理常式：

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> using Windows.System.Threading;
>
>     TimeSpan period = TimeSpan.FromSeconds(60);
>
>     ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>     },
>     period,
>     (source) =>
>     {
>         //
>         // TODO: Handle periodic timer cancellation.
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher->RunAsync(CoreDispatcherPriority.High,
>             ()=>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //                 
>
>                 // Periodic timer cancelled.
>
>             }));
>     });
> ```
> ``` cpp
> using namespace Windows::System::Threading;
> using namespace Windows::UI::Core;
>
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
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
>                 }));
>
>         }),
>         period,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle periodic timer cancellation.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                     // Periodic timer cancelled.
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>取消計時器

必要時，可呼叫 [**Cancel**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.cancel) 方法來停止定期工作項目避免重複。 如果在工作項目執行中取消定期計時器，該工作項目還是可以繼續完成。 所有定期工作項目的執行個體都完成時，就會呼叫 [**TimerDestroyedHandler**](https://docs.microsoft.com/uwp/api/windows.system.threading.timerdestroyedhandler) (若提供的話)。

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> PeriodicTimer.Cancel();
> ```
> ``` cpp
> PeriodicTimer->Cancel();
> ```

## <a name="remarks"></a>備註

如需單次計時器的詳細資訊，請參閱[使用計時器提交工作項目](use-a-timer-to-submit-a-work-item.md)。

## <a name="related-topics"></a>相關主題

* [將工作項目提交至執行緒集區](submit-a-work-item-to-the-thread-pool.md)
* [使用執行緒集區的最佳做法](best-practices-for-using-the-thread-pool.md)
* [使用計時器提交工作項目](use-a-timer-to-submit-a-work-item.md)
 
