---
Description: 瞭解如何將次要磚釘選到工作列。
title: 將次要磚釘選到工作列
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: windows 10，uwp，釘選到工作列，次要磚，將次要磚釘選到工作列，快捷方式
ms.localizationpriority: medium
ms.openlocfilehash: 5d4041faf2fbc729291da902e66be1e0979f9d97
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82971013"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>將次要磚釘選到工作列

如同釘選次要磚以開始，您可以將次要磚固定到工作列，讓使用者快速存取應用程式內的內容。

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **有限的存取 API**：此 API 是有限的存取功能。 若要使用此 API，請[taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar)洽詢。

> **需要2018年10月更新**：您必須以 SDK 17763 為目標，並執行組建17763或更高版本，才能釘選到工作列。


## <a name="guidance"></a>指導方針

次要磚提供一致且有效率的方式，讓使用者直接存取應用程式內的特定區域。 雖然使用者選擇是否要將次要磚「釘選」到工作列，但應用程式中的可固定區域是由開發人員決定。 如需詳細指引，請參閱[次要磚指引](secondary-tiles-guidance.md)。


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. 判斷 API 是否存在，並將受限存取權解除鎖定

較舊的裝置沒有工作列釘選 Api （如果您是以舊版 Windows 10 為目標）。 因此，您不應該在無法釘選的裝置上顯示 [釘選] 按鈕。

