---
Description: Learn how to store and retrieve local, roaming, and temporary app data.
title: 儲存和擷取設定及其他 App 資料
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a5e3a29a252b091b1e52dbea5fa7af5058488ed5
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8214186"
---
# <a name="store-and-retrieve-settings-and-other-app-data"></a>儲存及擷取設定和其他 app 資料

*app 資料*是特定 app 特有的可變動資料。 它包含執行階段狀態、使用者喜好設定及其他設定。 app 資料不同於*使用者資料*，後者是使用者在使用 app 時建立和管理的資料。 使用者資料包括文件或媒體檔案、電子郵件或通訊記錄，或保存使用者建立之內容的資料庫記錄。 使用者資料可能對於多個 app 都是實用或有意義的。 使用者資料通常是使用者想要當做與應用程式本身無關之實體來操作或傳送的資料 (如文件)。

**有關 app 資料的重要注意事項：** app 資料的存留期與 app 的存留期綁在一起。 如果移除應用程式，所有應用程式資料也會隨之遺失。 請勿使用 app 資料來儲存使用者資料或使用者可能認為重要且無法取代的任何資料。 建議以使用者的媒體櫃和 Microsoft OneDrive 來儲存這類資訊。 app 資料適合儲存 app 特定的使用者喜好設定、各種設定值和我的最愛。

## <a name="types-of-app-data"></a>app 資料類型


app 資料有兩種類型：設定和檔案。

-   **設定**

    您可以使用設定來儲存使用者喜好設定和 app 狀態資訊。 應用程式資料 API 可讓您輕鬆建立及擷取設定 (本文稍後將說明一些範例)。

    以下是可用於應用程式設定的資料類型：

    -   **UInt8**、**Int16**、**UInt16**、**Int32**、**UInt32**、**Int64**、**UInt64**、**Single**、**Double**
    -   **布林值**
    -   **Char16**、**String**
    -   [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)、[**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/br225996)
    -   **GUID**、[**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)、 [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)、[**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994)
    -   [**ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588)：一組必須個別序列化和還原序列化的相關 app 設定。 使用複合設定，可以輕鬆管理不可部分完成的互相依存設定更新。 在並行存取和漫遊期間，系統可確保複合設定的完整性。 複合設定是針對少量資料最佳化，如果針對大型資料集使用複合設定，可能會拖慢系統效能。
-   **檔案**

    使用檔案可儲存二進位資料，或啟用您自己的自訂序列化類型。

## <a name="storing-app-data-in-the-app-data-stores"></a>將 app 資料儲存在 app 資料存放區


安裝 app 時，系統會為 app 提供分別用於設定和檔案的個別使用者資料存放區。 您不需知道這個資料存在的位置與形式，因為系統會負責管理實體存放裝置，以確保資料會與其他 app 和其他使用者隔離。 當使用者安裝您的 app 更新時，系統也會保存這些資料存放區的內容，而當使用者解除安裝您的 app 時，系統會徹底地移除這些資料存放區的內容。

每個應用程式在它的應用程式資料存放區中，都有系統定義的數個根目錄：一個給本機檔案、一個給漫遊檔案，還有一個給暫存檔案。 您的應用程式可在這些根目錄中新增檔案與新容器。

## <a name="local-app-data"></a>本機應用程式資料


本機應用程式資料應該用於需要在應用程式工作階段之間保留，而且不適用應用程式資料漫遊的任何資訊。 不適用於其他裝置上的資料也應該儲存在這裡。 儲存的本機資料沒有一般大小限制。 對於不需漫遊的資料或大型資料集，請使用本機應用程式資料存放區。

### <a name="retrieve-the-local-app-data-store"></a>擷取本機應用程式資料存放區

