---
title: 使用串流-.NET TraceProcessing
description: 在本教學課程中，您將瞭解如何使用串流來立即存取追蹤資料，並使用較少的記憶體。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: e04f306a6a5c03d1f502b9cfb6c2cbb737e0098f
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629078"
---
# <a name="use-streaming-with-traceprocessor"></a>搭配 TraceProcessor 使用串流

根據預設，TraceProcessor 會在處理追蹤時，將資料載入記憶體中來存取資料。 這種緩衝處理方法很容易使用，但在記憶體使用量方面可能會很耗費資源。

TraceProcessor 也提供追蹤。UseStreaming （），支援以資料流程方式存取多種類型的追蹤資料（處理從追蹤檔案讀取的資料，而不是在記憶體中緩衝資料）。 例如，syscalls 追蹤可能相當大，而在追蹤中緩衝整個 syscalls 清單可能相當耗費資源。

## <a name="accessing-buffered-data"></a>存取緩衝的資料

下列程式碼示範如何透過追蹤以標準、緩衝的方式存取 syscall 資料。UseSyscalls():

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

使用大型的 syscalls 追蹤時，嘗試緩衝處理記憶體中的 syscall 資料可能會相當耗費資源，或甚至可能無法這麼做。 下列程式碼顯示如何以資料流程方式存取相同的 syscall 資料，並取代 trace。具有 trace 的 UseSyscalls （）。UseStreaming().UseSyscalls():

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

根據預設，在第一次通過追蹤時，會提供所有串流資料，而來自其他來源的緩衝資料則無法使用。 上述範例顯示如何結合串流與緩衝處理–執行緒資料會在資料流程資料流程之前進行緩衝處理。 因此，追蹤必須讀取兩次-一次以取得緩衝的執行緒資料，而第二次使用已緩衝處理的執行緒資料來存取串流的 syscall 資料。 為了以這種方式結合資料流程和緩衝處理，此範例會將 ConsumerSchedule. SecondPass 傳遞至 trace。UseStreaming().UseSyscalls （），這會使 syscall 處理在第二次通過追蹤時發生。 藉由在第二個階段中執行，syscall 回呼可以從追蹤存取暫止的結果。處理每個 syscall 時的 UseThreads （）。 如果沒有這個選擇性的引數，則 syscall 串流會在第一次通過追蹤時執行（只會有一次傳遞），以及追蹤的暫止結果。UseThreads （）尚無法使用。 在這種情況下，回呼仍然可以從 syscall 存取 ThreadId，但是它無法存取執行緒的進程（因為處理連結資料的執行緒是透過其他可能尚未處理的事件來提供）。

緩衝和串流處理之間的一些主要差異：

1. 緩衝會傳回[IPendingResult&lt;t&gt;](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.ipendingresult-1)，而且它所保留的結果只有在處理追蹤之前才可使用。 在處理追蹤之後，您可以使用 foreach 和 LINQ 之類的技巧來列舉結果。
2. 資料流程會傳回 void，並改為採用回呼引數。 當每個專案都可供使用時，它會呼叫回呼一次。 因為資料未經過緩衝處理，所以永遠不會有要使用 foreach 或 LINQ 列舉的結果清單–串流回呼必須緩衝處理完成後要儲存以供使用的任何資料部分。
3. 處理緩衝資料的程式碼會在呼叫 trace 之後出現。進程（），當擱置的結果可用時。
4. 處理串流資料的程式碼會出現在追蹤的呼叫之前。進程（），作為追蹤的回呼。UseStreaming。使用 .。。（）方法。
5. 串流取用者可以選擇只處理部分資料流程，並藉由呼叫內容來取消未來的回呼。取消（）。 緩衝取用者一律會提供完整的緩衝清單。

## <a name="correlated-streaming-data"></a>相互關聯的串流資料

有時追蹤資料會出現在一連串的事件中，例如，syscalls 是透過個別的進入和結束事件來記錄，但這兩個事件的結合資料可能更有説明。 方法追蹤。UseStreaming().UseSyscalls （）會將這兩個事件的資料相互關聯，並在配對可供使用時提供。 有幾種類型的相互關聯資料可透過追蹤來取得。UseStreaming():

