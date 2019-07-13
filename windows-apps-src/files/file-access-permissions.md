---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: 檔案存取權限
description: App 預設可以存取特定的檔案系統位置。 App 也可以透過檔案選擇器或宣告功能，以存取其他位置。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- javascript
ms.openlocfilehash: 9adc872554e0823eb0a4e1fdbebef19b876b6198
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321413"
---
# <a name="file-access-permissions"></a>檔案存取權限

通用 Windows 平台 (UWP) 應用程式預設可存取某些檔案系統位置。 App 也可以透過檔案選擇器或宣告功能，以存取其他位置。

## <a name="the-locations-that-all-apps-can-access"></a>所有 app 都能存取的位置

當您建立新的 app 時，預設可以存取下列檔案系統位置：

### <a name="application-install-directory"></a>應用程式安裝目錄
在使用者系統上安裝您應用程式的資料夾。

有兩種主要方式可用來存取應用程式安裝目錄中的檔案和資料夾：

1. 您可以抓取代表 app 安裝目錄的 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)，如下：

    ```csharp
    Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
    ```
    
    ```javascript
    var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
    ```
    
    ```cppwinrt
    #include <winrt/Windows.Storage.h>
    ...
    Windows::Storage::StorageFolder installedLocation{ Windows::ApplicationModel::Package::Current().InstalledLocation() };
    ```
    
    ```cpp
    Windows::Storage::StorageFolder^ installedLocation = Windows::ApplicationModel::Package::Current->InstalledLocation;
    ```

    您可以接著使用 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) 方法，來存取目錄中的檔案和資料夾。 在此範例中，這個 **StorageFolder** 儲存在 `installDirectory` 變數中。 您可以從 GitHub 上的[應用程式套件資訊範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package) \(英文\) 深入了解如何使用應用程式套件和安裝目錄。

2. 您可以使用 App URI，從 App 的安裝目錄直接擷取檔案，如下所示：

    ```csharp
    using Windows.Storage;            
    StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///file.txt"));
    ```
    
    ```javascript
    Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
        function(file) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
    {
        Windows::Storage::StorageFile file{
            co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{L"ms-appx:///file.txt"})
        };
        // Process file
    }
    ```
    
    ```cpp
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appx:///file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```

    當 [**GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) \(英文\) 完成時，它會傳回 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) \(英文\)，其代表應用程式安裝目錄 (範例中為 `file`) 中的 `file.txt` 檔案。
    
    URI 中的「ms-appx:///」前置詞是指 app 的安裝目錄。 您可以在[如何使用 URI 來參考內容](https://docs.microsoft.com/previous-versions/windows/apps/hh781215(v=win.10))中，深入了解如何使用 app URI。

此外，與其他位置不同的是，您也可以使用[通用 Windows 平台 (UWP) app 的 Win32 和 COM](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps) 及 [Microsoft Visual Studio 的 C/C++ 標準程式庫功能](https://docs.microsoft.com/cpp/cpp/c-cpp-language-and-standard-libraries)，來存取 app 安裝目錄中的檔案。

app 安裝目錄是唯讀位置。 您無法透過檔案選擇器取得安裝目錄的存取權。

### <a name="application-data-locations"></a>應用程式資料位置
您的 app 可以儲存資料的資料夾。 這些資料夾 (本機、漫遊及暫存) 都是在您 app 安裝時所建立。

有兩種主要方式可以從應用程式資料位置存取檔案和資料夾：

1.  使用 [**ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData) 屬性來抓取應用程式資料資料夾。

    例如，您可以使用 [**ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData).[**LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder)，以抓取代表應用程式本機資料夾的 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)，如下：
    
    ```csharp
    using Windows.Storage;
    StorageFolder localFolder = ApplicationData.Current.LocalFolder;
    ```
    
    ```javascript
    var localFolder = Windows.Storage.ApplicationData.current.localFolder;
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFolder storageFolder{
        Windows::Storage::ApplicationData::Current().LocalFolder()
    };
    ```
    
    ```cpp
    using namespace Windows::Storage;
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    ```
    
    如果您想要存取 App 的漫遊或暫存資料夾，請改用 [**RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder) 或 [**TemporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) 屬性。
    
    在您擷取代表應用程式資料位置的 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) 之後，可以使用 **StorageFolder** 方法存取該位置中的檔案和資料夾。 在此範例中，這些 **StorageFolder** 物件儲存在 `localFolder` 變數中。 您可以透過 [ApplicationData 類別](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata) \(英文\) 頁面上的指導方針，以及從 GitHub 下載[應用程式資料範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) \(英文\) 來深入了解使用應用程式資料位置。

