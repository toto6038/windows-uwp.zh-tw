---
author: laurenhughes
title: 快速存取 UWP 中的檔案屬性
description: 有效收集程式庫的檔案和其屬性清單以便用於 UWP app。
ms.author: lahugh
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 檔案, 屬性
ms.localizationpriority: medium
ms.openlocfilehash: e2f63e848820361a64a2a96348a8e1cc2419f233
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7285707"
---
# <a name="fast-access-to-file-properties-in-uwp"></a>快速存取 UWP 中的檔案屬性 

了解如何快速收集程式庫的檔案和其屬性清單，以便在應用程式中使用這些屬性。  

必要條件 
- **通用 Windows 平台 (UWP) 應用程式的非同步程式設計**    您可以了解如何在 C# 或 Visual Basic 撰寫非同步的 app，請參閱[呼叫非同步 Api 以 C# 或 Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)。 若要了解如何使用 C++ 撰寫非同步的 App，請參閱 [C++ 的非同步程式設計](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。 
- **程式庫的存取權限**這些範例中的程式碼需要**picturesLibrary**功能，但您的檔案位置可能需要其他功能或不需要功能完全。 若要深入了解，請參閱[檔案存取權限](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)。 
- **簡易檔案列舉**這個範例使用[QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions)來設定幾個進階的列舉屬性。 若要深入了解取得小型目錄的簡易檔案清單，請參閱[列舉和查詢檔案和資料夾](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders)。 

## <a name="usage"></a>使用方式  
許多應用程式需要列出一組檔案的屬性，但不一定需要與檔案直接互動。 例如，音樂應用程式一次播放 (開啟) 一個檔案，但它需要資料夾中所有檔案的屬性，好讓應用程式可以顯示歌曲佇列，或讓使用者可以選擇有效檔案來播放。 

此頁面上的範例不應用於會修改每個檔案或應用程式的中繼資料，並且會與所有產生的 StorageFiles 進行讀取屬性以外互動行為的應用程式。 如需詳細資訊，請參閱[列舉和查詢檔案和資料夾](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders)。 

## <a name="enumerate-all-the-pictures-in-a-location"></a>列舉位置中的所有圖片 
在此範例中，我們將
-  建置 [QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) 物件來指定應用程式想要盡快列舉檔案。
-  分頁 StorageFile 物件至應用程式來擷取檔案屬性。 分頁檔案可以降低應用程式所使用的記憶體，並改進其認知的回應性。

### <a name="creating-the-query"></a>建立查詢 
若要建置查詢，我們使用 QueryOptions 物件，指定應用程式有興趣只列舉特定類型的影像檔案以及篩選掉受到 Windows 資訊保護 (System.Security.EncryptionOwners) 所保護的檔案。 

請務必使用 [QueryOptions.SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch) 設定應用程式將會存取的屬性。 如果應用程式存取未預先擷取的屬性，會耗用大量效能。

設定 [IndexerOption.OnlyUseIndexerAndOptimzeForIndexedProperties](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.IndexerOption) 以告知系統盡快傳回結果，但僅包含 [SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch) 中所指定的屬性。 

### <a name="paging-in-the-results"></a>在結果中分頁 
使用者的圖片媒體櫃中可能會有成千上萬個檔案，因此呼叫 [GetFilesAsync](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) 會造成電腦負擔，因為它會為每個影像建立 StorageFile。 解決方法是一次建立固定數量的 StorageFiles，將它們處理成為 UI，再釋放記憶體。 

在我們的範例中，做法是使用 [(UInt32 StartIndex，UInt32 maxNumberOfItems) StorageFileQueryResult.GetFilesAsync](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) 一次僅擷取 100 個檔案。 接著應用程式會處理檔案，並允許作業系統隨後釋放記憶體。 這項技術限制了應用程式的記憶體上限，並確保系統仍然能夠回應。 當然，您需要根據您的狀況調整傳回的檔案數，但為了確保能對所有使用者提供有回應的體驗，建議您一次不要擷取超過 500 個檔案。


**範例**  
```csharp
StorageFolder folderToEnumerate = KnownFolders.PicturesLibrary; 
// Check if the folder is indexed before doing anything. 
IndexedState folderIndexedState = await folderToEnumerate.GetIndexedStateAsync(); 
if (folderIndexedState == IndexedState.NotIndexed || folderIndexedState == IndexedState.Unknown) 
{ 
    // Only possible in indexed directories.  
    return; 
} 
 
QueryOptions picturesQuery = new QueryOptions() 
{ 
    FolderDepth = FolderDepth.Deep, 
    // Filter out all files that have WIP enabled
    ApplicationSearchFilter = "System.Security.EncryptionOwners:[]", 
    IndexerOption = IndexerOption.OnlyUseIndexerAndOptimizeForIndexedProperties 
}; 

picturesQuery.FileTypeFilter.Add(".jpg"); 
string[] otherProperties = new string[] 
{ 
    SystemProperties.GPS.LatitudeDecimal, 
    SystemProperties.GPS.LongitudeDecimal 
}; 
 
picturesQuery.SetPropertyPrefetch(PropertyPrefetchOptions.BasicProperties | PropertyPrefetchOptions.ImageProperties, 
                                    otherProperties); 
SortEntry sortOrder = new SortEntry() 
{ 
    AscendingOrder = true, 
    PropertyName = "System.FileName" // FileName property is used as an example. Any property can be used here.  
}; 
picturesQuery.SortOrder.Add(sortOrder); 
 
// Create the query and get the results 
uint index = 0; 
const uint stepSize = 100; 
if (!folderToEnumerate.AreQueryOptionsSupported(picturesQuery)) 
{ 
    log("Querying for a sort order is not supported in this location"); 
    picturesQuery.SortOrder.Clear(); 
} 
StorageFileQueryResult queryResult = folderToEnumerate.CreateFileQueryWithOptions(picturesQuery); 
IReadOnlyList<StorageFile> images = await queryResult.GetFilesAsync(index, stepSize); 
while (images.Count != 0 || index < 10000) 
{ 
    foreach (StorageFile file in images) 
    { 
        // With the OnlyUseIndexerAndOptimizeForIndexedProperties set, this won't  
        // be async. It will run synchronously. 
        var imageProps = await file.Properties.GetImagePropertiesAsync(); 
 
        // Build the UI 
        log(String.Format("{0} at {1}, {2}", 
                    file.Path, 
                    imageProps.Latitude, 
                    imageProps.Longitude)); 
    } 
    index += stepSize; 
    images = await queryResult.GetFilesAsync(index, stepSize); 
} 
```

### <a name="results"></a>結果 
產生的 StorageFile 檔案只包含要求的屬性，但相較於其他 IndexerOptions，傳回速度快 10 倍。應用程式仍然可以要求存取查詢中未包含的屬性，但開啟檔案並擷取這些屬性會降低效能。  

## <a name="adding-folders-to-libraries"></a>將資料夾新增到媒體櫃 
應用程式可以要求使用者使用 [StorageLibrary.RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibrary.RequestAddFolderAsync) 將位置新增至索引。 包含位置之後，它將會自動編製索引，應用程式即可使用這項技術來列舉檔案。
 
## <a name="see-also"></a>請參閱
[QueryOptions API 參考](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions)  
[列舉和查詢檔案和資料夾](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders)  
[檔案存取權限](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)  
[快速屬性存取逐步解說](https://blogs.msdn.microsoft.com/adamdwilson/2017/12/20/fast-file-enumeration-with-partially-initialized-storagefiles/)
 
 
