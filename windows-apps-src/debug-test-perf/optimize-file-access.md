---
ms.assetid: 40122343-1FE3-4160-BABE-6A2DD9AF1E8E
title: 最佳化檔案存取
description: 建立可有效存取檔案系統的通用 Windows 平台 (UWP) 應用程式，避免因為磁碟延遲和記憶體/CPU 週期而發生效能問題。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: cacd915530bb599936730ec404a6e524fef0105d
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8205828"
---
# <a name="optimize-file-access"></a>最佳化檔案存取


建立可有效存取檔案系統的通用 Windows 平台 (UWP) app，避免因為磁碟延遲和記憶體/CPU 週期而發生效能問題。

當您想要存取大量的檔案，而且想要存取一般 Name、FileType 以及 Path 屬性以外的屬性值時，請透過建立 [**QueryOptions**](https://msdn.microsoft.com/library/windows/apps/BR207995) 存取屬性，並呼叫 [**SetPropertyPrefetch**](https://msdn.microsoft.com/library/windows/apps/hh973319)。 **SetPropertyPrefetch** 方法可以動態改善 app 的效能，以顯示從檔案系統取得的項目集合，例如影像的集合。 下一組範例顯示一些存取多個檔案的方法。

第一個範例使用 [**Windows.Storage.StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/BR227273) 來為一組檔案抓取名稱資訊。 這個方法提供良好的效能，因為這個範例只存取 name 屬性。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> 
> for (int i = 0; i < files.Count; i++)
> {
>     // do something with the name of each file
>     string fileName = files[i].Name;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) =
>     Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> 
> For i As Integer = 0 To files.Count - 1
>     ' do something with the name of each file
>     Dim fileName As String = files(i).Name
> Next i
> ```

第二個範例使用 [**Windows.Storage.StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/BR227273)，然後抓取每個檔案的影像屬性。 這個方法提供不佳的效能。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> for (int i = 0; i < files.Count; i++)
> {
>     ImageProperties imgProps = await files[i].Properties.GetImagePropertiesAsync();
> 
>     // do something with the date the image was taken
>     DateTimeOffset date = imgProps.DateTaken;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) = Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> For i As Integer = 0 To files.Count - 1
>     Dim imgProps As ImageProperties =
>         Await files(i).Properties.GetImagePropertiesAsync()
> 
>     ' do something with the date the image was taken
>     Dim dateTaken As DateTimeOffset = imgProps.DateTaken
> Next i
> ```

第三個範例使用 [**QueryOptions**](https://msdn.microsoft.com/library/windows/apps/BR207995) 來取得關於一組檔案的資訊。 這個方法提供比上一個範例更好的效能。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> // Set QueryOptions to prefetch our specific properties
> var queryOptions = new Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, null);
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView, 100,
>         ThumbnailOptions.ReturnOnlyIfCached);
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties, 
>        new string[] {"System.Size"});
> 
> StorageFileQueryResult queryResults = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions);
> IReadOnlyList<StorageFile> files = await queryResults.GetFilesAsync();
> 
> foreach (var file in files)
> {
>     ImageProperties imageProperties = await file.Properties.GetImagePropertiesAsync();
> 
>     // Do something with the date the image was taken.
>     DateTimeOffset dateTaken = imageProperties.DateTaken;
> 
>     // Performance gains increase with the number of properties that are accessed.
>     IDictionary<String, object> propertyResults =
>         await file.Properties.RetrievePropertiesAsync(
>               new string[] {"System.Size" });
> 
>     // Get/Set extra properties here
>     var systemSize = propertyResults["System.Size"];
> }
> ```
> ```vb
> ' Set QueryOptions to prefetch our specific properties
> Dim queryOptions = New Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, Nothing)
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView,
>             100, Windows.Storage.FileProperties.ThumbnailOptions.ReturnOnlyIfCached)
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties,
>                                  New String() {"System.Size"})
> 
> Dim queryResults As StorageFileQueryResult = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions)
> Dim files As IReadOnlyList(Of StorageFile) = Await queryResults.GetFilesAsync()
> 
> 
> For Each file In files
>     Dim imageProperties As ImageProperties = Await file.Properties.GetImagePropertiesAsync()
> 
>     ' Do something with the date the image was taken.
>     Dim dateTaken As DateTimeOffset = imageProperties.DateTaken
> 
>     ' Performance gains increase with the number of properties that are accessed.
>     Dim propertyResults As IDictionary(Of String, Object) =
>         Await file.Properties.RetrievePropertiesAsync(New String() {"System.Size"})
> 
>     ' Get/Set extra properties here
>     Dim systemSize = propertyResults("System.Size")
> 
> Next file
> ```
如果您在 Windows.Storage 物件上執行多項操作 (例如 `Windows.Storage.ApplicationData.Current.LocalFolder`)，請建立一個區域變數以參照該存放裝置來源，如此您就不必在每次存取該存放裝置時，重新建立中繼物件。

## <a name="stream-performance-in-c-and-visual-basic"></a>C# 和 Visual Basic 中的資料流效能

### <a name="buffering-between-uwp-and-net-streams"></a>UWP 與 .NET 資料流之間的緩衝

在許多情況下，您可能想要將 UWP 資料流 (例如 [**Windows.Storage.Streams.IInputStream**](https://msdn.microsoft.com/library/windows/apps/BR241718) 或 [**IOutputStream**](https://msdn.microsoft.com/library/windows/apps/BR241728)) 轉換為 .NET 資料流 ([**System.IO.Stream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.aspx))。 例如，當您在撰寫 UWP app 並希望使用 UWP 檔案系統搭配運行資料流的現有 .NET 程式碼時，這個做法很實用。 若要啟用此功能，適用於 UWP app 的.NET Api 會提供延伸方法，讓您在.NET 與 UWP 資料流類型之間轉換。 如需詳細資訊，請參閱 [**WindowsRuntimeStreamExtensions**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.aspx)。

將 UWP 資料流轉換為 .NET 資料流時，您實際上建立了基礎 UWP 資料流的配接器。 在某些情況下，會有與 UWP 資料流叫用方法相關的執行階段成本。 這可能會影響 app 的速度，尤其是在執行許多小型且經常性讀取或寫入作業的情況中更是如此。

為了加快 app 的速度，UWP 資料流配接器會包含一個資料緩衝區。 下列程式碼範例示範使用 UWP 資料流配接器搭配預設緩衝區大小的小型連續讀取。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFile file = await Windows.Storage.ApplicationData.Current
>     .LocalFolder.GetFileAsync("example.txt");
> Windows.Storage.Streams.IInputStream windowsRuntimeStream = 
>     await file.OpenReadAsync();
> 
> byte[] destinationArray = new byte[8];
> 
> // Create an adapter with the default buffer size.
> using (var managedStream = windowsRuntimeStream.AsStreamForRead())
> {
> 
>     // Read 8 bytes into destinationArray.
>     // A larger block is actually read from the underlying 
>     // windowsRuntimeStream and buffered within the adapter.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> 
>     // Read 8 more bytes into destinationArray.
>     // This call may complete much faster than the first call
>     // because the data is buffered and no call to the 
>     // underlying windowsRuntimeStream needs to be made.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> }
> ```
> ```vb
> Dim file As StorageFile = Await Windows.Storage.ApplicationData.Current -
> .LocalFolder.GetFileAsync("example.txt")
> Dim windowsRuntimeStream As Windows.Storage.Streams.IInputStream =
>     Await file.OpenReadAsync()
> 
> Dim destinationArray() As Byte = New Byte(8) {}
> 
> ' Create an adapter with the default buffer size.
> Dim managedStream As Stream = windowsRuntimeStream.AsStreamForRead()
> Using (managedStream)
> 
>     ' Read 8 bytes into destinationArray.
>     ' A larger block is actually read from the underlying 
>     ' windowsRuntimeStream and buffered within the adapter.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
>     ' Read 8 more bytes into destinationArray.
>     ' This call may complete much faster than the first call
>     ' because the data is buffered and no call to the 
>     ' underlying windowsRuntimeStream needs to be made.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
> End Using
> ```

