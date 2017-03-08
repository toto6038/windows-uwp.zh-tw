---
author: mcleblanc
description: "UI 的背後是商務與資料層。"
title: "移植 Windows Phone Silverlight 商務與資料層至 UWP"
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 40b3d2b5b4bafac25726e27fd19a2cd5d71c4fa3
ms.lasthandoff: 02/07/2017

---

#  <a name="porting-windows-phone-silverlight-business-and-data-layers-to-uwp"></a>移植 Windows Phone Silverlight 商務與資料層至 UWP

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

前一個主題是 [I/O、裝置與 app 模型的移植](wpsl-to-uwp-input-and-sensors.md)。

UI 的背後是商務與資料層。 這些層中的程式碼會呼叫作業系統和 .NET Framework API (例如背景處理、位置、相機、檔案系統、網路及其他資料存取)。 大多數這些皆[可供通用 Windows 平台 (UWP) 應用程式使用](https://msdn.microsoft.com/library/windows/apps/br211369)，因此您應該能夠一成不變地移植此程式碼中的大部分。

## <a name="asynchronous-methods"></a>非同步方法

通用 Windows 平台 (UWP) 的其中一個優先考量，是讓您建置能真正一致地回應的應用程式。 動畫永遠流暢，並且觸控互動 (例如移動瀏覽和撥動) 即時而無延遲，感覺起來 UI 就像是黏附著手指一樣。 為了達到這個目的，任何無法保證在 50 毫秒內完成的 UWP API 皆已被設為非同步，並且其名稱後面會加上 **Async**。 您的 UI 執行緒將會從呼叫 **Async** 方法立即返回，而工作則會在另一個執行緒上執行。 在語法上，藉由使用 C# **await** 運算子、JavaScript Promise 物件及 C++ 接續，使得取用 **Async** 方法變得相當簡單 (如需詳細資訊，請參閱[非同步程式設計](https://msdn.microsoft.com/library/windows/apps/mt187335))。

## <a name="background-processing"></a>背景處理

Windows Phone Silverlight app 可以使用受管理的 **ScheduledTaskAgent** 物件，以在 app 工作不在前景時執行工作。 UWP app 會使用 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 類別，以類似的方式來建立並登錄背景工作。 您需定義會實作您背景工作之工作的類別。 系統會定期執行您的背景工作，藉由呼叫您類別的 [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 方法來執行工作。 在 UWP app 中，請記得在 app 套件資訊清單中設定 [**背景工作**] 宣告。 如需詳細資訊，請參閱[使用背景工作支援 app](https://msdn.microsoft.com/library/windows/apps/mt299103)。

若要在背景傳輸大量資料檔案，Windows Phone Silverlight app 會使用 **BackgroundTransferService** 類別。 UWP app 會在 [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) 命名空間中使用 API 來執行這個動作。 這些功能使用類似的模式來起始傳輸，但新 API 已經改進性能和效能。 如需詳細資訊，請參閱[在背景傳輸資料](https://msdn.microsoft.com/library/windows/apps/xaml/hh452975)。

Windows Phone Silverlight app 會使用 **Microsoft.Phone.BackgroundAudio** 命名空間中的受管理類別，以在 app 不在前景時播放音訊。 UWP 使用 Windows Phone 市集 app 模型，請參閱[背景音訊](https://msdn.microsoft.com/library/windows/apps/mt282140)與[背景音訊](http://go.microsoft.com/fwlink/p/?linkid=619997)範例。

## <a name="cloud-services-networking-and-databases"></a>雲端服務、網路功能及資料庫

使用 Azure 在雲端裝載資料與應用程式服務是可行的。 請參閱[開始使用行動服務](http://go.microsoft.com/fwlink/p/?LinkID=403138)。 針對需要線上和離線資料的解決方案，請參閱：[使用行動服務中的離線資料同步](http://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/)。

UWP 部分支援 **System.Net.HttpWebRequest** 類別，但是不支援 **System.Net.WebClient** 類別。 建議的預見替代方案是 [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 類別 (或者是 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)，如果您需要能夠移植到其他支援 .NET 之平台的程式碼)。 這些 API 使用 [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) 來代表 HTTP 要求。

UWP app 目前沒有內建處理大量資料案例 (如企業營運系統 (LOB) 案例) 的支援。 不過，您可以使用 SQLite 來執行本機異動資料庫服務。 如需詳細資訊，請參閱 [SQLite](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936)。

將絕對 URI (而非相對 URI) 傳遞至 Windows 執行階段類型。 請參閱[將 URI 傳送到 Windows 執行階段](https://msdn.microsoft.com/library/hh763341.aspx)。

## <a name="launchers-and-choosers"></a>啟動程式與選擇器

使用「啟動程式」和「選擇器」(可在 **Microsoft.Phone.Tasks** 命名空間中找到)，Windows Phone Silverlight app 便可以與作業系統互動以執行常見的作業，例如撰寫電子郵件、選擇相片，或是與另一個 app 中共用某些類型的資料。 在 [Windows Phone Silverlight 到 Windows 10 命名空間與類別對應](wpsl-to-uwp-namespace-and-class-mappings.md)主題中搜尋 **Microsoft.Phone.Tasks**，以尋找對等的 UWP 類型。 這些範圍包括從類似的機制 (稱為啟動程式和選擇器)，到實作在 app 之間共用資料的協定。

Windows Phone Silverlight 應用程式可被置於休眠狀態，或甚至是被標記起來 (例如使用相片「選擇器」工作時)。 使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 類別的同時，UWP app 仍會保持使用中並且執行。

## <a name="monetization-trial-mode-and-in-app-purchases"></a>賺錢 (試用模式和在在應用程式內購買)

Windows Phone Silverlight app 可以將 UWP [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 類別用於其大部分的試用模式與在應用程式內購買功能，因此不需要移植該程式碼。 但是 Windows Phone Silverlight app 會呼叫 **MarketplaceDetailTask.Show** 來提供要供購買的 app：

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

移植該程式碼以呼叫 UWP [**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813) 方法：

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

如果您有模擬 app 購買和在在應用程式內購買功能的程式碼以供測試，則您可以移植該程式碼來改用 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 類別。

## <a name="notifications-for-tile-or-toast-updates"></a>磚或快顯通知更新的通知

通知是 Windows Phone Silverlight 應用程式的推播通知模型延伸。 當您從「Windows 推播通知服務」(WNS) 收到通知時，您可以藉由磚更新或快顯通知將資訊顯示到 UI 上。 如需移植您通知功能的 UI 端，請參閱[磚和快顯通知](w8x-to-uwp-porting-xaml-and-ui.md)。

如需有關在 UWP app 中使用通知的詳細資料，請參閱[傳送快顯通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868266)。

如需有關在使用 C++、C# 或 Visual Basic 的 Windows 執行階段應用程式中使用磚、快顯通知、徽章、橫幅以及通知的資訊和教學課程，請參閱[使用磚、徽章以及快顯通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259)。

## <a name="storage-file-access"></a>儲存區 (檔案存取)

您可以輕鬆移植將應用程式設定以機碼值組方式儲存在隔離儲存區中的 Windows Phone Silverlight 程式碼。 以下是移植前和移植後的對照範例，首先是 Windows Phone Silverlight 版本：

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

雖然有 **Windows.Storage** 命名空間子集可供其使用，但是許多 Windows Phone Silverlight app 仍然會使用 **IsolatedStorageFile** 類別來執行檔案 I/O，因為其受支援的時間較久。 假設目前使用 **IsolatedStorageFile**，以下是寫入和讀取檔案的前後對照範例，首先是 Windows Phone Silverlight 版本：

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

Windows Phone Silverlight 應用程式對選用的 SD 記憶卡具有唯讀存取權。 UWP app 擁有 SD 記憶卡的讀寫存取權限。 如需詳細資訊，請參閱[存取 SD 記憶卡](https://msdn.microsoft.com/library/windows/apps/mt188699)。

如需有關在 UWP 應用程式中存取相片、音樂與影片檔案的資訊，請參閱[音樂、圖片及影片媒體櫃中的檔案和資料夾](https://msdn.microsoft.com/library/windows/apps/mt188703)。

如需詳細資訊，請參閱[檔案、資料夾和媒體櫃](https://msdn.microsoft.com/library/windows/apps/mt185399)。

下一個主題是[尺寸與 UX 的移植](wpsl-to-uwp-form-factors-and-ux.md)。

## <a name="related-topics"></a>相關主題

* [命名空間與類別對應](wpsl-to-uwp-namespace-and-class-mappings.md)
 


