---
title: 追蹤在背景中的檔案系統變更
description: 說明當使用者在系統中移動檔案和資料夾時，如何在背景中追蹤其變更。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 15a1b05281215136afc75190bc3812811ee69ea8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173232"
---
# <a name="track-file-system-changes-in-the-background"></a>追蹤在背景中的檔案系統變更

**重要 API**

-   [**StorageLibraryChangeTracker**](/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](/uwp/api/windows.storage.storagelibrary)

當使用者在系統中移動檔案和資料夾時，[**StorageLibraryChangeTracker**](/uwp/api/Windows.Storage.StorageLibraryChangeTracker) 類別可讓應用程式在背景中追蹤其變更。 使用 **StorageLibraryChangeTracker** 類別，應用程式可以追蹤：

- 檔案作業包括新增、刪除、修改。
- 資料夾作業，例如重新命名和刪除。
- 在磁碟機上移動的檔案和資料夾。

使用本指南來了解使用變更追蹤器的程式設計模型、檢視一些範例程式碼，以及了解 **StorageLibraryChangeTracker** 所追蹤的不同檔案作業類型。

**StorageLibraryChangeTracker** 適用於使用者程式庫，或本機電腦上的任何資料夾。 這包括次要磁碟機或抽取式磁碟機，但不包括 NAS 磁碟機或網路磁碟機。

## <a name="using-the-change-tracker"></a>使用變更追蹤器

變更追蹤器會在系統上當作循環緩衝區實作，以供儲存最後 *N* 個檔案系統作業。 應用程式能夠讀取緩衝區的變更，然後將它們處理成自己的經驗。 應用程式完成變更後，它會將變更標示為已處理，且永遠不再看到它們。

若要在資料夾上使用變更追蹤器，請遵循下列步驟：

1. 啟用資料夾的變更追蹤。
2. 等候變更。
3. 讀取變更。
4. 接受變更。

後續幾節會使用一些程式碼範例逐步說明每個步驟。 本文結尾會提供完整的程式碼範例。

### <a name="enable-the-change-tracker"></a>啟用變更追蹤器

應用程式必須進行第一件事就是告知系統，它對給定程式庫的追蹤變更有興趣。 其做法是針對感興趣的程式庫，在變更追蹤器上呼叫 [**Enable**](/uwp/api/windows.storage.storagelibrarychangetracker.enable)方法。

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

一些重要注意事項：

- 在建立 [**StorageLibrary**](/uwp/api/windows.storage.storagelibrary) 物件之前，請確定應用程式具有資訊清單中正確程式庫的權限。 如需詳細資訊，請參閱[檔案存取權限](./file-access-permissions.md)。
- [**Enable**](/uwp/api/windows.storage.storagelibrarychangetracker.enable)為執行緒安全，不會重設您的指標，而且可以視需要呼叫許多次 (稍後有更詳細的說明)。