將 UWP 資料流轉換為 .NET 資料流的大多數情況中，都可以使用這個預設緩衝行為。 不過，在某些情況下，您可能需要調整緩衝行為以增加效能。

### <a name="working-with-large-data-sets"></a>使用大型資料集

在轉譯或撰寫較大型資料集時，您可以在 [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx)、[**AsStreamForWrite**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforwrite.aspx) 及 [**AsStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx) 延伸方法提供大型緩衝區大小，增加讀取或寫入傳送量。 這可以提供資料流配接器較大的內部緩衝區大小。 例如，將大型檔案的資料流傳送到 XML 剖析器時，剖析器可對資料流進行許多循序的小型讀取。 大型緩衝區可以減少對基礎 UWP 資料流的呼叫數，並提升效能。

> **注意：** 請務必謹慎在設定緩衝區大小大於約 80 KB，因為這可能會導致記憶體回收行程堆積分散 （請參閱[改善記憶體回收效能](improve-garbage-collection-performance.md)）。 下列程式碼範例會建立具有 81,920 位元組緩衝區的管理資料流配接器。

> [!div class="tabbedCodeSnippets"]
```csharp
// Create a stream adapter with an 80 KB buffer.
Stream managedStream = nativeStream.AsStreamForRead(bufferSize: 81920);
```
```vb
' Create a stream adapter with an 80 KB buffer.
Dim managedStream As Stream = nativeStream.AsStreamForRead(bufferSize:=81920)
```

