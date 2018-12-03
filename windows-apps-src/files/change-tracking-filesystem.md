---
title: 追蹤在背景中的檔案系統變更
description: 說明如何追蹤檔案中的變更和在背景中的資料夾，因為使用者會將它們移動系統。
ms.date: 11/20/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e90692753924572a932767b9c188ed6d24f94593
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8459751"
---
# <a name="track-file-system-changes-in-the-background"></a>追蹤在背景中的檔案系統變更

[StorageLibraryChangeTracker](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)類別可讓使用者移動系統一樣追蹤變更的檔案和資料夾的應用程式。 使用**StorageLibraryChangeTracker**類別，可以追蹤應用程式：

- 檔案作業包括新增、 刪除、 修改。
- 例如在重新命名和刪除資料夾作業。
- 檔案和移動的磁碟機上的資料夾。

使用此指南來了解使用變更追蹤器程式設計模型、 檢視範例程式碼，並了解不同類型的檔案作業所**StorageLibraryChangeTracker**追蹤。

**StorageLibraryChangeTracker**會在本機電腦上運作的使用者程式庫，或任何資料夾。 這包括次要磁碟機或抽取式磁碟機，但不包含 NAS 磁碟機或網路磁碟機。

## <a name="using-the-change-tracker"></a>使用變更追蹤

變更追蹤系統上實作為循環緩衝區儲存的最後一個*N*檔案系統作業。 應用程式都能閱讀從緩衝區的變更，然後再將它們處理到他們自己的體驗。 一旦應用程式完成與變更它所做的變更會標示為已處理，並將永遠不會再次看到它們。

若要在資料夾上使用變更追蹤，請依照下列步驟：

1. 啟用變更追蹤的資料夾。
2. 等待的變更。
3. 讀取的變更。
4. 接受變更。

接下來的小節逐步解說每個步驟，以某些程式碼範例。 完整程式碼範例會提供文件的結尾。

### <a name="enable-the-change-tracker"></a>啟用變更追蹤

應用程式所需的第一件事是以告訴系統興趣變更追蹤特定的程式庫。 它會呼叫的[啟用](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)方法來變更追蹤上針對感興趣的程式庫。

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

幾個重要注意事項：

- 請確定您的應用程式建立[StorageLibrary](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)物件之前，先以正確的程式庫，在資訊清單中有權限。 如需詳細資訊，請參閱[檔案存取權限](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions)。
- [啟用](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)執行緒安全、 不會重設您的指標，並可呼叫您要 （詳情請更新版本） 的次數。

