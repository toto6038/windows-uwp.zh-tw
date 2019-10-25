---
Description: 瞭解如何儲存和取出本機、漫遊和暫存的應用程式資料。
title: 儲存和抓取設定和其他應用程式資料
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0eb7ef49d0ce1876635dc36e84f43432c13e1791
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690361"
---
# <a name="store-and-retrieve-settings-and-other-app-data"></a>儲存和抓取設定和其他應用程式資料

*應用程式資料*是可變動的資料，由特定應用程式所建立及管理。 其中包含執行時間狀態、應用程式設定、使用者喜好設定、參考內容（例如字典應用程式中的字典定義），以及其他設定。 應用程式資料與*使用者資料*不同，使用者在使用應用程式時所建立和管理的資料。 使用者資料包括檔或媒體檔案、電子郵件或通訊文字記錄，或保存使用者所建立之內容的資料庫記錄。 使用者資料對於一個以上的應用程式而言可能很有用或有意義。 通常，這是使用者想要操作或傳輸為獨立于應用程式本身的實體（例如檔）的資料。

**關於應用程式資料的重要注意事項：** 應用程式資料的存留期會與應用程式的存留期相關聯。 如果應用程式已移除，所有的應用程式資料都會遺失。 請勿使用應用程式資料來儲存使用者資料，或使用者可能會感覺為有價值且無法取代的任何專案。 我們建議使用使用者的程式庫和 Microsoft OneDrive 來儲存這類資訊。 應用程式資料適合用來儲存應用程式特定的使用者喜好設定和 [我的最愛]。

## <a name="types-of-app-data"></a>應用程式資料的類型

有兩種類型的應用程式資料：設定和檔案。

### <a name="settings"></a>設定

使用 [設定] 來儲存使用者喜好設定和應用程式狀態資訊。 應用程式資料 API 可讓您輕鬆地建立和抓取設定（我們將在本文稍後說明一些範例）。

以下是您可以用於應用程式設定的資料類型：

- **UInt8**、 **Int16**、 **UInt16**、 **Int32**、 **UInt32**、 **Int64**、 **UInt64**、 **Single**、 **Double**
- **布林值**
- **Char16**，**字串**
- [**DateTime**](/uwp/api/Windows.Foundation.DateTime)、 [ **TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan)
    - 若C#為/.NET，請使用： [**system.object**](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0)、 [**TimeSpan**](/dotnet/api/system.timespan?view=dotnet-uwp-10.0)
- **GUID**、 [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)、 [**Size**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size)、 [**Rect**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect)
- [**ApplicationDataCompositeValue**](/uwp/api/Windows.Storage.ApplicationDataCompositeValue)：一組相關的應用程式設定，必須以不可部分完成的方式進行序列化和還原序列化。 使用複合設定，輕鬆處理相互相依設定的不可部分完成更新。 系統會在平行存取和漫遊期間，確保複合設定的完整性。 複合設定會針對少量資料進行優化，如果您將其用於大型資料集，效能可能會變差。

### <a name="files"></a>檔案

使用檔案來儲存二進位資料，或啟用您自己的自訂序列化類型。

## <a name="storing-app-data-in-the-app-data-stores"></a>將應用程式資料儲存在應用程式資料存放區


安裝應用程式時，系統會針對設定和檔案，提供它自己的每個使用者資料存放區。 您不需要知道此資料存在的位置或方式，因為系統負責管理實體儲存體，確保資料與其他應用程式和其他使用者保持隔離。 當使用者將更新安裝到您的應用程式時，系統也會保留這些資料存放區的內容，並在您的應用程式卸載時完整且完全移除這些資料存放區的內容。

在其應用程式資料存放區中，每個應用程式都有系統定義的根目錄：一個用於本機檔案，一個用於漫遊檔案，另一個用於暫存檔案。 您的應用程式可以將新的檔案和新容器新增至每個根目錄。

## <a name="local-app-data"></a>本機應用程式資料


本機應用程式資料應用於需要在應用程式會話之間保留的任何資訊，而不適合用于漫遊應用程式資料。 其他裝置上不適用的資料也應該儲存在這裡。 儲存的本機資料沒有一般大小限制。 將本機應用程式資料存放區用於漫遊和大型資料集沒有意義的資料。

