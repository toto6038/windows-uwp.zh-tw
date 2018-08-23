---
author: QuinnRadich
title: 在 2010 年 8 月 2018 Windows 文件中新功能-開發 UWP 應用程式
description: 新功能、 影片、 範例及開發人員指南已新增至 Windows 10 開發人員文件的 2010 年 8 月 2018年。
keywords: 什麼是新增、 更新、 功能、 開發人員指引，Windows 10、 august
ms.author: quradic
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c294dedc8e19605bc2cee0308022bed8624df57e
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/23/2018
ms.locfileid: "2815967"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>2010 年 8 月 2018 Windows 開發人員文件中新增功能

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列功能概觀、 開發人員指南及影片已可使用年 8 月中。

在 Windows10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows App](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="design"></a>設計

下列功能已新增 windows 內部預覽組建，可以透過[Windows 內部](https://insider.windows.com/)程式。

* [Windows UI 文件庫](https://aka.ms/winui-docs)是 NuGet 套件提供控制項和其他使用者 interfact 元素 UWP 應用程式的一組。 讓您的應用程式的運作即使您的使用者不需要的最新的 OS 版本這些套件也包含與舊版 Windows 10 相容。

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)、 [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button)，以及[ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button)提供使用特殊功能來加強您的應用程式使用者介面按鈕控制項。

![分割按鈕選取前景色彩](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView 現在也支援[上方導覽列中](../design/controls-and-patterns/navigationview.md)，針對您的應用程式具有較小的數字的導覽選項與您的應用程式內容需要更多空間的案例。

* 樹狀檢視已增強，可以支援[資料繫結，項目範本，並將拖放。](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>套件支援架構

套件支援架構是可協助您修正 win32 應用程式時套用您不需要存取來源的程式碼，使其能夠繼續運作 MSIX 容器中的開放原始碼套件。

如需了解，請參閱[套用 runtime 修正至使用套件支援架構的 MSIX 套件](../porting/package-support-framework.md)。

## <a name="developer-guidance"></a>開發人員指引

### <a name="web-api-extensions"></a>Web API 延伸模組

[舊版 Microsoft API 副檔名](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)的清單已新增到跨瀏覽器 web 開發的 Mozilla Developer Network 文件。 這些 API 副檔名是唯一 Internet Explorer 或 Microsoft Edge 和補充現有 MDN 網頁文件中的相容性評估和 broswer 支援的相關資訊。舊版 Microsoft [CSS 副檔名](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)和[JavaScript 副檔名](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)也可使用，且您可以找到豐富 web MDN API 資訊的呈現直接在[Visual Studio 程式碼。](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + + WinRT 程式碼範例

我們已新增 250 [C + + WinRT](../cpp-and-winrt-apis/index.md)程式碼伴隨現有 C + 在我們的文件中的主題清單 + CX 程式碼範例。

### <a name="project-rome"></a>Project Rome

[Project 羅馬文件](https://docs.microsoft.com/windows/project-rome/)網站具有已重新組織成功能優先方法。 這應該方便開發人員尋找時，要尋找的項目及實作多個平台他們選擇的功能。

## <a name="videos"></a>影片

### <a name="xbox-live-unity-plugin"></a>Xbox Live 凝聚力外掛程式

Xbox Live 外掛程式的凝聚力包含支援新增 Xbox Live 簽署、 stats、 朋友清單、 雲端儲存及排行榜至您的標題。 [觀賞影片](https://youtu.be/fVQZ-YgwNpY)來了解更多，然後再開始[下載 GitHub 套件](https://aka.ms/UnityPlugin)。

### <a name="one-dev-question"></a>一個開發人員問題

在一個開發人員問題系列影片、 longtime Microsoft 開發人員會涵蓋一系列有關 Windows 開發、 小組文化特性和歷程記錄。 以下是我們已接聽電話的最新問題 ！

Raymond Chen：

* [如何沒有核心了解何時要重新啟動視訊驅動程式？](https://youtu.be/3SNAdyO1l5c)

Larry Osterman：

* [本文後面 Burgermaster 物件在 Windows 中新功能](https://youtu.be/0TDSbyAIvX0)