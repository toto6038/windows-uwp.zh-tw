---
author: laurenhughes
ms.assetid: 27914C0A-2A02-473F-BDD5-C931E3943AA0
title: "建立、寫入和讀取檔案"
description: "使用 StorageFile 物件讀取和寫入檔案。"
ms.author: lahugh
ms.date: 07/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: d566bac72b9e3a7e00adb9225342017168ca5f43
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/06/2017
---
# <a name="create-write-and-read-a-file"></a>建立、寫入和讀取檔案


\[ 針對 Windows10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**StorageFolder 類別**](https://msdn.microsoft.com/library/windows/apps/br227230)
-   [**StorageFile 類別**](https://msdn.microsoft.com/library/windows/apps/br227171)
-   [**FileIO 類別**](https://msdn.microsoft.com/library/windows/apps/hh701440)

使用 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件讀取和寫入檔案。

> [!NOTE] 
> 另請參閱[檔案存取範例](http://go.microsoft.com/fwlink/p/?linkid=619995)(英文)。

## <a name="prerequisites"></a>先決條件

-   **了解通用 Windows 平台 (UWP) App 的非同步程式設計**

    您可以參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)，以了解如何使用 C# 或 Visual Basic 撰寫非同步的 app。 若要了解如何使用 C++ 撰寫非同步的 App，請參閱 [C++ 的非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187334)。

-   **了解如何取得想要讀取、寫入或進行兩者的檔案**

    您可以在[使用選擇器開啟檔案和資料夾](quickstart-using-file-and-folder-pickers.md)中了解如何使用檔案選擇器取得檔案。

## <a name="creating-a-file"></a>建立檔案

以下說明如何在 app 本機資料夾中建立檔案。 如果已經存在，我們會取代它。

> [!div class="tabbedCodeSnippets"]
```cs  
// Create sample file; replace if exists.
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.CreateFileAsync("sample.txt",
        Windows.Storage.CreationCollisionOption.ReplaceExisting);
```
```cpp  
// Create a sample file; replace if exists.
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
concurrency::create_task(storageFolder->CreateFileAsync("sample.txt", CreationCollisionOption::ReplaceExisting));
```
```vb  
' Create sample file; replace if exists.
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.CreateFileAsync("sample.txt", CreationCollisionOption.ReplaceExisting)
```

## <a name="writing-to-a-file"></a>寫入檔案

以下說明如何使用 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 類別，將可寫入的檔案寫入磁碟。 寫入檔案常見的第一個步驟 (除非您在建立之後立即寫入) 是使用 [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 取得檔案。

> [!div class="tabbedCodeSnippets"]
```cs  
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```
```cpp  
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile) 
{
    // Process file
});
```
```vb  
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**將文字寫入檔案**

呼叫 [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440) 類別的 [**WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505) 方法，將文字寫入您的檔案。

> [!div class="tabbedCodeSnippets"]
```cs  
await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow");
```
```cpp 
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile) 
{
    //Write text to a file
    create_task(FileIO::WriteTextAsync(sampleFile, "Swift as a shadow"));
});
```
```vb  
Await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow")
```

**使用緩衝區將位元組寫入檔案 (2 個步驟)**

1.  首先，呼叫 [**ConvertStringToBinary**](https://msdn.microsoft.com/library/windows/apps/br241385) 以取得您要寫入檔案的位元組緩衝區 (根據任意字串)。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
            "What fools these mortals be", Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
    ```
    ```cpp  
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        // Create the buffer
        IBuffer^ buffer = CryptographicBuffer::ConvertStringToBinary
        ("What fools these mortals be", BinaryStringEncoding::Utf8);
    });
    ```
    ```vb  
    Dim buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
                        "What fools these mortals be",
                        Windows.Security.Cryptography.BinaryStringEncoding.Utf8)
    ```

2.  然後呼叫 [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440) 類別的 [**WriteBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701490) 方法，將緩衝區的位元組寫入您的檔案。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer);
    ```
    ```cpp  
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        // Create the buffer
        IBuffer^ buffer = CryptographicBuffer::ConvertStringToBinary
        ("What fools these mortals be", BinaryStringEncoding::Utf8);      
        // Write bytes to a file using a buffer
        create_task(FileIO::WriteBufferAsync(sampleFile, buffer));
    });
    ```
    ```vb  
    Await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer)
    ```

**使用串流將文字寫入檔案 (4 個步驟)**

1.  首先，呼叫 [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851) 方法來開啟檔案。 當開啟作業完成時，會傳回檔案內容的資料流。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
    ```
    ```cpp  
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        create_task(sampleFile->OpenAsync(FileAccessMode::ReadWrite)).then([sampleFile](IRandomAccessStream^ stream)
        {
            // Process stream
        });
    });
    ```
    ```vb  
    Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
    ```

