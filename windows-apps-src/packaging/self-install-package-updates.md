---
author: mcleanbyron
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: 從 Microsoft Store 下載與安裝套件更新
description: 了解如何在開發人員中心儀表板中將套件標記為強制性，以及在您的 app 中撰寫程式碼來下載並安裝套件更新。
ms.author: mcleans
ms.date: 04/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 39b3ba05b06b6d484804999a935accc7223b5c60
ms.sourcegitcommit: f5cf806a595969ecbb018c3f7eea86c7a34940f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2018
ms.locfileid: "3825738"
---
# <a name="download-and-install-package-updates-from-the-store"></a>從 Microsoft Store 下載與安裝套件更新

從 Windows 10 版本 1607 開始，您可以使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空間中 [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 類別的方法，以程式設計方式檢查 Microsoft Store 中目前 app 的套件更新，以及下載並安裝更新的套件。 您也可以查詢已在 Windows 開發人員中心儀表板上標記為強制性的套件，並在安裝強制更新之前停用 app 中的功能。

在 Windows 10 版本 1803 中導入的其他[StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)方法可讓您下載及以無訊息方式安裝套件更新（不向使用者顯示 UI 通知）、解除安裝[選用套件](optional-packages.md)，以及取得 app 下載和安裝佇列中套件的資訊。

這些功能可協助您使用 Microsoft Store 中的 App 最新版本、選用套件及相關服務，自動讓您的使用者保持最新狀態。

## <a name="download-and-install-package-updates-with-the-users-permission"></a>經使用者同意，下載並安裝套件更新

此程式碼範例示範如何使用[GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync)方法來探索 Microsoft Store 中的所有可用套件更新，然後呼叫[RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync)方法下載並安裝更新。 使用此方法來下載並安裝更新，作業系統會在下載更新之前顯示對話方塊，徵求使用者同意。

> [!NOTE]
> 這些方法支援應用程式的所需和[選用套件](optional-packages.md)。 選用套件相當適合用於附加可下載內容 (DLC)、根據大小限制將大型應用程式進行分割，或是傳送與您核心應用程式分離的額外內容。 請參閱 [Windows 開發人員支援](https://developer.microsoft.com/windows/support)，以取得將使用選用套件 (包括 DLC 附加內容) 的應用程式提交至 Microsoft Store 的權限。

此程式碼範例假設：

* 程式碼會在 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 的內容中執行。
* **Page** 包含名為 ```downloadProgressBar``` 的 [ProgressBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressbar.aspx)，可提供下載作業的狀態。
* 程式碼檔案含有使用 **Windows.Services.Store**、**Windows.Threading.Tasks** 及 **Windows.UI.Popups** 命名空間的 **using** 陳述式。
* App 是單一使用者 App，僅會在啟動 App 的使用者內容中執行。 針對[多使用者應用程式](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)，使用 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) 方法來取得 **StoreContext** 物件，而不是 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) 方法。

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

> [!NOTE]
> 若只要下載（而不安裝）可用的套件更新，請使用[RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync)方法。

### <a name="display-download-and-install-progress-info"></a>顯示下載與安裝進度資訊

呼叫 [RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync) 或 [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync) 方法時，您可以在此要求中，為每個套件的下載 (或下載與安裝) 程序指派其中每個步驟各呼叫一次的 [Progress](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncoperationwithprogress-2.progress) 處理常式。 處理常式會收到 [StorePackageUpdateStatus](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdatestatus) 物件，這個物件提供關於引發進度通知之更新套件的資訊。 先前的範例使用 **StorePackageUpdateStatus** 物件的 **PackageDownloadProgress** 欄位來顯示下載與安裝程序的進度。

請注意，當您在單一作業中呼叫 **RequestDownloadAndInstallStorePackageUpdatesAsync** 下載並安裝套件更新時，**PackageDownloadProgress** 欄位會在下載套件的過程中從 0.0 增加至 0.8，然後再於安裝期間從 0.8 增加至 1.0。 因此，如果直接將自訂進度 UI 顯示的用百分比對應至 **PackageDownloadProgress** 欄位的值，您的 UI 將會在完成套件下載且作業系統顯示安裝對話方塊時顯示 80%。 如果您希望自訂進度 UI 在套件已下載且準備好要安裝時顯示 100%，您可以將程式碼修改為，在 **PackageDownloadProgress** 欄位到達 0.8 時指派 100% 給自訂進度 UI。

