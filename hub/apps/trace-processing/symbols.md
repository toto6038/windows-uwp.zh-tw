---
title: 載入符號-.NET TraceProcessing
description: 在本教學課程中，您將瞭解如何在處理追蹤時載入符號。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: a6954538159c6fffb3185aa8b3137af26e17b32f
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629089"
---
# <a name="use-symbols-in-net-traceprocessing"></a>在 .NET TraceProcessing 中使用符號

[TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor)支援從數個數據源載入符號及取得堆疊。 下列主控台應用程式會查看 CPU 樣本，並輸出特定函式執行的預估持續時間（根據追蹤的 CPU 使用量統計取樣）。

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

（輸出詳細資料會依追蹤而有所不同）。

## <a name="symbols-format"></a>符號格式

就內部而言，TraceProcessor 會使用[SymCache](https://docs.microsoft.com/windows-hardware/test/wpt/loading-symbols#symcache-path)格式，這是儲存在 PDB 中部分資料的快取。 載入符號時，TraceProcessor 需要指定要用於這些 SymCache 檔（SymCache 路徑）的位置，並可選擇性地指定要存取 Pdb 的 SymbolPath。 當提供 SymbolPath 時，TraceProcessor 會視需要從 PDB 檔案建立 SymCache 檔案，而後續處理相同的資料可以直接使用 SymCache 檔案以獲得更好的效能。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已瞭解如何在處理追蹤時載入符號。

下一個步驟是瞭解如何[使用串流](streaming.md)來存取追蹤資料，而不需要緩衝處理記憶體中的所有專案。
