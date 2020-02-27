---
title: 擴充性-.NET TraceProcessing
description: 在本教學課程中，您將瞭解如何擴充 .NET TraceProcessing。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: bf5f7a7c1bb007b7f1a19508fa0ee7bbaf298654
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629139"
---
# <a name="extend-traceprocessor"></a>擴充 TraceProcessor

許多類型的追蹤資料在[TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor)中都有內建支援，但如果您有想要分析的其他提供者（包括您自己的自訂提供者），則在進行處理時，也可以從追蹤即時取得該資料。

例如，以下是在追蹤中取得提供者識別碼清單的簡單方法：

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

下列範例會顯示簡化的自訂資料來源：

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

下一步是瞭解如何載入追蹤的[符號](symbols.md)。
