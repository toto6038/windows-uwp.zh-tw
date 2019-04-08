---
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: 非同步程式設計
description: 本主題描述非同步程式設計在通用 Windows 平台 (UWP) 和其表示在C#，Microsoft Visual Basic.NET、 c + + 和 JavaScript。
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, 非同步
ms.localizationpriority: medium
ms.openlocfilehash: a8349b9a96dd67d64abb368f0fdadd822af2fe84
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613213"
---
# <a name="asynchronous-programming"></a>非同步程式設計
本主題描述非同步程式設計在通用 Windows 平台 (UWP) 和其表示在C#，Microsoft Visual Basic.NET、 c + + 和 JavaScript。

使用非同步程式設計可協助 app 在執行需要花一段時間才能完成的工作，還是能保持回應。 例如，從網際網路下載內容的應用程式可能需要花費一些時間來等候內容。 如果您在 UI 執行緒使用同步方法抓取內容，則應用程式會遭到封鎖，直到該方法傳回為止。 應用程式將不會回應使用者的互動，而因為應用程式似乎沒有回應，使用者就會感到失望。 比較好的做法是使用非同步程式設計，如此一來，app 可以在等候某項作業完成的同時，繼續執行以及回應 UI。

對於需要較長時間才能完成的方法，使用非同步程式設計是標準做法，而不是 UWP 的特例。 JavaScript 中， C#，Visual Basic 和 c + + 每個非同步方法提供語言支援。

## <a name="asynchronous-programming-in-the-uwp"></a>UWP 的非同步程式設計
許多 UWP 功能，例如[ **MediaCapture** ](https://msdn.microsoft.com/library/windows/apps/BR241124) Api 並[ **StorageFile** ](https://msdn.microsoft.com/library/windows/apps/BR227171) Api 會公開為非同步 Api。 依照慣例，非同步 Api 的名稱結尾都是"Async"，以指出其執行的一部分很控制項傳回給呼叫者後，會發生。

在通用 Windows 平台 (UWP) app 中使用非同步 API 時，程式碼會以一致的方式發出非封鎖性呼叫。 因此當您在自己的 API 定義中實作這些非同步模式時，呼叫者就可理解並以可預測的方式來使用您的程式碼。

以下是一些需要呼叫非同步 UWP API 的常見工作。

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
| C#                   | **async** 關鍵字、**await** 運算子 |
| Visual Basic         | **Async** 關鍵字、**Await** 運算子 |
| C++/WinRT            | 協同程式，並**co_await**運算子  |
| C++/CX               | **task** 類別，**.then** 方法      |
| JavaScript           | Promise 物件，**then** 函式     |

## <a name="asynchronous-patterns-in-uwp-using-c-and-visual-basic"></a>使用 C# 和 Visual Basic 的 UWP 中的非同步模式
使用 C# 或 Visual Basic 編寫的典型程式碼片段會以同步的方式執行，這表示必須在一行結束之後，才可以開始執行下一行。 雖然先前已有適合非同步執行作業的 Microsoft .NET 程式設計模型，但產生的程式碼大多傾向於強調執行非同步程式碼的機制，而不是著重於程式碼嘗試完成的工作。 UWP、.NET Framework 及 C# 和 Visual Basic 編譯器新增了從程式碼中抽取非同步機制的功能。 您可以針對 .NET 和 UWP 撰寫非同步程式碼，將焦點放在程式碼執行的動作而不是執行的方式和時機。 非同步程式碼看起來會與同步程式碼相當類似。 如需詳細資訊，請參閱[在 C# 或 Visual Basic 中呼叫非同步 API](call-asynchronous-apis-in-csharp-or-visual-basic.md)。

## <a name="asynchronous-patterns-in-uwp-with-cwinrt"></a>非同步模式中 UWP 使用 C + + /cli WinRT
使用 C + + /cli WinRT，使用協同程式，而**co_await**運算子。 如需詳細資訊，以及程式碼範例，請參閱[非同步程式設計，在 C + + /cli WinRT](../cpp-and-winrt-apis/concurrency.md)。

## <a name="asynchronous-patterns-in-uwp-with-ccx"></a>非同步模式中 UWP 使用 C + + /CX
在 C++/CX 中，非同步程式設計的基礎在於 [**task class**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750113.aspx) 以及其 [**then method**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750044.aspx)。 語法和 JavaScript Promise 的語法類似。 **task 類別**以及相關類型也會提供取消和管理執行緒內容的功能。 如需詳細資訊，請參閱 <<c0> [ 非同步程式設計，在 C + + /CX](asynchronous-programming-in-cpp-universal-windows-platform-apps.md)。

[**建立\_非同步函式**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750102.aspx)提供支援，以便產生可從 JavaScript 或任何其他語言可支援 UWP 取用的非同步 Api。 如需詳細資訊，請參閱 <<c0> [ 建立非同步作業 C + + /CX](https://msdn.microsoft.com/library/windows/apps/xaml/hh750082.aspx)。

## <a name="asynchronous-patterns-in-uwp-using-javascript"></a>使用 JavaScript 的 UWP 中的非同步模式
在 JavaScript 中，非同步程式設計採用 [Common JS Promises/A](https://wiki.commonjs.org/wiki/Promises/A) 建議的標準，讓非同步方法傳回 Promise 物件。 Promise 可用於 UWP 以及適用於 JavaScript 的 Windows Library。

Promise 物件代表會在未來完成的值。 在 UWP 中，您會從 Factory 函式取得 Promise 物件，按慣例它的名稱結尾會是 "Async"。

在許多情況下，呼叫非同步函式幾乎和呼叫傳統函式一樣容易。 其中的差異在於您是使用 [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) 或 [**done**](https://msdn.microsoft.com/library/windows/apps/Hh701079) 方法，為結果或錯誤指派處理常式以及啟動作業。

## <a name="related-topics"></a>相關主題
* [在 C# 或 Visual Basic 中呼叫非同步 API](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [非同步程式設計使用 Async 和 Await （C# 和 Visual Basic）](https://msdn.microsoft.com/library/hh191443(vs.110).aspx)
* [棋功能案例範例： 非同步程式碼](https://msdn.microsoft.com/library/windows/apps/xaml/jj712233.aspx#async)