![啟用空的變更追蹤器](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>等候變更

在變更追蹤器初始化之後，它就會開始記錄程式庫內發生的所有作業，即使應用程式不在執行中。 應用程式可以註冊 [**StorageLibraryChangedTrigger**](/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) 事件，註冊為要在有變更時隨時啟動。

![這些應用程式未讀取新增至變更追蹤器的變更](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>讀取變更

應用程式可接著從變更追蹤器輪詢變更，並收到上次檢查後的變更清單。 以下程式碼示範如何從變更追蹤器取得變更清單。

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

應用程式會接著負責將變更處理為自己的體驗或資料庫 (視需要而定)。

![將變更追蹤器中的變更讀取到應用程式資料庫中](images/changetracker-reading.png)

> [!TIP]
> 如果使用者將另一個資料夾新增到程式庫，而您的應用程式同時在讀取變更，第二次呼叫 Enable 是要防範競爭情況。 如果使用者正在變更其程式庫中的資料夾，若沒有額外的 **Enable** 呼叫，此程式碼將會失敗並出現 ecSearchFolderScopeViolation (0x80070490)。

### <a name="accept-the-changes"></a>接受變更

應用程式完成變更處理之後，它應該呼叫 [**AcceptChangesAsync**](/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync)方法，告知系統永遠不再顯示這些變更。

```csharp
await changeReader.AcceptChangesAsync();
```

![將變更標示為已讀取，使其永遠不再顯示](images/changetracker-accepting.png)

應用程式現在只會收到未來讀取變更追蹤器時的新變更。

- 如果變更發生於呼叫 [**ReadBatchAsync**](/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) 和 [AcceptChangesAsync](/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) 之間，則指標會變成只前進到應用程式所看見的最近變更。 其他變更仍可在下一次呼叫 **ReadBatchAsync** 時取得。
- 不接受變更會導致應用程式下一次呼叫 **ReadBatchAsync** 時，系統傳回同一組變更。

## <a name="important-things-to-remember"></a>重要注意事項

在使用變更追蹤器時，有幾件事您應該謹記在心，以確保一切運作正常。

### <a name="buffer-overruns"></a>緩衝區溢位

雖然我們嘗試在變更追蹤器中保留足夠的空間來保存系統上發生的所有作業，直到您的應用程式可以讀取它們為止，但很容易就可以想像以下案例：應用程式不會讀取循環緩衝區覆寫本身之前的變更。 尤其是在使用者從備份還原資料，或同步處理來自其照相手機的大型相片集合時。

在此情況下，**ReadBatchAsync** 會傳回錯誤碼 [**StorageLibraryChangeType.ChangeTrackingLost**](/uwp/api/windows.storage.storagelibrarychangetype)。 如果您的應用程式收到此錯誤碼，則表示下列幾件事：

* 自從您最後一次查看緩衝區，緩衝區本身已覆寫。 最好的行動方案是將程式庫重新編目，因為來自追蹤器的任何資訊會不完整。
* 在您呼叫 [**Reset**](/uwp/api/windows.storage.storagelibrarychangetracker.reset)前，變更追蹤器不會傳回任何其他變更。 在應用程式呼叫 Reset 之後，指標將會移至最近的變更，且追蹤會繼續正常運作。

這些情況應該很少見，但是是在使用者在其磁碟上移動大量檔案的案例中，我們不希望變更追蹤器快速膨脹並用掉太多儲存體。 這應該可讓應用程式因應大量檔案系統作業，同時不會破壞 Windows 中的客戶體驗。

### <a name="changes-to-a-storagelibrary"></a>StorageLibrary 變更

[**StorageLibrary**](/uwp/api/windows.storage.storagelibrary) 類別是以根資料夾的虛擬群組形式存在，其中包含其他資料夾。 為了協調此類型與檔案系統變更追蹤器，我們進行了下列選擇：

- 根程式庫資料夾子系的任何變更都會表現在變更追蹤器中。 您可以使用 [**Folders**](/uwp/api/windows.storage.storagelibrary.folders) 屬性找到根程式庫資料夾。
- 從 **StorageLibrary** 新增或移除根資料夾 (透過 [**RequestAddFolderAsync**](/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) 和 [**RequestRemoveFolderAsync**](/uwp/api/windows.storage.storagelibrary.requestremovefolderasync))，並不會在變更追蹤器中建立項目。 您可以透過 [**DefinitionChanged**](/uwp/api/windows.storage.storagelibrary.definitionchanged) 事件，或使用 [**Folders**](/uwp/api/windows.storage.storagelibrary.folders) 屬性列舉程式庫中的根資料夾，藉此追蹤這些變更。
- 如果具有內容的資料夾已新增至程式庫，則不會產生變更通知或變更追蹤器項目。 該資料夾子系的任何後續變更都會產生通知和變更追蹤器項目。

### <a name="calling-the-enable-method"></a>呼叫 Enable 方法

應用程式應在開始追蹤檔案系統時且每次列舉變更之前，立即呼叫 [**Enable**](/uwp/api/windows.storage.storagelibrarychangetracker.enable) 。 這可確保變更追蹤器會擷取所有的變更。  

## <a name="putting-it-together"></a>將它放在一起

以下是用來註冊影片庫中的變更並開始從變更追蹤器提取變更的所有程式碼。

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