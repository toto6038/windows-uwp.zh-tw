---
title: 使用檔案
description: 了解如何使用通用 Windows 平台中的檔案。
ms.date: 05/01/2018
ms.topic: article
keywords: 開始使用, uwp, windows 10, 學習曲目, 檔案, 檔案 io, 讀取檔案, 撰寫檔案, 建立檔案, 寫入文字, 閱讀文字
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5480638e201dca8a5eb5363d7a5944422c626f67
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "66366894"
---
# <a name="work-with-files"></a>使用檔案

本主題涵蓋您需要了解在通用 Windows 平台 (UWP) 應用程式中開始讀取與寫入檔案。 介紹了主要 API 與類型，並提供連結有助於您了解更多資訊。

這不是教學課程。 如果您想要教學課程，請參閱[建立、寫入及讀取檔案](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files)，除了示範如何建立，讀取和寫入檔案以外，也示範如何使用緩衝區和串流。 您可能也會對[檔案存取範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)有興趣，其示範如何建立、讀取、寫入、複製和刪除檔案，以及如何擷取檔案屬性，並請記住檔案或資料夾，讓您的應用程式可以輕鬆地再次存取。

我們會查看程式碼從檔案寫入及讀取文字，以及如何存取應用程式的本機、漫遊，以及暫存資料夾。

## <a name="what-do-you-need-to-know"></a>您需要知道的事項

以下是您必須知道關於讀取或從/至檔案寫入文字的主要類型：

- [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) 代表檔案。 這個類別有屬性，可提供有關建立、開啟、複製、刪除、與重新命名檔案的檔案與方法相關資訊。
可用於處理字串路徑。 有一些採用字串路徑的 UWP API，但您更經常使用 **StorageFile** 代表檔案，因為在 UWP 中一些您使用的檔案可能沒有路徑，或可能會有雜亂無章的路徑。 使用 [StorageFile.GetFileFromPathAsync()](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync) 將路徑轉換為 **StorageFile**。 

