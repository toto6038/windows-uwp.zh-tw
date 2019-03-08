---
title: 在 Windows 文件中於 2018 年 8 月新消息-開發 UWP 應用程式
description: 新功能、 影片、 範例和開發人員指引已新增至 2018 年 8 月的 Windows 10 開發人員文件。
keywords: 新功能、 更新、 功能、 開發人員指導方針，Windows 10 中，年 8 月
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9922aa1ad2442153dcc2c13d05520c05c3b56d31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616483"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Windows 開發人員文件在 2018 年 8 月新消息

Windows 開發人員文件一直持續不斷更新有關 Windows 平台上可供開發人員使用之新功能的資訊。 下列的功能概觀、 開發人員指引和影片已有可用在 8 月的月。

在 Windows 10 上[安裝工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows app](../get-started/create-uwp-apps.md)，或是探索[如何在 Windows 上使用現有的 App 程式碼](../porting/index.md)。

## <a name="features"></a>功能

### <a name="design"></a>設計

下列功能已加入 Windows Insider Preview 組建，可透過[Windows Insider](https://insider.windows.com/)程式。

* [Windows 的 UI 程式庫](https://aka.ms/winui-docs)是一組控制項和其他使用者 interfact 的項目為 UWP 應用程式提供的 NuGet 套件。 這些套件也會較早版本的 Windows 10 相容，因此即使您的使用者沒有最新的作業系統版本，適用於您的應用程式。

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)， [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button)，以及[ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button)提供特製化的功能，來加強您的應用程式使用者介面中的按鈕控制項。

![分割按鈕來選取前景色彩](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView 現在支援[上層導覽](../design/controls-and-patterns/navigationview.md)，您的應用程式具有較少的導覽選項且需要更多空間，您的應用程式內容的情況。

* 樹狀檢視已增強為支援[資料繫結項目範本，並將拖放。](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>封裝支援架構

封裝支援架構是開放原始碼套件，可協助您套用修正您的 win32 應用程式時您沒有存取權的原始程式碼，使其可以 MSIX 容器中執行。

若要進一步了解，請參閱[套用執行階段使用封裝支援架構修正 MSIX 封裝](../porting/package-support-framework.md)。

## <a name="developer-guidance"></a>開發人員指引

### <a name="web-api-extensions"></a>Web API 擴充功能

一份[舊版的 Microsoft API 擴充功能](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)已新增至跨瀏覽器 web 開發的 Mozilla Developer Network 文件。 這些 API 擴充功能是 Internet Explorer 或 Microsoft Edge 中，唯一的並補充 MDN 的 web 文件中的相容性和瀏覽器支援的現有資訊。舊版的 Microsoft [CSS 延伸模組](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)並[JavaScript 延伸模組](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)也可以使用，而且您可以找到豐富的 web API 資訊從 MDN 中直接呈現[Visual Studio Code。](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + + /cli WinRT 程式碼範例

我們已新增 250 [C + + WinRT](../cpp-and-winrt-apis/index.md)程式碼清單主題，在我們的文件中，然後再隨附的現有 C + + /CX 程式碼範例。

### <a name="project-rome"></a>Project Rome

[Project Rome docs](https://docs.microsoft.com/windows/project-rome/)站台已重新整理到功能優先的方法。 這應該簡化開發人員來找到他們所尋找的以及跨多個平台實作自己選擇的功能。

## <a name="videos"></a>影片

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 外掛程式

Unity 的 Xbox Live 外掛程式包含支援加入您的標題中的 Xbox Live 簽署、 統計資料、 朋友清單、 雲端存放裝置和排行榜。 [觀看影片](https://youtu.be/fVQZ-YgwNpY)若要進一步了解，然後[下載 GitHub 套件](https://aka.ms/UnityPlugin)開始著手。

### <a name="one-dev-question"></a>一個開發人員問題

在一個開發人員問題影片系列中，資深的 Microsoft 開發人員會討論一系列有關 Windows 開發、 team 文化特性和歷程記錄的問題。 以下是我們所回答的最新問題 ！

Raymond Chen:

* [如何在核心知道何時重新啟動的視訊驅動程式？](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [在 Windows 中的 Burgermaster 物件背後的故事是什麼？](https://youtu.be/0TDSbyAIvX0)