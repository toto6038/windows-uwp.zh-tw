---
title: 什麼是 .NET TraceProcessing-.NET TraceProcessing
description: 在此總覽中，您將瞭解什麼是 .NET TraceProcessing。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: 03f88d6bd1ac150d94f73f9f9177249c3b5ae78f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170552"
---
# <a name="process-etw-traces-in-net"></a>在 .NET 中處理 ETW 追蹤

[Windows 事件追蹤 (ETW) ](/windows/win32/etw/event-tracing-portal) 是 windows 作業系統內建的強大追蹤收集系統。 Windows 與 ETW 的深度整合，包括對內容切換、記憶體配置、進程建立和結束等事件，一直到核心的系統行為資料。 可從 ETW 取得的全系統資料，使其適合用於端對端效能分析或其他需要查看整個系統中多個元件之間互動的問題。

與文字記錄不同的是，ETW 提供針對自動化資料處理而設計的結構化事件。 Microsoft 在這些結構化事件之上建立了強大的工具，包括 [Windows Performance Analyzer (WPA) ](/windows-hardware/test/wpt/windows-performance-analyzer)，它提供圖形介面來視覺化和探索 ETW 追蹤檔中所捕捉的追蹤資料 ( .etl) 。

在 Microsoft 內部，我們會大量使用 ETW 追蹤來測量 Windows 新組建的效能。 由於產生的是 Windows 工程系統的資料量，因此自動化分析是不可或缺的。 針對我們的自動化追蹤分析，我們會大量使用 c # 和 .NET，因此我們建立了 [.Net TRACEPROCESSING API](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) 來存取許多種類的 ETW 追蹤資料。 這項技術也會用在 Windows Performance Analyzer 中，以增強其多個資料表的功能。

.NET TraceProcessing NuGet 套件可讓您使用 Microsoft 用來分析 Windows 的相同工具來分析您自己的應用程式和系統。

## <a name="next-steps"></a>後續步驟

在此總覽中，您已瞭解什麼是 .NET TraceProcessing。

下一步是 [處理您的第一個追蹤](quickstart.md)。

## <a name="related-topics"></a>相關主題

* [存取追蹤資料](tutorial.md)
* [擴充 TraceProcessor](extensibility.md)
* [載入符號](symbols.md)
* [使用串流](streaming.md)
* [範例](https://github.com/microsoft/eventtracing-processing-samples)
* [API 參考](reference.md)