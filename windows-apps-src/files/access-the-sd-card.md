---
author: laurenhughes
ms.assetid: CAC6A7C7-3348-4EC4-8327-D47EB6E0C238
title: "存取 SD 記憶卡"
description: "您可以在選用的 microSD 記憶卡上儲存和存取非必要的資料，尤其是內部儲存空間有限的低價行動裝置。"
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 3fc8bbaa0b665b640974b5342b2b60c9b7f90143
ms.lasthandoff: 02/07/2017

---
# <a name="access-the-sd-card"></a>存取 SD 記憶卡

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


您可以在選用的 microSD 記憶卡上儲存和存取非必要的資料，尤其是內部儲存空間有限的低價行動裝置。

在大部分情況下，您必須先在應用程式資訊清單檔案中指定 **removableStorage** 功能，App 才能儲存和存取 SD 記憶卡上的檔案。 通常您還必須登錄 App 所儲存和存取的檔案類型，以便處理這些檔案類型。

您可以藉由使用下列方法，在選用的 SD 記憶卡上儲存和存取檔案：

- 檔案選擇器。

- [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) API。

## <a name="what-you-can-and-cant-access-on-the-sd-card"></a>SD 記憶卡上可存取和不可存取的項目

### <a name="what-you-can-access"></a>可以存取的項目

- 您的應用程式只能讀寫已在應用程式資訊清單檔案中登錄為可處理的檔案類型。

- 您的應用程式也可以建立和管理資料夾。

### <a name="what-you-cant-access"></a>不可以存取的項目

- 您的應用程式看不到也不能存取系統資料夾以及其中包含的檔案。

- 您的應用程式看不到已標記隱藏屬性的檔案。 隱藏屬性一般用來降低意外刪除資料的風險。

