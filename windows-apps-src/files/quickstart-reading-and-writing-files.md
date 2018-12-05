---
ms.assetid: 27914C0A-2A02-473F-BDD5-C931E3943AA0
title: 建立、寫入和讀取檔案
description: 使用 StorageFile 物件讀取和寫入檔案。
ms.date: 06/28/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: 6079ea8ca844efc912b970c00c6907d98378dd07
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8705636"
---
# <a name="create-write-and-read-a-file"></a>建立、寫入和讀取檔案

**重要 API**

-   [**StorageFolder 類別**](/uwp/api/windows.storage.storagefolder)
-   [**StorageFile 類別**](/uwp/api/windows.storage.storagefile)
-   [**FileIO 類別**](/uwp/api/windows.storage.fileio)

使用 [**StorageFile**](/uwp/api/windows.storage.storagefile) 物件讀取和寫入檔案。

> [!NOTE]
> 另請參閱[檔案存取範例](http://go.microsoft.com/fwlink/p/?linkid=619995)(英文)。

## <a name="prerequisites"></a>先決條件

-   **了解通用 Windows 平台 (UWP) App 的非同步程式設計**

    您可以參閱[在 C# 或 Visual Basic 中呼叫非同步 API](/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)，以了解如何使用 C# 或 Visual Basic 撰寫非同步的 app。 了解如何撰寫非同步的 app 在 C + + /winrt，請參閱[並行和非同步作業，使用 C + + WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency)。 若要了解如何撰寫非同步的 app 在 C + + /CX，請參閱[非同步程式設計在 C + + /CX](/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。

-   **了解如何取得想要讀取、寫入或進行兩者的檔案**

    您可以在[使用選擇器開啟檔案和資料夾](quickstart-using-file-and-folder-pickers.md)中了解如何使用檔案選擇器取得檔案。

## <a name="creating-a-file"></a>建立檔案

以下說明如何在 app 本機資料夾中建立檔案。 如果已經存在，我們會取代它。

```csharp
// Create sample file; replace if exists.
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.CreateFileAsync("sample.txt",
        Windows.Storage.CreationCollisionOption.ReplaceExisting);
```

```cppwinrt
// MainPage.h
#include <winrt/Windows.Storage.h>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    // Create a sample file; replace if exists.
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    co_await storageFolder.CreateFileAsync(L"sample.txt", Windows::Storage::CreationCollisionOption::ReplaceExisting);
}
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

以下說明如何使用 [**StorageFile**](/uwp/api/windows.storage.storagefile) 類別，將可寫入的檔案寫入磁碟。 寫入檔案常見的第一個步驟 (除非您在建立之後立即寫入) 是使用 [**StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync) 取得檔案。

```csharp
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```

```cppwinrt
// MainPage.h
#include <winrt/Windows.Storage.h>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.CreateFileAsync(L"sample.txt", Windows::Storage::CreationCollisionOption::ReplaceExisting) };
    // Process sampleFile
}
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

藉由呼叫[**FileIO.WriteTextAsync**](/uwp/api/windows.storage.fileio.writetextasync)方法，以您的檔案寫入文字。

```csharp
await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow");
```

```cppwinrt
// MainPage.h
#include <winrt/Windows.Storage.h>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
    // Write text to the file.
    co_await Windows::Storage::FileIO::WriteTextAsync(sampleFile, L"Swift as a shadow");
}
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

1.  首先，呼叫以取得 （根據字串） 的位元組緩衝區[**CryptographicBuffer.ConvertStringToBinary**](/uwp/api/windows.security.cryptography.cryptographicbuffer.convertstringtobinary)您想要寫入檔案。

```csharp
var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
    "What fools these mortals be", Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
```

```cppwinrt
// MainPage.h
#include <winrt/Windows.Security.Cryptography.h>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
    // Create the buffer.
    Windows::Storage::Streams::IBuffer buffer{
        Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(
            L"What fools these mortals be", Windows::Security::Cryptography::BinaryStringEncoding::Utf8)};
    // The code in step 2 goes here.
}
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

2.  然後藉由呼叫[**FileIO.WriteBufferAsync**](/uwp/api/windows.storage.fileio.writebufferasync)方法撰寫從您的緩衝區至您的檔案位元組。

```csharp
await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer);
```

```cppwinrt
co_await Windows::Storage::FileIO::WriteBufferAsync(sampleFile, buffer);
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

1.  首先，呼叫 [**StorageFile.OpenAsync**](/uwp/api/windows.storage.storagefile.openasync) 方法來開啟檔案。 當開啟作業完成時，會傳回檔案內容的資料流。

```csharp
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```

```cppwinrt
// MainPage.h
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
    Windows::Storage::Streams::IRandomAccessStream stream{ co_await sampleFile.OpenAsync(Windows::Storage::FileAccessMode::ReadWrite) };
    // The code in step 2 goes here.
}
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

2.  接下來，透過呼叫從[**IRandomAccessStream.GetOutputStreamAt**](/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat)方法，以取得輸出資料流`stream`。 如果您使用 C#，然後括住這即可管理輸出資料流的存留期的**using**陳述式。 如果您使用[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，則您可以控制其存留期封入它在區塊中，或將它設定為`nullptr`當您完成使用它。

