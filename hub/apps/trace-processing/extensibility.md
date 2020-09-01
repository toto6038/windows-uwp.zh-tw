---
title: 擴充性-.NET TraceProcessing
description: 在本教學課程中，您將瞭解如何擴充 .NET TraceProcessing。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: f5680bdc6502c4b917667e5a59084286b445063c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166732"
---
# <a name="extend-traceprocessor"></a>擴充 TraceProcessor

許多種類的追蹤資料在 [TraceProcessor](/dotnet/api/microsoft.windows.eventtracing.traceprocessor)中都有內建支援，但如果您有想要分析的其他提供者 (包括) 的自訂提供者，則在進行處理時，該資料也會在即時追蹤中提供。

> [!NOTE]
> 這部分的 API 目前處於預覽狀態，且正在進行開發。 未來的版本可能會變更。

例如，以下是取得追蹤中提供者識別碼清單的簡單方法。

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    HashSet<Guid> providerIds = new HashSet<Guid>();
    trace.Use((e) => providerIds.Add(e.ProviderId));
    trace.Process();

    foreach (Guid providerId in providerIds)
    {
        Console.WriteLine(providerId);
    }
}
```

下列範例顯示簡化的自訂資料來源。

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    CustomDataSource customDataSource = new CustomDataSource();
    trace.Use(customDataSource);

    trace.Process();

    Console.WriteLine(customDataSource.Count);
}

class CustomDataSource : IFilteredEventConsumer
{
    public IReadOnlyList<Guid> ProviderIds { get; } = new Guid[] { new Guid("your provider ID") };

    public int Count { get; private set; }

    public void Process(EventContext eventContext)
    {
        ++Count;
    }
}
```

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已瞭解如何擴充 TraceProcessor。

下一步是瞭解如何載入追蹤的 [符號](symbols.md) 。