2.  接著從 `stream` 呼叫 [**GetOutputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241738) 方法，以取得輸出資料流。 將它放在 **using** 陳述式中，即可管理輸出資料流的存留期。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var outputStream = stream.GetOutputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
    stream.Dispose(); // Or use the stream variable (see previous code snippet) with a using statement as well.
    ```
    ```cpp 
    // Add to "Process stream" in part 1
    IOutputStream^ outputStream = stream->GetOutputStreamAt(0);
    ```
    ```vb 
    Using outputStream = stream.GetOutputStreamAt(0)
    ' We'll add more code here in the next step.
    End Using
    ```

3.  現在，在現有的 **using** 陳述式中新增這個程式碼來寫入輸出資料流，方法是建立新的 [**DataWriter**](https://msdn.microsoft.com/library/windows/apps/br208154) 物件並呼叫 [**DataWriter.WriteString**](https://msdn.microsoft.com/library/windows/apps/br241642) 方法。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var dataWriter = new Windows.Storage.Streams.DataWriter(outputStream))
    {
        dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
    }
    ```
    ```cpp  
    // Added after code from part 2
    DataWriter^ dataWriter = ref new DataWriter(outputStream);
    dataWriter->WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
    ```
    ```vb  
    Dim dataWriter As New DataWriter(outputStream)
    dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.")
    ```

4.  最後，新增這個程式碼 (在內部 **using** 陳述式中) 以使用 [**StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171) 將文字儲存至您的檔案，以及使用 [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br241729) 關閉資料流。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    await dataWriter.StoreAsync();
        await outputStream.FlushAsync();
    ```
    ```cpp   
    // Added after code from part 3
    dataWriter->StoreAsync();
    outputStream->FlushAsync();
    ```
    ```vb  
    Await dataWriter.StoreAsync()
        Await outputStream.FlushAsync()
    ```

## <a name="reading-from-a-file"></a>讀取檔案

以下說明如何使用 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 類別從磁碟上的檔案讀取。 從檔案讀取常見的第一個步驟是使用 [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 取得檔案。

> [!div class="tabbedCodeSnippets"]
```cs  
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```
```cpp  
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
{
    // Process file
});
```
```vb  
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**從檔案讀取文字**

呼叫 [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440) 類別的 [**ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482) 方法，即可從檔案讀取文字。

> [!div class="tabbedCodeSnippets"]
```cs  
string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
```
```cpp  
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
{
    return FileIO::ReadTextAsync(sampleFile);
});
```
```vb  
Dim text As String = Await Windows.Storage.FileIO.ReadTextAsync(sampleFile)
```

**使用緩衝區從檔案讀取文字 (2 個步驟)**

1.  首先，呼叫 [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440) 類別的 [**ReadBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701468) 方法。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    var buffer = await Windows.Storage.FileIO.ReadBufferAsync(sampleFile);
    ```

    ```cpp  
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        return FileIO::ReadBufferAsync(sampleFile);

    }).then([](Streams::IBuffer^ buffer)
    {
        // Process buffer
    });
    ```

    ```vb  
    Dim buffer = Await Windows.Storage.FileIO.ReadBufferAsync(sampleFile)
    ```

2.  然後，使用 [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) 物件，先讀取緩衝區的長度，再讀取其內容。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var dataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer))
    {
        string text = dataReader.ReadString(buffer.Length);
    }
    ```
    ```cpp  
    // Add to "Process buffer" section from part 1
    auto dataReader = DataReader::FromBuffer(buffer);
    String^ bufferText = dataReader->ReadString(buffer->Length);
    ```
    ```vb  
    Dim dataReader As DataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer)
    Dim text As String = dataReader.ReadString(buffer.Length)
    ```

**使用串流從檔案讀取文字 (4 個步驟)**

1.  呼叫 [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851) 方法，開啟您檔案上的串流。 當作業完成時，會傳回檔案內容的串流。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.Read);
    ```
    ```cpp  
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        create_task(sampleFile->OpenAsync(FileAccessMode::Read)).then([sampleFile](IRandomAccessStream^ stream)
        {
            // Process stream
        });
    });
    ```
    ```vb  
    Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.Read)
    ```

2.  取得串流的大小以供稍後使用。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    ulong size = stream.Size;
    ```
    ```cpp  
    // Add to "Process stream" from part 1
    UINT64 size = stream->Size;
    ```
    ```vb  
    Dim size = stream.Size
    ```

3.  呼叫 [**GetInputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241737) 方法，以取得輸入資料流。 將它放在 **using** 陳述式中，即可管理資料流的存留期。 當您呼叫 **GetInputStreamAt** 時請指定 0，以將位置設定在資料流的開頭。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var inputStream = stream.GetInputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
    ```
    ```cpp  
    // Add after code from part 2
    IInputStream^ inputStream = stream->GetInputStreamAt(0);
    auto dataReader = ref new DataReader(inputStream);
    ```
    ```vb  
    Using inputStream = stream.GetInputStreamAt(0)
        ' We'll add more code here in the next step.
    End Using
    ```

4.  最後，在現有的 **using** 陳述式中新增這個程式碼，以取得資料流上的 [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) 物件，然後藉由呼叫 [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/br208135) 和 [**DataReader.ReadString**](https://msdn.microsoft.com/library/windows/apps/br208147) 來讀取文字。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var dataReader = new Windows.Storage.Streams.DataReader(inputStream))
    {
        uint numBytesLoaded = await dataReader.LoadAsync((uint)size);
        string text = dataReader.ReadString(numBytesLoaded);
    }
    ```
    ```cpp 
    // Add after code from part 3
    create_task(dataReader->LoadAsync(size)).then([sampleFile, dataReader](unsigned int numBytesLoaded)
    {
        String^ streamText = dataReader->ReadString(numBytesLoaded);
    });
    ```
    ```vb  
    Dim dataReader As New DataReader(inputStream)
    Dim numBytesLoaded As UInteger = Await dataReader.LoadAsync(CUInt(size))
    Dim text As String = dataReader.ReadString(numBytesLoaded)
    ```

 

 
