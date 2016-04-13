---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
檔案存取權限
App 預設可以存取特定的檔案系統位置。 App 也可以透過檔案選擇器或宣告功能，以存取其他位置。
---
# 檔案存取權限

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


App 預設可以存取特定的檔案系統位置。 App 也可以透過檔案選擇器或宣告功能，以存取其他位置。

## 所有 app 都能存取的位置

當您建立新的 app 時，預設可以存取下列檔案系統位置：

-   **應用程式安裝目錄**。 在使用者系統上安裝您 app 的資料夾。

    有兩種主要方式可以存取 app 安裝目錄的檔案和資料夾：

    1.  您可以擷取代表 app 安裝目錄的 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)，如下所示：
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
        ```
        ```javascript
        var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
        ```

       您可以接著使用 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 方法，來存取目錄中的檔案和資料夾。 在此範例中，這個 **StorageFolder** 儲存在 `installDirectory` 變數中。 您可以下載適用於 Windows 8.1 的 [App 套件資訊範例](http://go.microsoft.com/fwlink/p/?linkid=231526)，並在您的 Windows 10 app 中重複使用其原始程式碼，以深入了解如何使用 app 套件和安裝目錄。

    2.  您可以使用 app URI，從 app 的安裝目錄直接擷取檔案，如下所示：
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;            
        StorageFile file = await StorageFile.GetFileFromApplicationUriAsync("ms-appx:///file.txt");
        ```
        ```javascript
        Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
            function(file) {
                // Process file
            }
        );
        ```
        
        當 [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 完成時，就會傳回 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)，代表 app 安裝目錄 (範例中為 `file`) 中的 *file.txt* 檔案。

        URI 中的「ms-appx:///」前置詞是指 app 的安裝目錄。 您可以在[如何使用 URI 來參考內容](https://msdn.microsoft.com/library/windows/apps/hh781215)中，深入了解如何使用 app URI。

    此外，與其他位置不同的是，您也可以使用[通用 Windows 平台 (UWP) app 的 Win32 和 COM](https://msdn.microsoft.com/library/windows/apps/br205757) 及 [Microsoft Visual Studio 的 C/C++ 標準程式庫功能](http://msdn.microsoft.com/library/hh875057.aspx)，來存取 app 安裝目錄中的檔案。

    app 安裝目錄是唯讀位置。 您無法透過檔案選擇器取得安裝目錄的存取權。

-   **應用程式資料位置。** 您的 app 可以儲存資料的資料夾。 這些資料夾 (本機、漫遊及暫存) 都是在您 app 安裝時所建立。

    有兩種主要方式可以從應用程式資料位置存取檔案和資料夾：

    1.  使用 [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587) 屬性來抓取應用程式資料資料夾。

        例如，您可以使用 [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587).[**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621)，來擷取代表 app 本機資料夾的 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)，如下所示：
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFolder localFolder = ApplicationData.Current.LocalFolder;
        ```
        ```javascript
        var localFolder = Windows.Storage.ApplicationData.current.localFolder;
        ```
 
        如果您想要存取 app 的漫遊或暫存資料夾，請改用 [**RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) 或 [**TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) 屬性。

        在您擷取代表應用程式資料位置的 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 之後，可以使用 **StorageFolder** 方法存取該位置中的檔案和資料夾。 在此範例中，這些 **StorageFolder** 物件儲存在 `localFolder` 變數中。 您可以在[管理應用程式資料](https://msdn.microsoft.com/library/windows/apps/hh465109)中，以及藉由下載適用於 Windows 8.1 的[應用程式資料範例](http://go.microsoft.com/fwlink/p/?linkid=231478)，並在您的 Windows 10 app 中重複使用其原始程式碼，來深入了解如何使用 app 資料位置。

    2.  例如，您可以使用 app URI 從 app 的本機資料夾直接擷取檔案，如下所示：
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFile file = await StorageFile.GetFileFromApplicationUriAsync("ms-appdata:///local/file.txt");
        ```
        ```javascript
        Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
            function(file) {
                // Process file
            }
        );
        ```

        當 [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 完成時，就會傳回 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)，代表 app 本機資料夾 (範例中為 `file`) 中的 *file.txt* 檔案。

        URI 中的「ms-appdata:///local/」前置詞是指 app 的本機資料夾。 若要存取 app 的漫遊或暫存資料夾中的檔案，請改用「ms-appdata:///roaming/」或「ms-appdata:///temporary/」。 您可以在[如何載入檔案資源](https://msdn.microsoft.com/library/windows/apps/hh781229)中，深入了解如何使用 app URI。

    此外，與其他位置不同的是，您也可以使用[適用於 UWP app 的 Win32 和 COM](https://msdn.microsoft.com/library/windows/apps/br205757) 及 Visual Studio 的 C/C++ 標準程式庫功能，來存取 app 資料位置中的檔案。

    您無法透過檔案選擇器存取本機、漫遊或暫存資料夾。

-   **卸除式裝置。** 此外，您的 app 預設可以存取連接裝置上的部分檔案。 如果您的 app 在使用者連接裝置 (例如相機或 USB 快閃磁碟機) 時，會使用[自動播放延伸](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh464906.aspx#autoplay)來自動啟動，就可以使用這個選項。 您的 app 可以存取的檔案受限於特定的檔案類型，這些檔案類型是在應用程式資訊清單中的檔案類型關聯宣告中指定。

    當然，您也可以藉由呼叫檔案選擇器 (使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 和 [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881))，並讓使用者為您的 app 挑選要存取的檔案和資料夾，以取得卸除式裝置上檔案和資料夾的存取權。 在[使用選擇器開啟檔案和資料夾](quickstart-using-file-and-folder-pickers.md)中，深入了解如何使用檔案選擇器。

    **注意** 如需從行動 app 存取 SD 記憶卡的詳細資訊，請參閱[存取 SD 記憶卡](access-the-sd-card.md)。

     

## Windows 市集應用程式可以存取的位置

-   **使用者的 [下載] 資料夾** 預設儲存下載檔案的資料夾。

    根據預設，您的 app 只能存取 app 在使用者的 [下載] 資料夾中建立的檔案和資料夾。 不過，您可以藉由呼叫檔案選擇器 ([**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 或 [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881))，以取得使用者下載資料夾中檔案和資料夾的存取權，如此一來，使用者便能針對您的應用程式瀏覽和挑選要存取的檔案或資料夾。

    -   您可以在使用者的 [下載] 資料夾中建立檔案，如下所示：
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFile newFile = await DownloadsFolder.CreateFileAsync("file.txt");
        ```
        ```javascript
        Windows.Storage.DownloadsFolder.createFileAsync("file.txt").done(
            function(newFile) {
                // Process file
            }
        );
        ```
 
        [
            **DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh996761) 已超載，所以您可以在 [下載] 資料夾中已經存在相同名稱的檔案時，指定系統應該怎麼做。 當這些方法完成時，它們會傳回 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)，代表已建立的檔案。 這個檔案在範例中稱為 `newFile`。

    -   您可以在使用者的 [下載] 資料夾中建立子資料夾，如下所示：
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFolder newFolder = await DownloadsFolder.CreateFolderAsync("New Folder");
        ```
        ```javascript
        Windows.Storage.DownloadsFolder.createFolderAsync("New Folder").done(
            function(newFolder) {
                // Process folder
            }
        );
        ```
 
        [
            **DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFolderAsync**](https://msdn.microsoft.com/library/windows/apps/hh996763) 已超載，所以您可以在 [下載] 資料夾中已經存在相同名稱的子資料夾時，指定系統應該怎麼做。 當這些方法完成時，它們會傳回 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)，代表已建立的子資料。 這個檔案在範例中稱為 `newFolder`。

    如果您在下載資料夾中建立檔案或資料夾，建議您將該項目新增到應用程式的 [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457)，如此一來，您的應用程式未來便能輕易存取該項目。

## 存取其他位置

除了預設位置，App 還可以在應用程式資訊清單中宣告功能 (請參閱 [App 功能宣告](https://msdn.microsoft.com/library/windows/apps/mt270968))，或是呼叫檔案選擇器來讓使用者挑選 App 要存取的檔案和資料夾 (請參閱[使用選擇器開啟檔案和資料夾](quickstart-using-file-and-folder-pickers.md))，藉以存取其他檔案和資料夾。

下表列出可透過宣告功能及使用關聯的 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) API 來存取的其他位置：

| 位置 | 功能 | Windows.Storage API |
|----------|------------|---------------------|
| 文件 | DocumentsLibrary <br><br>注意：您必須將檔案類型關聯新增到您的應用程式資訊清單，宣告您的 app 可以在這個位置中存取的特定檔案類型。 <br><br>如果您的 app 符合下列條件，則可使用此功能：<br>- 使用有效的 OneDrive URL 或資源識別碼，協助對特定的 OneDrive 內容進行跨平台離線存取<br>- 在離線時自動將開啟的檔案儲存到使用者的 OneDrive | [KnownFolders.DocumentsLibrary](https://msdn.microsoft.com/library/windows/apps/br227152) |
| 音樂     | MusicLibrary <br>另請參閱[音樂、圖片及影片媒體櫃中的檔案和資料夾](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。 | [KnownFolders.MusicLibrary](https://msdn.microsoft.com/library/windows/apps/br227155) |    
| 圖片  | PicturesLibrary<br> 另請參閱[音樂、圖片及影片媒體櫃中的檔案和資料夾](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。 | [KnownFolders.PicturesLibrary](https://msdn.microsoft.com/library/windows/apps/br227156) |  
| 影片    | VideosLibrary<br>另請參閱[音樂、圖片及影片媒體櫃中的檔案和資料夾](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。 | [KnownFolders.VideosLibrary](https://msdn.microsoft.com/library/windows/apps/br227159) |   
| 卸除式裝置  | RemovableDevices <br><br>請注意，您必須將檔案類型關聯新增到您的應用程式資訊清單，宣告您的 app 可以在這個位置中存取的特定檔案類型。 <br><br>另請參閱[存取 SD 記憶卡](access-the-sd-card.md)。 | [KnownFolders.RemovableDevices](https://msdn.microsoft.com/library/windows/apps/br227158) |  
| 家用群組媒體櫃  | 至少需要下列其中一個功能。 <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.HomeGroup](https://msdn.microsoft.com/library/windows/apps/br227153) |      
| 媒體伺服器裝置 (DLNA) | 至少需要下列其中一個功能。 <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.MediaServerDevices](https://msdn.microsoft.com/library/windows/apps/br227154) | 
| 通用命名慣例 (UNC) 資料夾 | 需要下列功能的組合。 <br><br>家用與工作場所網路功能： <br>- PrivateNetworkClientServer <br><br>同時至少要有一個網際網路和公用網路功能： <br>- InternetClient <br>- InternetClientServer <br><br>此外，如果適當，還要有網域認證功能：<br>- EnterpriseAuthentication <br><br>注意：您必須將檔案類型關聯新增到您的應用程式資訊清單，宣告您的 app 可以在這個位置中存取的特定檔案類型。 | 使用下列方式擷取資料夾： <br>[StorageFolder.GetFolderFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227278) <br><br>使用下列方式擷取檔案： <br>[StorageFile.GetFileFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227206) |



<!--HONumber=Mar16_HO1-->


