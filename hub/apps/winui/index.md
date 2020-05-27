---
title: Windows UI 程式庫 (WinUI)
description: 用於 Windows 應用程式開發的 WinUI 程式庫。
ms.topic: article
ms.date: 05/11/2020
keywords: windows 10, uwp, 工具組 sdk, winui, Windows UI 程式庫
ms.openlocfilehash: 01eed4a82bfe14b70c86ade1b82c487e33e6f6f6
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775885"
---
# <a name="windows-ui-library-winui"></a>Windows UI 程式庫 (WinUI)

![工具組主圖影像](../images/logo-winui.png)

Windows UI 程式庫 (WinUI) 是適用於 Win32 和 UWP 上 Windows 應用程式的新型原生使用者介面 (UI) 平台。

WinUI 將 [Fluent Design 系統](https://www.microsoft.com/design/fluent/#/)併入所有控制項和樣式中，以使用最新的 UI 模式來提供一致、直覺性且可存取的體驗。

而且因為有 Win32 和 UWP 應用程式的支援，您可以使用 WinUI 來從頭開始進行建置，或以您自己的步調遷移現有的 MFC、WinForms 或 WPF 應用程式，以及使用 C++、C#、Visual Basic 等熟悉的語言，甚至是透過 [React Native for Windows](https://microsoft.github.io/react-native-windows/) 使用 JAVAscript。

有兩個 WinUI 版本需要注意：**WinUI 2.x** 和 **WinUI 3.0**。

## <a name="windows-ui-2x-library"></a>Windows UI 2.x 程式庫

WinUI 2.x 可以立即在 UWP 應用程式中使用，並透過 [XAML Islands](/windows/apps/desktop/modernize/xaml-islands) 合併到您現有的桌面應用程式中。

WinUI 2.x 程式庫可與 UWP SDK 緊密結合，並提供適用於 UWP 應用程式的官方原生 Windows UI 控制項和其他 UI 元素。

因為保持了舊版 Windows 10 的向下相容性，即便使用者沒有最新的作業系統，WinUI 2.x 控制項仍可運作。

![WinUI 2.x 平台支援](../images/platforms-winui2.png)

> [!NOTE]
> WinUI 2.4 是最新的 WinUI 2.x 版本。 如需下一版中的計畫工作清單，請參閱 [WinUI 2.5 里程碑](https://github.com/microsoft/microsoft-ui-xaml/milestone/10)。

如需安裝指示，請參閱[開始使用 Windows UI 程式庫](winui2/getting-started.md)。

### <a name="related-links-for-winui-2x"></a>WinUI 2.x 的相關連結

- [WinUI 2.x 程式庫概觀](winui2/index.md)
- [API 文件](https://docs.microsoft.com/uwp/api/overview/winui/)
- [原始程式碼](https://aka.ms/winui)
- [XAML 控制項庫應用程式](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-30-library-preview-1"></a>Windows UI 3.0 程式庫 (預覽版 1)

WinUI 3 是 WinUI 的下一個版本，這是與 UWP SDK 完全分離的原生 Windows 10 UI 平台。

因為與 UWP SDK 中的 Xaml、撰寫和輸入 API 完全分離，Windows UI 3.0 程式庫可以大幅擴充 WinUI 的範圍，以包含完整的 Windows 10 原生 UI 平台。

WinUI 可作為所有 Windows 應用程式向前邁進的路徑 - 您可以使用此平台作為原生 UWP 或 Win32 應用程式上的 UI 層，也可以搭配 [Xaml Islands](https://docs.microsoft.com/windows/apps/desktop/modernize/xaml-islands) 來逐一現代化您的桌面應用程式。

> [!NOTE]
> WinUI 3.0 預覽版 1 是 WinUI 3.0 的發行前版本。 我們歡迎您在 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml) 中提供意見反應。

所有新的 Xaml 功能都會隨附在 WinUI 中。 隨附於作業系統的現有 UWP Xaml API 將不會再收到新的功能更新。 不過會隨著 Windows 10 支援週期，繼續收到安全性更新和重大修正程式。

通用 Windows 平台包含的不只是 Xaml 架構 (例如應用程式和安全性模型、媒體管線、Xbox 和 Windows 10 殼層整合、廣泛的裝置支援)，而且會繼續受到支援。

![WinUI 3.0 平台支援](../images/platforms-winui3.png)

> [!Important]
> WinUI 3.0 預覽版 1 的目的是進行早期評估，並從開發人員社群收集意見反應。 **不會**用於生產應用程式。

### <a name="related-links-for-winui-30"></a>WinUI 3.0 的相關連結

- [WinUI 3.0 預覽版 1 (2020 年 5月)](winui3/index.md)
- [XAML 控制項資源庫 (WinUI 3.0 預覽版 1) 應用程式](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)

## <a name="winui-resources"></a>WinUI 資源

**Github**：WinUI 是裝載於 Github 上的開放原始碼專案。 在 [WinUI 存放庫](https://github.com/microsoft/microsoft-ui-xaml)上，您可以提出功能要求或錯誤、與 WinUI 小組互動，以及從[藍圖](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)中檢視小組的 WinUI 3 計畫及其他事項。

**網站**：[WinUI 網站](https://aka.ms/winui)提供許多實用資訊，例如產品的比較、WinUI 的優點，以及持續關注產品和產品小組最新資訊的方式。
