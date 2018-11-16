---
author: stevewhims
description: 使用背景傳輸 API 在網路上可靠地複製檔案。
title: 背景傳輸
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
ms.author: stwhi
ms.date: 03/23/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6bc007ee1725ea3048895ccb9e7340bc0f08e8b8
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6972346"
---
# <a name="background-transfers"></a>背景傳輸
使用背景傳輸 API 在網路上可靠地複製檔案。 背景傳輸 API 提供進階的上傳和下載功能，這些功能會在 app 暫停期間於背景執行，並在 app 終止後保留。 API 會監視網路狀態，並自動在連線中斷時暫停和繼續傳輸，傳輸作業會是數據用量感知和電池用量感知，這表示下載活動會根據您目前的連線能力與裝置電池狀態進行調整。 API 適用於上傳和下載使用 HTTP(S) 的大型檔案。 也支援 FTP，但只限於下載項目。

背景傳輸會與呼叫 app 分開執行，而且其設計主要是針對影片、音樂和大型影像等資源的長時間傳輸操作所設計。 對於這些案例，必須使用背景傳輸，因為 app 即使暫停，下載還是會繼續進行。

如果您是下載可以快速完成的小量資源，則應該改用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) API 來取代背景傳輸。

## <a name="using-windowsnetworkingbackgroundtransfer"></a>使用 Windows.Networking.BackgroundTransfer