## <a name="download-and-install-package-updates-silently"></a>以無訊息方式下載及安裝套件更新

從 Windows 10 版本 1803 起，您可以使用[TrySilentDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync)和[TrySilentDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync)方法以無訊息方式下載及安裝套件更新（不向使用者顯示 UI 通知）。 只有在使用者已在 Microsoft Store 中啟用**\[自動更新 App\]** 設定，而且使用者不是在計量付費網路上，這項作業才會成功。 呼叫這些方法之前，您可以先查看[CanSilentlyDownloadStorePackageUpdates](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.cansilentlydownloadstorepackageupdates)屬性，判斷目前是否符合這些條件。

此程式碼範例示範如何使用[GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync)方法來探索所有可用的套件更新，然後呼叫[TrySilentDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) 和 [TrySilentDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) 方法以無訊息方式下載並安裝更新。

此程式碼範例假設：
* 程式碼檔案含有 **Windows.Services.Store** 和 **System.Threading.Tasks** 命名空間的 **using** 陳述式。
* App 是單一使用者 App，僅會在啟動 App 的使用者內容中執行。 針對[多使用者應用程式](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)，使用 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) 方法來取得 **StoreContext** 物件，而不是 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) 方法。

> [!NOTE]
> 此範例程式碼所呼叫的**IsNowAGoodTimeToRestartApp**、**RetryDownloadAndInstallLater**和**RetryInstallLater**方法是預留位置方法，用來依據您自己的應用程式設計視需要實作。

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesInBackgroundAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> storePackageUpdates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (storePackageUpdates.Count > 0)
    {

        if (!context.CanSilentlyDownloadStorePackageUpdates)
        {
            return;
        }

        // Start the silent downloads and wait for the downloads to complete.
        StorePackageUpdateResult downloadResult =
            await context.TrySilentDownloadStorePackageUpdatesAsync(storePackageUpdates);

        switch (downloadResult.OverallState)
        {
            case StorePackageUpdateState.Completed:
                // The download has completed successfully. At this point, confirm whether your app
                // can restart now and then install the updates (for example, you might only install
                // packages silently if your app has been idle for a certain period of time). The
                // IsNowAGoodTimeToRestartApp method is not implemented in this example, you should
                // implement it as needed for your own app.
                if (IsNowAGoodTimeToRestartApp())
                {
                    await InstallUpdate(storePackageUpdates);
                }
                else
                {
                    // Retry/reschedule the installation later. The RetryInstallLater method is not  
                    // implemented in this example, you should implement it as needed for your own app.
                    RetryInstallLater();
                    return;
                }
                break;
            // If the user cancelled the download or you can't perform the download for some other
            // reason (for example, Wi-Fi might have been turned off and the device is now on
            // a metered network) try again later. The RetryDownloadAndInstallLater method is not  
            // implemented in this example, you should implement it as needed for your own app.
            case StorePackageUpdateState.Canceled:
            case StorePackageUpdateState.ErrorLowBattery:
            case StorePackageUpdateState.ErrorWiFiRecommended:
            case StorePackageUpdateState.ErrorWiFiRequired:
            case StorePackageUpdateState.OtherError:
                RetryDownloadAndInstallLater();
                return;
            default:
                break;
        }
    }
}

