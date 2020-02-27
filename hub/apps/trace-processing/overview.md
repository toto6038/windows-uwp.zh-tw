---
title: 什麼是 .NET TraceProcessing-.NET TraceProcessing
description: 在此總覽中，您將瞭解什麼是 .NET TraceProcessing。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: 1283a6b183673cbfb0d3d27290fc24ae6b020302
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629069"
---
# <a name="process-event-tracing-for-windows-etw-traces-in-net"></a>在 .NET 中處理 Windows 事件追蹤（ETW）追蹤

[Windows 事件追蹤（ETW）](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal)是 windows 作業系統內建的功能強大的追蹤集合系統。 Windows 與 ETW 的深度整合，包括針對內容切換、記憶體配置、進程建立和結束等事件，向下到核心的系統行為資料。 可從 ETW 取得的全系統資料，非常適合用於端對端效能分析或其他問題，而需要查看整個系統中許多元件之間的互動。

與文字記錄不同的是，ETW 提供了專為自動化資料處理而設計的結構化事件。 Microsoft 已經在這些結構化事件之上建立強大的工具，包括[Windows Performance Analyzer （WPA）](https://docs.microsoft.com/windows-hardware/test/wpt/windows-performance-analyzer)，它提供圖形化介面來視覺化和流覽 ETW 追蹤檔案（.etl）中所捕捉的追蹤資料。

在 Microsoft 內部，我們會大量使用 ETW 追蹤來測量 Windows 新組建的效能。 由於產生 Windows 工程系統的資料量，自動化分析是不可或缺的。 針對我們的自動化追蹤分析，我們大量C#使用和 .net，因此我們建立了[.net TraceProcessing API](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All)來存取許多種類的 ETW 追蹤資料。 Windows Performance Analyzer 內也使用這項技術來增強其中幾個資料表。

.NET TraceProcessing NuGet 套件可讓您使用 Microsoft 用來分析 Windows 的相同工具來分析您自己的應用程式和系統。

## <a name="next-steps"></a>後續步驟

在此總覽中，您已瞭解什麼是 .NET TraceProcessing。

下一個步驟是[處理您的第一個追蹤](quickstart.md)。

## <a name="in-this-section"></a>本節內容

* [存取追蹤資料](tutorial.md)
* [擴充 TraceProcessor](extensibility.md)
* [載入符號](symbols.md)
* [使用串流](streaming.md)
* [範例](https://github.com/microsoft/eventtracing-processing-samples)
* [API 參考](reference.md)