此外，這項功能在有限存取之下受到鎖定。 若要取得存取權，請洽詢 Microsoft。 **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**、 **[TaskbarManager、IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 和**[TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** 的 API 呼叫將會失敗，並出現「拒絕存取」例外狀況。 應用程式不允許在沒有許可權的情況下使用此 API，而且 API 定義可能會隨時變更。

使用[ApiInformation. IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_)方法來判斷 api 是否存在。 然後使用**[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** api 來嘗試解除鎖定 API。

```csharp
if (ApiInformation.IsMethodPresent("Windows.UI.Shell.TaskbarManager", "RequestPinSecondaryTileAsync"))
{
    // API present!
    // Unlock the pin to taskbar feature
    var result = LimitedAccessFeatures.TryUnlockFeature(
        "com.microsoft.windows.secondarytilemanagement",
        "<tokenFromMicrosoft>",
        "<publisher> has registered their use of com.microsoft.windows.secondarytilemanagement with Microsoft and agrees to the terms of use.");

    // If unlock succeeded
    if ((result.Status == LimitedAccessFeatureStatus.Available) ||
        (result.Status == LimitedAccessFeatureStatus.AvailableWithoutToken))
    {
        // Continue
    }
    else
    {
        // Don't show pin to taskbar button or call any of the below APIs
    }
}

else
{
    // Don't show pin to taskbar button or call any of the below APIs
}
```


## <a name="2-get-the-taskbarmanager-instance"></a>2. 取得 TaskbarManager 實例

Windows 應用程式可以在各種不同的裝置上執行;並非全部都支援工作列。 目前只有傳統型裝置支援工作列。 此外，也有可能出現並不存在工作列。 若要檢查工作列目前是否存在，請呼叫**[TaskbarManager. GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** 方法，並檢查傳回的實例是否不是 null。 如果工作列不存在，請勿顯示 [釘選] 按鈕。

建議您在單一作業持續期間（例如釘選）保留實例，然後在下次需要執行另一項作業時抓取新的實例。

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();

if (taskbarManager != null)
{
    // Continue
}
else
{
    // Taskbar not present, don't display a pin button
}
```


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. 檢查您的磚目前是否已釘選到工作列

如果您的磚已釘選，您應該改為顯示 [取消釘選] 按鈕。 您可以使用**[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 方法來檢查您的磚目前是否已釘選（使用者可以隨時將其取消釘選）。 在此方法中，您會傳遞您想要知道的磚的**TileId**已釘選。

```csharp
if (await taskbarManager.IsSecondaryTilePinnedAsync("myTileId"))
{
    // The tile is already pinned. Display the unpin button.
}

else 
{
    // The tile is not pinned. Display the pin button.
}
```


## <a name="4-check-whether-pinning-is-allowed"></a>4. 檢查是否允許釘選

群組原則可以停用釘選到工作列。 [TaskbarManager. IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed)屬性可讓您檢查是否允許釘選。

當使用者按一下您的 [釘選] 按鈕時，您應該檢查此屬性，如果是 false，您應該會顯示訊息對話方塊，通知使用者此電腦上不允許釘選。

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager == null)
{
    // Display message dialog informing user that taskbar is no longer present, and then hide the button
}

else if (taskbarManager.IsPinningAllowed == false)
{
    // Display message dialog informing user pinning is not allowed on this machine
}

else
{
    // Continue pinning
}
```


## <a name="5-construct-and-pin-your-tile"></a>5. 建立和釘選您的磚

使用者已按下您的 [釘選] 按鈕，而且您已判斷 Api 存在、有工作列存在，而且允許釘選 .。。要釘選的時間！

首先，請與您在釘選開始時一樣，建立次要磚。 若要深入瞭解次要磚屬性，請閱讀[釘選次要磚以開始](secondary-tiles-pinning.md)。 不過，當釘選到工作列時，除了先前所需的屬性之外，也需要 Square44x44Logo （這是工作列所使用的標誌）。 否則便會擲回例外狀況。

然後，將磚傳遞至**[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** 方法。 由於這是有限的存取權，因此不會顯示確認對話方塊，也不需要 UI 執行緒。 但在未來，當這項功能不受限制存取的情況下開啟時，呼叫端不會使用有限存取權時，將會收到對話方塊，而且必須要有此 UI 執行緒。

```csharp
// Initialize the tile (all properties below are required)
SecondaryTile tile = new SecondaryTile("myTileId");
tile.DisplayName = "PowerPoint 2016 (Remote)";
tile.Arguments = "app=powerpoint";
tile.VisualElements.Square44x44Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square44x44Logo.png");
tile.VisualElements.Square150x150Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square150x150Logo.png");

// Pin it to the taskbar
bool isPinned = await taskbarManager.RequestPinSecondaryTileAsync(tile);
```

這個方法會傳回布林值，指出您的磚現在是否已釘選到工作列。 如果您的磚已釘選，方法會更新現有的磚，並傳回 true。 如果不允許釘選或不支援工作列，此方法會傳回 false。


## <a name="enumerate-tiles"></a>列舉磚

若要查看您所建立且仍固定在某個位置（[開始]、[工作列] 或兩者）的所有磚，請使用**[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**。 接著，您可以檢查這些磚是否已釘選到工作列及/或開始。 如果介面不受支援，這些方法會傳回 false。

```csharp
var taskbarManager = TaskbarManager.GetDefault();
var startScreenManager = StartScreenManager.GetDefault();

// Look through all tiles
foreach (SecondaryTile tile in await SecondaryTile.FindAllAsync())
{
    if (taskbarManager != null && await taskbarManager.IsSecondaryTilePinnedAsync(tile.TileId))
    {
        // Tile is pinned to the taskbar
    }

    if (startScreenManager != null && await startScreenManager.ContainsSecondaryTileAsync(tile.TileId))
    {
        // Tile is pinned to Start
    }
}
```


## <a name="update-a-tile"></a>更新磚

若要更新已釘選的圖格，您可以使用[**SecondaryTile. UpdateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync)方法，如[更新次要磚](secondary-tiles-pinning.md#updating-a-secondary-tile)中所述。


## <a name="unpin-a-tile"></a>取消釘選磚

如果圖格目前已釘選，您的應用程式應該提供 [取消釘選] 按鈕。 若要取消釘選磚，只要呼叫**[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**，傳入您想要取消釘選之次要磚的**TileId**即可。

這個方法會傳回布林值，指出您的磚是否不再釘選到工作列。 如果您的磚並未在第一個位置固定，這也會傳回 true。 如果不允許取消釘選，這會傳回 false。

如果您的磚只釘選到工作列，這會刪除磚，因為它不再釘選到任何位置。

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>刪除磚

如果您想要從任何位置取消釘選磚（[開始]、[工作列]），請使用**[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** 方法。

這適用于使用者所釘選的內容不再適用的情況。 例如，如果您的應用程式可讓您將筆記本釘選到 [啟動] 和 [工作列]，然後使用者刪除筆記本，則您應該直接刪除與筆記本相關聯的磚。

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>僅從開始取消釘選

如果您只想要從啟動時取消釘選次要磚，而將它保留在工作列上，您可以呼叫**[StartScreenManager. TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** 方法。 如果圖格已不再釘選到任何其他表面上，這也會刪除磚。

這個方法會傳回布林值，指出您的磚是否不再釘選以開始。 如果您的磚並未在第一個位置固定，這也會傳回 true。 如果不允許取消釘選，或不支援啟動，則會傳回 false。

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>資源

* [TaskbarManager 類別](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [釘選次要磚以開始](secondary-tiles-pinning.md)