### <a name="how-does-the-background-transfer-feature-work"></a>背景傳送功能如何運作？
當應用程式使用背景傳輸來起始傳輸時，會使用 [**BackgroundDownloader**](https://msdn.microsoft.com/library/windows/apps/br207126) 或 [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 類別物件設定和初始化要求。 每個傳輸作業會由系統分別處理，並與呼叫的應用程式分隔。 如果您想要在應用程式 UI 中為使用者提供狀態，而且應用程式可以在傳輸進行暫停、繼續、取消，甚至讀取資料，則可以使用進度資訊。 系統處理傳輸的方式可以運用更智慧的電力使用方法，而且可以避免當連線 app 發生 app 暫停、終止或是突發性網路狀態變更這類事件時引發的問題。

> [!NOTE]
> 由於每個 app 的資源限制，因此應用程式在任何時候都不應有超過 200 個傳輸 (DownloadOperations + UploadOperations)。 超過該限制可能會使得應用程式的傳輸佇列處於無法復原狀態。

啟動應用程式時，它必須在所有的現有[**DownloadOperation**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation?branch=live)和[**UploadOperation**](/uwp/api/windows.networking.backgroundtransfer.uploadperation?branch=live)物件上呼叫[**Downloadoperation**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation.AttachAsync) 。 未執行此動作會導致已經完成傳輸流失，並將最終轉譯您使用背景傳輸功能都毫無用處。

### <a name="performing-authenticated-file-requests-with-background-transfer"></a>使用背景傳輸來執行已驗證的檔案要求
背景傳輸提供的方法，支援基本伺服器與 Proxy 認證、Cookie，並且支援在每個傳輸作業使用自訂的 HTTP 標頭 (透過 [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146))。

### <a name="how-does-this-feature-adapt-to-network-status-changes-or-unexpected-shutdowns"></a>這項功能如何適應網路狀態變更或意外的關機？
當網路狀態變更時，背景傳輸功能會維護每個傳輸操作的一致體驗，以智慧方式使用[連線能力](https://msdn.microsoft.com/library/windows/apps/hh452990)功能所提供的連線能力與電信業者數據傳輸方案狀態資訊。 若要定義不同網路案例的行為，app 會使用 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 定義的值來設定每個傳輸作業的成本原則。

例如，為作業定義的成本原則可以指出裝置使用計量付費網路時應該自動暫停作業。 當建立「不受限制」網路的連線時，會自動繼續 (或重新啟動) 傳輸。 如需如何以成本定義網路的詳細資訊，請參閱 [**NetworkCostType**](https://msdn.microsoft.com/library/windows/apps/br207292)。

儘管背景傳輸功能有它自己的網路狀態變更處理機制，網路連線的應用程式還有其他一般連線考量。 請參閱[利用可用的網路連線資訊](https://msdn.microsoft.com/library/windows/apps/hh452983)來取得其他資訊。

> **注意：** 的行動裝置上執行的應用程式，有些功能讓使用者來監視和限制傳輸的資料是根據連線類型、 漫遊狀態數量及使用者資料的計劃。 因此，即使 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 指示傳輸應該繼續，手機上的背景傳輸還是可能被暫停。

下表說明每個 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 值何時可在手機上允許背景傳輸 (根據手機的目前狀態)。 您可以使用 [**ConnectionCost**](https://msdn.microsoft.com/library/windows/apps/br207244) 類別來判定電話的目前狀態。

| 裝置狀態                                                                                                                      | 僅無限制 | 預設值 | 永遠 |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| 連線到 WiFi                                                                                                                 | 允許            | 允許   | 允許  |
| 計量付費連線、非漫遊、低於資料限制、追蹤以維持低於限制                                                   | 拒絕             | 允許   | 允許  |
| 計量付費連線、非漫遊、低於資料限制、追蹤是否超過限制                                                       | 拒絕             | 拒絕    | 允許  |
| 計量付費連線、漫遊、低於資料限制                                                                                     | 拒絕             | 拒絕    | 允許  |
| 計量付費連線、超過資料限制 只有當使用者啟用「限制資料感應 UI 中的背景資料」時才會發生這個狀態。 | 拒絕             | 拒絕    | 拒絕   |

## <a name="uploading-files"></a>上傳檔案
使用背景傳輸時，上傳以 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 的方式存在，可公開用於重新啟動或取消作業的一些控制方法。 系統會對每個 **UploadOperation** 自動處理 app 事件 (例如暫停或終止) 和連線變更；在 app 暫停期間上傳仍將繼續，或在 app 終止之後會暫停或持續下去。 此外，設定 [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) 屬性將指出您的 app 是否開始上傳，而且網際網路連線會使用計量付費網路。

以下的範例會逐步引導您建立和初始化基本上傳，以及如何列舉和重新引入之前 app 工作階段中的持續操作。

### <a name="uploading-a-single-file"></a>上傳單一檔案
上傳的建立從 [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 開始。 這個類別用來提供讓您的 app 先設定上傳，再建立結果 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 的方法。 下列範例示範如何使用必要的 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 和 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件來執行此動作。

**識別上傳的檔案和目的地**

在開始建立 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 之前，我們必須識別要上傳的目標位置 URI，以及要上傳的檔案。 在下列範例中，填入 *uriString* 值的方式是使用 UI 輸入中的字串，而填入 *file* 值的方式則是使用 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) 操作傳回的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件。

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_B "Identify the file and destination for the upload")]

**建立和初始化上傳作業**

在上一個步驟中，*uriString* 和 *file* 值已傳遞至我們下一個範例 UploadOp 的執行個體，這兩個值將被用來設定和啟動新的上傳操作。 首先，會剖析 *uriString* 以建立必要的 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 物件。

接下來，[**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 會使用所提供之 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) (*file*) 的屬性來填入要求標頭，並以 **StorageFile** 物件來設定 *SourceFile* 屬性。 接著會呼叫 [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146) 方法，插入以字串方式提供的檔案名稱和 [**StorageFile.Name**](https://msdn.microsoft.com/library/windows/apps/br227220) 屬性。

最後，[**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 會建立 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) (*upload*)。

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_A "Create and initialize the upload operation")]

請注意，非同步方法呼叫是使用 JavaScript Promise 定義的。 看看上個範例的行：

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

非同步呼叫後面跟著一個指示方法的 then 陳述式，由應用程式定義，會在非同步方法呼叫傳回結果時呼叫它。 如需這種程式設計模式的詳細資訊，請參閱[使用 Promise 在 JavaScript 的非同步程式設計](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx)。

### <a name="uploading-multiple-files"></a>上傳多個檔案
**識別上傳的檔案和目的地**

在使用一個 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 傳輸多個檔案的案例中，程序會照一般的方法開始，也就是先提供必要的目的地 URI 和本機檔案資訊。 就像上一節的範例，使用者以字串提供 URI，而 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 可以用來透過使用者介面指出檔案。 不過，在這個案例中，應用程式應該改為呼叫 [**PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851) 方法，啟用透過 UI 選取多個檔案的功能。

```javascript
function uploadFiles() {
       var filePicker = new Windows.Storage.Pickers.FileOpenPicker();
       filePicker.fileTypeFilter.replaceAll(["*"]);

       filePicker.pickMultipleFilesAsync().then(function (files) {
          if (files === 0) {
             printLog("No file selected");
                return;
          }

          var upload = new UploadOperation();
          var uriString = document.getElementById("serverAddressField").value;
          upload.startMultipart(uriString, files);

          // Persist the upload operation in the global array.
          uploadOperations.push(upload);
       });
    }
```

**針對提供的參數建立物件**

接下來的兩個範例使用單一範例方法 **startMultipart** 中的程式碼，並在上個步驟的結尾呼叫它。 為了清楚說明，已將建立 [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) 物件陣列方法中的程式碼與建立結果 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 的程式碼分開。

首先，使用者提供的 URI 字串被初始化為 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)。 接下來，會針對傳送給這個方法的 [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) 物件陣列 (**files**) 逐一查看，並使用每個物件來建立新的 [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) 物件，然後放置在 **contentParts** 陣列中。

```javascript
    upload.startMultipart = function (uriString, files) {
        try {
            var uri = new Windows.Foundation.Uri(uriString);
            var uploader = new Windows.Networking.BackgroundTransfer.BackgroundUploader();

            var contentParts = [];
            files.forEach(function (file, index) {
                var part = new Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart("File" + index, file.name);
                part.setFile(file);
                contentParts.push(part);
            });
```

**建立和初始化多部分上傳作業**

使用我們的 contentParts 陣列 (其中已填入代表每個要上傳的 [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) 的所有 [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) 物件)，我們已準備好使用 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 來呼叫 [**CreateUploadAsync**](https://msdn.microsoft.com/library/windows/apps/hh923973)，以指示傳送要求的目的地。

```javascript
        // Create a new upload operation.
            uploader.createUploadAsync(uri, contentParts).then(function (uploadOperation) {

               // Start the upload and persist the promise to be able to cancel the upload.
               upload = uploadOperation;
               promise = uploadOperation.startAsync().then(complete, error, progress);
            });

         } catch (err) {
             displayError(err);
         }
     };
```

### <a name="restarting-interrupted-upload-operations"></a>正在重新啟動已中斷的上傳操作
在 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 完成或取消時，會釋放任何關聯的系統資源。 不過，如果您的應用程式在這兩件事發生之前終止，就會暫停任何進行中的作業，而且仍會佔用與每個作業關聯的資源。 如果這些操作沒有列舉且重新引回下一個應用程式工作階段，這些操作將不會完成，而且仍然會繼續佔用裝置資源。

1.  在定義列舉持續作業的功能之前，我們必須建立一個陣列來包含它將傳回的 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 物件：

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Restart interrupted upload operation")]

1.  接著我們要定義列舉持續作業的函式，然後將它們儲存在陣列。 請注意，將回呼重新指派到 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 時所呼叫的 **load** 方法 (應在應用程式終止期間都持續著)，位於我們稍後在本節所定義的 UploadOp 類別中。

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Enumerate persisted operations")]

## <a name="downloading-files"></a>下載檔案
使用背景傳輸時，每個下載以 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 的方式存在，可公開用於暫停、繼續、重新啟動和取消作業的一些控制方法。 系統會對每個 **DownloadOperation** 自動處理 app 事件 (例如暫停或終止) 和連線變更；在 app 暫停期間下載仍將繼續，或在 app 終止之後會暫停或持續下去。 對於行動網路案例，設定 [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) 屬性將指出您的 app 是否將開始或繼續下載 (當網際網路連線使用計量付費網路時)。

如果您是下載可以快速完成的小量資源，則應該改用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) API 來取代背景傳輸。

以下的範例會逐步引導您建立和初始化基本下載，以及如何列舉和重新引進之前 app 工作階段中的持續作業。

### <a name="configure-and-start-a-background-transfer-file-download"></a>設定和啟動背景傳輸檔案下載
下列範例示範代表 URI 和檔案名稱的字串如何可以用來建立 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 物件，以及將包含所要求檔案的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)。 在這個範例中，會自動將新檔案放在預先定義的位置。 或者，您可以使用 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) 讓使用者指出將檔案儲存在裝置上的哪個位置。 請注意，將回呼重新指派到 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 時所呼叫的 **load** 方法 (應於 app 終止期間都持續著)，位於稍後在本節所定義的 DownloadOp 類別中。

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

