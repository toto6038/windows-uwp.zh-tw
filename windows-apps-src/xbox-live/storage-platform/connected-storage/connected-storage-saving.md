---
title: 使用連接儲存體來儲存資料
description: 了解如何使用連接儲存體來儲存資料。
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，連接的儲存體
ms.localizationpriority: medium
ms.openlocfilehash: 4140e3bbe0f0ab229e3637008e01892f4179292e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617613"
---
# <a name="use-connected-storage-to-save-data"></a>使用連接儲存體來儲存資料


藉由建立以非同步方式儲存資料`ConnectedStorageContainer`中`ConnectedStorageSpace`使用者及呼叫`SubmitUpdatesAsync`容器上的方法。

> [!IMPORTANT]
> 連接的儲存體容器之間的資料相依性是不安全。 例如上, 傳至雲端的一個容器可能會完成，而另一個可能會保留在佇列上傳。 如果使用者移至另一個主控台，同步處理作業可讓同步處理和在第二個主控台中，存取不存在的第一個容器的第一個容器。

## <a name="to-save-data-to-connected-storage"></a>若要將資料儲存至連接的儲存體

1.  擷取`ConnectedStorageSpace`藉由呼叫使用者物件`GetForUserAsync`。

    在傳回的 XDK 範例`ConnectedStorageSpace`物件會被新增至地圖，讓您輕鬆管理`ConnectedStorageSpace`多個使用者的物件。

2.  建立`ConnectedStorageContainer`藉由呼叫物件`CreateContainer`上`ConnectedStorageSpace`物件。
3.  呼叫`SubmitUpdatesAsync`上`ConnectedStorageContainer`遊戲儲存資料的 blob，作為與`blobsToWrite`參數。

## <a name="c-xdk-sample"></a>C + + XDK 範例

```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() {return true;};

User^ gCurrentUser;
IBuffer^ WrapRawBuffer( void* ptr, size_t size );

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

enum Color { RED, BLUE };
enum EngineSize { BIG, SMALL };

struct CarData
{
    Color color;
    bool hasWheels;
    bool hasFancyRims;
    EngineSize engineSize;
};


const int MAX_CARS = 10;

struct SaveData
{
    CarData cars[MAX_CARS];
    int numCars;
    int currentCar;
    int cash;
};

SaveData gMySaveData;

void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user);

bool ItIsTimeToSaveACheckpoint() {return true;};

void Update()
{
    if (ItIsTimeToSaveACheckpoint())
        SaveCheckpoint(WrapRawBuffer(&gMySaveData,sizeof(SaveData)),gCurrentUser);
}


void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user)
{
     auto storageSpace = gConnectedStorageSpaceForUsers->Lookup( user->Id );

     auto container = storageSpace->CreateContainer("Saves/Checkpoint");

     auto updates = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
     updates->Insert("data", buffer);

     auto op = container->SubmitUpdatesAsync(updates->GetView(), nullptr);

     //Save is happening here asynchronously

     op->Completed = ref new AsyncActionCompletedHandler(
               [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
                   //Save function has completed
                   //This area can be filled with further post save logic.
     });
}
```

您可以找到 XDK 連接儲存體 Api 記載於路徑下的 XDK.chm 檔案：**Xbox 一個 XDK >> API 參考 >> 平台 API 參考 >> 系統 API 參考 >> Windows.Xbox.Storage**。
XDK Api 也會記載於[developer.microsoft.com 網站](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n)。
XDK Api 的連結，您需要已啟用 Xbox 開發人員 Kit(XDK) 存取 Microsoft 帳戶 （msa）。
Windows.Xbox.Storage 是 Xbox One 主控台連接的儲存體命名空間的名稱。

## <a name="c-uwp-sample"></a>C#UWP 範例

XDK 遊戲和 UWP 應用程式可能使用不同的 Api，而 UWP API 被仿造 XDK API 非常密切。 若要儲存的資料仍然必須遵循相同的基本步驟，同時讓某些命名空間和類別名稱變更的附註。 而不是使用命名空間`Windows::Xbox::Storage`您將使用`Windows.Gaming.XboxLive.Storage`。 此類別`ConnectedStorageSpace`，相當於`GameSaveProvider`。 此類別`ConnectedStorageContainer`相當於`GameSaveContainer`。 這些變更連接儲存體一節中進一步詳述[移植 Xbox Live 的程式碼從 XDK 至 UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md)。

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
//string name

// To store a value in the container, it needs to be written into a buffer, then stored with
// a blob name in a Dictionary.

DataWriter writer = new DataWriter();

writer.WriteInt32(intData); //some number you want to save, in this case 23.

IBuffer dataBuffer = writer.DetachBuffer();

var blobsToWrite = new Dictionary<string, IBuffer>();

blobsToWrite.Add(c_saveBlobName, dataBuffer);

GameSaveOperationResult gameSaveOperationResult = await gameSaveContainer.SubmitUpdatesAsync(blobsToWrite, null, c_saveContainerDisplayName);
//IReadOnlyDictionary<String, IBuffer> blobsToWrite
//IEnumerable<string> blobsToDelete
//string displayName
```

連接儲存體 Api，針對 UWP 應用程式所述[Xbox Live API 參考](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage)。
若要查看連接的儲存體簽出的另一個範例[Xbox Live API 範例遊戲儲存專案](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave)。