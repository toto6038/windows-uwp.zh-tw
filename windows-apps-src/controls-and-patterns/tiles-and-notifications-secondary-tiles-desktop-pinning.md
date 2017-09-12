---
author: vladimp
Description: "Windows 傳統型應用程式因為傳統型橋接器之故而可以釘選次要磚！"
title: "從傳統型應用程式釘選次要磚"
label: Pin secondary tiles from desktop application
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, desktop bridge, secondary tiles, pin, pinning, quickstart, code sample, example, secondarytile, desktop application, win32, winforms, wpf, 傳統型橋接器, 次要磚, 釘選, 快速入門, 程式碼範例, 範例, 次要磚, 傳統型應用程式"
ms.openlocfilehash: dea17368a21983b9ca800aac2efa6410dab06958
ms.sourcegitcommit: 6396a69aab081f5c7a9a59739c83538616d3b1c7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/30/2017
---
# <a name="pin-secondary-tiles-from-desktop-application"></a>從傳統型應用程式釘選次要磚
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

由於[傳統型橋接器](https://developer.microsoft.com/en-us/windows/bridges/desktop)之故，Windows 傳統型應用程式 (如 Win32、Windows Form 及 WPF) 可釘選次要磚！

> [!IMPORTANT]
> **發行前版本 | 需要秋季版 Creators Update**：您必須執行[測試人員組建 16199](https://blogs.windows.com/windowsexperience/2017/05/17/announcing-windows-10-insider-preview-build-16199-pc-build-15215-mobile/#bDqf2Ah3Gd7FM66g.97) 或更高版本，才能從您的傳統型應用程式使用次要磚。

![次要磚螢幕擷取畫面](images/secondarytiles.png)

從 WPF 或 WinForms 應用程式新增次要磚的方式，與單純的 UWP 應用程式非常類似。 唯一的不同是，您必須指定您的主要視窗控制代碼 (HWND)。 這是因為當釘選磚時，Windows 會顯示強制回應對話方塊要求使用者確認是否要釘選磚。 如果傳統型應用程式未透過擁有者視窗設定 SecondaryTile 物件，則 Windows 不會知道要在何處繪製對話方塊，作業將會失敗。


## <a name="package-your-app-with-desktop-bridge"></a>使用傳統型橋接器封裝應用程式

如果您尚未使用傳統型橋接器封裝您的應用程式，[您必須先這麼做](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-root)，才能使用任何的 UWP API。


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

初始化新的次要磚物件，就和一般的 UWP 應用程式完全相同。 若要深入了解如何建立與釘選次要磚，請參閱[次要磚](tiles-and-notifications-secondary-tiles-pinning.md)。

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


## <a name="resources"></a>資源

* [完整程式碼範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [次要磚概觀](tiles-and-notifications-secondary-tiles.md)
* [釘選次要磚 (UWP)](tiles-and-notifications-secondary-tiles-pinning.md)
* [傳統型橋接器](https://developer.microsoft.com/en-us/windows/bridges/desktop)
* [傳統型橋接器程式碼範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)