2. 您可以使用應用程式 URI 從應用程式的本機資料夾直接擷取檔案，如下所示：
    
    ```csharp
    using Windows.Storage;
    StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appdata:///local/file.txt"));
    ```
    
    ```javascript
    Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
        function(file) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFile file{
        co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{ L"ms-appdata:///local/file.txt" })
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appdata:///local/file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```
    
    當 [**GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) \(英文\) 完成時，它會傳回 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) \(英文\)，其代表應用程式本機資料夾 (範例中為 `file`) 中的 `file.txt` 檔案。
    
    URI 中的「ms-appdata:///local/」前置詞是指 app 的本機資料夾。 若要存取 app 的漫遊或暫存資料夾中的檔案，請改用「ms-appdata:///roaming/」或「ms-appdata:///temporary/」。 您可以在[如何載入檔案資源](https://docs.microsoft.com/previous-versions/windows/apps/hh781229(v=win.10))中，深入了解如何使用 app URI。

此外，與其他位置不同的是，您也可以使用[適用於 UWP app 的 Win32 和 COM](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps) 及 Visual Studio 的 C/C++ 標準程式庫功能，來存取 app 資料位置中的檔案。

您無法透過檔案選擇器存取本機、漫遊或暫存資料夾。

### <a name="removable-devices"></a>卸除式裝置
此外，您的 app 預設可以存取連接裝置上的部分檔案。 如果您的 app 在使用者連接裝置 (例如相機或 USB 快閃磁碟機) 時，會使用[自動播放延伸](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))來自動啟動，就可以使用這個選項。 您的 app 可以存取的檔案受限於特定的檔案類型，這些檔案類型是在應用程式資訊清單中的檔案類型關聯宣告中指定。

當然，您也可以藉由呼叫檔案選擇器 (使用 [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 和 [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker))，並讓使用者為您的 app 挑選要存取的檔案和資料夾，以取得卸除式裝置上檔案和資料夾的存取權。 在[使用選擇器開啟檔案和資料夾](quickstart-using-file-and-folder-pickers.md)中，深入了解如何使用檔案選擇器。

> [!NOTE]
> 如需存取 SD 記憶卡或其他卸除式裝置的詳細資訊，請參閱[存取 SD 記憶卡](access-the-sd-card.md)。

## <a name="locations-that-uwp-apps-can-access"></a>UWP 應用程式能存取的位置
### <a name="users-downloads-folder"></a>使用者的 [下載] 資料夾

預設儲存下載檔案的資料夾。

根據預設，您的 app 只能存取 app 在使用者的 [下載] 資料夾中建立的檔案和資料夾。 不過，您可以藉由呼叫檔案選擇器 ([**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 或 [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker))，以取得使用者下載資料夾中檔案和資料夾的存取權，如此一來，使用者便能針對您的應用程式瀏覽和挑選要存取的檔案或資料夾。

- 您可以在使用者的下載資料夾中建立檔案，如下：

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
    
    ```cppwinrt
    Windows::Storage::StorageFile newFile{
        co_await Windows::Storage::DownloadsFolder::CreateFileAsync(L"file.txt")
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFileTask = create_task(DownloadsFolder::CreateFileAsync(L"file.txt"));
    createFileTask.then([](StorageFile^ newFile)
    {
        // Process file
    });
    ```

    [**DownloadsFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.downloadsfolder.createfileasync) \(英文\) 已超載，所以您可以在 [下載] 資料夾中已經存在相同名稱的檔案時，指定系統應該怎麼做。 當這些方法完成時，它們會傳回 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)，代表已建立的檔案。 這個檔案在範例中稱為 `newFile`。

- 您可以在使用者的 [下載] 資料夾中建立子資料夾，如下所示：

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
    
    ```cppwinrt
    Windows::Storage::StorageFolder newFolder{
        co_await Windows::Storage::DownloadsFolder::CreateFolderAsync(L"New Folder")
    };
    // Process folder
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFolderTask = create_task(DownloadsFolder::CreateFolderAsync(L"New Folder"));
    createFolderTask.then([](StorageFolder^ newFolder)
    {
        // Process folder
    });
    ```

    [**DownloadsFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.downloadsfolder.createfolderasync) \(英文\) 已超載，所以您可以在 [下載] 資料夾中已經存在相同名稱的子資料夾時，指定系統應該怎麼做。 當這些方法完成時，它們會傳回 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)，代表已建立的子資料。 這個檔案在範例中稱為 `newFolder`。

如果您在下載資料夾中建立檔案或資料夾，建議您將該項目新增到應用程式的 [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist)，如此一來，您的應用程式未來便能輕易存取該項目。

## <a name="accessing-additional-locations"></a>存取其他位置

除了預設位置，App 還可以在應用程式資訊清單中宣告功能 (請參閱 [App 功能宣告](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations))，或是呼叫檔案選擇器來讓使用者挑選 App 要存取的檔案和資料夾 (請參閱[使用選擇器開啟檔案和資料夾](quickstart-using-file-and-folder-pickers.md))，藉以存取其他檔案和資料夾。