### <a name="retrieve-the-local-app-data-store"></a>取出本機應用程式資料存放區

您必須先取出本機應用程式資料存放區，才能讀取或寫入本機應用程式資料。 若要取出本機應用程式資料存放區，請使用[**ApplicationData. LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings)屬性，以[**windows.storage.applicationdatacontainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer)物件的形式取得應用程式的本機設定。 使用[**ApplicationData. LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder)屬性來取得[**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)物件中的檔案。 使用[**ApplicationData. LocalCacheFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localcachefolder)屬性可取得本機應用程式資料存放區中的資料夾，您可以在其中儲存不包含在備份和還原中的檔案。

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <a name="create-and-retrieve-a-simple-local-setting"></a>建立和取出簡單的本機設定

若要建立或寫入設定，請使用[**windows.storage.applicationdatacontainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values)屬性來存取在上一個步驟中取得的 `localSettings` 容器中的設定。 這個範例會建立名為 `exampleSetting`的設定。

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

若要取得此設定，您可以使用您用來建立設定的相同[**Windows.storage.applicationdatacontainer 值**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values)屬性。 這個範例示範如何取出我們剛才建立的設定。

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <a name="create-and-retrieve-a-local-composite-value"></a>建立和取出本機複合值

若要建立或寫入複合值，請建立[**ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue)物件。 這個範例會建立名為 `exampleCompositeSetting` 的複合設定，並將它新增至 `localSettings` 容器。

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

