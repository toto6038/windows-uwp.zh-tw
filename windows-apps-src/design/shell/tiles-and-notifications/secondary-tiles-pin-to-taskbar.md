---
description: 瞭解如何將次要磚釘選到工作列，讓使用者可以快速存取您應用程式內的內容。
title: 將次要磚釘選到工作列
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: windows 10、uwp、釘選到工作列、次要磚、將次要磚釘選到工作列、快捷方式
ms.localizationpriority: medium
ms.openlocfilehash: 23feaf6cbc2293951116167662ab5647e3d35c44
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172322"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>將次要磚釘選到工作列

就像釘選次要磚以開始，您可以將次要磚釘選到工作列，讓使用者快速存取應用程式內的內容。

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **有限的存取 api**：此 api 是一項受限的存取功能。 若要使用此 API，請聯絡 [taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar) 。

> **需要2018年10月更新**：您必須以 SDK 17763 為目標，並執行組建17763或更高版本，才能釘選到工作列。


## <a name="guidance"></a>指引

次要磚可提供一致且有效率的方式，讓使用者直接存取應用程式內的特定區域。 雖然使用者選擇是否要將次要磚「釘選」到工作列，但應用程式中的可釘選區域是由開發人員決定。 如需詳細指引，請參閱 [次要磚指引](secondary-tiles-guidance.md)。


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. 判斷 API 是否存在並釋放保留存取

如果您以舊版的 Windows 10) 為目標，則較舊的裝置沒有 (的工作列釘選 Api。 因此，您不應該在無法釘選的裝置上顯示釘選按鈕。

此外，這項功能會在有限存取下鎖定。 若要取得存取權，請洽詢 Microsoft。 對 **[TaskbarManager. RequestPinSecondaryTileAsync](/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**、 **[TaskbarManager. IsSecondaryTilePinnedAsync](/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 和 **[TaskbarManager](/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** 的 API 呼叫將會失敗，並出現「拒絕存取」例外狀況。 不允許應用程式在沒有許可權的情況下使用此 API，而且 API 定義可能會隨時變更。

使用 [ApiInformation. IsMethodPresent](/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) 方法來判斷 api 是否存在。 然後使用 **[LimitedAccessFeatures](/uwp/api/windows.applicationmodel.limitedaccessfeatures)** api 來嘗試解除鎖定 api。

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

Windows 應用程式可以在各種不同的裝置上執行;並非所有專案都支援工作列。 目前只有傳統型裝置支援工作列。 此外，也有可能出現工作列。 若要檢查是否目前有工作列存在，請呼叫 **[TaskbarManager GetDefault](/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** 方法，並檢查傳回的實例是否不是 null。 如果工作列不存在，請不要顯示 [釘選] 按鈕。

建議您在單一作業的持續時間內（例如釘選）保存實例，然後在您下次需要進行另一項作業時，再抓取新的實例。

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

如果您的磚已釘選，您應該改為顯示 [取消釘選] 按鈕。 您可以使用 **[IsSecondaryTilePinnedAsync](/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 方法來檢查您的磚目前是否已釘選 (使用者可以隨時將其取消釘選) 。 在此方法中，您會將您想要知道的磚 **TileId** 傳遞給釘選。

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

群組原則可以停用釘選到工作列。 [TaskbarManager. IsPinningAllowed](/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed)屬性可讓您檢查是否允許釘選。

當使用者按一下 [釘選] 按鈕時，您應該檢查這個屬性，如果是 false，您應該會顯示訊息對話方塊，通知使用者此電腦上不允許釘選。

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


## <a name="5-construct-and-pin-your-tile"></a>5. 建立和釘選磚

使用者已按下您的 [釘選] 按鈕，且您已判斷出 Api 存在、工作列存在，而且允許釘選 .。。pin 的時間！

首先，請建立您的次要磚，就像釘選開始時一樣。 您可以讀取 [釘選次要磚來開始，以](secondary-tiles-pinning.md)深入瞭解次要磚屬性。 不過，當釘選到工作列時，除了先前需要的屬性之外，Square44x44Logo (這是工作列) 所使用的標誌。 否則便會擲回例外狀況。

然後，將磚傳遞給 **[RequestPinSecondaryTileAsync](/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** 方法。 由於這是受限制的存取權，因此不會顯示確認對話方塊，也不需要 UI 執行緒。 但在未來，當這項功能不受限制存取時，呼叫端不會使用有限存取權，將會收到一個對話方塊，而且必須使用 UI 執行緒才能使用。

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

這個方法會傳回布林值，指出您的磚現在是否已釘選到工作列。 如果您的磚已釘選，方法會更新現有的磚，並傳回 true。 如果不允許釘選，或不支援工作列，則方法會傳回 false。


## <a name="enumerate-tiles"></a>列舉磚

若要查看您建立的所有圖格，但仍固定在某個位置 (開始、工作列或兩個) ，請使用 **[FindAllAsync](/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**。 接著，您可以檢查這些磚是否釘選到工作列和/或啟動。 如果介面不受支援，則這些方法會傳回 false。

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

若要更新已釘選的圖格，您可以使用 [**SecondaryTile. >updateasync**](/uwp/api/windows.ui.startscreen.secondarytile.updateasync) 方法（如 [更新次要磚](secondary-tiles-pinning.md#updating-a-secondary-tile)中所述）。


## <a name="unpin-a-tile"></a>取消釘選磚

如果磚目前已釘選，您的應用程式應該提供 [取消釘選] 按鈕。 若要取消釘選磚，只要呼叫 **[TryUnpinSecondaryTileAsync](/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**，然後傳入您要取消固定的次要磚 **TileId** 即可。

這個方法會傳回布林值，指出您的磚是否不再釘選到工作列。 如果您的磚未固定在第一個位置，這也會傳回 true。 如果不允許取消釘選，則會傳回 false。

如果您的磚只釘選到工作列，這將會刪除磚，因為它已不再釘選到任何位置。

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>刪除磚

如果您想要從任何地方取消釘選圖格 (開始，工作列) ，請使用 **[RequestDeleteAsync](/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** 方法。

這適用于使用者釘選的內容不再適用的情況。 例如，如果您的應用程式可讓您將筆記本釘選到 [開始] 和 [工作列]，然後使用者刪除筆記本，您應該只刪除與筆記本相關聯的磚。

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>僅從開始取消釘選

如果您只想要在工作列上將次要磚取消釘選，請呼叫 **[StartScreenManager. TryRemoveSecondaryTileAsync](/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** 方法。 同樣地，如果磚不再釘選到任何其他表面，也會刪除磚。

這個方法會傳回布林值，指出您的磚是否不再釘選為開始。 如果您的磚未固定在第一個位置，這也會傳回 true。 如果不允許取消釘選，或不支援啟動，則會傳回 false。

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>資源

* [TaskbarManager 類別](/uwp/api/windows.ui.shell.taskbarmanager)
* [將次要磚釘選到開始](secondary-tiles-pinning.md)