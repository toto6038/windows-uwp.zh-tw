---
title: 若要刪除資料的使用連接儲存體
description: 了解如何使用連接儲存體來刪除 blob 和容器的資料。
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，連接的儲存體
ms.localizationpriority: medium
ms.openlocfilehash: 756de46d05cdbf64d85491b4e8c6f783122f2356
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601313"
---
# <a name="use-connected-storage-to-delete-data"></a>若要刪除資料的使用連接儲存體

藉由建立以非同步方式刪除資料的 blob`ConnectedStorageContainer`中`ConnectedStorageSpace`使用者及呼叫`SubmitUpdatesAsync`提供一份字串代表要刪除的 blobsToDelete 參數命名的 blob 容器上的方法。

藉由建立以非同步方式刪除資料容器`ConnectedStorageContainer`，然後呼叫其`DeleteContainerAsync`方法。

## <a name="to-delete-blob-data-from-connected-storage"></a>若要從連接的儲存體中刪除 blob 資料

1.  擷取`ConnectedStorageSpace`藉由呼叫使用者物件`GetForUserAsync`。

    在傳回的 XDK 範例`ConnectedStorageSpace`物件會被新增至地圖，讓您輕鬆管理`ConnectedStorageSpace`多個使用者的物件。

2.  建立`ConnectedStorageContainer`藉由呼叫物件`CreateContainer`上`ConnectedStorageSpace`物件。
3.  呼叫`SubmitUpdatesAsync`上`ConnectedStorageContainer`物件。

## <a name="to-delete-a-container-from-connected-storage"></a>若要從連接的儲存體中刪除容器

1.  擷取`ConnectedStorageSpace`藉由呼叫使用者物件`GetForUserAsync`。

    在傳回的 XDK 範例`ConnectedStorageSpace`物件會被新增至地圖，讓您輕鬆管理`ConnectedStorageSpace`多個使用者的物件。

2.  呼叫`DeleteContainerAsync`ConnectedStorageSpace 方法的方法。

## <a name="c-xdk-sample"></a>C + + XDK 範例
```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() { return true; };

User^ gCurrentUser;
IBuffer^ WrapRawBuffer(void* ptr, size_t size);

// Acquire a Connected Storage space for a user. A Connected Storage space is required to manipulate Connected Storage Data.
void PrepareConnectedStorage(User^ user)
{
  auto op = ConnectedStorageSpace::GetForUserAsync(user);
  op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
    {
      switch(status)
      {
        case Windows::Foundation::AsyncStatus::Completed:
          gConnectedStorageSpaceForUsers->Insert(user->Id, operation->GetResults());
          break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
          // Present user option: ?Would you like to continue without saving progress??
          if( GetUserInputYesOrNo() )
            //If the users opts yes, continue in offline mode
          else
            //If the users opts no, retry.
          break;
      }
    });
}

// Delete data from a fixed container/blob name into an IBuffer
void DeleteBlob(User^ user)
{

    //Create a list of blob names to delete
    std::vector<Platform::String^> blobsToDelete;

    //Add the blob name to your Generic Iterable
    blobsToDelete.push_back(L"someBlobName");

    auto blobNames = ref new Platform::Collections::VectorView<Platform::String^>(blobsToDelete);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(user->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    //Delete is occurring asynchronously at this point.

    auto op = container->SubmitUpdatesAsync(nullptr, blobNames);

    op->Completed = ref new AsyncActionCompletedHandler(
        [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
            switch(status)
            {
                case Windows::Foundation::AsyncStatus::Completed:
                //Delete was successful no need for action
                break;
                case Windows::Foundation::AsyncStatus::Error:
                case Windows::Foundation::AsyncStatus::Canceled:
                // Handle failed deletion
            }
     });
}

void DeleteContainer(User^ user)
{
    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(user->Id);

    auto deleteOperation = storageSpace->DeleteContainerAsync("Saves/Checkpoint");

    deleteOperation->Completed = ref new AsyncActionCompletedHandler(
        [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
            switch(status)
            {
                case Windows::Foundation::AsyncStatus::Completed:
                //Delete was successful no need for action
                break;
                case Windows::Foundation::AsyncStatus::Error:
                case Windows::Foundation::AsyncStatus::Canceled:
                // Handle failed deletion
            }
     });

}
```

您可以找到 XDK 連接儲存體 Api 記載於路徑下的 XDK.chm 檔案：**Xbox 一個 XDK >> API 參考 >> 平台 API 參考 >> 系統 API 參考 >> Windows.Xbox.Storage**。
XDK Api 也會記載於[developer.microsoft.com 網站](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n)。
XDK Api 的連結，您需要已啟用 Xbox 開發人員 Kit(XDK) 存取 Microsoft 帳戶 （msa）。
Windows.Xbox.Storage 是 Xbox One 主控台連接的儲存體命名空間的名稱。


## <a name="c-uwp-sample"></a>C#UWP 範例

XDK 遊戲和 UWP 應用程式可能使用不同的 Api，而 UWP API 被仿造 XDK API 非常密切。 若要刪除的資料仍然必須遵循相同的基本步驟，同時讓某些命名空間和類別名稱變更的附註。 而不是使用命名空間`Windows::Xbox::Storage`您將使用`Windows.Gaming.XboxLive.Storage`。 此類別`ConnectedStorageSpace`，相當於`GameSaveProvider`。 此類別`ConnectedStorageContainer`相當於`GameSaveContainer`。 這些變更連接儲存體一節中進一步詳述[移植 Xbox Live 的程式碼從 XDK 至 UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md)。

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 23;
const string c_saveBlobName = "Jersey";
const string c_saveContainerDisplayName = "GameSave";
const string c_saveContainerName = "GameSaveContainer";
GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetForUserAsync(users[0], context.AppConfig.ServiceConfigurationId);
//Parameters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
else
{
    return;
    //throw new Exception("Game Save Provider Initialization failed");
}

//Now you have a GameSaveProvider (formerly ConnectedStorageSpace)
//Next you need to call CreateContainer to get a GameSaveContainer (formerly ConnectedStorageContainer)

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName); // this will create a new named game save container with the name = to the input name
//Parameter
//string name (name of container to be created) 

//DELETE A BLOB
//form an array of strings containing the blob names you would like to delete.
string[] blobsToDelete = new string[] { c_saveBlobName };

//Call SubmitUpdatesAsync with the dictionary of the names of blobs to delete as the second parameter.
GameSaveOperationResult gameSaveOperationResult = await gameSaveContainer.SubmitUpdatesAsync(null, blobsToDelete, c_saveContainerDisplayName);
//Parameters
//IReadOnlyDictionary<String, IBuffer> blobsToWrite
//IEnumerable<string> blobsToDelete
//string displayName

//Check status of the operation to ensure successful delete.
if(gameSaveOperationResult.Status == GameSaveErrorStatus.Ok)
{
    //Successful Deletion Logic
}
else
{
    //handle failed deletion logic. 
}

//DELETE A SAVE CONTAINER
GameSaveOperationResult deleteContainerResult = await gameSaveProvider.DeleteContainerAsync(c_saveContainerName);
//Parameter
//string name (name of container to be deleted)

//Check status of the operation to ensure successful delete.
if(deleteContainerResult.Status == GameSaveErrorStatus.Ok)
{
    //Successful Deletion Logic
}
else
{
    //handle failed deletion logic. 
}
```

連接儲存體 Api，針對 UWP 應用程式所述[Xbox Live API 參考](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage)。
若要查看連接的儲存體簽出的另一個範例[Xbox Live API 範例遊戲儲存專案](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave)。
