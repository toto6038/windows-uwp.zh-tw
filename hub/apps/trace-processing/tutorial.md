---
title: 存取追蹤資料-.NET TraceProcessing
description: 在本教學課程中，您將瞭解如何使用 TraceProcessor 存取追蹤資料。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: ef4d3df6e5a5dd93dbcb2caadc8e3f299aad581c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173692"
---
# <a name="access-trace-data"></a>存取追蹤資料

您可以使用下列套件識別碼，從 [NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) 取得 .net TraceProcessing：

EventTracing 處理。全部

此封裝可讓您存取追蹤檔中的資料。 如果您還沒有追蹤檔案，您可以使用 [Windows Performance Recorder](/windows-hardware/test/wpt/start-a-recording) 建立一個。

下列範例主控台應用程式顯示如何存取追蹤中包含的所有進程的命令列：

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using System;

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
            IPendingResult<IProcessDataSource> pendingProcessData = trace.UseProcesses();

            trace.Process();

            IProcessDataSource processData = pendingProcessData.Result;

            foreach (IProcess process in processData.Processes)
            {
                Console.WriteLine(process.CommandLine);
            }
        }
    }
}
```

## <a name="using-traceprocessor"></a>使用 TraceProcessor

若要處理追蹤，請呼叫[TraceProcessor。](/dotnet/api/microsoft.windows.eventtracing.traceprocessor.create) 核心介面是 [ITraceProcessor](/dotnet/api/microsoft.windows.eventtracing.itraceprocessor)，使用此介面包含下列模式：

1. 首先，告知處理器您要在追蹤中使用的資料
2. 其次，處理追蹤;和
3. 最後，存取結果。

告知處理器您想要的資料類型，表示您不需要花時間處理所有可能種類的追蹤資料。 相反地， [TraceProcessor](/dotnet/api/microsoft.windows.eventtracing.traceprocessor) 只會執行所需的工作，以提供您所要求的特定資料類型。

## <a name="recommended-project-settings"></a>建議的專案設定

使用 TraceProcessor 時，我們建議使用幾個專案設定：

1. 建議您以64位的形式執行 exe。

    新 c # .NET Framework 主控台應用程式的 Visual Studio 預設為已核取32位的任何 CPU。 .NET Core 的預設值可能已經有建議的設定。

    追蹤處理可能會耗用海量儲存體，尤其是針對較大的追蹤，我們建議您將平臺目標變更為 x64 (或取消核取在使用 TraceProcessor 的 exe 中偏好使用32位) 。 若要變更這些設定，請參閱專案的 [屬性] 底下的 [組建] 索引標籤。 若要變更所有設定的這些設定，請確定設定下拉式清單設定為 [所有設定]，而不是 [預設的目前設定]。

2. 建議您搭配使用 NuGet 與較新樣式的 PackageReference 模式，而不是舊版的 packages.config 模式。

    若要變更新專案的預設值，請參閱工具、NuGet 封裝管理員、封裝管理員設定套件管理、預設封裝管理格式。

## <a name="built-in-data-sources"></a>內建資料來源

Etl 檔案可以在追蹤中捕捉許多種類的資料。 請注意，etl 檔案中的哪些資料取決於在捕獲追蹤時啟用的提供者。 下列清單顯示可從 TraceProcessor 取得的追蹤資料類型：

| 程式碼                                      | 描述                                                                                                                | 相關的 WPA 專案                                                    |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| 跟蹤。UseClassicEvents ( # A1                  | 從追蹤（不包含架構資訊）提供傳統 ETW 事件。                                         | 當事件種類為傳統或 WPP 時，泛型事件資料表 ()              |
| 跟蹤。UseConnectedStandbyData ( # A1           | 提供有關系統進入和離開連線待命之追蹤的資料。                                        | CS 摘要資料表                                                     |
| 跟蹤。UseCpuIdleStates ( # A1                  | 從關於 CPU C 狀態的追蹤提供資料。                                                                             | 當類型為實際) 時，CPU 閒置狀態資料表 (                          |
| 跟蹤。UseCpuSamplingData ( # A1                | 根據指令指標的定期取樣，提供關於 CPU 使用量的追蹤資料。                          | CPU 使用量 (取樣) 資料表                                            |
| 跟蹤。UseCpuSchedulingData ( # A1              | 提供關於 CPU 執行緒排程的追蹤資料，包括內容切換和就緒執行緒事件。                | CPU 使用量 (精確) 資料表                                            |
| 跟蹤。UseDevicePowerData ( # A1                | 從有關裝置 D 狀態的追蹤提供資料。                                                                          | 裝置 DState 資料表                                                  |
| 跟蹤。UseDirectXData ( # A1                    | 提供有關 DirectX 活動的追蹤資料。                                                                         | GPU 使用率資料表                                                |
| traceUseDiskIOData ( # A1                      | 提供有關磁片 i/o 活動的追蹤資料。                                                                        | 磁片使用量表                                                     |
| 跟蹤。UseEnergyEstimationData ( # A1           | 從能源預估引擎的預估每一進程能源使用量，提供追蹤的資料。                         | 依進程) 資料表的能源估計引擎摘要 (                  |
| 跟蹤。UseEnergyMeterData ( # A1                | 提供有關從能源計量介面測量能源使用量 (EMI) 的追蹤資料。                                  | 由 Emi) 資料表 (的能源估計引擎                              |
| 跟蹤。UseFileIOData ( # A1                     | 提供有關檔案 i/o 活動的追蹤資料。                                                                        | 檔案 i/o 資料表                                                       |
| 跟蹤。UseGenericEvents ( # A1                  | 提供來自追蹤的資訊清單和 TraceLogging 事件。                                                                  | 當事件種類為 TraceLogging 時，泛型事件資料表 ()  |
| 跟蹤。UseHandles ( # A1                        | 提供有關使用中核心控制碼之追蹤的部分資料。                                                            | 處理資料表                                                        |
| 跟蹤。UseHardFaults ( # A1                     | 從追蹤中提供有關硬分頁錯誤的資料。                                                                         | 硬錯誤資料表                                                    |
| 跟蹤。UseHeapSnapshots ( # A1                  | 從追蹤提供有關進程堆積使用的資料。                                                                       | 堆積快照集資料表                                                  |
| 跟蹤。UseHypercalls ( # A1                     | 提供追蹤期間發生之 Hyper-v 虛擬化的相關資料。                                                        |                                                                      |
| 跟蹤。UseImageSections ( # A1                  | 提供有關影像區段的追蹤資料。                                                                 | CPU 使用量 () 資料表取樣的區段名稱資料行                 |
| 跟蹤。UseInterruptHandlingData ( # A1          | 提供有關插斷服務常式 (ISR) 和延後程序呼叫 (DPC) 活動的追蹤資料。               | DPC/ISR 資料表                                                        |
| 跟蹤。UseMarks ( # A1                          | 提供從追蹤)  (標示之時間戳記的標記。                                                                      | 標記資料表                                                          |
| 跟蹤。UseMemoryUtilizationData ( # A1          | 提供有關系統記憶體總使用率的追蹤資料。                                                          | 記憶體使用量資料表                                             |
| 跟蹤。UseMetadata ( # A1                       | 提供可用的追蹤中繼資料，而不需要進一步處理。                                                              | 系統組態、追蹤和一般                             |
| 跟蹤。UsePlatformIdleStates ( # A1             | 提供有關系統的目標和實際平臺閒置狀態的追蹤資料。                                   | 平臺閒置狀態資料表                                            |
| 跟蹤。UsePoolAllocations ( # A1                | 從追蹤提供有關核心集區記憶體使用量的資料。                                                                 | 集區摘要資料表                                                   |
| 跟蹤。UsePowerConfigurationData ( # A1         | 從追蹤提供關於系統電源設定的資料。                                                               | 系統組態，電源設定                                 |
| 跟蹤。UsePowerDependencyCoordinatorData ( # A1 | 從追蹤提供有關作用中電源相依性協調器階段的資料。                                               | 通知階段摘要資料表                                     |
| 跟蹤。UseProcesses ( # A1                      | 提供追蹤期間使用中之進程的相關資料，以及其映射和 Pdb。                                      | 進程資料表;Images 資料表;符號中樞                           |
| 跟蹤。UseProcessorCounters ( # A1              | 從處理器計數器監視器 (PCM) 的處理器效能計數器值的追蹤提供資料。                |                                                                      |
| 跟蹤。UseProcessorFrequencyData ( # A1         | 提供有關處理器執行頻率的追蹤資料。                                                    | 當類型為實際) 時的處理器頻率資料表 (                      |
| 跟蹤。UseProcessorProfileData ( # A1           | 提供有關使用中處理器電源設定檔的追蹤資料。                                                       | 處理器設定檔資料表                                             |
| 跟蹤。UseProcessorParkingData ( # A1           | 提供有關已暫止或已離開哪些處理器之追蹤的資料。                                                 | 處理器停車狀態資料表                                        |
| 跟蹤。UseProcessorParkingLimits ( # A1         | 提供有關允許的已離開處理器數目上限的追蹤資料。                                        | 核心暫止端點狀態資料表                                         |
| 跟蹤。UseProcessorQualityOfServiceData ( # A1  | 提供有關每個處理器之服務等級品質的追蹤資料。                                          | 處理器 Qos 類別表                                            |
| 跟蹤。UseProcessorThrottlingData ( # A1        | 提供有關處理器最大頻率節流的追蹤資料。                                                   | 處理器條件約束資料表                                          |
| 跟蹤。UseReadyBootData ( # A1                  | 從準備的開機中提供有關開機預先提取活動的資料。                                                | 就緒開機事件資料表                                              |
| 跟蹤。UseReferenceSetData ( # A1               | 提供有關每個處理常式所使用之虛擬記憶體頁面的追蹤資料。                                             | 參考集資料表                                                  |
| 跟蹤。UseRegionsOfInterest ( # A1              | 根據 xml 設定檔中指定的追蹤，從追蹤中提供感興趣間隔的命名區域。                       | 感興趣的區域資料表                                            |
| 跟蹤。UseRegistryData ( # A1                   | 提供追蹤期間登錄活動的相關資料。                                                                      | 登錄資料表                                                       |
| 跟蹤。UseResidentSetData ( # A1                | 針對駐留在實體記憶體中的每個處理常式，提供有關虛擬記憶體頁面的追蹤資料。       | 常駐集資料表                                                   |
| 跟蹤。UseRundownData ( # A1                    | 從追蹤中提供追蹤取消資料收集期間發生之間隔的資料。                            | 圖形時間軸中的陰影區域                                 |
| 跟蹤。UseScheduledTasks ( # A1                 | 提供追蹤期間所執行之排程工作的相關資料。                                                               | 排程的工作資料表                                                |
| 跟蹤。UseServices ( # A1                       | 提供有關服務的資料，這些服務是作用中或在追蹤期間捕捉的狀態。                                  | 服務資料表;系統組態，服務                       |
| 跟蹤。UseStacks ( # A1                         | 提供追蹤期間所記錄之堆疊的相關資料。                                                                        |                                                                      |
| 跟蹤。UseStackEvents ( # A1                    | 提供與追蹤期間所記錄之堆疊相關聯之事件的相關資料。                                                 | 堆疊資料表                                                         |
| 跟蹤。UseStackTags ( # A1                      | 提供對應程式，將追蹤中的堆疊分組為 XML 設定檔中所指定的堆疊標記。               | 堆疊標記和堆疊 (框架標記之類的資料行)                      |
| 跟蹤。UseSymbols ( # A1                        | 提供載入追蹤符號的功能。                                                                          | 設定符號路徑;載入符號                                 |
| 跟蹤。UseSyscalls ( # A1                       | 提供追蹤期間發生之 syscalls 的相關資料。                                                                 | Syscalls 資料表                                                       |
| 跟蹤。UseSystemMetadata ( # A1                 | 提供來自追蹤的一般全系統中繼資料。                                                                       | 系統設定                                                 |
| 跟蹤。UseSystemPowerSourceData ( # A1          | 提供有關使用中系統電源來源 (AC 與 DC) 的追蹤資料。                                                | 系統電源來源資料表                                            |
| 跟蹤。UseSystemSleepData ( # A1                | 提供有關整體系統電源狀態的追蹤資料。                                                               | 電源轉換表                                               |
| 跟蹤。UseTargetCpuIdleStates ( # A1            | 提供有關目標 CPU C 狀態的追蹤資料。                                                                      | 當類型為目標時，CPU 閒置狀態資料表 ()                           |
| 跟蹤。UseTargetProcessorFrequencyData ( # A1   | 提供有關目標處理器頻率的追蹤資料。                                                             | 當類型為目標時，處理器頻率資料表 ()                       |
| 跟蹤。UseThreads ( # A1                        | 提供追蹤期間使用中線程的相關資料。                                                                         | 執行緒存留期資料表                                               |
| 跟蹤。UseTraceStatistics ( # A1                | 提供追蹤中事件的相關統計資料。                                                                           | 系統組態、追蹤統計資料                               |
| 跟蹤。UseUtcData ( # A1                        | 使用通用遙測用戶端 (UTC) ，從有關 Microsoft 遙測活動的追蹤中提供資料。                      | Utc 資料表                                                            |
| 跟蹤。UseWindowInFocus ( # A1                  | 從追蹤中提供有關作用中 UI 視窗變更的追蹤資料。                                                 | 焦點資料表中的視窗                                                |
| 跟蹤。UseWindowsTracePreprocessorEvents ( # A1 | 從追蹤提供 Windows 軟體追蹤預處理器 (WPP) 事件。                                                    | WPP 追蹤資料表;當事件種類為 WPP 時，泛型事件資料表 ()        |
| 跟蹤。UseWinINetData ( # A1                    | 透過 Windows Internet (WinINet) ，從有關網際網路活動的追蹤中提供資料。                                         | 下載詳細資料表格                                               |
| 跟蹤。UseWorkingSetData ( # A1                 | 針對每個進程或核心類別之工作集中的虛擬記憶體頁面，提供追蹤的資料。 | 虛擬記憶體快照集資料表                                       |

另請參閱 [ITraceSource](/dotnet/api/microsoft.windows.eventtracing.itracesource) 上適用于所有可用追蹤資料的擴充方法，或檢查 "trace" 提供的方法。 由 IntelliSense 顯示。

## <a name="next-steps"></a>後續步驟

在此總覽中，您已瞭解如何使用 TraceProcessor 和它可以存取的內建資料來源來存取追蹤資料。

下一步是瞭解如何 [擴充](extensibility.md) TraceProcessor 以存取自訂追蹤資料。