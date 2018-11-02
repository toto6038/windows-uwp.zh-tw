---
author: laurenhughes
ms.assetid: 8BDDE64A-77D2-4F9D-A1A0-E4C634BCD890
title: 使用選擇器儲存檔案
description: 使用 FileSavePicker，讓使用者指定想要您的應用程式儲存檔案的名稱和位置。
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2a053047324fcb795a30951d70c5e0e78fbb5547
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5928833"
---
# <a name="save-a-file-with-a-picker"></a>使用選擇器儲存檔案

**重要 API**

-   [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)
-   [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)

使用 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)，讓使用者指定想要您的 app 儲存檔案的名稱和位置。

> [!NOTE]
> 另請參閱[檔案選擇器範例](http://go.microsoft.com/fwlink/p/?linkid=619994)(英文)。

 

## <a name="prerequisites"></a>先決條件


-   **了解通用 Windows 平台 (UWP) App 的非同步程式設計**

    您可以參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)，以了解如何使用 C# 或 Visual Basic 撰寫非同步的 app。 若要了解如何使用 C++ 撰寫非同步的 App，請參閱 [C++ 的非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187334)。

-   **位置的存取權限**

    請參閱[檔案存取權限](file-access-permissions.md)。

## <a name="filesavepicker-step-by-step"></a>FileSavePicker：逐步說明

使用 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)，讓使用者可以指定儲存檔案的名稱、類型及位置。 建立、自訂和顯示檔案選擇器物件，然後透過傳回的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件 (代表所挑選的檔案) 儲存資料。

1.  **建立和自訂 FileSavePicker**

```cs
var savePicker = new Windows.Storage.Pickers.FileSavePicker();
savePicker.SuggestedStartLocation =
    Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
// Dropdown of file types the user can save the file as
savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
// Default file name if the user does not type one in or select a file to replace
savePicker.SuggestedFileName = "New Document";
```

在與使用者和您的 app 相關的檔案選擇器物件上設定屬性。

這個範例會設定三個屬性：[**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207880)、[**FileTypeChoices**](https://msdn.microsoft.com/library/windows/apps/br207875) 和 [**SuggestedFileName**](https://msdn.microsoft.com/library/windows/apps/br207878)。

> [!NOTE]
>[**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) 物件會使用 [**PickerViewMode.List**](https://msdn.microsoft.com/library/windows/apps/br207891) 來顯示檔案選擇器。
     
- 因為使用者要儲存文件或文字檔，所以範例會使用 [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621)，將 [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207880) 設定成 app 的本機資料夾。 將 [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207854) 設定為儲存檔案類型的適當位置，例如，音樂、圖片、影片或文件。 使用者可以從開始位置瀏覽到其他位置。

- 因為我們要確認 app 能夠在儲存檔案之後予以開啟，所以我們使用範例支援的 [**FileTypeChoices**](https://msdn.microsoft.com/library/windows/apps/br207875) 來指定檔案類型 (Microsoft Word 文件與文字檔)。 確認 app 可支援您指定的所有檔案類型。 使用者能夠將檔案儲存成您指定的任何檔案類型。 他們也可以選取您指定的其他檔案類型，以變更檔案類型。 預設會選取清單中的第一個檔案類型選項：若要進行控制，請設定 [**DefaultFileExtension**](https://msdn.microsoft.com/library/windows/apps/br207873) 屬性。

> [!NOTE]
> 因為檔案選擇器也會使用目前選取的檔案類型來篩選它顯示的檔案，所以只會為使用者顯示符合所選檔案類型的檔案類型。

- 為了儲存使用者輸入，範例會設定 [**SuggestedFileName**](https://msdn.microsoft.com/library/windows/apps/br207878)。 讓您建議的檔案名稱與所儲存的檔案相關。 例如，就 Word 而言，您可以建議現有的檔案名稱 (如果已經有的話)，或是文件第一行 (如果使用者儲存的檔案尚未命名)。

2.  **顯示 FileSavePicker 並儲存至挑選的檔案**

    藉由呼叫 [**PickSaveFileAsync**](https://msdn.microsoft.com/library/windows/apps/br207876) 來顯示檔案選擇器。 使用者指定檔案名稱、檔案類型、位置並確認要儲存檔案之後，**PickSaveFileAsync** 會傳回一個代表該已儲存檔案的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件。 現在您已經有檔案的讀取和寫入權限，所以您可以擷取和處理這個檔案。

    ```cs
    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
        if (file != null)
        {
            // Prevent updates to the remote version of the file until
            // we finish making changes and call CompleteUpdatesAsync.
            Windows.Storage.CachedFileManager.DeferUpdates(file);
            // write to file
            await Windows.Storage.FileIO.WriteTextAsync(file, file.Name);
            // Let Windows know that we're finished changing the file so
            // the other app can update the remote version of the file.
            // Completing updates may require Windows to ask for user input.
            Windows.Storage.Provider.FileUpdateStatus status =
                await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
            if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
            {
                this.textBlock.Text = "File " + file.Name + " was saved.";
            }
            else
            {
                this.textBlock.Text = "File " + file.Name + " couldn't be saved.";
            }
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
    ```

這個範例會檢查檔案是否有效，並且將自己的檔案名稱寫入其中。 另請參閱[建立、寫入和讀取檔案](quickstart-reading-and-writing-files.md)。

**提示：** 您應該永遠檢查儲存的檔案，以確定它是有效的再執行任何其他處理。 然後，您可以按照您的 app 適用的方式將內容儲存到檔案，並在挑選的檔案無效時提供適當的行為。