localSettings.Values["exampleCompositeSetting"] = composite;
```

這個範例示範如何取出我們剛才建立的複合值。

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)localSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-read-a-local-file"></a>建立和讀取本機檔案

若要在本機應用程式資料存放區中建立及更新檔案，請使用檔案 Api，例如[**StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync)和[**FileIO. WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync)。 這個範例會在 `localFolder` 容器中建立名為 `dataFile.txt` 的檔案，並將目前的日期和時間寫入檔案。 [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption)列舉中的**ReplaceExisting**值表示要取代檔案（如果已經存在的話）。

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await localFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

若要開啟和讀取本機應用程式資料存放區中的檔案，請使用檔案 Api，例如[**StorageFolder. GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync)、 [**StorageFile GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)和[**windows. FileIO。** ](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). 這個範例會開啟在上一個步驟中建立的 `dataFile.txt` 檔案，並從檔案讀取日期。 如需從不同位置載入檔案資源的詳細資訊，請參閱[如何載入檔案資源](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10))。

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await localFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="roaming-data"></a>漫遊資料


如果您在應用程式中使用漫遊資料，您的使用者可以輕鬆地讓應用程式的應用程式資料在多個裝置之間保持同步。 如果使用者在多個裝置上安裝您的應用程式，作業系統會讓應用程式資料保持同步，減少使用者在第二個裝置上為您的應用程式所需執行的設定工作量。 漫遊也可以讓您的使用者繼續執行工作，例如撰寫清單，直接在不同裝置上離開。 作業系統會在更新時將漫遊資料複寫至雲端，並將資料同步處理至已安裝應用程式的其他裝置。

作業系統會限制每個應用程式可能漫遊的應用程式資料大小。 請參閱[**ApplicationData. RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota)。 如果應用程式達到此限制，應用程式的所有應用程式資料都不會複寫到雲端，直到應用程式的已漫遊應用程式資料總數小於限制為止。 基於這個理由，最佳做法是僅針對使用者喜好設定、連結和小型資料檔案使用漫遊資料。

只要使用者在所需的時間間隔內從某些裝置存取，就可以在雲端中使用應用程式的漫遊資料。 如果使用者未執行超過此時間間隔的應用程式，則會從雲端移除其漫遊資料。 如果使用者卸載應用程式，其漫遊資料不會自動從雲端移除，會保留。 如果使用者在時間間隔內重新安裝應用程式，則會從雲端同步處理漫遊資料。

### <a name="roaming-data-dos-and-donts"></a>漫遊資料的執行和不確定

- 針對使用者喜好設定和自訂、連結和小型資料檔案使用漫遊。 例如，使用漫遊來保留使用者在所有裝置上的背景色彩喜好設定。
- 使用漫遊可讓使用者在裝置之間繼續工作。 例如，漫遊應用程式資料，例如草稿電子郵件的內容或讀取器應用程式中最近看到的頁面。
- 藉由更新應用程式資料來處理[**DataChanged**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged)事件。 當應用程式資料剛完成從雲端同步處理時，就會發生此事件。
- 漫遊內容的參考，而不是原始資料。 例如，漫遊 URL，而不是線上文章的內容。
- 如需重要的時間緊迫設定，請使用與[**RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings)相關聯的*HighPriority*設定。
- 不要漫遊裝置特定的應用程式資料。 某些資訊僅與本機相關，例如本機檔案資源的路徑名稱。 如果您決定漫遊本機資訊，請確定在次要裝置上的資訊無效時，應用程式可以復原。
- 不要漫遊大型的應用程式資料集。 應用程式可以漫遊的應用程式資料量有限制;請使用[**RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota)屬性來取得此上限。 如果應用程式達到此限制，則在應用程式資料存放區的大小不再超過限制之前，不會漫遊資料。 當您設計應用程式時，請考慮如何將系結置於較大的資料，以免超過限制。 例如，如果儲存遊戲狀態需要10KB 每個，應用程式可能只允許使用者儲存10個遊戲。
- 請不要針對依賴即時同步的資料使用漫遊。 Windows 並不保證立即同步。如果使用者已離線或在高延遲的網路上，漫遊可能會明顯延遲。 請確定您的 UI 不會相依于立即同步處理。
- 請勿使用漫遊來進行經常變更的資料。 例如，如果您的應用程式追蹤經常變更的資訊，例如首次歌曲中的位置，請勿將此儲存為漫遊應用程式資料。 相反地，選擇較不頻繁的標記法，但仍提供良好的使用者體驗，例如目前播放的歌曲。

### <a name="roaming-pre-requisites"></a>漫遊必要條件

如果使用者使用 Microsoft 帳戶登入其裝置，則可以受益于漫遊應用程式資料。 不過，使用者和群組原則系統管理員可以隨時關閉裝置上的漫遊應用程式資料。 如果使用者選擇不使用 Microsoft 帳戶或停用漫遊資料功能，她仍然可以使用您的應用程式，但應用程式資料會在每個裝置的本機上。

只有當使用者將裝置設為「受信任」時，儲存在[**PasswordVault**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.PasswordVault)中的資料才會轉換。 如果裝置不受信任，此保存庫中受保護的資料將不會漫遊。

### <a name="conflict-resolution"></a>衝突解決

漫遊應用程式資料不適合同時在多個裝置上同時使用。 如果在同步處理期間發生衝突，因為兩個裝置上的特定資料單位已變更，系統一律會優先使用最後寫入的值。 這可確保應用程式會利用最新的資訊。 如果資料單位是設定複合，則設定單位的層級上仍然會發生衝突解決，這表示將會同步處理具有最新變更的複合。

### <a name="when-to-write-data"></a>寫入資料的時機

視設定的預期存留期而定，資料應該在不同的時間寫入。 應該立即寫入不常或緩慢變更的應用程式資料。 不過，經常變更的應用程式資料應僅定期寫入（例如每隔5分鐘一次），以及應用程式暫停時。 例如，當新的歌曲開始播放時，音樂應用程式可能會寫入「目前的歌曲」設定，不過，歌曲中的實際位置只能在暫停時寫入。

### <a name="excessive-usage-protection"></a>過度使用的保護

系統具有各種保護機制，以避免不當使用資源。 如果應用程式資料未如預期般轉換，可能是裝置暫時受到限制。 等候一段時間通常會自動解決這種情況，而且不需要採取任何動作。

### <a name="versioning"></a>版本控制

應用程式資料可以利用版本設定，從一個資料結構升級至另一個。 版本號碼與應用程式版本不同，而且可以設定為。 雖然不強制執行，但強烈建議您使用增加的版本號碼，因為如果您嘗試轉換成較低的資料版本號碼來表示較新的資料，就可能發生不想要的複雜問題（包括資料遺失）。

應用程式資料只會在使用相同版本號碼的已安裝應用程式之間漫遊。 例如，第2版上的裝置會在彼此之間轉換資料，而第3版的裝置則會執行相同的動作，但執行第2版的裝置與執行第3版的裝置之間不會進行漫遊。 如果您安裝的新應用程式使用了其他裝置上的各種版本號碼，新安裝的應用程式將會同步處理與最高版本號碼相關聯的應用程式資料。

### <a name="testing-and-tools"></a>測試和工具

開發人員可以鎖定其裝置，以觸發漫遊應用程式資料的同步處理。 如果應用程式資料似乎不會在特定時間範圍內轉換，請檢查下列專案，並確認：

- 您的漫遊資料不會超過大小上限（如需詳細資訊，請參閱[**RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota) ）。
- 您的檔案已關閉並正確地發行。
- 至少有兩個裝置正在執行相同版本的應用程式。


### <a name="register-to-receive-notification-when-roaming-data-changes"></a>註冊以在漫遊資料變更時收到通知

若要使用漫遊應用程式資料，您必須註冊漫遊資料變更並抓取漫遊資料容器，以便您可以讀取和寫入設定。

1.  註冊即可在漫遊資料變更時收到通知。

    當漫遊資料變更時， [**DataChanged**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged)事件會通知您。 這個範例會將 `DataChangeHandler` 設定為漫遊資料變更的處理常式。

```csharp
void InitHandlers()
    {
       Windows.Storage.ApplicationData.Current.DataChanged += 
          new TypedEventHandler<ApplicationData, object>(DataChangeHandler);
    }

    void DataChangeHandler(Windows.Storage.ApplicationData appData, object o)
    {
       // TODO: Refresh your data
    }
