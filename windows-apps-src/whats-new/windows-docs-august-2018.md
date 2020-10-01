---
title: 2018 年 8 月 Windows 文件的新增功能 - 開發 UWP 應用程式
description: 新功能、影片、範例及開發人員指引已加入 2018 年 8 月的 Windows 10 開發人員文件中。
keywords: 新增功能, 更新, 功能, 開發人員指引, Windows 10, 8 月
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 92ed09940800160af8fcff6aec7dde26144cffe9
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220331"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>2018 年 8 月 Windows 開發人員文件的新增功能

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列功能概觀、開發人員指引和影片已在 8 月份提供使用。

在 Windows 10 上[安裝工具和 SDK](https://developer.microsoft.com/windows/downloads#_blank) 之後，就表示您已經準備好[建立新的通用 Windows 應用程式](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的應用程式程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="design"></a>設計

Windows Insider Preview 組建中已加入下列功能，可透過 [Windows 測試人員](https://insider.windows.com/)程式取得。

* [Windows UI 程式庫](/uwp/toolkits/winui/)是一組 NuGet 套件，可提供 UWP 應用程式的控制項和其他使用者介面元素。 這些套件也提供對舊版 Windows 10 的向下相容性，因此即便您的使用者沒有最新版本的作業系統，您的應用程式仍可運作。

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)、[SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button) 和 [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) 提供了具有專門功能的按鈕控制項，以增強您的應用程式使用者介面。

![用來選取前景色彩的分割按鈕](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView 現在支援[最上層導覽](../design/controls-and-patterns/navigationview.md)，適用於您的應用程式導覽選項較少且應用程式需要更多內容空間的情況。

* TreeView 已增強，可支援[資料繫結、項目範本與拖放功能](../design/controls-and-patterns/tree-view.md)。

### <a name="package-support-framework"></a>套件支援架構

套件支援架構是開放原始碼套件，可在您無法存取原始碼時，協助將修正程式套用到 win32 應用程式，使其可以在 MSIX 容器中執行。

若要深入了解，請參閱[使用套件支援架構將執行階段修正程式套用到 MSIX 套件](/windows/msix/psf/package-support-framework)。

## <a name="developer-guidance"></a>開發人員指引

### <a name="web-api-extensions"></a>Web API 擴充功能

Mozilla Developer Network 文件中新增了[舊版 Microsoft API 擴充功能](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)的清單，以用於跨瀏覽器 Web 開發。 這些 API 擴充功能是 Internet Explorer 或 Microsoft Edge 獨有的，並補充了有關 MDN Web 文件中的相容性和和瀏覽器支援的現有資訊。舊版 Microsoft [CSS 延伸模組](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)和 [JavaScript 延伸模組](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)也可以使用，您可以在 [Visual Studio Code](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn) 中直接找到 MDN 中的豐富 Web API 資訊。

### <a name="cwinrt-code-examples"></a>C++/WinRT 程式碼範例

我們在文件中針對主題新增了 250 個 [C++/WinRT](../cpp-and-winrt-apis/index.md) 程式碼清單，並隨附現有的 C++/CX 程式碼範例。

### <a name="project-rome"></a>Project Rome

[Project Rome 文件](/windows/project-rome/)網站已經重新整理為功能優先的方法。 這應該會讓開發人員更容易找到他們正在尋找的東西，並跨多個平台實作他們選擇的功能。

## <a name="videos"></a>視訊

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 外掛程式

Unity 的 Xbox Live 外掛程式包含對您的標題加入 Xbox Live 簽章、統計資料、朋友清單、雲端儲存空間和排行榜。 [觀看影片](https://youtu.be/fVQZ-YgwNpY)以深入了解，然後[下載 GitHub 套件](/gaming/xbox-live/get-started/setup-ide/creators/unity-win10/live-cr-unity-win10-nav?WT.mc_id=windowsdocs-twi)即可開始著手。

### <a name="one-dev-question"></a>One Dev Question

在 One Dev Question 影片系列中，資深的 Microsoft 開發人員會談論一系列關於 Windows 開發、團隊文化和歷史的問題。 以下是我們所回答的最新問題！

Raymond Chen：

* [核心如何知道何時重新啟動視訊驅動程式？](https://youtu.be/3SNAdyO1l5c)

Larry Osterman：

* [Windows 中 Burgermaster 物件背後的故事是什麼？](https://youtu.be/0TDSbyAIvX0)