請注意，非同步方法呼叫是使用 JavaScript Promise 定義的。 請查看前面程式碼範例的第 17 行：

```javascript
promise = download.startAsync().then(complete, error, progress);
```

非同步呼叫後面跟著一個指示方法的 then 陳述式，由應用程式定義，會在非同步方法呼叫傳回結果時呼叫它。 如需這種程式設計模式的詳細資訊，請參閱[使用 Promise 在 JavaScript 的非同步程式設計](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx)。

### <a name="adding-additional-operation-control-methods"></a>新增其他的操作控制項方法
可以藉由實作額外的 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 方法來增加控制層級。 例如，將下列程式碼新增至上述範例將引進取消下載的功能。

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### <a name="enumerating-persisted-operations-at-start-up"></a>在啟動時列舉持續作業
在 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 完成或取消時，會釋放任何關聯的系統資源。 不過，如果應用程式在上述任一事件發生之前就終止了，則下載會暫停並保留在背景中。 下面的範例示範如何將保留的下載重新引入新的應用程式工作階段中。

1.  在定義列舉持續作業的功能之前，我們必須建立一個陣列來包含它將傳回的 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 物件：

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

1.  接著我們要定義列舉持續作業的函式，然後將它們儲存在陣列。 請注意，重新指派回呼以進行持續 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 時所呼叫的 **load** 方法，位於我們稍後在本節所定義的 DownloadOp 範例中。

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

