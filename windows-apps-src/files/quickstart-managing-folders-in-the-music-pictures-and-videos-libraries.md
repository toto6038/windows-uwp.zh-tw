---
author: laurenhughes
ms.assetid: 1AE29512-7A7D-4179-ADAC-F02819AC2C39
title: 音樂、圖片及影片媒體櫃中的檔案和資料夾
description: 將現有的音樂、圖片或視訊資料夾新增到對應的媒體櫃中。 您也可以從媒體櫃中移除資料夾、取得媒體櫃中的資料夾清單，以及尋找已儲存的相片、音樂和影片。
ms.author: lahugh
ms.date: 06/18/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1859d758806b4e92758decb40b8a30d02acb254d
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5639316"
---
# <a name="files-and-folders-in-the-music-pictures-and-videos-libraries"></a>音樂、圖片及影片媒體櫃中的檔案和資料夾

將現有的音樂、圖片或視訊資料夾新增到對應的媒體櫃中。 您也可以從媒體櫃中移除資料夾、取得媒體櫃中的資料夾清單，以及尋找已儲存的相片、音樂和影片。

媒體櫃是一個虛擬資料夾集合，依預設會包含已知的資料夾，外加使用者透過您的 app 或其中一個內建 app 新增至媒體櫃的任何其他資料夾。 例如，圖片媒體櫃依預設會包含 [圖片] 這個已知資料夾。 使用者可以透過您的 app 或內建的 [相片] app，在圖片媒體櫃中新增或移除資料夾。

## <a name="prerequisites"></a>先決條件


-   **了解通用 Windows 平台 (UWP) App 的非同步程式設計**

    您可以參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)，以了解如何使用 C# 或 Visual Basic 撰寫非同步的 app。 若要了解如何使用 C++ 撰寫非同步的 App，請參閱 [C++ 的非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187334)。

-   **位置的存取權限**

    在 Visual Studio 中，於「資訊清單設計工具」中開啟 app 資訊清單檔案。 在 **\[功能\]** 頁面上，選取您應用程式所管理的媒體櫃。

    -   **音樂媒體櫃**
    -   **圖片媒體櫃**
    -   **視訊庫**

    若要深入了解，請參閱[檔案存取權限](file-access-permissions.md)。

## <a name="get-a-reference-to-a-library"></a>取得對媒體櫃的參考