private async Task InstallUpdate(IReadOnlyList<StorePackageUpdate> storePackageUpdates)
{
    // Start the silent installation of the packages. Because the packages have already
    // been downloaded in the previous method, the following line of code just installs
    // the downloaded packages.
    StorePackageUpdateResult downloadResult =
        await context.TrySilentDownloadAndInstallStorePackageUpdatesAsync(storePackageUpdates);

    switch (downloadResult.OverallState)
    {
        // If the user cancelled the installation or you can't perform the installation  
        // for some other reason, try again later. The RetryInstallLater method is not  
        // implemented in this example, you should implement it as needed for your own app.
        case StorePackageUpdateState.Canceled:
        case StorePackageUpdateState.ErrorLowBattery:
        case StorePackageUpdateState.OtherError:
            RetryInstallLater();
            return;
        default:
            break;
    }
}
```

## <a name="mandatory-package-updates"></a>強制性套件更新

當您針對目標為 Windows 10 版本 1607 或更新版本的 app 建立套件提交時，可以將[套件標記為強制性](../publish/upload-app-packages.md#mandatory-update)，並標記其成為強制性的日期/時間。 當設定此屬性，且 App 發現有套件更新可供使用時，App 就可以判斷更新套件是否為強制，並變更其行為，直到安裝更新為止 (例如，App 可以停用功能)。

> [!NOTE]
> Microsoft 並未強迫建立套件更新的強制狀態，作業系統也不會提供 UI 來指示使用者必須安裝強制性應用程式更新。 開發人員必須刻意使用強制性設定，以便在程式碼中實施強制性應用程式更新。  

將套件提交標記為強制性：

1. 登入[開發人員中心儀表板](https://dev.windows.com/overview)並瀏覽到 app 的概觀頁面。
2. 按一下提交的名稱，其中包含您想要變成強制性的套件更新。
3. 瀏覽到提交的 **\[套件\]** 頁面。 在此頁面底部附近選取 **\[使此更新變成強制性\]**，然後選擇套件更新變成強制性的日期和時間。 這個選項適用於提交中的所有 UWP 套件。

如需詳細資訊，請參閱[上傳應用程式套件](../publish/upload-app-packages.md)。

> [!NOTE]
> 如果您建立[套件正式發行前小眾測試版](../publish/package-flights.md)，就可以使用正式發行前小眾測試版 **\[套件\]** 頁面上的類似 UI，將套件標記為強制性。 在此情況下，強制性套件更新只適用於正式發行前小眾測試版群組的客戶。

### <a name="code-example-for-mandatory-packages"></a>強制性套件的程式碼範例

下列程式碼範例示範如何判斷任何更新套件是否為強制性。 通常，如果無法成功下載或安裝強制性套件更新，則您應該針對使用者適當地將您的 App 體驗降級。

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

## <a name="uninstall-optional-packages"></a>解除安裝選用套件

從 Windows 10 版本 1803 起，您可以使用[RequestUninstallStorePackageAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync)或[RequestUninstallStorePackageByStoreIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackagebystoreidasync)方法解除安裝目前應用程式的[選用套件](optional-packages.md)（包括 DLC 套件）。 例如，如果您有透過選用套件安裝內容的應用程式，可能會想要提供 UI，讓使用者解除安裝選用套件以釋放磁碟空間。

下列程式碼範例示範如何呼叫 [RequestUninstallStorePackageAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync)。 此範例假設：
* 程式碼檔案含有 **Windows.Services.Store** 和 **System.Threading.Tasks** 命名空間的 **using** 陳述式。
* App 是單一使用者 App，僅會在啟動 App 的使用者內容中執行。 針對[多使用者應用程式](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)，使用 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) 方法來取得 **StoreContext** 物件，而不是 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) 方法。

```csharp
public async Task UninstallPackage(Windows.ApplicationModel.Package package)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    var storeContext = StoreContext.GetDefault();
    IAsyncOperation<StoreUninstallStorePackageResult> uninstallOperation =
        storeContext.RequestUninstallStorePackageAsync(package);

    // At this point, you can update your app UI to show that the package
    // is installing.

    uninstallOperation.Completed += (asyncInfo, status) =>
    {
        StoreUninstallStorePackageResult result = uninstallOperation.GetResults();
        switch (result.Status)
        {
            case StoreUninstallStorePackageStatus.Succeeded:
                {
                    // Update your app UI to show the package as uninstalled.
                    break;
                }
            default:
                {
                    // Update your app UI to show that the package uninstall failed.
                    break;
                }
        }
    };
}
```

## <a name="get-download-queue-info"></a>取得下載佇列資訊

從 Windows 10 版本 1803 起，您可以使用[GetAssociatedStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync)和[GetStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstorequeueitemsasync)方法取得 Microsoft Store 目前下載和安裝佇列中套件的資訊。 如果您的應用程式或遊戲支援需要數小時或數天才能下載並安裝的大型選用套件（包括 DLC），而您想要正常處理客戶在下載和安裝程序完成之前關閉應用程式或遊戲的案例，這些方法會非常有用。 當客戶再次啟動您的應用程式或遊戲，您的程式碼可以使用這些方法取得仍然在下載和安裝佇列中的套件狀態資訊，讓您可以向客戶顯示每個套件的狀態。

下列程式碼範例示範如何呼叫[GetAssociatedStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync)，針對目前的應用程式取得進行中套件更新的清單中，並擷取每個套件的狀態資訊。 此範例假設：
* 程式碼檔案含有 **Windows.Services.Store** 和 **System.Threading.Tasks** 命名空間的 **using** 陳述式。
* App 是單一使用者 App，僅會在啟動 App 的使用者內容中執行。 針對[多使用者應用程式](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)，使用 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) 方法來取得 **StoreContext** 物件，而不是 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) 方法。

> [!NOTE]
> 此範例程式碼所呼叫的**MarkUpdateInProgressInUI**、**RemoveItemFromUI**、**MarkInstallCompleteInUI**、**MarkInstallErrorInUI**和**MarkInstallPausedInUI**方法是預留位置方法，用來依據您自己的應用程式設計視需要實作。

```csharp
private StoreContext context = null;

