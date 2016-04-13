---
ms.assetid: 066711E0-D5C4-467E-8683-3CC64EDBCC83
在 C# 或 Visual Basic 中呼叫非同步 API
通用 Windows 平台 (UWP) 包含許多非同步 API，可以確保即使 app 執行需要花一段時間才能完成的工作，還是能保持回應。
---
# 在 C# 或 Visual Basic 中呼叫非同步 API

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


通用 Windows 平台 (UWP) 包含許多非同步 API，可以確保即使 app 執行需要花一段時間才能完成的工作，還是能保持回應。 本主題討論如何在 C# 或 Microsoft Visual Basic 使用 UWP 的非同步方法。

非同步 API 會讓您的應用程式不必等候大量操作完成，就能繼續執行。 例如，從網際網路下載資訊的應用程式可能需要花費一些時間來等候資訊。 如果您使用同步方法抓取資訊，則應用程式會遭到封鎖，直到該方法傳回為止。 app 將不會回應使用者的互動，而因為 app 似乎沒有回應，使用者就會感到失望。 透過提供非同步 API，UWP 可以協助確保 app 在執行長時間的操作時，仍然能夠回應使用者的動作。

UWP 中的多數非同步 API 沒有同步對應項目，所以您必須確定自己了解如何在通用 Windows 平台 (UWP) app 中使用非同步 API 搭配 C# 或 Visual Basic。 以下示範如何呼叫 UWP 的非同步 API。

## 使用非同步 API


根據慣例，非同步方法的名稱會以 "Async" 結尾。 您通常會為了回應使用者動作 (例如當使用者按一下按鈕時) 而呼叫非同步 API。 在事件處理常式中呼叫非同步方法，是使用非同步 API 最簡單的方法之一。 我們在這裡以 **await** 運算子為例。

假設您的應用程式會從某個位置列出部落格文章的標題。 App 含有一個 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)，使用者按一下該按鈕可以取得標題。 標題就會顯示在 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 中。 當使用者按一下按鈕後，app 在等待部落格網站資訊的同時，必須仍舊能保持回應。 為確保做出回應，UWP 提供下載摘要的非同步方法 [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)。

下列範例會呼叫非同步方法 [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460) 並等待結果，以從部落格取得部落格文章清單。