```csharp
using (var outputStream = stream.GetOutputStreamAt(0))
{
    // We'll add more code here in the next step.
}
stream.Dispose(); // Or use the stream variable (see previous code snippet) with a using statement as well.
```

```cppwinrt
Windows::Storage::Streams::IOutputStream outputStream{ stream.GetOutputStreamAt(0) };
// The code in step 3 goes here.
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

3.  現在將此新增到寫入輸出資料流，方法是建立新的[**DataWriter**](/uwp/api/windows.storage.streams.datawriter)物件，並呼叫[**DataWriter.WriteString**](/uwp/api/windows.storage.streams.datawriter.writestring)方法的程式碼 （如果您使用 C# 中，在現有的**using**陳述式內）。

```csharp
using (var dataWriter = new Windows.Storage.Streams.DataWriter(outputStream))
{
    dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
}
```

```cppwinrt
Windows::Storage::Streams::DataWriter dataWriter;
dataWriter.WriteString(L"DataWriter has methods to write to various types, such as DataTimeOffset.");
// The code in step 4 goes here.
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

4.  最後，將此新增到您的檔案，內含[**DataWriter.StoreAsync**](/uwp/api/windows.storage.streams.datawriter.storeasync)儲存文字，並關閉[**IOutputStream.FlushAsync**](/uwp/api/windows.storage.streams.ioutputstream.flushasync)與資料流的程式碼 （如果您使用 C# 中，**使用**內部陳述式中）。

```csharp
await dataWriter.StoreAsync();
await outputStream.FlushAsync();
```

```cppwinrt
dataWriter.StoreAsync();
outputStream.FlushAsync();
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

以下說明如何使用 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 類別從磁碟上的檔案讀取。 從檔案讀取常見的第一個步驟是使用 [**StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync) 取得檔案。

```csharp
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```

```cppwinrt
Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
// Process file
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

藉由呼叫[**FileIO.ReadTextAsync**](/uwp/api/windows.storage.fileio.readtextasync)方法，從檔案讀取文字。

```csharp
string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
```

```cppwinrt
Windows::Foundation::IAsyncOperation<winrt::hstring> ExampleCoroutineAsync()
{
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
    co_return co_await Windows::Storage::FileIO::ReadTextAsync(sampleFile);
}
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

1.  首先，呼叫[**FileIO.ReadBufferAsync**](/uwp/api/windows.storage.fileio.readbufferasync)方法。

```csharp
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(sampleFile);
```

```cppwinrt
Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
Windows::Storage::Streams::IBuffer buffer{ co_await Windows::Storage::FileIO::ReadBufferAsync(sampleFile) };
// The code in step 2 goes here.
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

2.  然後，使用 [**DataReader**](/uwp/api/windows.storage.streams.datareader) 物件，先讀取緩衝區的長度，再讀取其內容。

```csharp
using (var dataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer))
{
    string text = dataReader.ReadString(buffer.Length);
}
```

```cppwinrt
auto dataReader{ Windows::Storage::Streams::DataReader::FromBuffer(buffer) };
winrt::hstring bufferText{ dataReader.ReadString(buffer.Length()) };
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

1.  呼叫 [**StorageFile.OpenAsync**](/uwp/api/windows.storage.storagefile.openasync) 方法，開啟您檔案上的串流。 當作業完成時，會傳回檔案內容的串流。

```csharp
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.Read);
```

```cppwinrt
Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
Windows::Storage::Streams::IRandomAccessStream stream{ co_await sampleFile.OpenAsync(Windows::Storage::FileAccessMode::Read) };
// The code in step 2 goes here.
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

```csharp
ulong size = stream.Size;
```

```cppwinrt
uint64_t size{ stream.Size() };
// The code in step 3 goes here.
```

```cpp
// Add to "Process stream" from part 1
UINT64 size = stream->Size;
```

```vb
Dim size = stream.Size
```

3.  透過呼叫[**IRandomAccessStream.GetInputStreamAt**](/uwp/api/windows.storage.streams.irandomaccessstream.getinputstreamat)方法，以取得輸入資料流。 將它放在 **using** 陳述式中，即可管理資料流的存留期。 當您呼叫 **GetInputStreamAt** 時請指定 0，以將位置設定在資料流的開頭。

```csharp
using (var inputStream = stream.GetInputStreamAt(0))
{
    // We'll add more code here in the next step.
}
```

```cppwinrt
Windows::Storage::Streams::IInputStream inputStream{ stream.GetInputStreamAt(0) };
Windows::Storage::Streams::DataReader dataReader{ inputStream };
// The code in step 4 goes here.
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

4.  最後，在現有的 **using** 陳述式中新增這個程式碼，以取得資料流上的 [**DataReader**](/uwp/api/windows.storage.streams.datareader) 物件，然後藉由呼叫 [**DataReader.LoadAsync**](/uwp/api/windows.storage.streams.datareader.loadasync) 和 [**DataReader.ReadString**](/uwp/api/windows.storage.streams.datareader.readstring) 來讀取文字。

```csharp
using (var dataReader = new Windows.Storage.Streams.DataReader(inputStream))
{
    uint numBytesLoaded = await dataReader.LoadAsync((uint)size);
    string text = dataReader.ReadString(numBytesLoaded);
}
```

```cppwinrt
unsigned int cBytesLoaded{ co_await dataReader.LoadAsync(size) };
winrt::hstring streamText{ dataReader.ReadString(cBytesLoaded) };
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