private async Task GetQueuedInstallItemsAndBuildInitialStoreUI()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the Store packages in the install queue.
    IReadOnlyList<StoreQueueItem> storeUpdateItems = await context.GetAssociatedStoreQueueItemsAsync();

    foreach (StoreQueueItem storeItem in storeUpdateItems)
    {
        // In this example we only care about package updates.
        if (storeItem.InstallKind != StoreQueueItemKind.Update)
            continue;

        StoreQueueItemStatus currentStatus = storeItem.GetCurrentStatus();
        StoreQueueItemState installState = currentStatus.PackageInstallState;
        StoreQueueItemExtendedState extendedInstallState =
            currentStatus.PackageInstallExtendedState;

        // Handle the StatusChanged event to display current status to the customer.
        storeItem.StatusChanged += StoreItem_StatusChanged;

        switch (installState)
        {
            // Download and install are still in progress, so update the status for this  
            // item and provide the extended state info. The following methods are not
            // implemented in this example; you should implement them as needed for your
            // app's UI.
            case StoreQueueItemState.Active:
                MarkUpdateInProgressInUI(storeItem, extendedInstallState);
                break;
            case StoreQueueItemState.Canceled:
                RemoveItemFromUI(storeItem);
                break;
            case StoreQueueItemState.Completed:
                MarkInstallCompleteInUI(storeItem);
                break;
            case StoreQueueItemState.Error:
                MarkInstallErrorInUI(storeItem);
                break;
            case StoreQueueItemState.Paused:
                MarkInstallPausedInUI(storeItem, installState, extendedInstallState);
                break;
        }
    }
}

private void StoreItem_StatusChanged(StoreQueueItem sender, object args)
{
    StoreQueueItemStatus currentStatus = sender.GetCurrentStatus();
    StoreQueueItemState installState = currentStatus.PackageInstallState;
    StoreQueueItemExtendedState extendedInstallState = currentStatus.PackageInstallExtendedState;

    switch (installState)
    {
        // Download and install are still in progress, so update the status for this  
        // item and provide the extended state info. The following methods are not
        // implemented in this example; you should implement them as needed for your
        // app's UI.
        case StoreQueueItemState.Active:
            MarkUpdateInProgressInUI(sender, extendedInstallState);
            break;
        case StoreQueueItemState.Canceled:
            RemoveItemFromUI(sender);
            break;
        case StoreQueueItemState.Completed:
            MarkInstallCompleteInUI(sender);
            break;
        case StoreQueueItemState.Error:
            MarkInstallErrorInUI(sender);
            break;
        case StoreQueueItemState.Paused:
            MarkInstallPausedInUI(sender, installState, extendedInstallState);
            break;
    }
}
```

## <a name="related-topics"></a>相關主題

* [選用套件及相關集合的製作](optional-packages.md)