![啟用空白變更追蹤](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>等待的變更

變更追蹤初始化之後，它將會開始錄製中的所有應用程式未執行時，即使程式庫，內發生的作業。 應用程式可以登錄以啟用的隨時[StorageLibraryChangedTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)事件登錄沒有變更。

![不需讀取這些 app 新增到變更追蹤的變更](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>讀取所做的變更

應用程式可以接著輪詢變更追蹤的變更，並檢查其最後一次收到一份所做的變更。 下列程式碼示範如何取得一份變更從變更追蹤。

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

應用程式就負責處理到其本身的經驗或資料庫視所做的變更。

![變更追蹤從讀取所做的變更，到應用程式資料庫](images/changetracker-reading.png)

> [!TIP]
> 若要啟用的第二個呼叫是防禦競爭情形，如果使用者將新增另一個資料夾至媒體櫃時您的應用程式正在閱讀的變更。 如果沒有額外的呼叫來**啟用**程式碼會失敗 ecSearchFolderScopeViolation (0x80070490) 如果使用者已變更媒體櫃中資料夾

### <a name="accept-the-changes"></a>接受所做的變更

應用程式完成之後處理所做的變更，它應該會告訴系統永遠不會顯示這些變更藉由呼叫[AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync)方法。

```csharp
await changeReader.AcceptChangesAsync();
```

![標示為變更閱讀，這樣他們將永遠不會顯示一次](images/changetracker-accepting.png)

應用程式現在只會收到新的變更未來讀取變更追蹤時。

- 如果呼叫[ReadBatchAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync)和[AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync)之間有發生變更，將會在指標僅進階到應用程式已見最近的變更。 這些其他變更仍然可以使用它會呼叫**ReadBatchAsync**在下一次。
- 不接受所做的變更將導致系統以傳回相同的變更一組應用程式會呼叫**ReadBatchAsync**在下一次。

## <a name="important-things-to-remember"></a>請記住的重要事項

當使用變更追蹤時，有幾件事，您應該牢記以確定所有項目正常運作。

### <a name="buffer-overruns"></a>緩衝區溢位

雖然我們嘗試保留足夠的空間來保存發生系統上，直到您的應用程式可以讀取它們的所有作業變更追蹤時，它是很容易就能想像一下，其中應用程式不會讀取所做的變更之前的循環緩衝區會覆寫本身。 尤其是當使用者從備份還原資料或同步處理大型收藏的相片從相機手機。

在此情況下， **ReadBatchAsync**將會傳回的錯誤碼[StorageLibraryChangeType.ChangeTrackingLost](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype)。 如果您的應用程式會收到這個錯誤碼，表示幾件事：

* 緩衝區已覆寫本身您看最後一次。 最好是做法的重新編目程式庫，因為從追蹤器的任何資訊將會不完整。
* 變更追蹤器將不會傳回任何更多的變更，直到您呼叫[重設](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset)。 應用程式會呼叫重設之後，將指標移至最新的變更，而追蹤會繼續正常執行。

它應該是極少數以取得這些情況下，但在使用者移動大量的繞著其磁碟上的檔案的案例中，我們不希望球形並佔用太多儲存變更追蹤。 這應該可讓應用程式來反應大規模的檔案系統作業時不損壞 Windows 中的客戶體驗。

### <a name="changes-to-a-storagelibrary"></a>StorageLibrary 的變更

[StorageLibrary](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)類別存在為虛擬包含其他資料夾的根資料夾的群組。 為調解這與檔案系統變更追蹤，我們會將下列選項：

- Descendent 根媒體櫃資料夾的任何變更將會以變更追蹤。 您可以使用[資料夾](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders)屬性找到根媒體櫃資料夾。
- 新增或移除從**StorageLibrary** （透過[RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync)和[RequestRemoveFolderAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)） 的根資料夾不會建立一個項目中變更追蹤。 透過[DefinitionChanged](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged)事件或列舉中使用[資料夾](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders)屬性的程式庫的根資料夾，可以追蹤這些變更。
- 如果與已經在它的內容資料夾新增到媒體櫃，雖然仍不會變更通知或變更產生的追蹤器項目。 該資料夾的子系的任何後續變更將會產生通知，並變更追蹤器的項目。

### <a name="calling-the-enable-method"></a>呼叫啟用方法

應用程式應該呼叫[啟用](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)儘速他們開始追蹤檔案系統和之前所做的變更的每個列舉。 這樣可確保變更追蹤器會擷取所有的變更。  

## <a name="putting-it-together"></a>將它放在一起

以下是所有的程式碼，用來從視訊媒體櫃，變更登錄和啟動變更追蹤從提取所做的變更。

```csharp
private async void EnableChangeTracker()
{
    StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
    videoTracker.Enable();
}

private async void GetChanges()
{
    StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    videosLibrary.ChangeTracker.Enable();
    StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
    IReadOnlyList changeSet = await changeReader.ReadBatchAsync();


    //Below this line is for the blog post. Above the line is for the magazine
    foreach (StorageLibraryChange change in changeSet)
    {
        if (change.ChangeType == StorageLibraryChangeType.ChangeTrackingLost)
        {
            //We are in trouble. Nothing else is going to be valid.
            log("Resetting the change tracker");
            videosLibrary.ChangeTracker.Reset();
            return;
        }
        if (change.IsOfType(StorageItemTypes.Folder))
        {
            await HandleFileChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.File))
        {
            await HandleFolderChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.None))
        {
            if (change.ChangeType == StorageLibraryChangeType.Deleted)
            {
                RemoveItemFromDB(change.Path);
            }
        }
    }
    await changeReader.AcceptChangesAsync();
}
```