> [!div class="tabbedCodeSnippets" data-resources="OutlookServices.Calendar"]
[!code-csharp[Main](./AsyncSnippets/csharp/MainPage.xaml.cs#SnippetDownloadRSS)]
[!code-vb[Main](./AsyncSnippets/vbnet/MainPage.xaml.vb#SnippetDownloadRSS)]

這個範例有幾個重點。 首先，`SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` 程式行使用 **await** 運算子搭配對非同步方法 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460) 的呼叫。 您可以想像成 **await** 運算子告知編譯器您正在呼叫非同步方法，因而使編譯器代替您執行一些額外的工作。 其次，事件處理常式的宣告包含 **async** 關鍵字。 您必須在您使用 **await** 運算子的任何方法的方法宣告中包含這個關鍵字。

在這個主題中，我們將不深入探討使用 **await** 運算子時編譯器所執行工作的細節，但將檢查應用程式執行什麼工作，使得應用程式具有非同步性質和回應能力。 以使用同步程式碼時所發生的情況為例。 例如，假設有一個稱為 `SyndicationClient.RetrieveFeed` 的同步方法 (並沒有這種方法，但請想像有)。如果您的 app 包含 `SyndicationFeed feed = client.RetrieveFeed(feedUri)` 程式行而非 `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)`，在取得 `RetrieveFeed` 的傳回值之前，app 會停止執行。 而且，當您的 app 在等待方法完成時，不會回應任何其他事件 (另一個 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 事件)。 也就是說，您的 app 在 `RetrieveFeed` 傳回之前會被封鎖。

但是，如果您呼叫 `client.RetrieveFeedAsync`，該方法會起始擷取作業並立即傳回。 當您使用 **await** 搭配 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460) 時，app 會暫時結束事件處理常式。 接著，當 **RetrieveFeedAsync** 以非同步方式執行時，app 就可以處理其他事件。 這樣可以保持 app 對使用者的回應能力。 當 **RetrieveFeedAsync** 完成而且 [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485) 可用時，app 基本上會重新進入先前在 `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` 之後停止的事件處理常式，然後完成方法的其餘部分。

使用 **await** 運算子的好處是，程式碼的撰寫方式與您使用虛構的 `RetrieveFeed` 方法時的程式碼撰寫方式並無太大差異。 有數種方法可以在 C# 或 Visual Basic 中撰寫不含 **await** 運算子的非同步程式碼，但產生的程式碼傾向強調以非同步方式執行的技巧。 這讓非同步程式碼不易撰寫、難以理解，而且不容易維護。 透過使用 **await** 運算子，既可以獲得非同步應用程式的好處，又不會使程式碼過於複雜。

## 傳回非同步 API 的類型與結果


如果您瀏覽連結 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)，可能會發現 **RetrieveFeedAsync** 的傳回類型不是 [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485)。 相反地，傳回類型是 `IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress>`。 從原始語法的角度而言，非同步 API 會傳回其中包含結果的物件。 將非同步方法想像成可以等待是很常見的想法 (有時很有用)，**await** 運算子實際上是在方法的傳回值 (而非方法) 上操作。 當您套用 **await** 運算子時，您得到的是針對方法傳回的物件呼叫 **GetResult** 的結果。 在這個範例中，**SyndicationFeed** 是 **RetrieveFeedAsync.GetResult()** 的結果。

當您使用非同步方法時，您可以檢查簽章以查看等待等待方法傳回的值之後會得到什麼項目。 UWP 中的所有非同步 API 會傳回下列其中一個類型：

-   [**IAsyncOperation&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)
-   [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206594)
-   [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580)
-   [**IAsyncActionWithProgress&lt;TProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206580withprogress_1)

非同步方法的結果類型與 `      TResult` 類型參數相同。 不含 `TResult` 的類型將不會產生結果。 您可以將結果想像成是 **void**。 在 Visual Basic 中，[Sub](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/831f9wka.aspx) 程序等同於含有 **void** 傳回類型的方法。

下表提供非同步方法的範例，並列出每個方法的傳回類型和結果類型。

| 非同步方法                                                                           | 傳回類型                                                                                                                                        | 結果類型                                       |
|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)     | [**IAsyncOperationWithProgress&lt;SyndicationFeed, RetrievalProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206594)                                 | [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485) |
| [**FileOpenPicker.PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) | [**IAsyncOperation&lt;StorageFile&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)                                                                                | [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)          |
| [**XmlDocument.SaveToFileAsync**](https://msdn.microsoft.com/library/windows/apps/BR206284)                 | [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580)                                                                                                           | **void**                                          |
| [**InkStrokeContainer.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/Hh701757)               | [**IAsyncActionWithProgress&lt;UInt64&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206580withprogress_1)                                                                   | **void**                                          |
| [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/BR208135)                            | [
            **DataReaderLoadOperation**](https://msdn.microsoft.com/library/windows/apps/BR208120)，實作 **IAsyncOperation&lt;UInt32&gt;** 的自訂結果類別 | [**UInt32**](T:System.UInt32)                     |

 

[
            **.NET for UWP apps**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230232.aspx) 中定義的非同步方法含有[**Task**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/system.threading.tasks.task.aspx) 或 [**Task&lt;TResult&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/dd321424.aspx) 傳回類型。 傳回 **Task** 的方法與 UWP 中傳回 [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580) 的非同步方法類似。 在所有情況下，非同步方法的結果都是 **void**。 傳回類型 **Task&lt;TResult&gt;** 與 [**IAsyncOperation&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598) 的相似處在於執行工作時非同步方法的結果與 `TResult` 類型參數的類型相同。 如需使用 **.NET for UWP apps** 和工作的詳細資訊，請參閱[適用於 Windows 執行階段應用程式的 .NET 概觀](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230302.aspx)。

## 處理錯誤


使用 **await** 運算子從非同步方法取得您的結果時，您可以使用 **try/catch** 區塊來處理發生在非同步方法中的錯誤，如同處理同步方法的錯誤一樣。 先前的範例將 **RetrieveFeedAsync** 方法與 **await** 操作包裝在 **try/catch** 區塊中，以處理例外狀況擲回時的錯誤。

當非同步方法呼叫其他非同步方法時，任何造成例外狀況的非同步方法都將被傳播到外部方法中。 這表示您可以在最外層的方法中放入 **try/catch** 區塊，以攔截巢狀非同步方法的錯誤。 這和您攔截同步方法錯誤的方式類似。 不過，在 **catch** 區塊中不能使用 **await**。

**提示：**從 Microsoft Visual Studio 2005 中的 C# 開始，您可以在 **catch** 區塊中使用 **await**。

## 摘要與後續步驟

這裡示範的非同步方法呼叫模式，是當您在事件處理常式中呼叫非同步 API 時最容易使用的一種呼叫模式。 當您在某個會在 Visual Basic 中傳回 **void** 或 **Sub** 的覆寫方法中呼叫非同步方法時，也可以使用這種模式。

當您在 UWP 遇到非同步方法時，請記住下列幾點：

-   根據慣例，非同步方法的名稱會以 "Async" 結尾。
-   任何使用 **await** 運算子的方法都必須為其宣告標示 **async** 關鍵字。
-   當應用程式發現 **await** 運算子時，會在非同步方法執行的同時保持對使用者互動的回應。
-   等待非同步方法傳回的值傳回包含結果的物件。 在大部分情況下，有用的資訊是傳回值中的結果，而非傳回值本身。 您可以透過查看非同步方法的傳回類型，來尋找結果內包含的值類型。
-   使用非同步 API 與 **async** 模式通常可以提高應用程式回應性。

本主題中的範例會輸出如下文字。

``` syntax
Windows Experience Blog
PC Snapshot: Sony VAIO Y, 8/9/2011 10:26:56 AM -07:00
Tech Tuesday Live Twitter #Chat: Too Much Tech #win7tech, 8/8/2011 12:48:26 PM -07:00
Windows 7 themes: what’s new and what’s popular!, 8/4/2011 11:56:28 AM -07:00
PC Snapshot: Toshiba Satellite A665 3D, 8/2/2011 8:59:15 AM -07:00
Time for new school supplies? Find back-to-school deals on Windows 7 PCs and Office 2010, 8/1/2011 2:14:40 PM -07:00
Best PCs for blogging (or working) on the go, 8/1/2011 10:08:14 AM -07:00
Tech Tuesday – Blogging Tips and Tricks–#win7tech, 8/1/2011 9:35:54 AM -07:00
PC Snapshot: Lenovo IdeaPad U460, 7/29/2011 9:23:05 AM -07:00
GIVEAWAY: Survive BlogHer with a Sony VAIO SA and a Samsung Focus, 7/28/2011 7:27:14 AM -07:00
3 Ways to Stay Cool This Summer, 7/26/2011 4:58:23 PM -07:00
Getting RAW support in Photo Gallery & Windows 7 (…and a contest!), 7/26/2011 10:40:51 AM -07:00
Tech Tuesdays Live Twitter Chats: Photography Tips, Tricks and Essentials, 7/25/2011 12:33:06 PM -07:00
3 Tips to Go Green With Your PC, 7/22/2011 9:19:43 AM -07:00
How to: Buy a Green PC, 7/22/2011 9:13:22 AM -07:00
Windows 7 themes: the distinctive artwork of Cheng Ling, 7/20/2011 9:53:07 AM -07:00
```



<!--HONumber=Mar16_HO1-->


