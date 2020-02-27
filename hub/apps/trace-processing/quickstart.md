---
title: 快速入門處理追蹤-.NET TraceProcessing
description: 在本快速入門中，瞭解如何存取 ETW 追蹤中的資料。
author: maiak
ms.author: maiak
ms.date: 02/20/2020
ms.topic: quickstart
ms.openlocfilehash: 162646baff9b2d08f6fc0ea4862802216cff9619
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629099"
---
# <a name="quickstart-process-your-first-trace"></a>快速入門：處理您的第一個追蹤

試用 TraceProcessor 以存取 Windows 事件追蹤（ETW）追蹤中的資料。 TraceProcessor 可讓您以 .NET 物件的形式存取 ETW 追蹤資料。

在本快速入門中，您將瞭解如何：

1. 安裝 TraceProcessing NuGet 套件。
2. 建立 TraceProcessor。
3. 使用 TraceProcessor 來存取追蹤中包含的進程命令列。

## <a name="prerequisites"></a>必要條件

Visual Studio 2019

## <a name="install-the-traceprocessing-nuget-package"></a>安裝 TraceProcessing NuGet 套件

您可以從[NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All)使用下列套件識別碼來取得 .net TraceProcessing：

EventTracing 處理。全部

您可以在主控台應用程式中使用此套件，以列出 ETW 追蹤（.etl 檔案）中包含的進程命令列。

1. 建立新的 .NET Core 主控台應用程式。 在 Visual Studio 中，選取 [檔案]、[新增]、[專案 ...]，然後選擇的 [主控台應用C#程式（.net Core）] 範本。

    輸入專案名稱（例如，TraceProcessorQuickstart），然後選擇 [建立]。

2. 在方案總管中，以滑鼠右鍵按一下 [相依性]，然後選擇 [管理 NuGet 套件 ...]並切換至 [流覽] 索引標籤。

3. 在 [搜尋] 方塊中，輸入 EventTracing，然後按 [搜尋]。

    在具有該名稱的 NuGet 套件上選取 [安裝]，然後關閉 NuGet 視窗。

## <a name="create-a-traceprocessor"></a>建立 TraceProcessor

1. 將 Program.cs 變更為下列內容：

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
                // TODO: call trace.Use...

                trace.Process();

                Console.WriteLine("TODO: Access data from the trace");
            }
        }
    }
    ```

2. 提供執行專案時所要使用的追蹤名稱。

    在方案總管中，以滑鼠右鍵按一下專案，然後選擇 [屬性]。 切換至 [Debug] 索引標籤，並在應用程式引數中輸入追蹤（.etl 檔案）的路徑。

    如果您還沒有追蹤檔案，可以使用[Windows Performance 錄製](https://docs.microsoft.com/windows-hardware/test/wpt/start-a-recording)器來建立一個。

3. 執行應用程式。

    選擇 [Debug]、[啟動但不進行偵錯工具] 來執行程式碼。

## <a name="use-traceprocessor-to-access-process-command-lines-contained-in-the-trace"></a>使用 TraceProcessor 存取追蹤中包含的進程命令列

1. 將 Program.cs 變更為下列內容：

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

2. 再次執行應用程式。

    這次，您應該會看到記錄追蹤時，來自所有進程的清單命令列。

## <a name="next-steps"></a>後續步驟

在本快速入門中，您已建立主控台應用程式、安裝 TraceProcessor，並使用它從 ETW 追蹤存取進程命令列。 現在您有一個可存取追蹤資料的應用程式。

進程資訊只是儲存在您的應用程式可以存取的 ETW 追蹤中的多種資料類型之一。

下一個步驟是深入瞭解[TraceProcessor](tutorial.md) ，以及它可以存取的其他資料來源。