| 程式碼                                        | 描述                                                                                                                                     |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| 追蹤.UseStreaming().UseCoNtextSwitchData() | 資料流程相互關聯的內容切換資料（從精簡和非 compact 事件，SwitchInThreadIds 比原始非 compact 事件更精確）。 |
| 追蹤.UseStreaming().UseScheduledTasks()    | 串流相互關聯的排程工作資料。                                                                                                         |
| 追蹤.UseStreaming().UseSyscalls()          | 串流相互關聯的系統呼叫資料。                                                                                                            |
| 追蹤.UseStreaming().UseWindowInFocus()     | 串流相互關聯的視窗-焦點資料。                                                                                                        |

## <a name="standalone-streaming-events"></a>獨立串流事件

此外，追蹤。UseStreaming （）會針對數個不同的獨立事件種類提供剖析的事件：

| 程式碼                                               | 描述                                     |
|----------------------------------------------------|-------------------------------------------------|
| 追蹤.UseStreaming().UseLastBranchRecordEvents()   | 串流剖析的最後一個分支記錄（LBR）事件。 |
| 追蹤.UseStreaming().UseReadyThreadEvents()        | 資料流程已剖析已準備好的執行緒事件。             |
| 追蹤.UseStreaming().UseThreadCreateEvents()       | 串流剖析的執行緒建立事件。            |
| 追蹤.UseStreaming().UseThreadExitEvents()         | 資料流程剖析的執行緒結束事件。              |
| 追蹤.UseStreaming().UseThreadRundownStartEvents() | 串流剖析的執行緒取消啟動事件。     |
| 追蹤.UseStreaming().UseThreadRundownStopEvents()  | 串流剖析的執行緒取消停止事件。      |
| 追蹤.UseStreaming().UseThreadSetNameEvents()      | 資料流程剖析的執行緒集名稱事件。          |

## <a name="underlying-streaming-events-for-correlated-data"></a>相互關聯資料的基礎串流事件

最後，追蹤。UseStreaming （）也會提供用來將上述清單中的資料相互關聯的基礎事件。 這些基礎事件如下：

| 程式碼                                                        | 描述                                                                                | 包含於                                 |
|-------------------------------------------------------------|--------------------------------------------------------------------------------------------|---------------------------------------------|
| 追蹤.UseStreaming().UseCompactCoNtextSwitchEvents()        | 串流剖析的精簡內容切換事件。                                              | 追蹤.UseStreaming().UseCoNtextSwitchData() |
| 追蹤.UseStreaming().UseCoNtextSwitchEvents()               | 資料流程剖析的內容切換事件。 在某些情況下，SwitchInThreadIds 可能不精確。 | 追蹤.UseStreaming().UseCoNtextSwitchData() |
| 追蹤.UseStreaming().UseFocusChangeEvents()                 | 串流分析的視窗焦點變更事件。                                                 | 追蹤.UseStreaming().UseWindowInFocus()     |
| 追蹤.UseStreaming().UseScheduledTaskStartEvents()          | 串流已剖析的排程工作啟動事件。                                                | 追蹤.UseStreaming().UseScheduledTasks()    |
| 追蹤.UseStreaming().UseScheduledTaskStopEvents()           | 已剖析已排程工作停止事件的資料流程。                                                 | 追蹤.UseStreaming().UseScheduledTasks()    |
| 追蹤.UseStreaming().UseScheduledTaskTriggerEvents()        | 已剖析已排程工作觸發程式事件的資料流程。                                              | 追蹤.UseStreaming().UseScheduledTasks()    |
| 追蹤.UseStreaming().UseSessionLayerSetActiveWindowEvents() | 資料流程剖析的工作階段層集使用中視窗事件。                                     | 追蹤.UseStreaming().UseWindowInFocus()     |
| 追蹤.UseStreaming().UseSyscallEnterEvents()                | 串流剖析的 syscall 輸入事件。                                                       | 追蹤.UseStreaming().UseSyscalls()          |
| 追蹤.UseStreaming().UseSyscallExitEvents()                 | 串流剖析的 syscall 結束事件。                                                        | 追蹤.UseStreaming().UseSyscalls()          |

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已瞭解如何使用串流立即存取追蹤資料，並使用較少的記憶體。

下一個步驟是從您的追蹤中，尋找您想要的資料。 請參閱[範例](https://github.com/microsoft/eventtracing-processing-samples)以瞭解一些概念。 請注意，並非所有追蹤都包含所有支援的資料類型。