宣告 [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) \(英文\) 擴充的應用程式對於其從主控台視窗中啟動所在的目錄，以及其下層目錄具有檔案系統權限。

下表列出可透過宣告功能及使用關聯的 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) API 來存取的其他位置：

| Location | 功能 | Windows.Storage API |
|----------|------------|---------------------|
| 使用者可存取的所有檔案。 例如：文件、圖片、相片、下載、桌面、OneDrive 等。 | broadFileSystemAccess<br><br>這是受限的功能。 存取權可在 [設定]   > [隱私權]   > [檔案系統]  中設定。 由於使用者隨時都可以在 [設定]  中授與或拒絕權限，您應該確定應用程式針對那些變更具備復原性。 如果您發現應用程式沒有存取權，則可以選擇向使用者提供 [Windows 10 檔案系統存取權與隱私權](https://support.microsoft.com/help/4468237/windows-10-file-system-access-and-privacy-microsoft-privacy)一文的連結，來提示他們變更設定。 請注意，使用者必須關閉應用程式，切換該設定，然後再重新啟動應用程式。 如果他們在應用程式執行期間切換該設定，平台將會暫停您的應用程式以讓您儲存狀態，然後強制終止該應用程式以套用新設定。 在 2018 年 4 月更新中，該權限的預設值為 [開啟]。 在 2018 年 10 月更新中，預設值為 [關閉]。<br /><br />如果您將應用程式提交至會宣告此功能的 Microsoft Store，則須提供其他描述以說明應用程式為何需要此功能，以及其意圖使用此功能的方式。<br>此功能適用於 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) \(英文\) 命名空間中的 API。 請參閱本文結尾的＜範例＞  小節，以取得在應用程式中啟用此功能的範例。 | 不適用 |
| 文件 | DocumentsLibrary <br><br>注意：您必須將檔案類型關聯新增到應用程式資訊清單中，其會宣告您的應用程式可在這個位置中存取的特定檔案類型。 <br><br>如果您的 app 符合下列條件，則可使用此功能：<br>- 使用有效的 OneDrive URL 或資源識別碼，協助對特定的 OneDrive 內容進行跨平台離線存取<br>- 在離線時自動將開啟的檔案儲存到使用者的 OneDrive | [KnownFolders.DocumentsLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.documentslibrary) \(英文\) |
| 音樂     | MusicLibrary <br>另請參閱[音樂、圖片及影片媒體櫃中的檔案和資料夾](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。 | [KnownFolders.MusicLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.musiclibrary) \(英文\) |    
| 圖片  | PicturesLibrary<br> 另請參閱[音樂、圖片及影片媒體櫃中的檔案和資料夾](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。 | [KnownFolders.PicturesLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.pictureslibrary) \(英文\) |  
| 影片    | VideosLibrary<br>另請參閱[音樂、圖片及影片媒體櫃中的檔案和資料夾](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。 | [KnownFolders.VideosLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.videoslibrary) \(英文\) |   
| 卸除式裝置  | RemovableDevices <br><br>注意：您必須將檔案類型關聯新增到應用程式資訊清單中，其會宣告您的應用程式可在這個位置中存取的特定檔案類型。 <br><br>另請參閱[存取 SD 記憶卡](access-the-sd-card.md)。 | [KnownFolders.RemovableDevices](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.removabledevices) \(英文\) |  
| 家用群組媒體櫃  | 至少需要下列其中一個功能。 <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.HomeGroup](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.homegroup) \(英文\) |      
| 媒體伺服器裝置 (DLNA) | 至少需要下列其中一個功能。 <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.MediaServerDevices](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.mediaserverdevices) \(英文\) |
| 通用命名慣例 (UNC) 資料夾 | 需要下列功能的組合。 <br><br>家用與工作場所網路功能： <br>- PrivateNetworkClientServer <br><br>同時至少要有一個網際網路和公用網路功能： <br>- InternetClient <br>- InternetClientServer <br><br>此外，如果適當，還要有網域認證功能：<br>- EnterpriseAuthentication <br><br>注意：您必須將檔案類型關聯新增到應用程式資訊清單中，其會宣告您的應用程式可在這個位置中存取的特定檔案類型。 | 使用下列方式擷取資料夾： <br>[StorageFolder.GetFolderFromPathAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfolderfrompathasync) \(英文\) <br><br>使用下列方式擷取檔案： <br>[StorageFile.GetFileFromPathAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync) \(英文\) |

**範例**

此範例會新增受限的 `broadFileSystemAccess` 功能。 除了指定功能之外，還必須新增 `rescap` 命名空間，其也會被新增至 `IgnorableNamespaces`：

```xaml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp uap5 rescap">
...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```

> [!NOTE]
> 如需完整的應用程式功能清單，請參閱[應用程式功能宣告](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) \(部分機器翻譯\)。