> [!NOTE]
> 請記得宣告適當的功能。 如需詳細資訊，請參閱[應用程式功能宣告](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。
 

若要取得對使用者的 [音樂]、[圖片] 或 [影片] 媒體櫃的參考，請呼叫 [**StorageLibrary.GetLibraryAsync**](https://msdn.microsoft.com/library/windows/apps/dn251725) 方法。 從 [**KnownLibraryId**](https://msdn.microsoft.com/library/windows/apps/dn298399) 列舉提供對應的值。

-   [**KnownLibraryId.Music**](https://msdn.microsoft.com/library/windows/apps/br227155)
-   [**KnownLibraryId.Pictures**](https://msdn.microsoft.com/library/windows/apps/br227156)
-   [**KnownLibraryId.Videos**](https://msdn.microsoft.com/library/windows/apps/br227159)

```cs
var myPictures = await Windows.Storage.StorageLibrary.GetLibraryAsync(Windows.Storage.KnownLibraryId.Pictures);
```

## <a name="get-the-list-of-folders-in-a-library"></a>取得媒體櫃中資料夾的清單


若要取得媒體櫃中資料夾的清單，請取得 [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) 屬性的值。

```cs
using Windows.Foundation.Collections;
IObservableVector<Windows.Storage.StorageFolder> myPictureFolders = myPictures.Folders;
```

## <a name="get-the-folder-in-a-library-where-new-files-are-saved-by-default"></a>取得媒體櫃中預設儲存新檔案的資料夾


若要取得媒體櫃中預設儲存新檔案的資料夾，請取得 [**StorageLibrary.SaveFolder**](https://msdn.microsoft.com/library/windows/apps/dn251728) 屬性的值。

```cs
Windows.Storage.StorageFolder savePicturesFolder = myPictures.SaveFolder;
```

## <a name="add-an-existing-folder-to-a-library"></a>將現有的資料夾新增到媒體櫃

若要將資料夾新增至媒體櫃，您可以呼叫 [**StorageLibrary.RequestAddFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251726)。 以圖片媒體櫃為例，呼叫此方法時會隨即對使用者顯示資料夾選擇器，並出現 [**將此資料夾新增至圖片**] 按鈕。 如果使用者挑選一個資料夾，則該資料夾會保留在磁碟的原始位置上，且成為 [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) 屬性 (和內建的 [相片] app) 中的項目，但是該資料夾不會顯示為檔案總管中 [圖片] 資料夾的子項。


```cs
Windows.Storage.StorageFolder newFolder = await myPictures.RequestAddFolderAsync();
```

## <a name="remove-a-folder-from-a-library"></a>從媒體櫃中移除資料夾

若要從媒體櫃中移除資料夾，請呼叫 [**StorageLibrary.RequestRemoveFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251727) 方法，並指定要移除的資料夾。 您可以使用 [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) 和 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 控制項 (或類似項目)，讓使用者選取要移除的資料夾。

當您呼叫 [**StorageLibrary.RequestRemoveFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251727) 時，使用者會看到確認對話方塊，指出資料夾「不會再出現在 [圖片] 中，但也不會被刪除」。 這表示，資料夾仍保留在磁碟的原始位置上、已從 [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) 屬性中移除，且將不再包含在內建的 [相片] app 中。

下列範例假設使用者已從名為 **lvPictureFolders** 的 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 控制項中選取要移除的資料夾。


```cs
bool result = await myPictures.RequestRemoveFolderAsync(folder);
```

## <a name="get-notified-of-changes-to-the-list-of-folders-in-a-library"></a>取得媒體櫃中資料夾清單變更的通知


若要取得與媒體櫃中資料夾清單變更相關的通知，請為媒體櫃的 [**StorageLibrary.DefinitionChanged**](https://msdn.microsoft.com/library/windows/apps/dn251723) 事件註冊一個處理常式。


```cs
myPictures.DefinitionChanged += MyPictures_DefinitionChanged;

void HandleDefinitionChanged(Windows.Storage.StorageLibrary sender, object args)
{
    // ...
}
```

## <a name="media-library-folders"></a>媒體櫃資料夾


裝置會為使用者和 app 提供五個預先定義的位置來儲存媒體檔案。 內建 app 會將使用者建立的媒體和下載的媒體都儲存在這些位置。

這些位置包括：

-   **\[圖片\]** 資料夾。 包含圖片。

    -   **\[手機相簿\]** 資料夾。 包含內建相機中的相片和視訊。

    -   **\[儲存的圖片\]** 資料夾。 包含使用者從其他 app 儲存的圖片。

-   **\[音樂\]** 資料夾。 包含歌曲、播客和有聲書。

-   **\[影片\]** 資料夾。 包含視訊。

使用者或應用程式也可以將媒體檔案儲存在媒體櫃資料夾以外的 SD 記憶卡上。 若要尋找確實在 SD 記憶卡上的媒體檔案，請掃描 SD 記憶卡的內容，或要求使用者使用檔案選擇器來找出檔案。 如需詳細資訊，請參閱[存取 SD 記憶卡](access-the-sd-card.md)。

## <a name="querying-the-media-libraries"></a>查詢媒體櫃

若要取得檔案集合，指定媒體櫃及您要的檔案類型

```cs
using Windows.Storage;
using Windows.Storage.Search;

private async void getSongs()
{
    QueryOptions queryOption = new QueryOptions
        (CommonFileQuery.OrderByTitle, new string[] { ".mp3", ".mp4", ".wma" });

    queryOption.FolderDepth = FolderDepth.Deep;

    Queue<IStorageFolder> folders = new Queue<IStorageFolder>();

    var files = await KnownFolders.MusicLibrary.CreateFileQueryWithOptions
      (queryOption).GetFilesAsync();

    foreach (var file in files)
    {
        // do something with the music files
    }
}
```

### <a name="query-results-include-both-internal-and-removable-storage"></a>查詢結果同時包含內部和卸除式儲存空間

使用者可以選擇預設將檔案儲存到選用的 SD 記憶卡。 不過，應用程式可以選擇不允許將檔案儲存到 SD 記憶卡。 因此，媒體櫃可以分割到裝置的內部儲存空間及 SD 記憶卡上。

您不需要編寫其他程式碼即可處理這項操作。 [
              **Windows.Storage**
            ](https://msdn.microsoft.com/library/windows/apps/br227346) 命名空間中明確查詢已知資料夾的方法會結合來自這兩個位置的查詢結果。 您不需要在 app 資訊清單檔案中指定 **removableStorage** 功能，即可取得這些結合的結果。

考量下圖中裝置儲存空間的狀態：

![電話和 SD 記憶卡的影像](images/phone-media-locations.png)

如果您透過呼叫 `await KnownFolders.PicturesLibrary.GetFilesAsync()` 來查詢圖片媒體櫃的內容，結果會包含 internalPic.jpg 與 SDPic.jpg 兩者。


## <a name="working-with-photos"></a>使用相片

如果裝置的相機會同時儲存每張圖片的低解析度影像和高解析度影像，則深層查詢只會傳回低解析度影像。

[手機相簿] 和 [儲存的圖片] 資料夾不支援深層查詢。

**在拍攝相片的 app 中開啟該相片**

如果您想要讓使用者稍後可以在拍攝相片的 app 中再次開啟該相片，您可以使用與下列範例類似的程式碼，將 **CreatorAppId** 與相片的中繼資料儲存在一起。 在此範例中，**testPhoto** 是 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)。

```cs
IDictionary<string, object> propertiesToSave = new Dictionary<string, object>();

propertiesToSave.Add("System.CreatorOpenWithUIOptions", 1);
propertiesToSave.Add("System.CreatorAppId", appId);

testPhoto.Properties.SavePropertiesAsync(propertiesToSave).AsyncWait();   
```

## <a name="using-stream-methods-to-add-a-file-to-a-media-library"></a>使用串流方法將檔案新增到媒體櫃

當您使用已知資料夾來存取媒體櫃 (例如 **KnownFolders.PictureLibrary**)，並且使用串流方法將檔案新增到媒體櫃時，請務必關閉您的程式碼所開啟的所有串流。 否則，這些方法將無法如預期般將檔案新增到媒體櫃，因為至少還有一個串流仍然擁有檔案的控制代碼。

舉例來說，當您執行下列程式碼時，系統並不會將檔案新增到媒體櫃。 在 `using (var destinationStream = (await destinationFile.OpenAsync(FileAccessMode.ReadWrite)).GetOutputStreamAt(0))` 程式碼行中，**OpenAsync** 方法和**GetOutputStreamAt** 方法皆開啟了一個串流。 不過，由於 **using** 陳述式，系統只會處置 **GetOutputStreamAt** 方法所開啟的串流。 另一個串流會保持開啟，而導致無法儲存檔案。

```cs
StorageFolder testFolder = await StorageFolder.GetFolderFromPathAsync(@"C:\test");
StorageFile sourceFile = await testFolder.GetFileAsync("TestImage.jpg");
StorageFile destinationFile = await KnownFolders.CameraRoll.CreateFileAsync("MyTestImage.jpg");
using (var sourceStream = (await sourceFile.OpenReadAsync()).GetInputStreamAt(0))
{
    using (var destinationStream = (await destinationFile.OpenAsync(FileAccessMode.ReadWrite)).GetOutputStreamAt(0))
    {
        await RandomAccessStream.CopyAndCloseAsync(sourceStream, destinationStream);
    }
}
```

若要順利使用串流方法將檔案新增到媒體櫃，請務必關閉您的程式碼所開啟的所有串流，如下列範例所示。

```cs
StorageFolder testFolder = await StorageFolder.GetFolderFromPathAsync(@"C:\test");
StorageFile sourceFile = await testFolder.GetFileAsync("TestImage.jpg");
StorageFile destinationFile = await KnownFolders.CameraRoll.CreateFileAsync("MyTestImage.jpg");

using (var sourceStream = await sourceFile.OpenReadAsync())
{
    using (var sourceInputStream = sourceStream.GetInputStreamAt(0))
    {
        using (var destinationStream = await destinationFile.OpenAsync(FileAccessMode.ReadWrite))
        {
            using (var destinationOutputStream = destinationStream.GetOutputStreamAt(0))
            {
                await RandomAccessStream.CopyAndCloseAsync(sourceInputStream, destinationStream);
            }
        }
    }
}
```
