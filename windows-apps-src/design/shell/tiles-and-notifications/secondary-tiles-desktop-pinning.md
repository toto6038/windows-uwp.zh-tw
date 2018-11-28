---
Description: Windows desktop applications can pin secondary tiles thanks to the Desktop Bridge!
title: 從傳統型應用程式釘選次要磚
label: Pin secondary tiles from desktop application
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: windows 10, desktop bridge, secondary tiles, pin, pinning, quickstart, code sample, example, secondarytile, desktop application, win32, winforms, wpf, 傳統型橋接器, 次要磚, 釘選, 快速入門, 程式碼範例, 範例, 次要磚, 傳統型應用程式
ms.localizationpriority: medium
ms.openlocfilehash: 1e713f37cd5e5fbf4b2771e76fb7e132b5976629
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7838138"
---
# <a name="pin-secondary-tiles-from-desktop-application"></a>從傳統型應用程式釘選次要磚


由於[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)之故，Windows 傳統型應用程式 (如 Win32、Windows Form 及 WPF) 可釘選次要磚！

![次要磚螢幕擷取畫面](images/secondarytiles.png)

> [!IMPORTANT]
> **需要 Fall Creators Update**：您的目標必須是 SDK 16299 並執行組建 16299 或更新版本，才能從您的傳統型橋接器應用程式釘選次要磚。

從 WPF 或 WinForms 應用程式新增次要磚的方式，與單純的 UWP 應用程式非常類似。 唯一的不同是，您必須指定您的主要視窗控制代碼 (HWND)。 這是因為當釘選磚時，Windows 會顯示強制回應對話方塊要求使用者確認是否要釘選磚。 如果傳統型應用程式未透過擁有者視窗設定 SecondaryTile 物件，則 Windows 不會知道要在何處繪製對話方塊，作業將會失敗。


## <a name="package-your-app-with-desktop-bridge"></a>使用傳統型橋接器封裝應用程式

如果您尚未使用傳統型橋接器封裝您的應用程式，[您必須先這麼做](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)，才能使用任何的 UWP API。


## <a name="enable-access-to-iinitializewithwindow-interface"></a>允許存取 IInitializeWithWindow 介面

如果您的應用程式是以受管理的語言 (例如 C# 或 Visual Basic) 撰寫的，請在您的應用程式程式碼中以 [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx) 與 Guid 屬性宣告 IInitializeWithWindow 介面，如以下 C# 範例所示。 此範例假設您的程式碼檔案有 System.Runtime.InteropServices 命名空間的 using 陳述式。

```csharp
[ComImport]
[Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IInitializeWithWindow
{
    void Initialize(IntPtr hwnd);
}
```

或者，如果您正在使用 C++，請在程式碼中新增對 **shobjidl.h** 標頭檔案的參考。 此標頭檔案包含 *IInitializeWithWindow* 介面的宣告。


## <a name="initialize-the-secondary-tile"></a>初始化次要磚

初始化新的次要磚物件，就和一般的 UWP 應用程式完全相同。 若要深入了解如何建立與釘選次要磚，請參閱[次要磚](secondary-tiles-pinning.md)。

```csharp
// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    "myTileId5391",
    "Display name",
    "myActivationArgs",
    new Uri("ms-appx:///Assets/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="assign-the-window-handle"></a>指派視窗控制代碼

這是傳統型應用程式的主要步驟。 將物件投射到 [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx) 物件。 接著，呼叫 [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx) 方法，並傳遞您要成為強制回應對話方塊之擁有者的視窗控制代碼。 下列 C# 範例說明如何將您應用程式主要視窗的控制代碼傳遞給方法。

```csharp
// Assign the window handle
IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)tile;
initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
```


## <a name="pin-the-tile"></a>釘選磚

最後，要求釘選磚，就如同一般 UWP 應用程式的做法。

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="send-tile-notifications"></a>傳送磚通知

> [!IMPORTANT]
> **需要 2018 年 4 月版本 17134.81 或更新版本**：您必須執行組建 17134.81 或更新版本，以從傳統型橋接器應用程式傳送磚或徽章通知至次要磚。 這個 .81 維護更新之前，當從傳統型橋接器應用程式傳送磚或徽章通知至次要磚時，發生 0x80070490 *找不到元素* 例外。

傳送磚或徽章通知與 UWP app 相同。 如需詳細資訊，請參閱[傳送本機磚通知](sending-a-local-tile-notification.md)以開始使用。


## <a name="resources"></a>資源

* [完整程式碼範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [次要磚概觀](secondary-tiles.md)
* [釘選次要磚 (UWP)](secondary-tiles-pinning.md)
* [傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)
* [傳統型橋接器程式碼範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)