1.  您現在可以使用填入的清單重新啟動擱置的作業。

## <a name="post-processing"></a>後續處理
Windows 10 中的新功能是能夠在背景傳輸完成時執行應用程式程式碼，即使在應用程式未執行時。 例如，您的 app 可能會想要在影片完成下載後更新可用的影片清單，而不是每次啟動 app 時掃描新的影片。 或者，您的 app 可能會想要嘗試使用不同的伺服器或連接埠，重新處理失敗的檔案傳輸。 成功和失敗的傳輸都會叫用後續處理，因此您可以用它來實作自訂的錯誤處理和重試邏輯。

Postprocessing 會使用現有的背景工作基礎結構。 您可以建立背景工作，並將它與傳輸建立關聯之後再開始傳輸。 傳輸接著會在背景執行，並在完成時呼叫您的背景工作以執行後續處理。

後續處理會使用新的類別：[**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209)。 這個類別類似於現有的 [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030)，在此類別中，它可以讓您將背景傳輸群組在一起，但 **BackgroundTransferCompletionGroup** 會新增傳輸完成時指定待執行背景工作的能力。

起始背景傳輸與後續處理，如下所示。

1.  建立 [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209) 物件。 接著，建立 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 物件。 將建立器物件的 **Trigger** 屬性設定為完成群組物件，並將建立器的 **TaskEntryPoint** 屬性設定為應在傳輸完成時執行之背景工作的進入點。 最後，呼叫 [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 方法來登錄背景工作。 請注意，許多完成群組可以共用一個背景工作進入點，但每個背景工作登錄只能有一個完成群組。

```csharp
var completionGroup = new BackgroundTransferCompletionGroup();
BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

builder.Name = "MyDownloadProcessingTask";
builder.SetTrigger(completionGroup.Trigger);
builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

BackgroundTaskRegistration downloadProcessingTask = builder.Register();
```

2.  接下來，將背景傳輸與完成群組建立關聯。 建立所有傳輸之後，啟用完成群組。

```csharp
BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
DownloadOperation download = downloader.CreateDownload(uri, file);
Task<DownloadOperation> startTask = download.StartAsync().AsTask();

// App still sees the normal completion path
startTask.ContinueWith(ForegroundCompletionHandler);

// Do not enable the CompletionGroup until after all downloads are created.
downloader.CompletinGroup.Enable();
```

3.  背景工作中的程式碼會從觸發詳細資料中擷取操作清單，您的程式碼接著可以檢查每個操作的詳細資料，並為每個操作執行適當的後續處理。

```csharp
public class BackgroundDownloadProcessingTask : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {
    var details = (BackgroundTransferCompletionGroupTriggerDetails)taskInstance.TriggerDetails;
    IReadOnlyList<DownloadOperation> downloads = details.Downloads;

    // Do post-processing on each finished operation in the list of downloads
    }
}
```

後續處理工作是一般的背景工作。 它是所有背景工作集區的一部分，它會受限於與所有背景工作一樣的相同資源管理原則。

另外，請注意，後續處理不會取代前景完成處理常式。 如果您的 app 定義前景完成處理常式，且檔案傳輸完成時您的 app 正在執行中，則系統會呼叫前景完成處理常式和背景完成處理常式。 我們不保證前景和背景工作的呼叫順序。 如果您定義兩者，您應該確定這兩個工作將會正常運作，而且在兩者同時執行的情況下不會互相干擾。

## <a name="request-timeouts"></a>要求逾時
要考量的主要連線逾時情況有兩種：

-   建立要傳輸的新連線時，如果在五分鐘內未建立連線要求，將會中止該要求。

-   建立連線之後，將中止任何在兩分鐘內未收到回應的 HTTP 要求訊息。

> **注意：** 在任一種情況下，假設有網際網路連線，背景傳輸會自動重試要求三次。 在偵測不到網際網路連線的事件中，其他要求將等到偵測到連線為止。

## <a name="debugging-guidance"></a>偵錯指導方針
在 Microsoft Visual Studio 中停止偵錯工作階段就等同於關閉 app；PUT 上傳會被暫停，POST 上傳會被終止。 即使在偵錯時，應用程式應該列舉然後重新啟動或取消任何之前仍然存在的下載。 例如，如果偵錯工作階段與之前的操作無關，您可以在應用程式啟動時，讓應用程式取消已列舉的持續上傳作業。

偵錯工作階段期間在應用程式啟動時列舉下載/上傳，如果該偵錯工作階段與之前的作業無關，您可以讓應用程式取消它們。 請注意，如果有 Visual Studio 專案更新，像是變更 app 資訊清單，而 app 已解除安裝並重新部署，則 [**GetCurrentUploadsAsync**](https://msdn.microsoft.com/library/windows/apps/hh701149) 無法列舉使用前一個 app 部署建立的作業。

開發期間使用背景傳送時，您可能會面臨使用中及已完成傳送作業的內部快取不同步的情況。這會導致無法開始新的傳送作業或無法與現有的作業和 [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) 物件互動。 在某些情況下，嘗試與現有作業互動會造成當機。 如果 [**TransferBehavior**](https://msdn.microsoft.com/library/windows/apps/dn279033) 屬性設為 **Parallel**，就會發生這種結果。 這個問題只在開發期間的特定情況下發生，不適用於您應用程式的一般使用者。

使用 Visual Studio 時有四種情況會導致這個問題。

-   您使用與現有專案相同的名稱但不同的語言 (例如，從 C++ 變更為 C#) 建立新的專案。
-   您變更現有專案中的目標架構 (例如，從 x86 變更為 x64)。
-   您變更現有專案中的文化特性 (例如，從中性變更為 en-US)。
-   您在現有專案的套件資訊清單中新增或移除功能 (例如，新增 **\[企業驗證\]**)。

一般應用程式服務，包括新增或移除功能的資訊清單更新，並不會在應用程式的一般使用者部署上引起這個問題。
若要解決這個問題，請完整解除安裝應用程式的所有版本，然後使用新的語言、架構、文化特性或功能來重新部署。 這個操作可以透過 **\[開始\]** 畫面或使用 PowerShell 和 **Remove-AppxPackage** Cmdlet 來完成。

## <a name="exceptions-in-windowsnetworkingbackgroundtransfer"></a>Windows.Networking.BackgroundTransfer 中的例外狀況
如果傳送到 [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 物件建構函式的統一資源識別項 (URI) 字串無效時，即會擲回例外狀況。

**.NET：**[**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 型別在 C# 和 VB 中顯示為 [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx)。

在 C# 和 Visual Basic 中，可在建構 URI 之前，於 .NET 4.5 中使用 [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) 類別和其中一個 [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) 方法來測試接收自應用程式使用者的字串，以避免發生這個錯誤。

在 C++ 中，沒有可以嘗試將字串剖析為 URI 的方法。 如果應用程式取得使用者為 [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 輸入的值，則建構函式應在 try/catch 區塊中。 如果發生例外狀況，app 可通知使用者並要求新的主機名稱。

[**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) 命名空間具備便利的協助程式方法，可以在 [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空間中使用列舉來處理錯誤。 這對於在您的應用程式中以不同的方式處理特定網路例外狀況時很有用。

在 [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) 命名空間中非同步方法內遇到的錯誤會以 **HRESULT** 值的形式傳回。 使用 [**BackgroundTransferError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701093) 方法，將背景傳輸作業的網路錯誤轉換為 [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) 列舉值。 大多數 **WebErrorStatus** 列舉值都會對應到原始 HTTP 或 FTP 用戶端作業所傳回的錯誤。 app 可以篩選特定 **WebErrorStatus** 列舉值，依據例外狀況的發生原因來修改 app 行為。

針對參數驗證錯誤，app 也可以使用來自例外狀況的 **HRESULT**，深入了解更多關於導致例外狀況的錯誤詳細資訊。 可能的 **HRESULT** 值列在 *Winerror.h* 標頭檔中。 針對大多數的參數驗證錯誤，傳回的 **HRESULT** 是 **E\_INVALIDARG**。

## <a name="important-apis"></a>重要 API
* [**Windows.Networking.BackgroundTransfer**](/uwp/api/windows.networking.backgroundtransfer)
* [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri)
* [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets)
