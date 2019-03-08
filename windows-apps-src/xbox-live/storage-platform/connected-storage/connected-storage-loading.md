---
title: 使用連接儲存體將資料載入
description: 了解如何使用連接儲存體載入資料。
ms.assetid: c660a456-fe7d-453a-ae7b-9ecaa2ba0a15
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，連接的儲存體
ms.localizationpriority: medium
ms.openlocfilehash: a2f7498e8063e290dc506de72b34d2c77d29b26e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637133"
---
# <a name="use-connected-storage-to-load-data"></a>使用連接儲存體將資料載入

使用以非同步方式讀取資料`ReadAsync`或`GetAsync`連線儲存方法。

### <a name="to-load-data-from-connected-storage"></a>若要從連接的儲存體載入資料

1.  擷取`ConnectedStorageSpace`藉由呼叫使用者`GetForUserAsync`。

    在傳回的 XDK 範例`ConnectedStorageSpace`要加入至地圖，讓您輕鬆管理`ConnectedStorageSpace`多個使用者的物件。

2.  建立`ConnectedStorageContainer`藉由呼叫`CreateContainer`上`ConnectedStorageSpace`。
3.  擷取資料，藉由呼叫`ReadAsync`或是`GetAsync`上`ConnectedStorageContainer`。 `ReadAsync` 需要您傳入緩衝區，以同時`GetAsync`會配置新緩衝區，以儲存所讀取的資料。

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

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status);

// Load data from a fixed container/blob name into an IBuffer
void Load(Windows::Storage::Streams::IBuffer^ buffer)
{

    auto reads = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
    reads->Insert("data", buffer);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(gCurrentUser->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    //Save Data is Loading

    auto op = container->ReadAsync(reads->GetView());

    op->Completed = ref new AsyncActionCompletedHandler(OnLoadCompleted);
}

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status)
{
    switch (action->Status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        //Successful load logic here.
        break;

    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        //Fail logic here
        break;

    default:
        //all other possible values of action->status are also failures, alternate fail logic here. 
        break;
    }
}
```

您可以找到 XDK 連接儲存體 Api 記載於路徑下的 XDK.chm 檔案：**Xbox 一個 XDK >> API 參考 >> 平台 API 參考 >> 系統 API 參考 >> Windows.Xbox.Storage**。
XDK Api 也會記載於[developer.microsoft.com 網站](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n)。
XDK Api 的連結，您需要已啟用 Xbox 開發人員 Kit(XDK) 存取 Microsoft 帳戶 （msa）。
Windows.Xbox.Storage 是 Xbox One 主控台連接的儲存體命名空間的名稱。

## <a name="c-uwp-sample"></a>C#UWP 範例

XDK 遊戲和 UWP 應用程式可能使用不同的 Api，而 UWP API 被仿造 XDK API 非常密切。 若要將資料載入您仍然必須遵循相同的基本步驟，同時讓某些命名空間和類別名稱變更的附註。 而不是使用命名空間`Windows::Xbox::Storage`您將使用`Windows.Gaming.XboxLive.Storage`。 此類別`ConnectedStorageSpace`，相當於`GameSaveProvider`。 此類別`ConnectedStorageContainer`相當於`GameSaveContainer`。 這些變更連接儲存體一節中進一步詳述[移植 Xbox Live 的程式碼從 XDK 至 UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md)。

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 0;
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
    //throw new Exception("Game Save Provider Initialization failed");;
}

//Now you have a GameSaveProvider
//Next you need to call CreateContainer to get a GameSaveContainer

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName);
//Parameter
//string name (name of the GameSaveContainer Created)

//form an array of strings containing the blob names you would like to read.
string[] blobsToRead = new string[] { c_saveBlobName };

// GetAsync allocates a new Dictionary to hold the retrieved data. You can also use ReadAsync
// to provide your own preallocated Dictionary.
GameSaveBlobGetResult result = await gameSaveContainer.GetAsync(blobsToRead);

int loadedData = 0;

//Check status to make sure data was read from the container
if(result.Status == GameSaveErrorStatus.Ok)
{
    //prepare a buffer to receive blob
    IBuffer loadedBuffer;

    //retrieve the named blob from the GetAsync result, place it in loaded buffer.
    result.Value.TryGetValue(c_saveBlobName, out loadedBuffer);

    if(loadedBuffer == null)
    {

        throw new Exception(String.Format("Didn't find expected blob \"{0}\" in the loaded data.", c_saveBlobName));

    }
    DataReader reader = DataReader.FromBuffer(loadedBuffer);
    loadedData = reader.ReadInt32();
}
```

連接儲存體 Api，針對 UWP 應用程式所述[Xbox Live API 參考](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage)。
若要查看連接的儲存體簽出的另一個範例[Xbox Live API 範例遊戲儲存專案](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave)。