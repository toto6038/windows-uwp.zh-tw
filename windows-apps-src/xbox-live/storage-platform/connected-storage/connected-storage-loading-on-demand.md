---
title: 連接隨選的儲存體載入
description: 了解如何將連接儲存體的資料，視情況下，而不是全部一次。
ms.assetid: a0797a14-c972-4017-864c-c6ba0d5a3363
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，連接的儲存體
ms.localizationpriority: medium
ms.openlocfilehash: 31c1893f09e6d56682b4e718ee8b905ce72c7ad8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631713"
---
# <a name="connected-storage-loading-on-demand"></a>連接隨選的儲存體載入

`GetSyncOnDemandForUserAsync` 可讓您以雲端為基礎的資料，從連接的儲存空間 「 依需求 」 載入而不是全部一次。 這可以改善效能`GetForUserAsync`檔案的儲存位置的情況下很特別大。

## <a name="to-load-data-from-a-connected-storage-space-on-demand"></a>將資料從連接的儲存空間，依需求載入

### <a name="1--call-getsyncondemandforuserasync"></a>1：呼叫 `GetSyncOnDemandForUserAsync`

這會觸發下載一份容器以及它們的中繼資料從雲端，但不是包括其內容的部分同步處理。 這項作業是快速，而且狀況良好的網路時，使用者應該不會看到任何載入 UI。

```cpp
auto op = ConnectedStorageSpace::GetSyncOnDemandForUserAsync(user);
op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
{
    switch (status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        auto storageSpace = operation->GetResults();
        break;
    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        // Present user option: ?Would you like to continue without saving progress??
        if (GetUserInputYesOrNo())
            SetGameState(LoadSaveState::NO_SAVE_MODE);
        else
            SetGameState(LoadSaveState::RETRY_LOAD);
        break;
    }
});
```

```csharp
var users = await Windows.System.User.FindAllAsync();

GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetSyncOnDemandForUserAsync(users[0], context.AppConfig.ServiceConfigurationId); 
//Paramaters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
```


### <a name="2--perform-a-container-query-using-getcontainerinfo2async"></a>2：執行容器的查詢使用 `GetContainerInfo2Async`

這會傳回的集合`ContainerInfo2`，其中包含 3 個新的中繼資料欄位：

    -   `DisplayName`:您已撰寫使用超載的任何顯示名稱`SubmitUpdatesAsync`會做為參數的顯示名稱字串。 我們建議一律使用此欄位，來儲存使用者易記的名稱。
    -   `LastModifiedTime`:上次修改此容器。 請注意在衝突的本機和遠端時間戳記，使用遠端的項目。
    -   `NeedsSync`:布林值，指出此容器具有資料必須從雲端下載。

    使用此額外的中繼資料，您可以呈現給使用者使用他們的遊戲儲存的核心資訊 （包括名稱、 最後一次使用，以及是否選取其中一項需要下載） 而不實際從雲端中執行完整的下載。

```cpp
auto containerQuery = storageSpace->CreateContainerInfoQuery(nullptr); //return list of containers in ConnectedStorageSpace
auto queryOperation = containerQuery->GetContainerInfo2Async();

queryOperation->Completed = ref new AsyncOperationCompletedHandler<IVectorView<ContainerInfo2>^ >( 
    [=] (IAsyncOperation<IVectorView<ContainerInfo2>^ >^ operation, Windows::Foundation::AsyncStatus status)
    {
        switch (status)
        {
        case Windows::Foundation::AsyncStatus::Completed:
            // get the resulting vector of container info
            auto infoVector = operation->GetResults();
            break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
            // handle error cases
            break;
        }
    });
```

```csharp
GameSaveContainerInfoQuery infoQuery = gameSaveProvider.createContainerInfoQuery();
GameSaveContainerInfoGetResult containerInfoResult = await infoQuery.GetContainerInfoAsync();
var containerInfoList;

if(containerInfoResult.Status == GameSaveErrorStatus.Ok)
{
    containerInfoList = containerInfoResult.value;
}

// Use the containerInfoList to inform further actions or display container data to user. 
```

### <a name="3--trigger-a-sync"></a>3：觸發同步處理

藉由呼叫任何下列的現有連接的儲存體 API，將會觸發已連接的儲存體 synce:

**C++**

    -   BlobInfoQueryResult::GetBlobInfoAsync
    -   BlobInfoQueryResult::GetItemCountAsync
    -   ConnectedStorageContainer::GetAsync
    -   ConnectedStorageContainer::ReadAsync
    -   ConnectedStorageSpace::DeleteContainerAsync

**C#**

    -   GameSaveBlobInfoQuery.GetBlobInfoAsync
    -   GameSaveBlobInfoQuery.GetItemCountAsync
    -   GameSaveContainer.GetAsync
    -   GameSaveContainer.ReadAsync
    -   GameSaveProvider.DeleteContainerAsync

這會導致使用者下載資料從選取的容器時，請參閱一般的同步處理 UI 及進度列。 請注意，只有從選取的容器的資料同步處理;從其他容器的資料不會下載。

在呼叫這些 API 上的隨選同步處理的內容中，這些作業可能所有會產生下列新的錯誤碼：

-   `ConnectedStorageErrorStatus::ContainerSyncFailed`(`GameSaveErrorStatus.ContainerSyncFailed` UWP 中C#API):此錯誤表示作業失敗，並可能未與雲端同步處理的容器。 最可能的原因是使用者的網路狀況導致同步處理失敗。 它們可能會想要再試一次後他們已修正他們的網路，或他們可能會選擇要使用的容器，他們不需要同步處理。您的 UI 應該允許其中一個選項。 因為它們會已經看過的系統 UI 重試 對話方塊，為必要項，無重試 對話方塊。

-   `ConnectedStorageErrorStatus::ContainerNotInSync`(`GameSaveErrorStatus.ContainerNotInSync` UWP 中C#API):此錯誤指出您的標題不正確地嘗試寫入未同步處理的容器。 呼叫`ConnectedStorageContainer::SubmitUpdatesAsync`(`GameSaveContainer.SubmitUpdatesAsync` UWP 中C#API) 的 NeedsSync 旗標是在容器上不允許設為 true。 您必須先讀取容器來寫入它之前觸發的同步處理。 如果您將撰寫一個容器，而不閱讀，從它，很可能您的標題有的 bug，您將可能會遺失使用者進度。

此行為是不同的情況下，在使用者播放離線的時。 雖然離線狀態時，不會指示的容器是否會同步處理，因此使用者必須在稍後解決任何衝突。 在此情況下，不過，系統會知道使用者需要同步處理，因此它不會允許他們進入不正常的狀態 （但如果他們想要的選項，但它們仍會重新啟動標題，並離線播放） 使用過期的容器。

### <a name="4--use-the-rest-of-the-connected-storage-api-as-normal"></a>4:如往常一樣使用其餘的已連接的儲存體 API

依需求同步處理時，連接的儲存體行為保持不變。
