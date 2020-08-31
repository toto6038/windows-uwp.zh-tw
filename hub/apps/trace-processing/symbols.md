---
title: 載入符號-.NET TraceProcessing
description: 在本教學課程中，您將瞭解如何在處理追蹤時載入符號。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 72264b51edcc0b02aa335b8766100c196a0d5090
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168812"
---
# <a name="use-symbols-in-net-traceprocessing"></a>在 .NET TraceProcessing 中使用符號

[TraceProcessor](/dotnet/api/microsoft.windows.eventtracing.traceprocessor) 支援從數個數據源載入符號和取得堆疊。 下列主控台應用程式會查看 CPU 範例，並輸出特定函式執行的預估持續時間 (依據 CPU 使用量) 的追蹤統計取樣。

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Cpu;
using Microsoft.Windows.EventTracing.Symbols;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 3)
        {
            Console.Error.WriteLine("Usage: GetCpuSampleDuration.exe <trace.etl> <imageName> <functionName>");
            return;
        }

        string tracePath = args[0];
        string imageName = args[1];
        string functionName = args[2];
        Dictionary<string, Duration> matchDurationByCommandLine = new Dictionary<string, Duration>();

        using (ITraceProcessor trace = TraceProcessor.Create(tracePath))
        {
            IPendingResult<ISymbolDataSource> pendingSymbolData = trace.UseSymbols();
            IPendingResult<ICpuSampleDataSource> pendingCpuSamplingData = trace.UseCpuSamplingData();

            trace.Process();

            ISymbolDataSource symbolData = pendingSymbolData.Result;
            ICpuSampleDataSource cpuSamplingData = pendingCpuSamplingData.Result;

            symbolData.LoadSymbolsForConsoleAsync(SymCachePath.Automatic, SymbolPath.Automatic).GetAwaiter().GetResult();

            Console.WriteLine();
            IThreadStackPattern pattern = AnalyzerThreadStackPattern.Parse($"{imageName}!{functionName}");

            foreach (ICpuSample sample in cpuSamplingData.Samples)
            {
                if (sample.Stack != null && sample.Stack.Matches(pattern))
                {
                    string commandLine = sample.Process.CommandLine;

                    if (!matchDurationByCommandLine.ContainsKey(commandLine))
                    {
                        matchDurationByCommandLine.Add(commandLine, Duration.Zero);
                    }

                    matchDurationByCommandLine[commandLine] += sample.Weight;
                }
            }

            foreach (string commandLine in matchDurationByCommandLine.Keys)
            {
                Console.WriteLine($"{commandLine}: {matchDurationByCommandLine[commandLine]}");
            }
        }
    }
}
```

執行此程式會產生類似下列的輸出：

```shell
C:\GetCpuSampleDuration\bin\Debug\> GetCpuSampleDuration.exe C:\boot.etl user32.dll LoadImageInternal
0.0% (0 of 1165; 0 loaded)
<snip>
100.0% (1165 of 1165; 791 loaded)
wininit.exe: 15.99 ms
C:\Windows\Explorer.EXE: 5 ms
winlogon.exe: 20.15 ms
"C:\Users\AdminUAC\AppData\Local\Microsoft\OneDrive\OneDrive.exe" /background: 2.09 ms
```

 (輸出詳細資料會根據追蹤) 而有所不同。

## <a name="symbols-format"></a>符號格式

就內部而言，TraceProcessor 會使用 [SymCache](/windows-hardware/test/wpt/loading-symbols#symcache-path) 格式，也就是儲存在 PDB 中的部分資料快取。 載入符號時，TraceProcessor 需要指定要用於這些 SymCache 檔 (SymCache 路徑的位置) ，並支援選擇性地指定 SymbolPath 來存取 Pdb。 提供 SymbolPath 時，TraceProcessor 會視需要建立 PDB 檔案中的 SymCache 檔案，而後續的相同資料處理也可以直接使用 SymCache 檔案，以提升效能。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已瞭解如何在處理追蹤時載入符號。

下一步是瞭解如何 [使用串流](streaming.md) 來存取追蹤資料，而不需要緩衝處理記憶體中的所有專案。