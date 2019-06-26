---
ms.assetid: 3604524F-112A-474F-B0CA-0726DC8DB885
title: 判斷 Microsoft OneDrive 檔案的可用性
description: 使用 StorageFile.IsAvailable 屬性判斷 Microsoft OneDrive 檔案是否可供使用。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: effb28fa453ec884152dbc404245f00f4893ef5a
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369419"
---
# <a name="determining-availability-of-microsoft-onedrive-files"></a>判斷 Microsoft OneDrive 檔案的可用性


**重要 API**

-   [**FileIO 類別**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) \(英文\)
-   [**StorageFile 類別**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) \(英文\)
-   [**StorageFile.IsAvailable 屬性**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) \(英文\)

使用 [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) 屬性判斷 Microsoft OneDrive 檔案是否可供使用。

## <a name="prerequisites"></a>必要條件

-   **了解通用 Windows 平台 (UWP) 應用程式的非同步程式設計**

    您可以參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)，以了解如何使用 C# 或 Visual Basic 撰寫非同步的 app。 若要了解如何使用 C++ 撰寫非同步的 App，請參閱 [C++ 的非同步程式設計](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。

-   **App 功能宣告**

    請參閱[檔案存取權限](file-access-permissions.md)。

## <a name="using-the-storagefileisavailable-property"></a>使用 StorageFile.IsAvailable 屬性

使用者可以將 OneDrive 檔案標示為可離線使用 (預設值) 或僅限線上存取。 這個功能可以讓使用者將大型檔案 (例如圖片或影片) 移到他們的 OneDrive、將檔案標示為僅限線上存取，以及節省磁碟空間 (本機保存的項目僅限中繼資料檔案)。

[**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) \(英文\)，可用來判斷檔案目前是否可用。 下表顯示 **StorageFile.IsAvailable** 屬性在各種案例中的值。

| 檔案類型                              | Online | 計量付費網路        | 離線 |
|-------------------------------------------|--------|------------------------|---------|
| 本機檔案                                | True   | True                   | True    |
| 標示為可離線使用的 OneDrive 檔案 | True   | True                   | True    |
| 標示為僅限線上存取的 OneDrive 檔案       | True   | 以使用者設定為基礎 | False   |
| 網路檔案                              | True   | 以使用者設定為基礎 | False   |

 

下列步驟說明如何判斷檔案目前是否可用。

1.  宣告某項功能適用於您想要存取的媒體櫃。
2.  包含 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) 命名空間。 這個命名空間包含管理檔案、資料夾及應用程式設定的類型。 其中也會包含所需的 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 類型。
3.  針對所需的檔案取得 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 物件。 如果您正在列舉媒體櫃，通常可呼叫 [**StorageFolder.CreateFileQuery**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfilequery) 方法，然後呼叫所產生之 [**StorageFileQueryResult**](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.StorageFileQueryResult) 物件的 [**GetFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync) 方法，來完成這個步驟。 **GetFilesAsync** 方法會傳回 **StorageFile** 物件的 [IReadOnlyList](https://go.microsoft.com/fwlink/p/?LinkId=324970) 集合。
4.  一旦您具備代表所需檔案之 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 物件的存取權後，[**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) 屬性的值就會反映檔案是否可供使用。

下列泛型方法說明如何列舉任意資料夾，並傳回該資料夾的 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 物件集合。 接著，呼叫方法會在參考每個檔案之 [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) 屬性的傳回集合上重複執行。

```cs
/// <summary>
/// Generic function that retrieves all files from the specified folder.
/// </summary>
/// <param name="folder">The folder to be searched.</param>
/// <returns>An IReadOnlyList collection containing the file objects.</returns>
async Task<System.Collections.Generic.IReadOnlyList<StorageFile>> GetLibraryFilesAsync(StorageFolder folder)
{
    var query = folder.CreateFileQuery();
    return await query.GetFilesAsync();
}

private async void CheckAvailabilityOfFilesInPicturesLibrary()
{
    // Determine availability of all files within Pictures library.
    var files = await GetLibraryFilesAsync(KnownFolders.PicturesLibrary);
    for (int i = 0; i < files.Count; i++)
    {
        StorageFile file = files[i];

        StringBuilder fileInfo = new StringBuilder();
        fileInfo.AppendFormat("{0} (on {1}) is {2}",
                    file.Name,
                    file.Provider.DisplayName,
                    file.IsAvailable ? "available" : "not available");
    }
}
```