- 您的應用程式無法使用 [**KnownFolders.DocumentsLibrary**](https://msdn.microsoft.com/library/windows/apps/br227152) 來查看或存取 [文件] 媒體櫃。 不過，您可以藉由周遊檔案系統來存取 SD 記憶卡上的 [文件] 媒體櫃。

## <a name="security-and-privacy-considerations"></a>安全性和隱私權考量

當 app 將檔案儲存在 SD 記憶卡上的通用位置時，並不會加密這些檔案，因此其他 app 通常可以存取它們。

- 裝置中有 SD 記憶卡時，已登錄可處理相同檔案類型的其他 app 可以存取您的檔案。

- 從裝置移除 SD 記憶卡並從電腦開啟時，可以在檔案總管中看到您的檔案，其他應用程式也可以存取它們。

不過，當安裝在 SD 記憶卡上的應用程式將檔案儲存在其 [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) 中時，這些檔案會受到加密，而無法供其他應用程式存取。

## <a name="requirements-for-accessing-files-on-the-sd-card"></a>存取 SD 記憶卡上檔案的需求

若要存取 SD 記憶卡上的檔案，您通常必須指定下列各項。

1.  您必須在應用程式資訊清單檔案中指定 **removableStorage** 功能。
2.  您還必須將與您想要存取之媒體類型關聯的副檔名登錄為可處理的副檔名。

前述方法也可讓您不需參考已知資料夾 (例如 **KnownFolders.MusicLibrary**) 即可存取 SD 記憶卡上的媒體檔案，或是存取儲存在媒體櫃資料夾外的媒體檔案。

若要使用已知資料夾來存取儲存在媒體櫃 ([音樂]、[相片] 或 [影片]) 中的媒體檔案，您只需要在應用程式資訊清單檔案中指定相關的功能 (**musicLibrary**、**picturesLibrary** 或 **videoLibrary**) 即可。 您不需要指定 **removableStorage** 功能。 如需詳細資訊，請參閱[音樂、圖片及影片媒體櫃中的檔案和資料夾](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。

## <a name="accessing-files-on-the-sd-card"></a>存取 SD 記憶卡上的檔案

### <a name="getting-a-reference-to-the-sd-card"></a>取得 SD 記憶卡的參照

[**KnownFolders.RemovableDevices**](https://msdn.microsoft.com/library/windows/apps/br227158) 資料夾是目前與裝置連接之一組卸除式裝置的邏輯根 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)。 如果有 SD 記憶卡，**KnownFolders.RemovableDevices** 資料夾下的第一個 (也是唯一一個) **StorageFolder** 代表 SD 記憶卡。

使用與下面類似的程式碼，判斷是否有 SD 記憶卡，並取得它的參照當做 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)。

```csharp
using Windows.Storage;

...

            // Get the logical root folder for all external storage devices.
            StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

            // Get the first child folder, which represents the SD card.
            StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

            if (sdCard != null)
            {
                // An SD card is present and the sdCard variable now contains a reference to it.
            }
            else
            {
                // No SD card is present.
            }
```

### <a name="querying-the-contents-of-the-sd-card"></a>查詢 SD 記憶卡的內容

SD 記憶卡可能包含許多無法被辨識為已知資料夾，也無法使用來自 [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) 之位置進行查詢的資料夾和檔案。 若要尋找檔案，您的 App 必須以遞迴方式周遊檔案系統，以列舉記憶卡的內容。 使用 [**GetFilesAsync (CommonFileQuery.DefaultQuery)**](https://msdn.microsoft.com/library/windows/apps/br227274) 與 [**GetFoldersAsync (CommonFolderQuery.DefaultQuery)**](https://msdn.microsoft.com/library/windows/apps/br227281) 可有效率地取得 SD 記憶卡的內容。

建議您使用背景執行緒來周遊 SD 記憶卡。 SD 記憶卡可以包含許多 GB 的檔案。

您的 app 也可以要求使用者使用資料夾選擇器來選擇特定資料夾。

當您使用衍生自 [**KnownFolders.RemovableDevices**](https://msdn.microsoft.com/library/windows/apps/br227158) 的路徑來存取 SD 記憶卡上的檔案系統時，下列方法會以下列方式運作。

-   [**GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227273) 方法會傳回一個聯集，這個聯集是由您登錄為可處理的副檔名以及與您已指定之任何媒體櫃功能關聯的副檔名所組成。

-   如果您尚未將您嘗試存取之檔案的副檔名登錄為可處理，[**GetFileFromPathAsync**](https://msdn.microsoft.com/library/windows/apps/br227206) 方法將會失敗。

## <a name="identifying-the-individual-sd-card"></a>識別個別的 SD 記憶卡

第一次掛接 SD 記憶卡時，作業系統會為記憶卡產生一個唯一的識別碼。 它會將這個識別碼儲存在記憶卡根目錄的 WPSystem 資料夾內的檔案中。 應用程式可以使用這個識別碼來判斷它是否可以辨識此記憶卡。 如果應用程式可以辨識這張卡，應用程式就能夠延遲之前完成的特定作業。 不過，在應用程式上次存取記憶卡之後，可能其中的內容已經變更。

例如，想想看建立電子書索引的應用程式。 如果應用程式之前掃描過整個 SD 記憶卡尋找電子書檔案並建立電子書的索引，在記憶卡重新插入且應用程式可辨識它的時候，即可立即顯示清單。 它可以另外啟動一個低優先順序的背景執行緒來搜尋新電子書。 當使用者嘗試存取已刪除的電子書，它也可以處理找不到之前存在的電子書的情形。

包含此識別碼的屬性名稱為 **WindowsPhone.ExternalStorageId**。

```csharp
using Windows.Storage;

...

            // Get the logical root folder for all external storage devices.
            StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

            // Get the first child folder, which represents the SD card.
            StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

            if (sdCard != null)
            {
                var allProperties = sdCard.Properties;
                IEnumerable<string> propertiesToRetrieve = new List<string> { "WindowsPhone.ExternalStorageId" };

                var storageIdProperties = await allProperties.RetrievePropertiesAsync(propertiesToRetrieve);

                string cardId = (string)storageIdProperties["WindowsPhone.ExternalStorageId"];

                if (...) // If cardID matches the cached ID of a recognized card.
                {
                    // Card is recognized. Index contents opportunistically.
                }
                else
                {
                    // Card is not recognized. Index contents immediately.
                }
            }
```

 

 

