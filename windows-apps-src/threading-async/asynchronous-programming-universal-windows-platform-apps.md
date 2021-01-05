---
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: 非同步程式設計
description: '本主題說明通用 Windows 平臺 (UWP) 的非同步程式設計，以及在 c #、Microsoft Visual Basic .NET、c + + 和 JavaScript 中的標記法。'
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, 非同步
ms.localizationpriority: medium
ms.openlocfilehash: 1c0798a1d58d3a97d02e030bcd0094f8a7364d16
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860382"
---
# <a name="asynchronous-programming"></a>非同步程式設計
本主題說明通用 Windows 平臺 (UWP) 的非同步程式設計，以及在 c #、Microsoft Visual Basic .NET、c + + 和 JavaScript 中的標記法。

使用非同步程式設計可協助 app 在執行需要花一段時間才能完成的工作，還是能保持回應。 例如，從網際網路下載內容的應用程式可能需要花費一些時間來等候內容。 如果您在 UI 執行緒使用同步方法抓取內容，則應用程式會遭到封鎖，直到該方法傳回為止。 應用程式將不會回應使用者的互動，而因為應用程式似乎沒有回應，使用者就會感到失望。 比較好的做法是使用非同步程式設計，如此一來，app 可以在等候某項作業完成的同時，繼續執行以及回應 UI。

對於需要較長時間才能完成的方法，使用非同步程式設計是標準做法，而不是 UWP 的特例。 JavaScript、c #、Visual Basic 和 c + + 都提供非同步方法的語言支援。

## <a name="asynchronous-programming-in-the-uwp"></a>UWP 的非同步程式設計
許多 UWP 功能（例如 [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) Api 和 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) api）都會公開為非同步 api。 依照慣例，非同步 Api 的名稱會以 "Async" 結尾，表示在控制項傳回給呼叫端之後，可能會發生部分的執行。

在通用 Windows 平台 (UWP) app 中使用非同步 API 時，程式碼會以一致的方式發出非封鎖性呼叫。 因此當您在自己的 API 定義中實作這些非同步模式時，呼叫者就可理解並以可預測的方式來使用您的程式碼。

以下是一些需要呼叫非同步 Windows 執行階段 Api 的一般工作。

-   顯示訊息對話方塊

-   使用檔案系統，顯示檔案選擇器

-   從網際網路傳送和接收資料

-   使用通訊端、資料流、連線能力

-   使用約會、通訊錄、行事曆

-   使用檔案類型，像是開啟可攜式文件格式 (PDF) 檔案或是解碼影像或媒體格式

-   與裝置或服務互動

使用 UWP 非同步模式時，完全可以避免明確地管理執行緒。 每一種程式設計語言都會以自己的方式支援 UWP 的非同步模式：

| 程式設計語言 | 非同步表示法           |
|----------------------|---------------------------------------|
| C#                   | **async** 關鍵字， **await** 運算子 |
| Visual Basic         | **Async** 關鍵字、**Await** 運算子 |
| C++/WinRT            | 協同程式和 **co_await** 運算子  |
| C++/CX               | **task** 類別，**.then** 方法      |
| JavaScript           | Promise 物件，**then** 函式     |

## <a name="asynchronous-patterns-in-uwp-using-c-and-visual-basic"></a>使用 C# 和 Visual Basic 的 UWP 中的非同步模式
使用 C# 或 Visual Basic 編寫的典型程式碼片段會以同步的方式執行，這表示必須在一行結束之後，才可以開始執行下一行。 雖然先前已有適合非同步執行作業的 Microsoft .NET 程式設計模型，但產生的程式碼大多傾向於強調執行非同步程式碼的機制，而不是著重於程式碼嘗試完成的工作。 UWP、.NET Framework 及 C# 和 Visual Basic 編譯器新增了從程式碼中抽取非同步機制的功能。 您可以針對 .NET 和 UWP 撰寫非同步程式碼，將焦點放在程式碼執行的動作而不是執行的方式和時機。 非同步程式碼看起來會與同步程式碼相當類似。 如需詳細資訊，請參閱[在 C# 或 Visual Basic 中呼叫非同步 API](call-asynchronous-apis-in-csharp-or-visual-basic.md)。

## <a name="asynchronous-patterns-in-uwp-with-cwinrt"></a>使用 c + +/WinRT 之 UWP 中的非同步模式
使用 c + +/WinRT，您可以使用協同程式和 **co_await** 運算子。 如需詳細資訊和程式碼範例，請參閱 [c + + 中的非同步程式設計/WinRT](../cpp-and-winrt-apis/concurrency.md)。

## <a name="asynchronous-patterns-in-uwp-with-ccx"></a>使用 c + +/CX 的 UWP 中的非同步模式
在 C++/CX 中，非同步程式設計的基礎在於 [**task class**](/cpp/parallel/concrt/reference/task-class) 以及其 [**then method**](/cpp/parallel/concrt/reference/task-class?view=vs-2017&preserve-view=true)。 語法和 JavaScript Promise 的語法類似。 **task 類別** 以及相關類型也會提供取消和管理執行緒內容的功能。 如需詳細資訊，請參閱 [c + +/cx 中的非同步程式設計](asynchronous-programming-in-cpp-universal-windows-platform-apps.md)。

[**Create \_ async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017&preserve-view=true)函式提供的支援可讓您產生可從 JavaScript 取用的非同步 api，或任何其他支援 UWP 的語言。 如需詳細資訊，請參閱 [在 c + +/cx 中建立異步操作](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)。

## <a name="asynchronous-patterns-in-uwp-using-javascript"></a>使用 JavaScript 的 UWP 中的非同步模式
在 JavaScript 中，非同步程式設計採用 [Common JS Promises/A](https://wiki.commonjs.org/wiki/Promises/A) 建議的標準，讓非同步方法傳回 Promise 物件。 Promise 可用於 UWP 以及適用於 JavaScript 的 Windows Library。

Promise 物件代表會在未來完成的值。 在 UWP 中，您會從 Factory 函式取得 Promise 物件，按慣例它的名稱結尾會是 "Async"。

在許多情況下，呼叫非同步函式幾乎和呼叫傳統函式一樣容易。 其中的差異在於您是使用 [**then**](/previous-versions/windows/apps/br229728(v=win.10)) 或 [**done**](/previous-versions/windows/apps/hh701079(v=win.10)) 方法，為結果或錯誤指派處理常式以及啟動作業。

## <a name="related-topics"></a>相關主題
* [以 c # 或 Visual Basic 呼叫非同步 Api](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [使用 Async 和 Await 設計非同步程式 (C# 和 Visual Basic)](/previous-versions/visualstudio/visual-studio-2012/hh191443(v=vs.110))
* [黑白棋範例功能案例：非同步程式碼](/previous-versions/windows/apps/jj712233(v=win.10))
