---
author: andrewleader
Description: Learn how to pin a secondary tile from your UWP app.
title: 釘選次要磚
label: Pin secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
keywords: windows 10, uwp, secondary tiles, pin, pinning, quickstart, code sample, example, secondarytile, 次要磚, 釘選, 快速入門, 程式碼範例, 範例
ms.localizationpriority: medium
ms.openlocfilehash: 7fcea65a43ec3ca3d7e29056bec129e0f03d4229
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5567455"
---
# <a name="pin-secondary-tiles"></a>釘選次要磚


本主題會引導您完成為 UWP 應用程式建立次要磚並釘選到 \[開始\] 功能表的步驟。

![次要磚螢幕擷取畫面](images/secondarytiles.png)

若要深入了解次要磚，請參閱[次要磚概觀](secondary-tiles.md)。


## <a name="add-namespace"></a>新增命名空間

Windows.UI.StartScreen 命名空間包含 SecondaryTile 類別。

```csharp
using Windows.UI.StartScreen;
```


## <a name="initialize-the-secondary-tile"></a>初始化次要磚

次要磚包含一些主要元件...

* **TileId**：唯一識別碼，可讓您能在其他次要磚之間識別出該磚。
* **DisplayName**：您要在磚上顯示的名稱。
* **Arguments**：當使用者按一下您的磚時，您要傳回到應用程式的引數。
* **Square150x150Logo**：所需的標誌，顯示在中型磚上 (如果未提供小型標誌，可調整成小型磚)。

您**必須**為所有上述的屬性提供初始化的值，否則會發生例外狀況。

有各種建構函式可供您使用，但使用採用 tileId、displayName、arguments、square150x150Logo 及 desiredSize 的建構函式可幫助您確認設定所有的必要屬性。

```csharp
// Construct a unique tile ID, which you will need to use later for updating the tile
string tileId = "City" + zipCode;

// Use a display name you like
string displayName = cityName;

// Provide all the required info in arguments so that when user
// clicks your tile, you can navigate them to the correct content
string arguments = "action=viewCity&zipCode=" + zipCode;

// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    tileId,
    displayName,
    arguments,
    new Uri("ms-appx:///Assets/CityTiles/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="optional-add-support-for-larger-tile-sizes"></a>選用：新增大型磚的支援

如果您會在次要磚上顯示豐富的磚通知，您可能會想要允許使用者將其磚調寬或調大，讓他們可以看到更多的內容。

若要允許磚大小更寬、更大，您必須提供 *Wide310x150Logo* 與 *Square310x310Logo*。 此外若有可能，您應為小型磚提供 *Square71x71Logo* (否則我們將針對小型磚縮小您所需的 Square150x150Logo)。

您也可以提供唯一的 *Square44x44Logo* (當出現通知時會選擇性地顯示在右下角)。 如果您未提供，則將改用您主要磚中的 *Square44x44Logo*。

```csharp
// Enable wide and large tile sizes
tile.VisualElements.Wide310x150Logo = new Uri("ms-appx:///Assets/CityTiles/Wide310x150Logo.png");
tile.VisualElements.Square310x310Logo = new Uri("ms-appx:///Assets/CityTiles/Square310x310Logo.png");

// Add a small size logo for better looking small tile
tile.VisualElements.Square71x71Logo = new Uri("ms-appx:///Assets/CityTiles/Square71x71Logo.png");

// Add a unique corner logo for the secondary tile
tile.VisualElements.Square44x44Logo = new Uri("ms-appx:///Assets/CityTiles/Square44x44Logo.png");
```


## <a name="optional-enable-showing-the-display-name"></a>選用：允許顯示顯示名稱

顯示名稱預設為「不」顯示。 若要在中/寬/大型磚上顯示顯示名稱，請新增下列程式碼。

```csharp
// Show the display name on all sizes
tile.VisualElements.ShowNameOnSquare150x150Logo = true;
tile.VisualElements.ShowNameOnWide310x150Logo = true;
tile.VisualElements.ShowNameOnSquare310x310Logo = true;
```


## <a name="optional-3d-secondary-tiles"></a>選用：3D 次要磚
加入 3D 資產，可以美化 Windows Mixed Reality 的次要磚。 在混合實境環境中使用您的應用程式時，使用者可以直接在其 Windows Mixed Reality 首頁放置 3D 磚，而不是放在 [開始] 功能表中。 例如，您可以建立直接連結至 360 度相片檢視器應用程式的 360 度全景相片，或讓使用者從家具型錄放置椅子的 3D 模型，在選取時會開啟關於該物件的定價與色彩選項等詳細資料。 若要開始，請參考[混合實境開發人員文件](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_deep_links_for_your_app_in_the_windows_mixed_reality_home)。



## <a name="pin-the-secondary-tile"></a>釘選次要磚

最後，要求釘選該磚。 請注意，這此必須從 UI 執行緒呼叫。 在桌面上將會出現一個對話方塊，要求使用者確認是否要釘選磚。

> [!IMPORTANT]
> 如果是使用傳統型橋接器的 Windows 傳統型應用程式，您必須先額外執行[從傳統型應用程式釘選](secondary-tiles-desktop-pinning.md)中所述的步驟。

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="check-if-a-secondary-tile-exists"></a>檢查次要磚是否存在

如果您的使用者造訪您應用程式中他們已釘選到 \[開始\] 畫面的頁面，您會想要改為顯示 \[取消釘選\] 按鈕。

因此在選擇要顯示哪一個按鈕時，您必須先檢查次要磚目前是否已釘選。

```csharp
// Check if the secondary tile is pinned
bool isPinned = SecondaryTile.Exists(tileId);

// TODO: Update UI to reflect whether user can either unpin or pin
```


## <a name="unpinning-a-secondary-tile"></a>取消釘選次要磚

如果次要磚目前已釘選，且使用者點擊取消釘選按鈕，您會想要取消釘選 (刪除) 次要磚。

```csharp
// Initialize a secondary tile with the same tile ID you want removed
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then unpin the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="updating-a-secondary-tile"></a>更新次要磚

如果您必須更新標誌、顯示名稱，或次要磚上的任何其他項目，則可使用 *RequestUpdateAsync*。

```csharp
// Initialize a secondary tile with the same tile ID you want to update
SecondaryTile tile = new SecondaryTile(tileId);

// Assign ALL properties, including ones you aren't changing

// And then update it
await tile.UpdateAsync();
```


## <a name="enumerating-all-pinned-secondary-tiles"></a>列舉所有已釘選的次要磚

如果您必須找出所有已由您使用者釘選的磚，您可以改為使用 *SecondaryTile.FindAllAsync()*，而非使用 *SecondaryTile.Exists*。

```csharp
// Get all secondary tiles
var tiles = await SecondaryTile.FindAllAsync();
```


## <a name="send-a-tile-notification"></a>傳送磚通知

若要了解如何透過磚通知在您的磚上顯示豐富內容，請參閱[傳送本機磚通知](sending-a-local-tile-notification.md)。


## <a name="related"></a>相關

* [次要磚概觀](secondary-tiles.md)
* [次要磚指導方針](secondary-tiles-guidance.md)
* [磚資產](app-assets.md)
* [磚內容文件](create-adaptive-tiles.md)
* [傳送本機磚通知](sending-a-local-tile-notification.md)
