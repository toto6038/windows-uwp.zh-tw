---
description: UI 的背後是商務與資料層。
title: 移植到 UWP 的 Windows Phone Silverlight 商務和資料層
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 25e7fdcb4195dcc0dffed7657d41bd02bea8a5c2
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322306"
---
#  <a name="porting-windowsphone-silverlight-business-and-data-layers-to-uwp"></a>移植到 UWP 的 Windows Phone Silverlight 商務和資料層


前一個主題是 [I/O、裝置與 app 模型的移植](wpsl-to-uwp-input-and-sensors.md)。

UI 的背後是商務與資料層。 這些層中的程式碼會呼叫作業系統和 .NET Framework API (例如背景處理、位置、相機、檔案系統、網路及其他資料存取)。 大多數這些皆[可供通用 Windows 平台 (UWP) 應用程式使用](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10))，因此您應該能夠一成不變地移植此程式碼中的大部分。

## <a name="asynchronous-methods"></a>非同步方法

通用 Windows 平台 (UWP) 的其中一個優先考量，是讓您建置能真正一致地回應的應用程式。 動畫永遠流暢，並且觸控互動 (例如移動瀏覽和撥動) 即時而無延遲，感覺起來 UI 就像是黏附著手指一樣。 為了達到這個目的，任何無法保證在 50 毫秒內完成的 UWP API 皆已被設為非同步，並且其名稱後面會加上 **Async**。 您的 UI 執行緒將會從呼叫 **Async** 方法立即返回，而工作則會在另一個執行緒上執行。 在語法上，藉由使用 C# **await** 運算子、JavaScript Promise 物件及 C++ 接續，使得取用 **Async** 方法變得相當簡單 (如需詳細資訊，請參閱[非同步程式設計](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps))。

## <a name="background-processing"></a>背景處理