您必須先擷取本機應用程式資料存放區，才可以讀取或寫入本機應用程式資料。 若要擷取本機 app 資料存放區，請使用 [**ApplicationData.LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 屬性取得 app 的本機設定，以做為 [**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599) 物件。 使用 [**ApplicationData.LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) 屬性來取得 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 物件中的檔案。 使用 [**ApplicationData.LocalCacheFolder**](https://msdn.microsoft.com/library/windows/apps/dn633825) 屬性來取得本機 app 資料存放區中的資料夾，讓您可以儲存不包含在備份與還原中的檔案。

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <a name="create-and-retrieve-a-simple-local-setting"></a>建立及擷取簡單的本機設定

若要建立或寫入設定，請使用 [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) 屬性存取上一個步驟中取得之 `localSettings` 容器的設定。 這個範例會建立名為 `exampleSetting` 的設定。

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

若要擷取設定，請使用您用來建立設定的相同 [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) 屬性。 此範例說明如何擷取我們剛建立的設定。

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <a name="create-and-retrieve-a-local-composite-value"></a>建立及擷取本機複合值

若要建立或寫入複合值，請建立 [**ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588) 物件。 這個範例會建立名為 `exampleCompositeSetting` 的複合設定，然後將它新增到 `localSettings` 容器。

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

localSettings.Values["exampleCompositeSetting"] = composite;
```

此範例說明如何擷取我們剛建立的複合值。

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

若要在本機 app 資料存放區中建立和更新檔案，請使用檔案 API，例如 [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) 和 [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505)。 這個範例會在 `localFolder` 容器中建立名為 `dataFile.txt` 的檔案，然後在這個檔案中寫入目前的日期與時間。 來自 [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) 列舉的 **ReplaceExisting** 值，指出檔案如果已經存在，就會取代它。

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

若要在本機 app 資料存放區中開啟和讀取檔案，請使用檔案 API，例如 [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272)、[**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 與 [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482)。 這個範例會開啟上一個步驟中建立的 `dataFile.txt` 檔案，然後讀取該檔案的日期。 如需從各種位置載入檔案資源的詳細資料，請參閱[如何載入檔案資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322)。

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


如果您在應用程式中使用漫遊資料，使用者就能夠跨多種裝置，輕鬆地讓您的應用程式資料保持同步。 如果使用者在多個裝置上安裝應用程式，作業系統會保持應用程式資料的同步，這樣能減少使用者在第二個裝置上需進行的應用程式設定工作量。 即使在不同的裝置上，使用者也可利用漫遊資料以繼續上次未完成的工作，像是撰寫清單。 作業系統會在更新時將漫遊資料複寫到雲端，然後將資料同步到安裝應用程式的其他裝置上。

作業系統會限制每個應用程式的應用程式資料漫遊大小。 請參閱 [**ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)。 如果應用程式到達這個限制，在應用程式的總應用程式資料漫遊再次少於這個限額之前，將不會再複寫任何應用程式資料到雲端。 基於這個原因，僅針對使用者喜好設定、連結以及小型的資料檔案使用漫遊資料是最佳做法。

使用者只要透過一些裝置，就能在需要的時間間隔內存取雲端中的應用程式漫遊資料。 如果使用者執行應用程式的時間少於這個時間間隔，漫遊資料就會從雲端中移除。 如果使用者解除安裝應用程式，漫遊資料會保存在雲端，而不會自動移除。 如果使用者在這段時間間隔內重新安裝 app，就會從雲端重新同步漫遊資料。

### <a name="roaming-data-dos-and-donts"></a>漫遊資料的可行與禁止事項

-   為使用者喜好設定和自訂、連結和小型資料檔案使用漫遊。 例如，使用漫遊在使用者的所有裝置上保留背景色彩喜好設定。
-   使用漫遊功能讓使用者可以繼續在不同的裝置完成工作。 例如，漫遊草稿電子郵件的內容或閱讀應用程式中最近檢視的頁面等應用程式資料。
-   透過更新應用程式資料的方式處理 [**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620) 事件。 當應用程式資料剛完成從雲端同步時，會發生此事件。
-   漫遊內容的參照而不是原始資料。 例如，漫遊 URL 而不是線上文章的內容。
-   對於重要且具時效性的設定，可使用與 [**RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624) 關聯的 *HighPriority* 設定。
-   不要漫遊裝置專屬的 app 資料。 有些資訊只與本機相關，如本機檔案資源的路徑名稱。 如果您決定漫遊本機資訊，請確認如果資訊在第二個裝置上無效時，應用程式可以復原它。
-   不要漫遊大量應用程式資料。 應用程式可以漫遊的應用程式資料數量有限；請使用 [**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625) 屬性取得此最大值。 如果應用程式達到這個限制，必須等到應用程式資料存放區的大小不再超過此限制時才能漫遊資料。 設計應用程式時，請考量如何在較大型資料中放置界限，以免超過該限制。 例如，如果個別儲存遊戲狀態需要 10KB，則應用程式只能允許使用者儲存最多 10 個遊戲。
-   不要在依賴立即同步化的資料使用漫遊。 Windows 不保證立即同步化；如果使用者處於離線狀態或使用高延遲網路，可能會嚴重延遲漫遊。 請確定您的 UI 不需要立即同步化。
-   不要使用漫遊經常變更的資料。 例如，如果您的應用程式追蹤經常變更的資訊，例如追蹤歌曲每秒的位置，請勿將它儲存為漫遊的應用程式資料。 請改為挑選較不頻繁但仍提供良好使用者體驗的表示法，像是目前正在播放的歌曲。

### <a name="roaming-pre-requisites"></a>漫遊的先決條件

如果使用者使用 Microsoft 帳戶登入裝置，便能享有漫遊應用程式資料的好處。 不過，使用者和群組原則系統管理員可以隨時關閉裝置上的漫遊應用程式資料。 如果使用者選擇不使用 Microsoft 帳戶或停用漫遊資料功能，還是可以使用您的應用程式，但是應用程式只有每個裝置的本機資料。

只有在使用者將裝置設成「受信任」時，才會轉換儲存在 [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) 中的資料。 如果裝置不受信任，保存在此保存庫中的資料將不會漫遊。

### <a name="conflict-resolution"></a>衝突解決方案

漫遊應用程式資料並不是為了要一次在多個裝置上同時使用。 如果同步化時因為兩個裝置上的特定資料單位變更而引發衝突，系統一律保留最後寫入的值。 這樣可確保應用程式使用的是最新資訊。 如果資料單位是一個設定複合項目，則在設定單位的層級仍然會發生衝突解決，這表示將會同步具有最新變更的複合項目。

### <a name="when-to-write-data"></a>寫入資料的時機

視設定的預期存留期而定，資料應該在不同時間寫入。 非經常變更或變更緩慢的應用程式資料應該立即寫入。 不過，經常變更的應用程式資料應該只在固定的時間間隔 (例如每 5 分鐘一次) 以及應用程式暫停時才定期寫入。 例如，音樂應用程式可以在開始播放新歌曲時寫入「目前的歌曲」設定，不過，會在暫停時才寫入歌曲的實際位置。

### <a name="excessive-usage-protection"></a>過度使用保護

系統備有各種適當的保護機制以避免不當使用資源。 如果應用程式資料未依預期方式轉換，有可能是因為裝置暫時受到限制。 等待一些時間通常能夠自動解決這種情況，不需要採取任何動作。

### <a name="versioning"></a>版本設定

應用程式資料可以使用版本設定，將某個資料結構升級為另一個資料結構。 這種版本號碼不同於應用程式版本，而且能隨意設定。 雖然不強制，但強烈建議使用遞增的版本號碼，因為在嘗試轉換至代表較新資料的較低資料版本號碼時，可能發生不必要的麻煩 (包括資料遺失)。

應用程式資料只能在具有相同版本號碼的已安裝應用程式之間漫遊。 例如，版本 2 的裝置可以彼此互相轉換資料，而版本 3 的裝置也可以同樣彼此轉換資料，但執行版本 2 與執行版本 3 的裝置之間則不會發生漫遊。 如果您安裝使用其他裝置上不同版本號碼的新應用程式，新安裝的應用程式將會同步與最高版本關聯的應用程式資料。

### <a name="testing-and-tools"></a>測試與工具

開發人員可以鎖定自己的裝置以觸發應用程式資料漫遊同步處理。 如果應用程式資料似乎未在某個時間範圍內轉換，請檢查並確定下列項目：

-   您的漫遊資料未超過大小上限 (如需詳細資訊，請參閱[**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625))。
-   您的檔案已正確關閉並釋放。
-   至少有兩個裝置執行相同的 app 版本。


### <a name="register-to-receive-notification-when-roaming-data-changes"></a>登錄以在漫遊資料變更時收到通知

若要使用應用程式資料漫遊，您必須登錄漫遊資料變更並擷取漫遊資料容器，以便讀取和寫入設定。

1.  登錄以在漫遊資料變更時收到通知。

    [**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620) 事件會在漫遊資料變更時通知您。 這個範例會將 `DataChangeHandler` 設定成處理漫遊資料變更的處理常式。

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

2.  取得應用程式各項設定和檔案的容器。

    使用 [**ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624) 屬性來取得設定，而使用 [**ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) 屬性來取得檔案。

```csharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <a name="create-and-retrieve-roaming-settings"></a>建立及擷取漫遊設定

使用 [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) 屬性存取上一節中取得之 `roamingSettings` 容器的設定。 此範例會建立名為 `exampleSetting` 的簡單設定和名為 `composite` 的複合值。

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

此範例會擷取我們剛建立的設定。

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

### <a name="create-and-retrieve-roaming-files"></a>建立及擷取漫遊檔案

若要在漫遊 app 資料存放區中建立和更新檔案，請使用檔案 API，例如 [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) 和 [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505)。 這個範例會在 `roamingFolder` 容器中建立名為 `dataFile.txt` 的檔案，然後在這個檔案中寫入目前的日期與時間。 來自 [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) 列舉的 **ReplaceExisting** 值，指出檔案如果已經存在，就會取代它。

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

若要在漫遊 app 資料存放區中開啟和讀取檔案，請使用檔案 API，例如 [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272)、[**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 與 [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482)。 這個範例會開啟上一節中建立的 `dataFile.txt` 檔案，然後讀取該檔案的日期。 如需從各種位置載入檔案資源的詳細資料，請參閱[如何載入檔案資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322)。

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


## <a name="temporary-app-data"></a>暫時的 app 資料


暫時的應用程式資料存放區的作用如同快取。 這個存放區的檔案不會漫遊，並且隨時可以移除。 系統維護工作可以隨時自動刪除儲存在這個位置的資料。 使用者也可以使用磁碟清理功能來清除暫時的資料存放區檔案。 暫時應用程式資料可以用來儲存應用程式工作階段期間的暫時資訊。 應用程式工作階段結束之後，這些資料不一定會繼續存留，因為系統可能會在需要時回收使用的空間。 透過 [**temporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) 屬性可取得此位置。

### <a name="retrieve-the-temporary-data-container"></a>擷取暫存資料容器

使用 [**ApplicationData.TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) 屬性來取得檔案。 下一個步驟會使用這個步驟的 `temporaryFolder` 變數。

```csharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;
```

### <a name="create-and-read-temporary-files"></a>建立和讀取暫存檔案

若要在暫時的 app 資料存放區中建立和更新檔案，請使用檔案 API，例如 [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) 和 [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505)。 這個範例會在 `temporaryFolder` 容器中建立名為 `dataFile.txt` 的檔案，然後在這個檔案中寫入目前的日期與時間。 來自 [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) 列舉的 **ReplaceExisting** 值，指出檔案如果已經存在，就會取代它。


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

若要在暫時的 app 資料存放區中開啟和讀取檔案，請使用檔案 API，例如 [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272)、[**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 及 [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482)。 這個範例會開啟上一個步驟中建立的 `dataFile.txt` 檔案，然後讀取該檔案的日期。 如需從各種位置載入檔案資源的詳細資料，請參閱[如何載入檔案資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322)。

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

## <a name="organize-app-data-with-containers"></a>使用容器組織應用程式資料


若要適當整理您的應用程式資料設定和檔案，您可以建立容器 (以 [**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599) 物件表示)，而無須直接使用目錄。 您可以將容器新增到本機、漫遊和暫存的應用程式資料存放區。 容器的巢狀深度最多可達 32 層。

若要建立設定容器，請呼叫 [**ApplicationDataContainer.CreateContainer**](https://msdn.microsoft.com/library/windows/apps/br241611) 方法。 此範例會建立名為 `exampleContainer` 的本機設定容器，然後新增名為 `exampleSetting` 的設定。 來自 [**ApplicationDataCreateDisposition**](https://msdn.microsoft.com/library/windows/apps/br241616) 列舉的 **Always** 值，指出如果還沒有容器，就會建立一個容器。

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


若要刪除您的 app 不再需要的簡單設定，請使用 [**ApplicationDataContainerSettings.Remove**](https://msdn.microsoft.com/library/windows/apps/br241608) 方法。 此範例會刪除我們稍早建立的 `exampleSetting` 本機設定。

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

若要刪除複合設定，請使用 [**ApplicationDataCompositeValue.Remove**](https://msdn.microsoft.com/library/windows/apps/br241597) 方法。 此範例會刪除我們在先前的範例中建立的 `exampleCompositeSetting` 複合設定。

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

若要刪除容器，請呼叫 [**ApplicationDataContainer.DeleteContainer**](https://msdn.microsoft.com/library/windows/apps/br241612) 方法。 此範例會刪除我們稍早建立的本機 `exampleContainer` 設定。

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <a name="versioning-your-app-data"></a>管理應用程式資料的版本


您可以根據您的需求，為應用程式建立應用程式資料版本。 如此一來，您就能建立應用程式資料格式會改變的應用程式後續版本，而不會造成與應用程式舊版本的相容性問題。 應用程式會檢查資料存放區中的應用程式資料版本，如果版本低於應用程式預期的版本，應用程式會將應用程式資料更新成新格式並更新版本。 如需詳細資訊，請參閱 [**Application.Version**](https://msdn.microsoft.com/library/windows/apps/br241630) 屬性與 [**ApplicationData.SetVersionAsync**](https://msdn.microsoft.com/library/windows/apps/hh701429) 方法。

## <a name="related-articles"></a>相關文章

* [**Windows.Storage.ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587)
* [**Windows.Storage.ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624)
* [**Windows.Storage.ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623)
* [**Windows.Storage.ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)
* [**Windows.Storage.ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588)