- [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) 類別提供讀取和寫入文字的簡易方式。 這個類別也可以讀取/寫入位元組陣列，或緩衝區的內容。 這個類別與 [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 類別非常相似。 主要差異在於，它採用 **StorageFile**，而不是採用字串路徑作為 **PathIO**。
- [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) 代表資料夾 (目錄)。 這個類別有建立檔案、查詢資料夾內容、建立、重新命名，以及刪除資料夾和可提供資料夾相關資訊的屬性的方法。 

取得 **StorageFolder** 的常見方式包含：

- [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) 讓使用者瀏覽他們想要使用的資料夾。
- [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) 提供特定於本機應用程式資料夾之一的 **StorageFolder**，例如本機、漫遊，以及暫存資料夾。
- [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders) 提供 **StorageFolder** 用於已知的媒體櫃，例如 [音樂] 或 [圖片] 媒體櫃。

## <a name="write-text-to-a-file"></a>將文字寫入檔案

 針對此簡介，我們會著重在簡單案例上：讀取和寫入文字。 我們從查看一些使用 **FileIO** 類別將文字寫入檔案的程式碼開始。

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.CreateFileAsync("test.txt",
        Windows.Storage.CreationCollisionOption.OpenIfExists);

await Windows.Storage.FileIO.WriteTextAsync(file, "Example of writing a string\r\n");

// Append a list of strings, one per line, to the file
var listOfStrings = new List<string> { "line1", "line2", "line3" };
await Windows.Storage.FileIO.AppendLinesAsync(file, listOfStrings); // each entry in the list is written to the file on its own line.
```

首先指出檔案應該要位於哪裡。 `Windows.Storage.ApplicationData.Current.LocalFolder` 可讓您存取安裝應用程式時便已建立的本機資料資料夾。 如需應用程式可以存取的資料夾之相關詳細資訊，請參閱[存取檔案系統](#access-the-file-system)。

然後，使用 **StorageFolder** 來建立檔案 (或如果已經存在，便加以開啟)。

**FileIO** 類別提供方便將文字寫入檔案的方式。 `FileIO.WriteTextAsync()` 使用所提供的文字取代整個檔案內容。 `FileIO.AppendLinesAsync()` 將字串的集合附加到檔案，每一行寫入一個字串。

## <a name="read-text-from-a-file"></a>從檔案讀取文字

與寫入檔案一樣，從指定檔案的位置開始讀取檔案。 我們會使用如上述範例中相同的位置。 然後我們會使用 **FileIO** 類別讀取其內容。

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.GetFileAsync("test.txt");

string text = await Windows.Storage.FileIO.ReadTextAsync(file);
```

您也可以使用 `IList<string> contents = await Windows.Storage.FileIO.ReadLinesAsync(sampleFile);` 將檔案的每一行讀取至集合中的個別字串裡

## <a name="access-the-file-system"></a>存取檔案系統

在 UWP 平台中，限制資料夾存取權以確保使用者資料的完整性及隱私權。 

### <a name="app-folders"></a>應用程式資料夾

安裝 UWP 應用程式時，在 c:\users\<user name>\AppData\Local\Packages\<app package identifier>\ 底下建立幾個資料夾來儲存除其他內容外，應用程式的本機、漫遊與暫存檔案。 應用程式不需要宣告任何功能，便能存取這些資料夾，且其他應用程式無法存取這些資料夾。 解除安裝應用程式時，也會移除這些資料夾。

以下是您較常使用的應用程式資料夾：

- **LocalState**：用於目前裝置的資料本機。 備份裝置時，便可將此目錄中的資料儲存在 OneDrive 的備份圖像中。 如果使用者重設或取代裝置，則會還原資料。 使用 `Windows.Storage.ApplicationData.Current.LocalFolder.` 存取此資料夾，儲存您不想備份到您可以存取 `Windows.Storage.ApplicationData.Current.LocalCacheFolder` 的 **LocalCacheFolder** 中的 OneDrive 的本機資料。

- **RoamingState**：用於應在所有已安裝應用程式的裝置上複寫的資料。 Windows 限制漫遊的資料量，因此只在此儲存使用者設定和小檔案。 使用 `Windows.Storage.ApplicationData.Current.RoamingFolder` 存取漫遊資料夾。

- **TempState**：用於無法執行應用程式時，可能刪除的資料。 使用 `Windows.Storage.ApplicationData.Current.TemporaryFolder` 存取此資料夾。

### <a name="access-the-rest-of-the-file-system"></a>存取其餘檔案系統

UWP 應用程式必須宣告它要透過將對應功能新增至資訊清單來存取特定使用者媒體櫃的意圖。 當安裝應用程式來確認它們是否授權存取指定的媒體櫃時，提示使用者。 如果沒有，則無法安裝應用程式。 有存取相片、影片與音樂媒體櫃的功能。 如需完整清單，請參閱[應用程式功能宣告](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。 若要取得 **StorageFolder** 用於這些程式庫，請使用 [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders) 類別。

#### <a name="documents-library"></a>文件庫

雖然有存取使用者文件庫的功能，但該功能受限，其表示 Microsoft Store 拒絕應用程式宣告它，除非您依照程序取得特殊核准。 不是供一般使用。 請改用檔案或資料夾選擇器 (請參閱[使用選擇器開啟檔案和資料夾](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers)和[使用選擇器儲存檔案](https://docs.microsoft.com/windows/uwp/files/quickstart-save-a-file-with-a-picker))，讓使用者可瀏覽資料夾或檔案。 當使用者瀏覽資料夾或檔案時，隱含授予他們存取應用程式的權限且系統允許存取。

#### <a name="general-access"></a>標準存取

或者，您的應用程式可以宣告其資訊清單中受限制的 [broadFileSystem](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) 的功能，這也需要 Microsoft Store 的核准。 然後應用程式可以存取使用者有存取權的任何檔案，不須要檔案或資料夾選擇器的介入。

如需應用程式可以存取的位置完整清單，請參閱[檔案存取權限](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)。

## <a name="useful-apis-and-docs"></a>實用的 API 和文件

以下是 API 的快速摘要，以及其他實用的文件，有助於您開始使用檔案與資料夾。

### <a name="useful-apis"></a>實用的 API

| API | 描述 |
|------|---------------|
|  [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) | 提供有關建立、開啟、複製、刪除與重新命名檔案的檔案之方法相關資訊。 |
| [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) | 提供有關資料夾、建立檔案的方法，以及建立、重新命名檔案與刪除資料夾的方法的相關資訊。 |
| [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) |  提供讀取和寫入文字的簡易方式。 這個類別也可以讀取/寫入位元組陣列，或緩衝區的內容。 |
| [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio) | 提供讀取/寫入文字從/至指定字串路徑的檔案適用於檔案的簡單方法。 這個類別也可以讀取/寫入位元組陣列，或緩衝區的內容。 |
| [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) & [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) |  讀取和寫入緩衝區、位元組、整數、Guid、TimeSpans，以及更多從/至串流。 |
| [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) | 可讓您存取為應用程式所建立的資料夾，例如本機資料夾、漫遊資料夾，和暫存檔案資料夾。 |
| [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) |  可讓使用者選擇資料夾，並將 **StorageFolder** 傳回給它。 這是您如何存取預設下應用程式也無法存取的位置。 |
| [Windows.Storage.Pickers.FileOpenPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker) | 可讓使用者選擇檔案開啟與將 **StorageFolder** 傳回給它。 這是您如何存取預設下應用程式也無法存取的檔案。 |
| [Windows.Storage.Pickers.FileSavePicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker) | 可讓使用者選擇檔案的檔案名稱、副檔名和儲存位置。 傳回 **StorageFile**。 這是您如何將檔案儲存至預設下應用程式也無法存取的位置。 |
|  [Windows.Storage.Streams 命名空間](https://docs.microsoft.com/uwp/api/windows.storage.streams) | 涵蓋讀取和寫入串流。 尤其是，請查看 [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) 和 [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) 類別，其讀取和寫入緩衝區、位元組、整數、Guid、TimeSpans，以及更多。 |

### <a name="useful-docs"></a>實用的文件

| 主題 | 描述 |
|-------|----------------|
| [Windows.Storage 命名空間](https://docs.microsoft.com/uwp/api/windows.storage) | API 參考文件。 |
| [檔案、資料夾和媒體櫃](https://docs.microsoft.com/windows/uwp/files/) | 概念文件。 |
| [建立、寫入和讀取檔案](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files) | 涵蓋建立、讀取和寫入文字、二進位資料以及串流。 |
| [開始使用儲存本機應用程式資料](https://blogs.windows.com/buildingapps/2016/05/10/getting-started-storing-app-data-locally/#pCbJKGjcShh5DTV5.97) | 除了涵蓋儲存本機資料的最佳做法，還涵蓋了 LocalSettings 和 LocalCache 資料夾的用途。 |
| [開始使用漫遊應用程式資料](https://blogs.windows.com/buildingapps/2016/05/03/getting-started-with-roaming-app-data/#RgjgLt5OkU9DbVV8.97) | 有關如何使用漫遊應用程式資料的兩個系列。 |
| [漫遊應用程式資料的指導方針](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data) | 設計應用程式時，請遵循這些資料漫遊指導方針。 |
| [儲存及擷取設定和其他應用程式資料](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data) | 提供各種應用程式資料存放區的概觀，例如本機、漫遊，以及暫存資料夾。 如需指導方針與寫入裝置間漫遊的資料的其他相關資訊，請參閱[漫遊資料](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#roaming-data)章節。 |
| [檔案存取權限](https://docs.microsoft.com/windows/uwp/files/file-access-permissions) | 應用程式可以存取的檔案系統位置之相關資訊。 |
| [使用選擇器開啟檔案和資料夾](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) | 透過讓使用者藉由選擇器 UI 決定，來示範如何存取檔案和資料夾。 |
| [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) | 用來讀取和寫入串流的類型。 |
| [音樂、圖片及影片媒體櫃中的檔案和資料夾](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries) | 涵蓋如何從媒體櫃中移除資料夾、取得媒體櫃中的資料夾清單，以及尋找已儲存的相片、音樂和影片。 |

## <a name="useful-code-samples"></a>實用的程式碼範例

| 程式碼範例 | 描述 |
|-----------------|---------------|
| [應用程式資料範例](https://code.msdn.microsoft.com/windowsapps/ApplicationData-sample-fb043eb2) | 透過使用應用程式資料 API 來示範如何儲存和擷取每個使用者的特定資料。 |
| [檔案存取範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) | 示範如何建立、讀取、寫入、複製和刪除檔案。 |
| [檔案選擇器範例](https://code.msdn.microsoft.com/windowsapps/File-picker-sample-9f294cba) | 透過讓使用者使用 UI 選擇，來示範如何存取檔案和資料夾，以及如何儲存檔案，讓使用者可以指定要儲存的名稱、檔案類型和檔案的位置。 |
| [JSON 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Json) | 使用 [Windows.Data.Json 命名空間](https://docs.microsoft.com/uwp/api/Windows.Data.Json) 來示範如何編碼和解碼 JavaScript 物件標記法 (JSON) 物件、陣列、字串、數字和布林。 |
| [其他程式碼範例](https://developer.microsoft.com//windows/samples) | 在分類下拉式清單中選擇 [檔案、資料夾和媒體櫃]  。 |