Windows Phone Silverlight 應用程式可以使用 managed **ScheduledTaskAgent**來執行工作，而應用程式不在前景中的物件。 UWP app 會使用 [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 類別，以類似的方式來建立並登錄背景工作。 您需定義會實作您背景工作之工作的類別。 系統會定期執行您的背景工作，藉由呼叫您類別的 [**Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 方法來執行工作。 在 UWP app 中，請記得在 app 套件資訊清單中設定 [**背景工作**] 宣告。 如需詳細資訊，請參閱[使用背景工作支援 app](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks)。

若要傳送大型資料檔案，在背景中的，Windows Phone Silverlight 應用程式會使用**BackgroundTransferService**類別。 UWP app 會在 [**Windows.Networking.BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) 命名空間中使用 API 來執行這個動作。 這些功能使用類似的模式來起始傳輸，但新 API 已經改進性能和效能。 如需詳細資訊，請參閱[在背景傳輸資料](https://docs.microsoft.com/previous-versions/windows/apps/hh452975(v=win.10))。

Windows Phone Silverlight 應用程式使用中的 managed 的類別**Microsoft.Phone.BackgroundAudio**播放音訊時應用程式的命名空間不在前景中。 UWP 使用 Windows Phone 市集 app 模型，請參閱[背景音訊](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)與[背景音訊](https://go.microsoft.com/fwlink/p/?linkid=619997)範例。

## <a name="cloud-services-networking-and-databases"></a>雲端服務、網路功能及資料庫

使用 Azure 在雲端裝載資料與應用程式服務是可行的。 請參閱[開始使用行動服務](https://go.microsoft.com/fwlink/p/?LinkID=403138)。 解決方案需要線上和離線的資料，請參閱：[在行動服務中使用離線資料同步](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/)。

UWP 部分支援 **System.Net.HttpWebRequest** 類別，但是不支援 **System.Net.WebClient** 類別。 建議的預見替代方案是 [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 類別 (或者是 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))，如果您需要能夠移植到其他支援 .NET 之平台的程式碼)。 這些 API 使用 [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) 來代表 HTTP 要求。

UWP app 目前沒有內建處理大量資料案例 (如企業營運系統 (LOB) 案例) 的支援。 不過，您可以使用 SQLite 來執行本機異動資料庫服務。 如需詳細資訊，請參閱 [SQLite](https://marketplace.visualstudio.com/items?itemName=SQLiteDevelopmentTeam.SQLiteforUniversalWindowsPlatform)。

將絕對 URI (而非相對 URI) 傳遞至 Windows 執行階段類型。 請參閱[將 URI 傳送到 Windows 執行階段](https://docs.microsoft.com/dotnet/standard/cross-platform/passing-a-uri-to-the-windows-runtime)。

## <a name="launchers-and-choosers"></a>啟動程式與選擇器

啟動器與選擇器 (位於**Microsoft.Phone.Tasks**命名空間)、 Windows Phone Silverlight 應用程式可以與互動來執行一般作業，例如撰寫電子郵件中，選擇一張照片，作業系統或另一個應用程式共用特定種類的資料。 搜尋**Microsoft.Phone.Tasks**主題中的 <<c4> [ 命名空間和類別對應的 Windows 10 的 Windows Phone Silverlight](wpsl-to-uwp-namespace-and-class-mappings.md)尋找對等的 UWP 型別。 這些範圍包括從類似的機制 (稱為啟動程式和選擇器)，到實作在 app 之間共用資料的協定。

Windows Phone Silverlight 應用程式可以成為進入休眠狀態或甚至是加上標記時使用，例如相片選擇器工作。 使用 [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 類別的同時，UWP app 仍會保持使用中並且執行。

## <a name="monetization-trial-mode-and-in-app-purchases"></a>賺錢 (試用模式和在 app 內購買)

Windows Phone Silverlight 應用程式可以使用 UWP [**CurrentApp** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp)類別大部分其試用模式和應用程式內購買功能，使程式碼不需要移植。 但是，Windows Phone Silverlight 應用程式會呼叫**MarketplaceDetailTask.Show**提供採購的應用程式：

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

移植該程式碼以呼叫 UWP [**RequestAppPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) 方法：

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

如果您有模擬 app 購買和在 app 內購買功能的程式碼以供測試，則您可以移植該程式碼來改用 [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 類別。

## <a name="notifications-for-tile-or-toast-updates"></a>磚或快顯通知更新的通知

通知是 Windows Phone Silverlight 應用程式的推播通知模型的擴充功能。 當您從「Windows 推播通知服務」(WNS) 收到通知時，您可以藉由磚更新或快顯通知將資訊顯示到 UI 上。 如需移植您通知功能的 UI 端，請參閱[磚和快顯通知](w8x-to-uwp-porting-xaml-and-ui.md)。

如需有關在 UWP app 中使用通知的詳細資料，請參閱[傳送快顯通知](https://docs.microsoft.com/previous-versions/windows/apps/hh868266(v=win.10))。

如需有關在使用 C++、C# 或 Visual Basic 的 Windows 執行階段應用程式中使用磚、快顯通知、徽章、橫幅以及通知的資訊和教學課程，請參閱[使用磚、徽章以及快顯通知](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10))。

## <a name="storage-file-access"></a>儲存區 (檔案存取)

將應用程式設定儲存為索引鍵 / 值組，隔離儲存區中的 Windows Phone Silverlight 程式碼輕鬆地移植。 以下是一個之前和之後的範例，第一次的 Windows Phone Silverlight 版本：

```csharp
    var propertySet = IsolatedStorageSettings.ApplicationSettings;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    propertySet.Save();
    string myFavoriteAuthor = propertySet.Contains(key) ? (string)propertySet[key] : "<none>";
```

和 UWP 對等物件：

```csharp
    var propertySet = Windows.Storage.ApplicationData.Current.LocalSettings.Values;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    string myFavoriteAuthor = propertySet.ContainsKey(key) ? (string)propertySet[key] : "<none>";
```

雖然子集**Windows.Storage**命名空間是提供給他們，許多 Windows Phone Silverlight 應用程式執行檔案 i/o， **IsolatedStorageFile**類別，因為它所支援的較長。 假設**IsolatedStorageFile**是正在使用中，以下是寫入和讀取檔案，第一次的 Windows Phone Silverlight 版本之前和之後的範例：

```csharp
    const string filename = "FavoriteAuthor.txt";
    using (var store = IsolatedStorageFile.GetUserStoreForApplication())
    {
        using (var streamWriter = new StreamWriter(store.CreateFile(filename)))
        {
            streamWriter.Write("Charles Dickens");
        }
        using (var StreamReader = new StreamReader(store.OpenFile(filename, FileMode.Open, FileAccess.Read)))
        {
            string myFavoriteAuthor = StreamReader.ReadToEnd();
        }
    }
```

和使用 UWP 的相同功能：

```csharp
    const string filename = "FavoriteAuthor.txt";
    var store = Windows.Storage.ApplicationData.Current.LocalFolder;
    Windows.Storage.StorageFile file = await store.CreateFileAsync(filename, Windows.Storage.CreationCollisionOption.ReplaceExisting);
    await Windows.Storage.FileIO.WriteTextAsync(file, "Charles Dickens");
    file = await store.GetFileAsync(filename);
    string myFavoriteAuthor = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Windows Phone Silverlight 應用程式可以選擇性的 sd 記憶卡的唯讀存取。 UWP app 擁有 SD 記憶卡的讀寫存取權限。 如需詳細資訊，請參閱[存取 SD 記憶卡](https://docs.microsoft.com/windows/uwp/files/access-the-sd-card)。

如需有關在 UWP 應用程式中存取相片、音樂與影片檔案的資訊，請參閱[音樂、圖片及影片媒體櫃中的檔案和資料夾](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries)。

如需詳細資訊，請參閱[檔案、資料夾和媒體櫃](https://docs.microsoft.com/windows/uwp/files/index)。

下一個主題是[尺寸與 UX 的移植](wpsl-to-uwp-form-factors-and-ux.md)。

## <a name="related-topics"></a>相關主題

* [命名空間和類別的對應](wpsl-to-uwp-namespace-and-class-mappings.md)
 

