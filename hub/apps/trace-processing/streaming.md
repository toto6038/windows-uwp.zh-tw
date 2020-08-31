---
title: 使用串流-.NET TraceProcessing
description: 在本教學課程中，您將瞭解如何使用串流立即存取追蹤資料，並使用較少的記憶體。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 6ad0f5977ed4d739ce3133c9e67c0eefc6e0cbd9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168822"
---
# <a name="use-streaming-with-traceprocessor"></a>搭配使用串流與 TraceProcessor

根據預設，TraceProcessor 會在處理追蹤時將資料載入記憶體中，藉以存取資料。 這種緩衝處理方法很容易使用，但在記憶體使用量方面可能很耗費資源。

TraceProcessor 也提供追蹤。UseStreaming ( # A1，可支援以資料流程方式存取多種類型的追蹤資料， (處理從追蹤檔案讀取的資料，而不是在記憶體) 中緩衝處理資料。 例如，syscalls 追蹤可能很大，而且在追蹤中緩衝處理整個 syscalls 清單可能相當昂貴。

## <a name="accessing-buffered-data"></a>存取緩衝的資料

下列程式碼示範如何透過追蹤以正常、緩衝的方式存取 syscall 資料。UseSyscalls ( # A1：

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 1)
        {
            Console.Error.WriteLine("Usage: <trace.etl>");
            return;
        }

        using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
        {
            IPendingResult<ISyscallDataSource> pendingSyscallData = trace.UseSyscalls();

            trace.Process();

            ISyscallDataSource syscallData = pendingSyscallData.Result;

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            foreach (ISyscall syscall in syscallData.Syscalls)
            {
                IProcess process = syscall.Thread?.Process;

                if (process == null)
                {
                    continue;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            }

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="accessing-streaming-data"></a>存取串流資料

使用大型 syscalls 追蹤時，嘗試緩衝處理記憶體中的 syscall 資料可能會相當昂貴，或甚至可能無法進行。 下列程式碼示範如何以串流方式存取相同的 syscall 資料，並取代 trace。UseSyscalls ( # A1 with trace。UseStreaming ( # A3。UseSyscalls ( # A5：

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 1)
        {
            Console.Error.WriteLine("Usage: <trace.etl>");
            return;
        }

        using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
        {
            IPendingResult<IThreadDataSource> pendingThreadData = trace.UseThreads();

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            trace.UseStreaming().UseSyscalls(ConsumerSchedule.SecondPass, context =>
            {
                Syscall syscall = context.Data;
                IProcess process = syscall.GetThread(pendingThreadData.Result)?.Process;

                if (process == null)
                {
                    return;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            });

            trace.Process();

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="how-streaming-works"></a>串流的運作方式

根據預設，所有的串流資料會在第一次通過追蹤時提供，而來自其他來源的緩衝資料則無法使用。 上述範例顯示如何結合串流與緩衝處理，執行緒資料會在資料串流處理之前先經過緩衝處理。 因此，追蹤必須讀取兩次（一次是取得緩衝的執行緒資料），而第二次使用已緩衝處理的執行緒資料來存取串流的 syscall 資料。 為了以這種方式結合串流和緩衝處理，此範例會將 ConsumerSchedule 傳送至追蹤。UseStreaming ( # A1。UseSyscalls ( # A3，會導致在第二次通過追蹤時發生 syscall 處理。 在第二個階段中執行時，syscall 回呼可以從追蹤存取暫止的結果。UseThreads 在處理每個 syscall 時 ( # A1。 如果沒有這個選擇性引數，則會在第一次通過追蹤時執行 syscall 串流 (只有一個傳遞) ，以及追蹤的暫止結果。UseThreads ( # A3 還無法使用。 在這種情況下，回呼仍可以從 syscall 存取 ThreadId，但它無法存取執行緒的進程 (因為處理連結資料的執行緒是透過其他事件提供，這些事件可能尚未經過處理) 。

緩衝處理和串流處理之間的一些主要差異：

1. 緩衝會傳回[IPendingResult &lt; T &gt; ](/dotnet/api/microsoft.windows.eventtracing.ipendingresult-1)，而且只有在處理追蹤之前，它所保存的結果才可供使用。 處理追蹤之後，您可以使用 foreach 和 LINQ 之類的技術來列舉結果。
2. 串流會傳回 void，並改為使用回呼引數。 當每個專案都可供使用時，它就會呼叫回呼一次。 因為資料不會進行緩衝處理，所以永遠不會有要使用 foreach 或 LINQ 列舉的結果清單。串流回呼需要在處理完成之後，緩衝處理要儲存之資料的哪個部分以供使用。
3. 處理緩衝資料的程式碼會在呼叫追蹤之後出現。當擱置的結果可用時，進程 ( # A1。
4. 處理串流資料的程式碼會在呼叫追蹤之前出現。進程 ( # A1，作為追蹤的回呼。UseStreaming。請使用 ... ( # A3 方法。
5. 串流取用者可以選擇只處理部分的資料流程，並藉由呼叫內容來取消未來的回呼。取消 ( # A1。 緩衝取用者一律會提供完整、緩衝的清單。

## <a name="correlated-streaming-data"></a>相互關聯的串流資料

有時追蹤資料會出現在一連串的事件中-例如，syscalls 是透過個別的進入和結束事件來記錄，但是這兩個事件的結合資料會更有説明。 方法追蹤。UseStreaming ( # A1。UseSyscalls ( # A3 會將這兩個事件的資料相互關聯，並在配對變成可用時加以提供。 您可以透過追蹤來取得一些相互關聯資料類型。UseStreaming ( # A1：

| 程式碼                                        | 描述                                                                                                                                     |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| 跟蹤。UseStreaming ( # A1。UseCoNtextSwitchData ( # A3 | 從 compact 和非 compact 事件 (串流相互關聯的內容切換資料，並以比原始非 compact 事件) 更精確的 SwitchInThreadIds。 |
| 跟蹤。UseStreaming ( # A1。UseScheduledTasks ( # A3    | 串流相關聯的排程工作資料。                                                                                                         |
| 跟蹤。UseStreaming ( # A1。UseSyscalls ( # A3          | 串流相互關聯的系統呼叫資料。                                                                                                            |
| 跟蹤。UseStreaming ( # A1。UseWindowInFocus ( # A3     | 串流相關的視窗焦點資料。                                                                                                        |

## <a name="standalone-streaming-events"></a>獨立串流事件

此外，追蹤。UseStreaming ( # A1 提供許多不同獨立事件種類的剖析事件：

| 程式碼                                               | 描述                                     |
|----------------------------------------------------|-------------------------------------------------|
| 跟蹤。UseStreaming ( # A1。UseLastBranchRecordEvents ( # A3   | 剖析最後一個分支記錄 (LBR) 事件的資料流程。 |
| 跟蹤。UseStreaming ( # A1。UseReadyThreadEvents ( # A3        | 串流剖析的就緒執行緒事件。             |
| 跟蹤。UseStreaming ( # A1。UseThreadCreateEvents ( # A3       | 串流剖析的執行緒建立事件。            |
| 跟蹤。UseStreaming ( # A1。UseThreadExitEvents ( # A3         | 串流剖析的執行緒結束事件。              |
| 跟蹤。UseStreaming ( # A1。UseThreadRundownStartEvents ( # A3 | 資料流程剖析執行緒取消開始事件。     |
| 跟蹤。UseStreaming ( # A1。UseThreadRundownStopEvents ( # A3  | 串流剖析的執行緒取消事件。      |
| 跟蹤。UseStreaming ( # A1。UseThreadSetNameEvents ( # A3      | 串流剖析的執行緒集名稱事件。          |

## <a name="underlying-streaming-events-for-correlated-data"></a>相互關聯資料的基礎串流事件

最後，追蹤。UseStreaming ( # A1 也提供用來將上述清單中的資料相互關聯的基礎事件。 這些基礎事件如下：

| 程式碼                                                        | 描述                                                                                | 包含在                                 |
|-------------------------------------------------------------|--------------------------------------------------------------------------------------------|---------------------------------------------|
| 跟蹤。UseStreaming ( # A1。UseCompactCoNtextSwitchEvents ( # A3        | 串流剖析的 compact 內容切換事件。                                              | 跟蹤。UseStreaming ( # A1。UseCoNtextSwitchData ( # A3 |
| 跟蹤。UseStreaming ( # A1。UseCoNtextSwitchEvents ( # A3               | 串流剖析的內容切換事件。 在某些情況下，SwitchInThreadIds 可能不准確。 | 跟蹤。UseStreaming ( # A1。UseCoNtextSwitchData ( # A3 |
| 跟蹤。UseStreaming ( # A1。UseFocusChangeEvents ( # A3                 | 串流剖析的視窗焦點變更事件。                                                 | 跟蹤。UseStreaming ( # A1。UseWindowInFocus ( # A3     |
| 跟蹤。UseStreaming ( # A1。UseScheduledTaskStartEvents ( # A3          | 串流剖析的排程工作開始事件。                                                | 跟蹤。UseStreaming ( # A1。UseScheduledTasks ( # A3    |
| 跟蹤。UseStreaming ( # A1。UseScheduledTaskStopEvents ( # A3           | 串流剖析的排程工作停止事件。                                                 | 跟蹤。UseStreaming ( # A1。UseScheduledTasks ( # A3    |
| 跟蹤。UseStreaming ( # A1。UseScheduledTaskTriggerEvents ( # A3        | 串流剖析的排程工作觸發程式事件。                                              | 跟蹤。UseStreaming ( # A1。UseScheduledTasks ( # A3    |
| 跟蹤。UseStreaming ( # A1。UseSessionLayerSetActiveWindowEvents ( # A3 | 串流剖析的工作階段層設定使用中視窗事件。                                     | 跟蹤。UseStreaming ( # A1。UseWindowInFocus ( # A3     |
| 跟蹤。UseStreaming ( # A1。UseSyscallEnterEvents ( # A3                | 剖析的 syscall 進入事件的資料流程。                                                       | 跟蹤。UseStreaming ( # A1。UseSyscalls ( # A3          |
| 跟蹤。UseStreaming ( # A1。UseSyscallExitEvents ( # A3                 | 串流剖析的 syscall 結束事件。                                                        | 跟蹤。UseStreaming ( # A1。UseSyscalls ( # A3          |

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已瞭解如何使用串流立即存取追蹤資料，並使用較少的記憶體。

下一步是查看從您的追蹤存取您想要的資料。 查看 [範例](https://github.com/microsoft/eventtracing-processing-samples) 以瞭解一些想法。 請注意，並非所有追蹤都包含所有支援的資料類型。