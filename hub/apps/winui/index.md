---
title: Windows UI 程式庫 (WinUI)
description: 用於 Windows 應用程式開發的 WinUI 程式庫。
ms.topic: article
ms.date: 07/15/2020
keywords: windows 10, uwp, 工具組 sdk, winui, Windows UI 程式庫
ms.openlocfilehash: d90101cfd674ddb2d422b200443fe7c8552f8f7a
ms.sourcegitcommit: 539b428bcf3d72c6bda211893df51f2a27ac5206
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2021
ms.locfileid: "102629276"
---
# <a name="windows-ui-library-winui"></a>Windows UI 程式庫 (WinUI)

![WinUI 標誌](../images/logo-winui.png)

Windows UI 程式庫 (WinUI) 是同時適用於 Windows 桌面和 UWP 應用程式的原生使用者體驗 (UX)。

WinUI 將 [Fluent Design 系統](https://www.microsoft.com/design/fluent/#/)併入所有體驗、控制項和樣式中，以使用最新的使用者介面 (UI) 模式來提供一致、直覺性且可存取的體驗。

因為有桌面和 UWP 應用程式的支援，您可以使用 WinUI 來從頭開始進行建置，或使用熟悉的語言 (例如 C++、C#、Visual Basic 和 Javascript，透過 [React Native for Windows](https://microsoft.github.io/react-native-windows/))，逐漸遷移現有的 MFC、WinForms 或 WPF 應用程式。

> [!Important]
> WinUI 有兩個版本：**WinUI 2.x** 和 **WinUI 3**。

## <a name="windows-ui-2x-library"></a>Windows UI 2.x 程式庫

WinUI 2.x 可以在 UWP 應用程式中使用，並透過使用 [XAML Islands](../desktop/modernize/xaml-islands.md) 合併到新的或現有的桌面應用程式中。

> [!NOTE]
> WinUI 2.5 是最新的 WinUI 2.x 版本。 如需下一版中的計畫工作清單，請參閱 [WinUI 2.6 里程碑](https://github.com/microsoft/microsoft-ui-xaml/milestone/11)。

WinUI 2.x 程式庫可與 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) 緊密結合，並提供適用於 UWP 應用程式的官方原生 Windows UI 控制項和其他 UI 元素。

因為保持了舊版 Windows 10 的向下相容性，即便使用者沒有最新的作業系統，WinUI 2.x 控制項仍可運作。

![WinUI 2.x 平台支援](../images/platforms-winui2.png)

**如需安裝指示，請參閱 [開始使用 Windows UI 程式庫](winui2/getting-started.md)。**

### <a name="related-links-for-winui-2x"></a>WinUI 2.x 的相關連結

- [WinUI 2.x 程式庫概觀](winui2/index.md)
- [API 文件](/windows/winui/api/)
- [原始程式碼](https://aka.ms/winui)
- [XAML 控制項庫應用程式](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-3-library-project-reunion-05-preview"></a>Windows UI 3 程式庫 (Project 留尼旺島 0.5 Preview) 

WinUI 3 是 WinUI 的下一個版本，這是與 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) 完全分離的原生 Windows 10 UI 平台。

> [!Important]
> 此 WinUI 3 Preview 旨在搶先評估並從開發人員群體收集意見反應。 **不會** 用於生產應用程式。
>
> 我們將在2021年3月的第一期推出第一次正式支援的版本。 它將會是 Project 留尼旺島0.5 套件的一部分。
>
> 請使用 [WinUI GitHub 存放庫](https://github.com/microsoft/microsoft-ui-xaml)以提供意見反應及記錄建議和問題。

因為 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) 與 XAML、撰寫和輸入 API 完全分離，所以 WinUI 3.0 的範圍包含完整的 Windows 10 原生 UI 平台。

WinUI 3 是 [Project 留尼旺島](../project-reunion/index.md)的元件，它提供一組整合的 api 和工具，可讓您在一組廣泛的目標 WINDOWS 10 作業系統版本上，以一致的方式使用這些應用程式。 作為專案留尼旺島的元件，WinUI 3 隨附于專案留尼旺島套件中; 如需詳細資訊，請參閱 [WINDOWS UI 程式庫 3-Project 留尼旺島 0.5 Preview (3 月 2021) ](winui3/index.md) 。

WinUI 3 是所有 Windows 應用程式的前進路徑，您可以使用它做為原生 UWP 或 Win32 應用程式的 UI 層，也可以使用 [XAML 孤島](../desktop/modernize/xaml-islands.md)逐漸將桌面應用程式現代化。

所有新的 XAML 功能最終都會隨附在 WinUI 中。 隨附於作業系統的現有 UWP XAML API 將不會再收到新的功能更新。 不過會隨著 Windows 10 支援週期，繼續收到安全性更新和重大修正程式。

請注意，通用 Windows 平台包含的不只是 XAML 架構。 應用程式和安全性模型、媒體管線、Xbox 和 Windows 10 命令介面整合等功能，以及與各種不同裝置的相容性，將會繼續進行開發和支援。

![WinUI 3 平台支援](../images/platforms-winui3.png)

### <a name="related-links-for-winui-3"></a>WinUI 3 的相關連結

- [Windows UI 程式庫 3-Project 留尼旺島 0.5 Preview (3 月 2021) ](winui3/index.md)
- [XAML 控制項資源庫 (WinUI 3 Preview) 應用程式](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)

## <a name="winui-resources"></a>WinUI 資源

**Github**：WinUI 是裝載於 Github 上的開放原始碼專案。 使用 [WinUI 存放庫](https://github.com/microsoft/microsoft-ui-xaml)，提出功能要求或錯誤、與 WinUI 小組互動，以及從[藍圖](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)中檢視小組的 WinUI 3 計畫及其他事項。

**網站**： [WinUI 網站](https://aka.ms/winui) 具有產品比較、說明 WinUI 的各種優點，並提供與產品和產品團隊保持聯繫的方式。