---
ms.assetid: FA25562A-FE62-4DFC-9084-6BD6EAD73636
title: 讓 UI 執行緒保持回應
description: 不論使用何種電腦，使用者都希望 app 在進行計算時仍然能夠回應。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ba35ae10333481f36f68d666abe5b6bf3b1355fe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166162"
---
# <a name="keep-the-ui-thread-responsive"></a>讓 UI 執行緒保持回應


不論使用何種電腦，使用者都希望 app 在進行計算時仍然能夠回應。 這對於不同的 app 有不同的意義。 對某些 app 而言，這表示提供更實際的物理性質、更快地從磁碟或網路載入資料、快速地呈現複雜的場景和在頁面之間瀏覽、立即找到方向，或是快速處理資料。 不論執行何種計算，使用者都會希望 app 可以回應輸入，不希望有看似在「思考」而停止回應的情況發生。

您的 app 是事件驅動，這表示程式碼會執行工作來回應事件，接著進入閒置狀態，直到出現下一個事件。 平台的 UI 程式碼 (配置、輸入、引發事件等等) 及 app 的 UI 程式碼都在相同的 UI 執行緒上執行。 該執行緒上一次只能執行一個指令，如果您的 app 程式碼花費太多時間處理事件，則架構無法執行配置或引發代表使用者互動的新事件。 App 的回應性與是否有 UI 執行緒來處理工作相關。

幾乎所有對 UI 執行緒所做的變更都需要用到 UI 執行緒，包括建立 UI 類型和存取其成員。 您無法從背景執行緒更新 UI，但可以使用 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 傳遞訊息給它，就在那裡執行程式碼。

> **注意**  例外的是另有一個轉譯執行緒可以套用 UI 變更，而不會影響輸入的處理方式或基本配置。 例如，許多不會影響版面配置的動畫和轉場效果可以在這個轉譯執行緒上執行。

## <a name="delay-element-instantiation"></a>延遲元素具現化

app 中最慢的一些階段包括啟動和切換檢視。 顯示使用者最初看到的 UI 時不要太過花俏。 例如，不要建立那種逐漸展露 UI 和快顯內容的 UI。

-   使用 [x:Load 屬性](../xaml-platform/x-load-attribute.md)或 [x: DeferLoadStrategy](../xaml-platform/x-deferloadstrategy-attribute.md) 延遲具現化元素。
-   以程式設計方式隨需將元素插入樹狀目錄。

[**CoreDispatcher.RunIdleAsync**](/uwp/api/windows.ui.core.coredispatcher.runidleasync) 佇列適用於 UI 執行緒不忙碌時處理。

## <a name="use-asynchronous-apis"></a>使用非同步 API

為了協助保持 App 的回應性，平台為其許多 API 提供非同步的版本。 非同步 API 可確保使用中的執行緒不會被封鎖太長的時間。 當您從 UI 執行緒呼叫 API 時，如果有非同步版本可用，請使用非同步版本。 如需使用 **async** 模式進行設計程式的詳細資訊，請參閱[非同步程式設計](../threading-async/asynchronous-programming-universal-windows-platform-apps.md)或[在 C# 或 Visual Basic 中呼叫非同步 API](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md)。

## <a name="offload-work-to-background-threads"></a>將工作卸載到背景執行緒

撰寫可快速返回的事件處理常式。 在需要執行不少的工作量時，排定給背景執行緒並返回。

您可以使用 C# 的 **await** 運算子、Visual Basic 的 **Await** 運算子或 C++ 中的委派，以非同步方式排程工作。 但這並不保證您排程的工作一定會在背景執行緒中執行。 許多通用 Windows 平台 (UWP) API 會為您在背景執行緒中排程工作，但如果僅使用 **await** 或委派呼叫您的 app 程式碼，則會在 UI 執行緒中執行該委派或方法。 您必須明確指示何時要在背景執行緒中執行您的 app 程式碼。 在 C# 與 Visual Basic 中，將程式碼傳送到 [**Task.Run**](/dotnet/api/system.threading.tasks.task.run) 即可做出明確的指示。

請記住，只有從 UI 執行緒才能存取 UI 元素。 啟動背景工作之前先使用 UI 執行緒存取 UI 元素，以及/或在背景執行緒上使用 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 或 [**CoreDispatcher.RunIdleAsync**](/uwp/api/windows.ui.core.coredispatcher.runidleasync)。

例如，遊戲中的人工智慧計算，就是背景執行緒中可以執行的工作。 計算電腦下一步行動的程式碼需要花很多時間執行。

```csharp
public class AsyncExample
{
    private async void NextMove_Click(object sender, RoutedEventArgs e)
    {
        // The await causes the handler to return immediately.
        await System.Threading.Tasks.Task.Run(() => ComputeNextMove());
        // Now update the UI with the results.
        // ...
    }

    private async System.Threading.Tasks.Task ComputeNextMove()
    {
        // Perform background work here.
        // Don't directly access UI elements from this method.
    }
}
```

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public class Example
> {
>     // ...
>     private async void NextMove_Click(object sender, RoutedEventArgs e)
>     {
>         await Task.Run(() => ComputeNextMove());
>         // Update the UI with results
>     }
> 
>     private async Task ComputeNextMove()
>     {
>         // ...
>     }
>     // ...
> }
> ```
> ```vb
> Public Class Example
>     ' ...
>     Private Async Sub NextMove_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
>         Await Task.Run(Function() ComputeNextMove())
>         ' update the UI with results
>     End Sub
> 
>     Private Async Function ComputeNextMove() As Task
>         ' ...
>     End Function
>     ' ...
> End Class
> ```

在這個範例中，`NextMove_Click` 處理常式會回到 **await**，讓 UI 執行緒保持回應。 但在 `ComputeNextMove` (在背景執行緒上執行) 完成之後，該處理常式又會再次執行。 處理常式中的其餘程式碼會以結果更新 UI。

> **注意**  UWP 還有 [**ThreadPool**](/uwp/api/Windows.System.Threading.ThreadPool) 和 [**ThreadPoolTimer**](/uwp/api/windows.system.threading.threadpooltimer) API 可用於類似的案例。 如需詳細資訊，請參閱[執行緒和非同步程式設計](../threading-async/index.md)。

## <a name="related-topics"></a>相關主題

* [自訂使用者互動](../design/layout/index.md)