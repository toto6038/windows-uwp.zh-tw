---
title: 追蹤在背景中的檔案系統變更
description: 說明如何追蹤檔案中的變更，並在背景中的資料夾，當使用者會將它們移動系統。
ms.date: 12/19/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b0ec7762fd64f0f0b8de65faa1aaf079bdaba3a3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621573"
---
# <a name="track-file-system-changes-in-the-background"></a>追蹤在背景中的檔案系統變更

**重要的 Api**

-   [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)

[ **StorageLibraryChangeTracker** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)類別可讓應用程式，以追蹤使用者會將它們移動系統的檔案及資料夾中的變更。 使用**StorageLibraryChangeTracker**類別，可以追蹤應用程式：

- 檔案作業包括新增、 刪除、 修改。
- 資料夾作業，例如重新命名和刪除。
- 檔案和資料夾的磁碟機上移動。

使用本指南來了解使用變更追蹤程式的程式設計模型、 檢視一些範例程式碼，並了解不同類型的所追蹤的檔案作業**StorageLibraryChangeTracker**。

**StorageLibraryChangeTracker**運作在本機電腦上的使用者程式庫，或任何資料夾。 這包括次要磁碟機或抽取式磁碟機，但不包括 NAS 磁碟機或網路磁碟機。

## <a name="using-the-change-tracker"></a>使用變更追蹤程式

變更追蹤做為系統上儲存最後一個循環緩衝區*N*檔案系統作業。 應用程式可讀取的緩衝區上的變更，然後將它們處理至它們自己的經驗。 一旦應用程式已完成的變更，它標記為已處理的變更，並永遠不會再次看到它們。

若要使用變更追蹤程式的資料夾，請遵循下列步驟：

1. 啟用變更追蹤資料夾。
2. 等候變更。
3. 讀取變更。
4. 接受變更。

下一節中一一介紹一些程式碼範例的步驟。 在本文結尾會提供完整的程式碼範例。

### <a name="enable-the-change-tracker"></a>啟用變更追蹤程式

應用程式必須進行第一件事是告訴系統它所需要的變更追蹤給定程式庫。 其做法是呼叫[**啟用**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)上變更追蹤程式感興趣的程式庫的方法。

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

少數的重要注意事項：

- 請確定您的應用程式的正確的程式庫中的資訊清單有權建立之前[ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)物件。 請參閱[檔案的存取權限](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions)如需詳細資訊。
- [**啟用**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)是執行緒安全，不會重設您的指標，而且可以呼叫嗎 （更詳細的更新版本） 的次數。

![啟用空的變更追蹤程式](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>等候變更

變更追蹤程式初始化之後，它就會開始記錄的所有作業時所發生內程式庫，即使應用程式未執行。 應用程式可註冊來註冊有所變更隨時啟用[ **StorageLibraryChangedTrigger** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)事件。

![未讀取這些應用程式新增至變更追蹤的變更](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>讀取所做的變更

應用程式可以輪詢有變更，從 變更追蹤然後其簽入最後一次收到變更的清單。 下列程式碼示範如何從 變更追蹤程式取得變更的清單。

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

應用程式還負責處理將變更存入它自己的經驗或所需的資料庫。

![在應用程式資料庫中讀取變更追蹤所做的變更](images/changetracker-reading.png)

> [!TIP]
> 若要啟用第二個呼叫的是防範競爭條件，如果使用者會新增至程式庫的另一個資料夾時您的應用程式會讀取變更。 而沒有額外的呼叫**啟用**如果使用者變更其文件庫中的資料夾，將會失敗並 ecSearchFolderScopeViolation (0x80070490) 的程式碼

### <a name="accept-the-changes"></a>接受所做的變更

完成應用程式之後處理的變更，它應該會告知系統永遠不會顯示這些變更藉由呼叫[ **AcceptChangesAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync)方法。

```csharp
await changeReader.AcceptChangesAsync();
```

![標記變更為讀取，讓它們將永遠不會再顯示](images/changetracker-accepting.png)

應用程式現在只會收到新的變更讀取在未來的變更追蹤時。

- 如果變更發生錯誤的呼叫之間[ **ReadBatchAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync)並[AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync)，指標會變成僅限進階應用程式從未見過最新變更。 這些其他的變更仍然可以在下一次，它會呼叫**ReadBatchAsync**。
- 不接受所做的變更會導致系統傳回的相同一組變更下一個時間執行的應用程式呼叫**ReadBatchAsync**。

## <a name="important-things-to-remember"></a>重要的注意事項

當使用變更追蹤程式，有幾件事您應該謹記在心，以確定一切運作正常。

### <a name="buffer-overruns"></a>緩衝區溢位

雖然我們嘗試將保留足夠的空間來保存發生在系統上，直到您的應用程式可以讀取它們的所有作業的變更追蹤，就很容易就能想像的案例，其中應用程式不會讀取所做的變更之前的循環緩衝區會覆寫本身。 特別是如果使用者是從備份還原資料，或同步處理來自其照相手機圖片的大型集合。

在此情況下， **ReadBatchAsync**會傳回錯誤碼[ **StorageLibraryChangeType.ChangeTrackingLost**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype)。 如果您的應用程式收到此錯誤的程式碼，它會表示幾件事：

* 緩衝區，已覆寫本身您查看最後一次。 最好是做法的重新編目程式庫，因為追蹤程式之任何資訊將會不完整。
* 變更追蹤程式不會傳回任何其他變更，直到您呼叫[**重設**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset)。 這個應用程式會重設之後，指標將會移至最新的變更，並追蹤將會繼續正常運作。

它應該很少見，若要取得這些情況下，但在使用者移動大量的檔案其磁碟上的案例中，我們不想變更 tracker 快速擴充，不會佔用太多的儲存體。 這樣應該可讓大量的檔案系統作業回應時不會破壞 Windows 中的客戶體驗的應用程式。

### <a name="changes-to-a-storagelibrary"></a>StorageLibrary 的變更

[ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)虛擬群組包含其他資料夾的根資料夾的形式存在的類別。 若要協調這與檔案系統的變更追蹤程式，我們可以進行下列選擇：

- 變更追蹤程式中，將表示子系的根文件庫資料夾的任何變更。 可以使用找到的根文件庫資料夾[**資料夾**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders)屬性。
- 新增或移除根資料夾，從**StorageLibrary** (透過[ **RequestAddFolderAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync)並[ **RequestRemoveFolderAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) 將不會在變更追蹤器中建立項目。 這些變更可以透過追蹤[ **DefinitionChanged** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged)事件或列舉中的程式庫使用的根資料夾[**資料夾**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders)屬性。
- 如果已在其中的內容的資料夾新增至程式庫時，將不會有變更產生的通知，或是變更追蹤程式項目。 該資料夾的子系的任何後續變更會產生通知，並變更追蹤程式項目。

### <a name="calling-the-enable-method"></a>呼叫 Enable 方法

應用程式應該呼叫[**啟用**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)當他們開始追蹤的檔案系統，以及之前變更的每個列舉型別。 這可確保所有變更將會都擷取藉由變更追蹤程式。  

## <a name="putting-it-together"></a>將它放在一起

以下是用來註冊影片庫的變更並啟動提取所做的變更，從 變更追蹤程式的所有程式碼。

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
