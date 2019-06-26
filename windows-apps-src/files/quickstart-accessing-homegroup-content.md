---
ms.assetid: 12ECEA89-59D2-4BCE-B24C-5A4DD525E0C7
title: 存取 HomeGroup 內容
description: 存取儲存在使用者 HomeGroup 資料夾中的內容，包括圖片、音樂及視訊。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 07d94f5b11acfe14bf55392c5cbf2c1b7bcfbeef
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369390"
---
# <a name="accessing-homegroup-content"></a>存取 HomeGroup 內容



**重要 API**

-   [**Windows.Storage.KnownFolders 類別**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) \(英文\)

存取儲存在使用者 HomeGroup 資料夾中的內容，包括圖片、音樂及視訊。

## <a name="prerequisites"></a>必要條件

-   **了解通用 Windows 平台 (UWP) 應用程式的非同步程式設計**

    您可以參閱[在 C# 或 Visual Basic 中呼叫非同步 API](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)，以了解如何使用 C# 或 Visual Basic 撰寫非同步的 app。 若要了解如何使用 C++ 撰寫非同步的 App，請參閱 [C++ 的非同步程式設計](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。

-   **App 功能宣告**

    若要存取 HomeGroup 內容，使用者的電腦必須設定 HomeGroup，而且您的 app 必須至少具有下列其中一項功能：**picturesLibrary**、**musicLibrary** 或 **videosLibrary**。 當您的 app 存取 HomeGroup 資料夾時，它將只會看到與您在 app 資訊清單中宣告之功能對應的媒體櫃。 若要深入了解，請參閱[檔案存取權限](file-access-permissions.md)。

    > [!NOTE]
    > 無論應用程式資訊清單中是否宣告了這些功能，或者無論使用者是否使用分享設定，您的應用程式均看不到 HomeGroup 的「文件」媒體櫃中的內容。     

-   **了解如何使用檔案選擇器**

    您通常會使用檔案選擇器來存取 HomeGroup 中的檔案和資料夾。 若要深入了解如何使用檔案選擇器，請參閱[使用選擇器開啟檔案和資料夾](quickstart-using-file-and-folder-pickers.md)。

-   **了解檔案和資料夾查詢**

    您可以使用查詢來列舉 HomeGroup 中的檔案和資料夾。 若要了解檔案和資料夾查詢，請參閱[列舉和查詢檔案和資料夾](quickstart-listing-files-and-folders.md)。

## <a name="open-the-file-picker-at-the-homegroup"></a>在 HomeGroup 開啟檔案選擇器

遵循下列步驟開啟檔案選擇器的執行個體，讓使用者從 HomeGroup 挑選檔案檔案和資料夾：

1.  **建立和自訂檔案選擇器**

    使用 [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 建立檔案選擇器，然後將選擇器的 [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) 設定成 [**PickerLocationId.HomeGroup**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.PickerLocationId)。 或者，設定與您的使用者和 app 相關的其他屬性。 如需協助您決定如何自訂檔案選擇器的指導方針，請參閱[檔案選擇器的指導方針和檢查清單](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers)。

    這個範例會建立在 HomeGroup 開啟的檔案選擇器 (包含任何檔案類型)，並以縮圖影像顯示檔案：
    ```cs
    Windows.Storage.Pickers.FileOpenPicker picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.HomeGroup;
    picker.FileTypeFilter.Clear();
    picker.FileTypeFilter.Add("*");
    ```

2.  **顯示檔案選擇器並處理被挑選的檔案。**

    建立和自訂檔案選擇器後，讓使用者呼叫 [**FileOpenPicker.PickSingleFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) 以挑選一個檔案，或呼叫 [**FileOpenPicker.PickMultipleFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync) 以挑選多個檔案。

    這個範例顯示讓使用者挑選一個檔案的檔案選擇器：
    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    if (file != null)
    {
        // Do something with the file.
    }
    else
    {
        // No file returned. Handle the error.
    }   
    ```

## <a name="search-the-homegroup-for-files"></a>搜尋 HomeGroup 中的檔案

本節示範如何尋找符合使用者查詢字詞的 HomeGroup 項目。

1.  **從使用者取得查詢字詞。**

    在這裡我們要取得使用者在名為 `searchQueryTextBox` 的 [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) 控制項中輸入的查詢字詞：
    ```cs
    string queryTerm = this.searchQueryTextBox.Text;    
    ```

2.  **設定查詢選項和搜尋篩選。**

    查詢選項會決定搜尋結果的排序方式，而搜尋篩選則會決定搜尋結果中會包含的項目。

    這個範例會設定先以相關性再以修改日期來排序搜尋結果的查詢選項。 搜尋篩選是使用者在前面步驟中輸入的查詢字詞：
    ```cs
    Windows.Storage.Search.QueryOptions queryOptions =
            new Windows.Storage.Search.QueryOptions
                (Windows.Storage.Search.CommonFileQuery.OrderBySearchRank, null);
    queryOptions.UserSearchFilter = queryTerm.Text;
    Windows.Storage.Search.StorageFileQueryResult queryResults =
            Windows.Storage.KnownFolders.HomeGroup.CreateFileQueryWithOptions(queryOptions);    
    ```

3.  **執行查詢並處理結果。**

    下列範例會在 HomeGroup 中執行搜尋查詢，並將任何相符檔案的名稱儲存為字串清單。
    ```cs
    System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFile> files =
        await queryResults.GetFilesAsync();

    if (files.Count > 0)
    {
        outputString += (files.Count == 1) ? "One file found\n" : files.Count.ToString() + " files found\n";
        foreach (Windows.Storage.StorageFile file in files)
        {
            outputString += file.Name + "\n";
        }
    }    
    ```


## <a name="search-the-homegroup-for-a-particular-users-shared-files"></a>搜尋 HomeGroup 中特定使用者的分享檔案

本節說明如何尋找由特定使用者分享的 HomeGroup 檔案。

1.  **取得 HomeGroup 使用者的集合。**

    HomeGroup 中的每個第一層資料夾都代表不同的 HomeGroup 使用者。 因此，若要取得 HomeGroup 使用者的集合，請呼叫 [**GetFoldersAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfoldersasync) 擷取最上層 HomeGroup 資料夾。
    ```cs
    System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFolder> hgFolders =
        await Windows.Storage.KnownFolders.HomeGroup.GetFoldersAsync();    
    ```

2.  **尋找目標使用者的資料夾，然後建立其範圍為該使用者之資料夾的檔案查詢。**

    下列範例會逐一查看所擷取的資料夾，以尋找目標使用者的資料夾。 接著，它會設定先以相關性再以修改日期來排序的查詢選項，以尋找該資料夾中的所有檔案。 範例會建置一個字串，用來報告找到的檔案數目及檔案名稱。
    ```cs
    bool userFound = false;
    foreach (Windows.Storage.StorageFolder folder in hgFolders)
    {
        if (folder.DisplayName == targetUserName)
        {
            // Found the target user's folder, now find all files in the folder.
            userFound = true;
            Windows.Storage.Search.QueryOptions queryOptions =
                new Windows.Storage.Search.QueryOptions
                    (Windows.Storage.Search.CommonFileQuery.OrderBySearchRank, null);
            queryOptions.UserSearchFilter = "*";
            Windows.Storage.Search.StorageFileQueryResult queryResults =
                folder.CreateFileQueryWithOptions(queryOptions);
            System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFile> files =
                await queryResults.GetFilesAsync();

            if (files.Count > 0)
            {
                string outputString = "Searched for files belonging to " + targetUserName + "'\n";
                outputString += (files.Count == 1) ? "One file found\n" : files.Count.ToString() + " files found\n";
                foreach (Windows.Storage.StorageFile file in files)
                {
                    outputString += file.Name + "\n";
                }
            }
        }
    }    
    ```

## <a name="stream-video-from-the-homegroup"></a>從 HomeGroup 串流視訊

遵循這些步驟從 HomeGroup 串流視訊內容：

1.  **在應用程式中包含 MediaElement。**

    [  **MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 可以讓您在 app 中播放音訊和視訊內容。 如需音訊和視訊播放的詳細資訊，請參閱[建立自訂傳輸控制項](https://docs.microsoft.com/windows/uwp/controls-and-patterns/custom-transport-controls)與[音訊、視訊和相機](https://docs.microsoft.com/windows/uwp/audio-video-camera/index)。
    ```HTML
    <Grid x:Name="Output" HorizontalAlignment="Left" VerticalAlignment="Top" Grid.Row="1">
        <MediaElement x:Name="VideoBox" HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0" Width="400" Height="300"/>
    </Grid>    
    ```

2.  **在 HomeGroup 開啟檔案選擇器，套用包含您應用程式所支援格式之視訊檔案的篩選。**

    這個範例在檔案開啟選擇器中包含了 .mp4 和 .wmv 檔案。
    ```cs
    Windows.Storage.Pickers.FileOpenPicker picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.HomeGroup;
    picker.FileTypeFilter.Clear();
    picker.FileTypeFilter.Add(".mp4");
    picker.FileTypeFilter.Add(".wmv");
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();   
    ```

3.  **開啟使用者的檔案選取項目以取得讀取權限，並將檔案資料流設定為** [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) \(英文\) 的來源，然後播放該檔案。
    ```cs
    if (file != null)
    {
        var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        VideoBox.SetSource(stream, file.ContentType);
        VideoBox.Stop();
        VideoBox.Play();
    }
    else
    {
        // No file selected. Handle the error here.
    }    
    ```
