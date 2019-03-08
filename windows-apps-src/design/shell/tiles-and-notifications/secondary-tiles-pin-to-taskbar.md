---
Description: 了解如何釘選到工作列的次要磚。
title: 次要磚釘選到工作列
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: windows 10 uwp，釘選到工作列，第二個圖格，釘選到工作列，第二個圖格快速鍵
ms.localizationpriority: medium
ms.openlocfilehash: 7ad322fe371b0e1f3605ffb4c29108a15bb28e0c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591973"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>次要磚釘選到工作列

就像釘選到開始的次要磚，您可以釘選到工作列，次要磚讓您的使用者快速存取內容應用程式內。

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **有限存取 API**:此 API 是有限的存取權的功能。 若要使用此 API，請連絡[ taskbarsecondarytile@microsoft.com ](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar)。

> **需要 2018 年 10 月更新**:您必須為目標 SDK 17763 和執行組建 17763 或更高版本以釘選到工作列。


## <a name="guidance"></a>指導方針

次要的圖格會提供一致且有效率的方式，讓使用者直接存取 應用程式中的特定區域。 雖然使用者可以選擇要 「 」 次要將磚釘選到工作列，由開發人員決定應用程式中內賴以為重的領域。 如需詳細指引，請參閱[次要磚指引](secondary-tiles-guidance.md)。


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1.判斷 API 是否存在，並解除鎖定有限存取權

較舊的裝置沒有工作列 （如果目標為舊版的 Windows 10） 釘選的 Api。 因此，您不應該顯示釘選按鈕不能釘選這些裝置上。

此外，這項功能已被鎖定在有限存取權。 若要存取，請連絡 Microsoft。 API 呼叫來 **[TaskbarManager.RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**，  **[TaskbarManager.IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)**，以及**[TaskbarManager.TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** 將會失敗並發生拒絕存取的例外狀況。 不允許使用沒有權限，此 API 應用程式和 API 定義可能隨時變更。

使用[ApiInformation.IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_)方法，以判斷 Api 是否存在。 然後使用**[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** 嘗試解除鎖定此 API 的 API。

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

UWP 應用程式可在各種裝置上執行；並非所有的裝置皆支援工作列。 目前只有傳統型裝置支援工作列。 此外，在工作列的存在可能轉瞬即逝。 若要檢查是否目前存在於工作列，請呼叫**[TaskbarManager.GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** 方法，並確認傳回的執行個體不是 null。 如果工作列不存在，則不顯示釘選按鈕。

我們建議您保存到執行個體的單一作業，例如釘選，和接著抓取的下次您需要執行另一項作業的新執行個體的持續時間。

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


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3.請檢查您的磚目前釘選到工作列

如果您的磚已釘選，您應該改為顯示 [取消釘選] 按鈕。 您可以使用**[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 方法來檢查是否目前釘選您並排顯示 （使用者可以取消釘選在任何時間）。 在這種方法，您會傳遞**TileId**您想要知道的磚已釘選。

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

釘選到工作列可由群組原則停用。 [TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed)屬性可讓您檢查是否允許釘選。

當使用者按一下釘選按鈕時，您應該檢查這個屬性，如果為 false，您應該會顯示一個訊息對話方塊，告訴釘選在這部電腦不允許的使用者。

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


## <a name="5-construct-and-pin-your-tile"></a>5.建構和您的圖格釘選

使用者已按一下釘選按鈕，並決定 Api 有、 工作列已存在，及可釘選...釘選的時間 ！

首先，建構您的次要磚，就如同在釘選到開始時。 您可以深入了解次要的圖格屬性，請閱讀[次要磚釘選到開始](secondary-tiles-pinning.md)。 不過，釘選到工作列，除了先前所需的屬性，Square44x44Logo （這是使用工作列的標誌） 時，也需要。 否則，會擲回例外狀況。

然後，將圖格，以傳遞**[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** 方法。 由於這是在有限存取權，這將不會顯示確認對話方塊中，而且不需要在 UI 執行緒。 但是，未來這開啟時超出有限存取權，呼叫端不利用有限存取權會出現一個對話方塊，並需要使用 UI 執行緒。

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

這個方法會傳回布林值，指出您的磚會現在釘選到工作列。 如果您的磚已經釘選，方法會更新現有的圖格，並傳回 true。 如果釘選不被允許或不支援的工作列，則方法會傳回 false。


## <a name="enumerate-tiles"></a>列舉的圖格

若要查看所有您所建立的圖格和仍會釘選某處 （開始、 工作列，或兩者），請使用 **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**。 您接著可以檢查這些圖格已釘選到工作列及 （或） 開始。 如果不支援的介面，這些方法會傳回 false。

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

若要更新已釘選的磚，您可以使用[ **SecondaryTile.UpdateAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync)方法中所述[更新次要磚](secondary-tiles-pinning.md#updating-a-secondary-tile)。


## <a name="unpin-a-tile"></a>取消釘選磚

如果目前已釘選圖格，您的應用程式應該提供 [取消釘選] 按鈕。 若要取消釘選磚，只要呼叫 **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**，並傳入**TileId**次要磚您想要取消固定。

這個方法會傳回布林值，指出您的圖格已不再釘選到工作列。 如果您未釘選的磚一開始，這也會傳回 true。 如果不允許取消釘選，這會傳回 false。

如果您的磚只釘選到工作列，這將刪除圖格，因為它已不再釘選任何一處。

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>刪除圖格

如果您想要取消釘選磚，以從任何位置 (Start、 工作列)，使用**[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** 方法。

這是適用於其中的內容釘選的使用者不再適用的案例。 比方說，如果您的應用程式可讓您釘選的 notebook 啟動和工作列，然後在使用者刪除 notebook，您應該只刪除 notebook 與相關聯的圖格。

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>只從開始取消釘選

如果您只想要同時又讓它在工作列上，從開始的次要磚取消釘選，您可以呼叫**[StartScreenManager.TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** 方法。 同樣地，如果它已不再釘選到任何其他介面，這將刪除圖格。

這個方法會傳回布林值，指出您的圖格已不再釘選到開始。 如果您未釘選的磚一開始，這也會傳回 true。 如果取消釘選不被允許或不支援開始，這會傳回 false。

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>資源

* [TaskbarManager 類別](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [次要磚釘選到開始](secondary-tiles-pinning.md)