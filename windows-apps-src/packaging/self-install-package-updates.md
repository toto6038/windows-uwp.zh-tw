---
author: mcleanbyron
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: "下載與安裝 App 的套件更新"
description: "了解如何在開發人員中心儀表板中將套件標記為強制性，以及在您的 app 中撰寫程式碼來下載並安裝套件更新。"
ms.author: mcleans
ms.date: 03/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 07b8b769cbcaf86bfa70a562de568cab65c91a77
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="download-and-install-package-updates-for-your-app"></a>下載與安裝應用程式的套件更新

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

從 Windows 10 版本 1607 開始，您可以在 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空間中使用 API，以程式設計方式檢查目前 app 的套件更新，以及下載並安裝更新的套件。 您也可以查詢已[在 Windows 開發人員中心儀表板上標記為強制性](#mandatory-dashboard)的套件，並在安裝強制更新之前停用 app 中的功能。

這些功能可協助您使用 App 的最新版本及相關服務，自動讓您的使用者保持最新狀態。

## <a name="api-overview"></a>API 概觀

目標為 Windows 10 版本 1607 或更新版本的 App 可以使用 [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 類別的下列方法，以下載並安裝套件更新：

|  方法  |  說明  |
|----------|---------------|
| [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetAppAndOptionalStorePackageUpdatesAsync) | 呼叫這個方法以取得可用套件更新的清單。 請注意，在套件通過認證程序之時到 **GetAppAndOptionalStorePackageUpdatesAsync** 方法辨識出有可供 App 使用的套件更新之時，中間會有最多一天的延遲。 |
| [RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_RequestDownloadStorePackageUpdatesAsync_Windows_Foundation_Collections_IIterable_Windows_Services_Store_StorePackageUpdate__) | 呼叫這個方法以下載 (但不安裝) 可用的套件更新。 此作業系統會顯示對話方塊，詢問使用者是否可以下載更新。 |
| [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_RequestDownloadAndInstallStorePackageUpdatesAsync_Windows_Foundation_Collections_IIterable_Windows_Services_Store_StorePackageUpdate__) | 呼叫這個方法以下載並安裝可用的套件更新。 作業系統會顯示對話方塊，詢問使用者是否可以下載並安裝更新。 如果您已經藉由呼叫 **RequestDownloadStorePackageUpdatesAsync** 來下載套件更新，這個方法就會略過下載程序，而只會安裝更新。  |

<span/>

這些方法會使用 [StorePackageUpdate](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdate) 物件來代表可用的更新套件。 使用下列 **StorePackageUpdate** 屬性來取得更新套件的相關資訊。

|  屬性  |  說明  |
|----------|---------------|
| [Mandatory](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdate#Windows_Services_Store_StorePackageUpdate_Mandatory) | 使用此屬性以判斷在開發人員中心儀表板中套件是否標記為強制性。 |
| [Package](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdate#Windows_Services_Store_StorePackageUpdate_Package) | 使用此屬性以存取套件相關的基礎資料。 |

<span/>

## <a name="code-examples"></a>程式碼範例

下列程式碼範例示範如何在 app 中下載並安裝套件更新。 這些範例假設：
* 程式碼會在 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 的內容中執行。
* **Page** 包含名為 ```downloadProgressBar``` 的 [ProgressBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressbar.aspx)，可提供下載作業的狀態。
* 程式碼檔案含有使用 **Windows.Services.Store**、**Windows.Threading.Tasks** 及 **Windows.UI.Popups** 命名空間的 **using** 陳述式。
* App 是單一使用者 App，僅會在啟動 App 的使用者內容中執行。 針對[多使用者應用程式](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)，使用 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetForUser_Windows_System_User_) 方法來取得 **StoreContext** 物件，而不是 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetDefault) 方法。

<span/>

### <a name="download-and-install-all-package-updates"></a>下載並安裝所有的套件更新

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

### <a name="handle-mandatory-package-updates"></a>處理強制性套件更新

下列程式碼範例會根據前一個範例來建置，並示範如何判斷是否已將任何更新套件[在 Windows 開發人員中心儀表板上標記為強制性](#mandatory-dashboard)。 通常，如果無法成功下載或安裝強制性套件更新，則您應該針對使用者適當地將您的 App 體驗降級。

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

### <a name="display-progress-info-for-the-download-and-install"></a>顯示下載與安裝的進度資訊

呼叫 **RequestDownloadStorePackageUpdatesAsync** 或 **RequestDownloadAndInstallStorePackageUpdatesAsync** 時，您可以在此要求中，為每個套件的下載 (或下載與安裝) 程序指派其中每個步驟各呼叫一次的 [Progress](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) 處理常式。 處理常式會收到 [StorePackageUpdateStatus](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdatestatus) 物件，這個物件提供關於引發進度通知之更新套件的資訊。 先前的範例會使用 **StorePackageUpdateStatus** 物件的 **PackageDownloadProgress** 欄位來顯示下載與安裝程序的進度。

請注意，當您在單一作業中呼叫 **RequestDownloadAndInstallStorePackageUpdatesAsync** 下載並安裝套件更新時，**PackageDownloadProgress** 欄位會在下載套件的過程中從 0.0 增加至 0.8，然後再於安裝期間從 0.8 增加至 1.0。 因此，如果直接將自訂進度 UI 顯示的用百分比對應至 **PackageDownloadProgress** 欄位的值，您的 UI 將會在完成套件下載且作業系統顯示安裝對話方塊時顯示 80%。 如果您希望自訂進度 UI 在套件已下載且準備好要安裝時顯示 100%，您可以將程式碼修改為，在 **PackageDownloadProgress** 欄位到達 0.8 時指派 100% 給自訂進度 UI。

<span id="mandatory-dashboard" />
## <a name="make-a-package-submission-mandatory-in-the-dev-center-dashboard"></a>在開發人員中心儀表板上將套件提交設定為強制性

當您針對目標為 Windows 10 版本 1607 或更新版本的 app 建立套件提交時，可以將套件標記為強制性，並標記其成為強制性的日期/時間。 當設定此屬性，且 App 使用本文先前所述 API 發現有套件更新可供使用時，App 就可以判斷更新套件是否為強制，並變更其行為，直到安裝更新為止 (例如，App 可以停用功能)。

> [!NOTE]
> Microsoft 並未強迫建立套件更新的強制狀態，作業系統也不會提供 UI 來指示使用者必須安裝強制性應用程式更新。 開發人員必須刻意使用強制性設定，以便在程式碼中實施強制性應用程式更新。  

將套件提交標記為強制性：

1. 登入[開發人員中心儀表板](https://dev.windows.com/overview)並瀏覽到 app 的概觀頁面。
2. 按一下提交的名稱，其中包含您想要變成強制性的套件更新。
3. 瀏覽到提交的 **\[套件\]** 頁面。 在此頁面底部附近選取 **\[使此更新變成強制性\]**，然後選擇套件更新變成強制性的日期和時間。 這個選項適用於提交中的所有 UWP 套件。

如需在開發人員中心儀表板上設定套件的詳細資訊，請參閱[上傳應用程式套件](../publish/upload-app-packages.md)。

  > [!NOTE]
  > 如果您建立[套件正式發行前小眾測試版](../publish/package-flights.md)，就可以使用正式發行前小眾測試版 **\[套件\]** 頁面上的類似 UI，將套件標記為強制性。 在此情況下，強制性套件更新只適用於正式發行前小眾測試版群組的客戶。
