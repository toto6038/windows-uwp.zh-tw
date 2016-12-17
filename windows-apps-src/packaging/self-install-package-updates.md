---
author: mcleanbyron
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: "下載與安裝 App 的套件更新"
description: "了解如何在開發人員中心儀表板中將套件標記為強制性，以及在您的 app 中撰寫程式碼來下載並安裝套件更新。"
translationtype: Human Translation
ms.sourcegitcommit: b96d4074a8960db314313c612955900c6a05dc48
ms.openlocfilehash: 4da8ffe72435501876a1e859d10a16cf19eb11fd

---
# 下載與安裝 App 的套件更新

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

從 Windows 10 版本 1607 開始，您可以在 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間中使用 API，以程式設計方式檢查目前 app 的套件更新，以及下載並安裝更新的套件。 您也可以查詢已[在 Windows 開發人員中心儀表板上標記為強制性](#mandatory-dashboard)的套件，並在安裝強制更新之前停用 app 中的功能。

這些功能可協助您使用 App 的最新版本及相關服務，自動讓您的使用者保持最新狀態。

## API 概觀

目標為 Windows 10 版本 1607 或更新版本的 App 可以使用 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別的下列方法，以下載並安裝套件更新：

|  方法  |  說明  |
|----------|---------------|
| [GetAppAndOptionalStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync.aspx) | 呼叫這個方法以取得可用套件更新的清單。<br/><br/>**注意**
              在套件通過認證程序，以及 [GetAppAndOptionalStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync.aspx) 方法辨識出有可供 App 使用的套件更新之間，最多會有一天的延遲。 |
| [RequestDownloadStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706586.aspx) | 呼叫這個方法以下載 (但不安裝) 可用的套件更新。 此作業系統會顯示對話方塊，詢問使用者是否可以下載更新。 |
| [RequestDownloadAndInstallStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706585.aspx) | 呼叫這個方法以下載並安裝可用的套件更新。 作業系統會顯示對話方塊，詢問使用者是否可以下載並安裝更新。 如果您已經藉由呼叫 [RequestDownloadStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706586.aspx) 來下載套件更新，這個方法就會略過下載程序，而只會安裝更新。  |

<span/>

這些方法會使用 [StorePackageUpdate](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.aspx) 物件來代表可用的更新套件。 使用下列 [StorePackageUpdate](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.aspx) 屬性來取得更新套件的相關資訊。

|  屬性  |  說明  |
|----------|---------------|
| [Mandatory](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.mandatory.aspx) | 使用此屬性以判斷在開發人員中心儀表板中套件是否標記為強制性。 |
| [Package](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.package.aspx) | 使用此屬性以存取套件相關的基礎資料。 |

<span/>

## 程式碼範例

下列程式碼範例示範如何在 app 中下載並安裝套件更新。 這些範例假設：
* 程式碼會在 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 的內容中執行。
* **Page** 包含名為 ```downloadProgressBar``` 的 [ProgressBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressbar.aspx)，可提供下載作業的狀態。
* 程式碼檔案含有適用於 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間的 **using** 陳述式。
* App 是單一使用者 app，僅會在啟動 app 的使用者內容中執行。 針對[多使用者應用程式](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)，使用 [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) 方法來取得 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件，而不是 [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) 方法。

<span/>

### 下載並安裝所有的套件更新

下列程式碼範例示範如何下載並安裝所有可用的套件更新。  

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        // Alert the user that updates are available and ask for their consent
        // to start the updates.
        MessageDialog dialog = new MessageDialog(
            "Download and install updates now? This may cause the application to exit.", "Download and Install?");
        dialog.Commands.Add(new UICommand("Yes"));
        dialog.Commands.Add(new UICommand("No"));
        IUICommand command = await dialog.ShowAsync();

        if (command.Label.Equals("Yes", StringComparison.CurrentCultureIgnoreCase))
        {
            // Download and install the updates.
            IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
                context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

            // The Progress async method is called one time for each step in the download
            // and installation process for each package in this request.
            downloadOperation.Progress = async (asyncInfo, progress) =>
            {
                await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                () =>
                {
                    downloadProgressBar.Value = progress.PackageDownloadProgress;
                });
            };

            StorePackageUpdateResult result = await downloadOperation.AsTask();
        }
    }
}
```

### 處理強制性套件更新

下列程式碼範例會根據前一個範例來建置，並示範如何判斷是否已將任何更新套件[在 Windows 開發人員中心儀表板上標記為強制性](#mandatory-dashboard)。 通常，如果無法成功下載或安裝強制性套件更新，則您應該針對使用者適當地將您的 app 體驗降級。

```csharp
private StoreContext context = null;

