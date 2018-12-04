---
Description: Learn how to pin secondary tiles to taskbar.
title: 釘選到工作列的次要磚
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: windows 10，uwp，釘選到工作列，次要磚，釘選到工作列，次要磚捷徑
ms.localizationpriority: medium
ms.openlocfilehash: 7ad322fe371b0e1f3605ffb4c29108a15bb28e0c
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8474920"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>釘選到工作列的次要磚

就像釘選到 [開始] 的次要磚，您可以釘選到工作列，次要磚提供您的使用者快速存取內容內您的應用程式。

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **有限的存取權 API**： 此 API 是有限的存取權 」 功能。 若要使用此 API，請連絡[taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar)。

> **需要 2018 年 10 月更新**： 您必須以 SDK 17763 目標並執行組建 17763 或更高版本來釘選到工作列。


## <a name="guidance"></a>指引

次要磚提供一致且有效率的方式，讓使用者直接存取應用程式中的特定區域。 雖然使用者可選擇要 「 釘選 」 到工作列的次要磚，應用程式中的可釘選區域是由開發人員決定的。 如需詳細指導方針，請參閱[次要磚指導方針](secondary-tiles-guidance.md)。


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1.判斷 API 是否存在，並解除鎖定限制存取

舊款裝置沒有工作列釘選 Api （如果您的目標較舊版本的 Windows 10）。 因此，您不應顯示的 pin 按鈕不是能夠釘選這些裝置上。

此外，此功能已鎖定有限存取 \] 下方。 為了進行存取，請連絡 Microsoft。 拒絕存取例外狀況**[TaskbarManager.RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**、 **[TaskbarManager.IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)**，以及**[TaskbarManager.TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** API 呼叫會失敗。 若要使用此 API 權限，而不允許應用程式和 API 定義可能會隨時變更。

使用[ApiInformation.IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_)方法來判斷 Api 是否存在。 然後再嘗試解除鎖定 API 中使用**[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** API。

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


## <a name="2-get-the-taskbarmanager-instance"></a>2.取得 TaskbarManager 執行個體

UWP 應用程式可在各種裝置上執行；並非所有的裝置皆支援工作列。 目前只有傳統型裝置支援工作列。 此外，可能會有工作列存在，且移。 若要檢查工作列是否為目前存在、 呼叫**[TaskbarManager.GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** 方法並檢查傳回的執行個體不是 null。 如果工作列不存在，不要顯示 pin 按鈕。

我們建議按住不放到執行個體上的單一的作業，例如釘選，然後抓取您需要執行其他操作在下一次的新執行個體的持續時間。

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


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3.檢查您的磚目前是否釘選到工作列

如果您的磚已釘選，您應該改為顯示取消釘選按鈕。 您可以使用**[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 方法來檢查您的磚目前是否釘選 （使用者可以取消釘選它在任何時間）。 您可以在這種方法，傳遞您想要知道磚**TileId**是否已釘選。

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


## <a name="4-check-whether-pinning-is-allowed"></a>4.檢查是否允許釘選

釘選到工作列上可以透過群組原則停用。 [TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed)屬性可讓您檢查是否允許釘選。

當使用者按一下您的 pin 按鈕時，您應該檢查這個屬性，以及如果它是 false，您應該會顯示訊息對話方塊，告知使用者的這部電腦上不允許釘選。

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


## <a name="5-construct-and-pin-your-tile"></a>5.建構和釘選您的磚

使用者按一下您的 pin 的按鈕，並決定的 Api 都存在，工作列已存在，並允許釘選，...時間釘選到 ！

就像您釘選到 [開始] 時，第一次，建構您的次要磚。 您可以深入了解次要磚屬性讀取[釘選到 [開始] 的次要磚](secondary-tiles-pinning.md)。 不過，當釘選到工作列，除了先前所需的屬性，Square44x44Logo （這是工作列所使用的標誌） 也是必要。 否則，將會擲回例外狀況。

然後，將磚傳遞至**[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** 方法。 這是有限存取 \] 下方，因為這不會顯示確認對話方塊，並不需要在 UI 執行緒。 但在未來這開啟時超出限制存取不利用有限存取會接收對話方塊，並要求使用 UI 執行緒呼叫者。

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

這個方法會傳回布林值，指出您的磚是否現在釘選到工作列。 如果您的磚已釘選，該方法會更新現有的磚，並會傳回 true。 如果不允許釘選，或不支援工作列，此方法會傳回 false。


## <a name="enumerate-tiles"></a>列舉磚

若要查看您建立並釘選仍然某處的所有磚 （[開始] 畫面的、 工作列，或兩者），使用**[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**。 接下來，您可以檢查這些磚是否釘選到工作列和/或 [開始] 畫面。 如果 surface 不受支援，這些方法會傳回 false。

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

若要更新已釘選的磚，您可以使用[**SecondaryTile.UpdateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync)方法[更新次要磚](secondary-tiles-pinning.md#updating-a-secondary-tile)中所述。


## <a name="unpin-a-tile"></a>取消釘選磚

磚目前已釘選您的應用程式應該提供取消釘選按鈕。 若要取消釘選磚，只需呼叫**[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**，傳入您想要取消釘選次要磚**TileId** 。

這個方法會傳回布林值，指出您的磚已不再釘選到工作列。 如果您的磚未釘選一開始，這也會傳回 true。 如果不允許取消釘選，這會傳回 false。

如果只有您的磚已釘選到工作列，這將會刪除磚，因為它不會再釘選任何地方。

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>刪除磚

如果您想要取消釘選磚，以從任何地方 （開始，工作列），請使用**[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** 方法。

這是適當的情況下，使用者釘選的內容會不再適用。 例如，如果您的應用程式可讓您釘選到 [開始] 畫面和工作列，筆記型電腦，然後使用者刪除筆記本，您應該只需刪除筆記本相關聯的磚。

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>只會從 [開始] 畫面取消釘選

如果您只想要同時讓它保持在工作列上，次要磚從開始畫面取消釘選，您可以呼叫**[StartScreenManager.TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** 方法。 同樣地，如果不再釘選到任何其他的表面，這將會刪除磚。

這個方法會傳回布林值，指出您的磚已不再釘選到 [開始]。 如果您的磚未釘選一開始，這也會傳回 true。 如果取消釘選不被允許或不支援 [開始] 畫面，這會傳回 false。

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>資源

* [TaskbarManager 類別](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [釘選到 [開始] 的次要磚](secondary-tiles-pinning.md)