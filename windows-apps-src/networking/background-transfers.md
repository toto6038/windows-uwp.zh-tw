---
使用背景傳輸 API 在網路上可靠地複製檔案。
背景傳輸
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
---

# 背景傳輸

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242)
-   [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)
-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)

使用背景傳輸 API 在網路上可靠地複製檔案。 背景傳輸 API 提供進階的上傳和下載功能，這些功能會在 app 暫停期間於背景執行，並在 app 終止後保留。 API 會監視網路狀態，並自動在連線中斷時暫停和繼續傳輸，傳輸作業會是數據用量感知和電池用量感知，這表示下載活動會根據您目前的連線能力與裝置電池狀態進行調整。 API 適用於上傳和下載使用 HTTP(S) 的大型檔案。 也支援 FTP，但只限於下載項目。

背景傳輸會與呼叫 app 分開執行，而且其設計主要是針對影片、音樂和大型影像等資源的長時間傳輸操作所設計。 對於這些案例，必須使用背景傳輸，因為 app 即使暫停，下載還是會繼續進行。

如果您是下載可以快速完成的小量資源，則應該改用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) API 來取代背景傳輸。

## 使用 Windows.Networking.BackgroundTransfer


### 背景傳送功能如何運作？

當應用程式使用背景傳輸來起始傳輸時，會使用 [**BackgroundDownloader**](https://msdn.microsoft.com/library/windows/apps/br207126) 或 [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) 類別物件設定和初始化要求。 每個傳輸作業會由系統分別處理，並與呼叫的應用程式分隔。 如果您想要在應用程式 UI 中為使用者提供狀態，而且應用程式可以在傳輸進行暫停、繼續、取消，甚至讀取資料，則可以使用進度資訊。 系統處理傳輸的方式可以運用更智慧的電力使用方法，而且可以避免當連線 app 發生 app 暫停、終止或是突發性網路狀態變更這類事件時引發的問題。

### 使用背景傳輸來執行已驗證的檔案要求

背景傳輸提供的方法，支援基本伺服器與 Proxy 認證、Cookie，並且支援在每個傳輸作業使用自訂的 HTTP 標頭 (透過 [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146))。

### 這項功能如何適應網路狀態變更或意外的關機？

當網路狀態變更時，背景傳輸功能會維護每個傳輸操作的一致體驗，以智慧方式使用[連線能力](https://msdn.microsoft.com/library/windows/apps/hh452990)功能所提供的連線能力與電信業者數據傳輸方案狀態資訊。 若要定義不同網路案例的行為，app 會使用 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 定義的值來設定每個傳輸作業的成本原則。

例如，為作業定義的成本原則可以指出裝置使用計量付費網路時應該自動暫停作業。 當建立「不受限制」網路的連線時，會自動繼續 (或重新啟動) 傳輸。 如需如何以成本定義網路的詳細資訊，請參閱 [**NetworkCostType**](https://msdn.microsoft.com/library/windows/apps/br207292)。

儘管背景傳輸功能有它自己的網路狀態變更處理機制，網路連線的應用程式還有其他一般連線考量。 請參閱[利用可用的網路連線資訊](https://msdn.microsoft.com/library/windows/apps/hh452983)來取得其他資訊。

> **注意：**在行動裝置上執行的 app 中，有些功能讓使用者能夠根據連線類型、漫遊狀態及使用者數據傳輸方案來監視和限制傳輸的資料量。 因此，即使 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 指示傳輸應該繼續，手機上的背景傳輸還是可能被暫停。

下表說明每個 [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) 值何時可在手機上允許背景傳輸 (根據手機的目前狀態)。 您可以使用 [**ConnectionCost**](https://msdn.microsoft.com/library/windows/apps/br207244) 類別來判定電話的目前狀態。

| 裝置狀態                                                                                                                      | 僅無限制 | 預設值 | 永遠 |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| 連線到 WiFi                                                                                                                 | 允許            | 允許   | 允許  |
| 計量付費連線、非漫遊、低於資料限制、追蹤以維持低於限制                                                   | 拒絕             | 允許   | 允許  |
| 計量付費連線、非漫遊、低於資料限制、追蹤是否超過限制                                                       | 拒絕             | 拒絕    | 允許  |
| 計量付費連線、漫遊、低於資料限制                                                                                     | 拒絕             | 拒絕    | 允許  |
| 計量付費連線、超過資料限制 只有當使用者啟用「限制資料感應 UI 中的背景資料」時才會發生這個狀態。 | 拒絕             | 拒絕    | 拒絕   |

 

## 上傳檔案


使用背景傳輸時，上傳以 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 的方式存在，可公開用於重新啟動或取消作業的一些控制方法。 系統會對每個 **UploadOperation** 自動處理 app 事件 (例如暫停或終止) 和連線變更；在 app 暫停期間上傳仍將繼續，或在 app 終止之後會暫停或持續下去。 此外，設定 [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) 屬性將指出您的 app 是否開始上傳，而且網際網路連線會使用計量付費網路。

以下的範例會逐步引導您建立和初始化基本上傳，以及如何列舉和重新引入之前 app 工作階段中的持續操作。

### 上傳單一檔案

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

    The async method call is followed by a then statement which indicates methods, defined by the app, that are called when a result from the async method call is returned. For more information on this programming pattern, see [Asynchronous programming in JavaScript using promises](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### 上傳多個檔案

**識別上傳的檔案和目的地**

    In a scenario involving multiple files transferred with a single [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), the process begins as it usually does by first providing the required destination URI and local file information. Similar to the example in the previous section, the URI is provided as a string by the end-user and [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) can be used to provide the ability to indicate files through the user interface as well. However, in this scenario the app should instead call the [**PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851) method to enable the selection of multiple files through the UI.

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

    The next two examples use code contained in a single example method, **startMultipart**, which was called at the end of the last step. For the purpose of instruction the code in the method that creates an array of [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects has been split from the code that creates the resultant [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224).

    First, the URI string provided by the user is initialized as a [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998). Next, the array of [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) objects (**files**) passed to this method is iterated through, each object is used to create a new [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) object which is then placed in the **contentParts** array.

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

    With our contentParts array populated with all of the [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects representing each [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) for upload, we are ready to call [**CreateUploadAsync**](https://msdn.microsoft.com/library/windows/apps/hh923973) using the [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) to indicate where the request will be sent.

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

### 正在重新啟動已中斷的上傳作業

在 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 完成或取消時，會釋放任何關聯的系統資源。 不過，如果您的應用程式在這兩件事發生之前終止，就會暫停任何進行中的作業，而且仍會佔用與每個作業關聯的資源。 如果這些操作沒有列舉且重新引回下一個應用程式工作階段，這些操作將不會完成，而且仍然會繼續佔用裝置資源。

1.  在定義列舉持續作業的功能之前，我們必須建立一個陣列來包含它將傳回的 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 物件：

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Restart interrupted upload operation")]

2.  接著我們要定義列舉持續作業的功能，然後將這些作業儲存在我們的陣列。 請注意，將回呼重新指派到 [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) 時所呼叫的 **load** 方法 (應在應用程式終止期間都持續著)，位於我們稍後在本節所定義的 UploadOp 類別中。

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Enumerate persisted operations")]

## 下載檔案

使用背景傳輸時，每個下載以 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 的方式存在，可公開用於暫停、繼續、重新啟動和取消作業的一些控制方法。 系統會對每個 **DownloadOperation** 自動處理 app 事件 (例如暫停或終止) 和連線變更；在 app 暫停期間下載仍將繼續，或在 app 終止之後會暫停或持續下去。 對於行動網路案例，設定 [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) 屬性將指出您的 app 是否將開始或繼續下載 (當網際網路連線使用計量付費網路時)。

如果您是下載可以快速完成的小量資源，則應該改用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) API 來取代背景傳輸。

以下的範例會逐步引導您建立和初始化基本下載，以及如何列舉和重新引進之前 app 工作階段中的持續作業。

### 設定和啟動背景傳輸檔案下載

下列範例示範代表 URI 和檔案名稱的字串如何可以用來建立 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 物件，以及將包含所要求檔案的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)。 在這個範例中，會自動將新檔案放在預先定義的位置。 或者，您可以使用 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) 讓使用者指出將檔案儲存在裝置上的哪個位置。 請注意，將回呼重新指派到 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 時所呼叫的 **load** 方法 (應於 app 終止期間都持續著)，位於稍後在本節所定義的 DownloadOp 類別中。

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

請注意，非同步方法呼叫是使用 JavaScript Promise 定義的。 請查看前面程式碼範例的第 17 行：

```javascript
promise = download.startAsync().then(complete, error, progress);
```

非同步呼叫後面跟著一個指示方法的 then 陳述式，由應用程式定義，會在非同步方法呼叫傳回結果時呼叫它。 如需這種程式設計模式的詳細資訊，請參閱[使用 Promise 在 JavaScript 的非同步程式設計](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx)。

### 新增其他的操作控制項方法

可以藉由實作額外的 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 方法來增加控制層級。 例如，將下列程式碼新增至上述範例將引進取消下載的功能。

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### 在啟動時列舉持續作業

在 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 完成或取消時，會釋放任何關聯的系統資源。 不過，如果應用程式在上述任一事件發生之前就終止了，則下載會暫停並保留在背景中。 下面的範例示範如何將保留的下載重新引入新的應用程式工作階段中。

1.  在定義列舉持續作業的功能之前，我們必須建立一個陣列來包含它將傳回的 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 物件：

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

2.  接著我們要定義列舉持續作業的功能，然後將這些作業儲存在我們的陣列。 請注意，重新指派回呼以進行持續 [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) 時所呼叫的 **load** 方法，位於我們稍後在本節所定義的 DownloadOp 範例中。

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

3.  您現在可以使用填入的清單重新啟動擱置的作業。

## 後續處理

Windows 10 的新功能是能夠在背景傳輸完成時 (即使 app 未執行) 執行 app 程式碼。 例如，您的 app 可能會想要在影片完成下載後更新可用的影片清單，而不是每次啟動 app 時掃描新的影片。 或者，您的 app 可能會想要嘗試使用不同的伺服器或連接埠，重新處理失敗的檔案傳輸。 成功和失敗的傳輸都會叫用後續處理，因此您可以用它來實作自訂的錯誤處理和重試邏輯。

Postprocessing 會使用現有的背景工作基礎結構。 您可以建立背景工作，並將它與傳輸建立關聯之後再開始傳輸。 傳輸接著會在背景執行，並在完成時呼叫您的背景工作以執行後續處理。

後續處理會使用新的類別：[**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209)。 這個類別類似於現有的 [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030)，在此類別中，它可以讓您將背景傳輸群組在一起，但 **BackgroundTransferCompletionGroup** 會新增傳輸完成時指定待執行背景工作的能力。

起始背景傳輸與後續處理，如下所示。

1.  建立 [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209) 物件。 接著，建立 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 物件。 將建立器物件的 **Trigger** 屬性設定為完成群組物件，並將建立器的 **TaskEngtyPoint** 屬性設定為應在傳輸完成時執行之背景工作的進入點。 最後，呼叫 [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 方法來登錄背景工作。 請注意，許多完成群組可以共用一個背景工作進入點，但每個背景工作登錄只能有一個完成群組。

   ```csharp
    var completionGroup = new BackgroundTransferCompletionGroup();
    BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

    builder.Name = "MyDownloadProcessingTask";
    builder.SetTrigger(completionGroup.Trigger);
    builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

    BackgroundTaskRegistration downloadProcessingTask = builder.Register();
    ```

2.  Next you associate background transfers with the completion group. Once all transfers are created, enable the completion group.

   ```csharp
    BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
    DownloadOperation download = downloader.CreateDownload(uri, file);
    Task<DownloadOperation> startTask = download.StartAsync().AsTask();

    // App still sees the normal completion path
    startTask.ContinueWith(ForegroundCompletionHandler);

    // Do not enable the CompletionGroup until after all downloads are created.
    downloader.CompletinGroup.Enable();
    ```

3.  The code in the background task extracts the list of operations from the trigger details, and your code can then inspect the details for each operation and perform appropriate post-processing for each operation.

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

The post-processing task is a regular background task. It is part of the pool of all background tasks, and it is subject to the same resource management policy as all background tasks.

Also, note that post-processing does not replace foreground completion handlers. If your app defines a foreground completion handler, and your app is running when the file transfer completes, then both your foreground completion handler and your background completion handler will be called. The order in which foreground and background tasks are called is not guaranteed. If you define both, you should ensure that the two tasks will work properly and not interfere with each other if they are running concurrently.

## Request timeouts

There are two primary connection timeout scenarios to take into consideration:

-   When establishing a new connection for a transfer, the connection request is aborted if it is not established within five minutes.

-   After a connection has been established, an HTTP request message that has not received a response within two minutes is aborted.

> **Note**  In either scenario, assuming there is Internet connectivity, Background Transfer will retry a request up to three times automatically. In the event Internet connectivity is not detected, additional requests will wait until it is.

## Debugging guidance

Stopping a debugging session in Microsoft Visual Studio is comparable to closing your app; PUT uploads are paused and POST uploads are terminated. Even while debugging, your app should enumerate and then restart or cancel any persisted uploads. For example, you can have your app cancel enumerated persisted upload operations at app startup if there is no interest in previous operations for that debug session.

While enumerating downloads/uploads on app startup during a debug session, you can have your app cancel them if there is no interest in previous operations for that debug session. Note that if there are Visual Studio project updates, like changes to the app manifest, and the app is uninstalled and re-deployed, [**GetCurrentUploadsAsync**](https://msdn.microsoft.com/library/windows/apps/hh701149) cannot enumerate operations created using the previous app deployment.

When using Background Transfer during development, you may get into a situation where the internal caches of active and completed transfer operations can get out of sync. This may result in the inability to start new transfer operations or interact with existing operations and [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) objects. In some cases, attempting to interact with existing operations may trigger a crash. This result can occur if the [**TransferBehavior**](https://msdn.microsoft.com/library/windows/apps/dn279033) property is set to **Parallel**. This issue occurs only in certain scenarios during development and is not applicable to end users of your app.

Four scenarios using Visual Studio can cause this issue.

-   You create a new project with the same app name as an existing project, but a different language (from C++ to C#, for example).
-   You change the target architecture (from x86 to x64, for example) in an existing project.
-   You change the culture (from neutral to en-US, for example) in an existing project.
-   You add or remove a capability in the package manifest (adding **Enterprise Authentication**, for example) in an existing project.

Regular app servicing, including manifest updates which add or remove capabilities, do not trigger this issue on end user deployments of your app.
To work around this issue, completely uninstall all versions of the app and re-deploy with the new language, architecture, culture, or capability. This can be done via the **Start** screen or using PowerShell and the **Remove-AppxPackage** cmdlet.

## Exceptions in Windows.Networking.BackgroundTransfer

An exception is thrown when an invalid string for a the Uniform Resource Identifier (URI) is passed to the constructor for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) object.

**.NET:  **The [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) type appears as [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) in C# and VB.

In C# and Visual Basic, this error can be avoided by using the [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) class in the .NET 4.5 and one of the [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) methods to test the string received from the app user before the URI is constructed.

In C++, there is no method to try and parse a string to a URI. If an app gets input from the user for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), the constructor should be in a try/catch block. If an exception is thrown, the app can notify the user and request a new hostname.

The [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace has convenient helper methods and uses enumerations in the [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) namespace for handling errors. This can be useful for handling specific network exceptions differently in your app.

An error encountered on an asynchronous method in the [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace is returned as an **HRESULT** value. The [**BackgroundTransferError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701093) method is used to convert a network error from a background transfer operation to a [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) enumeration value. Most of the **WebErrorStatus** enumeration values correspond to an error returned by the native HTTP or FTP client operation. An app can filter on specific **WebErrorStatus** enumeration values to modify app behavior depending on the cause of the exception.

For parameter validation errors, an app can also use the **HRESULT** from the exception to learn more detailed information on the error that caused the exception. Possible **HRESULT** values are listed in the *Winerror.h* header file. For most parameter validation errors, the **HRESULT** returned is **E\_INVALIDARG**.



<!--HONumber=Mar16_HO1-->


