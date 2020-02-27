---
title: 存取追蹤資料-.NET TraceProcessing
description: 在本教學課程中，您將瞭解如何使用 TraceProcessor 存取追蹤資料。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 170a8c3084e180714a319d67dca2b6a5756ea474
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629108"
---
# <a name="access-trace-data"></a>存取追蹤資料

您可以從[NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All)使用下列套件識別碼來取得 .net TraceProcessing：

EventTracing 處理。全部

此封裝可讓您存取追蹤檔案中的資料。 如果您還沒有追蹤檔案，可以使用[Windows Performance 錄製](https://docs.microsoft.com/windows-hardware/test/wpt/start-a-recording)器來建立一個。

下列範例主控台應用程式顯示如何存取追蹤中包含之所有進程的命令列：

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

若要處理追蹤，請呼叫[TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor.create)。 核心介面為[ITraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.itraceprocessor)，且使用此介面包含下列模式：

1. 首先，告訴處理器您想要從追蹤使用哪些資料
2. 第二，處理追蹤;和
3. 最後，存取結果。

告訴處理器您最前面想要的資料種類，表示您不需要花時間處理大量可能的追蹤資料類型。 相反地， [TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor)只會執行所需的工作，以提供您所要求的特定資料類型。

## <a name="recommended-project-settings"></a>建議的專案設定

我們建議您在 TraceProcessor 中使用幾個專案設定：

1. 我們建議您以64位執行 exe。

    新C# .NET Framework 主控台應用程式的 Visual Studio 預設值是已核取 [使用32位的任何 CPU]。 .NET Core 的預設值可能已經有建議的設定。

    追蹤處理可能會耗用海量儲存體，特別是使用較大的追蹤，建議您在使用 TraceProcessor 的 exe 中，將平臺目標變更為 x64 （或取消核取 [偏好32位]）。 若要變更這些設定，請參閱專案 [屬性] 底下的 [組建] 索引標籤。 若要變更所有設定的這些設定，請確定 [設定] 下拉式清單已設為 [所有設定]，而不是 [僅限目前設定] 的預設值。

2. 我們建議使用 NuGet 搭配較新樣式的 PackageReference 模式，而不是較舊的封裝 .config 模式。

    若要變更新專案的預設值，請參閱 [工具]、[NuGet 套件管理員]、[套件管理員設定]、[套件管理、預設套件管理格式]。

## <a name="built-in-data-sources"></a>內建資料來源

.Etl 檔案可以在追蹤中捕捉許多種類的資料。 請注意，哪些資料是在 .etl 檔案中，取決於捕獲追蹤時啟用的提供者。 下列清單顯示可從 TraceProcessor 取得的追蹤資料類型：

| 程式碼                                      | 描述                                                                                                                | 相關的 WPA 專案                                                    |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| 追蹤.UseClassicEvents()                  | 提供來自追蹤的傳統 ETW 事件，其中不包含架構資訊。                                         | 一般事件資料表（當事件種類為 [傳統] 或 [WPP] 時）             |
| 追蹤.UseConnectedStandbyData()           | 提供有關系統輸入和結束連線待命的追蹤資料。                                        | CS 摘要表                                                     |
| 追蹤.UseCpuIdleStates()                  | 提供有關 CPU C 狀態的追蹤資料。                                                                             | CPU 閒置狀態資料表（當類型為實際時）                          |
| 追蹤.UseCpuSamplingData()                | 根據指令指標的定期取樣，從關於 CPU 使用量的追蹤中提供資料。                          | CPU 使用量（取樣）資料表                                            |
| 追蹤.UseCpuSchedulingData()              | 提供有關 CPU 執行緒排程的追蹤資料，包括內容切換和就緒執行緒事件。                | CPU 使用量（精確）資料表                                            |
| 追蹤.UseDevicePowerData()                | 提供關於裝置 D 狀態的追蹤資料。                                                                          | 裝置 DState 資料表                                                  |
| 追蹤.UseDirectXData()                    | 提供有關 DirectX 活動的追蹤資料。                                                                         | GPU 使用率資料表                                                |
| traceUseDiskIOData()                      | 提供有關磁片 i/o 活動的追蹤資料。                                                                        | 磁片使用量資料表                                                     |
| 追蹤.UseEnergyEstimationData()           | 提供從能源估計引擎估計的每一處理程式能源使用量的追蹤資料。                         | 能源估計引擎摘要（依進程）資料表                  |
| 追蹤.UseEnergyMeterData()                | 提供來自能源計量介面（EMI）之測量能源使用量的追蹤資料。                                  | 能源估計引擎（透過 Emi）資料表                              |
| 追蹤.UseFileIOData()                     | 提供有關檔案 i/o 活動的追蹤資料。                                                                        | 檔案 i/o 資料表                                                       |
| 追蹤.UseGenericEvents()                  | 提供追蹤中的 TraceLogging 事件。                                                                  | 一般事件資料表（當事件種類為 [已列入] 或 [TraceLogging] 時） |
| 追蹤.UseHandles()                        | 提供有關使用中核心控制碼之追蹤的部分資料。                                                            | 控制碼資料表                                                        |
| 追蹤.UseHardFaults()                     | 提供有關硬體分頁錯誤的追蹤資料。                                                                         | 硬性錯誤資料表                                                    |
| 追蹤.UseHeapSnapshots()                  | 提供有關處理常式堆積使用量的追蹤資料。                                                                       | 堆積快照集資料表                                                  |
| 追蹤.UseHypercalls()                     | 提供追蹤期間發生之 Hyper-v 虛擬化的相關資料。                                                        |                                                                      |
| 追蹤.UseImageSections()                  | 提供有關影像區段的追蹤資料。                                                                 | CPU 使用量（取樣）資料表的區段名稱資料行                 |
| 追蹤.UseInterruptHandlingData()          | 提供有關「插斷服務常式」（ISR）和「延遲程序呼叫」（DPC）活動的追蹤資料。               | DPC/ISR 資料表                                                        |
| 追蹤.UseMarks()                          | 提供追蹤的標記（加上標籤）。                                                                      | 標記資料表                                                          |
| 追蹤.UseMemoryUtilizationData()          | 提供追蹤中有關總系統記憶體使用量的資料。                                                          | 記憶體使用率資料表                                             |
| 追蹤.UseMetadata()                       | 提供可用的追蹤中繼資料，而不需要進一步處理。                                                              | 系統組態、追蹤和一般                             |
| 追蹤.UsePlatformIdleStates()             | 提供有關系統的目標和實際平臺閒置狀態的追蹤資料。                                   | 平臺閒置狀態資料表                                            |
| 追蹤.UsePoolAllocations()                | 提供有關核心集區記憶體使用量的追蹤資料。                                                                 | 集區摘要資料表                                                   |
| 追蹤.UsePowerConfigurationData()         | 提供有關系統電源設定的追蹤資料。                                                               | 系統組態，電源設定                                 |
| 追蹤.UsePowerDependencyCoordinatorData() | 提供有關作用中的電源相依性協調器階段的追蹤資料。                                               | 通知階段摘要資料表                                     |
| 追蹤.UseProcesses()                      | 提供追蹤期間作用中進程的相關資料，以及其影像和 Pdb。                                      | 進程資料表;Images 資料表;符號中樞                           |
| 追蹤.UseProcessorCounters()              | 從處理器計數器監視器（PCM）的處理器效能計數器值的追蹤中提供資料。                |                                                                      |
| 追蹤.UseProcessorFrequencyData()         | 從追蹤中提供處理器執行頻率的相關資料。                                                    | 處理器頻率資料表（當類型為實際時）                      |
| 追蹤.UseProcessorProfileData()           | 提供有關使用中處理器電源設定檔的追蹤資料。                                                       | 處理器設定檔資料表                                             |
| 追蹤.UseProcessorParkingData()           | 提供有關已暫停或已離開之處理器的追蹤資料。                                                 | 處理器停車狀態資料表                                        |
| 追蹤.UseProcessorParkingLimits()         | 從追蹤中提供所允許的已起始處理器數目上限的資料。                                        | 核心停車端點狀態表                                         |
| 追蹤.UseProcessorQualityOfServiceData()  | 提供有關每個處理器之服務品質等級的追蹤資料。                                          | 處理器 Qos 類別表                                            |
| 追蹤.UseProcessorThrottlingData()        | 提供有關處理器最大頻率節流的追蹤資料。                                                   | 處理器條件約束表                                          |
| 追蹤.UseReadyBootData()                  | 從準備開機的開機預先提取活動的追蹤中提供資料。                                                | 就緒開機事件資料表                                              |
| 追蹤.UseReferenceSetData()               | 提供有關每個進程所使用之虛擬記憶體頁面的追蹤資料。                                             | 參考集資料表                                                  |
| 追蹤.UseRegionsOfInterest()              | 根據 xml 設定檔中所指定的追蹤，提供相關間隔的指定區域。                       | 感利率的區域資料表                                            |
| 追蹤.UseRegistryData()                   | 提供追蹤期間登錄活動的相關資料。                                                                      | 登錄資料表                                                       |
| 追蹤.UseResidentSetData()                | 針對常駐在實體記憶體中的每個進程，提供有關虛擬記憶體頁面的追蹤資料。       | 常駐集資料表                                                   |
| 追蹤.UseRundownData()                    | 針對追蹤取消資料收集發生的間隔，從追蹤提供資料。                            | 圖形時間軸中的陰影區域                                 |
| 追蹤.UseScheduledTasks()                 | 提供追蹤期間執行之排程工作的相關資料。                                                               | 排定的工作資料表                                                |
| 追蹤.UseServices()                       | 提供有關在追蹤期間使用中或已捕捉其狀態之服務的資料。                                  | 服務資料表;系統組態，服務                       |
| 追蹤.UseStacks()                         | 提供追蹤期間所記錄之堆疊的相關資料。                                                                        |                                                                      |
| 追蹤.UseStackEvents()                    | 提供與追蹤期間記錄之堆疊相關聯的事件資料。                                                 | 堆疊資料表                                                         |
| 追蹤.UseStackTags()                      | 提供的對應工具會將追蹤的堆疊分組到 XML 設定檔中所指定的堆疊標記。               | 堆疊標記和堆疊之類的資料行（框架標記）                     |
| 追蹤.UseSymbols()                        | 提供載入追蹤符號的功能。                                                                          | 設定符號路徑;載入符號                                 |
| 追蹤.UseSyscalls()                       | 提供追蹤期間發生之 syscalls 的相關資料。                                                                 | Syscalls 資料表                                                       |
| 追蹤.UseSystemMetadata()                 | 提供追蹤的一般全系統中繼資料。                                                                       | 系統設定                                                 |
| 追蹤.UseSystemPowerSourceData()          | 提供有關使用中系統電源來源（AC vs DC）之追蹤的資料。                                                | 系統電源來源資料表                                            |
| 追蹤.UseSystemSleepData()                | 提供有關整體系統電源狀態的追蹤資料。                                                               | 電源轉換表                                               |
| 追蹤.UseTargetCpuIdleStates()            | 提供有關目標 CPU C 狀態的追蹤資料。                                                                      | CPU 閒置狀態資料表（當類型為 Target 時）                          |
| 追蹤.UseTargetProcessorFrequencyData()   | 提供有關目標處理器頻率的追蹤資料。                                                             | 處理器頻率資料表（當類型為 Target 時）                      |
| 追蹤.UseThreads()                        | 提供追蹤期間作用中線程的相關資料。                                                                         | 執行緒存留期資料表                                               |
| 追蹤.UseTraceStatistics()                | 提供追蹤中事件的相關統計資料。                                                                           | 系統組態，追蹤統計資料                               |
| 追蹤.UseUtcData()                        | 使用通用遙測用戶端（UTC），從有關 Microsoft 遙測活動的追蹤中提供資料。                      | Utc 資料表                                                            |
| 追蹤.UseWindowInFocus()                  | 提供追蹤中使用中 UI 視窗變更的相關資料。                                                 | 焦點資料表中的視窗                                                |
| 追蹤.UseWindowsTracePreprocessorEvents() | 提供追蹤的 Windows 軟體追蹤預處理器（WPP）事件。                                                    | WPP 追蹤資料表;一般事件資料表（當事件種類為 WPP 時）       |
| 追蹤.UseWinINetData()                    | 透過 Windows Internet （WinINet）從關於網際網路活動的追蹤提供資料。                                         | 下載詳細資料表格                                               |
| 追蹤.UseWorkingSetData()                 | 針對每個進程或核心類別之工作集內的虛擬記憶體頁面，提供來自追蹤的資料。 | 虛擬記憶體快照集資料表                                       |

另請參閱[ITraceSource](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.itracesource)上的擴充方法，以取得所有可用的追蹤資料，或檢查 "trace" 中提供的方法。 由 IntelliSense 顯示。

## <a name="next-steps"></a>後續步驟

在此總覽中，您已瞭解如何使用 TraceProcessor 以及它可以存取的內建資料來源來存取追蹤資料。

下一個步驟是瞭解如何[擴充](extensibility.md)TraceProcessor 來存取自訂追蹤資料。