// Downloads and installs package updates in separate steps.
public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }  
    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count != 0)
    {
        // Download the packages.
        bool downloaded = await DownloadPackageUpdatesAsync(updates);

        if (downloaded)
        {
            // Install the packages.
            await InstallPackageUpdatesAsync(updates);
        }
    }
}

// Helper method for downloading package updates.
private async Task<bool> DownloadPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    bool downloadedSuccessfully = false;

    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
        this.context.RequestDownloadStorePackageUpdatesAsync(updates);

    // The Progress async method is called one time for each step in the download process for each
    // package in this request.
    downloadOperation.Progress = async (asyncInfo, progress) =>
    {
        await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            downloadProgressBar.Value = progress.PackageDownloadProgress;
        });
    };

    StorePackageUpdateResult result = await downloadOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            downloadedSuccessfully = true;
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(
                failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory. Perform whatever actions you
                // want to take for your app: for example, notify the user and disable
                // features in your app.
                HandleMandatoryPackageError();
            }
            break;
    }

    return downloadedSuccessfully;
}

// Helper method for installing package updates.
private async Task InstallPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> installOperation =
        this.context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

    // The package updates were already downloaded separately, so this method skips the download
    // operatation and only installs the updates; no download progress notifications are provided.
    StorePackageUpdateResult result = await installOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory, so tell the user.
                HandleMandatoryPackageError();
            }
            break;
    }
}

// Helper method for handling the scenario where a mandatory package update fails to
// download or install. Add code to this method to perform whatever actions you want
// to take, such as notifying the user and disabling features in your app.
private void HandleMandatoryPackageError()
{
}
```

<span id="mandatory-dashboard" />
## 在開發人員中心儀表板上使套件提交成為強制性

當您針對目標為 Windows 10 版本 1607 或更新版本的 app 建立套件提交時，可以將套件標記為強制性，並標記其成為強制性的日期/時間。 當設定此屬性，且 App 使用本文先前所述 API 發現有套件更新可供使用時，App 就可以判斷更新套件是否為強制，並變更其行為，直到安裝更新為止 (例如，App 可以停用功能)。

>**注意**
              套件更新的強制狀態並非由 Microsoft 來實施，作業系統不會提供 UI 來指示使用者必須安裝強制性應用程式更新。 開發人員必須刻意使用強制性設定，以便在程式碼中實施強制性應用程式更新。  

將套件提交標記為強制性：

1. 登入[開發人員中心儀表板](https://dev.windows.com/overview)並瀏覽到 app 的概觀頁面。
2. 按一下提交的名稱，其中包含您想要變成強制性的套件更新。
3. 瀏覽到提交的 \[套件\] 頁面。 在此頁面底部附近選取 \[使此更新變成強制性\]，然後選擇套件更新變成強制性的日期和時間。 這個選項適用於提交中的所有 UWP 套件。

如需在開發人員中心儀表板上設定套件的詳細資訊，請參閱[上傳應用程式套件](https://msdn.microsoft.com/windows/uwp/publish/upload-app-packages)。

  >**注意**
              如果您建立[套件正式發行前小眾測試版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)，就可以使用正式發行前小眾測試版 \[套件\] 頁面上類似的 UI，將套件標記為強制性。 在此情況下，強制性套件更新只適用於正式發行前小眾測試版群組的客戶。



<!--HONumber=Nov16_HO1-->