```

2.  取得應用程式設定和檔案的容器。

    使用[**ApplicationData. RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings)屬性來取得設定和[**ApplicationData. RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder)屬性以取得檔案。

```csharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <a name="create-and-retrieve-roaming-settings"></a>建立和取出漫遊設定

使用[**windows.storage.applicationdatacontainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values)屬性來存取上一節所取得 `roamingSettings` 容器中的設定。 這個範例會建立名為 `exampleSetting` 的簡單設定和名為 `composite`的複合值。

```csharp
// Simple setting

roamingSettings.Values["exampleSetting"] = "Hello World";
// High Priority setting, for example, last page position in book reader app
roamingSettings.values["HighPriority"] = "65";

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

roamingSettings.Values["exampleCompositeSetting"] = composite;

```

這個範例會抓取我們剛才建立的設定。

```csharp
// Simple setting

Object value = roamingSettings.Values["exampleSetting"];

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)roamingSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-retrieve-roaming-files"></a>建立和取出漫遊檔案

若要在漫遊應用程式資料存放區中建立及更新檔案，請使用檔案 Api，例如[**StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync)和[**FileIO. WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync)。 這個範例會在 `roamingFolder` 容器中建立名為 `dataFile.txt` 的檔案，並將目前的日期和時間寫入檔案。 [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption)列舉中的**ReplaceExisting**值表示要取代檔案（如果已經存在的話）。

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await roamingFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

若要在漫遊應用程式資料存放區中開啟和讀取檔案，請使用檔案 Api，例如[**StorageFolder. GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync)、 [**StorageFile GetFileFromApplicationUriAsync 和。** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) [**FileIO. ReadTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync)。 這個範例會開啟在上一節中建立的 `dataFile.txt` 檔案，並從檔案讀取日期。 如需從不同位置載入檔案資源的詳細資訊，請參閱[如何載入檔案資源](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10))。

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await roamingFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```


## <a name="temporary-app-data"></a>暫存應用程式資料


暫存應用程式資料存放區的運作方式就像快取一樣。 其檔案不會漫遊，而且隨時都可以移除。 系統維護工作可以隨時自動刪除儲存在此位置的資料。 使用者也可以使用 [磁片清理] 來清除暫存資料存放區中的檔案。 暫存應用程式資料可用於在應用程式會話期間儲存暫存資訊。 這項資料不保證會在應用程式會話結束後持續保存，因為系統可能會視需要回收已使用的空間。 此位置可透過[**temporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder)屬性取得。