[**Stream.CopyTo**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.copyto.aspx) 和 [**CopyToAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.copytoasync.aspx) 方法也會配置本機緩衝區，以便在資料流之間進行複製。 對於 [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforread.aspx) 延伸方法，您可以覆寫預設的緩衝區大小，讓大型資料流複本獲得較佳效能。 下列程式碼範例會示範變更 **CopyToAsync** 呼叫的預設緩衝區大小。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> MemoryStream destination = new MemoryStream();
> // copies the buffer into memory using the default copy buffer
> await managedStream.CopyToAsync(destination);
> 
> // Copy the buffer into memory using a 1 MB copy buffer.
> await managedStream.CopyToAsync(destination, bufferSize: 1024 * 1024);
> ```
> ```vb
> Dim destination As MemoryStream = New MemoryStream()
> ' copies the buffer into memory using the default copy buffer
> Await managedStream.CopyToAsync(destination)
> 
> ' Copy the buffer into memory using a 1 MB copy buffer.
> Await managedStream.CopyToAsync(destination, bufferSize:=1024 * 1024)
> ```

此範例使用 1 MB 的緩衝區大小，這比先前建議的 80 KB 更大。 使用如此大的緩衝區，可增加超大型資料集 (亦即數百 MB) 的複製作業傳送量。 不過，此緩衝區是配置在大型物件堆積上，而且很可能會降低記憶體回收的效能。 您應該只在能顯著改善應用程式效能時，使用大型緩衝區大小。

當您同時使用大量資料流時，可能需要減少或消除緩衝區的記憶體負荷。 您可以指定較小的緩衝區，或將 *bufferSize* 參數設為 0，將該資料流配接器的緩衝完全關閉。 如果針對管理資料流執行大型讀取和寫入，仍可在沒有緩衝的情況下達到良好的傳送量效能。

### <a name="performing-latency-sensitive-operations"></a>執行延遲敏感作業

如果您需要低延遲的讀取和寫入，而且不希望在基礎 UWP 資料流讀取大型區塊時，也要避免緩衝。 例如，如果您使用網路通訊資料流，可能需要低延遲讀取和寫入。

在聊天應用程式中，您可在網路介面上使用資料流來回傳送訊息。 在此情況下，您要在訊息完成後立即傳送出去，而不是等到緩衝區滿了才傳送。 呼叫 [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforread.aspx)、[**AsStreamForWrite**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforwrite.aspx) 及 [**AsStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx) 延伸方法時，如果將緩衝區大小設定為 0，則產生的配接器將不會配置緩衝區，所有呼叫都會直接操作基礎 UWP 資料流。


