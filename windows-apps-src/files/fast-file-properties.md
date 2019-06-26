---
title: 快速存取 UWP 中的檔案屬性
description: 有效收集程式庫的檔案和其屬性清單以便用於 UWP 應用程式。
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp, 檔案, 屬性
ms.localizationpriority: medium
ms.openlocfilehash: 5ae884ca5424f50a7a835bc55602b5aa7c54096d
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "63799616"
---
# <a name="fast-access-to-file-properties-in-uwp"></a>快速存取 UWP 中的檔案屬性 

了解如何從程式庫快速收集檔案和其屬性的清單，並在應用程式中使用那些屬性。  

必要條件 
- **適用於通用 Windows 平台 (UWP) 應用程式的非同步程式設計**       您可以參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic) \(部分機器翻譯\)，以了解如何在 C# 或 Visual Basic 中撰寫非同步應用程式。 若要了解如何使用 C++ 撰寫非同步的 App，請參閱 [C++ 的非同步程式設計](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。 
- **針對程式庫的存取權限**   這些範例中的程式碼需要 **picturesLibrary** 功能，但是您的檔案位置可能需要其他功能，或不需要任何功能。 若要深入了解，請參閱[檔案存取權限](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)。 
- **簡易檔案列舉**    此範例使用 [QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) \(英文\) 來設定幾個進階列舉屬性。 若要深入了解僅取得小型目錄的簡易檔案清單，請參閱[列舉和查詢檔案和資料夾](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders) \(部分機器翻譯\)。 

## <a name="usage"></a>用途  
有許多應用程式都需要列出一組檔案的屬性，但不一定需要與那些檔案直接互動。 例如，某個音樂應用程式一次會播放 (開啟) 一個檔案，但它需要資料夾中所有檔案的屬性，好讓應用程式可以顯示歌曲佇列，或讓使用者可以選擇有效檔案來播放。 

此頁面上的範例不應用於會修改每個檔案的中繼資料，或是會與所有產生的 StorageFiles 進行讀取屬性以外之互動行為的應用程式。 如需詳細資訊，請參閱[列舉和查詢檔案和資料夾](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders) \(部分機器翻譯\)。 

## <a name="enumerate-all-the-pictures-in-a-location"></a>列舉某個位置中的所有圖片 
在此範例中，我們將
-  建置 [QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) \(英文\) 物件來指定應用程式想要盡快列舉檔案。
-  透過將 StorageFile 物件分頁至應用程式來擷取檔案屬性。 對檔案進行分頁可以降低應用程式所使用的記憶體，並改善其使用者所體驗到的回應性。

### <a name="creating-the-query"></a>建立查詢 
若要建置查詢，我們會使用 QueryOptions 物件來指定應用程式有興趣只列舉特定類型的影像檔案，並篩選掉受到 Windows 資訊保護 (System.Security.EncryptionOwners) 所保護的檔案。 

請務必使用 [QueryOptions.SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch)(英文\) 來設定應用程式將會存取的屬性。 如果應用程式存取未預先擷取的屬性，將會產生顯著的效能負面影響。

設定 [IndexerOption.OnlyUseIndexerAndOptimzeForIndexedProperties](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.IndexerOption) \(英文\) 會告知系統盡快傳回結果，但僅包含在 [SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch) \(英文\) 中所指定的屬性。 

### <a name="paging-in-the-results"></a>在結果中進行分頁 
使用者的圖片媒體櫃中可能會有上百萬個檔案，因此呼叫 [GetFilesAsync](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) \(英文\) 將會使其電腦無法負擔，因為它會為每個影像建立 StorageFile。 解決方法是一次建立固定數量的 StorageFiles，將它們處理至 UI，然後釋放記憶體。 

在我們的範例中，我們的做法是使用 [StorageFileQueryResult.GetFilesAsync(UInt32 StartIndex, UInt32 maxNumberOfItems)](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) \(英文\) 來一次僅擷取 100 個檔案。 應用程式接著會處理檔案，並允許作業系統隨後釋放記憶體。 此技巧會限制應用程式的記憶體上限，並確保系統能保持回應性。 當然，您必須根據自己的案例調整傳回的檔案數目，但為了確保所有使用者都能獲得具回應性的體驗，建議您一次不要擷取超過 500 個檔案。


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
結果的 StorageFile 檔案只會包含所要求的屬性，且相較於其他 IndexerOptions，其傳回的速度快了 10 倍。 應用程式仍然可以要求存取查詢中未包含的屬性，但開啟檔案並擷取那些屬性將會產生效能負面影響。  

## <a name="adding-folders-to-libraries"></a>將資料夾新增到媒體櫃 
應用程式可以要求使用者使用 [StorageLibrary.RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibrary.RequestAddFolderAsync) \(英文\) 來將位置新增至索引。 包含該位置之後，它會自動被編製索引，而應用程式可以使用此技巧來列舉檔案。
 
## <a name="see-also"></a>請參閱
[QueryOptions API 參考](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions) \(英文\)  
[列舉和查詢檔案和資料夾](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders)  
[檔案存取權限](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)  
 
 
