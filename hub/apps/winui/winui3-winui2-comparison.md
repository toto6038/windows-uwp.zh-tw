---
title: WinUI 3 與 WinUI 2 的比較
description: WinUI 3 和 WinUI 2 的快速比較。
ms.topic: article
ms.date: 03/19/2021
keywords: windows 10, uwp, 工具組 sdk, winui, Windows UI 程式庫
ms.openlocfilehash: 8c415b3c08e0f7cd215cd99f2c54fc3cb760d3e0
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730865"
---
# <a name="comparison-of-winui-3-and-winui-2"></a>WinUI 3 與 WinUI 2 的比較

:::image type="content" source="../images/logo-winui2-vs-winui3.png" alt-text="Win UI 2 與 Win UI 3 標誌":::

目前有兩個獨特的 Windows UI 程式庫 (WinUI) ： WinUI 2 和 WinUI 3。 雖然兩者都提供以最新 [Fluent Design 系統](https://www.microsoft.com/design/fluent) 準則和程式為基礎的使用者介面和經驗，但各有不同開發進度和發行排程的不同開發目標和範圍。

[WinUI 2](winui2/index.md)和[WinUI 3](winui3/index.md)都支援在 Windows 10 上開發可供生產環境使用的應用程式，同時支援使用 c # 或 c + +/Win32 來建立您的應用程式。

這兩個都是在主動式開發中，並定期發行更新。

## <a name="the-major-differences"></a>主要差異

| WinUI 3                                                                                                                                                                                                                                                                                                        | WinUI 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| UX 堆疊和控制項程式庫完全與作業系統和 [WINDOWS 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/)分離，包括 ux 堆疊的核心架構、組合和輸入層。                                                                        | UX 堆疊和控制項程式庫會緊密結合到作業系統和 [WINDOWS 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/)。                                                                                                                                                                                                                                                                                                                                                          |
| WinUI 3 可用來建立可供生產環境使用的 Windows **desktop/Win32** 應用程式。 | WinUI 2 無法用來建立 Windows **desktop/Win32** 應用程式。 |
| WinUI 3 隨附于 [Project 留尼旺島](../project-reunion/index.md) framework 套件的元件中，專案中的 Visual Studio 專案範本 Visual Studio 擴充功能 (VSIX) 。 | WinUI 2 的一部分隨附于作業系統本身 (Windows. *) UWP WinRT Api 的系列中，其中一部分隨附為「Windows UI 程式庫 ) 2」 ( 的程式庫，其中包含在作業系統本身所包含的其他控制項、元素和最新的樣式。 使用 WinUI 2，這些功能會隨附于可下載的 NuGet 套件中。 不過，其他重要的 UI 堆疊部分仍會內建于 OS，例如核心 XAML 架構、輸入和組合圖層。 |
| WinUI 3 支援適用于桌面應用程式的 c #/.NET 5。 | WinUI 2 僅支援 c #/.NET 原生應用程式。 |
| 適用于生產環境的 UWP 應用程式的 WinUI 3 支援目前為預覽狀態，請參閱 [WinUI 3-Project 留尼旺島 0.5 preview](winui3/release-notes/winui3-project-reunion-0.5-preview.md)。                                                                                                                                | 將 NuGet 套件安裝到新的或現有的 UWP 專案中，即可將 WinUI 2 併入生產 UWP 應用程式。 然後，您可以直接在新的應用程式中或藉由更新 "windows." 來參考 WinUI 控制項和樣式。 "microsoft. ui" 的命名空間參考。 在現有的應用程式中。                                                                                                                                                                                    |
| WinUI 3 支援以 Chromium 為基礎的 [WebView2](/microsoft-edge/webview2/) 控制項 |  WinUI 2 支援  [Web 視圖](/windows/uwp/design/controls-and-patterns/web-view) 控制項 |
| WinUI 3 適用于 Windows 10 2018 年10月更新 (1809 版、OS 組建 17763) 。 | WinUI 2 Windows 10 Creators Update (1703 版，作業系統組建 15063) 。 |

## <a name="see-also"></a>另請參閱

- [使用 Project 留尼旺島0.5 建立桌面 Windows 應用程式](../project-reunion/index.md)

- [Windows UI 程式庫 (WinUI)](index.md)

- [Windows UI 程式庫 2.x](winui2/index.md)

- [Windows UI 程式庫 3-Project 留尼旺島 0.5 (三月 2021) ](winui3/index.md)