### <a name="retrieve-the-temporary-data-container"></a>取出暫存資料容器

使用[**ApplicationData. TemporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder)屬性來取得檔案。 接下來的步驟會使用此步驟中的 `temporaryFolder` 變數。

```csharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;
```

### <a name="create-and-read-temporary-files"></a>建立和讀取暫存檔案

若要在暫存應用程式資料存放區中建立及更新檔案，請使用檔案 Api，例如[**StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync)和[**FileIO. WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync)。 這個範例會在 `temporaryFolder` 容器中建立名為 `dataFile.txt` 的檔案，並將目前的日期和時間寫入檔案。 [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption)列舉中的**ReplaceExisting**值表示要取代檔案（如果已經存在的話）。


```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await temporaryFolder.CreateFileAsync("dataFile.txt", 
       CreateCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

若要開啟和讀取暫存應用程式資料存放區中的檔案，請使用檔案 Api，例如[**StorageFolder. GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync)、 [**StorageFile GetFileFromApplicationUriAsync 和。** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) [**FileIO. ReadTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync)。 這個範例會開啟在上一個步驟中建立的 `dataFile.txt` 檔案，並從檔案讀取日期。 如需從不同位置載入檔案資源的詳細資訊，請參閱[如何載入檔案資源](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10))。

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await temporaryFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="organize-app-data-with-containers"></a>使用容器來組織應用程式資料


為了協助您組織應用程式資料設定和檔案，您可以建立容器（以[**windows.storage.applicationdatacontainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer)物件表示），而不是直接使用目錄。 您可以將容器新增至本機、漫遊和暫存的應用程式資料存放區。 容器最多可以嵌套到32層級。

若要建立設定容器，請呼叫[**windows.storage.applicationdatacontainer. CreateContainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.createcontainer)方法。 這個範例會建立名為 `exampleContainer` 的本機設定容器，並新增名為 `exampleSetting`的設定。 [**ApplicationDataCreateDisposition**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCreateDisposition)列舉中的**永遠**值表示如果容器不存在，則會建立該容器。

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Setting in a container
Windows.Storage.ApplicationDataContainer container = 
   localSettings.CreateContainer("exampleContainer", Windows.Storage.ApplicationDataCreateDisposition.Always);

if (localSettings.Containers.ContainsKey("exampleContainer"))
{
   localSettings.Containers["exampleContainer"].Values["exampleSetting"] = "Hello Windows";
}
```

## <a name="delete-app-settings-and-containers"></a>刪除應用程式設定和容器


若要刪除您的應用程式不再需要的簡單設定，請使用[**ApplicationDataContainerSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainersettings.remove)方法。 這個範例會 deletesthe 我們稍早建立 `exampleSetting` 本機設定。

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

若要刪除複合設定，請使用[**ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacompositevalue.remove)方法。 這個範例會刪除我們在先前範例中建立的本機 `exampleCompositeSetting` 複合設定。

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

若要刪除容器，請呼叫[**windows.storage.applicationdatacontainer. DeleteContainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.deletecontainer)方法。 這個範例會刪除稍早建立的本機 `exampleContainer` 設定容器。

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <a name="versioning-your-app-data"></a>版本控制您的應用程式資料


您可以選擇性地為應用程式提供應用程式資料的版本。 這可讓您建立應用程式的未來版本，以變更其應用程式資料的格式，而不會造成應用程式舊版的相容性問題。 應用程式會檢查資料存放區中的應用程式資料版本，如果版本小於應用程式所預期的版本，應用程式應該將應用程式資料更新為新格式，並更新版本。 如需詳細資訊，請參閱 ApplicationData 屬性和[**SetVersionAsync**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.setversionasync) [**方法。** ](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.version)

## <a name="related-articles"></a>相關文章

* [**ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData)
* [**ApplicationData. RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings)
* [**ApplicationData. RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder)
* [**ApplicationData. RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota)
* [**ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue)
